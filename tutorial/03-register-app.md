---
ms.openlocfilehash: 89bc4baff47ae895c7c0dfd34a8f8d4d0da091f5
ms.sourcegitcommit: 8a65c826f6b229c287a782d784b6d9629aa5a3d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/20/2021
ms.locfileid: "51899435"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="a4aad-101">この演習では、Azure Active Directory 管理センターを使用AD新しい Azure AD Web アプリケーション登録を作成します。</span><span class="sxs-lookup"><span data-stu-id="a4aad-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="a4aad-102">ブラウザーを開き、[Azure Active Directory 管理センター](https://aad.portal.azure.com)に移動します。</span><span class="sxs-lookup"><span data-stu-id="a4aad-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="a4aad-103">**個人用アカウント** (別名: Microsoft アカウント)、または **職場/学校アカウント** を使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="a4aad-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="a4aad-104">左側のナビゲーションで **[Azure Active Directory]** を選択し、それから **[管理]** で **[アプリの登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a4aad-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="a4aad-105">アプリの登録のスクリーンショット</span><span class="sxs-lookup"><span data-stu-id="a4aad-105">A screenshot of the App registrations</span></span> ](images/app-registrations.png)

1. <span data-ttu-id="a4aad-106">**[新規登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a4aad-106">Select **New registration**.</span></span> <span data-ttu-id="a4aad-107">**[アプリケーションを登録]** ページで、次のように値を設定します。</span><span class="sxs-lookup"><span data-stu-id="a4aad-107">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="a4aad-108">`Office Add-in Graph Tutorial` に **[名前]** を設定します。</span><span class="sxs-lookup"><span data-stu-id="a4aad-108">Set **Name** to `Office Add-in Graph Tutorial`.</span></span>
    - <span data-ttu-id="a4aad-109">**[サポートされているアカウントの種類]** を **[任意の組織のディレクトリ内のアカウントと個人用の Microsoft アカウント]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="a4aad-109">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="a4aad-110">**[リダイレクト URI]** で、最初のドロップダウン リストを `Single-page application (SPA)` に設定し、それから `https://localhost:3000/consent.html` に値を設定します。</span><span class="sxs-lookup"><span data-stu-id="a4aad-110">Under **Redirect URI**, set the first drop-down to `Single-page application (SPA)` and set the value to `https://localhost:3000/consent.html`.</span></span>

    ![[アプリケーションを登録する] ページのスクリーンショット](images/register-an-app.png)

1. <span data-ttu-id="a4aad-112">**[登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a4aad-112">Select **Register**.</span></span> <span data-ttu-id="a4aad-113">[アドイン **Officeの** チュートリアル] ページで、 **アプリケーション (クライアント) ID** の値をコピーして保存します。次の手順で必要になります。</span><span class="sxs-lookup"><span data-stu-id="a4aad-113">On the **Office Add-in Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新しいアプリ登録のアプリケーション ID のスクリーンショット](images/application-id.png)

1. <span data-ttu-id="a4aad-115">**[管理]** の下の **[認証]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a4aad-115">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="a4aad-116">[暗黙的な **付与] セクションを見** つけて **、Access トークンと** ID トークン **を有効にします**。</span><span class="sxs-lookup"><span data-stu-id="a4aad-116">Locate the **Implicit grant** section and enable **Access tokens** and **ID tokens**.</span></span> <span data-ttu-id="a4aad-117">**[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a4aad-117">Select **Save**.</span></span>

    ![[暗黙的な許可] セクションのスクリーンショット](./images/aad-implicit-grant.png)

1. <span data-ttu-id="a4aad-119">**[管理]** で **[証明書とシークレット]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a4aad-119">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="a4aad-120">
            \*\*[新しいクライアント シークレット]** ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="a4aad-120">Select the **New client secret** button.</span></span> <span data-ttu-id="a4aad-121">**[説明]** に値を入力して、**[有効期限]** のオプションのいずれかを選び、**[追加]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a4aad-121">Enter a value in **Description** and select one of the options for **Expires** and select **Add**.</span></span>

1. <span data-ttu-id="a4aad-122">クライアント シークレットの値をコピーしてから、このページから移動します。</span><span class="sxs-lookup"><span data-stu-id="a4aad-122">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="a4aad-123">次の手順で行います。</span><span class="sxs-lookup"><span data-stu-id="a4aad-123">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="a4aad-124">このクライアント シークレットは今後表示されないため、今必ずコピーするようにしてください。</span><span class="sxs-lookup"><span data-stu-id="a4aad-124">This client secret is never shown again, so make sure you copy it now.</span></span>

1. <span data-ttu-id="a4aad-125">[管理 **] で [API アクセス** 許可] **を選択し**、[アクセス許可 **の追加] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="a4aad-125">Select **API permissions** under **Manage**, then select **Add a permission**.</span></span>

1. <span data-ttu-id="a4aad-126">[Microsoft **Graph] を選択** し、[ **委任されたアクセス許可] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="a4aad-126">Select **Microsoft Graph**, then **Delegated permissions**.</span></span>

1. <span data-ttu-id="a4aad-127">次のアクセス許可を選択し、[アクセス許可の **追加] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="a4aad-127">Select the following permissions, then select **Add permissions**.</span></span>

    - <span data-ttu-id="a4aad-128">**offline_access** - これにより、アプリが期限切れになったときにアクセス トークンを更新できます。</span><span class="sxs-lookup"><span data-stu-id="a4aad-128">**offline_access** - this will allow the app to refresh access tokens when they expire.</span></span>
    - <span data-ttu-id="a4aad-129">**Calendars.ReadWrite** - これにより、アプリはユーザーの予定表に対する読み取りおよび書き込みを行います。</span><span class="sxs-lookup"><span data-stu-id="a4aad-129">**Calendars.ReadWrite** - this will allow the app to read and write to the user's calendar.</span></span>
    - <span data-ttu-id="a4aad-130">**MailboxSettings.Read** - これにより、アプリはメールボックス設定からユーザーのタイム ゾーンを取得できます。</span><span class="sxs-lookup"><span data-stu-id="a4aad-130">**MailboxSettings.Read** - this will allow the app to get the user's time zone from their mailbox settings.</span></span>

    ![構成されているアクセス許可のスクリーンショット](images/configured-permissions.png)

## <a name="configure-office-add-in-single-sign-on"></a><span data-ttu-id="a4aad-132">アドインOfficeシングル サインオンの構成</span><span class="sxs-lookup"><span data-stu-id="a4aad-132">Configure Office Add-in single sign-on</span></span>

<span data-ttu-id="a4aad-133">このセクションでは、アプリの登録を更新して、Office シングル サインオン [(SSO) をサポートします](https://docs.microsoft.com/office/dev/add-ins/develop/sso-in-office-add-ins)。</span><span class="sxs-lookup"><span data-stu-id="a4aad-133">In this section you'll update the app registration to support [Office Add-in single sign-on (SSO)](https://docs.microsoft.com/office/dev/add-ins/develop/sso-in-office-add-ins).</span></span>

1. <span data-ttu-id="a4aad-134">[API **の公開] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="a4aad-134">Select **Expose an API**.</span></span> <span data-ttu-id="a4aad-135">[この **API で定義されたスコープ] セクションで** 、[スコープの **追加] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="a4aad-135">In the **Scopes defined by this API** section, select **Add a scope**.</span></span> <span data-ttu-id="a4aad-136">アプリケーション ID URI の設定を求めるメッセージが **表示されたら**、値をアプリケーション ID に `api://localhost:3000/YOUR_APP_ID_HERE` `YOUR_APP_ID_HERE` 置き換えるに設定します。</span><span class="sxs-lookup"><span data-stu-id="a4aad-136">When prompted to set an **Application ID URI**, set the value to `api://localhost:3000/YOUR_APP_ID_HERE`, replacing `YOUR_APP_ID_HERE` with the application ID.</span></span> <span data-ttu-id="a4aad-137">[保存 **して続行] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="a4aad-137">Choose **Save and continue**.</span></span>

1. <span data-ttu-id="a4aad-138">次のようにフィールドに入力し、[スコープの追加] **を選択します**。</span><span class="sxs-lookup"><span data-stu-id="a4aad-138">Fill in the fields as follows and select **Add scope**.</span></span>

    - <span data-ttu-id="a4aad-139">**スコープ名:**`access_as_user`</span><span class="sxs-lookup"><span data-stu-id="a4aad-139">**Scope name:** `access_as_user`</span></span>
    - <span data-ttu-id="a4aad-140">**同意できるユーザー:管理者とユーザー**</span><span class="sxs-lookup"><span data-stu-id="a4aad-140">**Who can consent?: Admins and users**</span></span>
    - <span data-ttu-id="a4aad-141">**管理者の同意表示名:**`Access the app as the user`</span><span class="sxs-lookup"><span data-stu-id="a4aad-141">**Admin consent display name:** `Access the app as the user`</span></span>
    - <span data-ttu-id="a4aad-142">**管理者の同意の説明:**`Allows Office Add-ins to call the app's web APIs as the current user.`</span><span class="sxs-lookup"><span data-stu-id="a4aad-142">**Admin consent description:** `Allows Office Add-ins to call the app's web APIs as the current user.`</span></span>
    - <span data-ttu-id="a4aad-143">**ユーザーの同意表示名:**`Access the app as you`</span><span class="sxs-lookup"><span data-stu-id="a4aad-143">**User consent display name:** `Access the app as you`</span></span>
    - <span data-ttu-id="a4aad-144">**ユーザーの同意の説明:**`Allows Office Add-ins to call the app's web APIs as you.`</span><span class="sxs-lookup"><span data-stu-id="a4aad-144">**User consent description:** `Allows Office Add-ins to call the app's web APIs as you.`</span></span>
    - <span data-ttu-id="a4aad-145">**状態: 有効**</span><span class="sxs-lookup"><span data-stu-id="a4aad-145">**State: Enabled**</span></span>

    ![[スコープの追加] フォームのスクリーンショット](images/add-scope.png)

1. <span data-ttu-id="a4aad-147">[承認済 **みクライアント アプリケーション] セクションで、[** クライアント アプリケーション **の追加] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="a4aad-147">In the **Authorized client applications** section, select **Add a client application**.</span></span> <span data-ttu-id="a4aad-148">次の一覧からクライアント ID を入力し、[承認済みスコープ] でスコープを有効にし、[アプリケーションの追加]**を選択します**。</span><span class="sxs-lookup"><span data-stu-id="a4aad-148">Enter a client ID from the following list, enable the scope under **Authorized scopes**, and select **Add application**.</span></span> <span data-ttu-id="a4aad-149">リスト内のクライアント ID ごとにこのプロセスを繰り返します。</span><span class="sxs-lookup"><span data-stu-id="a4aad-149">Repeat this process for each of the client IDs in the list.</span></span>

    - <span data-ttu-id="a4aad-150">`d3590ed6-52b3-4102-aeff-aad2292ab01c` (Microsoft Office)</span><span class="sxs-lookup"><span data-stu-id="a4aad-150">`d3590ed6-52b3-4102-aeff-aad2292ab01c` (Microsoft Office)</span></span>
    - <span data-ttu-id="a4aad-151">`ea5a67f6-b6f3-4338-b240-c655ddc3cc8e` (Microsoft Office)</span><span class="sxs-lookup"><span data-stu-id="a4aad-151">`ea5a67f6-b6f3-4338-b240-c655ddc3cc8e` (Microsoft Office)</span></span>
    - <span data-ttu-id="a4aad-152">`57fb890c-0dab-4253-a5e0-7188c88b2bb4` (Office on the web)</span><span class="sxs-lookup"><span data-stu-id="a4aad-152">`57fb890c-0dab-4253-a5e0-7188c88b2bb4` (Office on the web)</span></span>
    - <span data-ttu-id="a4aad-153">`08e18876-6177-487e-b8b5-cf950c1e598c` (Office on the web)</span><span class="sxs-lookup"><span data-stu-id="a4aad-153">`08e18876-6177-487e-b8b5-cf950c1e598c` (Office on the web)</span></span>
