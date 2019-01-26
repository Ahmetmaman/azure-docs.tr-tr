---
title: B2B işbirliği davet e-- Azure Active Directory öğeleri | Microsoft Docs
description: Azure Active Directory B2B işbirliği davet e-posta şablonu
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.openlocfilehash: 57ba4b35cf470eff040d4a2dca42c60820fa9d9e
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2019
ms.locfileid: "55079979"
---
# <a name="the-elements-of-the-b2b-collaboration-invitation-email---azure-active-directory"></a>B2B işbirliği davet e-- Azure Active Directory öğeleri

Davet e-postalarına, iş ortakları karttaki B2B işbirliği kullanıcıları Azure AD'de getirmek için önemli bir bileşendir. Alıcının güveni artırmak için bunları kullanabilirsiniz. yasallığı ekleyebilirsiniz ve alıcı emin olmak için e-posta, sosyal kanıt hissettirir seçme ile rahat **Başlarken** davetiyeyi kabul etmek için düğme. Bu güven, paylaşım uyuşmazlıkları azaltmak bir anahtar anlamına gelir. Ve ayrıca e-posta harika görünecek yapmak istediğiniz!

![Azure AD B2b davet e-postası](media/invitation-email-elements/invitation-email.png)

## <a name="explaining-the-email"></a>E-posta açıklayan
En iyi şekilde nasıl yeteneklerini kullanılacak bilmesi e-postanın bazı öğeler bakalım.

### <a name="subject"></a>Özne
E-postanın şu deseni izler: Davet ettiğiniz &lt;kiracıadı&gt; kuruluş

### <a name="from-address"></a>Gönderici adresi
Bir LinkedIn desen Kimden adresi için kullanırız.  Davet eden olan temizleyin ve şirket, ve ayrıca bir Microsoft e-posta geldiğini açıklamak e-posta adresi gerekir. Biçimi şu şekildedir: &lt;Davet eden görünen adını&gt; gelen &lt;kiracıadı&gt; (Microsoft aracılığıyla) <invites@microsoft.com>

### <a name="reply-to"></a>Yanıtla
Yanıt için e-posta, davet eden'ın e-posta için kullanılabilir olduğunda, yeniden davet eden e-posta göndermesi için e-posta yanıtlama ayarlanır.

### <a name="branding"></a>Markalama
Kiracı kullanım davet e-postaları şirket markası, kiracınız için ayarlamış. Bu özellikten faydalanmak istiyorsanız [burada](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) nasıl yapılandırılacağı hakkında ayrıntılar bulunur. Başlık logosu, e-postada görünür. Görüntü boyutu izleyin ve kalite yönergeler [burada](https://docs.microsoft.com/azure/active-directory/active-directory-branding-custom-signon-azure-portal) en iyi sonuçlar için. Ayrıca, şirket adı da eylem çağrısı içinde gösterilir.

### <a name="call-to-action"></a>Eylemini çağırın
Eylem çağrısı iki bölümden oluşur: alıcı, e-posta neden aldı ve bununla ilgili alıcı ne sorulmaktadır açıklayan.
- "Neden" bölümünde şu biçimi kullanarak çözülebilir: Uygulamalara erişmek üzere davet edildiniz &lt;kiracıadı&gt; kuruluş

- Ve "ne, yapmak istenir" bölümünde varlığını tarafından belirtilen **Başlarken** düğmesi. Alıcı davetleri gerek kalmadan eklendiğinde, bu düğmeyi göstermez.

### <a name="inviters-information"></a>Davet eden'ın bilgi
Davet eden'ın görünen ad, e-postada dahil edilir. Ve ayrıca, Azure AD hesabınız için bir profil resmi oluşturan ayarladıysanız, davet eden e-posta o resmi de içerecektir. Her ikisi de, e-posta, alıcının ellerde artırmak için tasarlanmıştır.

Profil resminizi henüz ayarlamadıysanız, davet eden'ın baş resmi yerine bir simgeyle gösterilir:

  ![Davet eden'ın baş görüntüleme](media/invitation-email-elements/inviters-initials.png)

### <a name="body"></a>Gövde
Davet eden ne zaman ölçeklemesini ileti gövdesinde [directory, Grup veya uygulama için Konuk kullanıcı davet](add-users-administrator.md) veya [davet API kullanarak](customize-invitation-api.md). Güvenlik nedenleriyle HTML etiketlerini işlemek için bir metin alanı var.

### <a name="footer-section"></a>Alt bilgi bölümü
Alt bilgi, Microsoft şirket markası içerir ve e-posta izlenmeyen bir diğer addan gönderildiğini bilmeniz alıcı olanak tanır. Özel durumlar:

- Davet eden, davet eden Kiracı içinde bir e-posta adresi yok

  ![Resim davet eden, davet eden Kiracı bir e-posta adresi yok](media/invitation-email-elements/inviter-no-email.png)


- Alıcı davetini gerekmez

  ![ne zaman alıcı davetini gerekmez](media/invitation-email-elements/when-recipient-doesnt-redeem.png)


## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği hakkında aşağıdaki makalelere bakın:

- [Azure AD B2B işbirliği nedir](what-is-b2b.md)
- [Azure Active Directory yöneticileri B2B işbirliği kullanıcılarını nasıl ekleyebilir?](add-users-administrator.md)
- [Bilgi çalışanları B2B işbirliği kullanıcılarını nasıl ekleyebilir?](add-users-information-worker.md)
- [B2B işbirliği Davetiyesi kullanımı](redemption-experience.md)
- [B2B işbirliği kullanıcıları davet etmeden ekleme](add-user-without-invite.md)
