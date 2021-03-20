---
title: Azure Site Recovery kullanarak müşteri tarafından yönetilen siteler arasında (VMM ile) olağanüstü durum kurtarmayı kullanımdan kaldırma | Microsoft Docs
description: Hyper-V ' d i kullanarak müşterilerin sahip olduğu siteler arasında ve SCVMM ile Azure arasında yönetilen siteler arasında ve diğer seçenekler arasında DR 'nin kullanımdan kalkmasıyla ilgili ayrıntılar
services: site-recovery
author: Sharmistha-Rai
manager: gaggupta
ms.service: site-recovery
ms.topic: article
ms.date: 02/25/2020
ms.author: sharrai
ms.openlocfilehash: 9ffe7a3158b1de6828350947dcf81ef41d08708d
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "87421850"
---
# <a name="deprecation-of-disaster-recovery-between-customer-managed-sites-with-vmm-using-azure-site-recovery"></a>Azure Site Recovery kullanarak müşteri tarafından yönetilen siteler arasında (VMM ile) olağanüstü durum kurtarmayı kullanımdan kaldırma

Bu makalede gelecek kullanım dışı bırakma planı, ilgili etkileri ve müşteriler için aşağıdaki senaryo için kullanılabilecek alternatif seçenekler açıklanmaktadır:

Site Recovery kullanarak System Center Virtual Machine Manager (SCVMM) tarafından yönetilen müşterilerin sahip olduğu siteler arasında DR

> [!IMPORTANT]
> Müşterilerin ortamlarına herhangi bir kesinti yaşamamak için en erken düzeltme adımlarını gerçekleştirmeniz önerilir. 

## <a name="what-changes-should-you-expect"></a>Hangi değişiklikleri beklemeniz gerekir?

- 2020 'den itibaren, e-posta iletişimleri &, bu, Hyper-V VM 'lerinin siteden siteye çoğaltılmasına yönelik kullanım dışı bırakılmasıyla ilgili Azure portal bildirimler alacaksınız. Kullanımdan kaldırma, 2023 Mart için planlanmaktadır.

- Mevcut bir yapılandırmaya sahipseniz, kurulum üzerinde hiçbir etkisi olmaz.

- Müşteri diğer yaklaşımları izliyorsa senaryolar kullanım dışı olduktan sonra var olan çoğaltmalar bozulabilir. Müşteriler, Azure portal Azure Site Recovery deneyimi aracılığıyla DR ile ilgili işlemleri görüntüleyemez, yönetemez veya gerçekleştiremez.
 
## <a name="alternatives"></a>Alternatifler 

Bu, senaryonun kullanım dışı olduktan sonra DR stratejilerinin etkilenmemesini sağlamak için müşterinin aralarından seçim yapabileceğiniz alternatiflerdir. 

- Seçenek 1 (önerilir): Azure 'U [Dr hedefi olarak kullanmaya başlamak](hyper-v-vmm-azure-tutorial.md)için seçin.


- 2. seçenek: temel [Hyper-V çoğaltma çözümünü](/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica)kullanarak siteden siteye çoğaltmaya devam etmeyi seçin, ancak Azure Portal Azure SITE Recovery kullanarak Dr yapılandırmalarının yönetimi için işlem yapmanız gerekmez. 


## <a name="remediation-steps"></a>Düzeltme adımları

1. seçenek ile devam etmek istiyorsanız lütfen aşağıdaki adımları yürütün:

1. [VMMs ile ilişkili tüm sanal makinelerin korumasını devre dışı bırakın](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-replicating-to-secondary-vmm-server-using-the-system-center-vmm-to-vmm-scenario). Şirket içi çoğaltma ayarlarının temizlendiğinden emin olmak için **çoğaltmayı devre dışı bırak ve Kaldır** seçeneğini kullanın veya belirtilen betikleri çalıştırın. 

2. [Tüm VMM sunucularının](site-recovery-manage-registration-and-protection.md#unregister-a-vmm-server) siteden siteye çoğaltma yapılandırmasından kaydını kaldırın.

3. Sanal makinelerinizin çoğaltılmasını etkinleştirmek için [Azure kaynaklarını hazırlayın](tutorial-prepare-azure-for-hyperv.md) .
4. [Şirket içi Hyper-V sunucuları hazırlayın](hyper-v-prepare-on-premises-tutorial.md)
5. [VMM bulutundaki VM 'Ler için çoğaltmayı ayarlama](hyper-v-vmm-azure-tutorial.md)
6. İsteğe bağlı ancak önerilir: [Dr detayına git](tutorial-dr-drill-azure.md)

Hyper-V çoğaltmasını kullanma seçeneği 2 ' yi seçerek, aşağıdaki adımları yürütün:

1. **Korunan öğeler**  >  **çoğaltılan öğeler** bölümünde, **çoğaltmayı devre dışı bırakmak**> makineye sağ tıklayın.
2. **Çoğaltmayı devre dışı bırak**' da **Kaldır**' ı seçin.

    Bu, çoğaltılan öğeyi Azure Site Recovery kaldırır (Faturalandırma durdurulur). Şirket içi **sanal makinede çoğaltma yapılandırması temizlenmeyecektir.** 

## <a name="next-steps"></a>Sonraki adımlar
Kullanımdan kaldırma planlayın ve altyapınız ve işletmeniz için en uygun alternatif bir seçenek belirleyin. Bu ile ilgili herhangi bir sorgunuz varsa Microsoft Desteği ulaşın

