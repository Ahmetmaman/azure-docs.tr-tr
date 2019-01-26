---
title: Azure SKU'nun kullanılabilir olmaması hatalarını | Microsoft Docs
description: SKU ilgili sorunları giderme açıklar dağıtımı sırasında kullanılamaz hatası.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/19/2018
ms.author: tomfitz
ms.openlocfilehash: 4688acbb2742579e0f9f3fbb2604ffd8ef12bfd5
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2019
ms.locfileid: "55081050"
---
# <a name="resolve-errors-for-sku-not-available"></a>Hataları gidermek için SKU kullanılamıyor

Bu makalede çözümlemek nasıl **SkuNotAvailable** hata. Uygun SKU'su bu bölgede bulamıyor ya da iş karşılayan alternatif bölgelere gönderme gerekli bir [SKU isteği](https://aka.ms/skurestriction) üzere Azure desteği.


## <a name="symptom"></a>Belirti

Bir kaynak (genellikle bir sanal makine) dağıtım yaparken, aşağıdaki hata kodu ve hata iletisi alırsınız:

```
Code: SkuNotAvailable
Message: The requested tier for resource '<resource>' is currently not available in location '<location>' 
for subscription '<subscriptionID>'. Please try another tier or deploy to a different location.
```

## <a name="cause"></a>Nedeni

SKU (VM boyutu gibi), seçtiğiniz kaynak seçtiğiniz konum için mevcut olmadığı durumlarda bu hatayı alırsınız.

## <a name="solution-1---powershell"></a>Çözüm 1 - PowerShell

Bir bölgede kullanılabilen SKU'ları belirlemek için [Get-AzComputeResourceSku](/powershell/module/az.compute/get-azcomputeresourcesku) komutu. Konuma göre sonuçları filtreleyin. Bu komut için en son PowerShell sürümünü olması gerekir.

```azurepowershell-interactive
Get-AzComputeResourceSku | where {$_.Locations -icontains "centralus"}
```

Sonuçlar, konum ve SKU için herhangi bir kısıtlama için SKU listesini içerir. Bir SKU olarak listelenebilir bildirimi `NotAvailableForSubscription`.

```powershell
ResourceType          Name        Locations   Restriction                      Capability           Value
------------          ----        ---------   -----------                      ----------           -----
virtualMachines       Standard_A0 centralus   NotAvailableForSubscription      MaxResourceVolumeMB   20480
virtualMachines       Standard_A1 centralus   NotAvailableForSubscription      MaxResourceVolumeMB   71680
virtualMachines       Standard_A2 centralus   NotAvailableForSubscription      MaxResourceVolumeMB  138240
```

## <a name="solution-2---azure-cli"></a>Çözüm 2 - Azure CLI

Bir bölgede kullanılabilen SKU'ları belirlemek için `az vm list-skus` komutu. Kullanım `--location` kullanmakta olduğunuz konuma çıkışı filtrelemek için parametre. Kullanım `--size` kısmi boyutu adına göre aramak için parametre.

```azurecli-interactive
az vm list-skus --location southcentralus --size Standard_F --output table
```

Komut, benzer bir sonuç döndürür:

```azurecli
ResourceType     Locations       Name              Zones    Capabilities    Restrictions
---------------  --------------  ----------------  -------  --------------  --------------
virtualMachines  southcentralus  Standard_F1                ...             None
virtualMachines  southcentralus  Standard_F2                ...             None
virtualMachines  southcentralus  Standard_F4                ...             None
...
```


## <a name="solution-3---azure-portal"></a>Çözüm 3 - Azure portalı

Bir bölgede kullanılabilen SKU'ları belirlemek için [portalı](https://portal.azure.com). Portalda oturum açın ve arabirimi aracılığıyla bir kaynak ekleyin. Değerleri ayarlamak gibi bu kaynak için kullanılabilir SKU'ları bakın. Dağıtımı tamamlamak gerekmez.

Örneğin, bir sanal makine oluşturma işlemi başlatın. Diğer kullanılabilir boyut görmek için seçin **değiştirme boyutu**.

![VM oluşturma](./media/resource-manager-sku-not-available-errors/create-vm.png)

Filtreleme ve kaydırma aracılığıyla kullanılabilir boyutları.

![Kullanılabilir SKU'lar](./media/resource-manager-sku-not-available-errors/available-sizes.png)

## <a name="solution-4---rest"></a>Çözüm 4 - REST

Bir bölgede kullanılabilen SKU'ları belirlemek için [kaynak SKU'ları - liste](/rest/api/compute/resourceskus/list) işlemi.

Kullanılabilir SKU'lar ve bölgeleri şu biçimde döndürür:

```json
{
  "value": [
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A0",
      "tier": "Standard",
      "size": "A0",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A1",
      "tier": "Standard",
      "size": "A1",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    ...
  ]
}
```

