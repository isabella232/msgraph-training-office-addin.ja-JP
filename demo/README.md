---
ms.openlocfilehash: fed56663591ac36e4defcae3a8d0a222f86e3026
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274371"
---
# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="b164c-101">完成したプロジェクトを実行する方法</span><span class="sxs-lookup"><span data-stu-id="b164c-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b164c-102">前提条件</span><span class="sxs-lookup"><span data-stu-id="b164c-102">Prerequisites</span></span>

<span data-ttu-id="b164c-103">このフォルダーで完了したプロジェクトを実行するには、以下が必要です。</span><span class="sxs-lookup"><span data-stu-id="b164c-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="b164c-104">[Node.js](https://nodejs.org) コンピューター [にインストールされている新](https://yarnpkg.com/) しいファイルと新しいファイル。</span><span class="sxs-lookup"><span data-stu-id="b164c-104">[Node.js](https://nodejs.org) and [Yarn](https://yarnpkg.com/) installed on your development machine.</span></span> <span data-ttu-id="b164c-105">(**注:** このチュートリアルは、ノード バージョン 14.15.0 およびノード バージョン 1.22.0 で記述されています。</span><span class="sxs-lookup"><span data-stu-id="b164c-105">(**Note:** This tutorial was written with Node version 14.15.0 and Yarn version 1.22.0.</span></span> <span data-ttu-id="b164c-106">このガイドの手順は他のバージョンでも動作する可能性がありますが、テストは行ってはいではありません)。</span><span class="sxs-lookup"><span data-stu-id="b164c-106">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="b164c-107">メールボックスがメールボックスを持つ個人の Microsoft アカウントOutlook.com Microsoft の仕事用アカウントまたは学校アカウント。</span><span class="sxs-lookup"><span data-stu-id="b164c-107">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="b164c-108">Microsoft アカウントをお持ちない場合は、無料アカウントを取得するためのオプションが 2 つ提供されています。</span><span class="sxs-lookup"><span data-stu-id="b164c-108">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="b164c-109">新しい [個人用 Microsoft アカウントにサインアップできます](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。</span><span class="sxs-lookup"><span data-stu-id="b164c-109">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="b164c-110">Office [365 開発者プログラムにサインアップして、365](https://developer.microsoft.com/office/dev-program) サブスクリプションを無料Office取得できます。</span><span class="sxs-lookup"><span data-stu-id="b164c-110">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="b164c-111">Azure Active Directory 管理センターに Web アプリケーションを登録する</span><span class="sxs-lookup"><span data-stu-id="b164c-111">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="b164c-112">ブラウザーを開き、[Azure Active Directory 管理センター](https://aad.portal.azure.com)へ移動します。</span><span class="sxs-lookup"><span data-stu-id="b164c-112">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="b164c-113">**個人用アカウント** (別名: Microsoft アカウント)、または **職場/学校アカウント** を使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="b164c-113">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="b164c-114">左側のナビゲーションで **[Azure Active Directory]** を選択し、それから **[管理]** で **[アプリの登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b164c-114">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="b164c-115">アプリの登録のスクリーンショット</span><span class="sxs-lookup"><span data-stu-id="b164c-115">A screenshot of the App registrations</span></span> ](/tutorial/images/app-registrations.png)

1. <span data-ttu-id="b164c-116">**[新規登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b164c-116">Select **New registration**.</span></span> <span data-ttu-id="b164c-117">**[アプリケーションを登録]** ページで、次のように値を設定します。</span><span class="sxs-lookup"><span data-stu-id="b164c-117">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="b164c-118">`Office Add-in Graph Tutorial` に **[名前]** を設定します。</span><span class="sxs-lookup"><span data-stu-id="b164c-118">Set **Name** to `Office Add-in Graph Tutorial`.</span></span>
    - <span data-ttu-id="b164c-119">**[サポートされているアカウントの種類]** を **[任意の組織のディレクトリ内のアカウントと個人用の Microsoft アカウント]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="b164c-119">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="b164c-120">**[リダイレクト URI]** で、最初のドロップダウン リストを `Single-page application (SPA)` に設定し、それから `https://localhost:3000/consent.html` に値を設定します。</span><span class="sxs-lookup"><span data-stu-id="b164c-120">Under **Redirect URI**, set the first drop-down to `Single-page application (SPA)` and set the value to `https://localhost:3000/consent.html`.</span></span>

    ![[アプリケーションを登録する] ページのスクリーンショット](/tutorial/images/register-an-app.png)

1. <span data-ttu-id="b164c-122">**[登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b164c-122">Select **Register**.</span></span> <span data-ttu-id="b164c-123">[Office **Graph** チュートリアル] ページで、アプリケーション **(クライアント) ID** の値をコピーして保存します。次の手順で必要になります。</span><span class="sxs-lookup"><span data-stu-id="b164c-123">On the **Office Add-in Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新しいアプリ登録のアプリケーション ID のスクリーンショット](/tutorial/images/application-id.png)

1. <span data-ttu-id="b164c-125">**[管理]** で **[証明書とシークレット]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b164c-125">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="b164c-126">
            \*\*[新しいクライアント シークレット]** ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="b164c-126">Select the **New client secret** button.</span></span> <span data-ttu-id="b164c-127">**[説明]** に値を入力して、**[有効期限]** のオプションのいずれかを選び、**[追加]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="b164c-127">Enter a value in **Description** and select one of the options for **Expires** and select **Add**.</span></span>

1. <span data-ttu-id="b164c-128">クライアント シークレットの値をコピーしてから、このページから移動します。</span><span class="sxs-lookup"><span data-stu-id="b164c-128">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="b164c-129">次の手順で行います。</span><span class="sxs-lookup"><span data-stu-id="b164c-129">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="b164c-130">このクライアント シークレットは今後表示されないため、今必ずコピーするようにしてください。</span><span class="sxs-lookup"><span data-stu-id="b164c-130">This client secret is never shown again, so make sure you copy it now.</span></span>

1. <span data-ttu-id="b164c-131">[管理 **] で [API のアクセス** 許可 **] を選択し**、[アクセス許可の **追加] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="b164c-131">Select **API permissions** under **Manage**, then select **Add a permission**.</span></span>

1. <span data-ttu-id="b164c-132">**[Microsoft Graph] を選択** し、[**委任されたアクセス許可] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="b164c-132">Select **Microsoft Graph**, then **Delegated permissions**.</span></span>

1. <span data-ttu-id="b164c-133">次のアクセス許可を選択し、[アクセス許可の追加 **] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="b164c-133">Select the following permissions, then select **Add permissions**.</span></span>

    - <span data-ttu-id="b164c-134">**Calendars.ReadWrite** - これにより、アプリはユーザーの予定表の読み取りおよび書き込みを実行できます。</span><span class="sxs-lookup"><span data-stu-id="b164c-134">**Calendars.ReadWrite** - this will allow the app to read and write to the user's calendar.</span></span>
    - <span data-ttu-id="b164c-135">**MailboxSettings.Read** - これにより、アプリはメールボックス設定からユーザーのタイム ゾーンを取得できます。</span><span class="sxs-lookup"><span data-stu-id="b164c-135">**MailboxSettings.Read** - this will allow the app to get the user's time zone from their mailbox settings.</span></span>

    ![構成されているアクセス許可のスクリーンショット](/tutorial/images/configured-permissions.png)

## <a name="configure-office-add-in-single-sign-on"></a><span data-ttu-id="b164c-137">アドインOfficeシングル サインオンを構成する</span><span class="sxs-lookup"><span data-stu-id="b164c-137">Configure Office Add-in single sign-on</span></span>

<span data-ttu-id="b164c-138">アプリの登録を更新して、Office シングル サインオン [(SSO) をサポートします](https://docs.microsoft.com/office/dev/add-ins/develop/sso-in-office-add-ins)。</span><span class="sxs-lookup"><span data-stu-id="b164c-138">Update the app registration to support [Office Add-in single sign-on (SSO)](https://docs.microsoft.com/office/dev/add-ins/develop/sso-in-office-add-ins).</span></span>

1. <span data-ttu-id="b164c-139">[API **の公開] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="b164c-139">Select **Expose an API**.</span></span> <span data-ttu-id="b164c-140">[この **API で定義されているスコープ** ] セクションで、[スコープの追加 **] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="b164c-140">In the **Scopes defined by this API** section, select **Add a scope**.</span></span> <span data-ttu-id="b164c-141">アプリケーション ID **URI** の設定を求めるメッセージが表示されたら、アプリケーション ID に置き換える値 `api://localhost:3000/YOUR_APP_ID_HERE` `YOUR_APP_ID_HERE` を設定します。</span><span class="sxs-lookup"><span data-stu-id="b164c-141">When prompted to set an **Application ID URI**, set the value to `api://localhost:3000/YOUR_APP_ID_HERE`, replacing `YOUR_APP_ID_HERE` with the application ID.</span></span> <span data-ttu-id="b164c-142">[保存 **] を選択して続行します**。</span><span class="sxs-lookup"><span data-stu-id="b164c-142">Choose **Save and continue**.</span></span>

1. <span data-ttu-id="b164c-143">フィールドに次のように入力し、[範囲の追加 **] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="b164c-143">Fill in the fields as follows and select **Add scope**.</span></span>

    - <span data-ttu-id="b164c-144">**スコープ名:**`access_as_user`</span><span class="sxs-lookup"><span data-stu-id="b164c-144">**Scope name:** `access_as_user`</span></span>
    - <span data-ttu-id="b164c-145">**同意できるユーザー: 管理者とユーザー**</span><span class="sxs-lookup"><span data-stu-id="b164c-145">**Who can consent?: Admins and users**</span></span>
    - <span data-ttu-id="b164c-146">**管理者の同意の表示名:**`Access the app as the user`</span><span class="sxs-lookup"><span data-stu-id="b164c-146">**Admin consent display name:** `Access the app as the user`</span></span>
    - <span data-ttu-id="b164c-147">**管理者の同意の説明:**`Allows Office Add-ins to call the app's web APIs as the current user.`</span><span class="sxs-lookup"><span data-stu-id="b164c-147">**Admin consent description:** `Allows Office Add-ins to call the app's web APIs as the current user.`</span></span>
    - <span data-ttu-id="b164c-148">**ユーザーの同意の表示名:**`Access the app as you`</span><span class="sxs-lookup"><span data-stu-id="b164c-148">**User consent display name:** `Access the app as you`</span></span>
    - <span data-ttu-id="b164c-149">**ユーザーの同意の説明:**`Allows Office Add-ins to call the app's web APIs as you.`</span><span class="sxs-lookup"><span data-stu-id="b164c-149">**User consent description:** `Allows Office Add-ins to call the app's web APIs as you.`</span></span>
    - <span data-ttu-id="b164c-150">**状態: 有効**</span><span class="sxs-lookup"><span data-stu-id="b164c-150">**State: Enabled**</span></span>

    ![[範囲の追加] フォームのスクリーンショット](/tutorial/images/add-scope.png)

1. <span data-ttu-id="b164c-152">[承認済み **クライアント アプリケーション] セクションで** 、[クライアント アプリケーションの **追加] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="b164c-152">In the **Authorized client applications** section, select **Add a client application**.</span></span> <span data-ttu-id="b164c-153">次の一覧からクライアント ID を入力し、[承認済みスコープ] の下のスコープを有効にして、[アプリケーションの追加]**を選択します**。</span><span class="sxs-lookup"><span data-stu-id="b164c-153">Enter a client ID from the following list, enable the scope under **Authorized scopes**, and select **Add application**.</span></span> <span data-ttu-id="b164c-154">リスト内の各クライアント ID に対してこのプロセスを繰り返します。</span><span class="sxs-lookup"><span data-stu-id="b164c-154">Repeat this process for each of the client IDs in the list.</span></span>

    - <span data-ttu-id="b164c-155">`d3590ed6-52b3-4102-aeff-aad2292ab01c` (Microsoft Office)</span><span class="sxs-lookup"><span data-stu-id="b164c-155">`d3590ed6-52b3-4102-aeff-aad2292ab01c` (Microsoft Office)</span></span>
    - <span data-ttu-id="b164c-156">`ea5a67f6-b6f3-4338-b240-c655ddc3cc8e` (Microsoft Office)</span><span class="sxs-lookup"><span data-stu-id="b164c-156">`ea5a67f6-b6f3-4338-b240-c655ddc3cc8e` (Microsoft Office)</span></span>
    - <span data-ttu-id="b164c-157">`57fb890c-0dab-4253-a5e0-7188c88b2bb4` (Office on the web)</span><span class="sxs-lookup"><span data-stu-id="b164c-157">`57fb890c-0dab-4253-a5e0-7188c88b2bb4` (Office on the web)</span></span>
    - <span data-ttu-id="b164c-158">`08e18876-6177-487e-b8b5-cf950c1e598c` (Office on the web)</span><span class="sxs-lookup"><span data-stu-id="b164c-158">`08e18876-6177-487e-b8b5-cf950c1e598c` (Office on the web)</span></span>

## <a name="install-development-certificates"></a><span data-ttu-id="b164c-159">開発証明書をインストールする</span><span class="sxs-lookup"><span data-stu-id="b164c-159">Install development certificates</span></span>

1. <span data-ttu-id="b164c-160">次のコマンドを実行して、アドインの開発証明書を生成してインストールします。</span><span class="sxs-lookup"><span data-stu-id="b164c-160">Run the following command to generate and install development certificates for your add-in.</span></span>

    ```Shell
    npx office-addin-dev-certs install
    ```

    <span data-ttu-id="b164c-161">確認を求めるメッセージが表示されたら、操作を確認します。</span><span class="sxs-lookup"><span data-stu-id="b164c-161">If prompted for confirmation, confirm the actions.</span></span> <span data-ttu-id="b164c-162">コマンドが完了すると、次のような出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b164c-162">Once the command completes, you will see output similar to the following.</span></span>

    ```Shell
    You now have trusted access to https://localhost.
    Certificate: <path>\localhost.crt
    Key: <path>\localhost.key
    ```

1. <span data-ttu-id="b164c-163">localhost.crt と localhost.key へのパスをコピーします。次の手順で必要です。</span><span class="sxs-lookup"><span data-stu-id="b164c-163">Copy the paths to localhost.crt and localhost.key, you'll need them in the next step.</span></span>

## <a name="update-the-manifest"></a><span data-ttu-id="b164c-164">マニフェストを更新する</span><span class="sxs-lookup"><span data-stu-id="b164c-164">Update the manifest</span></span>

1. <span data-ttu-id="b164c-165">新しい **manifest.xml** 開き、次の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="b164c-165">Open the **manifest.xml** file and make the following changes.</span></span>
    1. <span data-ttu-id="b164c-166">次 `NEW_GUID_HERE` のような新しい GUID に置き換える `b4fa03b8-1eb6-4e8b-a380-e0476be9e019` 。</span><span class="sxs-lookup"><span data-stu-id="b164c-166">Replace `NEW_GUID_HERE` with a new GUID, like `b4fa03b8-1eb6-4e8b-a380-e0476be9e019`.</span></span>
    1. <span data-ttu-id="b164c-167">アプリの登録から、すべての `YOUR_APP_ID_HERE` インスタンスをアプリケーション ID に置き換える。</span><span class="sxs-lookup"><span data-stu-id="b164c-167">Replace all instances of `YOUR_APP_ID_HERE` with the application ID from your app registration.</span></span>

## <a name="configure-the-sample"></a><span data-ttu-id="b164c-168">サンプルを構成する</span><span class="sxs-lookup"><span data-stu-id="b164c-168">Configure the sample</span></span>

1. <span data-ttu-id="b164c-169">ファイルの名前 `example.env` を変更します `.env` 。</span><span class="sxs-lookup"><span data-stu-id="b164c-169">Rename the `example.env` file to `.env`.</span></span>
1. <span data-ttu-id="b164c-170">ファイルを `.env` 編集し、次の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="b164c-170">Edit the `.env` file and make the following changes.</span></span>
    1. <span data-ttu-id="b164c-171">アプリ `YOUR_APP_ID_HERE` 登録ポータル **から取得** したアプリケーション ID に置き換える。</span><span class="sxs-lookup"><span data-stu-id="b164c-171">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
    1. <span data-ttu-id="b164c-172">アプリ `YOUR_CLIENT_SECRET_HERE` 登録ポータルから受け取ったクライアント シークレットに置き換える。</span><span class="sxs-lookup"><span data-stu-id="b164c-172">Replace `YOUR_CLIENT_SECRET_HERE` with the client secret you got from the App Registration Portal.</span></span>
    1. <span data-ttu-id="b164c-173">コマンド `PATH_TO_LOCALHOST.CRT` の出力から localhost.crt ファイルへのパスに置き換 `npx office-addin-dev-certs install` える。</span><span class="sxs-lookup"><span data-stu-id="b164c-173">Replace `PATH_TO_LOCALHOST.CRT` with the path to your localhost.crt file from the output of the `npx office-addin-dev-certs install` command.</span></span>
    1. <span data-ttu-id="b164c-174">コマンド `PATH_TO_LOCALHOST.KEY` の出力から localhost.key ファイルへのパスに置き換 `npx office-addin-dev-certs install` える。</span><span class="sxs-lookup"><span data-stu-id="b164c-174">Replace `PATH_TO_LOCALHOST.KEY` with the path to your localhost.key file from the output of the `npx office-addin-dev-certs install` command.</span></span>

1. <span data-ttu-id="b164c-175">ファイルの名前 `config.example.js` を変更します `config.js` 。</span><span class="sxs-lookup"><span data-stu-id="b164c-175">Rename the `config.example.js` file to `config.js`.</span></span>
1. <span data-ttu-id="b164c-176">ファイルを `config.js` 編集し、次の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="b164c-176">Edit the `config.js` file and make the following changes.</span></span>
    1. <span data-ttu-id="b164c-177">アプリ `YOUR_APP_ID_HERE` 登録ポータル **から取得** したアプリケーション ID に置き換える。</span><span class="sxs-lookup"><span data-stu-id="b164c-177">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="b164c-178">コマンド ライン インターフェイス (CLI) で、このディレクトリに移動し、次のコマンドを実行して要件をインストールします。</span><span class="sxs-lookup"><span data-stu-id="b164c-178">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    yarn install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="b164c-179">サンプルを実行する</span><span class="sxs-lookup"><span data-stu-id="b164c-179">Run the sample</span></span>

1. <span data-ttu-id="b164c-180">CLI で次のコマンドを実行して、アプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="b164c-180">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    yarn start
    ```

1. <span data-ttu-id="b164c-181">ブラウザーで、ブラウザーに移動 [Office.com](https://www.office.com/) サインインします。</span><span class="sxs-lookup"><span data-stu-id="b164c-181">In your browser, go to [Office.com](https://www.office.com/) and sign in.</span></span> <span data-ttu-id="b164c-182">左側 **のツール** バーで [作成] を選択し、[スプレッドシート] を選択 **します**。</span><span class="sxs-lookup"><span data-stu-id="b164c-182">Select **Create** in the left-hand toolbar, then select **Spreadsheet**.</span></span>

    ![ページの [作成] メニューのOffice.com](/tutorial/images/office-select-excel.png)

1. <span data-ttu-id="b164c-184">[挿入 **] タブ** を選択し、アドイン **Office選択します**。</span><span class="sxs-lookup"><span data-stu-id="b164c-184">Select the **Insert** tab, then select **Office Add-ins**.</span></span>

1. <span data-ttu-id="b164c-185">[ **マイ アドインのアップロード] を選択し、[** 参照] を **選択します**。</span><span class="sxs-lookup"><span data-stu-id="b164c-185">Select **Upload My Add-in**, then select **Browse**.</span></span> <span data-ttu-id="b164c-186">ファイルを **manifest.xml** します。</span><span class="sxs-lookup"><span data-stu-id="b164c-186">Upload your **manifest.xml** file.</span></span>

1. <span data-ttu-id="b164c-187">[ホーム **] タブの** [予定表のインポート] ボタン **を選択して** 作業ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="b164c-187">Select the **Import Calendar** button on the **Home** tab to open the taskpane.</span></span>

    ![[ホーム] タブの [予定表のインポート] ボタンのスクリーンショット](/tutorial/images/get-started.png)
