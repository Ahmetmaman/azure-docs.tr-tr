---
title: Azure geçişi gereç hakkında SSS
description: Azure geçişi gereci hakkında sık sorulan soruların yanıtlarını alın.
ms.topic: conceptual
ms.date: 09/15/2020
ms.openlocfilehash: 7839c2678152837cc9217e6afe13f7cca36ab4b0
ms.sourcegitcommit: d479ad7ae4b6c2c416049cb0e0221ce15470acf6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91630500"
---
# <a name="azure-migrate-appliance-common-questions"></a>Azure geçişi gereci: genel sorular

Bu makalede, Azure geçişi gereci hakkında sık sorulan sorular yanıtlanmaktadır. Başka sorularınız varsa şu kaynakları kontrol edin:

- Azure geçişi hakkında [genel sorular](resources-faq.md)
- [Bulma, değerlendirme ve bağımlılık görselleştirmesiyle](common-questions-discovery-assessment.md) ilgili sorular
- [Sunucu geçişi](common-questions-server-migration.md) hakkında sorular
- [Azure geçişi forumundaki](https://aka.ms/AzureMigrateForum) soruların yanıtlarını alın

## <a name="what-is-the-azure-migrate-appliance"></a>Azure geçişi gereci nedir?

Azure geçişi gereci, Azure geçişi: Sunucu değerlendirmesi aracının, şirket içi veya herhangi bir buluttan fiziksel veya sanal sunucuları keşfetmek ve değerlendirmek için kullandığı hafif bir gereç. Azure geçişi: sunucu geçiş aracı Ayrıca şirket içi VMware VM 'lerinin aracısız geçişi için gereci kullanır.

Azure geçişi gereci hakkında daha fazla bilgi edinebilirsiniz:

- Gereç, şirket içinde bir VM veya fiziksel makine olarak dağıtılır.
- Gereç, şirket içi makineleri bulur ve sürekli olarak makine meta verilerini ve performans verilerini Azure geçişi 'ne gönderir.
- Gereç keşfi aracısız. Bulunan makinelerde hiçbir şey yüklü değil.

Gereç hakkında [daha fazla bilgi edinin](migrate-appliance.md) .

## <a name="how-can-i-deploy-the-appliance"></a>Gereci nasıl dağıtırım?

Gereç şu şekilde dağıtılabilir:

- VMware VM 'lerini bulmak için şablon kullanma (. OVA dosyası) ve Hyper-V VM 'Leri (. VHD dosyası) gereci barındıran yeni bir VM oluşturun.
- Bir şablon kullanmak istemiyorsanız, bir PowerShell yükleyici betiği kullanarak VMware VM 'leri veya Hyper-V VM 'lerini bulmak için mevcut bir fiziksel veya sanal makineye, portaldan bir ZIP dosyasında indirilebilir.
- Şirket içi veya herhangi bir buluttan fiziksel veya sanal sunucular için gereci, var olan bir sunucuda bir komut dosyası kullanarak her zaman dağıtırsınız.
- Azure Kamu için, üç gereçlerin hepsi yalnızca PowerShell yükleyici betiği kullanılarak dağıtılabilir.

## <a name="how-does-the-appliance-connect-to-azure"></a>Gereç Azure 'a nasıl bağlanır?

Gereç Internet üzerinden veya Azure ExpressRoute kullanarak bağlanabilir. Bu [URL 'Lerin](https://docs.microsoft.com/azure/migrate/migrate-appliance#url-access) Azure 'a bağlanmasına yönelik gereç için onaylanmış olduğundan emin olun.

- Azure ExpressRoute 'u Azure geçişi çoğaltma trafiği için kullanmak üzere Microsoft eşlemesi veya var olan bir genel eşleme gerekir (genel eşleme, yeni ER oluşturmaları için kullanım dışıdır).
- Azure ExpressRoute üzerinden (yalnızca) özel eşleme etkin olan çoğaltma desteklenmez.

Microsoft eşlemesi yapılandırılmış Azure ExpressRoute, çoğaltma trafiği için önerilen yönlendirme etki alanıdır.

## <a name="does-appliance-analysis-affect-performance"></a>Gereç Analizi performansı etkiler mi?

Azure, Gereç profillerini şirket içi makineleri performans verilerini ölçmek için sürekli olarak geçirin. Bu profil oluşturma, profili oluşturulmuş makinelerde neredeyse performans etkisine sahip değildir.

## <a name="can-i-harden-the-appliance-vm"></a>Gereç sanal makinesini kullanabilir miyim?

Gereç sanal makinesini oluşturmak için indirilen şablonu kullandığınızda, Azure geçişi gereci için gerekli olan iletişim ve güvenlik duvarı kurallarını yerinde bırakırsanız şablona bileşen ekleyebilirsiniz (örneğin, virüsten koruma).

## <a name="what-network-connectivity-is-required"></a>Hangi ağ bağlantısı gerekir?

Gerecin Azure URL 'Lerine erişmesi gerekiyor. URL listesini [gözden geçirin](migrate-appliance.md#url-access) .

## <a name="what-data-does-the-appliance-collect"></a>Gereç hangi verileri toplar?

Azure geçişi gerecinin VM 'lerde topladığı veriler hakkında bilgi edinmek için aşağıdaki makalelere bakın:

- **VMware VM**: toplanan verileri [gözden geçirin](migrate-appliance.md#collected-data---vmware) .
- **Hyper-V VM**: toplanan verileri [gözden geçirin](migrate-appliance.md#collected-data---hyper-v) .
- **Fiziksel veya sanal sunucular**: toplanan verileri[gözden geçirin](migrate-appliance.md#collected-data---physical) .

## <a name="how-is-data-stored"></a>Veriler nasıl depolanır?

Azure geçişi gereci tarafından toplanan veriler, Azure geçişi projesini oluşturduğunuz Azure konumunda depolanır.

Verilerin nasıl depolandığı hakkında daha fazla bilgi aşağıdadır:

- Toplanan veriler, CosmosDB 'de bir Microsoft aboneliğine güvenli bir şekilde depolanır. Azure geçişi projesini sildiğinizde veriler silinir. Depolama, Azure geçişi tarafından işlenir. Toplanan veriler için özel olarak bir depolama hesabı seçemezsiniz.
- [Bağımlılık görselleştirmesi](concepts-dependency-visualization.md)kullanıyorsanız, toplanan veriler Azure aboneliğinizde oluşturulan bir Azure Log Analytics çalışma alanında depolanır. Aboneliğinizdeki Log Analytics çalışma alanını sildiğinizde veriler silinir. 

## <a name="how-much-data-is-uploaded-during-continuous-profiling"></a>Sürekli profil oluşturma sırasında karşıya ne kadar veri yüklendi?

Azure geçişi 'ne gönderilen verilerin hacmi birden çok parametreye bağlıdır. Örnek olarak, 10 makineye sahip bir Azure geçişi projesi (her biri bir disk ve bir NIC ile) günde yaklaşık 50 MB veri gönderir. Bu değer yaklaşık değerlerdir; Gerçek değer, disklerin ve NIC 'Lerin veri noktalarının sayısına bağlı olarak değişir. Makinelerin, disklerin veya NIC 'lerin sayısı arttıkça, gönderilen verilerdeki artış doğrusal değildir.

## <a name="is-data-encrypted-at-rest-and-in-transit"></a>Veriler, bekleyen ve aktarım sırasında şifrelenir mi?

Evet, her ikisi için:

- Meta veriler, HTTPS üzerinden Internet üzerinden Azure geçişi hizmetine güvenli bir şekilde gönderilir.
- Meta veriler bir [Azure Cosmos](../cosmos-db/database-encryption-at-rest.md) veritabanında ve [Azure Blob depolamada](../storage/common/storage-service-encryption.md) bir Microsoft aboneliğine depolanır. Meta veriler, depolama için bekleyen bir şekilde şifrelenir.
- Bağımlılık Analizi verileri de aktarım sırasında şifrelenir (güvenli HTTPS tarafından). Bu, aboneliğinizdeki bir Log Analytics çalışma alanında depolanır. Veriler, bağımlılık analizi için geri kalanında şifrelenir.

## <a name="how-does-the-appliance-connect-to-vcenter-server"></a>Gereç vCenter Server nasıl bağlanır?

Bu adımlar, gerecin VMware vCenter Server nasıl bağlandığını anlatmaktadır:

1. Gereç, gereci ayarlarken verdiğiniz kimlik bilgilerini kullanarak vCenter Server (bağlantı noktası 443) öğesine bağlanır.
2. Gereç, vCenter Server tarafından yönetilen VM 'Lerle ilgili meta verileri toplamak üzere vCenter Server sorgulamak için VMware PowerCLI kullanır.
3. Gereç, sanal makineler (çekirdek, bellek, diskler, NIC 'Ler) ve geçmiş ay için her bir VM 'nin performans geçmişi hakkında yapılandırma verileri toplar.
4. Toplanan meta veriler, değerlendirme için Azure geçişi: Sunucu değerlendirmesi aracına (HTTPS üzerinden Internet üzerinden) gönderilir.

## <a name="can-the-azure-migrate-appliance-connect-to-multiple-vcenter-servers"></a>Azure geçişi gereci birden çok vCenter sunucusuna mi bağlanabilir?

Hayır. [Azure geçişi](migrate-appliance.md) gereci ve vCenter Server arasında bire bir eşleme var. Birden çok vCenter Server örneğinde VM 'Leri saptamak için birden çok gereç dağıtmanız gerekir. 

## <a name="can-an-azure-migrate-project-have-multiple-appliances"></a>Bir Azure geçişi projesi birden çok gereçte sahip olabilir mi?

Bir projede birden fazla gereç eklenmiş olabilir. Ancak, bir gereç yalnızca bir projeyle ilişkilendirilebilir. 

## <a name="can-the-azure-migrate-appliancereplication-appliance-connect-to-the-same-vcenter"></a>Azure geçiş gereci/çoğaltma gereci aynı vCenter 'a bağlanmasına mi?

Evet. Hem Azure geçiş gereci (değerlendirme ve aracısız VMware geçişi için kullanılır) hem de çoğaltma gereci (VMware VM 'lerinin aracı tabanlı geçişi için kullanılır) aynı vCenter sunucusuna ekleyebilirsiniz. Ancak, aynı VM üzerinde her iki gereç de ayarlamadığınızdan emin olun ve şu anda desteklenmemektedir.

## <a name="how-many-vms-or-servers-can-i-discover-with-an-appliance"></a>Bir gereç ile kaç VM veya sunucu keşfedebilirim?

En fazla 10.000 VMware VM, en fazla 5.000 Hyper-V VM ve tek bir gereç ile en fazla 1000 fiziksel sunucu bulabilirsiniz. Şirket içi ortamınızda daha fazla makineniz varsa, [Hyper-V değerlendirmesi ölçekleme](scale-hyper-v-assessment.md), [VMware değerlendirmesi ölçekleme](scale-vmware-assessment.md)ve [fiziksel sunucu değerlendirmesini ölçeklendirme](scale-physical-assessment.md)hakkında bilgi edinin.

## <a name="can-i-delete-an-appliance"></a>Bir gereci silebilir miyim?

Şu anda, bir gereci projeden silme desteklenmez.

Gereci silmenin tek yolu, Gereç ile ilişkili Azure geçişi projesini içeren kaynak grubunu silmektir.

Ancak, kaynak grubu silindiğinde diğer kayıtlı gereçler, bulunan envanter, değerlendirmeler ve proje ile ilişkili kaynak grubundaki diğer tüm Azure bileşenleri de silinir.

## <a name="can-i-use-the-appliance-with-a-different-subscription-or-project"></a>Gereci farklı bir abonelik ya da projeyle kullanabilir miyim?

Gereci farklı bir abonelik veya projeyle kullanmak için, Gereç makinesindeki belirli bir senaryo (VMware/Hyper-V/fiziksel) için PowerShell yükleyicisi betiğini çalıştırarak mevcut gereci yeniden yapılandırmanız gerekir. Komut dosyası, yeni bir gereç dağıtmak için mevcut gereç bileşenlerini ve ayarlarını temizler. Yeni dağıtılan gereç yapılandırma yöneticisini kullanmaya başlamadan önce lütfen tarayıcı önbelleğini temizlemeniz gerekir.

Ayrıca, yeniden yapılandırılmış bir gereç üzerinde mevcut bir Azure geçişi proje anahtarını yeniden kullanamazsınız. Gereç kaydını tamamlayabilmeniz için istenen abonelik/projeden yeni bir anahtar oluşturmadığınızdan emin olun.

## <a name="can-i-set-up-the-appliance-on-an-azure-vm"></a>Gereci bir Azure VM üzerinde ayarlayabilir miyim?

Hayır. Şu anda bu seçenek desteklenmez. 

## <a name="can-i-discover-on-an-esxi-host"></a>ESXi konağında bulunabilir miyim?

Hayır. VMware VM 'lerini öğrenmek için vCenter Server sahip olmanız gerekir.

## <a name="how-do-i-update-the-appliance"></a>Nasıl yaparım? gereç güncelleştirilsin mi?

Varsayılan olarak, Gereç ve yüklü aracıları otomatik olarak güncelleştirilir. Gereç, güncelleştirmeleri 24 saatte bir denetler. Başarısız olan güncelleştirmeler yeniden denenir. 

Yalnızca Gereç ve gereç aracıları bu otomatik güncelleştirmeler tarafından güncelleştirilir. İşletim sistemi, Azure tarafından otomatik güncelleştirme geçişi tarafından güncelleştirilmedi. İşletim sistemini güncel tutmak için Windows güncelleştirmelerini kullanın.

## <a name="can-i-check-agent-health"></a>Aracı sistem durumunu denetleyebilir miyim?

Evet. Portalda Azure geçişi: Sunucu değerlendirmesi veya Azure geçişi: sunucu geçiş aracı için **Aracı sistem durumu** sayfasına gidin. Burada, aracıda Azure ile bulma ve değerlendirme aracıları arasındaki bağlantı durumunu kontrol edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure geçişi 'ne genel bakış](migrate-services-overview.md)konusunu okuyun.
