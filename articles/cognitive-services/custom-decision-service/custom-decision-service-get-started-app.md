---
title: Custom Decision Service'i bir uygulamadan - API çağırma
titlesuffix: Azure Cognitive Services
description: Bir akıllı telefon uygulaması özel karar alma hizmeti API'leri çağırmak nasıl.
services: cognitive-services
author: slivkins
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-decision-service
ms.topic: conceptual
ms.date: 05/10/2018
ms.author: slivkins
ms.openlocfilehash: e7df982c178bff19dcad8df1ba42a5a97904cd4c
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46365037"
---
# <a name="call-api-from-an-app"></a>Uygulamadan API çağrısı yapma

Azure özel karar alma hizmeti API'lere giden çağrıların smartphone uygulamasından olun. Bu makalede, bazı temel seçeneklerle başlamak açıklanmaktadır.

Mutlaka [uygulamanızı kaydetmeniz](custom-decision-service-get-started-register.md), ilk.

Custom Decision Service için akıllı uygulamanızdan olun iki API çağrıları vardır: içeriğinizi ve bir ödül bildirmek için ödül API'sine yapılan bir çağrı kişilerinin sıralı bir listesini almak için sıralaması API'sine yapılan bir çağrı. Örnek çağrılarında işte [cURL](https://en.wikipedia.org/wiki/CURL).

Daha fazla bilgi için bkz [API](custom-decision-service-api-reference.md).

Derecelendirme API çağrısı ile başlayın. Dosya oluşturma `<request.json>`, hangi eylem kimliği olarak ayarlanmış Portalda girdiğiniz Atom akışı ya da bu kimliği karşılık gelen RSS adıdır:

```json
{"decisions":
     [{ "actionSets":[{"id":{"id":"<actionSetId>"}}] }]}
```

Birçok eylemi kümeleri gibi belirtilebilir:

```json
{"decisions":
    [{ "actionSets":[{"id":{"id":"<actionSetId1>"}},
                     {"id":{"id":"<actionSetId2>"}}] }]}
```

Bu JSON dosyası sıralaması isteğin bir parçası gönderilir:

```shell
curl -d @<request.json> -X POST https://ds.microsoft.com/api/v2/<appId>/rank --header "Content-Type: application/json"
```

Burada, `<appId>` uygulamanızın adını portalı üzerinde kayıtlı. Uygulamanızda oluşturulabilen içerik öğeleri kümesini almanız gerekir. Bir örnek dönüş şuna benzer:

```json
[{ "ranking":[{"id":"actionId3"}, {"id":"actionId1"}, {"id":"actionId2"}],
   "eventId":"<opaque event string>",
   "appId":"<your app id>",
   "rewardAction":"actionId3",
   "actionSets":[{"id":"<actionSetId1>","lastRefresh":"2017-04-29T22:34:25.3401438Z"},
                 {"id":"<actionSetId2>","lastRefresh":"2017-04-30T22:34:25.3401438Z"}]}]
```

Dönüş ilk bölümünü sıralanan Eylemler, işlem kimliği tarafından belirtilen bir listesine sahiptir. Bir makale için bir URL işlem kimliğidir. Genel istek de benzersiz bir sahip `<eventId>`, sistem tarafından oluşturulmuş.

Daha sonra bu olaydan olan ilk içerik öğesi tıklatıldığında, gözlemlenen belirtebilirsiniz `<actionId3>`. Ardından bir ödül bu bildirebileceğiniz `<eventId>` ödül API aracılığıyla Custom Decision Service için başka bir istek gibi:

```shell
curl -v https://ds.microsoft.com/api/v2/<appId>/reward/<eventId> -X POST
```

Son olarak eylem Custom Decision Service tarafından değerlendirilmesi için (Eylemler) makale listesi döndüren API kümesini, vermeniz gerekir. Bir RSS akışı olarak bu API, burada gösterildiği şekilde uygulayın:

```xml
<rss version="2.0">
<channel>
   <item>
      <title><![CDATA[title (possibly with url) ]]></title>
      <link>url</link>
      <pubDate>Thu, 27 Apr 2017 16:30:52 GMT</pubDate>
    </item>
   <item>
       ....
   </item>
</channel>
</rss>
```

Burada, her üst düzey `<item>` öğesi bir makale açıklar. `<link>` Zorunludur ve bir eylem kimliği Custom Decision Service tarafından kullanılır. Belirtin `<date>` (standart bir biçimde RSS) 15'ten fazla makaleleri belirttiyseniz. En son 15 makaleleri kullanılır. `<title>` İsteğe bağlıdır ve makale için metin güvenlikle ilgili özellikler oluşturmak için kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

* Çalışmak bir [öğretici](custom-decision-service-tutorial-news.md) daha ayrıntılı bir örnek.
* Başvuru başvurun [API](custom-decision-service-api-reference.md) sağlanan işlevselliği hakkında daha fazla bilgi için.