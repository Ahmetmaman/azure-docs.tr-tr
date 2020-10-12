---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-network
author: jimdial
ms.service: virtual-network
ms.topic: include
ms.date: 04/09/2018
ms.author: jdial
ms.custom: include file
ms.openlocfilehash: c63160d7514dccb0d2a9c2879db6d3fd614e1a96
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "75646406"
---
> [!div class="op_single_selector"]
> * [Azure portalındaki](../articles/virtual-network/virtual-network-multiple-ip-addresses-portal.md)
> * [PowerShell](../articles/virtual-network/virtual-network-multiple-ip-addresses-powershell.md)
> * [Azure CLI](../articles/virtual-network/virtual-network-multiple-ip-addresses-cli.md)
>

Bir Azure Sanal Makinesine (VM) bağlı bir veya daha fazla ağ arabirimi (NIC) vardır. Herhangi bir NIC’e atanmış bir veya daha fazla statik ya da dinamik ortak ve özel IP adresi olabilir. Bir sanal makineye birden fazla IP adresinin atanması aşağıdaki özellikleri sağlar:

* Tek bir sunucuda farklı IP adreslerine ve SSL sertifikalarına sahip birden fazla web sitesi veya hizmetin barındırılması.
* Güvenlik duvarı veya yük dengeleyici gibi bir sanal ağ gereci olarak görev yapma.
* NIC’lerin herhangi biri için herhangi bir özel IP adresini Azure Load Balancer arka uç havuzuna ekleyebilme. Geçmişte, arka uç havuzuna yalnızca birincil NIC’nin birincil IP adresi eklenebiliyordu. Birden fazla IP yapılandırmasının yük dengelemesi hakkında daha fazla bilgi için [Birden fazla IP yapılandırmasının yük dengelemesi](../articles/load-balancer/load-balancer-multiple-ip.md?toc=%2fazure%2fvirtual-network%2ftoc.json) makalesini okuyun.

Bir sanal makineye bağlanan her NIC ile ilişkili bir veya daha fazla IP yapılandırması vardır. Her yapılandırmaya bir statik veya dinamik özel IP adresi atanır. Her yapılandırmayla ilişkili bir genel IP adresi kaynağı da olabilir. Genel bir IP adresi kaynağına atanmış dinamik veya statik bir genel IP adresi vardır. Azure'daki IP adresleri hakkında daha fazla bilgi edinmek için [Azure’da IP adresleri](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md) makalesini okuyun. 

Bir NIC 'ye kaç özel IP adresi atanabileceği bir sınır vardır. Ayrıca, bir Azure aboneliğinde kullanılabilecek genel IP adreslerinin sayısı için bir sınır vardır. Ayrıntılar için [Azure limitleri](../articles/azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesini okuyun.
