---
title: Marka algılama - görüntü işleme
titleSuffix: Azure Cognitive Services
description: Görüntü işleme API'sini kullanarak marka/logoyu algılama için ilgili kavramları.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 01/26/2019
ms.author: pafarley
ms.openlocfilehash: a8822eb773842df3ea6fd50cd2a6ba36c3c4cd88
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2019
ms.locfileid: "55569606"
---
# <a name="brand-detection"></a>Marka algılama

Marka algılamadır özel bir modunu [nesne algılama](concept-object-detection.md) resim veya videodaki ticari markaları tanımlamak için genel logoları binlerce veritabanını kullanan. Örneğin, hangi markaları en popüler sosyal medyada veya medya ürün yerleşimiyle en yaygın olduğunu öğrenmek için bu özelliği kullanabilirsiniz.

Görüntü işleme hizmeti, belirli bir görüntüde marka logoları olup olmadığını algılar; Bu durumda, marka adı, bir güven puanı ve logonun çevresinde bir sınırlama kutusu koordinatları döndürür.

Yerleşik logosu veritabanı, tüketici elektroniği, giysi ve diğer popüler markaları kapsar. Aradığınız marka görüntü işleme hizmeti tarafından algılanmayan bulursanız, daha iyi oluşturma ve eğitim kendi logosu algılayıcısı kullanarak hizmet alırlar [Custom Vision](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/) hizmeti.

## <a name="brand-detection-example"></a>Marka algılama örneği

Aşağıdaki JSON yanıtları, görüntü işleme örnek görüntüleri markaları tespit edilirken döndürür göstermektedir.

![Bir gri sweatshirt bir Microsoft etiketi ve onu şirket logosu](./Images/gray-shirt-logo.jpg)

```json
{
   "brands":[
      {
         "name":"Microsoft",
         "confidence":0.706,
         "rectangle":{
            "x":470,
            "y":862,
            "w":338,
            "h":327
         }
      }
   ],
   "requestId":"5fda6b40-3f60-4584-bf23-911a0042aa13",
   "metadata":{
      "width":2286,
      "height":1715,
      "format":"Jpeg"
   }
}
```
Bazı durumlarda, marka algılayıcısı hem logo resmi ve stilize marka adı iki ayrı logo alacak.

![Bir red shirt bir Microsoft etiketi ve onu şirket logosu](./Images/red-shirt-logo.jpg)

```json
{
   "brands":[
      {
         "name":"Microsoft",
         "confidence":0.657,
         "rectangle":{
            "x":436,
            "y":473,
            "w":568,
            "h":267
         }
      },
      {
         "name":"Microsoft",
         "confidence":0.85,
         "rectangle":{
            "x":101,
            "y":561,
            "w":273,
            "h":263
         }
      }
   ],
   "requestId":"10dcd2d6-0cf6-4a5e-9733-dc2e4b08ac8d",
   "metadata":{
      "width":1286,
      "height":1715,
      "format":"Jpeg"
   }
}
```

## <a name="use-the-api"></a>API kullanın
Marka algılama özelliği parçasıdır [analiz görüntü](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API. Bu API'nin yerel SDK veya REST çağrılarını aracılığıyla çağırabilirsiniz. Yalnızca tam JSON yanıt aldığınızda içeriğini dizesi ayrıştırılamıyor. `"brands"` bölümü.

* [Hızlı Başlangıç: (.NET SDK) bir resmi çözümleme](./quickstarts-sdk/csharp-analyze-sdk.md)
* [Hızlı Başlangıç: Bir resmi (REST API'si) çözümleme](./quickstarts/csharp-analyze.md)