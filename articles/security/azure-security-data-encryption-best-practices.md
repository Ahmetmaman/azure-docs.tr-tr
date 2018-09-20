---
title: Veri güvenliği ve şifreleme en iyi uygulamalar | Microsoft Docs
description: Bu makalede bir dizi veri güvenliği için en iyi yöntemler ve şifreleme kullanılarak Azure özellikleri.
services: security
documentationcenter: na
author: barclayn
manager: mbalwin
editor: TomSh
ms.assetid: 17ba67ad-e5cd-4a8f-b435-5218df753ca4
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2018
ms.author: barclayn
ms.openlocfilehash: 263c04fd15240f365f2325c69d5cb25aa1a539f0
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46465886"
---
# <a name="azure-data-security-and-encryption-best-practices"></a>Azure veri güvenliği ve şifreleme için en iyi uygulamalar
Buluttaki verileri korumaya yardımcı olmak için verilerinizi oluşan ve bu durum için hangi denetimleri kullanılabilir durumda hesabı gerekir. Azure veri güvenliği ve şifreleme için en iyi uygulamalar aşağıdaki veri durumlarını ilgilidir:

- Bekleyen: Bu, tüm bilgileri depolama nesneleri, kapsayıcılar ve statik olarak fiziksel medyada manyetik olmadığını mevcut türleri veya optik diski içerir.
- Aktarım: veri bileşenleri, konumları ve programlar arasında aktarıldığı Aktarımdaki olur. Örnekler arasında (şirket içinden Bulut ve karma bağlantılar ile ExpressRoute gibi dahil olmak üzere tersi,), service bus ağ üzerinden aktarım olan veya bir giriş/çıkış işlemi sırasında.

Azure veri güvenliği ve şifreleme en iyi yöntemler koleksiyonu bu makalede ele alınacaktır. Bu en iyi uygulamaları, Azure veri güvenliği ve şifreleme ve deneyimler sizin gibi müşterilerin deneyimlerimizden türetilir.

En iyi her uygulama için açıklayacağız:

* En iyi nedir
* Bu en iyi etkinleştirmek istediğiniz neden
* En iyi etkinleştirme başarısız olursa ne sonuç olabilir
* En iyi olası alternatifler
* Nasıl en iyi etkinleştirmek bilgi edinebilirsiniz

Bu makalenin yazıldığı sırada oldukları gibi bu Azure veri güvenliği ve şifreleme için en iyi yöntemler makalesi bir fikir birliğine varılmış fikrim, Azure platformu özellikleri ve özellik kümeleri üzerinde temel alır. Fikirlerini ve teknolojileri zamanla değişir ve bu makalede, bu değişiklikleri yansıtacak şekilde düzenli olarak güncelleştirilir.

## <a name="choose-a-key-management-solution"></a>Anahtar yönetimi çözümü seçme
Anahtarlarınızı korumak, bulutta verilerinizi korumak için gereklidir.

[Azure Key Vault](../key-vault/key-vault-overview.md) korunmasına yardımcı şifreleme anahtarlarını ve bulut uygulamaları ve Hizmetleri gizli dizileri kullanabilirsiniz. Anahtar Kasası anahtar yönetimi işlemini kolaylaştırır ve verilerinize erişen ve bunları şifreleyen anahtarları denetiminizde tutmanıza olanak sağlar. Geliştiriciler, geliştirme ve test amacıyla dakikalar içinde anahtarları oluşturmak ve ardından bunları üretim anahtarlarına geçirin. Güvenlik yöneticileri gerektiğinde anahtarlara izin verebilir (ve iptal edebilir).

Key Vault, kasa adlı birden fazla güvenli kapsayıcı oluşturmak için kullanabilirsiniz. Bu kasalar Hsm'leri tarafından desteklenir. Kasalar, uygulama gizli dizilerinin depolanmasını merkezi hale getirerek güvenlik bilgilerini kazayla kaybetme olasılığını azaltmaya yardımcı olur. Anahtar kasaları ayrıca denetlemek ve içlerinde depolanmış her şeye erişimi oturum. Azure Key Vault, isteme ve Aktarım Katmanı Güvenliği (TLS) sertifikalarını yenileme başa çıkabilir. Sertifika yaşam döngüsü yönetimi için sağlam bir çözüm özellikleri sunar.

Azure Key Vault, uygulama anahtarlarını ve gizli dizilerini desteklemek için tasarlanmıştır. Key Vault, bir mağaza kullanıcı parolaları için tasarlanmamıştır.

Key Vault kullanmaya yönelik en iyi güvenlik yöntemleri aşağıda verilmiştir.

**En iyi yöntem**: kullanıcılar, gruplara ve uygulamalara belirli bir kapsamda için erişim verin.   
**Ayrıntı**: kullanım RBAC'ın önceden tanımlanmış roller. Örneğin, bir kullanıcıya anahtar kasalarını yönetmesi için erişim vermek için önceden tanımlanmış role atadığınız [Key Vault katkıda bulunanı](../role-based-access-control/built-in-roles.md) belirli bir kapsamda bu kullanıcıya. Kapsam, bir abonelik, kaynak grubu veya yalnızca belirli bir anahtar kasası bu durumda olacaktır. Önceden tanımlı roller ihtiyaçlarınıza uygun olmayan varsa [kendi rolleri tanımlama](../role-based-access-control/custom-roles.md).

**En iyi yöntem**: sahip kullanıcıların erişim denetimi.   
**Ayrıntı**: bir anahtar kasasına erişim, iki ayrı arabirim ile denetlenir: Yönetim düzlemi ve veri düzlemi. Yönetim düzlemi ve veri düzlemi erişim denetimleri birbirinden bağımsız olarak çalışır.

Hangi kullanıcıların denetlemek için RBAC kullanma erişime sahiptir. Örneğin, bir anahtar kasasındaki anahtarları kullanmak için bir uygulama erişim vermek istiyorsanız, yalnızca anahtar kasası erişim ilkelerini kullanarak veri düzlemi erişim izinleri vermeniz gerekir ve bu uygulama için yönetim düzlemi erişimi gereklidir. Buna karşılık, istediğiniz bir kullanıcının kasa özelliklerini okuyabilir ve etiketleri ancak hiçbir erişim anahtarları, parola veya sertifikalara sahip değil, RBAC kullanarak bu kullanıcı okuma erişimi verebilirsiniz ve veri düzlemi erişimi gerekli değildir.

**En iyi yöntem**: sertifika anahtar kasanıza Store. Sertifikalarınızı yüksek değeri var. Yanlış kişiye, uygulamanızın güvenlik veya veri güvenliğini tehlikeye girebilir.   
**Ayrıntı**: Azure Resource Manager Vm'leri dağıtıldığında, Azure Vm'leri için Azure Key vault'ta depolanan sertifikaları güvenli bir şekilde dağıtabilirsiniz. Anahtar kasası için uygun erişim ilkeleri ayarlayarak sertifikanıza erişim alan kişileri denetleyebilirsiniz. Azure anahtar Kasası'nda tek bir yerde tüm sertifikaların yönettiğiniz başka bir avantajdır. Bkz: [müşteri tarafından yönetilen Key vault'tan dağıtma sertifikaların vm'lere](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/) daha fazla bilgi için.

**En iyi yöntem**: anahtar kasalarını ya da anahtar kasası nesneleri bir silme işlemi kurtarabilirsiniz emin olun.   
**Ayrıntı**: silme anahtar kasaları ya da anahtar kasası nesne yanlışlıkla veya kötü amaçlı olabilir. Geçici silmeyi etkinleştir ve Key Vault, özellikle bekleyen verileri şifrelemek için kullanılan anahtarları için koruma özelliklerini temizleme. Silinen kasaları kurtarma ve gerekirse nesneleri kasa Bu anahtarları silme işlemi, veri kaybına eşdeğerdir. Key Vault kurtarma işlemleri düzenli aralıklarla uygulama.

> [!NOTE]
> Bir kullanıcı için bir anahtar kasası yönetim düzleminde katkıda bulunan izinlerine (RBAC) sahipse, bunlar kendilerini veri düzlemine erişim anahtar kasası erişim ilkesini ayarlayarak verebilirsiniz. Kimlerin katkıda bulunan erişimi yalnızca yetkili kişiler erişebilir ve anahtar kasası, anahtarlara, parolalara ve sertifikalara yönetme emin olmak için anahtar kasalarınıza, sıkı bir şekilde kontrol öneririz.
>
>

## <a name="manage-with-secure-workstations"></a>Güvenli iş istasyonları ile yönetme
> [!NOTE]
> Abonelik Yöneticisi veya sahibi güvenli erişim iş istasyonu veya ayrıcalıklı erişim iş istasyonu kullanmanız gerekir.
>
>

Saldırıları büyük çoğunluğu hedef için son kullanıcı, uç nokta saldırı birincil noktalarından birine hale gelir. Uç nokta etkilediğinde bir saldırganın kullanıcının kimlik bilgilerini kuruluş verilerine erişmek için kullanabilirsiniz. Çoğu uç nokta saldırıları, kullanıcılar kendi yerel iş istasyonları yöneticileri bulunmasına avantajlarından yararlanın.

**En iyi yöntem**: güvenli yönetim iş istasyonu hassas hesaplar, görevleri ve verileri korumak için kullanın.   
**Ayrıntı**: kullanım bir [ayrıcalıklı erişim iş istasyonu](https://technet.microsoft.com/library/mt634654.aspx) iş istasyonları saldırı yüzeyini azaltmak için. Bu güvenli yönetim iş istasyonları, bazı bu saldırıları azaltmak ve verilerinizi daha güvenli olmasına yardımcı olabilir.

**En iyi yöntem**: uç nokta koruması sağlayın.   
**Ayrıntı**: güvenlik ilkelerini zorunlu kılmasına (Bulut veya şirket içi) veri konumdan bağımsız olarak, veri tüketmek için kullanılan tüm cihazlar arasında.

## <a name="protect-data-at-rest"></a>Bekleyen verileri koruma
[Bekleyen verileri şifreleme](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) veri gizlilik, uyumluluk ve veri egemenliği doğru zorunlu bir adımdır.

**En iyi yöntem**: verilerinizin korunmasına yardımcı olmak için disk şifrelemesi uygulayın.   
**Ayrıntı**: kullanım [Azure Disk şifrelemesi](azure-security-disk-encryption.md). Bu, BT yöneticilerinin Windows ve Linux Iaas VM diskleri şifreleme sağlar. Disk şifrelemesi endüstri standardı Windows BitLocker özelliğini ve işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için Linux dm-crypt özelliğini birleştirir.

Azure depolama ve Azure SQL veritabanı bekleyen veriler varsayılan ve çoğu Hizmetleri teklif şifreleme bir seçenek olarak şifreleyin. Verilerinizi şifrelemek ve erişim anahtarları denetiminizde tutmanıza olanak Azure anahtar Kasası'nı kullanabilirsiniz. Bkz: [daha fazla bilgi için Azure kaynak sağlayıcıları şifreleme modeli desteği](azure-security-encryption-atrest.md#azure-resource-providers-encryption-model-support).

**En iyi uygulamalar**: riskleri azaltmanıza yardımcı olmak için şifreleme kullan ilgili verilere yetkisiz erişim.   
**Ayrıntı**: kendisine hassas verileri yazmadan önce sürücülerini şifreleyebilirsiniz.

Veri şifrelemeyi zorunlu olmayan kuruluşlar, veri bütünlüğü sorunları için daha fazla sunulur. Örneğin, kullanıcıların yetkisiz veya düzenleyen bir ele geçirilen hesaplar veri çalan veya açık bir biçimde kodlanmış verilere yetkisiz erişim. Şirketler, ayrıca sektör yönetmeliklerine uyum sağlamak için veri güvenliğini artırmak için dikkatli ve kullanarak doğru güvenlik denetimleri olduklarını kanıtlamaları gerekir.

## <a name="protect-data-in-transit"></a>Aktarımdaki verileri koruma
Aktarımdaki verileri korumak, veri koruma stratejinizin önemli bir parçası olmalıdır. Verileri geri ve İleri birçok konumlardan taşındığından, genellikle, her zaman SSL/TLS protokolleri farklı konumlar arasında veri alışverişi yapmak için kullanmanızı öneririz. Bazı durumlarda, şirket içi ve bulut arasındaki tüm iletişim kanalını yalıtmak isteyebilirsiniz VPN kullanarak altyapıları.

Verileri, şirket içi altyapı ile Azure arasında taşıma için HTTPS veya VPN gibi uygun güvenlik önlemleri göz önünde bulundurun. Bir Azure sanal ağı ve şirket içi konum arasında şifrelenmiş trafik genel internet üzerinden gönderirken kullanmak [Azure VPN ağ geçidi](https://docs.microsoft.com/azure/vpn-gateway/).

Azure VPN ağ geçidi, SSL/TLS ve HTTPS kullanarak belirli en iyi uygulamalar aşağıda verilmiştir.

**En iyi yöntem**: birden çok iş istasyonlarından güvenli erişimi bulunan şirket içi bir Azure sanal ağı.   
**Ayrıntı**: kullanım [siteden siteye VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).

**En iyi yöntem**: tek bir iş istasyonundan güvenli erişimi bulunan şirket içi bir Azure sanal ağı.   
**Ayrıntı**: kullanım [noktadan siteye VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md).

**En iyi yöntem**: adanmış bir yüksek hızlı WAN bağlantısı üzerinden daha büyük veri kümeleri taşıyın.   
**Ayrıntı**: kullanım [ExpressRoute](../expressroute/expressroute-introduction.md). ExpressRoute kullanmayı seçerseniz, ayrıca uygulama düzeyinde verileri kullanarak şifreleme de yapabilirsiniz [SSL/TLS](https://support.microsoft.com/kb/257591) veya diğer protokoller için ek koruma.

**En iyi yöntem**: Azure portalı üzerinden Azure depolama ile etkileşim.   
**Ayrıntı**: tüm HTTPS gerçekleşir. Ayrıca [depolama REST API'si](https://msdn.microsoft.com/library/azure/dd179355.aspx) ile etkileşim kurmak için HTTPS üzerinden [Azure depolama](https://azure.microsoft.com/services/storage/) ve [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/).

Aktarımdaki verileri korumak için başarısız olan kuruluşlar için daha elverişli [adam-de-adam saldırılarına](https://technet.microsoft.com/library/gg195821.aspx), [gizlice](https://technet.microsoft.com/library/gg195641.aspx)ve oturum ele geçirme. Bu tür saldırıları, gizli verilere erişimini ilk adımı olabilir.

## <a name="secure-email-documents-and-sensitive-data"></a>Güvenli e-posta, belgeler ve hassas veriler
Denetim ve e-posta, belgeler ve şirketinizin dışındaki kişilerle paylaştığınız hassas verilerin güvenliğini sağlamak istiyorsunuz. [Azure Information Protection](https://docs.microsoft.com/azure/information-protection/) sınıflandırmak, etiketlemek ve kendi belge ve e-postaları korumaya yardımcı olan bulut tabanlı bir çözüm. Bu otomatik olarak kural ve koşulları, el ile kullanıcı veya kullanıcılar önerileri nereden bir birleşimi tarafından tanımlayan yöneticiler tarafından yapılabilir.

Belirlenebilir her zaman, bağımsız olarak verilerin depolandığı veya kimle paylaşıldığından sınıflandırılmasıdır. Etiketler üst bilgi, alt bilgi veya filigran gibi görsel işaretler içerir. Meta veri dosyaları ve e-posta üst bilgilerine düz metin olarak eklenir. Düz metin, veri kaybını önleme çözümleri gibi diğer hizmetlerin Sınıflandırmayı tanımlayabilmesini ve uygun eylemde sağlar.

Koruma teknolojisi, Azure Rights Management (Azure RMS) kullanır. Bu teknoloji, diğer Microsoft bulut Hizmetleri ve Office 365 ve Azure Active Directory gibi uygulamaları ile tümleşiktir. Bu koruma teknolojisinde şifreleme, kimlik ve yetkilendirme ilkelerini kullanır. Kalır, Azure RMS uygulanan koruma da belgelerde ve e-posta, konumlarından — içinde veya dışında kuruluş, ağlar, dosya sunucuları ve uygulamalar.

Bu bilgi koruma çözümünü bile diğer kişilerle paylaşıldığında, verilerinizin denetimi tutar. Azure RMS iş kolu satır uygulama ve yazılım satıcılarından bilgi koruması çözümleri ile bu uygulama ve çözümlerin şirket içinde olup olmadığını veya bulutta kullanabilirsiniz.

Olmasını öneririz:

- [Azure Information Protection dağıtmadan](https://docs.microsoft.com/azure/information-protection/deployment-roadmap) kuruluşunuz için.
- İş gereksinimlerinizi yansıtan olarak etiketleri uygulayın. Örneğin: "çok gizli" tüm belgeler ve e-postaları sınıflandırmak ve bu verileri korumak için top-secret veriler içerdiğini, adlı bir etiket uygulayın. Ardından, yalnızca yetkili kullanıcılar bu veriler, belirttiğiniz herhangi bir kısıtlama olmadan erişebilirsiniz.
- Yapılandırma [Azure RMS Kullanım günlüğü](https://docs.microsoft.com/azure/information-protection/log-analyze-usage) böylece kuruluşunuzun koruma hizmeti nasıl kullandığını izleyebilirsiniz.

Zayıf üzerinde kuruluşlar [veri sınıflandırması](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) ve dosya koruması, veri sızıntısını veya veri kötüye kullanımdan daha elverişli olabilir. Uygun dosya koruma ile işletmenizi öngörü, riskli davranışları algılayıp ve düzeltici önlemler alabilir, belgelere erişimi izleyebilir ve benzeri veri akışlarını analiz edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [Azure güvenlik en iyi uygulamaları ve desenleri](security-best-practices-and-patterns.md) kullanmak üzere daha fazla güvenlik için en iyi yöntemler, tasarlama, dağıtma ve Azure'ı kullanarak bulut çözümlerinizi yönetme.

Aşağıdaki kaynaklar, Azure güvenliği ve ilgili Microsoft Hizmetleri hakkında daha fazla genel bilgi sağlamak kullanılabilir:
* [Azure güvenlik ekibi blogu](https://blogs.msdn.microsoft.com/azuresecurity/) - Azure güvenliği en güncel bilgi için
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx) - Azure ile ilgili sorunlar dahil Microsoft güvenlik açıklarının bildirilebileceği veya e-posta aracılığıyla secure@microsoft.com
