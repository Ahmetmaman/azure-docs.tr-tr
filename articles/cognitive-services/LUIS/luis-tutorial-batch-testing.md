---
title: 'Öğretici: sorunları bulmak için Batch testi-LUSıS'
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, batch test utterance tahmin sorunları uygulamanızı bulun ve bunları düzeltmek için nasıl kullanılacağı gösterilmektedir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 12/19/2019
ms.author: diberry
ms.openlocfilehash: 54beb26554fd823c46f961b4cc7057f347ad343c
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75447961"
---
# <a name="tutorial-batch-test-data-sets"></a>Öğretici: Batch test veri kümeleri

Bu öğreticide, batch test utterance tahmin sorunları uygulamanızı bulun ve bunları düzeltmek için nasıl kullanılacağı gösterilmektedir.

Toplu test etkin doğrulamanıza olanak tanır, modelin bilinen durumuyla etiketli konuşma ve varlıkların eğitim. JSON biçimli bir toplu iş dosyası Konuşma ekleme ve gereksinim duyduğunuz varlık etiketleri içinde utterance tahmin ayarlayın.

Toplu test etmek için gereksinimler:

* En fazla test başına 1000 konuşma.
* Yinelenen değer yok.
* İzin verilen varlık türleri: yalnızca, basit ve kompozit 'ın öğrenildiği varlıkları. Toplu test yalnızca öğrenilen Eve amaç ve varlıkları için yararlı olur.

Bu öğreticinin dışında bir uygulama kullanırken, yapmak *değil* zaten bir amaç için eklenen örnek konuşma kullanın.



**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Örnek uygulamayı içeri aktar
> * Toplu test dosyası oluşturma
> * Toplu test çalıştırma
> * Test sonuçlarını gözden geçirme
> * Hataları düzelt
> * Batch yeniden test et

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="import-example-app"></a>Örnek uygulamayı içeri aktar

Son öğreticide oluşturulan **HumanResources** adlı uygulamayla devam edin.

Aşağıdaki adımları kullanın:

1.  [Uygulama JSON dosyasını](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-review-HumanResources.json?raw=true) indirip kaydedin.


2. JSON'ı yeni bir uygulamaya içeri aktarın.

3. **Yönet** bölümünde **Sürümler** sekmesinde sürümü kopyalayın ve `batchtest` olarak adlandırın. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. Sürüm adı URL rotasının bir parçası olarak kullanıldığından ad bir URL'de geçerli olmayan hiçbir karakter içeremez.

4. Uygulamayı eğitin.

## <a name="batch-file"></a>Toplu iş dosyası

1. Oluşturma `HumanResources-jobs-batch.json` bir metin düzenleyicisinde veya [indirme](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/HumanResources-jobs-batch.json?raw=true) bu.

2. Konuşma ile JSON biçimli toplu iş dosyasında, ekleme **hedefi** testinde tahmin edilen istiyor.

   [!code-json[Add the intents to the batch test file](~/samples-luis/documentation-samples/tutorials/HumanResources-jobs-batch.json "Add the intents to the batch test file")]

## <a name="run-the-batch"></a>Toplu işlem çalıştırın

1. Seçin **Test** üst gezinti çubuğunda.

2. Seçin **test paneli toplu** Sağdaki panelde.

    [![Toplu test paneliyle vurgulanmış ekran görüntüsü, LUIS uygulama](./media/luis-tutorial-batch-testing/hr-batch-testing-panel-link.png)](./media/luis-tutorial-batch-testing/hr-batch-testing-panel-link.png#lightbox)

3. Seçin **alma dataset**.

    > [!div class="mx-imgBorder"]
    > Içeri aktarma veri kümesi vurgulanmış LUO uygulamasının ekran görüntüsünü ![](./media/luis-tutorial-batch-testing/hr-import-dataset-button.png)

4. Dosya konumunu seçin `HumanResources-jobs-batch.json` dosya.

5. Veri kümesi adı `intents only` seçip **Bitti**.

    ![Dosya seç](./media/luis-tutorial-batch-testing/hr-import-new-dataset-ddl.png)

6. **Çalıştır** düğmesini seçin.

7. Seçin **bkz sonuçları**.

8. Gösterge ve grafik sonuçlarını gözden geçirin.

    [![Toplu test sonuçları ile ekran görüntüsü, LUIS uygulaması](./media/luis-tutorial-batch-testing/hr-intents-only-results-1.png)](./media/luis-tutorial-batch-testing/hr-intents-only-results-1.png#lightbox)

## <a name="review-batch-results"></a>Toplu iş sonuçlarını gözden geçirin

Batch grafik dört quadrants sonuçlarını görüntüler. Grafiğin sağına bir filtredir. Filtre, amaçları ve varlıkları içerir. Seçtiğinizde, bir [grafik bölümünü](luis-concept-batch-test.md#batch-test-results) veya bir nokta grafik içinde ilişkili utterance(s) grafiğin altına görüntüleyebilirsiniz.

Grafik üzerine gelindiğinde, fare tekerleğini büyütebilir veya grafikte görüntülenecek azaltın. Bu, sıkı bir şekilde birlikte kümelenmiş grafik üzerinde çok sayıda noktası olduğunda yararlıdır.

Grafik dört Çeyrek dairelerle iki kırmızı renkte gösterilir bölümlerin birlikte kullanılıyor. **Temel odak noktası bölümlere bunlar**.

### <a name="getjobinformation-test-results"></a>GetJobInformation test sonuçları

**GetJobInformation** filtrede görüntülenen test sonuçları göster 2 dört tahminlerin başarılı. Grafiğin altındaki diğer adımları görmek için sol alt çeyrek altında **yanlış negatif** adını seçin.

Kullanıcı utisin tam metnini görmek için CTRL + E klavye tuşlarını kullanarak etiket görünümüne geçin.

Söylenişi `Is there a database position open in Los Colinas?`, _getjobınformation_ olarak etiketlidir ancak geçerli model, bir _applyforjob_olarak gösterilen şekilde tahmin edilir.

**Getjobınformation**'Ten **applyforjob** için pek çok örnek vardır. Bu örnek, **Applyforjob** hedefinin kullanım açısından, yanlış tahmine neden olacak şekilde tartılanmalar.

Her iki amacı aynı hataların sayısını olduğunu fark edeceksiniz. Tek amacı, yanlış bir tahmin diğer hedefi de etkiler. Konuşma yanlış bir amaç için tahmin edilen ve başka bir amaç için Ayrıca yanlış tahmin değil çünkü her ikisi de hatalarla karşılaştınız.

<a name="fix-the-app"></a>

## <a name="how-to-fix-the-app"></a>Uygulamayı çözme

Bu bölümde tüm sesleri doğru şekilde tahmin için hedefidir **GetJobInformation** uygulama düzeltme tarafından.

Bu toplu dosya konuşma doğru ıntent'e ekleme görünüşte hızlı düzeltme olacaktır. Bu, yapmak istediğiniz şeydir. LUIS, örnek olarak eklemeden Bu konuşma doğru şekilde tahmin etmek istediğiniz.

Konuşma alanından kaldırma hakkında da merak edebilirsiniz **ApplyForJob** utterance miktarı aynı olana kadar **GetJobInformation**. Test sonuçlarını giderebilir ancak bu hedefi doğru sonraki tahmin gelen LUIS azaltabilir.

Bu çözüm, **Getjobınformation**için daha fazla bildirim eklemektir. İş için uygulanamamakta olan iş bilgilerini bulma amacını hala hedeflerken, söylenişi uzunluğunu, sözcük seçimini ve sözcük düzenlemesini değiştirmeyi unutmayın.

### <a name="add-more-utterances"></a>Daha fazla Konuşma ekleme

1. Toplu test paneli seçerek kapatmak **Test** üst gezinti panelinde düğmesini.

2. Seçin **GetJobInformation** hedefleri listesinde.

3. Uzunluğu, word seçeneği ve word düzenleme, koşulları eklediğinizden emin olmak için değiştirilen daha fazla Konuşma ekleme `resume`, `c.v.`, ve `apply`:

    |İçin örnek konuşma **GetJobInformation** hedefi|
    |--|
    |Yeni bir stocker ambarı işinde bir özgeçmiş uygulayabilirim gerektiriyor mu?|
    |Bugün çatı işleri nerede?|
    |Ben bir özgeçmiş gerektiren Tıbbi bir kodlama işi oluştu olduğunu.|
    |Üniversite Çocuklar kendi c.v.s. yazma yardımcı olan bir işi istiyorum |
    |Bilgisayarları kullanarak yüksekokul, yeni bir gönderi mi arıyorsunuz my sürdürme aşağıda verilmiştir.|
    |Hangi konumları alt ve giriş dikkatli kullanılabilir mi?|
    |Stajyeri Masası gazete sırasında mı?|
    |My C.v. satın alma, bütçe ve kayıp para çözümleme en iyi ben gösterir. Bu türdeki bir iş için bir şey var mı?|
    |Burada dünya işleri şu anda ayrıntılara misiniz?|
    |Ben 8 yıl EMS sürücü olarak çalıştım. Tüm yeni işler?|
    |Yeni Gıda işleme işleri uygulama gerektiriyor?|
    |Kaç yeni yard iş işleri kullanılabilir mi?|
    |Yeni bir SA gönderi işçilik ilişkileri ve anlaşmaları var mı?|
    |Kitaplık ve Arşiv Yönetimi'nde bir asıl var. Tüm yeni konumlarına?|
    |Babysitting tüm işler için 13 yıl grubundakiler şehirde bugün var mı?|

    Etiket yok **iş** konuşma varlık. Öğreticinin bu bölümünde, yalnızca hedefi tahmin odaklanmıştır.

4. Seçerek uygulama eğitme **eğitme** üst sağ gezinti.

## <a name="verify-the-new-model"></a>Yeni model doğrulama

Toplu test konuşma doğru şekilde tahmin doğrulamak için batch testi yeniden çalıştırın.

1. Seçin **Test** üst gezinti çubuğunda. Toplu sonuçları hala açıksa seçin **listesine geri**.

1. Toplu işlem adının sağ tarafındaki üç nokta (***...***) düğmesini seçin ve **Çalıştır**' ı seçin. Toplu test işlemi tamamlanana kadar bekleyin. Dikkat **bkz sonuçları** düğme yeşildir şimdi. Başka bir deyişle, toplu işin tamamını başarıyla çalıştırıldı.

1. Seçin **bkz sonuçları**. Intents tüm hedefi adları solundaki yeşil simgeler olması gerekir.

## <a name="create-batch-file-with-entities"></a>Toplu iş dosyası ile varlıkları oluşturun

Toplu test varlıklarda doğrulamak için varlıkları batch JSON dosyasında etiketlenmesi gerekir.

Varlıklar için toplam word çeşitlemesi ([belirteci](luis-glossary.md#token)) sayısı, tahmin kalite etkileyebilir. Varlık uzunluklarının çeşitli amaca etiketli Konuşma ile sağlanan eğitim verilerini içerdiğinden emin olun.

İlk yazma ve toplu iş dosyaları test, birkaç Konuşma ile başlatmak en iyi ve bildiğiniz varlıkları çalışma yanı sıra, birkaç, düşünme yanlış tahmin. Bu, içinde sorun alanlara hızlıca odaklanmak yardımcı olur. Test sonra **GetJobInformation** ve **ApplyForJob** amacı, tahmin değil, birkaç farklı iş adları kullanarak tahmin ile ilgili bir sorun olup olmadığını görmek için bu toplu test dosyası geliştirildi için belirli değerlerle **iş** varlık.

Değerini bir **iş** test konuşma içinde sağlanan varlıktır genellikle daha fazla sözcük olan birkaç örnekleri içeren bir veya iki sözcük. Varsa _kendi_ İnsan Kaynakları uygulamasında genellikle birçok bir kelimelerin iş adları varsa, örnek Konuşma ile etiketlenmiş **iş** bu uygulamada varlık iyi çalışmamasına.

1. Oluşturma `HumanResources-entities-batch.json` gibi bir metin düzenleyicisinde [VSCode](https://code.visualstudio.com/) veya [indirme](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/HumanResources-entities-batch.json?raw=true) bu.

2. JSON biçimli bir toplu iş dosyasında bir konuşma ile içeren bir nesne dizisi Ekle **hedefi** utterance herhangi bir varlık konumlarını yanı sıra test tahmin edilen istiyor. Bir varlık belirteç tabanlı olduğundan, her varlık, bir karakter durdurmak ve başlatmak emin olun. Başlayamaz veya bitemez utterance bir alan üzerinde. Bu, toplu iş dosyasını içeri aktarma sırasında bir hataya neden olur.

   [!code-json[Add the intents and entities to the batch test file](~/samples-luis/documentation-samples/tutorials/HumanResources-entities-batch.json "Add the intents and entities to the batch test file")]


## <a name="run-the-batch-with-entities"></a>Batch ile varlıkları çalıştırma

1. Seçin **Test** üst gezinti çubuğunda.

2. Seçin **test paneli toplu** Sağdaki panelde.

3. Seçin **alma dataset**.

4. Dosya sistemi konumunu seçin `HumanResources-entities-batch.json` dosya.

5. Veri kümesi adı `entities` seçip **Bitti**.

6. **Çalıştır** düğmesini seçin. Test işlemi tamamlanana kadar bekleyin.

7. Seçin **bkz sonuçları**.

## <a name="review-entity-batch-results"></a>Varlık toplu sonuçları gözden geçirin

Grafik doğru şekilde tahmin edilen tüm hedefleri ile açılır. Hata içeren varlık tahminlerini bulmak için sağ taraftaki filtrenin altına gidin.

1. Seçin **iş** filtredeki varlık.

    ![Filtredeki hata varlığı tahminleri](./media/luis-tutorial-batch-testing/hr-entities-filter-errors.png)

    Varlık Öngörüler görüntülemek için grafik değişir.

2. Seçin **False negatif** quadrant grafiğin alt, sol. Ardından klavye birleşimi control + E belirteci görünümüne geçiş yapmak için kullanın.

    [![Varlık Öngörüler belirteci görünümü](./media/luis-tutorial-batch-testing/token-view-entities.png)](./media/luis-tutorial-batch-testing/token-view-entities.png#lightbox)

    Grafiğin altındaki konuşma gözden geçirme ortaya çıkarır tutarlı bir hata iş adı içerdiğinde `SQL`. Örnek konuşma ve iş ifade listesi gözden geçirme, SQL yalnızca bir kez kullanılır ve yalnızca büyük bir iş adı bir parçası olarak olduğu `sql/oracle database administrator`.

## <a name="fix-the-app-based-on-entity-batch-results"></a>Varlık toplu sonuçlarına göre uygulama düzeltme

Uygulama düzeltme SQL işleri çeşitleri doğru şekilde belirlemek LUIS gerektirir. Bu düzeltme için birkaç seçenek vardır.

* Açıkça SQL kullanın ve bir iş varlığı sözcükleri etiket daha fazla örnek konuşma ekleyin.
* Açıkça daha fazla SQL işleri ifade listesine ekleyin

Bu görevleri sizin için yapmasını bırakılır.

Ekleme bir [deseni](luis-concept-patterns.md) varlık doğru şekilde tahmin önce sorunu gidermek için gittiği değil. Desen deseninde tüm varlıkları algılanan kadar eşleşmeyecektir olmasıdır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bir batch test Geçerli modelde sorunları bulmak için kullanılır. Model sabit ve değişiklik doğru olduğunu doğrulamak için toplu iş dosyasını retested.

> [!div class="nextstepaction"]
> [Desenler hakkında bilgi edinin](luis-tutorial-pattern.md)

