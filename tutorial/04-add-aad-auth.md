---
ms.openlocfilehash: 69d77c19cb2c589086df2b4bf19eeadf41aad58a
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274391"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="e2139-101">この演習では、アドインで Office アドインのシングル サインオン [(SSO)](https://docs.microsoft.com/office/dev/add-ins/develop/sso-in-office-add-ins) を有効にし、代理フローをサポートするために Web API を拡張 [します](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow)。</span><span class="sxs-lookup"><span data-stu-id="e2139-101">In this exercise you will enable [Office Add-in single sign-on (SSO)](https://docs.microsoft.com/office/dev/add-ins/develop/sso-in-office-add-ins) in the add-in, and extend the web API to support [on-behalf-of flow](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow).</span></span> <span data-ttu-id="e2139-102">これは、Microsoft Graph を呼び出すのに必要な OAuth アクセス トークンを取得するために必要です。</span><span class="sxs-lookup"><span data-stu-id="e2139-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span>

## <a name="overview"></a><span data-ttu-id="e2139-103">概要</span><span class="sxs-lookup"><span data-stu-id="e2139-103">Overview</span></span>

<span data-ttu-id="e2139-104">Office SSO はアクセス トークンを提供しますが、そのトークンはアドインが独自の Web API を呼び出す場合にのみ有効です。</span><span class="sxs-lookup"><span data-stu-id="e2139-104">Office Add-in SSO provides an access token, but that token is only enables the add-in to call it's own web API.</span></span> <span data-ttu-id="e2139-105">Microsoft Graph への直接アクセスは有効ではありません。</span><span class="sxs-lookup"><span data-stu-id="e2139-105">It does not enable direct access to the Microsoft Graph.</span></span> <span data-ttu-id="e2139-106">このプロセスは次のように機能します。</span><span class="sxs-lookup"><span data-stu-id="e2139-106">The process works as follows.</span></span>

1. <span data-ttu-id="e2139-107">アドインは getAccessToken を呼び出して [トークンを取得します](https://docs.microsoft.com/javascript/api/office-runtime/officeruntime.auth?view=common-js#getaccesstoken-options-)。</span><span class="sxs-lookup"><span data-stu-id="e2139-107">The add-in gets a token by calling [getAccessToken](https://docs.microsoft.com/javascript/api/office-runtime/officeruntime.auth?view=common-js#getaccesstoken-options-).</span></span> <span data-ttu-id="e2139-108">このトークンの対象ユーザー (クレーム) は、アドインのアプリ登録の `aud` アプリケーション ID です。</span><span class="sxs-lookup"><span data-stu-id="e2139-108">This token's audience (the `aud` claim) is the application ID of the add-in's app registration.</span></span>
1. <span data-ttu-id="e2139-109">アドインは、Web API を呼び出す際に、ヘッダーで `Authorization` このトークンを送信します。</span><span class="sxs-lookup"><span data-stu-id="e2139-109">The add-in sends this token in the `Authorization` header when it makes a call to the web API.</span></span>
1. <span data-ttu-id="e2139-110">Web API はトークンを検証し、代理フローを使用してこのトークンを Microsoft Graph トークンと交換します。</span><span class="sxs-lookup"><span data-stu-id="e2139-110">The web API validates the token, then uses the on-behalf-of flow to exchange this token for a Microsoft Graph token.</span></span> <span data-ttu-id="e2139-111">この新しいトークンの対象ユーザーは次の値です `https://graph.microsoft.com` 。</span><span class="sxs-lookup"><span data-stu-id="e2139-111">This new token's audience is `https://graph.microsoft.com`.</span></span>
1. <span data-ttu-id="e2139-112">Web API は、新しいトークンを使用して Microsoft Graph を呼び出し、アドインに結果を返します。</span><span class="sxs-lookup"><span data-stu-id="e2139-112">The web API uses the new token to make calls to the Microsoft Graph, and returns the results back to the add-in.</span></span>

## <a name="configure-the-solution"></a><span data-ttu-id="e2139-113">ソリューションを構成する</span><span class="sxs-lookup"><span data-stu-id="e2139-113">Configure the solution</span></span>

1. <span data-ttu-id="e2139-114">**./.env を** 開き、アプリ登録のアプリケーション ID と `AZURE_APP_ID` クライアント `AZURE_CLIENT_SECRET` シークレットで更新します。</span><span class="sxs-lookup"><span data-stu-id="e2139-114">Open **./.env** and update the `AZURE_APP_ID` and `AZURE_CLIENT_SECRET` with the application ID and client secret from your app registration.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="e2139-115">git などのソース管理を使用している場合は **、.env** ファイルをソース管理から除外して、アプリ ID とクライアント シークレットが誤って漏洩しないようにする良い時期です。</span><span class="sxs-lookup"><span data-stu-id="e2139-115">If you're using source control such as git, now would be a good time to exclude the **.env** file from source control to avoid inadvertently leaking your app ID and client secret.</span></span>

1. <span data-ttu-id="e2139-116">**./manifest/manifest.xml** 開き、すべてのインスタンスをアプリ登録の `YOUR_APP_ID_HERE` アプリケーション ID に置き換える。</span><span class="sxs-lookup"><span data-stu-id="e2139-116">Open **./manifest/manifest.xml** and replace all instances of `YOUR_APP_ID_HERE` with the application ID from your app registration.</span></span>

1. <span data-ttu-id="e2139-117">**config.js** という名前の **./src/addin** ディレクトリに新しいファイルを作成し、アプリ登録のアプリケーション ID に置き換え、次のコード `YOUR_APP_ID_HERE` を追加します。</span><span class="sxs-lookup"><span data-stu-id="e2139-117">Create a new file in the **./src/addin** directory named **config.js** and add the following code, replacing `YOUR_APP_ID_HERE` with the application ID from your app registration.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/config.example.js":::

## <a name="implement-sign-in"></a><span data-ttu-id="e2139-118">サインインの実装</span><span class="sxs-lookup"><span data-stu-id="e2139-118">Implement sign-in</span></span>

1. <span data-ttu-id="e2139-119">**./src/api/auth.ts** を開き、ファイルの上部に次の `import` ステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="e2139-119">Open **./src/api/auth.ts** and add the following `import` statements at the top of the file.</span></span>

    ```typescript
    import jwt, { SigningKeyCallback, JwtHeader } from 'jsonwebtoken';
    import jwksClient from 'jwks-rsa';
    import * as msal from '@azure/msal-node';
    ```

1. <span data-ttu-id="e2139-120">ステートメントの後に次のコードを `import` 追加します。</span><span class="sxs-lookup"><span data-stu-id="e2139-120">Add the following code after the `import` statements.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/api/auth.ts" id="TokenExchangeSnippet":::

    <span data-ttu-id="e2139-121">このコード [は、MSAL 機密](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/initialize-confidential-client-application.md)クライアントを初期化し、アドインによって送信されたトークンから Graph トークンを取得する関数をエクスポートします。</span><span class="sxs-lookup"><span data-stu-id="e2139-121">This code [initializes an MSAL confidential client](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/initialize-confidential-client-application.md), and exports a function to get a Graph token from the token sent by the add-in.</span></span>

1. <span data-ttu-id="e2139-122">行の前に次のコードを `export default authRouter;` 追加します。</span><span class="sxs-lookup"><span data-stu-id="e2139-122">Add the following code before the `export default authRouter;` line.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/api/auth.ts" id="GetAuthStatusSnippet":::

    <span data-ttu-id="e2139-123">このコードは、アドイン トークンが Graph トークンと無音で交換される可能性をチェックする API ( `GET /auth/status` ) を実装します。</span><span class="sxs-lookup"><span data-stu-id="e2139-123">This code implements an API (`GET /auth/status`) that checks if the add-in token can be silently exchanged for a Graph token.</span></span> <span data-ttu-id="e2139-124">アドインは、この API を使用して、ユーザーに対話型ログインを表示する必要があるかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="e2139-124">The add-in will use this API to determine if it needs to present an interactive login to the user.</span></span>

1. <span data-ttu-id="e2139-125">**./src/addin/taskpane.js** 開き、次のコードをファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="e2139-125">Open **./src/addin/taskpane.js** and add the following code to the file.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="AuthUiSnippet":::

    <span data-ttu-id="e2139-126">このコードは、UI を更新し、ダイアログ API を使用して対話型Office [フロー](https://docs.microsoft.com/office/dev/add-ins/develop/dialog-api-in-office-add-ins) を開始する関数を追加します。</span><span class="sxs-lookup"><span data-stu-id="e2139-126">This code adds functions to update the UI, and to use the [Office Dialog API](https://docs.microsoft.com/office/dev/add-ins/develop/dialog-api-in-office-add-ins) to initiate an interactive authentication flow.</span></span>

1. <span data-ttu-id="e2139-127">次の関数を追加して、一時的なメイン UI を実装します。</span><span class="sxs-lookup"><span data-stu-id="e2139-127">Add the following function to implement a temporary main UI.</span></span>

    ```javascript
    function showMainUi() {
      $('.container').empty();
      $('<p/>', {
        class: 'ms-fontSize-24 ms-fontWeight-bold',
        text: 'Authenticated!'
      }).appendTo('.container');
    }
    ```

1. <span data-ttu-id="e2139-128">既存の通話を次 `Office.onReady` の呼び出しに置き換える。</span><span class="sxs-lookup"><span data-stu-id="e2139-128">Replace the existing `Office.onReady` call with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="OfficeReadySnippet":::

    <span data-ttu-id="e2139-129">このコードの動作を検討します。</span><span class="sxs-lookup"><span data-stu-id="e2139-129">Consider what this code does.</span></span>

    - <span data-ttu-id="e2139-130">作業ウィンドウは、最初に読み込まれると、アドインの Web API を対象範囲にしたトークンを取得 `getAccessToken` するために呼び出します。</span><span class="sxs-lookup"><span data-stu-id="e2139-130">When the task pane first loads, it calls `getAccessToken` to get a token scoped for the add-in's web API.</span></span>
    - <span data-ttu-id="e2139-131">このトークンを使用して API を呼び出し、ユーザーが Microsoft Graph のスコープにまだ同意した `/auth/status` のか確認します。</span><span class="sxs-lookup"><span data-stu-id="e2139-131">It uses that token to call the `/auth/status` API to check if the user has given consent to the Microsoft Graph scopes yet.</span></span>
        - <span data-ttu-id="e2139-132">ユーザーが同意していない場合は、ポップアップ ウィンドウを使用して、対話型ログインでユーザーの同意を取得します。</span><span class="sxs-lookup"><span data-stu-id="e2139-132">If the user has not consented, it uses a pop-up window to get the user's consent through an interactive login.</span></span>
        - <span data-ttu-id="e2139-133">ユーザーが同意した場合は、メイン UI が読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="e2139-133">If the user has consented, it loads the main UI.</span></span>

### <a name="getting-user-consent"></a><span data-ttu-id="e2139-134">ユーザーの同意を得る</span><span class="sxs-lookup"><span data-stu-id="e2139-134">Getting user consent</span></span>

<span data-ttu-id="e2139-135">アドインが SSO を使用している場合でも、ユーザーはアドインが Microsoft Graph を介してデータにアクセスする方法に同意する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e2139-135">Even though the add-in is using SSO, the user still has to consent to the add-in accessing their data via Microsoft Graph.</span></span> <span data-ttu-id="e2139-136">同意を得るプロセスは 1 回のプロセスです。</span><span class="sxs-lookup"><span data-stu-id="e2139-136">Getting consent is a one-time process.</span></span> <span data-ttu-id="e2139-137">ユーザーが同意を得た後は、ユーザーの操作なしで Graph トークンと SSO トークンを交換できます。</span><span class="sxs-lookup"><span data-stu-id="e2139-137">Once the user has granted consent, the SSO token can be exchanged for a Graph token without any user interaction.</span></span> <span data-ttu-id="e2139-138">このセクションでは [、msal-browser](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser)を使用してアドインに同意エクスペリエンスを実装します。</span><span class="sxs-lookup"><span data-stu-id="e2139-138">In this section you'll implement the consent experience in the add-in using [msal-browser](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser).</span></span>

1. <span data-ttu-id="e2139-139">**./src/addin** ディレクトリに新しいファイルを作成し、consent.js **コードを** 追加します。</span><span class="sxs-lookup"><span data-stu-id="e2139-139">Create a new file in the **./src/addin** directory named **consent.js** and add the following code.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/consent.js" id="ConsentJsSnippet":::

    <span data-ttu-id="e2139-140">このコードはユーザーのログインを行い、アプリの登録で構成されている Microsoft Graph のアクセス許可のセットを要求します。</span><span class="sxs-lookup"><span data-stu-id="e2139-140">This code does login for the user, requesting the set of Microsoft Graph permissions that are configured on the app registration.</span></span>

1. <span data-ttu-id="e2139-141">consent.html という名前の **./src/addin** ディレクトリに **新しいファイルを作成** し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="e2139-141">Create a new file in the **./src/addin** directory named **consent.html** and add the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/addin/consent.html" id="ConsentHtmlSnippet":::

    <span data-ttu-id="e2139-142">このコードは、ファイルを読み込む基本的な HTML **ページconsent.js** します。</span><span class="sxs-lookup"><span data-stu-id="e2139-142">This code implements a basic HTML page to load the **consent.js** file.</span></span> <span data-ttu-id="e2139-143">このページはポップアップ ダイアログに読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="e2139-143">This page will be loaded in a pop-up dialog.</span></span>

1. <span data-ttu-id="e2139-144">変更内容をすべて保存し、サーバーを再起動します。</span><span class="sxs-lookup"><span data-stu-id="e2139-144">Save all of your changes and restart the server.</span></span>

1. <span data-ttu-id="e2139-145">Excel でアドイン **manifest.xml読み** 込むのと同じ手順を使用して、ファイル [を再アップロードします](02-create-app.md#side-load-the-add-in-in-excel)。</span><span class="sxs-lookup"><span data-stu-id="e2139-145">Re-upload your **manifest.xml** file using the same steps in [Side-load the add-in in Excel](02-create-app.md#side-load-the-add-in-in-excel).</span></span>

1. <span data-ttu-id="e2139-146">[ホーム **] タブの** [予定表のインポート] **ボタンを選択** して、作業ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="e2139-146">Select the **Import Calendar** button on the **Home** tab to open the task pane.</span></span>

1. <span data-ttu-id="e2139-147">作業ウィンドウ **の [アクセス許可の** 付与] ボタンを選択して、ポップアップ ウィンドウで同意ダイアログを起動します。</span><span class="sxs-lookup"><span data-stu-id="e2139-147">Select the **Give permission** button in the task pane to launch the consent dialog in a pop-up window.</span></span> <span data-ttu-id="e2139-148">サインインして同意します。</span><span class="sxs-lookup"><span data-stu-id="e2139-148">Sign in and grant consent.</span></span>

1. <span data-ttu-id="e2139-149">作業ウィンドウが "Authenticated!" で更新されます。</span><span class="sxs-lookup"><span data-stu-id="e2139-149">The task pane updates with an "Authenticated!"</span></span> <span data-ttu-id="e2139-150">メッセージ。</span><span class="sxs-lookup"><span data-stu-id="e2139-150">message.</span></span> <span data-ttu-id="e2139-151">トークンは次のように確認できます。</span><span class="sxs-lookup"><span data-stu-id="e2139-151">You can check the tokens as follows.</span></span>

    - <span data-ttu-id="e2139-152">お使いである開発者ツールでは、API トークンがコンソールに表示されます。</span><span class="sxs-lookup"><span data-stu-id="e2139-152">In your brower's developer tools, the API token is shown in the Console.</span></span>
    - <span data-ttu-id="e2139-153">クライアント サーバーを実行している CLI Node.js Graph トークンが出力されます。</span><span class="sxs-lookup"><span data-stu-id="e2139-153">In your CLI where you are running the Node.js server, the Graph token is printed.</span></span>

    <span data-ttu-id="e2139-154">これらのトークンは次の値で比較できます [https://jwt.ms](https://jwt.ms) 。</span><span class="sxs-lookup"><span data-stu-id="e2139-154">You can compare these token at [https://jwt.ms](https://jwt.ms).</span></span> <span data-ttu-id="e2139-155">API トークンの対象ユーザー ( ) がアプリ登録のアプリケーション ID に設定され、スコープ ( ) が `aud` `scp` `access_as_user` .</span><span class="sxs-lookup"><span data-stu-id="e2139-155">Notice that the API token's audience (`aud`) is set to the application ID of your app registration, and the scope (`scp`) is `access_as_user`.</span></span>
