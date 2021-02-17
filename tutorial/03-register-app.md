---
ms.openlocfilehash: a336095a238eefeae22dac86d29e3140e8d94bf1
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274336"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="d3ac2-101">この演習では、Azure Active Directory 管理センターを使用AD新しい Azure AD Web アプリケーションの登録を作成します。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="d3ac2-102">ブラウザーを開き、[Azure Active Directory 管理センター](https://aad.portal.azure.com)へ移動します。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="d3ac2-103">**個人用アカウント** (別名: Microsoft アカウント)、または **職場/学校アカウント** を使用してログインします。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="d3ac2-104">左側のナビゲーションで **[Azure Active Directory]** を選択し、それから **[管理]** で **[アプリの登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="d3ac2-105">アプリの登録のスクリーンショット</span><span class="sxs-lookup"><span data-stu-id="d3ac2-105">A screenshot of the App registrations</span></span> ](images/app-registrations.png)

1. <span data-ttu-id="d3ac2-106">**[新規登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-106">Select **New registration**.</span></span> <span data-ttu-id="d3ac2-107">**[アプリケーションを登録]** ページで、次のように値を設定します。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-107">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="d3ac2-108">`Office Add-in Graph Tutorial` に **[名前]** を設定します。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-108">Set **Name** to `Office Add-in Graph Tutorial`.</span></span>
    - <span data-ttu-id="d3ac2-109">**[サポートされているアカウントの種類]** を **[任意の組織のディレクトリ内のアカウントと個人用の Microsoft アカウント]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-109">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="d3ac2-110">**[リダイレクト URI]** で、最初のドロップダウン リストを `Single-page application (SPA)` に設定し、それから `https://localhost:3000/consent.html` に値を設定します。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-110">Under **Redirect URI**, set the first drop-down to `Single-page application (SPA)` and set the value to `https://localhost:3000/consent.html`.</span></span>

    ![[アプリケーションを登録する] ページのスクリーンショット](images/register-an-app.png)

1. <span data-ttu-id="d3ac2-112">**[登録]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-112">Select **Register**.</span></span> <span data-ttu-id="d3ac2-113">[Office **Graph** チュートリアル] ページで、アプリケーション **(クライアント) ID** の値をコピーして保存します。次の手順で必要になります。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-113">On the **Office Add-in Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新しいアプリ登録のアプリケーション ID のスクリーンショット](images/application-id.png)

1. <span data-ttu-id="d3ac2-115">**[管理]** で **[証明書とシークレット]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-115">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="d3ac2-116">
            \*\*[新しいクライアント シークレット]** ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-116">Select the **New client secret** button.</span></span> <span data-ttu-id="d3ac2-117">**[説明]** に値を入力して、**[有効期限]** のオプションのいずれかを選び、**[追加]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-117">Enter a value in **Description** and select one of the options for **Expires** and select **Add**.</span></span>

1. <span data-ttu-id="d3ac2-118">クライアント シークレットの値をコピーしてから、このページから移動します。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-118">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="d3ac2-119">次の手順で行います。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-119">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="d3ac2-120">このクライアント シークレットは今後表示されないため、今必ずコピーするようにしてください。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-120">This client secret is never shown again, so make sure you copy it now.</span></span>

1. <span data-ttu-id="d3ac2-121">[管理 **] で [API のアクセス** 許可 **] を選択し**、[アクセス許可の **追加] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-121">Select **API permissions** under **Manage**, then select **Add a permission**.</span></span>

1. <span data-ttu-id="d3ac2-122">**[Microsoft Graph] を選択** し、[**委任されたアクセス許可] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-122">Select **Microsoft Graph**, then **Delegated permissions**.</span></span>

1. <span data-ttu-id="d3ac2-123">次のアクセス許可を選択し、[アクセス許可の追加 **] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-123">Select the following permissions, then select **Add permissions**.</span></span>

    - <span data-ttu-id="d3ac2-124">**offline_access** - これにより、アプリは期限切れになったときにアクセス トークンを更新できます。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-124">**offline_access** - this will allow the app to refresh access tokens when they expire.</span></span>
    - <span data-ttu-id="d3ac2-125">**Calendars.ReadWrite** - これにより、アプリはユーザーの予定表を読み書きできます。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-125">**Calendars.ReadWrite** - this will allow the app to read and write to the user's calendar.</span></span>
    - <span data-ttu-id="d3ac2-126">**MailboxSettings.Read** - これにより、アプリはメールボックス設定からユーザーのタイム ゾーンを取得できます。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-126">**MailboxSettings.Read** - this will allow the app to get the user's time zone from their mailbox settings.</span></span>

    ![構成されているアクセス許可のスクリーンショット](images/configured-permissions.png)

## <a name="configure-office-add-in-single-sign-on"></a><span data-ttu-id="d3ac2-128">アドインOfficeシングル サインオンを構成する</span><span class="sxs-lookup"><span data-stu-id="d3ac2-128">Configure Office Add-in single sign-on</span></span>

<span data-ttu-id="d3ac2-129">このセクションでは、アプリの登録を更新して、Office シングル サインオン [(SSO) をサポートします](https://docs.microsoft.com/office/dev/add-ins/develop/sso-in-office-add-ins)。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-129">In this section you'll update the app registration to support [Office Add-in single sign-on (SSO)](https://docs.microsoft.com/office/dev/add-ins/develop/sso-in-office-add-ins).</span></span>

1. <span data-ttu-id="d3ac2-130">[API **の公開] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-130">Select **Expose an API**.</span></span> <span data-ttu-id="d3ac2-131">[この **API で定義されているスコープ** ] セクションで、[スコープの追加 **] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-131">In the **Scopes defined by this API** section, select **Add a scope**.</span></span> <span data-ttu-id="d3ac2-132">アプリケーション ID **URI** の設定を求めるメッセージが表示されたら、アプリケーション ID に置き換える値 `api://localhost:3000/YOUR_APP_ID_HERE` `YOUR_APP_ID_HERE` を設定します。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-132">When prompted to set an **Application ID URI**, set the value to `api://localhost:3000/YOUR_APP_ID_HERE`, replacing `YOUR_APP_ID_HERE` with the application ID.</span></span> <span data-ttu-id="d3ac2-133">[保存 **] を選択して続行します**。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-133">Choose **Save and continue**.</span></span>

1. <span data-ttu-id="d3ac2-134">フィールドに次のように入力し、[範囲の追加 **] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-134">Fill in the fields as follows and select **Add scope**.</span></span>

    - <span data-ttu-id="d3ac2-135">**スコープ名:**`access_as_user`</span><span class="sxs-lookup"><span data-stu-id="d3ac2-135">**Scope name:** `access_as_user`</span></span>
    - <span data-ttu-id="d3ac2-136">**同意できるユーザー: 管理者とユーザー**</span><span class="sxs-lookup"><span data-stu-id="d3ac2-136">**Who can consent?: Admins and users**</span></span>
    - <span data-ttu-id="d3ac2-137">**管理者の同意の表示名:**`Access the app as the user`</span><span class="sxs-lookup"><span data-stu-id="d3ac2-137">**Admin consent display name:** `Access the app as the user`</span></span>
    - <span data-ttu-id="d3ac2-138">**管理者の同意の説明:**`Allows Office Add-ins to call the app's web APIs as the current user.`</span><span class="sxs-lookup"><span data-stu-id="d3ac2-138">**Admin consent description:** `Allows Office Add-ins to call the app's web APIs as the current user.`</span></span>
    - <span data-ttu-id="d3ac2-139">**ユーザーの同意の表示名:**`Access the app as you`</span><span class="sxs-lookup"><span data-stu-id="d3ac2-139">**User consent display name:** `Access the app as you`</span></span>
    - <span data-ttu-id="d3ac2-140">**ユーザーの同意の説明:**`Allows Office Add-ins to call the app's web APIs as you.`</span><span class="sxs-lookup"><span data-stu-id="d3ac2-140">**User consent description:** `Allows Office Add-ins to call the app's web APIs as you.`</span></span>
    - <span data-ttu-id="d3ac2-141">**状態: 有効**</span><span class="sxs-lookup"><span data-stu-id="d3ac2-141">**State: Enabled**</span></span>

    ![[範囲の追加] フォームのスクリーンショット](images/add-scope.png)

1. <span data-ttu-id="d3ac2-143">[承認済み **クライアント アプリケーション] セクションで** 、[クライアント アプリケーションの **追加] を選択します**。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-143">In the **Authorized client applications** section, select **Add a client application**.</span></span> <span data-ttu-id="d3ac2-144">次の一覧からクライアント ID を入力し、[承認済みスコープ] の下のスコープを有効にして、[アプリケーションの追加]**を選択します**。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-144">Enter a client ID from the following list, enable the scope under **Authorized scopes**, and select **Add application**.</span></span> <span data-ttu-id="d3ac2-145">リスト内の各クライアント ID に対してこのプロセスを繰り返します。</span><span class="sxs-lookup"><span data-stu-id="d3ac2-145">Repeat this process for each of the client IDs in the list.</span></span>

    - <span data-ttu-id="d3ac2-146">`d3590ed6-52b3-4102-aeff-aad2292ab01c` (Microsoft Office)</span><span class="sxs-lookup"><span data-stu-id="d3ac2-146">`d3590ed6-52b3-4102-aeff-aad2292ab01c` (Microsoft Office)</span></span>
    - <span data-ttu-id="d3ac2-147">`ea5a67f6-b6f3-4338-b240-c655ddc3cc8e` (Microsoft Office)</span><span class="sxs-lookup"><span data-stu-id="d3ac2-147">`ea5a67f6-b6f3-4338-b240-c655ddc3cc8e` (Microsoft Office)</span></span>
    - <span data-ttu-id="d3ac2-148">`57fb890c-0dab-4253-a5e0-7188c88b2bb4` (Office on the web)</span><span class="sxs-lookup"><span data-stu-id="d3ac2-148">`57fb890c-0dab-4253-a5e0-7188c88b2bb4` (Office on the web)</span></span>
    - <span data-ttu-id="d3ac2-149">`08e18876-6177-487e-b8b5-cf950c1e598c` (Office on the web)</span><span class="sxs-lookup"><span data-stu-id="d3ac2-149">`08e18876-6177-487e-b8b5-cf950c1e598c` (Office on the web)</span></span>
