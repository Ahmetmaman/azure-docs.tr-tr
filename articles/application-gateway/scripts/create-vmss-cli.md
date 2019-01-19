---
title: Azure CLI Betiği Örneği - Web trafiğini yönetme | Microsoft Docs
description: Azure CLI Betik Örneği - Bir uygulama ağ geçidi ve sanal makine ölçek kümesi ile web trafiğini yönetin.
services: application-gateway
documentationcenter: networking
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/29/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 638aa3bf0e278ce00c124d8217a1bf9104e878fb
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2019
ms.locfileid: "54413796"
---
# <a name="manage-web-traffic-using-the-azure-cli"></a>Azure CLI kullanarak web trafiğini yönetme

Bu betik, arka uç sunucuları için bir sanal makine ölçek kümesi kullanan bir uygulama ağ geçidi oluşturur. Ardından uygulama ağ geçidi web trafiğini yönetmek üzere yapılandırılabilir. Betiği çalıştırdıktan sonra, genel IP adresini kullanarak uygulama ağ geçidini test edebilirsiniz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/application-gateway/create-vmss/create-vmss.sh "Create application gateway")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, uygulama ağ geçidini ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive 
az group delete --name myResourceGroupAG --yes
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet#az-net) | Sanal ağ oluşturur. |
| [az network vnet subnet create](https://docs.microsoft.com/cli/azure/network/vnet/subnet#az-network_vnet_subnet_create) | Bir sanal ağda bir alt ağ oluşturur. |
| [az network public-ip create](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest) | Uygulama ağ geçidi için genel IP adresini oluşturur. |
| [az network application-gateway create](https://docs.microsoft.com/cli/azure/network/application-gateway?view=azure-cli-latest) | Uygulama ağ geçidi oluşturur. |
| [az vmss create](https://docs.microsoft.com/cli/azure/vmss#az-vmss-create) | Sanal makine ölçek kümesi oluşturur. |
| [az network public-ip show](https://docs.microsoft.com/cli/azure/network/public-ip) | Uygulama ağ geçidinin genel IP adresini alır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure/overview).

Ek uygulama ağ geçidi CLI betiği örnekleri, [Azure Windows VM belgeleri](../cli-samples.md) içinde bulunabilir.
