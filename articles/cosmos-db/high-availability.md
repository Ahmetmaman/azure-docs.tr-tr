---
title: Azure Cosmos DB yüksek kullanılabilirlik
description: Bu makalede, Azure Cosmos DB yüksek kullanılabilirlik sağladığını açıklanmaktadır
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/13/2020
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 2fb8b24d5d44ced8f9e363008354acf5bc2fde40
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93081884"
---
# <a name="how-does-azure-cosmos-db-provide-high-availability"></a>Azure Cosmos DB yüksek kullanılabilirlik sağlama
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Azure Cosmos DB, iki temel şekilde yüksek kullanılabilirlik sağlar. İlk olarak, Azure Cosmos DB verileri Cosmos hesabı içinde yapılandırılan bölgelerde çoğaltır. İkincisi, Azure Cosmos DB bir bölgedeki verilerin 4 çoğaltmasını korur.

Azure Cosmos DB, küresel olarak dağıtılmış bir veritabanı hizmetidir ve Azure 'da temel bir hizmettir. Varsayılan olarak, [Azure 'un kullanılabildiği tüm bölgelerde](https://azure.microsoft.com/global-infrastructure/services/?products=cosmos-db&regions=all)kullanılabilir. Azure Cosmos hesabınızla istediğiniz sayıda Azure bölgesini ilişkilendirebilirsiniz ve verileriniz otomatik olarak ve saydam olarak çoğaltılır. İstediğiniz zaman Azure Cosmos hesabınıza bir bölge ekleyebilir veya kaldırabilirsiniz. Cosmos DB, müşteriler tarafından kullanılabilen beş farklı Azure bulut ortamında kullanılabilir:

* Genel olarak kullanılabilen **Azure genel** bulutu.

* **Azure Çin 21Vianet** , Microsoft ve 21Vianet arasındaki, ülkenin en büyük İnternet sağlayıcılarından biri Çin 'deki benzersiz bir iş ortaklığı aracılığıyla sunulmaktadır.

* **Azure Almanya** , bir veri güvenliği modeli altında hizmetler sağlar ve bu da, Almanya veri güvenliği olarak davranan, Deutsche Telekod 'nin bir yan kuruluşu olan T-Systems International GmbH denetimi altındaki Almanya 'da kalmaya devam eder.

* **Azure Kamu** , ABD kamu kurumlarına ve iş ortaklarına Birleşik Devletler dört bölgede kullanılabilir.

* **Savunma Bakanlığı (DOD) Için Azure Kamu** , ABD savunma Bakanlığı Birleşik Devletler iki bölgede sunulmaktadır.

Bir bölge içinde Azure Cosmos DB, aşağıdaki görüntüde gösterildiği gibi verilerinizin fiziksel bölümlere çoğaltma olarak dört kopyasını tutar:

:::image type="content" source="./media/high-availability/cosmosdb-data-redundancy.png" alt-text="Fiziksel bölümlendirme" border="false":::

* Azure Cosmos kapsayıcıları içindeki veriler [yatay olarak bölümlenmiştir](partitioning-overview.md).

* Bölüm kümesi, birden çok çoğaltma kümesi koleksiyonudur. Her bölge içinde, her bölüm çoğaltılan ve çoğalan çoğalma işlemi tarafından kaydedilmiş tüm yazmaları olan bir çoğaltma kümesi tarafından korunur. Çoğaltmalar, çok sayıda 10-20 hata etki alanı üzerinden dağıtılır.

* Tüm bölgelerde her bölüm çoğaltılır. Her bölge, bir Azure Cosmos kapsayıcısının tüm veri bölümlerini içerir ve yazma işlemlerini kabul edebilir ve okumaları sunabilir.  

Azure Cosmos hesabınız *N* Azure bölgelerinde dağıtılırsa, tüm verilerinizin en az *n* x 4 kopyası olacaktır. 2 ' den fazla bölgede bir Azure Cosmos hesabı olması, uygulamanızın kullanılabilirliğini iyileştirir ve ilişkili bölgeler arasında düşük gecikme süresi sağlar.

## <a name="slas-for-availability"></a>Kullanılabilirlik için SLA 'Lar

Küresel olarak dağıtılmış bir veritabanı olarak, Azure Cosmos DB aktarım hızını çevreleyen kapsamlı SLA 'Lar, en fazla 99 ' luk bir gecikme süresi, tutarlılık ve yüksek kullanılabilirlik sağlar. Aşağıdaki tabloda, tek ve çok bölgeli hesaplar için Azure Cosmos DB tarafından sunulan yüksek kullanılabilirlik garantisi gösterilmektedir. Yüksek kullanılabilirlik için Azure Cosmos hesaplarınızı her zaman birden fazla yazma bölgesi olacak şekilde yapılandırın.

|İşlem türü  | Tek bölge |Çok bölgeli (tek bölge yazmaları)|Çok bölgeli (çok bölgeli yazma) |
|---------|---------|---------|-------|
|Yazmalar    | 99,99    |99,99   |99,999|
|Okumalar     | 99,99    |99,999  |99,999|

> [!NOTE]
> Uygulamada, sınırlı Eskime durumu, oturum, tutarlı ön ek ve nihai tutarlılık modelleriyle ilgili gerçek yazma kullanılabilirliği, yayımlanan SLA 'Lara göre önemli ölçüde daha yüksektir. Tüm tutarlılık seviyelerinin gerçek okuma kullanılabilirliği, yayımlanan SLA 'ların önemli ölçüde daha yüksektir.

## <a name="high-availability-with-azure-cosmos-db-in-the-event-of-regional-outages"></a>Bölgesel kesintiler durumunda Azure Cosmos DB ile yüksek kullanılabilirlik

Bölgesel kesintiden nadir durumlar için Azure Cosmos DB veritabanınızın her zaman yüksek oranda kullanılabilir olduğundan emin olur. Aşağıdaki ayrıntılar, Azure Cosmos hesap yapılandırmanıza bağlı olarak kesinti sırasında Azure Cosmos DB davranışını yakalar:

* Azure Cosmos DB, istemciye bir yazma işlemi alınmadan önce, veriler yazma işlemlerini kabul eden bölge içindeki çoğaltmaların bir çekirdeği tarafından işlenir. Daha ayrıntılı bilgi için bkz. [tutarlılık düzeyleri ve aktarım hızı](./consistency-levels.md#consistency-levels-and-throughput)

* Birden fazla yazma bölgesiyle yapılandırılan çok bölgeli hesaplar, yazma ve okuma işlemleri için yüksek oranda kullanılabilir olacaktır. Bölgesel yük devretme işlemleri Azure Cosmos DB istemcisinde algılanır ve işlenir. Bunlar da anlık ve uygulamadan herhangi bir değişiklik gerektirmez.

* Tek bölgeli hesaplar, bölgesel kesintiden sonraki kullanılabilirliği kaybedebilir. Her zaman yüksek kullanılabilirlik sağlamak için Azure Cosmos hesabınızla **en az iki bölge** (tercihen, en az iki yazma bölgesi) ayarlamanız önerilir.

### <a name="multi-region-accounts-with-a-single-write-region-write-region-outage"></a>Tek bir yazma bölgesi olan çok bölgeli hesaplar (bölge kesintisi yazma)

* Yazma bölgesi kesintisi sırasında, Azure Cosmos hesabı, Azure Cosmos hesabında **otomatik yük devretmeyi etkinleştir** seçeneği yapılandırıldığında, ikincil bölgeyi otomatik olarak yeni birincil yazma bölgesi olacak şekilde yükseltir. Etkinleştirildiğinde, yük devretme, belirlediğiniz bölge önceliği sırasına göre başka bir bölgeye yapılır.

* Daha önce etkilenen bölge yeniden çevrimiçi olduğunda, bölge başarısız olduğunda çoğaltılmamış olan tüm yazma verileri, [Çakışma akışı](how-to-manage-conflicts.md#read-from-conflict-feed)aracılığıyla kullanılabilir hale getirilir. Uygulamalar, çakışmalar akışını okuyabilir, uygulamaya özgü mantığa göre çakışmaları çözümleyebilir ve güncelleştirilmiş verileri uygun şekilde Azure Cosmos kapsayıcısına geri yazabilir.

* Daha önce etkilenen yazma bölgesi kurtarıldıktan sonra otomatik olarak bir okuma bölgesi olarak kullanılabilir hale gelir. Kurtarılan bölgeye yazma bölgesi olarak dönebilirsiniz. [PowerShell, Azure CLI veya Azure Portal](how-to-manage-database-account.md#manual-failover)kullanarak bölgeleri değiştirebilirsiniz. Bir **veri veya kullanılabilirlik kaybı** , yazma bölgesini değiştirmeden veya sonra, uygulamanız yüksek oranda kullanılabilir olmaya devam eder.

> [!IMPORTANT]
> **Otomatik yük devretmeyi etkinleştirmek** için üretim iş yükleri Için kullanılan Azure Cosmos hesaplarını yapılandırmanız önemle tavsiye edilir. El ile yük devretme, yük devretme sırasında veri kaybı olmadığından emin olmak için bir tutarlılık denetimini tamamlamaya yönelik ikincil ve birincil yazma bölgesi arasında bağlantı gerektirir. Birincil bölge kullanılamaz durumdaysa, bu tutarlılık denetimi tamamlanamaz ve el ile yük devretme başarısız olur ve bu durum, bölgesel kesinti süresi boyunca yazma kullanılabilirliği kaybı ile sonuçlanır.

### <a name="multi-region-accounts-with-a-single-write-region-read-region-outage"></a>Tek bir yazma bölgesi olan çok bölgeli hesaplar (okuma bölgesi kesintisi)

* Bir okuma bölgesi kesintisi sırasında, herhangi bir tutarlılık düzeyi veya üç veya daha fazla okuma bölgesi ile güçlü tutarlılık kullanan Azure Cosmos hesapları, okuma ve yazma işlemleri için yüksek oranda kullanılabilir olmaya devam edecektir.

* İki veya daha az okuma bölgesi (okuma & yazma bölgesini içerir) ile güçlü tutarlılık kullanan Azure Cosmos hesapları, okuma bölgesi kesintisi sırasında okuma yazma kullanılabilirliğini kaybeder.

* Etkilenen bölgenin bağlantısı otomatik olarak kesilir ve çevrimdışı olarak işaretlenir. [Azure Cosmos DB SDK 'ları](sql-api-sdk-dotnet.md) , okuma çağrılarını tercih edilen bölge listesindeki bir sonraki kullanılabilir bölgeye yönlendirir.

* Tercih edilen bölge listesindeki bölgelerin hiçbiri kullanılabilir durumda değilse çağrılar otomatik olarak geçerli yazma bölgesine döner.

* Okuma bölgesi kesintisi 'nı işlemek için uygulama kodunuzda değişiklik yapılması gerekmez. Etkilenen okuma bölgesi yeniden çevrimiçi olduğunda, otomatik olarak geçerli yazma bölgesiyle eşitlenir ve okuma isteklerine hizmeti sağlamak için yeniden kullanılabilir hale gelir.

* Sonraki okumalar kurtarılan bölgeye yönlendirilir ve bunun için uygulamanızın kodunda değişiklik yapılması gerekmez. Daha önce başarısız olan bir bölgenin yük devretmesi ve yeniden katılması sırasında, uyumluluk garantisi Azure Cosmos DB tarafından kabul edilir.

* Azure bölgesinin kalıcı olarak kurtarılabilir olduğu nadir ve talihsiz olayında bile, çok bölgeli Azure Cosmos hesabınız *güçlü* tutarlılık ile yapılandırıldıysa veri kaybı olmaz. Kalıcı ve kurtarılabilir bir yazma bölgesi durumunda, sınırlı stalet tutarlılığı ile yapılandırılmış çok bölgeli bir Azure Cosmos hesabı olan potansiyel veri kaybı penceresi, K = 100000 güncelleştirmelerinin ve T = 5 dakikadan kısa bir süre içinde ( *k* veya *t* ) kısıtlanmıştır. Oturum, tutarlı ön ek ve nihai tutarlılık seviyeleri için, olası veri kaybı penceresi en fazla 15 dakika sınırlı olur. Azure Cosmos DB için RTO ve RPO hedefleri hakkında daha fazla bilgi için bkz. [tutarlılık düzeyleri ve veri dayanıklılığı](./consistency-levels.md#rto)

## <a name="availability-zone-support"></a>Kullanılabilirlik alanı desteği

Çapraz bölge dayanıklılığına ek olarak, Azure Cosmos veritabanınız ile ilişkilendirilecek bölge seçerken **bölge yedekliliği** de etkinleştirebilirsiniz.

Kullanılabilirlik alanı desteğiyle Azure Cosmos DB, çoğaltmaların belirli bir bölgedeki birden çok bölgeye yerleştirildiğinden emin olur ve bu da, en fazla başarısızlık sırasında yüksek kullanılabilirlik ve esneklik sağlar. Bu yapılandırmadaki gecikme ve diğer SLA 'Lara yönelik bir değişiklik yoktur. Tek bir bölge hatası durumunda, bölge artıklığı RPO = 0 ve RTO = 0 ile kullanılabilirlik için tam veri dayanıklılığı sağlar.

Bölge artıklığı, [çok bölgeli yazma özelliğindeki çoğaltmaya](how-to-multi-master.md) yönelik *ek bir özelliktir* . Bölgesel dayanıklılık elde etmek için tek başına bölge artıklığı güvenlenemez. Örneğin, bölgeler genelinde bölgesel kesintiler veya düşük gecikme erişimi durumunda, bölge yedekliliğe ek olarak birden fazla yazma bölgesi olması önerilir.

Azure Cosmos hesabınız için çok bölgeli yazma yapılandırırken, ek ücret ödemeden bölge yedekliliği seçebilirsiniz. Aksi takdirde, lütfen bölge artıklığı desteğinin fiyatlandırmasıyla ilgili olarak aşağıdaki nota bakın. Bölgeyi kaldırarak ve bölge yedekliği etkinken yeniden ekleyerek, Azure Cosmos hesabınızın mevcut bir bölgesinde bölge yedekliliği etkinleştirebilirsiniz.

Bu özellik: *UK Güney, Güneydoğu Asya, Doğu ABD, Doğu ABD 2, Orta ABD, Batı Avrupa, Batı ABD 2, Japonya Doğu, Kuzey Avrupa* , Fransa orta, Avustralya Doğu, Doğu ABD 2 euap bölgelerinde kullanılabilir.

Aşağıdaki tabloda çeşitli hesap yapılandırmalarının yüksek kullanılabilirlik özelliği özetlenmektedir:

|KPI  |Kullanılabilirlik Alanları olmayan tek bölge (AZ değil)  |Kullanılabilirlik Alanları tek bölge (AZ)  |Kullanılabilirlik Alanları (AZ, 2 bölge) ile çok bölgeli yazma: en önerilen ayar |
|---------|---------|---------|---------|
|Kullanılabilirlik SLA 'Sı yaz | %99,99 | %99,99 | %99,999 |
|Kullanılabilirlik SLA 'sını oku  | %99,99 | %99,99 | %99,999 |
|Fiyat | Tek bölge faturalandırma oranı | Tek bölge kullanılabilirlik alanı faturalandırma oranı | Çok bölgeli fatura ücreti |
|Bölge arızaları – veri kaybı | Veri kaybı | Veri kaybı yok | Veri kaybı yok |
|Bölge arızaları – kullanılabilirlik | Kullanılabilirlik kaybı | Kullanılabilirlik kaybı yok | Kullanılabilirlik kaybı yok |
|Okuma gecikmesi | Çapraz bölge | Çapraz bölge | Düşük |
|Yazma gecikme süresi | Çapraz bölge | Çapraz bölge | Düşük |
|Bölgesel kesinti – veri kaybı | Veri kaybı |  Veri kaybı | Veri kaybı <br/><br/> Birden fazla yazma bölgesi ve birden fazla bölge ile sınırlı stalet tutarlılığı kullanılırken, veri kaybı hesabınızda yapılandırılan sınırlı stalet ile sınırlıdır <br /><br />Birden çok bölgeyle güçlü tutarlılığı yapılandırarak bölgesel bir kesinti sırasında veri kaybını önleyebilirsiniz. Bu seçenek, kullanılabilirliği ve performansı etkileyen bir denge sunar. Yalnızca tek bölgeli yazma işlemleri için yapılandırılmış hesaplarda yapılandırılabilir. |
|Bölgesel kesinti – kullanılabilirlik | Kullanılabilirlik kaybı | Kullanılabilirlik kaybı | Kullanılabilirlik kaybı yok |
|Aktarım hızı | X RU/sn sağlanan aktarım hızı | X RU/sn sağlanan üretilen iş * 1,25 | 2X/sn sağlanan aktarım hızı <br/><br/> İki bölge olduğundan, bu yapılandırma modu Kullanılabilirlik Alanları tek bir bölgeyle karşılaştırıldığında üretilen iş miktarı sayısını iki kez gerektirir. |

> [!NOTE]
> Çok bölgeli bir Azure Cosmos hesabı için kullanılabilirlik alanı desteğini etkinleştirmek üzere, hesapta çok bölgeli yazma işlemleri etkinleştirilmelidir.

Yeni veya mevcut Azure Cosmos hesaplarına bölge eklerken bölge yedekliliği etkinleştirebilirsiniz. Azure Cosmos hesabınızda bölge yedekliliği etkinleştirmek için `isZoneRedundant` bayrağını `true` belirli bir konum için ayarlamanız gerekir. Bu bayrağı konumlar özelliği içinde ayarlayabilirsiniz. Örneğin, aşağıdaki PowerShell kod parçacığı "Güneydoğu Asya" bölgesi için bölge yedekliliği sunar:

Kullanılabilirlik Alanları şu şekilde etkinleştirilebilir:

* [Azure portalındaki](how-to-manage-database-account.md#addremove-regions-from-your-database-account)

* [Azure PowerShell](manage-with-powershell.md#create-account)

* [Azure CLI](manage-with-cli.md#add-or-remove-regions)

* [Azure Resource Manager şablonları](./manage-with-templates.md)

## <a name="building-highly-available-applications"></a>Yüksek oranda kullanılabilir uygulamalar oluşturma

* Bu olaylar sırasında [Azure Cosmos SDK](troubleshoot-sdk-availability.md) 'larının beklenen davranışını gözden geçirin ve bunu etkileyen yapılandırma vardır.

* Yüksek yazma ve okuma kullanılabilirliği sağlamak için Azure Cosmos hesabınızı, birden çok yazma bölgesi ile en az iki bölgeye yaymak üzere yapılandırın. Bu yapılandırma, SLA 'Lar tarafından desteklenen hem okuma hem de yazma işlemleri için en yüksek kullanılabilirlik, en düşük gecikme süresi ve en iyi ölçeklenebilirlik sağlar. Daha fazla bilgi edinmek için bkz. [Azure Cosmos hesabınızı birden çok yazma bölgesi ile yapılandırma](tutorial-global-distribution-sql-api.md).

* Tek bir yazma bölgesiyle yapılandırılan çok bölgeli Azure Cosmos hesapları için, [Azure CLI veya Azure Portal kullanarak otomatik yük devretmeyi etkinleştirin](how-to-manage-database-account.md#automatic-failover). Otomatik yük devretmeyi etkinleştirdikten sonra, bölgesel bir olağanüstü durum olduğunda Cosmos DB hesabınıza otomatik olarak yük devreder.  

* Azure Cosmos hesabınız yüksek oranda kullanılabilir olsa bile, uygulamanız yüksek oranda kullanılabilir kalacak şekilde doğru şekilde tasarlanmayabilir. Uygulama test veya olağanüstü durum kurtarma (DR) detaylarının bir parçası olarak uygulamanızın uçtan uca yüksek kullanılabilirliğini test etmek için, hesapta otomatik yük devretmeyi geçici olarak devre dışı bırakın, [PowerShell, Azure CLI veya Azure Portal kullanarak el ile yük devretmeyi](how-to-manage-database-account.md#manual-failover)çağırın, sonra uygulamanızın yük devretmesini izleyin. Tamamlandıktan sonra, birincil bölgeye yeniden yük devreder ve hesap için otomatik yük devretmeyi geri yükleyebilirsiniz.

* Küresel olarak dağıtılmış bir veritabanı ortamında, bölge genelinde bir kesinti olması durumunda tutarlılık düzeyi ve veri dayanıklılığı arasında doğrudan bir ilişki vardır. İş sürekliliği planınızı geliştirirken, kesintiye uğratan bir olaydan sonra uygulamanın tam olarak kurtarmadan önce kabul edilebilir en uzun süreyi anlamanız gerekir. Uygulamanın tam olarak kurtarılması için gereken süre, kurtarma zamanı hedefi (RTO) olarak bilinir. Ayrıca, uygulamanın, kesintiye uğratan bir olaydan sonra kurtarılırken kabul edebildiği en son veri güncelleştirme süresini de anlamanız gerekir. Kaybetmeyi göze alabileceğiniz güncelleştirme süresi kurtarma noktası hedefini (RPO) olarak bilinir. RPO ve RTO Azure Cosmos DB için bkz. [tutarlılık düzeyleri ve veri dayanıklılığı](./consistency-levels.md#rto)

## <a name="next-steps"></a>Sonraki adımlar

Daha sonra aşağıdaki makaleleri okuyabilirsiniz:

* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans avantajları](./consistency-levels.md)

* [Sağlanan aktarım hızını küresel olarak ölçeklendirme](./request-units.md)

* [Genel dağıtım - başlık altında](global-dist-under-the-hood.md)

* [Azure Cosmos DB'deki tutarlılık düzeyleri](consistency-levels.md)

* [Cosmos hesabınızı birden çok yazma bölgesi ile yapılandırma](how-to-multi-master.md)

* [Çoklu bölgesel ortamlarda SDK davranışı](troubleshoot-sdk-availability.md)