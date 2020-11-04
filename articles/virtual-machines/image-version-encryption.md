---
title: Önizleme-kendi anahtarlarınız ile şifrelenmiş bir görüntü sürümü oluşturma
description: Paylaşılan görüntü galerisinde, müşteri tarafından yönetilen şifreleme anahtarlarını kullanarak bir görüntü sürümü oluşturun.
author: cynthn
ms.service: virtual-machines
ms.subservice: imaging
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 11/3/2020
ms.author: cynthn
ms.openlocfilehash: f6bf436110e9822d687419b74a8a22bad7a6d700
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93333473"
---
# <a name="preview-use-customer-managed-keys-for-encrypting-images"></a>Önizleme: görüntüleri şifrelemek için müşteri tarafından yönetilen anahtarları kullanın

Galeri görüntüleri yönetilen diskler olarak depolanır, bu nedenle otomatik olarak sunucu tarafı şifreleme kullanılarak şifrelenir. Sunucu tarafı şifreleme, 256 bit [AES şifrelemesi](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)kullanır, en güçlü blok şifrelemeleri KULLANILABILIR ve FIPS 140-2 uyumludur. Azure yönetilen diskleri temel alan şifreleme modülleri hakkında daha fazla bilgi için bkz [. şifreleme API 'si: yeni nesil](/windows/desktop/seccng/cng-portal)

Görüntülerinizin şifrelenmesi için platform tarafından yönetilen anahtarlar kullanabilir, kendi anahtarlarınızı kullanabilir veya Çift şifreleme için her ikisini birlikte kullanabilirsiniz. Şifrelemeyi kendi anahtarlarınız ile yönetmeyi seçerseniz, görüntülerinizdeki tüm diskleri şifrelemek ve şifrelerini çözmek için kullanılacak *müşteri tarafından yönetilen bir anahtar* belirtebilirsiniz. 

Müşteri tarafından yönetilen anahtarlar kullanılarak sunucu tarafı şifreleme Azure Key Vault kullanır. [RSA anahtarlarınızı](../key-vault/keys/hsm-protected-keys.md) Key Vault içeri aktarabilir ya da Azure Key Vault yeni RSA anahtarları oluşturabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, görüntünüzü çoğaltmak istediğiniz her bölgede zaten bir disk şifrelemesi ayarlamış olmanız gerekir.

- Yalnızca müşteri tarafından yönetilen bir anahtar kullanmak için bkz. [Azure Portal](./disks-enable-customer-managed-keys-portal.md) veya [PowerShell](./windows/disks-enable-customer-managed-keys-powershell.md#set-up-your-azure-key-vault-and-diskencryptionset)'i kullanarak, **müşteri tarafından yönetilen anahtarları sunucu tarafı şifrelemesiyle etkinleştirme** .

- Hem platform tarafından yönetilen hem de müşteri tarafından yönetilen anahtarları (Çift şifreleme için) kullanmak için bkz. [Azure Portal](./disks-enable-double-encryption-at-rest-portal.md) veya [PowerShell](./windows/disks-enable-double-encryption-at-rest-powershell.md)kullanarak **rest 'te çift şifrelemeyi etkinleştirme** .
    > [!IMPORTANT]
    > Azure portal erişmek için bu bağlantıyı kullanmanız gerekir [https://aka.ms/diskencryptionupdates](https://aka.ms/diskencryptionupdates) . Rest 'te Çift şifreleme, bağlantıyı kullanmadan Genel Azure portal Şu anda görünür değil.

## <a name="limitations"></a>Sınırlamalar

Paylaşılan görüntü Galerisi görüntülerini şifrelemek için müşteri tarafından yönetilen anahtarlar kullanılırken çeşitli sınırlamalar vardır:  

- Şifreleme anahtarı kümeleri görüntresimle aynı abonelikte olmalıdır.

- Şifreleme anahtarı kümeleri bölgesel kaynaklardır, bu nedenle her bölge farklı bir şifreleme anahtarı kümesi gerektirir.

- Müşteri tarafından yönetilen anahtarlar kullanan görüntüleri kopyalayamaz veya paylaşamazsınız. 

- Bir diski veya görüntüyü şifrelemek için kendi anahtarlarınızı kullandıysanız, bu diskleri veya görüntüleri şifrelemek için platform tarafından yönetilen anahtarları kullanmaya geri dönemezsiniz.


> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarları kullanarak şifreleme Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


## <a name="powershell"></a>PowerShell

Genel önizleme için önce özelliği kaydetmeniz gerekir.

```azurepowershell-interactive
Register-AzProviderFeature -FeatureName SIGEncryption -ProviderNamespace Microsoft.Compute
```

Kaydın tamamlanabilmesi birkaç dakika sürer. Özellik kaydının durumunu denetlemek için Get-AzProviderFeature kullanın.

```azurepowershell-interactive
Get-AzProviderFeature -FeatureName SIGEncryption -ProviderNamespace Microsoft.Compute
```

RegistrationState kaydedildiğinde, sonraki adıma geçebilirsiniz.

Sağlayıcı kaydınızı denetleyin. Döndürdüğünden emin olun `Registered` .

```azurepowershell-interactive
Get-AzResourceProvider -ProviderNamespace Microsoft.Compute | Format-table -Property ResourceTypes,RegistrationState
```

Döndürmezse `Registered` , sağlayıcıları kaydetmek için aşağıdakileri kullanın:

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
```

Bir görüntü sürümü için ayarlanmış bir disk şifrelemesi belirtmek için, parametresiyle  [New-Azgallerımagedefinition](/powershell/module/az.compute/new-azgalleryimageversion) komutunu kullanın `-TargetRegion` . 

```azurepowershell-interactive

$sourceId = <ID of the image version source>

$osDiskImageEncryption = @{DiskEncryptionSetId='subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.Compute/diskEncryptionSets/myDESet'}

$dataDiskImageEncryption1 = @{DiskEncryptionSetId='subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.Compute/diskEncryptionSets/myDESet1';Lun=1}

$dataDiskImageEncryption2 = @{DiskEncryptionSetId='subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.Compute/diskEncryptionSets/myDESet2';Lun=2}

$dataDiskImageEncryptions = @($dataDiskImageEncryption1,$dataDiskImageEncryption2)

$encryption1 = @{OSDiskImage=$osDiskImageEncryption;DataDiskImages=$dataDiskImageEncryptions}

$region1 = @{Name='West US';ReplicaCount=1;StorageAccountType=Standard_LRS;Encryption=$encryption1}

$eastUS2osDiskImageEncryption = @{DiskEncryptionSetId='subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.Compute/diskEncryptionSets/myEastUS2DESet'}

$eastUS2dataDiskImageEncryption1 = @{DiskEncryptionSetId='subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.Compute/diskEncryptionSets/myEastUS2DESet1';Lun=1}

$eastUS2dataDiskImageEncryption2 = @{DiskEncryptionSetId='subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.Compute/diskEncryptionSets/myEastUS2DESet2';Lun=2}

$eastUS2DataDiskImageEncryptions = @($eastUS2dataDiskImageEncryption1,$eastUS2dataDiskImageEncryption2)

$encryption2 = @{OSDiskImage=$eastUS2osDiskImageEncryption;DataDiskImages=$eastUS2DataDiskImageEncryptions}

$region2 = @{Name='East US 2';ReplicaCount=1;StorageAccountType=Standard_LRS;Encryption=$encryption2}

$targetRegion = @($region1, $region2)


# Create the image
New-AzGalleryImageVersion `
   -ResourceGroupName $rgname `
   -GalleryName $galleryName `
   -GalleryImageDefinitionName $imageDefinitionName `
   -Name $versionName -Location $location `
   -SourceImageId $sourceId `
   -ReplicaCount 2 `
   -StorageAccountType Standard_LRS `
   -PublishingProfileEndOfLifeDate '2020-12-01' `
   -TargetRegion $targetRegion
```

### <a name="create-a-vm"></a>VM oluştur

Paylaşılan görüntü galerisinden bir VM oluşturabilir ve diskleri şifrelemek için müşterinin yönettiği anahtarları kullanabilirsiniz. Söz dizimi, bir görüntüden [Genelleştirilmiş](vm-generalized-image-version-powershell.md) veya [özel](vm-specialized-image-version-powershell.md) bir VM oluşturma ile aynıdır, genişletilmiş parametre kümesini kullanmanız ve `Set-AzVMOSDisk -Name $($vmName +"_OSDisk") -DiskEncryptionSetId $diskEncryptionSet.Id -CreateOption FromImage` VM yapılandırmasına eklemeniz gerekir.

Veri disklerinde, `-DiskEncryptionSetId $setID` [Add-AzVMDataDisk](/powershell/module/az.compute/add-azvmdatadisk)kullandığınızda parametresini eklemeniz gerekir.


## <a name="cli"></a>CLI 

Genel önizleme için önce özelliği kaydetmeniz gerekir.

```azurecli-interactive
az feature register --namespace Microsoft.Compute --name SIGEncryption
```

Özellik kaydının durumunu denetleyin.

```azurecli-interactive
az feature show --namespace Microsoft.Compute --name SIGEncryption | grep state
```

Bu döndürüldüğünde bir `"state": "Registered"` sonraki adıma geçebilirsiniz.

Kaydınızı denetleyin.

```azurecli-interactive
az provider show -n Microsoft.Compute | grep registrationState
```

Kayıtlı değilse, aşağıdakileri çalıştırın:

```azurecli-interactive
az provider register -n Microsoft.Compute
```


Bir görüntü sürümü için ayarlanmış bir disk şifrelemesi belirtmek için, parametresiyle  [az Image Gallery Create-Image-Version](/cli/azure/sig/image-version#az-sig-image-version-create) kullanın `--target-region-encryption` . Biçimi, `--target-region-encryption` işletim sistemi ve veri disklerini şifrelemek için bir virgülle ayrılmış anahtar listesidir. Şöyle görünmelidir: `<encryption set for the OS disk>,<Lun number of the data disk>,<encryption set for the data disk>,<Lun number for the second data disk>,<encryption set for the second data disk>` . 

İşletim sistemi diskinin kaynağı yönetilen bir disk veya VM ise, `--managed-image` görüntü sürümü kaynağını belirtmek için öğesini kullanın. Bu örnekte, kaynak, LUN 0 ' da bir işletim sistemi diskine ve veri diskine sahip olan yönetilen bir görüntüdür. İşletim sistemi diski DiskEncryptionSet1 ile şifrelenir ve veri diski DiskEncryptionSet2 ile şifrelenir.

```azurecli-interactive
az sig image-version create \
   -g MyResourceGroup \
   --gallery-image-version 1.0.0 \
   --location westus \
   --target-regions westus=2=standard_lrs eastus2 \
   --target-region-encryption WestUSDiskEncryptionSet1,0,WestUSDiskEncryptionSet2 EastUS2DiskEncryptionSet1,0,EastUS2DiskEncryptionSet2 \
   --gallery-name MyGallery \
   --gallery-image-definition MyImage \
   --managed-image "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage"
```

İşletim sistemi diskinin kaynağı bir anlık görüntüdür, `--os-snapshot` işletim sistemi diskini belirtmek için kullanın. Aynı zamanda görüntü sürümünün parçası olması gereken veri diski anlık görüntüleri varsa, bunu `--data-snapshot-luns` LUN 'u belirtmek için kullanarak ve `--data-snapshots` anlık görüntüleri belirtecek şekilde ekleyin.

Bu örnekte, kaynaklar disk anlık görüntüleridir. LUN 0 ' da bir işletim sistemi diski ve bir veri diski vardır. İşletim sistemi diski DiskEncryptionSet1 ile şifrelenir ve veri diski DiskEncryptionSet2 ile şifrelenir.

```azurecli-interactive
az sig image-version create \
   -g MyResourceGroup \
   --gallery-image-version 1.0.0 \
   --location westus\
   --target-regions westus=2=standard_lrs eastus\
   --target-region-encryption WestUSDiskEncryptionSet1,0,WestUSDiskEncryptionSet2 EastUS2DiskEncryptionSet1,0,EastUS2DiskEncryptionSet2 \
   --os-snapshot "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/snapshots/myOSSnapshot" \
   --data-snapshot-luns 0 \
   --data-snapshots "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/snapshots/myDDSnapshot" \
   --gallery-name MyGallery \
   --gallery-image-definition MyImage 
   
```

### <a name="create-the-vm"></a>Sanal makineyi oluşturma

Paylaşılan görüntü galerisinden bir VM oluşturabilir ve diskleri şifrelemek için müşterinin yönettiği anahtarları kullanabilirsiniz. Söz dizimi, bir görüntüden [Genelleştirilmiş](vm-generalized-image-version-cli.md) veya [özel](vm-specialized-image-version-cli.md) bir VM oluşturma ile aynıdır; yalnızca, `--os-disk-encryption-set` şifreleme kümesinin kimliğiyle parametresini eklemeniz gerekir. Veri diskleri için, `--data-disk-encryption-sets` veri diskleri için disk şifreleme kümelerinin boşlukla ayrılmış bir listesiyle ekleyin.


## <a name="portal"></a>Portal

Portalda görüntü sürümünüzü oluşturduğunuzda, depolama şifreleme kümelerinizi Uygula ' yı girmek için **şifreleme** sekmesini kullanabilirsiniz.

> [!IMPORTANT]
> Çift şifrelemeyi kullanmak için, Azure portal erişmek için bu bağlantıyı kullanmanız gerekir [https://aka.ms/diskencryptionupdates](https://aka.ms/diskencryptionupdates) . Rest 'te Çift şifreleme, bağlantıyı kullanmadan Genel Azure portal Şu anda görünür değil.


1. **Görüntü sürümü oluştur** sayfasında **şifreleme** sekmesini seçin.
2. **Şifreleme türü** ' nde, **müşteri tarafından yönetilen bir anahtarla** veya **platform tarafından yönetilen ve müşteri tarafından yönetilen anahtarlarla çift Şifrelemeli** şifreleme ' yi seçin. 
3. Görüntüdeki her disk için, açılan listeden kullanılacak **disk şifrelemesi kümesini** seçin. 

### <a name="create-the-vm"></a>Sanal makineyi oluşturma

Bir görüntü sürümünden bir VM oluşturabilir ve diskleri şifrelemek için müşteri tarafından yönetilen anahtarları kullanabilirsiniz. Portalda VM oluştururken, **diskler** sekmesinde, **müşteri tarafından yönetilen anahtarlar** veya **şifreleme türü** Için müşteri tarafından yönetilen **anahtarlar ile çift şifreleme** ile bekleyen şifreleme ' yi seçin. Daha sonra açılan listeden şifreleme kümesini seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Sunucu tarafı disk şifrelemesi](./windows/disk-encryption.md)hakkında daha fazla bilgi edinin.

Satın alma planı bilgilerini sağlama hakkında daha fazla bilgi için bkz. [görüntü oluştururken Azure Marketi satın alma planı bilgilerini sağlama](marketplace-images.md).
