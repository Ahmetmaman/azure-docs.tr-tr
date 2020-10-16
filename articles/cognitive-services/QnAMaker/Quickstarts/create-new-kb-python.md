---
title: 'Hızlı başlangıç: Bilgi bankası oluşturma - REST, Python - Soru-Cevap Oluşturma'
description: Bu Python REST tabanlı hızlı başlangıçta Bilişsel Hizmetler API hesabınızdaki Azure Panonuzda görünecek olan örnek bir Soru-Cevap Oluşturma bilgi bankasını programlamayla oluşturma adımları gösterilmektedir.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.date: 12/16/2019
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27, devx-track-python
ms.topic: how-to
ms.openlocfilehash: 6155722b81b32e426a91a283910e8bdce926e342
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91777651"
---
# <a name="quickstart-create-a-knowledge-base-in-qna-maker-using-python"></a>Hızlı başlangıç: Python kullanarak Soru-Cevap Oluşturma’da bilgi bankası oluşturma

Bu hızlı başlangıçta program aracılığıyla örnek bir Soru-Cevap Oluşturma bilgi bankası (KB) oluşturma ve yayımlama adımları gösterilir. Soru-Cevap Oluşturma, [veri kaynaklarından](../Concepts/knowledge-base.md) ve SSS gibi yarı yapılandırılmış içerikten soru ve cevapları otomatik olarak ayıklar. JSON ile tanımlanan bilgi bankası modeli API isteğinin gövdesinde gönderilir.

Bu hızlı başlangıç şu Soru-Cevap Oluşturma API'lerini çağırır:
* [KB Oluşturma](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create)
* [İşlem Ayrıntılarını Alma](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/operations/getdetails)

[Başvuru belgeleri](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase)  |  [Python örneği](https://github.com/Azure-Samples/cognitive-services-qnamaker-python/blob/master/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* [Python 3.7](https://www.python.org/downloads/)
* Bir [soru-cevap oluşturma hizmetiniz](../How-To/set-up-qnamaker-service-azure.md)olmalıdır. Anahtarınızı ve uç noktanızı (kaynak adını da içerir) almak için Azure portal kaynağınız için **hızlı başlangıç** ' ı seçin.

## <a name="create-a-knowledge-base-python-file"></a>Bilgi bankası Python dosyası oluşturma

`create-new-knowledge-base-3x.py` adlı bir dosya oluşturun.

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Aşağıdaki satırları `create-new-knowledge-base-3x.py` adlı dosyanın en üstüne ekleyerek projeye gerekli bağımlılıkları dahil edin:

:::code language="python" source="~/cognitive-services-quickstart-code/python/QnAMaker/rest/create-kb.py" id="dependencies":::

## <a name="add-the-required-constants"></a>Gerekli sabitleri ekleme
Yukarıdaki gerekli bağımlılıklardan sonra Soru-Cevap Oluşturma hizmetine erişmek için gerekli sabitleri ekleyin. `<your-qna-maker-subscription-key>`Ve değerlerini `<your-resource-name>` kendi soru-cevap oluşturma anahtar ve kaynak adınızla değiştirin.

Program sınıfının en üstünde, Soru-Cevap Oluşturma erişmek için gereken sabitleri ekleyin.

Aşağıdaki değerleri ayarlayın:

* `<your-qna-maker-subscription-key>` - **Anahtar** , bir 32 karakter dizesidir ve hızlı başlangıç sayfasında Soru-Cevap Oluşturma kaynağında Azure Portal kullanılabilir. Bu, tahmin uç noktası anahtarıyla aynı değildir.
* `<your-resource-name>` - **Kaynak adınız** yazma için yazma uç noktası URL 'sini, biçiminde oluşturmak için kullanılır `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com` . Bu, tahmin uç noktasını sorgulamak için kullanılan URL 'nin aynı değildir.

:::code language="python" source="~/cognitive-services-quickstart-code/python/QnAMaker/rest/create-kb.py" id="constants":::

## <a name="add-the-kb-model-definition"></a>KB modeli tanımını ekleme

Sabitlerden sonra aşağıdaki KB modeli tanımını ekleyin. Model tanımdan sonra bir dizeye dönüştürülür.

:::code language="python" source="~/cognitive-services-quickstart-code/python/QnAMaker/rest/create-kb.py" id="model":::

## <a name="add-supporting-function"></a>Destekleyici işlevi ekleme

JSON sonucunu okunabilir biçimde yazdırmak için aşağıdaki işlevi ekleyin:

:::code language="python" source="~/cognitive-services-quickstart-code/python/QnAMaker/rest/create-kb.py" id="pretty":::

## <a name="add-function-to-create-kb"></a>KB oluşturma işlevini ekleme

Bilgi bankasını oluşturma amacıyla bir HTTP POST isteğinde bulunmak için aşağıdaki işlevi ekleyin.
Bu API çağrısı, **Location** üst bilgi alanında işlem kimliğini içeren bir JSON yanıtı döndürür. İşlem kimliğini KB'nin başarıyla oluşturulup oluşturulmadığını belirlemek için kullanın. `Ocp-Apim-Subscription-Key`, Soru-Cevap Oluşturma hizmeti anahtarıdır ve kimlik doğrulaması için kullanılır.

:::code language="python" source="~/cognitive-services-quickstart-code/python/QnAMaker/rest/create-kb.py" id="create_kb":::

Bu API çağrısı, işlem kimliğini içeren bir JSON yanıtı döndürür. İşlem kimliğini KB'nin başarıyla oluşturulup oluşturulmadığını belirlemek için kullanın.

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:19:01Z",
  "lastActionTimestamp": "2018-09-26T05:19:01Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "8dfb6a82-ae58-4bcb-95b7-d1239ae25681"
}
```

## <a name="add-function-to-check-creation-status"></a>Oluşturma durumunu denetlemek için işlev ekleme

Aşağıdaki işlev, URL yolunun sonundaki işlem kimliğini göndererek oluşturma durumunu denetler. `check_status` çağrısı, ana _while_ döngüsünün içinde yer alır.

:::code language="python" source="~/cognitive-services-quickstart-code/python/QnAMaker/rest/create-kb.py" id="get_status":::

Bu API çağrısı, işlem durumunu içeren bir JSON yanıtı döndürür:

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:22:53Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

Başarılı veya başarısız bir sonuç alana kadar çağrıyı tekrarlayın:

```JSON
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:23:08Z",
  "resourceLocation": "/knowledgebases/XXX7892b-10cf-47e2-a3ae-e40683adb714",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

## <a name="add-main-code-block"></a>Ana kod bloğunu ekleme
Aşağıdaki döngü, oluşturma işlemi tamamlanana kadar düzenli aralıklarla işlemin durumunu yoklar.

:::code language="python" source="~/cognitive-services-quickstart-code/python/QnAMaker/rest/create-kb.py" id="main":::

## <a name="build-and-run-the-program"></a>Programı derleme ve çalıştırma

Programı çalıştırmak için aşağıdaki komutu bir komut satırına yazın. Soru-Cevap Oluşturma API'sine KB oluşturma isteği gönderir ve 30 saniyede bir sonucu yoklar. Her yanıt konsol penceresine yazdırılır.

```bash
python create-new-knowledge-base-3x.py
```

Bilgi bankanız oluşturulduktan sonra Soru-Cevap Oluşturma Portalı’nızdaki [Bilgi bankalarım](https://www.qnamaker.ai/Home/MyServices) sayfasından görüntüleyebilirsiniz. Görüntülemek için bilgi bankanızın adını seçin, örneğin Soru-Cevap Oluşturma SSS.

[!INCLUDE [Clean up files and KB](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)
