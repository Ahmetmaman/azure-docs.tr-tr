---
title: Azure Yönetimi ve Operations Management Suite'e (OMS) genel bakış | Microsoft Docs
description: Daha önce Operations Management Suite (OMS) ile birlikte sunulan Azure yönetim araçlarıyla ilgili içeriğe yönlendiren bağlantılar da içeren Azure uygulamalarını ve kaynaklarını yönetme alanlarına genel bakış.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/07/2018
ms.author: bwren
ms.openlocfilehash: b56993b9ad03f2ab50fe3954ab5e8855d0d8bc0f
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57770028"
---
# <a name="azure-management---monitoring"></a>Azure Yönetimi - İzleme

Azure’da izleme, Azure Yönetimi’nin bir parçasıdır.  Bu makale, kullanmaya başlamanıza yardımcı olabilecek belgeleri içermesinin yanı sıra, Azure’daki uygulamalarınızı ve kaynaklarınızı dağıtıp korumanız için gereken farklı yönetim alanlarını kısaca tanımlar.

## <a name="management-in-azure"></a>Azure’da Yönetim

Yönetim, iş uygulamalarınızı ve onları destekleyen kaynaklarınızı korumak için gereken görevleri ve süreçleri ifade eder.  Azure’da sunulan çeşitli hizmetler ve araçlar, birlikte çalışarak yalnızca Azure’da değil diğer bulutlarda ve şirket içinde çalışan uygulamalarınızı da eksiksiz biçimde yönetmenizi sağlar.  Size sunulan farklı araçları ve bu araçların çeşitli yönetim senaryolarında nasıl kullanılabileceğini anlamak, eksiksiz bir yönetim ortamı tasarlamanın ilk adımıdır.

Aşağıdaki diyagramda herhangi bir uygulamayı veya kaynağı korumak için gereken farklı yönetim alanları gösterilmektedir.  Bu farklı alanlar, bir kaynağın ömrü boyunca sürekli olarak ve art arda kullanılacakları bir yaşam döngüsü biçiminde düşünülebilir.  Bu döngü ilk dağıtımla birlikte başlar, sürekli işlemlerle devam eder ve kullanımdan kaldırıldığında son bulur.

![Yönetim özellikleri](media/management-overview/management-capabilities.png)


Aşağıdaki bölümlerde farklı yönetim alanları kısaca açıklanır ve ilgili temel Azure hizmetlerindeki ayrıntılı içeriklere yönelik bağlantılar sunulur.

## <a name="monitor"></a>İzleme
İzleme, iş uygulamalarınızın ve bağımlı oldukları kaynakların performansını, sistem durumunu ve kullanılabilirliğini belirlemeye yönelik verileri toplayıp analiz etme eylemidir. Etkili bir izleme stratejisi, uygulamanızdaki farklı bileşenlerin ayrıntılı işlemlerini anlamanıza yardımcı olur ve bazı önemli zorlukları sorun haline gelmeden önce çözmenizi sağlamak amacıyla bunları proaktif bir biçimde size bildirerek çalışma sürenizi artırabilir. Azure'da izleme temel olarak [Azure İzleyici](../azure-monitor/overview.md) tarafından sağlanır. Bu hizmet izleme verilerini depolamak için ortak depolar, uygulamanızı destekleyen farklı katmanlardan veri toplamak için birden çok veri kaynağı ve toplanan verileri analiz etmek ve gerekli işlemleri gerçekleştirmek için özellikler sunar.

## <a name="configure"></a>Yapılandırma
Yapılandırma, uygulama ve kaynakların ilk dağıtımı ile yapılandırmasına ek olarak, yama ve güncelleştirmeler ile sürdürülen bakımlarını ifade eder.  Bu görevlerin betikler ve ilkeler aracılığıyla otomasyonu sayesinde artıklığı ortadan kaldırabilir, doğruluk ve verimliliğinizi artırmak için harcadığınız zamanı ve çabayı en aza indirebilirsiniz.  [Azure Otomasyonu](../automation/automation-intro.md), yapılandırma görevlerini otomatikleştirmek için toplu hizmetler sunar.  Süreçleri otomatikleştirmeye yönelik runbook’lara ek olarak, ilkeler aracılığıyla yapılandırmayı yönetmenize ve güncelleştirmeleri tanımlayıp dağıtmanıza yardımcı olan yapılandırma ve güncelleştirme yönetimi sağlar.

## <a name="govern"></a>İdare
İdare, Azure’daki uygulama ve kaynaklarınız üzerindeki denetimi sürdürmenize yönelik mekanizmalar ve süreçler sağlar.  Girişimlerinizi planlama ve stratejik öncelikleri belirleme de bu kapsama dahildir.  Azure’da İdare, temelde iki hizmet ile uygulanır.  [Azure İlkesi](../governance/policy/overview.md), kaynaklarınız üzerinden farklı kuralları ve eylemleri uygulatarak kaynaklarınızın kurumsal standartlarınız ve hizmet düzeyi sözleşmelerinizle uyumlu kalmasından emin olmak amacıyla ilke tanımlarını oluşturmanıza, atamanıza ve yönetmenize olanak sağlar. [Cloudyn Tarafından Sağlanan Azure Maliyet Yönetimi](../cost-management/overview.md), Azure kaynaklarınızın yanı sıra AWS ve Google dahil diğer bulut sağlayıcıları için bulut kullanımınızı ve harcamalarınızı izlemenize imkan tanır.

## <a name="secure"></a>Güvenlik
Uygulamalarınızın, kaynaklarınızın ve verilerinizin yönetimi kapsamında, değerlendirilen tehditlerin, toplanıp çözümlenen güvenlik verilerinin ve uygulama ile kaynaklarınızın güvenli bir şekilde tasarlanıp yapılandırılmasını sağlamanın bir birleşimi bulunur.  [Azure Güvenlik Merkezi](../security-center/security-center-intro.md)’nin sağladığı güvenlik izleme ve tehdit analizine hibrit bulut iş yükleri arasında birleşik güvenlik yönetimi ve gelişmiş tehdit koruması dahildir.  Azure’da güvenlik ve Azure kaynaklarını güvenli bir şekilde yapılandırma hakkında kapsamlı bilgi için [Azure Güvenlik’e Giriş](../security/azure-security.md) sayfasına da başvurabilirsiniz.


## <a name="protect"></a>Koruma
Koruma, kontrolünüzün dışındaki kesintilerde bile uygulamalarınızın ve verilerinizin her zaman kullanılabilir kalmasını sağlamayı ifade eder.  Azure’da Koruma, iki hizmetle sağlanır.  [Azure Backup](../backup/backup-introduction-to-azure-backup.md), hem buluttaki hem de şirket içindeki verilerinize yönelik yedekleme ve kurtarma sağlar.    [Azure Site Recovery](../site-recovery/site-recovery-overview.md), iş sürekliliği ve olağanüstü durum oluşması halinde anında kurtarma olanağı sunarak uygulamanızın yüksek oranda kullanılabilir olmasını sağlar.

## <a name="migrate"></a>Geçiş 
Geçiş, şirket içinde çalışan mevcut iş yüklerini Azure bulut ortamına geçirmeyi ifade eder.  [Azure Geçişi](../migrate/migrate-overview.md), performansı temel alan boyutlandırma ve maliyet tahminleri de dahil olmak üzere şirket içi sanal makinelerinizin Azure’a geçiş uygunluğunu değerlendirmenize yardımcı olur.  Azure Site Recovery, sanal makinelerin [şirket içinden](../site-recovery/migrate-tutorial-on-premises-azure.md) veya [Amazon Web Services’tan](../site-recovery/migrate-tutorial-aws-azure.md) fiili geçişini gerçekleştirmenize yardımcı olur.  [Azure Veritabanı Geçişi](../dms/dms-overview.md), birden çok veritabanı kaynağını Azure Data platformlarına geçirmenizde yardımcı olur.

