---
title: dosya dahil etme
description: dosya dahil etme
ms.topic: include
ms.custom: include file
services: time-series-insights
ms.service: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.date: 10/02/2020
ms.openlocfilehash: 7de4dc21391f7dbd817c56ce51606a808cf9e3c4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91665829"
---
1. [Azure Portal](https://ms.portal.azure.com/) **Azure Active Directory**  >  **App registrations**  >  **Yeni kayıt**uygulama kayıtları Azure Active Directory seçin.

   [![Azure Active Directory yeni uygulama kaydı](media/time-series-insights-aad-registration/active-directory-new-application-registration.png)](media/time-series-insights-aad-registration/active-directory-new-application-registration.png#lightbox)

    Uygulamanız kaydedildikten sonra burada listelenecektir.

1. Uygulamaya bir ad verin ve yalnızca API 'ye erişebilen **Desteklenen hesap türlerini** belirtmek için **bu kuruluş dizinindeki hesapları** seçin. [Ortak bir istemci uygulaması](https://docs.microsoft.com/azure/active-directory/develop/msal-client-application-configuration#redirect-uri)oluşturuyorsanız, geçerli bir yeniden yönlendirme URI 'si ekleyin ve ardından **kaydolun**.

   [![Uygulamayı Azure Active Directory oluşturma](media/time-series-insights-aad-registration/active-directory-registration.png)](media/time-series-insights-aad-registration/active-directory-registration.png#lightbox)

1. Önemli Azure Active Directory uygulama bilgileri, listelenen uygulamanızın **genel bakış** dikey penceresinde görüntülenir. **Sahip olunan uygulamalar**altında uygulamanızı seçin ve **genel bakış**' a tıklayın.

   [![Uygulama KIMLIĞINI kopyalama](media/time-series-insights-aad-registration/active-directory-copy-application-id.png)](media/time-series-insights-aad-registration/active-directory-copy-application-id.png#lightbox)

   İstemci uygulamanızda kullanmak için **uygulamanızın (istemci) kimliğini** kopyalayın.

1. **Kimlik doğrulama** dikey penceresi, önemli kimlik doğrulama yapılandırma ayarlarını belirtir.

    1. **+ Platform Ekle**' ye tıklayarak **yeniden yönlendirme URI 'Leri** ekleyin ve **erişim belirteçlerini** yapılandırın.

    1. Uygulamanın **ortak bir istemci** olup olmadığını ve **Evet** veya **Hayır**' a tıklayarak olmadığını saptayın.

    1. Hangi hesapların ve kiracıların desteklendiğini doğrulayın.

    [![Örtük izni Yapılandır](media/time-series-insights-aad-registration/active-directory-auth-blade.png)](media/time-series-insights-aad-registration/active-directory-auth-blade.png#lightbox)

1. Uygun platformu seçtikten sonra, **yeniden yönlendirme URI** 'larınızı ve yan paneldeki **erişim belirteçlerinizi** Kullanıcı arabiriminin sağına yapılandırın.

    1. **Yeniden yönlendirme URI 'leri** , kimlik doğrulama isteği tarafından sağlanan adresle eşleşmelidir:

        * Yerel bir geliştirme ortamında barındırılan uygulamalar için **ortak istemci (mobil & Masaüstü)** öğesini seçin. **Ortak Istemciyi** **Evet**olarak ayarladığınızdan emin olun.
        * Azure App Service barındırılan Single-Page uygulamalar için **Web**' i seçin.

    1. **Oturum kapatma URL 'sinin** uygun olup olmadığını belirleme.

    1. **Erişim belirteçlerini** veya **kimlik belirteçlerini**denetleyerek örtük izin akışını etkinleştirin.

    [![Yeniden yönlendirme URI 'Leri oluşturma](media/time-series-insights-aad-registration/active-directory-auth-redirect-uri.png)](media/time-series-insights-aad-registration/active-directory-auth-redirect-uri.png#lightbox)

    **Yapılandır**' a ve ardından **Kaydet**' e tıklayın.

1. İstemci uygulamanızın kimliğini kanıtlamak için kullanabileceği bir uygulama parolası oluşturmak için **sertifikalar & gizlilikler** ' ı ve ardından **yeni istemci gizli** anahtarını seçin.

   [![Yeni bir istemci parolası oluştur](media/time-series-insights-aad-registration/active-directory-application-keys-save.png)](media/time-series-insights-aad-registration/active-directory-application-keys-save.png#lightbox)

   Daha sonra, istemci gizli parolanız görüntülenir. Anahtarı en sevdiğiniz metin düzenleyiciye kopyalayın.

   > [!NOTE]
   > Bunun yerine bir sertifikayı içeri aktarma olanağınız vardır. Gelişmiş güvenlik için bir sertifika önerilir. Bir sertifika kullanmak için **sertifikayı karşıya yükle**' yi seçin.

1. Azure Active Directory uygulamanızın Azure Time Series Insights ilişkilendirin. **API izinlerini**seçin  >  **Add a permission**  >  **Kuruluşumun kullandığı izin API 'leri**ekleyin.

    [![Azure Active Directory uygulamanızla bir API 'YI ilişkilendirme](media/time-series-insights-aad-registration/active-directory-app-api-permission.png)](media/time-series-insights-aad-registration/active-directory-app-api-permission.png#lightbox)

   `Azure Time Series Insights`Arama çubuğuna yazın ve ardından öğesini seçin `Azure Time Series Insights` .

1. Ardından, uygulamanızın gerektirdiği tür API iznini belirtin. Varsayılan olarak, **temsilci izinleri** vurgulanacaktır. Bir izin türü seçin, sonra **Izin Ekle**' yi seçin.

    [![Uygulamanızın gerektirdiği API izninin türünü belirtin](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png)](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png#lightbox)
