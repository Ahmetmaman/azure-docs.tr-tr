---
title: Öğretici - Azure sanal makine ölçek kümesi oluşturma ve yönetme
description: Örnek başlatma ve durdurma veya ölçek kümesi kapasitesini değiştirme gibi bazı genel yönetim görevlerinin yanı sıra, sanal makine ölçek kümesi oluşturmak için Azure PowerShell'in nasıl kullanılacağını öğrenin.
author: cynthn
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: tutorial
ms.date: 05/18/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 14616fcc9fd63731c50c5977c88b5030f60664ff
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76271402"
---
# <a name="tutorial-create-and-manage-a-virtual-machine-scale-set-with-azure-powershell"></a>Öğretici: Azure PowerShell ile sanal makine ölçek kümesi oluşturma ve yönetme

Sanal makine ölçek kümesi, birbiriyle aynı ve otomatik olarak ölçeklendirilen sanal makine kümesi dağıtmanızı ve yönetmenizi sağlar. Sanal makine ölçek kümesinin yaşam döngüsü boyunca bir veya daha fazla yönetim görevi çalıştırmanız gerekebilir. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Sanal makine ölçek kümesi oluşturma ve sanal makine ölçek kümesine bağlanma
> * VM görüntülerini seçme ve kullanma
> * Belirli sanal makine örneği boyutlarını görüntüleme ve kullanma
> * Ölçek kümesini el ile ölçeklendirme
> * Genel ölçek kümesi yönetim görevlerini gerçekleştirme

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [updated-for-az.md](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]



## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Sanal makine ölçek kümesinden önce kaynak grubu oluşturulmalıdır. [Yeni-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutuyla bir kaynak grubu oluşturun. Bu örnekte, *EastUS* bölgesinde *myResourceGroup* adında bir kaynak grubu oluşturulur. 

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName "myResourceGroup" -Location "EastUS"
```
Bu öğreticide bir ölçek kümesi oluşturduğunuzda veya değiştirdiğinizde kaynak grubu adı belirtilir.


## <a name="create-a-scale-set"></a>Ölçek kümesi oluşturma
İlk olarak, VM örnekleri için [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile bir yönetici kullanıcı adı ve parola ayarlayın:

```azurepowershell-interactive
$cred = Get-Credential
```

Şimdi [Yeni-AzVmss](/powershell/module/az.compute/new-azvmss)ile ayarlanmış sanal bir makine ölçeği oluşturun. Her bir sanal makine örneklerine trafiği dağıtmak için bir yük dengeleyici de oluşturulur. Yük dengeleyici hem TCP bağlantı noktası 80 üzerinden trafiği dağıtmak hem de TCP bağlantı noktası 3389 üzerinden uzak masaüstü trafiğine hem de TCP bağlantı noktası 5985 üzerinden PowerShell uzaktan iletişimine olanak tanımak için kurallar içerir:

```azurepowershell-interactive
New-AzVmss `
  -ResourceGroupName "myResourceGroup" `
  -VMScaleSetName "myScaleSet" `
  -Location "EastUS" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -Credential $cred
```

Tüm ölçek kümesi kaynaklarının ve sanal makine örneklerinin oluşturulup yapılandırılması birkaç dakika sürer.


## <a name="view-the-vm-instances-in-a-scale-set"></a>Bir ölçek kümesindeki sanal makine örneklerini görüntüleme
Bir ölçek kümesindeki VM örneklerinin listesini görüntülemek için [Get-AzVmssVM'yi](/powershell/module/az.compute/get-azvmssvm) aşağıdaki gibi kullanın:

```azurepowershell-interactive
Get-AzVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```

Aşağıdaki örnek çıktı, ölçek kümesindeki iki sanal makine örneğini göstermektedir:

```powershell
ResourceGroupName         Name Location             Sku InstanceID ProvisioningState
-----------------         ---- --------             --- ---------- -----------------
MYRESOURCEGROUP   myScaleSet_0   eastus Standard_DS1_v2          0         Succeeded
MYRESOURCEGROUP   myScaleSet_1   eastus Standard_DS1_v2          1         Succeeded
```

Belirli bir VM örneği hakkında ek `-InstanceId` bilgileri görüntülemek için [Get-AzVmssVM](/powershell/module/az.compute/get-azvmssvm)parametresini ekleyin. Aşağıdaki örnekte, *1* sanal makine örneğiyle ilgili bilgiler görüntülenmektedir:

```azurepowershell-interactive
Get-AzVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "1"
```


## <a name="list-connection-information"></a>Bağlantı bilgilerini listeleme
Tek tek sanal makine örneklerine trafiği yönlendiren yük dengeleyiciye genel bir IP adresi atanır. Varsayılan olarak, belirtilen bir bağlantı noktasındaki her bir sanal makineye uzaktan bağlantı trafiğini ileten Azure Load Balancer’a Ağ Adresi Çevirisi (NAT) kuralları eklenir. Bir ölçek kümesindeki sanal makine örneklerine bağlanmak için, atanan bir genel IP adresine ve bağlantı noktası numarasına uzaktan bağlantı oluşturursunuz.

VM örneklerine bir ölçek kümesinde bağlanmak için NAT bağlantı noktalarını listelemek için, önce Yük dengeleyici nesnesini [Get-AzLoadBalancer](/powershell/module/az.network/Get-AzLoadBalancer)ile alın. Daha sonra [Get-AzLoadBalancerInboundNatRuleConfig](/powershell/module/az.network/Get-AzLoadBalancerInboundNatRuleConfig)ile gelen NAT kurallarını görüntüleyin:


```azurepowershell-interactive
# Get the load balancer object
$lb = Get-AzLoadBalancer -ResourceGroupName "myResourceGroup" -Name "myLoadBalancer"

# View the list of inbound NAT rules
Get-AzLoadBalancerInboundNatRuleConfig -LoadBalancer $lb | Select-Object Name,Protocol,FrontEndPort,BackEndPort
```

Aşağıdaki örnek çıktıda, yük dengeleyicinin genel IP adresi, örnek adı ve NAT kurallarının trafiği ilettiği bağlantı noktası numarası gösterilmektedir:

```powershell
Name             Protocol FrontendPort BackendPort
----             -------- ------------ -----------
myScaleSet3389.0 Tcp             50001        3389
myScaleSet5985.0 Tcp             51001        5985
myScaleSet3389.1 Tcp             50002        3389
myScaleSet5985.1 Tcp             51002        5985
```

Kuralın *adı,* önceki [Get-AzVmssVM](/powershell/module/az.compute/get-azvmssvm) komutunda gösterildiği gibi VM örneğinin adı ile hizalanır. Örneğin, sanal makine örneği *0*'a bağlanmak için *myScaleSet3389.0* kullanır ve *50001* numaralı bağlantı noktasına bağlanırsınız. Sanal makine örneği *1*'e bağlanmak için, *myScaleSet3389.1*'den gelen değeri kullanın ve *50002* numaralı bağlantı noktasına bağlanın. PowerShell uzaktan iletişimini kullanmak için, *TCP* bağlantı noktası *5985* için uygun sanal makine örneği kuralına bağlanırsınız.

[Get-AzPublicIpAddress](/powershell/module/az.network/Get-AzPublicIpAddress)ile yük bakiyesinin genel IP adresini görüntüleyin :


```azurepowershell-interactive
Get-AzPublicIpAddress -ResourceGroupName "myResourceGroup" -Name "myPublicIPAddress" | Select IpAddress
```

Örnek çıktı:

```powershell
IpAddress
---------
52.168.121.216
```

İlk sanal makine örneğinize bir uzak bağlantı oluşturun. Önceki komutlarda gösterildiği gibi, gerekli sanal makine örneği için genel IP adresinizi ve bağlantı noktası numaranızı belirtin. İstendiğinde, ölçek kümesini oluşturduğunuzda kullanılan kimlik bilgilerini girin (varsayılan olarak örnek komutlarda, *azureuser* ve *P\@ssw0rd!*). Azure Cloud Shell kullanıyorsanız, bu adımı yerel PowerShell isteminden veya Uzak Masaüstü İstemcisinden gerçekleştirin. Aşağıdaki örnek sanal makine örneği *1*'e bağlanır:

```powershell
mstsc /v 52.168.121.216:50001
```

Sanal makine örneğinde oturum açtıktan sonra, gerektiğinde bazı el ile yapılandırma değişiklikleri gerçekleştirebilirsiniz. Şimdilik, uzak bağlantıyı kapatın.


## <a name="understand-vm-instance-images"></a>Sanal makine örneği görüntülerini anlama
Azure marketi, sanal makine örnekleri oluşturmak için kullanılabilecek çok sayıda görüntü içerir. Kullanılabilir yayıncıların listesini görmek için [Get-AzVMImagePublisher](/powershell/module/az.compute/get-azvmimagepublisher) komutunu kullanın.

```azurepowershell-interactive
Get-AzVMImagePublisher -Location "EastUS"
```

Belirli bir yayımcının resim listesini görüntülemek için [Get-AzVMImageSku'yı](/powershell/module/az.compute/get-azvmimagesku)kullanın. Görüntü listesi `-PublisherName` veya `-Offer` kullanılarak da filtrelenebilir. Aşağıdaki örnekte liste, yayımcı adı *MicrosoftWindowsServer* olan ve *WindowsServer* ile eşleşen bir teklifi bulunan tüm görüntüler için filtrelenmiştir:

```azurepowershell-interactive
Get-AzVMImageSku -Location "EastUS" -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer"
```

Aşağıdaki örneğin çıkışı, tüm kullanılabilir Windows Server görüntülerini gösterir:

```powershell
Skus                                  Offer         PublisherName          Location
----                                  -----         -------------          --------
2008-R2-SP1                           WindowsServer MicrosoftWindowsServer eastus
2008-R2-SP1-smalldisk                 WindowsServer MicrosoftWindowsServer eastus
2012-Datacenter                       WindowsServer MicrosoftWindowsServer eastus
2012-Datacenter-smalldisk             WindowsServer MicrosoftWindowsServer eastus
2012-R2-Datacenter                    WindowsServer MicrosoftWindowsServer eastus
2012-R2-Datacenter-smalldisk          WindowsServer MicrosoftWindowsServer eastus
2016-Datacenter                       WindowsServer MicrosoftWindowsServer eastus
2016-Datacenter-Server-Core           WindowsServer MicrosoftWindowsServer eastus
2016-Datacenter-Server-Core-smalldisk WindowsServer MicrosoftWindowsServer eastus
2016-Datacenter-smalldisk             WindowsServer MicrosoftWindowsServer eastus
2016-Datacenter-with-Containers       WindowsServer MicrosoftWindowsServer eastus
2016-Datacenter-with-RDSH             WindowsServer MicrosoftWindowsServer eastus
2016-Nano-Server                      WindowsServer MicrosoftWindowsServer eastus
```

Öğreticinin başında bir ölçek kümesi oluşturduğunuzda, sanal makine örnekleri için varsayılan bir *Windows Server 2016 DataCenter* sanal makine görüntüsü sağlanmıştır. [Get-AzVMImageSku](/powershell/module/az.compute/get-azvmimagesku)çıktıdayalı farklı bir VM görüntü belirtebilirsiniz. Aşağıdaki örnek, *MicrosoftWindowsServer:WindowsServer:2016-Datacenter-with-Containers:latest* sanal makine görüntüsünü belirtmek için `-ImageName` parametresiyle bir ölçek kümesi oluşturabilir. Tüm ölçek kümesi kaynaklarının ve sanal makine örneklerinin oluşturulup yapılandırılması birkaç dakika süreceğinden, aşağıdaki ölçek kümesini dağıtmanız gerekmez:

```azurepowershell-interactive
New-AzVmss `
  -ResourceGroupName "myResourceGroup2" `
  -Location "EastUS" `
  -VMScaleSetName "myScaleSet2" `
  -VirtualNetworkName "myVnet2" `
  -SubnetName "mySubnet2" `
  -PublicIpAddressName "myPublicIPAddress2" `
  -LoadBalancerName "myLoadBalancer2" `
  -UpgradePolicyMode "Automatic" `
  -ImageName "MicrosoftWindowsServer:WindowsServer:2016-Datacenter-with-Containers:latest" `
  -Credential $cred
```


## <a name="understand-vm-instance-sizes"></a>Sanal makine örneği boyutlarını anlama
Sanal makine örneğinin boyutu veya *SKU*, sanal makine örneği tarafından kullanılabilen CPU, GPU ve bellek gibi işlem kaynaklarının miktarını belirler. Ölçek kümesindeki sanal makine örneklerinin beklenen iş yüküne uygun olarak boyutlandırılması gerekir.

### <a name="vm-instance-sizes"></a>Sanal makine örneği boyutları
Aşağıdaki tabloda genel sanal makine boyutları, kullanım durumlarına göre kategorilere ayrılmıştır.

| Tür                     | Ortak boyutlar           |    Açıklama       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Genel amaç](../virtual-machines/windows/sizes-general.md)         |Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7| Dengeli CPU/bellek. Küçük ve orta ölçekli uygulama ve veri çözümlerini geliştirmek/test etmek için idealdir.  |
| [İşlem için iyileştirilmiş](../virtual-machines/windows/sizes-compute.md)   | Fs, F             | Yüksek CPU/bellek. Orta düzey trafiğe sahip uygulamalar, ağ gereçleri ve toplu işlemler için idealdir.        |
| [Bellek için iyileştirilmiş](../virtual-machines/windows/sizes-memory.md)    | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Yüksek bellek/çekirdek. İlişkisel veritabanı, orta veya büyük boyutlu önbellekler ve bellek içi analiz için idealdir.                 |
| [Depolama için iyileştirilmiş](../virtual-machines/windows/sizes-storage.md)      | Ls                | Yüksek disk aktarım hızı ve GÇ. Büyük Veri, SQL ve NoSQL veritabanları için ideal.                                                         |
| [GPU](../virtual-machines/windows/sizes-gpu.md)          | NV, NC            | Ağır grafik işlemleri ile video düzenleme işlemleri için özel olarak hedeflenen VM’ler.       |
| [Yüksek performans](../virtual-machines/windows/sizes-hpc.md) | H, A8-11          | İşleme düzeyi yüksek olan isteğe bağlı ağ arabirimleri (RDMA) içeren VM’lerimiz, şimdiye kadarki en güçlü CPU ile sunuluyor. 

### <a name="find-available-vm-instance-sizes"></a>Kullanılabilir sanal makine örneği boyutlarını bulma
Belirli bir bölgede kullanılabilen VM örnek boyutlarının listesini görmek için [Get-AzVMSize](/powershell/module/az.compute/get-azvmsize) komutunu kullanın. 

```azurepowershell-interactive
Get-AzVMSize -Location "EastUS"
```

Çıktı, her sanal makine boyutuna atanan kaynakları gösteren, aşağıdaki sıkıştırılmış örneğe benzer:

```powershell
Name                   NumberOfCores MemoryInMB MaxDataDiskCount OSDiskSizeInMB ResourceDiskSizeInMB
----                   ------------- ---------- ---------------- -------------- --------------------
Standard_DS1_v2                    1       3584                4        1047552                 7168
Standard_DS2_v2                    2       7168                8        1047552                14336
[...]
Standard_A0                        1        768                1        1047552                20480
Standard_A1                        1       1792                2        1047552                71680
[...]
Standard_F1                        1       2048                4        1047552                16384
Standard_F2                        2       4096                8        1047552                32768
[...]
Standard_NV6                       6      57344               24        1047552               389120
Standard_NV12                     12     114688               48        1047552               696320
```

Öğreticinin başında bir ölçek kümesi oluşturduğunuzda, sanal makine örnekleri için varsayılan bir *Standard_DS1_v2* sanal makine SKU’su sağlanmıştır. [Get-AzVMSize'dan](/powershell/module/az.compute/get-azvmsize)çıkan çıktıyı temel alan farklı bir VM örnek boyutu belirtebilirsiniz. Aşağıdaki örnek, *Standard_F1* bir sanal makine örneği boyutu belirtmek için `-VmSize` parametresiyle bir ölçek kümesi oluşturur. Tüm ölçek kümesi kaynaklarının ve sanal makine örneklerinin oluşturulup yapılandırılması birkaç dakika süreceğinden, aşağıdaki ölçek kümesini dağıtmanız gerekmez:

```azurepowershell-interactive
New-AzVmss `
  -ResourceGroupName "myResourceGroup3" `
  -Location "EastUS" `
  -VMScaleSetName "myScaleSet3" `
  -VirtualNetworkName "myVnet3" `
  -SubnetName "mySubnet3" `
  -PublicIpAddressName "myPublicIPAddress3" `
  -LoadBalancerName "myLoadBalancer3" `
  -UpgradePolicyMode "Automatic" `
  -VmSize "Standard_F1" `
  -Credential $cred
```


## <a name="change-the-capacity-of-a-scale-set"></a>Ölçek kümesinin kapasitesini değiştirme
Ölçek kümesi oluşturduğunuzda iki sanal makine örneği istediniz. Ölçek kümesindeki sanal makine örneği sayısını artırmak veya azaltmak için kapasiteyi el ile değiştirebilirsiniz. Ölçek kümesi, gerektiği sayıda sanal makine örneği oluşturur veya kaldırır ve sonra trafiği dağıtmak için yük dengeleyiciyi yapılandırır.

İlk olarak, [Get-AzVmss](/powershell/module/az.compute/get-azvmss)ile bir ölçek kümesi nesnesi oluşturmak, sonra için `sku.capacity`yeni bir değer belirtin. Kapasite değişikliğini uygulamak için [Update-AzVmss'i](/powershell/module/az.compute/update-azvmss)kullanın. Aşağıdaki örnek, ölçek kümenizdeki sanal makine sayısını *3* olarak ayarlar:

```azurepowershell-interactive
# Get current scale set
$vmss = Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"

# Set and update the capacity of your scale set
$vmss.sku.capacity = 3
Update-AzVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet" -VirtualMachineScaleSet $vmss 
```

Ölçek kümenizin kapasitesinin güncelleştirilmesi birkaç dakika sürer. Şimdi ölçek kümesinde bulunan örnek sayısını görmek için [Get-AzVmss](/powershell/module/az.compute/get-azvmss)kullanın:

```azurepowershell-interactive
Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```

Aşağıdaki örnek çıkış, ölçek kümesi kapasitesinin şimdi *3* olduğunu gösterir:

```powershell
Sku        :
  Name     : Standard_DS2
  Tier     : Standard
  Capacity : 3
```


## <a name="common-management-tasks"></a>Genel yönetim görevleri
Artık bir ölçek kümesi oluşturabilir, bağlantı bilgilerini listeleyebilir ve sanal makine örneklerine bağlanabilirsiniz. Sanal makine örnekleriniz için farklı bir OS görüntüsünü nasıl kullanabileceğinizi veya örnek sayısını nasıl el ile ölçeklendirebileceğinizi öğrendiniz. Günlük yönetim işlemleriniz kapsamında, ölçek kümenizdeki sanal makine örneklerini durdurmanız, başlatmanız veya yeniden başlatmanız gerekebilir.

### <a name="stop-and-deallocate-vm-instances-in-a-scale-set"></a>Bir ölçek kümesindeki sanal makine örneklerini durdurma ve serbest bırakma
Bir ölçek kümesinde bir veya daha fazla VM'yi durdurmak için [Stop-AzVmss](/powershell/module/az.compute/stop-azvmss)kullanın. `-InstanceId` parametresi, durdurulacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler durdurulur. Aşağıdaki örnek, *1* örneğini durdurur:

```azurepowershell-interactive
Stop-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "1"
```

Varsayılan olarak, durdurulan sanal makineler serbest bırakılır ve bunlar için işlem ücreti alınmaz. Durdurulan sanal makinenin sağlama durumunda kalmasını istiyorsanız, önceki komuta `-StayProvisioned` parametresini ekleyin. Sağlama durumunda tutulan durdurulmuş sanal makineler için normal işlem ücreti alınır.

### <a name="start-vm-instances-in-a-scale-set"></a>Bir ölçek kümesindeki sanal makine örneklerini başlatma
Bir ölçek kümesinde bir veya daha fazla VM başlatmak için [Başlat-AzVmss'i](/powershell/module/az.compute/start-azvmss)kullanın. `-InstanceId` parametresi, başlatılacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler başlatılır. Aşağıdaki örnek, *1* örneğini başlatır:

```azurepowershell-interactive
Start-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "1"
```

### <a name="restart-vm-instances-in-a-scale-set"></a>Bir ölçek kümesindeki sanal makine örneklerini yeniden başlatma
Bir ölçek kümesinde bir veya daha fazla VM'yi yeniden başlatmak için [Retart-AzVmss](/powershell/module/az.compute/restart-azvmss)kullanın. `-InstanceId` parametresi, yeniden başlatılacak bir veya daha fazla sanal makine belirtmenize olanak sağlar. Örnek kimliği belirtmezseniz, ölçek kümesindeki tüm sanal makineler yeniden başlatılır. Aşağıdaki örnek, *1* örneğini yeniden başlatır:

```azurepowershell-interactive
Restart-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "1"
```


## <a name="clean-up-resources"></a>Kaynakları temizleme
Bir kaynak grubunu sildiğinizde, o kaynak grubunun içindeki sanal makine örnekleri, sanal ağ ve diskler gibi tüm kaynaklar da silinir. `-Force` parametresi kaynakları ek bir komut istemi olmadan silmek istediğinizi onaylar. `-AsJob` parametresi işlemin tamamlanmasını beklemeden denetimi komut istemine döndürür.

```azurepowershell-interactive
Remove-AzResourceGroup -Name "myResourceGroup" -Force -AsJob
```


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure PowerShell ile bazı temel ölçek kümesi oluşturma ve yönetme görevlerinin nasıl gerçekleştirileceğini öğrendiniz:

> [!div class="checklist"]
> * Sanal makine ölçek kümesi oluşturma ve sanal makine ölçek kümesine bağlanma
> * VM görüntülerini seçme ve kullanma
> * Belirli VM boyutlarını görüntüleme ve kullanma
> * Ölçek kümesini el ile ölçeklendirme
> * Genel ölçek kümesi yönetim görevlerini gerçekleştirme

Ölçek kümesi diskleri hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Veri disklerini ölçek kümeleri ile kullanma](tutorial-use-disks-powershell.md)
