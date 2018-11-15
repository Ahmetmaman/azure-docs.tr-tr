---
title: Akıllı kilitleme Azure AD kullanarak bir deneme yanılma saldırılarını önleme
description: Azure Active Directory akıllı kilitleme Kuruluşunuz parolalar tahmin etmeye çalışırken deneme yanılma saldırılarına karşı korumaya yardımcı olur.
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: rogoya
ms.openlocfilehash: 957aa05efab68f9531fb6576de775aa9901ab44d
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51685812"
---
# <a name="azure-active-directory-smart-lockout"></a>Azure Active Directory akıllı kilitleme

Akıllı kilitleme da bulut zekasından kullanıcılarınızın parolaları tahmin veya almak için deneme yanılma yöntemleri kullanmak için çalışan bir kötü aktörleri kullanıma kilitlemek için kullanır. O gösterimi geçerli kullanıcılardan gelen oturum açma tanıyabilir ve bunları farklı olanları saldırganlar ve bilinmeyen diğer kaynakları ele almanız. Akıllı kilitleme saldırganlar, izin vermek, kullanıcılarınızın hesaplarına erişmek ve üretken devam ederken kullanıma kilitler.

Varsayılan olarak, akıllı kilitleme oturum açma denemeleri başarısız olan 10 denemeden sonra bir dakika hesabından kilitler. Sonraki başarısız oturum açma girişimleri, ilk ve sonraki denemeler de daha uzun bir dakika sonra yeniden hesabı kilitler.

* Akıllı kilitleme kilidi açma sayacı reincrementing önlemek için son üç hatalı parola karmaları izler. Birisi birden çok kez aynı yanlış parola girerse, bu davranış, hesap kilitleme için neden olmaz.
   * Bu işlev etkin geçişli kimlik doğrulaması ile müşteriler için kullanılamıyor.

Akıllı kilitleme her zaman doğru karışımını güvenlik ve kullanılabilirlik sunan bu varsayılan ayarları olan tüm Azure AD müşterileri için açıktır. Akıllı kilitleme ayarları, kuruluşunuza özgü değerlerle özelleştirmesini kullanıcılarınız için Azure AD temel veya daha yüksek lisansı gerektirir.

Akıllı kilitleme kullanarak orijinal bir kullanıcı hiçbir zaman kilitlenir garanti etmez. Bir kullanıcı hesabı akıllı kilitleme kilitler biz kilitleme orijinal kullanıcı için elimizden geleni deneyin. Kötü aktörleri orijinal kullanıcı hesabı için erişim sağlayamadığı emin olmak kilitleme hizmeti çalışır.  

* Her Azure Active Directory veri merkezi kilitleme bağımsız olarak izler. Kullanıcı (threshold_limit * datacenter_count) kullanıcı, her veri merkezi değerse deneme sayısı.
* Akıllı kilitleme tanıdık vs bilinmeyen konum kötü bir aktör ve orijinal kullanıcı arasında ayırt etmek için kullanır. Tanınmayan ve tanıdık konumları ayrı kilitleme sayaçları sahip olur.

Akıllı kilitleme, saldırganlar tarafından kilitli şirket içi Active Directory hesapları korumak için parola karması eşitleme veya doğrudan kimlik doğrulaması'nı kullanarak, karma dağıtımlar ile tümleştirilebilir. Şirket içi Active Directory ulaşmadan önce akıllı kilitleme ilkeleri Azure AD'de uygun şekilde ayarlayarak saldırıları filtrelenebilen.

Kullanırken [geçişli kimlik doğrulaması](../hybrid/how-to-connect-pta.md), emin olmanız gerekir:

   * Azure AD kilitleme eşiği **daha az** Active Directory hesap kilitleme eşik değerinden. Böylece Active Directory hesap kilitleme eşiği en az iki veya üç kez Azure AD kilitleme eşik değerinden daha uzun değerleri ayarlayın. 
   * Azure AD kilitleme süresi **saniye** olduğu **uzun** Active Directory süreden sonra hesap kilitleme sayacını sıfırla daha **dakika**.

> [!IMPORTANT]
> Şu anda bunlar akıllı kilitleme özelliğini tarafından kilitlenmiş, bir yönetici kullanıcıların bulut hesapları kilidi açılamıyor. Yönetici, kilitleme süresi dolmak üzere beklemeniz gerekir.

## <a name="verify-on-premises-account-lockout-policy"></a>Şirket içi hesap kilitleme ilkesi doğrulayın

Şirket içi Active Directory hesap kilitleme ilkesi doğrulamak için aşağıdaki yönergeleri kullanın:

1. Grup İlkesi Yönetimi Aracı'nı açın.
2. Örneğin, kuruluşunuzun hesap kilitleme ilkesi içeren bir Grup İlkesi düzenleme **varsayılan etki alanı ilkesi**.
3. Gözat **Bilgisayar Yapılandırması** > **ilkeleri** > **Windows ayarları** > **güvenlik ayarları**   >  **Hesap ilkeleri** > **hesap kilitleme ilkesi**.
4. Doğrulayın, **hesap kilitleme eşiği** ve **sonra hesap kilitleme sayacını sıfırla** değerleri.

![Bir Grup İlkesi nesnesi kullanarak şirket içi Active Directory hesap kilitleme ilkesi](./media/howto-password-smart-lockout/active-directory-on-premises-account-lockout-policy.png)

## <a name="manage-azure-ad-smart-lockout-values"></a>Azure AD akıllı kilitleme değerleri yönetme

Kuruluş gereksinimlerinize bağlı olarak, akıllı kilitleme değerleri özelleştirilmiş gerekebilir. Akıllı kilitleme ayarları, kuruluşunuza özgü değerlerle özelleştirmesini kullanıcılarınız için Azure AD temel veya daha yüksek lisansı gerektirir.

Denetleyin veya kuruluşunuz için akıllı kilitleme değerleri değiştirmek için aşağıdaki adımları kullanın:

1. Oturum [Azure portalında](https://portal.azure.com), tıklayın **Azure Active Directory**, ardından **kimlik doğrulama yöntemleri**.
1. Ayarlama **kilitleme eşiği**bağlı olarak kaç başarısız oturum açma hesabınız, ilk kilitlenmeden önce izin verilir. Varsayılan değer 10'dur.
1. Ayarlama **kilitleme süresini saniye cinsinden**, her kilitleme saniye cinsinden uzunluğu. 60 saniye (bir dakika) varsayılandır.

> [!NOTE]
> İlk kilitleme ayrıca başarısız olduktan sonra oturum, hesabı yeniden kilitler durumunda. Bir hesabın tekrar tekrar kilitler, kilitleme süresi artar.

![Azure portalında Azure AD akıllı kilitleme ilkesi özelleştirme](./media/howto-password-smart-lockout/azure-active-directory-custom-smart-lockout-policy.png)
## <a name="next-steps"></a>Sonraki adımlar

[Azure AD kullanarak kuruluşunuzdaki yanlış parolalar yasaklamak öğrenin.](howto-password-ban-bad.md)

[Self Servis parola sıfırlamayı kullanıcılara kendi hesaplarının kilidini açmak izin verecek şekilde yapılandırın.](quickstart-sspr.md)