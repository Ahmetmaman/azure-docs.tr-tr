---
title: Azure CLI Betik Örneği - WordPress ile Linux VM Oluşturma | Microsoft Docs
description: Azure CLI Betik Örneği - WordPress ile Linux VM Oluşturma
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 1c543c2357dd31bb06a19c7d53284c65a7ca7299
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37928133"
---
# <a name="create-a-vm-with-wordpress"></a>WordPress ile VM oluşturma

Bu betik bir sanal makine oluşturur ve sonra Azure Sanal Makinesi özel betik uzantısını kullanarak WordPress yükler. Betiği çalıştırdıktan sonra `http://<public IP of VM>/wordpress` adresindeki WordPress yapılandırma sitesine erişebilirsiniz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-wordpress-mysql/create-wordpress-mysql.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir kaynak grubu, sanal makine ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Sanal makine oluşturur ve ağ kartına, sanal ağa, alt ağa ve NSG’ye bağlar. Bu komut ayrıca kullanılacak sanal makine görüntüsünü ve yönetici kimlik bilgilerini belirtir.  |
| [az vm open-port](https://docs.microsoft.com/cli/azure/vm#az_vm_open_port) | Gelen trafiğe izin veren bir ağ güvenlik grubu kuralı oluşturur. Bu örnekte, 80 numaralı bağlantı noktası HTTP trafiğine açılır. |
| [az vm extension set](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Özel Betik Uzantısını sanal makineye ekleyerek WordPress yüklemeye yönelik bir betiği çağırır. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek sanal makine CLI betiği örnekleri, [Azure Linux VM belgeleri](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.
