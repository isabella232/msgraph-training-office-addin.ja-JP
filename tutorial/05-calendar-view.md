---
ms.openlocfilehash: ade142f1518d9bd56fa1472889721a4c8995bc3c
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274323"
---
<!-- markdownlint-disable MD002 MD041 -->

この演習では、Microsoft Graph をアプリケーションに組み込む必要があります。 このアプリケーションでは [、microsoft-graph-client ライブラリ](https://github.com/microsoftgraph/msgraph-sdk-javascript) を使用して Microsoft Graph を呼び出します。

## <a name="get-calendar-events-from-outlook"></a>Outlook からカレンダー イベントを取得する

まず、ユーザーの予定表から予定表ビューを [取得する](https://docs.microsoft.com/graph/api/user-list-calendarview) API を追加します。

1. **./src/api/graph.ts** を開き、ファイルの一番上に次の `import` ステートメントを追加します。

    ```typescript
    import { zonedTimeToUtc } from 'date-fns-tz';
    import { findOneIana } from 'windows-iana';
    import * as graph from '@microsoft/microsoft-graph-client';
    import { Event, MailboxSettings } from 'microsoft-graph';
    import 'isomorphic-fetch';
    import { getTokenOnBehalfOf } from './auth';
    ```

1. Microsoft Graph SDK を初期化し、クライアントを返す次の関数を追加 **します**。

    :::code language="typescript" source="../demo/graph-tutorial/src/api/graph.ts" id="GetClientSnippet":::

1. 次の関数を追加して、ユーザーのメールボックス設定からユーザーのタイム ゾーンを取得し、その値を IANA タイム ゾーン識別子に変換します。

    :::code language="typescript" source="../demo/graph-tutorial/src/api/graph.ts" id="GetTimeZonesSnippet":::

1. API エンドポイント ( ) を実装するために、次の関数 (行の下 `const graphRouter = Router();` ) を追加します `GET /graph/calendarview` 。

    :::code language="typescript" source="../demo/graph-tutorial/src/api/graph.ts" id="GetCalendarViewSnippet":::

    このコードの動作を検討します。

    - ユーザーのタイム ゾーンを取得し、それを使用して要求されたカレンダー ビューの開始と終了を UTC 値に変換します。
    - これは `GET` `/me/calendarview` 、Graph API エンドポイントに対して行います。
        - 関数を使用してヘッダーを設定し、返されたイベントの開始時刻と終了時刻をユーザーのタイム ゾーン `header` `Prefer: outlook.timezone` に調整します。
        - この関数を `query` 使用して、カレンダー ビューの開始と終了を設定するパラメーター `startDateTime` `endDateTime` とパラメーターを追加します。
        - この関数を `select` 使用して、アドインで使用されるフィールドのみを要求します。
        - この関数を `orderby` 使用して、開始時刻で結果を並べ替える。
        - この関数を使用 `top` して、1 つの要求の結果を 25 に制限します。
    - **PageIteratorCallback** オブジェクトを使用して [](https://docs.microsoft.com/graph/sdks/paging)結果を反復処理し、より多くの結果ページが使用可能な場合は追加の要求を行います。

## <a name="update-the-ui"></a>UI を更新する

次に、作業ウィンドウを更新して、ユーザーがカレンダー ビューの開始日と終了日を指定できます。

1. **./src/addin/taskpane.js** 開き、既存の関数を次のように `showMainUi` 置き換える。

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="MainUiSnippet":::

    このコードは、ユーザーが開始日と終了日を指定できる簡単なフォームを追加します。 また、新しいイベントを作成するための 2 番目のフォームも実装します。 このフォームでは、今のところ何も行いません。次のセクションでこの機能を実装します。

1. 次のコードをファイルに追加して、カレンダー ビューから取得したイベントを含むテーブルをアクティブ ワークシートに作成します。

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="WriteToSheetSnippet":::

1. カレンダー ビュー API を呼び出す次の関数を追加します。

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="GetCalendarSnippet":::

1. すべての変更を保存し、サーバーを再起動し、Excel で作業ウィンドウを更新します (開いている作業ウィンドウを閉じて、再び開きます)。

    ![インポート フォームのスクリーンショット](images/get-calendar-view-ui.png)

1. 開始日と終了日を選択し、[インポート] を **選択します**。

    ![イベント表のスクリーンショット](images/calendar-view-table.png)
