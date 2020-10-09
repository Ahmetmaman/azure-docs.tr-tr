---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 04/02/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: de0cf260207747f4acb02a377819a13de8b8ba22
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "80628409"
---
[!INCLUDE [virtual-machines-disks-incremental-snapshots-description](virtual-machines-disks-incremental-snapshots-description.md)]

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik
[!INCLUDE [virtual-machines-disks-incremental-snapshots-regions](virtual-machines-disks-incremental-snapshots-regions.md)]

### <a name="restrictions"></a>Kısıtlamalar

[!INCLUDE [virtual-machines-disks-incremental-snapshots-restrictions](virtual-machines-disks-incremental-snapshots-restrictions.md)]

## <a name="portal"></a>Portal


1. [Azure Portal](https://portal.azure.com/) oturum açın ve anlık görüntü eklemek istediğiniz diske gidin.
1. Diskinizde **anlık görüntü oluştur** ' u seçin.

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/create-snapshot-button-incremental.png" alt-text="Yakala. Seçtiğiniz gibi, * * + anlık görüntü oluştur * * vurgulanmış olarak diskinizin dikey pencerenizin olması gerekir.":::

1. Kullanmak istediğiniz kaynak grubunu seçin ve bir ad girin.
1. **Artımlı** ' i seçin ve **gözden geçir + oluştur** seçeneğini belirleyin

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/incremental-snapshot-create-snapshot-blade.png" alt-text="Yakala. Seçtiğiniz gibi, * * + anlık görüntü oluştur * * vurgulanmış olarak diskinizin dikey pencerenizin olması gerekir.":::

1. **Oluştur**’u seçin

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/create-incremental-snapshot-validation.png" alt-text="Yakala. Seçtiğiniz gibi, * * + anlık görüntü oluştur * * vurgulanmış olarak diskinizin dikey pencerenizin olması gerekir.":::

## <a name="next-steps"></a>Sonraki adımlar

.NET kullanarak Artımlı anlık görüntülerin fark yeteneğini gösteren örnek kodu görmek isterseniz, bkz. [Azure yönetilen diskler yedeklemelerini, artımlı anlık görüntülerin fark yeteneği ile başka bir bölgeye kopyalama](https://github.com/Azure-Samples/managed-disks-dotnet-backup-with-incremental-snapshots).
