---
ms.openlocfilehash: facdbb5c42e60e5bb0ee98b06ef68939a6010a67
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274318"
---
<!-- markdownlint-disable MD002 MD041 -->

このセクションでは、ユーザーの予定表にイベントを作成する機能を追加します。

## <a name="implement-the-api"></a>API を実装する

1. **./src/api/graph.ts** を開き、次のコードを追加して新しいイベント API ( ) を実装します `POST /graph/newevent` 。

    :::code language="typescript" source="../demo/graph-tutorial/src/api/graph.ts" id="CreateEventSnippet":::

1. **./src/addin/taskpane.js** 開き、次の関数を追加して新しいイベント API を呼び出します。

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="CreateEventSnippet":::

1. すべての変更を保存し、サーバーを再起動し、Excel で作業ウィンドウを更新します (開いている作業ウィンドウを閉じて、再び開きます)。

    ![イベント作成フォームのスクリーンショット](images/create-event-ui.png)

1. フォームに入力し、[作成] を **選択します**。 イベントがユーザーの予定表に追加されるのを確認します。
