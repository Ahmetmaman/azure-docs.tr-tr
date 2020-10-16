---
title: ResourceSelector Kullanıcı arabirimi öğesi
description: Azure portal için Microsoft. Solutions. ResourceSelector Kullanıcı arabirimi öğesini açıklar. Mevcut kaynakların listesini almak için kullanılır.
author: tfitzmac
ms.topic: conceptual
ms.date: 07/13/2020
ms.author: tomfitz
ms.openlocfilehash: d4b21f092733f6ebb3c2bf5aa06c1d2782323730
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87100221"
---
# <a name="microsoftsolutionsresourceselector-ui-element"></a>Microsoft. Solutions. ResourceSelector Kullanıcı arabirimi öğesi

ResourceSelector, kullanıcıların bir abonelikten var olan bir kaynağı seçmesini sağlar.

## <a name="ui-sample"></a>UI örneği

![Microsoft. Solutions. ResourceSelector](./media/managed-application-elements/microsoft-solutions-resourceselector.png)

## <a name="schema"></a>Şema

```json
{
    "name": "storageSelector",
    "type": "Microsoft.Solutions.ResourceSelector",
    "label": "Select storage accounts",
    "resourceType": "Microsoft.Storage/storageAccounts",
    "options": {
        "filter": {
            "subscription": "onBasics",
            "location": "onBasics"
        }
    }
}
```

## <a name="sample-output"></a>Örnek çıktı

```json
"name": "{resource-name}",
"id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{resource-provider-namespace}/{resource-type}/{resource-name}",
"location": "{deployed-location}"
```

## <a name="remarks"></a>Açıklamalar

`resourceType`Özelliğinde, listede göstermek istediğiniz kaynak için kaynak sağlayıcısı ad alanı ve kaynak türü adı ' nı belirtin.

`filter`Özelliği, kaynaklar için kullanılabilir seçenekleri kısıtlar. Sonuçları konuma veya aboneliğe göre kısıtlayabilirsiniz. Yalnızca temel kavramlar ile eşleşen kaynakları göstermek için kullanın `onBasics` . Tüm kaynakları göstermek için kullanın `all` . Varsayılan değer: `all`.

## <a name="next-steps"></a>Sonraki adımlar

* UI tanımları oluşturmaya giriş için bkz. [Createuıdefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* UI öğelerindeki ortak özelliklerin açıklaması için bkz. [Createuıdefinition Elements](create-uidefinition-elements.md).
