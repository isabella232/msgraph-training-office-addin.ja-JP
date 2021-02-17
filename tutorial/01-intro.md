---
ms.openlocfilehash: 2c323d61632c62c82af0561536656f1d68fe8938
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274377"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="ef08d-101">このチュートリアルでは、Microsoft Graph API を使用してユーザーの予定表情報Office Excel 用アドインを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="ef08d-101">This tutorial teaches you how to build an Office Add-in for Excel that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="ef08d-102">完成したチュートリアルをダウンロードするだけの場合は [、GitHub](https://github.com/microsoftgraph/msgraph-training-office-addin)リポジトリをダウンロードまたは複製できます。</span><span class="sxs-lookup"><span data-stu-id="ef08d-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-office-addin).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ef08d-103">前提条件</span><span class="sxs-lookup"><span data-stu-id="ef08d-103">Prerequisites</span></span>

<span data-ttu-id="ef08d-104">このデモを開始する前に、開発用 [ コンピューターにNode.js](https://nodejs.org) [AndNode.jsインストール](https://yarnpkg.com/) されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef08d-104">Before you start this demo, you should have [Node.js](https://nodejs.org) and [Yarn](https://yarnpkg.com/) installed on your development machine.</span></span> <span data-ttu-id="ef08d-105">Node.jsまたは Node.jsオプションについては、前のリンクを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ef08d-105">If you do not have Node.js or Yarn, visit the previous link for download options.</span></span>

> [!NOTE]
> <span data-ttu-id="ef08d-106">Windows ユーザーは、C/C++ からコンパイルする必要Visual Studio NPM モジュールをサポートするために、Python とビルド ツールをインストールする必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="ef08d-106">Windows users may need to install Python and Visual Studio Build Tools to support NPM modules that need to be compiled from C/C++.</span></span> <span data-ttu-id="ef08d-107">Windows Node.jsインストーラーには、これらのツールを自動的にインストールするオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="ef08d-107">The Node.js installer on Windows gives an option to automatically install these tools.</span></span> <span data-ttu-id="ef08d-108">または、次の手順に従います [https://github.com/nodejs/node-gyp#on-windows](https://github.com/nodejs/node-gyp#on-windows) 。</span><span class="sxs-lookup"><span data-stu-id="ef08d-108">Alternatively, you can follow instructions at [https://github.com/nodejs/node-gyp#on-windows](https://github.com/nodejs/node-gyp#on-windows).</span></span>

<span data-ttu-id="ef08d-109">また、メールボックスを持つ個人用の Microsoft アカウントが Outlook.com Microsoft の仕事用アカウントまたは学校アカウントである必要があります。</span><span class="sxs-lookup"><span data-stu-id="ef08d-109">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="ef08d-110">Microsoft アカウントをお持ちない場合は、無料アカウントを取得するためのオプションが 2 つ提供されています。</span><span class="sxs-lookup"><span data-stu-id="ef08d-110">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="ef08d-111">新しい [個人用 Microsoft アカウントにサインアップできます](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。</span><span class="sxs-lookup"><span data-stu-id="ef08d-111">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="ef08d-112">Office [365 開発者プログラムにサインアップして、365](https://developer.microsoft.com/office/dev-program) サブスクリプションを無料Office取得できます。</span><span class="sxs-lookup"><span data-stu-id="ef08d-112">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="ef08d-113">このチュートリアルは、ノード バージョン 14.15.0 および Node バージョン 1.22.0 で記述されています。</span><span class="sxs-lookup"><span data-stu-id="ef08d-113">This tutorial was written with Node version 14.15.0 and Yarn version 1.22.0.</span></span> <span data-ttu-id="ef08d-114">このガイドの手順は他のバージョンでも動作する可能性がありますが、テストは行ってはいではありません。</span><span class="sxs-lookup"><span data-stu-id="ef08d-114">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="ef08d-115">フィードバック</span><span class="sxs-lookup"><span data-stu-id="ef08d-115">Feedback</span></span>

<span data-ttu-id="ef08d-116">このチュートリアルに関するフィードバックは [、GitHub リポジトリで提供してください](https://github.com/microsoftgraph/msgraph-training-office-addin)。</span><span class="sxs-lookup"><span data-stu-id="ef08d-116">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-office-addin).</span></span>
