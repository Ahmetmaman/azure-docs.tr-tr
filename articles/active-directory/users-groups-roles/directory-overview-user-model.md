---
title: Azure Active Directory Kullanıcılar, gruplar, lisanslama ve roller
description: Azure Active Directory'deki kullanıcılar ile atanan lisanslar, yönetici rolleri ve grup üyelikleri arasındaki ilişki
keywords: ''
author: curtand
manager: daveba
ms.author: curtand
ms.reviewer: vincesm
ms.date: 06/16/2020
ms.topic: overview
ms.service: active-directory
ms.subservice: users-groups-roles
ms.workload: identity
services: active-directory
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 89c96da30411705bf33b1c63f79a9beef11962a1
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86223831"
---
# <a name="what-is-enterprise-user-management"></a>Kurumsal Kullanıcı yönetimi nedir?

Bu makale Azure AD yöneticilerine gruplar, lisanslar, dağıtılan kurumsal uygulamalar ve yönetici rolleri açısından kullanıcılara yönelik yaygın [kimlik yönetimi](/azure/active-directory/fundamentals/identity-fundamentals?context=azure/active-directory/users-groups-roles/context/ugr-context) görevleri arasındaki ilişki hakkında bilgi sunmaktadır. Kuruluşunuz büyüdükçe Azure AD grupları ve yönetici rollerini kullanarak şu işlemleri gerçekleştirebilirsiniz:

* Lisansları kişiler yerine gruplara atama
* Azure AD yönetim görevlerini daha az ayrıcalıklı rollere dağıtmak için izinleri devretme
* Gruplara kurumsal uygulama erişimi atama

## <a name="assign-users-to-groups"></a>Kullanıcıları gruplara atama

Azure AD'deki grupları kullanarak çok sayıda kullanıcıya lisans atayabilir veya dağıtılan kurumsal uygulamalara kullanıcı erişimi verebilirsiniz. Azure AD 'de Genel yönetici hariç tüm yönetici rollerini atamak için grupları kullanabilir veya SaaS uygulamaları veya SharePoint siteleri gibi dış kaynaklara erişim izni verebilirsiniz.

Daha fazla esneklik sağlamak veya grup üyeliği yönetim yükünü azaltmak için Azure AD'deki [dinamik grupları](groups-create-rule.md) kullanarak grup üyeliklerinin otomatik olarak verilmesini ve alınmasını sağlayabilirsiniz. Bir veya daha fazla dinamik gruba üye olan her benzersiz kullanıcı için bir Azure AD Premium P1 lisansına ihtiyacınız vardır.

## <a name="assign-licenses-to-groups"></a>Gruplara lisans atama

Lisansları kullanıcılara tek tek atama veya kaldırma işlemleri zaman ve dikkat gerektirebilir. Bunun yerine [lisansları gruplara atayarak](/azure/active-directory/fundamentals/license-users-groups?context=azure/active-directory/users-groups-roles/context/ugr-context) büyük ölçekli lisans yönetim işlemlerini daha kolay gerçekleştirebilirsiniz.

Azure AD'de lisans atanan bir gruba katılan kullanıcılara uygun lisanslar otomatik olarak atanır. Azure AD gruptan ayrılan kullanıcıların lisans atamalarını kaldırır. Azure AD gruplarını kullanmadığınızda kuruluşa katılan veya ayrılan kullanıcılara toplu olarak lisans eklemek ya da lisansları kaldırmak için PowerShell betiği yazmanız veya Graph API'sini kullanmanız gerekir.

Yeterli sayıda lisans olmadığında veya aynı anda atanamayan hizmet planları gibi durumlar oluştuğunda Azure portalda lisanslama sorunlarının durumunu görebilirsiniz.

>[!NOTE]
>Grup tabanlı lisanslama özelliği şu anda genel önizleme sürümündedir. Önizleme sırasında bu özellik tüm ücretli Azure Active Directory (Azure AD) lisans planlarında veya deneme sürümlerinde sağlanır.

## <a name="delegate-administrator-roles"></a>Yönetici rollerini devretme

Çoğu büyük kuruluş, uygulama kaydetmesi gereken kullanıcılar gibi çalışanlarına güçlü Genel Yönetici rolünü atamadan işlerini yapma imkanı sağlayacak yeterli izinler sunmayı istemektedir. Uygulama yönetimi görevlerini daha ayrıntılı bir şekilde dağıtmanıza yardımcı olacak yeni Azure AD yönetici rolleri aşağıda verilmiştir:

 Rol adı | İzin özeti
 --------- | -------------------
 **Uygulama Yöneticisi** | Kurumsal uygulamalar ve uygulama kayıtları ekleyip yönetebilir, uygulama proxy'si ayarlarını yapılandırabilir. Uygulama yöneticileri, koşullu erişim ilkelerini ve cihazlarını görüntüleyebilir, ancak yönetmez.
 **Bulut Uygulaması Yöneticisi** | Kurumsal uygulamalar ve kurumsal uygulama kayıtları ekleyip yönetebilir. Bu rol, Uygulama Yöneticisi rolünün tüm izinlerine sahiptir ancak uygulama proxy'si ayarlarını yönetemez.
**Uygulama Geliştirici** | Uygulama kayıtlarını ekleyip yönetebilir ancak kurumsal uygulamaları yönetemez ve uygulama proxy'si yapılandıramaz.

Yeni Azure AD yönetici rolleri eklenmektedir. Kullanılabilir rollerin listesi için Azure portala veya [yönetici rolü izin başvurusuna](directory-assign-admin-roles.md) bakın.

## <a name="assign-app-access"></a>Uygulama erişimi atama

Azure AD [kuruluşunuzda dağıtılan kurumsal uygulamalara](/azure/active-directory/manage-apps/methods-for-assigning-users-and-groups?context=azure/active-directory/users-groups-roles/context/ugr-context)grup erişimi atamak IÇIN Azure ad kullanabilirsiniz. Dinamik grupları uygulamalara grup atama özelliğiyle birlikte kullanarak kuruluşunuz büyürken kullanıcıların uygulama erişimi atamalarını otomatikleştirebilirsiniz. Kurumsal uygulamalara erişim atamak için Azure Active Directory Premium P1 veya Premium P2 lisansına ihtiyacınız olacaktır.

Azure AD ayrıca uygulama ile erişim atadığınız gruplar arasındaki veri akışı üzerinde ayrıntılı denetime sahip olmanızı da sağlar. [Kurumsal Uygulamalar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps) sayfasında bir uygulamayı açıp **Sağlama**'yı seçerek:

* Destekleyen uygulamalar için otomatik sağlamayı ayarlayabilirsiniz
* Uygulamanın kullanıcı yönetim API'sine bağlanmak için kimlik bilgileri girebilirsiniz
* Kullanıcı hesapları sağlandığında veya güncelleştirildiğinde Azure AD ile uygulama arasında akışı gerçekleştirilen kullanıcı özniteliklerini denetleyen eşlemeler ayarlayabilirsiniz
* Bir uygulama için Azure AD sağlama hizmetini başlatıp durdurabilir, sağlama önbelleğini silebilir veya hizmeti yeniden başlatabilirsiniz
* Azure AD ile uygulama arasında oluşturulan, güncelleştirilen ve kaldırılan tüm kullanıcıların ve grupların günlüğünü sağlayan **Sağlama etkinlik raporunu** ve daha ayrıntılı hata iletilerini içeren **Sağlama hata raporunu** görüntüleyebilirsiniz

## <a name="next-steps"></a>Sonraki adımlar

Yeni bir Azure AD yöneticisiyseniz temel bilgiler için bkz. [Azure Active Directory Temel Konuları](https://docs.microsoft.com/azure/active-directory/fundamentals/index).

Dilerseniz [grup oluşturma](/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal?context=azure/active-directory/users-groups-roles/context/ugr-context), [lisans atama](/azure/active-directory/fundamentals/license-users-groups?context=azure/active-directory/users-groups-roles/context/ugr-context), [uygulama erişimi atama](/azure/active-directory/manage-apps/methods-for-assigning-users-and-groups?context=azure/active-directory/users-groups-roles/context/ugr-context) veya [yönetici rolü atama](directory-assign-admin-roles.md) işlemlerine başlayabilirsiniz.
