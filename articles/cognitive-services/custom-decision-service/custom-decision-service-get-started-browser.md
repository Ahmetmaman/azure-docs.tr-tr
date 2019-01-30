---
title: Bir tarayıcıdan - Custom Decision Service API çağırma
titlesuffix: Azure Cognitive Services
description: Custom Decision Service için doğrudan bir tarayıcıdan API çağrısı yaparak bir Web sayfası en iyi duruma getirme.
services: cognitive-services
author: slivkins
manager: cgronlun
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: slivkins
ms.openlocfilehash: db993693acc7e64a789564b92f4d0eacfa0e69f7
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55225270"
---
# <a name="call-api-from-a-browser"></a>Tarayıcıdan API çağrısı yapma

Bu makalede, Azure özel karar alma hizmeti API'lere giden çağrıların doğrudan bir tarayıcıdan yapmanıza yardımcı olur.

Mutlaka [uygulamanızı kaydetmeniz](custom-decision-service-get-started-register.md), ilk.

Başlayalım. Uygulamanız birkaç makale sayfaları için bağlantılar bir ön sayfa sahip olacak şekilde modellenir. Ön sayfa Custom Decision Service, makale sayfaları sıralama belirtmek için kullanır. Ön sayfanın HTML baş aşağıdaki kodu ekleyin:

```html
// Define the "callback function" to render UI
<script> function callback(data) { … } </script>

// call Ranking API, after callback() is defined
<script src="https://ds.microsoft.com/api/v2/<appId>/rank/<actionSetId>" async></script>
```

`data` İşlenecek URL'leri sıralamasını bağımsız değişken içeriyor. Daha fazla bilgi için bkz [API](custom-decision-service-api-reference.md).

Üst makale kullanıcı click işlemek için aşağıdaki kodu ön sayfada çağırın:

```javascript
// call Reward API to report a click
$.ajax({
    type: "POST",
    url: '//ds.microsoft.com/api/v2/<appId>/reward/' + data.eventId,
    contentType: "application/json" })
```

Burada, `data` bağımsız değişkenidir `callback()` işlevi. Bu uygulama örneği bulunabilir [öğretici](custom-decision-service-tutorial-news.md#use-the-apis).

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

Burada, her üst düzey `<item>` öğesi bir makale açıklar. `<link>` Zorunludur ve bir eylem kimliği Custom Decision Service tarafından kullanılır. Belirtin `<date>` (standart bir biçimde RSS) 15'ten fazla makaleler varsa. En son 15 makaleleri kullanılır. `<title>` İsteğe bağlıdır ve makale için metin güvenlikle ilgili özellikler oluşturmak için kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

* Çalışmak bir [öğretici](custom-decision-service-tutorial-news.md) daha ayrıntılı bir örnek.
* Başvuru başvurun [API](custom-decision-service-api-reference.md) sağlanan işlevselliği hakkında daha fazla bilgi için.
