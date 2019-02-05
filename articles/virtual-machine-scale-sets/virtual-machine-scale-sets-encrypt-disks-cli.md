---
title: Azure CLI ile Azure ölçek kümeleri için diskleri şifreleme | Microsoft Docs
description: Sanal makine örnekleri ve Linux sanal makine ölçek kümesi bağlı diskleri şifrelemek için Azure CLI'yı kullanmayı öğrenin
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/30/2018
ms.author: cynthn
ms.openlocfilehash: 417772b2e955b1a3664dd495f292a76ab2819165
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2019
ms.locfileid: "55734530"
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set-with-the-azure-cli-preview"></a>İşletim sistemi ve sanal makine ölçek kümesi Azure CLI (Önizleme) ile bağlı veri diskleri şifreleme

Dosya korumak ve endüstri standardında bir şifreleme teknolojisi kullanılarak, bekleme sırasında verileri korumak için Azure Disk şifrelemesi (ADE) sanal makine ölçek kümelerini destekler. Linux ve Windows sanal makinesi için şifreleme etkinleştirilebilir ölçek kümeleri. Daha fazla bilgi için [Linux ve Windows için Azure Disk şifrelemesi](../security/azure-security-disk-encryption.md).

> [!NOTE]
>  Sanal makine ölçek kümeleri için Azure disk şifrelemesi şu an Azure tüm ortak bölgelerde kullanılabilir genel Önizleme aşamasındadır.

Azure disk şifrelemesi desteklenmez:
- Ölçek kümeleri yönetilen diskler ile oluşturulan ve yerel (veya yönetilmeyen) disk ölçek kümeleri için desteklenmiyor.
- Windows ölçek kümesi işletim sistemi ve veri birimleri için. Devre dışı şifreleme, Windows ölçek kümesi için işletim sistemi ve veri birimleri için desteklenir.
- Linux ölçek kümelerinde veri birimleri için. İşletim sistemi disk şifreleme geçerli Önizleme Linux ölçek kümeleri için desteklenmez.

Geçerli Önizleme sürümünde, Ölçek kümesi VM yeniden görüntü oluşturma ve yükseltme işlemleri desteklenmiyor. Azure disk şifrelemesi için sanal makine ölçek kümeleri önizlemesi yalnızca test ortamlarında önerilir. Önizleme sürümünde, burada bir şifrelenmiş bir ölçek kümesindeki bir işletim sistemi görüntüsüne yükseltmeniz gerekebilir üretim ortamlarında disk şifrelemesi etkinleştirmeyin.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz Bu öğretici, Azure CLI Sürüm 2.0.31 çalıştırdığınız gerektirir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli).

## <a name="register-for-disk-encryption-preview"></a>Disk şifreleme Önizleme için kaydolun

Önizleme sanal makine ölçek kümeleri için Azure disk şifrelemesi, kendi aboneliğinize kaydetmeniz gerektirir [az özelliği kayıt](/cli/azure/feature). Yalnızca ilk kez disk şifreleme önizleme özelliğini kullandığınızda aşağıdaki adımları gerekir:

```azurecli-interactive
az feature register --name UnifiedDiskEncryption --namespace Microsoft.Compute
```

Bu, kayıt isteği yaymak için 10 dakikaya kadar sürebilir. Kayıt durumunu denetleyebilirsiniz [az özelliği show](/cli/azure/feature). Zaman `State` raporları *kayıtlı*, yeniden kaydetmeniz *Microsoft.Compute* sağlayıcısıyla [az provider register](/cli/azure/provider):

```azurecli-interactive
az provider register --namespace Microsoft.Compute
```

## <a name="create-a-scale-set"></a>Ölçek kümesi oluşturma

Ölçek kümesi oluşturabilmek için [az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Bu adımda [az vmss create](/cli/azure/vmss) ile bir sanal makine ölçek kümesi oluşturun. Aşağıdaki örnek, değişiklikler uygulandıkça otomatik güncelleştirilecek şekilde ayarlanan *myScaleSet* adlı bir ölçek kümesi oluşturur ve *~/.ssh/id_rsa* içinde yoksa SSH anahtarları oluşturur. Her sanal makine örneği ve Azure için 32Gb veri diskine bağlı [özel betik uzantısı](../virtual-machines/linux/extensions-customscript.md) ile veri diskleri hazırlamak için kullanılan [az vmss uzantı kümesi](/cli/azure/vmss/extension):

```azurecli-interactive
# Create a scale set with attached data disk
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --generate-ssh-keys \
  --data-disk-sizes-gb 32

# Prepare the data disk for use with the Custom Script Extension
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroup \
  --vmss-name myScaleSet \
  --settings '{"fileUris":["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.sh"],"commandToExecute":"./prepare_vm_disks.sh"}'
```

Tüm ölçek kümesi kaynaklarının ve VM'lerin oluşturulup yapılandırılması birkaç dakika sürer.

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Disk şifrelemesi için etkin bir Azure anahtar kasası oluşturma

Azure Key Vault, anahtarları, gizli dizileri veya uygulamalarınızda ve hizmetlerinizde güvenli bir şekilde uygulamak izin parolaları depolayabilirsiniz. Yazılım koruması kullanarak Azure Key Vault şifreleme anahtarlarını depolanır veya anahtarlarınızı FIPS 140-2 seviye 2 standartlarıyla sertifikalandırılmış donanım güvenlik modüllerinde (HSM'ler) oluşturun veya içeri aktarın. Bu şifreleme anahtarlarını, şifreleme ve şifre çözme, sanal Makineye eklenmiş sanal diskler için kullanılır. Bu şifreleme anahtarları denetiminizde korumak ve kullanımlarını denetleyebilirsiniz.

Kendi benzersiz olarak tanımlayan *keyvault_name*. Bir anahtar kasası ile oluşturup [az keyvault oluşturma](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-create) aynı abonelikte ve bölgededir yapmak ve ölçek *--etkin-için-disk şifreleme* erişim ilkesi.

```azurecli-interactive
# Provide your own unique Key Vault name
keyvault_name=myuniquekeyvaultname

# Create Key Vault
az keyvault create --resource-group myResourceGroup --name $keyvault_name --enabled-for-disk-encryption
```

### <a name="use-an-existing-key-vault"></a>Var olan bir Key Vault kullanma

Bu adım yalnızca, disk şifrelemesi ile kullanmak istediğiniz bir anahtar kasası varsa gereklidir. Bir Key Vault önceki bölümde oluşturduğunuz bu adımı atlayın.

Kendi benzersiz olarak tanımlayan *keyvault_name*. Daha sonra anahtar kasası ile güncelleştirilmiş [az keyvault update](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-update) ayarlayıp *--etkin-için-disk şifreleme* erişim ilkesi.

```azurecli-interactive
# Provide your own unique Key Vault name
keyvault_name=myuniquekeyvaultname

# Create Key Vault
az keyvault update --name $keyvault_name --enabled-for-disk-encryption
```

## <a name="enable-encryption"></a>Şifrelemeyi etkinleştir

Bir ölçek kümesindeki sanal makine örnekleri şifrelemek için ilk Key Vault kaynak kimliği ile hakkında bazı bilgileri alın [az keyvault show](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-show). Bu değişkenler ardından ile şifreleme işlemi başlatmak için kullanılan [az vmss şifrelemeyi etkinleştirme](/cli/azure/vmss/encryption#az-vmss-encryption-enable):

```azurecli-interactive
# Get the resource ID of the Key Vault
vaultResourceId=$(az keyvault show --resource-group myResourceGroup --name $keyvault_name --query id -o tsv)

# Enable encryption of the data disks in a scale set
az vmss encryption enable \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --disk-encryption-keyvault $vaultResourceId \
    --volume-type DATA
```

Bir veya şifreleme işlemini başlatmak için iki dakika sürebilir.

Daha önceki bir adımda ölçek kümesi içinde oluşturulan ölçek kümesini yükseltme ilkesi olarak ayarlanmış olan *otomatik*, sanal makine örneklerine otomatik olarak şifreleme işlemi başlatın. Yükseltme İlkesi el ile olarak nerede ölçek kümelerinde şifreleme ilkesi ile VM örneklerinde Başlat [az vmss update-instances](/cli/azure/vmss#az-vmss-update-instances).

## <a name="check-encryption-progress"></a>Şifreleme ilerleme durumunu denetleme

Disk şifreleme durumunu denetlemek için kullanmak [az vmss şifreleme show](/cli/azure/vmss/encryption#az-vmss-encryption-show):

```azurecli-interactive
az vmss encryption show --resource-group myResourceGroup --name myScaleSet
```

Sanal makine örnekleri şifrelenir, durum kodu raporları *EncryptionState ve şifrelenmiş*aşağıdaki örnek çıktıda gösterildiği gibi:

```bash
[
  {
    "disks": [
      {
        "encryptionSettings": null,
        "name": "myScaleSet_myScaleSet_0_disk2_3f39c2019b174218b98b3dfae3424e69",
        "statuses": [
          {
            "additionalProperties": {},
            "code": "EncryptionState/encrypted",
            "displayStatus": "Encryption is enabled on disk",
            "level": "Info",
            "message": null,
            "time": null
          }
        ]
      }
    ],
    "id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/virtualMachines/0",
    "resourceGroup": "MYRESOURCEGROUP"
  }
]
```

## <a name="disable-encryption"></a>Şifrelemeyi devre dışı bırakma

Artık şifrelenmiş VM örnekleri diskleri kullanmak istiyorsanız, şifrelemeyi devre dışı bırakabilirsiniz [az vmss şifrelemeyi devre dışı](/cli/azure/vmss/encryption?view=azure-cli-latest#az-vmss-encryption-disable) gibi:

```azurecli-interactive
az vmss encryption disable --resource-group myResourceGroup --name myScaleSet
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure CLI'yı bir sanal makine ölçek kümesi şifrelemek için kullanılır. Ayrıca [Azure PowerShell](virtual-machine-scale-sets-encrypt-disks-ps.md) veya şablonları [Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) veya [Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox).

Bir Linux ölçek kümesi veri disk şifrelemesi için uçtan uca toplu iş dosyası örneği bulunabilir [burada](https://gist.githubusercontent.com/ejarvi/7766dad1475d5f7078544ffbb449f29b/raw/03e5d990b798f62cf188706221ba6c0c7c2efb3f/enable-linux-vmss.bat). Bu örnek, bir kaynak grubu, Linux ölçek kümesi oluşturur, 5 GB'lık veri diskini bağlar ve sanal makine ölçek kümesi şifreler.
