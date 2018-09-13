---
title: Azure Active Directory B2B işbirliği API ve özelleştirme | Microsoft Docs
description: Azure Active Directory B2B işbirliği, iş ortaklarının kurumsal uygulamalarınıza seçmeli olarak erişmelerini mümkün kılarak şirketler arası ilişkilerinizi destekler.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/11/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: a41eadaecc203f9371da3eee05367a4f77747253
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35647166"
---
# <a name="azure-active-directory-b2b-collaboration-api-and-customization"></a>Azure Active Directory B2B işbirliği API ve özelleştirme

Biz, birçok müşteri, kuruluşları için en iyi şekilde davet sürecini özelleştirmek istedikleri bize vardı. API ile bunu yapabilirsiniz. [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="capabilities-of-the-invitation-api"></a>Davet API özellikleri
API, aşağıdaki özellikleri sunar:

1. İle bir dış kullanıcıyı davet *herhangi* e-posta adresi.

    ```
    "invitedUserDisplayName": "Sam"
    "invitedUserEmailAddress": "gsamoogle@gmail.com"
    ```

2. Bunlar, davetini kabul ettikten sonra kavuşmak kullanıcılarınıza istediğiniz özelleştirin.

    ```
    "inviteRedirectUrl": "https://myapps.microsoft.com/"
    ```

3. Standart bir davet e-posta yoluyla bize gönderebilirsiniz

    ```
    "sendInvitationMessage": true
    ```

  özelleştirebileceğiniz alıcı ile bir ileti

    ```
    "customizedMessageBody": "Hello Sam, let's collaborate!"
    ```

4. Kime seçin: kişiler bu ortak çalışanı davet hakkında döngüsünde tutmak istediğinizi.

5. Veya Azure AD üzerinden bildirim göndermek seçerek daveti ve ekleme iş akışı tamamen özelleştirebilirsiniz.

    ```
    "sendInvitationMessage": false
    ```

  Bu durumda, bir kullanım URL, bir e-posta şablonu, anlık ileti veya diğer dağıtım yöntemini seçeceğiniz katıştırabilirsiniz API'sinden ulaşırsınız.

6. Son olarak, bir yönetici, üye olarak kullanıcı davet seçebilirsiniz.

    ```
    "invitedUserType": "Member"
    ```


## <a name="authorization-model"></a>Yetkilendirme modeli
API aşağıdaki yetkilendirme modu çalıştırabilirsiniz:

### <a name="app--user-mode"></a>Uygulama + kullanıcı modu
Bu modda, kişi B2B davetleri olması oluşturma izinlerine sahip API gereksinimlerini kullanıyor.

### <a name="app-only-mode"></a>Tek uygulama modu
Uygulama yalnızca bağlamında, uygulamanın, başarılı olması davet User.Invite.All kapsamını gerekir.

Daha fazla bilgi için bkz: https://graph.microsoft.io/docs/authorization/permission_scopes


## <a name="powershell"></a>PowerShell
Artık, ekleme ve bir kuruluşun dış kullanıcılara bir kolayca davet PowerShell kullanmak da mümkündür. Cmdlet'ini kullanarak bir davet oluşturun:

```
New-AzureADMSInvitation
```

Aşağıdaki seçenekleri kullanabilirsiniz:

* -InvitedUserDisplayName
* -InvitedUserEmailAddress
* -SendInvitationMessage
* -InvitedUserMessageInfo

Ayrıca davet API Başvurusu'nda kullanıma denetleyebilirsiniz [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [B2B işbirliği davet e-posta öğeleri](invitation-email-elements.md)
- [B2B işbirliği Davetiyesi kullanımı](redemption-experience.md)
- [B2B işbirliği kullanıcıları davet etmeden ekleme](add-user-without-invite.md)

