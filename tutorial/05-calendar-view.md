---
ms.openlocfilehash: bd5901525561f7e169f2b7c71a280883dac8c762
ms.sourcegitcommit: 8a65c826f6b229c287a782d784b6d9629aa5a3d0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/20/2021
ms.locfileid: "51899416"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="fab81-101">この演習では、Microsoft Graph をアプリケーションに組み込む必要があります。</span><span class="sxs-lookup"><span data-stu-id="fab81-101">In this exercise you will incorporate Microsoft Graph into the application.</span></span> <span data-ttu-id="fab81-102">このアプリケーションでは [、microsoft-graph-client ライブラリ](https://github.com/microsoftgraph/msgraph-sdk-javascript) を使用して Microsoft Graph を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="fab81-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="fab81-103">Outlook からカレンダー イベントを取得する</span><span class="sxs-lookup"><span data-stu-id="fab81-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="fab81-104">まず、API を追加して、ユーザー [の予定表から](https://docs.microsoft.com/graph/api/user-list-calendarview) 予定表ビューを取得します。</span><span class="sxs-lookup"><span data-stu-id="fab81-104">Start by adding an API to get a [calendar view](https://docs.microsoft.com/graph/api/user-list-calendarview) from the user's calendar.</span></span>

1. <span data-ttu-id="fab81-105">**./src/api/graph.ts** を開き、次のステートメントをファイルの上部 `import` に追加します。</span><span class="sxs-lookup"><span data-stu-id="fab81-105">Open **./src/api/graph.ts** and add the following `import` statements to the top of the file.</span></span>

    ```typescript
    import { zonedTimeToUtc } from 'date-fns-tz';
    import { findIana } from 'windows-iana';
    import * as graph from '@microsoft/microsoft-graph-client';
    import { Event, MailboxSettings } from 'microsoft-graph';
    import 'isomorphic-fetch';
    import { getTokenOnBehalfOf } from './auth';
    ```

1. <span data-ttu-id="fab81-106">Microsoft Graph SDK を初期化し、クライアントを返す次の関数を追加 **します**。</span><span class="sxs-lookup"><span data-stu-id="fab81-106">Add the following function to initialize the Microsoft Graph SDK and return a **Client**.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/api/graph.ts" id="GetClientSnippet":::

1. <span data-ttu-id="fab81-107">次の関数を追加して、ユーザーのタイム ゾーンをメールボックス設定から取得し、その値を IANA タイム ゾーン識別子に変換します。</span><span class="sxs-lookup"><span data-stu-id="fab81-107">Add the following function to get the user's time zone from their mailbox settings, and to convert that value to an IANA time zone identifier.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/api/graph.ts" id="GetTimeZonesSnippet":::

1. <span data-ttu-id="fab81-108">次の関数 (行の下) を `const graphRouter = Router();` 追加して、API エンドポイント ( ) を実装します `GET /graph/calendarview` 。</span><span class="sxs-lookup"><span data-stu-id="fab81-108">Add the following function (below the `const graphRouter = Router();` line) to implement an API endpoint (`GET /graph/calendarview`).</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/api/graph.ts" id="GetCalendarViewSnippet":::

    <span data-ttu-id="fab81-109">このコードの動作を検討します。</span><span class="sxs-lookup"><span data-stu-id="fab81-109">Consider what this code does.</span></span>

    - <span data-ttu-id="fab81-110">ユーザーのタイム ゾーンを取得し、それを使用して要求された予定表ビューの開始と終了を UTC 値に変換します。</span><span class="sxs-lookup"><span data-stu-id="fab81-110">It gets the user's time zone and uses that to convert the start and end of the requested calendar view into UTC values.</span></span>
    - <span data-ttu-id="fab81-111">Graph `GET` API エンドポイントに対 `/me/calendarview` して a を実行します。</span><span class="sxs-lookup"><span data-stu-id="fab81-111">It does a `GET` to the `/me/calendarview` Graph API endpoint.</span></span>
        - <span data-ttu-id="fab81-112">この関数を使用してヘッダーを設定し、返されるイベントの開始時刻と終了時刻をユーザーのタイム ゾーン `header` `Prefer: outlook.timezone` に合わせて調整します。</span><span class="sxs-lookup"><span data-stu-id="fab81-112">It uses the `header` function to set the `Prefer: outlook.timezone` header, causing the start and end times of the returned events to be adjusted to the user's time zone.</span></span>
        - <span data-ttu-id="fab81-113">関数を使用 `query` してパラメーターを追加 `startDateTime` `endDateTime` し、カレンダー ビューの開始と終了を設定します。</span><span class="sxs-lookup"><span data-stu-id="fab81-113">It uses the `query` function to add the `startDateTime` and `endDateTime` parameters, setting the start and end of the calendar view.</span></span>
        - <span data-ttu-id="fab81-114">この関数を `select` 使用して、アドインで使用されるフィールドのみを要求します。</span><span class="sxs-lookup"><span data-stu-id="fab81-114">It uses the `select` function to request only the fields used by the add-in.</span></span>
        - <span data-ttu-id="fab81-115">関数を使用 `orderby` して、開始時刻で結果を並べ替える。</span><span class="sxs-lookup"><span data-stu-id="fab81-115">It uses the `orderby` function to sort the results by the start time.</span></span>
        - <span data-ttu-id="fab81-116">関数を使用 `top` して、1 つの要求の結果を 25 に制限します。</span><span class="sxs-lookup"><span data-stu-id="fab81-116">It uses the `top` function to limit the results in a single request to 25.</span></span>
    - <span data-ttu-id="fab81-117">**PageIteratorCallback** オブジェクトを使用して [](https://docs.microsoft.com/graph/sdks/paging)結果を反復処理し、結果のページが利用可能な場合は追加の要求を行います。</span><span class="sxs-lookup"><span data-stu-id="fab81-117">It uses a **PageIteratorCallback** object to [iterate through the results](https://docs.microsoft.com/graph/sdks/paging) and to make additional requests if more pages of results are available.</span></span>

## <a name="update-the-ui"></a><span data-ttu-id="fab81-118">UI の更新</span><span class="sxs-lookup"><span data-stu-id="fab81-118">Update the UI</span></span>

<span data-ttu-id="fab81-119">次に、作業ウィンドウを更新して、ユーザーが予定表ビューの開始日と終了日を指定できます。</span><span class="sxs-lookup"><span data-stu-id="fab81-119">Now let's update the task pane to allow the user to specify a start and end date for the calendar view.</span></span>

1. <span data-ttu-id="fab81-120">**./src/addin/taskpane.js開** き、既存の関数を次のように `showMainUi` 置き換える。</span><span class="sxs-lookup"><span data-stu-id="fab81-120">Open **./src/addin/taskpane.js** and replace the existing `showMainUi` function with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="MainUiSnippet":::

    <span data-ttu-id="fab81-121">このコードは、ユーザーが開始日と終了日を指定できるよう、単純なフォームを追加します。</span><span class="sxs-lookup"><span data-stu-id="fab81-121">This code adds a simple form so the user can specify a start and end date.</span></span> <span data-ttu-id="fab81-122">また、新しいイベントを作成するための 2 番目のフォームも実装します。</span><span class="sxs-lookup"><span data-stu-id="fab81-122">It also implements a second form for creating a new event.</span></span> <span data-ttu-id="fab81-123">このフォームは今のところ何も行いません。次のセクションでこの機能を実装します。</span><span class="sxs-lookup"><span data-stu-id="fab81-123">That form doesn't do anything for now, you'll implement that feature in the next section.</span></span>

1. <span data-ttu-id="fab81-124">次のコードをファイルに追加して、予定表ビューから取得したイベントを含むテーブルをアクティブワークシートに作成します。</span><span class="sxs-lookup"><span data-stu-id="fab81-124">Add the following code to the file to create a table in the active worksheet containing the events retrieved from the calendar view.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="WriteToSheetSnippet":::

1. <span data-ttu-id="fab81-125">カレンダー ビュー API を呼び出す次の関数を追加します。</span><span class="sxs-lookup"><span data-stu-id="fab81-125">Add the following function to call the calendar view API.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="GetCalendarSnippet":::

1. <span data-ttu-id="fab81-126">すべての変更を保存し、サーバーを再起動し、Excel で作業ウィンドウを更新します (開いている作業ウィンドウを閉じて、再び開きます)。</span><span class="sxs-lookup"><span data-stu-id="fab81-126">Save all of your changes, restart the server, and refresh the task pane in Excel (close any open task panes and re-open).</span></span>

    ![インポート フォームのスクリーンショット](images/get-calendar-view-ui.png)

1. <span data-ttu-id="fab81-128">開始日と終了日を選択し、[インポート] を **選択します**。</span><span class="sxs-lookup"><span data-stu-id="fab81-128">Choose start and end dates and choose **Import**.</span></span>

    ![イベント表のスクリーンショット](images/calendar-view-table.png)
