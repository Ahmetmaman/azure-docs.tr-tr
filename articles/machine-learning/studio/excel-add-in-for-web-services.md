---
title: Excel eklentisi için Machine Learning Web Hizmetleri | Microsoft Docs
description: Azure Machine Learning Web Hizmetleri, herhangi bir kod yazmaya gerek kalmadan doğrudan Excel'de kullanma
services: machine-learning
documentationcenter: ''
author: marthalc
ms.author: marthalc
ms.assetid: 9618079d-502f-4974-a3e2-8f924042a23f
ms.service: machine-learning
ms.component: studio
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 2/1/2018
ms.openlocfilehash: 0d0274b88a15688106f185ede87f576443bdadb0
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51229364"
---
# <a name="excel-add-in-for-azure-machine-learning-studio-web-services"></a>Azure Machine Learning Studio web hizmetleri için Excel Eklentisi
Excel web hizmetleri herhangi bir kod yazmak zorunda kalmadan doğrudan çağırmak kolaylaştırır.

## <a name="steps-to-use-an-existing-web-service-in-the-workbook"></a>Çalışma kitabında mevcut bir web hizmetini kullanma adımları

1. Açık [örnek Excel dosyasını](https://aka.ms/amlexcel-sample-2), Excel eklentisi ve Yolcuların Kurtulacağını üzerinde hakkındaki verileri içerir. 
 
> [!NOTE]
> Web Hizmetleri listesi dosyasına ve altındaki bir onay kutusu "Otomatik-tahmin etmek için" ilgili görürsünüz. Etkinleştirirseniz, otomatik-tahminler tahmin **tüm** hizmetlerinizi güncelleştirilmiş her olacaktır girişleri bir değişiklik yoktur. İşaretli değilse, "Tahmin"'ye tıklayın yenileme için gerekir. Etkinleştirmek için otomatik-6. adım bir hizmet düzeyi gidin, tahmin edin.

2. Web hizmeti tıklayarak seçin-"Titanic hayatta tahmin unsuru (Excel Eklentisi örneği) [puan]" Bu örnekte.
   
    ![Web hizmeti seçin][01]
3. Sayfasına yönlendirileceksiniz **Predıct** bölümü.  Bu çalışma kitabı zaten örnek veriler içerir, ancak için boş bir çalışma kitabını Excel'de bir hücreyi seçin ve tıklayın **örnek verileri kullanarak**.
4. Üst bilgileri ile verileri seçin ve giriş verilerini aralığı simgesine tıklayın.  "Verilerimin üst bilgileri var" kutunun işaretli olduğundan emin olun.
5. Altında **çıkış**, çıktı olacak şekilde, örneğin "H1" burada istediğiniz sayı girin.
6. Tıklayın **tahmin**. "Otomatik predıct" onay kutusunu seçerseniz seçili alanlara (giriş olarak belirtilenler) herhangi bir değişiklik isteği ve bir güncelleştirme predıct düğmesine basın etmenize gerek kalmadan çıkış hücrelerin tetikler.
   
    ![Bölüm tahmin edin][02]

Bir web hizmeti dağıtın veya mevcut bir Web hizmetini kullanın. Bir web hizmeti dağıtma hakkında daha fazla bilgi için bkz. [izlenecek 5. adım: Azure Machine Learning Web hizmetini dağıtma](walkthrough-5-publish-web-service.md).

Web hizmetiniz için API anahtarını alın. Gerçekleştirdiğiniz durumlarda bu eylemi yeni Machine Learning web hizmeti bir Machine Learning Klasik web hizmeti mi yayımlanan bağlıdır.

**Klasik web hizmetini kullanın** 

1. Machine Learning Studio'da tıklayın **WEB Hizmetleri** bölümünde, sol bölmede ve web hizmeti seçin.
   
    ![Bir Web hizmeti Studio seçin][04]
2. Web hizmeti için API anahtarını kopyalayın.
   
    ![Studio API anahtarı][05]
3. Üzerinde **PANO** web hizmeti için sekmesinde **istek/yanıt** bağlantı.
4. Aranacak **istek URI** bölümü.  Kopyalayın ve URL'yi kaydedin.

> [!NOTE]
> Oturum açmak artık mümkündür [Azure Machine Learning Web Hizmetleri](https://services.azureml.net) Machine Learning Klasik web hizmeti için API anahtarını almak için portalı.
> 
> 

**Yeni bir web hizmetini kullanın**

1. İçinde [Azure Machine Learning Web Hizmetleri](https://services.azureml.net) portal'ı tıklatın **Web Hizmetleri**, ardından web hizmetinizi seçin. 
2. Tıklayın **tüketen**.
3. Aranacak **temel tüketim bilgileri** bölümü. Kopyalayıp kaydedin **birincil anahtar** ve **istek-yanıt** URL'si.

## <a name="steps-to-add-a-new-web-service"></a>Yeni bir web hizmeti eklemek için adımları

1. Bir web hizmeti dağıtın veya mevcut bir Web hizmetini kullanın. Bir web hizmeti dağıtma hakkında daha fazla bilgi için bkz. [izlenecek 5. adım: Azure Machine Learning Web hizmetini dağıtma](walkthrough-5-publish-web-service.md).
2. Tıklayın **tüketen**.
3. Aranacak **temel tüketim bilgileri** bölümü. Kopyalayıp kaydedin **birincil anahtar** ve **istek-yanıt** URL'si.
4. Excel'de, Git **Web Hizmetleri** bölümü (açıksa **Predıct** bölümünde, web hizmetleri listesine dönmek için geri okuna tıklayın).
   
    ![Web hizmet seçimi için Git][03]
5. Tıklayın **Web hizmeti Ekle**.
6. Eklenti metin kutusu etiketli Excel'e URL'yi yapıştırın **URL**.
7. API/birincil anahtarı etiketli metin kutusuna yapıştırın **API anahtarı**.
8. **Ekle**'ye tıklayın.
   
    ![Klasik Web hizmeti URL'sini ve API anahtarı.][06]
9. Web hizmetini kullanmak için yukarıdaki yönergeleri, "bir varolan web hizmeti kullanmak için adımlar." izleyin.

## <a name="sharing-your-workbook"></a>Çalışma kitabınızı paylaşma
Çalışma kitabınızı kaydederseniz, eklediğiniz web hizmetleri için API/birincil anahtar da kaydedilir. Bu çalışma kitabını yalnızca güvendiğiniz kişilerle paylaşması gerekir anlamına gelir.

Aşağıdaki yorum bölümünde veya herhangi bir soru sorun bizim [Forumu](https://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).

[01]: ./media/excel-add-in-for-web-services/image1.png
[02]: ./media/excel-add-in-for-web-services/image2.png
[03]: ./media/excel-add-in-for-web-services/image3.png
[04]: ./media/excel-add-in-for-web-services/image4.png
[05]: ./media/excel-add-in-for-web-services/image5.png
[06]: ./media/excel-add-in-for-web-services/image6.png
