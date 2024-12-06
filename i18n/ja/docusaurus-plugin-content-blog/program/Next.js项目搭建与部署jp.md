---
slug: next.js-build-and-deploy
title: Next.js项目搭建与部署
date: 2022-07-13
authors: ragroup
tags: [next, react, ssr, vercel]
keywords: [next, react, ssr, vercel]
draft: false
---

<!-- truncate -->

公式ドキュメント [Next.js のスタートガイド (nextjs.org)](https://nextjs.org/docs/getting-started)

## [インストール](https://nextjs.org/docs/getting-started#automatic-setup)

```bash
npx create-next-app@latest --ts
# or
yarn create next-app --typescript
# or
pnpm create next-app --ts
```

実行する

```
npm run dev
```

http://localhost:3000 にアクセスしてください

## プロジェクトの構造

![image-20220712030637300](https://img.kuizuo.cn/image-20220712030637300.png)

| 文件           | 内容                 |
| -------------- | -------------------- |
| pages          | 页面文件             |
| pages/api      | api 数据接口         |
| public         | 静态资源文件         |
| styles         | 样式文件             |
| next-env.d.ts  | 确保 typescript 支持 |
| next.config.ts | next 配置文件        |

＃＃ ルーティング

nextjs はページの概念に基づいたファイル システム ルーターを備えており、ページの下に格納されている `.js`、`.js`、`.ts`、`.tsx` ファイルがコンポーネントとして使用されます。つまり、**ファイル パス → ページです。 routing** 、たとえば、ここではindex.tsxはindexにマッピングされ、`pages/about.js`は`/about`にマッピングされます。

動的ルーティングもサポートしており、`pages/user/[id].tsx` ファイルを作成して、`user/1`、`user/2` にアクセスします。

```tsx title="[id].tsx"
import { useRouter } from 'next/router'

const User = () => {
  const router = useRouter()
  const { id } = router.query

  return <div>User id:{id} </div>
}

export default User
```

この時点で、http://localhost:3000/user/1 にアクセスして「ユーザー ID: 1」を取得します。

router オブジェクトの下には param 属性はなく、クエリ パラメータに格納されます。たとえば、user/1?username=kuizuo にアクセスする場合、このときのクエリ値は `{username: 'kuizuo', id: '2 になります。 '}`

:::tip

ただし、ここで興味深い点があります。上記のコードで console.log を使用してクエリを出力すると、空のオブジェクト `{}` が vscode に出力され、空のオブジェクトと実際のオブジェクトが 1 回だけ出力されます。ブラウザのクエリ オブジェクト (および 2 回出力)
![image-20220712191356587](https://img.kuizuo.cn/image-20220712191356587.png)

:::

## データのレンダリング

コンソールを開いて返されたページを表示すると、応答には User id: だけが含まれていることがわかります。これは React の CSR (クライアント側) レンダリングと何ら変わりません。前の部分のコードと出力クエリからわかるように、これは SSR (サーバーサイド) レンダリングではありません。まず、両方のレンダリングのコードを示したいと思います。

### CSR クライアントのレンダリング

```tsx title="[id].tsx"
import { useEffect, useState } from 'react'
import { useRouter } from 'next/router'

const User = () => {
  const router = useRouter()
  const { id } = router.query

  const [data, setData] = useState({
    username: '',
    email: '',
  })

  useEffect(() => {
    fetch(`https://jsonplaceholder.typicode.com/users/${id}`)
      .then(res => res.json())
      .then(data => {
        setData(data)
      })
      .catch(err => {})
  }, [id])

  return (
    <div>
      <p>username:{data.username} </p>
      <p>email:{data.email} </p>
    </div>
  )
}

export default User
```

React をよく書く人は、フロントエンドがバックエンドにデータリクエストを送信し、データを受信して​​データに割り当て、レンダリングする上記のコードに精通しているはずです。データのリクエストに時間がかかるため、ページが表示された後、データを表示するまでにしばらく停止します（主にロードを書いていないため）、および最初の時点でIDが取得されていないため（上記のIDから） ）。

![image-20220712193009186](https://img.kuizuo.cn/image-20220712193009186.png)

ここから、クライアント側のレンダリングでは、ページ コンポーネントを取得するだけでなく、データをリクエストし、最終的に js を介してレンダリングする必要があります。

### SSR サーバー側レンダリング

次のサーバー側レンダリングでは getServerSideProps 関数を使用する必要があり、バックエンド データはこの関数内で取得され、prop を通じてフロントエンド コンポーネントに渡されます。
```tsx title="[id].tsx"
const User = ({ data }: { data: any }) => {
  return (
    <div>
      <p>username:{data.username} </p>
      <p>email:{data.email} </p>
    </div>
  )
}

export default User

export async function getServerSideProps(context: { query: { id: any } }) {
  const { id } = context.query // 这里context.param也能获取到id

  const res = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`)

  const data = await res.json()

  return {
    props: {
      data,
    },
  }
}
```

ページ表示を見ると実際には違いがありませんが、コンソールを開くと多くの違いが見つかります。

1 つ目は、リクエストされたページには直接データが含まれており、これはページを返すのと同じですが、クライアントでレンダリングするとコンポーネントが返されるため、表示するにはデータを自分でリクエストする必要があります。

![image-20220712192713634](https://img.kuizuo.cn/image-20220712192713634.png)

同時に、コンソールで Fetch/XHR を表示すると、データはフロントエンドではなくバックエンドによって送信されるため、要求されたデータは表示されません (そのため、クロスドメイン要求による制限を受けません)。

ここから、クライアント側レンダリングとサーバー側レンダリングの違いがわかります。

### SSG 静的生成

しかし、まだ終わっていません。まだ静的生成が残っています。最初にコードを見てみましょう。

```tsx タイトル="[id].tsx"
const ユーザー = ({ データ }: { データ: 任意の }) => {
 戻る （
 <div>
 <p>ユーザー名:{data.username} </p>
 <p>メール:{data.email} </p>
 </div>
 )
}

デフォルトのユーザーをエクスポートする

非同期関数のエクスポート getStaticProps(context: { params: { id: any } }) {
 const { id } = context.params

 const res = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`)

 const data = await res.json()

 戻る {
 小道具: {
 データ、
 }、
 }
}

非同期関数 getStaticPaths() をエクスポート {
 戻る {
 パス: new Array(20).fill(0).map((a, i) => ({ params: { id: String(i + 1) } }))、
 フォールバック: 'ブロッキング'、
 }
}
「」

主に、getServerSideProps が getStaticProps に置き換えられ、静的ページを生成するための getStaticPaths が追加されます。上記の getStaticPaths は、ID 1 ～ 20 のページを生成することを意味します。では、ID 21 のユーザーにアクセスするとどうなるでしょうか。ここでは `fallback: 'blocking'` が設定されているため、サーバー側のレンダリング部分が引き続き使用されます。ただし、「fallback: fasle」を設定した場合、user/21 にアクセスすると 404 のプロンプトが表示されます。

平たく言えば、サーバー側の処理を必要としない一連の静的ページを生成するため、データを変更するとサーバー側で再構築する必要があるという欠点が実際にはより顕著になります。一方、サーバー側のレンダリングは、完全な再構築を必要とせずにデータを動的に処理できます。

### ISR インクリメンタル静的生成

あまり詳しく説明せずに、ドキュメント [データの取得: 増分静的再生成 Next.js (nextjs.org)](https://nextjs.org/docs/basic-features/data-fetching/incremental-static) をお読みください。 -再生)

## APIインターフェース

上記のデータはすべて [JSONPlaceholder](http://jsonplaceholder.typicode.com/) を呼び出すことで提供される仮想データです。次にデータ インターフェイスを提供する場合は、生成された pages/api にそれを記述するだけです。ルート ルールはコンポーネントの場合と同じです。たとえば、pages/api/hello.ts は api/hello にマッピングされ、ブラウザは [http://localhost:3000/api/hello](http://localhost:3000/api/hello) にアクセスして ` を取得します。 {"名前 ": "ジョン・ドゥ"}`

```tsx title="hello.ts"
'next' からインポート タイプ { NextApiRequest, NextApiResponse }

データ型 = {
 名前: 文字列
}

デフォルト関数ハンドラーをエクスポート(req: NextApiRequest, res: NextApiResponse<Data>) {
 res.status(200).json({ 名前: 'ジョン ドゥ' })
}
「」

ここでの req と res は、ほとんどのノード バックエンド フレームワークと同じであり、ここでの記述方法はサーバーレスと一貫しています (これはサーバーレスである必要があります)。

上記は get リクエストですが、post リクエストの場合はどうでしょうか? http リクエストメソッドに関係なく、リクエストメソッドは req.method によって取得されます。区別したい場合は、次のコードを使用できます。

```tsx
デフォルト関数ハンドラーをエクスポート(req, res) {
 if (req.method === 'POST') {
 // POSTリクエストを処理する
 } それ以外 {
 // 他の HTTP メソッドを処理します
 }
}
「」

### 単純な CRUD を作成する

上記の関数のいくつかを理解したので、使い慣れた CRUD を考え出すこともできます。例として記事投稿は次のとおりです

ここではデータエンドに sqlite が使用されており、構成は表示されず、主要なコア機能のみが表示されます。

```typescript title="api/post/index.ts"
'next' からインポート タイプ { NextApiRequest, NextApiResponse }
「../../../lib/db」からデータベースをインポートします

デフォルト関数ハンドラーをエクスポート(req: NextApiRequest、res: NextApiResponse) {
 試す {
 スイッチ (必須メソッド) {
 ケース「GET」:
 db.all(`select * from post`, (err, rows) => {
 res.status(200).json(行)
 })
 壊す
 ケース「ポスト」:
 const {タイトル、コンテンツ} = req.body

 db.get(`post(title, content) に挿入 value(?, ?)`, [title, content], (err, rows) => {
 res.status(200).json(行)
 })
 壊す
 }
 } キャッチ (エラー) {
 res.status(500).end()
 }
}
```

```typescript title="api/post/[id].ts"
'next' からインポート タイプ { NextApiRequest, NextApiResponse }
「../../../lib/db」からデータベースをインポートします

デフォルト関数ハンドラーをエクスポート(req: NextApiRequest、res: NextApiResponse) {
 const {id} = 要求クエリ
 const { タイトル、内容 } = req.body

 試す {
 スイッチ (必須メソッド) {
 ケース「GET」:
 db.get(`select * from post where id=$id`, { $id: id }, (err, rows) => {
 res.status(200).json(行)
 })
 壊す
 '置く' の場合:
 db.get(
 `投稿セット title=?,content=? where id=?` を更新します
 [タイトル、コンテンツ、ID]、
 (エラー、行) => {
 res.status(200).json(行)
 }、
 )
 '削除'の場合:
 db.get(`id=$id の投稿から削除`, { $id: id }, (err, rows) => {
 res.status(200).json(行)
 })
 }
 } キャッチ (エラー) {
 res.status(500).end()
 }
}
```

RESTFUL スタイルに準拠するため、post の下に 2 つのファイルが書き込まれます。このとき、[http://localhost:3000/api/post](http://localhost:3000/api/post/2) をリクエストします。すべての記事データと基本的な CRUD が実装されています。

ここで SQL を記述するのは非常に面倒です。主に、このデモを複雑にしすぎたくないため、str または typeorm を使用しませんでした。実際のプロジェクトで使用する方がよいでしょう。

もちろん、これはバックエンド API インターフェースのデモです。フロントエンドの表示や書き込みに関しては、通常のフロントエンド開発と大きな違いはありません。基本的なバックエンド フレームワークでできることは、Next がバックエンドで多くのことを実行できることです。一般的には、インターフェイス転送、ミドルウェアなどとして使用されます。結局のところ、Next の主な強みはサーバー側のレンダリング機能です。

## パッケージのデプロイメント

導入に関して言えば、nextjs の親会社である [Vercel] (https://vercel.com) とは切っても切り離せない関係にあります。Vercel については以前に関連記事を書いたので、ここでは詳しく説明しません。

nextjs を vercel にデプロイするのは非常に簡単です。プロジェクトを github ウェアハウスにプッシュし、次に vercel で新しいプロジェクトを作成し、nextjs ウェアハウスを選択して「デプロイ」をクリックし、デプロイを待ちます。デプロイメントについては、こちらの記事をご覧ください [Vercel Deployment Personal Blog](https://kuizuo.cn/develop/Vercel Deployment Personal Blog)

これで、 [kz-next-app-demo.vercel.app](https://kz-next-app-demo.vercel.app/) にアクセスしてプロジェクトにアクセスし、 `/api/post` にアクセスしてみてください。 `user/1` を見てみましょう。

親会社にふさわしいとしか言​​いようがない。

他の展開についてはどうですか？ nextjs を使用しているということですが、デプロイメント用に独自のサーバーを構築することをまだ検討していますか?

## 要約する

今回の全体的なプロセスは比較的単純です。将来、完全なプロジェクトを作成するには nextjs を使用する必要があります (~~nuxt.js~~)。