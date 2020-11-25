---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 11/28/2019
ms.author: raynew
ms.openlocfilehash: de15e3028cf22cdd03ce29385278fc5e2babaa9b
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96008505"
---
Bu makalede aşağıdaki durumlar varsayılır

1. Şirket içi ağınız ile Azure Sanal Ağ arasında **Siteden Siteye VPN** veya **Express Route** bağlantısı zaten oluşturulmuştur.
2. Kullanıcı hesabınız sanal makinelerin yük devrettiği Azure Aboneliğinde yeni bir sanal makine oluşturma izinlerine sahiptir.
3. Aboneliğiniz, yeni bir Işlem sunucusu sanal makinesi çalıştırmak için en az 8 çekirdek kullanılabilir.
4. **Configuration Server Parolanız** mevcuttur.

> [!TIP]
> Sanal makinelerin yük devrettiği Azure Sanal Ağından Configuration Server (şirket içinde çalışan) 443 bağlantı noktasını bağlayabildiğinizden emin olun.
