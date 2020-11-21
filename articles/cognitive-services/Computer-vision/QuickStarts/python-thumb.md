---
title: 'Hızlı Başlangıç: Küçük resim oluşturma - REST, Python'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Python ile Görüntü İşleme API'sini kullanarak bir görüntüden küçük resim oluşturacaksınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 08/17/2020
ms.author: pafarley
ms.custom: seodec18, devx-track-python
ms.openlocfilehash: 8781fb07667645e7eaec342adc551950a4719ccc
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95013762"
---
# <a name="quickstart-generate-a-thumbnail-using-the-computer-vision-rest-api-and-python"></a>Hızlı başlangıç: Görüntü İşleme REST API ve Python kullanarak küçük resim oluşturma

Bu hızlı başlangıçta, Görüntü İşleme REST API kullanarak bir görüntüden küçük resim oluşturacaksınız. [Küçük resim al](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/56f91f2e778daf14a499f20c) yöntemiyle, istenen yüksekliği ve genişliği belirtebilir ve görüntü işleme akıllı kırpma kullanarak ilgilendiğiniz alanı ve bu bölgeyi temel alan kırpma koordinatları oluşturabilir.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/)
* Azure aboneliğiniz olduktan sonra, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> anahtarınızı ve uç noktanızı almak için Azure Portal bir görüntü işleme kaynağı oluşturun görüntü işleme bir kaynak oluşturun. Dağıtıldıktan sonra **Kaynağa Git ' e** tıklayın.
    * Uygulamanızı Görüntü İşleme hizmetine bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.
* Sırasıyla ve olarak adlandırılan anahtar ve uç nokta URL 'SI için [ortam değişkenleri oluşturun](../../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication) `COMPUTER_VISION_SUBSCRIPTION_KEY` `COMPUTER_VISION_ENDPOINT` .
- [Visual Studio Code](https://code.visualstudio.com/download)gibi bir kod Düzenleyicisi.

## <a name="create-and-run-the-sample"></a>Örnek oluşturma ve çalıştırma

Örneği oluşturmak ve çalıştırmak için, kod düzenleyicisine aşağıdaki kodu kopyalayın. 

```python
import os
import sys
import requests
# If you are using a Jupyter notebook, uncomment the following lines.
# %matplotlib inline
# import matplotlib.pyplot as plt
from PIL import Image
from io import BytesIO

# Add your Computer Vision subscription key and endpoint to your environment variables.
subscription_key = os.environ['COMPUTER_VISION_SUBSCRIPTION_KEY']
endpoint = os.environ['COMPUTER_VISION_ENDPOINT']

thumbnail_url = endpoint + "vision/v3.1/generateThumbnail"

# Set image_url to the URL of an image that you want to analyze.
image_url = "https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg"

# Construct URL
headers = {'Ocp-Apim-Subscription-Key': subscription_key}
params = {'width': '50', 'height': '50', 'smartCropping': 'true'}
data = {'url': image_url}
# Call API
response = requests.post(thumbnail_url, headers=headers, params=params, json=data)
response.raise_for_status()

# Open the image from bytes
thumbnail = Image.open(BytesIO(response.content))

# Verify the thumbnail size.
print("Thumbnail is {0}-by-{1}".format(*thumbnail.size))

# Save thumbnail to file
thumbnail.save('thumbnail.png')

# Display image
thumbnail.show()

# Optional. Display the thumbnail from Jupyter.
# plt.imshow(thumbnail)
# plt.axis("off")
```

Sonra, şunları yapın:

1. Seçim Değerini `image_url` kendi GÖRÜNTÜNÜZÜN URL 'siyle değiştirin.
1. Kodu, `.py` uzantısıyla bir dosya olarak kaydedin. Örneğin, `get-thumbnail.py`.
1. Bir komut istemi penceresi açın.
1. İstemde, örneği çalıştırmak için `python` komutunu kullanın. Örneğin, `python get-thumbnail.py`.

## <a name="examine-the-response"></a>Yanıtı inceleme

Başarılı bir yanıt, küçük resim için görüntü verilerini temsil eden ikili veriler olarak döndürülür. Örnek bu görüntüyü görüntülemelidir. İstek başarısız olursa, yanıt komut istemi penceresinde görüntülenir ve bir hata kodu içermelidir.

## <a name="run-in-jupyter-optional"></a>Jupyıter 'da Çalıştır (isteğe bağlı)

Bu hızlı başlangıcı, [Myciltçi](https://mybinder.org)üzerinde bir Jupyter Not defteri kullanarak bir adım adım biçimde çalıştırabilirsiniz. Bağlayıcıyı başlatmak için aşağıdaki düğmeyi seçin:

[![Bağlayıcısı](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=VisionAPI.ipynb)

## <a name="next-steps"></a>Sonraki adımlar

Daha sonra, optik karakter tanıma (OCR) yapmak için Görüntü İşleme kullanan bir Python uygulamasını keşfedebilir; Akıllı kırpılan küçük resimler oluşturun; ve görüntülerde görsel özellikleri tespit edin, kategorilere ayırın, etiketleyin ve tanıtın.

> [!div class="nextstepaction"]
> [Görüntü İşleme API'si Python Öğreticisi](https://github.com/Microsoft/Cognitive-Vision-Python)

* Görüntü İşleme API'sini hızlı bir şekilde denemeniz için [Open API test konsolu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-1-ga/operations/56f91f2e778daf14a499f20c) konusuna bakın.