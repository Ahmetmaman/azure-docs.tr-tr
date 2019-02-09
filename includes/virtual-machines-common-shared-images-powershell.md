---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 12/10/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 8770aaeff3e0d7b2d6a39f596aafebf15ed48b23
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2019
ms.locfileid: "55985012"
---
## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. 

Cloud Shell'i açmak için kod bloğunun sağ üst köşesinden **Deneyin**'i seçmeniz yeterlidir. İsterseniz [https://shell.azure.com/powershell](https://shell.azure.com/powershell) adresine giderek Cloud Shell'i ayrı bir tarayıcı sekmesinde de başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.


## <a name="preview-register-the-feature"></a>Önizleme: Özelliği kaydet

Paylaşılan resim galerileri Önizleme aşamasındadır ancak özelliğini kullanabilmeniz için önce kaydetmeniz gerekir. Paylaşılan resim galerileri özelliği kaydetmek için:

```azurepowershell-interactive
Register-AzProviderFeature `
   -FeatureName GalleryPreview `
   -ProviderNamespace Microsoft.Compute
Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
```

## <a name="get-the-managed-image"></a>Yönetilen bir görüntü al

Kullanarak bir kaynak grubu mevcut görüntülerin listesini görebilirsiniz [Get-AzImage](https://docs.microsoft.com/powershell/module/az.compute/get-azimage). Görüntü adı ve hangi kaynak grubunda öğrendikten sonra olduğundan, kullanabileceğiniz `Get-AzImage` yeniden görüntü nesnesini alın ve daha sonra kullanmak üzere bir değişkende depolayın. Bu örnek adlı bir görüntü alır *Myımage* "myResourceGroup" kaynak grubu ve bir değişkene atar *$managedImage*. 

```azurepowershell-interactive
$managedImage = Get-AzImage `
   -ImageName myImage `
   -ResourceGroupName myResourceGroup
```

## <a name="create-an-image-gallery"></a>Bir görüntü Galerisi oluşturma 

Bir görüntü Galerisine görüntü paylaşımına etkinleştirmek için kullanılan birincil kaynaktır. Galeri adları, abonelik içinde benzersiz olmalıdır. Kullanarak bir görüntü Galerisi oluşturma [yeni AzGallery](https://docs.microsoft.com/powershell/module/az.compute/new-azgallery). Aşağıdaki örnekte adlı bir galeridir oluşturur *myGallery* içinde *myGalleryRG* kaynak grubu.

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
   
## <a name="create-an-image-definition"></a>Bir görüntü tanımı oluşturun 

Galeri görüntüsü kullanarak tanım oluşturma [yeni AzGalleryImageDefinition](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion). Bu örnekte, Galeri görüntüsü adlı *myGalleryImage*.

```azurepowershell-interactive
$galleryImage = New-AzGalleryImageDefinition `
   -GalleryName $gallery.Name `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $gallery.Location `
   -Name 'myImageDefinition' `
   -OsState generalized `
   -OsType Windows `
   -Publisher 'myPublisher' `
   -Offer 'myOffer' `
   -Sku 'mySKU'
```

Gelecek bir sürümde, kişisel olarak tanımlanan kullanmak mümkün olacaktır **-yayımcı**, **-teklif** ve **- Sku** bulmak ve bir görüntü tanımı belirtmek için değerleri daha sonra VM'yi oluşturma eşleşen da görüntü tanımı en son görüntü sürümü kullanıyor. Örneğin, üç görüntü tanımlar ve değerleri şunlardır:

|Görüntü Tanımı|Yayımcı|Sunduğu|Sku|
|---|---|---|---|
|myImage1|myPublisher|myOffer|mySku|
|myImage2|myPublisher|standardOffer|mySku|
|myImage3|Test Etme|standardOffer|testSku|

Bu üç benzersiz değerler vardır. Gelecek sürümlerden birinde, belirli bir görüntünün en son sürümünü isteğinde bulunmak için bu değerleri birleştirmek mümkün olacaktır. 

```powershell
# The following should set the source image as myImage1 from the table above
$vmConfig = Set-AzVMSourceImage `
   -VM $vmConfig `
   -PublisherName myPublisher `
   -Offer myOffer `
   -Skus mySku 
```

Bu nasıl şu anda bu belirtebilirsiniz için benzer [Azure Market görüntüleri](../articles/virtual-machines/windows/cli-ps-findimage.md) bir VM oluşturmak için. Bunu aklınızda her görüntü tanımı bu değerler benzersiz bir dizi olmalıdır. Bir veya iki, ancak tüm üç değerden paylaşan görüntü sürümleri olabilir. 

##<a name="create-an-image-version"></a>Görüntü sürümü oluşturma

Görüntü sürümü kullanarak bir yönetilen görüntüsünü oluşturma [yeni AzGalleryImageVersion](https://docs.microsoft.com/powershell/module/az.compute/new-azgalleryimageversion) . Bu örnekte, görüntü sürümü olan *1.0.0* ve her ikisi de çoğaltılır *Batı Orta ABD* ve *Orta Güney ABD* veri merkezleri.


```azurepowershell-interactive
$region1 = @{Name='South Central US';ReplicaCount=1}
$region2 = @{Name='West Central US';ReplicaCount=2}
$targetRegions = @($region1,$region2)
$job = $imageVersion = New-AzGalleryImageVersion `
   -GalleryImageDefinitionName $galleryImage.Name `
   -GalleryImageVersionName '1.0.0' `
   -GalleryName $gallery.Name `
   -ResourceGroupName $resourceGroup.ResourceGroupName `
   -Location $resourceGroup.Location `
   -TargetRegion $targetRegions  `
   -Source $managedImage.Id.ToString() `
   -PublishingProfileEndOfLifeDate '2020-01-01' `
   -asJob 
```

Uygulamanın, biz ilerleme durumunu izleyebilmek bir işi oluşturduk şekilde görüntünün hedef bölgeler tümüne biraz sürebilir. İşinin ilerleme durumunu görmek için şunu yazın `$job.State`.

```azurepowershell-interactive
$job.State
```

