---
ms.openlocfilehash: fbd3e506358aa4be60dfe3891b50085691f7443a
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: Block
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274335"
---
# <a name="contributing-to-microsoft-graph-training-repositories"></a>Microsoft Graph トレーニング リポジトリへの貢献

このプロジェクトに貢献してありがとうございます。 プルリクエストを提出する前に、次の点を考慮してください。

## <a name="overview"></a>概要

このリポジトリ内のコードは、次の 3 つの目的に使用されます。

- チュートリアル フォルダー内の Markdown [ファイルは](/tutorial) 、Microsoft Graph のチュートリアル ページでチュートリアル [として公開](https://docs.microsoft.com/graph/tutorials) されます。
- デモ フォルダー内のサンプル [プロジェクト](/demo) は、Microsoft Graph クイック スタート [のソースです](https://developer.microsoft.com/graph/quick-start)。**\***
- デモ フォルダー内のサンプル プロジェクトも GitHub から直接ダウンロード可能であり、簡単な構成を行った後は、この時点で実行する必要があります。

> **\*** すべてのトレーニング リポジトリをクイック スタートとして利用できる (まだ) ではありません。

これは、ある場所での変更によって、同期を維持するために別の場所での変更が必要になる場合があるという点に念頭に置いておく必要があります。可能であれば、Markdown ファイルは (カスタム構文を使用して) ソース コード ファイルを直接参照し、ソース内のコードを更新すると Markdown のコードが自動的に `:::code` 更新されます。

## <a name="updating-code"></a>コードの更新

`:::code`Markdown で使用される構文は、ソース コード ファイル内の特定のコメントによって異なります。 これらのコメントは次のようになります。

```csharp
// <MySnippet>
Console.WriteLine("Hello World!");
// </MySnippet>
```

これらの "マーカー" コメントの間でコードを更新すると、Markdown ファイルは、Microsoft Graph ドキュメント サイトに公開された場合にそれらの変更を自動的に取得します。 これらのコメント以外のコードを更新する場合は、対応する Markdown を更新する必要がある可能性が高い可能性があります。

## <a name="adding-features"></a>機能の追加

評価は高く評価される一方で、サンプルに新機能を追加するプル要求は送信しないでおください。 このリポジトリは主に「最初のアプリをビルドする」チュートリアルなので、機能セットは設計上制限されています。

## <a name="submitting-pull-requests"></a>プル要求を送信する

すべてのプル要求をブランチに送信 `master` してください。

## <a name="when-do-changes-get-published"></a>変更が公開されるのは、いつですか?

Microsoft Graph チュートリアル サイトへの [更新プログラムの発行](https://docs.microsoft.com/graph/tutorials) は自動ではありません。 変更は最初に分岐に昇格し、次にサイト管理者がビルドをトリガー `live` する必要があります。 これは通常、"必要に応じて" 行われます。

## <a name="code-of-conduct"></a>Code of conduct

このプロジェクトでは、[Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) が採用されています。詳細については、「[規範に関する FAQ](https://opensource.microsoft.com/codeofconduct/faq/)」を参照してください。または、その他の質問やコメントがあれば、[opencode@microsoft.com](mailto:opencode@microsoft.com) までにお問い合わせください。
