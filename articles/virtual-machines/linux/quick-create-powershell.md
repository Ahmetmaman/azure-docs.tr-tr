---
title: Hızlı Başlangıç - Azure PowerShell ile Linux VM Oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta Azure PowerShell’i kullanarak Linux sanal makinesi oluşturmayı öğrenirsiniz
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/17/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: df6f99bfe9f1ae7b79f0f382fdee4fe4f1578bad
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49407546"
---
# <a name="quickstart-create-a-linux-virtual-machine-in-azure-with-powershell"></a>Hızlı Başlangıç: PowerShell ile Azure'da Linux sanal makinesi oluşturma

Azure PowerShell modülü, PowerShell komut satırından veya betik içinden Azure kaynakları oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıçta Azure PowerShell modülünü kullanarak Azure’da bir Linux sanal makinesinin (VM) nasıl dağıtılacağı gösterilir. Bu hızlı başlangıç Canonical’daki Ubuntu 16.04 LTS market görüntüsünü kullanmaktadır. Ayrıca VM'nizin çalıştığını görmek için SSH ile VM bağlantısı kurup NGINX web sunucusunu da yükleyeceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

PowerShell’i yerel olarak yükleyip kullanmak istiyorsanız bu öğretici, Azure PowerShell modülü 5.7.0 veya üzeri bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-ssh-key-pair"></a>SSH anahtar çifti oluşturma

Bu hızlı başlangıcı tamamlamak için bir SSH anahtar çifti gerekir. Bir SSH anahtar çiftiniz varsa bu adımı atlayabilirsiniz.

SSH anahtar çifti oluşturmak için bir Bash kabuğu açın ve [ssh-keygen](https://www.ssh.com/ssh/keygen/) komutunu kullanın. Yerel bilgisayarınızda Bash kabuğunuz yoksa [Azure Cloud Shell](https://shell.azure.com/bash)'i kullanabilirsiniz.  

```azurepowershell-interactive
ssh-keygen -t rsa -b 2048
```

PuTTy kullanımı dahil olmak üzere SSH anahtar çiftlerinin oluşturulması konusunda daha ayrıntılı bilgi edinmek için bkz. [Windows ile SSH anahtarları kullanma](ssh-from-windows.md).

SSH anahtar çiftinizi Cloud Shell'i kullanarak oluşturduğunuzda [Cloud Shell tarafından otomatik olarak oluşturulan bir depolama hesabında](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage) yer alan bir kapsayıcı görüntüsünde depolanır. Anahtarlarınızı alana kadar depolama hesabını veya içindeki dosya paylaşımını silmeyin. Aksi takdirde VM erişimini kaybedersiniz. 

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) ile yeni bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır:

```azurepowershell-interactive
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

## <a name="create-virtual-network-resources"></a>Sanal ağ kaynakları oluşturma

Bir sanal ağ, alt ağ ve genel IP adresi oluşturun. Bu kaynaklar, VM'ye ağ bağlantısı sağlamak ve VM'yi İnternet'e bağlamak için kullanılır:

```azurepowershell-interactive
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS" `
  -Name "myVNET" `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS" `
  -AllocationMethod Static `
  -IdleTimeoutInMinutes 4 `
  -Name "mypublicdns$(Get-Random)"
```

Azure Ağ Güvenlik Grubu ve trafik kuralı oluşturun. Ağ güvenlik Grubu, gelen ve giden kurallarını kullanarak VM'nin güvenliğini sağlar. Aşağıdaki örnekte, TCP bağlantı noktası 22 için SSH bağlantılarına izin veren bir gelen kuralı oluşturulur. Gelen web trafiğine izin vermek amacıyla, TCP bağlantı noktası 80 için de bir gelen kuralı oluşturulur.

```azurepowershell-interactive
# Create an inbound network security group rule for port 22
$nsgRuleSSH = New-AzureRmNetworkSecurityRuleConfig `
  -Name "myNetworkSecurityGroupRuleSSH"  `
  -Protocol "Tcp" `
  -Direction "Inbound" `
  -Priority 1000 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 22 `
  -Access "Allow"

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
  -Name "myNetworkSecurityGroupRuleWWW"  `
  -Protocol "Tcp" `
  -Direction "Inbound" `
  -Priority 1001 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access "Allow"

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS" `
  -Name "myNetworkSecurityGroup" `
  -SecurityRules $nsgRuleSSH,$nsgRuleWeb
```

[New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) komutuyla bir sanal ağ arabirim kartı (NIC) oluşturun. Sanal NIC, VM'yi bir alt ağa, Ağ Güvenlik Grubu'na ve genel IP adresine bağlar.

```azurepowershell-interactive
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
  -Name "myNic" `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS" `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id `
  -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

PowerShell’de bir VM oluşturmak için kullanılacak görüntü, boyut ve kimlik doğrulaması seçenekleri gibi ayarların olduğu bir yapılandırma oluşturursunuz. Ardından bu yapılandırma VM’yi derlemek için kullanılır.

SSH kimlik bilgilerini, işletim sistemi bilgilerini ve VM boyutunu tanımlayın. Bu örnekte SSH anahtarı `~/.ssh/id_rsa.pub`‘da depolanmaktadır. 

```azurepowershell-interactive
# Define a credential object
$securePassword = ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig `
  -VMName "myVM" `
  -VMSize "Standard_D1" | `
Set-AzureRmVMOperatingSystem `
  -Linux `
  -ComputerName "myVM" `
  -Credential $cred `
  -DisablePasswordAuthentication | `
Set-AzureRmVMSourceImage `
  -PublisherName "Canonical" `
  -Offer "UbuntuServer" `
  -Skus "16.04-LTS" `
  -Version "latest" | `
Add-AzureRmVMNetworkInterface `
  -Id $nic.Id

# Configure the SSH key
$sshPublicKey = cat ~/.ssh/id_rsa.pub
Add-AzureRmVMSshPublicKey `
  -VM $vmconfig `
  -KeyData $sshPublicKey `
  -Path "/home/azureuser/.ssh/authorized_keys"
```

Şimdi, [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) ile oluşturmak için önceki yapılandırma tanımlarını birleştirin:

```azurepowershell-interactive
New-AzureRmVM `
  -ResourceGroupName "myResourceGroup" `
  -Location eastus -VM $vmConfig
```

VM'nizin dağıtılması birkaç dakika sürer. Dağıtım tamamlandıktan sonra bir sonraki bölüme geçin.

## <a name="connect-to-the-vm"></a>VM’ye bağlanma

Genel IP adresini kullanarak VM ile bir SSH bağlantısı oluşturun. VM'nin genel IP adresini görmek için [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) cmdlet'ini kullanın:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroup" | Select "IpAddress"
```

SSH anahtar çiftini oluşturmak için kullandığınız Bash kabuğunu kullanarak ([Azure Cloud Shell](https://shell.azure.com/bash) veya yerel Bash kabuğunuz gibi) SSH bağlantı komutunu kabuğa yapıştırarak bir SSH oturumu oluşturun.

```bash
ssh azureuser@10.111.12.123
```

Sorulduğunda, kullanıcı adı *azureuser* olarak girilmelidir. SSH anahtarlarınızla birlikte bir parola kullanılıyorsa, istendiğinde bu parolayı girmelisiniz.


## <a name="install-nginx"></a>NGINX yükleme

Sanal makinenizin çalıştığını görmek için NGINX web sunucusunu yükleyin. SSH oturumunuzdan paket kaynaklarınızı güncelleştirip en son NGINX paketini yükleyin.

```bash
sudo apt-get -y update
sudo apt-get -y install nginx
```

İşlemi tamamladığınızda `exit` yazarak SSH oturumunu kapatın.


## <a name="view-the-web-server-in-action"></a>Web sunucusunu iş başında görün

İstediğiniz web tarayıcısını kullanarak varsayılan NGINX karşılama sayfasını görüntüleyin. VM'nin genel IP adresini web adresi olarak girin. Genel IP adresini VM genel bakış sayfasında veya önceden kullandığınız SSH bağlantı dizesinde bulabilirsiniz.

![Varsayılan NGINX sitesi](./media/quick-create-cli/nginx.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) cmdlet’ini kullanarak kaynak grubunu, VM’yi ve tüm ilgili kaynakları kaldırabilirsiniz:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta basit bir sanal makine dağıttınız, bir Ağ Güvenlik Grubu ve kuralı oluşturdunuz ve temel bir web sunucusu yüklediniz. Azure sanal makineleri hakkında daha fazla bilgi için Linux VM’lerine yönelik öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Linux sanal makine öğreticileri](./tutorial-manage-vm.md)
