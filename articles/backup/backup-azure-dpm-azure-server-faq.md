---
title: Azure Backup Sunucusu ve DPM hakkında SSS
description: Bu makalede, Microsoft Azure Backup sunucusu (MABS) ve DPM (Data Protection Manager) hakkında sık sorulan soruların yanıtlarını bulun.
ms.reviewer: srinathv
ms.topic: conceptual
ms.date: 07/05/2019
ms.openlocfilehash: 1663a842b7e00c611543451d4caef96b5b5a913f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "97955000"
---
# <a name="azure-backup-server-and-dpm---faq"></a>Azure Backup Sunucusu ve DPM-SSS

## <a name="general-questions"></a>Genel sorular

Bu makalede Azure Backup Sunucusu ve DPM hakkında sık sorulan sorular yanıtlanmaktadır.

### <a name="can-i-use-azure-backup-server-to-create-a-bare-metal-recovery-bmr-backup-for-a-physical-server"></a>Bir fiziksel sunucu için Tam Kurtarma (BMR) yedeklemesi oluşturmak üzere Azure Backup Sunucusu'nu kullanabilir miyim?

Evet.

### <a name="can-i-register-the-server-to-multiple-vaults"></a>Sunucuyu birden çok kasa 'ya kaydedebilir miyim?

Hayır. Bir DPM veya Azure Backup sunucusu yalnızca bir kasaya kaydedilebilir.

### <a name="can-i-use-dpm-to-back-up-apps-in-azure-stack"></a>Azure Stack uygulamaları yedeklemek için DPM 'yi kullanabilir miyim?

Hayır. Azure Stack korumak için Azure Backup kullanabilirsiniz Azure Backup, Azure Stack uygulamaları yedeklemek için DPM kullanımını desteklemez.

### <a name="if-ive-installed-azure-backup-agent-to-protect-my-files-and-folders-can-i-install-system-center-dpm-to-back-up-on-premises-workloads-to-azure"></a>Dosya ve klasörlerimi korumak için Azure Backup Aracısı yükledim, şirket içi iş yüklerini Azure 'a yedeklemek için System Center DPM 'yi yükleyebilir miyim?

Evet. Ancak önce DPM 'yi ayarlamanız ve ardından Azure Backup aracısını yüklemeniz gerekir.  Bileşenleri bu sırayla yüklemek Azure Backup aracısının DPM ile çalışmasına da sağlar. DPM 'yi yüklemeden önce aracının yüklenmesi önerilmez veya desteklenmez.

### <a name="why-cant-i-add-an-external-dpm-server-after-installing-ur7-and-latest-azure-backup-agent"></a>UR7 SÜRÜMLERIYLE ve en son Azure Backup aracısını yükledikten sonra neden dış DPM sunucusu ekleyemiyorum?

Buluta korunan veri kaynakları olan DPM sunucuları için (güncelleştirme paketi 7 ' den önceki bir güncelleştirme paketi kullanılarak), **dış DPM sunucusu Ekle**' yi başlatmak için UR7 sürümleriyle ve en son Azure Backup aracısını yükledikten sonra en az bir gün beklemeniz gerekir. DPM koruma gruplarının meta verilerini Azure 'a yüklemek için bir günlük zaman aralığı gereklidir. Koruma grubu meta verileri gecelik bir iş aracılığıyla ilk kez karşıya yüklenir.

### <a name="are-there-recommendations-for-configuring-exclusions-for-antivirus-software"></a>Virüsten koruma yazılımı için dışlamaları yapılandırmaya yönelik öneriler var mı?

Evet, virüsten koruma dışlamayı yapılandırmak önerilir. DPM için dışlamaları için bkz. [DPM sunucusunda virüsten koruma yazılımı çalıştırma](/system-center/dpm/run-antivirus-server). MABS için dışlamaları için bkz. [mabs sunucusu için virüsten koruma yapılandırma](backup-azure-mabs-troubleshoot.md#configure-antivirus-for-mabs-server).

## <a name="vmware-and-hyper-v-backup"></a>VMware ve Hyper-V yedeklemesi

### <a name="can-i-back-up-vmware-vcenter-servers-to-azure"></a>VMware vCenter sunucularını Azure'a yedekleyebilir miyim?

Evet. VMware vCenter Server ve ESXi konaklarının Azure 'a yedeklenmesi için Azure Backup Sunucusu kullanabilirsiniz.

- Desteklenen sürümler hakkında [daha fazla bilgi edinin](backup-mabs-protection-matrix.md) .
- Bir VMware sunucusunu yedeklemek için [Bu adımları izleyin](backup-azure-backup-server-vmware.md) .

### <a name="do-i-need-a-separate-license-to-recover-a-full-on-premises-vmwarehyper-v-cluster"></a>Tam Şirket içi VMware/Hyper-V kümesini kurtarmak için ayrı bir lisansa ihtiyacım var mı?

VMware/Hyper-V koruması için ayrı lisansa sahip olmanız gerekmez.

- System Center müşterisiyseniz, VMware VM 'lerini korumak için System Center Data Protection Manager (DPM) kullanın.
- Bir System Center müşterisi değilseniz, VMware VM 'lerini korumak için Azure Backup Sunucusu (Kullandıkça öde) kullanabilirsiniz.

### <a name="can-i-restore-a-backup-of-a-hyper-v-or-vmware-vm-stored-in-azure-to-azure-as-an-azure-vm"></a>Azure 'da depolanan bir Hyper-V veya VMware sanal makinesinin yedeklemesini Azure VM olarak Azure 'a geri yükleyebilir miyim?

Hayır, şu anda mümkün değildir. Yalnızca şirket içi bir konağa geri yükleyebilirsiniz.

## <a name="sharepoint"></a>SharePoint

### <a name="can-i-recover-a-sharepoint-item-to-the-original-location-if-sharepoint-is-configured-by-using-sql-alwayson-with-protection-on-disk"></a>SharePoint, SQL AlwaysOn (diskte koruma ile) kullanılarak yapılandırıldıysa bir SharePoint öğesini özgün konuma kurtarabilir miyim?

Evet, öğe özgün SharePoint sitesine kurtarılabilir.

### <a name="can-i-recover-a-sharepoint-database-to-the-original-location-if-sharepoint-is-configured-by-using-sql-alwayson"></a>SharePoint, SQL AlwaysOn kullanılarak yapılandırıldıysa bir SharePoint veritabanını özgün konuma kurtarabilir miyim?

SharePoint veritabanları SQL AlwaysOn 'da yapılandırıldığı için, kullanılabilirlik grubu kaldırılmadığı takdirde bunlar değiştirilemez. Sonuç olarak, DPM bir veritabanını özgün konumuna geri yükleyemiyor. SQL Server bir veritabanını başka bir SQL Server örneğine kurtarabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Diğer SSS belgelerini okuyun:

- Azure Backup Sunucusu ve DPM destek matrisi hakkında [daha fazla bilgi edinin](backup-support-matrix-mabs-dpm.md) .
- Azure Backup Sunucusu ve DPM sorun giderme yönergeleri hakkında [daha fazla bilgi edinin](backup-azure-mabs-troubleshoot.md) .
