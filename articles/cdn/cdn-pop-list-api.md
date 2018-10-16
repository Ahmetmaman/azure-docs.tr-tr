---
title: Geçerli Verizon POP listesini almak için Azure CDN | Microsoft Docs
description: REST API kullanarak geçerli Verizon POP listesini almak öğrenin.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: 34bd958260056d7a0297c4d3545d97543a363480
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344801"
---
# <a name="retrieve-the-current-verizon-pop-list-for-azure-cdn"></a>Azure CDN için geçerli Verizon POP listesini alma

Verizon'ın noktası varlık (POP) sunucuları için IP'ler kümesini almak için REST API kullanabilirsiniz. Bu POP sunucular Verizon profildeki Azure Content Delivery Network (CDN) uç noktası ile ilişkili kaynak sunuculara isteklerde (**verizon'dan Azure CDN standart** veya **verizon'danAzureCDNPremium**). Bu IP'ler kümesini istemci isteklerini Pop'lere yaparken göreceğiniz ıp'lerden farklı olduğunu unutmayın. 

POP listesini almak için REST API işlemi söz dizimi için bkz. [kenar düğümleri - liste](https://docs.microsoft.com/rest/api/cdn/edgenodes/edgenodes_list).

## <a name="typical-use-case"></a>Tipik kullanım örneği

Güvenlik nedeniyle, yalnızca geçerli Verizon POP istekleri kaynak sunucunuza yapıldığını zorlamak için bu IP listesi kullanabilirsiniz. Örneğin, birisi konak adı veya IP adresi için bir CDN uç noktasının kaynak sunucu bulunan, bir Azure CDN tarafından sağlanan güvenlik özellikleri ve bu nedenle ölçeklendirme atlayarak, kaynak sunucu için doğrudan istekleri yapabilirsiniz. Bu senaryoda bir kaynak sunucuda tek IP'leri izin gibi IP'ler döndürülen listede ayarlayarak, engellenebilir. Bunlar emin olmak için günde bir en az bir kez almak en son POP listesi vardır. 

## <a name="next-steps"></a>Sonraki adımlar

REST API hakkında daha fazla bilgi için bkz. [Azure CDN REST API](https://docs.microsoft.com/rest/api/cdn/).
