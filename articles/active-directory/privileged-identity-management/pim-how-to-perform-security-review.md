---
title: PIM'de erişim gözden geçirmesi Azure AD dizin rollerim gerçekleştirmek | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) erişim gözden geçirmesi, Azure AD Dizin rolleri gerçekleştirmeyi öğreneceksiniz.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 06/21/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: f80eadf15148865f62e2d0f51e2e9d96ac4d147a
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55190539"
---
# <a name="perform-an-access-review-of-my-azure-ad-directory-roles-in-pim"></a>PIM'de erişim gözden geçirmesi Azure AD dizin rollerim gerçekleştirin
Azure Active Directory (AD) Privileged Identity Management, kuruluşlara ayrıcalıklı Azure ad'deki ve Office 365 veya Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetlerine erişimi yönetme basitleştirir.  

Bir yönetici rolüne atanırsa, kuruluşunuzun ayrıcalıklı Rol Yöneticisi yine de bu rol için işinizi gerektiğini düzenli olarak doğrulamanızı isteyebilir. Bir bağlantısını içeren bir e-posta alabilir veya doğrudan gidebilirsiniz [Azure portalında](https://portal.azure.com). Bir kendi kendini gözden geçirin, atanan rollerinin gerçekleştirmek için bu makaledeki adımları izleyin.

Ayrıcalıklı Rol Yöneticisi veya genel yönetici erişim gözden geçirmelerine ilgilenen kullanıyorsanız daha fazla bilgi alın [erişim gözden geçirmesi başlatma](pim-how-to-start-security-review.md).

## <a name="add-the-privileged-identity-management-application"></a>Privileged Identity Management uygulamasını ekleme
Azure AD Privileged Identity Management (PIM) uygulamasında kullanabileceğiniz [Azure portalında](https://portal.azure.com/) , gözden geçirmek için.  Azure AD Privileged Identity Management uygulaması portalınızda yoksa, başlamak için aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure portalı ve burada şunları yapacaksınız, dizini seçin, sağ üst köşedeki kullanıcı adınızı işletim seçin.
3. **Tüm hizmetler** seçeneğini belirleyin ve **Azure AD Privileged Identity Management** araması yapmak için Filtre metin kutusunu kullanın.
4. **Panoya sabitle**'yi işaretleyin ve ardından **Oluştur**’a tıklayın. Privileged Identity Management uygulaması açılır.

## <a name="approve-or-deny-access"></a>Onaylayın veya reddedin erişim
Onaylayabilir ya da erişimi reddetmek yalnızca Gözden Geçiren, yine de bu rolü veya kullanıp kullanmadığını belirten. Seçin **Onayla** rolünde kalmak istiyorsanız veya **Reddet** erişim artık ihtiyacınız yoksa. Sonuçları Gözden Geçiren geçerli olana kadar durumu hemen değişmez.
Erişim değerlendirmesi tamamlama ve bulmak için aşağıdaki adımları izleyin:

1. PIM uygulamada seçin **ayrıcalıklı erişim incelemesi**. Tüm beklemedeki erişim gözden geçirmeleri varsa, bunlar Azure AD erişim gözden geçirmeleri dikey penceresinde görüntülenir.
2. Gözden geçirmeyi tamamlamak istiyorsanız seçin.
3. Gözden geçirmeyi oluşturduğunuz sürece gözden tek kullanıcısı olarak görünür. Adınızın yanındaki onay işaretini seçin.
4. Seçin ya da **onaylama** veya **Reddet**. İçinde kararınız için bir neden dahil etmeniz gerekebilir **nedeninizi belirtin** metin kutusu.  
5. Kapat **gözden geçirme Azure AD rolleri** dikey penceresi.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar

- [PIM hizmetinde Azure kaynak rollerimin erişim gözden geçirmesini gerçekleştirme](pim-resource-roles-perform-access-review.md)
