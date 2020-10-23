---
title: Azure CLı kullanarak Azure Cosmos DB Core (SQL) API kaynaklarını yönetme
description: Azure CLı kullanarak Azure Cosmos DB Core (SQL) API kaynaklarını yönetme.
author: markjbrown
ms.service: cosmos-db
ms.topic: how-to
ms.date: 10/13/2020
ms.author: mjbrown
ms.openlocfilehash: 631dd3242e695a1e7872a4b078ab3f9761591c56
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92277765"
---
# <a name="manage-azure-cosmos-core-sql-api-resources-using-azure-cli"></a>Azure CLı kullanarak Azure Cosmos Core (SQL) API kaynaklarını yönetme

Aşağıdaki kılavuzda Azure Cosmos DB hesaplarınız, veritabanlarınız ve kapsayıcılarınızı yönetme işleminin Azure CLI kullanılarak otomatikleştirilmesini sağlayan yaygın komutlar açıklanır. Tüm Azure Cosmos DB CLı komutlarına yönelik başvuru sayfaları, [Azure CLI başvurusunda](https://docs.microsoft.com/cli/azure/cosmosdb)bulunabilir. Ayrıca, MongoDB, Gremlin, Cassandra ve Tablo API'si için Cosmos DB hesapları, veritabanları ve kapsayıcıları oluşturma ve yönetme dahil olmak üzere [Azure Cosmos DB Için Azure CLI örneklerinde](cli-samples.md)daha fazla örnek bulabilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı altında, Azure CLı sürüm 2.12.1 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

Diğer API 'Ler için Azure CLı örnekleri için bkz. [Cassandra Için CLI örnekleri](cli-samples-cassandra.md), [MONGODB API için](cli-samples-mongodb.md)CLI örnekleri, [Gremlin için](cli-samples-gremlin.md)CLI örnekleri, [tablo için CLI](cli-samples-table.md) örnekleri

> [!IMPORTANT]
> Azure Cosmos DB kaynaklar, Azure Resource Manager kaynak URI 'leriyle nasıl çalıştığını ihlal ettiğinden yeniden adlandırılamaz.

## <a name="azure-cosmos-accounts"></a>Azure Cosmos hesapları

Aşağıdaki bölümlerde aşağıdakiler dahil olmak üzere Azure Cosmos hesabının nasıl yönetileceği gösterilmektedir:

* [Azure Cosmos hesabı oluşturma](#create-an-azure-cosmos-db-account)
* [Bölge ekleme veya kaldırma](#add-or-remove-regions)
* [Çok bölgeli yazmaları etkinleştirme](#enable-multiple-write-regions)
* [Bölgesel yük devretme önceliğini ayarla](#set-failover-priority)
* [Otomatik yük devretmeyi etkinleştir](#enable-automatic-failover)
* [El ile yük devretmeyi Tetikle](#trigger-manual-failover)
* [Hesap anahtarlarını listeleme](#list-account-keys)
* [Salt okuma hesap anahtarlarını listeleme](#list-read-only-account-keys)
* [Bağlantı dizelerini listeleme](#list-connection-strings)
* [Hesap anahtarını yeniden oluştur](#regenerate-account-key)

### <a name="create-an-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı oluşturma

Batı ABD 2 ve Doğu ABD 2 bölgelerinde SQL API 'SI, oturum tutarlılığı Azure Cosmos DB bir hesap oluşturun:

> [!IMPORTANT]
> Azure Cosmos hesap adı küçük ve 44 karakterden az olmalıdır.

```azurecli-interactive
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount' #needs to be lower case and less than 44 characters

az cosmosdb create \
    -n $accountName \
    -g $resourceGroupName \
    --default-consistency-level Session \
    --locations regionName='West US 2' failoverPriority=0 isZoneRedundant=False \
    --locations regionName='East US 2' failoverPriority=1 isZoneRedundant=False
```

### <a name="add-or-remove-regions"></a>Bölge ekleme veya kaldırma

İki bölgeyle bir Azure Cosmos hesabı oluşturun, bölge ekleyin ve bölge kaldırın.

> [!NOTE]
> Azure Cosmos hesabı için aynı anda bölge ekleyemez veya kaldıramaz `locations` ve diğer özellikleri değiştiremezsiniz. Bölgeleri değiştirmek, hesap kaynağında yapılan diğer değişiklikten farklı bir işlem olarak gerçekleştirilmelidir.
> [!NOTE]
> Bu komut, bölge eklemenize ve kaldırmanıza izin verir ancak yük devretme önceliklerini değiştirmenize veya el ile yük devretme tetiklemesine izin vermez. Bkz. [Yük devretme önceliğini ayarlama](#set-failover-priority) ve [El Ile yük devretmeyi tetikleme](#trigger-manual-failover).

```azurecli-interactive
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'

# Create an account with 2 regions
az cosmosdb create --name $accountName --resource-group $resourceGroupName \
    --locations regionName="West US 2" failoverPriority=0 isZoneRedundant=False \
    --locations regionName="East US 2" failoverPriority=1 isZoneRedundant=False

# Add a region
az cosmosdb update --name $accountName --resource-group $resourceGroupName \
    --locations regionName="West US 2" failoverPriority=0 isZoneRedundant=False \
    --locations regionName="East US 2" failoverPriority=1 isZoneRedundant=False \
    --locations regionName="South Central US" failoverPriority=2 isZoneRedundant=False

# Remove a region
az cosmosdb update --name $accountName --resource-group $resourceGroupName \
    --locations regionName="West US 2" failoverPriority=0 isZoneRedundant=False \
    --locations regionName="East US 2" failoverPriority=1 isZoneRedundant=False
```

### <a name="enable-multiple-write-regions"></a>Birden çok yazma bölgesini etkinleştir

Cosmos hesabı için çok bölgeli yazmaları etkinleştirme

```azurecli-interactive
# Update an Azure Cosmos account from single write region to multiple write regions
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'

# Get the account resource id for an existing account
accountId=$(az cosmosdb show -g $resourceGroupName -n $accountName --query id -o tsv)

az cosmosdb update --ids $accountId --enable-multiple-write-locations true
```

### <a name="set-failover-priority"></a>Yük devretme önceliğini ayarla

Otomatik yük devretme için yapılandırılmış bir Azure Cosmos hesabının yük devretme önceliğini ayarlama

```azurecli-interactive
# Assume region order is initially 'West US 2'=0 'East US 2'=1 'South Central US'=2 for account
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'

# Get the account resource id for an existing account
accountId=$(az cosmosdb show -g $resourceGroupName -n $accountName --query id -o tsv)

# Make South Central US the next region to fail over to instead of East US 2
az cosmosdb failover-priority-change --ids $accountId \
    --failover-policies 'West US 2=0' 'South Central US=1' 'East US 2=2'
```

### <a name="enable-automatic-failover"></a>Otomatik yük devretmeyi etkinleştir

```azurecli-interactive
# Enable automatic failover on an existing account
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'

# Get the account resource id for an existing account
accountId=$(az cosmosdb show -g $resourceGroupName -n $accountName --query id -o tsv)

az cosmosdb update --ids $accountId --enable-automatic-failover true
```

### <a name="trigger-manual-failover"></a>El ile yük devretmeyi Tetikle

> [!CAUTION]
> Öncelik = 0 olan bölgenin değiştirilmesi, bir Azure Cosmos hesabı için el ile yük devretmeyi tetikler. Diğer herhangi bir öncelik değişikliği, yük devretmeyi tetiklemez.

```azurecli-interactive
# Assume region order is initially 'West US 2=0' 'East US 2=1' 'South Central US=2' for account
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'

# Get the account resource id for an existing account
accountId=$(az cosmosdb show -g $resourceGroupName -n $accountName --query id -o tsv)

# Trigger a manual failover to promote East US 2 as new write region
az cosmosdb failover-priority-change --ids $accountId \
    --failover-policies 'East US 2=0' 'South Central US=1' 'West US 2=2'
```

### <a name="list-all-account-keys"></a><a id="list-account-keys"></a> Tüm hesap anahtarlarını Listele

Cosmos hesabı için tüm anahtarları alın.

```azurecli-interactive
# List all account keys
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'

az cosmosdb keys list \
   -n $accountName \
   -g $resourceGroupName
```

### <a name="list-read-only-account-keys"></a>Salt okuma hesap anahtarlarını listeleme

Cosmos hesabı için salt okuma anahtarları alın.

```azurecli-interactive
# List read-only account keys
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'

az cosmosdb keys list \
    -n $accountName \
    -g $resourceGroupName \
    --type read-only-keys
```

### <a name="list-connection-strings"></a>Bağlantı dizelerini listeleme

Cosmos hesabı için bağlantı dizelerini alın.

```azurecli-interactive
# List connection strings
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'

az cosmosdb keys list \
    -n $accountName \
    -g $resourceGroupName \
    --type connection-strings
```

### <a name="regenerate-account-key"></a>Hesap anahtarını yeniden oluştur

Cosmos hesabı için yeni bir anahtar oluşturun.

```azurecli-interactive
# Regenerate secondary account keys
# key-kind values: primary, primaryReadonly, secondary, secondaryReadonly
az cosmosdb keys regenerate \
    -n $accountName \
    -g $resourceGroupName \
    --key-kind secondary
```

## <a name="azure-cosmos-db-database"></a>Azure Cosmos DB veritabanı

Aşağıdaki bölümlerde aşağıdakiler de dahil olmak üzere Azure Cosmos DB veritabanının nasıl yönetileceği gösterilmektedir:

* [Veritabanı oluşturma](#create-a-database)
* [Paylaşılan verimlilik ile veritabanı oluşturma](#create-a-database-with-shared-throughput)
* [Veritabanını otomatik ölçeklendirme aktarım hızına geçirme](#migrate-a-database-to-autoscale-throughput)
* [Veritabanı aktarım hızını değiştirme](#change-database-throughput)
* [Veritabanının silinmesini önleme](#prevent-a-database-from-being-deleted)

### <a name="create-a-database"></a>Veritabanı oluşturma

Cosmos veritabanı oluşturma.

```azurecli-interactive
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'

az cosmosdb sql database create \
    -a $accountName \
    -g $resourceGroupName \
    -n $databaseName
```

### <a name="create-a-database-with-shared-throughput"></a>Paylaşılan verimlilik ile veritabanı oluşturma

Paylaşılan aktarım hızı ile Cosmos veritabanı oluşturma.

```azurecli-interactive
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
throughput=400

az cosmosdb sql database create \
    -a $accountName \
    -g $resourceGroupName \
    -n $databaseName \
    --throughput $throughput
```

### <a name="migrate-a-database-to-autoscale-throughput"></a>Veritabanını otomatik ölçeklendirme aktarım hızına geçirme

```azurecli-interactive
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'

# Migrate to autoscale throughput
az cosmosdb sql database throughput migrate \
    -a $accountName \
    -g $resourceGroupName \
    -n $databaseName \
    -t 'autoscale'

# Read the new autoscale max throughput
az cosmosdb sql database throughput show \
    -g $resourceGroupName \
    -a $accountName \
    -n $databaseName \
    --query resource.autoscaleSettings.maxThroughput \
    -o tsv
```

### <a name="change-database-throughput"></a>Veritabanı aktarım hızını değiştirme

Cosmos veritabanının verimini 1000 RU/s ile artırın.

```azurecli-interactive
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
newRU=1000

# Get minimum throughput to make sure newRU is not lower than minRU
minRU=$(az cosmosdb sql database throughput show \
    -g $resourceGroupName -a $accountName -n $databaseName \
    --query resource.minimumThroughput -o tsv)

if [ $minRU -gt $newRU ]; then
    newRU=$minRU
fi

az cosmosdb sql database throughput update \
    -a $accountName \
    -g $resourceGroupName \
    -n $databaseName \
    --throughput $newRU
```

### <a name="prevent-a-database-from-being-deleted"></a>Veritabanının silinmesini önleme

Silinmesini engellemek için bir veritabanına Azure Kaynak silme kilidi koyun. Bu özellik, Cosmos hesabının veri düzlemi SDK 'Ları tarafından değiştirilmesini gerektirir. Daha fazla bilgi edinmek için bkz. [SDK 'lardan değişiklikleri engellemeyi](role-based-access-control.md#prevent-sdk-changes). Azure kaynak kilitleri, bir kaynağın bir kilit türü belirtilerek değiştirilmesini de engelleyebilir `ReadOnly` . Cosmos veritabanı için, iş aktarımının değiştirilmesini engellemek üzere kullanılabilir.

```azurecli-interactive
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'

lockType='CanNotDelete' # CanNotDelete or ReadOnly
databaseParent="databaseAccounts/$accountName"
databaseLockName="$databaseName-Lock"

# Create a delete lock on database
az lock create --name $databaseLockName \
    --resource-group $resourceGroupName \
    --resource-type Microsoft.DocumentDB/sqlDatabases \
    --lock-type $lockType \
    --parent $databaseParent \
    --resource $databaseName

# Delete lock on database
lockid=$(az lock show --name $databaseLockName \
        --resource-group $resourceGroupName \
        --resource-type Microsoft.DocumentDB/sqlDatabases \
        --resource $databaseName \
        --parent $databaseParent \
        --output tsv --query id)
az lock delete --ids $lockid
```

## <a name="azure-cosmos-db-container"></a>Azure Cosmos DB kapsayıcı

Aşağıdaki bölümlerde aşağıdakiler dahil Azure Cosmos DB kapsayıcısının nasıl yönetileceği gösterilmektedir:

* [Kapsayıcı oluşturma](#create-a-container)
* [Otomatik ölçeklendirme ile kapsayıcı oluşturma](#create-a-container-with-autoscale)
* [TTL etkin olan bir kapsayıcı oluşturma](#create-a-container-with-ttl)
* [Özel dizin ilkesiyle kapsayıcı oluşturma](#create-a-container-with-a-custom-index-policy)
* [Kapsayıcı verimini değiştirme](#change-container-throughput)
* [Kapsayıcıyı otomatik ölçeklendirme aktarım hızına geçirme](#migrate-a-container-to-autoscale-throughput)
* [Kapsayıcının silinmesini önleme](#prevent-a-container-from-being-deleted)

### <a name="create-a-container"></a>Kapsayıcı oluşturma

Varsayılan dizin ilkesi, bölüm anahtarı ve 400 RU/s ile Cosmos kapsayıcısı oluşturun.

```azurecli-interactive
# Create a SQL API container
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
containerName='container1'
partitionKey='/myPartitionKey'
throughput=400

az cosmosdb sql container create \
    -a $accountName -g $resourceGroupName \
    -d $databaseName -n $containerName \
    -p $partitionKey --throughput $throughput
```

### <a name="create-a-container-with-autoscale"></a>Otomatik ölçeklendirme ile kapsayıcı oluşturma

Varsayılan dizin ilkesi, bölüm anahtarı ve 4000 otomatik ölçeklendirme RU/s ile Cosmos kapsayıcısı oluşturun.

```azurecli-interactive
# Create a SQL API container
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
containerName='container1'
partitionKey='/myPartitionKey'
maxThroughput=4000

az cosmosdb sql container create \
    -a $accountName -g $resourceGroupName \
    -d $databaseName -n $containerName \
    -p $partitionKey --max-throughput $maxThroughput
```

### <a name="create-a-container-with-ttl"></a>TTL ile kapsayıcı oluşturma

TTL etkin olan bir Cosmos kapsayıcısı oluşturun.

```azurecli-interactive
# Create an Azure Cosmos container with TTL of one day
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
containerName='container1'

az cosmosdb sql container update \
    -g $resourceGroupName \
    -a $accountName \
    -d $databaseName \
    -n $containerName \
    --ttl=86400
```

### <a name="create-a-container-with-a-custom-index-policy"></a>Özel dizin ilkesiyle kapsayıcı oluşturma

Özel dizin ilkesi, uzamsal dizin, bileşik dizin, bölüm anahtarı ve 400 RU/sn ile Cosmos kapsayıcısı oluşturun.

```azurecli-interactive
# Create a SQL API container
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
containerName='container1'
partitionKey='/myPartitionKey'
throughput=400

# Generate a unique 10 character alphanumeric string to ensure unique resource names
uniqueId=$(env LC_CTYPE=C tr -dc 'a-z0-9' < /dev/urandom | fold -w 10 | head -n 1)

# Define the index policy for the container, include spatial and composite indexes
idxpolicy=$(cat << EOF
{
    "indexingMode": "consistent",
    "includedPaths": [
        {"path": "/*"}
    ],
    "excludedPaths": [
        { "path": "/headquarters/employees/?"}
    ],
    "spatialIndexes": [
        {"path": "/*", "types": ["Point"]}
    ],
    "compositeIndexes":[
        [
            { "path":"/name", "order":"ascending" },
            { "path":"/age", "order":"descending" }
        ]
    ]
}
EOF
)
# Persist index policy to json file
echo "$idxpolicy" > "idxpolicy-$uniqueId.json"


az cosmosdb sql container create \
    -a $accountName -g $resourceGroupName \
    -d $databaseName -n $containerName \
    -p $partitionKey --throughput $throughput \
    --idx @idxpolicy-$uniqueId.json

# Clean up temporary index policy file
rm -f "idxpolicy-$uniqueId.json"
```

### <a name="change-container-throughput"></a>Kapsayıcı verimini değiştirme

Cosmos kapsayıcısının verimini 1000 RU/s ile artırın.

```azurecli-interactive
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
containerName='container1'
newRU=1000

# Get minimum throughput to make sure newRU is not lower than minRU
minRU=$(az cosmosdb sql container throughput show \
    -g $resourceGroupName -a $accountName -d $databaseName \
    -n $containerName --query resource.minimumThroughput -o tsv)

if [ $minRU -gt $newRU ]; then
    newRU=$minRU
fi

az cosmosdb sql container throughput update \
    -a $accountName \
    -g $resourceGroupName \
    -d $databaseName \
    -n $containerName \
    --throughput $newRU
```

### <a name="migrate-a-container-to-autoscale-throughput"></a>Kapsayıcıyı otomatik ölçeklendirme aktarım hızına geçirme

```azurecli-interactive
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
containerName='container1'

# Migrate to autoscale throughput
az cosmosdb sql container throughput migrate \
    -a $accountName \
    -g $resourceGroupName \
    -d $databaseName \
    -n $containerName \
    -t 'autoscale'

# Read the new autoscale max throughput
az cosmosdb sql container throughput show \
    -g $resourceGroupName \
    -a $accountName \
    -d $databaseName \
    -n $containerName \
    --query resource.autoscaleSettings.maxThroughput \
    -o tsv
```

### <a name="prevent-a-container-from-being-deleted"></a>Kapsayıcının silinmesini önleme

Bir kapsayıcıya bir Azure Kaynak silme kilidi koymak için bir kapsayıcı yerleştirin. Bu özellik, Cosmos hesabının veri düzlemi SDK 'Ları tarafından değiştirilmesini gerektirir. Daha fazla bilgi edinmek için bkz. [SDK 'lardan değişiklikleri engellemeyi](role-based-access-control.md#prevent-sdk-changes). Azure kaynak kilitleri, bir kaynağın bir kilit türü belirtilerek değiştirilmesini de engelleyebilir `ReadOnly` . Cosmos kapsayıcısı için bu, üretilen iş veya başka bir özelliğin değiştirilmesini engellemek için kullanılabilir.

```azurecli-interactive
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
containerName='container1'

lockType='CanNotDelete' # CanNotDelete or ReadOnly
databaseParent="databaseAccounts/$accountName"
containerParent="databaseAccounts/$accountName/sqlDatabases/$databaseName"
containerLockName="$containerName-Lock"

# Create a delete lock on container
az lock create --name $containerLockName \
    --resource-group $resourceGroupName \
    --resource-type Microsoft.DocumentDB/containers \
    --lock-type $lockType \
    --parent $containerParent \
    --resource $containerName

# Delete lock on container
lockid=$(az lock show --name $containerLockName \
        --resource-group $resourceGroupName \
        --resource-type Microsoft.DocumentDB/containers \
        --resource-name $containerName \
        --parent $containerParent \
        --output tsv --query id)
az lock delete --ids $lockid
```

## <a name="next-steps"></a>Sonraki adımlar

Azure CLı hakkında daha fazla bilgi için bkz.

* [Azure CLı 'yı yükler](/cli/azure/install-azure-cli)
* [Azure CLı başvurusu](https://docs.microsoft.com/cli/azure/cosmosdb)
* [Azure Cosmos DB için ek Azure CLı örnekleri](cli-samples.md)
