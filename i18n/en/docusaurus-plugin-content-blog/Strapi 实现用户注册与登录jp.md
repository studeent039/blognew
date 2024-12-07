---
slug: strapi-user-register-and-login
title: Strapi 实现用户注册与登录
date: 2022-09-03
authors: ragroup
tags: [strapi, nuxt, next]
keywords: [strapi, nuxt, next]
description: Strapi 实现用户注册与登录
image: https://img.kuizuo.cn/202312270300876.png
---

公式ブログ「Vue.js と Strapi による登録とログイン (認証)」(https://strapi.io/blog/registration-and-login-authentication-with-vue-js-and-strapi-1) で実証済み登録とログインの方法。実際の重要な部分は、Strapi の [役割と権限プラグイン](https://docs.strapi.io/developer-docs/latest/plugins/users-permissions.html) であると言えます。開発者は、ユーザーのログイン登録が認証に関連していることを考える必要がなくなります。

さらに、体験できるオンラインの例は次のとおりです: [Vitesse Nuxt 3 Strapi](https://vitesse-nuxt3-strapi.vercel.app)

<!-- truncate -->

## Strapi プロジェクトを作成する

Strapi プロジェクトの作成手順はここでは省略しますが、詳細は[クイックスタートガイド](https://docs.strapi.io/developer-docs/latest/getting-started/quick-start.html)を参照してください。プロジェクトを作成し、管理者アカウントを登録したら、管理パネルを開き、必要に応じてデータを作成します。以下に管理パネルの一部の操作を紹介します（以下は中国語パネルの場合）

### 役割リスト

**設定 => ユーザーと権限プラグイン => 役割リスト** を開きます

![image-20220825131929320](https://img.kuizuo.cn/image-20220825131929320.png)

デフォルトでは、Authenticated と Public という 2 つのロールがあり、どちらも削除できません。また、管理者権限を割り当てるために自分で作成した Admin ロールもあります。

Authenticated は、ログイン後のロール、つまり、jwt を含む **Authorization** プロトコル ヘッダーを持つユーザーに対応します。

もう 1 つの Public は、次のデフォルトの権限を持つ無許可ユーザーです。

![image-20220825132235027](https://img.kuizuo.cn/image-20220825132235027.png)

### 権限の割り当て

ロールをダブルクリックして権限の割り当てページに移動します。たとえば、レストラン テーブルのデータをクエリするために認証済みのロールを割り当てる場合は、次のオプションをオンにして、権限の 1 つ (追加、削除、変更) をチェックします。 、チェックしてください)、右側に対応するリクエスト API インターフェイス (ルーティング) が表示されます。

![image-20220825132716257](https://img.kuizuo.cn/image-20220825132716257.png)

###デフォルトの役割

**設定 => ユーザーと権限プラグイン => 詳細設定** でデフォルトの役割を割り当てることができます。さらに、登録、パスワードのリセット、その他の操作もここで構成できます。これらの機能については、従来の開発では非常に多くのコードを記述する必要がありますが、Strapi の [役割と権限](https://docs.strapi.io/developer-docs/latest/plugins/users-permissions.html) プラグインは、関数のこの部分の開発時間を節約できます。

![image-20220825132948740](https://img.kuizuo.cn/image-20220825132948740.png)

### 管理者権限

**[設定] => [管理者権限]** でもロール リストとユーザー リストを確認できますが、これは Strapi ダッシュボードにログインしているユーザーのみを対象としたものであり、実際のビジネス ユーザーとは関係ありません。平たく言えば、データベース システムのユーザーとバックエンド管理システムのユーザーの違いです。

初期ログイン パネルで作成されたユーザーは **[設定] => [管理者権限] => [ユーザー リスト]** に表示されます。一方、API http://localhost:1337/api/auth/local/register を通じて登録されたユーザーは ** に表示されます。コンテンツ管理 => ユーザー**。

## HTTP を使用してユーザー アクションを要求します (一般)

公式に提供されている登録アドレスとログイン アドレスは次のとおりです。

[http://localhost:1337/api/auth/local/register](http://localhost:1337/api/auth/local/register)

[http://localhost:1337/api/auth/local](http://localhost:1337/api/auth/local)

これらは、[ログイン](https://docs.strapi.io/developer-docs/latest/plugins/users-permissions.html#login) および [登録](https://docs.strapi.io/) で見つけることができます。開発者 - docs/latest/plugins/users-permissions.html#registration の公式デモの例を参照してください)。

'@theme/Tabs' からタブをインポートします。 '@theme/TabItem' から TabItem をインポートします。

```mdx コードブロック
<タブ>
<TabItem value="ログイン" ラベル="ログイン" デフォルト>
「」

```js {4}
「axios」から axios をインポートします

// APIをリクエストします。
アクシオス
 .post('http://localhost:1337/api/auth/local', {
 識別子: 'user@strapi.io'、
 パスワード: 'strapiパスワード',
 })
 .then(応答 => {
 // 成功を処理します。
 console.log('よくやった!')
 console.log('ユーザープロファイル',response.data.user)
 console.log('ユーザートークン'、response.data.jwt)
 })
 .catch(エラー => {
 // エラーを処理します。
 console.log('エラーが発生しました:', error.response)
 })
「」

```mdx コードブロック
</タブ項目>
<TabItem value="登録" label="登録">
「」

```js {4}
「axios」から axios をインポートします

// APIをリクエストします。
アクシオス
 .post('http://localhost:1337/api/auth/local/register', {
 ユーザー名: 'Strapi ユーザー',
 電子メール: 'user@strapi.io'、
 パスワード: 'strapiパスワード',
 })
 .then(応答 => {
 // 成功を処理します。
 console.log('よくやった!')
 console.log('ユーザープロファイル',response.data.user)
 console.log('ユーザートークン'、response.data.jwt)
 })
 .catch(エラー => {
 // エラーを処理します。
 console.log('エラーが発生しました:', error.response)
 })
「」

```mdx コードブロック
</タブ項目>
</タブ>
「」

ログインに加えて、個人情報の取得、パスワードのリセット、パスワードの変更、電子メール確認の送信など、使用できる API がいくつかあります。詳細については、[役割と権限](https://docs.strapi.io/developer-docs/latest/plugins/users-permissions.html#authentication) を参照してください。

HTTP を介したソリューションが最も一般的であると言えますが、一部のフレームワークでは、Strapi を呼び出すための対応するモジュールも提供しています。

##ヌクスト

公式 Nuxt3 は、Strapi を使用したフック ソリューションを提供します。詳細については、[Nuxt Strapi モジュール](https://strapi.nuxtjs.org/)を参照してください。 Nuxt2 は [こちら](https://strapi-v0.nuxtjs.org/hooks) でご覧いただけます。

対応するフックを通じて、ログイン、登録、データの追加、削除、変更、クエリの機能を実現できます。デモの例については、[使用方法](https://strapi.nuxtjs.org/usage)を参照してください。

これは私が作成したプリセット テンプレート [kuizuo/vitesse-nuxt3-strapi](https://github.com/kuizuo/vitesse-nuxt3-strapi) です。最初の例もこのテンプレートに基づいています。ただし、Strapi の現在の TypeScript サポートは、特にウィンドウで実行できない場合はそれほどフレンドリーではありません。詳細については、[pr](https://github.com/strapi/strapi/pull/14088) を参照してください。したがって、現在バックエンドは js を使用して作成され、その後 ts 関連のサポートが追加されるため、一部の ts サポートはそれほどフレンドリーではない可能性があります。

：：：注記

当初、スターター メソッドを使用して nuxt3 Strapi プロジェクトを作成することを検討していましたが、スターターとテンプレートを作成した後、`yarn create Strapi-starter Strapi-nuxt3 https://github.com/kuizuo/strapi- を使用する準備が整いました。 starter-nuxt3` テンプレートをダウンロードする際に予期せぬエラーが報告され、トラブルシューティングが困難だったので一旦諦めました。

総じて、またしても無駄な旅だった。

:::

＃＃ 次

次に、Nuxt のような Strapi サービスを提供できる関連ライブラリはまだ見つかりません。ただし、Strapi は、http リクエストを送信せずに Strapi サービスを呼び出すための [SDK](https://github.com/strapi/strapi-sdk-javascript) ソリューションを公式に提供しています。詳細については、公式の [SDK]() を参照してください。 https://github.com/strapi/strapi-sdk-javascript) 使用方法を参照してください。ここにはデモはありません。選択できる SDK は 2 つあります。

[strapi/strapi-sdk-javascript](https://github.com/strapi/strapi-sdk-javascript) 公式サイト

[Strapi SDK (strapi-sdk-js.netlify.app)](https://strapi-sdk-js.netlify.app/) コミュニティ
