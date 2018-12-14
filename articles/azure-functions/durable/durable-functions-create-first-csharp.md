---
title: Kullanarak azure'da dayanıklı ilk işlevinizi oluşturmaC#
description: Oluşturup Azure sürekli Visual Studio kullanarak işlevi yayımladıktan.
services: functions
documentationcenter: na
author: jeffhollan
manager: jeconnoc
keywords: azure işlevleri, işlevler, olay işleme, işlem, sunucusuz mimari
ms.service: azure-functions
ms.devlang: multiple
ms.topic: quickstart
ms.date: 11/07/2018
ms.author: azfuncdf
ms.openlocfilehash: a9794c25bd5f0acd48362611d13bac17fc502450
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53341057"
---
# <a name="create-your-first-durable-function-in-c"></a>C'de dayanıklı ilk işlevinizi oluşturma\#

*Dayanıklı işlevler* uzantısıdır [Azure işlevleri](../functions-overview.md) durum bilgisi olan işlevleri, sunucusuz bir ortamda yazmanızı sağlayan. Uzantı durumu ve kontrol noktaları yeniden sizin yerinize yönetir.

Bu makalede, yerel olarak oluşturma ve dayanıklı bir "Merhaba Dünya" işlevi test etmek için Azure işlevleri için Visual Studio 2017 araçlarını kullanmayı öğrenin.  Bu işlev, düzenlemek ve diğer işlevlere yapılan çağrıları zincir. Ardından işlev kodunu Azure’da yayımlayacaksınız. Bu araçlar, Visual Studio 2017’de Azure geliştirme iş yükünün parçası olarak kullanılabilir.

![Dayanıklı işlevi Azure'da çalışan](./media/durable-functions-create-first-csharp/functions-vs-complete.png)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

* [Visual Studio 2017](https://azure.microsoft.com/downloads/)’yi yükleyin ve **Azure geliştirme** iş yükünün de yüklendiğinden emin olun.

* [En son Azure İşlevleri araçlarınızın](../functions-develop-vs.md#check-your-tools-version) olduğundan emin olun.

* Doğrulayın [Azure Storage öykünücüsü](../../storage/common/storage-use-emulator.md) yüklü ve çalışır.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-function-app-project"></a>İşlev uygulaması projesi oluşturma

Visual Studio'daki Azure İşlevleri proje şablonu, Azure'daki bir işlev uygulamasında yayımlanabilen bir proje oluşturur. İşlev uygulaması, kaynakların yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır.

1. Visual Studio'da **Dosya** menüsünden **Yeni** > **Proje**’yi seçin.

2. **Yeni Proje** iletişim kutusunda **Yüklü**'yü seçin, **Visual C#** > **Bulut**'u genişletin, **Azure İşlevleri**'ni seçin, projeniz için bir **Ad** yazın ve **Tamam**'a tıklayın. İşlev uygulamasının adı, bir C# ad alanı olarak geçerli olmalıdır; bu nedenle alt çizgi, kısa çizgi veya alfasayısal olmayan herhangi bir karakter kullanmayın.

    ![Visual Studio'da işlev oluşturmaya yönelik yeni proje iletişim kutusu](./media/durable-functions-create-first-csharp/functions-vs-new-project.png)

3. Resmin altındaki tabloda belirtilen ayarları kullanın.

    ![Visual Studio'da Yeni işlev iletişim kutusu](./media/durable-functions-create-first-csharp/functions-vs-new-function.png)

    | Ayar      | Önerilen değer  | Açıklama                      |
    | ------------ |  ------- |----------------------------------------- |
    | **Sürüm** | Azure İşlevleri 2.x <br />(.NET Core) | .NET Core destekleyen Azure işlevleri'nin sürüm 2.x çalışma zamanı kullanan bir işlev projesi oluşturur. Azure İşlevleri 1.x, .NET Framework’ü destekler. Daha fazla bilgi için bkz. [Azure İşlevleri çalışma zamanı sürümünün hedefini belirleme](../functions-versions.md).   |
    | **Şablon** | Boş | Bu, bir boş işlev uygulaması oluşturur. |
    | **Depolama hesabı**  | Depolama Öykünücüsü | Dayanıklı işlevi durum yönetimi için bir depolama hesabı gereklidir. |

4. Tıklayın **Tamam** bir boş işlev projesi oluşturmak için.

## <a name="add-functions-to-the-app"></a>Uygulamaya işlevleri ekleme

Visual Studio, bir boş işlev uygulaması projesi oluşturur.  Bir uygulama için gerekli temel yapılandırma dosyaları içerir, ancak tüm işlevler henüz içermiyor.  Biz dayanıklı işlev şablonu projeye eklemeniz gerekir.

1. Visual Studio'da projeye sağ tıklayıp **Ekle** > **yeni Azure işlevi**.

    ![Yeni işlev Ekle](./media/durable-functions-create-first-csharp/functions-vs-add-new-function.png)

2. Doğrulama **Azure işlevi** Ekle Menüsü'nden seçilidir ve bu verin, C# dosya bir adı.  **Ekle**’ye basın.

3. Seçin **dayanıklı işlevler düzenleme** şablonu ve tıklatın **Tamam**

    ![Dayanıklı bir şablon seçin](./media/durable-functions-create-first-csharp/functions-vs-select-template.png)  

Dayanıklı bir işlev uygulamasına eklenecektir.  İçeriği görüntülemek için yeni dosyasını açın.  Bu kalıcı işlevi zincirleme örneği basit bir işlevdir.  

* `RunOrchestrator` Yöntemi, orchestrator işlevi ile ilişkilendirilmiş.  Bu işlev başlatmak, bir liste oluşturur ve sonucu üç işlev çağrıları listesine ekleyin.  Üç işlev çağrıları tamamlandığında, bir liste döndürür.  Bunu çağırır işlevi `SayHello` yöntemi (varsayılan çağrılır `<NameOfFile>_Hello`).
* `SayHello` İşlevi bir Merhaba döndürür.
* `HttpStart` Yöntemi düzenleme örneklerini başlatacak işlevi açıklar.  İle ilişkili bir [HTTP tetikleyicisi](../functions-bindings-http-webhook.md) , orchestrator'ın yeni bir örneği başlatın ve bir onay durumu yanıt geri dönün.

İşlev projenizi ve dayanıklı bir işlev oluşturdunuz, yerel bilgisayarınızda test edebilirsiniz.

## <a name="test-the-function-locally"></a>İşlevi yerel olarak test etme

Azure İşlevleri Temel Araçları, Azure İşlevleri projenizi yerel geliştirme bilgisayarınızda çalıştırmanıza olanak sağlar. Visual Studio'da ilk kez bir işlev başlattığınızda bu araçları yüklemeniz istenir.

1. İşlevinizi test etmek için F5’e basın. İstenirse Visual Studio'dan gelen Azure İşlevleri Temel (CLI) araçlarını indirme ve yükleme isteğini kabul edin. Aracın HTTP isteklerini işleyebilmesi için bir güvenlik duvarı özel durumu etkinleştirmeniz de gerekebilir.

2. Azure İşlevleri çalışma zamanı çıktısından işlevinizin URL'sini kopyalayın.

    ![Azure yerel çalışma zamanı](./media/durable-functions-create-first-csharp/functions-vs-debugging.png)

3. HTTP isteğinin URL'sini tarayıcınızın adres çubuğuna yapıştırın ve isteği yürütün. İşlevin döndürdüğü yerel GET isteğine tarayıcıda verilen yanıt aşağıda gösterilmiştir:

    ![Tarayıcıdaki işlev localhost yanıtı](./media/durable-functions-create-first-csharp/functions-vs-status.png)

    Yanıt, dayanıklı düzenleme bize bildirdiğiniz HTTP işlevi ilk sonuç başarıyla başlatıldı.  Bunu henüz düzenleme nihai sonucu değil.  Yanıt birkaç faydalı URL'leri içeriyor.  Şimdilik, şimdi orchestration durumunu sorgulayın.

4. URL değerini kopyalayın `statusQueryGetUri` ve tarayıcının adres yapıştırma çubuk ekleyip isteği yürütün.

    İstek orchestration örneği durumu için sorgular. Aşağıdaki gibi görünür nihai bir yanıt almanız gerekir.  Bu bize örneği tamamlandı ve çıktılar veya dayanıklı işlevinin sonuçlarını içeren gösterir.

    ```json
    {
        "instanceId": "d495cb0ac10d4e13b22729c37e335190",
        "runtimeStatus": "Completed",
        "input": null,
        "customStatus": null,
        "output": [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ],
        "createdTime": "2018-11-08T07:07:40Z",
        "lastUpdatedTime": "2018-11-08T07:07:52Z"
    }
    ```

5. Hata ayıklamayı durdurmak için **Shift + F5** tuşuna basın.

İşlevin yerel bilgisayarınızda düzgün çalıştığını doğruladıktan sonra, projeyi Azure'da yayımlamanın zamanı gelmiştir.

## <a name="publish-the-project-to-azure"></a>Projeyi Azure'da yayımlama

Projenizi yayımlayabilmeniz için önce Azure aboneliğinizde bir işlev uygulamanızın olması gerekir. Visual Studio'dan bir işlev uygulaması oluşturabilirsiniz.

[!INCLUDE [Publish the project to Azure](../../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a>Azure'da işlevinizi test etme

1. Yayımlama profili sayfasından işlev uygulamasının temel URL'sini kopyalayın. İşlevi yerel olarak test ederken kullandığınız URL’nin `localhost:port` kısmını, yeni temel URL ile değiştirin.

    Dayanıklı işlevi HTTP tetikleyicinize çağıran URL aşağıdaki biçimde olmalıdır:

        http://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>_HttpStart

2. HTTP isteğinin yeni URL’sini tarayıcınızın adres çubuğuna yapıştırın. Önce aynı durum yanıt olarak yayımlanan uygulama kullanılırken almanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Visual Studio oluşturma ve yayımlama için kullandığınız bir C# dayanıklı bir işlev uygulaması.

> [!div class="nextstepaction"]
> [Ortak dayanıklı işlevi desenler hakkında bilgi edinin.](durable-functions-overview.md)
