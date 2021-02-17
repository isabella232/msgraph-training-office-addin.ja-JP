---
ms.openlocfilehash: 69d77c19cb2c589086df2b4bf19eeadf41aad58a
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274391"
---
<!-- markdownlint-disable MD002 MD041 -->

この演習では、アドインで Office アドインのシングル サインオン [(SSO)](https://docs.microsoft.com/office/dev/add-ins/develop/sso-in-office-add-ins) を有効にし、代理フローをサポートするために Web API を拡張 [します](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow)。 これは、Microsoft Graph を呼び出すのに必要な OAuth アクセス トークンを取得するために必要です。

## <a name="overview"></a>概要

Office SSO はアクセス トークンを提供しますが、そのトークンはアドインが独自の Web API を呼び出す場合にのみ有効です。 Microsoft Graph への直接アクセスは有効ではありません。 このプロセスは次のように機能します。

1. アドインは getAccessToken を呼び出して [トークンを取得します](https://docs.microsoft.com/javascript/api/office-runtime/officeruntime.auth?view=common-js#getaccesstoken-options-)。 このトークンの対象ユーザー (クレーム) は、アドインのアプリ登録の `aud` アプリケーション ID です。
1. アドインは、Web API を呼び出す際に、ヘッダーで `Authorization` このトークンを送信します。
1. Web API はトークンを検証し、代理フローを使用してこのトークンを Microsoft Graph トークンと交換します。 この新しいトークンの対象ユーザーは次の値です `https://graph.microsoft.com` 。
1. Web API は、新しいトークンを使用して Microsoft Graph を呼び出し、アドインに結果を返します。

## <a name="configure-the-solution"></a>ソリューションを構成する

1. **./.env を** 開き、アプリ登録のアプリケーション ID と `AZURE_APP_ID` クライアント `AZURE_CLIENT_SECRET` シークレットで更新します。

    > [!IMPORTANT]
    > git などのソース管理を使用している場合は **、.env** ファイルをソース管理から除外して、アプリ ID とクライアント シークレットが誤って漏洩しないようにする良い時期です。

1. **./manifest/manifest.xml** 開き、すべてのインスタンスをアプリ登録の `YOUR_APP_ID_HERE` アプリケーション ID に置き換える。

1. **config.js** という名前の **./src/addin** ディレクトリに新しいファイルを作成し、アプリ登録のアプリケーション ID に置き換え、次のコード `YOUR_APP_ID_HERE` を追加します。

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/config.example.js":::

## <a name="implement-sign-in"></a>サインインの実装

1. **./src/api/auth.ts** を開き、ファイルの上部に次の `import` ステートメントを追加します。

    ```typescript
    import jwt, { SigningKeyCallback, JwtHeader } from 'jsonwebtoken';
    import jwksClient from 'jwks-rsa';
    import * as msal from '@azure/msal-node';
    ```

1. ステートメントの後に次のコードを `import` 追加します。

    :::code language="typescript" source="../demo/graph-tutorial/src/api/auth.ts" id="TokenExchangeSnippet":::

    このコード [は、MSAL 機密](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/initialize-confidential-client-application.md)クライアントを初期化し、アドインによって送信されたトークンから Graph トークンを取得する関数をエクスポートします。

1. 行の前に次のコードを `export default authRouter;` 追加します。

    :::code language="typescript" source="../demo/graph-tutorial/src/api/auth.ts" id="GetAuthStatusSnippet":::

    このコードは、アドイン トークンが Graph トークンと無音で交換される可能性をチェックする API ( `GET /auth/status` ) を実装します。 アドインは、この API を使用して、ユーザーに対話型ログインを表示する必要があるかどうかを判断します。

1. **./src/addin/taskpane.js** 開き、次のコードをファイルに追加します。

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="AuthUiSnippet":::

    このコードは、UI を更新し、ダイアログ API を使用して対話型Office [フロー](https://docs.microsoft.com/office/dev/add-ins/develop/dialog-api-in-office-add-ins) を開始する関数を追加します。

1. 次の関数を追加して、一時的なメイン UI を実装します。

    ```javascript
    function showMainUi() {
      $('.container').empty();
      $('<p/>', {
        class: 'ms-fontSize-24 ms-fontWeight-bold',
        text: 'Authenticated!'
      }).appendTo('.container');
    }
    ```

1. 既存の通話を次 `Office.onReady` の呼び出しに置き換える。

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="OfficeReadySnippet":::

    このコードの動作を検討します。

    - 作業ウィンドウは、最初に読み込まれると、アドインの Web API を対象範囲にしたトークンを取得 `getAccessToken` するために呼び出します。
    - このトークンを使用して API を呼び出し、ユーザーが Microsoft Graph のスコープにまだ同意した `/auth/status` のか確認します。
        - ユーザーが同意していない場合は、ポップアップ ウィンドウを使用して、対話型ログインでユーザーの同意を取得します。
        - ユーザーが同意した場合は、メイン UI が読み込まれます。

### <a name="getting-user-consent"></a>ユーザーの同意を得る

アドインが SSO を使用している場合でも、ユーザーはアドインが Microsoft Graph を介してデータにアクセスする方法に同意する必要があります。 同意を得るプロセスは 1 回のプロセスです。 ユーザーが同意を得た後は、ユーザーの操作なしで Graph トークンと SSO トークンを交換できます。 このセクションでは [、msal-browser](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser)を使用してアドインに同意エクスペリエンスを実装します。

1. **./src/addin** ディレクトリに新しいファイルを作成し、consent.js **コードを** 追加します。

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/consent.js" id="ConsentJsSnippet":::

    このコードはユーザーのログインを行い、アプリの登録で構成されている Microsoft Graph のアクセス許可のセットを要求します。

1. consent.html という名前の **./src/addin** ディレクトリに **新しいファイルを作成** し、次のコードを追加します。

    :::code language="html" source="../demo/graph-tutorial/src/addin/consent.html" id="ConsentHtmlSnippet":::

    このコードは、ファイルを読み込む基本的な HTML **ページconsent.js** します。 このページはポップアップ ダイアログに読み込まれます。

1. 変更内容をすべて保存し、サーバーを再起動します。

1. Excel でアドイン **manifest.xml読み** 込むのと同じ手順を使用して、ファイル [を再アップロードします](02-create-app.md#side-load-the-add-in-in-excel)。

1. [ホーム **] タブの** [予定表のインポート] **ボタンを選択** して、作業ウィンドウを開きます。

1. 作業ウィンドウ **の [アクセス許可の** 付与] ボタンを選択して、ポップアップ ウィンドウで同意ダイアログを起動します。 サインインして同意します。

1. 作業ウィンドウが "Authenticated!" で更新されます。 メッセージ。 トークンは次のように確認できます。

    - お使いである開発者ツールでは、API トークンがコンソールに表示されます。
    - クライアント サーバーを実行している CLI Node.js Graph トークンが出力されます。

    これらのトークンは次の値で比較できます [https://jwt.ms](https://jwt.ms) 。 API トークンの対象ユーザー ( ) がアプリ登録のアプリケーション ID に設定され、スコープ ( ) が `aud` `scp` `access_as_user` .
