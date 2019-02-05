---
title: Linux VM'ler için Azure anahtar kasası ayarlama | Microsoft Docs
description: Anahtar kasası Azure CLI ile bir Azure Resource Manager sanal makinesi ile kullanmak için ayarlama yapma.
services: virtual-machines-linux
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: bccdd5ab-5ccf-4760-9039-92c6eafb15bd
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 02/24/2017
ms.author: singhkay
ms.openlocfilehash: c38d901be627d9ad2f18ebe708c7a1fcaa63cc15
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2019
ms.locfileid: "55726999"
---
# <a name="how-to-set-up-key-vault-for-virtual-machines-with-the-azure-cli"></a>Azure CLI ile sanal makineler için anahtar kasası ayarlama

Azure Resource Manager yığınında gizli dizileri/sertifikaları Key Vault tarafından sağlanan kaynaklar olarak modellenir. Azure Key Vault hakkında daha fazla bilgi için bkz: [Azure anahtar kasası nedir?](../../key-vault/key-vault-whatis.md) Key Vault, Azure Resource Manager Vm'lerinde ile kullanılacak sırada *EnabledForDeployment* Key Vault özelliği ayarlanmalıdır true. Bu makalede, Azure için Azure CLI kullanarak sanal makineleri (VM'ler) ile kullanım için anahtar kasası ayarlama işlemini göstermektedir. 

Bu adımları gerçekleştirmek için en son gerekir [Azure CLI](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az login](/cli/azure/reference-index).

## <a name="create-a-key-vault"></a>Anahtar kasası oluşturma
Dağıtım ilkesi ile key vault oluşturma ve atama [az keyvault oluşturma](/cli/azure/keyvault). Aşağıdaki örnekte adlı bir anahtar kasası oluşturulmaktadır `myKeyVault` içinde `myResourceGroup` kaynak grubu:

```azurecli
az keyvault create -l westus -n myKeyVault -g myResourceGroup --enabled-for-deployment true
```

## <a name="update-a-key-vault-for-use-with-vms"></a>Sanal makineler ile kullanmak için bir Key Vault güncelleştirme
Dağıtım ilkesi var olan bir anahtar kasası ile kümesi [az keyvault update](/cli/azure/keyvault). Aşağıdaki adlı anahtar kasasını güncelleştirir `myKeyVault` içinde `myResourceGroup` kaynak grubu:

```azurecli
az keyvault update -n myKeyVault -g myResourceGroup --set properties.enabledForDeployment=true
```

## <a name="use-templates-to-set-up-key-vault"></a>Anahtar kasası ayarlama için şablonları kullanma
Bir şablon kullandığınızda ayarlamanız gerekir `enabledForDeployment` özelliğini `true` anahtarı gibi kaynak Kasası:

```json
{
    "type": "Microsoft.KeyVault/vaults",
    "name": "ContosoKeyVault",
    "apiVersion": "2015-06-01",
    "location": "<location-of-key-vault>",
    "properties": {
    "enabledForDeployment": "true",
    ....
    ....
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Şablonları kullanarak bir anahtar kasası oluşturduğunuzda, yapılandırabileceğiniz diğer seçenekleri için bkz: [anahtar kasası oluşturma](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
