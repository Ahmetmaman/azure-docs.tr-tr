---
title: Azure CLı betik örneği-Batch işi çalıştırma
description: Bu betik, bir Batch işi oluşturur ve bu işe bir dizi görev ekler. Ayrıca bir işi ve görevlerini izlemeyi gösterir.
ms.topic: sample
ms.date: 12/12/2019
ms.custom: devx-track-azurecli
ms.openlocfilehash: d8efce1bc8dc32070b770769104b7fdc38e683cd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87500241"
---
# <a name="cli-example-run-a-job-and-tasks-with-azure-batch"></a>CLI örneği: Azure Batch ile bir işi ve görevleri çalıştırma

Bu betik, bir Batch işi oluşturur ve bu işe bir dizi görev ekler. Ayrıca bir işi ve görevlerini izlemeyi gösterir. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0.20 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/batch/run-job/run-job.sh "Run Job")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az batch account create](/cli/azure/batch/account#az-batch-account-create) | Batch hesabını oluşturur. |
| [az batch account login](/cli/azure/batch/account#az-batch-account-login) | Daha fazla CLI etkileşimi için belirtilen Batch hesabına karşı kimlik doğrulaması yapar.  |
| [az batch pool create](/cli/azure/batch/pool#az-batch-pool-create) | İşlem düğümleri havuzu oluşturur.  |
| [az batch job create](/cli/azure/batch/job#az-batch-job-create) | Batch işi oluşturur.  |
| [az batch task create](/cli/azure/batch/task#az-batch-task-create) | Belirtilen Batch işine bir görev ekler.  |
| [az batch job set](/cli/azure/batch/job#az-batch-job-set) | Bir Batch işinin özelliklerini güncelleştirir.  |
| [az batch job show](/cli/azure/batch/job#az-batch-job-show) | Belirtilen Batch işinin ayrıntılarını alır.  |
| [az batch task show](/cli/azure/batch/task#az-batch-task-show) | Belirtilen Batch işinden bir görevin ayrıntılarını alır.  |
| [az group delete](/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).
