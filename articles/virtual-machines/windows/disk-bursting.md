---
title: Yönetilen disk patlaması
description: Azure diskleri için disk alma ve Azure sanal makineleri için disk alma hakkında bilgi edinin
author: albecker1
ms.author: albecker
ms.date: 09/22/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 547834542b605b226ebffd68e05296ee847dc6de
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91974337"
---
# <a name="disk-bursting"></a>Disk genişletme
[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting.md)]

## <a name="virtual-machine-level-bursting"></a>Sanal makine düzeyinde patlama
VM düzeyinde burdıya desteği, bu desteklenen boyutlarda genel buluttaki tüm bölgelerde etkin: 
- [Lsv2 serisi](../lsv2-series.md)

VM düzeyi patlaması, aşağıdaki desteklenen boyutlarda de Orta Batı ABD de kullanılabilir:
- [Dsv3 serisi](../dv3-dsv3-series.md)
- [Esv3 serisi](../ev3-esv3-series.md)

Bu işlemi destekleyen sanal makineler için burdıya varsayılan olarak etkindir.

## <a name="disk-level-bursting"></a>Disk düzeyinde patlama
Ayrıca, tüm bölgelerde P20 ve daha küçük bir disk boyutu için [Premium SSD](../disks-types.md#premium-ssd) 'larımız de mevcuttur. Disk patlaması, bunu destekleyen disk boyutlarının yeni dağıtımları üzerinde varsayılan olarak etkindir. Disk kullanımını destekliyorsa, mevcut disk boyutları aşağıdaki yöntemlerden birini kullanarak ani bir şekilde etkinleştirebilir: 
- **VM 'yi yeniden başlatma** 
- **Diski kullanımdan çıkarın ve yeniden bağlayın**


[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting-2.md)]