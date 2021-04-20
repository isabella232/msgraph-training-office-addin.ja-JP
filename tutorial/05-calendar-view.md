---
ms.openlocfilehash: bd5901525561f7e169f2b7c71a280883dac8c762
ms.sourcegitcommit: 8a65c826f6b229c287a782d784b6d9629aa5a3d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/20/2021
ms.locfileid: "51899416"
---
<!-- markdownlint-disable MD002 MD041 -->

この演習では、Microsoft Graph をアプリケーションに組み込む必要があります。 このアプリケーションでは [、microsoft-graph-client ライブラリ](https://github.com/microsoftgraph/msgraph-sdk-javascript) を使用して Microsoft Graph を呼び出します。

## <a name="get-calendar-events-from-outlook"></a>Outlook からカレンダー イベントを取得する

まず、API を追加して、ユーザー [の予定表から](https://docs.microsoft.com/graph/api/user-list-calendarview) 予定表ビューを取得します。

1. **./src/api/graph.ts** を開き、次のステートメントをファイルの上部 `import` に追加します。

    ```typescript
    import { zonedTimeToUtc } from 'date-fns-tz';
    import { findIana } from 'windows-iana';
    import * as graph from '@microsoft/microsoft-graph-client';
    import { Event, MailboxSettings } from 'microsoft-graph';
    import 'isomorphic-fetch';
    import { getTokenOnBehalfOf } from './auth';
    ```

1. Microsoft Graph SDK を初期化し、クライアントを返す次の関数を追加 **します**。

    :::code language="typescript" source="../demo/graph-tutorial/src/api/graph.ts" id="GetClientSnippet":::

1. 次の関数を追加して、ユーザーのタイム ゾーンをメールボックス設定から取得し、その値を IANA タイム ゾーン識別子に変換します。

    :::code language="typescript" source="../demo/graph-tutorial/src/api/graph.ts" id="GetTimeZonesSnippet":::

1. 次の関数 (行の下) を `const graphRouter = Router();` 追加して、API エンドポイント ( ) を実装します `GET /graph/calendarview` 。

    :::code language="typescript" source="../demo/graph-tutorial/src/api/graph.ts" id="GetCalendarViewSnippet":::

    このコードの動作を検討します。

    - ユーザーのタイム ゾーンを取得し、それを使用して要求された予定表ビューの開始と終了を UTC 値に変換します。
    - Graph `GET` API エンドポイントに対 `/me/calendarview` して a を実行します。
        - この関数を使用してヘッダーを設定し、返されるイベントの開始時刻と終了時刻をユーザーのタイム ゾーン `header` `Prefer: outlook.timezone` に合わせて調整します。
        - 関数を使用 `query` してパラメーターを追加 `startDateTime` `endDateTime` し、カレンダー ビューの開始と終了を設定します。
        - この関数を `select` 使用して、アドインで使用されるフィールドのみを要求します。
        - 関数を使用 `orderby` して、開始時刻で結果を並べ替える。
        - 関数を使用 `top` して、1 つの要求の結果を 25 に制限します。
    - **PageIteratorCallback** オブジェクトを使用して [](https://docs.microsoft.com/graph/sdks/paging)結果を反復処理し、結果のページが利用可能な場合は追加の要求を行います。

## <a name="update-the-ui"></a>UI の更新

次に、作業ウィンドウを更新して、ユーザーが予定表ビューの開始日と終了日を指定できます。

1. **./src/addin/taskpane.js開** き、既存の関数を次のように `showMainUi` 置き換える。

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="MainUiSnippet":::

    このコードは、ユーザーが開始日と終了日を指定できるよう、単純なフォームを追加します。 また、新しいイベントを作成するための 2 番目のフォームも実装します。 このフォームは今のところ何も行いません。次のセクションでこの機能を実装します。

1. 次のコードをファイルに追加して、予定表ビューから取得したイベントを含むテーブルをアクティブワークシートに作成します。

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="WriteToSheetSnippet":::

1. カレンダー ビュー API を呼び出す次の関数を追加します。

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="GetCalendarSnippet":::

1. すべての変更を保存し、サーバーを再起動し、Excel で作業ウィンドウを更新します (開いている作業ウィンドウを閉じて、再び開きます)。

    ![インポート フォームのスクリーンショット](images/get-calendar-view-ui.png)

1. 開始日と終了日を選択し、[インポート] を **選択します**。

    ![イベント表のスクリーンショット](images/calendar-view-table.png)
