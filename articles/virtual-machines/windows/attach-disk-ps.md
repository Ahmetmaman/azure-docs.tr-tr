---
title: PowerShell kullanarak azure'da bir Windows VM'ye veri diski ekleme | Microsoft Docs
description: Windows PowerShell ile Resource Manager dağıtım modelini kullanarak bir VM için yeni veya var olan veri diski ekleme yapma.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: cynthn
ms.subservice: disks
ms.openlocfilehash: 791322c71b4d1b49e1367fb0f179e7b0513a1e94
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2019
ms.locfileid: "55975662"
---
# <a name="attach-a-data-disk-to-a-windows-vm-with-powershell"></a>PowerShell ile Windows VM'ye veri diski ekleme

Bu makalede, PowerShell kullanarak bir Windows sanal makine için yeni ve var olan diskleri ekleme işlemini göstermektedir. 

İlk olarak, bu ipuçlarını gözden geçirin:
* Sanal makinenin boyutunu, iliştirebilirsiniz kaç veri diskinin denetler. Daha fazla bilgi için [sanal makine boyutları](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Premium depolama kullanmak için DS serisi veya GS serisi sanal makine gibi bir Premium depolama kullanan sanal makine türü gerekir. Daha fazla bilgi için [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama](premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]


## <a name="add-an-empty-data-disk-to-a-virtual-machine"></a>Boş veri diski bir sanal makineye ekleyin.

Bu örnek, varolan bir sanal makineye boş veri diski ekleme gösterir.

### <a name="using-managed-disks"></a>Yönetilen diskleri kullanma

```azurepowershell-interactive
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US' 
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128
$dataDisk1 = New-AzDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzVM -VM $vm -ResourceGroupName $rgName
```

### <a name="using-managed-disks-in-an-availability-zone"></a>Bir kullanılabilirlik alanında yönetilen diskleri kullanma
Bir kullanılabilirlik alanında bir disk oluşturmak için kullanın [yeni AzDiskConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azdiskconfig) ile `-Zone` parametresi. Aşağıdaki örnek, bölgesinde bir disk oluşturur *1*.


```powershell
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US 2' 
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128 -Zone 1
$dataDisk1 = New-AzDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzVM -VM $vm -ResourceGroupName $rgName
```


### <a name="initialize-the-disk"></a>Diski başlatın

Boş disk ekledikten sonra bunu başlatmak gerekir. Diski başlatmak için bir VM için oturum açın ve disk Yönetimi'ni kullanın. Etkinleştirilirse, [WinRM](https://docs.microsoft.com/windows/desktop/WinRM/portal) ve bir sertifika oluşturduğunuzda sanal makine diski başlatmak için Uzak PowerShell kullanabilirsiniz. Özel betik uzantısı da kullanabilirsiniz: 

```azurepowershell-interactive
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```
        
Örneğin diskleri başlatmak için kod komut dosyasını içerebilir:

```azurepowershell-interactive
    $disks = Get-Disk | Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { [char]$_ }
    $count = 0
    $labels = "data1","data2"

    foreach ($disk in $disks) {
        $driveLetter = $letters[$count].ToString()
        $disk | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] -Confirm:$false -Force
    $count++
    }
```


## <a name="attach-an-existing-data-disk-to-a-vm"></a>Bir VM'ye olan bir veri diski ekleme

Bir VM'ye veri diski olarak mevcut bir yönetilen disk ekleyebilirsiniz. 

```azurepowershell-interactive
$rgName = "myResourceGroup"
$vmName = "myVM"
$location = "East US" 
$dataDiskName = "myDisk"
$disk = Get-AzDisk -ResourceGroupName $rgName -DiskName $dataDiskName 

$vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzVMDataDisk -CreateOption Attach -Lun 0 -VM $vm -ManagedDiskId $disk.Id

Update-AzVM -VM $vm -ResourceGroupName $rgName
```

## <a name="next-steps"></a>Sonraki adımlar

Oluşturma bir [anlık görüntü](snapshot-copy-managed-disk.md).
