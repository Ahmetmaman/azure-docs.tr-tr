---
title: Azure AD Domain Services için e-posta bildirimleri | Microsoft Docs
description: Azure Active Directory Domain Services yönetilen bir etki alanındaki sorunlar hakkında sizi uyarmak için e-posta bildirimlerini nasıl yapılandıracağınızı öğrenin
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: b9af1792-0b7f-4f3e-827a-9426cdb33ba6
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 07/06/2020
ms.author: iainfou
ms.openlocfilehash: 149de14ed2430613e9ca6d5d12f8dce6ba425dfe
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "86040002"
---
# <a name="configure-email-notifications-for-issues-in-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services sorunları için e-posta bildirimlerini yapılandırın

Azure Active Directory Domain Services (Azure AD DS) yönetilen etki alanının sistem durumu, Azure platformu tarafından izlenir. Azure portal sistem durumu sayfasında, yönetilen etki alanı için tüm uyarılar gösterilir. Sorunların zamanında yanıt verdiğinden emin olmak için e-posta bildirimleri, Azure AD DS yönetilen etki alanında algılanan her zamanki gibi sistem durumu uyarılarını raporlamak üzere yapılandırılabilir.

Bu makalede, yönetilen bir etki alanı için e-posta bildirim alıcılarını nasıl yapılandıracağınız gösterilir.

## <a name="email-notification-overview"></a>E-posta bildirimine genel bakış

Yönetilen bir etki alanı ile ilgili sorunlar konusunda sizi uyarmak için e-posta bildirimleri yapılandırabilirsiniz. Bu e-posta bildirimleri, uyarının bulunduğu yönetilen etki alanını belirtir ve Azure portal, algılama zamanını ve durum sayfasına yönelik bir bağlantıyı verir. Daha sonra sorunları çözmek için, belirtilen sorun giderme önerisi ' ni izleyebilirsiniz.

Aşağıdaki örnek e-posta bildirimi, yönetilen etki alanında kritik bir uyarı veya uyarının oluşturulduğunu gösterir:

![Örnek e-posta bildirimi](./media/active-directory-domain-services-alerts/email-alert.png)

> [!WARNING]
> İletideki bağlantılara tıklamadan önce her zaman e-postanın doğrulanmış bir Microsoft göndericiden geldiğinden emin olun. E-posta bildirimleri her zaman `azure-noreply@microsoft.com` adresten gelir.

### <a name="why-would-i-receive-email-notifications"></a>Neden e-posta bildirimleri alıyorum?

Azure AD DS, yönetilen etki alanı hakkında önemli güncelleştirmeler için e-posta bildirimleri gönderir. Bu bildirimler yalnızca hizmeti etkileyen acil sorunlar içindir ve hemen ele alınmalıdır. Her e-posta bildirimi, yönetilen etki alanındaki bir uyarı tarafından tetiklenir. Uyarılar Azure portal de görüntülenir ve [Azure AD DS sistem durumu sayfasında][check-health]görüntülenebilir.

Azure AD DS tanıtım, güncelleştirme veya satış amaçları için e-posta göndermez.

### <a name="when-will-i-receive-email-notifications"></a>Ne zaman e-posta bildirimleri alabilirim?

Yönetilen bir etki alanında [Yeni bir uyarı][troubleshoot-alerts] bulunduğunda anında bir bildirim gönderilir. Uyarı çözümlenmemişse, her dört günde bir anımsatıcı olarak ek e-posta bildirimleri gönderilir.

### <a name="who-should-receive-the-email-notifications"></a>E-posta bildirimlerini kimler alacak?

Azure AD DS e-posta alıcılarının listesi, yönetilen etki alanında yönetebilen ve değişiklik yapabilecek kişilerden oluşmalıdır. Bu e-posta listesi, herhangi bir uyarı ve sorun için "ilk Yanıtlayıcılar" olarak düşünülmelidir.

E-posta bildirimleri için beş adede kadar ek e-posta alıcısı ekleyebilirsiniz. E-posta bildirimleri için Beş taneden fazla alıcı istiyorsanız, bir dağıtım listesi oluşturun ve bunu bunun yerine bildirim listesine ekleyin.

Ayrıca, Azure AD dizini 'nin tüm *genel yöneticilerinin* ve *AAD DC yöneticileri* grubunun her üyesinin e-posta bildirimleri almasını seçebilirsiniz. Azure AD DS, genel yöneticilerin ve AAD DC yöneticilerinin listesi dahil olmak üzere yalnızca 100 e-posta adresine bildirim gönderir.

## <a name="configure-email-notifications"></a>E-posta bildirimlerini yapılandırma

Var olan e-posta bildirim alıcılarını gözden geçirmek veya ek alıcılar eklemek için aşağıdaki adımları izleyin:

1. Azure portal, araması yapın ve **Azure AD Domain Services**seçin.
1. *Aaddscontoso.com*gibi yönetilen etki alanınızı seçin.
1. Azure AD DS kaynak penceresinin sol tarafında **bildirim ayarları**' nı seçin. E-posta bildirimleri için mevcut alıcılar gösterilir.
1. E-posta alıcısı eklemek için, ek alıcılar tablosuna e-posta adresini girin.
1. İşiniz bittiğinde, üstteki gezinmede **Kaydet** ' i seçin.

> [!WARNING]
> Bildirim ayarlarını değiştirdiğinizde, tüm yönetilen etki alanı için bildirim ayarları yalnızca kendi başınıza değil güncelleştirilir.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="i-received-an-email-notification-for-an-alert-but-when-i-logged-on-to-the-azure-portal-there-was-no-alert-what-happened"></a>Uyarı için bir e-posta bildirimi aldım, ancak Azure portal oturum açtıklarında hiçbir uyarı yoktu. Ne oldu?

Bir uyarı çözümlenirse, uyarı Azure portal temizlenir. En olası neden, e-posta bildirimleri alan başka birinin, yönetilen etki alanındaki uyarıyı çözdüğü veya Azure platformu tarafından yeniden çözümlenmesidir.

### <a name="why-can-i-not-edit-the-notification-settings"></a>Bildirim ayarlarını neden düzenleyemiyorum?

Azure portal bildirim ayarları sayfasına erişemezseniz, yönetilen etki alanını düzenleme izinleriniz yoktur. Azure AD DS kaynağını düzenleme veya alıcı listesinden kaldırma izinleri almak için genel bir yöneticiye başvurun.

### <a name="i-dont-seem-to-be-receiving-email-notifications-even-though-i-provided-my-email-address-why"></a>E-posta adresimi sağladığım halde e-posta bildirimleri almıyorum. Neden?

Bildirim için e-postalarınızın istenmeyen posta veya istenmeyen klasörünüzü denetleyin ve göndericiye izin vermeyi unutmayın `azure-noreply@microsoft.com` .

## <a name="next-steps"></a>Sonraki adımlar

Raporlanacak bazı sorunları giderme hakkında daha fazla bilgi için bkz. [yönetilen bir etki alanında uyarıları çözümleme][troubleshoot-alerts].

<!-- INTERNAL LINKS -->
[check-health]: check-health.md
[troubleshoot-alerts]: troubleshoot-alerts.md
