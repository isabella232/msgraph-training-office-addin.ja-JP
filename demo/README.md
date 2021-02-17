---
ms.openlocfilehash: fed56663591ac36e4defcae3a8d0a222f86e3026
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274371"
---
# <a name="how-to-run-the-completed-project"></a>完成したプロジェクトを実行する方法

## <a name="prerequisites"></a>前提条件

このフォルダーで完了したプロジェクトを実行するには、以下が必要です。

- [Node.js](https://nodejs.org) コンピューター [にインストールされている新](https://yarnpkg.com/) しいファイルと新しいファイル。 (**注:** このチュートリアルは、ノード バージョン 14.15.0 およびノード バージョン 1.22.0 で記述されています。 このガイドの手順は他のバージョンでも動作する可能性がありますが、テストは行ってはいではありません)。
- メールボックスがメールボックスを持つ個人の Microsoft アカウントOutlook.com Microsoft の仕事用アカウントまたは学校アカウント。

Microsoft アカウントをお持ちない場合は、無料アカウントを取得するためのオプションが 2 つ提供されています。

- 新しい [個人用 Microsoft アカウントにサインアップできます](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。
- Office [365 開発者プログラムにサインアップして、365](https://developer.microsoft.com/office/dev-program) サブスクリプションを無料Office取得できます。

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>Azure Active Directory 管理センターに Web アプリケーションを登録する

1. ブラウザーを開き、[Azure Active Directory 管理センター](https://aad.portal.azure.com)へ移動します。 **個人用アカウント** (別名: Microsoft アカウント)、または **職場/学校アカウント** を使用してログインします。

1. 左側のナビゲーションで **[Azure Active Directory]** を選択し、それから **[管理]** で **[アプリの登録]** を選択します。

    ![アプリの登録のスクリーンショット ](/tutorial/images/app-registrations.png)

1. **[新規登録]** を選択します。 **[アプリケーションを登録]** ページで、次のように値を設定します。

    - `Office Add-in Graph Tutorial` に **[名前]** を設定します。
    - **[サポートされているアカウントの種類]** を **[任意の組織のディレクトリ内のアカウントと個人用の Microsoft アカウント]** に設定します。
    - **[リダイレクト URI]** で、最初のドロップダウン リストを `Single-page application (SPA)` に設定し、それから `https://localhost:3000/consent.html` に値を設定します。

    ![[アプリケーションを登録する] ページのスクリーンショット](/tutorial/images/register-an-app.png)

1. **[登録]** を選択します。 [Office **Graph** チュートリアル] ページで、アプリケーション **(クライアント) ID** の値をコピーして保存します。次の手順で必要になります。

    ![新しいアプリ登録のアプリケーション ID のスクリーンショット](/tutorial/images/application-id.png)

1. **[管理]** で **[証明書とシークレット]** を選択します。 
            **[新しいクライアント シークレット]** ボタンを選択します。 **[説明]** に値を入力して、**[有効期限]** のオプションのいずれかを選び、**[追加]** を選択します。

1. クライアント シークレットの値をコピーしてから、このページから移動します。 次の手順で行います。

    > [!IMPORTANT]
    > このクライアント シークレットは今後表示されないため、今必ずコピーするようにしてください。

1. [管理 **] で [API のアクセス** 許可 **] を選択し**、[アクセス許可の **追加] を選択します**。

1. **[Microsoft Graph] を選択** し、[**委任されたアクセス許可] を選択します**。

1. 次のアクセス許可を選択し、[アクセス許可の追加 **] を選択します**。

    - **Calendars.ReadWrite** - これにより、アプリはユーザーの予定表の読み取りおよび書き込みを実行できます。
    - **MailboxSettings.Read** - これにより、アプリはメールボックス設定からユーザーのタイム ゾーンを取得できます。

    ![構成されているアクセス許可のスクリーンショット](/tutorial/images/configured-permissions.png)

## <a name="configure-office-add-in-single-sign-on"></a>アドインOfficeシングル サインオンを構成する

アプリの登録を更新して、Office シングル サインオン [(SSO) をサポートします](https://docs.microsoft.com/office/dev/add-ins/develop/sso-in-office-add-ins)。

1. [API **の公開] を選択します**。 [この **API で定義されているスコープ** ] セクションで、[スコープの追加 **] を選択します**。 アプリケーション ID **URI** の設定を求めるメッセージが表示されたら、アプリケーション ID に置き換える値 `api://localhost:3000/YOUR_APP_ID_HERE` `YOUR_APP_ID_HERE` を設定します。 [保存 **] を選択して続行します**。

1. フィールドに次のように入力し、[範囲の追加 **] を選択します**。

    - **スコープ名:**`access_as_user`
    - **同意できるユーザー: 管理者とユーザー**
    - **管理者の同意の表示名:**`Access the app as the user`
    - **管理者の同意の説明:**`Allows Office Add-ins to call the app's web APIs as the current user.`
    - **ユーザーの同意の表示名:**`Access the app as you`
    - **ユーザーの同意の説明:**`Allows Office Add-ins to call the app's web APIs as you.`
    - **状態: 有効**

    ![[範囲の追加] フォームのスクリーンショット](/tutorial/images/add-scope.png)

1. [承認済み **クライアント アプリケーション] セクションで** 、[クライアント アプリケーションの **追加] を選択します**。 次の一覧からクライアント ID を入力し、[承認済みスコープ] の下のスコープを有効にして、[アプリケーションの追加]**を選択します**。 リスト内の各クライアント ID に対してこのプロセスを繰り返します。

    - `d3590ed6-52b3-4102-aeff-aad2292ab01c` (Microsoft Office)
    - `ea5a67f6-b6f3-4338-b240-c655ddc3cc8e` (Microsoft Office)
    - `57fb890c-0dab-4253-a5e0-7188c88b2bb4` (Office on the web)
    - `08e18876-6177-487e-b8b5-cf950c1e598c` (Office on the web)

## <a name="install-development-certificates"></a>開発証明書をインストールする

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

1. localhost.crt と localhost.key へのパスをコピーします。次の手順で必要です。

## <a name="update-the-manifest"></a>マニフェストを更新する

1. 新しい **manifest.xml** 開き、次の変更を行います。
    1. 次 `NEW_GUID_HERE` のような新しい GUID に置き換える `b4fa03b8-1eb6-4e8b-a380-e0476be9e019` 。
    1. アプリの登録から、すべての `YOUR_APP_ID_HERE` インスタンスをアプリケーション ID に置き換える。

## <a name="configure-the-sample"></a>サンプルを構成する

1. ファイルの名前 `example.env` を変更します `.env` 。
1. ファイルを `.env` 編集し、次の変更を行います。
    1. アプリ `YOUR_APP_ID_HERE` 登録ポータル **から取得** したアプリケーション ID に置き換える。
    1. アプリ `YOUR_CLIENT_SECRET_HERE` 登録ポータルから受け取ったクライアント シークレットに置き換える。
    1. コマンド `PATH_TO_LOCALHOST.CRT` の出力から localhost.crt ファイルへのパスに置き換 `npx office-addin-dev-certs install` える。
    1. コマンド `PATH_TO_LOCALHOST.KEY` の出力から localhost.key ファイルへのパスに置き換 `npx office-addin-dev-certs install` える。

1. ファイルの名前 `config.example.js` を変更します `config.js` 。
1. ファイルを `config.js` 編集し、次の変更を行います。
    1. アプリ `YOUR_APP_ID_HERE` 登録ポータル **から取得** したアプリケーション ID に置き換える。
1. コマンド ライン インターフェイス (CLI) で、このディレクトリに移動し、次のコマンドを実行して要件をインストールします。

    ```Shell
    yarn install
    ```

## <a name="run-the-sample"></a>サンプルを実行する

1. CLI で次のコマンドを実行して、アプリケーションを起動します。

    ```Shell
    yarn start
    ```

1. ブラウザーで、ブラウザーに移動 [Office.com](https://www.office.com/) サインインします。 左側 **のツール** バーで [作成] を選択し、[スプレッドシート] を選択 **します**。

    ![ページの [作成] メニューのOffice.com](/tutorial/images/office-select-excel.png)

1. [挿入 **] タブ** を選択し、アドイン **Office選択します**。

1. [ **マイ アドインのアップロード] を選択し、[** 参照] を **選択します**。 ファイルを **manifest.xml** します。

1. [ホーム **] タブの** [予定表のインポート] ボタン **を選択して** 作業ウィンドウを開きます。

    ![[ホーム] タブの [予定表のインポート] ボタンのスクリーンショット](/tutorial/images/get-started.png)
