---
ms.openlocfilehash: 70df2dfceb1d2df527acfecab894b28019c6febb
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274383"
---
<!-- markdownlint-disable MD002 MD041 -->

この演習では [、Express](http://expressjs.com/)を使用してOfficeアドイン ソリューションを作成します。 ソリューションは 2 つの部分で構成されます。

- 静的 HTML ファイルおよび JavaScript ファイルとして実装されたアドイン。
- アドインNode.js提供し、アドインのデータを取得する Web API を実装する /Express サーバー。

## <a name="create-the-server"></a>サーバーを作成する

1. コマンド ライン インターフェイス (CLI) を開き、プロジェクトを作成するディレクトリに移動し、次のコマンドを実行してファイルpackage.js生成します。

    ```Shell
    yarn init
    ```

    必要に応じて、プロンプトの値を入力します。 不明な場合は、既定値で問題ありません。

1. 依存関係をインストールするには、次のコマンドを実行します。

    ```Shell
    yarn add express@4.17.1 express-promise-router@4.0.1 dotenv@8.2.0 node-fetch@2.6.1 jsonwebtoken@8.5.1@
    yarn add jwks-rsa@1.11.0 @azure/msal-node@1.0.0-beta.1 @microsoft/microsoft-graph-client@2.1.1
    yarn add date-fns@2.16.1 date-fns-tz@1.0.12 isomorphic-fetch@3.0.0 windows-iana@4.2.1
    yarn add -D typescript@4.0.5 ts-node@9.0.0 nodemon@2.0.6 @types/node@14.14.7 @types/express@4.17.9
    yarn add -D @types/node-fetch@2.5.7 @types/jsonwebtoken@8.5.0 @types/microsoft-graph@1.26.0
    yarn add -D @types/office-js@1.0.147 @types/jquery@3.5.4 @types/isomorphic-fetch@0.0.35
    ```

1. 次のコマンドを実行して、ファイルにtsconfig.jsを生成します。

    ```Shell
    tsc --init
    ```

1. テキスト **エディターで ./tsconfig.js** を開き、次の変更を行います。

    - 値を `target` 次に変更します `es6` 。
    - 値の非圧縮 `outDir` を解除し、次に設定します `./dist` 。
    - 値の非圧縮 `rootDir` を解除し、次に設定します `./src` 。

1. **./package.jsを開き**、次のプロパティを JSON に追加します。

    ```json
    "scripts": {
      "start": "nodemon ./src/server.ts",
      "build": "tsc --project ./"
    },
    ```

1. 次のコマンドを実行して、アドインの開発証明書を生成してインストールします。

    ```Shell
    npx office-addin-dev-certs install
    ```

    確認を求めるメッセージが表示されたら、操作を確認します。 コマンドが完了すると、次のような出力が表示されます。

    ```Shell
    You now have trusted access to https://localhost.
    Certificate: <path>\localhost.crt
    Key: <path>\localhost.key
    ```

1. プロジェクトのルートに **.env** という名前の新しいファイルを作成し、次のコードを追加します。

    :::code language="ini" source="../demo/graph-tutorial/example.env":::

    `PATH_TO_LOCALHOST.CRT`localhost.crt へのパスと、前のコマンドによる `PATH_TO_LOCALHOST.KEY` localhost.key 出力へのパスに置き換える。

1. src という名前のプロジェクトのルートに新しいディレクトリを作成 **します**。

1. **./src** ディレクトリに addin と api の 2 **つのディレクトリ** を作成 **します**。

1. **./src/api** ディレクトリに **auth.ts** という名前の新しいファイルを作成し、次のコードを追加します。

    ```typescript
    import Router from 'express-promise-router';

    const authRouter = Router();

    // TODO: Implement this router

    export default authRouter;
    ```

1. **./src/api** ディレクトリに **graph.ts** という名前の新しいファイルを作成し、次のコードを追加します。

    ```typescript
    import Router from 'express-promise-router';

    const graphRouter = Router();

    // TODO: Implement this router

    export default graphRouter;
    ```

1. **./src** ディレクトリに **server.ts という** 名前の新しいファイルを作成し、次のコードを追加します。

    :::code language="typescript" source="../demo/graph-tutorial/src/server.ts" id="ServerSnippet":::

## <a name="create-the-add-in"></a>アドインを作成する

1. **./src/addin** ディレクトリに **taskpane.html** という名前の新しいファイルを作成し、次のコードを追加します。

    :::code language="html" source="../demo/graph-tutorial/src/addin/taskpane.html" id="TaskPaneHtmlSnippet":::

1. **./src/addin** ディレクトリに **taskpane.css** という名前の新しいファイルを作成し、次のコードを追加します。

    :::code language="css" source="../demo/graph-tutorial/src/addin/taskpane.css":::

1. **./src/addin** ディレクトリに **taskpane.js** という名前の新しいファイルを作成し、次のコードを追加します。

    ```javascript
    // TEMPORARY CODE TO VERIFY ADD-IN LOADS
    'use strict';

    Office.onReady(info => {
      if (info.host === Office.HostType.Excel) {
        $(function() {
          $('p').text('Hello World!!');
        });
      }
    });
    ```

1. assets という名前の **.src/addin ディレクトリに新しい** ディレクトリを **作成します**。

1. 次の表に従って、このディレクトリに 3 つの PNG ファイルを追加します。

    | ファイル名   | ピクセル単位のサイズ |
    |-------------|----------------|
    | icon-80.png | 80x80          |
    | icon-32.png | 32x32          |
    | icon-16.png | 16x16          |

    > [!NOTE]
    > この手順に必要な任意のイメージを使用できます。 このサンプルで使用されている画像は [、GitHub](https://github.com/microsoftgraph/msgraph-training-office-addin/demo/graph-tutorial/src/addin/assets)から直接ダウンロードすることもできます。

1. マニフェストという名前のプロジェクトのルートに新しいディレクトリを作成 **します**。

1. **./manifest** フォルダーに **manifest.xml** という名前の新しいファイルを作成し、次のコードを追加します。 次 `NEW_GUID_HERE` のような新しい GUID に置き換える `b4fa03b8-1eb6-4e8b-a380-e0476be9e019` 。

    :::code language="xml" source="../demo/graph-tutorial/manifest/manifest.xml":::

## <a name="side-load-the-add-in-in-excel"></a>Excel でアドインをサイドロードする

1. 次のコマンドを実行して、サーバーを起動します。

    ```Shell
    yarn start
    ```

1. ブラウザーを開き、参照します `https://localhost:3000/taskpane.html` 。 メッセージが表示 `Not loaded` されます。

1. ブラウザーで、ブラウザーに移動 [Office.com](https://www.office.com/) サインインします。 左側 **のツール** バーで [作成] を選択し、[スプレッドシート] を **選択します**。

    ![ページの [作成] メニューのOffice.com](images/office-select-excel.png)

1. [挿入 **] タブ** を選択し、アドイン **Office選択します**。

1. [ **マイ アドインのアップロード] を選択し、[** 参照] を **選択します**。 **./manifest/manifest.xmlファイルをアップロード** します。

1. [ホーム **] タブの** [予定表のインポート] ボタン **を選択して** 作業ウィンドウを開きます。

    ![[ホーム] タブの [予定表のインポート] ボタンのスクリーンショット](images/get-started.png)

1. 作業ウィンドウが開くと、メッセージが表示 `Hello World!` されます。

    ![Hello World メッセージのスクリーンショット](images/hello-world.png)
