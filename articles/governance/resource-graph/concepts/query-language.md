---
title: Sorgu dilini anlama
description: Kaynak grafik tablolarını ve kullanılabilir kusto veri türlerini, işleçlerini ve Azure Kaynak Graf ile kullanılabilir işlevleri açıklar.
ms.date: 11/18/2020
ms.topic: conceptual
ms.openlocfilehash: 34aaaa60ed9d757cc1a63ffaebb2225900cff61f
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94966692"
---
# <a name="understanding-the-azure-resource-graph-query-language"></a>Azure Kaynak Grafiği sorgu dilini anlama

Azure Kaynak grafiğinin sorgu dili, bir dizi işleci ve işlevi destekler. Her iş ve [kusto sorgu diline (KQL)](/azure/kusto/query/index)göre çalışır. Kaynak Graph tarafından kullanılan sorgu dili hakkında bilgi edinmek için, [KQL öğreticisi](/azure/kusto/query/tutorial)ile başlayın.

Bu makalede kaynak Graph tarafından desteklenen dil bileşenleri ele alınmaktadır:

- [Kaynak grafik tabloları](#resource-graph-tables)
- [Kaynak Grafiği özel dil öğeleri](#resource-graph-custom-language-elements)
- [Desteklenen KQL dil öğeleri](#supported-kql-language-elements)
- [Sorgunun kapsamı](#query-scope)
- [Kaçış karakterleri](#escape-characters)

## <a name="resource-graph-tables"></a>Kaynak grafik tabloları

Kaynak Grafiği Azure Resource Manager kaynak türlerini ve bunların özelliklerini depolayan veriler için birkaç tablo sağlar. Bazı tablolar, `join` `union` ilişkili kaynak türlerinden özellikleri almak için veya işleçleriyle birlikte kullanılabilir. Kaynak grafiğinde kullanılabilen tabloların listesi aşağıda verilmiştir:

|Kaynak Grafiği tablosu |Can `join` ? |Description |
|---|---|
|Kaynaklar |Yes |Sorguda tanımlanmazsa varsayılan tablo. Çoğu Kaynak Yöneticisi kaynak türü ve özelliği burada bulunur. |
|ResourceContainers |Yes |Abonelik (Önizleme-- `Microsoft.Resources/subscriptions` ) ve kaynak grubu ( `Microsoft.Resources/subscriptions/resourcegroups` ) kaynak türleri ve verileri içerir. |
|Danışmanlaştırın kaynakları |No |İle _ilgili_ kaynakları içerir `Microsoft.Advisor` . |
|AlertsManagementResources |No |İle _ilgili_ kaynakları içerir `Microsoft.AlertsManagement` . |
|GuestConfigurationResources |No |İle _ilgili_ kaynakları içerir `Microsoft.GuestConfiguration` . |
|MaintenanceResources |No |İle _ilgili_ kaynakları içerir `Microsoft.Maintenance` . |
|PolicyResources |No |İle _ilgili_ kaynakları içerir `Microsoft.PolicyInsights` . (**Önizleme**)|
|SecurityResources |No |İle _ilgili_ kaynakları içerir `Microsoft.Security` . |
|ServiceHealthResources |No |İle _ilgili_ kaynakları içerir `Microsoft.ResourceHealth` . |

Kaynak türleri dahil olmak üzere kapsamlı bir liste için bkz. [Başvuru: desteklenen tablolar ve kaynak türleri](../reference/supported-tables-resources.md).

> [!NOTE]
> _Kaynaklar_ varsayılan tablodur. _Kaynaklar_ tablosu sorgulanırken, `join` veya kullanılmamışsa tablo adının sağlanması gerekmez `union` . Ancak önerilen uygulama, her zaman sorguya ilk tabloyu dahil etmek için kullanılır.

Her tabloda hangi kaynak türlerinin kullanılabildiğini öğrenmek için portalda kaynak grafiği gezginini kullanın. Alternatif olarak, `<tableName> | distinct type` kaynak türlerinin bir listesini almak için gibi bir sorgu kullanın, belirtilen kaynak grafik tablosu ortamınızda var olan kaynağı destekler.

Aşağıdaki sorguda basit gösterilmektedir `join` . Sorgu sonucu sütunları bir araya ve birleştirilmiş tablodaki tüm yinelenen sütun adlarını, bu örnekteki _Resourcecontainer_ 'leri **1**' de eklenmiş şekilde harmanlar. _Resourcecontainers_ tablosunda hem abonelikler hem de kaynak grupları için türler olduğundan, _kaynak tablosundan kaynağa_ katılması için her iki tür de kullanılabilir.

```kusto
Resources
| join ResourceContainers on subscriptionId
| limit 1
```

Aşağıdaki sorguda daha karmaşık bir kullanımı gösterilmektedir `join` . Sorgu, birleştirilmiş tabloyu abonelik kaynakları ile sınırlandırır ve `project` yalnızca özgün alan _SubscriptionID_ ve _ad_ alanı, _SubName_ olarak yeniden adlandırılır. Alan, `join` _kaynaklar_ bölümünde zaten mevcut olduğundan, yeniden adlandırma, _name1_ olarak eklenmesini önler. Özgün tablo ile filtrelenmiştir `where` ve aşağıdakiler `project` her iki tablodan sütunları içerir. Sorgu sonucu, türünü, anahtar kasasının adını ve içindeki aboneliğin adını gösteren tek bir Anahtar Kasası.

```kusto
Resources
| where type == 'microsoft.keyvault/vaults'
| join (ResourceContainers | where type=='microsoft.resources/subscriptions' | project SubName=name, subscriptionId) on subscriptionId
| project type, name, SubName
| limit 1
```

> [!NOTE]
> `join`İle sonuçları sınırlandırırken `project` , `join` Yukarıdaki örnekteki SubscriptionID, yukarıdaki örnekteki _abonelik_ , ' ye dahil olmalıdır `project` .

## <a name="extended-properties-preview"></a><a name="extended-properties"></a>Genişletilmiş Özellikler (Önizleme)

_Önizleme_ özelliği olarak, kaynak grafiğindeki kaynak türlerinden bazılarının, Azure Resource Manager tarafından sağlanan özelliklerin ötesinde sorgu için kullanılabilecek ek tür ile ilgili özellikleri vardır. _Genişletilmiş özellikler_ olarak bilinen bu değer kümesi, içindeki desteklenen bir kaynak türünde mevcuttur `properties.extended` . Hangi kaynak türlerinin _genişletilmiş özelliklere_ sahip olduğunu görmek için aşağıdaki sorguyu kullanın:

```kusto
Resources
| where isnotnull(properties.extended)
| distinct type
| order by type asc
```

Örnek: sanal makinelerin sayısını şu şekilde alın `instanceView.powerState.code` :

```kusto
Resources
| where type == 'microsoft.compute/virtualmachines'
| summarize count() by tostring(properties.extended.instanceView.powerState.code)
```

## <a name="resource-graph-custom-language-elements"></a>Kaynak Grafiği özel dil öğeleri

### <a name="shared-query-syntax-preview"></a><a name="shared-query-syntax"></a>Paylaşılan sorgu sözdizimi (Önizleme)

Önizleme özelliği olarak, [paylaşılan sorguya](../tutorials/create-share-query.md) doğrudan kaynak Graph sorgusunda erişilebilir. Bu senaryo, standart sorguları paylaşılan sorgular olarak oluşturmayı ve bunları yeniden kullanmayı mümkün kılar. Bir kaynak grafiği sorgusunda paylaşılan bir sorgu çağırmak için `{{shared-query-uri}}` söz dizimini kullanın. Paylaşılan sorgunun URI 'SI, bu sorgunun **Ayarlar** sayfasındaki paylaşılan SORGUNUN _kaynak kimliğidir_ . Bu örnekte, paylaşılan sorgu URI 'imiz `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SharedQueries/providers/Microsoft.ResourceGraph/queries/Count VMs by OS` .
Bu URI, başka bir sorguda başvurmak istediğimiz paylaşılan sorgunun abonelik, kaynak grubu ve tam adını gösterir. Bu sorgu, öğreticide oluşturulan bir sorgu ile aynıdır [: sorgu oluşturma ve paylaşma](../tutorials/create-share-query.md).

> [!NOTE]
> Paylaşılan bir sorguya paylaşılan sorgu olarak başvuran bir sorgu kaydedemezsiniz.

Örnek 1: yalnızca paylaşılan sorguyu kullanın

Bu kaynak Graph sorgusunun sonuçları, paylaşılan sorguda depolanan sorgu ile aynıdır.

```kusto
{{/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SharedQueries/providers/Microsoft.ResourceGraph/queries/Count VMs by OS}}
```

Örnek 2: paylaşılan sorguyu daha büyük bir sorgunun parçası olarak dahil etme

Bu sorgu önce paylaşılan sorguyu kullanır ve ardından `limit` sonuçları daha fazla kısıtlamak için kullanır.

```kusto
{{/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/SharedQueries/providers/Microsoft.ResourceGraph/queries/Count VMs by OS}}
| where properties_storageProfile_osDisk_osType =~ 'Windows'
```

## <a name="supported-kql-language-elements"></a>Desteklenen KQL dil öğeleri

Kaynak Grafiği bir KQL [veri türleri](/azure/kusto/query/scalar-data-types/), [skaler işlevler](/azure/kusto/query/scalarfunctions), [skaler işleçler](/azure/kusto/query/binoperators)ve [toplama işlevlerinin](/azure/kusto/query/any-aggfunction)bir alt kümesini destekler. Belirli [tablolu işleçler](/azure/kusto/query/queries) , bazılarının farklı davranışları olan kaynak Graph tarafından desteklenir.

### <a name="supported-tabulartop-level-operators"></a>Desteklenen tablolu/en üst düzey işleçler

Aşağıda belirli örneklere sahip kaynak Graph tarafından desteklenen KQL tablolu işleçlerinin listesi verilmiştir:

|KQL |Kaynak Grafiği örnek sorgusu |Notlar |
|---|---|---|
|[biriktirme](/azure/kusto/query/countoperator) |[Ana kasaları say](../samples/starter.md#count-keyvaults) | |
|[ayrı](/azure/kusto/query/distinctoperator) |[Belirli bir diğer ad için farklı değerleri göster](../samples/starter.md#distinct-alias-values) | |
|[genişletmeyi](/azure/kusto/query/extendoperator) |[Sanal makineleri işletim sistemi türüne göre sayma](../samples/starter.md#count-os) | |
|[ayrılma](/azure/kusto/query/joinoperator) |[Abonelik adı olan Anahtar Kasası](../samples/advanced.md#join) |Birleşim türleri desteklenir: [ınnerunique](/azure/kusto/query/joinoperator#default-join-flavor), [Inner](/azure/kusto/query/joinoperator#inner-join), [soltouter](/azure/kusto/query/joinoperator#left-outer-join). `join`Tek bir sorgudaki 3 sınırı. Yayın katılımı gibi özel JOIN stratejilerine izin verilmez. Hangi tabloların kullanabileceği hakkında `join` bilgi için bkz. [kaynak grafik tabloları](#resource-graph-tables). |
|[sınırlı](/azure/kusto/query/limitoperator) |[Tüm genel IP adreslerini listele](../samples/starter.md#list-publicip) |Öğesinin eşanlamlısı `take` . [Skip](./work-with-data.md#skipping-records)ile çalışmaz. |
|[mvexpand](/azure/kusto/query/mvexpandoperator) | | Eski işleç yerine kullanın `mv-expand` . _RowLimit_ en fazla 400. Varsayılan değer 128 ' dir. |
|[MV-Genişlet](/azure/kusto/query/mvexpandoperator) |[Belirli yazma konumlarına sahip Cosmos DB listeleyin](../samples/advanced.md#mvexpand-cosmosdb) |_RowLimit_ en fazla 400. Varsayılan değer 128 ' dir. |
|[order](/azure/kusto/query/orderoperator) |[Ada göre sıralanan kaynakları Listele](../samples/starter.md#list-resources) |Eş anlamlısı `sort` |
|[Proje](/azure/kusto/query/projectoperator) |[Ada göre sıralanan kaynakları Listele](../samples/starter.md#list-resources) | |
|[Proje-dışarıda](/azure/kusto/query/projectawayoperator) |[Sütunları sonuçlardan kaldır](../samples/advanced.md#remove-column) | |
|[düzenine](/azure/kusto/query/sortoperator) |[Ada göre sıralanan kaynakları Listele](../samples/starter.md#list-resources) |Eş anlamlısı `order` |
|[ölçütü](/azure/kusto/query/summarizeoperator) |[Azure kaynaklarını sayma](../samples/starter.md#count-resources) |Yalnızca Basitleştirilmiş ilk sayfa |
|[take](/azure/kusto/query/takeoperator) |[Tüm genel IP adreslerini listele](../samples/starter.md#list-publicip) |Öğesinin eşanlamlısı `limit` . [Skip](./work-with-data.md#skipping-records)ile çalışmaz. |
|[Sayfanın Üstü](/azure/kusto/query/topoperator) |[Ada ve işletim sistemi türlerine göre ilk beş sanal makineyi göster](../samples/starter.md#show-sorted) | |
|[birleşim](/azure/kusto/query/unionoperator) |[İki sorgudan alınan sonuçları tek bir sonuç halinde birleştirin](../samples/advanced.md#unionresults) |Tek tablo izin verildi: _T_ `| union` \[ `kind=` `inner` \| `outer` \] \[ `withsource=` _ColumnName_ \] _tablosu_. `union`Tek bir sorgudaki 3 Tag sınırı. `union`Bacak tablolarının benzer çözümüne izin verilmez. Tek bir tablo içinde veya _kaynaklar_ Ile _resourcecontainers_ tabloları arasında kullanılabilir. |
|[olmadığı](/azure/kusto/query/whereoperator) |[Depolama içeren kaynakları göster](../samples/starter.md#show-storage) | |

## <a name="query-scope"></a>Sorgu kapsamı

Kaynakları bir sorgu tarafından döndürülen aboneliklerin kapsamı, kaynak grafiğine erişme yöntemine bağlıdır. Azure CLı ve Azure PowerShell yetkili kullanıcının bağlamına göre isteğe dahil edilecek Aboneliklerin listesini doldurun. Abonelikler listesi, sırasıyla **abonelikler** ve **abonelik** parametreleriyle birlikte el ile tanımlanabilir.
REST API ve diğer tüm SDK 'larda, kaynak dahil edilecek aboneliklerin listesi, isteğin bir parçası olarak açıkça tanımlanmalıdır.

Bir **Önizleme** olarak REST API sürüm, `2020-04-01-preview` sorgunun kapsamını bir [yönetim grubuna](../../management-groups/overview.md)eklemek için bir özellik ekler. Bu önizleme API 'SI de abonelik özelliğini isteğe bağlı hale getirir. Bir yönetim grubu veya abonelik listesi tanımlanmamışsa, sorgu kapsamı, kimliği doğrulanmış kullanıcının erişebileceği [Azure Use](../../../lighthouse/concepts/azure-delegated-resource-management.md) Temsilcili kaynaklar dahil olmak üzere tüm kaynaklarıdır. Yeni `managementGroupId` özellik, yönetim grubunun adından farklı olan yönetim grubu kimliğini alır. Belirtildiğinde `managementGroupId` , belirtilen yönetim grubu hiyerarşisinde veya altında ilk 5000 aboneliklerden kaynaklar dahil edilir. `managementGroupId` , ile aynı anda kullanılamaz `subscriptions` .

Örnek: ' myMG ' KIMLIĞIYLE ' My Management Group ' adlı Yönetim grubu hiyerarşisinde tüm kaynakları sorgulayın.

- REST API URI'si

  ```http
  POST https://management.azure.com/providers/Microsoft.ResourceGraph/resources?api-version=2020-04-01-preview
  ```

- İstek Gövdesi

  ```json
  {
      "query": "Resources | summarize count()",
      "managementGroupId": "myMG"
  }
  ```

## <a name="escape-characters"></a>Kaçış karakterleri

Ya da dahil olanlar gibi bazı özellik adları, `.` `$` sorgunun sarmalanması veya kaçışlanması ya da özellik adının yanlış yorumlanması ve beklenen sonuçları sağlamamalıdır.

- `.` -Özellik adını şu şekilde kaydırın: `['propertyname.withaperiod']`
  
  OData özelliğini sarmalayan örnek sorgu _. tür_:

  ```kusto
  where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.['odata.type']
  ```

- `$` -Özellik adındaki karakteri kaçış. Kullanılan kaçış karakteri, Shell kaynak grafiğine göre çalıştırılır.

  - **Bash** - `\`

    Bash içindeki özellik _\$ türünü_ iptal eden örnek sorgu:

    ```kusto
    where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.\$type
    ```

  - **cmd** -karakterden kaçmayın `$` .

  - **PowerShell** - ``` ` ```

    PowerShell 'deki özellik _\$ türünü_ iptal eden örnek sorgu:

    ```kusto
    where type=~'Microsoft.Insights/alertRules' | project name, properties.condition.`$type
    ```

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Başlangıç sorgularında](../samples/starter.md)kullanılan dil.
- Gelişmiş [sorgularda](../samples/advanced.md)gelişmiş kullanımlar bölümüne bakın.
- [Kaynakları araştırma](explore-resources.md)hakkında daha fazla bilgi edinin.
