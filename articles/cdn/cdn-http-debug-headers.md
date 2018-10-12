---
title: Azure CDN kurallar altyapısı için X-EC-Debug HTTP üstbilgileri | Microsoft Docs
description: X-EC-Debug hata ayıklama önbellek isteği üst bilgisi istenen varlık için uygulanan önbellek İlkesi hakkında daha fazla bilgi sağlar. Verizon için bu üstbilgileri özgüdür.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2018
ms.author: magattus
ms.openlocfilehash: 4ba42850ee28e2e212d9bc2b7b64be103218757c
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49094233"
---
# <a name="x-ec-debug-http-headers-for-azure-cdn-rules-engine"></a>Azure CDN kurallar altyapısı için X-EC-Debug HTTP üstbilgileri
Hata ayıklama önbellek istek üstbilgisi `X-EC-Debug`, istenen varlığa uygulanır önbellek İlkesi hakkında ek bilgi sağlar. Bu üst özgü **verizon'dan Azure CDN Premium** ürünleri.

## <a name="usage"></a>Kullanım
POP sunuculardan bir kullanıcıya gönderilen yanıt içeriyor `X-EC-Debug` yalnızca aşağıdaki koşullar karşılandığında bir üst bilgi:

- [Önbellek yanıt üst bilgileri hata ayıklama özelliği](cdn-rules-engine-reference-features.md#debug-cache-response-headers) kuralları altyapısını belirtilen istek için etkin.
- Belirtilen isteği, yanıta dahil edilecek hata ayıklama önbellek yanıt üstbilgilerini kümesini tanımlar.

## <a name="requesting-debug-cache-information"></a>Hata ayıklama önbellek bilgi isteniyor
Yanıta dahil edilecek hata ayıklama bilgilerini önbelleğe al tanımlamak için belirtilen istekte aşağıdaki yönergeleri kullanın:

İstek üstbilgisi | Açıklama |
---------------|-------------|
X-EC-Debug: x-ec-önbellek | [Önbellek durum kodu](#cache-status-code-information)
X-EC-Debug: x-ec-cache-remote | [Önbellek durum kodu](#cache-status-code-information)
X-EC-Debug: x-ec-onay-önbelleğe alınabilir | [Önbelleğe alınabilir](#cacheable-response-header)
X-EC-Debug: x-ec-cache-key | [Önbellek anahtarı](#cache-key-response-header)
X-EC-Debug: x-ec-cache-durumu | [Önbellek durumu](#cache-state-response-header)

### <a name="syntax"></a>Sözdizimi

Hata ayıklama önbellek yanıt üst bilgileri, aşağıdaki üst bilgi ve belirtilen yönergeleri istekte dahil ederek istenebilir:

`X-EC-Debug: Directive1,Directive2,DirectiveN`

### <a name="sample-x-ec-debug-header"></a>Örnek X-EC-Debug üst bilgisi

`X-EC-Debug: x-ec-cache,x-ec-check-cacheable,x-ec-cache-key,x-ec-cache-state`

## <a name="cache-status-code-information"></a>Durum kodu bilgilerini önbelleğe al
X-EC-Debug yanıt üst bilgisi, bir sunucu ve aşağıdaki yönergeleri aracılığıyla yanıta işlenme belirleyebilirsiniz:

Üst bilgi | Açıklama
-------|------------
X-EC-Debug: x-ec-önbellek | Bu üst bilgi içeriği CDN üzerinden yönlendirilir her bildirilir. Bu, isteği yerine getiren POP sunucu tanımlar.
X-EC-Debug: x-ec-cache-remote | Bir kaynak kalkan sunucu veya bir ADN ağ geçidi sunucusu üzerinde istenen içeriğin yalnızca önbelleğe ilişkili olduğunda bu başlığı bildirilir.

### <a name="response-header-format"></a>Yanıt üst bilgisi biçimi

X-EC-Debug üst bilgi önbelleği durum kodu bilgilerini aşağıdaki biçimde raporları:

- `X-EC-Debug: x-ec-cache: <StatusCode from Platform (POP/ID)>`

- `X-EC-Debug: x-ec-cache-remote: <StatusCode from Platform (POP/ID)>`

Yukarıdaki yanıt üst bilgisi sözdiziminde kullanılan terimler şu şekilde tanımlanır:
- StatusCode: önbellek durum kodu ile temsil edilen CDN tarafından talep edilen içeriği nasıl işlendiğini gösterir.
    
    Belirteç tabanlı kimlik doğrulama nedeniyle yetkisiz bir istek reddedildiğinde TCP_DENIED durum kodu hiçbiri yerine bildirilebilir. Ancak, NONE durum kodu, önbellek durumu raporları ya da günlük ham verileri görüntülerken kullanılacak devam eder.

- Platform: içeriği istendi platform gösterir. Aşağıdaki kodu, bu alan için geçerlidir:

    Kod  | Platform
    ------| --------
    ECAcc | HTTP büyük
    ECS   | HTTP küçük
    ECD   | Uygulama teslim ağı (ADN)

- POP: Gösterir [POP](cdn-pop-abbreviations.md) , işlenen istek. 

### <a name="sample-response-headers"></a>Örnek yanıt üstbilgileri

Aşağıdaki örnek üstbilgileri, bir istek için önbellek durumu kodu bilgileri sağlayın:

- `X-EC-Debug: x-ec-cache: TCP_HIT from ECD (lga/0FE8)`

- `X-EC-Debug: x-ec-cache-remote: TCP_HIT from ECD (dca/EF00)`

## <a name="cacheable-response-header"></a>Önbelleğe alınabilir yanıt üst bilgisi
`X-EC-Debug: x-ec-check-cacheable` Yanıt üst bilgisi, istenen içeriği önbelleğe alınmamış olup olmadığını gösterir.

Bu yanıt üst bilgisi önbelleğe alma gerçekleşen olup olmadığını göstermez. Bunun yerine, istek önbelleğe almak için uygun olup olmadığını gösterir.

### <a name="response-header-format"></a>Yanıt üst bilgisi biçimi

`X-EC-Debug` Şu biçimde bir istek önbelleğe alınmamış olup raporlama yanıt üst bilgisi olan:

`X-EC-Debug: x-ec-check-cacheable: <cacheable status>`

Yukarıdaki yanıt üst bilgisi sözdiziminde kullanılacak terimi şu şekilde tanımlanır:

Değer  | Açıklama
-------| --------
EVET    | Talep edilen içeriği önbelleğe almak için uygun olduğunu gösterir.
HAYIR     | Talep edilen içeriği önbelleğe almak için uygun olduğunu gösterir. Bu durum aşağıdaki nedenlerden biri nedeniyle olabilir: <br /> -Müşteriye özgü yapılandırma: hesabınıza belirli bir yapılandırma için bir varlık olarak önbelleğe alınan pop sunucuları engelleyebilirsiniz. Örneğin, kural altyapısı, istekleri uygun için önbellek atlama özelliği etkinleştirerek önbelleğe alınmasını bir varlık engelleyebilirsiniz.<br /> -Cache yanıt üstbilgileri: Önbelleğe alınan POP sunucuları istenen varlığın Cache-Control veya Expires başlıklarına engelleyebilirsiniz.
BİLİNMİYOR | Sunucuları istenen varlık önbelleğe gerektirmediğine açamıyoruz gösterir. Bu durum, genellikle isteği nedeniyle belirteç tabanlı kimlik doğrulaması reddedildi oluşur.

### <a name="sample-response-header"></a>Örnek yanıt üst bilgisi

Aşağıdaki örnek yanıt üst bilgisi, istenen içeriği önbelleğe alınmamış olup olmadığını gösterir:

`X-EC-Debug: x-ec-check-cacheable: YES`

## <a name="cache-key-response-header"></a>Önbellek anahtarı yanıt üst bilgisi
`X-EC-Debug: x-ec-cache-key` Yanıt üst bilgisi, istenen içerikle ilişkili fiziksel önbellek anahtarını belirtir. Fiziksel bir önbellek anahtarını önbelleğe alma amacıyla bir varlığı tanımlayan bir yol oluşur. Diğer bir deyişle, sunucuları bir varlık yolu göre önbelleğe alınmış bir sürümü için cache-anahtara göre tanımlanan şekilde kontrol eder.

Bu fiziksel önbellek anahtarı çift İleri eğik çizgi ile başlar (/ /) (HTTP veya HTTPS) içerik istemek için kullanılan protokolü tarafından izlenen. Bu protokol içerik erişim noktasıyla başlatır istenen varlığa göreli yolu tarafından izlenir (örneğin, _/000001/_).

Varsayılan olarak HTTP platformları kullanmak üzere yapılandırılmış *standart önbellek*, yani sorgu dizelerini önbelleğe alma mekanizması tarafından göz ardı edilir. Bu tür bir yapılandırma, sorgu dizesi verileri dahil olmak üzere, önbellek anahtarını engeller.

Bir sorgu dizesi önbellek anahtarını kayıtlıysa, onu karma eşdeğerine dönüştürülür ve ardından istenen varlık adı ve dosya uzantısını arasında eklenir (örneğin, varlık&lt;karma değeri&gt;.html).

### <a name="response-header-format"></a>Yanıt üst bilgisi biçimi

`X-EC-Debug` Yanıt üst bilgisi şu biçimde fiziksel önbellek anahtarı bilgileri bildirir:

`X-EC-Debug: x-ec-cache-key: CacheKey`

### <a name="sample-response-header"></a>Örnek yanıt üst bilgisi

Aşağıdaki örnek yanıt üst bilgisi fiziksel önbellek anahtarı istenen içerik için gösterir:

`X-EC-Debug: x-ec-cache-key: //http/800001/origin/images/foo.jpg`

## <a name="cache-state-response-header"></a>Önbellek durumu yanıt üst bilgisi
`X-EC-Debug: x-ec-cache-state` Yanıt üst bilgisi cache durumu istenen içeriğin işlemi istendi zaman gösterir.

### <a name="response-header-format"></a>Yanıt üst bilgisi biçimi

`X-EC-Debug` Yanıt üst bilgisi önbellek durumu bilgilerini aşağıdaki biçimde bildirir:

`X-EC-Debug: x-ec-cache-state: max-age=MASeconds (MATimePeriod); cache-ts=UnixTime (ddd, dd MMM yyyy HH:mm:ss GMT); cache-age=CASeconds (CATimePeriod); remaining-ttl=RTSeconds (RTTimePeriod); expires-delta=ExpiresSeconds`

Yukarıdaki yanıt üst bilgisi sözdiziminde kullanılan terimler şu şekilde tanımlanır:

- MASeconds: İstenen içeriğin Cache-Control üst bilgileri tarafından tanımlandığı şekilde, max-age (saniye cinsinden) belirtir.

- MATimePeriod:, max-age değeri (diğer bir deyişle, MASeconds) daha büyük bir birimin yaklaşık denk dönüştürür (örneğin, gün cinsinden). 

- UnixTime: (diğer adıyla) UNIX sürede istenen içerik önbelleği zaman damgasını gösterir POSIX zaman ya da UNIX dönem). Bir varlığın TTL hesaplanır başlangıç tarih/saat önbellek zaman damgasını gösterir. 

    Kaynak sunucu, sunucu veya sunucu Age yanıtı üstbilgisi döndürmezse, önbelleğe alma bir üçüncü taraf HTTP kullanmaz, önbellek zaman damgası her zaman zaman varlık alınan yeniden doğrulanır veya tarih/saat olacaktır. Aksi takdirde, POP sunucuları yaş alanı varlığın TTL şu şekilde hesaplamak için kullanır: alma/RevalidateDateTime - yaş.

- ddd, dd MMM yyyy ss: dd: GMT: İstenen içerik önbelleği zaman damgasını gösterir. Daha fazla bilgi için lütfen yukarıdaki UnixTime terimi bakın.

- CASeconds: önbellek zaman damgası beri geçen saniye sayısını belirtir.

- RTSeconds: kendisi için önbelleğe alınmış içeriği yeni olarak kabul edilecek kalan saniye sayısını belirtir. Bu değer şu şekilde hesaplanır: RTSeconds max-age - = önbelleğe yaş.

- RTTimePeriod: kalan TTL değeri (diğer bir deyişle, RTSeconds) daha büyük bir birimin yaklaşık denk dönüştürür (örneğin, gün cinsinden).

- ExpiresSeconds: Belirtilen tarih/saat önce kalan saniye sayısını gösteren `Expires` yanıtı üstbilgisi. Varsa `Expires` yanıt üst bilgisi yanıta dahil ve bu terim değeri *hiçbiri*.

### <a name="sample-response-header"></a>Örnek yanıt üst bilgisi

Aşağıdaki örnek yanıt üst bilgisi, istenen sırada talep edilen içeriği önbellek durumunu gösterir:

```X-EC-Debug: x-ec-cache-state: max-age=604800 (7d); cache-ts=1341802519 (Mon, 09 Jul 2012 02:55:19 GMT); cache-age=0 (0s); remaining-ttl=604800 (7d); expires-delta=none```

