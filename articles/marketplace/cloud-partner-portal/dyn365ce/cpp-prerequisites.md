---
title: Dynamics 365 müşteri katılımı için Önkoşullar - Azure Marketi'nde teklif | Microsoft Docs
description: Azure uygulaması yayımlama önkoşulları Azure Marketi'nde teklif.
services: Dynamics 365 for Customer Engagement offer, Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 12/20/2018
ms.author: pbutlerm
ms.openlocfilehash: 4b4859c41e7a3903de68b62e8587f1c85805a782
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54083336"
---
# <a name="dynamics-365-for-customer-engagement-prerequisites"></a>Dynamics 365 müşteri katılımı önkoşulları

Bu makalede, bir Dynamics 365 Customer Engagement uygulama teklifine AppSource Market'te yayımlamak için teknik ve işletmeye önkoşulları açıklanır.


## <a name="technical-requirements"></a>Teknik gereksinimler

Dynamics 365 Customer Engagement uygulaması için uymalıdır [Microsoft Appsource'ta uygulamayı gözden geçirme yönergeleri](https://smp-cdn-prod.azureedge.net/documents/AppsourceGuidelines/Microsoft%20AppSource%20app%20review%20guidelines_v5.pdf), aşağıdaki gereksinimleri içerir:


|              Gereksinim             |        Açıklama           |
|            ---------------           |      ---------------         |
| Azure Active Directory tümleştirmesi   | Uygulamanızı Azure Active Directory Federasyon çoklu oturum açma izin vermelidir (AAD Federasyon SSO) ile etkin onay. Daha fazla bilgi için [Azure Active Directory için AppSource sertifikalı alma](https://docs.microsoft.com/azure/active-directory/develop/howto-get-appsource-certified). |
| (İsteğe bağlı) Microsoft Cloud services ile tümleştirme | Bu işlev, gerekli olduğunda, uygulama hizmetleri, Microsoft Power BI, Microsoft Flow veya Microsoft Azure machine learning gibi veya bilişsel hizmetler gibi diğer Microsoft Cloud services ile tümleşmelidir. |
| İş kolu satır odaklanır            |  Uygulamanız gerekir iyi tanımlanmış iş süreci üzerinde odaklanmak veya sorun, öncelikli olarak iş müşterileri hedefleyin ve kendi iş kimlik bilgilerinizle (kullanıcı adı ve parola) oturum açmasını etkinleştirin.  |
| Ücretsiz deneme süresi ve deneme sürümü deneyimi |  Bir müşteri uygulamanızı sınırlı bir süre için ücretsiz kullanmak mümkün olması gerekir: "Bunu şimdi al" için uygulamalar, bir "ücretsiz deneme sürümü", "Test sürüşü" demonstrator gibi belirli bir süre için ücretsiz ya da bir "benimle iletişim kurun" seçeneği istek.  |
| Hayır/düşük yapılandırma                 | Uygulamanızı, kolay ve hızlı yapılandırıp (hiçbir geliştirme veya özelleştirme gerekli ayarlama) olmalıdır.  |
| Müşteri desteği                     | Uygulamanızı müşterilerin Yardım nereden destek bağlantısı içermelidir desteği.  |
| Kullanılabilirlik/çalışma süresi                  | Uygulamanızı en az % 99,9 çalışma süresi olması gerekir. |
|  |  |


## <a name="business-requirements"></a>İş gereksinimleri

Aşağıdaki yordam, sözleşmeye dayalı ve yasal yükümlülüklerin yerine iş gereksinimleri şunlardır:

* Üzerinde kaydedilmelidir [Microsoft iş ortağı ağı (MPN)](https://partners.microsoft.com/PartnerProgram/simplifiedenrollment.aspx) veya kayıtlı bir bulut Market yayımcısı. Kayıtlı değil, adımları [bulut Market yayımcısı haline](../../become-publisher.md).  (Üçüncü adım için bunun yerine kullanın [AppSource iş ortağı ADAYLIK formu](https://appsource.microsoft.com/partners/signup)). 

    >[!NOTE]
    >Bulut iş ortağı portalında oturum açmak için aynı Microsoft Developer Center kayıt hesabı kullanmanız gerekir. Azure Marketi Teklifleriniz için yalnızca bir Microsoft hesabı olması gerekir. Bu hesap, bireysel hizmetlerin veya teklifler için belirli olmamalıdır.

* Appsource'ta yayımlama seçeneği ticari etkin sağlamaz çünkü hiçbir ek yatırım ya da değişiklikler geçerli sıralama ve fatura altyapı kullanmalısınız.
* Teknik Destek kullanılabilir müşterilere ticari açıdan makul bir şekilde yapmaktan sorumlu olursunuz. Bu destek, ücretsiz, ücretli veya topluluk yaklaşım olabilir.
* Yazılımınızı ve üçüncü taraf yazılım bağımlılıkları lisansı sağlamaktan sorumlu.
* İlişkili pazarlama Güvenceleri resmi uygulama adı, açıklama (HTML biçiminde), logo görüntülerde PNG biçiminde (40 x 40, 90 x 90, 115 x 115 ve 255 x 115 piksel cinsinden) ve kullanım koşulları ve gizlilik ilkesi gibi oluşturmuş olmalıdır.  


## <a name="next-steps"></a>Sonraki adımlar

Bu gereksinimleri karşıladığınızdan sonra [Dynamics 365 müşteri katılımı teklif oluşturma](./cpp-create-offer.md) 
