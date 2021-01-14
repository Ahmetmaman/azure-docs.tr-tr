---
title: Görüntü İşleme yenilikler nelerdir?
titleSuffix: Azure Cognitive Services
description: Bu makale Görüntü İşleme hakkındaki haberleri içerir.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: overview
ms.date: 01/13/2021
ms.author: pafarley
ms.openlocfilehash: 33987be39258adc74cf4f88dbb0544f7026f6086
ms.sourcegitcommit: 0aec60c088f1dcb0f89eaad5faf5f2c815e53bf8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98183362"
---
# <a name="whats-new-in-computer-vision"></a>Görüntü İşleme yenilikleri

Hizmette nelerin yeni olduğunu öğrenin. Bu öğeler sürüm notları, videolar, blog gönderileri ve diğer bilgi türleri olabilir. Hizmette güncel kalmak için bu sayfaya yer işareti ekleyin.

## <a name="january-2021"></a>Ocak 2021

### <a name="spatial-analysis-container-update"></a>Uzamsal analiz kapsayıcısı güncelleştirmesi

Yeni bir özellik kümesiyle, [uzamsal analiz kapsayıcısının](spatial-analysis-container.md) yeni bir sürümü yayınlandı. Bu Docker kapsayıcısı, fiziksel ortamlar aracılığıyla insanlar ile hareketleri arasındaki uzamsal ilişkileri anlamak için gerçek zamanlı akış videosunu analiz etmenizi sağlar. 

* [Uzamsal analiz işlemleri](spatial-analysis-operations.md) , bir kişinin maske gibi bir koruyucu yüzeyi takmakta olup olmadığını algılamak için yapılandırılabilir. 
    * `personcount` `personcrossingline` `personcrossingpolygon` Parametresi yapılandırılarak, ve işlemleri için bir maske Sınıflandırıcısı etkinleştirilebilir `ENABLE_FACE_MASK_CLASSIFIER` .
    * Bu öznitelikler `face_mask` ve `face_noMask` video akışında algılanan her bir kişi için güvenirlik puanı olan meta veriler olarak döndürülecek


## <a name="october-2020"></a>Ekim 2020

### <a name="computer-vision-api-v31-ga"></a>Görüntü İşleme API'si v 3.1 GA

Genel kullanılabilirlik Görüntü İşleme API'si v 3.1 'e yükseltildi.

## <a name="september-2020"></a>Eylül 2020

### <a name="spatial-analysis-container-preview"></a>Uzamsal analiz kapsayıcısı önizlemesi

[Uzamsal analiz kapsayıcısı](spatial-analysis-container.md) artık önizlemededir. Görüntü İşleme uzamsal analiz özelliği, kişiler ve fiziksel ortamlar aracılığıyla taşınanlar arasındaki uzamsal ilişkileri anlamak için gerçek zamanlı akış videosunu analiz etmenizi sağlar. Uzamsal analiz, şirket içinde kullanabileceğiniz bir Docker kapsayıcısıdır. 

### <a name="read-api-v31-public-preview-adds-ocr-for-japanese"></a>Okuma API v 3.1 Genel Önizleme Japonca için OCR ekler
Görüntü İşleme Read API v 3.1 Genel önizlemesi şu özellikleri ekler:
* Japonca için OCR dili
* Her metin çizgisi için, görünümün el ile mi yoksa yazdırma stili mi olduğunu belirtin ve bir güven puanı (yalnızca Latin dilleri) ile birlikte görüntülenir.
* Birden çok sayfalı belge için yalnızca seçili sayfalar veya sayfa aralığı için metin ayıkla.

* Okuma API 'sinin bu önizleme sürümü Ingilizce, Felemenkçe, Fransızca, Almanca, Italyanca, Japonca, Portekizce, Basitleştirilmiş Çince ve Ispanyolca dilleri destekler.

Daha fazla bilgi için [okuma API 'sine genel bakış](concept-recognizing-text.md) bölümüne bakın.

> [!div class="nextstepaction"]
> [Okuma API v 3.1 genel önizleme 2 hakkında daha fazla bilgi edinin](https://westus2.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-preview-2/operations/5d986960601faab4bf452005)

## <a name="july-2020"></a>Temmuz 2020

### <a name="read-api-v31-public-preview-with-ocr-for-simplified-chinese"></a>Basitleştirilmiş Çince için OCR ile API v 3.1 Genel önizlemeyi okuyun
Görüntü İşleme okuma API v 3.1 genel önizleme, Basitleştirilmiş Çince desteği ekler.

* Okuma API 'sinin bu önizleme sürümü Ingilizce, Felemenkçe, Fransızca, Almanca, Italyanca, Portekizce, Basitleştirilmiş Çince ve Ispanyolca dilleri destekler.

Daha fazla bilgi için [okuma API 'sine genel bakış](concept-recognizing-text.md) bölümüne bakın.

> [!div class="nextstepaction"]
> [Okuma API v 3.1 Genel Önizleme 1 hakkında daha fazla bilgi edinin](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-preview-1/operations/5d986960601faab4bf452005)

## <a name="may-2020"></a>Mayıs 2020
Görüntü İşleme API'si v 3.0 genel kullanılabilirliği ve [API 'Yi okumak](concept-recognizing-text.md)için güncelleştirmeler ile girilmiş:

* Ingilizce, Felemenkçe, Fransızca, Almanca, Italyanca, Portekizce ve Ispanyolca desteği
* İyileştirilmiş doğruluk
* Ayıklanan her sözcüğün Güvenirlik puanı
* Yeni çıkış biçimi

## <a name="march-2020"></a>Mart 2020

* TLS 1,2, bu hizmete yönelik tüm HTTP istekleri için de zorlanır. Daha fazla bilgi için bkz. Azure bilişsel [Hizmetler güvenliği](../cognitive-services-security.md).

## <a name="january-2020"></a>Ocak 2020

### <a name="read-api-30-public-preview"></a>API 3,0 genel önizlemeyi okuyun

Artık görüntülerden yazdırılmış veya el yazısı metin ayıklamak için Read API 'sinin 3,0 sürümünü kullanma seçeneğiniz vardır. Önceki sürümlere kıyasla 3,0 şunları sağlar:
* İyileştirilmiş doğruluk
* Yeni çıkış biçimi
* Ayıklanan her sözcüğün Güvenirlik puanı
* Ek dil parametresiyle hem Ispanyolca hem de Ingilizce diller için destek

3,0 API 'sini kullanmaya başlamak için [Extract metin hızlı](./quickstarts/csharp-hand-text.md?tabs=version-3) başlangıcını izleyin.

## <a name="cognitive-service-updates"></a>Bilişsel hizmet güncelleştirmeleri

[Bilişsel hizmetler için Azure Update duyuruları](https://azure.microsoft.com/updates/?product=cognitive-services)