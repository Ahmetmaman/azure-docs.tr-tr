---
title: Azure PowerShell Betiği Örneği - Windows Sanal Makinesi şifreleme | Microsoft Docs
description: Azure PowerShell Betiği Örneği - Windows Sanal Makinesi şifreleme
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/12/2017
ms.author: cynthn
ms.openlocfilehash: 3a3bfe80f251c6a11e19f33adb915d20b9819784
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56116040"
---
# <a name="encrypt-a-windows-virtual-machine-with-azure-powershell"></a>Azure PowerShell ile Windows sanal makinesi şifreleme

Bu betik güvenli bir Azure Key Vault, şifreleme anahtarları, Azure Active Directory hizmet sorumlusu ve bir Windows sanal makinesi (VM) oluşturur. VM daha sonra Key Vault şifreleme anahtarı ve hizmet sorumlusu kimlik bilgileri kullanılarak şifrelenir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine/encrypt-vm/encrypt-windows-vm.ps1 "Encrypt VM disks")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzKeyVault](https://docs.microsoft.com/powershell/module/az.keyvault/new-azkeyvault) | Şifreleme anahtarları gibi güvenli verileri depolamak için bir Azure Key Vault oluşturur. |
| [Add-AzureKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/add-azkeyvaultkey) | Key Vault içinde bir şifreleme anahtarı oluşturur. |
| [Yeni AzADServicePrincipal](https://docs.microsoft.com/powershell/module/az.resources/new-azadserviceprincipal) | Güvenli bir şekilde kimlik doğrulamak ve şifreleme anahtarlarına erişimi denetlemek üzere bir Azure Active Directory hizmet sorumlusu oluşturur. |
| [Set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) | Key Vault üzerinde hizmet sorumlusuna şifreleme anahtarları için erişim verecek izinleri ayarlar. |
| [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) | Sanal makine oluşturur ve ağ kartına, sanal ağa, alt ağa ve ağ güvenlik grubuna bağlar. Bu komut ayrıca 80 numaralı bağlantı noktasını açar ve yönetici kimlik bilgilerini ayarlar. |
| [Get-AzKeyVault](https://docs.microsoft.com/powershell/module/az.keyvault/get-azkeyvault) | Key Vault ile ilgili gerekli bilgileri alır |
| [Set-AzVMDiskEncryptionExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmdiskencryptionextension) | Hizmet sorumlusu kimlik bilgilerini ve şifreleme anahtarını kullanarak VM üzerinde şifrelemeyi etkinleştirir. |
| [Get-AzVmDiskEncryptionStatus](https://docs.microsoft.com/powershell/module/az.compute/get-azvmdiskencryptionstatus) | VM şifreleme işleminin durumunu gösterir. |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal makine PowerShell betiği örnekleri, [Azure Windows VM belgeleri](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) içinde bulunabilir.
