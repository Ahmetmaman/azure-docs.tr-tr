---
title: Yönetilen bir diskin VHD 'sini başka bir bölgenin hesabına (Windows) aktarma/kopyalama-PowerShell
description: Azure PowerShell betik örneği - Yönetilen diskin VHD dosyasını aynı veya farklı bölgedeki bir depolama hesabına aktarma/kopyalama
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/17/2018
ms.author: ramankum
ms.openlocfilehash: a02b55adf4ac1838e9fcb98b9dffcfbd2b4b52d4
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91969944"
---
# <a name="exportcopy-the-vhd-of-a-managed-disk-to-a-storage-account-in-different-region-with-powershell-windows"></a>Yönetilen bir diskin VHD 'sini PowerShell ile farklı bölgedeki bir depolama hesabına dışarı aktarma/kopyalama (Windows)

Bu betik yönetilen diskin VHD dosyasını farklı bölgede bulunan bir depolama hesabına aktarır. İlk olarak yönetilen diskin SAS URI'sini oluşturur ve sonra onu VHD dosyasını farklı bölgedeki bir depolama hesabına kopyalamak için kullanır. Bu betiği bölgesel genişleme için yönetilen diskleri farklı bölgelere kopyalama amacıyla kullanabilirsiniz.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-managed-disks-vhd-to-storage-account/copy-managed-disks-vhd-to-storage-account.ps1 "Copy the VHD of a managed disk")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir yönetilen diskin SAS URI'sini oluşturmak için aşağıdaki komutları kullanır ve SAS URI'sini kullanarak VHD dosyasını bir depolama hesabına kopyalar. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Grant-AzDiskAccess](/powershell/module/az.compute/grant-azdiskaccess) | VHD dosyasını bir depolama hesabına kopyalamak için kullanılan yönetilen disk SAS URI değerini oluşturur. |
| [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext) | Hesap adını ve anahtarını kullanarak depolama hesabı bağlamı oluşturur. Bu bağlam depolama hesabında okuma/yazma işlemi gerçekleştirmek için kullanılabilir. |
| [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/start-azurestorageblobcopy) | Bir anlık görüntüde kullanılan VHD dosyasını bir depolama hesabına kopyalar |

## <a name="next-steps"></a>Sonraki adımlar

[VHD'den yönetilen disk oluşturma](./virtual-machines-powershell-sample-create-managed-disk-from-vhd.md?toc=%252fpowershell%252fmodule%252ftoc.json)

[Yönetilen diskten sanal makine oluşturma](./virtual-machines-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/).

Ek sanal makine PowerShell betiği örnekleri, [Azure Windows VM belgeleri](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) içinde bulunabilir.