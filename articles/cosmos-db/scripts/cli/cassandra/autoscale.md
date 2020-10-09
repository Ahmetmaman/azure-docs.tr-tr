---
title: Azure Cosmos DB için otomatik ölçeklendirmeyi olan Cassandra anahtar alanı ve tablo oluşturma
description: Azure Cosmos DB için otomatik ölçeklendirmeyi olan Cassandra anahtar alanı ve tablo oluşturma
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: sample
ms.date: 7/29/2020
ms.openlocfilehash: 0f11d022567a5e221a306cd7c34f31cae0ef48aa
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87433731"
---
# <a name="create-an-azure-cosmos-cassandra-api-account-keyspace-and-table-with-autoscale-using-azure-cli"></a>Azure CLı kullanarak otomatik ölçeklendirme ile bir Azure Cosmos Cassandra API hesabı, anahtar alanı ve tablo oluşturma

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı altında, Azure CLı sürüm 2.9.1 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/cassandra/autoscale.sh "Create an Azure Cosmos DB Cassandra API account, keyspace, and table with autoscale.")]

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
| [az cosmosdb Cassandra anahtar alanı oluşturma](/cli/azure/cosmosdb/cassandra/keyspace#az-cosmosdb-cassandra-keyspace-create) | Azure Cosmos Cassandra keyspace oluşturur. |
| [az cosmosdb Cassandra tablo oluşturma](/cli/azure/cosmosdb/cassandra/table#az-cosmosdb-cassandra-table-create) | Azure Cosmos Cassandra tablosu oluşturur. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

CLı Azure Cosmos DB hakkında daha fazla bilgi için bkz. [Azure Cosmos DB CLI belgeleri](/cli/azure/cosmosdb).

Tüm Azure Cosmos DB CLı betiği örnekleri [Azure Cosmos DB CLI GitHub deposunda](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)bulunabilir.
