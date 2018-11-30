---
title: Azure API Management'ta dış bir önbellek kullanma | Microsoft Docs
description: Yapılandırma ve Azure API Management'ta dış bir önbellek kullanma hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: erikre
editor: ''
ms.assetid: 740f6a27-8323-474d-ade2-828ae0c75e7a
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/27/2018
ms.author: apimpm
ms.openlocfilehash: f57b6b35ffff85aad4d970cf9aa908d2a80eadf1
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52621667"
---
# <a name="use-an-external-redis-cache-in-azure-api-management"></a>Azure API Management'te bir dış Redis önbelleğini kullanma

Yerleşik önbelleği kullanan ek olarak, Azure API Management Ayrıca dış bir Redis önbelleğinde yanıtları önbelleğe alma işlemi için sağlar.

Dış bir önbellek kullanmak, bazı sınırlamalar yerleşik bir önbellek üstesinden gelmek için sağlar. İsteyip istemediğini özellikle yararlıdır:

* API Management güncelleştirmeleri sırasında düzenli aralıklarla temizlenmiş önbelleğinizi olmamasına özen gösterin
* Önbelleği yapılandırma hakkında daha fazla denetime sahip
* API Management katmanınız için izin verdiğinden daha fazla veri önbelleği
* API Management tüketim katmanı ile önbelleğe almayı kullanma

Önbelleğe alma hakkında daha ayrıntılı bilgi için bkz. [API Management önbelleğe alma ilkeleri](api-management-caching-policies.md) ve [Azure API Management'te özel önbelleğe alma](api-management-sample-cache-by-key.md).

![APIM için kendi önbelleğinizi Getir](media/api-management-howto-cache-external/overview.png)

Öğrenecekleriniz:

> [!div class="checklist"]
> * API Yönetimi'nde bir dış önbellek Ekle

## <a name="availability"></a>Kullanılabilirlik

> [!NOTE]
> Bu özellik şu anda yalnızca kullanılabilir **tüketim** Azure API Yönetimi katmanı.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yapmanız gerekir:

+ [Azure API Management örneği oluşturma](get-started-create-service-instance.md)
+ Anlamak [Azure API Yönetimi'nde önbelleğe alma](api-management-howto-cache.md)

## <a name="create-cache"> </a> Azure Redis Cache oluşturma

Bu bölümde, Azure Redis cache oluşturma açıklanmaktadır. Redis önbelleği, içinde veya dışında Azure, zaten varsa <a href="#add-external-cache">atla</a> sonraki bölüme.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="add-external-cache"> </a>Bir dış önbellek Ekle

Azure API Yönetimi'nde bir dış Redis önbelleğine eklemek için aşağıdaki adımları izleyin.

![APIM için kendi önbelleğinizi Getir](media/api-management-howto-cache-external/add-external-cache.png)

> [!NOTE]
> **Kullanıldığında** ayar bölgesel dağıtım iletişim hangi API Management API yönetimi, çok bölgeli yapılandırmasını durumunda yapılandırılmış önbellek ile belirtir. Belirtilen önbellek **varsayılan** önbelleklerle bölgesel bir değer tarafından geçersiz kılınır.
>
> Örneğin, API Management, Doğu ABD, Güneydoğu Asya ve Batı Avrupa bölgelerinde barındırılır ve varsa iki önbellekler yapılandırılmış, biri **varsayılan** , diğeri **Güneydoğu Asya**, API Yönetimi'nde  **Güneydoğu Asya** diğer iki bölgeleri kullanırken, kendi önbellek kullanacağı **varsayılan** önbellek girişi.

### <a name="add-an-azure-redis-cache-from-the-same-subscription"></a>Azure Redis Cache aynı abonelikten Ekle

1. API Management örneğinizin Azure portalındaki göz atın.
2. Seçin **dış önbellek** sol taraftaki menüden sekmesi.
3. Tıklayın **+ Ekle** düğmesi.
4. Önbelleğinize seçin **önbellek örneği** alan açılır.
5. Seçin **varsayılan** veya istenen bölgede belirtin **kullanıldığında** alan açılır.
6. **Kaydet**’e tıklayın.

### <a name="add-a-redis-cache-hosted-outside-of-the-current-azure-subscription-or-azure-in-general"></a>Genel olarak geçerli bir Azure aboneliği veya Azure dışında barındırılan bir Redis önbelleği ekleme

1. API Management örneğinizin Azure portalındaki göz atın.
2. Seçin **dış önbellek** sol taraftaki menüden sekmesi.
3. Tıklayın **+ Ekle** düğmesi.
4. Seçin **özel** içinde **önbellek örneği** alan açılır.
5. Seçin **varsayılan** veya istenen bölgede belirtin **kullanıldığında** alan açılır.
6. Redis cache bağlantı dizenizi girin **bağlantı dizesi** alan.
7. **Kaydet**’e tıklayın.

## <a name="use-the-external-cache"></a>Dış önbellek kullanma

Dış önbellek, Azure API Yönetimi'nde yapılandırıldıktan sonra önbelleğe alma ilkeleri aracılığıyla kullanılabilir. Bkz: [Azure API Management performansını artırmak için önbelleğe alma ekleme](api-management-howto-cache.md) ayrıntılı adımlar için.

## <a name="next-steps"> </a>Sonraki adımlar
* Önbelleğe alma ilkeleri hakkında daha fazla bilgi için bkz. [API Management ilke başvurusunda][API Management policy reference] [Önbelleğe alma ilkeleri][Caching policies].
* Anahtar kullanım ilkesi ifadeleri hakkında daha fazla bilgi için bkz. [Azure API Management’te özel önbelleğe alma](api-management-sample-cache-by-key.md).

[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx