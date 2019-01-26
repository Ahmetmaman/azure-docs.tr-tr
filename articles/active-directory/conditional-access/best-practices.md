---
title: Azure Active Directory'de koşullu erişim için en iyi yöntemler | Microsoft Docs
description: Bunun yapılması, koşullu erişim ilkeleri yapılandırırken kaçınmalısınız nedir ve bilmeniz gerekenler hakkında bilgi edinin.
services: active-directory
keywords: uygulamalara koşullu erişim, Azure AD ile koşullu erişim, şirket kaynaklarına güvenli erişim, koşullu erişim ilkeleri
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/24/2019
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 3ed4d3874056eca93e5c94e225ba25d94e7826e3
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2019
ms.locfileid: "54911940"
---
# <a name="best-practices-for-conditional-access-in-azure-active-directory"></a>Azure Active Directory'de koşullu erişim için en iyi uygulamalar

İle [Azure Active Directory (Azure AD) koşullu erişim](../active-directory-conditional-access-azure-portal.md), bulut uygulamalarınızı nasıl yetkili kullanıcılara erişimi denetleyebilirsiniz. Bu makalede hakkında bilgiler sağlar:

- Bilmeniz gerekenler 
- Bunun ne olduğunu, koşullu erişim ilkeleri yapılandırırken yapılması kaçınmanız gerekir. 

Bu makalede tanıdık, kavramlar ve terminoloji özetlenen olduğunu varsayar [Azure Active Directory'de koşullu erişim nedir?](../active-directory-conditional-access-azure-portal.md)



## <a name="whats-required-to-make-a-policy-work"></a>Hangi iş bir ilkeyi gerekli?

Yeni bir ilke oluşturduğunuzda, hiçbir kullanıcıları, grupları, uygulamaları veya seçili erişim denetimleri vardır.

![Bulut uygulamaları](./media/best-practices/02.png)


İlkenizi çalışır hale getirmek için yapılandırmanız gerekir:


|Nesne           | Nasıl                                  | Neden|
|:--            | :--                                  | :-- |
|**Bulut uygulamaları** |Bir veya daha fazla uygulama seçmeniz gerekir.  | Nasıl yetkili kullanıcıların denetim sağlamak için koşullu erişim ilkesi amacı olan bulut uygulamalarına erişebilirsiniz.|
| **Kullanıcılar ve gruplar** | En az bir kullanıcı veya seçili bulut uygulamalarınıza erişimi için yetkilendirilmiş grubu seçmeniz gerekir. | Asla tetiklenmez, hiçbir atanan kullanıcıların ve grupların sahip bir koşullu erişim ilkesi. |
| **Erişim denetimleri** | En az bir erişim denetimi seçmeniz gerekir. | Koşullarınızda sağlanırsa, ilke işlemciniz ne yapılacağını bilmesi gerekir.|




## <a name="what-you-should-know"></a>Bilmeniz gerekenler



### <a name="how-are-conditional-access-policies-applied"></a>Koşullu erişim ilkelerini nasıl uygulanır?

Bir kullanıcı bir uygulama eriştiğinde, birden fazla koşullu erişim ilkesi uygulanabilir. Bu durumda uygulanan tüm ilkeler sağlanmalıdır. Bir ilkeleri, örneğin, bir kullanıcı için mfa'yı gerektirir ve ikinci uyumlu bir cihaz kullanıcı MFA gerekir ve uyumlu bir cihaz olması gerekir. 

Tüm ilkeleri, iki aşamada uygulanır:

- İçinde **ilk** aşaması, tüm ilkeleri değerlendirilir ve koşullar karşılanırsa tüm erişim denetimleri toplanır. 

- İçinde **ikinci** , bilgisi henüz karşılanmadığı gereksinimlerini karşılamak için aşama. Bir ilke erişimi engellerse, engellenmiş ve diğer ilke denetimleri istenir değil. İlkelerin hiçbiri engeller, diğer ilke denetimleri aşağıdaki sırayla istenir:

    ![Sipariş verme](./media/best-practices/06.png)
    
    Dış MFA sağlayıcıları ve kullanım koşulları İleri gelmektedir.



### <a name="how-are-assignments-evaluated"></a>Atamalar nasıl değerlendirilir?

Tüm atamaları mantıksal olarak **and işleciyle**. Yapılandırılmış birden fazla atama varsa, bir ilkeyi tetiklemek için tüm atamaları sağlanmalıdır.  

Kuruluşunuzun ağına dışında yapılan tüm bağlantılar için geçerli bir konum koşulu yapılandırmanız gerekiyorsa:

- Dahil **tüm konumlar**
- Dışlama **güvenilen tüm IP'ler**


### <a name="what-to-do-if-you-are-locked-out-of-the-azure-ad-admin-portal"></a>Azure AD Yönetim Portalı dışında kilitliyse yapmak ne olacak?

Yanlış bir ayar bir koşullu erişim ilkesi nedeniyle Azure AD portalından kilitliyse:

- Kuruluşunuzda henüz engellenmemiş diğer yöneticilerin olup olmadığını doğrulayın. Azure portalına erişimi olan bir yönetici oturum açma etkileyen ilke devre dışı bırakabilirsiniz. 

- İlkeyi kuruluşunuzdaki Yöneticiler hiçbiri güncelleştirebilirsiniz, bir destek talebi göndermek gerekir. Microsoft desteği, gözden geçirin ve erişimi engelleyen koşullu erişim ilkeleri güncelleştirin.


### <a name="what-happens-if-you-have-policies-in-the-azure-classic-portal-and-azure-portal-configured"></a>Klasik Azure portalı ve Azure portalında yapılandırılmış ilkeler varsa ne olur?  

Azure Active Directory tarafından her iki ilke uygulanır ve yalnızca tüm gereksinimleri karşılandığında kullanıcı erişim alır.

### <a name="what-happens-if-you-have-policies-in-the-intune-silverlight-portal-and-the-azure-portal"></a>İlkeleri Intune Silverlight portal ve Azure Portalı'nda varsa ne olur?

Azure Active Directory tarafından her iki ilke uygulanır ve yalnızca tüm gereksinimleri karşılandığında kullanıcı erişim alır.

### <a name="what-happens-if-i-have-multiple-policies-for-the-same-user-configured"></a>Yapılandırılan aynı kullanıcı için birden çok ilke varsa ne olur?  

Her oturum açma, Azure Active Directory, tüm ilkeleri değerlendirir ve kullanıcıya erişim izni verilsin önce tüm gereksinimlerin karşılandığını sağlar. Erişimi engelle, diğer tüm yapılandırma ayarlarını trumps. 


### <a name="does-conditional-access-work-with-exchange-activesync"></a>Koşullu erişim, Exchange ActiveSync ile çalışır mı?

Evet, Exchange ActiveSync koşullu erişim ilkesinde kullanabilirsiniz.






## <a name="what-you-should-avoid-doing"></a>Bunun yapılması kaçınmanız gerekir

Koşullu erişim framework harika yapılandırma esnekliği sağlar. Ancak, büyük esneklik de dikkatli bir şekilde her bir yapılandırma İlkesi istenmeyen sonuçları oluşturmamak için bırakmadan önce gözden geçirmeniz gereken anlamına gelir. Bu bağlamda, özel atamaları tam kümeleri gibi etkileyen dikkat **tüm kullanıcılar / gruplar / bulut uygulamaları**.

Ortamınızda, aşağıdaki yapılandırmaları kaçınmanız gerekir:


**Tüm kullanıcılar için tüm bulut uygulamaları:**

- **Erişimi engelle** -kesinlikle bir fikir değil, tüm kuruluş bu yapılandırma engeller.

- **Uyumlu cihaz gerektir** - kullanıcılar için kayıtlı cihazlarını henüz Bu ilke Intune portalına erişim de dahil olmak üzere tüm erişimi engeller. Kayıtlı bir cihaz olmadan bir yöneticiyseniz Bu ilke, ilkeyi değiştirmek için geri döndükten bulaşmasından engeller.

- **Etki alanına katılmayı gerektirecek** - Bu ilke bloğu erişimi de olan etki alanına katılmış bir cihaz henüz yoksa, kuruluşunuzdaki tüm kullanıcılar için erişimi engellemek için olası.


**Tüm kullanıcılar, tüm bulut uygulamaları, tüm cihaz platformları için:**

- **Erişimi engelle** -kesinlikle bir fikir değil, tüm kuruluş bu yapılandırma engeller.


## <a name="how-should-you-deploy-a-new-policy"></a>Nasıl yeni bir ilke dağıtmanız gerekir mi?

İlk adım, ilkeyi kullanan değerlendirmelidir [durum aracı](what-if-tool.md).

Yeni bir ilke ortamınıza dağıtmaya hazır olduğunuzda, bu aşamada yapmanız gerekir:

1. Küçük bir grup kullanıcı için bir ilke uygulanır ve beklediğiniz gibi davrandığını doğrulayın. 

2.  Daha fazla kullanıcı eklemek için bir ilke genişlettiğinizde, tüm yöneticilerin ilkeden dışlamak devam edin. Bu, yöneticilerin yine de erişim vardır ve bir değişiklik varsa, bir ilke güncelleştirebilirsiniz sağlar.

3. Yalnızca bu gerçekten gerekliyse bir ilke, tüm kullanıcılar için geçerlidir. 

En iyi uygulama, bir kullanıcı hesabıyla oluşturun:

- İlke yönetimi için ayrılmış 
- Tüm ilkelerden hariç


## <a name="policy-migration"></a>İlke geçişi

Geçiş için Azure portalında oluşturmadınız ilkeleri göz önünde bulundurun:

- Şimdi, önce işleyemedi senaryoları karşılayabilirsiniz.

- Bunları birleştirerek yönetmek için kullandığınız ilke sayısına azaltabilir.   

- Tüm koşullu erişim ilkelerinizi tek bir merkezi konumda yönetebilir.

- Klasik Azure portalı kullanımdan kaldırıldı.   


Daha fazla bilgi için [Azure portalında Klasik ilkeleri geçirme](policy-migration.md).


## <a name="next-steps"></a>Sonraki adımlar

Bilmek istiyorsanız:

- Koşullu erişim ilkesi yapılandırmak için bkz. nasıl [Azure Active Directory koşullu erişimiyle belirli uygulamalar için MFA gerektiren](app-based-mfa.md).
- Koşullu erişim ilkelerinizi planlama nasıl [Azure Active Directory'de koşullu erişim dağıtımınızı planlamak nasıl](plan-conditional-access.md).
