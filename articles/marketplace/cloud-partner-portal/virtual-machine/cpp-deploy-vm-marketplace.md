---
title: Azure Market'ten bir VM dağıtma | Microsoft Docs
description: Azure Market'te önceden yapılandırılmış sanal makineden bir sanal makine dağıtma işlemi açıklanmaktadır.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 11/29/2018
ms.author: pbutlerm
ms.openlocfilehash: 45baa709e715cb94c8c9c6ac7548b89813c8194b
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53197325"
---
# <a name="deploy-a-virtual-machine-from-the-azure-marketplace"></a>Azure Market'ten sanal makine dağıtma

Bu makalede, Azure PowerShell betiğini kullanarak bir Azure Market'teki önceden yapılandırılmış bir sanal makine (VM) dağıtmak açıklanmaktadır.  Bu betik, VM üzerindeki WinRM HTTP ve HTTPS uç noktaları da kullanıma sunar.  Azure anahtar Kasası'na yüklenen sertifika açıklandığı gibi sahip olduğunuz komut dosyası gerektirir [sertifikaları oluşturmak için Azure anahtar kasası](./cpp-create-key-vault-cert.md). 


## <a name="vm-deployment-template"></a>VM Dağıtım Şablonu

Hızlı Başlangıç Azure VM Dağıtım Şablonu çevrimiçi dosya kullanılabilir [azuredeploy.json](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-vm-winrm-keyvault-windows/azuredeploy.json).  Aşağıdaki parametreleri içerir:

|  **Parametre**        |   **Açıklama**                                 |
|  -------------        |   ---------------                                 |
| newStorageAccountName | Depolama hesabının adı                       |
| dnsNameForPublicIP    | Genel IP için DNS adı. Küçük harf olması gerekir.    |
| adminUserName         | Yönetici kullanıcı adı                          |
| adminPassword         | Yönetici parolası                          |
| imagePublisher        | Görüntü yayımcısı                                   |
| imageOffer            | Görüntü teklifi                                       |
| imageSKU              | Görüntü SKU'su                                         |
| vmSize                | VM boyutu                                    |
| vmName                | VM adı                                    |
| VaultName             | Anahtar kasasının adı                             |
| vaultResourceGroup    | Anahtar kasasının kaynak grubu                   |
| certificateUrl        | KeyVault örneğin sürümü dahil bir sertifika URL'si  https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7 |
|  |  |


## <a name="deployment-script"></a>Dağıtım betiği

Aşağıdaki Azure PowerShell Betiği düzenleyin ve belirtilen Azure Market sanal Makineyi dağıtmak için yürütün.

```powershell

New-AzureRmResourceGroupDeployment -Name "dplvm$postfix" -ResourceGroupName "$rgName" -TemplateUri "https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-vm-winrm-keyvault-windows/azuredeploy.json" -newStorageAccountName "test$postfix" -dnsNameForPublicIP $vmName -adminUserName "isv" -adminPassword $pwd -vmSize "Standard_A2" -vmName $vmName -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id 

```


## <a name="next-steps"></a>Sonraki adımlar

Önceden yapılandırılmış bir VM dağıttıktan sonra yapılandırma ve çözümleri ve Hizmetleri içerdiği erişmek veya daha fazla geliştirme için kullanın. 
