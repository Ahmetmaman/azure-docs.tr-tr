---
title: 'Hızlı başlangıç: Azure REST API ve kıvrımlı bir görüntüdeki yüzeyleri algılama'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, görüntüdeki yüzeyleri algılamak için Azure yüz REST API kıvrımlı olarak kullanacaksınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 11/23/2020
ms.author: pafarley
ms.openlocfilehash: 922b261ffeb6cb7a7170a50a3ba5e5ca91a10a11
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "96012008"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-face-rest-api-and-curl"></a>Hızlı başlangıç: yüz REST API ve kıvrımlı kullanarak görüntüdeki yüzeyleri algılama

Bu hızlı başlangıçta, bir görüntüdeki insan yüzlerini saptamak için Azure yüz REST API kıvrımlı olarak kullanacaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/cognitive-services/) oluşturun. 

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/)
* Azure aboneliğiniz olduktan sonra, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesFace"  title=" bir yüz kaynağı oluşturun "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> Azure Portal anahtar ve uç noktanıza ulaşmak için bir yüz kaynağı oluşturun. Dağıtıldıktan sonra **Kaynağa Git ' e** tıklayın.
    * Uygulamanızı Yüz Tanıma API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.

## <a name="write-the-command"></a>Komutu yazın
 
Yüz Tanıma API'si çağırmak ve bir görüntüden yüz öznitelik verileri almak için aşağıdaki gibi bir komut kullanacaksınız. İlk olarak, kodu bir metin düzenleyicisine kopyalayın, &mdash; çalıştırmadan önce komutun belirli kısımlarında değişiklik yapmanız gerekir.

:::code language="shell" source="~/cognitive-services-quickstart-code/curl/face/detect.sh" id="detection_model_2":::

### <a name="subscription-key"></a>Abonelik anahtarı
`<Subscription Key>`Geçerli yüz abonelik anahtarınızla değiştirin.

### <a name="face-endpoint-url"></a>Yüz uç noktası URL 'SI

URL, `https://<My Endpoint String>.com/face/v1.0/detect` Sorgulanacak Azure yüz uç noktasını belirtir. Bu URL 'nin ilk kısmını, abonelik anahtarınıza karşılık gelen uç noktayla eşleşecek şekilde değiştirmeniz gerekebilir.

[!INCLUDE [subdomains-note](../../../../includes/cognitive-services-custom-subdomains-note.md)]

### <a name="image-source-url"></a>Görüntü kaynağı URL 'SI
Kaynak URL 'SI, giriş olarak kullanılacak resmi belirtir. Bunu, çözümlemek istediğiniz herhangi bir görüntüyü işaret ederek değiştirebilirsiniz.

```
https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg
``` 

## <a name="run-the-command"></a>Yeni bir “kurtarma VM’si” oluşturmak ve sorunlu VM’nin işletim sistemi diskini kurtarma VM’sine veri diski olarak takmak için

Değişikliklerinizi yaptıktan sonra, bir komut istemi açın ve yeni komutu girin. Konsol penceresinde JSON verileri olarak görünen yüz bilgilerini görmeniz gerekir. Örnek:

```json
[
  {
    "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f",
    "faceRectangle": {
      "top": 131,
      "left": 177,
      "width": 162,
      "height": 162
    }
  }
]  
```

## <a name="extract-face-attributes"></a>Yüz özniteliklerini Ayıkla
 
Yüz özniteliklerini ayıklamak için, algılama modeli 1 ' i kullanın ve `returnFaceAttributes` sorgu parametresini ekleyin. Komut artık aşağıdaki gibi görünmelidir. Daha önce olduğu gibi, yüz abonelik anahtarınızı ve uç noktayı ekleyin.

:::code language="shell" source="~/cognitive-services-quickstart-code/curl/face/detect.sh" id="detection_model_1":::

Döndürülen yüz bilgileri artık yüz öznitelikleri içeriyor. Örnek:

```json
[
  {
    "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f",
    "faceRectangle": {
      "top": 131,
      "left": 177,
      "width": 162,
      "height": 162
    },
    "faceAttributes": {
      "smile": 0,
      "headPose": {
        "pitch": 0,
        "roll": 0.1,
        "yaw": -32.9
      },
      "gender": "female",
      "age": 22.9,
      "facialHair": {
        "moustache": 0,
        "beard": 0,
        "sideburns": 0
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0,
        "disgust": 0,
        "fear": 0,
        "happiness": 0,
        "neutral": 0.986,
        "sadness": 0.009,
        "surprise": 0.005
      },
      "blur": {
        "blurLevel": "low",
        "value": 0.06
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.67
      },
      "noise": {
        "noiseLevel": "low",
        "value": 0
      },
      "makeup": {
        "eyeMakeup": true,
        "lipMakeup": true
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
      "hair": {
        "bald": 0,
        "invisible": false,
        "hairColor": [
          {
            "color": "brown",
            "confidence": 1
          },
          {
            "color": "black",
            "confidence": 0.87
          },
          {
            "color": "other",
            "confidence": 0.51
          },
          {
            "color": "blond",
            "confidence": 0.08
          },
          {
            "color": "red",
            "confidence": 0.08
          },
          {
            "color": "gray",
            "confidence": 0.02
          }
        ]
      }
    }
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir görüntüdeki yüzeyleri tespit etmek ve özniteliklerini döndürmek için Azure yüz hizmetini çağıran bir kıvrımlı komut yazdınız. Daha fazla bilgi edinmek için Yüz Tanıma API'si başvuru belgelerini inceleyin.

> [!div class="nextstepaction"]
> [Yüz Tanıma API'si](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
