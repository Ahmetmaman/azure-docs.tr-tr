---
title: Microsoft Bing konuşma hizmeti | Microsoft Docs
description: Microsoft konuşma tanıma API'si, kullanıcıların gerçek zamanlı etkileşime dahil olmak üzere uygulamalarınıza sesle yönetilen işlemler eklemek için kullanın.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: d58642b95a60d4f1c83dfd969d0c76511dca4653
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43097404"
---
# <a name="microsoft-bing-speech-api-overview"></a>Microsoft Bing konuşma API'si genel bakış

Bulut tabanlı Microsoft Bing konuşma tanıma API'si, geliştiricilerin uygulamalarında, sesli komut denetimi, doğal konuşma konuşma ve konuşma transkripsiyonu ve dikte kullanarak kullanıcı iletişim gibi konuşma tanıma özellikli güçlü özellikler oluşturmak için kolay bir yol sağlar. Microsoft konuşma tanıma API'si her ikisini de destekler *Konuşmayı metne dönüştürme* ve *metin okuma* dönüştürme.

- **Konuşmayı metne dönüştürme** API İnsan konuşma uygulamanızı denetlemek için komutları veya girdi kullanılabilir metne dönüştürür.
- **Metin okuma** API uygulamanızın kullanıcıya çalınabilecek ses akışları metin dönüştürür.

> [!NOTE] 
> Mayıs 2018'de yeni yayımladık [konuşma hizmeti](../speech-service/overview.md) genel önizlemeye sunuldu. Öneriyoruz [ücretsiz denemenin](../speech-service/get-started.md).

## <a name="speech-to-text-speech-recognition"></a>Konuşmadan metne (konuşma tanıma)

Microsoft konuşma tanıma API'si *dönüştürür* uygulamanızı görüntülemek için kullanıcı veya olarak alacak bir metne ses akışları giriş komutu. Geliştiriciler, uygulamalarına konuşma eklemek iki yol sunar: REST API'ler **veya** Websocket tabanlı istemci kitaplıkları.

- [REST API'leri](GetStarted/GetStartedREST.md): geliştiriciler, konuşma tanıma hizmeti uygulamalarını HTTP çağrıları kullanabilir.
- [İstemci kitaplıkları](GetStarted/GetStartedClientLibraries.md): Gelişmiş özellikler için geliştiriciler Microsoft Speech istemci kitaplıklarını indirin ve kendi uygulamalarınızda bağlantı.  İstemci kitaplıkları (C#, Java, JavaScript, ObjectiveC) farklı dilleri kullanan çeşitli platformlarda (Windows, Android, iOS) kullanılabilir. REST API'ler farklı olarak, istemci kitaplıkları Websocket tabanlı protokolü kullanır.

| Uygulama alanları | [REST API'ler](GetStarted/GetStartedREST.md) | [İstemci kitaplıkları](GetStarted/GetStartedClientLibraries.md) |
|-----|-----|-----|
| Dönüştürme kısa konuşmada geçen bir ses (ses uzunluğu < 15 s) geçiş sonuçları gibi komutları. | Evet | Evet |
| Uzun bir ses (> 15 s) dönüştürme | Hayır | Evet |
| İstenen Ara sonuçlarla Stream ses | Hayır | Evet |
| LUIS kullanarak seslerden Dönüştürülen metin anlama | Hayır | Evet |

Hangi yaklaşımın geliştiriciler (REST API veya istemci kitaplıkları) seçin, Microsoft konuşma hizmeti aşağıdakileri destekler:

- Konuşma tanıma teknolojileri Cortana, dikte Office, Office Translator ve diğer Microsoft ürünleri tarafından kullanılan Microsoft tarafından sunulan Gelişmiş.
- Gerçek zamanlı sürekli tanıma. Konuşma tanıma API'si, kullanıcıların gerçek zamanlı ve destekler şimdiye tanınan bir kelimelerin Ara sonuçlar almak için metne ses özelliği sağlar. Konuşma hizmeti, konuşma uç algılama da destekler. Ayrıca, kullanıcılar, büyük/küçük harf ve noktalama işaretleri ve maskeleme küfür metin normalleştirme gibi ek biçimlendirme özellikleri seçebilirsiniz.
- Destekler, konuşma tanıma sonuçları için iyileştirilmiş *etkileşimli*, *konuşma*, ve *dikte* senaryoları. Özel dil modelleri ve akustik modeller gerektiren kullanıcı senaryoları için [özel konuşma hizmeti](../custom-speech-service/cognitive-services-custom-speech-home.md) uygulamanız ve kullanıcılarınız için uyarlanmış konuşma modelleri oluşturmanıza olanak sağlar.
- Birçok konuşulan dili içinde birden çok diyalektleri destekler. Her tanıma modunda desteklenen dillerin tam listesi için bkz. [tanıma diller](api-reference-rest/supportedlanguages.md).
- Language understanding ile tümleştirme. Giriş Sesi metne dönüştürme yanı sıra *Konuşmayı metne dönüştürme* uygulamaları metin ne anlama geldiğini anlamak için ek bir özellik sağlar. Kullandığı [Language Understanding Intelligent Service(LUIS)](../LUIS/what-is-luis.md) amaç ve varlıkları tanınan ından cümleler ayıklamak için.

### <a name="next-steps"></a>Sonraki adımlar

- Microsoft konuşma tanıma hizmeti ile kullanmaya başlama [REST API'leri](GetStarted/GetStartedREST.md) veya [istemci kitaplıkları](GetStarted/GetStartedClientLibraries.md).
- Kullanıma [örnek uygulamalar](samples.md) tercih ettiğiniz programlama dili.
- Bulunacak başvuru bölümüne Git [Microsoft konuşma Protokolü](API-Reference-REST/websocketprotocol.md) ayrıntıları ve API başvuruları.

## <a name="text-to-speech-speech-synthesis"></a>Metin okuma (konuşma sentezi)

*Metin okuma* API'lerini yapılandırılmış metin bir ses akışına dönüştürmek için REST kullanma. API'leri, çeşitli seslerle ve dilleri hızlı metinleri konuşmaya dönüştürme sağlar. Ayrıca kullanıcılar da telaffuz, birim, aralık vb. gibi ses özelliklerini değiştirmek için sahipsiniz. SSML'yi kullanarak etiketler.

### <a name="next-steps"></a>Sonraki adımlar

- Microsoft metin okuma hizmetini kullanmaya başlama: [metin okuma API Başvurusu için](api-reference-rest/bingvoiceoutput.md). Diller ve seslerle metin okuma tarafından desteklenen tam listesi için bkz: [desteklenen yerel ayarlar ve ses tiplerini](api-reference-rest/bingvoiceoutput.md#SupLocales).
