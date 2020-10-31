---
title: Kuruluşunuzun uygulama & gruplarına erişimini yönetme-Azure AD
description: Uygulamalarım portalından Kuruluşunuzun uygulamalarına ve gruplarına yönelik güvenlik erişimini yönetmek için bir erişim incelemesi gerçekleştirmeyi öğrenin.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: end-user-help
ms.date: 10/19/2020
ms.author: curtand
ms.reviewer: kasimpso
ms.custom: user-help, seo-update-azuread-jan
ms.openlocfilehash: 8da9e97aaa2c6d99990df4221d8b6e99ab18ac83
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93124821"
---
# <a name="perform-an-access-review-from-the-my-apps-portal"></a>Uygulamalarım portalından bir erişim incelemesi gerçekleştirme

Uygulamalarınız ve gruplarınız için erişim gözden geçirmeleri gerçekleştirmek üzere Web tabanlı **My Apps** portalından iş veya okul hesabınızı kullanabilirsiniz. Erişim gözden geçirmeleri, eski erişimi yönetmenize veya erişim gereksinimlerini değiştirmenize ve bunların incelenip güncelleştirilmesini sağlamanıza yardımcı olur.

**Uygulamalarım** portalına erişiminiz yoksa, Izin Için yardım masanızla iletişime geçin.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-my-apps-portal.md)]

>[!Important]
>Bu içerik, **uygulamalarım** kullanıcılarına yöneliktir. Yöneticiyseniz, [uygulama yönetimi belgelerinde](../manage-apps/index.yml)bulut tabanlı uygulamalarınızın nasıl ayarlanacağı ve yönetileceği hakkında daha fazla bilgi edinebilirsiniz.

## <a name="manage-access-reviews"></a>Erişim incelemelerini yönetme

Yöneticiniz kendi erişim incelemelerinizi gerçekleştirme izni verdiyse, kendi **uygulamalarım** veya uygulamalar erişim gözden geçirmeleri sayfasındaki **erişim İncelemeleri** kutucuğundan gruplarınızı veya uygulamalarınızı yönetebilirsiniz.

>[!Note]
>**Erişim gözden geçirmeleri** kutucuğunu görmüyorsanız, bu, erişim gözden geçirmeleri gerçekleştirme izniniz olmadığı ya da Onayınızı bekleyen herhangi bir gözden geçirmede sahip olmadığınız anlamına gelir. Kutucuğa erişiminizin olması gerektiğini düşünüyorsanız yardım için yardım masasına başvurun.

## <a name="to-perform-your-access-reviews"></a>Erişim incelemelerinizi gerçekleştirmek için

1. İş veya okul hesabınızda oturum açın.

2. Web tarayıcınızı açın ve adresine gidin https://myapps.microsoft.com veya kuruluşunuz tarafından sunulan bağlantıyı kullanın. Örneğin, kuruluşunuz için, gibi özelleştirilmiş bir sayfaya yönlendirilmeyebilirsiniz https://myapps.microsoft.com/contoso.com .

    Kuruluşunuzun sahip olduğu ve kullanabileceğiniz bulut tabanlı uygulamaların tümünü gösteren **uygulamalar** sayfası görüntülenir.

    ![Uygulamalarım portalındaki uygulamalar sayfası](media/my-apps-portal/my-apps-home.png)

3. Onayınızı bekleyen erişim gözden geçirmeleri listesini görmek için **erişim İncelemeleri** kutucuğunu seçin.

    ![Kuruluş için bekleyen erişim gözden geçirmeleri içeren erişim İncelemeleri sayfası](media/my-apps-portal/my-apps-portal-access-reviews-page.png)

4. Erişim incelemenizi başlatmak için **gözden geçirmeyi** Başlat ' ı seçin.

5. Erişiminizi gözden geçirin ve hâlâ gerekli olup olmadığını saptayın.

    ![İnceleme ayrıntılarını gösteren erişim gözden geçirme sayfası](media/my-apps-portal/my-apps-portal-perform-access-reviews-page.png)

    >[!Note]
    >Yöneticiyseniz ve kuruluşunuzun gruplara ve uygulamalara erişimini incelemeye izin verildiyse, farklı bir sayfa görürsünüz. Kuruluşunuz için grupları veya uygulamaları gözden geçirme hakkında daha fazla bilgi için bkz. [Azure AD erişim gözden geçirmeleriyle gruplara veya uygulamalara erişimi gözden geçirme](../governance/perform-access-review.md).

6. Erişiminizi **kaldırmak için, erişiminizi korumak Için** **Evet** ' i seçin.

    **Evet** ' i seçerseniz, **neden** kutusunda bir gerekçe belirtmeniz gerekebilir.

    ![Örnek metinle ilgili neden kutusunu gösteren erişim gözden geçirme sayfası](media/my-apps-portal/my-apps-portal-perform-access-reviews-reason-box.png)

7. **Gönder** ’i seçin.

    Erişim incelemeniz tamamlanmıştır ve **My Apps** portalına dönersiniz.

    >[!Note]
    >Erişiminizi, erişim gözden geçirme döneminizin bitene kadar istediğiniz zaman değiştirebilirsiniz. Bir uygulamaya veya gruba erişiminizi kaldırırsanız, bu, hemen kaldırılmaz. Kaldırma işlemi, erişim gözden geçirme süresi sona erdiğinde veya bir yönetici incelemeyi kapattığında gerçekleşir.

## <a name="next-steps"></a>Sonraki adımlar

- [Uygulamalarım portalındaki uygulamalara erişin ve bunları kullanın](my-apps-portal-end-user-access.md)
- [Profil bilgilerinizi değiştirme](./my-account-portal-settings.md)
- [Gruplarla ilgili bilgilerinizi görüntüleyin ve güncelleştirin](my-apps-portal-end-user-groups.md)