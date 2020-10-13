---
title: Azure adanmış Konakları Azure PowerShell kullanarak dağıtma
description: Azure PowerShell kullanarak VM 'Leri adanmış ana bilgisayarlara dağıtın.
author: cynthn
ms.service: virtual-machines-windows
ms.topic: how-to
ms.workload: infrastructure
ms.date: 08/01/2019
ms.author: cynthn
ms.reviewer: zivr
ms.openlocfilehash: 884a9e82dacb2a0dfc6763809a2ccfd2b886df1a
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91974184"
---
# <a name="deploy-vms-to-dedicated-hosts-using-the-azure-powershell"></a>Azure PowerShell kullanarak VM 'Leri adanmış konaklara dağıtma

Bu makalede, sanal makinelerinizi (VM 'Ler) barındırmak için Azure [adanmış ana bilgisayar](../dedicated-hosts.md) oluşturma konusunda size kılavuzluk eder. 

Azure PowerShell sürüm 2.8.0 veya üstünü yüklediğinizden ve ' de bir Azure hesabında oturum açtığınızdan emin olun `Connect-AzAccount` . 

## <a name="limitations"></a>Sınırlamalar

- Sanal Makine Ölçek Kümeleri Şu anda adanmış konaklarda desteklenmiyor.
- Adanmış konaklar için kullanılabilen Boyutlar ve donanım türleri bölgelere göre farklılık gösterir. Daha fazla bilgi edinmek için konak [fiyatlandırma sayfasına](https://aka.ms/ADHPricing) bakın.

## <a name="create-a-host-group"></a>Konak grubu oluştur

**Konak grubu** , adanmış Konakları koleksiyonunu temsil eden bir kaynaktır. Bir bölge ve kullanılabilirlik bölgesinde bir konak grubu oluşturur ve bu gruba ana bilgisayar ekleyebilirsiniz. Yüksek kullanılabilirlik için planlama yaparken ek seçenekler vardır. Aşağıdaki seçeneklerden birini veya her ikisini de adanmış konaklarınız ile kullanabilirsiniz: 
- Birden çok kullanılabilirlik alanı arasında yayılır. Bu durumda, kullanmak istediğiniz her bölgede bir konak grubunuz olması gerekir.
- Fiziksel raflarla eşlenen birden çok hata etki alanı arasında yayılma. 
 
Her iki durumda da, konak grubunuz için hata etki alanı sayısını sağlamanız gerekir. Grubunuzdaki hata etki alanlarına yayılması istemiyorsanız, hata etki alanı sayısı 1 ' i kullanın. 

Hem kullanılabilirlik bölgelerini hem de hata etki alanlarını kullanmaya karar verebilirsiniz. Bu örnek, 2 hata etki alanı ile bölge 1 ' de bir konak grubu oluşturur. 


```azurepowershell-interactive
$rgName = "myDHResourceGroup"
$location = "EastUS"

New-AzResourceGroup -Location $location -Name $rgName
$hostGroup = New-AzHostGroup `
   -Location $location `
   -Name myHostGroup `
   -PlatformFaultDomain 2 `
   -ResourceGroupName $rgName `
   -Zone 1
```


`-SupportAutomaticPlacement true`VM 'leri ve ölçek kümesi örneklerinizin konak grubundaki konaklara otomatik olarak yerleştirilmesini sağlamak için parametresini ekleyin. Daha fazla bilgi için bkz. [el ile ve otomatik yerleştirme ](../dedicated-hosts.md#manual-vs-automatic-placement).

> [!IMPORTANT]
> Otomatik yerleştirme Şu anda genel önizlemededir.
> Önizlemeye katılmak için Önizleme ekleme anketini şurada doldurun [https://aka.ms/vmss-adh-preview](https://aka.ms/vmss-adh-preview) .
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="create-a-host"></a>Konak Oluşturma

Şimdi konak grubunda adanmış bir konak oluşturalım. Konak için bir ada ek olarak, ana bilgisayar için SKU sağlamanız gerekir. Ana bilgisayar SKU 'SU, desteklenen VM serisini ve adanmış ana bilgisayarınız için donanım oluşturmayı yakalar.

Konak SKU 'Ları ve fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure ayrılmış ana bilgisayar fiyatlandırması](https://aka.ms/ADHPricing).

Konak grubunuz için bir hata etki alanı sayısı ayarlarsanız, ana bilgisayarınız için hata etki alanını belirtmeniz istenir. Bu örnekte, ana bilgisayar için hata etki alanını 1 olarak ayarlayacağız.


```azurepowershell-interactive
$dHost = New-AzHost `
   -HostGroupName $hostGroup.Name `
   -Location $location -Name myHost `
   -ResourceGroupName $rgName `
   -Sku DSv3-Type1 `
   -AutoReplaceOnFailure 1 `
   -PlatformFaultDomain 1
```

## <a name="create-a-vm"></a>VM oluşturma

Adanmış konakta bir sanal makine oluşturun. 

Konak grubunuzu oluştururken bir kullanılabilirlik alanı belirttiyseniz, sanal makineyi oluştururken aynı bölgeyi kullanmanız gerekir. Bu örnekte, ana bilgisayar grubumuz bölge 1 ' de olduğundan, VM 'yi bölge 1 ' de oluşturmamız gerekiyor.  


```azurepowershell-interactive
$cred = Get-Credential
New-AzVM `
   -Credential $cred `
   -ResourceGroupName $rgName `
   -Location $location `
   -Name myVM `
   -HostId $dhost.Id `
   -Image Win2016Datacenter `
   -Zone 1 `
   -Size Standard_D4s_v3
```

> [!WARNING]
> Yeterli kaynağı olmayan bir konakta sanal makine oluşturursanız, sanal makine başarısız durumda oluşturulur. 

## <a name="check-the-status-of-the-host"></a>Konağın durumunu denetleme

Ana bilgisayar sistem durumunu ve [GetAzHost](/powershell/module/az.compute/get-azhost) kullanarak yine de konağa ne kadar sanal makine dağıtacağınızı kontrol edebilirsiniz `-InstanceView` .

```azurepowershell-interactive
Get-AzHost `
   -ResourceGroupName $rgName `
   -Name myHost `
   -HostGroupName $hostGroup.Name `
   -InstanceView
```

Çıktı şuna benzer olacaktır:

```
ResourceGroupName      : myDHResourceGroup
PlatformFaultDomain    : 1
AutoReplaceOnFailure   : True
HostId                 : 12345678-1234-1234-abcd-abc123456789
ProvisioningTime       : 7/28/2019 5:31:01 PM
ProvisioningState      : Succeeded
InstanceView           : 
  AssetId              : abc45678-abcd-1234-abcd-123456789abc
  AvailableCapacity    : 
    AllocatableVMs[0]  : 
      VmSize           : Standard_D2s_v3
      Count            : 32
    AllocatableVMs[1]  : 
      VmSize           : Standard_D4s_v3
      Count            : 16
    AllocatableVMs[2]  : 
      VmSize           : Standard_D8s_v3
      Count            : 8
    AllocatableVMs[3]  : 
      VmSize           : Standard_D16s_v3
      Count            : 4
    AllocatableVMs[4]  : 
      VmSize           : Standard_D32-8s_v3
      Count            : 2
    AllocatableVMs[5]  : 
      VmSize           : Standard_D32-16s_v3
      Count            : 2
    AllocatableVMs[6]  : 
      VmSize           : Standard_D32s_v3
      Count            : 2
    AllocatableVMs[7]  : 
      VmSize           : Standard_D64-16s_v3
      Count            : 1
    AllocatableVMs[8]  : 
      VmSize           : Standard_D64-32s_v3
      Count            : 1
    AllocatableVMs[9]  : 
      VmSize           : Standard_D64s_v3
      Count            : 1
  Statuses[0]          : 
    Code               : ProvisioningState/succeeded
    Level              : Info
    DisplayStatus      : Provisioning succeeded
    Time               : 7/28/2019 5:31:01 PM
  Statuses[1]          : 
    Code               : HealthState/available
    Level              : Info
    DisplayStatus      : Host available
Sku                    : 
  Name                 : DSv3-Type1
Id                     : /subscriptions/10101010-1010-1010-1010-101010101010/re
sourceGroups/myDHResourceGroup/providers/Microsoft.Compute/hostGroups/myHostGroup/hosts
/myHost
Name                   : myHost
Location               : eastus
Tags                   : {}
```

## <a name="create-a-scale-set-preview"></a>Ölçek kümesi oluşturma (Önizleme)

> [!IMPORTANT]
> Adanmış konaklardaki sanal makine ölçek kümeleri Şu anda genel önizlemededir.
> Önizlemeye katılmak için Önizleme ekleme anketini şurada doldurun [https://aka.ms/vmss-adh-preview](https://aka.ms/vmss-adh-preview) .
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bir ölçek kümesi dağıttığınızda konak grubunu belirtirsiniz.

```azurepowershell-interactive
New-AzVmss `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS" `
  -VMScaleSetName "myDHScaleSet" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicyMode "Automatic"`
  -HostGroupId $hostGroup.Id
```

Ölçek kümesinin dağıtılacağı Konağı el ile seçmek istiyorsanız `--host` konağın adını ekleyin.



## <a name="add-an-existing-vm"></a>Var olan bir VM 'yi ekleme 

Ayrılmış bir konağa mevcut bir VM ekleyebilirsiniz, ancak önce VM 'nin Stop\satıcılarla locatedolması gerekir. Bir VM 'yi adanmış bir konağa taşımadan önce, VM yapılandırmasının desteklendiğinden emin olun:

- VM boyutu, ayrılmış konakla aynı büyüklükte bir aile içinde olmalıdır. Örneğin, adanmış ana bilgisayarınız DSv3 ise sanal makine boyutu Standard_D4s_v3 olabilir, ancak bir Standard_A4_v2 olamaz. 
- VM 'nin adanmış konakla aynı bölgede bulunması gerekir.
- VM, bir yakınlık yerleşimi grubunun parçası olamaz. Ayrılmış bir konağa taşımadan önce VM 'yi yakınlık yerleşimi grubundan kaldırın. Daha fazla bilgi için bkz. [bir VM 'yi bir yakınlık yerleşimi grubundan taşıma](./proximity-placement-groups.md#move-an-existing-vm-out-of-a-proximity-placement-group)
- VM bir kullanılabilirlik kümesinde olamaz.
- VM bir kullanılabilirlik bölgeindeyse, konak grubuyla aynı Kullanılabilirlik bölgesi olması gerekir. VM ve konak grubu için kullanılabilirlik bölgesi ayarlarının eşleşmesi gerekir.

Değişkenlerin değerlerini kendi bilgileriniz ile değiştirin.

```azurepowershell-interactive
$vmRGName = "movetohost"
$vmName = "myVMtoHost"
$dhRGName = "myDHResourceGroup"
$dhGroupName = "myHostGroup"
$dhName = "myHost"

$myDH = Get-AzHost `
   -HostGroupName $dhGroupName `
   -ResourceGroupName $dhRGName `
   -Name $dhName
   
$myVM = Get-AzVM `
   -ResourceGroupName $vmRGName `
   -Name $vmName
   
$myVM.Host = New-Object Microsoft.Azure.Management.Compute.Models.SubResource

$myVM.Host.Id = "$myDH.Id"

Stop-AzVM `
   -ResourceGroupName $vmRGName `
   -Name $vmName -Force
   
Update-AzVM `
   -ResourceGroupName $vmRGName `
   -VM $myVM -Debug
   
Start-AzVM `
   -ResourceGroupName $vmRGName `
   -Name $vmName
```


## <a name="clean-up"></a>Temizleme

Hiçbir sanal makine dağıtılmamışsa bile adanmış ana bilgisayarlar için ücret ödersiniz. Maliyetleri kaydetmek için şu anda kullanmadığınız tüm Konakları silmelisiniz.  

Bir konağı yalnızca, onu kullanan daha fazla sanal makine olmadığında silebilirsiniz. [Remove-AzVM](/powershell/module/az.compute/remove-azvm)' i kullanarak VM 'leri silin.

```azurepowershell-interactive
Remove-AzVM -ResourceGroupName $rgName -Name myVM
```

VM 'Leri sildikten sonra [Remove-AzHost](/powershell/module/az.compute/remove-azhost)komutunu kullanarak Konağı silebilirsiniz.

```azurepowershell-interactive
Remove-AzHost -ResourceGroupName $rgName -Name myHost
```

Tüm konaklarınızı sildikten sonra [Remove-AzHostGroup](/powershell/module/az.compute/remove-azhostgroup)komutunu kullanarak konak grubunu silebilirsiniz. 

```azurepowershell-interactive
Remove-AzHost -ResourceGroupName $rgName -Name myHost
```

Ayrıca, [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup)komutunu kullanarak tüm kaynak grubunu tek bir komutta silebilirsiniz. Bu işlem, tüm VM 'Ler, konaklar ve konak grupları dahil olmak üzere grupta oluşturulan tüm kaynakları siler.
 
```azurepowershell-interactive
Remove-AzResourceGroup -Name $rgName
```


## <a name="next-steps"></a>Sonraki adımlar

- [Burada](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md), bir bölgedeki maksimum dayanıklılık için hem bölge hem de hata etki alanı kullanan örnek şablon vardır.

- Ayrıca, [Azure Portal](../dedicated-hosts-portal.md)kullanarak adanmış konaklar da dağıtabilirsiniz.