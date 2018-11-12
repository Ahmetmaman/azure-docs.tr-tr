---
title: Azure kaynaklarını birden çok örneğini dağıtma | Microsoft Docs
description: Kopyalama işlemi ve kullanma diziler bir Azure Resource Manager şablonunda yinelemek için birden çok kez kaynakları dağıtırken.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
editor: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2018
ms.author: tomfitz
ms.openlocfilehash: e1edf0ed0c9efcb9f0c81718621706550bf3c4d7
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51012012"
---
# <a name="deploy-more-than-one-instance-of-a-resource-or-property-in-azure-resource-manager-templates"></a>Bir kaynağa veya Azure Resource Manager şablonları özelliğinde birden fazla örneğini dağıtma

Bu makalede bir kaynağın birden fazla örneğini oluşturmak için Azure Resource Manager şablonunuzda yineleme gösterilmektedir. Belirtmeniz gerekiyorsa olup kaynağın dağıtıldığı tüm bkz [koşul öğesi](resource-manager-templates-resources.md#condition).

Bir öğretici için bkz. [öğretici: Resource Manager şablonlarını kullanarak birden çok kaynak örneğini oluşturma](./resource-manager-tutorial-create-multiple-instances.md).

## <a name="resource-iteration"></a>Kaynak yineleme

Bir kaynak bir veya daha fazla örneğini oluşturmak için dağıtım sırasında karar verdiğinizde, ekleme bir `copy` kaynak türü için öğesi. Kopyalama öğesinde, bu döngü için yineleme sayısı ve bir ad belirtin. Sayısı değeri pozitif bir tamsayı olmalıdır ve 800'den fazla olamaz. 

Kaynak birkaç kez oluşturmak için aşağıdaki biçimi alır:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        }
    ],
    "outputs": {}
}
```

Her kaynağın adını içeren bir bildirim `copyIndex()` geçerli yineleme döngüsünde döndüren işlev. `copyIndex()` sıfır tabanlıdır. Bunu, aşağıdaki örnekte:

```json
"name": "[concat('storage', copyIndex())]",
```

Bu adlar oluşturur:

* storage0
* storage1
* storage2.

Dizin değerini kaydırmak için copyIndex () işlevine bir değer geçirebilirsiniz. Copy öğesinde gerçekleştirmek için yineleme sayısını hala belirtilmiş ancak Copyındex değerini belirtilen değere göre uzaklığını. Bunu, aşağıdaki örnekte:

```json
"name": "[concat('storage', copyIndex(1))]",
```

Bu adlar oluşturur:

* storage1
* storage2
* Depolaması3

Kopyalama işlemi, dizideki her öğe yinelemek çünkü dizilerle çalışırken yararlıdır. Kullanım `length` yineleme sayısını belirtmek için bir dizi işlev ve `copyIndex` dizideki geçerli dizin alınamıyor. Bunu, aşağıdaki örnekte:

```json
"parameters": { 
  "org": { 
     "type": "array", 
     "defaultValue": [ 
         "contoso", 
         "fabrikam", 
         "coho" 
      ] 
  }
}, 
"resources": [ 
  { 
      "name": "[concat('storage', parameters('org')[copyIndex()])]", 
      "copy": { 
         "name": "storagecopy", 
         "count": "[length(parameters('org'))]" 
      }, 
      ...
  } 
]
```

Bu adlar oluşturur:

* storagecontoso
* storagefabrikam
* storagecoho

Varsayılan olarak, Resource Manager kaynakları paralel olarak oluşturur. Oluşturulan sırasını garanti yoktur. Ancak, kaynakları sırayla dağıtılır belirtmek isteyebilirsiniz. Örneğin, bir üretim ortamında güncelleştirirken güncelleştirmeleri şekilde basamaklandırmak isteyebileceğiniz herhangi bir anda yalnızca belirli sayıda güncelleştirilir.

Bir kaynağın birden fazla örneği seri olarak dağıtmak için ayarlanmış `mode` için **seri** ve `batchSize` teker teker dağıtılacak örnek sayısı. Önceki toplu işin tamamlanana kadar tek bir toplu başlamaz seri moduyla Resource Manager döngünün önceki örnekleri üzerinde bir bağımlılık oluşturur.

Örneğin, iki depolama hesapları aynı anda seri olarak dağıtmak için kullanın:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 4,
                "mode": "serial",
                "batchSize": 2
            }
        }
    ],
    "outputs": {}
}
``` 

Mod özelliği de kabul eder **paralel**, varsayılan değer olan.

## <a name="property-iteration"></a>Özellik yineleme

Bir kaynak üzerinde bir özellik için birden fazla değer oluşturmak için bir `copy` özellikler öğesindeki dizisi. Bu dizi nesnelerini içerir ve her nesne aşağıdaki özelliklere sahiptir:

* ad - çeşitli değerleri oluşturmak için özellik adı
* Count - oluşturulacak değer sayısı. Sayısı değeri pozitif bir tamsayı olmalıdır ve 800'den fazla olamaz.
* Giriş - özelliğe atanacak değerleri içeren bir nesne  

Aşağıdaki örnek nasıl uygulanacağını gösterir `copy` Storageprofile özelliğine bir sanal makinede:

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "copy": [{
          "name": "dataDisks",
          "count": 3,
          "input": {
              "lun": "[copyIndex('dataDisks')]",
              "createOption": "Empty",
              "diskSizeGB": "1023"
          }
      }],
      ...
```

Kullanırken dikkat `copyIndex` özelliği yineleme içinde yineleme adı sağlamanız gerekir. Kaynak yineleme ile kullanıldığında adı sağlamanız gerekmez.

Resource Manager'ı genişletir `copy` dağıtımı sırasında bir dizi. Dizi adını, özelliğin adı olur. Giriş değerleri, nesne özelliklerini haline gelir. Dağıtılan şablon olur:

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "dataDisks": [
          {
              "lun": 0,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          },
          {
              "lun": 1,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          },
          {
              "lun": 2,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          }
      }],
      ...
```

Copy öğesinde bir dizi olduğundan kaynak için birden fazla özelliğe belirtebilirsiniz. Oluşturmak her bir özellik için bir nesne ekleyin.

```json
{
    "name": "string",
    "type": "Microsoft.Network/loadBalancers",
    "apiVersion": "2017-10-01",
    "properties": {
        "copy": [
          {
              "name": "loadBalancingRules",
              "count": "[length(parameters('loadBalancingRules'))]",
              "input": {
                ...
              }
          },
          {
              "name": "probes",
              "count": "[length(parameters('loadBalancingRules'))]",
              "input": {
                ...
              }
          }
        ]
    }
}
```

Kaynak ve özellik yineleme birlikte kullanabilirsiniz. Ada göre özellik yineleme başvuru.

```json
{
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[concat(parameters('vnetname'), copyIndex())]",
    "apiVersion": "2018-04-01",
    "copy":{
        "count": 2,
        "name": "vnetloop"
    },
    "location": "[resourceGroup().location]",
    "properties": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "copy": [
            {
                "name": "subnets",
                "count": 2,
                "input": {
                    "name": "[concat('subnet-', copyIndex('subnets'))]",
                    "properties": {
                        "addressPrefix": "[variables('subnetAddressPrefix')[copyIndex('subnets')]]"
                    }
                }
            }
        ]
    }
}
```

## <a name="variable-iteration"></a>Değişken yineleme

Bir değişken birden çok örneğini oluşturmak için kullanın `copy` değişkenler bölümünde öğesi. İle ilişkili değerler nesnelerin birden çok örnek oluşturma ve ardından kaynak örnekleri için değerler atayın. Bir dizi özelliği ya da dizi ya da nesne oluşturmak için kopyalama kullanabilirsiniz. Her iki yaklaşım, aşağıdaki örnekte gösterilmiştir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {
    "disk-array-on-object": {
      "copy": [
        {
          "name": "disks",
          "count": 5,
          "input": {
            "name": "[concat('myDataDisk', copyIndex('disks', 1))]",
            "diskSizeGB": "1",
            "diskIndex": "[copyIndex('disks')]"
          }
        }
      ]
    },
    "copy": [
      {
        "name": "disks-top-level-array",
        "count": 5,
        "input": {
          "name": "[concat('myDataDisk', copyIndex('disks-top-level-array', 1))]",
          "diskSizeGB": "1",
          "diskIndex": "[copyIndex('disks-top-level-array')]"
        }
      }
    ]
  },
  "resources": [],
  "outputs": {
    "exampleObject": {
      "value": "[variables('disk-array-on-object')]",
      "type": "object"
    },
    "exampleArrayOnObject": {
      "value": "[variables('disk-array-on-object').disks]",
      "type" : "array"
    },
    "exampleArray": {
      "value": "[variables('disks-top-level-array')]",
      "type" : "array"
    }
  }
}
```

Birden fazla değişken belirtebilmeniz için her iki yaklaşım ile copy öğesinde bir dizidir. Bir nesne oluşturmak her bir değişken ekleyin.

```json
"copy": [
  {
    "name": "first-variable",
    "count": 5,
    "input": {
      "demoProperty": "[concat('myProperty', copyIndex('first-variable'))]",
    }
  },
  {
    "name": "second-variable",
    "count": 3,
    "input": {
      "demoProperty": "[concat('myProperty', copyIndex('second-variable'))]",
    }
  },
]
```

## <a name="depend-on-resources-in-a-loop"></a>Kaynakları bir döngüye bağlıdır
Bir kaynak başka bir kaynak sonra kullanılarak dağıtılan belirttiğiniz `dependsOn` öğesi. Döngü içinde kaynak koleksiyonunu bağımlı kaynak dağıtmak için kopyalama döngüsü dependsOn öğesinde adını sağlayın. Aşağıdaki örnek, sanal makineyi dağıtmadan önce üç depolama hesapları dağıtmayı gösterir. Tam sanal makine tanımı gösterilmiyor. Copy öğesinde ayarlanan adı olduğuna dikkat edin `storagecopy` ve dependsOn öğe sanal makineleri için ayrıca kümesine `storagecopy`.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        },
        {
            "apiVersion": "2015-06-15", 
            "type": "Microsoft.Compute/virtualMachines", 
            "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
            "dependsOn": ["storagecopy"],
            ...
        }
    ],
    "outputs": {}
}
```

<a id="looping-on-a-nested-resource" />

## <a name="iteration-for-a-child-resource"></a>Yineleme alt kaynak
Alt kaynak için bir kopyalama döngüsü kullanamazsınız. Genellikle iç içe geçmiş içinde başka bir kaynak olarak tanımlayan bir kaynağın birden fazla örneği oluşturmak için bunun yerine, kaynak düzey bir kaynakla tam oluşturmanız gerekir. İlişkiyi üst kaynak türü ve adı özellikleri aracılığıyla ile tanımlayabilirsiniz.

Örneğin, bir veri fabrikası içinde alt kaynak olarak bir veri kümesi tanımlarsınız varsayalım.

```json
"resources": [
{
    "type": "Microsoft.DataFactory/datafactories",
    "name": "exampleDataFactory",
    ...
    "resources": [
    {
        "type": "datasets",
        "name": "exampleDataSet",
        "dependsOn": [
            "exampleDataFactory"
        ],
        ...
    }
}]
```

Birden fazla veri kümesi oluşturmak için data factory dışında taşıyın. Veri kümesi data factory ile aynı düzeyde olması gerekir, ancak hala bir data Factory alt kaynak. Veri kümesi ve veri fabrikası tür ve ad özellikleri aracılığıyla arasındaki ilişkiyi korur. Türü artık içinden şablonu içindeki konumu gösterilen bu yana, tam olarak nitelenmiş tür biçiminde sağlamalısınız: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.

Data Factory örneğiyle birlikte bir üst/alt ilişkisi oluşturmak için üst kaynak adını içeren bir veri kümesi için bir ad sağlayın. Biçimini kullanın: `{parent-resource-name}/{child-resource-name}`.  

Aşağıdaki örnek, bir uygulama gösterilmektedir:

```json
"resources": [
{
    "type": "Microsoft.DataFactory/datafactories",
    "name": "exampleDataFactory",
    ...
},
{
    "type": "Microsoft.DataFactory/datafactories/datasets",
    "name": "[concat('exampleDataFactory', '/', 'exampleDataSet', copyIndex())]",
    "dependsOn": [
        "exampleDataFactory"
    ],
    "copy": { 
        "name": "datasetcopy", 
        "count": "3" 
    } 
    ...
}]
```

## <a name="example-templates"></a>Örnek şablonları

Aşağıdaki örnekler, bir kaynak ya da özellik birden fazla örneğini oluşturmak için yaygın senaryoları gösterir.

|Şablon  |Açıklama  |
|---------|---------|
|[Kopya depolama alanı](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystorage.json) |Bir dizin numarasını ad ile birden fazla depolama hesabı dağıtır. |
|[Seri kopya depolama alanı](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/serialcopystorage.json) |Birkaç depolama hesabı bir anda dağıtır. Ad, dizin numarasını içerir. |
|[Depolama dizisi ile kopyalama](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystoragewitharray.json) |Birkaç depolama hesabı dağıtır. Ad, bir dizi arasında bir değer içerir. |
|[Değişken sayıda veri diski ile VM dağıtımı](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-windows-copy-datadisks) |Bir sanal makine ile birden fazla veri diski dağıtır. |
|[Değişkenleri kopyalayın](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) |Yineleme değişkenleri farklı yollarını gösterir. |
|[Birden çok güvenlik kuralları](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) |Birçok güvenlik kuralı için bir ağ güvenlik grubu dağıtır. Bu, bir parametre gelen güvenlik kuralları oluşturur. Parametresi için bkz: [birden fazla NSG parametre dosyası](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json). |

## <a name="next-steps"></a>Sonraki adımlar

* Bir öğreticiyi incelemek için bkz: [öğretici: Resource Manager şablonlarını kullanarak birden çok kaynak örneğini oluşturma](./resource-manager-tutorial-create-multiple-instances.md).

* Bir şablon bölümleri hakkında bilgi edinmek istiyorsanız bkz [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Şablonunuzu dağıtmak nasıl öğrenmek için bkz. [Azure Resource Manager şablonu ile uygulama dağıtma](resource-group-template-deploy.md).

