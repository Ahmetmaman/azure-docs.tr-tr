---
title: Sanal makineler için Azure ayrılmış konaklarına genel bakış
description: Azure adanmış ana bilgisayarlarının Linux sanal makinelerini dağıtmak için nasıl kullanılabileceği hakkında daha fazla bilgi edinin.
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.date: 07/28/2020
ms.author: cynthn
ms.openlocfilehash: acf79448c0dcb81bbe644be822414a7e51b0ee36
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91328165"
---
# <a name="azure-dedicated-hosts-for-virtual-machines"></a>Sanal makineler için Azure ayrılmış Konakları

Azure adanmış ana bilgisayar, bir veya daha fazla sanal makineyi bir Azure aboneliğine ayrılmış olarak barındırabilecek fiziksel sunucular sağlayan bir hizmettir. Adanmış konaklar, kaynak olarak sağlanmış olan veri merkezlerimizde kullanılan fiziksel sunuculardır. Bir bölge, kullanılabilirlik alanı ve hata etki alanı içinde adanmış konaklar sağlayabilirsiniz. Daha sonra, gereksinimlerinizi en iyi şekilde karşılayan her yapılandırma için VM 'Leri doğrudan sağlanan konaklarınıza yerleştirebilirsiniz.



[!INCLUDE [virtual-machines-common-dedicated-hosts](../../../includes/virtual-machines-common-dedicated-hosts.md)]


## <a name="next-steps"></a>Sonraki adımlar

- [Azure CLI](dedicated-hosts-cli.md), [Portal](dedicated-hosts-portal.md)ve [PowerShell](../windows/dedicated-hosts-powershell.md)'i kullanarak adanmış bir konak dağıtabilirsiniz.

- Daha fazla bilgi için bkz. [adanmış ana bilgisayarlara](dedicated-hosts.md) genel bakış.

- [Burada](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md), bir bölgedeki maksimum dayanıklılık için hem bölge hem de hata etki alanı kullanan örnek şablon vardır.

- Ayrıca, [ayrılmış bir Azure ayrılmış ana bilgisayar örneğiyle](../prepay-dedicated-hosts-reserved-instances.md)maliyetlerle tasarruf edebilirsiniz.
