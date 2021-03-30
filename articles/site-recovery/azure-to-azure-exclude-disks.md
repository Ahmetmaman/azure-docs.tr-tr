---
title: Azure Site Recovery ve Azure PowerShell ile Azure VM disklerini çoğaltmadan dışlama
description: Azure PowerShell kullanarak Azure Site Recovery sırasında Azure sanal makinelerinin disklerini dışarıda bırakmayı öğrenin.
author: sideeksh
manager: rochakm
ms.topic: how-to
ms.date: 02/18/2019
ms.openlocfilehash: a21460279420c46b11c43615ae5ecc7bfa81de4d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "86135815"
---
# <a name="exclude-disks-from-powershell-replication-of-azure-vms"></a>Azure VM 'lerinin PowerShell çoğaltmasıyla diskleri dışlama

Bu makalede, Azure sanal makinelerini çoğalttığınızda disklerin nasıl hariç tutulacağı açıklanır. Kullanılan çoğaltma bant genişliğini veya bu disklerin kullandığı hedef tarafı kaynakları iyileştirmek için diskleri dışlayabilirsiniz. Şu anda bu özellik yalnızca Azure PowerShell ile kullanılabilir.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce:

- [Olağanüstü durum kurtarma mimarisini ve bileşenlerini](azure-to-azure-architecture.md)anladığınızdan emin olun.
- Tüm bileşenler için [destek gereksinimlerini](azure-to-azure-support-matrix.md) gözden geçirin.
- Azurerd PowerShell "az" modülüne sahip olduğunuzdan emin olun. PowerShell 'i yüklemek veya güncelleştirmek için bkz. [Azure PowerShell modülünü yüklemek](/powershell/azure/install-az-ps).
- Bir kurtarma hizmetleri kasası ve korunan sanal makineleri en az bir kez oluşturmuş olduğunuzdan emin olun. Bu işlemleri yapmadıysanız, [Azure PowerShell kullanarak Azure sanal makineleri için olağanüstü durum kurtarmayı ayarlama](azure-to-azure-powershell.md)sürecini izleyin.
- Çoğaltma için etkin bir Azure VM 'ye disk ekleme hakkında bilgi arıyorsanız, [Bu makaleyi gözden geçirin](azure-to-azure-enable-replication-added-disk.md).

## <a name="why-exclude-disks-from-replication"></a>Disklerin neden çoğaltmanın dışında tutulması
Diskleri çoğaltmanın dışında bırakmanız gerekebilir çünkü:

- Sanal makineniz [veri değişikliği hızlarını çoğaltmak için sınırlara Azure Site Recovery](./azure-to-azure-support-matrix.md)ulaştı.

- Dışlanan diske eklenen veriler önemli değildir veya çoğaltılmaması gerekmez.

- Verileri çoğaltmayana depolama ve ağ kaynaklarını kaydetmek istiyorsunuz.

## <a name="how-to-exclude-disks-from-replication"></a>Diskleri çoğaltmanın dışında bırakma

Örneğimizde, bir işletim sistemi ve Doğu ABD bölgesindeki üç veri diskine sahip bir sanal makineyi Batı ABD 2 bölgesine çoğalttık. Sanal makinenin adı *AzureDemoVM*' dir. Disk 1 ' i dışarıda bırakır ve 2. ve 3. diskleri saklar.

## <a name="get-details-of-the-virtual-machines-to-replicate"></a>Çoğaltılacak sanal makinelerin ayrıntılarını alın

```azurepowershell
# Get details of the virtual machine
$VM = Get-AzVM -ResourceGroupName "A2AdemoRG" -Name "AzureDemoVM"

Write-Output $VM     
```

```
ResourceGroupName  : A2AdemoRG
Id                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/A2AdemoRG/providers/Microsoft.Compute/virtualMachines/AzureDemoVM
VmId               : 1b864902-c7ea-499a-ad0f-65da2930b81b
Name               : AzureDemoVM
Type               : Microsoft.Compute/virtualMachines
Location           : eastus
Tags               : {}
DiagnosticsProfile : {BootDiagnostics}
HardwareProfile    : {VmSize}
NetworkProfile     : {NetworkInterfaces}
OSProfile          : {ComputerName, AdminUsername, WindowsConfiguration, Secrets}
ProvisioningState  : Succeeded
StorageProfile     : {ImageReference, OsDisk, DataDisks}
```

Sanal makinenin diskleri hakkında ayrıntılı bilgi alın. Bu bilgiler daha sonra VM çoğaltmasını başlattığınızda kullanılacaktır.

```azurepowershell
$OSDiskVhdURI = $VM.StorageProfile.OsDisk.Vhd
$DataDisk1VhdURI = $VM.StorageProfile.DataDisks[0].Vhd
```

## <a name="replicate-an-azure-virtual-machine"></a>Azure sanal makinesini çoğaltma

Aşağıdaki örnekte, zaten bir önbellek depolama hesabınız, çoğaltma ilkeniz ve eşlemeler olduğunu varsaytık. Bu şeylere sahip değilseniz, [Azure PowerShell kullanarak Azure sanal makineleri için olağanüstü durum kurtarmayı ayarlama](azure-to-azure-powershell.md)sürecini izleyin.

*Yönetilen disklerle* bir Azure sanal makinesini çoğaltın.

```azurepowershell

#Get the resource group that the virtual machine must be created in when failed over.
$RecoveryRG = Get-AzResourceGroup -Name "a2ademorecoveryrg" -Location "West US 2"

#Specify replication properties for each disk of the VM that is to be replicated (create disk replication configuration).

#OsDisk
$OSdiskId =  $vm.StorageProfile.OsDisk.ManagedDisk.Id
$RecoveryOSDiskAccountType = $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType

$OSDiskReplicationConfig = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $EastUSCacheStorageAccount.Id `
         -DiskId $OSdiskId -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryOSDiskAccountType

# Data Disk 1 i.e StorageProfile.DataDisks[0] is excluded, so we will provide it during the time of replication.

# Data disk 2
$datadiskId2  = $vm.StorageProfile.DataDisks[1].ManagedDisk.id
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.DataDisks[1]. StorageAccountType
$RecoveryTargetDiskAccountType = $vm.StorageProfile.DataDisks[1]. StorageAccountType

$DataDisk2ReplicationConfig  = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $CacheStorageAccount.Id `
         -DiskId $datadiskId2 -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryTargetDiskAccountType

# Data Disk 3

$datadiskId3  = $vm.StorageProfile.DataDisks[2].ManagedDisk.id
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.DataDisks[2]. StorageAccountType
$RecoveryTargetDiskAccountType = $vm.StorageProfile.DataDisks[2]. StorageAccountType

$DataDisk3ReplicationConfig  = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $CacheStorageAccount.Id `
         -DiskId $datadiskId3 -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryTargetDiskAccountType

#Create a list of disk replication configuration objects for the disks of the virtual machine that are to be replicated.
$diskconfigs = @()
$diskconfigs += $OSDiskReplicationConfig, $DataDisk2ReplicationConfig, $DataDisk3ReplicationConfig


#Start replication by creating a replication protected item. Using a GUID for the name of the replication protected item to ensure uniqueness of name.
$TempASRJob = New-ASRReplicationProtectedItem -AzureToAzure -AzureVmId $VM.Id -Name (New-Guid).Guid -ProtectionContainerMapping $EusToWusPCMapping -AzureToAzureDiskReplicationConfiguration $diskconfigs -RecoveryResourceGroupId $RecoveryRG.ResourceId
```

Çoğaltma başlatma işlemi başarılı olduğunda, VM verileri kurtarma bölgesine çoğaltılır.

Azure portal giderek çoğaltılan VM 'Leri "çoğaltılan öğeler" altında görebilirsiniz.

Çoğaltma işlemi, kurtarma bölgesindeki sanal makinenin çoğaltılan disklerinin bir kopyasının dengeli şekilde başlatılır. Bu aşamaya ilk çoğaltma aşaması denir.

İlk çoğaltma tamamlandıktan sonra çoğaltma, fark eşitleme aşamasına gider. Bu noktada, sanal makine korunur. Korunan sanal makineyi seçerek herhangi bir diskin dışlandığından bkz.

## <a name="next-steps"></a>Sonraki adımlar

[Yük devretme testi çalıştırma](site-recovery-test-failover-to-azure.md)hakkında bilgi edinin.
