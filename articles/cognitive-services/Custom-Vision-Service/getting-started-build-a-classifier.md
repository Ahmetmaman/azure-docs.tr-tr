---
title: Custom Vision Service'e - sınıflandırıcı oluşturma
titlesuffix: Azure Cognitive Services
description: Özel görüntü işleme Web sitesi bir görüntü sınıflandırma modeli oluşturmak için kullanmayı öğrenin.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: anroth
ms.openlocfilehash: 088ca29a2b10845730e986995256eed99caa7401
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55210231"
---
# <a name="how-to-build-a-classifier-with-custom-vision"></a>Özel görüntü ile bir sınıflandırıcı oluşturma

Custom Vision Service'e görüntü sınıflandırması için kullanılacak bir sınıflandırıcı modeli oluşturmalısınız. Bu kılavuzda, özel görüntü işleme Web sitesi üzerinden sınıflandırıcı oluşturma öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

- Geçerli bir [Microsoft hesabı](https://account.microsoft.com/account) veya bir Azure Active Directory (AAD) hesabı ("iş veya Okul hesabı").

    > [!IMPORTANT] 
    > Oturum açma AAD kullanıcıları için [Microsoft Ulusal Bulutlar](https://www.microsoft.com/en-us/trustcenter/cloudservices/nationalcloud) şu anda desteklenmiyor.
- Sınıflandırıcınızı eğitmek görüntüleri bir dizi. Görüntüleri seçme hakkında ipuçları için aşağıya bakın.
- İsteğe bağlı olarak: Microsoft hesabınızı veya AAD hesabı ile ilişkili bir Azure aboneliği. Azure aboneliğiniz yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) başlamadan önce. Bir Azure aboneliği, yalnızca iki oluşturmak mümkün olmayacak __sınırlı deneme__ projeleri.

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

Web tarayıcınızda gidin [Custom Vision web sayfası](https://customvision.ai) seçip __oturum__.

![Oturum açma sayfasının görüntüsü](./media/browser-home.png)

Bir Azure hesabınız varsa, özel görüntü işleme hizmeti eğitim ve tahmin kaynakları oluşturmak için istenir [Azure portalında](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision) proje oluşturma sırasında.

1. İlk projenizi oluşturmak için Seç **yeni proje**. **Yeni proje oluştur** iletişim kutusu görüntülenir.

    ![Yeni Proje iletişim kutusu, adını, açıklamasını ve etki alanları için alanlar içerir.](./media/getting-started-build-a-classifier/new-project.png)

1. Bir ad ve proje için bir açıklama girin. Ardından, bir kaynak grubu seçin. Oturum açtığınız hesabınız bir Azure hesabı ile ilişkili ise, kaynak grubu açılan bir özel görüntü işleme hizmeti kaynağı içeren Azure kaynak gruplarınızdaki tüm görüntüler. Her iki durumda da seçebilirsiniz __sınırlı deneme__ bu açılır listesinden.

1. Seçin __sınıflandırma__ altında __proje türleri__. Ardından, altında __sınıflandırma türleri__, seçin ya da **Multilabel** veya **veya çoklu sınıflar**kullanım Örneğinize bağlı olarak. (Gönderdiğiniz her resim büyük olasılıkla etiketine sıralanacağını) tek kategorilere görüntüleri sınıflı sınıflandırma sıralar sırada multilabel sınıflandırma etiketleri herhangi bir sayıda görüntüye (sıfır veya daha fazla) uygulanır. İsterseniz daha sonra sınıflandırma türünü değiştirmek mümkün olacaktır.

1. Ardından, mevcut etki alanlarından birini seçin. Aşağıdaki tabloda açıklandığı gibi her etki alanı görüntüleri, belirli türde bir sınıflandırıcı iyileştirir. İsterseniz daha sonra etki alanını değiştirmek mümkün olacaktır.

    |Domain|Amaç|
    |---|---|
    |__Genel__| Çok sayıda görüntü sınıflandırma görevleri için en iyi duruma getirilmiş. Diğer etki alanlarıyla uygun yok ya da seçmek için hangi etki alanı emin değilseniz, genel etki alanını seçin. |
    |__Gıda__|Bir restoran menüsünde göreceğiniz şekilde çanakları fotoğraflarını için en iyi duruma getirilmiş. Bireysel MEYVELERİ veya et fotoğraflarını sınıflandırma istiyorsanız, Yemek etki alanını kullanın.|
    |__Yer işareti__|Tanınabilir için yer işareti, doğal ve yapay en iyi duruma getirilmiş. Yer işareti fotoğraf açıkça görünür olduğunda bu etki alanı en iyi şekilde çalışır. Bu etki alanında yer işareti biraz önündeki kişiler tarafından engellendiği bile çalışır.|
    |__Perakende__|Bir alışveriş katalog veya alışveriş Web sitesinde bulunan görüntüleri için en iyi duruma getirilmiş. Elbiselerini pants ve gömlekler arasında yüksek duyarlık sınıflandırmak istiyorsanız, bu etki alanını kullanın.|
    |__Yetişkin__|Yetişkinlere yönelik içerik ve yetişkin olmayan içerik daha iyi tanımlamak için en iyi duruma getirilmiş. Örneğin, görüntüleri bathing cins insanların engellemek istiyorsanız, bu etki alanı, bunu yapmak için özel bir sınıflandırıcı oluşturmanıza olanak sağlar.|
    |__Compact etki alanları__| Mobil cihazlarda gerçek zamanlı sınıflandırma kısıtlamaları için en iyi duruma getirilmiş. Compact etki alanları tarafından oluşturulan modelleri, yerel olarak çalıştırmak için verilebilir.|
    
1. Son olarak, seçin __proje oluştur__.

## <a name="choose-training-images"></a>Eğitim resmi seçin

Minimum olarak ilk eğitimi kümesinde etiket başına en az 30 görüntüleri kullanmanızı öneririz. Bunu eğitildi sonra modelinizi test etmek için birkaç fazladan görüntüleri toplamak isteyebilirsiniz.

Etkili bir şekilde, modeli eğitmek için görsel çeşitli ile görüntüleri kullanın. Seçim, görüntülerle göre değişiklik gösterir:
* Kamera Açısı
* aydınlatma
* Arka plan
* Görsel stili
* kişi ve gruplandırılmış subject(s)
* boyut
* type

Ayrıca, tüm eğitim görüntülerinizin aşağıdaki ölçütleri karşıladığından emin olun:
* .jpg, .png veya .bmp biçimi
* en fazla 6MB boyutunda (öngörü görüntülerini 4MB)
* kısa kenarı en az 256 piksel; Bu değerden daha kısa herhangi bir görüntü otomatik olarak özel görüntü işleme hizmeti tarafından ölçeklendirileceği

## <a name="upload-and-tag-images"></a>Görüntüleri karşıya yükleme ve etiketleme

Bu bölümde karşıya yükleme ve sınıflandırıcı eğitmenize yardımcı olmak için görüntüleri el ile etiketleyin. 

1. Görüntüleri eklemek için tıklatın __görüntüleri ekleme__ düğmesine ve ardından __yerel dosyalara Gözat__. Seçin __açık__ etiketleme için taşınır. Etiket seçiminizi istenen etiketlerine göre farklı gruplardaki görüntüleri karşıya yüklemek kolaydır, bu nedenle, karşıya yüklemek için seçtiğiniz görüntüleri grubunun uygulanır. Karşıya yüklediğiniz sonra da ayrı görüntüleri için etiketleri değiştirebilirsiniz.

    ![Denetim Ekle görüntüleri, sol üst ve alt merkezinde düğme olarak gösterilir.](./media/getting-started-build-a-classifier/add-images01.png)


1. Bir etiket oluşturmak için metin girin __My etiketleri__ alanına girin ve Enter tuşuna basın. Etiket zaten varsa, açılan menüde görünür. Multilabel bir projede birden fazla etiket görüntülerinizin ekleyebilirsiniz, ancak bir çok sınıflı projesinde yalnızca bir tane ekleyebilirsiniz. Görüntüleri karşıya yükleme işlemini tamamlamak için kullanmak __[sayı] dosyaları karşıya yükleme__ düğmesi. 

    ![Etiket ve karşıya yükleme sayfasının görüntüsü](./media/getting-started-build-a-classifier/add-images03.png)

1. Seçin __Bitti__ görüntüleri karşıya yüklendikten sonra.

    ![İlerleme çubuğu, tamamlanan tüm görevleri gösterir.](./media/getting-started-build-a-classifier/add-images04.png)

Başka bir dizi görüntüleri karşıya yüklemek için bu bölümde dön ve adımları yineleyin. Projenizdeki belirli bir noktada eklemeniz gerekebilir _negatif örnekleri_ sınıflandırıcınızı daha doğru hale getirilmesine yardımcı olacak. Negatif örnekleri diğer etiketlerden herhangi birini eşleşmeyen olanlardır. Bu görüntüleri karşıya yüklediğinizde, özel uygulama **negatif** için bunları etiketleyin.

## <a name="train-the-classifier"></a>Sınıflandırıcıyı eğitme

Sınıflandırıcı eğitmek için seçin **eğitme** düğmesi. Sınıflandırıcı, tüm geçerli görüntüleri her etiketin visual kalitelerini tanımlayan bir model oluşturmak için kullanır.

![Üst eğit düğmesine web sayfasının üst araç çubuğunun sağ](./media/getting-started-build-a-classifier/train01.png)

Eğitim işlemini yalnızca birkaç dakika sürer. Eğitim süreci hakkında bilgi görüntülenir bu süre boyunca **performans** sekmesi.

![Tarayıcı penceresini ana bölümünde bir eğitim iletişim kutusu](./media/getting-started-build-a-classifier/train02.png)

## <a name="evaluate-the-classifier"></a>Sınıflandırıcı değerlendir

Alıştırma tamamlandıktan sonra modelin performans tahmini ve görüntülenir. Custom Vision Service'e duyarlık ve geri çağırırsanız, bir işlem kullanılarak hesaplamak eğitim adlı için size gönderilen görüntüleri kullanan [k mızın çapraz doğrulama](https://en.wikipedia.org/wiki/Cross-validation_(statistics)). Duyarlık ve geri çağırma verimliliğine dair bir sınıflandırıcı iki farklı ölçümler şunlardır:

- **Duyarlık** doğru tanımlanan sınıflandırmalar bölümünü gösterir. Örneğin, model 100 görüntü köpekler olarak tanımlanır ve bunların 99 köpekler aslında olan, ardından Duyarlığı % 99 olabilir.
- **Geri çağırma** doğru olarak tanımlanmıştı gerçek sınıflandırmaları bölümünü gösterir. Örneğin, gerçekte 100 görüntülerini elma vardı ve model 80 elma olarak tanımlanmış geri çağırma % 80 olacaktır.

![Eğitim sonuçları genel kesinlik ve geri çağırma ve duyarlık Göster ve sınıflandırıcı her etiket için geri çağırma.](./media/getting-started-build-a-classifier/train03.png)

### <a name="probability-threshold"></a>Olasılık eşiği

Not **olasılık eşiği** sol bölmesinde slider'da **performans** sekmesi. Duyarlık ve geri çağırma ile ilgili işlem yapılırken doğru olarak değerlendirilmesi tahmin edilen bir olasılık eşiği budur.

Tahmin çağrısı ile yüksek olasılık eşiği yorumlama eğilimlidir yüksek duyarlık geri çekme çoğaltamaz sonuçları döndürmek için (bulunan sınıflandırmaları doğru, ancak çoğu bulunamadı); düşük olasılık eşiği tersi yok (gerçek sınıflandırmaları çoğunu bulundu, ancak bu kümesi içinde hatalı pozitif sonuçları vardır). Bunu aklınızda olasılık eşiği projeniz belirli ihtiyaçlarına göre ayarlamanız gerekir. Daha sonra istemci tarafında aynı olasılık Eşiği değeri filtre olarak tahmin sonuçlarını modelden alırken kullanmanız gerekir.

## <a name="manage-training-iterations"></a>Eğitim yinelemelerini yönetme

Her zaman sınıflandırıcınızı eğitmek, yeni oluşturduğunuz _yineleme_ kendi güncelleştirilmiş performans ölçümleri ile. Tüm yinelemelerinizi'nın sol bölmesinde görüntüleyebileceğiniz **performans** sekmesi. Bunlardan birini seçtiğinizde bunu yapma seçeneğiniz vardır _varsayılan yineleme_ tıklayarak **varsayılan yap** üstünde düğme. _Varsayılan yineleme_ tahmin API aracılığıyla sorguladığınızda, varsayılan olarak kullanılacak modelidir (bir uygulamadan örneği için). Güncelleştirilecek reddetmeniz durumunda _varsayılan yineleme_, uygulamanızın geçerli davranışını; etkilemeden modelinizi eğitmek devam edebilirsiniz ve ardından, geliştirilmiş modeliyle memnun olduğunuzda, varsayılan güncelleştirebilirsiniz.

Sol bölmede, ayrıca bulacaksınız **Sil** düğmesi, eski ise, bir yineleme silmek için kullanabilirsiniz. Bir yineleme sildiğinizde, benzersiz olarak onunla ilişkili tüm görüntüleri silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, özel görüntü işleme Web sitesini kullanarak bir görüntü sınıflandırma modeli eğitmek ve oluşturma hakkında bilgi edindiniz. Ardından, modelinizi geliştirme, yinelemeli işlemi hakkında daha fazla bilgi alın.

[Modeli test etme ve yeniden eğitme](test-your-model.md)

