---
title: Azure Event Grid olayları filtreleme
description: Bu makalede, bir Event Grid aboneliği oluştururken olayların (olay türüne, konuya göre, işleçlere ve verilere göre) nasıl filtreleneceği gösterilir.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 99fb00f99a055033ccfcd99e32a52d423878fb44
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "86105600"
---
# <a name="filter-events-for-event-grid"></a>Olayları Event Grid filtrele

Bu makalede, Event Grid aboneliği oluştururken olayların nasıl filtreleneceği gösterilir. Olay filtreleme seçenekleri hakkında bilgi edinmek için bkz. [Event Grid abonelikleri için olay filtrelemeyi anlama](event-filtering.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="filter-by-event-type"></a>Olay türüne göre filtrele

Event Grid bir abonelik oluştururken, hangi [olay türlerinin](event-schema.md) uç noktaya gönderileceğini belirtebilirsiniz. Bu bölümdeki örnekler, bir kaynak grubu için olay abonelikleri oluşturur ancak ve ' a gönderilen olayları sınırlandırır `Microsoft.Resources.ResourceWriteFailure` `Microsoft.Resources.ResourceWriteSuccess` . Olayları olay türlerine göre filtrelerken daha fazla esneklik gerekiyorsa bkz. filtre gelişmiş işleçler ve veri alanları.

PowerShell için, `-IncludedEventType` aboneliği oluştururken parametresini kullanın.

```powershell
$includedEventTypes = "Microsoft.Resources.ResourceWriteFailure", "Microsoft.Resources.ResourceWriteSuccess"

New-AzEventGridSubscription `
  -EventSubscriptionName demoSubToResourceGroup `
  -ResourceGroupName myResourceGroup `
  -Endpoint <endpoint-URL> `
  -IncludedEventType $includedEventTypes
```

Azure CLı için `--included-event-types` parametresini kullanın. Aşağıdaki örnek, bir bash kabuğu 'nda Azure CLı kullanır:

```azurecli
includedEventTypes="Microsoft.Resources.ResourceWriteFailure Microsoft.Resources.ResourceWriteSuccess"

az eventgrid event-subscription create \
  --name demoSubToResourceGroup \
  --resource-group myResourceGroup \
  --endpoint <endpoint-URL> \
  --included-event-types $includedEventTypes
```

Kaynak Yöneticisi şablonu için `includedEventTypes` özelliğini kullanın.

```json
"resources": [
  {
    "type": "Microsoft.EventGrid/eventSubscriptions",
    "name": "[parameters('eventSubName')]",
    "apiVersion": "2018-09-15-preview",
    "properties": {
      "destination": {
        "endpointType": "WebHook",
        "properties": {
          "endpointUrl": "[parameters('endpoint')]"
        }
      },
      "filter": {
        "subjectBeginsWith": "",
        "subjectEndsWith": "",
        "isSubjectCaseSensitive": false,
        "includedEventTypes": [
          "Microsoft.Resources.ResourceWriteFailure",
          "Microsoft.Resources.ResourceWriteSuccess"
        ]
      }
    }
  }
]
```

## <a name="filter-by-subject"></a>Konuya göre filtrele

Olayları olay verilerinde konuya göre filtreleyebilirsiniz. Konunun başlangıcı veya bitişi için eşleştirilecek bir değer belirtebilirsiniz. Olayları konuya göre filtrelerken daha fazla esneklik gerekiyorsa bkz. Gelişmiş Operatörler ve veri alanlarına göre filtreleme.

Aşağıdaki PowerShell örneğinde, konusunun başlangıcına göre filtreleyen bir olay aboneliği oluşturacaksınız. `-SubjectBeginsWith`Belirli bir kaynaktaki olayları sınırlamak için parametresini kullanabilirsiniz. Bir ağ güvenlik grubunun kaynak KIMLIĞINI geçirirsiniz.

```powershell
$resourceId = (Get-AzResource -ResourceName demoSecurityGroup -ResourceGroupName myResourceGroup).ResourceId

New-AzEventGridSubscription `
  -Endpoint <endpoint-URL> `
  -EventSubscriptionName demoSubscriptionToResourceGroup `
  -ResourceGroupName myResourceGroup `
  -SubjectBeginsWith $resourceId
```

Sonraki PowerShell örneği bir blob depolaması için bir abonelik oluşturur. Olayları ile biten bir konuyla sınırlı olarak kısıtlar `.jpg` .

```powershell
$storageId = (Get-AzStorageAccount -ResourceGroupName myResourceGroup -AccountName $storageName).Id

New-AzEventGridSubscription `
  -EventSubscriptionName demoSubToStorage `
  -Endpoint <endpoint-URL> `
  -ResourceId $storageId `
  -SubjectEndsWith ".jpg"
```

Aşağıdaki Azure CLı örneğinde, konunun başlangıcına göre filtreleyen bir olay aboneliği oluşturacaksınız. `--subject-begins-with`Belirli bir kaynaktaki olayları sınırlamak için parametresini kullanabilirsiniz. Bir ağ güvenlik grubunun kaynak KIMLIĞINI geçirirsiniz.

```azurecli
resourceId=$(az resource show --name demoSecurityGroup --resource-group myResourceGroup --resource-type Microsoft.Network/networkSecurityGroups --query id --output tsv)

az eventgrid event-subscription create \
  --name demoSubscriptionToResourceGroup \
  --resource-group myResourceGroup \
  --endpoint <endpoint-URL> \
  --subject-begins-with $resourceId
```

Sonraki Azure CLı örneği bir blob depolaması için bir abonelik oluşturur. Olayları ile biten bir konuyla sınırlı olarak kısıtlar `.jpg` .

```azurecli
storageid=$(az storage account show --name $storageName --resource-group myResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --resource-id $storageid \
  --name demoSubToStorage \
  --endpoint <endpoint-URL> \
  --subject-ends-with ".jpg"
```

Aşağıdaki Kaynak Yöneticisi şablonu örneğinde, konunun başlangıcına göre filtreleyen bir olay aboneliği oluşturursunuz. `subjectBeginsWith`Belirli bir kaynaktaki olayları sınırlamak için özelliğini kullanın. Bir ağ güvenlik grubunun kaynak KIMLIĞINI geçirirsiniz.

```json
"resources": [
  {
    "type": "Microsoft.EventGrid/eventSubscriptions",
    "name": "[parameters('eventSubName')]",
    "apiVersion": "2018-09-15-preview",
    "properties": {
      "destination": {
        "endpointType": "WebHook",
        "properties": {
          "endpointUrl": "[parameters('endpoint')]"
        }
      },
      "filter": {
        "subjectBeginsWith": "[resourceId('Microsoft.Network/networkSecurityGroups','demoSecurityGroup')]",
        "subjectEndsWith": "",
        "isSubjectCaseSensitive": false,
        "includedEventTypes": [ "All" ]
      }
    }
  }
]
```

Sonraki Kaynak Yöneticisi şablonu örneği bir blob depolaması için bir abonelik oluşturur. Olayları ile biten bir konuyla sınırlı olarak kısıtlar `.jpg` .

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts/providers/eventSubscriptions",
    "name": "[concat(parameters('storageName'), '/Microsoft.EventGrid/', parameters('eventSubName'))]",
    "apiVersion": "2018-09-15-preview",
    "properties": {
      "destination": {
        "endpointType": "WebHook",
        "properties": {
          "endpointUrl": "[parameters('endpoint')]"
        }
      },
      "filter": {
        "subjectEndsWith": ".jpg",
        "subjectBeginsWith": "",
        "isSubjectCaseSensitive": false,
        "includedEventTypes": [ "All" ]
      }
    }
  }
]
```

## <a name="filter-by-operators-and-data"></a>İşleçlere ve verilere göre filtrele

Filtrelemeye ilişkin daha fazla esneklik için İşleçleri ve veri özelliklerini kullanarak olayları filtreleyebilirsiniz.

### <a name="subscribe-with-advanced-filters"></a>Gelişmiş filtrelerle abone olma

Gelişmiş filtreleme için kullanabileceğiniz işleçler ve anahtarlar hakkında bilgi edinmek için bkz. [Gelişmiş filtreleme](event-filtering.md#advanced-filtering).

Bu örnekler özel bir konu oluşturur. Özel konuya abone olur ve veri nesnesindeki bir değere göre filtre uygulayın. Color özelliği mavi, Red veya yeşil olarak ayarlanmış olaylar aboneliğe gönderilir.

Azure CLI için şunu kullanın:

```azurecli
topicName=<your-topic-name>
endpointURL=<endpoint-URL>

az group create -n gridResourceGroup -l eastus2
az eventgrid topic create --name $topicName -l eastus2 -g gridResourceGroup

topicid=$(az eventgrid topic show --name $topicName -g gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --source-resource-id $topicid \
  -n demoAdvancedSub \
  --advanced-filter data.color stringin blue red green \
  --endpoint $endpointURL \
  --expiration-date "<yyyy-mm-dd>"
```

Abonelik için bir [sona erme tarihi](concepts.md#event-subscription-expiration) belirlendiğine dikkat edin.

PowerShell için şunu kullanın:

```powershell
$topicName = <your-topic-name>
$endpointURL = <endpoint-URL>

New-AzResourceGroup -Name gridResourceGroup -Location eastus2
New-AzEventGridTopic -ResourceGroupName gridResourceGroup -Location eastus2 -Name $topicName

$topicid = (Get-AzEventGridTopic -ResourceGroupName gridResourceGroup -Name $topicName).Id

$expDate = '<mm/dd/yyyy hh:mm:ss>' | Get-Date
$AdvFilter1=@{operator="StringIn"; key="Data.color"; Values=@('blue', 'red', 'green')}

New-AzEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint $endpointURL `
  -ExpirationDate $expDate `
  -AdvancedFilter @($AdvFilter1)
```

### <a name="test-filter"></a>Test filtresi

Filtreyi test etmek için, Color alanı yeşil olarak ayarlanmış bir olay gönderin. Yeşil, filtredeki değerlerden biri olduğundan, olay uç noktaya teslim edilir.

Azure CLI için şunu kullanın:

```azurecli
topicEndpoint=$(az eventgrid topic show --name $topicName -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name $topicName -g gridResourceGroup --query "key1" --output tsv)

event='[ {"id": "'"$RANDOM"'", "eventType": "recordInserted", "subject": "myapp/vehicles/cars", "eventTime": "'`date +%Y-%m-%dT%H:%M:%S%z`'", "data":{ "model": "SUV", "color": "green"},"dataVersion": "1.0"} ]'

curl -X POST -H "aeg-sas-key: $key" -d "$event" $topicEndpoint
```

PowerShell için şunu kullanın:

```powershell
$endpoint = (Get-AzEventGridTopic -ResourceGroupName gridResourceGroup -Name $topicName).Endpoint
$keys = Get-AzEventGridTopicKey -ResourceGroupName gridResourceGroup -Name $topicName

$eventID = Get-Random 99999
$eventDate = Get-Date -Format s

$htbody = @{
    id= $eventID
    eventType="recordInserted"
    subject="myapp/vehicles/cars"
    eventTime= $eventDate
    data= @{
        model="SUV"
        color="green"
    }
    dataVersion="1.0"
}

$body = "["+(ConvertTo-Json $htbody)+"]"

Invoke-WebRequest -Uri $endpoint -Method POST -Body $body -Headers @{"aeg-sas-key" = $keys.Key1}
```

Olayın gönderilmediği bir senaryoyu test etmek için, Color alanı sarı olarak ayarlanmış bir olay gönderin. Sarı, abonelikte belirtilen değerlerden biri değildir, bu nedenle etkinlik aboneliğinize teslim edilmemiş.

Azure CLI için şunu kullanın:

```azurecli
event='[ {"id": "'"$RANDOM"'", "eventType": "recordInserted", "subject": "myapp/vehicles/cars", "eventTime": "'`date +%Y-%m-%dT%H:%M:%S%z`'", "data":{ "model": "SUV", "color": "yellow"},"dataVersion": "1.0"} ]'

curl -X POST -H "aeg-sas-key: $key" -d "$event" $topicEndpoint
```
PowerShell için şunu kullanın:

```powershell
$htbody = @{
    id= $eventID
    eventType="recordInserted"
    subject="myapp/vehicles/cars"
    eventTime= $eventDate
    data= @{
        model="SUV"
        color="yellow"
    }
    dataVersion="1.0"
}

$body = "["+(ConvertTo-Json $htbody)+"]"

Invoke-WebRequest -Uri $endpoint -Method POST -Body $body -Headers @{"aeg-sas-key" = $keys.Key1}
```

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimatlarını izleme hakkında bilgi için bkz. [izleyici Event Grid ileti teslimi](monitor-event-delivery.md).
* Kimlik doğrulama anahtarı hakkında daha fazla bilgi için bkz. [Event Grid Security and Authentication](security-authentication.md).
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid abonelik şeması](subscription-creation-schema.md).
