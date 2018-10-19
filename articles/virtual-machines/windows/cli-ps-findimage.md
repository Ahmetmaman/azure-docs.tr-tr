---
title: Azure'da Windows VM görüntülerini seçme | Microsoft Docs
description: Yayımcı, teklif, SKU ve sürümü için Market VM görüntülerini belirlemek için Azure PowerShell kullanırsınız.
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 188b8974-fabd-4cd3-b7dc-559cbb86b98a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/08/2018
ms.author: danlep
ms.openlocfilehash: 5934e955d2a18d111c625670bced134df37ef045
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49409603"
---
# <a name="find-windows-vm-images-in-the-azure-marketplace-with-azure-powershell"></a>Azure Marketi'nde Azure PowerShell ile Windows VM görüntüleri bulma

Bu makalede, sanal makine görüntüleri Azure Market'te bulmak için Azure PowerShell kullanmayı açıklar. Ardından bir VM PowerShell ile programlı olarak oluşturduğunuzda bir Market görüntüsü belirtebilirsiniz Resource Manager şablonları ya da başka araçlar.

Kullanılabilir görüntüleri ve teklifler kullanarak göz atabilirsiniz [Azure Marketi](https://azuremarketplace.microsoft.com/) vitrini, [Azure portalında](https://portal.azure.com), veya [Azure CLI](../linux/cli-ps-findimage.md). 

Sizin yüklü ve en son yapılandırıldığından emin olun [Azure PowerShell Modülü](/powershell/azure/install-azurerm-ps).

[!INCLUDE [virtual-machines-common-image-terms](../../../includes/virtual-machines-common-image-terms.md)]

## <a name="table-of-commonly-used-windows-images"></a>Yaygın olarak kullanılan Windows görüntüleri tablosu
| Yayımcı | Sunduğu | Sku |
|:--- |:--- |:--- |:--- |
| MicrosoftWindowsServer |WindowsServer |2016-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2016-Datacenter-Server-Core |
| MicrosoftWindowsServer |WindowsServer |Kapsayıcılar ile 2016 Datacenter |
| MicrosoftWindowsServer |WindowsServer |2012-R2-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2012-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2008-R2-SP1 |
| MicrosoftDynamicsNAV |DynamicsNAV |2017 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2016 |
| MicrosoftSQLServer |SQL2017 WS2016 |Enterprise |
| MicrosoftRServer |RServer WS2016 |Enterprise |

## <a name="navigate-the-images"></a>Görüntüleri gidin

Görüntüyü bir konumda çalıştırılacak bulmak için tek yönlü [Get-Azurermvmımagepublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-Azurermvmımageoffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), ve [Get-Azurermvmımagesku](/powershell/module/azurerm.compute/get-azurermvmimagesku) sırayla cmdlet'leri:

1. Görüntü yayımcılarını listeleyin.
2. Belirli bir yayımcı varsa yayımcının tekliflerini listeleyin.
3. Belirli bir teklif varsa SKU’larını listeleyin.

Ardından, seçilen SKU için çalıştırın [Get-AzureRMVMImage](/powershell/module/azurerm.compute/get-azurermvmimage) dağıtmak için sürümleri listesi.

1. Yayımcıları listeleyin:

    ```powershell
    $locName="<Azure location, such as West US>"
    Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
    ```

2. Seçtiğiniz yayımcı adını girin ve tekliflerini listeleyin:

    ```powershell
    $pubName="<publisher>"
    Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
    ```

3. Seçtiğiniz teklif adınızı girin ve SKU'ları listeler:

    ```powershell
    $offerName="<offer>"
    Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
    ```
    
4. Seçilen SKU adınızı girin ve görüntüsü sürümü alma:

    ```powershell
    $skuName="<SKU>"
    Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Sku $skuName | Select Version
    ```
    
Çıktısından `Get-AzureRMVMImage` komutu, yeni bir sanal makine dağıtmak için bir sürümü görüntüsü seçebilirsiniz.

Aşağıdaki örnek komutlar ve bunların çıkışları tam dizisini gösterir:

```powershell
$locName="West US"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

Kısmi çıkış:

```
PublisherName
-------------
...
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

İçin *MicrosoftWindowsServer* yayımcı:

```powershell
$pubName="MicrosoftWindowsServer"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Çıktı:

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServerSemiAnnual
```

İçin *WindowsServer* sunar:

```powershell
$offerName="WindowsServer"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Çıktı:

```
Skus
----
2008-R2-SP1
2008-R2-SP1-smalldisk
2012-Datacenter
2012-Datacenter-smalldisk
2012-R2-Datacenter
2012-R2-Datacenter-smalldisk
2016-Datacenter
2016-Datacenter-Server-Core
2016-Datacenter-Server-Core-smalldisk
2016-Datacenter-smalldisk
2016-Datacenter-with-Containers
2016-Datacenter-with-RDSH
```

Sonra *2016-Datacenter* SKU:

```powershell
$skuName="2016-Datacenter"
Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Sku $skuName | Select Version
```

Bir URN içinde seçilen yayımcı, teklif, SKU ve sürüm birleştirebilirsiniz artık (virgülle ayrılmış değerler tarafından:). Bu URN ile geçirin `--image` ile bir VM oluşturduğunuzda, parametre [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) cmdlet'i. İsteğe bağlı olarak, görüntünün en son sürümünü almak için "son" URN sürüm numarasını değiştirebilirsiniz.

Resource Manager şablonu ile VM dağıtma sonra görüntüyü parametreleri tek tek de ayarlarsınız `imageReference` özellikleri. Bkz: [şablon başvurusu](/azure/templates/microsoft.compute/virtualmachines).

[!INCLUDE [virtual-machines-common-marketplace-plan](../../../includes/virtual-machines-common-marketplace-plan.md)]

### <a name="view-plan-properties"></a>Plan özelliklerini görüntüleme

Görüntünün satın alma planı bilgilerini görüntülemek için çalıştırın `Get-AzureRMVMImage` cmdlet'i. Varsa `PurchasePlan` çıktıda özelliği değil `null`, koşulları resimle önce programlamalı dağıtım'ı kabul etmeniz gerekir.  

Örneğin, *Windows Server 2016 Datacenter* görüntü ek koşullar, yoksa böylece `PurchasePlan` bilgileri `null`:

```powershell
$version = "2016.127.20170406"
Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Skus $skuName -Version $version
```

Çıktı:

```
Id               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/
                   Versions/2016.127.20170406
Location         : westus
PublisherName    : MicrosoftWindowsServer
Offer            : WindowsServer
Skus             : 2016-Datacenter
Version          : 2016.127.20170406
FilterExpression :
Name             : 2016.127.20170406
OSDiskImage      : {
                     "operatingSystem": "Windows"
                   }
PurchasePlan     : null
DataDiskImages   : []

```

Aşağıdaki örnek, benzer bir komut için gösterir *veri bilimi sanal makinesi - Windows 2016* aşağıdaki olan görüntü `PurchasePlan` özellikleri: `name`, `product`, ve `publisher`. Bazı görüntüleri de bir `promotion code` özelliği. Bu görüntü dağıtmak için koşulları kabul etmek ve programlamalı dağıtımını etkinleştirmek için aşağıdaki bölümlere bakın.

```powershell
Get-AzureRMVMImage -Location "westus" -Publisher "microsoft-ads" -Offer "windows-data-science-vm" -Skus "windows2016" -Version "0.2.02"
```

Çıktı:

```
Id               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/microsoft-ads/ArtifactTypes/VMIma
                   ge/Offers/windows-data-science-vm/Skus/windows2016/Versions/0.2.02
Location         : westus
PublisherName    : microsoft-ads
Offer            : windows-data-science-vm
Skus             : windows2016
Version          : 0.2.02
FilterExpression :
Name             : 0.2.02
OSDiskImage      : {
                     "operatingSystem": "Windows"
                   }
PurchasePlan     : {
                     "publisher": "microsoft-ads",
                     "name": "windows2016",
                     "product": "windows-data-science-vm"
                   }
DataDiskImages   : []

```

### <a name="accept-the-terms"></a>Koşulları kabul et

Lisans koşullarını görüntülemek için kullanın [Get-AzureRmMarketplaceterms](/powershell/module/azurerm.marketplaceordering/get-azurermmarketplaceterms) cmdlet'i ve geçişinde satın alma planı parametreleri. Çıkış koşullarını Market görüntüsü için bir bağlantı sağlar ve daha önce koşulları kabul olup olmadığını gösterir. Parametre değerlerini tamamen küçük harf kullandığınızdan emin olun.

```powershell
Get-AzureRmMarketplaceterms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016"
```

Çıktı:

```
Publisher         : microsoft-ads
Product           : windows-data-science-vm
Plan              : windows2016
LicenseTextLink   : https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_MICROSOFT%253a2DADS%253a24WINDOWS%253a2DDATA%253a2DSCIENCE%253a2DV
                    M%253a24WINDOWS2016%253a24OC5SKMQOXSED66BBSNTF4XRCS4XLOHP7QMPV54DQU7JCBZWYFP35IDPOWTUKXUC7ZAG7W6ZMDD6NHWNKUIVSYBZUTZ245F44SU5AD7Q.txt
PrivacyPolicyLink : https://www.microsoft.com/EN-US/privacystatement/OnlineServices/Default.aspx
Signature         : 2UMWH6PHSAIM4U22HXPXW25AL2NHUJ7Y7GRV27EBL6SUIDURGMYG6IIDO3P47FFIBBDFHZHSQTR7PNK6VIIRYJRQ3WXSE6BTNUNENXA
Accepted          : False
Signdate          : 2/23/2018 7:43:00 PM
```

Kullanım [kümesi AzureRmMarketplaceterms](/powershell/module/azurerm.marketplaceordering/set-azurermmarketplaceterms) cmdlet'i kabul edin veya koşulları reddetmek için. Görüntü için abonelik başına bir kez kabul etmek yeterlidir. Parametre değerlerini tamamen küçük harf kullandığınızdan emin olun. 

```powershell
$agreementTerms=Get-AzureRmMarketplaceterms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016"

Set-AzureRmMarketplaceTerms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016" -Terms $agreementTerms -Accept
```

Çıktı:

```
Publisher         : microsoft-ads
Product           : windows-data-science-vm
Plan              : windows2016
LicenseTextLink   : https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_MICROSOFT%253a2DADS%253a24WINDOWS%253a2DDATA%253a2DSCIENCE%253a2DV
                    M%253a24WINDOWS2016%253a24OC5SKMQOXSED66BBSNTF4XRCS4XLOHP7QMPV54DQU7JCBZWYFP35IDPOWTUKXUC7ZAG7W6ZMDD6NHWNKUIVSYBZUTZ245F44SU5AD7Q.txt
PrivacyPolicyLink : https://www.microsoft.com/EN-US/privacystatement/OnlineServices/Default.aspx
Signature         : XXXXXXK3MNJ5SROEG2BYDA2YGECU33GXTD3UFPLPC4BAVKAUL3PDYL3KBKBLG4ZCDJZVNSA7KJWTGMDSYDD6KRLV3LV274DLBXXXXXX
Accepted          : True
Signdate          : 2/23/2018 7:49:31 PM
```

### <a name="deploy-using-purchase-plan-parameters"></a>Satın alma planı parametreleri kullanarak dağıtma

Bir görüntü için koşulları kabul ettikten sonra bu Abonelikteki bir VM dağıtabilirsiniz. Aşağıdaki kod parçacığında gösterildiği gibi kullanın [kümesi AzureRmVMPlan](/powershell/module/azurerm.compute/set-azurermvmplan) cmdlet'i, VM nesnesinin Market plan bilgisini ayarlamak için. VM için ağ ayarları oluşturma ve dağıtımını tamamlamak tam betik için bkz [PowerShell betik örnekleri](powershell-samples.md).

```powershell
...

$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize Standard_D1

# Set the Marketplace plan information

$publisherName = "microsoft-ads"

$productName = "windows-data-science-vm"

$planName = "windows2016"

$vmConfig = Set-AzureRmVMPlan -VM $vmConfig -Publisher $publisherName -Product $productName -Name $planName

$cred=Get-Credential

$vmConfig = Set-AzureRmVMOperatingSystem -Windows -VM $vmConfig -ComputerName "myVM" -Credential $cred

# Set the Marketplace image

$offerName = "windows-data-science-vm"

$skuName = "windows2016"

$version = "0.2.02"

$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisherName -Offer $offerName -Skus $skuName -Version $version
...
```
VM yapılandırması için ağ yapılandırma nesneleri ile birlikte ardından ileteceksiniz `New-AzureRmVM` cmdlet'i.

## <a name="next-steps"></a>Sonraki adımlar
Bir sanal makineyi hızlıca oluşturmak için `New-AzureRmVM` temel görüntü bilgileri kullanarak cmdlet'i bkz [PowerShell ile bir Windows sanal makinesi oluşturma](quick-create-powershell.md).

Bir PowerShell Betiği örneği için bkz. [tam olarak yapılandırılmış bir sanal makine oluşturma](../scripts/virtual-machines-windows-powershell-sample-create-vm.md).


