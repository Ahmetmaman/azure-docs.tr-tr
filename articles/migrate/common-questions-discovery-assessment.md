---
title: Azure geçişi 'nde bulma, değerlendirme ve bağımlılık analizi ile ilgili sorular
description: Azure geçişi 'nde bulma, değerlendirme ve bağımlılık analizi hakkında sık sorulan soruların yanıtlarını alın.
ms.topic: conceptual
ms.date: 06/09/2020
ms.openlocfilehash: cb1696c521f436280177f0263abd66aa2bfed7dc
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92312928"
---
# <a name="discovery-assessment-and-dependency-analysis---common-questions"></a>Keşif, değerlendirme ve bağımlılık analizi-genel sorular

Bu makalede, Azure geçişi 'nde bulma, değerlendirme ve bağımlılık analizleri hakkında sık sorulan sorular yanıtlanmaktadır. Başka sorularınız varsa şu kaynakları kontrol edin:

- Azure geçişi hakkında [genel sorular](resources-faq.md)
- [Azure geçişi](common-questions-appliance.md) gereci hakkında sorular
- [Sunucu geçişi](common-questions-server-migration.md) hakkında sorular
- [Azure geçişi forumundaki](https://aka.ms/AzureMigrateForum) soruların yanıtlarını alın


## <a name="what-geographies-are-supported-for-discovery-and-assessment-with-azure-migrate"></a>Azure geçişi ile bulma ve değerlendirme için hangi coğrafi dosyalar desteklenir?

[Genel](migrate-support-matrix.md#supported-geographies-public-cloud) ve [kamu bulutları](migrate-support-matrix.md#supported-geographies-azure-government) için desteklenen coğrafyaları inceleyin.


## <a name="how-many-vms-can-i-discover-with-an-appliance"></a>Bir gereç ile kaç tane VM keşfedebilirim?

En fazla 10.000 VMware VM, en fazla 5.000 Hyper-V VM ve tek bir gereç kullanarak en fazla 1000 fiziksel sunucu bulabilirsiniz. Daha fazla makineniz varsa, [Hyper-V değerlendirmesi ölçekleme](scale-hyper-v-assessment.md), [bir VMware değerlendirmesi ölçekleme](scale-vmware-assessment.md)veya [fiziksel sunucu değerlendirmesini ölçeklendirme](scale-physical-assessment.md)hakkında bilgi edinin.

## <a name="how-do-i-choose-the-assessment-type"></a>Değerlendirme türünü nasıl seçebilirim?

- Azure VM 'lerine geçiş için şirket içi [VMware VM](how-to-set-up-appliance-vmware.md)'lerinizi, [Hyper-V VM](how-to-set-up-appliance-hyper-v.md)'lerini ve [fiziksel sunucuları](how-to-set-up-appliance-physical.md) değerlendirmek istediğinizde **Azure VM değerlendirmelerini** kullanın. [Daha Fazla Bilgi](concepts-assessment-calculation.md)

- Bu değerlendirme türünü kullanarak [Azure VMware çözümüne (AVS)](../azure-vmware/introduction.md) geçiş için şirket Içi [VMware VM](how-to-set-up-appliance-vmware.md) 'Lerinizi değerlendirmek istediğinizde **Azure VMware çözümü (AVS)** değerlendirmelerini kullanın. [Daha fazla bilgi edinin](concepts-azure-vmware-solution-assessment-calculation.md)

- Her iki tür değerlendirmeyi de çalıştırmak için yalnızca VMware makinelerini içeren ortak bir grup kullanabilirsiniz. Azure Geçişi'nde AVS değerlendirmelerini ilk kez çalıştırıyorsanız, yeni bir VMware makineleri grubu oluşturmanızın önerildiğini aklınızda bulundurun.
 

## <a name="why-is-performance-data-missing-for-someall-vms-in-my-assessment-report"></a>Değerlendirme raporumda bazı/tüm VM'lerin performans verileri neden eksik?

"Performans tabanlı" değerlendirmede Azure Geçişi aleti şirket içi VM’ler için performans verilerini toplayamazsa değerlendirme raporu dışarı aktarmasında 'PercentageOfCoresUtilizedMissing' veya 'PercentageOfMemoryUtilizedMissing' hatası verir. Lütfen şunu denetleyin:

- Değerlendirmeyi oluşturduğunuz süre boyunca VM'lerin açılıp açılmadığı
- Yalnızca bellek sayaçları eksikse ve Hyper-V sanal makinelerini değerlendirmeye çalışıyorsanız, bu sanal makinelerde dinamik belleğin etkinleştirilip etkinleştirilmediğini denetleyin. Bu bilinen bir sorundur ve şu anda Azure Geçişi aletinin bu tür VM'ler için bellek kullanımı bilgilerini toplayamamasından kaynaklanır.
- Tüm performans sayaçları eksikse 443 numaralı bağlantı noktasından giden bağlantılara (HTTPS) izin verildiğinden emin olun.

Not - Performans sayaçlarından biri eksikse, Azure Geçişi: Sunucu Değerlendirmesi şirket içinde ayrılmış çekirdeklere/belleğe geri döner ve buna uygun bir sanal makine boyutu önerir.

## <a name="why-is-the-confidence-rating-of-my-assessment-low"></a>Değerlendirmemin güvenilirlik derecesi neden düşük?

"Performans tabanlı" değerlendirmeler için güvenilirlik derecesi, değerlendirmeyi hesaplarken gereken [kullanılabilir veri noktaları](./concepts-assessment-calculation.md#ratings) yüzdesi temelinde hesaplanır. Değerlendirme güvenilirlik derecesinin düşük olmasının nedenlerini aşağıda bulunabilirsiniz:

- Değerlendirmeyi oluşturduğunuz süre boyunca ortamınızın profilini oluşturmadınız. Örneğin değerlendirmeyi bir hafta olarak ayarlanmış performans süresiyle oluşturuyorsanız, toplanacak tüm veri noktalarının keşfini başlattıktan sonra en az bir hafta beklemeniz gerekir. Süreyi bekleyemiyorsanız, performans süresini daha kısa olacak şekilde değiştirin ve değerlendirmeyi 'Yeniden Hesaplayın'.
 
- Sunucu Değerlendirmesi, değerlendirme süresi içinde VM'lerin bir bölümü veya tümü için performans verilerini toplayamadı. Değerlendirme süresi boyunca VM'lerin açık olup olmadığını, 443 numaralı bağlantı noktasında giden bağlantılara izin verilip verilmediğini denetleyin. Hyper-V VM'leri için dinamik bellek etkinleştirildiyse bellek sayaçları eksik olabilir ve bu da düşük güvenilirlik derecelendirmesine yol açar. Güvenilirlik derecelendirmesindeki en son değişiklikleri yansıtacak şekilde değerlendirmeyi 'Yeniden Hesaplayın'. 

- Sunucu Değerlendirmesi'nde bulma işlemi başlatıldıktan sonra birkaç VM oluşturulmuştur. Örneğin, son bir ayın performans geçmişi için değerlendirme oluşturuyorsanız, ancak yalnızca bir hafta önce ortamda birkaç sanal makine oluşturulduysa. Bu durumda, sürenin tamamında yeni VM'lerin performans verileri sağlanmaz ve güvenilirlik derecesi düşük olabilir.

Güvenilirlik derecesi hakkında [daha fazla bilgi edinin](./concepts-assessment-calculation.md#confidence-ratings-performance-based).

## <a name="i-cant-see-some-groups-when-i-am-creating-an-azure-vmware-solution-avs-assessment"></a>Azure VMware çözümü (AVS) değerlendirmesi oluştururken bazı grupları göremiyorum

- AVS değerlendirmesi yalnızca VMware makineleri içeren gruplarda yapılabilir. AVS değerlendirmesi yapmayı amaçlıyorsanız VMware dışı tüm makineleri gruptan kaldırın.
- Azure Geçişi'nde AVS değerlendirmelerini ilk kez çalıştırıyorsanız, yeni bir VMware makineleri grubu oluşturmanız önerilir.

## <a name="i-cant-see-some-vm-types-in-azure-government"></a>Azure Kamu 'da bazı VM türlerini göremiyorum

Değerlendirme ve geçiş için desteklenen VM türleri, Azure Kamu konumunda kullanılabilirliğine bağlıdır. Azure Kamu 'da VM türlerini [gözden geçirebilir ve karşılaştırabilirsiniz](https://azure.microsoft.com/global-infrastructure/services/?regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia&products=virtual-machines) .

## <a name="the-size-of-my-vm-changed-can-i-run-an-assessment-again"></a>VM 'imin boyutu değişti. Bir değerlendirmeyi yeniden çalıştırabilir miyim?

Azure geçişi gereci, şirket içi ortam hakkındaki bilgileri sürekli olarak toplar.  Değerlendirme, şirket içi VM 'lerin bir zaman noktası anlık görüntüsüdür. Değerlendirmek istediğiniz bir VM 'deki ayarları değiştirirseniz, en son değişikliklerle değerlendirmesi güncelleştirmek için yeniden hesapla seçeneğini kullanın.

## <a name="how-do-i-discover-vms-in-a-multitenant-environment"></a>Çok kiracılı bir ortamda VM 'Leri Nasıl yaparım?.

- **VMware**: bir ortam kiracılar arasında paylaşılmışsa ve bir kiracının başka bir kiracının aboneliğindeki VM 'leri keşfetmeniz gerekmiyorsa, yalnızca keşfetmeyi Istediğiniz VM 'lere erişebilen VMware vCenter Server kimlik bilgileri oluşturun. Daha sonra, Azure geçiş gereci 'nda bulmaya başladığınızda bu kimlik bilgilerini kullanın.
- **Hyper-v**: bulma Işlemi, Hyper-v ana bilgisayar kimlik bilgilerini kullanır. VM 'Ler aynı Hyper-V konağını paylaşıyorsa, şu anda bulmayı ayırmanın bir yolu yoktur.  

## <a name="do-i-need-vcenter-server"></a>VCenter Server ihtiyacım var mı?

Evet, Azure geçişi, bulma işlemini gerçekleştirmek için bir VMware ortamında vCenter Server gerektirir. Azure geçişi, vCenter Server tarafından yönetilmeyen ESXi konaklarının bulunmasını desteklemez.

## <a name="what-are-the-sizing-options-in-an-azure-vm-assessment"></a>Azure VM değerlendirmesinde boyutlandırma seçenekleri nelerdir?

Şirket içi olarak boyutlandırılması sayesinde Azure geçişi, değerlendirme için VM performans verilerini kabul etmez. Azure değerlendirir VM boyutlarını şirket içi yapılandırmaya göre geçirin. Performans tabanlı boyutlandırma ile boyutlandırma, kullanım verilerini temel alır.

Örneğin, bir şirket içi VM 'nin %50 CPU kullanımı ve %50 bellek kullanımı için dört çekirdeğe ve 8 GB belleğe sahip olması durumunda:
- Şirket içi boyutlandırma, dört çekirdeğe ve 8 GB belleğe sahip bir Azure VM SKU 'SU önerir.
- Performans tabanlı boyutlandırma, kullanım yüzdesi kabul edildiği için iki çekirdeğe ve 4 GB belleğe sahip bir VM SKU 'SU önermenizi sağlayacak.

Benzer şekilde, disk boyutlandırma, boyutlandırma ölçütlerine ve depolama türüne bağlıdır:
- Boyutlandırma ölçütü performans tabanlıdır ve depolama türü otomatik ise, Azure geçişi, hedef disk türünü (Standart veya Premium) belirlediğinde, diskin ıOPS ve aktarım hızı değerlerini hesaba göre alır.
- Boyutlandırma ölçütü performans tabanlıdır ve depolama türü Premium ise, Azure geçişi, şirket içi diskin boyutuna bağlı olarak bir Premium disk SKU 'SU önerir. Aynı mantık, boyutlandırma şirket içinde olduğunda ve depolama türü standart veya Premium olduğunda disk boyutlandırılmasına uygulanır.

## <a name="does-performance-history-and-utilization-affect-sizing-in-an-azure-vm-assessment"></a>Performans geçmişi ve kullanımı bir Azure VM değerlendirmesinde boyutlandırmayı etkiler mi?

Evet, performans geçmişi ve kullanımı bir Azure VM değerlendirmesinde boyutlandırmayı etkiler.

### <a name="performance-history"></a>Performans geçmişi

Yalnızca performans tabanlı boyutlandırma için Azure geçişi, şirket içi makinelerin performans geçmişini toplar ve ardından Azure 'da VM boyutunu ve disk türünü önermek için kullanır:

1. Gereç, her 20 saniyede bir gerçek zamanlı kullanım verilerini toplamak için şirket içi ortamı sürekli olarak profillerdir.
2. Gereç, toplanan 20 saniyelik örnekleri kaydeder ve bunları 15 dakikada bir tek bir veri noktası oluşturmak için kullanır.
3. Veri noktasını oluşturmak için gereç tüm 20 saniyelik örneklerden tepe değeri seçer.
4. Gereç, veri noktasını Azure 'a gönderir.

### <a name="utilization"></a>Kullanım

Azure 'da bir değerlendirme oluşturduğunuzda, performans süresine ve ayarlanan performans geçmişi yüzdebirlik değerine bağlı olarak, Azure geçişi etkin kullanım değerini hesaplar ve ardından boyutlandırmak için kullanır.

Örneğin, performans süresini bir güne ve yüzdebirlik değerini 95. yüzdebirlik olarak ayarlarsanız, Azure geçişi, son gün için toplayıcı tarafından gönderilen 15 dakikalık örnek noktalarını artan düzende sıralar. Etkin kullanım olarak 95. yüzdebirlik değerini seçer.

95. yüzdebirlik değerinin kullanılması, aykırı değerlerin yoksayılmasını sağlar. Azure geçirilmesi 99. yüzdebirlik ' i kullanıyorsa, dış değerler dahil edilebilir. Tüm aykırı değerleri içermeyen dönemin en yoğun kullanımını seçmek için Azure geçişi ' ni 99. yüzdebirlik 'i kullanacak şekilde ayarlayın.


## <a name="how-are-import-based-assessments-different-from-assessments-with-discovery-source-as-appliance"></a>İçeri aktarma tabanlı değerlendirmeler, bulgu kaynağı olan değerlendirmelere gereç olarak nasıl farklıdır?

İçeri aktarma tabanlı Azure VM değerlendirmeleri, bir CSV dosyası kullanarak Azure geçişi 'ne aktarılan makinelerle oluşturulan değerlendirmelerdir. İçeri aktarmanın yalnızca dört alanı zorunludur: sunucu adı, çekirdekler, bellek ve işletim sistemi. Dikkat edilmesi gereken bazı noktalar şunlardır: 
 - Hazırlık ölçütleri, önyükleme türü parametresindeki içeri aktarma tabanlı değerlendirmelere daha az sıkıdır. Önyükleme türü sağlanmazsa, makinenin BIOS önyükleme türü olduğu ve makinenin **koşullu**olarak işaretli olmadığı varsayılır. Bulgu kaynağı bir gereç olarak bulunan değerlendirmelerinde, önyükleme türü eksikse hazır olma durumu **koşullu** olarak işaretlenir. Hazırlık hesaplamasında bu fark, kullanıcıların içeri aktarma tabanlı değerlendirmeler tamamlandığında geçiş planlamasının erken aşamalarında makinelerde tüm bilgilere sahip olmaması olabilir. 
 - Performans tabanlı içeri aktarma değerlendirmeleri, doğru boyutlandırma hesaplamaları için Kullanıcı tarafından sunulan kullanım değerini kullanır. Kullanım değeri Kullanıcı tarafından sağlandığı için, değerlendirme özelliklerinde **performans geçmişi** ve **yüzdebirlik kullanım** seçenekleri devre dışıdır. Bulgu kaynağı bir gereç olarak bulunan değerlendirmelerinde, seçilen yüzdebirlik değeri gereç tarafından toplanan performans verilerinden çekilir.

## <a name="why-is-the-suggested-migration-tool-in-import-based-avs-assessment-marked-as-unknown"></a>İçeri aktarma tabanlı AVS değerlendirmesi bilinmeyen olarak işaretlenmiş olan geçiş aracı neden bilinmiyor?

Bir CSV dosyası aracılığıyla içeri aktarılan makineler için, bir AVS değerlendirmesinde varsayılan geçiş aracı bilinmez. Ancak, VMware makinelerinde, VMware karma bulut uzantısı (HCX) çözümünün kullanılması önerilir. [Daha Fazla Bilgi Edinin](../azure-vmware/tutorial-deploy-vmware-hcx.md).


## <a name="what-is-dependency-visualization"></a>Bağımlılık görselleştirmesi nedir?

Bağımlılık görselleştirmesi, daha fazla güvenle geçirilecek VM gruplarını değerlendirmenize yardımcı olabilir. Bağımlılık görselleştirmesi, değerlendirme çalıştırmadan önce makine bağımlılıklarını çapraz denetler. Bu, herhangi bir şeyin gerisinde kalmasını sağlamaya yardımcı olur ve Azure 'a geçiş yaparken beklenmedik kesintilerden kaçınmaya yardımcı olur. Azure geçişi, bağımlılık görselleştirmesini etkinleştirmek için Azure Izleyici 'de Hizmet Eşlemesi çözümünü kullanır. [Daha fazla bilgi edinin](concepts-dependency-visualization.md).

> [!NOTE]
> Aracı tabanlı bağımlılık analizi, Azure Kamu 'da kullanılamaz. Aracısız bağımlılık analizini kullanabilirsiniz

## <a name="whats-the-difference-between-agent-based-and-agentless"></a>Aracı tabanlı ve aracısız arasındaki fark nedir?

Aracısız görselleştirme ve aracı tabanlı görselleştirme arasındaki farklılıklar tabloda özetlenmiştir.

**Gereksinim** | **Aracısız** | **Aracı tabanlı**
--- | --- | ---
Destek | Bu seçenek şu anda önizleme aşamasındadır ve yalnızca VMware VM 'Leri için kullanılabilir. Desteklenen işletim sistemlerini [gözden geçirin](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless) . | Genel kullanılabilirlik (GA).
Aracı | Çapraz denetlemek istediğiniz makinelere aracı yüklemeye gerek yoktur. | Çözümlemek istediğiniz her şirket içi makineye yüklenecek aracılar: [Microsoft Monitoring Agent (MMA)](../azure-monitor/platform/agent-windows.md)ve [bağımlılık Aracısı](../azure-monitor/platform/agents-overview.md#dependency-agent). 
Ön koşullar | Önkoşulları ve dağıtım gereksinimlerini [gözden geçirin](concepts-dependency-visualization.md#agentless-analysis) . | Önkoşulları ve dağıtım gereksinimlerini [gözden geçirin](concepts-dependency-visualization.md#agent-based-analysis) .
Log Analytics | Gerekli değildir. | Azure geçişi, bağımlılık görselleştirmesi için [Azure izleyici günlüklerinde](../azure-monitor/log-query/log-query-overview.md) [hizmet eşlemesi](../azure-monitor/insights/service-map.md) çözümünü kullanır. [Daha fazla bilgi edinin](concepts-dependency-visualization.md#agent-based-analysis).
Nasıl çalışır? | Bağımlılık görselleştirmesi için etkinleştirilen makinelerde TCP bağlantı verilerini yakalar. Bulmadan sonra, verileri beş dakikalık aralıklarla toplar. | Bir makineye yüklü Hizmet Eşlemesi aracılar, her bir işlem için TCP işlemleri ve gelen/giden bağlantılarla ilgili verileri toplar.
Veriler | Kaynak makine sunucu adı, işlem, uygulama adı.<br/><br/> Hedef makine sunucu adı, işlem, uygulama adı ve bağlantı noktası. | Kaynak makine sunucu adı, işlem, uygulama adı.<br/><br/> Hedef makine sunucu adı, işlem, uygulama adı ve bağlantı noktası.<br/><br/> Bağlantı sayısı, gecikme süresi ve veri aktarımı bilgilerinin toplanması ve Log Analytics sorguları için kullanılabilir olması. 
Görselleştirme | Tek bir sunucunun bağımlılık eşlemesi, bir saat ile 30 güne kadar bir süre içinde görüntülenebilir. | Tek bir sunucunun bağımlılık eşlemesi.<br/><br/> Eşleme, yalnızca bir saat boyunca görüntülenebilir.<br/><br/> Bir sunucu grubunun bağımlılık eşlemesi.<br/><br/> Harita görünümünden bir gruptaki sunucuları ekleyin ve kaldırın.
Veri dışarı aktarma | Son 30 günlük veri, CSV biçiminde indirilebilir. | Veriler, Log Analytics ile sorgulanabilir.


## <a name="do-i-need-to-deploy-the-appliance-for-agentless-dependency-analysis"></a>Aracısız bağımlılık analizine yönelik gereci dağıtmem gerekir mi?

Evet, [Azure geçişi](migrate-appliance.md) gerecinin dağıtılması gerekir.

## <a name="do-i-pay-for-dependency-visualization"></a>Bağımlılık görselleştirmesi için ücret ödemem gerekiyor mu?

Hayır. [Azure geçişi fiyatlandırması](https://azure.microsoft.com/pricing/details/azure-migrate/)hakkında daha fazla bilgi edinin.

## <a name="what-do-i-install-for-agent-based-dependency-visualization"></a>Aracı tabanlı bağımlılık görselleştirmesi için ne yükleyebilirim?

Aracı tabanlı bağımlılık görselleştirmesini kullanmak için, değerlendirmek istediğiniz her şirket içi makineye aracıları indirip yükleyin:

- [Microsoft Monitoring Agent (MMA)](../azure-monitor/platform/agent-windows.md)
- [Bağımlılık aracısı](../azure-monitor/platform/agents-overview.md#dependency-agent)
- İnternet bağlantısı olmayan makineleriniz varsa Log Analytics ağ geçidini indirip yükleyin.

Bu aracılara yalnızca aracı tabanlı bağımlılık görselleştirmesi kullanıyorsanız ihtiyacınız vardır.

## <a name="can-i-use-an-existing-workspace"></a>Mevcut bir çalışma alanını kullanabilir miyim?

Evet, aracı tabanlı bağımlılık görselleştirmesi için, var olan bir çalışma alanını geçiş projesine iliştirebilir ve bunu bağımlılık görselleştirmesi için kullanabilirsiniz. 

## <a name="can-i-export-the-dependency-visualization-report"></a>Bağımlılık görselleştirme raporunu dışarı aktarabilir miyim?

Hayır, aracı tabanlı görselleştirmede bağımlılık görselleştirme raporu verilemez. Bununla birlikte, Azure geçişi Hizmet Eşlemesi kullanır ve bağımlılıkları JSON biçiminde almak için [Hizmet Eşlemesi REST API](/rest/api/servicemap/machines/listconnections) de kullanabilirsiniz.

## <a name="can-i-automate-agent-installation"></a>Aracı yüklemesini otomatikleştirebilirim miyim?

Aracı tabanlı bağımlılık görselleştirmesi için:

- [Bağımlılık aracısını yüklemek için bir komut dosyası](../azure-monitor/insights/vminsights-enable-hybrid.md#dependency-agent)kullanın.
- MMA için [komut satırını veya Otomasyonu kullanın](../azure-monitor/platform/log-analytics-agent.md#installation-options)veya bir [komut dosyası](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab)kullanın.
- Betiklerin yanı sıra, aracıları dağıtmak için Microsoft uç nokta Configuration Manager ve [ıntigua](https://www.intigua.com/intigua-for-azure-migration) gibi dağıtım araçlarını kullanabilirsiniz.

## <a name="what-operating-systems-does-mma-support"></a>MMA hangi işletim sistemlerini destekler?

- [MMA 'nın desteklediği Windows işletim sistemlerinin](../azure-monitor/platform/log-analytics-agent.md#installation-options)listesini görüntüleyin.
- [MMA 'nın desteklediği Linux işletim sistemlerinin](../azure-monitor/platform/log-analytics-agent.md#installation-options)listesini görüntüleyin.

## <a name="can-i-visualize-dependencies-for-more-than-one-hour"></a>Bağımlılıkları birden fazla saat için görselleştirebilir miyim?

Aracı tabanlı görselleştirme için, bağımlılıkları bir saate kadar görselleştirebilirsiniz. Geçmişte belirli bir tarihe kadar bir ay kadar geri dönebilirsiniz, ancak görselleştirme için en uzun süre bir saat olabilir. Örneğin, dün bağımlılıklarını görüntülemek için bağımlılık eşlemesindeki süre süresini kullanabilirsiniz, ancak yalnızca bir saatlik pencerenin bağımlılıklarını görüntüleyebilirsiniz. Ancak, daha uzun bir süre boyunca [bağımlılık verilerini sorgulamak](./how-to-create-group-machine-dependencies.md) Için Azure izleyici günlüklerini kullanabilirsiniz.

Aracısız görselleştirme için, tek bir sunucunun bağımlılık haritasını bir saat ile 30 gün arasında bir süre izleyebilirsiniz.

## <a name="can-i-visualize-dependencies-for-groups-of-more-than-10-vms"></a>10 ' dan fazla VM 'nin bağımlılıklarını görselleştirebilir miyim?

10 adede kadar VM 'ye sahip olan gruplar için [bağımlılıkları görselleştirebilirsiniz](./how-to-create-a-group.md#refine-a-group-with-dependency-mapping) . 10 ' dan fazla VM 'ye sahip bir grubunuz varsa, grubu daha küçük gruplara bölmeniz ve sonra bağımlılıkları görselleştirmenizi öneririz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure geçişi 'ne genel bakış](migrate-services-overview.md)konusunu okuyun.