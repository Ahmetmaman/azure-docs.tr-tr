---
title: Otomatik ML sınıflandırma modelleri oluşturma
titleSuffix: Azure Machine Learning
description: Sınıflandırma modellerini Azure Machine Learning otomatik makine öğrenimi (otomatik ML) arabirimiyle nasıl eğeceğinizi &.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: cartacioS
ms.author: sacartac
ms.reviewer: nibaccam
ms.date: 12/21/2020
ms.custom: automl
ms.openlocfilehash: 90c827774f38f07b9791a6399a53b0304bbe28c8
ms.sourcegitcommit: b6267bc931ef1a4bd33d67ba76895e14b9d0c661
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2020
ms.locfileid: "97695197"
---
# <a name="tutorial-create-a-classification-model-with-automated-ml-in-azure-machine-learning"></a>Öğretici: Azure Machine Learning otomatik ML ile sınıflandırma modeli oluşturma


Bu öğreticide, Azure Machine Learning Studio 'da otomatik makine öğrenimi kullanarak tek bir kod satırı yazmadan basit bir sınıflandırma modeli oluşturmayı öğreneceksiniz. Bu sınıflandırma modeli, bir istemci bir mali kurum ile sabit bir havale 'e abone olurken tahmin edilir.

Otomatik makine öğrenimi sayesinde yoğun zamanda yoğun görevleri otomatik hale getirebilirsiniz. Otomatikleştirilmiş makine öğrenimi, seçtiğiniz başarı ölçümünü temel alarak en iyi modeli bulmanıza yardımcı olmak üzere birçok algoritma ve hiper parametre kombinasyonu üzerinde hızlı bir şekilde yinelenir.

Bir zaman serisi tahmin örneği için bkz. [öğretici: talep tahmini &](tutorial-automated-ml-forecast.md).

Bu öğreticide, aşağıdaki görevleri nasıl gerçekleştireceğinizi öğreneceksiniz:

> [!div class="checklist"]
> * Azure Machine Learning çalışma alanı oluşturun.
> * Otomatik makine öğrenmesi denemesi çalıştırma.
> * Deneme ayrıntılarını görüntüleyin.
> * Modeli dağıtma.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://aka.ms/AMLFree) oluşturun.

* [**bankmarketing_train.csv**](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) veri dosyasını indirin. **Y** sütunu, bir müşterinin, daha sonra bu öğreticide tahmine yönelik hedef sütun olarak tanımlanan sabit bir dönem yatırma abone olup olmadığını gösterir. 

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Azure Machine Learning çalışma alanı, bulutta makine öğrenimi modellerini denemek, eğmek ve dağıtmak için kullandığınız temel bir kaynaktır. Azure aboneliğiniz ve kaynak grubunuz, hizmette kolayca tüketilen bir nesne ile aynı olur. 

[Çalışma alanı oluşturmanın birçok yolu](how-to-manage-workspace.md)vardır. Bu öğreticide, Azure kaynaklarınızı yönetmek için Web tabanlı bir konsol olan Azure portal bir çalışma alanı oluşturursunuz.

[!INCLUDE [aml-create-portal](../../includes/aml-create-in-portal.md)]

>[!IMPORTANT] 
> **Çalışma alanınızı** ve **aboneliğinizi** bir yere göz atın. Denemenizin doğru yerde oluşturulmasını sağlamak için bunlara ihtiyacınız olacaktır. 

## <a name="get-started-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio 'da çalışmaya başlama

Aşağıdaki deneme kurulumunu tamamlayıp, https://ml.azure.com tüm beceri seviyelerinin veri bilimi senaryolarına yönelik veri bilimi senaryoları gerçekleştirmek için Machine Learning araçları 'nı içeren birleştirilmiş bir Web arabirimi olan Azure Machine Learning Studio aracılığıyla adımları gerçekleştirin. Studio, Internet Explorer tarayıcılarında desteklenmez.

1. [Azure Machine Learning Studio](https://ml.azure.com)'da oturum açın.

1. Aboneliğinizi ve oluşturduğunuz çalışma alanını seçin.

1. **Kullanmaya** başlayın ' ı seçin.

1. Sol bölmede **Yazar** bölümü altında **Otomatik ml** ' yi seçin.

   Bu ilk otomatik ML denemenize ait olduğundan, boş bir liste ve belge bağlantıları görürsünüz.

   ![Başlarken sayfası](./media/tutorial-first-experiment-automated-ml/get-started.png)

1. **+ Yeni OTOMATIK ml Çalıştır**' ı seçin. 

## <a name="create-and-load-dataset"></a>Veri kümesi oluşturma ve yükleme

Denemenizi yapılandırmadan önce, veri dosyanızı Azure Machine Learning veri kümesi biçiminde çalışma alanınıza yükleyin. Bunun yapılması, verilerinizin denemenize uygun şekilde biçimlendirildiğinden emin olmanızı sağlar.

1. **+ Veri kümesi oluştur** açılır listesinden **yerel dosyalardan** seçim yaparak yeni bir veri kümesi oluşturun. 

    1. **Temel bilgi** formunda, veri kümenize bir ad verin ve isteğe bağlı bir açıklama sağlayın. Otomatik ML arabirimi şu anda yalnızca Tabulardataset 'leri desteklediğinden veri kümesi türü *tablosal* olmalıdır.

    1. Sol alt kısımdaki **İleri ' yi** seçin

    1. **Veri deposu ve dosya seçimi** formunda, çalışma alanı oluşturma, çalışma alanı **BlobStore (Azure Blob depolama)** sırasında otomatik olarak ayarlanan varsayılan veri deposunu seçin. Bu, çalışma alanınız için kullanılabilir hale getirmek üzere veri dosyanızı karşıya yükleyeceksiniz.

    1. **Gözat**'ı seçin.
    
    1. Yerel bilgisayarınızda **bankmarketing_train.csv** dosyasını seçin. Bu, bir [Önkoşul](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv)olarak indirdiğiniz dosyadır.

    1. Veri kümenize benzersiz bir ad verin ve isteğe bağlı bir açıklama sağlayın. 

    1. Çalışma alanı oluşturma sırasında otomatik olarak ayarlanan varsayılan kapsayıcıya yüklemek için sol alt kısımdaki **İleri** ' yi seçin.  
    
       Karşıya yükleme tamamlandığında, ayarlar ve önizleme formu dosya türüne göre önceden doldurulur. 
       
    1. **Ayarlar ve önizleme** formunun aşağıdaki gibi doldurulduğunu doğrulayın ve **İleri ' yi** seçin.
        
        Alan|Açıklama| Öğretici için değer
        ---|---|---
        Dosya biçimi|Bir dosyada depolanan verilerin yerleşimini ve türünü tanımlar.| Ted
        Sınırlayıcı|&nbsp;Düz metin veya diğer veri akışlarında ayrı, bağımsız bölgeler arasındaki sınırı belirtmek için bir veya daha fazla karakter. |Virgül
        Encoding|Veri kümenizi okumak için kullanılacak bit karakter şeması tablosunu belirler.| UTF-8
        Sütun başlıkları| Veri kümesinin üst bilgilerinin (varsa) nasıl değerlendirileceğini gösterir.| Tüm dosyaların aynı üst bilgileri var
        Satırları atla | Veri kümesinde kaç tane, ne varsa satırların atlandığını gösterir.| Hiçbiri

    1. **Şema** formu, bu deneme için verilerinizin daha fazla yapılandırılmasını sağlar. Bu örnekte, hiçbir seçim yapmayız. **İleri**’yi seçin.

    1. **Ayrıntıları Onayla** formunda, bilgilerin daha önce **temel bilgiler, veri deposu ve dosya seçimi** ve **Ayarlar ve önizleme** formlarında doldurulduğu ile eşleştiğini doğrulayın.
    
    1. Veri kümenizin oluşturulmasını gerçekleştirmek için **Oluştur** ' u seçin.
    
    1. Listede göründükten sonra veri kümenizi seçin.
    
    1. **Day_of_week** dahil etmediğinizden emin olmak için **veri önizlemeyi** gözden geçirin ve ardından **Kapat**' ı seçin.

    1. **İleri ' yi** seçin.

## <a name="configure-run"></a>Çalıştırmayı Yapılandır

Verilerinizi yükleyip yapılandırdıktan sonra, denemenizin kurulumunu yapabilirsiniz. Bu kurulum, işlem ortamınızın boyutunu seçme ve tahmin etmek istediğiniz sütunu belirtme gibi tasarım görevlerini içerir. 

1. Yeni radyo **Oluştur** düğmesini seçin.

1. **Yapılandırma çalıştırması** formunu aşağıdaki gibi doldurun:
    1. Bu deneme adını girin: `my-1st-automl-experiment`

    1. Hedef sütun olarak **y** ' yi, ne tahmin etmek istediğinizi seçin. Bu sütun, istemcinin yatırma bir terime abone olup olmadığını gösterir.
    
    1. **+ Yeni Işlem oluştur** ' u seçin ve işlem hedefini yapılandırın. İşlem hedefi, eğitim betiğinizi çalıştırmak veya hizmet dağıtımınızı barındırmak için kullanılan yerel veya bulut tabanlı bir kaynak ortamıdır. Bu deneme için bulut tabanlı bir işlem kullanıyoruz. 
        1. İşlem ortamınızı ayarlamak için **sanal makine** formunu doldurun.

            Alan | Açıklama | Öğretici için değer
            ----|---|---
            Sanal &nbsp; makine &nbsp; önceliği |Denemenizin hangi önceliğe sahip olması gerektiğini seçin| Ayrılmış
            Sanal &nbsp; makine &nbsp; türü| İşlem için sanal makine türünü seçin.|CPU (Merkezi Işlem birimi)
            Sanal &nbsp; makine &nbsp; boyutu| İşlem için sanal makine boyutunu seçin. Önerilen boyutların listesi verilerinize ve deneme türüne göre belirlenir. |Standard_DS12_V2
        
        1. **Yapılandırma ayarları formunu** doldurmak için **İleri ' yi** seçin.
        
            Alan | Açıklama | Öğretici için değer
            ----|---|---
            İşlem adı |  İşlem bağlamını tanımlayan benzersiz bir ad. | Oto ml-işlem
            En az/en fazla düğüm| Veri profili için, 1 veya daha fazla düğüm belirtmeniz gerekir.|En az düğümler: 1<br>En fazla düğüm: 6
            Ölçeği ölçeklendirmeye başlamadan önce boşta geçen Saniyeler | Küme otomatik olarak en düşük düğüm sayısına ölçeklendirildiğinde boşta geçen süre.|120 (varsayılan)
            Gelişmiş ayarlar | Denemeniz için bir sanal ağ yapılandırma ve yetkilendirme ayarları.| Hiçbiri               

        1. İşlem hedefini oluşturmak için **Oluştur** ' u seçin. 

            **Bu, tamamlanacak birkaç dakika sürer.** 

             ![Ayarlar sayfası](./media/tutorial-first-experiment-automated-ml/compute-settings.png)

        1. Oluşturulduktan sonra, açılan listeden yeni işlem hedefini seçin.

    1. **İleri**’yi seçin.

1. **Görev türü ve ayarlar** formunda, Machine Learning görev türünü ve yapılandırma ayarlarını BELIRTEREK otomatikleştirilmiş ml denemenizin kurulumunu doldurun.
    
    1.  Makine öğrenimi görev türü olarak **Sınıflandırmayı** seçin.

    1. **Ek yapılandırma ayarlarını görüntüle** ' yi seçin ve alanları aşağıdaki gibi doldurun. Bu ayarlar, eğitim işini daha iyi denetliyor. Aksi takdirde, denemeler seçimine ve verilerine göre varsayılan ayarlar uygulanır.

        Ek &nbsp; yapılandırma|Açıklama|&nbsp;Öğretici için &nbsp; değer
        ------|---------|---
        Birincil ölçüm| Makine öğrenimi algoritmasının ölçülecek değerlendirme ölçümü.|AUC_weighted
        En iyi modeli açıkla| Otomatik ML tarafından oluşturulan en iyi modelde explainability 'yi otomatik olarak gösterir.| Etkinleştir
        Engellenen algoritmalar | Eğitim işinden dışlamak istediğiniz algoritmalar| Hiçbiri
        Çıkış ölçütü| Bir kriterle karşılanırsa eğitim işi durdurulur. |Eğitim &nbsp; işi &nbsp; süresi (saat): 1 <br> Ölçüm &nbsp; puan &nbsp; eşiği: yok
        Doğrulama | Çapraz doğrulama türü ve test sayısı seçin.|Doğrulama türü:<br>&nbsp;&nbsp;çapraz doğrulamayı yana kesme <br> <br> Doğrulama sayısı: 2
        Eşzamanlılık| Yineleme başına yürütülen en fazla paralel yineleme sayısı| En fazla &nbsp; eşzamanlı &nbsp; yineleme: 5
        
        **Kaydet**’i seçin.
    
    1. **Görünüm özelliği ayarları**' nı seçin. Bu örnekte, **day_of_week** özelliği için geçiş anahtarını seçin. bu nedenle, bu denemede uygun hale getirme için dahil edilmez.

        ![Korleştirme seçimi](./media/tutorial-first-experiment-automated-ml/featurization-setting-config.gif)   
 
        **Kaydet**’i seçin.

1. Denemeyi çalıştırmak için **son** ' u seçin. **Çalışma ayrıntısı** ekranı, deneme hazırlığı başladığında en üstteki **çalıştırma durumuyla** birlikte açılır. Deneme ilerledikçe bu durum güncellenir. Deneme larınızın durumunu bilgilendirmek için Ayrıca, Studio 'nun sağ üst köşesinde bildirimler görüntülenir.

>[!IMPORTANT]
> Hazırlık, deneme çalıştırmasının hazırlanmasına **10-15 dakika** sürer.
> Çalışmaya başladıktan sonra, **her yinelemede 2-3 dakika daha** sürer.  <br> <br>
> Üretimde, büyük olasılıkla biraz daha fazla yol göstereceğiz. Ancak bu öğreticide, diğer kullanıcılar çalışmaya devam ederken, **modeller** sekmesinde sınanan algoritmaları keşfetmeye başlayacağız. 

##  <a name="explore-models"></a>Modelleri keşfet

Test edilen algoritmaları (modeller) görmek için **modeller** sekmesine gidin. Modeller, varsayılan olarak, tamamlandığı gibi ölçüm puanına göre sıralanır. Bu öğretici için, seçili **AUC_weighted** ölçüsüne göre en yüksek düzeyde puan veren model listenin en üstünde yer alır.

Deneme modellerinin tümünün bitmesini beklerken, performans ayrıntılarını araştırmak için tamamlanmış bir modelin **algoritma adını** seçin. 

Aşağıdakiler, seçili modelin özelliklerini, ölçümlerini ve performans grafiklerini görüntülemek için **Ayrıntılar** ve **ölçümler** sekmelerinde gezinir. 

![Yineleme ayrıntısı Çalıştır](./media/tutorial-first-experiment-automated-ml/run-detail.gif)

## <a name="deploy-the-best-model"></a>En iyi modeli dağıtma

Otomatik makine öğrenimi arabirimi, birkaç adımda Web hizmeti olarak en iyi modeli dağıtmanıza olanak tanır. Dağıtım, yeni verileri tahmin etmek ve potansiyel fırsat bölgelerini belirlemek için modelin Tümleştirmesidir. 

Bu deneme için, bir Web hizmetine dağıtım, artık, potansiyel sabit depozito müşterilerinin tanımlanmasından sonra, mali kurumda yinelenen ve ölçeklenebilir bir Web çözümüne sahip olduğu anlamına gelir. 

Denemenizin tamamlanıp tamamlanmamalarınızın olup olmadığını denetleyin. Bunu yapmak için ekranın en üstünde **1. Çalıştır** ' ı seçerek üst çalışma sayfasına gidin. Ekranın sol üst kısmında **tamamlanmış** bir durum gösterilir. 

Deneme çalıştırması tamamlandıktan sonra, **Ayrıntılar** sayfası **en iyi model Özeti** bölümü ile doldurulur. Bu deneme bağlamında, **Votingensediskte** **AUC_weighted** ölçüsüne göre en iyi model kabul edilir.  

Bu modeli dağıyoruz ancak yapmanız önerilir, dağıtımın tamamlaması yaklaşık 20 dakika sürer. Dağıtım işlemi, modeli kaydetme, kaynakları oluşturma ve bunları Web hizmeti için yapılandırma dahil olmak üzere birkaç adım gerektirir.

1. Modele özgü sayfayı açmak için **Votingensebir** öğesini seçin.

1. Sol üstteki **Dağıt** düğmesini seçin.

1. **Model dağıt** bölmesini aşağıdaki gibi doldurun:

    Alan| Değer
    ----|----
    Dağıtım adı| My-Oto ml-Deploy
    Dağıtım açıklaması| İlk otomatik makine öğrenimi deneme dağıtımı
    İşlem türü | Azure Işlem örneği (acı) seçin
    Kimlik doğrulamayı etkinleştir| Dıı. 
    Özel dağıtımlar kullanın| Dıı. Varsayılan sürücü dosyası (Puanlama betiği) ve ortam dosyasının yeniden oluşturulmasına izin verir. 
    
    Bu örnekte, *Gelişmiş* menüsünde belirtilen Varsayılanları kullanırız. 

1. **Dağıt**'ı seçin.  

    **Çalıştır** ekranının üst kısmında yeşil başarı iletisi görünür ve **model Özeti** bölmesinde **dağıtım durumu** altında bir durum iletisi görüntülenir. Dağıtım durumunu denetlemek için düzenli aralıklarla **Yenile** ' yi seçin.
    
Artık tahminleri oluşturmak için işlemsel bir Web hizmetiniz vardır. 

Yeni Web hizmetinizi kullanma hakkında daha fazla bilgi edinmek için [**sonraki adımlara**](#next-steps) ilerleyin ve Power BI yerleşik Azure Machine Learning desteğini kullanarak tahminlerinizi test edin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Dağıtım dosyaları veri ve deneme dosyalarından daha büyüktür, bu nedenle daha fazla depolama alanı maliyetlidir. Hesap maliyetlerini en aza indirmek için yalnızca dağıtım dosyalarını silin veya çalışma alanınızı ve deneme dosyalarını korumak istiyorsanız. Aksi takdirde, herhangi bir dosyayı kullanmayı planlamıyorsanız tüm kaynak grubunu silin.  

### <a name="delete-the-deployment-instance"></a>Dağıtım örneğini silme

\/Diğer öğreticiler ve araştırmayla ilgili kaynak grubunu ve çalışma alanını tutmak istiyorsanız, yalnızca https:/ml.Azure.com/konumundaki Azure Machine Learning dağıtım örneğini silin. 

1. [Azure Machine Learning](https://ml.azure.com/)gidin. Çalışma alanınıza gidin ve **varlıklar** bölmesinin sol tarafında **uç noktalar**' ı seçin. 

1. Silmek istediğiniz dağıtımı seçin ve **Sil**' i seçin. 

1. **Devam**' ı seçin.

### <a name="delete-the-resource-group"></a>Kaynak grubunu silme

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu otomatik makine öğrenimi öğreticisinde, bir sınıflandırma modeli oluşturmak ve dağıtmak için Azure Machine Learning otomatik ML arabirimini kullandınız. Daha fazla bilgi ve sonraki adımlar için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Bir web hizmetini kullanma](https://docs.microsoft.com/power-bi/connect-data/service-aml-integrate?context=azure/machine-learning/context/ml-context)

+ [Otomatik makine öğrenimi](concept-automated-ml.md)hakkında daha fazla bilgi edinin.
+ Sınıflandırma ölçümleri ve grafikler hakkında daha fazla bilgi için bkz. [otomatik makine öğrenimi sonuçlarını anlama](how-to-understand-automated-ml.md) makalesi.
+ [Korleştirme](how-to-configure-auto-features.md#featurization)hakkında daha fazla bilgi edinin.
+ [Veri profili oluşturma](how-to-connect-data-ui.md#profile)hakkında daha fazla bilgi edinin.


>[!NOTE]
> Bu banka pazarlama veri kümesi, [Creative Commons (CCO: genel etki alanı) lisansı](https://creativecommons.org/publicdomain/zero/1.0/)altında kullanılabilir hale getirilir. Veritabanının bireysel içeriklerinde her türlü hak, [veritabanı Içeriği lisansı](https://creativecommons.org/publicdomain/zero/1.0/) kapsamında lisanslanır ve [kada](https://www.kaggle.com/janiobachmann/bank-marketing-dataset)mevcuttur. Bu veri kümesi, ilk olarak [UCI Machine Learning veritabanı](https://archive.ics.uci.edu/ml/datasets/bank+marketing)dahilinde kullanılabilir.<br><br>
> [Moro et al., 2014] S. Moro, P. Cortez ve P. Rita. A Data-Driven Approach to Predict the Success of Bank Telemarketing. Decision Support Systems, Elsevier, 62:22-31, Haziran 2014.
