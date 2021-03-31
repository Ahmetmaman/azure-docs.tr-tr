---
title: MongoDB için Azure Cosmos DB API 'SI Kaynak Yöneticisi şablonları
description: MongoDB için Azure Cosmos DB API 'SI oluşturmak ve yapılandırmak üzere Azure Resource Manager şablonlarını kullanın.
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: how-to
ms.date: 10/14/2020
ms.author: mjbrown
ms.openlocfilehash: 7664d48bad153b34e7557e9faaf4c8aa0d4215ad
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "93340640"
---
# <a name="manage-azure-cosmos-db-mongodb-api-resources-using-azure-resource-manager-templates"></a>Azure Resource Manager şablonları kullanarak Azure Cosmos DB MongoDB API kaynaklarını yönetme
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

Bu makalede, MongoDB API 'SI, veritabanları ve koleksiyonları için Azure Cosmos DB hesaplarınızı dağıtma ve yönetmeye yardımcı olmak üzere Azure Resource Manager şablonlarını nasıl kullanacağınızı öğreneceksiniz.

Bu makalede, yalnızca MongoDB için Azure Cosmos DB API 'sine örnek verilmiştir. diğer API türü hesaplara yönelik örnekler bulmak için bkz. [Cassandra](templates-samples-cassandra.md), [Gremlin](templates-samples-gremlin.md), [SQL](templates-samples-sql.md)ve [tablo](templates-samples-table.md) makaleleri için Azure Cosmos DB API 'si ile Azure Resource Manager şablonları kullanma.

> [!IMPORTANT]
>
> * Hesap adları, tümü küçük harfle 44 karakter ile sınırlıdır.
> * Verimlilik değerlerini değiştirmek için, şablonu güncelleştirilmiş RU/s ile yeniden dağıtın.
> * Azure Cosmos hesabına konum eklediğinizde veya kaldırdığınızda, diğer özellikleri aynı anda değiştiremezsiniz. Bu işlemlerin ayrı olarak yapılması gerekir.

Aşağıdaki Azure Cosmos DB kaynaklarından herhangi birini oluşturmak için aşağıdaki örnek şablonu yeni bir JSON dosyasına kopyalayın. İsteğe bağlı olarak, farklı adlara ve değerlere sahip aynı kaynağın birden çok örneğini dağıttığınızda kullanmak üzere bir Parameters JSON dosyası oluşturabilirsiniz. [Azure Portal](../azure-resource-manager/templates/deploy-portal.md), [Azure CLI](../azure-resource-manager/templates/deploy-cli.md), [Azure PowerShell](../azure-resource-manager/templates/deploy-powershell.md) ve [GitHub](../azure-resource-manager/templates/deploy-to-azure-button.md)dahil Azure Resource Manager şablonları dağıtmanın birçok yolu vardır.

<a id="create-autoscale"></a>

## <a name="azure-cosmos-account-for-mongodb-with-autoscale-provisioned-throughput"></a>MongoDB için otomatik ölçeklendirme sağlanan aktarım hızı ile Azure Cosmos hesabı

Bu şablon, MongoDB API 'SI (3,2 veya 3,6) için bir Azure Cosmos hesabını, otomatik ölçeklendirme verimini veritabanı düzeyinde paylaşan iki koleksiyon oluşturacak şekilde oluşturur. Bu şablon, Azure hızlı başlangıç şablonları galerisinden tek tıklamayla dağıtım için de kullanılabilir.

[:::image type="content" source="../media/template-deployments/deploy-to-azure.svg" alt-text="Azure’a dağıtın":::](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cosmosdb-mongodb-autoscale%2Fazuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-cosmosdb-mongodb-autoscale/azuredeploy.json":::

<a id="create-manual"></a>

## <a name="azure-cosmos-account-for-mongodb-with-standard-provisioned-throughput"></a>Standart sağlanan aktarım hızı ile MongoDB için Azure Cosmos hesabı

Bu şablon, MongoDB API 'SI (3,2 veya 3,6) için, veritabanı düzeyinde 400 RU/sn standart (el ile) aktarım hızını paylaşan iki koleksiyon ile bir Azure Cosmos hesabı oluşturur. Bu şablon, Azure hızlı başlangıç şablonları galerisinden tek tıklamayla dağıtım için de kullanılabilir.

[:::image type="content" source="../media/template-deployments/deploy-to-azure.svg" alt-text="Azure’a dağıtın":::](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cosmosdb-mongodb%2Fazuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-cosmosdb-mongodb/azuredeploy.json":::

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki ek kaynakları da inceleyebilirsiniz:

* [Azure Resource Manager belgeleri](../azure-resource-manager/index.yml)
* [Azure Cosmos DB kaynak sağlayıcısı şeması](/azure/templates/microsoft.documentdb/allversions)
* [Azure Cosmos DB hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
* [Ortak Azure Resource Manager Dağıtım hatalarını giderme](../azure-resource-manager/templates/common-deployment-errors.md)