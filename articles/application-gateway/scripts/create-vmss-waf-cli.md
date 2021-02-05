---
title: Azure CLI Betiği Örneği - Web trafiğini kısıtlama | Microsoft Docs
description: Azure CLI Betik Örneği - Bir web uygulaması güvenlik duvarı ve trafiği kısıtlamak için OWASP kurallarını kullanan bir sanal makine ölçek kümesi ile bir uygulama ağ geçidi oluşturun.
services: application-gateway
documentationcenter: networking
author: vhorne
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/29/2018
ms.author: victorh
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: e9201f41c9552b6a60f9ccd8eacda60ac46f89eb
ms.sourcegitcommit: f377ba5ebd431e8c3579445ff588da664b00b36b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99591645"
---
# <a name="restrict-web-traffic-using-the-azure-cli"></a>Azure CLI kullanarak web trafiğini kısıtlama

Bu betik, arka uç sunucuları için bir sanal makine ölçek kümesi kullanan bir web uygulaması güvenlik duvarı ile bir uygulama ağ geçidi oluşturur. Web uygulaması güvenlik duvarı, OWASP kurallarına göre web trafiğini kısıtlar. Betiği çalıştırdıktan sonra, genel IP adresini kullanarak uygulama ağ geçidini test edebilirsiniz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/application-gateway/create-vmss/create-vmss-waf.sh "Create application gateway")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, uygulama ağ geçidini ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive 
az group delete --name myResourceGroupAG --yes
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create) | Sanal ağ oluşturur. |
| [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) | Bir sanal ağda bir alt ağ oluşturur. |
| [az network public-ip create](/cli/azure/network/public-ip) | Uygulama ağ geçidi için genel IP adresini oluşturur. |
| [az network application-gateway create](/cli/azure/network/application-gateway) | Uygulama ağ geçidi oluşturur. |
| [az vmss create](/cli/azure/vmss#az-vmss-create) | Sanal makine ölçek kümesi oluşturur. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Bir depolama hesabı oluşturur. |
| [az monitor diagnostic-settings create](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) | Bir depolama hesabı oluşturur. |
| [az network public-ip show](/cli/azure/network/public-ip#az-network-public-ip-show) | Uygulama ağ geçidinin genel IP adresini alır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure/overview).

Ek uygulama ağ geçidi CLI betiği örnekleri, [Azure uygulama ağ geçidi belgeleri](../cli-samples.md) içinde bulunabilir.
