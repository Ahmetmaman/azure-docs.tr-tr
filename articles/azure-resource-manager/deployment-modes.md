---
title: Azure Resource Manager dağıtım modları | Microsoft Docs
description: Azure Resource Manager ile tam veya artımlı dağıtım modu kullanıp kullanmayacağınızı açıklar.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2019
ms.author: tomfitz
ms.openlocfilehash: 618412f27efb71caf6e044b4768d7be00f0d0f47
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2019
ms.locfileid: "57009249"
---
# <a name="azure-resource-manager-deployment-modes"></a>Azure Resource Manager dağıtım modları

Kaynaklarınızı dağıtırken dağıtım Artımlı güncelleştirme ya da tam güncelleştirme olduğunu belirtin.  Bu iki mod arasındaki başlıca fark, Resource Manager şablonunda olmayan mevcut kaynaklar kaynak grubunda nasıl işlediğini ' dir. Artımlı varsayılan moddur.

Her iki mod için Resource Manager şablonunda belirtilen tüm kaynakları oluşturmaya çalışır. Kaynağın kaynak grubunda zaten mevcut ve ayarlarına aynıdır, işlem bu kaynak için alınır. Kaynak, bir kaynak için özellik değerlerini değiştirirseniz, bu yeni değerleri ile güncelleştirilir. Konum veya mevcut bir kaynak türünü güncelleştirmek çalışırsanız, dağıtım bir hata ile başarısız olur. Bunun yerine, yeni bir kaynak konumu ile dağıtın veya sizi gereken yazın.

## <a name="complete-mode"></a>Tam modda

Tam modda, Resource Manager **siler** kaynak grubunda var, ancak şablonda belirtilmeyen kaynakları. Şablonda belirtilen olan ancak çünkü dağıtılan kaynakları bir [koşul](resource-manager-templates-resources.md#condition) yanlış olarak değerlendirilir, silinmez.

Kaynak türleri tam modda silme işlemlerinin nasıl işleneceğini içinde bazı fark vardır. Bir şablon değil, tam modunda dağıtıldığında üst kaynaklar otomatik olarak silinir. Bazı alt kaynakları değil, şablon, otomatik olarak silinmez. Ancak, bu alt kaynak silinir üst kaynak silinir. 

Kaynak grubunuz bir DNS bölgesi (Microsoft.Network/dnsZones kaynak türü) ve bir CNAME kaydı (Microsoft.Network/dnsZones/CNAME kaynak türü) içeriyorsa, örneğin, DNS bölgesini üst CNAME kaydını kaynaktır. Tam modda ile dağıtma ve DNS bölgesini, şablonunuzda içermez, DNS bölgesi ve CNAME kaydı, hem de silinir. Varsa DNS bölgesi, şablona dahil ancak CNAME kaydı içermez, CNAME silinmez. 

Kaynak türleri silme nasıl gerçekleştirdiğine ilişkin bir listesi için bkz: [tam modda dağıtımlar için silme işlemi, Azure kaynaklarını](complete-mode-deletion.md).

> [!NOTE]
> Yalnızca kök düzeyinde şablonu tam dağıtım modunu destekler. İçin [bağlı veya iç içe şablonlar](resource-group-linked-templates.md), artımlı modu kullanmanız gerekir. 
>
> [Abonelik düzeyi dağıtımları](deploy-to-subscription.md) tam modu desteklemez.
>
> Şu anda, portalda tam modunu desteklemiyor.
>

## <a name="incremental-mode"></a>Artımlı modu

Artımlı modda, Resource Manager **değişmeden kalır** kaynak grubunda var, ancak şablonda belirtilmeyen kaynakları. Artımlı modda bir kaynak dağıtarak, yalnızca güncelleştirmekte olanlara kaynak için tüm özellik değerlerini belirtin. Bazı özellikler belirtmezseniz, Resource Manager güncelleştirme bu değerlerin üzerine yazar olarak yorumlar.

## <a name="example-result"></a>Örnek sonucu

Artımlı ve tam modları arasındaki farkı anlamak için aşağıdaki senaryoyu göz önünde bulundurun.

**Kaynak grubu** içerir:

* Kaynak
* Kaynak B
* C kaynak

**Şablon** içerir:

* Kaynak
* Kaynak B
* Kaynak D

Dağıtılmış olduğunda **artımlı** modu, kaynak grubu vardır:

* Kaynak
* Kaynak B
* C kaynak
* Kaynak D

Dağıtılmış olduğunda **tam** modu, kaynak C silinir. Kaynak grubu vardır:

* Kaynak
* Kaynak B
* Kaynak D

## <a name="set-deployment-mode"></a>Dağıtım modunu ayarlama

PowerShell ile dağıtım yaparken, dağıtım modu ayarlamak için kullanın `Mode` parametresi.

```azurepowershell-interactive
New-AzResourceGroupDeployment `
  -Mode Complete `
  -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json
```

Azure CLI ile dağıtım yaparken, dağıtım modu ayarlamak için kullanın `mode` parametresi.

```azurecli-interactive
az group deployment create \
  --name ExampleDeployment \
  --mode Complete \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS
```

Aşağıdaki örnek, artımlı dağıtım moduna bağlı bir şablon gösterir:

```json
"resources": [
  {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          <nested-template-or-external-template>
      }
  }
]
```

## <a name="next-steps"></a>Sonraki adımlar

* Resource Manager şablonları oluşturma hakkında bilgi edinmek için [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md).
* Kaynakları dağıtma hakkında bilgi edinmek için [Azure Resource Manager şablonu ile uygulama dağıtma](resource-group-template-deploy.md).
* Bir kaynak sağlayıcısı işlemleri görüntülemek için bkz: [Azure REST API'si](/rest/api/).
