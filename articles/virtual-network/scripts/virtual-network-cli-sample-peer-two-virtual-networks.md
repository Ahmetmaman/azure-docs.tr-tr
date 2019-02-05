---
title: Azure CLI betiği örneği - İki sanal ağı eşleme | Microsoft Docs
description: Azure CLI betiği örneği - İki sanal ağı eşleme.
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
ms.openlocfilehash: 142b36e5c1f0908ccfc1bf2e475442028e02edf7
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2019
ms.locfileid: "55728461"
---
# <a name="peer-two-virtual-networks-script-sample"></a>İki sanal ağı eşleme betiği örneği

Bu betik, Azure ağı aracılığıyla aynı bölgede iki sanal ağ oluşturur ve bu sanal ağları birbirine bağlar. Betiği çalıştırdıktan sonra iki sanal ağ arasında eşleme gerçekleştirmiş olursunuz.

Azure [Cloud Shell](https://shell.azure.com/bash)’den veya yerel bir Azure CLI yüklemesinden betiği yürütebilirsiniz. CLI’yi yerel olarak kullanıyorsanız bu betik, 2.0.28 veya üzeri bir sürümü çalıştırmanızı gerektirir. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). CLI’yi yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `az login` komutunu da çalıştırmanız gerekir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.sh "Peer two networks")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, sanal makineyi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın:

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir kaynak grubu, sanal makine ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Aşağıdaki tabloda yer alan her komut, komuta özgü belgelere yönlendirir:

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az network vnet create](/cli/azure/network/vnet) | Bir Azure sanal ağı ve alt ağ oluşturur. |
| [az network vnet peering create](/cli/azure/network/vnet/peering) | İki sanal ağ arasında eşleme oluşturur.  |
| [az group delete](/cli/azure/vm/extension) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek sanal ağ CLI betiği örnekleri, [Sanal ağ CLI örnekleri](../cli-samples.md) bölümünde bulunabilir.
