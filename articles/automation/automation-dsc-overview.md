---
title: Azure Otomasyonu durum yapılandırmasına genel bakış
description: Bir genel bakış, Azure Otomasyonu durum yapılandırması (DSC), terimleri ve bilinen sorunlar
keywords: PowerShell dsc, istenen durum yapılandırması, powershell dsc azure
services: automation
ms.service: automation
ms.component: dsc
author: bobbytreed
ms.author: robreed
ms.date: 11/06/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1f28f642d1a5fc30055c73a4b7d60c076c83d204
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51250110"
---
# <a name="azure-automation-state-configuration-overview"></a>Azure Otomasyonu durum yapılandırmasına genel bakış

Azure Otomasyonu durumu yapılandırmadır yazmak, yönetmek ve PowerShell Desired State Configuration (DSC) derleme olanak tanıyan bir Azure hizmeti [yapılandırmaları](/powershell/dsc/configurations), içeri aktarma [DSC kaynakları](/powershell/dsc/resources), ve Hedef düğümler, tüm bulut yapılandırmaları atayın.

## <a name="why-use-azure-automation-state-configuration"></a>Neden Azure Otomasyon durum yapılandırması kullanın

Azure Otomasyonu durum yapılandırması üzerinden Azure dışındaki DSC kullanarak çeşitli avantajlar sağlar.

### <a name="built-in-pull-server"></a>Yerleşik çekme sunucusu

Azure Otomasyonu durumu yapılandırma, benzer bir DSC çekme sunucusu sağlar [Windows özelliği DSC hizmet](/powershell/dsc/pullserver) hedef düğümler otomatik olarak yapılandırmaları alabilmesi adına, istenen duruma uyumlu hale gelir ve geri raporlama kendi Uyumluluk. Azure automation'da yerleşik çekme sunucusu ayarlama ve kendi çekme sunucunuz ihtiyacını ortadan kaldırır. Azure Otomasyonu, sanal veya fiziksel Windows veya Linux makineleri, bulutta veya şirket içi hedefleyebilirsiniz.

### <a name="management-of-all-your-dsc-artifacts"></a>Tüm DSC yapıların Yönetimi

Azure Otomasyonu durum Yapılandırması Yönetim katmanının aynısını getirir [PowerShell Desired State Configuration](/powershell/dsc/overview) Azure Otomasyonu, PowerShell betikleri için sunduğu.

Azure portaldan veya powershell'den, tüm, DSC yapılandırmaları, kaynak ve hedef düğümler olarak yönetebilirsiniz.

![Azure Otomasyonu sayfasının ekran görüntüsü](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a>Log Analytics'e raporlama verilerini içeri aktarma

Azure Otomasyonu durum yapılandırması ile yönetilen düğümler ayrıntılı raporlama Durum verilerini yerleşik çekme sunucusuna gönderir. Log Analytics çalışma alanınıza bu veri göndermek için Azure Otomasyon durum yapılandırması yapılandırabilirsiniz. Log Analytics çalışma alanınıza durum yapılandırması Durum verilerini gönderme hakkında bilgi edinmek için bkz: [İleri Azure Otomasyon durum raporlama verilerini Log Analytics'e Yapılandırması](automation-dsc-diagnostics.md).

## <a name="network-planning"></a>Ağınızı yapılandırmak

Aşağıdaki bağlantı noktası ve URL'leri için durum yapılandırması (otomasyon ile iletişim kurmak için DSC) gereklidir:

* Bağlantı noktası: Yalnızca TCP 443 giden internet erişimi için gereklidir.
* Genel URL: *.azure-automation.net
* ABD Devleti Virginia genel URL: *.azure-automation.us
* Aracı hizmeti: https://\<Workspaceıd\>.agentsvc.azure-automation.net

Özel durumlar tanımlarken listelenen adreslerini kullanmak için önerilir. IP adreslerinin indirebilirsiniz [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). Bu dosya haftalık olarak güncelleştirilir ve şu anda dağıtılmış aralıkları ve IP adreslerinde gelecekte yapılacak değişiklikleri vardır.

Belirli bir bölge için tanımlanmış bir Otomasyon hesabınız varsa bu Bölgesel veri merkezi iletişimin kısıtlayabilirsiniz. Aşağıdaki tabloda, her bölge için DNS kaydı verilmiştir:

| **Bölge** | **DNS kaydı** |
| --- | --- |
| Batı Orta ABD | wcus-jobruntimedata-prod-su1.azure-automation.net</br>wcus-agentservice-prod-1.azure-automation.net |
| Orta Güney ABD |scus-jobruntimedata-prod-su1.azure-automation.net</br>scus-agentservice-prod-1.azure-automation.net |
| Doğu ABD 2 |eus2-jobruntimedata-prod-su1.azure-automation.net</br>eus2-agentservice-prod-1.azure-automation.net |
| Orta Kanada |cc-jobruntimedata-prod-su1.azure-automation.net</br>cc-agentservice-prod-1.azure-automation.net |
| Batı Avrupa |we-jobruntimedata-prod-su1.azure-automation.net</br>we-agentservice-prod-1.azure-automation.net |
| Kuzey Avrupa |ne-jobruntimedata-prod-su1.azure-automation.net</br>ne-agentservice-prod-1.azure-automation.net |
| Güneydoğu Asya |sea-jobruntimedata-prod-su1.azure-automation.net</br>sea-agentservice-prod-1.azure-automation.net|
| Orta Hindistan |cid-jobruntimedata-prod-su1.azure-automation.net</br>cid-agentservice-prod-1.azure-automation.net |
| Japonya Doğu |jpe-jobruntimedata-prod-su1.azure-automation.net</br>jpe-agentservice-prod-1.azure-automation.net |
| Avustralya Güneydoğu |ase-jobruntimedata-prod-su1.azure-automation.net</br>ase-agentservice-prod-1.azure-automation.net |
| Birleşik Krallık Güney | uks-jobruntimedata-prod-su1.azure-automation.net</br>uks-agentservice-prod-1.azure-automation.net |
| ABD Devleti Virginia | usge-jobruntimedata-prod-su1.azure-automation.us<br>usge-agentservice-prod-1.azure-automation.us |

Bölge bölge adları yerine IP adresleri listesi için indirme [Azure veri merkezi IP adresi](https://www.microsoft.com/download/details.aspx?id=41653) XML dosyasından Microsoft Download Center.

> [!NOTE]
> Azure veri merkezi IP adresi XML dosyası Microsoft Azure veri merkezlerinde kullanılan IP adresi aralıklarını listeler. İşlem, SQL ve depolama aralıkları dosyası içerir.
>
>Güncelleştirilen bir dosya haftalık olarak gönderilir. Dosya şu anda dağıtılmış aralıkları ve IP adreslerinde gelecekte yapılacak değişiklikleri yansıtır. Dosyada görünen yeni aralıklar en az bir hafta boyunca veri merkezlerinde kullanılmaz.
>
> Her hafta yeni XML dosyasını indirmek için iyi bir fikirdir. Ardından, Azure'da çalışan hizmetleri doğru şekilde tanımlamak üzere sitenizde güncelleştirin. Azure ExpressRoute kullanıcıların bu dosyayı her ayın ilk haftasında Azure alanındaki Border Gateway Protocol (BGP) reklamı güncelleştirmek için kullanıldığını unutmamalısınız.

## <a name="introduction-video"></a>Tanıtım videosu

Okumak yerine izlemeyi mi tercih ediyorsunuz? Azure Otomasyonu durum yapılandırması ilk zaman Duyuruldu Mayıs 2015 aşağıdaki videoya göz vardır.

> [!NOTE]
> Bu videoda ele alınan yaşam döngüsü ve kavramları doğru olsa da, bu videonun kayda alındığı beri Azure Otomasyonu durumu yapılandırma çok ilerledikten. Genel kullanıma sunulmuştur, Azure portalında bir çok daha geniş bir kullanıcı Arabirimine sahiptir ve çok sayıda ek özellikleri destekler.

[!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a>Sonraki adımlar

- Başlamak için bkz: [Azure Otomasyon durum yapılandırması ile çalışmaya başlama](automation-dsc-getting-started.md)
- Bilgi edinmek için nasıl yerleşik düğümlerine bkz [makineleri Azure Otomasyon durum yapılandırması tarafından Yönetim için hazırlama](automation-dsc-onboarding.md)
- Hedef düğümleri atayabilirsiniz böylece, DSC yapılandırmaları derleme hakkında bilgi edinmek için [yapılandırmaları Azure Automation durumu yapılandırma derleme](automation-dsc-compile.md)
- PowerShell cmdlet başvurusu için bkz. [Azure Otomasyonu durumu yapılandırma cmdlet'leri](/powershell/module/azurerm.automation/#automation)
- Fiyatlandırma bilgileri için bkz: [Azure Otomasyon durum yapılandırması için fiyatlandırma](https://azure.microsoft.com/pricing/details/automation/)
- Bir sürekli dağıtım işlem hattı, Azure Otomasyonu durum yapılandırmasını kullanarak bir örnek görmek için bkz: [sürekli dağıtımı kullanarak Azure Otomasyon durum yapılandırması ve Chocolatey](automation-dsc-cd-chocolatey.md)