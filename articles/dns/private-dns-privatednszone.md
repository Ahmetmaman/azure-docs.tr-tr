---
title: Azure DNS özel bölgesi nedir?
description: Özel bir DNS bölgesine genel bakış
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 9/24/2019
ms.author: rohink
ms.openlocfilehash: 9eaa320e79f1d595303c6d9fe1399df12cb6c52b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94954418"
---
# <a name="what-is-a-private-azure-dns-zone"></a>Özel Azure DNS bölgesi nedir?

Azure Özel DNS, bir sanal ağdaki etki alanı adlarını yönetmek ve çözümlemek için özel bir DNS çözümü eklemeye gerek kalmadan güvenilir, güvenli bir DNS hizmeti sağlar. Özel DNS bölgelerini kullanarak, bugün kullanılabilir olan Azure tarafından sağlanan adlar yerine kendi özel etki alanı adlarınızı kullanabilirsiniz. 

Özel bir DNS bölgesinde bulunan kayıtlar Internet 'ten çözümlenemez. Özel bir DNS bölgesine karşı DNS çözümlemesi yalnızca kendisiyle bağlantılı olan sanal ağlardan işe yarar.

[Sanal ağ bağlantıları](./private-dns-virtual-network-links.md)oluşturarak, özel bir DNS bölgesini bir veya daha fazla sanal ağa bağlayabilirsiniz.
Ayrıca, bir sanal ağda dağıtılan sanal makineler için DNS kayıtlarının yaşam döngüsünü otomatik olarak yönetmek üzere [otomatik kayıt](./private-dns-autoregistration.md) özelliğini de etkinleştirebilirsiniz.

## <a name="limits"></a>Sınırlar

Bir abonelikte kaç tane özel DNS bölgesinin oluşturabileceğiniz ve özel bir DNS bölgesinde kaç kayıt kümesinin desteklendiği hakkında bilgi edinmek için bkz. [Azure DNS sınırları](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-dns-limits)

## <a name="restrictions"></a>Kısıtlamalar

* Tek etiketli özel DNS bölgeleri desteklenmez. Özel DNS bölgeniz iki veya daha fazla etikete sahip olmalıdır. Örneğin contoso.com, noktayla ayrılmış iki etikete sahiptir. Özel bir DNS bölgesi en fazla 34 etikete sahip olabilir.
* Özel bir DNS bölgesinde bölge temsilcileri (NS kayıtları) oluşturamazsınız. Bir alt etki alanı kullanmayı planlıyorsanız, etki alanını özel bir DNS bölgesi olarak doğrudan oluşturabilir ve üst bölgeden bir ad sunucusu temsili ayarlamadan sanal ağa bağlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure PowerShell](./private-dns-getstarted-powershell.md) veya [Azure CLI](./private-dns-getstarted-cli.md)kullanarak Azure DNS bir özel bölge oluşturmayı öğrenin.

* Azure DNS özel bölgelerle gerçekleştirilen bazı yaygın [özel bölge senaryoları](./private-dns-scenarios.md) hakkında bilgi edinin.

* Belirli işlemler için bekleneceğiniz belirli davranış dahil olmak üzere Azure DNS özel bölgeler hakkında sık sorulan sorular ve yanıtlar için, bkz. [özel DNS SSS](./dns-faq-private.md).