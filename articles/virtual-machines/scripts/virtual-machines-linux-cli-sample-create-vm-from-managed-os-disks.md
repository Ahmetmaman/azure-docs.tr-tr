---
title: Yönetilen bir diski işletim sistemi diski olarak ekleyerek VM oluşturma-CLı örneği
description: Azure CLI Betik Örneği - Yönetilen bir diski işletim sistemi diski olarak ekleyerek sanal makine oluşturma
services: virtual-machines-linux
documentationcenter: virtual-machines
author: ramankumarlive
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 83f0ea094c26a1ff664ef27729731b77d987e7ec
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86501509"
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-cli"></a>Yönetilen bir diski işletim sistemi diski olarak ekleyerek sanal makine oluşturma

Bu betik, mevcut bir yönetilen diski işletim sistemi diski olarak ekleyerek bir sanal makine oluşturur. Yukarıdaki senaryolarda bu betiği kullanın:
* Farklı abonelikteki bir yönetilen diskten kopyalanmış mevcut bir yönetilen işletim sistemi diskinden VM oluşturma
* Özelleştirilmiş bir VHD dosyasından oluşturulmuş mevcut bir yönetilen diskten VM oluşturma 
* Anlık görüntüden oluşturulmuş mevcut bir yönetilen işletim sistemi diskinden VM oluşturma 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-attach-existing-managed-os-disk/create-vm-attach-existing-managed-os-disk.sh "Create VM from a managed disk")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik yönetilen disk özelliklerini almak, yeni bir VM’ye yönetilen bir disk eklemek ve bir VM oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az disk show](/cli/azure/disk) | Disk adı ve kaynak grubu adını kullanarak yönetilen disk özelliklerini alır. Yeni bir VM’ye yönetilen disk eklemek için kimlik özelliği kullanılır |
| [az vm create](/cli/azure/vm) | Yönetilen bir işletim sistemi diskini kullanarak VM oluşturur |
## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek sanal makine CLI betiği örnekleri, [Azure Linux VM belgeleri](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.
