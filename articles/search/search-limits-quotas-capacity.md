---
title: Azure Search'te hizmet sınırları | Microsoft Docs
description: Kapasite planlaması için kullanılan hizmet sınırları ve istekleri ve yanıtları Azure Search için en yüksek sınırlar.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: heidist
ms.openlocfilehash: 8abcc90bf72544e6226d6c8487d2951b60ea6d29
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48802161"
---
# <a name="service-limits-in-azure-search"></a>Azure Search'te hizmet sınırları
En fazla depolama, iş yüklerini ve dizinleri, belgeler, miktarlarını sınırlar ve bağımlı nesneler bağımsız olarak, [Azure Search sağlama](search-create-service-portal.md) adresindeki **ücretsiz**, **temel**, veya **Standart** fiyatlandırma katmanları.

+ **Ücretsiz** Azure aboneliğinizle birlikte gelen bir çok kiracılı paylaşılan bir hizmettir.

+ **Temel** daha küçük ölçekli üretim iş yükleri için adanmış işlem kaynakları sağlar.

+ **Standart** ayrılmış makineye daha fazla depolama ve işleme kapasitesi olan her düzeyinde çalışır. Standart dört Düzeyler halinde sunulur: S1, S2, S3 ve S3 HD.

  S3 yüksek yoğunluklu (S3 HD) belirli iş yükleri için tasarlandı: [çok kiracılı](search-modeling-multitenant-saas-applications.md) ve büyük miktarlarda küçük dizinleri (bir milyon dizin başına belge, hizmet başına üç bin dizin). Bu katman sağlamaz [dizin oluşturucu özelliği](search-indexer-overview.md). S3 HD üzerinde dizin kaynaktan veri göndermek için API çağrıları kullanarak anında iletme yaklaşım, veri alımı yararlanarak gerekir. 

> [!NOTE]
> Belirli bir katmanda sağlanan bir hizmet. Kapasite sağlamak için katmanları atlama sağlama (yerinde yükseltme yoktur) yeni bir hizmeti içerir. Daha fazla bilgi için [bir SKU katmanı seçin veya](search-sku-tier.md). Bir hizmet zaten sağlanmış kapasitesiyle ayarlama hakkında daha fazla bilgi için bkz: [sorgu ve dizin oluşturma iş yükleri için kaynak düzeylerini ölçeklendirme](search-capacity-planning.md).
>

## <a name="subscription-limits"></a>Abonelik sınırları
[!INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## <a name="storage-limits"></a>Depolama sınırları
[!INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

<a name="index-limits"></a>

## <a name="index-limits"></a>Dizin sınırları

| Kaynak | Ücretsiz | Temel&nbsp;<sup>1</sup>  | S1 | S2 | S3 | S3&nbsp;HD |
| -------- | ---- | ------------------- | --- | --- | --- | --- |
| En fazla dizin |3 |5 veya 15 |50 |200 |200 |Bölüm başına 1000 veya hizmet başına 3000 |
| Dizin başına en fazla alanları |1000 |100 |1000 |1000 |1000 |1000 |
| En fazla [öneri Araçları](https://docs.microsoft.com/rest/api/searchservice/suggesters) dizin başına |1 |1 |1 |1 |1 |1 |
| En fazla [Puanlama profilleri](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) dizin başına |100 |100 |100 |100 |100 |100 |
| Profil başına en fazla işlevleri |8 |8 |8 |8 |8 |8 |

<sup>1</sup> geç 2017 15 dizinleri, veri kaynağı ve dizin artan bir sınırı oluşturduktan sonra oluşturulan temel Hizmetleri. Daha önce oluşturduğunuz Hizmetleri 5 sahiptir. Temel katmanı, yalnızca SKU ile dizin başına 100 alanların bir alt limit içindir.

<a name="document-limits"></a>

## <a name="document-limits"></a>Belge limitleri 

Ekim 2018'den itibaren artık herhangi bir katmandaki Faturalanabilir (temel, S1, S2, S3, S3 HD) herhangi bir bölgede oluşturulan tüm yeni hizmet için herhangi bir belge limitleri vardır. Birçok bölgedeki almışken sınırsız belge sayılarını Kasım/aralık 2017 beri belge limitleri dayatmak için devam beş bölgeleri vardı. Ne zaman ve nerede bir arama hizmeti oluşturduğunuz bağlı olarak, hala belge sınırlara tabidir bir hizmetinin çalışıyor olması.

Hizmetinizi belge limitleri olup olmadığını belirlemek için kullanım kutucuğunda hizmetinizin genel bakış sayfasında bakın. Belge sayısı olan sınırsız veya katmanını temel alan bir sınıra konu.

  ![Kullanım kutucuğu](media/search-limits-quotas-capacity/portal-usage-tile.png)

### <a name="regions-previously-having-document-limits"></a>Daha önce belge limitleri bölgeleri

Portal bir belge limiti gösteriyorsa, hizmetinizi ya da geç 2017'den önce oluşturulan ya da barındırma için Azure Search Hizmetleri daha düşük kapasiteli kümelerini kullanarak bir veri merkezi oluşturulduğu:

+ Avustralya Doğu
+ Doğu Asya
+ Orta Hindistan
+ Japonya Batı
+ Batı Orta ABD

Belge limitleri tabi hizmetler için aşağıdaki en yüksek sınırlar geçerlidir:

|  Ücretsiz | Temel | S1 | S2 | S3 | S3&nbsp;HD |
|-------|-------|----|----|----|-------|
|  10,000 |1 milyon |Bölüm başına 15 milyon veya hizmet başına 180 milyon |Bölüm başına 60 milyon veya hizmet başına 720 milyon |Bölüm başına 120 milyon veya hizmet başına 1.4 milyar |Dizin başına 1 milyon veya bölüm başına 200 milyon |

Hizmetinizi engelliyor mu sınırları varsa, yeni bir hizmet oluşturun ve sonra bu hizmet için tüm içeriği yeniden yayımlayın. Hizmetinizin Sahne arkasında yeni donanıma sorunsuz bir şekilde çıkış için bir mekanizma yoktur.

> [!Note] 
> Geç 2017 sonrasında oluşturulan S3 yüksek yoğunluklu hizmetler için 1 milyon belge başına dizin sınırı kalır ancak bölüm başına 200 milyon belge kaldırıldı.


### <a name="document-size-limits-per-api-call"></a>API çağrısı başına belge boyutu sınırları

Bir dizin API'nin çağrılması durumunda en fazla belge yaklaşık 16 megabayt boyutudur.

Belge, aslında bir dizin API istek gövdesi boyutu sınırı boyutudur. Dizin API için aynı anda birden çok belge toplu geçirebilirsiniz olduğundan, boyut sınırını gerçekçi bir toplu işlemde kaç belgeleri olduğunuza bağlıdır. Tek bir belge ile bir batch için JSON 16 MB maksimum belge boyutudur.

Belge boyutunu tutmak, istekten sorgulanamayan verileri dışlamak unutmayın. Görüntüleri ve diğer ikili verileri doğrudan sorgulanabilir değil ve dizinde depolanan olmamalıdır. Arama sonuçlarında sorgulanamayan veri tümleştirmek için bir URL kaynağına başvuru depolar aranabilir olmayan bir alan tanımlayın.

## <a name="indexer-limits"></a>Dizin Oluşturucu sınırları

Geç 2017'den sonra oluşturulan temel Hizmetleri 15 dizinleri, veri kaynakları, uzmanlık becerileri ve dizin oluşturucular artan bir sınırı vardır.

Kaynak Kullanımı Yoğun işlemleri, Azure blob dizin oluşturma veya bilişsel arama, doğal dil işleme, görüntü analizi gibi diğer dizin oluşturma işleri kullanılabilmesi için en çok kısa çalışan süreleri olması. İzin verilen en uzun süre içinde bir dizin oluşturma işi tamamlayamıyor bir zamanlamaya göre çalıştırmayı deneyin. Zamanlayıcı, dizin oluşturma durumunu izler. Zamanlanmış bir dizin oluşturma işi herhangi bir nedenden dolayı kesilirse, dizin oluşturucunun son zamanlanan sonraki çalışmaya kaldığı yukarı seçebilirsiniz.

| Kaynak | Ücretsiz&nbsp;<sup>1</sup> | Temel&nbsp;<sup>2</sup>| S1 | S2 | S3 | S3&nbsp;HD&nbsp;<sup>3</sup>|
| -------- | ----------------- | ----------------- | --- | --- | --- | --- |
| En fazla dizin oluşturucu |3 |5 veya 15|50 |200 |200 |Yok |
| En fazla veri kaynağı |3 |5 veya 15 |50 |200 |200 |Yok |
| En fazla becerilerini <sup>4</sup> |3 |5 veya 15 |50 |200 |200 |Yok |
| Çağrı başına en fazla dizin yükleme |10.000 belge |En fazla belge yalnızca sınırlıdır |En fazla belge yalnızca sınırlıdır |En fazla belge yalnızca sınırlıdır |En fazla belge yalnızca sınırlıdır |Yok |
| En fazla çalışma süresi <sup>5</sup> | 1-3 dakika |24 saat |24 saat |24 saat |24 saat |Yok  |
| Bilişsel arama becerilerini veya blob dizini oluşturmanın görüntü Analizi ile çalışan en fazla <sup>5</sup> | 3-10 dakika |2 saat |2 saat |2 saat |2 saat |Yok  |
| BLOB dizin oluşturucu: en yüksek blob boyutu, MB |16 |16 |128 |256 |256 |Yok  |
| BLOB dizin oluşturucu: bir blobun ayıkladığınız içeriği en fazla karakter |32,000 |64,000 |4 milyonluk |4 milyonluk |4 milyonluk |Yok |

<sup>1</sup> blob kaynakları için ve diğer tüm veri kaynakları için 1 dakika dizin oluşturucu en uzun yürütme süresi 3 dakikalık ücretsiz hizmetlere sahip.

<sup>2</sup> geç 2017 15 dizinleri, veri kaynağı ve dizin artan bir sınırı oluşturduktan sonra oluşturulan temel Hizmetleri. Daha önce oluşturduğunuz Hizmetleri 5 sahiptir.

<sup>3</sup> Hizmetleri S3 HD dizin oluşturucu desteği içermez.

<sup>4</sup> 30 becerilerinden beceri kümesi başına en fazla.

<sup>5</sup> bilişsel arama iş yükleri ve Azure blob dizin içinde görüntü analizi, normal metin dizin oluşturma daha kısa çalışan süreleri sahip. Görüntü analizi ve doğal dil işleme yoğun işlem gücü gerektiren ve orantısız miktarda kullanılabilir işleme gücünü kullanır. Çalışma süresi sırasındaki diğer işleri çalıştırmak için bir fırsat vermek şekilde azaltılmıştır.  

## <a name="queries-per-second-qps"></a>Sorgular / saniye (QPS)

QPS tahminleri bağımsız olarak her müşteri tarafından geliştirilmiş olmalıdır. Dizin boyutu ve karmaşıklığı, sorgu boyutu ve karmaşıklığı ve trafik miktarını QPS, birincil determinantlar var. Anlamlı tahminleri gibi faktörleri bilinmeyen olduğunda sunmak için hiçbir yolu yoktur.

Ayrılmış kaynaklarda (temel ve standart Katmanlar) üzerinde çalışan hizmetleri hesaplandığında daha öngörülebilir biriminizdeki tahmini fiyatlardır. Daha fazla parametre üzerinde denetime sahip olduğundan daha fazla QPS yakından tahmin edebilirsiniz. Yaklaşım tahmin etme ile ilgili yönergeler için bkz [Azure Search performans ve iyileştirme](search-performance-optimization.md).

## <a name="api-request-limits"></a>API isteği sınırları
* İstek başına 16 MB maksimum <sup>1</sup>
* 8 KB'lık URL uzunluğu üst sınırı
* Dizinin toplu iş başına en fazla 1000 belge yükler, birleştirmeler veya siler
* $Orderby tümcesinde en fazla 32 alanları
* En fazla arama terimi boyutu 32.766 bayt (2 bayt eksi 32 KB) UTF-8 kodlamalı metni olur.

<sup>1</sup> , Azure Search, bir istek gövdesi sınırı içeriğine göre her bir alanı veya teorik sınırları sınırlı olduğu değil koleksiyonları izlenmesi 16 MB'lık üst sınır tabidir (bkz [desteklenen veri türleri](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) alanı oluşturma ve kısıtlamaları hakkında daha fazla bilgi için).

## <a name="api-response-limits"></a>API yanıtı sınırları
* Arama sonuçlarının bir sayfa başına döndürülen en fazla 1000 belge
* Öner API istek döndürülen en fazla 100 önerileri

## <a name="api-key-limits"></a>API anahtarı sınırları
API anahtarları, hizmet kimlik doğrulaması için kullanılır. İki tür vardır. Yönetici anahtarları istek üst bilgisinde belirtilen ve hizmet için tam okuma-yazma erişimi. Salt okunur, belirtilen URL'yi ve genellikle istemci uygulamaları için dağıtılmış sorgu anahtarları.

* 2 yönetici anahtarları hizmet başına en fazla
* 50 sorgu anahtarları hizmet başına en fazla
