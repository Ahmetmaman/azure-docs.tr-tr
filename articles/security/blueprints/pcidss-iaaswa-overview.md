---
title: Azure güvenlik ve uyumluluk planı - Iaas Web uygulaması için PCI DSS
description: Azure güvenlik ve uyumluluk planı - Iaas Web uygulaması için PCI DSS
services: security
author: meladie
ms.assetid: 9c825380-2abe-4fdd-800c-59d1fed1780b
ms.service: security
ms.topic: article
ms.date: 07/03/2018
ms.author: meladie
ms.openlocfilehash: 4143390ea4cd0bfd664632a1f378dd5620235768
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45576005"
---
# <a name="azure-security-and-compliance-blueprint-iaas-web-application-for-pci-dss"></a>Azure güvenlik ve uyumluluk planı: Iaas Web uygulaması için PCI DSS

## <a name="overview"></a>Genel Bakış

Azure güvenlik ve uyumluluk planı toplama, depolama ve alma için uygun bir hizmet (Iaas) ortamı olarak ödeme kartı sektörü veri güvenliği standartları (PCI DSS 3.2) uyumlu bir altyapı dağıtımı için yönergeler sağlar bir kart sahibi verilerini. Bu yaygın bir başvuru mimarisi gösteren ve güvenli, uyumlu, çok katmanlı bir ortamda (kartı numarası, geçerlilik süresi ve doğrulama verileri dahil) kredi kartı verileriyle doğru işlenmesi gösterir. Bu şema yükünü ve dağıtım maliyetini azaltmak için bulut tabanlı bir yaklaşım isteyen kuruluşların ihtiyaçlarını karşılamak için bir uçtan uca çözüm gösterilir.

Bu başvuru mimarisi, Uygulama Kılavuzu ve tehdit modeli müşteriler, PCI DSS 3.2 gereksinimlerine uymak sağlamasıdır. Bu çözüm, müşterilerin iş yükleri, PCI DSS 3.2 uyumlu bir şekilde Azure'a dağıtmak için bir temel sağlar. Ancak, bu çözümü olarak kullanılmamalıdır-çünkü bir üretim ortamında ek yapılandırma gerekli değildir.

PCI DSS uyumluluğu elde etmek için bir akredite nitelikli güvenlik değerlendiricisi'nı (QSA) üretim müşteri çözüm onaylamak gerektirir. Müşteriler, uygun güvenlik yürütmek için sorumludur ve her bir müşterinin uygulama ayrıntılarına bağlı uyumluluk değerlendirme gereksinimleri değişebilir, bu mimariyi kullanarak oluşturulan bir çözümün temel.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri

Bu çözüm, bir başvuru mimarisi için SQL Server arka ucuna sahip bir Iaas web uygulaması dağıtır. Bir web katmanı, veri katmanı, Active Directory altyapı, uygulama ağ geçidi ve yük dengeleyici mimarisi içerir. Web ve veri katmanları için dağıtılan sanal makinelerin bir kullanılabilirlik kümesine yapılandırılır ve SQL Server örneklerinin bir Always On kullanılabilirlik grubu yüksek kullanılabilirlik için yapılandırılır. Etki alanına katılmış sanal makineleri ve Active Directory grup ilkeleri işletim sistemi düzeyinde güvenlik ve uyumluluk yapılandırmaları uygulamak için kullanılır. Yönetim Burcu ana bilgisayarı, yöneticilerin dağıtılan kaynaklara güvenli bir bağlantı sağlar. **Azure başvuru mimarisi alt ağa yönetimi ve veri içeri aktarma için bir VPN veya ExpressRoute bağlantısı yapılandırma önerir.**

![Iaas Web uygulaması için PCI DSS başvurusu mimari şeması](images/pcidss-iaaswa-architecture.png "Iaas Web uygulaması için PCI DSS başvuru mimarisi diyagramı")

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisi ayrıntılarını yerleştirilir [dağıtım mimarisi](#deployment-architecture) bölümü.

- Kullanılabilirlik Kümeleri
    - (1) active Directory etki alanı denetleyicileri
    - (1) SQL küme düğümleri
    - (1) web/IIS
- Azure Active Directory
- Azure Application Gateway
    - (1) web uygulaması güvenlik duvarı
        - Güvenlik Duvarı modu: önleme
        - Kural kümesi: OWASP 3.0
        - Dinleyici bağlantı noktası: 443
- Azure Key Vault
- Azure Load Balancer
- Azure İzleyici
- Azure Resource Manager
- Azure Güvenlik Merkezi
- Azure Storage
    - (7) coğrafi olarak yedekli depolama hesapları
- Azure Sanal Makineler
    - (1) yönetim/savunma (Windows Server 2016 Datacenter)
    - (2) active Directory etki alanı denetleyicisi (Windows Server 2016 Datacenter)
    - (2) SQL Server kümesi düğümünün (Windows Server 2016 üzerinde SQL Server 2017)
    - (2) web/IIS (Windows Server 2016 Datacenter)
- Azure Sanal Ağ
    - (1) /16 ağ
    - (5) /24 ağlar
    - (5) ağ güvenlik grupları
- Bulut tanığı
- Kurtarma Hizmetleri kasası

## <a name="deployment-architecture"></a>Dağıtım mimarisi

Aşağıdaki bölümde dağıtım ve uygulama öğeleri ayrıntılı olarak açıklanmaktadır.

**Kale ana bilgisayarı**: Burcu ana bilgisayarı tek kullanıcılara bu ortama dağıtılan kaynaklara erişmek giriş noktasıdır. Kale ana bilgisayarı, genel IP adreslerinden gelen uzak trafiğine yalnızca güvenli bir listede vererek dağıtılan kaynaklara güvenli bir bağlantı sağlar. Uzak Masaüstü (RDP) trafiğine izin vermek için trafik kaynağını ağ güvenlik grubu listesinde tanımlanması gerekir.

Bu çözüm aşağıdaki yapılandırmaları olan bir etki alanına katılmış Burcu ana bilgisayarı olarak bir sanal makine oluşturur:
-   [Kötü amaçlı yazılımdan koruma uzantısı](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [Azure tanılama uzantısı](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) Azure Key Vault kullanma
-   Bir [otomatik kapatma ilke](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) kullanımda olmadığında sanal makine kaynakların tüketimini azaltmak için
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) kimlik bilgilerini ve diğer gizli dizileri çalışan işletim sisteminden yalıtılmış korumalı bir ortamda çalıştırın böylece etkin

### <a name="virtual-network"></a>Sanal ağ

10.200.0.0/16 bir adres alanı ile özel bir sanal ağ mimarisini tanımlar.

**Ağ güvenlik grupları**: Bu çözüm kaynakları ayrı web alt ağı, veritabanı alt ağı, Active Directory alt ve bir sanal ağ içinde yönetim alt ağı ile bir mimari dağıtır. Alt ağlar için yalnızca bu gerekli system ve yönetim işlevselliği için alt ağlar arasındaki trafiği kısıtlamak için ayrı alt ağlara uygulanan ağ güvenlik grubu kuralları tarafından mantıksal olarak ayrılır.

Yapılandırma için bkz: [ağ güvenlik grupları](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) ile bu çözümü dağıtıldı. Kuruluşlar, ağ güvenlik grupları kullanarak yukarıda dosyasını düzenleyerek yapılandırabilirsiniz [bu belgeleri](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) bir kılavuz olarak.

Alt ağlar, ayrılmış bir ağ güvenlik grubu vardır:
- Uygulama ağ geçidi (LBNSG) için 1 ağ güvenlik grubu
- Kale ana bilgisayarı (MGTNSG) 1 ağ güvenlik grubu
- 1 ağ güvenlik grubu için birincil ve yedek etki alanı denetleyicilerine (ADNSG)
- SQL sunucuları ve bulut tanığı (SQLNSG) için 1 ağ güvenlik grubu
- 1 ağ güvenlik grubu için web Katmanı (WEBNSG)

### <a name="data-in-transit"></a>Aktarımdaki verileri

Azure, Azure veri merkezlerinden tüm iletişimi varsayılan olarak şifreler. Ayrıca, Azure depolama için tüm işlemleri Azure portalı üzerinden, HTTPS oluşur.

### <a name="data-at-rest"></a>Bekleyen veriler

Mimari, şifreleme ve veritabanı denetimi gibi birden çok ölçü kullanılmadıkları verilerini korur.

**Azure depolama**: şifrelenmiş verileri rest gereksinimleri karşılamak için tüm [Azure depolama](https://azure.microsoft.com/services/storage/) kullanan [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption). Bu, kuruluş güvenlik ve uyumluluk gereksinimlerini PCI DSS 3.2 tarafından tanımlanan desteklemek üzere bir kart sahibi verilerini koruyarak yardımcı olur.

**Azure Disk şifrelemesi**: Azure Disk şifrelemesi Windows Iaas sanal makine disklerini şifrelenmiş için kullanılır. [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için Windows BitLocker özelliğidir yararlanır. Çözüm denetlemenize ve disk şifreleme anahtarlarını yönetmek için Azure anahtar kasası ile tümleştirilmiştir.

**SQL Server**: SQL Server örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:
- [Active Directory kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) veritabanı kullanıcıları ve diğer Microsoft Hizmetleri tek bir merkezi konumda kimlik yönetimini sağlar.
-   [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) veritabanı olaylarını izler ve bir denetim günlüğüne bir Azure depolama hesabında yazar.
-   SQL veritabanını kullanacak şekilde yapılandırıldığını [saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), gerçek zamanlı şifreleme ve şifre çözme veritabanının, ilişkili yedeklemeler gerçekleştirir ve bilgileri korumak için işlem günlüğü dosyaları bekletin. Saydam veri şifrelemesi, kart sahibi verilerini depolanan güvencesi yetkisiz erişim ayarlanmamış sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) doğru izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) şüpheli veritabanı etkinlikleri, potansiyel açıklar, SQL ekleme saldırıları ve anormal veritabanı erişim için güvenlik uyarıları sağlayarak oluşunca algılama ve olası tehditlere yanıt sağlar. desenler.
-   [Always Encrypted sütunları](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) hassas bir kart sahibi verilerini hiçbir zaman içinde bir veritabanı sistemi düz metin olarak göründüğünden emin olun. Veri şifrelemesi etkinleştirildikten sonra yalnızca istemci uygulamaları veya uygulama sunucuları anahtarlarına erişimi ile düz metin verilere erişebilir.
- [Genişletilmiş özellikler](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) özelliği veri sahipleri işlenmesini kesmek için kullanılabilir veritabanı nesneleri için özel özellikler ekleme ve veri "önlemek için uygulama mantığını destekleyecek şekilde artık Üretilmiyor" etiketi kullanıcılara verdiğinden ilişkili bir kart sahibi verilerini işliyor.
- [Satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) işleme devam etmek için veri erişimini kısıtlamak için ilkeler tanımlamak üzere kullanıcıların sağlar.
- [SQL veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) aktaran hassas verilerin görünürlüğünü ayrıcalık sahibi tarafından ayrıcalıksız kullanıcılar veya uygulamalar için veri maskeleme sınırlar. Dinamik veri maskeleme, otomatik olarak olası hassas verileri bulun ve uygulanması için uygun maskeleri önerin. Bu, tanımlamak ve yetkisiz erişim veritabanı yok, kart sahibi verilerini erişimi azaltma için yardımcı olur. Müşteriler, dinamik veri maskeleme, veritabanı şeması uyması için ayarları ayarlamak için sorumludur.

### <a name="identity-management"></a>Kimlik yönetimi

Özellikleri, Azure ortamında veri erişimi yönetmek için aşağıdaki teknolojileri sağlar:

- [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) Microsoft&#39;s çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüm için tüm kullanıcılar Azure Active Azure SQL veritabanına erişen kullanıcılar dahil olmak üzere dizininde, oluşturulur.
- Azure Active Directory kullanan uygulamaya kimlik doğrulaması gerçekleştirilir. Daha fazla bilgi için [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Ayrıca, Azure SQL veritabanı uygulamaya kimlik doğrulaması için Azure Active Directory veritabanı sütun şifreleme kullanır. Daha fazla bilgi için bkz. nasıl [Azure SQL veritabanındaki hassas verileri korumaya](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
- [Azure rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) yöneticilerin kullanıcıların işlerini yapması için gereken sadece erişim miktarını vermek için ayrıntılı erişim izinleri tanımlamanıza olanak tanır. Azure kaynakları için her sınırsız kullanıcı izin vermek yerine, yöneticiler verilerine erişmek için yalnızca belirli eylemleri izin verebilir. Abonelik yöneticisine abonelik erişimi sınırlıdır.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) belirli bilgilere erişebilen kullanıcı sayısını en aza indirmek müşterilerin sağlar. Yöneticiler, Azure Active Directory Privileged Identity Management, bulmak, kısıtlamak ve ayrıcalıklı kimlikleri ve bu kimliklerin kaynaklara erişimini izlemek için kullanabilirsiniz. Bu işlev, gerektiğinde talep üzerine tam zamanında yönetimsel erişim uygulamak için de kullanılabilir.
- [Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) kuruluş etkileyen olası güvenlik açıklarını algılar&#39;s kimlikleri, bir kuruluş için ilgili algılanan kuşkulu eylemleri için otomatik yanıtlar yapılandırır&#39;s kimlikleri ve bunları gidermek için uygun eylemde için şüpheli olayları araştırır.

### <a name="security"></a>Güvenlik

**Gizli dizileri Yönetim**: Çözüm [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) anahtar ve gizli dizi yönetimi. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Azure Key Vault özellikleri müşterilerin korumak ve böyle verilere Yardımı:

- Gelişmiş erişim ilkeleri gereksinim olarak yapılandırılır.
- Key Vault erişim ilkeleri anahtarlara ve gizli anahtarları en düşük gerekli izinlerle tanımlanır.
- Tüm anahtarları ve gizli anahtarları Key vault'ta sona erme tarihi vardır.
- Tüm anahtarları Key vault'ta özel bir donanım güvenlik modülleri tarafından korunur. Anahtar bir HSM korumalı anahtarlar 2048 bit RSA anahtarı türüdür.
- Tüm kullanıcılar ve kimlikler rol tabanlı erişim denetimi kullanarak minimum gerekli izinleri verilir.
- Key Vault için tanılama günlükleri ile 365 gün en az bir saklama süresi etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri gerekli olanlarla sınırlıdır.
- Çözüm, Iaas sanal makine disk şifreleme anahtarlarını ve gizli anahtarları yönetmek için Azure anahtar kasası ile tümleştirilmiştir.

**Düzeltme Eki Yönetimi**: Bu başvuru mimarisinin bir parçası olarak dağıtılmış Windows sanal makineleri, Windows Update hizmetinden otomatik güncelleştirmeleri almak için varsayılan olarak yapılandırılır. Bu çözüm ayrıca içerir [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) üzerinden güncelleştirilmiş dağıtımları oluşturulabilir düzeltme eki gerektiğinde sanal makinelere için hizmet.

**Kötü amaçlı yazılımdan koruma**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) sanal makineler için yardımcı tanımlamak ve virüslerin, casus yazılımların ve diğer kötü amaçlı yazılım, yapılandırılabilir uyarı Kaldır gerçek zamanlı koruma özelliği sağlar. bilinen kötü amaçlı veya istenmeyen yazılım yükleme veya korumalı sanal makineler üzerinde çalışmayı denediğinde olduğunda.

**Azure Güvenlik Merkezi**: ile [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro), müşterilerin merkezi olarak uygulama ve iş yüklerinizde güvenlik ilkelerini yönetme, sınırlama, tehditlere maruz kalma riskinizi ve algılayabilir ve saldırılara karşılık vermek. Ayrıca, Azure Güvenlik Merkezi, yapılandırma ve güvenlik duruşunu ve verilerin korunmasına yardımcı olmak için hizmet öneriler sağlamak üzere Azure hizmetlerinin mevcut yapılandırmaları erişir.

Azure Güvenlik Merkezi, ortamlarını hedefleyen potansiyel saldırılar müşterileri uyarmak için çeşitli algılama özellikleri kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Azure Güvenlik Merkezi'nde bulunan bir dizi [güvenlik uyarıları önceden tanımlanmış](https://docs.microsoft.com/azure/security-center/security-center-alerts-type)bir tehdit tetiklenen veya şüpheli etkinlik gerçekleştiğinde. [Özel uyarı kuralları](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) Azure Güvenlik Merkezi'nde müşterilerin kendi ortamından toplanmış verileri temel alan yeni güvenlik uyarılarını tanımlamanıza izin verin.

Azure Güvenlik Merkezi, müşterilerin bulmasını ve olası güvenlik sorunlarını gidermek için daha basit hale öncelikli güvenlik uyarıları ve olayları sağlar. A [tehdit zekası raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) her olay yanıt ekiplerinin tehditleri araştırmanıza ve düzeltme yardımcı olmak için tehdit algılanan için oluşturulur.

**Azure Application Gateway**: Mimari Azure Application Gateway ile yapılandırılmış bir web uygulaması güvenlik duvarı ve etkin OWASP ruleset kullanarak güvenlik açıklarını riskini azaltır. Ek özellikler şunlardır:

- [SSL uç bitiş](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Etkinleştirme [SSL yük boşaltma](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Devre dışı [TLS sürüm 1.0 ve v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Web uygulaması güvenlik duvarı](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (önleme modu)
- [Önleme modu](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) OWASP 3.0 ruleset ile
- Etkinleştirme [Tanılama Günlüğü](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)
- [Özel durum araştırmaları](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center) ve [Azure Danışmanı](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) ek koruma ve bildirimleri sağlar. Azure Güvenlik Merkezi, ayrıca bir saygınlığı sistemi sağlar.

### <a name="business-continuity"></a>İş sürekliliği

**Yüksek kullanılabilirlik**: tüm sanal makinelerin çözümü dağıtan bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Kullanılabilirlik kümeleri, sanal makinelerin kullanılabilirliğini artırmak için birden fazla yalıtılmış donanım kümesi arasında dağıtılmasını sağlar. En az bir sanal makine % 99,95 oranında toplantı planlı veya Plansız bakım olayı sırasında kullanılabilir Azure SLA'sı.

**Kurtarma Hizmetleri kasası**: [kurtarma Hizmetleri kasası](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) yedekleme verileri barındırır ve tüm yapılandırmaları bu mimaride Azure sanal makineleri korur. Bir kurtarma Hizmetleri kasası ile müşterilerin dosya ve klasörleri bir Iaas sanal makineden sanal makinenin tamamını geri yüklemeden daha hızlı geri yükleme süreleri etkinleştirme geri yükleyebilirsiniz.

**Bulut tanığı**: [bulut tanığı](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) yararlanan Azure eklenen ve yönetim noktası olarak Windows Server 2016 yük devretme kümesi çekirdek tanığı türüdür. Diğer tüm çekirdek tanıkları gibi bulut tanığı, bir oy alır ve çekirdek hesaplamalarına katılabilir, ancak standart genel kullanıma açık Azure Blob Depolama kullanır. Bu, genel bulutta barındırılan sanal makineleri bakım ek yükü ortadan kaldırır.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve Denetim

Azure Hizmetleri, sistem ve kullanıcı etkinliğini yanı sıra, sistem durumu kapsamlı bir şekilde oturum:
- **Etkinlik günlükleri**: [etkinlik günlüklerini](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bir Abonelikteki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlükleri bir işlemin Başlatıcı belirlemek yardımcı olabilir, oluşumunu ve durum zaman.
- **Tanılama günlükleri**: [tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından oluşturulan tüm günlükleri içerir. Bu günlükler, Windows olayı sistem günlükleri, Azure depolama günlükleri, anahtar kasası denetim günlüklerini ve Application Gateway erişim ve güvenlik duvarı günlükleri içerir. Tüm tanılama günlükleri için merkezi ve şifrelenmiş Azure depolama hesabına arşivleme yazın. Bekletme kuruluşa özgü saklama gereksinimlerini karşılamak için kullanıcı-730 gün için yapılandırılabilir,.

**Log Analytics**: Bu günlükler, birleştirilmiş [Log Analytics](https://azure.microsoft.com/services/log-analytics/) işleme, depolama ve Panosu raporlama. Toplandığında, veriler birlikte analiz ve böylece özgün kaynağına bakılmaksızın tüm verilerin Operations Management Suite çalışma içindeki her veri türü için ayrı tablolar halinde düzenlenir. Ayrıca, Azure Güvenlik Merkezi, güvenlik olay verilerine erişmek ve diğer hizmetlerden gelen verilerle birleştirmek için Log Analytics sorguları kullanmak için sağlayarak müşterilerin Log Analytics ile entegre olur.

Aşağıdaki Log Analytics [yönetim çözümleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) Bu mimarinin bir parçası olarak dahil edilir:
-   [Active Directory değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözümü risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve öneriler için dağıtılan sunucu altyapısı belirli öncelikli bir listesini sağlar.
- [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözümü risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve müşterilerin Önceliklendirilmiş öneriler için dağıtılan sunucu altyapısı belirli listesini sağlar.
- [Aracı sistem durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): aracı durumu çözümü, kaç aracının dağıtılır ve kullanıcıların coğrafi dağılımı yanı sıra yanıt vermeyen aracı sayısı ve işletimsel veriler gönderen aracıların sayısını raporlar.
-   [Etkinlik günlüğü analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Activity Log Analytics çözümünü, bir müşteri için tüm Azure abonelikleri arasında Azure etkinlik günlüklerini analiziyle destekler.

**Azure Otomasyonu**: [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) runbook'ları yöneten depolar ve çalıştırır. Bu çözümde, runbook'ları, Azure SQL veritabanı'ndan günlüklerini toplama yardımcı olur. Otomasyon [değişiklik izleme](https://docs.microsoft.com/azure/automation/automation-change-tracking) ortamındaki değişiklikler kolayca belirlemek müşterilerin çözüm sağlar.

**Azure İzleyici**: [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) kullanıcıların performans izleme, güvenliği koruma ve denetim, uyarılar oluşturabilir ve bunların Azure'da API çağrıları izleme dahil olmak üzere, verileri arşivlemek kuruluşların etkinleştirerek eğilimleri belirlemenize yardımcı olur kaynaklar.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisine yönelik veri akış diyagramı (VAD) kullanılabilir [indirme](https://aka.ms/pcidss-iaaswa-tm) veya altında bulunabilir. Bu model, değişiklikler yaparken sistemi altyapısında potansiyel risk puanları anlamasına yardımcı olabilir.

![Iaas Web uygulaması için PCI DSS başvurusu mimari şeması](images/pcidss-iaaswa-threat-model.png "Iaas Web uygulaması için PCI DSS başvuru mimarisi diyagramı")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri

[Azure güvenlik ve uyumluluk planı - PCI DSS müşteri sorumluluk matris](https://aka.ms/pcidss-crm) sorumlulukları PCI DSS 3.2 tüm gereksinimleri listeler.

[Azure güvenlik ve uyumluluk planı - PCI DSS Iaas Web uygulaması uygulama matris](https://aka.ms/pcidss-iaaswa-cim) dahil olmak üzere ayrıntılı bilgiler üzerinde hangi PCI DSS 3.2 gereksinimleri Iaas Web uygulaması mimarisi tarafından açıklanmıştır sağlar Uygulama her kapsanan bir makaleyi gereksinimlerini nasıl karşıladığı, açıklamaları.

## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler

### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute

Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) güvenli bir şekilde bu Iaas Web uygulaması başvuru mimarisi bir parçası olarak dağıtılan kaynakların bir bağlantı kurmak için yapılandırılmış olması gerekir. Müşteriler, uygun bir VPN veya ExpressRoute ayarlayarak, Aktarımdaki veriler için koruma katmanı ekleyebilirsiniz.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ile bir Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantı, Internet üzerinden gerçekleştirilir ve şifreli bir bağlantı müşterinin ağınız ve Azure arasında güvenli bir şekilde "tüneli" bilgilerin müşterilere sağlar. Siteden siteye VPN ölçeklerde tarafından dağıtılan yıllardır güvenli, olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) şifreleme mekanizması olarak bu seçenek kullanılır.

İçindeki VPN tüneli trafik Internet bir siteden siteye VPN ile çapraz olduğundan, Microsoft başka bir ve daha güvenli bir bağlantı seçeneği sunar. Azure ExpressRoute adanmış WAN olan Azure ve şirket içi konum veya Exchange barındırma sağlayıcısı arasındaki bağlantı. ExpressRoute bağlantıları Internet üzerinden geçmemektedir gibi bu bağlantılar, daha fazla güvenilirlik, daha yüksek hız, daha düşük gecikme süreleri ve daha yüksek güvenlik tipik Internet üzerinden sunar. Ayrıca, bu müşterinin telekomünikasyon sağlayıcıları, doğrudan bir bağlantı olduğu için veriler Internet üzerinden yolculuk ediyor mu değil ve ona bu nedenle olarak gösterilmez.

Bir şirket içi ağı Azure'a genişleten güvenli bir hibrit ağı uygulamak için en iyi uygulamalardan bazılarıdır [kullanılabilir](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Bildirim

- Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR EXPRESS, ZIMNİ VEYA YASAL BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-olduğundan." Bilgi ve URL ve diğer Internet Web sitesi referansları da dahil olmak üzere bu belgede, bildirilmeksizin değiştirilebilir. Müşteriler bu belgeyi okuma KULLANIMLARDAN doğacak riskler size aittir.
- Bu belge, müşterilerle herhangi bir Microsoft ürünü veya çözümler üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.
- Müşteriler kopyalayabilir ve dahili başvuru amacıyla bu belgeyi kullanın.
- Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.
- Bu mimari, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır.
- Bu belge, bir başvuru olarak geliştirilir ve tüm anlamına gelir, bir müşteri özel uyumluluk gereksinimlerini ve düzenlemeleri karşılayabilecek tanımlamak için kullanılmamalıdır. Müşterilerin onaylı müşteri uygulamaları kuruluşları yasal Destek'ten arama.
