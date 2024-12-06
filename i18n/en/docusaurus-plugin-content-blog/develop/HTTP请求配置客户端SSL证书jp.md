---
slug: http-config-client-ssl-certificate
title: HTTP请求配置客户端SSL证书
date: 2022-02-17
authors: ragroup
tags: [http, ssl]
keywords: [http, ssl]
---

Android リバース エンジニアリングを学習する場合、APP に遭遇すると、サーバーは要求された SSL 証明書を検出し、要求を正常に送信する前に SSL 証明書を送信する必要があります。パケット キャプチャとプロトコルの繰り返しが有効になっている場合、リクエストは正常に送信できますが、サーバーは 400 エラーを返します。そこでこの記事はそれを記録するために書きました。

<!-- truncate -->

## 説明します

サーバーはクライアントから送信された証明書を検証するため、プロキシ サーバー (FD、Charles など) を使用してパケットをキャプチャすると、クライアントから送信された証明書がサーバーの証明書と一致しないことを検証すると、ローカル証明書が置き換えられます。実際には、リクエストは送信できますが、サーバーによって拒否されます。一般に **双方向認証** として知られています

したがって、解決策は、正しい証明書をリクエストと一緒に送信し、サーバーが検証時に通常の応答結果をクライアントに返すようにすることです。つまり、**カスタム証明書を構成**します。

＃＃＃ 例

APPの例: 曖昧

証明書を取得する具体的な方法は、Android リバース エンジニアリングに関連する部分です。ここでは証明書ファイルのみを提供し、アプリは提供しません。

ダウンロードアドレスとパスワードを貼り付けます

証明書: https://img.kuizuo.cn/cert.p12

パスワード: `xinghekeji888.x`

### 証明書の変換

[証明書形式変換(myssl.com)](https://myssl.com/cert_convert.html)

[SSLオンラインツール - オンライン証明書形式変換 - 証明書オンライン結合 - p12、pfx、jks証明書オンライン合成・解析 - SSLeye公式サイト](https://www.ssleye.com/ssltool/jks_pkcs12.html)

OpenSSL ツールを使用して証明書を変換することもできます。

## HTTP送信リクエスト

### ノードの軸

```javascript
const axios = require('axios').default
const fs = require('fs')
const https = require('https')

axios
  .post(
    `https://app.yyueapp.com/api/passLogin`,
    {
      mobile: '15212345678',
      password: 'a123456',
    },
    {
      httpsAgent: new https.Agent({
        cert: fs.readFileSync('./cert.cer'),
        key: fs.readFileSync('./cert.key'),
        // pfx: fs.readFileSync('./cert.p12'),
        // passphrase: 'xinghekeji888.x,
      }),
    },
  )
  .then((res) => {
    console.log(res.data)
  })
  .catch((error) => {
    console.log(error.response.data)
  })
```

httpsAgent が設定されていない場合、つまり証明書が設定されていない場合は、400 エラー「400 必要な SSL 証明書が送信されませんでした」(`400 No required SSL certificate was sent`。)が返されます。

設定が成功すると、正しい応答が返されます。

```javascript
{ code: 998, msg: '系统维护中...', data: null }
```
### Python リクエスト

リクエストは p12 形式の証明書をサポートしていないため、次のように他の証明書形式を使用する必要があります。

```python
import requests

r = requests.post('https://app.yyueapp.com/api/passLogin', data={
                  'mobile': '15212345678', 'password': 'a123456'}, cert=('./cert.cer', './cert.key'))
print(r.status_code)
print(r.text)
```