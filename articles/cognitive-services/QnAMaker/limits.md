---
title: Sınırları ve sınır - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu, bölümlerini Bilgi Bankası ve hizmet için meta-sınırlara sahiptir. Sınama ve yayımlama için bu sınırları içinde bilgi bankanızı tutmak önemlidir.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/24/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 6f36000eb5a17e58d1450a064897dd9caef89bad
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55859360"
---
# <a name="qna-maker-knowledge-base-limits-and-boundaries"></a>Soru-cevap Oluşturucu Bilgi Bankası sınırları ve sınır
Soru-cevap Oluşturucu arasında sınırları kapsamlı bir listesi.

## <a name="knowledge-bases"></a>Bilgi bankaları

* Bilgi bankaları en fazla sayısını temel alarak [Azure arama katmanı sınırları](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)

|**Azure arama katmanı** | **Ücretsiz** | **Temel** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Yayımlanan bilgi bankalarından izin verilen en yüksek sayısı|2|14|49|199|199|2,999|

 Örneğin, 15 izin verilen dizinler katmanınızı varsa 14 bilgi bankalarından (yayımlanan Bilgi Bankası başına 1 dizini) yayımlayabilirsiniz. On beşinci dizini `testkb`, geliştirme ve test için tüm bilgi bankaları için kullanılır. 

## <a name="extraction-limits"></a>Ayıklama sınırları
* En fazla ayıklanabileceği dosya sayısını ve en büyük dosya boyutu: Bkz: [QnAMaker fiyatlandırması](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/)
* SSS HTML sayfaları için ayıklama bankalarıyla gezinilebilen derin bağlantılar sayısı üst sınırı: 20

## <a name="metadata-limits"></a>Meta veri sınırları
* Meta veri alanları Bilgi Bankası başına en fazla sayısını temel alarak [Azure arama katmanı sınırları](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)

|**Azure arama katmanı** | **Ücretsiz** | **Temel** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Soru-cevap Oluşturucu hizmeti (genelinde tüm KB'leri) başına en fazla meta veri alanları|1000|100 *|1000|1000|1000|1000|

## <a name="knowledge-base-content-limits"></a>Bilgi Bankası içerik sınırları
Bilgi Bankası'nda içeriği genel sınırlamaları:
* Yanıt metnin uzunluğu: 25,000
* Soru metnin uzunluğu: 1000
* Meta verileri anahtar/değer metnin uzunluğu: 100
* Meta veri adı için desteklenen karakterler: Harfler, rakamlar ve _  
* Meta veri değeri için desteklenen karakterler: Hariç: ve | 
* Dosya adının uzunluğu: 200
* Desteklenen dosya biçimleri: ".tsv", ".pdf", ".txt", ".docx", ".xlsx".
* Diğer sorular sayısı üst sınırı: 100
* Soru-cevap çiftlerini sayısı üst sınırı: Bağımlı [Azure arama katmanı](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity#document-limits) seçildi 

## <a name="create-knowledge-base-call-limits"></a>Bilgi Bankası araması sınırları oluşturun:
Bu temsil Bilgi Bankası eylem her sınırları oluşturmak; diğer bir deyişle, tıklayıp *oluşturma KB* veya CreateKnowledgeBase API'ye çağrı yapma.
* Diğer sorular yanıt başına en fazla sayısı: 100
* URL maksimum sayısı: 10
* En fazla dosya sayısı: 10

## <a name="update-knowledge-base-call-limits"></a>Bilgi Bankası araması sınırları güncelleştir
Bunlar, her güncelleştirme eylemi sınırlarını temsil eder; diğer bir deyişle, tıklayıp *kaydedin ve eğitme* veya UpdateKnowledgeBase API'ye çağrı yapma.
* Her kaynak adının uzunluğu: 300
* En fazla sayı eklendiğinde veya silindiğinde diğer sorular: 100
* En fazla eklendiğinde veya silindiğinde meta verileri alan sayısı: 10
* Yenilenebilir URL'leri sayısı üst sınırı: 5

## <a name="next-steps"></a>Sonraki adımlar

Ne zaman ve hizmet katmanlarını değiştirme öğrenin:

* [Soru-cevap Oluşturucu](how-to/upgrade-qnamaker-service.md#upgrade-qna-maker-management-sku): fiyatlandırma katmanı, soru-cevap Oluşturucu hizmetini yükseltmeniz geçerli katmanınızı ötesinde, Bilgi Bankası'nda daha fazla sorularını ve yanıtlarını olması gerekir.
* [Arama](how-to/upgrade-qnamaker-service.md#upgrade-app-service) - bilgi bankanızı gerektiğinde istemci uygulamanıza ilişkin daha fazla isteklere hizmet app service fiyatlandırma katmanına yükseltin.
* [App service](how-to/upgrade-qnamaker-service.md#upgrade-azure-search-service): birçok bilgi bankaları planlama yaparken, Azure Search hizmetinizin fiyatlandırma katmanı yükseltin.
