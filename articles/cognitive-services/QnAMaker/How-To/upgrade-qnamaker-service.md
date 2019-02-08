---
title: Soru-cevap Oluşturucu, soru-cevap Oluşturucu hizmeti - yükseltme
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu yığınının tek tek bileşenler ilk oluşturulduktan sonra yükseltmeyi seçebilir.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/24/2019
ms.author: tulasim
ms.openlocfilehash: e37a903112f30917ff5051305bb2144981de1c9d
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55883282"
---
# <a name="upgrade-your-qna-maker-service"></a>Soru-Cevap Oluşturma hizmetinizi yükseltme
Soru-cevap Oluşturucu yığınının tek tek bileşenler ilk oluşturulduktan sonra yükseltmeyi seçebilir. SKU seçimi ve bağımlı bileşenler ayrıntılarını görmek [burada](https://aka.ms/qnamaker-docs-capacity).

## <a name="upgrade-qna-maker-management-sku"></a>Soru-cevap Oluşturucu Yönetimi SKU yükseltme

Daha fazla sorular ve cevaplar, Bilgi Bankası'ndaki, ihtiyacınız olduğunda geçerli katmanınızı fiyatlandırma katmanı, soru-cevap Oluşturucu hizmetini yükseltin. 

Soru-cevap Oluşturucu yönetim SKU yükseltmek için:

1. Azure Portalı'nda soru-cevap Oluşturucu kaynağınıza gidin ve seçin **fiyatlandırma katmanı**.

    ![Soru-cevap Oluşturucu kaynak](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

2. Uygun SKU ve ENTER tuşuna seçin **seçin**.

    ![Soru-cevap Oluşturucu fiyatlandırması](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)

## <a name="upgrade-app-service"></a>App Service'i Yükselt

 Bilgi bankanızı istemci uygulamanıza ilişkin daha fazla isteklere hizmet gerektiğinde, app service fiyatlandırma katmanına yükseltin.

Yapabilecekleriniz [ölçeği](https://docs.microsoft.com/azure/app-service/web-sites-scale) veya uygulama hizmeti ölçeklendirin.

1. Azure portalında uygulama hizmeti kaynağına gidin ve seçin **ölçeği** veya **ölçeğini** gerektiği şekilde Seçenekler.

    ![Soru-cevap Oluşturucu app service ölçek](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

## <a name="upgrade-azure-search-service"></a>Azure arama hizmetini yükseltin

Birçok bilgi bankaları planlama yaparken, Azure Search hizmetinizin fiyatlandırma katmanına yükseltin. 

Şu anda bir yerinde gerçekleştirmek mümkün değil Azure yükseltmesini SKU arayın. Ancak, yeni bir Azure Search'ü kaynak istenen SKU ile oluşturmak, yeni kaynak için verileri geri ve soru-cevap Oluşturucu yığına Bağla.

1. Azure portalında yeni bir Azure Search'ü kaynak oluşturmak ve istenen SKU seçin.

    ![Soru-cevap Oluşturucu Azure arama kaynak](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

2. Dizinler, özgün Azure Search'ü kaynaktan yeni bir tane geri yükleyin. Yedekleme geri yükleme örnek kodu görmek [burada](https://github.com/pchoudhari/QnAMakerBackupRestore).

3. Verileri geri yüklendikten sonra yeni Azure Search'ü kaynağa, select Git **anahtarları**ve Not **adı** ve **yönetici anahtarını**.

    ![Soru-cevap Oluşturucu Azure arama anahtarları](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-keys.png)

4. Yeni Azure Search'ü kaynak için soru-cevap Oluşturucu yığın bağlamak için soru-cevap Oluşturucu App Service'e gidin.

    ![Soru-cevap Oluşturucu appservice](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource-list-appservice.png)

5. Seçin **uygulama ayarları** değiştirin **AzureSearchName** ve **AzureSearchAdminKey** 3. adımdaki alanları.

    ![Soru-cevap Oluşturucu appservice ayarı](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-settings.png)

6. App service'ı yeniden başlatın.

    ![Soru-cevap Oluşturucu appservice yeniden başlatma](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-cevap Oluşturucu API'si kullanma](../Quickstarts/csharp.md)
