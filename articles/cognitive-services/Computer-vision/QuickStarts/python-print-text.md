---
title: 'Hızlı başlangıç: yazdırılan metni Ayıkla-REST, Python'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Python ile Görüntü İşleme API’si kullanarak bir görüntüden yazdırılan metni ayıklayacaksınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 08/05/2020
ms.author: pafarley
ms.custom: seodec18, devx-track-python
ms.openlocfilehash: 539f6fe35a2d6eb0566871dbab4e9758a300e999
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94541887"
---
# <a name="quickstart-extract-printed-text-ocr-using-the-computer-vision-rest-api-and-python"></a>Hızlı başlangıç: Görüntü İşleme REST API ve Python kullanarak yazdırılan metni (OCR) Ayıkla

> [!NOTE]
> Ingilizce dil metnini ayıklıyorsanız, yeni [okuma işlemini](https://docs.microsoft.com/azure/cognitive-services/computer-vision/concept-recognizing-text)kullanmayı göz önünde bulundurun. [Python hızlı](https://docs.microsoft.com/azure/cognitive-services/computer-vision/quickstarts/python-hand-text) başlangıcı kullanılabilir. 

Bu hızlı başlangıçta, Görüntü İşleme REST API kullanarak bir görüntüden optik karakter tanıma (OCR) ile yazdırılan metni ayıklayacaksınız. [OCR](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/56f91f2e778daf14a499f20d) yöntemiyle, bir görüntüdeki yazdırılan metni algılayabilir ve tanınan karakterleri makine tarafından kullanılabilir bir karakter akışı halinde ayıklayabilirsiniz.

[MyBinder](https://mybinder.org) üzerinde bir Jupyter not defteri kullanarak bu hızlı başlangıcı adım adım görüntülenecek şekilde çalıştırabilirsiniz. Bağlayıcıyı başlatmak için aşağıdaki düğmeyi seçin:

[![Bağlayıcısı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=VisionAPI.ipynb)


## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/)
* Örneği yerel olarak çalıştırmak istiyorsanız [Python](https://www.python.org/downloads/) yüklenmiş olmalıdır.
* Azure aboneliğiniz olduktan sonra, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> anahtarınızı ve uç noktanızı almak için Azure Portal bir görüntü işleme kaynağı oluşturun görüntü işleme bir kaynak oluşturun. Dağıtıldıktan sonra **Kaynağa Git ' e** tıklayın.
    * Uygulamanızı Görüntü İşleme hizmetine bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.
* Sırasıyla ve olarak adlandırılan anahtar ve uç nokta URL 'SI için [ortam değişkenleri oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) `COMPUTER_VISION_SUBSCRIPTION_KEY` `COMPUTER_VISION_ENDPOINT` .

## <a name="create-and-run-the-sample"></a>Örnek oluşturma ve çalıştırma

Örneği oluşturup çalıştırmak için aşağıdaki adımları uygulayın:

1. Aşağıdaki kodu bir metin düzenleyicisine kopyalayın.
1. İsteğe bağlı olarak, `image_url` değerini, içinden yazdırılan metni ayıklamak istediğiniz başka bir görüntünün URL’si ile değiştirin.
1. Kodu, `.py` uzantısıyla bir dosya olarak kaydedin. Örneğin, `get-printed-text.py`.
1. Bir komut istemi penceresi açın.
1. İstemde, örneği çalıştırmak için `python` komutunu kullanın. Örneğin, `python get-printed-text.py`.

```python
import os
import sys
import requests
# If you are using a Jupyter notebook, uncomment the following line.
# %matplotlib inline
import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle
from PIL import Image
from io import BytesIO

# Add your Computer Vision subscription key and endpoint to your environment variables.
if 'COMPUTER_VISION_SUBSCRIPTION_KEY' in os.environ:
    subscription_key = os.environ['COMPUTER_VISION_SUBSCRIPTION_KEY']
else:
    print("\nSet the COMPUTER_VISION_SUBSCRIPTION_KEY environment variable.\n**Restart your shell or IDE for changes to take effect.**")
    sys.exit()

if 'COMPUTER_VISION_ENDPOINT' in os.environ:
    endpoint = os.environ['COMPUTER_VISION_ENDPOINT']

ocr_url = endpoint + "vision/v3.1/ocr"

# Set image_url to the URL of an image that you want to analyze.
image_url = "https://upload.wikimedia.org/wikipedia/commons/thumb/a/af/" + \
    "Atomist_quote_from_Democritus.png/338px-Atomist_quote_from_Democritus.png"

headers = {'Ocp-Apim-Subscription-Key': subscription_key}
params = {'language': 'unk', 'detectOrientation': 'true'}
data = {'url': image_url}
response = requests.post(ocr_url, headers=headers, params=params, json=data)
response.raise_for_status()

analysis = response.json()

# Extract the word bounding boxes and text.
line_infos = [region["lines"] for region in analysis["regions"]]
word_infos = []
for line in line_infos:
    for word_metadata in line:
        for word_info in word_metadata["words"]:
            word_infos.append(word_info)
word_infos

# Display the image and overlay it with the extracted text.
plt.figure(figsize=(5, 5))
image = Image.open(BytesIO(requests.get(image_url).content))
ax = plt.imshow(image, alpha=0.5)
for word in word_infos:
    bbox = [int(num) for num in word["boundingBox"].split(",")]
    text = word["text"]
    origin = (bbox[0], bbox[1])
    patch = Rectangle(origin, bbox[2], bbox[3],
                      fill=False, linewidth=2, color='y')
    ax.axes.add_patch(patch)
    plt.text(origin[0], origin[1], text, fontsize=20, weight="bold", va="top")
plt.show()
plt.axis("off")
```

## <a name="upload-image-from-local-storage"></a>Yerel depodan görüntü yükleme

Yerel bir görüntüyü çözümlemek istiyorsanız, Content-Type üst bilgisini Application/sekizli-Stream olarak ayarlayın ve istek gövdesini JSON verileri yerine bir bayt dizisi olarak ayarlayın.

```python
image_path = "<path-to-local-image-file>"
# Read the image into a byte array
image_data = open(image_path, "rb").read()
# Set Content-Type to octet-stream
headers = {'Ocp-Apim-Subscription-Key': subscription_key, 'Content-Type': 'application/octet-stream'}
# put the byte array into your post request
response = requests.post(ocr_url, headers=headers, params=params, data = image_data)
```


## <a name="examine-the-response"></a>Yanıtı inceleme

Başarılı bir yanıt JSON biçiminde döndürülür. Örnek web sayfası, aşağıdaki örneğe benzer şekilde başarılı bir yanıtı ayrıştırıp komut istemi penceresinde görüntüler:

```json
{
  "language": "en",
  "orientation": "Up",
  "textAngle": 0,
  "regions": [
    {
      "boundingBox": "21,16,304,451",
      "lines": [
        {
          "boundingBox": "28,16,288,41",
          "words": [
            {
              "boundingBox": "28,16,288,41",
              "text": "NOTHING"
            }
          ]
        },
        {
          "boundingBox": "27,66,283,52",
          "words": [
            {
              "boundingBox": "27,66,283,52",
              "text": "EXISTS"
            }
          ]
        },
        {
          "boundingBox": "27,128,292,49",
          "words": [
            {
              "boundingBox": "27,128,292,49",
              "text": "EXCEPT"
            }
          ]
        },
        {
          "boundingBox": "24,188,292,54",
          "words": [
            {
              "boundingBox": "24,188,292,54",
              "text": "ATOMS"
            }
          ]
        },
        {
          "boundingBox": "22,253,297,32",
          "words": [
            {
              "boundingBox": "22,253,105,32",
              "text": "AND"
            },
            {
              "boundingBox": "144,253,175,32",
              "text": "EMPTY"
            }
          ]
        },
        {
          "boundingBox": "21,298,304,60",
          "words": [
            {
              "boundingBox": "21,298,304,60",
              "text": "SPACE."
            }
          ]
        },
        {
          "boundingBox": "26,387,294,37",
          "words": [
            {
              "boundingBox": "26,387,210,37",
              "text": "Everything"
            },
            {
              "boundingBox": "249,389,71,27",
              "text": "else"
            }
          ]
        },
        {
          "boundingBox": "127,431,198,36",
          "words": [
            {
              "boundingBox": "127,431,31,29",
              "text": "is"
            },
            {
              "boundingBox": "172,431,153,36",
              "text": "opinion."
            }
          ]
        }
      ]
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Daha sonra, optik karakter tanıma (OCR) yapmak için Görüntü İşleme kullanan bir Python uygulamasını keşfedebilir; Akıllı kırpılan küçük resimler oluşturun; ve görüntülerde görsel özellikleri tespit edin, kategorilere ayırın, etiketleyin ve tanıtın.

> [!div class="nextstepaction"]
> [Görüntü İşleme API'si Python Öğreticisi](../Tutorials/PythonTutorial.md)

* Görüntü İşleme API'sini hızlı bir şekilde denemeniz için [Open API test konsolu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/56f91f2e778daf14a499f21b/console) konusuna bakın.
