---
title: Azure'a VMware çoğaltma için hedef ortamı hazırlama | Microsoft Docs
description: Bu makalede Azure ortamı azure'a VMware VM çoğaltma için hedef hazırlamayı öğrenin.
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/03/2019
ms.author: mayg
ms.openlocfilehash: 238e7a26be67fcfd2a0b79a87409e5c0d57e0cbf
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57338374"
---
# <a name="prepare-the-target-environment-for-disaster-recovery-of-vmware-vms-or-physical-servers-to-azure"></a>Fiziksel sunucuları azure'a VMware vm'lerinin olağanüstü durum kurtarma için hedef ortamı hazırlama

Bu makalede, hedef VMware sanal makineleri veya fiziksel sunucuları Azure'a çoğaltmaya başlamak için Azure ortamını hazırlama açıklar.

## <a name="prerequisites"></a>Önkoşullar

Varsayılır:
- Kurtarma Hizmetleri kasası üzerinde oluşturduğunuz [Azure portalında](http://portal.azure.com "Azure portalında") kaynak makinelerinizi korumak için
- Kaynak çoğaltmak için şirket içi ortamınızı kurulumun [VMware sanal makinelerini](vmware-azure-set-up-source.md) veya [fiziksel sunucuları](physical-azure-set-up-source.md) azure'a.

## <a name="prepare-target"></a>Hedefi hazırla

Tamamladıktan sonra **1. adım: Koruma hedefi seçme** ve **2. adım: Kaynak hazırlama**, yönlendirilirsiniz **3. adım: Hedef**

![Hedefi hazırla](./media/vmware-azure-set-up-target/prepare-target-vmware-to-azure.png)

1. **Abonelik:** Aşağı açılan menüden sanal makineleri veya fiziksel sunucuları çoğaltmak istediğiniz aboneliği seçin.
2. **Dağıtım modeli:** Dağıtım modelini (Klasik veya Resource Manager) seçin

Seçilen dağıtım modeline bağlı olarak, bir doğrulama, en az bir sanal ağ çoğaltılabilir ve yük devretme için hedef Abonelikteki sanal makine ya da fiziksel sunucuya sahip olmasını sağlamak için çalışır.

Doğrulama başarıyla tamamladıktan sonra sonraki adıma geçmek için Tamam'a tıklayın.

Bir sanal ağınız yoksa, bir tıklayarak oluşturabilirsiniz **+ ağ** sayfanın üstünde düğme.

## <a name="next-steps"></a>Sonraki adımlar
[Çoğaltma ayarlarını yapılandırma](vmware-azure-set-up-replication.md).
