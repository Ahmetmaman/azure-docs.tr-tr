---
title: PIM'de erişim gözden geçirmesi Azure kaynağı rollerim gerçekleştirmek | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) erişim gözden geçirmesi, Azure kaynağı rolleri gerçekleştirmeyi öğreneceksiniz.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 3b9c998351432a591ece0a61d60256aea89a6f8f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180900"
---
# <a name="perform-an-access-review-of-my-azure-resource-roles-in-pim"></a>PIM'de erişim gözden geçirmesi Azure kaynağı rollerim gerçekleştirin
Azure kaynakları için Privileged Identity Management (PIM), kuruluşların azure'daki kaynaklara ayrıcalıklı erişimi yönetme sürecini basitleştirir. 

Bir yönetici rolüne atanırsa, kuruluşunuzun ayrıcalıklı Rol Yöneticisi yine de bu rol için işinizi gerektiğini düzenli olarak doğrulamanızı isteyebilir. Bir bağlantısını içeren bir e-posta alabilir veya doğrudan gidebilirsiniz [Azure portalında](https://portal.azure.com). Bir kendi kendini gözden geçirin, atanan rollerinin gerçekleştirmek için bu makaledeki adımları izleyin.

Erişim gözden geçirmelerine ilgilenen bir ayrıcalıklı rol yöneticisi değilseniz, daha fazla bilgi edinin [erişim gözden geçirmesi başlatma](pim-resource-roles-start-access-review.md).

## <a name="add-the-privileged-identity-management-application"></a>Privileged Identity Management uygulamasını ekleme
Azure Active Directory (Azure AD) PIM uygulamasında kullanabileceğiniz [Azure portalında](https://portal.azure.com/) , gözden geçirmek için. Portalınızda uygulamanız yoksa, başlamak için aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Kullanıcı Azure portalının sağ üst köşedeki adlandırın ve burada yapacaklarınız dizini seçin seçin çalışıyor olabilir.
3. Seçin **tüm hizmetleri**ve **filtre** aramak için arama kutusunu *Azure AD Privileged Identity Management*.
4. Denetleme **panoya Sabitle**ve ardından **Oluştur**. PIM uygulamasını açar.

## <a name="approve-or-deny-access"></a>Onaylayın veya reddedin erişim
Onaylayabilir ya da erişimi reddetmek yalnızca Gözden Geçiren, yine de bu rolü veya kullanıp kullanmadığını belirten. Seçin **Onayla** rolünde kalmak istiyorsanız veya **Reddet** erişim artık ihtiyacınız yoksa. Gözden Geçiren sonuçlar yalnızca geçerli olduğu durumlarda durumunuzu değiştirir.

Erişim değerlendirmesi tamamlama ve bulmak için aşağıdaki adımları izleyin:
1. Azure AD PIM uygulamaya göz atın.
2. Seçin **erişimi gözden geçir** dikey penceresi.

   ![Seçili gözden geçirme erişim dikey penceresi ekran görüntüsü, PIM uygulamanızdan](media/azure-pim-resource-rbac/rbac-access-review-complete.png)

3. Gözden geçirmeyi tamamlamak istiyorsanız seçin. 
4. Seçin ya da **onaylama** veya **Reddet**. İçinde **neden kutusu sağlama**, kararınız için bir neden eklemeniz gerekir.

   ![Ekran görüntüsü, gözden geçirme Ayrıntıları sayfası](media/azure-pim-resource-rbac/rbac-access-review-choice.png)

## <a name="next-steps"></a>Sonraki adımlar

- [PIM hizmetinde Azure AD dizin rollerimin erişim gözden geçirmesini gerçekleştirme](pim-how-to-perform-security-review.md)
