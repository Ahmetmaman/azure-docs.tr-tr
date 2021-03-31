---
title: "ML Studio (klasik): Excel 'de Web hizmetini tüketme-Azure"
description: Azure Machine Learning Studio (klasik), herhangi bir kod yazmaya gerek kalmadan Web hizmetlerini doğrudan Excel 'den çağırmayı kolaylaştırır.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio-classic
ms.topic: how-to
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 02/01/2018
ms.openlocfilehash: e8e7404a2b643cf90866253e5a7453381b66f73e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "100517867"
---
# <a name="consuming-an-azure-machine-learning-studio-classic-web-service-from-excel"></a>Excel 'den bir Azure Machine Learning Studio (klasik) Web hizmeti kullanma

**Uygulama hedefi:** ![ İçin geçerlidir. ](../../../includes/media/aml-applies-to-skus/yes.png) Machine Learning Studio (klasik) ![ için geçerlidir.](../../../includes/media/aml-applies-to-skus/no.png)[ Azure Machine Learning](../overview-what-is-machine-learning-studio.md#ml-studio-classic-vs-azure-machine-learning-studio)  

Azure Machine Learning Studio (klasik), herhangi bir kod yazmaya gerek kalmadan Web hizmetlerini doğrudan Excel 'den çağırmayı kolaylaştırır.

Excel 2013 (veya üzeri) veya Excel Online kullanıyorsanız, Excel [Excel eklentisini](excel-add-in-for-web-services.md)kullanmanızı öneririz.



## <a name="steps"></a>Adımlar
Web hizmeti yayımlayın. [Öğretici 3: Kredi risk modelini dağıtma](tutorial-part3-credit-risk-deploy.md) , bunun nasıl yapılacağını açıklar. Şu anda Excel çalışma kitabı özelliği yalnızca tek bir çıkışı olan Istek/yanıt Hizmetleri için desteklenir (yani, tek bir Puanlama etiketi). 

Bir Web hizmetiniz olduktan sonra, Studio 'nun sol tarafındaki **Web Hizmetleri** bölümüne tıklayın ve sonra Excel 'den kullanmak için Web hizmeti ' ni seçin.

**Klasik Web hizmeti**

1. Web hizmetinin **Pano** sekmesinde, **istek/yanıt** hizmeti için bir satırdır. Bu hizmetin tek bir çıkışı varsa, bu satırda **Excel çalışma kitabı indir** bağlantısını görmeniz gerekir.

    ![Studio (klasik) Web hizmeti portalını kullanarak Excel çalışma kitabını indirme](./media/consuming-from-excel/excellink.png)
2. **Excel çalışma kitabını indir**' e tıklayın.

**Yeni Web hizmeti**

1. Azure Machine Learning Web hizmeti portalında **tüketme**' yi seçin.
2. Kullanım sayfasında, **Web hizmeti tüketim seçenekleri** bölümünde, Excel simgesine tıklayın.

**Çalışma kitabını kullanma**

1. Çalışma kitabını açın.
2. Bir güvenlik uyarısı görünür; **Düzenle 'Yi etkinleştir** düğmesine tıklayın.

    ![Korumalı Görünüm güvenlik uyarısını kaldırmak için Düzenle 'yi etkinleştir](./media/consuming-from-excel/enableeditting.png)
3. Bir güvenlik uyarısı görüntülenir. Elektronik tablonuzun makrolarını çalıştırmak için **Içeriği etkinleştir** düğmesine tıklayın.

    ![Makroları devre dışı bırakma güvenlik uyarısını kapatmak için Içeriği etkinleştirin](./media/consuming-from-excel/enablecontent.png)
4. Makrolar etkinleştirildikten sonra bir tablo oluşturulur. , RR 'ler Web hizmetine veya **parametrelerine** giriş olarak Blue sütunları gereklidir. RR hizmetinin çıkışını, **tahmin EDILEN değerleri** yeşil olarak dikkate alın. Belirli bir satır için tüm sütunlar dolduğunda, çalışma kitabı Puanlama API 'sini otomatik olarak çağırır ve puanlanmış sonuçları görüntüler.

    ![Parametre girişleri ve sonuç tahmini değerleri için tablo](./media/consuming-from-excel/sampletable.png)
5. Birden fazla satıra puan vermek için ikinci satırı verilerle doldurup tahmin edilen değerler üretilir. Aynı anda birden çok satırı yapıştırabilirsiniz.

Verilerin görselleştirilmesine yardımcı olmak için, tahmin edilen değerlerle Excel özelliklerinden herhangi birini (grafikler, Power Map, koşullu biçimlendirme vb.) kullanabilirsiniz.

## <a name="sharing-your-workbook"></a>Çalışma kitabınızı paylaşma
Makroların çalışması için API anahtarınızın elektronik tablonun parçası olması gerekir. Bu, çalışma kitabını yalnızca güvendiğiniz varlıklarla/kişilerle paylaşmanız gerektiği anlamına gelir.

## <a name="automatic-updates"></a>Otomatik güncelleştirmeler
Bu iki durumda bir RR çağrısı yapılır:

1. Bir satırda, tüm **parametrelerinde** içerik olduğunda ilk kez
2. **Parametrelerin** tüm **parametreleri** girilen bir satırdaki herhangi bir zaman değişir.