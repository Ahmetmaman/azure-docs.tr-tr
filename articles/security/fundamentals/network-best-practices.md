---
title: Ağ güvenliği için en iyi uygulamalar-Microsoft Azure
description: Bu makalede, yerleşik Azure özelliklerini kullanarak ağ güvenliği için en iyi yöntemler sunulmaktadır.
services: security
author: TerryLanfear
manager: barbkess
editor: TomShinder
ms.assetid: 7f6aa45f-138f-4fde-a611-aaf7e8fe56d1
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/02/2019
ms.author: TomSh
ms.openlocfilehash: 4793216a12b17c4e4ea03f62d5a0ba512febc232
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101735735"
---
# <a name="azure-best-practices-for-network-security"></a>Ağ güvenliği için en iyi Azure yöntemleri
Bu makalede, ağ güveninizi geliştirmeye yönelik bir Azure en iyi yöntemleri koleksiyonu ele alınmaktadır. Bu en iyi uygulamalar, Azure ağ deneyimimizden ve bizim gibi müşterilerin deneyimlerimizden türetilir.

Her bir en iyi uygulama için, bu makalede şunları açıklanmaktadır:

* En iyi yöntem
* Bu en iyi yöntemi neden etkinleştirmek istiyorsunuz?
* En iyi uygulamayı etkinleştiremeyebilirsiniz ne olur?
* En iyi yöntem için olası alternatifler
* En iyi uygulamayı nasıl etkinleştirebileceğinizi öğrenin

Bu en iyi uygulamalar, bu makalede yazıldığı sırada olduğu gibi, bir konsensus görüşlerine ve Azure platformu özelliklerine ve özellik kümelerine dayanır. Değişiklikler ve teknolojiler zamana göre değişir ve bu makale, bu değişiklikleri yansıtacak şekilde düzenli olarak güncelleştirilir.

## <a name="use-strong-network-controls"></a>Güçlü ağ denetimleri kullanma
Azure sanal [makinelerini (VM 'ler)](https://azure.microsoft.com/services/virtual-machines/) ve gereçler 'ı [Azure sanal ağlarına](../../virtual-network/index.yml)yerleştirerek diğer ağa bağlı cihazlara bağlayabilirsiniz. Diğer bir deyişle, ağ özellikli cihazlar arasında TCP/IP tabanlı iletişime izin vermek için sanal ağ arabirim kartlarını bir sanal ağa bağlayabilirsiniz. Bir Azure sanal ağına bağlı sanal makineler, aynı sanal ağ, farklı sanal ağlar, internet veya kendi şirket içi ağlarınızdaki cihazlara bağlanabilir.

Ağınızı ve ağınızın güvenliğini planlarken şunları merkezileştirmenizi öneririz:

- ExpressRoute, sanal ağ ve alt ağ sağlama ve IP adresleme gibi çekirdek ağ işlevlerinin yönetimi.
- ExpressRoute, sanal ağ ve alt ağ sağlama ve IP adresleme gibi ağ sanal gereç işlevleri gibi ağ güvenlik öğelerinin İdaresi.

Ağınızı ve ağınızın güvenliğini izlemek için ortak bir yönetim araçları kümesi kullanıyorsanız, her ikisine de açık görünürlük alırsınız. Basit ve Birleşik bir güvenlik stratejisi, insan anlama ve otomasyonunun güvenilirliğini artırdığı için hataları azaltır.

## <a name="logically-segment-subnets"></a>Mantıksal olarak segment alt ağları
Azure sanal ağları, şirket içi ağınızdaki lan 'Lara benzer. Bir Azure sanal ağının arkasındaki düşünce, tüm Azure sanal makinelerinizi yerleştirebileceğiniz tek bir özel IP adresi alanına göre bir ağ oluşturmanız olabilir. Kullanılabilir özel IP adresi alanları A (10.0.0.0/8), sınıf B (172.16.0.0/12) ve sınıf C (192.168.0.0/16) aralıklarında Sınıflarındedir.

Alt ağları mantıksal olarak parçalara ayırma için en iyi uygulamalar şunlardır:

**En iyi yöntem**: geniş aralıklara izin verme kuralları atamayın (örneğin, 0.0.0.0 ila 255.255.255.255 'ye izin verin).  
**Ayrıntı**: sorun giderme yordamlarının bu tür kuralları ayarlamayı önleyin veya yasaklaın. Bu kurallar, önemli bir güvenlik hissi oluşmasına neden olur ve kırmızı takımlar tarafından sık olarak bulunur ve bu açıktan yararlanabilir.

**En iyi yöntem**: daha büyük adres alanını alt ağlara bölüler.   
**Ayrıntı**: alt ağlarınızı oluşturmak için [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)tabanlı alt ağ oluşturma ilkelerini kullanın.

**En iyi yöntem**: alt ağlar arasında ağ erişim denetimleri oluşturun. Alt ağlar arasında yönlendirme otomatik olarak gerçekleşir ve yönlendirme tablolarını el ile yapılandırmanız gerekmez. Varsayılan olarak, bir Azure sanal ağında oluşturduğunuz alt ağlar arasında ağ erişim denetimi yoktur.   
**Ayrıntı**: Azure alt ağlarındaki istenmeyen trafiğe karşı korumak için bir [ağ güvenlik grubu](../../virtual-network/virtual-network-vnet-plan-design-arm.md) kullanın. Ağ güvenlik grupları, ağ trafiği için izin verme/reddetme kuralları oluşturmak için 5 tanımlama grubu yaklaşımını (kaynak IP, kaynak bağlantı noktası, hedef IP, hedef bağlantı noktası ve katman 4 Protokolü) kullanan basit, durum bilgisi olmayan paket denetleme cihazlarıdır. Tek bir IP adresine, birden fazla IP adresine veya, tüm alt ağlardan/veya bunların tamamına giden trafiğe izin verir veya bu adresi engelleyin.

Alt ağlar arasında ağ erişim denetimi için ağ güvenlik grupları 'nı kullandığınızda, kendi alt ağlarında aynı güvenlik bölgesine veya rolüne ait olan kaynakları yerleştirebilirsiniz.

**En iyi yöntem**: basitlik ve esneklik sağlamak için küçük sanal ağlardan ve alt ağlardan kaçının.   
**Ayrıntı**: çoğu kuruluş başlangıçta planlanandan daha fazla kaynak ekler ve adreslerin yeniden tahsis etme işlemi yoğun olur. Küçük alt ağların kullanılması sınırlı güvenlik değeri ekler ve bir ağ güvenlik grubunu her alt ağ ile eşleyerek ek yük ekler. Büyüme esnekliğine sahip olduğunuzdan emin olmak için alt ağları geniş bir şekilde tanımlayın.

**En iyi yöntem**: [uygulama güvenlik gruplarını](https://azure.microsoft.com/blog/applicationsecuritygroups/)tanımlayarak ağ güvenlik grubu kural yönetimini kolaylaştırın.  
**Ayrıntı**: gelecekte değiştirebileceğiniz veya birçok ağ güvenlik grubu genelınde kullanılabilecek IP adresleri listeleri Için bir uygulama güvenlik grubu tanımlayın. Başkalarının içeriğini ve amacını anlayabilmesi için uygulama güvenlik gruplarını açık bir şekilde girdiğinizden emin olun.

## <a name="adopt-a-zero-trust-approach"></a>Sıfır güven yaklaşımını benimseyin
Çevre tabanlı ağlar, bir ağdaki tüm sistemlere güvenildiğini varsayımıyla çalışır. Ancak, günümüzde çalışanlar kuruluşunuzun kaynaklarına çeşitli cihazlar ve uygulamalar üzerinde her yerden erişir ve bu da çevre güvenlik denetimlerinin ilgisiz olmasını sağlar. Yalnızca bir kaynağa erişebilecek olan erişim denetimi ilkeleri yeterli değildir. Güvenlik ve üretkenlik arasındaki dengeyi ana şekilde sağlamak için, güvenlik yöneticilerinin bir kaynağa nasıl erişildiği *konusunda* da etken olması gerekir.

Ağlarda güvenlik açıkları ihlal edildiği için ağların geleneksel savunma tehlikeleri geliştirilmesi gerekir: bir saldırgan, güvenilir sınır dahilinde tek bir uç noktanın güvenliğini tehlikeye atabilir ve ardından tüm ağ genelinde bir ağsalı hızlı bir şekilde genişletebilir. [Sıfır güven](https://www.microsoft.com/security/blog/2018/06/14/building-zero-trust-networks-with-microsoft-365/) ağı, bir çevre içindeki ağ konumuna dayalı olarak güven kavramını ortadan kaldırır. Bunun yerine, sıfır güvensel mimarilerin kurumsal verilere ve kaynaklara erişimini kesintiye yönelik cihaz ve Kullanıcı güven taleplerini kullanır. Yeni girişimler için, erişim sırasında güveni doğrulayan sıfır güven yaklaşımının benimsenmesini benimseyin.

En iyi uygulamalar şunlardır:

**En iyi yöntem**: cihaza, kimliğe, güvenine, ağ konumuna ve daha fazlasına göre kaynaklara koşullu erişim verin.  
**Ayrıntı**: [Azure AD koşullu erişim](../../active-directory/conditional-access/overview.md) , gerekli koşullara göre otomatik erişim denetimi kararları uygulayarak doğru erişim denetimlerini uygulamanıza olanak tanır. Daha fazla bilgi için bkz. [koşullu erişim Ile Azure yönetimine erişimi yönetme](../../active-directory/conditional-access/howto-conditional-access-policy-azure-management.md).

**En iyi yöntem**: yalnızca iş akışı onayını yaptıktan sonra bağlantı noktası erişimini etkinleştirin.  
**Ayrıntı**: Azure [Güvenlik Merkezi 'NDE tam zamanında VM erişimi](../../security-center/security-center-just-in-time.md) 'ni kullanarak Azure VM 'lerinize gelen trafiği kilitleyebilir ve gerektiğinde sanal makinelere bağlanmak için kolay erişim sağlarken saldırılara maruz kalmayı azaltabilirsiniz.

**En iyi yöntem**: ayrıcalıklı görevleri gerçekleştirmek için geçici izinler verin, bu, kötü amaçlı veya yetkisiz kullanıcıların izinlerin dolduktan sonra erişim kazanmasını önler. Erişim yalnızca kullanıcılara ihtiyaç duymadan verilir.  
**Ayrıntı**: ayrıcalıklı görevleri gerçekleştirmek için izin vermek üzere Azure AD Privileged Identity Management veya bir üçüncü taraf çözümünde tam zamanında erişim kullanın.

Sıfır güveni, ağ güvenliğine bir sonraki gelişmelidir. Cybersaldýrıların durumu, "ihlal varsay" Mindset değerini alır, ancak bu yaklaşım sınırlandırmamalıdır. Sıfır güven ağı, kuruluşların her zaman, her yerden, herhangi bir şekilde üretken olmalarını sağlayan teknolojiler kullanarak modern bir çalışma alanı derleyebilmesini sağlarken şirket verilerini ve kaynaklarını korur.

## <a name="control-routing-behavior"></a>Denetim yönlendirme davranışı
Bir sanal makineyi bir Azure sanal ağına yerleştirdiğinizde, diğer VM 'Ler farklı alt ağlarda olsa bile VM aynı sanal ağdaki başka bir VM 'ye bağlanabilir. Bu, varsayılan olarak etkinleştirilen bir sistem yolları koleksiyonu bu tür iletişim sağladığından mümkündür. Bu varsayılan yollar, aynı sanal ağdaki VM 'Lerin birbirleriyle ve internet ile bağlantı başlatmasına izin verir (yalnızca İnternet 'e giden iletişimler için).

Varsayılan sistem yolları birçok dağıtım senaryosunda yararlı olsa da, dağıtımlarınız için yönlendirme yapılandırmasını özelleştirmek istediğinizde zaman vardır. Belirli hedeflere ulaşmak için sonraki atlama adresini yapılandırabilirsiniz.

Bir sanal ağ için güvenlik gereci dağıtırken [Kullanıcı tanımlı yolları](../../virtual-network/virtual-networks-udr-overview.md) yapılandırmanızı öneririz. Bu, [önemli Azure hizmeti kaynaklarınızı yalnızca sanal ağlarınızla güvenli hale getirmek](network-best-practices.md#secure-your-critical-azure-service-resources-to-only-your-virtual-networks)başlıklı sonraki bir bölümde konuşacağız.

> [!NOTE]
> Kullanıcı tanımlı yollar gerekli değildir ve varsayılan sistem rotaları genellikle çalışır.
>
>

## <a name="use-virtual-network-appliances"></a>Sanal ağ gereçlerini kullanma
Ağ güvenlik grupları ve Kullanıcı tanımlı yönlendirme, [OSI modelinin](https://en.wikipedia.org/wiki/OSI_model)ağ ve aktarım katmanlarındaki belirli bir ağ güvenliği ölçüsünü sağlayabilir. Ancak bazı durumlarda, yığındaki yüksek düzeyde güvenliği etkinleştirmeniz veya istemeniz gerekir. Bu gibi durumlarda, Azure iş ortakları tarafından sunulan sanal ağ güvenliği gereçlerini dağıtmanızı öneririz.

Azure ağ güvenlik gereçleri, ağ düzeyi denetimlerinin sağladığı daha iyi güvenlik sunabilir. Sanal ağ güvenlik gereçlerinin ağ güvenliği özellikleri şunlardır:


* Güvenlik Duvarı
* Yetkisiz giriş algılama/yetkisiz erişim önleme
* Güvenlik açığı yönetimi
* Uygulama denetimi
* Ağ tabanlı anomali algılama
* Web filtreleme
* Virüsten Koruma
* Botnet koruması

Kullanılabilir Azure sanal ağ güvenlik gereçlerini bulmak için [Azure Marketi](https://azure.microsoft.com/marketplace/) ' ne gidin ve "güvenlik" ve "ağ güvenliği" için arama yapın.

## <a name="deploy-perimeter-networks-for-security-zones"></a>Güvenlik bölgeleri için çevre ağları dağıtma
[Çevre ağı](/azure/architecture/vdc/networking-virtual-datacenter) (DMZ olarak da bilinir), varlıklarınız ve internet arasında ek bir güvenlik katmanı sağlayan bir fiziksel veya mantıksal ağ kesimdir. Çevre ağının kenarındaki özelleştirilmiş ağ erişim denetimi cihazları yalnızca sanal ağınıza yönelik istenen trafiğe izin verir.

Ağ erişim denetimi yönetimi, izleme, günlüğe kaydetme ve raporlarınıza Azure sanal ağınızın kenarındaki cihazlarda odaklanabilmeniz için çevre ağları yararlı olur. Çevre ağı, genellikle dağıtılmış hizmet reddi (DDoS) engellemesini, yetkisiz giriş algılama/yetkisiz erişim önleme sistemlerini (KIMLIKLER/IP 'ler), güvenlik duvarı kuralları ve ilkeleri, web filtrelemesi, ağ kötü amaçlı yazılımdan koruma ve daha fazlasını etkinleştirdiğiniz yerdir. Ağ güvenlik cihazları Internet ve Azure sanal ağınız arasında yer alır ve her iki ağda bir arabirime sahiptir.

Bu, bir çevre ağının temel tasarımı olsa da, geri dönüş, Üçlü ve çok ana bilgisayarlı gibi birçok farklı tasarım vardır.

Daha önce bahsedilen sıfır güven kavramına göre, Azure kaynaklarınıza yönelik ağ güvenliği ve erişim denetimi düzeyini iyileştirmek için tüm yüksek güvenlikli dağıtımlar için bir çevre ağı kullanmayı düşünmeniz önerilir. Varlıklarınız ve internet arasında ek bir güvenlik katmanı sağlamak için Azure veya üçüncü taraf bir çözüm kullanabilirsiniz:

- Azure yerel denetimleri. Application Gateway ' deki [Azure Güvenlik](../../firewall/overview.md) duvarı ve [Web uygulaması güvenlik duvarı](../../application-gateway/features.md#web-application-firewall) , hizmet olarak tam durum bilgisi olan güvenlik duvarı, yerleşik yüksek kullanılabilirlik, KıSıTLANMAMıŞ bulut ölçeklenebilirliği, FQDN filtrelemesi, OWASP çekirdek kural kümeleri için destek ve basit kurulum ve yapılandırma ile temel güvenlik sağlar.
- Üçüncü taraf teklifleri. [Azure Marketi 'nde](https://azuremarketplace.microsoft.com/) yeni nesil güvenlik duvarı (NGFW) ve bilinen güvenlik araçları ve önemli ölçüde gelişmiş ağ güvenliği düzeyleri sağlayan diğer üçüncü taraf teklifleri arayın. Yapılandırma daha karmaşık olabilir, ancak üçüncü taraf bir teklif mevcut özellikleri ve becerileri kullanmanıza izin verebilir.

## <a name="avoid-exposure-to-the-internet-with-dedicated-wan-links"></a>Adanmış WAN bağlantılarıyla internet 'ten Etkilenmemeye özen gösterin
Birçok kuruluş, karma BT yolunu seçti. Karma BT sayesinde, şirketin bazı bilgi varlıkları Azure 'da, diğerleri ise şirket içinde kalır. Birçok durumda, bir hizmetin bazı bileşenleri Azure 'da çalışıyor, diğer bileşenler şirket içinde kalır.

Karma BT senaryosunda, genellikle bazı şirket içi bağlantı türleri vardır. Şirketler arası bağlantı, şirketin şirket içi ağlarını Azure sanal ağlarına bağlanmasına izin verir. İki şirket içi bağlantı çözümü mevcuttur:

* [Siteden sıteye VPN](../../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md). Bu, güvenilir, güvenilir ve kurulu bir teknolojidir, ancak bağlantı Internet üzerinden gerçekleşir. Bant genişliği en fazla yaklaşık 1,25 Gbps ile sınırlıdır. Siteden siteye VPN, bazı senaryolarda istenen seçenektir.
* **Azure ExpressRoute**. Şirket içi bağlantınız için [ExpressRoute](../../expressroute/expressroute-introduction.md) kullanmanızı öneririz. ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan özel bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar. ExpressRoute ile Azure, Microsoft 365 ve Dynamics 365 gibi Microsoft bulut hizmetleriyle bağlantı kurabilirsiniz. ExpressRoute, şirket içi konumunuz veya Microsoft Exchange barındırma sağlayıcısı arasında adanmış bir WAN bağlantıdır. Bu bir telekomünikasyon bağlantısı olduğundan, verileriniz Internet üzerinden hareket etmez, bu nedenle internet iletişimlerinin olası risklerine maruz değildir.

ExpressRoute bağlantınızın konumu güvenlik duvarı kapasitesini, ölçeklenebilirliği, güvenilirliği ve ağ trafiği görünürlüğünü etkileyebilir. Mevcut (Şirket içi) ağlarda ExpressRoute 'un nerede sonlandırılacağına yönelik bir kimlik belirlemeniz gerekir. Seçenekleriniz şunlardır:

- Trafiğin görünürlüğünü istiyorsanız güvenlik duvarının dışında (çevre ağı paradigma) sonlandırın, veri merkezlerini yalıtmak için mevcut bir uygulamaya devam etmeniz veya yalnızca extranet kaynaklarını yalnızca Azure 'a yerleştirmekten devam etmeniz gerekir.
- Güvenlik duvarının içinde sonlandırın (ağ uzantısı paradigması). Bu, varsayılan önerinin. Diğer tüm durumlarda, Azure 'u n. veri merkezi olarak değerlendirmesini öneririz.

## <a name="optimize-uptime-and-performance"></a>Çalışma süresini ve performansı iyileştirme
Bir hizmet kapalıysa, bilgilere erişilemez. Performans, verilerin kullanılamamasına neden olduysa, erişilemeyen verileri göz önünde bulundurun. Bir güvenlik perspektifinden, hizmetlerinizin en iyi çalışma süresine ve performansa sahip olduğundan emin olmak için her şeyi yapmanız gerekir.

Kullanılabilirliği ve performansı geliştirmek için popüler ve etkili bir yöntem yük dengedir. Yük Dengeleme, bir hizmetin parçası olan sunucular arasında ağ trafiği dağıtmaya yönelik bir yöntemdir. Örneğin, hizmetinizin bir parçası olarak ön uç web sunucularınız varsa, trafiği birden çok ön uç Web sunucunuz genelinde dağıtmak için yük dengelemeyi kullanabilirsiniz.

Bu trafik dağıtımı, Web sunucularından biri kullanılamaz hale gelirse, yük dengeleyici bu sunucuya trafik göndermeyi durdurup, yine de çevrimiçi olan sunuculara yeniden yönlendirdiğinden, bu trafiğin dağıtılması kullanılabilirliği artırır. Yük Dengeleme, isteklere hizmet veren işlemci, ağ ve bellek yükü, yük dengeli tüm sunuculara dağıtıldığı için performansa de yardımcı olur.

Hizmetleriniz için uygun olan her durumda yük dengelemeyi kullanmanızı öneririz. Aşağıdaki senaryolar hem Azure sanal ağ düzeyinde hem de genel düzeyde, her biri için Yük Dengeleme seçenekleriyle birlikte senaryolardır.

**Senaryo**: şunları içeren bir uygulamanız vardır:

- Aynı kullanıcı/istemci oturumundan gelen isteklerin aynı arka uç sanal makineye ulaşmasını gerektirir. Bunun örnekleri, alışveriş sepeti uygulamaları ve web posta sunucularıdır.
- Yalnızca güvenli bir bağlantı kabul eder, bu nedenle sunucuya şifrelenmemiş iletişim kabul edilebilir bir seçenek değildir.
- Aynı uzun süre çalışan aynı TCP bağlantısında, farklı arka uç sunucularına yönlendirilmek veya yük dengelemesi yapmak için birden çok HTTP isteği gerektirir.

**Yük Dengeleme seçeneği**: http Web trafiği yük dengeleyici olan [Azure Application Gateway](../../application-gateway/overview.md)kullanın. Application Gateway, ağ geçidinde uçtan uca TLS şifrelemesini ve [TLS sonlandırmasını](../../application-gateway/overview.md) destekler. Daha sonra Web sunucuları şifreleme ve şifre çözme ek yükünün yanı sıra şifrelenmemiş akış ile arka uç sunucularına akışı yapılabilir.

**Senaryo**: bir Azure sanal ağında bulunan sunucularınız arasında internet 'ten gelen bağlantıların yükünü dengelemenize gerek duyarsınız. Senaryolar şunlardır:

- İnternet 'ten gelen istekleri kabul eden durum bilgisiz uygulamalar vardır.
- Yapışkan oturumlara veya TLS yük boşaltma gerektirmeyin. Yapışkan oturumlar, sunucu benzeşimi elde etmek için uygulama yük dengelemesi ile kullanılan bir yöntemdir.

**Yük Dengeleme seçeneği**: daha yüksek düzeyde kullanılabilirlik sağlamak için birden çok sanal makineye gelen istekleri yayan [bir dış yük dengeleyici oluşturmak](../../load-balancer/quickstart-load-balancer-standard-public-portal.md) için Azure Portal kullanın.

**Senaryo**: internet üzerinde olmayan VM 'lerden yük dengeleme bağlantıları yapmanız gerekir. Çoğu durumda, yük dengeleme için kabul edilen bağlantılar SQL Server örnekleri veya iç Web sunucuları gibi bir Azure sanal ağındaki cihazlar tarafından başlatılır.   
**Yük Dengeleme seçeneği**: daha yüksek düzeyde kullanılabilirlik sağlamak için birden çok sanal makineye gelen istekleri yayan [bir iç yük dengeleyici oluşturmak](../../load-balancer/quickstart-load-balancer-standard-public-portal.md) için Azure Portal kullanın.

**Senaryo**: şu şekilde genel yük dengelemeye ihtiyacınız vardır:

- Birden çok bölgeye yaygın olarak dağıtılan ve mümkün olan en yüksek çalışma süresi düzeyini (kullanılabilirlik) gerektiren bir bulut çözümüne sahip olmak.
- Tüm veri merkezinde kullanılamaz hale gelirse emin olmak için hizmetinizin kullanılabilir olmasını sağlamak için en yüksek çalışma süresi düzeyi gereklidir.

**Yük Dengeleme seçeneği**: Azure Traffic Manager kullanın. Traffic Manager, Kullanıcı konumuna göre hizmetlerinize olan bağlantıların yükünü dengelemeye olanak tanır.

Örneğin, Kullanıcı, AB 'den hizmetinize bir istek yapıyorsa bağlantı, bir AB veri merkezinde bulunan hizmetlerinize yönlendirilir. Traffic Manager küresel yük dengelemenin bu bölümü, en yakın veri merkezine bağlanmak, en çok kullanılan veri merkezlerine bağlanmadan daha hızlı olduğundan performansı artırmaya yardımcı olur.

## <a name="disable-rdpssh-access-to-virtual-machines"></a>Sanal makinelere RDP/SSH erişimini devre dışı bırak
[Uzak Masaüstü Protokolü](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) (RDP) ve [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) (SSH) protokolünü kullanarak Azure sanal makinelerine ulaşmak mümkündür. Bu protokoller VM'lerin uzak konumlardan yönetilmesine olanak tanır ve veri merkezi bilgi işleminde standarttır.

Bu protokolleri Internet üzerinden kullanmayla ilgili olası güvenlik sorunu, saldırganların Azure sanal makinelerine erişim kazanmak için [deneme yanılma](https://en.wikipedia.org/wiki/Brute-force_attack) tekniklerini kullanmasına olanak sağlar. Saldırganlar erişim kazandıktan sonra VM'nizi başlangıç noktası olarak kullanıp sanal ağınızdaki diğer makinelerin gizliliğini bozabilir, hatta Azure dışındaki ağa bağlı cihazlara bile saldırabilir.

İnternet 'ten Azure sanal makinelerinize doğrudan RDP ve SSH erişimini devre dışı bırakmanızı öneririz. İnternet 'ten doğrudan RDP ve SSH erişimi devre dışı bırakıldıktan sonra, bu VM 'Lere uzaktan yönetim için erişmek üzere kullanabileceğiniz diğer seçenekleriniz vardır.

**Senaryo**: tek bir kullanıcının Internet üzerinden bir Azure sanal ağına bağlanmasını etkinleştirin.   
**Seçenek**: [Noktadan siteye VPN](../../vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal.md) , uzaktan erişim VPN istemcisi/sunucu bağlantısı için başka bir terimdir. Noktadan siteye bağlantı kurulduktan sonra, Kullanıcı, kullanıcının Noktadan siteye VPN aracılığıyla bağlandığı Azure sanal ağ üzerinde bulunan tüm VM 'lere bağlanmak için RDP veya SSH kullanabilir. Bu, kullanıcının bu VM 'Lere erişme yetkisi olduğunu varsayar.

Kullanıcının bir VM 'ye bağlanmadan önce iki kez kimlik doğrulaması yapması gerektiğinden, Noktadan siteye VPN Direct RDP veya SSH bağlantılarından daha güvenlidir. İlk olarak, kullanıcının Noktadan siteye VPN bağlantısı kurması için kimlik doğrulaması yapması (ve yetkilendirilmesi) gerekir. İkincisi, kullanıcının RDP veya SSH oturumu kurması için kimlik doğrulaması yapması (ve yetkilendirilmesi) gerekir.

**Senaryo**: şirket içi ağınızdaki kullanıcıların Azure sanal ağınızdaki VM 'lere bağlanmasını etkinleştirin.   
**Seçenek**: [siteden siteye VPN](../../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) , ağın tamamını Internet üzerinden başka bir ağa bağlar. Şirket içi ağınızı bir Azure sanal ağına bağlamak için siteden siteye VPN kullanabilirsiniz. Şirket içi ağınızdaki kullanıcılar, siteden siteye VPN bağlantısı üzerinden RDP veya SSH protokolünü kullanarak bağlanır. İnternet üzerinden doğrudan RDP veya SSH erişimine izin vermeniz gerekmez.

**Senaryo**: sıteden siteye VPN 'ye benzer işlevler sağlamak için ADANMıŞ bir WAN bağlantısı kullanın.   
**Seçenek**: [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/)kullanın. Siteden siteye VPN 'ye benzer işlevsellik sağlar. Temel farklılıklar şunlardır:

- Adanmış WAN bağlantısı Internet 'te geçiş yapmaz.
- Adanmış WAN bağlantıları genellikle daha kararlı olur ve daha iyi gerçekleştirilir.

## <a name="secure-your-critical-azure-service-resources-to-only-your-virtual-networks"></a>Kritik Azure hizmeti kaynaklarınızı yalnızca sanal ağlarınızla güvenli hale getirin
Azure özel bağlantısı 'nı kullanarak, sanal ağınızdaki özel bir uç nokta üzerinden Azure PaaS hizmetlerine (örneğin, Azure depolama ve SQL veritabanı) erişin. Özel uç noktalar, kritik Azure hizmeti kaynaklarınızı yalnızca sanal ağlarınızla korumanıza olanak sağlar. Sanal ağınızdan Azure hizmetine giden trafik her zaman Microsoft Azure omurga ağında kalır. Azure PaaS hizmetlerini kullanmak için Sanal ağınızı genel İnternet 'e sunma işlemi artık gerekli değildir. 

Azure özel bağlantısı aşağıdaki avantajları sağlar:
- Azure **hizmet kaynaklarınız Için geliştirilmiş güvenlik**: Azure özel bağlantısı ile Azure hizmet kaynakları, Özel uç nokta kullanılarak sanal ağınızla korunmuş olabilir. Sanal ağdaki özel bir uç noktaya hizmet kaynaklarının güvenliğini sağlamak, kaynaklara genel İnternet erişimini tamamen kaldırarak ve yalnızca sanal ağınızdaki özel uç noktadan gelen trafiğe izin vererek geliştirilmiş güvenlik sağlar.
- Azure **platformunda Azure hizmet kaynaklarına özel olarak erişin**: özel uç noktalar kullanarak Sanal ağınızı Azure 'daki hizmetlere bağlayın. Genel IP adresine gerek yoktur. Özel bağlantı platformu, Azure omurga ağı üzerinden tüketici ve hizmetler arasındaki bağlantıyı işleymeyecektir.
- Şirket **içi ve eşlenmiş ağlardan erişim**: Azure 'Da, ExpressRoute özel EŞLEMESI, VPN tünelleri ve eşlenmiş sanal ağlar üzerinden şirket içinden çalışan hizmetlere özel uç noktalar kullanarak erişin. ExpressRoute Microsoft eşlemesi 'ni yapılandırmaya veya internet 'e ulaşmaya gerek yoktur. Özel bağlantı, iş yüklerini Azure 'a geçirmek için güvenli bir yol sağlar.
- **Veri sızıntılarına karşı koruma**: özel bir uç nokta, tüm hizmet yerine bir PaaS kaynağı örneğine eşlenir. Tüketiciler yalnızca belirli bir kaynağa bağlanabilir. Hizmette başka bir kaynağa erişim engellenir. Bu mekanizma, veri sızıntısı risklerine karşı koruma sağlar.
- **Küresel erişim**: diğer bölgelerde çalışan hizmetlere özel olarak bağlanın. Tüketicinin sanal ağı A bölgesinde olabilir ve B bölgesindeki hizmetlere bağlanabilir.
- **Kolay bir şekilde ayarlanır ve yönetin**: Azure kaynaklarını bir IP güvenlik duvarı aracılığıyla güvenli hale getirmek için sanal ağlarınızda artık ayrılmış, genel IP adresleri gerekmez. Özel uç noktaları ayarlamak için herhangi bir NAT veya ağ geçidi cihazı gerekmez. Özel uç noktalar basit bir iş akışı aracılığıyla yapılandırılır. Hizmet tarafında Ayrıca, Azure hizmet kaynağınızın bağlantı isteklerini kolayca yönetebilirsiniz. Azure özel bağlantısı, farklı Azure Active Directory kiracılarına ait tüketiciler ve hizmetler için de geçerlidir. 
    
Özel uç noktalar ve özel uç noktaların kullanılabildiği Azure hizmetleri ve bölgeleri hakkında daha fazla bilgi edinmek için bkz. [Azure özel bağlantısı](../../private-link/private-link-overview.md).


## <a name="next-steps"></a>Sonraki adımlar
Azure 'u kullanarak bulut çözümlerinizi tasarlarken, dağıttığınızda ve yönetirken en iyi güvenlik uygulamaları için bkz. [Azure Güvenlik en iyi uygulamaları ve desenleri](best-practices-and-patterns.md) .