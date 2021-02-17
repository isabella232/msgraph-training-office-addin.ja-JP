---
ms.openlocfilehash: facdbb5c42e60e5bb0ee98b06ef68939a6010a67
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274318"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="1da56-101">このセクションでは、ユーザーの予定表にイベントを作成する機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="1da56-101">In this section you will add the ability to create events on the user's calendar.</span></span>

## <a name="implement-the-api"></a><span data-ttu-id="1da56-102">API を実装する</span><span class="sxs-lookup"><span data-stu-id="1da56-102">Implement the API</span></span>

1. <span data-ttu-id="1da56-103">**./src/api/graph.ts** を開き、次のコードを追加して新しいイベント API ( ) を実装します `POST /graph/newevent` 。</span><span class="sxs-lookup"><span data-stu-id="1da56-103">Open **./src/api/graph.ts** and add the following code to implement a new event API (`POST /graph/newevent`).</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/api/graph.ts" id="CreateEventSnippet":::

1. <span data-ttu-id="1da56-104">**./src/addin/taskpane.js** 開き、次の関数を追加して新しいイベント API を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="1da56-104">Open **./src/addin/taskpane.js** and add the following function to call the new event API.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="CreateEventSnippet":::

1. <span data-ttu-id="1da56-105">すべての変更を保存し、サーバーを再起動し、Excel で作業ウィンドウを更新します (開いている作業ウィンドウを閉じて、再び開きます)。</span><span class="sxs-lookup"><span data-stu-id="1da56-105">Save all of your changes, restart the server, and refresh the task pane in Excel (close any open task panes and re-open).</span></span>

    ![イベント作成フォームのスクリーンショット](images/create-event-ui.png)

1. <span data-ttu-id="1da56-107">フォームに入力し、[作成] を **選択します**。</span><span class="sxs-lookup"><span data-stu-id="1da56-107">Fill in the form and choose **Create**.</span></span> <span data-ttu-id="1da56-108">イベントがユーザーの予定表に追加されるのを確認します。</span><span class="sxs-lookup"><span data-stu-id="1da56-108">Verify that the event is added to the user's calendar.</span></span>
