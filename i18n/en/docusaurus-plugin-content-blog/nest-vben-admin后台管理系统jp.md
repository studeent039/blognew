---
slug: nest-vben-admin
title: nest-vben-admin后台管理系统
date: 2022-05-08
authors: ragroup
tags: [project, admin, vue, nest]
keywords: [project, admin, vue, nest]
description: 一款基于 NestJs + TypeScript + TypeORM + Redis + MySql + Vben Admin 编写的一款前后端分离的权限管理系统
image: /img/project/nest-vben-admin.png
---

私が初めて Web 開発を学んだとき、Web ブログを書くことに加えて、2 番目の選択肢は、データ管理を必要とするさまざまなプロジェクトに適用できるバックエンド管理システムにすぎませんでした。

**NestJs + TypeScript + TypeORM + Redis + MySql + Vben Admin** に基づいて記述されたフロントエンドとバックエンドの分離された権限管理システム

デモアドレス: [KzAdmin](https://admin.kuizuo.cn) 管理者アカウント: admin パスワード: 123456

<!-- truncate -->

![image-20220505171231754](https://img.kuizuo.cn/image-20220505171231754.png)

＃＃ フロントエンド

**[Vben Admin](https://vvbin.cn/doc-next/) に基づいて開発され、Vue3、Vite、TypeScript などの最新テクノロジー スタックを使用し、共通の機能コンポーネント、権限検証が組み込まれています。 、動的ルーティング。 **

ウェアハウスアドレス: https://github.com/kuizuo/kz-vue-admin

### [プロジェクト構造](https://vvbin.cn/doc-next/guide/#%E7%9B%AE%E5%BD%95%E8%AF%B4%E6%98%8E)

```bash
├── build # スクリプト関連のパッケージ化
│   ├── config # 設定ファイル
│   ├── generate # ジェネレータ
│   ├── script # スクリプト
│   └── vite # vite設定
├── mock # モックフォルダー
├── public # 公共静态资源目录
├── src # 主目录
│   ├── api # 接口文件
│   ├── assets # 资源文件
│   │   ├── icons # icon sprite 图标文件夹
│   │   ├── images # 项目存放图片的文件夹
│   │   └── svg # 项目存放svg图片的文件夹
│   ├── components # 公共组件
│   ├── design # 样式文件
│   ├── directives # 指令
│   ├── enums # 枚举/常量
│   ├── hooks # hook
│   │   ├── component # 组件相关hook
│   │   ├── core # 基础hook
│   │   ├── event # 事件相关hook
│   │   ├── setting # 配置相关hook
│   │   └── web # web相关hook
│   ├── layouts # 布局文件
│   │   ├── default # 默认布局
│   │   ├── iframe # iframe布局
│   │   └── page # 页面布局
│   ├── locales # 多语言
│   ├── logics # 逻辑
│   ├── main.ts # 主入口
│   ├── router # 路由配置
│   ├── settings # 项目配置
│   │   ├── componentSetting.ts # 组件配置
│   │   ├── designSetting.ts # 样式配置
│   │   ├── encryptionSetting.ts # 加密配置
│   │   ├── localeSetting.ts # 多语言配置
│   │   ├── projectSetting.ts # 项目配置
│   │   └── siteSetting.ts # 站点配置
│   ├── store # 数据仓库
│   ├── utils # 工具类
│   └── views # 页面
├── test # 测试
│   └── server # 测试用到的服务
│       ├── api # 测试服务器
│       ├── upload # 测试上传服务器
│       └── websocket # 测试ws服务器
├── types # 类型文件
├── vite.config.ts # vite配置文件
└── windi.config.ts # windcss配置文件
```

### プロジェクトを開始する

ノード プロジェクトを管理するには、pnpm パッケージ マネージャーを使用することをお勧めします。インストールするには、「npm install -g pnpm」を​​使用します。

```bash
pnpm install

pnpm run dev
```

走行結果

```bash
  vite v2.9.5 dev server running at:

  > Network:  https://192.168.184.1:3100/
  > Local:    https://localhost:3100/

  ready in 5057ms.
```
> 注: 開発環境で初めてプロジェクトをロードする場合は、少し時間がかかります (Vite は依存関係を動的に解決します)。

フロントエンド プロジェクトの仕様の詳細については、非常に詳細な [Vben Admin Document](https://vvbin.cn/doc-next/guide/introduction.html) を直接参照してください。

＃＃ 後部

**NestJs + TypeScript + TypeORM + Redis + MySql に基づいて作成されたフロントエンドとバックエンドの分離権限管理システム**

ウェアハウスアドレス: https://github.com/kuizuo/kz-nest-admin

### [プロジェクト構造](https://blog.si-yee.com/sf-admin-cli/nest/usage.html#%E7%9B%AE%E5%BD%95%E7%BB%93 %E6%9E%84%E8%AF%B4%E6%98%8E)

```bash
|─setup-swagger.ts # Swaager文档配置
|─main.ts # 主入口
|─config # 配置文件
|─shared
| |─redis # redisModule
| | |─redis.module.ts
| | |─redis.interface.ts
| | |─redis.constants.ts
| |─shared.module.ts
| |─services # 全局通用Provider
|─app.module.ts
|─mission
| |─mission.module.ts
| |─mission.decorator.ts # 任务装饰器，所有任务都需要定义该装饰器，否则无法运行
| |─jobs # 后台定时任务定义
|─common # 系统通用定义
| |─dto # 通用DTO定义
| |─contants
| | |─error-code.contants.ts # 系统错误码定义
| | |─decorator.contants.ts # 装饰器常量
| |─filters # 通用过滤器定义
| |─interceptors # 通用拦截器定义
| |─decorators # 通用装饰器定义
| |─exceptions # 系统内置通用异常定义
| |─class # Class Model 不使用Interface定义，使用Interface无法让Swagger识别
|─modules
| |─admin
| | |─core # 核心功能
| | | |─interceptors # 后台管理拦截器定义
| | | |─decorators # 后台管理注解定义
| | | |─provider # 后台管理提供者定义
| | | |─guards # 后台管理守卫定义
| | |─system # 系统模块定义
| | |─account # 用户账户模块定义
| | |─login # 登录模块定义
| | |─admin.module.ts # 后台管理模块
| | |─admin.constants.ts # 后台管理模块通用常量
| | |─admin.interface.ts # Admin通用interface定义
| |─ws # Socket模块
|─entities # TypeORM 实体文件定义
```

### プロジェクトを開始する

フロントエンドとバックエンドの依存関係のインストールと実行パッケージ化コマンドは同じですが、事前に.env.developmentでデータベース関連の設定を変更し、sql/init.sqlを実行してデータを初期化する必要があります。

＃＃＃ 成し遂げる

プロジェクトのディレクトリ構造設計の大部分は [sf-nest-admin](https://github.com/hackycy/sf-nest-admin) に基づいていますが、一部のデータ フィールド名は主に私自身のコーディングに合わせて変更されています。スタイル、インターフェイスのメソッド、インターフェイスの応答形式など。

同時に、これらのバックグラウンド管理デモのほとんどでは、ユーザー、役割、メニュー、部門が通常定義されています。これらの部分は今後のプロジェクトでは必要なくなる可能性が高いため、部門関連のコードを削除し、主に今後の管理プロジェクトで使用するためにこのテンプレートのセットを作成し、いくつかの無関係なモジュールを削除しました。

#### ユーザー役割権限

このシステムの最も重要な部分は権限管理ですが、このバックエンド管理システムでは、ここでの権限はメニュー、フロントエンド ルーティングによるメニューのレンダリング、およびバックエンド認証と共有されます。以下のメニューテーブルは権限テーブルとしても使用されます。

これら 3 つのテーブルの関係は次のとおりです (ここでは例として外部キーとデータベース モデルを使用します。実際のプロジェクトでは外部キーを使用しないため、その使用は推奨されません)。

![image-20220508235534026](https://img.kuizuo.cn/image-20220508235534026.png)

user-role と role-permission は両方とも多対多の関係を採用します。つまり、2 つのテーブル間の関係をマップするために新しいテーブルが作成されます。これらはすべて mysql の基本に属するため、詳細は説明しません。

権限管理で最も重要なのは権限テーブルです。このバックグラウンド管理システムにはフロントエンドの左メニューも含まれるため、ここでの権限テーブルはメニュー テーブルに置き換えられ、フィールド権限は権限値を表します。データベース内のメニュー表は以下の通りです

![image-20220508234343594](https://img.kuizuo.cn/image-20220508234343594.png)

主な分野の紹介:

- **parent**: 親子関係のあるテーブルの場合、parent_id (ここでは親) フィールドが親ノードを表すために作成されます。そうでない場合は、それが最上位ノードになります。

- **permission**: バックエンド インターフェイスに応じた許可識別子。たとえば、新しいユーザーの URL が `sys/user/add` の場合、許可識別子は通常 / を: に置き換えます。 `sys:user:add ` (主にインターフェイスの URL との混同を防ぐため)。
- **type**: 0 ディレクトリ 1 メニュー (フロントエンドコンポーネント) 2 権限 メニューと権限が混在しているため、これらを区別するためにタイプが使用されます。
- **アイコン**: 左側のメニューアイコン
- **order_no**: 左側のメニューの並べ替え
- **コンポーネント**: コンポーネント、ディレクトリはLAYOUT、メニューは対応するコンポーネント、権限はなし

これらのデータを使用して、**フロントエンド メニュー管理**、**ロールに基づいてすべてのメニューを取得**、**すべてのユーザー権限**に従って、データをツリー構造データに結合する必要があります。

##### フロントエンド メニュー管理

すべてのメニュー リスト データを取得し、再帰によって対応するメニュー ツリー構造を生成します。特定の再帰コードは、`src/modules/core/permission/index.ts` の `generatorMenu` メソッドにあります。

特定のスプライシング データが多すぎるため、コンソール (F12) -> ネットワークを開き、メニュー管理ページに移動してデータを取得できます。ここには表示されません (後のスプライシング データも同様です)。

##### ロールに基づいてすべてのメニューを取得する

まず、ユーザー ID に基づいてユーザーのすべてのロール ID を検索し、結合テーブルを通じてロール ID に対応するメニュー データを検索します。

``` 
 /**
   * 現在のユーザーのすべての権限を取得します
   */
  async getPerms(uid: number): Promise<string[]> {
    const roleIds = await this.roleService.getRoleIdByUser(uid);
    let permission: any[] = [];
    let result: any = null;
    if (includes(roleIds, this.rootRoleId)) {
      result = await this.menuRepository.find({
        permission: Not(IsNull()),
        type: 2,
      });
    } else {
      result = await this.menuRepository
        .createQueryBuilder('menu')
        .innerJoinAndSelect('sys_role_menu', 'role_menu', 'menu.id = role_menu.menu_id')
        .andWhere('role_menu.role_id IN (:...roldIds)', { roldIds: roleIds })
        .andWhere('menu.type = 2')
        .andWhere('menu.permission IS NOT NULL')
        .getMany();
    }
    if (!isEmpty(result)) {
      result.forEach((e) => {
        permission = concat(permission, e.permission.split(','));
      });
      permission = uniq(permission);
    }
    return permission;
  }
```
同じ `generatorRouters` 関数が `src/modules/core/permission/index.ts` にもあります。

##### ユーザーのすべての権限に基づく

上の例と同じですが、ここでの主な目的は権限フィールドの取得なので、条件に「menu.type = 2」と「menu.permission IS NOT NULL」を追加し、権限をつなぎ合わせます。配列。

```typescript
  /**
   * 获取当前用户的所有权限
   */
  async getPerms(uid: number): Promise<string[]> {
    const roleIds = await this.roleService.getRoleIdByUser(uid);
    let permission: any[] = [];
    let result: any = null;
    if (includes(roleIds, this.rootRoleId)) {
      result = await this.menuRepository.find({
        permission: Not(IsNull()),
        type: 2,
      });
    } else {
      result = await this.menuRepository
        .createQueryBuilder('menu')
        .innerJoinAndSelect('sys_role_menu', 'role_menu', 'menu.id = role_menu.menu_id')
        .andWhere('role_menu.role_id IN (:...roldIds)', { roldIds: roleIds })
        .andWhere('menu.type = 2')
        .andWhere('menu.permission IS NOT NULL')
        .getMany();
    }
    if (!isEmpty(result)) {
      result.forEach((e) => {
        permission = concat(permission, e.permission.split(','));
      });
      permission = uniq(permission);
    }
    return permission;
  }
```

許可の値は次のとおりです

```json
["sys:user:add"、"sys:user:delete"、"sys:user:update"、"sys:user:list"、"sys:user:info"]
```

次に、auth.guard.ts ガードで権限を取得し、認証が必要なインターフェイスに対して要求が行われるたびに、権限識別子がインターフェイス URL に変換され、その URL が含まれているかどうかが判断されます。 、アクセス権限がありません。

メニューは[メニュー管理ページ](https://admin.kuizuo.cn/#/system/menu)で操作でき、詳細はセルフテストできます。

この時点で、メニュー テーブルのデータは、権限管理と動的ルーティングの目的を達成するために、これら 3 つの部分のデータに分割されています。

#### その他の書類

[https://admin.kuizuo.cn/swagger-ui](https://admin.kuizuo.cn/swagger-ui 'https://admin.kuizuo.cn/swagger-ui') にアクセスして表示できます。 nest-vben-admin の Swagger ドキュメント

json 形式は [https://admin.kuizuo.cn/swagger-ui/json](https://admin.kuizuo.cn/swagger-ui/json 'https://admin.kuizuo.cn/swagger-) です。 ui /json')、ApiFox へのインポートに使用されます。

ApiFox オンライン リンク: [https://www.apifox.cn/apidoc/shared-7a07def2-5b82-4c71-bf57-915514f61f25](https://www.apifox.cn/apidoc/shared-7a07def2-5b82-4c71- bf57-915514f61f25 'https://www.apifox.cn/apidoc/shared-7a07def2-5b82-4c71-bf57-915514f61f25') アクセスパスワード: net-vben-admin

## 余談

実際、1 年以上前、私はその後のプロジェクトで使用するために、比較的完全なバックエンド管理システム テンプレートを作成したいと考えていました。しかし、私は長い間テンプレートのセットを書き始めていませんでした。その代わりに、ビジネスのニーズに基づいて一連の乱雑なコードを絶えず修正したり書き直したりしてきたため、メンテナンスが非常に面倒になっています。つい最近までたまたま使っていたのですが、以前書いたクソの山のようなコードも修正しました。

**私が遭遇した問題: プロジェクトの構造が乱雑で、コードのスタイルが乱雑で、コードの保守は非常に面倒です**

そこで、この冬休みに、この一連のテンプレートを改善する準備をしましたが、その時点ではプロジェクト構造の作成だけが完了し、機能の実装、テスト、デプロイが正式に完了したのは今だけです。正直に言うと、あまりに先延ばしにしていたので、このテンプレートを書くのを諦めそうになりました。しかし、ドラッグには私にとって一定の利点があるのはなぜですか?このアイデアを思いついたとき、市場にはこのテクノロジー スタックの実装がほとんどありませんでしたが、冬休み中に関連する実装を探したところ、関連するオープン ソース コードがあったので、そこから学んでプロジェクトを作成することができました。もっと面白い。

プロジェクト全体の執筆プロセスを振り返ると、1 か月未満、あるいはそれ未満かかる場合もありますが、多くの場合、プロジェクトの期限が過ぎたり、特定のテクノロジー スタックを学習したりするためのさまざまな遅延が原因です。集中して課題をこなすのが難しい理由としては、目標が大きすぎるからかもしれないし、日常生活の些細な事のせいかもしれないが、ほとんどは自分の怠惰によるものだと思う。