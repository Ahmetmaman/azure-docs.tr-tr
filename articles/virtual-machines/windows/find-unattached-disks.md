---
title: Bulup silmesine eklenmemiş Azure yönetilen ve yönetilmeyen disk | Microsoft Docs
description: Nasıl bulun ve Azure PowerShell kullanarak eklenmemiş Azure yönetilen ve yönetilmeyen (VHD/sayfa BLOB'ları) diskleri silin.
services: virtual-machines-windows
documentationcenter: ''
author: ramankumarlive
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: ramankum
ms.subservice: disks
ms.openlocfilehash: cc8813b0ac90ded1c777f9b1200f4e26737168b9
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55459709"
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks"></a>Bulma ve eklenmemiş Azure yönetilen ve yönetilmeyen disk silme
Bir sanal makine (VM), azure'daki varsayılan olarak, sildiğinizde VM'ye bağlı tüm diskleri silinmez. Bu özellik VM'ler yanlışlıkla silinmesini nedeniyle veri kaybını önlemeye yardımcı olur. VM silindikten sonra eklenmemiş disk için ödeme yapmaya devam edersiniz. Bu makalede bulup eklenmemiş tüm diskleri silin ve gereksiz maliyetleri azaltın gösterilmektedir. 


## <a name="managed-disks-find-and-delete-unattached-disks"></a>Yönetilen diskler: Bulma ve eklenmemiş disk silme 

Aşağıdaki komut dosyasını arar eklenmemiş [yönetilen diskler](managed-disks-overview.md) değerini inceleme tarafından **ManagedBy** özelliği. Bir VM'ye yönetilen disk eklendiğinde **ManagedBy** özelliği, VM kaynak Kimliğini içerir. Yönetilen disk eklenmemiş, olduğunda **ManagedBy** özelliği null. Betik bir Azure aboneliğindeki tüm yönetilen diskler inceler. Betik bir yönetilen disk ile ne zaman bulur **ManagedBy** özelliği null, betik disk eklenmemiş belirler.

>[!IMPORTANT]
>İlk olarak ayarlayarak betiği çalıştırmak **deleteUnattachedDisks** 0 değişken. Bu eylem, bulma ve görüntüleme eklenmemiş tüm yönetilen diskler sağlar.
>
>Tüm eklenmemiş disk gözden geçirdikten sonra betiğini yeniden çalıştırın ve ayarlama **deleteUnattachedDisks** 1 değişken. Bu eylem eklenmemiş tüm yönetilen diskler silmenize olanak sağlar.
>

```azurepowershell-interactive

# Set deleteUnattachedDisks=1 if you want to delete unattached Managed Disks
# Set deleteUnattachedDisks=0 if you want to see the Id of the unattached Managed Disks
$deleteUnattachedDisks=0

$managedDisks = Get-AzureRmDisk

foreach ($md in $managedDisks) {
    
    # ManagedBy property stores the Id of the VM to which Managed Disk is attached to
    # If ManagedBy property is $null then it means that the Managed Disk is not attached to a VM
    if($md.ManagedBy -eq $null){

        if($deleteUnattachedDisks -eq 1){
            
            Write-Host "Deleting unattached Managed Disk with Id: $($md.Id)"

            $md | Remove-AzureRmDisk -Force

            Write-Host "Deleted unattached Managed Disk with Id: $($md.Id) "

        }else{

            $md.Id

        }
           
    }
     
 } 
```

## <a name="unmanaged-disks-find-and-delete-unattached-disks"></a>Yönetilmeyen diskler: Bulma ve eklenmemiş disk silme 

Yönetilmeyen diskler olarak depolanmış VHD dosyaları, [sayfa blobları](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) içinde [Azure depolama hesapları](../../storage/common/storage-create-storage-account.md). Aşağıdaki komut dosyası değerini inceleyerek eklenmemiş yönetilmeyen diskler (sayfa blobları) arar **LeaseStatus** özelliği. Yönetilmeyen disk bir sanal makineye bağlı olduğu **LeaseStatus** özelliği **kilitli**. Yönetilmeyen disk eklenmemiş, olduğunda **LeaseStatus** özelliği **kilitli değil**. Betik bir Azure aboneliğindeki tüm Azure depolama hesaplarındaki yönetilmeyen tüm diskler inceler. Betik bulur yönetilmeyen disk ile ne zaman bir **LeaseStatus** özelliğini **kilitli değil**, komut dosyası disk eklenmemiş olduğunu belirler.

>[!IMPORTANT]
>İlk olarak ayarlayarak betiği çalıştırmak **deleteUnattachedVHDs** 0 değişken. Bu eylem, bulma ve görüntüleme tüm eklenmemiş yönetilmeyen VHD'ler sağlar.
>
>Tüm eklenmemiş disk gözden geçirdikten sonra betiğini yeniden çalıştırın ve ayarlama **deleteUnattachedVHDs** 1 değişken. Bu eylem tüm eklenmemiş yönetilmeyen VHD'ler silmenize olanak sağlar.
>

```azurepowershell-interactive
   
# Set deleteUnattachedVHDs=1 if you want to delete unattached VHDs
# Set deleteUnattachedVHDs=0 if you want to see the Uri of the unattached VHDs
$deleteUnattachedVHDs=0

$storageAccounts = Get-AzureRmStorageAccount

foreach($storageAccount in $storageAccounts){

    $storageKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccount.ResourceGroupName -Name $storageAccount.StorageAccountName)[0].Value

    $context = New-AzureStorageContext -StorageAccountName $storageAccount.StorageAccountName -StorageAccountKey $storageKey

    $containers = Get-AzureStorageContainer -Context $context

    foreach($container in $containers){

        $blobs = Get-AzureStorageBlob -Container $container.Name -Context $context

        #Fetch all the Page blobs with extension .vhd as only Page blobs can be attached as disk to Azure VMs
        $blobs | Where-Object {$_.BlobType -eq 'PageBlob' -and $_.Name.EndsWith('.vhd')} | ForEach-Object { 
        
            #If a Page blob is not attached as disk then LeaseStatus will be unlocked
            if($_.ICloudBlob.Properties.LeaseStatus -eq 'Unlocked'){
              
                  if($deleteUnattachedVHDs -eq 1){

                        Write-Host "Deleting unattached VHD with Uri: $($_.ICloudBlob.Uri.AbsoluteUri)"

                        $_ | Remove-AzureStorageBlob -Force

                        Write-Host "Deleted unattached VHD with Uri: $($_.ICloudBlob.Uri.AbsoluteUri)"
                  }
                  else{

                        $_.ICloudBlob.Uri.AbsoluteUri

                  }

            }
        
        }

    }

}
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [depolama hesabını Sil](../../storage/common/storage-create-storage-account.md) ve [tanımlamak yalnız bırakılmış diskler PowerShell kullanılarak](https://blogs.technet.microsoft.com/ukplatforms/2018/02/21/azure-cost-optimisation-series-identify-orphaned-disks-using-powershell/)



