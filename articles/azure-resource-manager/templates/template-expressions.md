---
title: Şablon sözdizimi ve ifadeleri
description: Azure Resource Manager şablonları için bildirim temelli JSON sözdizimini açıklar (ARM şablonları).
ms.topic: conceptual
ms.date: 03/17/2020
ms.openlocfilehash: 44a386ed849771dfba717c8d1414e64422d0c7bd
ms.sourcegitcommit: ab829133ee7f024f9364cd731e9b14edbe96b496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/28/2020
ms.locfileid: "97797052"
---
# <a name="syntax-and-expressions-in-arm-templates"></a>ARM şablonlarındaki sözdizimi ve ifadeler

Azure Resource Manager şablonunun (ARM şablonu) temel sözdizimi JavaScript Nesne Gösterimi (JSON). Ancak, şablon içinde kullanılabilir olan JSON değerlerini genişletmek için ifadeleri kullanabilirsiniz.  İfadeler köşeli ayraçla başlayıp biter: `[` ve `]`. İfadenin değeri, şablon dağıtıldığında değerlendirilir. İfade dize, tamsayı, boole, dizi veya nesne döndürebilir.

Şablon ifadesi 24.576 karakterden uzun olamaz.

## <a name="use-functions"></a>İşlev kullanma

Azure Resource Manager, bir şablonda kullanabileceğiniz [işlevleri](template-functions.md) sağlar. Aşağıdaki örnek, bir parametresinin varsayılan değerinde bir işlevi kullanan bir ifade gösterir:

```json
"parameters": {
  "location": {
    "type": "string",
    "defaultValue": "[resourceGroup().location]"
  }
},
```

İfade içinde sözdizimi, `resourceGroup()` Kaynak Yöneticisi bir şablon içinde kullanmak için sağladığı işlevlerden birini çağırır. Bu durumda, [resourceGroup](template-functions-resource.md#resourcegroup) işlevi. JavaScript içinde olduğu gibi, işlev çağrıları olarak biçimlendirilir `functionName(arg1,arg2,arg3)` . Söz dizimi, `.location` Bu işlev tarafından döndürülen nesneden bir özelliği alır.

Şablon işlevleri ve parametreleri büyük/küçük harfe duyarlıdır. Örneğin, Kaynak Yöneticisi `variables('var1')` `VARIABLES('VAR1')` , ve aynı şekilde çözümlenir. Değerlendirildiğinde, işlev açıkça durumu (veya gibi) değiştirmediği sürece `toUpper` `toLower` işlev, büyük/küçük harf durumunu korur. Belirli kaynak türlerinde, işlevlerin nasıl değerlendirildiğinden ayrı olarak durum gereksinimleri olabilir.

Bir parametreye parametre olarak bir dize değeri geçirmek için tek tırnak kullanın.

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]"
```

Çoğu işlev aynı şekilde bir kaynak grubuna, aboneliğe, yönetim grubuna veya kiracıya dağıtılıp aynı şekilde çalışır. Aşağıdaki işlevlerde kapsama dayalı kısıtlamalar vardır:

* [resourceGroup](template-functions-resource.md#resourcegroup) -yalnızca bir kaynak grubuna yapılan dağıtımlarda kullanılabilir.
* [RESOURCEID](template-functions-resource.md#resourceid) -herhangi bir kapsamda kullanılabilir, ancak geçerli parametreler kapsama göre değişir.
* [abonelik](template-functions-resource.md#subscription) -yalnızca bir kaynak grubuna veya aboneliğe yapılan dağıtımlarda kullanılabilir.

## <a name="escape-characters"></a>Kaçış karakterleri

Değişmez bir dizenin bir sol köşeli ayraç ile başlaması `[` ve sağ parantez ile bitmesi `]` , ancak bir ifade olarak yorumlanmaması için, dizeyi ile başlatmak için fazladan bir köşeli ayraç ekleyin `[[` . Örneğin, değişkeni:

```json
"demoVar1": "[[test value]"
```

Olarak çözümlenir `[test value]` .

Ancak, değişmez dize bir köşeli ayraç ile bitmezse ilk köşeli ayracı atmayın. Örneğin, değişkeni:

```json
"demoVar2": "[test] value"
```

Olarak çözümlenir `[test] value` .

Şablonda JSON nesnesi ekleme gibi bir ifadede çift tırnak işaretleri için ters eğik çizgi kullanın.

```json
"tags": {
    "CostCenter": "{\"Dept\":\"Finance\",\"Environment\":\"Production\"}"
},
```

Parametre değerlerini geçirirken, kaçış karakterlerinin kullanılması parametre değerinin nerede belirtildiğinize bağlıdır. Şablonda varsayılan bir değer ayarlarsanız, fazladan sol köşeli ayraç gerekir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "demoParam1":{
            "type": "string",
            "defaultValue": "[[test value]"
        }
    },
    "resources": [],
    "outputs": {
        "exampleOutput": {
            "type": "string",
            "value": "[parameters('demoParam1')]"
        }
    }
}
```

Varsayılan değeri kullanırsanız, şablon döndürür `[test value]` .

Ancak, komut satırı aracılığıyla bir parametre değeri geçirirseniz, karakterler tam olarak yorumlanır. Önceki şablonu ile dağıtma:

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName demoGroup -TemplateFile azuredeploy.json -demoParam1 "[[test value]"
```

`[[test value]` döndürür. Bunun yerine şunu kullanın:

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName demoGroup -TemplateFile azuredeploy.json -demoParam1 "[test value]"
```

Bir parametre dosyasından değerleri geçirirken aynı biçimlendirme geçerlidir. Karakterler tam olarak yorumlanır. Önceki şablonla birlikte kullanıldığında, aşağıdaki parametre dosyası şunu döndürür `[test value]` :

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "demoParam1": {
            "value": "[test value]"
        }
   }
}
```

## <a name="null-values"></a>Null değerler

Bir özelliği null olarak ayarlamak için `null` veya `[json('null')]` kullanabilirsiniz. [JSON işlevi](template-functions-object.md#json) , parametre olarak sağladığınızda boş bir nesne döndürür `null` . Her iki durumda da Kaynak Yöneticisi şablonlar, özelliği mevcut olmadığı gibi kabul eder.

```json
"stringValue": null,
"objectValue": "[json('null')]"
```

## <a name="next-steps"></a>Sonraki adımlar

* Şablon işlevlerinin tam listesi için bkz. [ARM şablon işlevleri](template-functions.md).
* Şablon dosyaları hakkında daha fazla bilgi için bkz. [ARM şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
