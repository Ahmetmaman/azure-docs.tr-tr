---
title: "Hızlı başlangıç: Bilgi Bankası 'nı yayımlama, REST, go-Soru-Cevap Oluşturma"
description: Bu Go, REST tabanlı hızlı başlangıç bilgi tabanınızı yayımlar ve uygulamanızda veya sohbet bot 'ta çağrılabilecek bir uç nokta oluşturur.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.date: 02/08/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27
ms.topic: how-to
ms.openlocfilehash: e2041fe1ea4f79a951cb78dc3f5d36acdfe0b085
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91777524"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-go"></a>Hızlı başlangıç: Go kullanarak Soru-Cevap Oluşturma’da bilgi bankası yayımlama

Bu REST tabanlı hızlı başlangıç, bilgi bankanızı (KB) programlı bir şekilde yayımlama konusunda size yol gösterir. Yayımlama, bilgi tabanının en son sürümünü adanmış bir Azure Bilişsel Arama dizinine gönderir ve uygulamanızda veya sohbet bot 'ta çağrılabilecek bir uç nokta oluşturur.

Bu hızlı başlangıç şu Soru-Cevap Oluşturma API'lerini çağırır:
* [Publish](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish): Bu API için istek gövdesinde herhangi bir bilgi iletilmesi gerekmez.

## <a name="prerequisites"></a>Ön koşullar

* [Go 1.10.1](https://golang.org/dl/)
* Bir [soru-cevap oluşturma hizmetiniz](../How-To/set-up-qnamaker-service-azure.md)olmalıdır. Anahtarınızı ve uç noktanızı (kaynak adını da içerir) almak için Azure portal kaynağınız için **hızlı başlangıç** ' ı seçin.

* `kbid`Sorgu dizesi PARAMETRESINDEKI URL 'de aşağıda gösterildiği gibi soru-cevap oluşturma Bilgi Bankası (KB) kimliği bulundu.

    ![Soru-Cevap Oluşturma bilgi bankası kimliği](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    Henüz bir bilgi bankanız yoksa, bu hızlı başlangıçta kullanmak için bir örneğini oluşturabilirsiniz: [Yeni bilgi bankası oluşturma](create-new-kb-csharp.md).

> [!NOTE]
> Tam çözüm dosyası (ler) [ **Azure-Samples/bilişsel hizmetler-qnamaker-go** GitHub deposunda](https://github.com/Azure-Samples/cognitive-services-qnamaker-go/tree/master/documentation-samples/quickstarts/publish-knowledge-base)bulunur.

## <a name="create-a-go-file"></a>Go dosyası oluşturma

VSCode 'u açın ve adlı yeni bir dosya oluşturun `publish-kb.go` .

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Aşağıdaki satırları `publish-kb.go` adlı dosyanın en üstüne ekleyerek projeye gerekli bağımlılıkları dahil edin:

:::code language="go" source="~/cognitive-services-quickstart-code/go/QnAMaker/rest/publish-kb.go" id="dependencies":::

## <a name="create-the-main-function"></a>Main işlevini oluşturma

Gerekli bağımlılıklardan sonra aşağıdaki sınıfı ekleyin:

```Go
package main

func main() {

}
```

## <a name="add-post-request-to-publish-kb"></a>KB yayımlamak için POST isteğini ekleme

Bir Bilgi Bankası yayınlamak ve yanıtı almak için Soru-Cevap Oluşturma API'si HTTPS isteği yapan aşağıdaki kodu ekleyin:

:::code language="go" source="~/cognitive-services-quickstart-code/go/QnAMaker/rest/publish-kb.go" id="main":::

Yayımlama başarılı olursa API çağrısı boş yanıt gövdesiyle 204 durumunu döndürür. Kod 204 yanıtları için içerik ekler.

Diğer yanıtlarda döndürülen yanıt değiştirilmez.

## <a name="build-and-run-the-program"></a>Programı derleme ve çalıştırma

Dosyayı derlemek için aşağıdaki komutu girin. Komut istemi başarılı bir derleme için herhangi bir bilgi döndürmez.

```bash
go build publish-kb.go
```

Programı çalıştırmak için aşağıdaki komutu bir komut satırına yazın. Bu işlem, KB 'yi yayımlamak için Soru-Cevap Oluşturma API'si isteği gönderir, sonra başarı veya hatalar için 204 yazdır.

```bash
./publish-kb
```

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bilgi Bankası yayımlandıktan sonra, [bir yanıt oluşturmak için uç nokta URL 'sine](./get-answer-from-knowledge-base-go.md)ihtiyacınız vardır.

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)
