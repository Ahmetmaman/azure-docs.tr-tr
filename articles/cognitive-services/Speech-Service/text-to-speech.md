---
title: Metinden konuşmaya genel bakış-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma hizmetindeki metinden konuşmaya özelliği, uygulamalarınızın, araçlarınızın veya cihazların metni doğal insan benzeri sentezleştirilmiş konuşmaya dönüştürmesine olanak sağlar. Bu makale, metinden konuşmaya hizmetinin avantajları ve özelliklerine genel bir bakış sunar.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/01/2020
ms.author: trbye
ms.custom: cog-serv-seo-aug-2020
keywords: Metinden konuşmaya
ms.openlocfilehash: 54db794d4e9cf0cd2a049a865f2e7bb52698a4bd
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98599532"
---
# <a name="what-is-text-to-speech"></a>Metin okuma nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Bu genel bakışta, uygulamanızın, araçların veya cihazların metni insan benzeri sentezleştirilmiş konuşmaya dönüştürmesini sağlayan, metinden konuşmaya hizmetinin avantajları ve özellikleri hakkında bilgi edinebilirsiniz. Standart ve sinir sesler arasından seçim yapın ya da ürün veya marka için benzersiz özel bir ses oluşturun. 75 + standart sesler 45 ' den fazla dilde ve yerel ayarlarda kullanılabilir ve bir dizi dilde ve yerel ayara 5 sinir seste erişilebilir. Desteklenen seslerin, dillerin ve yerel ayarların tam listesi için bkz. [desteklenen diller](language-support.md#text-to-speech).

> [!NOTE]
> Bing Konuşma, 15 Ekim 2019 ' de kullanımdan çıkarıldı. Uygulamalarınız, araçlar veya ürünleriniz Bing Konuşma API 'Leri veya Özel Konuşma Tanıma kullanıyorsa, konuşma hizmetine geçiş yapmanıza yardımcı olacak kılavuzlar oluşturduk.
> - [Bing Konuşma konuşma hizmetine geçirme](how-to-migrate-from-bing-speech.md)

## <a name="core-features"></a>Temel Özellikler

* Konuşma birleştirme-standart, sinir veya özel sesler kullanarak metin okumayı dönüştürmek için [konuşma SDK 'sını](./get-started-text-to-speech.md) veya [REST API](rest-text-to-speech.md) kullanın.

* Zaman uyumsuz birleştirme uzun seslidir-metin okuma dosyalarını 10 dakikadan uzun zaman uyumsuz bir şekilde birleştirmek için [uzun ses API](long-audio-api.md) 'sini kullanın (örneğin, ses defterleri veya seminerler). Konuşma SDK 'Sı veya konuşmadan metne REST API kullanarak senkinden farklı olarak, yanıtlar gerçek zamanlı olarak döndürülmez. Beklentiler, isteklerin zaman uyumsuz olarak gönderilmesi, yanıtların yoklandığının ve hizmetten kullanıma hazır hale getirilme sesinin indirilmesinden kaynaklandır. Yalnızca özel sinir sesleri desteklenir.

* Standart sesler-Istatistiksel parametrik sen, ve/veya birleştirme birleştirme tekniklerini kullanarak oluşturulur. Bu sesler, yüksek oranda anlaşılır ve sessiz doğal bir şekilde yapılır. Uygulamalarınızın çok sayıda dilde 45 konuşmasını, çok çeşitli ses seçenekleriyle kolayca sağlayabilirsiniz. Bu sesler, kısaltmalar, kısaltma genişletmeleri, tarih/saat yorumlamalar, polyphones ve daha fazlası için destek de dahil olmak üzere yüksek telaffuz doğruluk sağlar. Standart seslerin tam listesi için bkz. [desteklenen diller](language-support.md#text-to-speech).

* Sinir seslerde, geleneksel konuşma senillerinin sınırlarını aşmak için derin sinir ağları, konuşulan dilde stres ve kullanım gibi Prosody tahmini ve ses birleştirmesinin eşzamanlı olarak gerçekleştirilmesi, daha akıcı ve doğal bir çıkış ile sonuçlanır. Sinir sesleri, chatbots ve ses yardımcılarıyla daha doğal ve etkileyici bir şekilde etkileşim kurmak, e-kitaplar gibi dijital metinleri audiobooks 'a dönüştürmek ve oto içi gezinti sistemlerini geliştirmek için kullanılabilir. İnsan benzeri doğal Prosody ve sözcüklerin bir kısmını temizleyerek, AI sistemleriyle etkileşime geçerek sinir seslileri büyük ölçüde dinlemeyi azaltır. Sinir sesin tam listesi için bkz. [desteklenen diller](language-support.md#text-to-speech).

* SSML ile konuşma stillerini ayarlama-konuşma Sensleme dili (SSML), konuşmadan metne çıktıları özelleştirmek için kullanılan XML tabanlı bir biçimlendirme dilidir. SSML ile, aralığı ayarlayabilir, duraklamalar ekleyebilir, söylenişi artırabilir veya yavaşlatır, konuşma hızını artırabilir veya azaltabilir, hacmi artırabilir veya azaltabilirsiniz ve tek bir belgeye birden çok ses özniteliği ekleyebilirsiniz. Konuşma stillerini [ayarlama için bkz](speech-synthesis-markup.md) ..

## <a name="get-started"></a>başlarken

Metinden konuşmaya başlamak için [hızlı](get-started-text-to-speech.md) başlangıca bakın. Metinden konuşmaya hizmeti, [konuşma SDK 'sı](speech-sdk.md), [REST API](rest-text-to-speech.md)ve [konuşma CLI](spx-overview.md) aracılığıyla kullanılabilir

## <a name="sample-code"></a>Örnek kod

Metinden konuşmaya yönelik örnek kod GitHub ' da kullanılabilir. Bu örnekler, en popüler programlama dillerinde metinden konuşmaya dönüştürmeyi kapsar.

- [Metinden konuşmaya örnekleri (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
- [Metinden konuşmaya örnekleri (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="customization"></a>Özelleştirme

Standart ve sinir seslerinizin yanı sıra, ürün veya marka için benzersiz özel sesler oluşturabilir ve bunları hassas bir şekilde ayarlayabilirsiniz. Kullanmaya başlamak her şey için çok sayıda ses dosyası ve ilişkili dökümlerdir. Daha fazla bilgi için bkz. [özel sesle çalışmaya başlama](how-to-custom-voice.md)

## <a name="pricing-note"></a>Fiyatlandırma notunun

Metin okuma hizmeti kullanılırken, noktalama, noktalama dahil olmak üzere her bir karakter için faturalandırılırsınız. SSML belgesinin kendisi faturalanabilir olsa da, metnin, alfabesine ve sıklık gibi bir şekilde konuşmaya dönüştürülmesini ayarlamak için kullanılan isteğe bağlı öğeler faturalanabilir karakter olarak sayılır. Faturalandırılabilir nelerin listesi aşağıda verilmiştir:

- İsteğin SSML gövdesinde metin okuma hizmetine geçilen metin
- SSML biçimindeki istek gövdesinin metin alanı içindeki `<speak>` ve etiketleri hariç tüm biçimlendirme `<voice>`
- Harfler, noktalama, boşluk, sekme, biçimlendirme ve tüm beyaz boşluk karakterleri
- Unicode 'da tanımlanan her kod noktası

Ayrıntılı bilgi için bkz. [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

> [!IMPORTANT]
> Her Çince, Japonca ve Kore dili karakteri faturalandırma için iki karakter olarak sayılır.

## <a name="reference-docs"></a>Başvuru belgeleri

- [Konuşma SDK'sı](speech-sdk.md)
- [REST API: metinden konuşmaya](rest-text-to-speech.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Ücretsiz bir konuşma hizmeti aboneliği alın](overview.md#try-the-speech-service-for-free)
- [Konuşma SDK 'sını alın](speech-sdk.md)