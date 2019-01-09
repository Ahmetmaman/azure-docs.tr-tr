---
title: Visual Studio Code’u kullanarak Azure’da ilk işlevinizi oluşturma
description: Visual Studio Code’daki Azure İşlevleri uzantısını kullanarak basit bir HTTP ile tetiklenen işlev oluşturun ve Azure’da yayımlayın.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: azure işlevleri, işlevler, olay işleme, işlem, sunucusuz mimari
ms.service: azure-functions
ms.devlang: multiple
ms.topic: quickstart
ms.date: 09/07/2018
ms.author: glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: d265c391b12a46f509c8663c64892372c8d806ba
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54104531"
---
# <a name="create-your-first-function-using-visual-studio-code"></a>Visual Studio Code kullanarak ilk işlevinizi oluşturma

Azure İşlevleri, öncelikle bir VM oluşturmak veya bir web uygulaması yayımlamak zorunda kalmadan kodunuzu [sunucusuz](https://azure.microsoft.com/solutions/serverless/) bir ortamda yürütmenize olanak tanır.

Bu makalede Microsoft Visual Studio Code’u kullanarak yerel bilgisayarınızda bir “merhaba dünya” işlevi oluşturup test etmek üzere [Visual Studio Code için Azure İşlevleri uzantısı] kullanmayı öğreneceksiniz. Ardından işlev kodunu Visual Studio Code ile Azure’da yayımlayacaksınız.

![Visual Studio projesinde Azure İşlevleri kodu](./media/functions-create-first-function-vs-code/functions-vscode-intro.png)

Uzantı şu anda tam olarak destekler C#, JavaScript ve Java işlevleri, Python desteği şu anda önizlemede. Bu makaledeki adımlar, Azure İşlevleri projenizde tercih ettiğiniz dile göre değişiklik gösterebilir. Uzantı şu an önizleme aşamasındadır. Daha fazla bilgi edinmek için [Visual Studio Code için Azure İşlevleri uzantısı] sayfasına bakın.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için:

* [Desteklenen platformlardan](https://code.visualstudio.com/docs/supporting/requirements#_platforms) birinde [Visual Studio Code](https://code.visualstudio.com/)’u yükleyin. Bu makale macOS (High Sierra) ile çalışan bir cihazda geliştirilmiş ve test edilmiştir.

* [Azure İşlevleri Temel Araçları](functions-run-local.md#v2)’nın hala önizleme aşamasında olan 2.x sürümünü yükleyin.

* Seçtiğiniz dile özgü gereksinimleri yükleyin:

    | Dil | Dahili numara |
    | -------- | --------- |
    | **C#** | [Visual Studio Code için C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)<br/>[.NET Core CLI araçları](https://docs.microsoft.com/dotnet/core/tools/?tabs=netcore2x)*   |
    | **Java** | [Java hata ayıklayıcısı](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debug)<br/>[Java 8](https://aka.ms/azure-jdks)<br/>[Maven 3+](https://maven.apache.org/) |
    | **JavaScript** | [Node 8.0+](https://nodejs.org/)  |

    \* Ayrıca Core Araçları için gereklidir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [functions-install-vs-code-extension](../../includes/functions-install-vs-code-extension.md)]

[!INCLUDE [functions-create-function-app-vs-code](../../includes/functions-create-function-app-vs-code.md)]

## <a name="create-an-http-triggered-function"></a>HTTP ile tetiklenen işlev oluşturma

1. Gelen **Azure: İşlevleri**, Create FUNCTION simgesini seçin.

    ![İşlev oluşturma](./media/functions-create-first-function-vs-code/create-function.png)

1. İşlev uygulaması projenizin yer aldığı klasörü seçin ve **HTTP tetikleyicisi** işlev şablonunu seçin.

    ![HTTP tetikleyicisi şablonunu seçin](./media/functions-create-first-function-vs-code/create-function-choose-template.png)

1. İşlev adı için `HTTPTrigger` yazın ve Enter tuşuna basın ve **Anonim** kimlik doğrulamasını seçin.

    ![Anonim kimlik doğrulamasını seçin](./media/functions-create-first-function-vs-code/create-function-anonymous-auth.png)

    HTTP ile tetiklenen işlevin şablonu kullanılarak, seçtiğiniz dilde bir işlev oluşturulur.

    ![Visual Studio Code’daki HTTP ile tetiklenen işlev şablonu](./media/functions-create-first-function-vs-code/new-function-full.png)

function.json dosyasını değiştirerek işlevinize giriş ve çıkış bağlamaları ekleyebilirsiniz. Daha fazla bilgi için bkz. [Azure İşlevleri tetikleyicileri ve bağlama kavramları](functions-triggers-bindings.md).

İşlev projenizi ve HTTP ile tetiklenen işlev oluşturduğunuza göre, artık bunu yerel bilgisayarınızda test edebilirsiniz.

## <a name="test-the-function-locally"></a>İşlevi yerel olarak test etme

Azure İşlevleri Temel Araçları, Azure İşlevleri projenizi yerel geliştirme bilgisayarınızda çalıştırmanıza olanak sağlar. Visual Studio Code'da ilk kez bir işlev başlattığınızda bu araçları yüklemeniz istenir.  

1. İşlevinizi test etmek için işlev kodunda bir kesme noktası ayarlayın ve işlev uygulaması projesini başlatmak için F5 tuşuna basın. Temel Araçlar’daki çıktı, **Terminal** panelinde görüntülenir.

1. **Terminal** panelinde, HTTP ile tetiklenen işlevinizin URL uç noktasını kopyalayın.

    ![Azure yerel çıktısı](./media/functions-create-first-function-vs-code/functions-vscode-f5.png)

1. HTTP isteğinin URL’sini tarayıcınızın adres çubuğuna yapıştırın. `?name=<yourname>` sorgu dizesini bu URL’ye ekleyip isteği yürütün. Kesme noktasına gelindiğinde yürütme duraklatılır.

    ![Visual Studio Code’da kesme noktasına gelen işlev](./media/functions-create-first-function-vs-code/function-debug-vscode-js.png)

1. Yürütmeyi sürdürdüğünüzde GET isteğine tarayıcıda verilen yanıt aşağıda gösterilmiştir:

    ![Tarayıcıdaki işlev localhost yanıtı](./media/functions-create-first-function-vs-code/functions-test-local-browser.png)

1. Hata ayıklamayı durdurmak için Shift + F5 tuşuna basın.

İşlevin yerel bilgisayarınızda düzgün çalıştığını doğruladıktan sonra, projeyi Azure'da yayımlamanın zamanı gelmiştir.

[!INCLUDE [functions-sign-in-vs-code](../../includes/functions-sign-in-vs-code.md)]

[!INCLUDE [functions-publish-project-vscode](../../includes/functions-publish-project-vscode.md)]

## <a name="test-your-function-in-azure"></a>Azure'da işlevinizi test etme

1. **Çıktı** panelinden HTTP tetikleyicisinin URL’sini kopyalayın. Daha önce olduğu gibi, `?name=<yourname>` sorgu dizesini bu URL’nin sonuna ekleyip isteği yürütmeyi unutmayın.

    HTTP tarafından tetiklenen işlevinizi çağıran URL aşağıdaki biçimde olmalıdır:

        http://<functionappname>.azurewebsites.net/api/<functionname>?name=<yourname> 

1. HTTP isteğinin yeni URL’sini tarayıcınızın adres çubuğuna yapıştırın. İşlevin döndürdüğü uzak GET isteğine tarayıcıda verilen yanıt aşağıda gösterilmiştir: 

    ![Tarayıcıdaki işlev yanıtı](./media/functions-create-first-function-vs-code/functions-test-remote-browser.png)

## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen basit bir işlevi kullanarak bir işlev uygulaması oluşturmak için Visual Studio Code’u kullandınız. Belirli bir dildeki geliştirme işlevleri hakkında daha fazla bilgi almak için [JavaScript](functions-reference-node.md), [.NET](functions-dotnet-class-library.md) veya [Java](functions-reference-java.md)’ya yönelik dil başvuru kılavuzlarına bakın.

Ardından, Azure İşlevleri Temel Araçları’nı kullanarak Terminal veya komut istemcisinden yerel testler gerçekleştirme veya hat ayıklama hakkında daha fazla bilgi edinmek isteyebilirsiniz.

> [!div class="nextstepaction"]
> [Yerel olarak kod yazma ve test etme](functions-run-local.md)

[Azure Functions Core Tools]: functions-run-local.md
[Visual Studio Code için Azure İşlevleri uzantısı]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
