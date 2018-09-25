---
title: include dosyası
description: include dosyası
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: df9c291136fc6a48effb08cd59eeb66486eb641c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47003969"
---
### <a name="virtual-networks"></a>Sanal Ağlar

|  |  |
|---------|---------|
| [İzin verilen Application Gateway SKU’ları](../articles/governance/policy/samples/allowed-app-gate-sku.md) | Uygulama ağ geçitlerinin onaylı bir SKU kullanmasını gerektirir. Onaylı bir SKU dizisi belirtirsiniz. |
| [ER ağı ile ağ eşlemesi yok](../articles/governance/policy/samples/no-peering-er-net.md) | Bir ağ eşlemesinin belirli bir kaynak grubundaki bir ağ ile ilişkilendirilmesini önler. Merkezi yönetilen ağ altyapısı ile bağlantıyı engellemek için kullanın. İlişkilendirmeyi önlemek için kaynak grubunun adını belirtirsiniz. |
| [Kullanıcı Tanımlı Yol Tablosu Yok](../articles/governance/policy/samples/no-user-def-route-table.md)  |Sanal ağların kullanıcı tanımlı bir yönlendirme tablosu ile dağıtılmasını engeller. |
| [Her alt ağ üzerinde NSG X](../articles/governance/policy/samples/nsg-on-subnet.md) | Her sanal alt ağ ile belirli bir ağ güvenlik grubunun kullanılmasını gerektirir. Kullanılacak ağ güvenlik grubunun kimliğini belirtirsiniz. |
| [VM ağ arabirimlerinde onaylanan alt ağı kullan](../articles/governance/policy/samples/use-approved-subnet-vm-nics.md) | Ağ arabirimlerinin onaylı bir alt ağ kullanmasını gerektirir. Onaylanan alt ağın kimliğini belirtirsiniz. |
| [VM ağ arabirimlerinde onaylanan sanal ağı kullan](../articles/governance/policy/samples/use-approved-vnet-vm-nics.md) | Ağ arabirimlerinin onaylı bir sanal ağ kullanmasını gerektirir. Onaylanan sanal ağın kimliğini belirtirsiniz. |