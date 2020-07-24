---
title: Azure CLI Betik Örneği - NGINX ile Linux VM Oluşturma
description: Azure CLI Betik Örneği - NGINX ile Linux VM Oluşturma
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: f894d4f29ce8729ff88faa72b0d6c470fd6f87c9
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87088691"
---
# <a name="create-a-vm-with-nginx"></a>NGINX ile VM oluşturma

Bu betik bir Azure Sanal Makinesi oluşturur ve Azure Sanal Makinesi Özel Betik Uzantısını kullanarak NGINX yükler. Betiği çalıştırdıktan sonra sanal makinenin genel IP adresi üzerindeki bir tanıtım web sitesine erişebilirsiniz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nginx/create-vm-nginx.sh "Quick Create VM")]

## <a name="custom-script-extension"></a>Özel Betik Uzantısı

Özel betik uzantısı bu betiği sanal makinenin üzerine kopyalar. Daha sonra bir NGINX web sunucusu yüklemek ve yapılandırmak için betik çalıştırılır.

```bash
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir kaynak grubu, sanal makine ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az vm create](/cli/azure/vm) | Sanal makineyi oluşturur. Bu komut ayrıca kullanılacak sanal makine görüntüsünü ve yönetici kimlik bilgilerini belirtir.  |
| [az vm open-port](/cli/azure/network/nsg/rule) | Gelen trafiğe izin veren bir ağ güvenlik grubu kuralı oluşturur. Bu örnekte, 80 numaralı bağlantı noktası HTTP trafiğine açılır. |
| [azure vm extension set](/cli/azure/vm/extension) | Bir VM’ye sanal makine uzantısı ekler ve çalıştırır. Bu örnekte NGINX yüklemek için özel betik uzantısı kullanılır.|
| [az group delete](/cli/azure/vm/extension) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek sanal makine CLI betiği örnekleri, [Azure Linux VM belgeleri](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.
