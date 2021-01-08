---
title: Dahili kullanıcıları B2B işbirliğiyle davet etme-Azure AD
description: İş ortakları, dağıtıcılar, tedarikçiler, satıcılar ve diğer konuklar için dahili kullanıcı hesaplarınız varsa, kendi dış kimlik bilgileriyle veya oturum açmayla oturum açmak üzere davet ederek Azure AD B2B işbirliğiyle geçiş yapabilirsiniz. PowerShell veya Microsoft Graph davet API 'sini kullanın.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 04/12/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8acc547552cecaebb60888bb7b9777f6279b9b7c
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98015768"
---
# <a name="invite-internal-users-to-b2b-collaboration"></a>Dahili kullanıcıları B2B işbirliğiyle davet etme

> [!NOTE]
> B2B işbirliğini kullanmak için dahili kullanıcıları davet etme, Azure Active Directory genel bir önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Kuruluşlar, Azure AD B2B işbirliğinin kullanılabilirliğine başlamadan önce dağıtıcılar, tedarikçiler, satıcılar ve diğer konuk kullanıcılarla işbirliği yaparak bunlar için iç kimlik bilgilerini ayarlayabilir. Bunun gibi iç Konuk kullanıcılarınız varsa, Azure AD B2B avantajlarından yararlanabilmeniz için bunları B2B işbirliğinin kullanımına davet edebilirsiniz. B2B Konuk kullanıcılarınız, oturum açmak için kendi kimliklerini ve kimlik bilgilerini kullanabiliyor ve parola korumanız veya hesap yaşam döngülerini yönetmeniz gerekmez.

Mevcut bir iç hesaba davetiye göndermek, bu kullanıcının nesne KIMLIĞI, UPN, grup üyelikleri ve uygulama atamalarını korumanızı sağlar. Kullanıcıyı el ile silip yeniden davet etmeniz veya kaynakları yeniden atamanız gerekmez. Kullanıcıyı davet etmek için davet API 'sini kullanarak hem iç Kullanıcı nesnesini hem de Konuk kullanıcının e-posta adresini daveti ile birlikte geçirebilirsiniz. Kullanıcı daveti kabul ettiğinde, B2B hizmeti mevcut iç Kullanıcı nesnesini bir B2B kullanıcısına değiştirir. Bundan sonra Kullanıcı, B2B kimlik bilgilerini kullanarak bulut kaynakları hizmetlerinde oturum açması gerekir. Şirket kaynaklarına erişmek için kendi iç kimlik bilgilerini kullanmaya devam edebilirler, ancak iç hesaptaki parolayı sıfırlayarak veya değiştirerek bunu önleyebilirsiniz.

> [!NOTE]
> Davet tek yönlü. B2B işbirliğini kullanmak için dahili kullanıcıları davet edebilirsiniz, ancak B2B kimlik bilgilerini eklendikten sonra kaldıramazsınız. Kullanıcıyı yalnızca dahili bir kullanıcıya dönüştürmek için Kullanıcı nesnesini silip yeni bir tane oluşturmanız gerekir.

Genel önizleme aşamasında, bu makalede iç kullanıcıları B2B işbirliğiyle davet etmek için açıklanan Yöntem şu örneklerde kullanılamaz:

- Dahili kullanıcının atanmış bir Exchange lisansı vardır.
- Kullanıcı, dizininizde doğrudan Federasyon için ayarlanmış bir etki alanıdır.
- Dahili Kullanıcı yalnızca bulut hesabıdır ve ana hesabı Azure AD 'de değildir.

Bu örneklerde, iç Kullanıcı bir B2B kullanıcısına değiştirilmeli, iç hesabı silmeniz ve kullanıcıya B2B işbirliği için bir davet göndermeniz gerekir.

Şirket **içi eşitlenmiş kullanıcılar**: şirket içi ve bulut arasında eşitlenen Kullanıcı hesapları için şirket içi DIZIN, B2B işbirliği kullanım için davet edildikten sonra yetkili kaynağı olarak kalır. Şirket içi hesapta yaptığınız tüm değişiklikler, hesabı devre dışı bırakma veya silme dahil olmak üzere bulut hesabıyla eşitlenir. Bu nedenle, kullanıcının şirket içi hesabında oturum açmasını engellemez, ancak şirket içi hesabı silerek bulut hesabını korur. Bunun yerine, şirket içi hesap parolasını rastgele bir GUID veya diğer bilinmeyen bir değer olarak ayarlayabilirsiniz.

## <a name="how-to-invite-internal-users-to-b2b-collaboration"></a>Dahili kullanıcıları B2B işbirliğiyle davet etme

İç kullanıcıya bir B2B davetiyesi göndermek için PowerShell veya davet API 'sini kullanabilirsiniz. Davet için kullanmak istediğiniz e-posta adresinin iç Kullanıcı nesnesinde dış e-posta adresi olarak ayarlandığından emin olun.

- Davet için Kullanıcı. posta özelliğindeki e-posta adresini kullanmanız gerekir.
- Kullanıcının posta özelliğindeki etki alanının, oturum açmak için kullandıkları hesapla eşleşmesi gerekir. Aksi takdirde, takımlar gibi bazı hizmetler kullanıcının kimliğini doğrulayamayacaktır.

Varsayılan olarak, davet kullanıcıya davet edildiklerini bildiren bir e-posta gönderir, ancak bunun yerine bu e-postayı engelleyebilir ve kendi kendinize gönderebilirsiniz.

> [!NOTE]
> Kendi e-postanızı veya diğer iletişiminizi göndermek için-SendInvitationMessage: $false New-AzureADMSInvitation kullanarak kullanıcıları sessizce davet edebilir ve ardından kendi e-posta iletinizi dönüştürülen kullanıcıya gönderebilirsiniz. Bkz. [Azure AD B2B işbirliği API 'si ve özelleştirmesi](customize-invitation-api.md).

## <a name="use-powershell-to-send-a-b2b-invitation"></a>B2B davetiyesi göndermek için PowerShell 'i kullanma

Kullanıcıyı B2B işbirliği 'ya davet etmek için aşağıdaki komutu kullanın:

```powershell
Uninstall-Module AzureADPreview
Install-Module AzureADPreview
$ADGraphUser = Get-AzureADUser -objectID "UPN of Internal User"
$msGraphUser = New-Object Microsoft.Open.MSGraph.Model.User -ArgumentList $ADGraphUser.ObjectId
New-AzureADMSInvitation -InvitedUserEmailAddress <<external email>> -SendInvitationMessage $True -InviteRedirectUrl "http://myapps.microsoft.com" -InvitedUser $msGraphUser
```

## <a name="use-the-invitation-api-to-send-a-b2b-invitation"></a>Bir B2B davetiyesi göndermek için davet API 'sini kullanma

Aşağıdaki örnek, bir iç kullanıcıyı B2B kullanıcısı olarak davet etmek için davet API 'sinin nasıl çağrılacağını gösterir.

```json
POST https://graph.microsoft.com/v1.0/invitations
Authorization: Bearer eyJ0eX...
ContentType: application/json
{
    "invitedUserEmailAddress": "<<external email>>",
    "sendInvitationMessage": true,
    "invitedUserMessageInfo": {
        "messageLanguage": "en-US",
        "ccRecipients": [
            {
                "emailAddress": {
                    "name": null,
                    "address": "<<optional additional notification email>>"
                }
            }
        ],
        "customizedMessageBody": "<<custom message>>"
    },
    "inviteRedirectUrl": "https://myapps.microsoft.com?tenantId=",
    "invitedUser": {
        "id": "<<ID for the user you want to convert>>"
    }
}
```

Yeni bir konuk kullanıcıyı dizine davet ettiğinizde, API 'ye yönelik yanıt aldığınız yanıttır.

## <a name="next-steps"></a>Sonraki adımlar

- [B2B işbirliği daveti satın alma](redemption-experience.md)
