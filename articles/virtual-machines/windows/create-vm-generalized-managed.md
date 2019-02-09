---
title: Azure'da yönetilen bir görüntüden VM oluşturma | Microsoft Docs
description: Genelleştirilmiş bir yönetilen görüntüsü Resource Manager dağıtım modelinde Azure PowerShell veya Azure portalını kullanarak bir Windows sanal makine oluşturun.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/17/2018
ms.author: cynthn
ms.openlocfilehash: 9157765afaa610d207a47e19b73f80ae3898fd68
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2019
ms.locfileid: "55977567"
---
# <a name="create-a-vm-from-a-managed-image"></a>Yönetilen bir görüntüden VM oluşturma

Birden çok sanal makine (VM) yönetilen bir Azure VM'den oluşturabilirsiniz Azure portal veya PowerShell kullanarak görüntü. Yönetilen bir VM görüntüsü işletim sistemi ve veri diskleri dahil olmak üzere, bir VM oluşturmak gerekli bilgileri içerir. Sanal sabit hem işletim sistemi diskleri ve tüm veri diskleri dahil olmak üzere, görüntüyü oluşturan diskleri (VHD'ler), yönetilen diskler olarak depolanır. 

Yeni bir sanal makine oluşturmadan önce şunları yapmanız gerekir [yönetilen bir VM görüntüsü oluşturma](capture-image-resource.md) kaynak görüntü olarak kullanılacak. 


## <a name="use-the-portal"></a>Portalı kullanma

1. [Azure portalı](https://portal.azure.com) açın.
2. Sol menüden **tüm kaynakları**. Kaynaklara göre sıralayabilirsiniz **türü** görüntülerinizi kolayca bulmak için.
3. Listeden kullanmak istediğiniz görüntüyü seçin. Görüntü **genel bakış** sayfası açılır.
4. Seçin **VM Oluştur** menüsünde.
5. Sanal makine bilgilerini girin. Burada girilen parola ve kullanıcı adı, sanal makineye oturum açmak için kullanılır. İşlem tamamlandığında seçin **Tamam**. Mevcut bir kaynak grubunda yeni bir VM oluşturun veya seçin **Yeni Oluştur** VM depolamak için yeni bir kaynak grubu oluşturmak için.
6. VM için bir boyut seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. 
7. Altında **ayarları**seçin ve gerekli değişiklikleri yapmak **Tamam**. 
8. Özet sayfasında, görüntü adınız olarak listelenen görmelisiniz bir **özel görüntü**. Seçin **Tamam** sanal makine dağıtımını başlatın.


## <a name="use-powershell"></a>PowerShell kullanma

Basitleştirilmiş parametre kümesi kullanarak görüntüden bir VM oluşturmak için PowerShell kullanabilirsiniz [New-AzVm](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) cmdlet'i. Görüntü, burada VM oluşturacağınız aynı kaynak grubunda olması gerekiyor.

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

İçin Basitleştirilmiş parametre kümesi [New-AzVm](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) yalnızca bir görüntüden bir VM oluşturmak için ad, kaynak grubu ve görüntü adı sağlamanız gerekir. Yeni-AzVm değerini kullanacağınız **-adı** parametre adı olarak tüm kaynakları otomatik olarak oluşturur. Bu örnekte, kaynakların her biri için ayrıntılı adlar sağlar ancak onları otomatik olarak oluşturmasını cmdlet'i sağlar. Ayrıca, kaynak sanal ağ gibi önceden oluşturabilir ve kaynak adı cmdlet'e geçirin. Yeni-AzVm adlarına göre bunları bulabilirsiniz varolan kaynakları kullanır.

Aşağıdaki örnekte adlı bir VM oluşturur *Myımage*, *myResourceGroup* kaynak grubundan görüntüsünü *Myımage*. 


```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVMfromImage" `
    -ImageName "myImage" `
    -Location "East US" `
    -VirtualNetworkName "myImageVnet" `
    -SubnetName "myImageSubnet" `
    -SecurityGroupName "myImageNSG" `
    -PublicIpAddressName "myImagePIP" `
    -OpenPorts 3389
```



## <a name="next-steps"></a>Sonraki adımlar
[Oluşturma ve Azure PowerShell modülü ile Windows Vm'leri yönetme](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

