---
title: 'Hızlı başlangıç: Özel Görüntü İşleme Web sitesiyle bir nesne algılayıcısı oluşturma'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Özel Görüntü İşleme Web sitesini kullanarak bir nesne algılayıcı modeli oluşturma, eğitme ve test etme hakkında bilgi edineceksiniz.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 01/29/2021
ms.author: pafarley
ms.custom: cog-serv-seo-aug-2020
keywords: görüntü tanıma, görüntü tanıma uygulaması, Özel Vizyon
ms.openlocfilehash: 5ecd5fee565a8d31e0ff05f3b234771446242d02
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "99221304"
---
# <a name="quickstart-build-an-object-detector-with-the-custom-vision-website"></a>Hızlı başlangıç: Özel Görüntü İşleme Web sitesiyle bir nesne algılayıcısı oluşturma

Bu hızlı başlangıçta, Özel Görüntü İşleme Web sitesini kullanarak bir nesne algılayıcı modeli oluşturma hakkında bilgi edineceksiniz. Bir model oluşturduktan sonra, yeni görüntülerle test edebilir ve sonuç olarak kendi görüntü tanıma uygulamanız ile tümleştirebilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/cognitive-services/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

- Algılayıcı modelinize eğiten bir görüntü kümesi. GitHub üzerinde [örnek görüntüler](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/tree/master/samples/vision/images) kümesini kullanabilirsiniz. Ya da aşağıdaki ipuçlarını kullanarak kendi görüntülerinizi seçebilirsiniz.

## <a name="create-custom-vision-resources"></a>Özel Görüntü İşleme kaynakları oluşturma

[!INCLUDE [create-resources](includes/create-resources.md)]

## <a name="create-a-new-project"></a>Yeni proje oluşturma

Web tarayıcınızda [özel görüntü işleme Web sayfasına](https://customvision.ai) gidin ve __oturum aç '__ ı seçin. Azure portal oturum açmak için kullandığınız hesapla oturum açın.

![Oturum açma sayfasının görüntüsü](./media/browser-home.png)


1. İlk projenizi oluşturmak için **Yeni proje**' yi seçin. **Yeni proje oluştur** iletişim kutusu görüntülenir.

    ![Yeni proje iletişim kutusunda ad, açıklama ve etki alanları için alanlar bulunur.](./media/get-started-build-detector/new-project.png)

1. Proje için bir ad ve açıklama girin. Ardından bir kaynak grubu seçin. Oturum açmış hesabınız bir Azure hesabıyla ilişkiliyse, kaynak grubu açılır listesi bir Özel Görüntü İşleme Hizmeti kaynağı içeren tüm Azure Kaynak gruplarınızı görüntüler. 

   > [!NOTE]
   > Kullanılabilir kaynak grubu yoksa, lütfen [Azure Portal](https://portal.azure.com/)oturum açmak için kullandığınız hesapla [customvision.ai](https://customvision.ai) oturum açtığınızdan emin olun. Ayrıca, lütfen Özel Görüntü İşleme kaynaklarınızın bulunduğu Azure portal dizin olarak Özel Görüntü İşleme Web sitesinde aynı "Dizin" i seçtiğinizi onaylayın. Her iki sitede, ekranın sağ üst köşesindeki açılan hesap menüsünden dizininizi seçebilirsiniz. 

1. __Proje türleri__ altında __nesne algılamayı__ seçin.

1. Sonra, kullanılabilir etki alanlarından birini seçin. Her etki alanı, aşağıdaki tabloda açıklandığı gibi belirli görüntü türleri için algılayıcısının en iyi duruma getirir. İsterseniz, etki alanını daha sonra değiştirebileceksiniz.

    |Etki alanı|Amaç|
    |---|---|
    |__Genel__| Çok çeşitli nesne algılama görevleri için iyileştirilmiştir. Diğer etki alanlarından hiçbiri uygun değilse veya hangi etki alanının seçeceğinizden emin değilseniz, genel etki alanını seçin. |
    |__Logo__|Görüntülerde marka logolarını bulmak için iyileştirildi.|
    |__Rafların ürünleri__|Rafların ürünlerini algılamak ve sınıflandırmak için iyileştirildi.|
    |__Compact etki alanları__| Mobil cihazlarda gerçek zamanlı nesne algılama kısıtlamaları için iyileştirildi. Compact etki alanları tarafından oluşturulan modeller yerel olarak çalışacak şekilde aktarılabilir.|

1. Son olarak, __proje oluştur__' u seçin.

## <a name="choose-training-images"></a>Eğitim görüntülerini seçin

[!INCLUDE [choose training images](includes/choose-training-images.md)]

## <a name="upload-and-tag-images"></a>Görüntüleri karşıya yükleme ve etiketleme

Bu bölümde, algılayıcısının eğitilmesi için görüntüleri karşıya yükleyecek ve el ile etiketlemelisiniz. 

1. Görüntü eklemek için __Görüntü ekle__ ' yi seçin ve ardından __yerel dosyalara gözatamazsınız__' ı seçin. Görüntüleri karşıya yüklemek için __Aç__ ' ı seçin.

    ![Resim ekle denetimi, sol üst köşede ve alt ortadaki bir düğme olarak gösterilir.](./media/get-started-build-detector/add-images.png)

1. Karşıya yüklenen görüntülerinizi, Kullanıcı arabiriminin **etiketlenmemiş** bölümünde görürsünüz. Bir sonraki adım, algılayıcısının tanıması için öğrenimini istediğiniz nesneleri el ile etiketleyecaktır. Etiketleme iletişim penceresini açmak için ilk resme tıklayın. 

    ![Karşıya yüklenen görüntüler, etiketlenmemiş bölüm](./media/get-started-build-detector/images-untagged.png)

1. Görüntinizdeki nesnenin etrafına bir dikdörtgeni tıklatın ve sürükleyin. Ardından, düğme ile yeni bir etiket adı girin **+** veya açılan listeden varolan bir etiketi seçin. Algılayıcı, eğitiminde negatif bir örnek olarak etiketlenmemiş arka plan alanını kullandığından, algılamak istediğiniz nesnelerin her örneğinin etiketedilmesi önemlidir. Etiketlerinizi tamamladığınızda, sağ taraftaki oka tıklayarak etiketlerinizi kaydedin ve sonraki görüntüde geçiş yapın.

    ![Bir nesneyi dikdörtgen seçim ile etiketleme](./media/get-started-build-detector/image-tagging.png)

Başka bir görüntü kümesini karşıya yüklemek için bu bölümün üst kısmına dönün ve adımları tekrarlayın.

## <a name="train-the-detector"></a>Algılayıcısı eğitme

Algılayıcı modelini eğiteetmek için **eğitme** düğmesini seçin. Algılayıcı, her etiketli nesneyi tanımlayan bir model oluşturmak için tüm geçerli görüntüleri ve bunların etiketlerini kullanır.

![Web sayfasının başlık araç çubuğunun sağ üst köşesindeki eğitme düğmesi](./media/getting-started-build-a-classifier/train01.png)

Eğitim süreci yalnızca birkaç dakika sürer. Bu süre boyunca, eğitim işlemiyle ilgili bilgiler **performans** sekmesinde görüntülenir.

![Ana bölümde eğitim iletişim kutusuyla tarayıcı penceresi](./media/get-started-build-detector/training.png)

## <a name="evaluate-the-detector"></a>Algılayıcısının değerlendirilmesi

Eğitim tamamlandıktan sonra modelin performansı hesaplanır ve görüntülenir. Özel Görüntü İşleme Hizmeti, duyarlılık, geri çağırma ve ortalama ortalama duyarlık hesaplamak için eğitim için gönderdiğiniz görüntüleri kullanır. Duyarlık ve geri çağırma, bir algılayıcısının verimliliğinden oluşan iki farklı ölçümlerdir:

- **Duyarlık** , doğru olan belirlenen sınıflandırmaların kesirini gösterir. Örneğin, model, 100 görüntüsünü köpekler olarak tanımlarsa ve bunların 99 ' i gerçekten dotalar ise, duyarlık %99% olur.
- **Geri çağırma** , doğru şekilde tanımlanan gerçek sınıflandırmaların kesirini belirtir. Örneğin, gerçekten de 100 resim ve bir model, her ikisi de 80 olarak tanımlanmış ise, geri çekme %80% olur.
- Ortalama **duyarlık** , ortalama Duyarlığın ortalama DEĞERIDIR (AP). AP duyarlık/geri çağırma eğrisinin (her tahmin için geri çağırmaya yönelik olarak çizilen duyarlık) altındaki alandır.

![Eğitim sonuçları, genel duyarlık ve geri çağırma ve ortalama duyarlık hassasiyetini gösterir.](./media/get-started-build-detector/trained-performance.png)

### <a name="probability-threshold"></a>Olasılık eşiği

[!INCLUDE [probability threshold](includes/probability-threshold.md)]

### <a name="overlap-threshold"></a>Çakışma eşiği

**Çakışma eşiği** kaydırıcısı, eğitim bölümünde "doğru" olarak kabul edilmesi gereken bir nesne tahmininin nasıl doğru düşünüldüğiyle ilgilenir. Tahmin edilen nesne sınırlayıcı kutusu ile Kullanıcı tarafından girilen gerçek sınırlayıcı kutusu arasında izin verilen minimum örtüşme süresi belirler. Sınırlayıcı kutular bu derecede çakışmazsa, tahmin doğru kabul edilmez.

## <a name="manage-training-iterations"></a>Eğitim yinelemelerini yönetme

Algılayıcısının her eğişinizde, kendi güncelleştirilmiş performans ölçümleriyle yeni bir _yineleme_ oluşturursunuz. Tüm yinelemelerinizi **performans** sekmesinin sol bölmesinde görüntüleyebilirsiniz. Sol bölmede, bir yinelemeyi silmek için kullanabileceğiniz **Sil** düğmesini de bulabilirsiniz. Bir yinelemeyi sildiğinizde, onunla benzersiz olarak ilişkili olan tüm görüntüleri silersiniz.

Eğitilen modellerinize programlı bir şekilde erişme hakkında bilgi edinmek için bkz. [modelinizi tahmın API 'Siyle kullanma](./use-prediction-api.md) .

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Özel Görüntü İşleme Web sitesini kullanarak bir nesne algılayıcı modeli oluşturmayı ve eğiteyi öğrendiniz. Daha sonra, modelinizi iyileştirmeye yönelik yinelemeli süreç hakkında daha fazla bilgi alın.

> [!div class="nextstepaction"]
> [Modeli test etme ve yeniden eğitme](test-your-model.md)

* [Özel Görüntü İşleme nedir?](./overview.md)
