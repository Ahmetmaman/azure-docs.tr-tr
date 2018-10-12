---
title: Azure Active Directory B2B işbirliği API ve özelleştirme | Microsoft Docs
description: Azure Active Directory B2B işbirliği, iş ortaklarının kurumsal uygulamalarınıza seçmeli olarak erişmelerini mümkün kılarak şirketler arası ilişkilerinizi destekler.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: reference
ms.date: 04/11/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 8809a5a8b4f76d6e33bbb934e13931e86f2d681c
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49091784"
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
Ekleme ve bir kuruluşun dış kullanıcılara bir kolayca davet için PowerShell kullanabilirsiniz. Cmdlet'ini kullanarak bir davet oluşturun:

```
New-AzureADMSInvitation
```

Aşağıdaki seçenekleri kullanabilirsiniz:

* -InvitedUserDisplayName
* -InvitedUserEmailAddress
* -SendInvitationMessage
* -InvitedUserMessageInfo

### <a name="invitation-status"></a>Davet durumu

Bir dış kullanıcıyı Davet Gönder sonra kullanabileceğiniz **Get-AzureADUser** bunlar kabul, görmek için cmdlet. Get-AzureADUser aşağıdaki özelliklerini bir dış kullanıcıyı davet gönderildiğinde doldurulur:

* **UserState** davet olup olmadığını belirten **PendingAcceptance** veya **kabul edilen**.
* **UserStateChangedOn** yapılan son değişikliği için zaman damgasını gösterir **UserState** özelliği.

Kullanabileceğiniz **filtre** sonuçlarına göre filtreleme seçeneğini **UserState**. Aşağıdaki örnek, bekleyen davet sahip kullanıcıları göstermek için sonuçları filtrelemek gösterilmektedir. Örnek ayrıca gösterir **Format-List** seçeneği, görüntülenecek özelliklerini belirtmenize olanak tanır. 
 
```
Get-AzureADUser -Filter "UserState eq 'PendingAcceptance'" | Format-List -Property DisplayName,UserPrincipalName,UserState,UserStateChangedOn
```

> [!NOTE]
> AzureAD PowerShell modülü veya AzureADPreview PowerShell modülünün en son sürümü olduğundan emin olun. 

## <a name="see-also"></a>Ayrıca bkz.

Davet API başvurusunda kullanıma [ https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation ](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [B2B işbirliği davet e-posta öğeleri](invitation-email-elements.md)
- [B2B işbirliği Davetiyesi kullanımı](redemption-experience.md)
- [B2B işbirliği kullanıcıları davet etmeden ekleme](add-user-without-invite.md)

