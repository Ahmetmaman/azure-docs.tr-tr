---
title: Application Insights özel ilkelerle ilgili sorunları giderme
titleSuffix: Azure AD B2C
description: Özel ilkelerinizin yürütülmesini izlemek için Application Insights ayarlama.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: troubleshooting
ms.date: 03/10/2021
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 435a0b85d205328d10f8762498c7a981d7ee45f5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "102611836"
---
# <a name="collect-azure-active-directory-b2c-logs-with-application-insights"></a>Application Insights ile Azure Active Directory B2C günlüklerini toplayın

Bu makalede, özel ilkelerinizle ilgili sorunları tanılamanıza olanak tanımak için Active Directory B2C (Azure AD B2C) günlüklerini toplama adımları sunulmaktadır. Application Insights özel durumları tanılamak ve uygulama performansı sorunlarını görselleştirmek için bir yol sağlar. Azure AD B2C'nin Application Insights'a veri göndermek için bir özelliği vardır.

Burada açıklanan ayrıntılı etkinlik günlükleri **yalnızca** özel ilkelerinizin geliştirilmesi sırasında etkinleştirilmelidir.

> [!WARNING]
> Öğesini `DeploymentMode` üretim ortamlarında olarak ayarlamayın `Development` . Günlükler, kimlik sağlayıcılarından ve bu kaynaklardan gönderilen tüm talepleri toplar. Geliştirici, Application Insights günlüklerinde toplanan kişisel verilerin sorumluluğunu kabul eder. Bu ayrıntılı Günlükler yalnızca, ilke **GELIŞTIRICI moduna** yerleştirildiğinde toplanır.

## <a name="set-up-application-insights"></a>Application Insights ayarlama

Henüz bir tane yoksa, aboneliğinizde bir Application Insights örneği oluşturun.

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Üst menüden **Dizin + abonelik** filtresi ' ni seçin ve Azure aboneliğinizi içeren dizini (Azure AD B2C dizininiz değil) seçin.
1. Sol taraftaki gezinti menüsünde **kaynak oluştur** ' u seçin.
1. **Application Insights** arayıp seçin ve ardından **Oluştur**' u seçin.
1. Formu doldurun, **gözden geçir + oluştur**' u seçin ve ardından **Oluştur**' u seçin.
1. Dağıtım tamamlandıktan sonra **Kaynağa Git**' i seçin.
1. Application Insights menüsünde **Yapılandır** menüsünde **Özellikler**' i seçin.
1. **Izleme anahtarını** sonraki bir adımda kullanmak üzere kaydedin.

## <a name="configure-the-custom-policy"></a>Özel ilkeyi yapılandırma

1. Bağlı olan taraf (RP) dosyasını açın, örneğin *SignUpOrSignin.xml*.
1. Aşağıdaki öznitelikleri `<TrustFrameworkPolicy>` öğesine ekleyin:

   ```xml
   DeploymentMode="Development"
   UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
   ```

1. Zaten mevcut değilse, `<UserJourneyBehaviors>` düğüme bir alt düğüm ekleyin `<RelyingParty>` . Bu, öğesinden sonra yerleştirilmelidir `<DefaultUserJourney ReferenceId="UserJourney Id" from your extensions policy, or equivalent (for example:SignUpOrSigninWithAAD" />` .
1. Aşağıdaki düğümü öğesinin alt öğesi olarak ekleyin `<UserJourneyBehaviors>` . ' İ `{Your Application Insights Key}` daha önce kaydettiğiniz Application Insights **izleme anahtarıyla** değiştirdiğinizden emin olun.

    ```xml
    <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
    ```

    * `DeveloperMode="true"` ApplicationInsights 'ın işleme işlem hattı aracılığıyla Telemetriyi hızlandırmasını söyler. Geliştirme için iyi, ancak yüksek birimlerde kısıtlanmıştır. Üretimde öğesini `DeveloperMode` olarak ayarlayın `false` .
    * `ClientEnabled="true"` İzleme sayfası görünümü ve istemci tarafı hataları için ApplicationInsights istemci tarafı betiği gönderir. Bunları, Application Insights portalındaki **Browserzamanlamalar** tablosunda görüntüleyebilirsiniz. Ayar olarak `ClientEnabled= "true"` , sayfa betiklerine Application Insights ekler ve sayfa yüklerinin ve Ajax çağrılarının, sayımların, tarayıcı özel durumlarının ve Ajax hatalarının ayrıntılarının yanı sıra Kullanıcı ve oturum sayımlarının zamanlamalarını alırsınız. Bu alan **isteğe bağlıdır** ve varsayılan olarak olarak ayarlanır `false` .
    * `ServerEnabled="true"` Application Insights için, var olan Kullanıcıgünneyıkaydedicisi JSON 'sini özel bir olay olarak gönderir.

    Örnek:

    ```xml
    <TrustFrameworkPolicy
      ...
      TenantId="fabrikamb2c.onmicrosoft.com"
      PolicyId="SignUpOrSignInWithAAD"
      DeploymentMode="Development"
      UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
    >
    ...
    <RelyingParty>
      <DefaultUserJourney ReferenceId="UserJourney ID from your extensions policy, or equivalent (for example: SignUpOrSigninWithAzureAD)" />
      <UserJourneyBehaviors>
        <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
      </UserJourneyBehaviors>
      ...
    </TrustFrameworkPolicy>
    ```

1. İlkeyi karşıya yükleyin.

## <a name="see-the-logs-in-application-insights"></a>Günlüklere bakın Application Insights

Application Insights yeni Günlükler görebilmeniz için genellikle beş dakikadan kısa bir gecikme olur.

1. [Azure Portal](https://portal.azure.com)oluşturduğunuz Application Insights kaynağını açın.
1. **Genel bakış** sayfasında **Günlükler**' i seçin.
1. Application Insights yeni bir sekme açın.

Günlükleri görmek için kullanabileceğiniz bir sorgu listesi aşağıda verilmiştir:

| Sorgu | Description |
|---------------------|--------------------|
`traces` | Azure AD B2C tarafından oluşturulan tüm günlüklere bakın |
`traces | where timestamp > ago(1d)` | Son gün için Azure AD B2C tarafından oluşturulan tüm günlüklere bakın

Girişler uzun olabilir. Daha yakından bir görünüm için CSV 'ye aktarın.

Sorgulama hakkında daha fazla bilgi için bkz. [Azure izleyici 'de günlük sorgularına genel bakış](../azure-monitor/logs/log-query-overview.md).

## <a name="configure-application-insights-in-production"></a>Üretimde Application Insights yapılandırma

Üretim ortamınızın performansını ve daha iyi kullanıcı deneyimini geliştirmek için, ilkenizi önemli olmayan iletileri yoksayacak şekilde yapılandırmak önemlidir. Application Insights yalnızca kritik hata iletileri göndermek için aşağıdaki yapılandırmayı kullanın. 

1. `DeploymentMode` [TrustFrameworkPolicy](trustframeworkpolicy.md) özniteliğini olarak ayarlayın `Production` . 

   ```xml
   <TrustFrameworkPolicy xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06" PolicySchemaVersion="0.3.0.0"
   TenantId="yourtenant.onmicrosoft.com"
   PolicyId="B2C_1A_signup_signin"
   PublicPolicyUri="http://yourtenant.onmicrosoft.com/B2C_1A_signup_signin"
   DeploymentMode="Production"
   UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights">
   ```

1. Sürekli ' `DeveloperMode` ın ' [](relyingparty.md#journeyinsights) i olarak ayarlayın `false` .

   ```xml
   <UserJourneyBehaviors>
     <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="false" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
   </UserJourneyBehaviors>
   ```
   
1. İlkenizi karşıya yükleyin ve test edin.

## <a name="next-steps"></a>Sonraki adımlar

Topluluk kimlik geliştiricilerine yardımcı olmak için bir kullanıcı yolculuğu izleyicisi geliştirmiştir. Application Insights örneğinizden okur ve kullanıcı yolculuğu olayları içi iyi yapılandırılmış bir görünüm sağlar. Kaynak kodunu elde edersiniz ve kendi çözümünüze dağıtırsınız.

Kullanıcı yolculuğu oyuncusu Microsoft tarafından desteklenmez ve tamamen olduğu gibi kullanılabilir hale getirilir.

GitHub 'daki Application Insights olayları okuyan görüntüleyici sürümünü buradan öğrenebilirsiniz:

[Azure-Samples/Active-Directory-B2C-Advanced-policies](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/wingtipgamesb2c/src/WingTipUserJourneyPlayerWebApplication)
