---
title: Azure Cosmos DB için otomatik ölçeklendirmeyi içeren Tablo API'si tablosu oluşturma
description: Azure Cosmos DB için otomatik ölçeklendirmeyi içeren Tablo API'si tablosu oluşturma
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: sample
ms.date: 7/29/2020
ms.openlocfilehash: 6805d9c464357dfb8b0b0bdd3bdafcfa22399d78
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87433723"
---
# <a name="create-an-azure-cosmos-table-api-account-and-table-with-autoscale-using-azure-cli"></a>Azure CLı kullanarak otomatik ölçeklendirme ile bir Azure Cosmos Tablo API'si hesabı ve tablosu oluşturma

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı altında, Azure CLı sürüm 2.9.1 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/table/autoscale.sh "Create an Azure Cosmos DB Table API account and table with autoscale.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```azurecli-interactive
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) | Azure Cosmos DB hesabı oluşturur. |
| [az cosmosdb Table Create](/cli/azure/cosmosdb/table#az-cosmosdb-table-create) | Azure Cosmos Tablo API'si tablosu oluşturur. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

CLı Azure Cosmos DB hakkında daha fazla bilgi için bkz. [Azure Cosmos DB CLI belgeleri](/cli/azure/cosmosdb).

Tüm Azure Cosmos DB CLı betiği örnekleri [Azure Cosmos DB CLI GitHub deposunda](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)bulunabilir.
