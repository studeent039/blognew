---
slug: vercel-deploy-blog
title: Vercel部署个人博客
date: 2022-05-11
authors: ragroup
tags: [vercel, blog]
keywords: [vercel, blog]
description: 使用 Vercel 部署个人博客过程记录，简单方便、访问快、免费部署。
image: https://img.kuizuo.cn/image-20220511170700075.png
---

![image-20220511170700075](https://img.kuizuo.cn/image-20220511170700075.png)

:::tip 閲覧前の注意事項

[Vercel](https://vercel.com/) 静的リソース Web サイトの展開は非常にシンプルで便利**で、**アクセス速度も非常に高速です。最も重要なことは、**無料の展開**です。

まだ試したことがない場合は、ぜひ使用することをお勧めします。

:::

【vercelの紹介】(https://zhuanlan.zhihu.com/p/452654619)

類似製品[Netfily](https://netlify.com)、民営化を導入したい場合は[Coolify](https://coolify.io)を推奨

このようなサイトを構築したい場合は、私の [Docusaurus テーマの魔法の修正](/docs/docusaurus-guides) を参照するとよいでしょう。

:::danger DNS汚染

何らかの理由で、vercel.app は DNS によって汚染されており (ブロックされており)、独自のドメイン名を持っていて CNAME 解決を通じてドメイン名にアクセスしない限り、現在中国では開くことができません。

**したがって、国内でアクセスしたい場合は、Vercel を使用せずに Netlify を使用することをお勧めします。 **

:::

<!-- truncate -->

## アカウントを登録する

[Vercel](https://vercel.com) 公式 Web サイトにアクセスし、アカウントを登録します。[Github](https://github.com/) アカウントを登録してから、その Github アカウントを使用してログを記録することをお勧めします。ヴェルセルへ。

## Web サイトをデプロイする

[ダッシュボード](https://vercel.com/dashboard)に入ります

![image-20220511170233559](https://img.kuizuo.cn/image-20220511170233559.png)

[新しいプロジェクト](https://vercel.com/new)をクリックします。

![image-20220511165902993](https://img.kuizuo.cn/image-20220511165902993.png)

ここで、既存の git リポジトリからインポートするか、テンプレートを選択できます。

ここで Github アカウントにログインし、ウェアハウスを選択して、ブログ ウェアハウスの横にある [インポート] をクリックします。もちろん、私のウェアハウスを直接取得することもできます。ウェアハウスのアドレス: [kuizuo/blog](https://github.com/kuizuo/blog)

![image-20220511165513526](https://img.kuizuo.cn/image-20220511165513526.png)

「デプロイ」をクリックし、Web サイトが依存関係をインストールしてデプロイするまで待ちます。後で以下のページが表示されます。

![image-20220511170700075](https://img.kuizuo.cn/image-20220511170700075.png)

この時点で、Web サイトは正常に構築されました。画像をクリックすると、vercel が提供する第 2 レベルのドメイン名にジャンプします。

非常にシンプルではないでしょうか？ **構築された Web サイトにアクセスするためにコマンドを入力する必要さえありません。 **

## カスタム ドメイン名

独自のドメイン名をお持ちの場合は、vercel で設定することもできます。

まず、ブログコンソールに入り、[設定] -> [ドメイン] でドメイン名を追加します。

![image-20220511171144240](https://img.kuizuo.cn/image-20220511171144240.png)

次に、ドメイン名が vercel によって提供されたレコード値への DNS 解決が必要であることを要求するプロンプトが表示されます。

![image-20220511171359148](https://img.kuizuo.cn/image-20220511171359148.png)

現在いるドメイン ネーム サービス プロバイダーにログインし、Vercel が提供するレコード値 cname.vercel-dns.com に基づいて 2 つのレコードを追加します。

![image-20220511172741663](https://img.kuizuo.cn/image-20220511172741663.png)

Vercel に戻ると、記録された値が正常に有効になっていることがわかります。

![image-20220511172027570](https://img.kuizuo.cn/image-20220511172027570.png)

この時、独自ドメイン名でアクセスするとページへのアクセスも可能となり、同時にアクセス速度もかなりのものになります。

### SSL証明書を自動発行する

デフォルトでは、Vercel は SSL 証明書を発行し、自動的に更新します。 (手動で証明書を申請したり、証明書を自分で設定したりする必要がなく、非常に便利です)

![image-20220511172240999](https://img.kuizuo.cn/image-20220511172240999.png)

## 継続的インテグレーション (CI)/継続的デプロイメント (CD)

> To update your Production Deployment, push to the "main" branch.

コードがメイン ブランチにプッシュされると、Vercel はコードを再プルし、単体テストとデプロイのために再構築します (ビルド速度はかなり速くなります)。

![image-20220511173442694](https://img.kuizuo.cn/image-20220511173442694.png)

## サーバーレス

同時に、vercel はサーバーレスもサポートしています。つまり、静的サイトだけでなくバックエンド サービスも展開できますが、一定の制限がある必要があります。

[Vercel Deploy Serverless](/blog/vercel-deploy-serverless)

## エッジ関数

エッジ関数として翻訳すると、Vercel の CDN 上で実行される関数として理解できます。サーバー上でコードを実行せずに、Vercel の CDN 上でコードを実行できます。

このような関数は、静的リソースと同様に CDN を通じて分散されるため、非常に高速に実行されます。

公式サイト紹介：【Edge Functions】(https://vercel.com/docs/concepts/functions/edge-functions)

## バーセル CLI

Web ページにログインして新しいプロジェクトを作成し、ウェアハウスを選択してデプロイメントをプルするのではなく、プロジェクトの直下にコマンドを入力してデプロイメントを完了したい場合があります。 Vercel は、開発者が使用できる対応するスキャフォールディング **[CLI](https://vercel.com/docs/cli)** を当然提供します。

インストール

```
npm i -g vercel
```
プロジェクトのルートディレクトリに入力します

```
vercel --prod
```

初回は、対応するプラットフォームを選択すると、ブラウザが自動的に開き、認証が完了します。通常は、Enter キーを押すだけで実行結果が表示されます。

```
Vercel CLI 24.2.1
? Set up and deploy “F:\Project\React\online-tools”? [Y/n] y
? Which scope do you want to deploy to? kuizuo
? Link to existing project? [y/N] n
? What’s your project’s name? online-tools
? In which directory is your code located? ./
Auto-detected Project Settings (Create React App):
- Build Command: react-scripts build
- Output Directory: build
- Development Command: react-scripts start
? Want to override the settings? [y/N] n
🔗  Linked to kuizuo12/online-tools (created .vercel and added it to .gitignore)
🔍  Inspect: https://vercel.com/kuizuo12/online-tools/6t8Vt8rG3waGVHTKU7ZzJuGc6Hoq [2s]
✅  Production: https://online-tools-phi.vercel.app [copied to clipboard] [2m]
📝  Deployed to production. Run `vercel --prod` to overwrite later (https://vercel.link/2F).
💡  To change the domain or build command, go to https://vercel.com/kuizuo12/online-tools/settings
```

実行後、ルート ディレクトリに .vercel フォルダーが作成され、project.json には以下で使用する orgId と projectId が格納されます。この時点で、プロジェクトが [ダッシュボード](https://vercel.com/dashboard) にデプロイされていることも確認できます。

ただし、この方法でデプロイされたコードは git ウェアハウスに接続されません。コンソールでウェアハウスを選択する必要があります。

Github アクションで使用する場合は、新しいステップを作成し、対応する変数を設定します。

```
	- name: Deploy to Vercel
        run: npx vercel --token ${{VERCEL_TOKEN}} --prod
        env:
            VERCEL_TOKEN: ${{ secrets.VERCEL_TOKEN }}
            VERCEL_PROJECT_ID: ${{ secrets.VERCEL_PROJECT_ID }}
            VERCEL_ORG_ID: ${{ secrets.VERCEL_ORG_ID }}
```

新しいトークンを作成するには、[Vercel 設定トークン](https://vercel.com/account/tokens) に移動する必要があります。

## 要約する

要約する必要はありません。ただ使い始めて、Vercel とその製品、[Next.js](https://github.com/vercel/next.js) および [Turbo] に夢中になると思います。 (https://github.com/vercel/turbo) など。