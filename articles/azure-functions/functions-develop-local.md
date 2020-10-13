---
title: Azure işlevlerini yerel olarak geliştirin ve çalıştırın
description: Azure işlevleri 'nde çalıştırmadan önce yerel bilgisayarınızdaki Azure işlevlerini nasıl kodleyeceğinizi ve test leyeceğinizi öğrenin.
ms.topic: conceptual
ms.date: 09/04/2018
ms.openlocfilehash: 2362fcd27f1b5f7c0b3411eae5b3b94c7dc6cf4c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "86540188"
---
# <a name="code-and-test-azure-functions-locally"></a>Azure İşlevleri’ni yerel olarak kodlama ve test etme

[Azure Portal]Azure işlevleri geliştirip test edebilirken birçok geliştirici, yerel bir geliştirme deneyimi tercih eder. İşlevler, yerel bilgisayarınızda işlevleri oluşturmak ve test etmek için en sevdiğiniz kod düzenleyicinizi ve geliştirme araçlarını kullanmayı kolaylaştırır. Yerel işlevleriniz canlı Azure hizmetlerine bağlanabilir ve tüm Işlevler çalışma zamanını kullanarak yerel bilgisayarınızda hata ayıklaması yapabilirsiniz.

## <a name="local-development-environments"></a>Yerel geliştirme ortamları

Yerel bilgisayarınızda işlevleri geliştirme yönteminiz, [dilinize](supported-languages.md) ve araç tercihlerinize göre değişir. Aşağıdaki tablodaki ortamlar yerel geliştirmeyi destekler:

|Ortam                              |Diller         |Açıklama|
|-----------------------------------------|------------|---|
|[Visual Studio Code](functions-develop-vs-code.md)| [C# (sınıf kitaplığı)](functions-dotnet-class-library.md), [c# betiği (. CSX)](functions-reference-csharp.md), [JavaScript](functions-reference-node.md), [PowerShell](./functions-create-first-function-vs-code.md?pivots=programming-language-powershell), [Python](functions-reference-python.md) | [Vs Code Için Azure işlevleri uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) , vs Code işlev desteği ekler. Temel araçları gerektirir. , Çekirdek araçların 2. x 'i kullanılırken Linux, MacOS ve Windows için geliştirmeyi destekler. Daha fazla bilgi için bkz. [Visual Studio Code kullanarak ilk işlevinizi oluşturma](functions-create-first-function-vs-code.md). |
| [Komut istemi veya Terminal](functions-run-local.md) | [C# (sınıf kitaplığı)](functions-dotnet-class-library.md), [c# betiği (. CSX)](functions-reference-csharp.md), [JavaScript](functions-reference-node.md), [PowerShell](functions-reference-powershell.md), [Python](functions-reference-python.md) | [Azure Functions Core Tools] , yerel geliştirmeyi etkinleştiren işlevler oluşturmak için çekirdek çalışma zamanı ve şablonları sağlar. Sürüm 2. x, Linux, MacOS ve Windows üzerinde geliştirmeyi destekler. Tüm ortamlar yerel Işlevler çalışma zamanına yönelik temel araçlara güvenir. |
| [Visual Studio 2019](functions-develop-vs.md) | [C# (sınıf kitaplığı)](functions-dotnet-class-library.md) | Azure Işlevleri araçları, [Visual Studio 2019](https://www.visualstudio.com/vs/) ve sonraki sürümlerin **Azure geliştirme** iş yüküne dahildir. Bir sınıf kitaplığındaki işlevleri derlemenize ve. dll dosyasını Azure 'a yayımlamanıza olanak sağlar. Yerel test için temel araçları içerir. Daha fazla bilgi için bkz. [Visual Studio kullanarak Azure Işlevleri geliştirme](functions-develop-vs.md). |
| [Maven](./functions-create-first-azure-function-azure-cli.md?pivots=programming-language-java&tabs=bash,browser) (çeşitli) | [Java](functions-reference-java.md) | , Java işlevlerinin geliştirilmesini sağlamak için çekirdek araçlarla tümleştirilir. Sürüm 2. x, Linux, MacOS ve Windows üzerinde geliştirmeyi destekler. Daha fazla bilgi için bkz. [Java ve Maven ile ilk işlevinizi oluşturma](./functions-create-first-azure-function-azure-cli.md?pivots=programming-language-java&tabs=bash,browser). Ayrıca, [Çakışan Küreler](functions-create-maven-eclipse.md) ve [IntelliJ fikrini](functions-create-maven-intellij.md) kullanarak geliştirmeyi destekler |

[!INCLUDE [Don't mix development environments](../../includes/functions-mixed-dev-environments.md)]

Bu yerel geliştirme ortamlarının her biri, işlev uygulaması projeleri oluşturmanıza ve yeni işlevler oluşturmak için önceden tanımlanmış Işlevler şablonlarını kullanmanıza imkan sağlar. , İşlevlerinizi diğer uygulamalarda olduğu gibi kendi makinenizde gerçek Işlevler çalışma zamanına karşı sınayabilmeniz ve hatalarını ayıklayabilmeniz için temel araçları kullanır. Ayrıca, bu ortamların herhangi birinden işlev uygulama projenizi Azure 'a yayımlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

+ Visual Studio 2019 kullanarak derlenmiş C# işlevlerinin yerel geliştirmesi hakkında daha fazla bilgi edinmek için bkz. [Visual Studio kullanarak Azure Işlevleri geliştirme](functions-develop-vs.md).
+ Bir Mac, Linux veya Windows bilgisayarında VS Code kullanarak işlevlerin yerel geliştirmesi hakkında daha fazla bilgi edinmek için, bkz. [vs Code Azure Işlevlerini dağıtma](/azure/developer/javascript/tutorial-vscode-serverless-node-01).
+ Komut isteminden veya terminalden işlev geliştirme hakkında daha fazla bilgi için bkz. [Azure Functions Core Tools Ile çalışma](functions-run-local.md).

<!-- LINKS -->

[Azure Functions Core Tools]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure portalındaki]: https://portal.azure.com
[Node.js]: https://docs.npmjs.com/getting-started/installing-node#osx-or-windows
