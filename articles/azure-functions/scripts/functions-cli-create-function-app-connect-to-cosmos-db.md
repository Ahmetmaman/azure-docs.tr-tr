---
title: Azure Cosmos DB-Azure CLı ile işlev uygulaması oluşturma
description: Azure CLI Betiği Örneği - Azure Cosmos DB'ye bağlanan bir Azure İşlevi oluşturma
ms.topic: sample
ms.date: 07/03/2018
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 760017439ddc65ee929db1612b34b093d8bed8a0
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94565425"
---
# <a name="create-an-azure-function-that-connects-to-an-azure-cosmos-db"></a>Azure Cosmos DB’ye bağlanan bir Azure İşlevi oluşturma

Bu Azure İşlevleri örnek betiği, bir işlev uygulaması oluşturur ve işlevi bir Azure Cosmos DB veritabanına bağlar. Bağlantıyı içeren oluşturulan uygulama ayarı, [Azure Cosmos DB tetikleyicisi veya bağlaması](../functions-bindings-cosmosdb.md) ile birlikte kullanılabilir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - Bu öğretici, Azure CLı 'nin 2,0 veya sonraki bir sürümünü gerektirir. Azure Cloud Shell kullanılıyorsa, en son sürüm zaten yüklüdür. 

## <a name="sample-script"></a>Örnek betik

Bu örnek, bir Azure İşlevi uygulaması oluşturur ve uygulama ayarlarına Cosmos DB uç noktası ve erişim anahtarı ekler.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-cosmos-db/create-function-app-connect-to-cosmos-db.sh "Create an Azure Function that connects to an Azure Cosmos DB")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Konum ile bir kaynak grubu oluşturun |
| [az storage accounts create](/cli/azure/storage/account#az-storage-account-create) | Depolama hesabı oluşturma |
| [az functionapp create](/cli/azure/functionapp#az-functionapp-create) | Sunucusuz [Tüketim planında](../functions-scale.md#consumption-plan)bir işlev uygulaması oluşturur. |
| [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) | Azure Cosmos DB veritabanı oluşturun. |
| [az cosmosdb Show](/cli/azure/cosmosdb#az-cosmosdb-show)| Veritabanı hesabı bağlantısını alır. |
| [az cosmosdb list-keys](/cli/azure/cosmosdb#az-cosmosdb-list-keys)| Veritabanı için anahtarları alır. |
| [az functionapp config appSettings kümesi](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) | İşlev uygulamasındaki uygulama ayarı olarak bağlantı dizesini ayarlar. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek Azure İşlevleri CLI betiği örnekleri, [Azure İşlevleri belgelerinde](../functions-cli-samples.md) bulunabilir.




