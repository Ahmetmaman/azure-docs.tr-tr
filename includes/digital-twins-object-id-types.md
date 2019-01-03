---
title: include dosyası
description: include dosyası
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 12/20/2018
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: 40ab53c941a7ac619ebb09d381a4ae0450f26e8b
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2018
ms.locfileid: "53795930"
---
`objectIdType` (Veya **nesne tanımlayıcı türü**) bir role verilen kimlik türünü ifade eder. Gelen apart `DeviceId` ve `UserDefinedFunctionId` türleri, nesne tanımlayıcısı türleri için Azure Active Directory nesnelerin özelliklerini karşılık gelir.

Aşağıdaki tabloda Azure dijital İkizlerini desteklenen nesne tanımlayıcı türlerini içerir:

| Tür | Açıklama |
| --- | --- |
| UserId | Bir rol, bir kullanıcıya atar. |
| DeviceId | Bir rol için bir cihaz atar. |
| DomainName | Bir rol için bir etki alanı adı atar. Her kullanıcı belirtilen etki alanı adına karşılık gelen rolü erişim haklarına sahiptir. |
| TenantId | Bir rol için bir kiracı atar. Belirtilen ait her kullanıcının Azure AD Kiracı kimliği, karşılık gelen rolü erişim haklarına sahip. |
| Serviceprincipalıd | Bir rol için bir hizmet sorumlusu nesne kimliği atar. |
| UserDefinedFunctionId | Bir rol, kullanıcı tanımlı bir işlev için (UDF) atar. |