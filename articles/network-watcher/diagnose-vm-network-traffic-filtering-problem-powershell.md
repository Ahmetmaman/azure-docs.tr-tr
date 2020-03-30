---
title: 'Hızlı başlatma: VM ağ trafiği filtresi sorununu tanıla - Azure PowerShell'
titleSuffix: Azure Network Watcher
description: Bu hızlı başlangıçta, Azure Ağ İzleyicisi'nin IP akış doğrulama özelliği kullanılarak sanal makine ağ trafiği filtreleme sorununun nasıl tanılanacağını öğrenirsiniz.
services: network-watcher
documentationcenter: network-watcher
author: damendo
editor: ''
tags: azure-resource-manager
Customer intent: I need to diagnose a virtual machine (VM) network traffic filter problem that prevents communication to and from a VM.
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: network-watcher
ms.workload: infrastructure
ms.date: 04/20/2018
ms.author: damendo
ms.custom: mvc
ms.openlocfilehash: 5438cc07670393cab69344544ea1b68c46c42bd6
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76844033"
---
# <a name="quickstart-diagnose-a-virtual-machine-network-traffic-filter-problem---azure-powershell"></a>Hızlı Başlangıç: Sanal makine ağ trafiği filtreleme sorununu tanılama - Azure PowerShell

Bu hızlı başlangıçta bir sanal makine (VM) dağıtır ve sonra bir IP adresi ve URL ile iletişimleri ve bir IP adresinden gelen iletişimleri denetlersiniz. Bir iletişim hatasının nedenini ve bu hatayı nasıl çözeceğinizi belirlersiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, `Az` bu hızlı başlatma için Azure PowerShell modülü gerekir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.



## <a name="create-a-vm"></a>VM oluşturma

Bir sanal makine oluşturabilmeniz için sanal makineyi içerecek bir kaynak grubu oluşturmanız gerekir. [New-AzResourceGroup](/powershell/module/az.Resources/New-azResourceGroup)ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroup -Location EastUS
```

[Yeni-AzVM](/powershell/module/az.compute/new-azvm)ile VM oluşturun. Bu adımı çalıştırırken kimlik bilgileri istenir. Girdiğiniz değerler, sanal makinenin kullanıcı adı ve parolası olarak yapılandırılır.

```azurepowershell-interactive
$vM = New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVm" `
    -Location "East US"
```

Sanal makinenin oluşturulması birkaç dakika sürer. Sanal makine oluşturulup PowerShell çıktı döndürünceye kadar kalan adımlara devam etmeyin.

## <a name="test-network-communication"></a>Ağ iletişimini test etme

Ağ İzleyicisi ile ağ iletişimini test etmek için önce test etmek istediğiniz sanal makinenin bulunduğu bölgede bir ağ izleyicisini etkinleştirmeniz ve sonra iletişimi test etmek için Ağ İzleyicisinin IP akışı doğrulama özelliğini kullanmanız gerekir.

### <a name="enable-network-watcher"></a>Ağ izleyicisini etkinleştirme

Doğu ABD bölgesinde zaten etkin bir ağ izleyiciniz varsa, ağ izleyicisini almak için [Get-AzNetworkWatcher'ı](/powershell/module/az.network/get-aznetworkwatcher) kullanın. Aşağıdaki örnekte, *NetworkWatcherRG* kaynak grubunda bulunan *NetworkWatcher_eastus* adlı mevcut ağ izleyicisi alınır:

```azurepowershell-interactive
$networkWatcher = Get-AzNetworkWatcher `
  -Name NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG
```

Doğu ABD bölgesinde zaten etkin bir ağ izleyiciniz yoksa, Doğu ABD bölgesinde bir ağ izleyicisi oluşturmak için [New-AzNetworkWatcher'ı](/powershell/module/az.network/new-aznetworkwatcher) kullanın:

```azurepowershell-interactive
$networkWatcher = New-AzNetworkWatcher `
  -Name "NetworkWatcher_eastus" `
  -ResourceGroupName "NetworkWatcherRG" `
  -Location "East US"
```

### <a name="use-ip-flow-verify"></a>IP akışı doğrulamayı kullanma

Bir sanal makine oluşturduğunuzda Azure varsayılan olarak sanal makineye/sanal makineden ağ trafiğine izin verir veya ağ trafiğini reddeder. Daha sonra Azure’ın varsayılanlarını geçersiz kılarak ek trafik türlerine izin verebilir veya ek trafik türlerini reddedebilirsiniz. Farklı hedeflere ve kaynak IP adresinden trafiğe izin verilip verilmediğini veya reddedilip reddedilemediğini test etmek için [Test-AzNetworkWatcherIPFlow](/powershell/module/az.network/test-aznetworkwatcheripflow) komutunu kullanın.

Sanal makineden, www.bing.com adresinin IP adreslerinden birine giden iletişimi test etme:

```azurepowershell-interactive
Test-AzNetworkWatcherIPFlow `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $vM.Id `
  -Direction Outbound `
  -Protocol TCP `
  -LocalIPAddress 192.168.1.4 `
  -LocalPort 60000 `
  -RemoteIPAddress 13.107.21.200 `
  -RemotePort 80
```

Birkaç saniye sonra döndürülen sonuç, **AllowInternetOutbound** adlı bir güvenlik kuralı tarafından erişime izin verildiğini size bildirir.

Sanal makineden 172.31.0.100 adresine giden iletişimi test etme:

```azurepowershell-interactive
Test-AzNetworkWatcherIPFlow `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $vM.Id `
  -Direction Outbound `
  -Protocol TCP `
  -LocalIPAddress 192.168.1.4 `
  -LocalPort 60000 `
  -RemoteIPAddress 172.31.0.100 `
  -RemotePort 80
```

Döndürülen sonuç, **DefaultOutboundDenyAll** adlı bir güvenlik kuralı tarafından erişimin reddedildiğini size bildirir.

172.31.0.100 adresinden sanal makineye gelen iletişimi test etme:

```azurepowershell-interactive
Test-AzNetworkWatcherIPFlow `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $vM.Id `
  -Direction Inbound `
  -Protocol TCP `
  -LocalIPAddress 192.168.1.4 `
  -LocalPort 80 `
  -RemoteIPAddress 172.31.0.100 `
  -RemotePort 60000
```

Döndürülen sonuç, **DefaultInboundDenyAll** adlı bir güvenlik kuralı tarafından erişimin reddedildiğini size bildirir. Hangi güvenlik kurallarının bir sanal makineye/sanal makineden trafiğe izin verdiğini veya trafiği reddettiğini öğrendiğinize göre sorunların nasıl çözümleneceğini belirleyebilirsiniz.

## <a name="view-details-of-a-security-rule"></a>Bir güvenlik kuralının ayrıntılarını görüntüleme

[Test ağ iletişimindeki](#test-network-communication) kuralların neden iletişime izin vererek veya engellemeyi belirlemek için [Get-AzEffectiveNetworkSecurityGroup](/powershell/module/az.network/get-azeffectivenetworksecuritygroup)ile ağ arabirimi için etkili güvenlik kurallarını gözden geçirin:

```azurepowershell-interactive
Get-AzEffectiveNetworkSecurityGroup `
  -NetworkInterfaceName myVm `
  -ResourceGroupName myResourceGroup
```

Döndürülen çıktı, [IP akışı doğrulamayı kullanma](#use-ip-flow-verify) bölümünde yer alan ve www.bing.com adresine giden erişime izin veren **AllowInternetOutbound** kuralı için aşağıdaki metni içerir:

```powershell
{
  "Name":
"defaultSecurityRules/AllowInternetOutBound",
  "Protocol": "All",
  "SourcePortRange": [
    "0-65535"
  ],
  "DestinationPortRange": [
    "0-65535"
  ],
  "SourceAddressPrefix": [
    "0.0.0.0/0"
  ],
  "DestinationAddressPrefix": [
    "Internet"
  ],
  "ExpandedSourceAddressPrefix": [],
  "ExpandedDestinationAddressPrefix": [
    "1.0.0.0/8",
    "2.0.0.0/7",
    "4.0.0.0/6",
    "8.0.0.0/7",
    "11.0.0.0/8",
    "12.0.0.0/6",
    ...
    ],
    "Access": "Allow",
    "Priority": 65001,
    "Direction": "Outbound"
  },
```

Çıktıda **destinationAddressPrefix** öğesinin **İnternet** olduğunu görebilirsiniz. [IP akışı doğrulamayı kullanma](#use-ip-flow-verify) altında test ettiğiniz 13.107.21.200 adresinin **İnternet** ile nasıl ilişkilendirildiği açık değildir. **ExpandedDestinationAddressPrefix**altında listelenen birkaç adres önekleri görürsünüz. Listedeki ön eklerden biri **12.0.0.0/6**'dır ve IP adreslerinin 12.0.0.1-15.255.255.254 aralığını kapsar. 13.107.21.200 bu adres aralığı içinde yer aldığından **AllowInternetOutBound** kuralı, giden trafiğe izin verir. Ayrıca, `Get-AzEffectiveNetworkSecurityGroup` tarafından döndürülen çıktıda listelenen kurallarda bu kuralı geçersiz kılan daha yüksek **öncelikli** (daha küçük numaralı) kurallar da yoktur. 13.107.21.200 adresine giden iletişimi reddetmek için, IP adresine giden 80 numaralı bağlantı noktasını reddeden, daha yüksek önceliğe sahip bir güvenlik kuralı ekleyebilirsiniz.

[IP akışı doğrulamayı kullanma](#use-ip-flow-verify) bölümünde 172.131.0.100 adresine giden iletişimi test etmek için `Test-AzNetworkWatcherIPFlow` komutunu çalıştırdığınızda elde edilen çıktı size **DefaultOutboundDenyAll** kuralının iletişimi reddettiğini bildirdi. **DefaultOutboundDenyAll** kuralı, `Get-AzEffectiveNetworkSecurityGroup` komutundan elde edilen aşağıdaki çıktıda listelenen **DenyAllOutBound** kuralına karşılık gelir:

```powershell
{
"Name": "defaultSecurityRules/DenyAllOutBound",
"Protocol": "All",
"SourcePortRange": [
  "0-65535"
],
"DestinationPortRange": [
  "0-65535"
],
"SourceAddressPrefix": [
  "0.0.0.0/0"
],
"DestinationAddressPrefix": [
  "0.0.0.0/0"
],
"ExpandedSourceAddressPrefix": [],
"ExpandedDestinationAddressPrefix": [],
"Access": "Deny",
"Priority": 65500,
"Direction": "Outbound"
}
```

Kural, **0.0.0.0/0'ı** **DestinationAddressPrefix**olarak listeler. Kural, adres `Get-AzEffectiveNetworkSecurityGroup` komuttan çıkan diğer giden kuralların **DestinationAddressPrefix** içinde olmadığından, 172.131.0.100 giden iletişimi reddeder. Giden iletişime izin vermek için, 172.131.0.100 adresinde 80 numaralı bağlantı noktasına giden trafiğe izin veren daha yüksek öncelikli bir güvenlik kuralı ekleyebilirsiniz.

[IP akışı doğrulamayı kullanma](#use-ip-flow-verify) bölümünde 172.131.0.100 adresinden gelen iletişimi test etmek için `Test-AzNetworkWatcherIPFlow` komutunu çalıştırdığınızda, elde edilen çıktı size **DefaultInboundDenyAll** kuralının iletişimi reddettiğini bildirdi. **DefaultInboundDenyAll** kuralı, `Get-AzEffectiveNetworkSecurityGroup` komutundan elde edilen aşağıdaki çıktıda listelenen **DenyAllInBound** kuralına karşılık gelir:

```powershell
{
"Name": "defaultSecurityRules/DenyAllInBound",
"Protocol": "All",
"SourcePortRange": [
  "0-65535"
],
"DestinationPortRange": [
  "0-65535"
],
"SourceAddressPrefix": [
  "0.0.0.0/0"
],
"DestinationAddressPrefix": [
  "0.0.0.0/0"
],
"ExpandedSourceAddressPrefix": [],
"ExpandedDestinationAddressPrefix": [],
"Access": "Deny",
"Priority": 65500,
"Direction": "Inbound"
},
```

Çıktıda gösterildiği gibi, `Get-AzEffectiveNetworkSecurityGroup` komutundan elde edilen çıktıda, 172.131.0.100 adresinden sanal makineye gelen 80 numaralı bağlantı noktasına izin veren daha yüksek öncelikli başka bir kural olmadığından **DenyAllInBound** kuralı uygulanır. Gelen iletişime izin vermek için, 172.131.0.100 adresinden gelen 80 numaralı bağlantı noktasına izin veren daha yüksek öncelikli bir güvenlik kuralı ekleyebilirsiniz.

Bu hızlı başlangıçtaki denetimlerinde Azure yapılandırması test edilmiştir. Denetimler beklenen sonuçları döndürdüğü halde ağ sorunları yaşamaya devam ediyorsanız, sanal makineniz ve iletişim kurduğunuz uç nokta arasında bir güvenlik duvarı olmadığından ve sanal makinenizdeki işletim sisteminin, iletişime izin veren veya iletişimi reddeden bir güvenlik duvarının olmadığından emin olun.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulmadığında, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [Kaldır-AzResourceGroup'u](/powershell/module/az.resources/remove-azresourcegroup) kullanabilirsiniz:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir sanal makine oluşturdunuz ve gelen ve giden ağ trafiği filtrelerini tanıladınız. Ağ güvenlik grubu kurallarının bir sanal makineye gelen ve sanal makineden giden trafiğe izin verdiğini veya bu trafikleri reddettiğini öğrendiniz. [Güvenlik kuralları](../virtual-network/security-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) ve [güvenlik kuralları oluşturma](../virtual-network/manage-network-security-group.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#create-a-security-rule) hakkında daha fazla bilgi edinin.

Uygun ağ trafiği filtreleri mevcut olduğunda bile, yönlendirme yapılandırması nedeniyle bir sanal makineyle iletişim başarısız olabilir. Tek bir araçla sanal makine ağ yönlendirme sorunlarını tanılama hakkında bilgi edinmek için [Sanal makine yönlendirme sorunlarını tanılama](diagnose-vm-network-routing-problem-powershell.md) bölümüne veya giden yönlendirme, gecikme ve trafik filtreleme sorunlarını tanılama hakkında bilgi edinmek için [Bağlantı sorunlarını giderme](network-watcher-connectivity-powershell.md) bölümüne bakın.
