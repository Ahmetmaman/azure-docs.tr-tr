---
title: Bilgi Bankası, REST yayımlama,C#
titleSuffix: QnA Maker- Azure Cognitive Services
description: Bu C# REST tabanlı bir hızlı başlangıç size yol gösterir, yayımlanan Bilgi Bankası temsil eden adanmış bir Azure Search dizini için test edilmiş Bilgi Bankası en son sürümünü iter, Bilgi Bankası yayımlama aracılığıyla. Ayrıca uygulamanızda veya sohbet botunuzda çağrılabilecek bir uç nokta da oluşturulur.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 11/18/2018
ms.author: diberry
ms.openlocfilehash: d8ca655eb3c1c06871b421acb840948aaee25dcd
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55214430"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-c"></a>Hızlı Başlangıç: Soru-cevap Oluşturucu kullanarak Bilgi Bankası yayımlamaC#

REST tabanlı bu hızlı başlangıçta, Bilgi Bankası (KB) program aracılığıyla yayımlama aracılığıyla size yol gösterir. Yayımlama, bilgi bankanızın son sürümünü adanmış bir Azure Search dizinine gönderir ve uygulamanızda ya da sohbet botunuzda çağrılabilecek bir uç nokta oluşturur.

Bu hızlı başlangıç şu Soru-Cevap Oluşturma API'lerini çağırır:
* [Publish](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fe): Bu API için istek gövdesinde herhangi bir bilgi iletilmesi gerekmez.

## <a name="prerequisites"></a>Önkoşullar

* En son [**Visual Studio Community sürümü**](https://www.visualstudio.com/downloads/).
* [Soru-Cevap Oluşturma hizmetine](../How-To/set-up-qnamaker-service-azure.md) sahip olmanız gerekir. Anahtarınızı almak için, panonuzda **Kaynak Yönetimi** altında **Anahtarlar** öğesini seçin. 
* Soru-Cevap Oluşturma bilgi bankası (KB) kimliği aşağıda gösterildiği gibi URL'nin kbid sorgu dizesi bölümünde bulunur.

    ![Soru-Cevap Oluşturma bilgi bankası kimliği](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    Bilgi Bankası henüz sahip değilseniz, bu hızlı başlangıç için kullanmak üzere bir örnek oluşturabilirsiniz: [Yeni Bilgi Bankası oluşturma](create-new-kb-csharp.md).

> [!NOTE] 
> Eksiksiz bir çözüm dosyaları kullanılabilir [ **Azure-Samples/bilişsel-services-qnamaker-csharp** GitHub deposu](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/quickstarts/publish-knowledge-base).

## <a name="create-knowledge-base-project"></a>Bilgi bankası projesi oluşturma

1. Visual Studio 2017 Community Edition'ı açın.
1. Yeni bir **Konsol Uygulaması (.NET Core)** projesi oluşturun ve `QnaMakerQuickstart` olarak adlandırın. Diğer ayarlar için varsayılan değerleri kabul edin.

## <a name="add-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Program.cs dosyasının en üst kısmındaki tek using deyimini aşağıdaki satırlarla değiştirerek projeye gerekli bağımlılıkları ekleyin:

[!code-csharp[Add the required dependencies](~/samples-qnamaker-csharp/documentation-samples/quickstarts/publish-knowledge-base/QnAMakerPublishQuickstart/Program.cs?range=1-2 "Add the required dependencies")]

## <a name="add-required-constants"></a>Gerekli sabitleri ekleme

İçinde **ana** yöntemi, soru-cevap Oluşturucu erişmek için gerekli sabitleri ekleyin. Değerleri kendi değerlerinizle değiştirin.

[!code-csharp[Add the required constants](~/samples-qnamaker-csharp/documentation-samples/quickstarts/publish-knowledge-base/QnAMakerPublishQuickstart/Program.cs?range=11-14 "Add the required constants")]

## <a name="add-post-request-to-publish-knowledge-base"></a>Bilgi Bankası yayımlama için POST isteği Ekle

Sonra gerekli sabitleri, Bilgi Bankası yayımlama için soru-cevap Oluşturucu API'si bir HTTPS isteği yapar ve yanıtı alan aşağıdaki kodu ekleyin:

[!code-csharp[Add HTTP Post request and response](~/samples-qnamaker-csharp/documentation-samples/quickstarts/publish-knowledge-base/QnAMakerPublishQuickstart/Program.cs?range=16-29&dedent=8 "Add HTTP Post request and response")]

Yayımlama başarılı olursa API çağrısı boş yanıt gövdesiyle 204 durumunu döndürür. 
 
## <a name="build-and-run-the-program"></a>Programı derleme ve çalıştırma

Programı derleyin ve çalıştırın. Otomatik olarak Bilgi Bankası yayımlama için soru-cevap Oluşturucu API'si isteği gönderir ve yanıtı konsol penceresinde yazdırılır.

Bilgi bankanız yayımlandıktan sonra istemci uygulaması veya sohbet botu ile uç noktadan sorgulayabilirsiniz. 

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Sonraki adımlar

Bilgi Bankası yayımlandıktan sonra ihtiyacınız [yanıt oluşturmak için uç nokta URL'si](../Tutorials/create-publish-answer.md#generating-an-answer). 

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
