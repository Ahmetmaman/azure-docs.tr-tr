---
title: 'Hızlı başlangıç: Bilgi Bankası oluşturma, eğitme ve yayımlama-Soru-Cevap Oluşturma'
description: SSS sayfaları veya ürün kılavuzları gibi sahip olduğunuz içerikleri kullanarak bir Soru-Cevap Oluşturma bilgi bankası (KB) oluşturabilirsiniz. Bu makale, Soru-Cevap Oluşturma soruları yanıtlamak için basit bir SSS Web sayfasından Soru-Cevap Oluşturma Bilgi Bankası oluşturma örneği içerir.
ms.topic: quickstart
ms.date: 05/26/2020
ms.openlocfilehash: 3cc38ca49820b1a97ec11c890bfd0ef1670f6eef
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "89535858"
---
# <a name="quickstart-create-train-and-publish-your-qna-maker-knowledge-base"></a>Hızlı başlangıç: Soru-Cevap Oluşturma bilgi bankasını oluşturma, eğitme ve yayımlama

SSS sayfaları veya ürün kılavuzları gibi sahip olduğunuz içerikleri kullanarak bir Soru-Cevap Oluşturma bilgi bankası (KB) oluşturabilirsiniz. Bu makale, Soru-Cevap Oluşturma soruları yanıtlamak için basit bir SSS Web sayfasından Soru-Cevap Oluşturma Bilgi Bankası oluşturma örneği içerir.

## <a name="prerequisites"></a>Ön koşullar

> [!div class="checklist"]
> * Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/cognitive-services/) oluşturun.
> * Azure portal oluşturulan Soru-Cevap Oluşturma [kaynağı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) . Kaynağı oluştururken seçtiğiniz Azure Active Directory KIMLIĞI, aboneliğiniz, QnA kaynak adınızı unutmayın.

## <a name="create-your-first-qna-maker-knowledge-base"></a>İlk Soru-Cevap Oluşturma bilgi bankasında oluşturma

1. Azure kimlik bilgilerinizle [QnAMaker.ai](https://QnAMaker.ai) portalında oturum açın.

1. Soru-Cevap Oluşturma portalında **Bilgi Bankası oluştur**' u seçin.

1. Soru-Cevap Oluşturma kaynağınız zaten varsa **Oluştur** sayfasında **1. adımı** atlayın.

    Kaynağı henüz oluşturmadıysanız **QnA hizmeti oluştur**' u seçin. Aboneliğinizde bir Soru-Cevap Oluşturma hizmeti ayarlamak için [Azure portala](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) yönlendirilirsiniz. Kaynağı oluştururken seçtiğiniz Azure Active Directory KIMLIĞI, aboneliğiniz, QnA kaynak adınızı unutmayın.

    Azure portal kaynağı oluşturmayı tamamladıktan sonra, Soru-Cevap Oluşturma portalına geri dönün, tarayıcı sayfasını yenileyin ve **2. adıma**geçin.

1. **Adım 3**' te, etkin dizin, abonelik, hizmet (kaynak) ve hizmette oluşturulan tüm bilgi tabanlarının dilini seçin.

    :::image type="content" source="../media/qnamaker-create-publish-knowledge-base/qnaservice-selection.png" alt-text="Soru-Cevap Oluşturma Hizmeti Bilgi Bankası seçme ekran görüntüsü":::

1. **Adım 3**' te bilgi tabanınızı **örnek QNA KB**olarak adlandırın.

1. **4. adımda**ayarları aşağıdaki tabloyla yapılandırın:

    |Ayar|Değer|
    |--|--|
    |**URL 'Ler,. PDF veya. docx dosyalarından Çoklu açma ayıklamasını etkinleştirin.**|İşaretli|
    |**Varsayılan yanıt metni**| `Quickstart - default answer not found.`|
    |**+ URL Ekle**|`https://docs.microsoft.com/azure/cognitive-services/qnamaker/troubleshooting`|
    |**Günlük konuşma**|**Profesyonel** seçin|

1. **5. adımda**, **KB 'nizi oluştur**' u seçin.

    Ayıklama işlemi birkaç dakika sürer ve belgeyi okuyup soruları ve yanıtları belirler.

    Bilgi Bankası Soru-Cevap Oluşturma başarıyla oluşturulduktan sonra **Bilgi Bankası** sayfası açılır. Bilgi Bankası 'nın içeriğini bu sayfada düzenleyebilirsiniz.

## <a name="add-a-new-question-and-answer-set"></a>Yeni soru ve yanıt kümesi ekleme

1. Soru-Cevap Oluşturma portalında, **Düzen** sayfasında, bağlam araç çubuğundan **+ QNA çifti Ekle** ' yi seçin.
1. Aşağıdaki soruyu ekleyin:

    `How many Azure services are used by a knowledge base?`

1. _Markaşağı_ile biçimlendirilen yanıtı ekleyin:

    ` * Azure QnA Maker service\n* Azure Cognitive Search\n* Azure web app\n* Azure app plan`

    :::image type="content" source="../media/qnamaker-create-publish-knowledge-base/add-question-and-answer.png" alt-text="Soru-Cevap Oluşturma Hizmeti Bilgi Bankası seçme ekran görüntüsü":::

    Markaşağı simgesi, `*` , madde işareti noktaları için kullanılır. `\n`Yeni bir satır için kullanılır.

    **Düzenle** sayfası markın gösterir. Daha sonra **Test** panelini kullandığınızda markaşağı doğru görüntülendiğini görürsünüz.

## <a name="save-and-train"></a>Kaydet ve eğit

Düzenlemelerinizi kaydetmek ve Soru-Cevap Oluşturma eğitmeniz için sağ üst köşedeki kaydet ' i **ve eğit** ' i seçin. Kaydedilmeyen düzenlemeler silinir.

## <a name="test-the-knowledge-base"></a>Bilgi Bankası 'nı test etme

1. Soru-Cevap Oluşturma portalında, sağ üst köşedeki **Test** ' i seçerek yaptığınız değişikliklerin etkili olduğunu test edin.
1. Metin kutusuna bir örnek Kullanıcı sorgusu girin.

    `How many Azure services are used by a knowledge base?`

    :::image type="content" source="../media/qnamaker-create-publish-knowledge-base/test-panel-in-qna-maker.png" alt-text="Soru-Cevap Oluşturma Hizmeti Bilgi Bankası seçme ekran görüntüsü":::

1. Yanıtı daha ayrıntılı bir şekilde incelemek için **Inspect** (Denetle) öğesini seçin. Bilgi tabanınızı yayımlamadan önce Bilgi Bankası 'nda yaptığınız değişiklikleri test etmek için test penceresi kullanılır.

1. **Test panelini kapatmak** Için yeniden **Test** ' i seçin.

## <a name="publish-the-knowledge-base"></a>Bilgi bankasını yayımlama

Bir Bilgi Bankası yayımladığınızda, bilgi Bankalarınızın içeriği `test` dizinden `prod` Azure Search 'te bir dizine taşınır.

![Bilgi Bankalarınızın içeriğini taşıma ekran görüntüsü](../media/qnamaker-how-to-publish-kb/publish-prod-test.png)

1. Soru-Cevap Oluşturma portalında **Yayımla**' yı seçin. Ardından onaylamak için sayfadaki **Publish** (Yayımla) öğesini seçin.

    Soru-Cevap Oluşturma hizmeti başarıyla yayımlandı. Uç noktayı uygulamanızda veya bot kodunuzda kullanabilirsiniz.

    ![Başarılı yayımlamanın ekran görüntüsü](../media/qnamaker-create-publish-knowledge-base/publish-knowledge-base-to-endpoint.png)

## <a name="create-a-bot"></a>Bot oluştur

Yayımladıktan sonra **Yayımla** sayfasından bir bot oluşturabilirsiniz:

* Her biri için farklı bölgeler veya fiyatlandırma planları için aynı bilgi tabanına işaret eden her türlü botu hızlı bir şekilde oluşturabilirsiniz.
* Bilgi Bankası için yalnızca bir bot istiyorsanız, geçerli botunuzun bir listesini görüntülemek için **Azure Portal bağlantı çubuğunda tüm botlarınızı görüntüle** ' yi kullanın.

Bilgi bankasında değişiklikler yaptığınızda ve yeniden yayımladığınızda, bot ile daha fazla işlem yapmanız gerekmez. Bilgi Bankası ile çalışmak üzere zaten yapılandırılmıştır ve ileride Bilgi Bankası değişiklikleriyle birlikte çalışır. Bilgi Bankası 'nı her yayımladığınızda, kendisine bağlı olan tüm botlar otomatik olarak güncelleştirilir.

1. Soru-Cevap Oluşturma portalında, **Yayımla** sayfasında, **bot oluştur**' u seçin. Bu düğme yalnızca Bilgi Bankası 'nı yayımladıktan sonra görünür.

    ![Bot oluşturma ekran görüntüsü](../media/qnamaker-create-publish-knowledge-base/create-bot-from-published-knowledge-base-page.png)

1. Azure bot hizmetinin oluşturma sayfası ile Azure portal için yeni bir tarayıcı sekmesi açılır. Azure bot hizmetini yapılandırın. Bot ve Soru-Cevap Oluşturma Web App Service planını paylaşabilir, ancak Web uygulamasını paylaşamaz. Bu, bot 'ın **uygulama adının** soru-cevap oluşturma hizmeti için uygulama adından farklı olması anlamına gelir.

    * **Gösterme**
        * Bot tanıtıcısını Değiştir-benzersiz değilse.
        * SDK dilini seçin. Bot oluşturulduktan sonra, kodu yerel geliştirme ortamınıza indirebilir ve geliştirme sürecine devam edebilirsiniz.
    * **Yapmayın**
        * bot oluştururken Azure portal aşağıdaki ayarları değiştirin. Bunlar, mevcut bilgi tabanınız için önceden doldurulur:
           * QnA auth anahtarı
           * App Service planı ve konumu


1. Bot oluşturulduktan sonra, **bot hizmeti** kaynağını açın.
1. **Bot Management**altında **Web sohbeti ' nda test**' i seçin.
1. **Iletinizi yazın**sohbet isteminde şunu girin:

    `Azure services?`

    Sohbet bot, bilgi tabanınızdan bir cevap vererek yanıt verir.

    :::image type="content" source="../media/qnamaker-create-publish-knowledge-base/test-web-chat.png" alt-text="Soru-Cevap Oluşturma Hizmeti Bilgi Bankası seçme ekran görüntüsü":::

## <a name="what-did-you-accomplish"></a>Ne başardınız?

Yeni bir Bilgi Bankası oluşturdunuz, Bilgi Bankası 'na genel bir URL eklediniz, kendi QnA çiftiniz eklediniz, eğitilen, test edilmiş ve bilgi bankasından yayımladınız.

Bilgi Bankası 'nı yayımladıktan sonra, bir bot oluşturdunuz ve bot 'ı test ettikten sonra.

Bu, herhangi bir kod yazmak veya içeriği temizlemek zorunda kalmadan birkaç dakikada gerçekleştirildi.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki hızlı başlangıca devam değilseniz, Azure portal Soru-Cevap Oluşturma ve bot Framework kaynaklarını silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Meta verilerle soru ekleme](add-question-metadata-portal.md)

Daha fazla bilgi için:

* [Yanıt olarak markaşağı biçimi](../reference-markdown-format.md)
* [Veri kaynaklarını](../concepts/knowledge-base.md)soru-cevap oluşturma.


