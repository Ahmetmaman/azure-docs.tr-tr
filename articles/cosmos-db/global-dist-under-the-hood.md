---
title: Azure Cosmos DB genel dağıtımını - başlık altında
description: Bu makalede Azure Cosmos DB genel dağıtımını ilgili teknik ayrıntılar sağlar
services: cosmos-db
author: dharmas-cosmos
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: dharmas
ms.reviewer: sngun
ms.openlocfilehash: 418a98e0b5eeed6bc5b94ca78b8636116620b614
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51705421"
---
# <a name="azure-cosmos-db-global-distribution---under-the-hood"></a>Azure Cosmos DB genel dağıtımını - başlık altında

Azure Cosmos DB, Azure'nın temel bir hizmet olduğundan tüm Azure bölgeleri arasında genel, bağımsız, Savunma Bakanlığı (DoD) ve kamu bulutlarında dahil olmak üzere dünya çapında dağıtılır. Bir veri merkezi içinde dağıtın ve Azure Cosmos DB, makinelerin büyük Damgalar üzerinde her ayrılmış yerel depolama ile yönetin. Bir veri merkezi içinde Azure Cosmos DB her potansiyel olarak birden fazla donanım Nesilleri çalıştıran birçok kümeler arasında dağıtılır. Kümesindeki makineleri genellikle 10-20 hata etki alanlarına yayılır. Cosmos DB genel dağıtım sistemin topolojisi aşağıdaki resimde gösterilmektedir:

![Sistemin topolojisi](./media/global-dist-under-the-hood/distributed-system-topology.png)

**Azure Cosmos DB'de küresel dağıtım, anahtar teslim:** dilediğiniz zaman birkaç tıklama ile veya programlama yoluyla tek bir API çağrısıyla ekleyebilir veya kendi Cosmos veritabanıyla ilişkili coğrafi bölgeler kaldırın. Bir Cosmos veritabanı, sırayla Cosmos kapsayıcıların bir kümesinden oluşur. Cosmos DB'de kapsayıcıları dağıtımı ve ölçeklenebilirliği mantıksal birimleri görev yapar. Koleksiyonlar, tablolar ve grafikler oluşturduğunuz (dahili) yalnızca Cosmos kapsayıcılardır. Kapsayıcılardır tamamen şemadan ve bir sorgu için bir kapsam belirtin. Bir Cosmos kapsayıcıdaki veri alımı sırasında otomatik olarak dizine alınır. Otomatik dizin oluşturma, şema veya dizin yönetimi, özellikle küresel olarak dağıtılan bir kurulumda yaşamadan ile uğraşmak zorunda kalmadan verileri sorgulamak kullanıcıların sağlar.  

- Belirli bir bölgede bir kapsayıcı içindeki veri bölümü-sağlayın ve şeffaf bir şekilde temel alınan fiziksel bölümler (yerel dağıtım) tarafından yönetilen anahtar kullanılarak dağıtılır.  

- Her kaynak bölümü aynı zamanda coğrafi bölgeler arasında çoğaltılır (genel dağıtım). 

Ne zaman esnek bir şekilde Cosmos DB kullanarak uygulama aktarım hızını (veya daha fazla depolama alanını kullanan) bir Cosmos kapsayıcı, Cosmos DB bölüm yönetimi işlemlerini şeffaf bir şekilde işler (bölme, kopyalama, silme) tüm bölgeler arasında. Ölçek, dağıtım veya hatalardan bağımsız, Cosmos DB verilerin herhangi sayıda bölgesinde Global olarak dağıtılmış kapsayıcılar içinde tek sistem görüntüsünü sağlamaya devam eder.  

Aşağıdaki görüntüde gösterildiği gibi bir kapsayıcıdaki verilerin iki boyutta dağıtılır:  

![Fiziksel bölümler](./media/global-dist-under-the-hood/distribution-of-resource-partitions.png)

Bir fiziksel bölüm, bir çoğaltma kümesine adlı çoğaltma grubu tarafından uygulanır. Her makine, önceki görüntüde gösterildiği gibi çeşitli fiziksel bölümler işlemlerin sabit bir dizi karşılık çoğaltmaları yüzlerce barındırır. Çoğaltmaları karşılık gelen fiziksel bölümlere dinamik olarak yerleştirilir ve bir küme ve veri merkezleri bölge içindeki makineler arasında Yük Dengelemesi.  

Bir yinelemenin benzersiz bir Azure Cosmos DB kiracıya ait. Her yineleme Cosmos DB'NİN'ın bir örneğini barındıran [veritabanı altyapısı](https://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), ilişkili dizinleri yanı sıra kaynakları yönetir. Cosmos DB'nin veritabanı altyapısı bir atom kayıt sırasını (ARS) temel tür sisteminde çalışır. Altyapı, bir şema ve kayıtların yapısını ve örnek değerleri arasındaki sınırı Bulanıklaştırma kavramına bağımsızdır. Cosmos DB, otomatik olarak her şeyi alımı sırasında şema veya dizin yönetimiyle ilgilenmenize gerek kalmadan, Global olarak dağıtılmış verileri sorgulamak kullanıcılara verimli bir biçimde dizin oluşturma tarafından tam şema agnosticism ulaşır.

Cosmos DB'nin veritabanı altyapısı, çeşitli koordinasyon temelleri, dil çalışma zamanlarını, Sorgu işlemcisi, depolama ve alt sistemlerin sorumlu için işlem depolama dizini oluşturma ve veri dizin uygulaması dahil olmak üzere bileşenden oluşur, sırasıyla. Dayanıklılık ve yüksek kullanılabilirlik sağlamak için veritabanı altyapısı, veri ve dizin ssd'lerde devam ediyorsa ve yineleme içinde veritabanı altyapısı örneği arasında çoğaltır-kümelerini sırasıyla. Daha büyük kiracılar, daha yüksek aktarım hızı ve depolama ölçeğini için karşılık gelen ve daha büyük ya da daha sayısı, yinelemeler veya her ikisini de vardır. Sistemin her bileşenin tam olarak zaman uyumsuz: hiç olmadığı kadar hiçbir iş parçacığını engeller ve her bir iş parçacığı kısa süreli bir gereksiz iş parçacığı anahtar ödemeden çalışır. Genelinde tüm giriş denetimi yığından tüm g/ç yolu için oran sınırlandırma ve arka baskısı genişletilmeyecek. Veritabanı altyapımız, ayrıntılı eşzamanlılık yararlanmaya ve yüksek aktarım hızı frugal miktarda sistem kaynağı içinde çalışırken sunmak için tasarlanmıştır.

Cosmos DB'nin genel dağıtım üzerinde iki anahtar soyutlama kullanır: çoğaltma kümeleri ve bölüm ayarlar. Çoğaltma kümesi modüler bir Lego blok düzenlemesi için ve dinamik bir katmana coğrafi olarak dağıtılmış bir veya daha fazla fiziksel bölüm bölüm kümesi ise. Nasıl genel dağıtımını anlamak için çalışır, ihtiyacımız bu iki anahtar soyutlama anlamak. 

## <a name="replica-sets"></a>Çoğaltma-ayarlar

Kaynak bölümü, bir çoğaltma kümesi olarak adlandırılan birden çok hata etki, alanlarına çoğaltmalarının kendi kendini yöneten ve dinamik olarak yük dengelemesi yapılmış bir grup olarak tanımdır. Bu verilerin kaynak bölümü içinde yüksek oranda kullanılabilir, dayanıklı ve tutarlı hale getirmek için çoğaltılan durum makine Protokolü topluca uygular. Çoğaltma kümesi üyelik N dinamik – NMin ve regenerate/kurtarmak için hataları, yönetim işlemleri ve süre başarısız çoğaltmalar için temel NMax arasında geciktirmeye tutar. Üyelik değişiklikleri bağlı olarak, çoğaltma, ayrıca yeniden yapılandırır Okuma boyutu protokolünü ve çekirdekleri yazma. İki fikirleri kullanıyoruz birörnek belirtilen kaynak bölüme atanan aktarım hızı dağıtmak için: ilk olarak, öncü yazma isteklerini işleme maliyeti üzerinde İzleyici güncelleştirmelerini uygulama maliyeti daha yüksek. Öncü ayrılmaması gelenlere, takipçi değerinden daha fazla sistem kaynakları. İkincisi, mümkün olduğunca uzak onaylamaktan okuma çekirdek verilen tutarlılık düzeyi için özel olarak İzleyici çoğaltmaları oluşur. Biz okuma gerekmedikçe hizmet vermek için öncü başvurarak kaçının. Bir dizi arasındaki ilişkiyi üzerinde yapılan araştırma fikirlerinden kullanıyoruz [yükü ve kapasite](http://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf) Cosmos DB, beş tutarlılık modelleri için çekirdek tabanlı sistemlerde destekler.  

## <a name="partition-sets"></a>Bölüm-ayarlar

Her Cosmos veritabanı bölge ile yapılandırılmış bir fiziksel bölüm grubu, tüm yapılandırılmış bölgede çoğaltılan anahtarları aynı kümesini yönetmek için oluşur. Bu daha yüksek koordinasyon temel eleman coğrafi olarak dağıtılmış bir dinamik katmana belirli bir anahtar kümesini yönetme fiziksel bölümlerinin bölüm-kümesi - adı verilir. Belirtilen kaynak bölümü (bir çoğaltma kümesi) bir küme içinde kapsamlıdır, ancak bir bölüm kümesi kümeler, yayılabilir veri merkezleri ve aşağıdaki görüntüde gösterildiği gibi coğrafi bölgeler:  

![Bölüm ayarlar](./media/global-dist-under-the-hood/dynamic-overlay-of-resource-partitions.png)

Bir coğrafi olarak dağınık "süper çoğaltma çoğaltma-anahtarları kümesinin aynısına sahip olan birden fazla oluşan kümesi", bölüm bir dizi düşünebilirsiniz. Çoğaltma kümesi, bir bölümü kümenin üyelik ayrıca dinamik benzer – belirli bir bölüm kümesi/yeni bölümler eklemek/kaldırmak için örtük kaynak bölüm yönetimi işlemlerini göre dalgalanma (örneğin, ne zaman ölçeği aktarım hızı üzerinde bir kapsayıcı, bölge, Cosmos veritabanı veya hata gerçekleştiğinde bir ekleme/kaldırma) olması sayesinde her bölüm (kümesinin bir bölüm-) yönetme bölüm kümesi üyelik kendi çoğaltma kümesi içinde üyelik tamamen merkezi olmayan ve yüksek oranda kullanılabilir. Bir bölüm kümesi yeniden yapılandırma sırasında topolojisi, fiziksel bölümler arasında katman da oluşturulur. Topoloji tutarlılık düzeyi, coğrafi uzaklıktan ve kaynak ve hedef fiziksel bölümler arasında kullanılabilir ağ bant genişliği temelinde dinamik olarak seçilir.  

Hizmet bir tek bir yazma bölgesi veya birden çok yazma bölgeleri ile Cosmos veritabanlarınızı yapılandırmanıza olanak tanır ve seçime bağlı olarak, bölüm kümeleri yazma tam olarak bir ya da tüm bölgelerde kabul edecek şekilde yapılandırılmış. İki düzeyli, iç içe geçmiş fikir birliğine varılmış Protokolü sistem kullanır: bir düzey yazma kabul eden bir kaynak bölümü, bir çoğaltma kümesi çoğaltmalarını içinde çalışır ve diğer tüm tam sıralama garantisi sağlamak için bir bölüm kümesi düzeyinde çalışır bölüm kümesindeki taahhüt yazar. Bu çok katmanlı, iç içe geçmiş fikir birliğine varılmış, Cosmos DB, müşterilerine sağlar. tutarlılık modeli uygulamasının yanı sıra, yüksek kullanılabilirlik için katı SLA'larımız uygulanması için önemlidir.  

## <a name="conflict-resolution"></a>Çakışma çözümleme

Güncelleştirme yayma, çakışma çözümü ve izleme nedensellik ilişkilerini bizim tasarımı üzerinde Önceki işten esinlenilmiştir [epidemic algoritmaları](http://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf) ve [Bayou](http://zoo.cs.yale.edu/classes/cs422/2013/bib/terry95managing.pdf) sistem. Fikirleri çekirdekler kurtulan ve Cosmos DB'nin sistem tasarımı iletişim kurmak için kullanışlı bir çerçeve başvuru sağlar, ancak Cosmos DB sistemde uyguladığımız bunları gibi bunlar da önemli dönüştürme yapılmıştır. Önceki sistemleri kaynak İdaresi ne, Cosmos DB çalışmaya (örneğin, sınırlanmış eskime durumu tutarlılık) özellikleri ve katı ve kapsamlı sağlamak ya da ölçekleme ile tasarlanmış çünkü bu, gerekli Cosmos DB, müşterilerine sunduğu SLA'lar.  

Bir bölüm kümesi birden çok bölgede dağıtılır ve belirli bir bölüm kümesi oluşturan fiziksel bölümler arasında veri çoğaltmak için Cosmos Db'ler (çok ana) çoğaltmayı Protokolü aşağıdaki geri çağırma. Her kaynak bölümü (bölüm-kümesi), yazma kabul eder ve bu bölge için yerel istemcileri için okuma tipik olarak görev yapar. Bir bölgede bir kaynak bölümü tarafından kabul edilen yazma arızaya taahhüt verdiniz ve istemciye kabul edildiğini önce kaynak bölümü içinde yüksek oranda kullanılabilir hale. Bunlar belirsiz yazma ve koruma entropi kanalı kullanılarak bölüm kümesi içinde fiziksel diğer bölümlerine yayılır. İstemciler, bir istek üst bilgisi geçirerek belirsiz veya kaydedilmiş yazma isteyebilir. (Yayma sıklığını dahil) koruma entropi yayma dinamik fiziksel bölümler ve tutarlılık düzeyi yapılandırılmış bölüm kümesi, bölgesel yakınlık topolojiye bağlı. Bir bölüm kümesinde Cosmos DB, dinamik olarak seçilen arbiter bölümü olan bir birincil işleme düzeni izler. Arbiter seçimi dinamiktir ve yeniden yapılandırma bölümü kümesinin bir parçası katmana topolojisine dayanır. Taahhüt edilen (çok-row/toplu güncelleştirmeler dahil) yazma sipariş edilmesi garanti edilir. 

Nedensellik ilişkilerini izlemek ve algılamak ve çözmek için vektörleri güncelleştirme çakışması sürümü için kodlanmış vektör saatleri (sırasıyla bölge kodu ve mantıksal saatler her çoğaltma kümesi, fikir birliğine varılmış düzeyine karşılık gelen ve bölüm kümesi içeren) kullanıyoruz. Topoloji ve eş seçimi algoritması, sabit ve en az depolama ve sürüm vektörü, en az bir ağ yükünü sağlamak için tasarlanmıştır. Algoritma katı yakınsama özelliği garanti eder.  

Birden çok yazma bölgeleri ile yapılandırılmış Cosmos veritabanları için sistem esnek otomatik çakışma çözümleme ilkeler arasından, geliştiriciler için de dahil olmak üzere sunar: 

- Son yazma WINS (varsayılan olarak (zaman eşitleme saati protokolünü temel alır) bir sistem tanımlı bir zaman damgası özelliği kullanır, LWW). Cosmos DB Çakışma çözümlemesi için kullanılan diğer özelliklerden herhangi birini özel sayısal belirtmenizi sağlar.  
- Uygulama tanımlı semantiği mutabakat çakışmaları için tasarlanmış olan (birleştirme yordamları ifade edilir) uygulama tanımlı özel çakışma çözüm ilkesi. Bu yordamlar, bir veritabanı işlemi sunucu tarafı şirket himayesinde yazma yazma çakışmaları algılanması üzerine çağrılır. Tam olarak sistemidir taahhüt protokolünün bir parçası olarak bir birleştirme yordamının yürütülmesi için bir kez garanti. Birkaç örneği ile yürütmek kullanılabilir.  

## <a name="consistency-models"></a>Tutarlılık modeli

Tek bir veya birden çok yazma bölgeleri ile Cosmos veritabanı yapılandırma olsun, beş iyi tanımlanmış tutarlılık modellerinden seçebilirsiniz. Yeni eklenen desteği sayesinde, birden fazla yazma bölgeleri etkinleştirmek için birkaç önemli yönlerini tutarlılık düzeyleri şunlardır:  

Olarak önce sınırlanmış eskime durumu tutarlılık tüm okuma k ön eklerine veya t son yazma saniyeler içinde tüm bölgelerin olacağını garanti eder. Ayrıca, sınırlanmış eskime durumu tutarlılık okuma monoton ve tutarlı ön ek Garantisi ile sağlanır. Koruma entropi protokolü, oranı sınırlı bir biçimde çalışır ve ön ekler değil accumulate ve yazma backpressure uygulanacak yok sağlar. Daha önce oturum tutarlılık monoton garanti eder, monoton yazma, kendi yazma okuma gibi yazma aşağıdaki okuma ve tutarlı ön ek dünya genelinde garanti eder. Güçlü tutarlılık ile yapılandırılmış veritabanları için sistem anahtarlara tek bir öncü her bölüm kümelerinin atayarak yazma bölgesi. 

Cosmos DB'de beş tutarlılık modeli semantiği açıklanan [burada](consistency-levels.md) ve üst düzey bir TLA + belirtimlerini kullanarak matematiksel olarak gösterilen [burada](https://github.com/Azure/azure-cosmos-tla).

## <a name="next-steps"></a>Sonraki adımlar

Ardından aşağıdaki makaleleri kullanarak genel dağıtımı yapılandırma işlemleri gerçekleştirmeyi öğreneceksiniz:

* [Birden çok giriş için istemcileri yapılandırma](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Bölge ekleme veya Azure Cosmos DB hesabınızdan kaldırma](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [SQL API hesabı için bir özel çakışma çözüm ilkesi oluşturma](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)
