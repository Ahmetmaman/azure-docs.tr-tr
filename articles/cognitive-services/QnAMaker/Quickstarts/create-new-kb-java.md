---
title: 'Hızlı başlangıç: Bilgi bankası oluşturma - REST, Java - Soru-Cevap Oluşturma'
description: Bu Java REST tabanlı hızlı başlangıç, programlama yoluyla, bilişsel hizmetler API hesabınızın Azure panonuzda görünecek bir örnek Soru-Cevap Oluşturma Bilgi Bankası oluşturma işlemi boyunca size yol gösterir.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.date: 12/16/2019
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27, devx-track-java
ms.topic: how-to
ms.openlocfilehash: 275b1976e51789d4a458e7bd9f7cfa3240cc81d5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91777664"
---
# <a name="quickstart-create-a-knowledge-base-in-qna-maker-using-java"></a>Hızlı başlangıç: Java kullanarak Soru-Cevap Oluşturma’da bilgi bankası oluşturma

Bu hızlı başlangıçta program aracılığıyla örnek bir Soru-Cevap Oluşturma bilgi bankası (KB) oluşturma adımları gösterilmektedir. Soru-Cevap Oluşturma, [veri kaynaklarından](../Concepts/knowledge-base.md) ve SSS gibi yarı yapılandırılmış içerikten soru ve cevapları otomatik olarak ayıklar. JSON ile tanımlanan bilgi bankası modeli API isteğinin gövdesinde gönderilir.

Bu hızlı başlangıç şu Soru-Cevap Oluşturma API'lerini çağırır:
* [KB Oluşturma](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create)
* [İşlem Ayrıntılarını Alma](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/operations/getdetails)

[Başvuru belgeleri](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase)  |  [Java örneği](https://github.com/Azure-Samples/cognitive-services-qnamaker-java/blob/master/documentation-samples/quickstarts/create-knowledge-base/CreateKB.java)

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* [Go 1.10.1](https://golang.org/dl/)
* Bir [soru-cevap oluşturma hizmetiniz](../How-To/set-up-qnamaker-service-azure.md)olmalıdır. Anahtarınızı ve uç noktanızı (kaynak adını da içerir) almak için Azure portal kaynağınız için **hızlı başlangıç** ' ı seçin.

[Örnek kod](https://github.com/Azure-Samples/cognitive-services-qnamaker-java/blob/master/documentation-samples/quickstarts/create-knowledge-base/CreateKB.java) , Java ile soru-cevap oluşturma için GitHub deposunda kullanılabilir.

## <a name="create-a-knowledge-base-file"></a>Bilgi bankası dosyası oluşturma

`CreateKB.java` adlı bir dosya oluşturun

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Aşağıdaki satırları `CreateKB.java` adlı dosyanın en üstüne ekleyerek projeye gerekli bağımlılıkları dahil edin:

:::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/CreateKB.java" id="dependencies":::

## <a name="add-the-required-constants"></a>Gerekli sabitleri ekleme
Yukarıdaki gerekli bağımlılıklardan sonra Soru-Cevap Oluşturma hizmetine erişmek için `CreateKB` sınıfına gerekli sabitleri ekleyin.

Bir [soru-cevap oluşturma hizmetiniz](../How-To/set-up-qnamaker-service-azure.md)olmalıdır. Anahtarınızı ve kaynak adınızı almak için Soru-Cevap Oluşturma kaynağınız için Azure portal **hızlı başlangıç** ' ı seçin.

Aşağıdaki değerleri ayarlayın:

* `<your-qna-maker-subscription-key>` - **Anahtar** , bir 32 karakter dizesidir ve hızlı başlangıç sayfasında Soru-Cevap Oluşturma kaynağında Azure Portal kullanılabilir. Bu anahtar, tahmin uç noktası anahtarıyla aynı değildir.
* `<your-resource-name>` - **Kaynak adınız** yazma için yazma uç noktası URL 'sini, biçiminde oluşturmak için kullanılır `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com` . Bu kaynak adı, tahmin uç noktasını sorgulamak için kullanılan ile aynı değil.

Sınıfı bitirmek için küme ayracı eklemeniz gerekmez; bu, hızlı başlangıcın sonundaki en son kod parçacığındadır.

:::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/CreateKB.java" id="constants":::


## <a name="add-the-kb-model-definition-classes"></a>KB modeli tanım sınıfları ekleme
Sabitlerden sonra model tanımı nesnesini JSON’da seri hale getirmek için `CreateKB` sınıfına aşağıdaki sınıf ve işlevleri ekleyin.

:::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/CreateKB.java" id="model":::

## <a name="add-supporting-functions"></a>Destekleyici işlevleri ekleme

Bir sonraki adımda `CreateKB` sınıfının içine aşağıdaki destekleyici işlevleri ekleyin.

1. JSON sonucunu okunabilir biçimde yazdırmak için aşağıdaki işlevi ekleyin:

    :::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/CreateKB.java" id="pretty":::

2. HTTP yanıtını yönetmek için aşağıdaki sınıfı ekleyin:

    :::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/CreateKB.java" id="response":::

3. Soru-Cevap Oluşturma API'lerine POST isteği göndermek için aşağıdaki yöntemi ekleyin. `Ocp-Apim-Subscription-Key`, Soru-Cevap Oluşturma hizmeti anahtarıdır ve kimlik doğrulaması için kullanılır.

    :::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/CreateKB.java" id="post":::

4. Soru-Cevap Oluşturma API'lerine GET isteği göndermek için aşağıdaki yöntemi ekleyin.

    :::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/CreateKB.java" id="get":::

## <a name="add-a-method-to-create-the-kb"></a>KB oluşturmak için yöntem ekleme
Post yöntemine çağırarak KB’yi oluşturmak için aşağıdaki yöntemi ekleyin.

:::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/CreateKB.java" id="create_kb":::

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

## <a name="add-a-method-to-get-status"></a>Durumu almak için bir yöntem ekleme
Oluşturma durumunu denetlemek için aşağıdaki yöntemi ekleyin.

:::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/CreateKB.java" id="get_status":::

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

## <a name="add-a-main-method"></a>Main yöntemi ekleme
Main yöntemi KB'yi oluşturur, sonra da durum için yoklama yapar. İşlem KIMLIĞI, yanıt gönder üst bilgisi alan **konumunda**döndürülür, ardından Get isteğindeki yolun bir parçası olarak kullanılır. `while`Tamamlandıysa, döngü durumu yeniden dener.

:::code language="java" source="~/cognitive-services-quickstart-code/java/QnAMaker/rest/CreateKB.java" id="main":::

## <a name="compile-and-run-the-program"></a>Programı derleme ve çalıştırma

1. Gson kitaplığının `./libs` dizininde bulunduğundan emin olun. Komut satırında, dosyayı derleyin `CreateKB.java` :

    ```bash
    javac -cp ".;libs/*" CreateKB.java
    ```

2. Programı çalıştırmak için komut satırına aşağıdaki komutu girin. Soru-Cevap Oluşturma API'sine KB oluşturma isteği gönderir ve 30 saniyede bir sonucu yoklar. Her yanıt konsol penceresine yazdırılır.

    ```bash
    java -cp ",;libs/*" CreateKB
    ```

Bilgi bankanız oluşturulduktan sonra Soru-Cevap Oluşturma Portalı’nızdaki [Bilgi bankalarım](https://www.qnamaker.ai/Home/MyServices) sayfasından görüntüleyebilirsiniz.

[!INCLUDE [Clean up files and KB](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)
