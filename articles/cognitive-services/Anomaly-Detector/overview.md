---
title: Anomali Algılayıcısı API'si nedir?
titleSuffix: Azure Cognitive Services
description: Zaman serisi verilerinize anomali algılama uygulamak için anomali algılayıcı API 'sinin algoritmalarını kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: overview
ms.date: 09/22/2020
ms.author: aahi
keywords: anomali algılama, makine öğrenimi, algoritmalar
ms.custom: cog-serv-seo-aug-2020
ms.openlocfilehash: 2e48c189df23ed94cd54e1bc9097b7d9709c9ed3
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91319325"
---
# <a name="what-is-the-anomaly-detector-api"></a>Anomali Algılayıcısı API'si nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Anomali algılayıcı API 'SI, makine öğrenimini bilmeniz gerekmeden zaman serisi verilerinizde bulunan normalleştirikleri izlemenize ve algılamanıza olanak sağlar. Anomali algılayıcı API 'sinin algoritmaları, sektör, senaryo veya veri hacminin ne olursa olsun verilerinize en uygun modelleri otomatik olarak tanımlayarak ve uygulayarak uyarlar. API, zaman serisi verilerinizi kullanarak anomali algılama, beklenen değerler ve hangi veri noktalarının güvenlik sınırlarını belirler.

![Hizmet isteklerindeki model değişikliklerini Algıla](./media/anomaly_detection2.png)

Anomali algılayıcısının kullanılması, Machine Learning 'de önceki bir deneyim gerektirmez ve yeniden işleme API 'SI, hizmeti uygulamalarınızın ve süreçlerinizle kolayca tümleştirmenize olanak sağlar.

## <a name="features"></a>Özellikler

Anomali algılayıcısı sayesinde, zaman serisi verileriniz genelinde veya gerçek zamanlı olarak oluşan anormallikleri otomatik olarak tespit edebilirsiniz.

|Özellik  |Açıklama  |
|---------|---------|
|Gerçek zamanlı anomali algılama. | En son birinin bir anomali olup olmadığını belirlemek için daha önce görülmüş veri noktalarını kullanarak akış verilerinizde bozukluklar algılayın. Bu işlem, göndereceğiniz veri noktalarını kullanarak bir model oluşturur ve hedef noktanın bir anomali olup olmadığını belirler. Oluşturduğunuz her yeni veri noktasıyla API çağırarak, verileri oluşturulduğu gibi izleyebilirsiniz. |
|Veri kümesinin tamamında bir toplu iş olarak, anormallikleri tespit edin. | Verilerinizin tamamında mevcut olabilecek tüm anormallikleri algılamak için zaman serinizi kullanın. Bu işlem, her bir noktanın aynı modelle çözümlenme işleminde, tüm zaman serisi verilerinizi kullanarak bir model oluşturur.         |
|Veri kümesi genelinde bir toplu iş olarak değişiklik noktalarını tespit edin. | Verilerinizde bulunan tüm eğilim değişiklik noktalarını algılamak için zaman serinizi kullanın. Bu işlem, her bir noktanın aynı modelle çözümlenme işleminde, tüm zaman serisi verilerinizi kullanarak bir model oluşturur.    |
| Verileriniz hakkında daha fazla bilgi alın. | Verileriniz ve beklenen değerler, anomali sınırları ve pozisyonları dahil tüm gözlemlenen bozukluklar hakkında yararlı Ayrıntılar alın. |
| Anomali algılama sınırlarını ayarlayın. | Anomali algılayıcı API 'SI, anomali algılama için sınırları otomatik olarak oluşturur. Bu sınırları, API 'nin veri bozuklularındaki duyarlılığını artırmak veya azaltmak için ayarlayın ve verilerinize daha iyi uyum sağlar. |

## <a name="demo"></a>Tanıtım

Anomali algılayıcısının nasıl çalıştığını anlamak için bu [etkileşimli tanıtımı](https://aka.ms/adDemo) inceleyin.
Tanıtımı çalıştırmak için bir anomali algılayıcısı kaynağı oluşturmanız ve API anahtarını ve uç noktasını almanız gerekir.

## <a name="notebook"></a>Not Defteri

Anomali algılayıcı API 'sini çağırmayı öğrenmek için bu [Not defterini](https://aka.ms/adNotebook)deneyin. Bu Jupyter Notebook, bir API isteğinin nasıl gönderileceğini ve sonucu görselleştirmeyi gösterir.

Not defterini çalıştırmak için aşağıdaki adımları gerçekleştirin:

1. Geçerli bir anomali algılayıcı API 'SI abonelik anahtarı ve bir API uç noktası alın. Aşağıdaki bölümde kaydolma talimatları bulunur.
1. Oturum açın ve sağ üst köşedeki Kopyala ' ya tıklayın.
1. Kopyalama işlemini tamamlamadan önce iletişim kutusundaki "genel" seçeneğinin işaretini kaldırın, aksi takdirde tüm abonelik anahtarları dahil Not defteriniz herkese açık olur.
1. **Ücretsiz işlem sırasında Çalıştır 'a** tıklayın
1. Not defterlerinden birini seçin.
1. Geçerli anomali algılayıcısı API abonelik anahtarınızı `subscription_key` değişkenine ekleyin.
1. Değişkeni bitiş noktanızla değiştirin `endpoint` . Örnek: `https://westus2.api.cognitive.microsoft.com/anomalydetector/v1.0/timeseries/last/detect`
1. Üstteki menü çubuğunda **hücre**' ya ve ardından **Tümünü Çalıştır**' ı tıklatın.

## <a name="workflow"></a>İş akışı

Anomali algılayıcı API 'SI, bir daha fazla Web hizmeti olduğundan, HTTP istekleri yapıp JSON 'u ayrıştırabilen herhangi bir programlama dilinden çağrı yapmayı kolaylaştırır.

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]

Kaydolduktan sonra:

1. Zaman serisi verilerinizi alın ve geçerli bir JSON biçimine dönüştürün. En iyi sonuçları elde etmek için verilerinizi hazırlarken [en iyi uygulamaları](concepts/anomaly-detection-best-practices.md) kullanın.
1. Anomali algılayıcısı API 'sine verilerinize bir istek gönderin.
1. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.

## <a name="algorithms"></a>Algoritmalar

* Kullanılan algoritmalar hakkında daha fazla bilgi için aşağıdaki teknik bloglara bakın:
    * [Azure Anomali Algılayıcısı API’sine giriş](https://techcommunity.microsoft.com/t5/AI-Customer-Engineering-Team/Introducing-Azure-Anomaly-Detector-API/ba-p/490162)
    * [Azure anomali algılayıcısı 'nda SR-CNN algoritmasına genel bakış](https://techcommunity.microsoft.com/t5/AI-Customer-Engineering-Team/Overview-of-SR-CNN-algorithm-in-Azure-Anomaly-Detector/ba-p/982798)

Microsoft 'un geliştirdiği SR-CNN algoritmaları hakkında daha fazla bilgi edinmek için [Microsoft 'ta kağıt zaman serisi anomali algılama hizmeti](https://arxiv.org/abs/1906.03821) 'ni okuyabilirsiniz (kdd 2019 tarafından kabul edilir).


> [!VIDEO https://www.youtube.com/embed/ERTaAnwCarM]

## <a name="join-the-anomaly-detector-community"></a>Anomali Algılayıcısı topluluğuna katılın

* [Microsoft ekiplerinde anomali algılayıcı danışmanları grubuna](https://aka.ms/AdAdvisorsJoin) katılarak
* Seçilen [Kullanıcı tarafından oluşturulan içeriğe](user-generated-content.md) bakın

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: anomali algılayıcısının kullanıldığı zaman serisi verilerinizde bozukluklar Algıla REST API](quickstarts/detect-data-anomalies-csharp.md)
* Anomali algılayıcı API 'SI [çevrimiçi tanıtımı](https://notebooks.azure.com/AzureAnomalyDetection/projects/anomalydetector)
* Anomali algılayıcısı [REST API başvurusu](https://aka.ms/anomaly-detector-rest-api-ref)
