---
title: Azure kaynakları-Soru-Cevap Oluşturma
description: Soru-Cevap Oluşturma, her biri farklı amaçla çeşitli Azure kaynakları kullanır. Bunların nasıl kullanıldığını anlamak, doğru fiyatlandırma katmanını planlayıp seçmenizi veya fiyatlandırma katmanınızı ne zaman değiştireceğimizi görmenizi sağlar. Bunların birleşimde nasıl kullanıldığını anlamak, sorunları ortaya çıktığında bulmanıza ve düzeltmenize izin verir.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 11/09/2020
ms.openlocfilehash: cd64c19e7e9af05becd7a6978ceb4d0306112170
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96351904"
---
# <a name="azure-resources-for-qna-maker"></a>Soru-Cevap Oluşturma için Azure kaynakları

Soru-Cevap Oluşturma, her biri farklı amaçla çeşitli Azure kaynakları kullanır. Bunların nasıl kullanıldığını anlamak, doğru fiyatlandırma katmanını planlayıp seçmenizi veya fiyatlandırma katmanınızı ne zaman değiştireceğimizi görmenizi sağlar. Bunların _birleşimde_ nasıl kullanıldığını anlamak, sorunları ortaya çıktığında bulmanıza ve düzeltmenize izin verir.

## <a name="resource-planning"></a>Kaynak planlama

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/v1)

İlk olarak bir Soru-Cevap Oluşturma Bilgi Bankası geliştirirken, prototip aşamasında, hem test hem de üretim için tek bir Soru-Cevap Oluşturma kaynağı olması yaygındır.

Projenin geliştirme aşamasına geçtiğinizde şunları göz önünde bulundurmanız gerekir:

* Bilgi Bankası sisteminizin kaç dilde tutacağız?
* Bilgi tabanınız için kaç bölgeye ihtiyacınız var?
* Sisteminizdeki her etki alanındaki belge sayısı ne kadar olacak?

Tek bir Soru-Cevap Oluşturma kaynağına sahip olmak için, aynı dile, aynı bölgeye ve aynı konu etki alanı birleşimine sahip tüm bilgi bankalarının bir arada olmasını planlayın.

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/v2)

Soru-Cevap Oluşturma yönetilen bir Bilgi Bankası ilk kez geliştirirken, prototip aşamasında, hem test hem de üretim için tek bir Soru-Cevap Oluşturma yönetilen kaynağa sahip olmak yaygındır.

Projenin geliştirme aşamasına geçtiğinizde şunları göz önünde bulundurmanız gerekir:

* Bilgi Bankası sisteminizin kaç dilde tutacağız?
* Bilgi tabanınız için kaç bölgeye ihtiyacınız var?
* Sisteminizdeki her etki alanındaki belge sayısı ne kadar olacak?

---

## <a name="pricing-tier-considerations"></a>Fiyatlandırma Katmanı konuları

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/v1)

Genellikle göz önünde bulundurmanız gereken üç parametre vardır:

* **Hizmetten ihtiyacınız olan aktarım hızı**:
    * Gereksinimlerinize göre App Service için uygun [uygulama planını](https://azure.microsoft.com/pricing/details/app-service/plans/) seçin. Uygulamanın [ölçeğini](../../../app-service/manage-scale-up.md) değiştirebilir veya azaltabilirsiniz.
    * Bunun yanı sıra Azure **bilişsel arama** SKU seçiminizi de etkilemelidir, daha fazla ayrıntı için [buradaki](../../../search/search-sku-tier.md)ayrıntıları inceleyin. Ayrıca, çoğaltmalarla Bilişsel Arama [kapasiteyi](../../../search/search-capacity-planning.md) ayarlamanız gerekebilir.

* **Bilgi bankalarının boyutu ve sayısı**: senaryonuz Için uygun [Azure Search SKU](https://azure.microsoft.com/pricing/details/search/) 'sunu seçin. Genellikle, farklı konu etki alanlarının sayısına göre ihtiyacınız olan bilgi tabanı sayısına karar verirsiniz. Konu etki alanı (tek bir dil için) bir bilgi tabanında olmalıdır.

    N-1 bilgi bankasını belirli bir katmanda yayımlayabilirsiniz; burada N, katmanda izin verilen en fazla dizin olabilir. Ayrıca, katman başına izin verilen maksimum boyut ve belge sayısını kontrol edin.

    Örneğin, katmanınızda 15 ' in izin verilen dizini varsa, 14 bilgi tabanı (yayımlanan Bilgi Bankası başına 1 Dizin) yayımlayabilirsiniz. On beşinci Dizin, yazma ve test için tüm bilgi tabanları için kullanılır.

* **Kaynak olarak belge sayısı**: soru-cevap oluşturma yönetim hizmetinin ücretsiz SKU 'su, Portal ve API 'ler aracılığıyla (her bırı 1 MB boyutunda) yönetebileceğiniz belge sayısını sınırlar. Standart SKU, yönetebileceğiniz belge sayısıyla sınırlı değildir. Daha fazla ayrıntı için [buraya](https://aka.ms/qnamaker-pricing)bakın.

Aşağıdaki tabloda bazı üst düzey yönergeler sunulmaktadır.

|                            | Soru-Cevap Oluşturma yönetimi | App Service | Azure Bilişsel Arama | Sınırlamalar                      |
| -------------------------- | -------------------- | ----------- | ------------ | -------------------------------- |
| **Deneme**        | Ücretsiz SKU             | Ücretsiz katman   | Ücretsiz katman    | 2 KBs 'e kadar yayımlayın, 50 MB boyutunda  |
| **Geliştirme ve test ortamı**   | Standart SKU         | Paylaşılan      | Temel        | En fazla 14 KBs, 2 GB boyut yayımlayın    |
| **Üretim ortamı** | Standart SKU         | Temel       | Standart     | 49 KBs 'e kadar yayımlayın, 25 GB boyut |

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/v2)

Genellikle göz önünde bulundurmanız gereken üç parametre vardır:

* **Hizmetten ihtiyacınız olan aktarım hızı**:
    * Soru-Cevap Oluşturma yönetilen (Önizleme), ücretsiz bir hizmettir ve aktarım hızı şu anda hem yönetim API 'Leri hem de tahmin API 'Leri için 10 TPS 'ye atlar.
    * Bunun yanı sıra Azure **bilişsel arama** SKU seçiminizi de etkilemelidir, daha fazla ayrıntı için [buradaki](../../../search/search-sku-tier.md)ayrıntıları inceleyin. Ayrıca, çoğaltmalarla Bilişsel Arama [kapasiteyi](../../../search/search-capacity-planning.md) ayarlamanız gerekebilir.

* **Bilgi bankalarının boyutu ve sayısı**: senaryonuz Için uygun [Azure Search SKU](https://azure.microsoft.com/pricing/details/search/) 'sunu seçin. Genellikle, farklı konu etki alanlarının sayısına göre ihtiyacınız olan bilgi tabanı sayısına karar verirsiniz. Konu etki alanı (tek bir dil için) bir bilgi tabanında olmalıdır.

    Soru-Cevap Oluşturma yönetilen (Önizleme) ile, Soru-Cevap Oluşturma hizmetinizi tek bir dilde veya birden çok dilde KBs için ayarlama seçeneğiniz vardır. Soru-Cevap Oluşturma yönetilen (Önizleme) hizmetinize ilk bilgi bankasını oluştururken bu seçimi yapabilirsiniz.

    ![Soru-Cevap Oluşturma yönetilen (Önizleme) çok dilli Bilgi Bankası seçimi](../media/concept-plan-your-knowledge-base/qnamaker-v2-select-multilanguage-knowledge-base.png)

    Tek bir dilin N-1 bilgi bankasını veya belirli bir katmandaki farklı dillerin N/2 bilgi temellerini yayımlayabilirsiniz; burada N, katmanda izin verilen en fazla dizin sayısıdır. Ayrıca, katman başına izin verilen maksimum boyut ve belge sayısını kontrol edin.

    Örneğin, katmanınızda 15 ' in izin verilen dizini varsa, aynı dilin 14 bilgi esaslarını yayımlayabilirsiniz (yayımlanan Bilgi Bankası başına 1 Dizin). On beşinci Dizin, yazma ve test için tüm bilgi tabanları için kullanılır. Bilgi tabanlarının farklı dillerde olmasını seçerseniz, yalnızca 7 bilgi bankasını yayımlayabilirsiniz.

* **Kaynak olarak belge sayısı**: soru-cevap oluşturma yönetilen (Önizleme), ücretsiz bir hizmettir ve kaynak olarak ekleyebileceğiniz belge sayısında bir sınır yoktur. Daha fazla ayrıntı için [buraya](https://aka.ms/qnamaker-pricing)bakın.

Aşağıdaki tabloda bazı üst düzey yönergeler sunulmaktadır.

|                            |Azure Bilişsel Arama | Sınırlamalar                      |
| -------------------------- |------------ | -------------------------------- |
| **Deneme**        |Ücretsiz katman    | 2 KBs 'e kadar yayımlayın, 50 MB boyutunda  |
| **Geliştirme ve test ortamı**   |Temel        | En fazla 14 KBs, 2 GB boyut yayımlayın    |
| **Üretim ortamı** |Standart     | 49 KBs 'e kadar yayımlayın, 25 GB boyut |

---

## <a name="recommended-settings"></a>Önerilen ayarlar

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/v1)

|Hedef QPS | App Service | Azure Bilişsel Arama |
| -------------------- | ----------- | ------------ |
| 3             | S1, 1 örnek   | S1, 1 örnek    |
| 50         | S3, 10 örnek       | S1, 12 örnek         |
| 80         | S3, 10 örnek      |  S3, 12 örnek  |
| 100         | P3V2, 10 örnek  | S3, 12 örnek, 3 Bölüm   |
| 200 250         | P3V2, 20 örnek | S3, 12 örnek, 3 Bölüm    |

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/v2)

Soru-Cevap Oluşturma yönetilen ücretsiz bir hizmettir ve aktarım hızı şu anda hem yönetim API 'Leri hem de tahmin API 'Leri için saniyede 10 işlem üzerinden alınır. Hizmetiniz için saniyede 10 işlem hedeflemek üzere Azure Bilişsel Arama S1 (1 örneği) SKU 'SU önerilir.

---

## <a name="when-to-change-a-pricing-tier"></a>Fiyatlandırma katmanının ne zaman değiştirileceği

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/v1)

|Yükseltme|Nedeni|
|--|--|
|[Yükseltme](../How-to/set-up-qnamaker-service-azure.md#upgrade-qna-maker-sku) Soru-Cevap Oluşturma Management SKU 'SU|Bilgi bankalarınızda daha fazla QnA çifti veya belge kaynağı olmasını istiyorsunuz.|
|[Yükseltme](../How-to/set-up-qnamaker-service-azure.md#upgrade-app-service) SKU 'YU App Service ve Bilişsel Arama katmanını denetleyin ve [bilişsel arama çoğaltmaları oluşturun](../../../search/search-capacity-planning.md)|Bilgi tabanınız, bir sohbet bot gibi istemci uygulamanızdan daha fazla istek sunması gerekir.|
|[Yükseltme](../How-to/set-up-qnamaker-service-azure.md#upgrade-the-azure-cognitive-search-service) Azure Bilişsel Arama hizmeti|Birçok Bilgi Bankası 'na sahip olmanız planlanır.|

[Azure portal App Service güncelleyerek](../how-to/set-up-qnamaker-service-azure.md#get-the-latest-runtime-updates)en son çalışma zamanı güncelleştirmelerini alın.

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/v2)

[Yükseltme](../How-to/set-up-qnamaker-service-azure.md#upgrade-the-azure-cognitive-search-service) Birçok bilgi tabanı olduğunu planlarken Azure Bilişsel Arama hizmeti.

---

## <a name="resource-naming-considerations"></a>Kaynak adlandırma konuları

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/v1)

Soru-Cevap Oluşturma kaynağı için kaynak adı, `qna-westus-f0-b` diğer kaynakları adlandırmak için de kullanılır.

Azure portal oluştur penceresi, bir Soru-Cevap Oluşturma kaynağı oluşturup diğer kaynakların fiyatlandırma katmanlarını seçmenizi sağlar.

> [!div class="mx-imgBorder"]
> ![Soru-Cevap Oluşturma kaynak oluşturma için Azure portal ekran görüntüsü](../media/concept-azure-resource/create-blade.png)

Kaynaklar oluşturulduktan sonra, isteğe bağlı Application Insights kaynağı dışında aynı ada sahip olur, bu da karakterleri ada erteler.

> [!div class="mx-imgBorder"]
> ![Azure portal kaynak listesinin ekran görüntüsü](../media/concept-azure-resource/all-azure-resources-created-qnamaker.png)

> [!TIP]
> Bir Soru-Cevap Oluşturma kaynağı oluşturduğunuzda yeni bir kaynak grubu oluşturun. Bu, kaynak grubuna göre arama yaparken Soru-Cevap Oluşturma kaynağıyla ilişkili tüm kaynakları görmenizi sağlar.

> [!TIP]
> Kaynak veya kaynak grubu adı içindeki fiyatlandırma katmanlarını göstermek için bir adlandırma kuralı kullanın. Yeni bir Bilgi Bankası oluşturma veya yeni belgeler ekleme hakkında hata aldığınızda, Bilişsel Arama fiyatlandırma katmanı sınırı yaygın bir sorundur.

### <a name="resource-purposes"></a>Kaynak amaçları

Soru-Cevap Oluşturma ile oluşturulan her Azure kaynağı belirli bir amaca sahiptir:

* Soru-Cevap Oluşturma kaynağı
* Bilişsel Arama kaynağı
* App Service
* Uygulama planı hizmeti
* Application Insights hizmeti


### <a name="cognitive-search-resource"></a>Bilişsel Arama kaynağı

[Bilişsel arama](../../../search/index.yml) kaynak şu şekilde kullanılır:

* QnA çiftlerini depolayın
* Çalışma zamanında QnA çiftlerinin başlangıç derecelendirmesini (Ranker #1) sağlama

#### <a name="index-usage"></a>Dizin kullanımı

Kaynak, bir dizinin test dizini olarak davranmasını ve kalan dizinlerin her biri yayımlanmış bir Bilgi Bankası ile ilişkilendirilmesi için bir dizin tutar.

15 Dizin tutmak için ücretlendirilen bir kaynak, 14 yayımlanan bilgi esaslarını tutar ve tüm bilgi temellerini test etmek için bir dizin kullanılır. Etkileşimli test bölmesini kullanan bir sorgunun test dizinini kullanması, ancak yalnızca belirli Bilgi Bankası ile ilişkili belirli bir bölümden sonuçları döndürmesi için, bu test dizini Bilgi Bankası tarafından bölümlenir.

#### <a name="language-usage"></a>Dil kullanımı

Soru-Cevap Oluşturma kaynağında oluşturulan ilk bilgi tabanı, Bilişsel Arama kaynağı ve tüm dizinleri için ayarlanmış _tek_ dili belirlemekte kullanılır. Bir Soru-Cevap Oluşturma Hizmeti için yalnızca _bir dil kümesi_ kullanabilirsiniz.

### <a name="qna-maker-resource"></a>Soru-Cevap Oluşturma kaynağı

Soru-Cevap Oluşturma kaynağı yazma ve yayımlama API 'Lerine, çalışma zamanında de QnA çiftlerinin doğal dil işleme (NLP) tabanlı ikinci derecelendirme katmanına (Ranker #2) erişim sağlar.

İkinci derecelendirme, meta veri ve izleme istemlerini içerebilen akıllı filtreler uygular.

#### <a name="qna-maker-resource-configuration-settings"></a>Soru-Cevap Oluşturma kaynak yapılandırma ayarları

[Soru-cevap oluşturma portalında](https://qnamaker.ai)yeni bir Bilgi Bankası oluşturduğunuzda, **dil** ayarı kaynak düzeyinde uygulanan tek ayardır. Kaynak için ilk Bilgi Bankası 'nı oluştururken dili seçersiniz.

### <a name="app-service-and-app-service-plan"></a>App Service ve App Service planı

[App Service](../../../app-service/index.yml) , istemci uygulamanız tarafından, yayımlanan bilgi matrahına çalışma zamanı uç noktası aracılığıyla erişmek için kullanılır.

Yayımlanan Bilgi Bankası 'nı sorgulamak için, yayımlanan tüm bilgi tabanları aynı URL uç noktasını kullanır, ancak rota içinde **bilgi BANKASı kimliğini** belirtir.

`{RuntimeEndpoint}/qnamaker/knowledgebases/{kbId}/generateAnswer`

### <a name="application-insights"></a>Application Insights

[Application Insights](../../../azure-monitor/app/app-insights-overview.md) , sohbet günlüklerini ve telemetrisini toplamak için kullanılır. Hizmetiniz hakkında bilgi edinmek için ortak [kusto sorgularını](../how-to/get-analytics-knowledge-base.md) gözden geçirin.

## <a name="share-services-with-qna-maker"></a>Hizmetleri Soru-Cevap Oluşturma ile paylaşma

Soru-Cevap Oluşturma çeşitli Azure kaynakları oluşturur. Yönetim ve maliyet paylaşımının avantajlarından yararlanmak için aşağıdaki tabloyu kullanarak neleri paylaşabdiklerinizi ve neleri paylaşabileceğinizi öğrenin:

|Hizmet|Paylaş|Nedeni|
|--|--|--|
|Bilişsel Hizmetler|X|Tasarım tarafından mümkün değil|
|App Service planı|✔|App Service planı için ayrılan sabit disk alanı. Aynı App Service planını paylaşan diğer uygulamalar önemli disk alanı kullanıyorsa, QnAMaker App Service örneği sorunlarla karşılaşacaktır.|
|App Service|X|Tasarım tarafından mümkün değil|
|Application Insights|✔|Paylaşılabilir|
|Arama hizmeti|✔|1. `testkb` QnAMaker hizmeti için ayrılmış bir addır; diğerleri tarafından kullanılamaz.<br>2. ad ile eş anlamlı eşleme `synonym-map` , QnAMaker hizmeti için ayrılmıştır.<br>3. yayımlanan bilgi tabanı sayısı arama hizmeti katmanıyla sınırlıdır. Kullanılabilir ücretsiz dizinler varsa, diğer hizmetler bunları kullanabilir.|

### <a name="using-a-single-cognitive-search-service"></a>Tek bir Bilişsel Arama hizmeti kullanma

Portal üzerinden bir QnA hizmeti ve bağımlılıklarını (arama gibi) oluşturursanız, sizin için bir arama hizmeti oluşturulur ve Soru-Cevap Oluşturma hizmetine bağlanır. Bu kaynaklar oluşturulduktan sonra, önceden var olan bir arama hizmetini kullanmak için App Service ayarını güncelleştirebilir ve yeni oluşturduğunuz birini kaldırabilirsiniz.

Soru-Cevap Oluşturma, Soru-Cevap Oluşturma kaynak oluşturma işleminin parçası olarak oluşturutından farklı bir bilişsel hizmet kaynağı kullanmak üzere [nasıl yapılandıracağınızı](../How-To/set-up-qnamaker-service-azure.md#configure-qna-maker-to-use-different-cognitive-search-resource) öğrenin.

## <a name="management-service-region"></a>Yönetim hizmeti bölgesi

Soru-Cevap Oluşturma yönetim hizmeti yalnızca Soru-Cevap Oluşturma portalı ve ilk veri işleme için kullanılır. Bu hizmet yalnızca **Batı ABD** bölgesinde kullanılabilir. Bu Batı ABD hizmetinde hiçbir müşteri verisi depolanmaz.

## <a name="keys-in-qna-maker"></a>Soru-Cevap Oluşturma anahtarlar

Soru-Cevap Oluşturma hizmetiniz iki tür anahtara sahiptir: uygulama hizmetinde barındırılan çalışma zamanında kullanılan anahtar ve **sorgu uç noktası anahtarları** **yazma** .

**Abonelik anahtarınızı** arıyorsanız, [terminoloji değişmiştir](#subscription-keys).

API aracılığıyla hizmete istek yaparken bu anahtarları kullanın.

![Anahtar yönetimi](../media/qnamaker-how-to-key-management/key-management.png)

|Ad|Konum|Amaç|
|--|--|--|
|Yazma anahtarı|[Azure Portal](https://azure.microsoft.com/free/cognitive-services/)|Bu anahtarlar [soru-cevap oluşturma Management Service API 'lerine](/rest/api/cognitiveservices/qnamaker4.0/knowledgebase)erişmek için kullanılır. Bu API 'Ler, bilgi bankasındaki soruları ve yanıtları düzenlemenize ve bilgi tabanınızı yayımlamanıza olanak sağlar. Yeni bir Soru-Cevap Oluşturma hizmeti oluşturduğunuzda bu anahtarlar oluşturulur.<br><br>Bu anahtarları **anahtarlar** sayfasındaki bilişsel **Hizmetler** kaynağında bulabilirsiniz.|
|Sorgu uç noktası anahtarı|[Soru-Cevap Oluşturma portalı](https://www.qnamaker.ai)|Bu anahtarlar, bir Kullanıcı sorusu için yanıt almak üzere yayımlanmış bilgi tabanı uç noktasını sorgulamak için kullanılır. Bu sorgu uç noktasını genellikle sohbet bot 'inizdeki veya Soru-Cevap Oluşturma hizmetine bağlanan istemci uygulama kodunda kullanırsınız. Bu anahtarlar Soru-Cevap Oluşturma bilgi bankasını yayımladığınızda oluşturulur.<br><br>Bu anahtarları **hizmet ayarları** sayfasında bulabilirsiniz. Bu sayfayı, açılan menüdeki sayfanın sağ üst kısmındaki kullanıcının menüsünden bulabilirsiniz.|

### <a name="subscription-keys"></a>Abonelik anahtarları

Hüküm yazma ve sorgu uç noktası anahtarı düzeltici koşullardır. Önceki terim **abonelik anahtarıdır**. Abonelik anahtarlarına başvuran başka belgeler görürseniz, bunlar yazma ve sorgu uç noktası anahtarlarına eşdeğerdir (çalışma zamanında kullanılır).

Hangi anahtarı bulmanız gerektiğini öğrenmek için, anahtarın ne eriştiğini, Bilgi Bankası yönetimini veya Bilgi Bankası sorguladığını bilmeniz gerekir.

### <a name="recommended-settings-for-network-isolation"></a>Ağ yalıtımı için önerilen ayarlar

* [Sanal ağı yapılandırarak](../../cognitive-services-virtual-networks.md?tabs=portal)bilişsel hizmet kaynağını ortak erişime karşı koruyun.
* App Service (QnA Runtime) ortak erişime karşı koruma:
    * Yalnızca bilişsel hizmet IP 'lerinden gelen trafiğe izin verin. Bunlar, "Biliveservicesmanagement" hizmet etiketinde zaten yer almaktadır. Bu, App Service 'i çağırmak ve Azure Search hizmeti 'ni uygun şekilde güncelleştirmek için API 'Leri yazma (oluşturma/güncelleştirme KB) için gereklidir.
    * Ayrıca, bot hizmeti, Soru-Cevap Oluşturma Portal (Corpnet olabilir) gibi diğer giriş noktalarına da izin verdiğinizden emin olun. tahmin için "GenerateAnswer" API erişimi.
    * [Hizmet etiketleri hakkında daha fazla bilgi edinin.](../../../virtual-network/service-tags-overview.md)

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/v2)

Soru-Cevap Oluşturma yönetilen (Önizleme) kaynağı için kaynak adı, `qna-westus-f0-b` diğer kaynakları adlandırmak için de kullanılır.

Azure portal oluştur penceresi, Soru-Cevap Oluşturma yönetilen (Önizleme) kaynağı oluşturmanızı ve diğer kaynakların fiyatlandırma katmanlarını seçmenizi sağlar.

> [!div class="mx-imgBorder"]
> ![Soru-Cevap Oluşturma yönetilen (Önizleme) kaynak oluşturma için Azure portal ekran görüntüsü ](../media/qnamaker-how-to-setup-service/enter-qnamaker-v2-info.png) kaynaklar oluşturulduktan sonra aynı ada sahiptir.

> [!div class="mx-imgBorder"]
> ![Soru-Cevap Oluşturma yönetilen Azure portal kaynak listesinin ekran görüntüsü (Önizleme)](../media/qnamaker-how-to-setup-service/resources-created-v2.png)
> [!TIP]
> Bir Soru-Cevap Oluşturma kaynağı oluşturduğunuzda yeni bir kaynak grubu oluşturun. Bu, kaynak grubuna göre arama yaparken Soru-Cevap Oluşturma yönetilen (Önizleme) kaynağıyla ilişkili tüm kaynakları görmenizi sağlar.
> [!TIP]
> Kaynak veya kaynak grubu adı içindeki fiyatlandırma katmanlarını göstermek için bir adlandırma kuralı kullanın. Yeni bir Bilgi Bankası oluşturma veya yeni belgeler ekleme hakkında hata aldığınızda, Bilişsel Arama fiyatlandırma katmanı sınırı yaygın bir sorundur.

### <a name="resource-purposes"></a>Kaynak amaçları

Soru-Cevap Oluşturma yönetilen (Önizleme) ile oluşturulan her Azure kaynağı belirli bir amaca sahiptir:

* Soru-Cevap Oluşturma kaynağı
* Bilişsel Arama kaynağı

### <a name="azure-cognitive-search-resource"></a>Azure Bilişsel Arama kaynağı

[Bilişsel arama](../../../search/index.yml) kaynak şu şekilde kullanılır:

* QnA çiftlerini depolayın
* Çalışma zamanında QnA çiftlerinin başlangıç derecelendirmesini (Ranker #1) sağlama

#### <a name="index-usage"></a>Dizin kullanımı

Tek bir dilin N-1 bilgi bankasını veya belirli bir katmandaki farklı dillerin N/2 bilgi temellerini yayımlayabilirsiniz; burada N, Azure Bilişsel Arama katmanında izin verilen en fazla dizin sayısıdır. Ayrıca, katman başına izin verilen maksimum boyut ve belge sayısını kontrol edin.

Örneğin, katmanınızda 15 ' in izin verilen dizini varsa, aynı dilin 14 bilgi esaslarını yayımlayabilirsiniz (yayımlanan Bilgi Bankası başına 1 Dizin). On beşinci Dizin, yazma ve test için tüm bilgi tabanları için kullanılır. Bilgi tabanlarının farklı dillerde olmasını seçerseniz, yalnızca 7 bilgi bankasını yayımlayabilirsiniz.

#### <a name="language-usage"></a>Dil kullanımı

Soru-Cevap Oluşturma yönetilen (Önizleme) ile, Soru-Cevap Oluşturma hizmetinizi tek bir dilde veya birden çok dilde Bilgi Bankası için ayarlama seçeneğiniz vardır. Bu seçeneği, Soru-Cevap Oluşturma hizmetinizde ilk Bilgi Bankası oluşturma sırasında yaparsınız. Bilgi Bankası başına dil ayarını [etkinleştirme bölümüne bakın](#pricing-tier-considerations) .

### <a name="qna-maker-resource"></a>Soru-Cevap Oluşturma kaynağı

Soru-Cevap Oluşturma yönetilen (Önizleme) kaynağı, yazma ve yayımlama API 'Lerine erişim sağlar, derecelendirme çalışma zamanını barındırır ve telemetri sağlar.

## <a name="region-support"></a>Bölge desteği

Soru-Cevap Oluşturma yönetilen (Önizleme) içinde hem yönetim hem de tahmin hizmetleri aynı bölgede birlikte bulunur. Şu anda Soru-Cevap Oluşturma yönetilmiş (Önizleme) **Orta Güney ABD, Kuzey Avrupa ve Avustralya Doğu** sunulmaktadır.

### <a name="keys-in-qna-maker-managed-preview"></a>Soru-Cevap Oluşturma yönetilen anahtarlar (Önizleme)

Soru-Cevap Oluşturma yönetilen (Önizleme) hizmetiniz, iki tür anahtar ile ilgilidir: müşterinin aboneliğindeki hizmete erişmek için kullanılan **yazma anahtarları** ve **Azure bilişsel arama anahtarları** .

**Abonelik anahtarınızı** arıyorsanız, [terminoloji değişmiştir](#subscription-keys).

API aracılığıyla hizmete istek yaparken bu anahtarları kullanın.

![Anahtar yönetimi ile yönetilen Önizleme](../media/qnamaker-how-to-key-management/qnamaker-v2-key-management.png)

|Ad|Konum|Amaç|
|--|--|--|
|Yazma anahtarı|[Azure Portal](https://azure.microsoft.com/free/cognitive-services/)|Bu anahtarlar [soru-cevap oluşturma Management Service API 'lerine](/rest/api/cognitiveservices/qnamaker4.0/knowledgebase)erişmek için kullanılır. Bu API 'Ler, bilgi bankasındaki soruları ve yanıtları düzenlemenize ve bilgi tabanınızı yayımlamanıza olanak sağlar. Yeni bir Soru-Cevap Oluşturma hizmeti oluşturduğunuzda bu anahtarlar oluşturulur.<br><br>Bu anahtarları **anahtarlar** sayfasındaki bilişsel **Hizmetler** kaynağında bulabilirsiniz.|
|Azure Bilişsel Arama yönetici anahtarı|[Azure Portal](../../../search/search-security-api-keys.md)|Bu anahtarlar, kullanıcının Azure aboneliğinde dağıtılan Azure bilişsel arama hizmeti ile iletişim kurmak için kullanılır. Soru-Cevap Oluşturma yönetilen (Önizleme) hizmeti ile bir Azure bilişsel aramayı ilişkilendirdiğinizde, yönetici anahtarı otomatik olarak Soru-Cevap Oluşturma hizmetine geçirilir. <br><br>Bu anahtarları **anahtarlar** sayfasında **Azure bilişsel arama** kaynağında bulabilirsiniz.|

### <a name="subscription-keys"></a>Abonelik anahtarları

Hüküm yazma ve sorgu uç noktası anahtarı düzeltici koşullardır. Önceki terim **abonelik anahtarıdır**. Abonelik anahtarlarına başvuran başka belgeler görürseniz, bunlar yazma ve sorgu uç noktası anahtarlarına eşdeğerdir (çalışma zamanında kullanılır).

Hangi anahtarı bulmanız gerektiğini öğrenmek için, anahtarın ne eriştiğini, Bilgi Bankası yönetimini veya Bilgi Bankası sorguladığını bilmeniz gerekir.

### <a name="recommended-settings-for-network-isolation"></a>Ağ yalıtımı için önerilen ayarlar 

[Sanal ağı yapılandırarak](../../cognitive-services-virtual-networks.md?tabs=portal)bilişsel hizmet kaynağını ortak erişime karşı koruyun.

---

## <a name="next-steps"></a>Sonraki adımlar

* Soru-Cevap Oluşturma [Bilgi Bankası](../index.yml) hakkında bilgi edinin
* [Bilgi Bankası yaşam döngüsünü](development-lifecycle-knowledge-base.md) anlama
* Hizmeti ve Bilgi Bankası [sınırlarını](../limits.md) gözden geçirin