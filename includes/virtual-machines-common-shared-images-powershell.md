---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/18/2020
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 807cbd283cf7971bf4256451028ffa16a0911266
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95553577"
---
## <a name="create-an-image-gallery"></a>Görüntü galerisi oluşturma 

Görüntü Galerisi, görüntü paylaşımını etkinleştirmek için kullanılan birincil kaynaktır. Galeri adı için izin verilen karakterler büyük veya küçük harflerden, rakamlardan, noktalardan ve noktalardan oluşur. Galeri adı tire içeremez. Galeri adları, aboneliğiniz dahilinde benzersiz olmalıdır. 

[New-AzGallery](/powershell/module/az.compute/new-azgallery)kullanarak bir görüntü galerisi oluşturun. Aşağıdaki örnek *Mygallerrg* kaynak grubunda *MyGallery* adlı bir galeri oluşturur.

```azurepowershell-interactive
$resourceGroup = New-AzResourceGroup `
   -Name 'myGalleryRG' `
   -Location 'West Central US'  
$gallery = New-AzGallery `
   -GalleryName 'myGallery' `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $resourceGroup.Location `
   -Description 'Shared Image Gallery for my organization'  
```
   

## <a name="share-the-gallery"></a>Galeriyi paylaşma

Görüntü Galerisi düzeyinde erişimi paylaşmanızı öneririz. Kullanıcının nesne KIMLIĞINI almak için bir e-posta adresi ve [Get-AzADUser](/powershell/module/az.resources/get-azaduser) cmdlet 'ini kullanın, ardından galeriye erişim sağlamak için [New-azroleatama](/powershell/module/Az.Resources/New-AzRoleAssignment) kullanın. Örnek e-postayı, alinne_montes@contoso.com Bu örnekte kendi bilgileriniz ile değiştirin.

```azurepowershell-interactive
# Get the object ID for the user
$user = Get-AzADUser -StartsWith alinne_montes@contoso.com
# Grant access to the user for our gallery
New-AzRoleAssignment `
   -ObjectId $user.Id `
   -RoleDefinitionName Reader `
   -ResourceName $gallery.Name `
   -ResourceType Microsoft.Compute/galleries `
   -ResourceGroupName $resourceGroup.ResourceGroupName

```