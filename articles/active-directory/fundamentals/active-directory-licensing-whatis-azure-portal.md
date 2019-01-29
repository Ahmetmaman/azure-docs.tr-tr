---
title: Hangi Grup tabanlı lisanslama - Azure Active Directory | Microsoft Docs
description: Nasıl çalıştığını ve en iyi uygulamalar da dahil olmak üzere Azure Active Directory grup tabanlı lisanslama hakkında bilgi edinin.
services: active-directory
keywords: Azure AD lisanslama
author: eross-msft
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.topic: conceptual
ms.workload: identity
ms.date: 10/29/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.openlocfilehash: c28895a81128af2276bebedcd043e4cc740a4666
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2019
ms.locfileid: "55093501"
---
# <a name="what-is-group-based-licensing-in-azure-active-directory"></a>Grup tabanlı Azure Active Directory lisansı nedir?

Microsoft'un ücretli bulut hizmetleri Office 365, Enterprise Mobility + Security, Dynamics 365 ve diğer benzer ürünler lisans gerektirir. Bu lisanslar, bu hizmetlere erişmesi gereken her kullanıcıya atanır. Lisansları yönetmek için, yöneticiler yönetim portallarından birini (Office veya Azure) ve PowerShell cmdlet'lerini kullanır. Azure Active Directory (Azure AD), tüm Microsoft bulut hizmetleri için kimlik yönetimini destekleyen temel altyapıdır. Azure AD kullanıcıların lisans atama durumları hakkındaki bilgileri depolar.

Şimdiye kadar, lisanslar yalnızca tek tek kullanıcı düzeyinde atanabiliyordu ve bu da geniş ölçekli yönetimi zorlaştırabiliyordu. Örneğin, kuruluşta veya bölümde yeni katılan veya ayrılan kullanıcılar gibi kurumsal değişikliklere göre kullanıcı lisansları eklemek veya kaldırmak için, yöneticinin çoğunlukla karmaşık bir PowerShell betiği yazması gerekir. Bu betik bulut hizmetine tek tek çağrılar yapar.

Bu zorlukları gidermek için, Azure AD artık grup tabanlı lisanslama özelliğini içermektedir. Gruba bir veya birden çok ürün lisansı atayabilirsiniz. Azure AD lisansların tüm grup üyelerine atanmasını güvence altına alır. Gruba katılan tüm yeni üyelere uygun lisanslar atanır. Kullanıcılar gruptan ayrıldığında, söz konusu lisanslar kaldırılır. Bu lisans yönetimi kullanıcı başına esasına göre kuruluş ve departman yapısına değişiklikleri yansıtacak şekilde PowerShell aracılığıyla Lisans Yönetimi otomatik hale getirmek için ihtiyacını ortadan kaldırır.

## <a name="licensing-requirements"></a>Lisans gereksinimleri
Grup tabanlı lisanslama kullanmak için aşağıdaki lisanslardan birini olmalıdır:

- Azure AD temel için ücretli veya deneme aboneliği

- Office 365 Kurumsal E3 veya Office 365 A3 ve yukarıdaki ücretli veya deneme sürümü

### <a name="required-number-of-licenses"></a>Gerekli lisans sayısı
Bir lisans atanmış tüm gruplar için benzersiz her üyesi için bir Lisansı olmalıdır. Bir lisans grubunun her üyesi atamanız gerekmez ancak tüm üyeleri dahil etmek için en az yeterince lisansa sahip olmalıdır. Örneğin, kiracınızdaki lisanslı gruplarının bir parçası olan 1000 benzersiz üyeleriniz varsa, lisans sözleşmesini karşılamak için en az 1000 Lisansı olmalıdır.

## <a name="features"></a>Özellikler

Grup tabanlı lisanslamanın başlıca özellikleri şunlardır:

- Lisanslar Azure AD'de her güvenlik grubuna atanabilir. Güvenlik grupları, Azure AD Connect kullanarak şirket içi, eşitlenebilir. Ayrıca, güvenlik gruplarını doğrudan Azure AD'de (yalnızca bulut grupları olarak da adlandırılır) veya Azure AD dinamik grup özelliği yoluyla otomatik olarak oluşturabilirsiniz.

- Gruba bir ürün lisansı atandığında, yönetici üründe bir veya birden çok hizmet planını devre dışı bırakabilir. Genellikle, kuruluş henüz bir üründe bir hizmeti kullanmaya başlamak için hazır olmadığında bu atama gerçekleştirilir. Örneğin, yönetici bir bölüme Office 365'i atayabilir ama Yammer hizmetini geçici olarak devre dışı bırakabilir.

- Kullanıcı düzeyi lisanslama gerektiren tüm Microsoft bulut hizmetleri desteklenir. Bu, tüm Office 365 ürünleri, Enterprise Mobility + Security ve Dynamics 365 destekler.

- Grup tabanlı lisanslama şu anda yalnızca [Azure portal](https://portal.azure.com) üzerinden sağlanır. Kullanıcı ve grup yönetimi için öncelikli olarak Office 365 portalı gibi diğer yönetim portallarını kullanıyorsanız, bunu yapmaya devam edebilirsiniz. Ancak, lisansları grup düzeyinde yönetmek için Azure portalını kullanmalısınız.

- Azure AD, grup üyeliği değişikliklerinden kaynaklanan lisans değişikliklerini otomatik olarak yönetir. Normalde, lisans değişiklikleri üyelik değişikliğini izleyen birkaç dakika içinde geçerlilik kazanır.

- Kullanıcı, lisans ilkeleri belirtilmiş olan birden çok gruba üye olabilir. Ayrıca kullanıcının, herhangi bir grubun dışında doğrudan atanmış bazı lisansları da olabilir. Sonuçta elde edilen kullanıcı durumu tüm atanan ürün ve hizmet lisanslarının bir bileşimidir. Birden fazla kaynaktan atanmış bir kullanıcı aynı lisans, lisans yalnızca bir kez tüketilir.

- Bazı durumlarda, bir kullanıcıya lisans atanamayabilir. Örneğin, kiracıda yeterli kullanılabilir lisans olmayabilir veya aynı anda çakışan hizmetler atanmış olabilir. Yöneticiler, Azure AD'nin grup lisanslarını tam olarak işleyemediği kullanıcılar hakkındaki bilgilere erişebilir. Bu bilgiler temelinde düzeltme eylemi gerçekleştirebilir.

## <a name="your-feedback-is-welcome"></a>Geri bildiriminiz bizim için çok önemlidir!

Geri bildirim ya da özellik isteğiniz varsa, bunları kullanarak bizimle paylaşın [Azure AD yönetici Forumu](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=162510).

## <a name="next-steps"></a>Sonraki adımlar

Grup tabanlı lisanslama aracılığıyla lisans yönetimine yönelik diğer senaryolar hakkında daha fazla bilgi edinmek için bkz:

* [Azure Active Directory'de gruba lisans atama](../users-groups-roles/licensing-groups-assign.md)
* [Azure Active Directory'de grubun lisans sorunlarını tanımlama ve çözme](../users-groups-roles/licensing-groups-resolve-problems.md)
* [Azure Active Directory'de tek tek lisanslı kullanıcıları grup tabanlı lisanslamaya geçirme](../users-groups-roles/licensing-groups-migrate-users.md)
* [Kullanıcılar Azure Active Directory'de Grup tabanlı lisanslama kullanarak ürün lisansları arasında geçirme](../users-groups-roles/licensing-groups-change-licenses.md)
* [Azure Active Directory grup tabanlı lisanslamayla ilgili ek senaryolar](../users-groups-roles/licensing-group-advanced.md)
* [Azure Active Directory'de Grup tabanlı lisanslama için PowerShell örnekleri](../users-groups-roles/licensing-ps-examples.md)
