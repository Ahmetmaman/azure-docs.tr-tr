---
title: Azure rol tabanlı erişim denetimi ile yedeklemeleri Yönetme '
description: Kurtarma Hizmetleri kasasına Yedekleme Yönetimi işlemlerinde erişimi yönetmek için rol tabanlı erişim denetimi kullanın.
services: backup
author: trinadhk
manager: shreeshd
ms.service: backup
ms.topic: conceptual
ms.date: 12/09/2018
ms.author: trinadhk
ms.openlocfilehash: e86595ceb940ebcfa702823e9c9b8ad3ef50bb45
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2019
ms.locfileid: "56674642"
---
# <a name="use-role-based-access-control-to-manage-azure-backup-recovery-points"></a>Azure Backup kurtarma noktaları yönetmek için rol tabanlı erişim denetimi kullanma
Azure Rol Tabanlı Erişim Denetimi (RBAC), Azure için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak ekibiniz içinde görevleri ayırabilir, bu işlere gerek duyan kişilere sadece erişim miktarını verebilirsiniz.

> [!IMPORTANT]
> Azure Backup tarafından sağlanan rolleri Azure portalında veya REST API aracılığıyla gerçekleştirilen eylemlere sınırlı veya PowerShell veya CLI cmdlet'leri kurtarma Hizmetleri kasası. Data Protection Manager kullanıcı arabirimini veya Azure Backup sunucusu kullanıcı Arabirimi denetimi bu rollerden dışında olan Azure Yedekleme aracısı istemcisi kullanıcı Arabirimi veya System center gerçekleştirilen eylemler.

Azure Backup, yedekleme yönetim işlemlerini denetlemek için 3 yerleşik rol sağlar. [Azure RBAC yerleşik rolleri](../role-based-access-control/built-in-roles.md) hakkında daha fazla bilgi edinin

* [Yedekleme Katılımcısı](../role-based-access-control/built-in-roles.md#backup-contributor) -bu rol oluşturmak ve kurtarma Hizmetleri kasası oluşturmaya ve başkalarına erişim verme dışındaki yedekleme yönetmek için tüm izinlere sahiptir. Bu rol tüm yedekleme Yönetimi işlemi yapmak için Yedekleme Yönetimi Yöneticisi olarak düşünün.
* [Yedekleme işleci](../role-based-access-control/built-in-roles.md#backup-operator) -bu rolün katkıda bulunan yedekleme ve yönetmeye yedekleme ilkelerini kaldırma dışındaki her şey için izinlere sahip. İçeren yedeklemeyi durdurma verileri silmek ya da şirket içi kaynaklara kaydını kaldırma gibi zararlı işlemler gerçekleştirilemiyor dışında bu rol için katkıda bulunan eşdeğerdir.
* [Yedekleme okuyucusu](../role-based-access-control/built-in-roles.md#backup-reader) -bu rolün tüm yedekleme yönetimi işlemlerini görüntüleme iznine sahiptir. Bu rol, bir izleme kişi olmasını düşünün.

Daha fazla denetim için kendi rolleri tanımlamak için arıyorsanız, bkz. nasıl [özel roller oluşturma](../role-based-access-control/custom-roles.md) Azure rbac'de.



## <a name="mapping-backup-built-in-roles-to-backup-management-actions"></a>Yedekleme yönetim eylemleri için eşleme yedekleme yerleşik roller
Aşağıdaki tabloda, yedekleme yönetim eylemleri ve bu işlemi gerçekleştirmek için gerekli olan karşılık gelen en düşük RBAC rolü yakalar.

| Yönetim işlemi | Gerekli en düşük RBAC rolü | Kapsamı gereklidir |
| --- | --- | --- |
| Kurtarma Hizmetleri kasası oluşturma | Katılımcı | Kasa içeren kaynak grubu |
| Azure VM yedeklemeyi etkinleştirme | Yedekleme İşleci | Kasa içeren kaynak grubu |
| | Sanal Makine Katılımcısı | VM kaynağı |
| İsteğe bağlı yedekleme VM | Yedekleme İşleci | Kurtarma kasası kaynağı |
| VM'yi geri yükle | Yedekleme İşleci | Kurtarma Hizmetleri kasası |
| | Katılımcı | VM'nin dağıtılacağı kaynak grubu |
| | Sanal Makine Katılımcısı | Kaynak yedeklenen sanal makine |
| Yönetilmeyen diskler VM yedeklemesini geri yükleme | Yedekleme İşleci | Kurtarma kasası kaynağı |
| | Sanal Makine Katılımcısı | Kaynak yedeklenen sanal makine |
| | Depolama Hesabı Katılımcısı | Diskleri geri nerede bulunacağını depolama hesabı kaynağı |
| Yönetilen diskler, VM yedekten geri yükleyin | Yedekleme İşleci | Kurtarma kasası kaynağı |
| | Sanal Makine Katılımcısı | Kaynak yedeklenen sanal makine |
| | Depolama Hesabı Katılımcısı | Yönetilen disklere dönüştürmeden önce verileri kasadan tutmak için geri yükleme işleminin bir parçası olarak seçili olan geçici depolama hesabı |
| | Katılımcı | Kaynak grubu için yönetilen diskleri geri yüklenmesi |
| Sanal makine yedeklemesini tek tek dosyaları geri yükleme | Yedekleme İşleci | Kurtarma kasası kaynağı |
| | Sanal Makine Katılımcısı | Kaynak yedeklenen sanal makine |
| Azure VM yedeklemesi için yedekleme ilkesi oluşturma | Yedekleme Katılımcısı | Kurtarma kasası kaynağı |
| Azure VM yedekleme, yedekleme ilkesini değiştirme | Yedekleme Katılımcısı | Kurtarma kasası kaynağı |
| Azure VM yedekleme, yedekleme ilkesini silme | Yedekleme Katılımcısı | Kurtarma kasası kaynağı |
| Yedeklemeyi Durdur (verileri tut ile veya veri silme) VM yedeklemesi hakkında | Yedekleme Katılımcısı | Kurtarma kasası kaynağı |
| Şirket içi Windows Server/istemci/SCDPM ya da Azure Backup sunucusu kaydetme | Yedekleme İşleci | Kurtarma kasası kaynağı |
| Şirket içi Windows Server/istemci/SCDPM ya da Azure Backup sunucusu silme kayıtlı | Yedekleme Katılımcısı | Kurtarma kasası kaynağı |

> [!IMPORTANT]
> VM katkıda bulunan bir sanal makine kaynak kapsamda belirtin ve yedekleme VM ayarlarının bir parçası tıklayın, sanal makine zaten yalnızca abonelik düzeyinde yedekleme durumu çalıştığını doğrulamak için bir çağrı olarak yedeklenen olsa bile 'Yedeklemeyi Etkinleştir' ekranı açılır. Bunu önlemek için ya da kasa ve sanal makinenin yedekleme öğesi görünümü açma veya abonelik düzeyinde VM katkıda bulunanı rolü belirtin gidin. 

## <a name="next-steps"></a>Sonraki adımlar
* [Rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md): Azure portalında RBAC ile çalışmaya başlayın.
* Erişim ile yönetmeyi öğrenin:
  * [PowerShell](../role-based-access-control/role-assignments-powershell.md)
  * [Azure CLI](../role-based-access-control/role-assignments-cli.md)
  * [REST API](../role-based-access-control/role-assignments-rest.md)
* [Rol tabanlı erişim denetimi sorunlarını giderme](../role-based-access-control/troubleshooting.md): Yaygın sorunları çözme için öneriler alın.
