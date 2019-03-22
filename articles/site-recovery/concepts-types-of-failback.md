---
title: Yeniden çalışma sırasında Azure Site Recovery ile olağanüstü durum kurtarma | Microsoft Docs
description: Bu makalede, çeşitli türlerde geri dönme ve şirket içi dön yük boyunca Azure Site Recovery hizmeti ile olağanüstü durum kurtarma sırasında değerlendirilmesi için uyarılar genel bakış sağlar.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: raynew
ms.openlocfilehash: 6f178ef1a0aec7f742bce4c2570962b995876026
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58316278"
---
# <a name="failback-after-disaster-recovery-of-vmware-vms"></a>VMware vm'lerinin olağanüstü durum kurtarma işleminden sonra yeniden çalışma

Üzerinden Azure'a olağanüstü durum kurtarma işleminin bir parçası başarısız olduktan sonra şirket içi sitenize geri dönebilirsiniz. Yeniden çalışma, Azure Site Recovery ile olası iki farklı tür vardır: 

- Özgün konuma geri başarısız 
- Alternatif bir konuma başarısız

Bir VMware sanal makinesi üzerinde başarısız olursa geri aynı kaynak şirket içi sanal makineye hala devam ediyorsa başarısız olabilir. Bu senaryoda, geri yalnızca değişiklikler çoğaltılır. Bu senaryo olarak da bilinen **özgün konuma kurtarma**. Şirket içi sanal makine mevcut değilse senaryodur bir **alternatif konum kurtarma**.

> [!NOTE]
> Yalnızca özgün vCenter ve yapılandırma sunucusu için başarısız olabilir. Yeni bir yapılandırma sunucusunu dağıtma ve kullanarak tekrar başarısız olamaz. Ayrıca, yeni bir vCenter mevcut yapılandırma sunucusu ve yeniden çalışma için yeni vCenter ekleyemezsiniz.

## <a name="original-location-recovery-olr"></a>Özgün konum kurtarması (OLR ile)
Orijinal sanal makine başarısız seçerseniz, aşağıdaki koşulların karşılanması gerekir:

* Sanal makine vCenter sunucusu tarafından yönetiliyorsa ana hedefin ESX konağı sanal makinenin veri deposunu erişimi sahip olmalıdır.
* Sanal makine bir ESX konağında olmakla birlikte vCenter tarafından yönetilmeyen sanal makine sabit diskini ana hedefin ana erişebilen bir veri deposunda olmalıdır.
* Sanal makinenize bir ESX konağında ve vCenter kullanmıyorsa, yeniden koruma önce olan ana hedef ESX konağının bulma tamamlamanız gerekir. Fiziksel sunucuları yeniden, çok devrediyorsanız bu geçerlidir.
* Bir sanal depolama alanı ağı (vSAN) veya (RDM) diskleri zaten var ve şirket içi sanal makineye bağlı eşleme ham cihaz temel bir disk için geri dönebilirsiniz.

> [!IMPORTANT]
> Yeniden çalışma sırasında Azure Site Recovery hizmeti bekleyen değişiklikleri yazılacağı sanal makinede özgün VMDK tanımlayamayabilir disk.enableUUID= TRUE sağlamak önemlidir. Bu değer ayarlı değil ise TRUE olması, ardından hizmet karşılık gelen bir şirket içi VMDK en iyi çaba ilkesine göre tanımlamayı dener. Doğru VMDK bulunmazsa, ek bir disk oluşturur ve açın, yazılan veri.

## <a name="alternate-location-recovery-alr"></a>Alternatif konuma Kurtarma (alr özelliğini)
Senaryo, şirket içi sanal makinenin sanal makine yeniden korunuyor önce mevcut değilse alternatif konuma kurtarma adı verilir. Yeniden koruma iş akışı, şirket içi sanal makine yeniden oluşturur. Bu ayrıca tam veri indirme neden olur.

* Alternatif bir konuma başarısız olursa, sanal makinenin dağıtıldığı ana hedef sunucusunda aynı ESX ana bilgisayarına kurtarılır. Disk oluşturmak için kullanılan veri deposu, sanal makine yeniden korunuyor, seçilen veri deposu olacaktır.
* Başarısız olabilir geriye yalnızca bir sanal makine dosya sistemi (VMFS) veya vSAN veri deposu. Bir RDM varsa, yeniden koruma ve yeniden çalışma çalışmaz.
* Yeniden koruma tarafından değişiklikleri izleyen bir büyük ilk veri aktarımını kapsar. Şirket içi sanal makine var olmadığından bu işlem var. Verilerin tamamını geri çoğaltılması gerekir. Bu yeniden koruma, ayrıca bir özgün konuma kurtarma daha uzun sürer.
* Geri RDM tabanlı disklere çalışma özelliğini kullanamazsınız. Yalnızca bir VMFS/vSAN veri deposu yeni sanal makine disklerinin (Vmdk) oluşturulabilir.

> [!NOTE]
> Fiziksel makine, Azure'a devredilen başarısız yalnızca bir VMware sanal makinesi yedekleyin. Bu, aynı iş akışının alternatif konuma kurtarma olarak izler. En az bir ana hedef sunucusu ve yeniden çalışma gereken gerekli ESX/ESXi konaklarının Bul emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Adımları gerçekleştirmek için [yeniden çalışma işlemi](vmware-azure-failback.md).

