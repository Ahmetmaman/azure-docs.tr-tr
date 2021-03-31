---
title: Azure CDN için standart kurallar altyapısındaki eylemler | Microsoft Docs
description: Azure Content Delivery Network için standart kurallar altyapısındaki (Azure CDN) eylemler için başvuru belgeleri.
services: cdn
author: asudbring
ms.service: azure-cdn
ms.topic: article
ms.date: 08/04/2020
ms.author: allensu
ms.openlocfilehash: 051737a9f5e0d4092cda26a3f7ce3df1d7f535ef
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "87760133"
---
# <a name="actions-in-the-standard-rules-engine-for-azure-cdn"></a>Azure CDN için standart kurallar altyapısındaki eylemler

Azure Content Delivery Network için [standart kurallar altyapısında](cdn-standard-rules-engine.md) (Azure CDN), bir kural bir veya daha fazla eşleşme koşulu ve bir eylemden oluşur. Bu makalede, Azure CDN için standart kurallar altyapısında kullanabileceğiniz eylemlerin ayrıntılı açıklamaları sağlanmaktadır.

Bir kuralın ikinci bölümü bir eylemdir. Bir eylem, eşleşme koşulunun veya eşleştirme koşulları kümesinin tanımladığı istek türüne uygulanan davranışı tanımlar.

## <a name="actions"></a>Eylemler

Aşağıdaki eylemler, Azure CDN için standart kurallar altyapısında kullanılmak üzere kullanılabilir. 

### <a name="cache-expiration"></a>Önbellek süre sonu

Kuralların eşleştiği talepler için uç noktanın yaşam süresi (TTL) değerinin üzerine yazmak için bu eylemi kullanın.

#### <a name="required-fields"></a>Gerekli alanlar

Önbellek davranışı |  Açıklama              
---------------|----------------
Atlama önbelleği | Bu seçenek belirlendiğinde ve kural eşleştiğinde, içerik önbelleğe alınmaz.
Geçersiz kıl | Bu seçenek belirlendiğinde ve kural eşleştiğinde, kaynaktan döndürülen TTL değeri, eylemde belirtilen değerle üzerine yazılır. Bu davranış yalnızca yanıtın önbelleklenebilir olması durumunda uygulanır. "No-Cache", "Private", "No-Store" değerlerine sahip Cache-Control yanıt üst bilgisi için, eylem geçerli olmayacaktır.
Eksikse ayarla | Bu seçenek belirlendiğinde ve kural eşleştiğinde, kaynaktan bir TTL değeri döndürülmezse, kural TTL 'yi eylemde belirtilen değere ayarlar. Bu davranış yalnızca yanıtın önbelleklenebilir olması durumunda uygulanır. "No-Cache", "Private", "No-Store" değerlerine sahip Cache-Control yanıt üst bilgisi için, eylem geçerli olmayacaktır.

#### <a name="additional-fields"></a>Ek alanlar

Gün | Saat | Dakika | Saniye
-----|-------|---------|--------
int | int | int | int 

### <a name="cache-key-query-string"></a>Önbellek anahtarı sorgu dizesi

Sorgu dizelerine göre önbellek anahtarını değiştirmek için bu eylemi kullanın.

#### <a name="required-fields"></a>Gerekli alanlar

Davranış | Açıklama
---------|------------
Şunları Dahil Et: | Bu seçenek belirlendiğinde ve kural eşleştiğinde, parametrelerde belirtilen sorgu dizeleri önbellek anahtarı oluşturulduğunda dahil edilir. 
Her benzersiz URL'yi önbelleğe al | Bu seçenek belirlendiğinde ve kural eşleştiğinde, her benzersiz URL 'nin kendi önbellek anahtarı vardır. 
Exclude | Bu seçenek belirlendiğinde ve kural eşleştiğinde, parametrelerde belirtilen sorgu dizeleri önbellek anahtarı oluşturulduğunda dışlanır.
Sorgu dizelerini yoksay | Bu seçenek belirlendiğinde ve kural eşleştiğinde, önbellek anahtarı oluşturulduğunda sorgu dizeleri değerlendirilmez. 

### <a name="modify-request-header"></a>İstek üst bilgisini Değiştir

Bu eylemi, kaynağına gönderilen isteklerde bulunan üst bilgileri değiştirmek için kullanın.

#### <a name="required-fields"></a>Gerekli alanlar

Eylem | HTTP üst bilgi adı | Değer
-------|------------------|------
Ekle | Bu seçenek belirlendiğinde ve kural eşleştiğinde, **üstbilgi adı** bölümünde belirtilen üstbilgi belirtilen değere sahip isteğe eklenir. Üst bilgi zaten mevcutsa, değer mevcut değere eklenir. | Dize
Üzerine yaz | Bu seçenek belirlendiğinde ve kural eşleştiğinde, **üstbilgi adı** bölümünde belirtilen üstbilgi belirtilen değere sahip isteğe eklenir. Üst bilgi zaten mevcutsa, belirtilen değer varolan değerin üzerine yazar. | Dize
Sil | Bu seçenek belirlendiğinde, kural eşleşir ve kuralda belirtilen üst bilgi bulunur, üst bilgi istekten silinir. | Dize

### <a name="modify-response-header"></a>Yanıt üst bilgisini Değiştir

İstemcilerinize döndürülen yanıtlarda bulunan üstbilgileri değiştirmek için bu eylemi kullanın.

#### <a name="required-fields"></a>Gerekli alanlar

Eylem | HTTP üst bilgi adı | Değer
-------|------------------|------
Ekle | Bu seçenek belirlendiğinde ve kural eşleştiğinde, **üst bilgi adı** 'nda belirtilen üst bilgi yanıta belirtilen **değer** kullanılarak eklenir. Üst bilgi zaten mevcutsa, **değer** var olan değere eklenir. | Dize
Üzerine yaz | Bu seçenek belirlendiğinde ve kural eşleştiğinde, **üst bilgi adı** 'nda belirtilen üst bilgi yanıta belirtilen **değer** kullanılarak eklenir. Üst bilgi zaten mevcutsa, **değer** varolan değerin üzerine yazar. | Dize
Sil | Bu seçenek belirlendiğinde, kural eşleşir ve kuralda belirtilen üst bilgi bulunur, üst bilgi yanıttan silinir. | Dize

### <a name="url-redirect"></a>URL yeniden yönlendirme

İstemcileri yeni bir URL 'ye yönlendirmek için bu eylemi kullanın. 

#### <a name="required-fields"></a>Gerekli alanlar

Alan | Açıklama 
------|------------
Tür | İstek sahibine döndürülecek yanıt türünü seçin: bulunan (302), taşınan (301), geçici yeniden yönlendirme (307) ve kalıcı yeniden yönlendirme (308).
Protokol | Match Isteği, HTTP, HTTPS.
Konak adı | İsteğin yeniden yönlendirilmesini istediğiniz ana bilgisayar adını seçin. Gelen ana bilgisayarı korumak için boş bırakın.
Yol | Yeniden Yönlendirmede kullanılacak yolu tanımlayın. Gelen yolu korumak için boş bırakın.  
Sorgu dizesi | Yeniden Yönlendirmede kullanılan sorgu dizesini tanımlayın. Gelen sorgu dizesini korumak için boş bırakın. 
Parça | Yeniden Yönlendirmede kullanılacak parçayı tanımlayın. Gelen parçayı korumak için boş bırakın. 

Mutlak bir URL kullanmanızı kesinlikle öneririz. Göreli bir URL kullanmak Azure CDN URL 'Leri geçersiz bir yola yönlendirebilir. 

### <a name="url-rewrite"></a>URL yeniden yazma

Bu eylemi, kaynağına yönlendiren bir isteğin yolunu yeniden yazmak için kullanın.

#### <a name="required-fields"></a>Gerekli alanlar

Alan | Açıklama 
------|------------
Kaynak stili | Değiştirilecek URL yolundaki kaynak modelini tanımlayın. Şu anda, kaynak stili önek tabanlı eşleşme kullanıyor. Tüm URL yollarını eşleştirmek için, **/** kaynak model değeri olarak bir eğik çizgi () kullanın.
Hedef | Yeniden yazma sırasında kullanılacak hedef yolu tanımlayın. Hedef yol, kaynak deseninin üzerine yazar.
Eşleşmeyen yolu koru | **Evet** olarak ayarlanırsa, kaynak örüntüden sonraki kalan yol yeni hedef yoluna eklenir. 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure CDN genel bakış](cdn-overview.md)
- [Standart kural altyapısı başvurusu](cdn-standard-rules-engine-reference.md)
- [Standart kurallar altyapısındaki koşulları Eşleştir](cdn-standard-rules-engine-match-conditions.md)
- [Standart kural altyapısını kullanarak HTTPS'yi zorlama](cdn-standard-rules-engine.md)
