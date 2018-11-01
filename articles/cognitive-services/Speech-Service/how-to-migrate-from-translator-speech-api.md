---
title: Translator konuşma API'sini konuşma Service'a dönüştürme
titleSuffix: Azure Cognitive Services
description: Uygulamalarınızı konuşma hizmeti için Translator konuşma tanıma API'SİNDEN geçirmeyi öğrenin.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: aahi
ms.openlocfilehash: 81513819fd60dc088c2ed4a781562684c84e803a
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50415483"
---
# <a name="migrate-from-the-translator-speech-api-to-the-speech-service"></a>Translator konuşma API'sini konuşma Service'a dönüştürme

Microsoft Translator konuşma tanıma API'si için uygulamalarınızı geçirmek için bu makaleyi kullanın [konuşma hizmeti](index.yml). Bu kılavuz, Translator konuşma tanıma API'si ve konuşma hizmeti arasındaki farkları özetler ve uygulamalarınızı geçiş stratejileri önerir.

> [!NOTE]
> Translator konuşma tanıma API'si abonelik anahtarınızı konuşma hizmeti tarafından kabul edilmez. Yeni bir konuşma tanıma hizmeti abonelik başlatmak gerekir.

## <a name="comparison-of-features"></a>Özelliklerin karşılaştırması

| Özellik                                           | Translator Konuşma Çevirisi API’si                                  | Konuşma Hizmeti | Ayrıntılar                                                                                                                                                                                                                                                                            |
|---------------------------------------------------|-----------------------------------------------------------------|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Metin çevirisi                               | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Konuşma çevirisi teknolojisini                             | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Genel uç noktası                                   | :heavy_check_mark:                                              | : heavy_minus_sign:                 | Konuşma hizmeti genel bir uç noktası şu anda sunmaz. Genel bir uç noktası otomatik olarak trafiği uygulamanızdaki gecikme süresini düşüren, bölgesel uç noktasına yakın yönlendirebilir.                                                    |
| Bölgesel uç noktaları                                | : heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Bağlantı zaman sınırı                             | 90 dakika                                               | SDK'sı ile sınırsız. 10 dakika WebSockets bağlantısı.                                                                                                                                                                                                                                                                                   |
| Üst bilgisindeki kimlik doğrulama anahtarı                                | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Tek bir istekte birden çok dil çevirisi | : heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Kullanılabilir SDK'lar                                    | : heavy_minus_sign:                                              | :heavy_check_mark:                 | Bkz: [konuşma hizmeti belgeleri](index.yml) kullanılabilir SDK'ları için.                                                                                                                                                    |
| WebSockets bağlantıları                             | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Dil API'si                                     | :heavy_check_mark:                                              | : heavy_minus_sign:                 | Konuşma hizmeti aynı açıklanan dilleri destekler [Translator API dil başvurusu](../translator-speech/languages-reference.md) makalesi. |
| Küfür filtresini ve işaret                       | : heavy_minus_sign:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| . WAV/PCM giriş                                 | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Giriş olarak diğer dosya türleri                         | : heavy_minus_sign:                                              | : heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Kısmi sonuçlar                                   | :heavy_check_mark:                                              | :heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Zamanlama bilgisi                                       | :heavy_check_mark:                                              | : heavy_minus_sign:                 |                                                                                                                                                                 |
| Bağıntı Kimliği                                    | :heavy_check_mark:                                              | : heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Özel konuşma modelleri                              | : heavy_minus_sign:                                              | :heavy_check_mark:                 | Konuşma hizmeti, konuşma tanıma, kuruluşunuzun benzersiz kelime için özelleştirmenize olanak sağlayan özel konuşma modeli sunar.                                                                                                                                           |
| Özel çeviri modellerini                         | : heavy_minus_sign:                                              | :heavy_check_mark:                 | Microsoft metin çevirisi API'sine abone kullanmanıza olanak sağlar [özel Translator](https://www.microsoft.com/translator/business/customization/) (için daha doğru çevirileri kendi verilerinizi kullanmak için şu anda Önizleme).                                                 |

## <a name="migration-strategies"></a>Geçiş stratejileri

Translator konuşma tanıma API'si kullanan uygulamaları geliştirme veya üretim, sizin veya kuruluşunuzun varsa, bunları konuşma hizmeti kullanacak şekilde güncelleştirmeniz gerekir. Bkz: [konuşma hizmeti](index.yml) belgeleri kullanılabilir SDK'lar, kod örnekleri ve öğreticiler. Geçirmekte olduğunuz aşağıdakileri dikkate alın:

* Konuşma hizmeti genel bir uç noktası şu anda sunmaz. Uygulamanızı verimli bir şekilde, bölgesel tek bir uç nokta tüm trafik için kullandığında işlevleri, belirleyin. Aksi durumda, coğrafi konum en verimli uç nokta belirlemek için kullanın.

* Uygulamanızı uzun süreli bağlantılar kullanır ve kullanılabilir SDK'lar kullanamazsınız, WebSockets bağlantı kullanabilirsiniz. 10 dakikalık zaman aşımı sınırı, uygun zamanlarda yeniden bağlanmayı tarafından yönetin.

* Translator konuşma tanıma API'si ve Translator Text API bir özel çeviri modellerini etkinleştirmek için uygulamanızın kullanıyorsa, konuşma hizmeti kullanarak doğrudan kategori kimlikleri ekleyebilirsiniz.

* Translator konuşma tanıma API'si, konuşma hizmeti birden fazla dilde tek bir istek halinde çevirileri tamamlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma hizmetini ücretsiz deneyin](get-started.md)
* [Hızlı Başlangıç: bir UWP uygulamasında Speech SDK'sı kullanarak konuşma tanıma](quickstart-csharp-uwp.md)

## <a name="see-also"></a>Ayrıca bkz.

* [Konuşma hizmeti nedir](overview.md)
* [Konuşma hizmeti ve SDK Belgeleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-qsg)
