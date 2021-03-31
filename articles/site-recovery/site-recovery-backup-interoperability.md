---
title: Azure Backup ile Azure Site Recovery kullanma desteği
description: Azure Site Recovery ve Azure Backup birlikte nasıl kullanılabileceğine ilişkin bir genel bakış sağlar.
author: sideeksh
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: sideeksh
ms.openlocfilehash: c334eee34eb878135d3d81ab15d03618c6604846
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "86135179"
---
# <a name="support-for-using-site-recovery-with-azure-backup"></a>Azure Backup ile Site Recovery kullanma desteği

Bu makalede, [Azure Backup hizmetiyle](../backup/backup-overview.md)birlikte [Site Recovery hizmetini](site-recovery-overview.md) kullanma desteği özetlenmektedir.

**Eylem** | **Site Recovery desteği** | **Ayrıntılar**
--- | --- | ---
**Hizmetleri birlikte dağıtma** | Desteklenir | Hizmetler birlikte çalışabilir ve birlikte yapılandırılabilir.
**Dosya yedekleme/geri yükleme** | Desteklenir | Yedekleme ve çoğaltma bir VM için etkinleştirildiğinde ve yedeklemeler çekilirken, kaynak tarafı VM 'lerde veya VM grupları üzerinde dosya geri yükleme konusu yoktur. Çoğaltma, çoğaltma durumunda hiçbir değişiklik yapmadan her zamanki gibi devam eder.
**Disk geri yükleme** | Geçerli destek yok | Yedeklenen bir diski geri yüklerseniz, VM için çoğaltmayı yeniden devre dışı bırakıp yeniden etkinleştirmeniz gerekir.
**VM geri yükleme** | Geçerli destek yok | Bir VM 'yi veya VM grubunu geri yüklerseniz, VM için çoğaltmayı devre dışı bırakıp yeniden etkinleştirmeniz gerekir.  


