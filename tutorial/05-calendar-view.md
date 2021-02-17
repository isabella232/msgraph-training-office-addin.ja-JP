---
ms.openlocfilehash: ade142f1518d9bd56fa1472889721a4c8995bc3c
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274323"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="d9e72-101">この演習では、Microsoft Graph をアプリケーションに組み込む必要があります。</span><span class="sxs-lookup"><span data-stu-id="d9e72-101">In this exercise you will incorporate Microsoft Graph into the application.</span></span> <span data-ttu-id="d9e72-102">このアプリケーションでは [、microsoft-graph-client ライブラリ](https://github.com/microsoftgraph/msgraph-sdk-javascript) を使用して Microsoft Graph を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="d9e72-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="d9e72-103">Outlook からカレンダー イベントを取得する</span><span class="sxs-lookup"><span data-stu-id="d9e72-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="d9e72-104">まず、ユーザーの予定表から予定表ビューを [取得する](https://docs.microsoft.com/graph/api/user-list-calendarview) API を追加します。</span><span class="sxs-lookup"><span data-stu-id="d9e72-104">Start by adding an API to get a [calendar view](https://docs.microsoft.com/graph/api/user-list-calendarview) from the user's calendar.</span></span>

1. <span data-ttu-id="d9e72-105">**./src/api/graph.ts** を開き、ファイルの一番上に次の `import` ステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="d9e72-105">Open **./src/api/graph.ts** and add the following `import` statements to the top of the file.</span></span>

    ```typescript
    import { zonedTimeToUtc } from 'date-fns-tz';
    import { findOneIana } from 'windows-iana';
    import * as graph from '@microsoft/microsoft-graph-client';
    import { Event, MailboxSettings } from 'microsoft-graph';
    import 'isomorphic-fetch';
    import { getTokenOnBehalfOf } from './auth';
    ```

1. <span data-ttu-id="d9e72-106">Microsoft Graph SDK を初期化し、クライアントを返す次の関数を追加 **します**。</span><span class="sxs-lookup"><span data-stu-id="d9e72-106">Add the following function to initialize the Microsoft Graph SDK and return a **Client**.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/api/graph.ts" id="GetClientSnippet":::

1. <span data-ttu-id="d9e72-107">次の関数を追加して、ユーザーのメールボックス設定からユーザーのタイム ゾーンを取得し、その値を IANA タイム ゾーン識別子に変換します。</span><span class="sxs-lookup"><span data-stu-id="d9e72-107">Add the following function to get the user's time zone from their mailbox settings, and to convert that value to an IANA time zone identifier.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/api/graph.ts" id="GetTimeZonesSnippet":::

1. <span data-ttu-id="d9e72-108">API エンドポイント ( ) を実装するために、次の関数 (行の下 `const graphRouter = Router();` ) を追加します `GET /graph/calendarview` 。</span><span class="sxs-lookup"><span data-stu-id="d9e72-108">Add the following function (below the `const graphRouter = Router();` line) to implement an API endpoint (`GET /graph/calendarview`).</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/api/graph.ts" id="GetCalendarViewSnippet":::

    <span data-ttu-id="d9e72-109">このコードの動作を検討します。</span><span class="sxs-lookup"><span data-stu-id="d9e72-109">Consider what this code does.</span></span>

    - <span data-ttu-id="d9e72-110">ユーザーのタイム ゾーンを取得し、それを使用して要求されたカレンダー ビューの開始と終了を UTC 値に変換します。</span><span class="sxs-lookup"><span data-stu-id="d9e72-110">It gets the user's time zone and uses that to convert the start and end of the requested calendar view into UTC values.</span></span>
    - <span data-ttu-id="d9e72-111">これは `GET` `/me/calendarview` 、Graph API エンドポイントに対して行います。</span><span class="sxs-lookup"><span data-stu-id="d9e72-111">It does a `GET` to the `/me/calendarview` Graph API endpoint.</span></span>
        - <span data-ttu-id="d9e72-112">関数を使用してヘッダーを設定し、返されたイベントの開始時刻と終了時刻をユーザーのタイム ゾーン `header` `Prefer: outlook.timezone` に調整します。</span><span class="sxs-lookup"><span data-stu-id="d9e72-112">It uses the `header` function to set the `Prefer: outlook.timezone` header, causing the start and end times of the returned events to be adjusted to the user's time zone.</span></span>
        - <span data-ttu-id="d9e72-113">この関数を `query` 使用して、カレンダー ビューの開始と終了を設定するパラメーター `startDateTime` `endDateTime` とパラメーターを追加します。</span><span class="sxs-lookup"><span data-stu-id="d9e72-113">It uses the `query` function to add the `startDateTime` and `endDateTime` parameters, setting the start and end of the calendar view.</span></span>
        - <span data-ttu-id="d9e72-114">この関数を `select` 使用して、アドインで使用されるフィールドのみを要求します。</span><span class="sxs-lookup"><span data-stu-id="d9e72-114">It uses the `select` function to request only the fields used by the add-in.</span></span>
        - <span data-ttu-id="d9e72-115">この関数を `orderby` 使用して、開始時刻で結果を並べ替える。</span><span class="sxs-lookup"><span data-stu-id="d9e72-115">It uses the `orderby` function to sort the results by the start time.</span></span>
        - <span data-ttu-id="d9e72-116">この関数を使用 `top` して、1 つの要求の結果を 25 に制限します。</span><span class="sxs-lookup"><span data-stu-id="d9e72-116">It uses the `top` function to limit the results in a single request to 25.</span></span>
    - <span data-ttu-id="d9e72-117">**PageIteratorCallback** オブジェクトを使用して [](https://docs.microsoft.com/graph/sdks/paging)結果を反復処理し、より多くの結果ページが使用可能な場合は追加の要求を行います。</span><span class="sxs-lookup"><span data-stu-id="d9e72-117">It uses a **PageIteratorCallback** object to [iterate through the results](https://docs.microsoft.com/graph/sdks/paging) and to make additional requests if more pages of results are available.</span></span>

## <a name="update-the-ui"></a><span data-ttu-id="d9e72-118">UI を更新する</span><span class="sxs-lookup"><span data-stu-id="d9e72-118">Update the UI</span></span>

<span data-ttu-id="d9e72-119">次に、作業ウィンドウを更新して、ユーザーがカレンダー ビューの開始日と終了日を指定できます。</span><span class="sxs-lookup"><span data-stu-id="d9e72-119">Now let's update the task pane to allow the user to specify a start and end date for the calendar view.</span></span>

1. <span data-ttu-id="d9e72-120">**./src/addin/taskpane.js** 開き、既存の関数を次のように `showMainUi` 置き換える。</span><span class="sxs-lookup"><span data-stu-id="d9e72-120">Open **./src/addin/taskpane.js** and replace the existing `showMainUi` function with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="MainUiSnippet":::

    <span data-ttu-id="d9e72-121">このコードは、ユーザーが開始日と終了日を指定できる簡単なフォームを追加します。</span><span class="sxs-lookup"><span data-stu-id="d9e72-121">This code adds a simple form so the user can specify a start and end date.</span></span> <span data-ttu-id="d9e72-122">また、新しいイベントを作成するための 2 番目のフォームも実装します。</span><span class="sxs-lookup"><span data-stu-id="d9e72-122">It also implements a second form for creating a new event.</span></span> <span data-ttu-id="d9e72-123">このフォームでは、今のところ何も行いません。次のセクションでこの機能を実装します。</span><span class="sxs-lookup"><span data-stu-id="d9e72-123">That form doesn't do anything for now, you'll implement that feature in the next section.</span></span>

1. <span data-ttu-id="d9e72-124">次のコードをファイルに追加して、カレンダー ビューから取得したイベントを含むテーブルをアクティブ ワークシートに作成します。</span><span class="sxs-lookup"><span data-stu-id="d9e72-124">Add the following code to the file to create a table in the active worksheet containing the events retrieved from the calendar view.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="WriteToSheetSnippet":::

1. <span data-ttu-id="d9e72-125">カレンダー ビュー API を呼び出す次の関数を追加します。</span><span class="sxs-lookup"><span data-stu-id="d9e72-125">Add the following function to call the calendar view API.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="GetCalendarSnippet":::

1. <span data-ttu-id="d9e72-126">すべての変更を保存し、サーバーを再起動し、Excel で作業ウィンドウを更新します (開いている作業ウィンドウを閉じて、再び開きます)。</span><span class="sxs-lookup"><span data-stu-id="d9e72-126">Save all of your changes, restart the server, and refresh the task pane in Excel (close any open task panes and re-open).</span></span>

    ![インポート フォームのスクリーンショット](images/get-calendar-view-ui.png)

1. <span data-ttu-id="d9e72-128">開始日と終了日を選択し、[インポート] を **選択します**。</span><span class="sxs-lookup"><span data-stu-id="d9e72-128">Choose start and end dates and choose **Import**.</span></span>

    ![イベント表のスクリーンショット](images/calendar-view-table.png)
