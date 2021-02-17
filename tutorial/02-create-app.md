---
ms.openlocfilehash: 70df2dfceb1d2df527acfecab894b28019c6febb
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274383"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="47733-101">この演習では [、Express](http://expressjs.com/)を使用してOfficeアドイン ソリューションを作成します。</span><span class="sxs-lookup"><span data-stu-id="47733-101">In this exercise you will create an Office Add-in solution using [Express](http://expressjs.com/).</span></span> <span data-ttu-id="47733-102">ソリューションは 2 つの部分で構成されます。</span><span class="sxs-lookup"><span data-stu-id="47733-102">The solution will consist of two parts.</span></span>

- <span data-ttu-id="47733-103">静的 HTML ファイルおよび JavaScript ファイルとして実装されたアドイン。</span><span class="sxs-lookup"><span data-stu-id="47733-103">The add-in, implemented as static HTML and JavaScript files.</span></span>
- <span data-ttu-id="47733-104">アドインNode.js提供し、アドインのデータを取得する Web API を実装する /Express サーバー。</span><span class="sxs-lookup"><span data-stu-id="47733-104">A Node.js/Express server that serves the add-in and implements a web API to retrieve data for the add-in.</span></span>

## <a name="create-the-server"></a><span data-ttu-id="47733-105">サーバーを作成する</span><span class="sxs-lookup"><span data-stu-id="47733-105">Create the server</span></span>

1. <span data-ttu-id="47733-106">コマンド ライン インターフェイス (CLI) を開き、プロジェクトを作成するディレクトリに移動し、次のコマンドを実行してファイルpackage.js生成します。</span><span class="sxs-lookup"><span data-stu-id="47733-106">Open your command-line interface (CLI), navigate to a directory where you want to create your project, and run the following command to generate a package.json file.</span></span>

    ```Shell
    yarn init
    ```

    <span data-ttu-id="47733-107">必要に応じて、プロンプトの値を入力します。</span><span class="sxs-lookup"><span data-stu-id="47733-107">Enter values for the prompts as appropriate.</span></span> <span data-ttu-id="47733-108">不明な場合は、既定値で問題ありません。</span><span class="sxs-lookup"><span data-stu-id="47733-108">If you're unsure, the default values are fine.</span></span>

1. <span data-ttu-id="47733-109">依存関係をインストールするには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="47733-109">Run the following commands to install dependencies.</span></span>

    ```Shell
    yarn add express@4.17.1 express-promise-router@4.0.1 dotenv@8.2.0 node-fetch@2.6.1 jsonwebtoken@8.5.1@
    yarn add jwks-rsa@1.11.0 @azure/msal-node@1.0.0-beta.1 @microsoft/microsoft-graph-client@2.1.1
    yarn add date-fns@2.16.1 date-fns-tz@1.0.12 isomorphic-fetch@3.0.0 windows-iana@4.2.1
    yarn add -D typescript@4.0.5 ts-node@9.0.0 nodemon@2.0.6 @types/node@14.14.7 @types/express@4.17.9
    yarn add -D @types/node-fetch@2.5.7 @types/jsonwebtoken@8.5.0 @types/microsoft-graph@1.26.0
    yarn add -D @types/office-js@1.0.147 @types/jquery@3.5.4 @types/isomorphic-fetch@0.0.35
    ```

1. <span data-ttu-id="47733-110">次のコマンドを実行して、ファイルにtsconfig.jsを生成します。</span><span class="sxs-lookup"><span data-stu-id="47733-110">Run the following command to generate a tsconfig.json file.</span></span>

    ```Shell
    tsc --init
    ```

1. <span data-ttu-id="47733-111">テキスト **エディターで ./tsconfig.js** を開き、次の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="47733-111">Open **./tsconfig.json** in a text editor and make the following changes.</span></span>

    - <span data-ttu-id="47733-112">値を `target` 次に変更します `es6` 。</span><span class="sxs-lookup"><span data-stu-id="47733-112">Change the `target` value to `es6`.</span></span>
    - <span data-ttu-id="47733-113">値の非圧縮 `outDir` を解除し、次に設定します `./dist` 。</span><span class="sxs-lookup"><span data-stu-id="47733-113">Uncomment the `outDir` value and set it to `./dist`.</span></span>
    - <span data-ttu-id="47733-114">値の非圧縮 `rootDir` を解除し、次に設定します `./src` 。</span><span class="sxs-lookup"><span data-stu-id="47733-114">Uncomment the `rootDir` value and set it to `./src`.</span></span>

1. <span data-ttu-id="47733-115">**./package.jsを開き**、次のプロパティを JSON に追加します。</span><span class="sxs-lookup"><span data-stu-id="47733-115">Open **./package.json** and add the following property to the JSON.</span></span>

    ```json
    "scripts": {
      "start": "nodemon ./src/server.ts",
      "build": "tsc --project ./"
    },
    ```

1. <span data-ttu-id="47733-116">次のコマンドを実行して、アドインの開発証明書を生成してインストールします。</span><span class="sxs-lookup"><span data-stu-id="47733-116">Run the following command to generate and install development certificates for your add-in.</span></span>

    ```Shell
    npx office-addin-dev-certs install
    ```

    <span data-ttu-id="47733-117">確認を求めるメッセージが表示されたら、操作を確認します。</span><span class="sxs-lookup"><span data-stu-id="47733-117">If prompted for confirmation, confirm the actions.</span></span> <span data-ttu-id="47733-118">コマンドが完了すると、次のような出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="47733-118">Once the command completes, you will see output similar to the following.</span></span>

    ```Shell
    You now have trusted access to https://localhost.
    Certificate: <path>\localhost.crt
    Key: <path>\localhost.key
    ```

1. <span data-ttu-id="47733-119">プロジェクトのルートに **.env** という名前の新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="47733-119">Create a new file named **.env** in the root of your project and add the following code.</span></span>

    :::code language="ini" source="../demo/graph-tutorial/example.env":::

    <span data-ttu-id="47733-120">`PATH_TO_LOCALHOST.CRT`localhost.crt へのパスと、前のコマンドによる `PATH_TO_LOCALHOST.KEY` localhost.key 出力へのパスに置き換える。</span><span class="sxs-lookup"><span data-stu-id="47733-120">Replace `PATH_TO_LOCALHOST.CRT` with the path to localhost.crt and `PATH_TO_LOCALHOST.KEY` with the path to localhost.key output by the previous command.</span></span>

1. <span data-ttu-id="47733-121">src という名前のプロジェクトのルートに新しいディレクトリを作成 **します**。</span><span class="sxs-lookup"><span data-stu-id="47733-121">Create a new directory in the root of your project named **src**.</span></span>

1. <span data-ttu-id="47733-122">**./src** ディレクトリに addin と api の 2 **つのディレクトリ** を作成 **します**。</span><span class="sxs-lookup"><span data-stu-id="47733-122">Create two directories in the **./src** directory: **addin** and **api**.</span></span>

1. <span data-ttu-id="47733-123">**./src/api** ディレクトリに **auth.ts** という名前の新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="47733-123">Create a new file named **auth.ts** in the **./src/api** directory and add the following code.</span></span>

    ```typescript
    import Router from 'express-promise-router';

    const authRouter = Router();

    // TODO: Implement this router

    export default authRouter;
    ```

1. <span data-ttu-id="47733-124">**./src/api** ディレクトリに **graph.ts** という名前の新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="47733-124">Create a new file named **graph.ts** in the **./src/api** directory and add the following code.</span></span>

    ```typescript
    import Router from 'express-promise-router';

    const graphRouter = Router();

    // TODO: Implement this router

    export default graphRouter;
    ```

1. <span data-ttu-id="47733-125">**./src** ディレクトリに **server.ts という** 名前の新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="47733-125">Create a new file named **server.ts** in the **./src** directory and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/server.ts" id="ServerSnippet":::

## <a name="create-the-add-in"></a><span data-ttu-id="47733-126">アドインを作成する</span><span class="sxs-lookup"><span data-stu-id="47733-126">Create the add-in</span></span>

1. <span data-ttu-id="47733-127">**./src/addin** ディレクトリに **taskpane.html** という名前の新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="47733-127">Create a new file named **taskpane.html** in the **./src/addin** directory and add the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/addin/taskpane.html" id="TaskPaneHtmlSnippet":::

1. <span data-ttu-id="47733-128">**./src/addin** ディレクトリに **taskpane.css** という名前の新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="47733-128">Create a new file named **taskpane.css** in the **./src/addin** directory and add the following code.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/addin/taskpane.css":::

1. <span data-ttu-id="47733-129">**./src/addin** ディレクトリに **taskpane.js** という名前の新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="47733-129">Create a new file named **taskpane.js** in the **./src/addin** directory and add the following code.</span></span>

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

1. <span data-ttu-id="47733-130">assets という名前の **.src/addin ディレクトリに新しい** ディレクトリを **作成します**。</span><span class="sxs-lookup"><span data-stu-id="47733-130">Create a new directory in the **.src/addin** directory named **assets**.</span></span>

1. <span data-ttu-id="47733-131">次の表に従って、このディレクトリに 3 つの PNG ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="47733-131">Add three PNG files in this directory according to the following table.</span></span>

    | <span data-ttu-id="47733-132">ファイル名</span><span class="sxs-lookup"><span data-stu-id="47733-132">File name</span></span>   | <span data-ttu-id="47733-133">ピクセル単位のサイズ</span><span class="sxs-lookup"><span data-stu-id="47733-133">Size in pixels</span></span> |
    |-------------|----------------|
    | <span data-ttu-id="47733-134">icon-80.png</span><span class="sxs-lookup"><span data-stu-id="47733-134">icon-80.png</span></span> | <span data-ttu-id="47733-135">80x80</span><span class="sxs-lookup"><span data-stu-id="47733-135">80x80</span></span>          |
    | <span data-ttu-id="47733-136">icon-32.png</span><span class="sxs-lookup"><span data-stu-id="47733-136">icon-32.png</span></span> | <span data-ttu-id="47733-137">32x32</span><span class="sxs-lookup"><span data-stu-id="47733-137">32x32</span></span>          |
    | <span data-ttu-id="47733-138">icon-16.png</span><span class="sxs-lookup"><span data-stu-id="47733-138">icon-16.png</span></span> | <span data-ttu-id="47733-139">16x16</span><span class="sxs-lookup"><span data-stu-id="47733-139">16x16</span></span>          |

    > [!NOTE]
    > <span data-ttu-id="47733-140">この手順に必要な任意のイメージを使用できます。</span><span class="sxs-lookup"><span data-stu-id="47733-140">You can use any image you want for this step.</span></span> <span data-ttu-id="47733-141">このサンプルで使用されている画像は [、GitHub](https://github.com/microsoftgraph/msgraph-training-office-addin/demo/graph-tutorial/src/addin/assets)から直接ダウンロードすることもできます。</span><span class="sxs-lookup"><span data-stu-id="47733-141">You can also download the images used in this sample directly from [GitHub](https://github.com/microsoftgraph/msgraph-training-office-addin/demo/graph-tutorial/src/addin/assets).</span></span>

1. <span data-ttu-id="47733-142">マニフェストという名前のプロジェクトのルートに新しいディレクトリを作成 **します**。</span><span class="sxs-lookup"><span data-stu-id="47733-142">Create a new directory in the root of the project named **manifest**.</span></span>

1. <span data-ttu-id="47733-143">**./manifest** フォルダーに **manifest.xml** という名前の新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="47733-143">Create a new file named **manifest.xml** in the **./manifest** folder and add the following code.</span></span> <span data-ttu-id="47733-144">次 `NEW_GUID_HERE` のような新しい GUID に置き換える `b4fa03b8-1eb6-4e8b-a380-e0476be9e019` 。</span><span class="sxs-lookup"><span data-stu-id="47733-144">Replace `NEW_GUID_HERE` with a new GUID, like `b4fa03b8-1eb6-4e8b-a380-e0476be9e019`.</span></span>

    :::code language="xml" source="../demo/graph-tutorial/manifest/manifest.xml":::

## <a name="side-load-the-add-in-in-excel"></a><span data-ttu-id="47733-145">Excel でアドインをサイドロードする</span><span class="sxs-lookup"><span data-stu-id="47733-145">Side-load the add-in in Excel</span></span>

1. <span data-ttu-id="47733-146">次のコマンドを実行して、サーバーを起動します。</span><span class="sxs-lookup"><span data-stu-id="47733-146">Start the server by running the following command.</span></span>

    ```Shell
    yarn start
    ```

1. <span data-ttu-id="47733-147">ブラウザーを開き、参照します `https://localhost:3000/taskpane.html` 。</span><span class="sxs-lookup"><span data-stu-id="47733-147">Open your browser and browse to `https://localhost:3000/taskpane.html`.</span></span> <span data-ttu-id="47733-148">メッセージが表示 `Not loaded` されます。</span><span class="sxs-lookup"><span data-stu-id="47733-148">You should see a `Not loaded` message.</span></span>

1. <span data-ttu-id="47733-149">ブラウザーで、ブラウザーに移動 [Office.com](https://www.office.com/) サインインします。</span><span class="sxs-lookup"><span data-stu-id="47733-149">In your browser, go to [Office.com](https://www.office.com/) and sign in.</span></span> <span data-ttu-id="47733-150">左側 **のツール** バーで [作成] を選択し、[スプレッドシート] を **選択します**。</span><span class="sxs-lookup"><span data-stu-id="47733-150">Select **Create** in the left-hand toolbar, then select **Spreadsheet**.</span></span>

    ![ページの [作成] メニューのOffice.com](images/office-select-excel.png)

1. <span data-ttu-id="47733-152">[挿入 **] タブ** を選択し、アドイン **Office選択します**。</span><span class="sxs-lookup"><span data-stu-id="47733-152">Select the **Insert** tab, then select **Office Add-ins**.</span></span>

1. <span data-ttu-id="47733-153">[ **マイ アドインのアップロード] を選択し、[** 参照] を **選択します**。</span><span class="sxs-lookup"><span data-stu-id="47733-153">Select **Upload My Add-in**, then select **Browse**.</span></span> <span data-ttu-id="47733-154">**./manifest/manifest.xmlファイルをアップロード** します。</span><span class="sxs-lookup"><span data-stu-id="47733-154">Upload your **./manifest/manifest.xml** file.</span></span>

1. <span data-ttu-id="47733-155">[ホーム **] タブの** [予定表のインポート] ボタン **を選択して** 作業ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="47733-155">Select the **Import Calendar** button on the **Home** tab to open the taskpane.</span></span>

    ![[ホーム] タブの [予定表のインポート] ボタンのスクリーンショット](images/get-started.png)

1. <span data-ttu-id="47733-157">作業ウィンドウが開くと、メッセージが表示 `Hello World!` されます。</span><span class="sxs-lookup"><span data-stu-id="47733-157">After the taskpane opens, you should see a `Hello World!` message.</span></span>

    ![Hello World メッセージのスクリーンショット](images/hello-world.png)
