---
title: Azure CLI betiği örneği - Çok katmanlı uygulamalar için ağ oluşturma | Microsoft Docs
description: Azure CLI betiği örneği - Çok katmanlı uygulamalar için sanal ağ oluşturma.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: jdial
ms.openlocfilehash: 134135d2cfa337bf2c2d7379327df83b429a35b1
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54851582"
---
# <a name="create-a-network-for-multi-tier-applications-script-sample"></a>Çok katmanlı uygulamalar için ağ oluşturma betiği örneği

Bu betik örneği, ön uç ve arka uç alt ağları ile sanal ağ oluşturur. 3306 numaralı bağlantı noktası için, ön uç alt ağına giden trafik HTTP ve SSH ile sınırlıyken, arka uç alt ağına giden trafik MySQL ile sınırlıdır. Betiği çalıştırdıktan sonra, her bir alt ağda, web sunucusunu ve MySQL yazılımını dağıtabileceğiniz iki sanal makineniz vardır.

Azure [Cloud Shell](https://shell.azure.com/bash)’den veya yerel bir Azure CLI yüklemesinden betiği yürütebilirsiniz. CLI’yi yerel olarak kullanıyorsanız bu betik, 2.0.28 veya üzeri bir sürümü çalıştırmanızı gerektirir. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). CLI’yi yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `az login` komutunu da çalıştırmanız gerekir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Örnek betik


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.sh  "Virtual network for multi-tier application")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, sanal makineyi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın:

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir kaynak grubu, sanal ağ ve ağ güvenliği grupları oluşturmak için aşağıdaki komutları kullanır. Aşağıdaki tabloda yer alan her komut, komuta özgü belgelere yönlendirir:

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) | Bir Azure sanal ağı ve ön uç alt ağı oluşturur. |
| [az network subnet create](/cli/azure/network/vnet/subnet) | Bir arka uç alt ağı oluşturur. |
| [az network public-ip create](/cli/azure/network/public-ip#az_network_public_ip_create) | İnternet’ten sanal makineye erişmek için genel IP adresi oluşturur. |
| [az network nic create](/cli/azure/network/nic#az_network_nic_create) | Sanal ağ arabirimleri oluşturur ve bunları sanal ağın ön uç ve arka uç alt ağlarına ekler. |
| [az network nsg create](/cli/azure/network/nsg) | Ön uç ve arka uç alt ağlarıyla ilişkilendirilmiş ağ güvenlik grupları (NSG) oluşturur. |
| [az network nsg rule create](/cli/azure/network/nsg/rule#az_network_nsg_rule_create) |Belirli alt ağlara yönelik belirli bağlantı noktalarına izin veren veya engelleyen NSG kuralları oluşturur. |
| [az vm create](/cli/azure/vm) | Sanal makineler oluşturur ve her sanal makineye bir NIC ekler. Bu komut ayrıca kullanılacak sanal makine görüntüsünü ve yönetici kimlik bilgilerini belirtir. |
| [az group delete](/cli/azure/group#az_group_delete) | Bir kaynak grubunu ve içerdiği tüm kaynakları siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek sanal ağ CLI betiği örnekleri, [Sanal ağ CLI örnekleri](../cli-samples.md) bölümünde bulunabilir.
