---
title: Yönetilen diskleri bir depolama hesabına kopyalama-CLı
description: Azure CLı örneği-yönetilen diskleri bir depolama hesabına aktarın veya kopyalayın.
documentationcenter: storage
author: ramankumarlive
manager: kavithag
ms.service: virtual-machines
ms.subservice: disks
ms.topic: sample
ms.workload: infrastructure
ms.date: 05/09/2019
ms.author: ramankum
ms.custom: mvc,seodec18, devx-track-azurecli
ms.openlocfilehash: c43a18f1dcb4122eb6c1407ca11b7c60653594c4
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89323223"
---
# <a name="exportcopy-a-managed-disk-to-a-storage-account-using-the-azure-cli"></a>Azure CLı kullanarak yönetilen bir diski depolama hesabına aktarma/kopyalama

Bu betik yönetilen diskin VHD dosyasını aynı veya farklı bölgede bulunan bir depolama hesabına aktarır. İlk olarak yönetilen diskin SAS URI'sini oluşturur ve sonra onu VHD dosyasını bir depolama hesabına kopyalamak için kullanır. Bu betiği bölgesel genişleme için yönetilen diskleri farklı bölgelere kopyalama amacıyla kullanabilirsiniz. Azure Marketi 'nde yönetilen bir diskin VHD dosyasını yayınlamak istiyorsanız, bu betiği kullanarak VHD dosyasını bir depolama hesabına kopyalayabilir ve ardından Market 'te yayımlamak için kopyalanmış VHD 'nin SAS URI 'sini oluşturabilirsiniz.   


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-managed-disks-vhd-to-storage-account/copy-managed-disks-vhd-to-storage-account.sh "Copy the VHD of a managed disk")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir yönetilen diskin SAS URI'sini oluşturmak için aşağıdaki komutları kullanır ve SAS URI'sini kullanarak VHD dosyasını bir depolama hesabına kopyalar. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az disk grant-access](/cli/azure/disk?view=azure-cli-latest#az-disk-grant-access) | Temel alınan VHD dosyasını bir depolama hesabına kopyalamak veya şirket içine indirmek üzere kullanılan salt okunur SAS oluşturur  |
| [az storage blob copy start](/cli/azure/storage/blob/copy) | Bir blobu bir depolama hesabından diğerine zaman uyumsuz olarak kopyalar |

## <a name="next-steps"></a>Sonraki adımlar

[VHD'den yönetilen disk oluşturma](virtual-machines-cli-sample-create-managed-disk-from-vhd.md)

[Yönetilen diskten sanal makine oluşturma](virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md)

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek sanal makine ve yönetilen disk CLI betiği örnekleri, [Azure Linux VM belgeleri](../linux/cli-samples.md) içinde bulunabilir.
