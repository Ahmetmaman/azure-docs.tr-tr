---
title: Azure API yönetimi ilkesi örneği-Iletilen üstbilgi ekleme | Microsoft Docs
description: Azure API Management ilkesi örneği-arka uç API 'sinin doğru URL 'Ler oluşturmasına izin vermek için gelen istek içinde bir Iletilen üstbilginin nasıl ekleneceğini gösterir.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: e525029ae8eab086d11126a4e18958423e207aa1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "70067496"
---
# <a name="add-a-forwarded-header"></a>Iletilen üst bilgi ekle

Bu makalede, arka uç API 'sinin doğru URL 'Ler oluşturmasına izin vermek için gelen istekte bir Iletilen üstbilginin nasıl ekleneceğini gösteren bir Azure API yönetim ilkesi örneği gösterilmektedir. Bir ilke kodu ayarlamak veya düzenlemek için, [Ilke ayarlama veya düzenleme](../set-edit-policies.md)bölümünde açıklanan adımları izleyin. Diğer örnekleri görmek için bkz. [ilke örnekleri](../policy-samples.md).

## <a name="code"></a>Kod

Kodu **gelen** bloğa yapıştırın.

[!code-xml[Main](../../../api-management-policy-samples/examples/Forward gateway hostname to backend for generating correct urls in responses.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APıM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)
