---
author: vhorne
ms.service: dns
ms.topic: include
ms.date: 11/25/2018
ms.author: victorh
ms.openlocfilehash: 74031a8dbc9b64d6a09533789eed1296ff334d47
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67189032"
---
DNS bölgesi, belirli bir etki alanına ait DNS kayıtlarını barındırmak için kullanılır. Etki alanınızı Azure DNS'de barındırmaya başlamak için bir DNS bölgesi oluşturmanız gerekir. Ardından bu DNS bölgesinde etki alanınız için tüm DNS kayıtları oluşturulur.

Örneğin "contoso.com" etki alanı, "mail.contoso.com" (bir posta sunucusu için) ve "www.contoso.com" (bir web sitesi için) gibi birden fazla DNS kaydını içerebilir.

Azure DNS'de bir DNS bölgesi oluştururken:

* Bölge adı kaynak grubunda benzersiz olmalıdır ve bölge daha önceden var olmamalıdır. Aksi takdirde işlem başarısız olur.
* Aynı bölge adı farklı bir kaynak grubunda veya farklı bir Azure aboneliğinde yeniden kullanılabilir.
* Birden fazla bölgenin aynı adı paylaştığı durumda her örneğe farklı bir ad sunucusu adresi atanır. Etki alanı kayıt şirketinde yalnızca bir adres kümesi yapılandırılabilir.

> [!NOTE]
> Azure DNS'de aynı etki alanıyla DNS bölgesi oluşturmak için bir etki alanına sahip değilsiniz. Ancak Azure DNS ad sunucularını etki alanı kayıt şirketinde etki alanı adı için doğru ad sunucuları olarak yapılandırmak üzere etki alanının sahibi olmanız gerekmez.
> 
> Daha fazla bilgi için bkz. [Bir etki alanını Azure DNS'ye devretme](../articles/dns/dns-domain-delegation.md).