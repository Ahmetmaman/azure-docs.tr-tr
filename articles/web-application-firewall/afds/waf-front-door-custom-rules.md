---
title: Azure ön kapısının Web uygulaması güvenlik duvarı özel kuralı
description: Web uygulaması güvenlik duvarı (WAF) özel kurallarını kullanarak Web uygulamalarınızı kötü amaçlı saldırılara karşı koruma hakkında bilgi edinin.
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 09/05/2019
ms.author: victorh
ms.openlocfilehash: 158bfe30bf48ee420be8efb9ff32fff0e555d9e7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "79475833"
---
#  <a name="custom-rules-for-web-application-firewall-with-azure-front-door"></a>Azure ön kapısına sahip Web uygulaması güvenlik duvarı için özel kurallar

Ön kapılı Azure Web uygulaması güvenlik duvarı (WAF), tanımladığınız koşullara göre Web uygulamalarınıza erişimi denetlemenize olanak tanır. Özel bir WAF kuralı bir öncelik numarası, kural türü, eşleşme koşulları ve bir eylemden oluşur. İki tür özel kural vardır: eşleştirme kuralları ve hız sınırı kuralları. Bir eşleştirme kuralı, eşleşen koşullara göre erişimi denetler, ancak bir hız sınırı kuralı, eşleşen koşullara ve gelen isteklerin ücretlerine göre erişimi denetler. Hesaplanmasını önlemek için özel bir kuralı devre dışı bırakabilirsiniz, ancak yine de yapılandırmayı devam edebilirsiniz. 

## <a name="priority-match-conditions-and-action-types"></a>Öncelik, eşleşme koşulları ve eylem türleri

Bir öncelik numarası, bir kural türü, eşleşme koşullarından oluşan bir dizi ve bir eylem tanımlayan özel bir WAf kuralıyla erişimi kontrol edebilirsiniz. 

- **Priority:** WAF kurallarının değerlendirilme sırasını açıklayan benzersiz bir tamsayıdır. Düşük öncelik değerleri olan kurallar, daha yüksek değerlere sahip kurallardan önce değerlendirilir. Öncelik numaraları tüm özel kurallar arasında benzersiz olmalıdır.

- **Eylem:** bir WAF kuralı eşleştirildiği takdirde bir isteğin nasıl yönlendirileceğini tanımlar. Bir istek özel bir kuralla eşleştiğinde uygulanacak aşağıdaki eylemlerden birini seçebilirsiniz.

    - *Allow* -WAF, isteği arka uca iletir, WAF günlüklerinde bir girişi günlüğe kaydeder ve çıkar.
    - *Blok* -istek engellendi, WAF, isteği arka uca iletmeksizin istemciye yanıt gönderiyor. WAF, WAF günlüklerinde bir girişi günlüğe kaydeder.
    - *Log* -WAF WAF günlüklerinde bir girişi günlüğe kaydeder ve sonraki kuralı değerlendirmeye devam eder.
    - *Yeniden yönlendirme* -WAF isteği BELIRTILEN bir URI 'ye yönlendirir, WAF günlüklerinde bir girişi günlüğe kaydeder ve çıkar.

- **Koşulu eşleştir:** bir Match değişkeni, işleç ve Match değeri tanımlar. Her kural birden fazla eşleşme koşulu içerebilir. Bir eşleşme koşulu, coğrafi konuma, istemci IP adreslerine (CıDR), boyuta veya dize eşleşmesi temel alabilir. Dize eşleşmesi, eşleşme değişkenlerinin bir listesine göre olabilir.
  - **Match değişkeni:**
    - RequestMethod
    - QueryString
    - PostArgs
    - RequestUri
    - RequestHeader
    - Istek gövdesi
    - Tanımlama bilgileri
  - **İşlecinde**
    - Any: herhangi bir kural eşleştirilyoksa varsayılan eylemi tanımlamak için genellikle kullanılır. Any, Match All işleçtir.
    - Eşittir
    - Contains
    - LessThan: boyut kısıtlaması
    - GreaterThan: boyut kısıtlaması
    - Yetersiz okarşılandığından al: boyut kısıtlaması
    - GreaterThanOrEqual: boyut kısıtlaması
    - BeginsWith
    - EndsWith
    - Regex
  
  - **Regex** aşağıdaki işlemleri desteklemez: 
    - Geri başvurular ve yakalama alt ifadeleri
    - Rastgele sıfır Genişlik onaylamaları
    - Alt yordam başvuruları ve özyinelemeli desenler
    - Koşullu desenler
    - Geri izleme denetim fiilleri
    - \C Single-Byte yönergesi
    - \R yeni satır eşleştirme yönergesi
    - Eşleşme sıfırlama yönergesinin \K başlangıcı
    - Belirtme çizgileri ve gömülü kod
    - Atomik gruplandırma ve sahip nicelik belirteçleri

  - **Negate [isteğe bağlı]:** Bir koşulun sonucu ölçü olması gerekiyorsa, *Negate* koşulunu true olarak ayarlayabilirsiniz.
      
  - **Dönüştürme [isteğe bağlı]:** Eşleşmesinden önce yapılacak dönüşümlerdeki adlara sahip dizelerin listesi. Bunlar aşağıdaki dönüşümler olabilir:
     - Büyük harfe 
     - Küçük harf
     - Trim
     - RemoveNulls
     - URL kod çözme
     - UrlEncode
     
   - **Eşleşme değeri:** Desteklenen HTTP istek yöntemi değerleri şunlardır:
     - GET
     - POST
     - PUT
     - HEAD
     - DELETE
     - INE
     - KALDıRıN
     - PROFİL
     - Seçenekler
     - PROPFIND
     - Pro
     - MKCOL
     - KOPYA
     - GEÇIŞ

## <a name="examples"></a>Örnekler

### <a name="waf-custom-rules-example-based-on-http-parameters"></a>Http parametrelerine göre WAF özel kurallar örneği

İki eşleşme koşulu olan özel bir kuralın yapılandırmasını gösteren bir örnek aşağıda verilmiştir. İstekler, başvuran tarafından tanımlanan belirli bir siteden ve sorgu dizesinde "Password" yok.

```
# http rules example
{
  "name": "AllowFromTrustedSites",
  "priority": 1,
  "ruleType": "MatchRule",
  "matchConditions": [
    {
      "matchVariable": "RequestHeader",
      "selector": "Referer",
      "operator": "Equal",
      "negateCondition": false,
      "matchValue": [
        "www.mytrustedsites.com/referpage.html"
      ]
    },
    {
      "matchVariable": "QueryString",
      "operator": "Contains",
      "matchValue": [
        "password"
      ],
      "negateCondition": true
    }
  ],
  "action": "Allow",
  "transforms": []
}

```
"PUT" metodunu engellemeye yönelik örnek bir yapılandırma aşağıda gösterildiği gibi gösterilmiştir:

``` 
# http Request Method custom rules
{
  "name": "BlockPUT",
  "priority": 2,
  "ruleType": "MatchRule",
  "matchConditions": [
    {
      "matchVariable": "RequestMethod",
      "selector": null,
      "operator": "Equal",
      "negateCondition": false,
      "matchValue": [
        "PUT"
      ]
    }
  ],
  "action": "Block",
  "transforms": []
}
```

### <a name="size-constraint"></a>Boyut kısıtlaması

Gelen isteğin bir bölümünde boyut kısıtlamasını belirten özel bir kural oluşturabilirsiniz. Örneğin, aşağıdaki kural 100 karakterden daha uzun bir URL 'Yi engelliyor.

```
# http parameters size constraint
{
  "name": "URLOver100",
  "priority": 5,
  "ruleType": "MatchRule",
  "matchConditions": [
    {
      "matchVariable": "RequestUri",
      "selector": null,
      "operator": "GreaterThanOrEqual",
      "negateCondition": false,
      "matchValue": [
        "100"
      ]
    }
  ],
  "action": "Block",
  "transforms": []
}
```

## <a name="next-steps"></a>Sonraki adımlar
- [Azure PowerShell kullanarak bir Web uygulaması güvenlik duvarı ilkesi yapılandırma](waf-front-door-custom-rules-powershell.md) 
- [Ön kapılı Web uygulaması güvenlik duvarı](afds-overview.md) hakkında bilgi edinin
- [Front Door oluşturmayı](../../frontdoor/quickstart-create-front-door.md) öğrenin.

