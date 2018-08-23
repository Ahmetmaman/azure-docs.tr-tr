---
title: Azure CLI betiği örneği - Ağ sanal gereci aracılığıyla trafiği yönlendirme | Microsoft Docs
description: Azure CLI betiği örneği - Güvenlik duvarı ağ sanal gereci aracılığıyla trafiği yönlendirme.
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
ms.openlocfilehash: 381cd913df41a0bbe03b6c12c44c62ec5654d639
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "41920639"
---
# <a name="route-traffic-through-a-network-virtual-appliance-script-sample"></a>Ağ sanal gereci aracılığıyla trafiği yönlendirme betiği örneği

Bu betik örneği, ön uç ve arka uç alt ağları ile sanal ağ oluşturur. Ayrıca iki alt ağ arasında trafiği yönlendirmek için IP iletme etkinleştirilmiş şekilde bir sanal makine de oluşturur. Betiği çalıştırdıktan sonra, sanal makineye güvenlik duvarı uygulaması gibi ağ yazılımı dağıtabilirsiniz.

Azure [Cloud Shell](https://shell.azure.com/bash)’den veya yerel bir Azure CLI yüklemesinden betiği yürütebilirsiniz. CLI’yi yerel olarak kullanıyorsanız bu betik, 2.0.28 veya üzeri bir sürümü çalıştırmanızı gerektirir. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). CLI’yi yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `az login` komutunu da çalıştırmanız gerekir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, sanal makineyi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın:

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir kaynak grubu, sanal ağ ve ağ güvenliği grupları oluşturmak için aşağıdaki komutları kullanır. Aşağıdaki tabloda yer alan her komut, komuta özgü belgelere yönlendirir:

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) | Bir Azure sanal ağı ve ön uç alt ağı oluşturur. |
| [az network subnet create](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create) | Arka uç ve DMZ alt ağları oluşturur. |
| [az network public-ip create](/cli/azure/network/public-ip#az_network_public_ip_create) | İnternet’ten sanal makineye erişmek için genel IP adresi oluşturur. |
| [az network nic create](/cli/azure/network/nic#az_network_nic_create) | Bir sanal ağ arabirimi oluşturur ve bunun için IP iletimini etkinleştirir. |
| [az network nsg create](/cli/azure/network/nsg#az_network_nsg_create) | Ağ güvenlik grubu (NSG) oluşturur. |
| [az network nsg rule create](/cli/azure/network/nsg/rule#az_network_nsg_rule_create) | Sanal makineye gelen HTTP ve HTTPS bağlantı noktalarına izin veren NSG kuralları oluşturur. |
| [az network vnet subnet update](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update)| NSG’leri ve rota tablolarını alt ağlarla ilişkilendirir. |
| [az network route-table create](/cli/azure/network/route-table#az-network-route-table-create)| Tüm rotalar için bir rota tablosu oluşturur. |
| [az network route-table route create](/cli/azure/network/route-table/route#az-network-route-table-route-create)| Sanal makine aracılığıyla alt ağlar ile İnternet arasında trafiği yönlendirmek için rotalar oluşturur. |
| [az vm create](/cli/azure/vm#az_vm_create) | Bir sanal makine oluşturur ve ona NIC’yi ekler. Bu komut ayrıca kullanılacak sanal makine görüntüsünü ve yönetici kimlik bilgilerini belirtir. |
| [az group delete](/cli/azure/group#az_group_delete) | Bir kaynak grubunu ve içerdiği tüm kaynakları siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek sanal ağ CLI betiği örnekleri, [Sanal ağ CLI örnekleri](../cli-samples.md) bölümünde bulunabilir.
