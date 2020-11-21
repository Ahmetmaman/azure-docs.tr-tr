---
title: Konuşma cihazları SDK 'Sı dizi önerileri
titleSuffix: Azure Cognitive Services
description: Konuşma cihazları SDK 'Sı dizi önerileri. Bu dizi geometrileri, Microsoft ses yığını ile kullanım için önerilir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/16/2019
ms.author: erhopf
ms.openlocfilehash: 698a1d52af6c2472d6c025851ead1a0b6a6dff82
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95015350"
---
# <a name="speech-devices-sdk-microphone-array-recommendations"></a>Konuşma cihazları SDK 'Sı dizi önerileri

Bu makalede, konuşma cihazları SDK 'Sı için bir mikrofon dizisi tasarlamayı öğreneceksiniz.

Konuşma cihazları SDK 'Sı, mikrofon geometrisi ve bileşen seçimi dahil olmak üzere aşağıdaki yönergelere göre tasarlanan bir mikrofon dizisi ile en iyi şekilde çalışmaktadır. Ayrıca, tümleştirme ve elektrik konuları hakkında rehberlik sağlanır.

## <a name="microphone-geometry"></a>Mikrofon geometrisi

Aşağıdaki dizi geometrileri, Microsoft ses yığını ile kullanım için önerilir. Belirli uygulamalara, Kullanıcı senaryolarına ve cihaz formu faktörüne sahip daha fazla sayıda mikrofondan daha fazla sayıda mikrofondan daha fazla sayıda mikrofondan daha fazla sayıda mikrofondan daha fazla sayıda mikrofondan

| Mik& geometrisi | Dairesel dizi | Dairesel dizi | Doğrusal dizi | Doğrusal dizi |
| --- | -------------- | --- | ------------ | --- |
|     | <img src="media/speech-devices-sdk/7-mic-c.png" alt="7 mic circular array" width="150"/> | <img src="media/speech-devices-sdk/4-mic-c.png" alt="4 mic circular array" width="150"/> | <img src="media/speech-devices-sdk/4-mic-l.png" alt="4 mic linear array" width="150"/> | <img src="media/speech-devices-sdk/2-mic-l.png" alt="2 mic linear array" width="150"/> |
| \# Akademik | 7 | 4 | 4 | 2 |
| Geometri | 6 dış, 1 orta, yarıçap = 42,5 mm, eşit aralıklı | 3 dış, 1 orta, yarıçap = 42,5 mm, eşit aralıklı | Uzunluk = 120 mm, Aralık = 40 mm | Aralık = 40 mm |

Mikrofon kanalları, yukarıdaki her dizi için gösterilen numaralandırmaya göre sıralanmalıdır, 0 ' dan artış yapılır. Microsoft ses yığını, yankı iptali gerçekleştirmek için ek bir ses kayıttan yürütme akışı gerektirir.

## <a name="component-selection"></a>Bileşen seçimi

Mikrofon bileşenleri, gürültü ve deformasyon dışında bir sinyali sorunsuz bir şekilde yeniden üretmek için seçilmelidir.

Mikrofonlar seçerken önerilen özellikler şunlardır:

| Parametre | Önerilen |
| --------- | ----------- |
| SNR | \>= 65 dB (1 kHz sinyal 94 dBSPL, A-ağırlıklı gürültü) |
| Genlik eşleştirme | ± 1 dB @ 1 kHz |
| Aşama eşleştirme | ± 2 ° @ 1 kHz |
| Akustik aşırı yükleme noktası (AOP) | \>= 120 dBSPL (THD = %10) |
| Bit hızı | En az 24 bit |
| Örnekleme Oranı | En düşük 16 kHz\* |
| Sıklık yanıtı | ± 3 dB, 200-8000 Hz kayan maske\* |
| Güvenilirlik | Depolama sıcaklığı aralığı-40 °C ila 70 °C<br />İşletim sıcaklığı aralığı-20 °C ila 55 °C |

\*_Yüksek kaliteli iletişimler (VoIP) uygulamaları için daha yüksek örnekleme oranları veya "daha geniş" Sıklık aralıkları gerekebilir_

Kullanılan bileşenlerin performansını impairing önlemek için iyi bir bileşen seçimi, iyi bir elektroakustik tümleştirmeyle eşleştirilmelidir. Benzersiz kullanım örnekleri ek gereksinimleri de gerektirebilir (örneğin, işletim sıcaklığı aralıkları).

## <a name="microphone-array-integration"></a>Mikrofon dizisi tümleştirmesi

Bir cihazla tümleşik hale geldiğinde Microphone dizisinin performansı, bileşen belirtiminden farklı olacaktır. Mikrofonların tümleştirmeden iyi eşleştiğinden emin olmak önemlidir. Bu nedenle, herhangi bir sabit kazanç veya EQ öğesinden sonra ölçülen cihaz performansı aşağıdaki önerileri karşılamalıdır:

| Parametre          | Önerilen                                        |
| ------------------ | -------------------------------------------------- |
| SNR                | \> 63 dB (1 kHz sinyal 94 dBSPL, A-ağırlıklı gürültü) |
| Çıkış duyarlılığı | -26 dBFS/PA @ 1 kHz (önerilir)                  |
| Genlik eşleştirme | ± 2 dB, 200-8000 Hz                                |
| % D\*             | ≤ 1%, 200-8000 Hz, 94 dBSPL, 5 sipariş             |
| Sıklık yanıtı | ± 6 dB, 200-8000 Hz kayan maske\*\*              |

\*\*_(Ör. Neumann KH120) ölçmek için düşük bir deformasyon hoparlörü gerekir_

\*\*_Yüksek kaliteli iletişimler (VoIP) uygulamaları için "daha geniş" Sıklık aralıkları gerekebilir_

## <a name="speaker-integration-recommendations"></a>Konuşmacı tümleştirme önerileri

Hoparlör içeren konuşma tanıma cihazları için yankı iptali gerekli olduğundan, konuşmacı seçimi ve tümleştirmesi için ek öneriler sağlanır.

| Parametre | Önerilen |
| --------- | ----------- |
| Doğrus konuları | Konuşmacı başvurusundan sonra doğrusal olmayan işlem yapılmaz, aksi takdirde donanım tabanlı geri döngü başvuru akışı gerekir |
| Konuşmacı geri döngüsü | WASAPı, özel API 'Ler, özel ALSA eklentisi (Linux) veya bellenim kanalı aracılığıyla temin edilmiştir |
| % D | 3. Octave bantları minimum 5 sipariş, 70 dBA kayıttan yürütme @ 0,8 5 ≤ 6,3%, 315-500 Hz ≤ 5%, 630-5000 Hz |
| Yankı bağlantısı-mikrofonlar | \> -10 dB TCLw, ITU-T G. 122.368 ek B. 4 metodunu kullanarak, MIC düzeyinde normalleştirilmiş<br />TCLw = Tclwölçülen \+ (ölçülen düzey-hedef çıkış duyarlılığı)<br />TCLw = Tclwölçülen \+ (ölçülen düzey-(-26)) |

## <a name="integration-design-architecture"></a>Tümleştirme tasarım mimarisi

Mikrofonlar bir cihazla tümleştirilirken mimari için aşağıdaki yönergeler gereklidir:

| Parametre | Öneri |
| --------- | -------------- |
| MIC bağlantı noktası benzerliği | Tüm mikrofon bağlantı noktaları dizideki aynı uzunluktadır |
| MIC bağlantı noktası boyutları | Bağlantı noktası boyutu Ø 0.8-1,0 mm. Bağlantı noktası uzunluğu/bağlantı noktası çapı \< 2 |
| Mik mühürleme         | Stack gaskets bir arada uygulandı. \>Fohar gaskets için %70 sıkıştırma oranı öner |
| MIC güvenilirliği     | Ara, toz ve giriş yapılmasını engellemek için kullanılmalıdır (altı bağlantı noktalı mikrofonlar ve mühürleme/en üst kapak için PCB arasında) |
| MIC yalıtımı       | Lastik gaskets ve titreşim, özellikle Tümleşik hoparlörler nedeniyle titreşim yollarını yalıtmak için yapı aracılığıyla ayrılır |
| Örnekleme saati      | Cihaz sesinin azalmaları ve düşük bir düşüş olması gerekir |
| Kayıt özelliği   | Cihazın tek tek kanal ham akışlarını aynı anda kaydedebilmesi gerekir |
| USB                 | Tüm USB ses girişi cihazlarının, [Rev3 spec](https://www.usb.org/document-library/usb-audio-devices-rev-30-and-adopters-agreement) 'e göre tanımlayıcıları ayarlaması gerekir |
| Mikrofon geometrisi | Sürücüler, [mikrofon dizisi geometrisi tanımlayıcılarını](/windows-hardware/drivers/audio/ksproperty-audio-mic-array-geometry) doğru uygulamalıdır |
| Edile     | Cihazlarda bulunmayan veya denetlenebilir olmayan donanım, bellenim veya üçüncü taraf yazılım tabanlı, cihaza/cihazdan, üçüncü taraf yazılım tabanlı olmayan ses işleme algoritmaları olmamalıdır |
| Yakalama biçimi      | Yakalama biçimleri, 16 kHz minimum örnekleme hızını ve önerilen 24 bit derinliği kullanmalıdır |

## <a name="electrical-architecture-considerations"></a>Elektrik mimarisi konuları

Uygun olduğunda, diziler bir USB ana bilgisayarına (Microsoft ses yığınını çalıştıran bir SoC gibi) ve konuşma Hizmetleri ya da diğer uygulamalara arabirimler bağlı olabilir.

PDM-TDM dönüşümü gibi donanım bileşenleri, mikrofonların dinamik aralığının ve SNR 'nin yeniden örnekleyiciler içinde korunduğundan emin olmalıdır.

Yüksek hızlı USB ses sınıfı 2,0, daha yüksek örnek ücretler ve bit derinlikleri üzerinde yedi kanala kadar gerekli bant genişliğini sağlamak için herhangi bir ses MCUs içinde desteklenmelidir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma cihazları SDK 'Sı hakkında daha fazla bilgi edinin](speech-devices-sdk.md)