---
title: Öğretici - Azure’da SSL sertifikalarıyla Windows web sunucusunun güvenliğini sağlama | Microsoft Docs
description: Bu öğreticide, Azure Key Vault’ta depolanan SSL sertifikaları ile IIS web sunucusunda çalışan bir Windows sanal makinesinin güvenliğini sağlamak için Azure PowerShell kullanmayı öğreneceksiniz.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/09/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: b964da7060ab2c793d15981b21aba7190a7b2bde
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2019
ms.locfileid: "55977941"
---
# <a name="tutorial-secure-a-web-server-on-a-windows-virtual-machine-in-azure-with-ssl-certificates-stored-in-key-vault"></a>Öğretici: Key Vault'ta depolanan bir SSL sertifikalarını kullanarak azure'da bir Windows sanal makinesi bir web sunucusunda güvenli hale getirme

Web sunucularının güvenliğini sağlamak için, web trafiğini şifrelemek üzere Güvenli Yuva Katmanı (SSL) sertifikası kullanılabilir. Bu SSL sertifikaları Azure Key Vault’ta depolanabilir ve sertifikaların Azure’daki Windows sanal makinelerine (VM’ler) güvenli bir şekilde dağıtılabilmesini sağlar. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure Key Vault oluşturma
> * Sertifikaları oluşturma veya Key Vault’a yükleme
> * VM oluşturma ve ISS web sunucusunu yükleme
> * Sertifikayı VM’ye ekleme ve ISS’yi bir SSL bağlamasıyla yapılandırma

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. İsterseniz [https://shell.azure.com/powershell](https://shell.azure.com/powershell) adresine giderek Cloud Shell'i ayrı bir tarayıcı sekmesinde de başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.


## <a name="overview"></a>Genel Bakış
Azure Key Vault, sertifikalar veya pasaportlar gibi şifreleme anahtarları ile gizli dizilerin güvenliğini sağlar. Key Vault, sertifika yönetimi işlemini kolaylaştırır ve bu sertifikalara erişen anahtarları denetiminizde tutmanıza olanak sağlar. Key Vault içinde otomatik olarak imzalanan bir sertifika oluşturabilir veya sahip olduğunuz güvenli bir sertifikayı karşıya yükleyebilirsiniz.

Oluşturulan sertifikaları içeren özel bir VM görüntüsü kullanmak yerine, sertifikaları çalışan bir VM’ye eklersiniz. Bu işlem, dağıtım sırasında web sunucusuna en güncel sertifikaların yüklenip yüklenmediğini kontrol eder. Ayrıca, bir sertifikayı yeniler veya değiştirirseniz yeni ve özel bir VM görüntüsü oluşturmanız da gerekmez. En güncel sertifikalar, siz ek VM oluşturdukça otomatik olarak eklenir. Bu işlem sırasında, sertifikalar Azure platformundan ayrılmaz veya bir betikte, komut satırı geçmişinde veya şablonda kullanıma sunulmaz.


## <a name="create-an-azure-key-vault"></a>Azure Key Vault oluşturma
Key Vault ve sertifikalarını oluşturabilmek, bir kaynak grubu oluşturun [yeni AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup). Aşağıdaki örnek *Doğu ABD* konumunda *myResourceGroupSecureWeb* adlı bir kaynak grubu oluşturur:

```azurepowershell-interactive
$resourceGroup = "myResourceGroupSecureWeb"
$location = "East US"
New-AzResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

Sonra, [New-AzureRmKeyVault](https://docs.microsoft.com/powershell/module/az.keyvault/new-azkeyvault) ile bir Key Vault oluşturun. Her Key Vault benzersiz bir ad gerektirir ve küçük harflerle yazılmalıdır. Aşağıdaki örnekte yer alan `mykeyvault` değerini, kendi benzersiz Key Vault adınızla değiştirin:

```azurepowershell-interactive
$keyvaultName="mykeyvault"
New-AzKeyVault -VaultName $keyvaultName `
    -ResourceGroup $resourceGroup `
    -Location $location `
    -EnabledForDeployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a>Sertifika oluşturma ve sertifikayı Key Vault’ta depolama
Üretim sırasında kullanım için, [Import-AzureKeyVaultCertificate](https://docs.microsoft.com/powershell/module/az.keyvault/import-azurekeyvaultcertificate) komutunu kullanarak güvenilen bir sağlayıcı tarafından imzalanan geçerli bir sertifikayı içeri aktarmalısınız. Bu öğreticide, [New-AzureKeyVaultCertificatePolicy](https://docs.microsoft.com/powershell/module/az.keyvault/new-azurekeyvaultcertificatepolicy) tarafından sağlanan varsayılan sertifika ilkesini kullanan [Add-AzureKeyVaultCertificate](https://docs.microsoft.com/powershell/module/az.keyvault/add-azurekeyvaultcertificate) ile nasıl otomatik olarak imzalanan sertifika oluşturabileceğiniz gösterilmektedir: 

```azurepowershell-interactive
$policy = New-AzureKeyVaultCertificatePolicy `
    -SubjectName "CN=www.contoso.com" `
    -SecretContentType "application/x-pkcs12" `
    -IssuerName Self `
    -ValidityInMonths 12

Add-AzureKeyVaultCertificate `
    -VaultName $keyvaultName `
    -Name "mycert" `
    -CertificatePolicy $policy 
```


## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
VM için [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile bir yönetici kullanıcı adı ve parola ayarlayın:

```azurepowershell-interactive
$cred = Get-Credential
```

Sanal makine ile oluşturabileceğiniz artık [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm). Aşağıdaki örnekte *EastUS* konumunda *myVM* adlı bir VM oluşturulur. Mevcut değilse, destekleyici ağ kaynakları oluşturulur. Güvenli web trafiği sağlanması için, cmdlet ayrıca *443* numaralı bağlantı noktasını açar.

```azurepowershell-interactive
# Create a VM
New-AzVm `
    -ResourceGroupName $resourceGroup `
    -Name "myVM" `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred `
    -OpenPorts 443

# Use the Custom Script Extension to install IIS
Set-AzVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location $location `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server -IncludeManagementTools"}'
```

VM’nin oluşturulması birkaç dakika sürer. Son adım Azure özel betik uzantısı ile IIS web sunucusunu yüklemek için kullanır. [kümesi AzVmExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmextension).


## <a name="add-a-certificate-to-vm-from-key-vault"></a>Key Vault’tan VM'ye sertifika ekleme
Key Vault’tan VM’ye sertifika eklemek için, [Get-AzureKeyVaultSecret](https://docs.microsoft.com/powershell/module/az.keyvault/get-azurekeyvaultsecret) ile sertifikanızın kimliğini alın. İle VM'ye sertifika ekleme [Ekle AzVMSecret](https://docs.microsoft.com/powershell/module/az.compute/add-azvmsecret):

```azurepowershell-interactive
$certURL=(Get-AzureKeyVaultSecret -VaultName $keyvaultName -Name "mycert").id

$vm=Get-AzVM -ResourceGroupName $resourceGroup -Name "myVM"
$vaultId=(Get-AzKeyVault -ResourceGroupName $resourceGroup -VaultName $keyVaultName).ResourceId
$vm = Add-AzVMSecret -VM $vm -SourceVaultId $vaultId -CertificateStore "My" -CertificateUrl $certURL

Update-AzVM -ResourceGroupName $resourceGroup -VM $vm
```


## <a name="configure-iis-to-use-the-certificate"></a>Sertifikayı kullanmak üzere IIS'yi yapılandırma
Özel betik uzantısı'yla yeniden [kümesi AzVMExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmextension) IIS yapılandırmasını güncelleştirmek için. Bu güncelleştirme, Key Vault’tan IIS’ye eklenen sertifikayı uygular ve web bağlamasını yapılandırır:

```azurepowershell-interactive
$PublicSettings = '{
    "fileUris":["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/secure-iis.ps1"],
    "commandToExecute":"powershell -ExecutionPolicy Unrestricted -File secure-iis.ps1"
}'

Set-AzVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location $location `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -SettingString $publicSettings
```


### <a name="test-the-secure-web-app"></a>Güvenli web uygulamasını sınama
İle sanal makinenizin genel IP adresini [Get-AzPublicIPAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress). Aşağıdaki örnek, daha önce oluşturulan `myPublicIP` için IP adresini alır:

```azurepowershell-interactive
Get-AzPublicIPAddress -ResourceGroupName $resourceGroup -Name "myPublicIPAddress" | select "IpAddress"
```

Artık bir web tarayıcısı açıp adres çubuğuna `https://<myPublicIP>` ifadesini girebilirsiniz. Otomatik olarak imzalanan sertifika kullandıysanız güvenlik uyarısını kabul etmek için, **Ayrıntılar**’ı seçin ve sonra **Web sayfasına gidin**:

![Web tarayıcısı güvenlik uyarısını kabul edin](./media/tutorial-secure-web-server/browser-warning.png)

Güvenli IIS siteniz, sonra aşağıdaki örnekte olduğu gibi görüntülenir:

![Çalışan güvenli IIS sitesini görüntüleme](./media/tutorial-secure-web-server/secured-iis.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure Key Vault’ta depolanan bir SSL sertifikasıyla IIS web sunucusunun güvenliğini sağladınız. Şunları öğrendiniz:

> [!div class="checklist"]
> * Azure Key Vault oluşturma
> * Sertifikaları oluşturma veya Key Vault’a yükleme
> * VM oluşturma ve ISS web sunucusunu yükleme
> * Sertifikayı VM’ye ekleme ve ISS’yi bir SSL bağlamasıyla yapılandırma

Hazır sanal makine betik örneklerini görmek için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [Windows sanal makinesi betik örnekleri](./powershell-samples.md)
