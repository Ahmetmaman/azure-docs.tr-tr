---
title: Azure AD erişim gözden geçirmeleri nelerdir? | Microsoft Docs
description: Azure Active Directory erişim gözden geçirmelerini kullanarak, yönetim, risk yönetimi ve kuruluşunuzdaki uyumluluk girişimlerini karşılamak için Grup üyeliği ve uygulama erişimini denetleyebilirsiniz.
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
ms.subservice: compliance
ms.date: 01/18/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 419a07ff6d423f363d6973da3df00fd4aa3f6278
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56727273"
---
# <a name="what-are-azure-ad-access-reviews"></a>Azure AD erişim gözden geçirmeleri nelerdir?

Azure Active Directory (Azure AD) erişim gözden geçirmeleri, kuruluşların grup üyeliğini yönetme, erişim kurumsal uygulamalar ve rol atamalarını verimli bir şekilde olanak sağlar. Kullanıcının erişimini yalnızca doğru kişilere erişim devam emin olmak için düzenli olarak gözden geçirilebilir.

Erişim gözden geçirmeleri hızlı bir genel bakış sağlayan bir video şu şekildedir:

>[!VIDEO https://www.youtube.com/embed/kDRjQQ22Wkk]

## <a name="why-are-access-reviews-important"></a>Erişimi neden önemli incelemeleri?

Azure AD, dahili olarak, kuruluşunuz içinde ve iş ortakları gibi dış kuruluşlardan kullanıcılarla işbirliği sağlar. Kullanıcıları gruplara katılmak, konuklar davet, bulut uygulamalarına bağlanın ve kendi iş veya kişisel cihazlardan uzaktan çalışın. Self Servis gücünden yararlanan kolaylık daha iyi erişim yönetimi özellikleri gereksinimini açmıştır.

- Nasıl yeni çalışanlar katılma gibi üretken olmak için doğru erişime sahip oldukları emin olabilirim?
- Nasıl kişiler, takımlar taşıyın veya şirketten gibi özellikle Konukları gerektirdiğinde, eski erişim kaldırıldı emin olabilirim?
- Aşırı erişim hakları geldikleri erişim üzerinde denetim olmaması gibi denetim bulgularını ve güvenlik ihlalleri neden olabilir.
- Kaynak sahiplerine kaynaklarına kimlerin erişebileceğini, düzenli olarak gözden emin olmak için proaktif olarak etkileşim kurmak zorunda.

## <a name="when-to-use-access-reviews"></a>Ne zaman kullanılır erişim gözden geçirmeleri?

- **Çok sayıda kullanıcı ayrıcalıklı roller:** Kaç kullanıcının yönetici erişimi denetlemek için iyi bir fikirdir, kaç bunları genel açtığınızdan ve olmadığını davet Konukları veya bir yönetim görevi gerçekleştirmek için atandıktan sonra kaldırılmamış iş ortakları. Rol ataması kullanıcılar onaylayabilirsiniz [Azure AD Dizin rolleri](../privileged-identity-management/pim-how-to-perform-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) gibi genel Yöneticiler veya [Azure kaynak rolleri](../privileged-identity-management/pim-resource-roles-perform-access-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) gibi kullanıcı erişimi Yöneticisi'nde [Azure AD Privileged Identity Management (PIM)](../privileged-identity-management/pim-configure.md) karşılaşırsınız.
- **Ne zaman Otomasyon olanaksız olacaktır:** Güvenlik grupları veya Office 365 gruplarında dinamik üyelik ancak ne ik veri Azure AD'de değil veya kullanıcılar yine de erişim grubu kendi değiştirme eğitmek için bırakarak sonra ihtiyacınız varsa için kurallar oluşturabilir miyim? Yine de erişmesi gereken kişiler erişimi devam etmesi gerekip emin olmak için bu Grup İnceleme oluşturabilirsiniz.
- **Bir gruba yeni bir amaç için kullanıldığında:** Azure AD ile eşitlenmesi için gittiği bir grubunuz varsa ya da satış ekibi gruptaki herkes için Salesforce uygulama etkinleştirmeyi planlıyorsanız, önce bir farklı risk ortak kullanılan grubun grup üyeliğini gözden geçirmek için Grup sahibi istemek yararlı olacaktır içeriği.
- **İş kritik veri erişimi:** belirli kaynaklar için onu dışında istemem için gerekli olabilecek düzenli olarak oturumunu kapatmak ve neden erişim denetim amacıyla için ihtiyaç duydukları üzerinde bir gerekçe vermek mümkün KILAR.
- **Bir ilkenin özel durum listesi tutmak için:** İdeal bir dünyada, tüm kullanıcıların erişimini izlemeniz gerekir ilkeleri, kuruluşunuzun kaynaklarına güvenli erişim için. Ancak, bazı durumlarda özel durumları yapmanızı gerektiren iş durumlar vardır. BT yöneticisi olarak, bu görevi yönetmek, İlkesi özel durumları sözleşmeli önlemek ve bu özel durumlar düzenli olarak gözden kavram ile denetçiler sağlayın.
- **Grup sahipleri kendi gruplarındaki Konukları çağrılmasına halen ihtiyaçları onaylamanız istenir:** Çalışan erişimi bazı şirket içi IAM ancak değil davet edilen Konuk otomatik. Bir grubu Konukları iş hassas içerik ve ardından onun Konukları onaylamak için Grup sahibinin sorumluluk çözümlenmedi erişim için bir işletme ihtiyaçlarına erişmenizi durumunda.
- **Düzenli aralıklarla yinelenen incelemeleri vardır:** Kümesi frekansları kullanıcıların erişim gözden geçirmeleri yinelenen gibi haftalık, aylık, üç aylık veya yıllık olarak ayarlayabilirsiniz ve gözden geçirenler her İnceleme başlangıcında bildirilir. Gözden geçirenler onaylayabilir veya Yardımı akıllı öneriler ve kullanıcı dostu bir arabirim ile erişimi engelle.

## <a name="where-do-you-create-reviews"></a>Gözden geçirmeler oluşturduğunuz?

Gözden geçirme istediğinize bağlı olarak, Azure AD erişim gözden geçirmeleri, Azure AD Kurumsal uygulamaları (önizlemede) veya Azure AD PIM, erişim gözden geçirmesi oluşturacaksınız.

| Kullanıcı erişim hakları | Gözden geçirenler olabilir | Oluşturulan gözden geçirme | Gözden Geçiren deneyimi |
| --- | --- | --- | --- |
| Güvenlik grubu üyeleri</br>Office grubu üyeleri | Belirtilen gözden geçirenler</br>Grup sahipleri</br>Kendi kendine gözden geçirin | Azure AD erişim gözden geçirmeleri</br>Azure AD grupları | Erişim paneli |
| Bağlı bir uygulamaya atanan | Belirtilen gözden geçirenler</br>Kendi kendine gözden geçirin | Azure AD erişim gözden geçirmeleri</br>Azure AD Kurumsal uygulamaları (önizlemede) | Erişim paneli |
| Azure AD dizin rolü | Belirtilen gözden geçirenler</br>Kendi kendine gözden geçirin | Azure AD PIM | Azure portal |
| Azure Kaynak rolü | Belirtilen gözden geçirenler</br>Kendi kendine gözden geçirin | Azure AD PIM | Azure portal |

## <a name="prerequisites"></a>Önkoşullar

Erişim gözden geçirmeleri kullanmak için aşağıdaki lisanslardan birine sahip olmalıdır:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5 lisansı

Daha fazla bilgi için [nasıl yapılır: Azure Active Directory Premium'a kaydolma](../fundamentals/active-directory-get-started-premium.md) veya [Enterprise Mobility + Security E5 deneme](https://aka.ms/emse5trial).

## <a name="get-started-with-access-reviews"></a>Erişim gözden geçirmeleri ile çalışmaya başlama

Oluşturma ve erişim gözden geçirmesi gerçekleştirme hakkında daha fazla bilgi için bu kısa bir tanıtım izleyin:

>[!VIDEO https://www.youtube.com/embed/6KB3TZ8Wi40]

Erişim gözden geçirmeleri, kuruluşunuzda dağıtmak hazırsanız, videonun ekleme, yöneticilerinize eğitmek için aşağıdaki adımları izleyin ve ilk erişim gözden geçirmesi oluşturma!

>[!VIDEO https://www.youtube.com/embed/X1SL2uubx9M]

## <a name="enable-access-reviews"></a>Erişim gözden geçirmeleri sağlar

Erişim gözden geçirmeleri etkinleştirmek için aşağıdaki adımları izleyin.

1. Bir genel yönetici veya kullanıcı hesabı yöneticisi olarak oturum açın [Azure portalında](https://portal.azure.com) erişim kullanmak istediğiniz inceler.

1. Tıklayın **tüm hizmetleri** ve erişim gözden geçirmeleri, hizmet bulma.

1. Tıklayın **erişim gözden Geçirmeleriyle**.

    ![Tüm hizmetleri - erişim gözden geçirmeleri](./media/access-reviews-overview/all-services-access-reviews.png)

1. Gezinti listesinde **katmanına** açmak için **ekleme erişim gözden geçirmeleri** sayfası.

    ![Erişim gözden geçirmelerine ekleme](./media/access-reviews-overview/onboard-button.png)

1. Tıklayın **Oluştur** erişimi etkinleştirmek için geçerli dizinde inceler.

    ![Erişim gözden geçirmeleri ekle](./media/access-reviews-overview/onboard-access-reviews.png)

    Erişim yeniden başlattığınızda Gözden Geçiren, erişim gözden geçirme seçenekleri etkin olacaktır.

    ![Erişim gözden geçirmeleri etkin](./media/access-reviews-overview/access-reviews-enabled.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Grupları ve uygulamaları, erişim gözden geçirmesi oluştur](create-access-review.md)
- [Bir Azure AD yönetici rolündeki kullanıcılar için erişim gözden geçirmesi oluşturma](../privileged-identity-management/pim-how-to-start-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json)
- [Gruplar veya uygulamalar için erişim gözden geçirin](perform-access-review.md)
- [Grupları ve uygulamaları, erişim değerlendirmesi tamamlama](complete-access-review.md)
