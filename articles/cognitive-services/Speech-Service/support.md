---
title: Destek ve Yardım seçenekleri
titlesuffix: Azure Cognitive Services
description: Nasıl yardım almak ve konuşma hizmeti ile tümleştirilen uygulamalar oluşturduğunuzda, soruları ve sorunları için destek
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/26/2018
ms.author: wolfma
ms.openlocfilehash: b8e6c8b125e8eeaadac2e6864b06d55c42d3b173
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57542011"
---
# <a name="support-and-help-options"></a>Destek ve Yardım seçenekleri

Konuşma hizmeti işlevselliğini keşfetmeye yeni başlayan? Uygulamanız için yeni bir özellik uyguluyor? Aşağıda, bir geliştirici olarak yardım alabileceğiniz hakkında öneriler yer almaktadır.

> [!div class="checklist"]
> * Yeni gelişmeler hakkında haberdar kalın *Azure Bilişsel Hizmetler*, veya ilgili son haberleri Bul *konuşma hizmeti*.
> * Sorununuzu topluluk tarafından ele alınan veya zaten uygulamak istediğiniz özelliği için varolan belgeleri varsa görmek için arama yapın.
> * Tatmin edici bir yanıt bulamazsanız, soru sorabilirsiniz *Stack Overflow*.
> * Github'da örnekleri biri ile ilgili bir sorun bulursanız, yükseltmek bir *GitHub* sorun.
> * Bir çözümde Ara *UserVoice forumumuzu*.

## <a name="stay-informed"></a>Bildirim alın

Bilişsel hizmetler ile ilgili Haberler toplanır [Bilişsel hizmetler blogu](https://azure.microsoft.com/blog/topics/cognitive-services/). Konuşma hizmeti hakkında en yeni bilgiler için izleme [konuşma hizmeti blog](https://azure.microsoft.com/blog/tag/speech-service/).

## <a name="search"></a>Arama

Belgeler, örnekler, gereksinim duyduğunuz yanıt bulabilir veya yanıtlarını [Stack Overflow](https://www.stackoverflow.com) sorular veya örnekleri.

### <a name="scoped-search"></a>Kapsamlı arama

Stack Overflow'da, belgeler için aramanızın kapsamını ve kod örnekleri aşağıdaki sorguyu kullanarak daha hızlı sonuçlar için [sık kullanılan arama motorunuz](https://bing.com):

```
{Your Search Terms} (site:stackoverflow.com OR site:docs.microsoft.com OR site:github.com/azure-samples)
```

Burada *{Your arama terimlerini}* arama anahtar sözcüklerinizi olduğu.

## <a name="create-an-azure-support-request"></a>Bir Azure destek isteği oluşturma

Azure müşterileri, oluşturun ve Azure portalında destek isteklerini yönetin.

* [Azure Portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)
* [Amerika Birleşik Devletleri kamu için Azure portalı](https://portal.azure.us)

## <a name="post-a-question-to-stack-overflow"></a>Stack Overflow için bir soru gönderin

Yığın taşması geliştirme ile ilgili sorular için tercih edilen bir kanaldır. Bu, burada topluluk üyeleri ve Microsoft Takım üyeleri doğrudan sorunlarınızı çözmenize yardımcı söz konusu olur.

Bir arama ile sorununuzun yanıtını bulamazsanız, yeni Stack Overflow soru gönderin. Soru formüle aşağıdaki etiketlerin birine kullanın:

|Bileşen/alan  |Etiketler  |
|---------|---------|
|Konuşma Tanıma |[[microsoft-bilişsel + Konuşmayı metne dönüştürme]](https://stackoverflow.com/questions/tagged/microsoft-cognitive+speech-to-text)|
|Konuşma sentezi |[[microsoft-bilişsel + metin okuma için]](https://stackoverflow.com/questions/tagged/microsoft-cognitive+text-to-speech)|
|Konuşma Çevirisi |[[microsoft-bilişsel + çeviri]](https://stackoverflow.com/questions/tagged/microsoft-cognitive+translation)|
|Konuşma amacı |[[microsoft-bilişsel + luıs]](https://stackoverflow.com/questions/tagged/microsoft-cognitive+luis)|
|Genel Speech SDK'sı |[[microsoft-bilişsel + microsoft-konuşma tanıma-API'si]](https://stackoverflow.com/questions/tagged/microsoft-cognitive+microsoft-speech-api)|

> [!TIP]
> Stack Overflow aşağıdaki gönderilerinden form soruların konusunda ipuçları içerir ve kaynak kodu ekleyin. Bu yönergeler, topluluk üyeleri değerlendirmek ve sorularınıza hızla yanıt şansını artırmak yardımcı olabilir:  
> * [İyi bir soru nasıl miyim?](https://stackoverflow.com/help/how-to-ask)
> * [En az, tam ve doğrulanabilir örnek oluşturma](https://stackoverflow.com/help/mcve)

## <a name="create-a-github-issue"></a>Bir GitHub sorunu oluşturun

Örnekler, genellikle açık kaynak gönderilir. Sorular ve sorunlar için oluşturma bir *sorunu* ilgili GitHub deposundaki. Bir çekme isteği çok gönderebilirsiniz. Aşağıdaki listede, örnek depoları bağlantılar içeriyor:

* [Konuşma SDK'sı](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues)
* [Cihaz SDK'sı](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK/issues)

Özellik isteği, bir hata raporu oluşturabilir veya genel bir soru sorun ve en iyi yöntemlerinizi paylaşın. Hata raporları için lütfen sağlanan şablon izleyin:

**Hatayı açıklayan**

Hata nedir, kısa ve öz açıklaması.

**Yeniden oluşturma**

Davranışı yeniden oluşturma adımları:
1. ...
2. ...

**Beklenen davranış**

Olmasını beklediğinizi ve anlaşılır açıklaması.

**Bilişsel hizmetler konuşma SDK sürümü**

SDK'sının hangi sürümünü kullandığınızı.

**Platform, işletim sistemi ve programlama dili**

 - İşletim sistemi: [örneğin Windows, Linux, Android, iOS,...] - Lütfen ayrıntılı belirtin
 - Donanım - x64, x86, ARM...
 - Tarayıcı [Örneğin, Chrome, Safari] (varsa)-Lütfen ayrıntılı belirtin

**Ek bağlam**

 - Hata iletileri, günlük bilgileri yığın izlemesini...
 - Bir hizmete etkileşimi için bir hata raporu, rapor edilen olay süresi (saat dilimi dahil) ve SessionID bildirin. SessionID tüm çağrı-telefonla geri/aldığınız olaylar bildirilir.
 - Herhangi bir ek bilgi


## <a name="uservoice-forum"></a>UserVoice forumumuzu

Bilişsel hizmetler oluşturmaya yönelik fikirlerinizi paylaşın ve eşlik eden API'leri geliştirdiğiniz uygulamalar için daha iyi çalışır. Büyüyen kendi bilgi Tabanımız yaygın soruların yanıtları bulmak için kullanın:

[UserVoice](https://cognitive.uservoice.com/)
