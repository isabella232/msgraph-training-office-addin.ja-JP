---
ms.openlocfilehash: 89bc4baff47ae895c7c0dfd34a8f8d4d0da091f5
ms.sourcegitcommit: 8a65c826f6b229c287a782d784b6d9629aa5a3d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/20/2021
ms.locfileid: "51899435"
---
<!-- markdownlint-disable MD002 MD041 -->

この演習では、Azure Active Directory 管理センターを使用AD新しい Azure AD Web アプリケーション登録を作成します。

1. ブラウザーを開き、[Azure Active Directory 管理センター](https://aad.portal.azure.com)に移動します。 **個人用アカウント** (別名: Microsoft アカウント)、または **職場/学校アカウント** を使用してログインします。

1. 左側のナビゲーションで **[Azure Active Directory]** を選択し、それから **[管理]** で **[アプリの登録]** を選択します。

    ![アプリの登録のスクリーンショット ](images/app-registrations.png)

1. **[新規登録]** を選択します。 **[アプリケーションを登録]** ページで、次のように値を設定します。

    - `Office Add-in Graph Tutorial` に **[名前]** を設定します。
    - **[サポートされているアカウントの種類]** を **[任意の組織のディレクトリ内のアカウントと個人用の Microsoft アカウント]** に設定します。
    - **[リダイレクト URI]** で、最初のドロップダウン リストを `Single-page application (SPA)` に設定し、それから `https://localhost:3000/consent.html` に値を設定します。

    ![[アプリケーションを登録する] ページのスクリーンショット](images/register-an-app.png)

1. **[登録]** を選択します。 [アドイン **Officeの** チュートリアル] ページで、 **アプリケーション (クライアント) ID** の値をコピーして保存します。次の手順で必要になります。

    ![新しいアプリ登録のアプリケーション ID のスクリーンショット](images/application-id.png)

1. **[管理]** の下の **[認証]** を選択します。 [暗黙的な **付与] セクションを見** つけて **、Access トークンと** ID トークン **を有効にします**。 **[保存]** を選択します。

    ![[暗黙的な許可] セクションのスクリーンショット](./images/aad-implicit-grant.png)

1. **[管理]** で **[証明書とシークレット]** を選択します。 
            **[新しいクライアント シークレット]** ボタンを選択します。 **[説明]** に値を入力して、**[有効期限]** のオプションのいずれかを選び、**[追加]** を選択します。

1. クライアント シークレットの値をコピーしてから、このページから移動します。 次の手順で行います。

    > [!IMPORTANT]
    > このクライアント シークレットは今後表示されないため、今必ずコピーするようにしてください。

1. [管理 **] で [API アクセス** 許可] **を選択し**、[アクセス許可 **の追加] を選択します**。

1. [Microsoft **Graph] を選択** し、[ **委任されたアクセス許可] を選択します**。

1. 次のアクセス許可を選択し、[アクセス許可の **追加] を選択します**。

    - **offline_access** - これにより、アプリが期限切れになったときにアクセス トークンを更新できます。
    - **Calendars.ReadWrite** - これにより、アプリはユーザーの予定表に対する読み取りおよび書き込みを行います。
    - **MailboxSettings.Read** - これにより、アプリはメールボックス設定からユーザーのタイム ゾーンを取得できます。

    ![構成されているアクセス許可のスクリーンショット](images/configured-permissions.png)

## <a name="configure-office-add-in-single-sign-on"></a>アドインOfficeシングル サインオンの構成

このセクションでは、アプリの登録を更新して、Office シングル サインオン [(SSO) をサポートします](https://docs.microsoft.com/office/dev/add-ins/develop/sso-in-office-add-ins)。

1. [API **の公開] を選択します**。 [この **API で定義されたスコープ] セクションで** 、[スコープの **追加] を選択します**。 アプリケーション ID URI の設定を求めるメッセージが **表示されたら**、値をアプリケーション ID に `api://localhost:3000/YOUR_APP_ID_HERE` `YOUR_APP_ID_HERE` 置き換えるに設定します。 [保存 **して続行] を選択します**。

1. 次のようにフィールドに入力し、[スコープの追加] **を選択します**。

    - **スコープ名:**`access_as_user`
    - **同意できるユーザー:管理者とユーザー**
    - **管理者の同意表示名:**`Access the app as the user`
    - **管理者の同意の説明:**`Allows Office Add-ins to call the app's web APIs as the current user.`
    - **ユーザーの同意表示名:**`Access the app as you`
    - **ユーザーの同意の説明:**`Allows Office Add-ins to call the app's web APIs as you.`
    - **状態: 有効**

    ![[スコープの追加] フォームのスクリーンショット](images/add-scope.png)

1. [承認済 **みクライアント アプリケーション] セクションで、[** クライアント アプリケーション **の追加] を選択します**。 次の一覧からクライアント ID を入力し、[承認済みスコープ] でスコープを有効にし、[アプリケーションの追加]**を選択します**。 リスト内のクライアント ID ごとにこのプロセスを繰り返します。

    - `d3590ed6-52b3-4102-aeff-aad2292ab01c` (Microsoft Office)
    - `ea5a67f6-b6f3-4338-b240-c655ddc3cc8e` (Microsoft Office)
    - `57fb890c-0dab-4253-a5e0-7188c88b2bb4` (Office on the web)
    - `08e18876-6177-487e-b8b5-cf950c1e598c` (Office on the web)
