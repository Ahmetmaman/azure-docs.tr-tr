---
title: Azure API Management geliştirici portalına genel bakış
titleSuffix: Azure API Management
description: API tüketicilerinin API 'lerinizi keşfedebileceği, API Management-özelleştirilebilir bir Web sitesindeki geliştirici portalı hakkında bilgi edinin.
services: api-management
documentationcenter: API Management
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/15/2020
ms.author: apimpm
ms.openlocfilehash: 30487218fc95be75d22b5a9ea5a6dbc224ffd025
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93074806"
---
# <a name="overview-of-the-developer-portal"></a>Geliştirici portalına genel bakış

Geliştirici portalı, API 'lerinizin belgelerinde otomatik olarak oluşturulan ve tamamen özelleştirilebilir bir Web sitesidir. API tüketicilerinin API 'lerinizi bulabileceği, bunları nasıl kullanacağınızı, erişim isteyeceğini ve bunları nasıl deneyebileceği de vardır.

Bu makalede, API Management 'de geliştirici portalının şirket içinde barındırılan ve yönetilen sürümleri arasındaki farklar açıklanmaktadır. Ayrıca, sık sorulan soruların yanıtlarını da sağlar.

![API Management geliştirici portalı](media/api-management-howto-developer-portal/cover.png)

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="migration-from-the-legacy-portal"></a>Eski portaldan geçiş

> [!IMPORTANT]
> Eski geliştirici portalı artık kullanım dışıdır ve yalnızca güvenlik güncelleştirmelerini alacaktır. Bu hizmeti, her zamanki gibi kullanmaya devam edebilirsiniz, bu, tüm API Management hizmetlerinden kaldırılacak olan 2023 Ekim 'de devre dışı kalır.

Yeni geliştirici portalına geçiş, [adanmış belgeler makalesinde](developer-portal-deprecated-migration.md)açıklanmaktadır.

## <a name="customization-and-styling"></a>Özelleştirme ve stil oluşturma

Geliştirici portalı, yerleşik, sürükle ve bırak görsel Düzenleyicisi aracılığıyla özelleştirilebilir ve Stillenebilir. Daha fazla bilgi için [Bu öğreticiye](api-management-howto-developer-portal-customize.md) bakın.

## <a name="extensibility"></a><a name="managed-vs-self-hosted"></a> Genişletilebilirlik

API Management hizmetiniz yerleşik, her zaman güncel ve **yönetilen** bir geliştirici portalı içerir. Azure portal arabiriminden erişebilirsiniz.

Kullanıma hazır olmayan, özel mantık ile genişletmeniz gerekiyorsa, kod temelini değiştirebilirsiniz. Portalın kod temeli [bir GitHub deposunda mevcuttur][1]. Örneğin, bir üçüncü taraf destek sistemiyle tümleşen yeni bir pencere öğesi uygulayabilirsiniz. Yeni işlevsellik uyguladığınızda, aşağıdaki seçeneklerden birini seçebilirsiniz:

- Elde edilen portalı API Management hizmetinizin dışında **kendi kendine barındırın** . Portalı kendinden barındırdığınızda, bakımcı olur ve yükseltmelerinden sorumlu olursunuz. Azure desteğinin yardımı, [deponun wiki bölümünde][2]belgelendiği gibi yalnızca şirket içinde barındırılan portalların temel kurulumuyla sınırlıdır.
- Yeni işlevselliği **yönetilen** portalın kod tabanına birleştirmek için API Management ekibine yönelik bir çekme isteği açın.

Genişletilebilirlik ayrıntıları ve yönergeleri için [GitHub deposuna][1] ve [pencere öğesi uygulama öğreticilerine][3]bakın. [Yönetilen portalı özelleştirme öğreticisi](api-management-howto-developer-portal-customize.md) , portalın yönetim panelinde, **yönetilen** ve **Şirket içinde barındırılan** sürümler için ortak olan bir adım adım yol gösterir.

## <a name="frequently-asked-questions"></a><a name="faq"></a> Sık sorulan sorular

Bu bölümde, genel doğası olan geliştirici portalı hakkında sık sorulan soruları yanıtlarız. Şirket içinde barındırılan sürüme özgü sorular için [GitHub deposunun wiki bölümüne](https://github.com/Azure/api-management-developer-portal/wiki)bakın.

### <a name="how-can-i-migrate-from-the-preview-version-of-the-portal"></a><a id="preview-to-ga"></a> Portalın önizleme sürümünden nasıl geçiş yapabilirim?

Geliştirici portalının önizleme sürümünü ilk kez başlattığınızda, API Management hizmetinizde varsayılan içeriğinin önizleme sürümünü sağlamış olursunuz. Varsayılan içerik, genel olarak kullanılabilen sürümde önemli ölçüde değiştirilmiştir. Örneğin, varsayılan içeriğin önizleme sürümü, oturum açma sayfalarında OAuth düğmeleri içermiyorsa, API 'Leri görüntülemek için farklı pencere öğeleri kullanır ve geliştirici portalı sayfalarını yapılandırmak için sınırlı yeteneklere bağımlıdır. İçerikte farklılıklar olsa da, geliştirici portalınızı her yayımladığınızda portalın altyapısı (temel pencere öğeleri dahil) otomatik olarak güncelleştirilir.

Portalınızı içeriğin önizleme sürümüne göre büyük ölçüde özelleştirdiyseniz, bunu olduğu gibi kullanmaya devam edebilir ve yeni pencere öğelerini portalın sayfalarına el ile yerleştirebilirsiniz. Aksi takdirde, portalınızın içeriğini yeni varsayılan içerikle değiştirmeniz önerilir.

Yönetilen bir portaldaki içeriği sıfırlamak için, **işlemler** menüsü bölümünde **içeriği Sıfırla** ' yı seçin. Bu işlem portalın tüm içeriğini kaldıracak ve yeni varsayılan içerik sağlayacak. Tüm geliştirici portalı özelleştirmelerini ve değişikliklerini kaybedersiniz. **Bu eylemi geri alamazsınız**.

![Portal içeriğini sıfırlama](media/api-management-howto-developer-portal/reset-content.png)

Şirket içinde barındırılan sürümü kullanıyorsanız, `scripts.v2/cleanup.bat` `scripts.v2/generate.bat` mevcut içeriği kaldırmak ve yeni içerik sağlamak için GitHub deposundan çalıştırma ve betikler çalıştırın. Portalınızın kodunu önceden GitHub deposundan en son sürüme yükseltdiğinizden emin olun.

İlk olarak, Kasım 2019 ' de genel kullanılabilirlik duyurusu sonrasında portala eriştiyseniz, yeni varsayılan içeriği zaten kullanıma hazır hale, başka bir eylem gerekli değildir.

### <a name="functionality-i-need-isnt-supported-in-the-portal"></a>Gerekli işlevsellik portalda desteklenmiyor

[GitHub deposunda][1] bir özellik isteği açabilir veya [eksik işlevselliği kendiniz uygulayabilirsiniz][3]. Daha fazla ayrıntı için yukarıdaki **genişletilebilirlik** bölümüne bakın.

### <a name="how-can-i-automate-portal-deployments"></a><a id="automate"></a> Portal dağıtımlarını nasıl otomatikleştirebilirim?

Yönetilen veya şirket içinde barındırılan bir sürümü kullanıyor olmanız fark etmeksizin, geliştirici portalının içeriğine REST API aracılığıyla programlı bir şekilde erişebilir ve yönetebilirsiniz.

API, [GitHub deposunun wiki bölümünde][2]belgelenmiştir. Ortamlar arasında portal içeriğinin (örneğin, bir test ortamından üretim ortamına) otomatik olarak geçirilmesi için kullanılabilir. Bu işlem hakkında daha fazla bilgi için GitHub 'daki [Bu belge makalesine](https://aka.ms/apimdocs/migrateportal) daha fazla bilgi edinebilirsiniz.

### <a name="how-do-i-move-from-the-managed-to-the-self-hosted-version"></a>Nasıl yaparım?, yönetilene şirket içinde barındırılan sürüme geçiş mi?

[GitHub 'daki geliştirici portalı deposunun wiki bölümündeki][2]ayrıntılı makaleye bakın.

### <a name="can-i-have-multiple-developer-portals-in-one-api-management-service"></a>Tek bir API Management hizmetinde birden çok geliştirici portalı olabilir mi?

Bir yönetilen portala ve birden çok şirket içinde barındırılan portala sahip olabilirsiniz. Tüm portalların içeriği aynı API Management hizmetinde depolanır, bu nedenle aynı olur. Portalların görünüm ve işlevselliğini birbirinden ayırt etmek istiyorsanız, bu dosyaları, örneğin URL 'sini temel alarak çalışma zamanında sayfaları dinamik olarak özelleştiren kendi özel pencere arkadaşlarınızla birlikte barındırabilirsiniz.

### <a name="does-the-portal-support-azure-resource-manager-templates-andor-is-it-compatible-with-api-management-devops-resource-kit"></a>Portal Azure Resource Manager şablonları destekliyor mu ve/veya API Management DevOps kaynak seti ile uyumlu mı?

Hayır.

### <a name="is-the-portals-content-saved-with-the-backuprestore-functionality-in-api-management"></a>Portalın içeriği API Management yedekleme/geri yükleme işleviyle mi kaydedildi?

Hayır.

### <a name="do-i-need-to-enable-additional-vnet-connectivity-for-the-managed-portal-dependencies"></a>Yönetilen Portal bağımlılıkları için ek VNet bağlantısını etkinleştirmem gerekir mi?

Çoğu durumda-Hayır.

API Management hizmetiniz bir iç VNet içindeyse, geliştirici portalınızın yalnızca ağ içinden erişilebilir olması gerekir. Yönetim uç noktasının ana bilgisayar adı, portalın yönetim arabirimine erişmek için kullandığınız makineden hizmetin iç VIP 'sine çözümlenmelidir. Yönetim uç noktasının DNS 'de kayıtlı olduğundan emin olun. Yanlış yapılandırma durumunda bir hata görürsünüz: `Unable to start the portal. See if settings are specified correctly in the configuration (...)` .

API Management hizmetiniz bir iç sanal ağda ise ve bu ağa Internet 'ten Application Gateway üzerinden erişiyorsanız, geliştirici portalına ve API Management yönetim uç noktalarına bağlantıyı etkinleştirdiğinizden emin olun. Web uygulaması güvenlik duvarı kurallarını devre dışı bırakmanız gerekebilir. Daha fazla bilgi için [Bu belge makalesine](api-management-howto-integrate-internal-vnet-appgateway.md) bakın.

### <a name="i-have-assigned-a-custom-api-management-domain-and-the-published-portal-doesnt-work"></a>Özel bir API Management etki alanı atadım ve yayımlanan Portal çalışmıyor

Etki alanını güncelleştirdikten sonra değişikliklerin etkili olması için [portalı yeniden yayımlamanız](api-management-howto-developer-portal-customize.md#publish) gerekir.

### <a name="i-have-added-an-identity-provider-and-i-cant-see-it-in-the-portal"></a>Bir kimlik sağlayıcısı ekledim ve Portal 'da göremiyorum

Bir kimlik sağlayıcısını yapılandırdıktan sonra (örneğin, Azure AD, Azure AD B2C), değişikliklerin etkili olması için [portalı yeniden yayımlamanız](api-management-howto-developer-portal-customize.md#publish) gerekir. Geliştirici portalı sayfalarınızın OAuth düğmeleri pencere öğesini içerdiğinden emin olun.

### <a name="i-have-set-up-delegation-and-the-portal-doesnt-use-it"></a>Temsilciyi ayarladım ve Portal bunu kullanmıyor

Temsilciyi ayarladıktan sonra değişikliklerin etkili olması için portalı yeniden [yayımlamanız](api-management-howto-developer-portal-customize.md#publish) gerekir.

### <a name="my-other-api-management-configuration-changes-havent-been-propagated-in-the-developer-portal"></a>Diğer API Management yapılandırma Değişikliklerim Geliştirici Portalında yayılmadı

Çoğu yapılandırma değişikliği (örneğin, VNet, oturum açma, ürün koşulları) [portalın yeniden yayımmesini](api-management-howto-developer-portal-customize.md#publish)gerektirir.

### <a name="im-getting-a-cors-error-when-using-the-interactive-console"></a><a name="cors"></a> Etkileşimli konsolu kullanırken CORS hatası alıyorum

Etkileşimli konsol tarayıcıdan istemci tarafı API isteği yapar. API 'lerinize [CORS ilkesi](api-management-cross-domain-policies.md#CORS) ekleyerek CORS sorununu çözün.

CORS ilkesinin durumunu, Azure portal API Management hizmetinizin **Portal genel bakış** bölümünde bulabilirsiniz. Bir uyarı kutusu, eksik veya yanlış yapılandırılmış bir ilkeyi gösterir.

![CORS ilkenizin durumunu nereden kontrol edebilirsiniz gösteren ekran görüntüsü.](media/api-management-howto-developer-portal/cors-azure-portal.png)

CORS ilkesini **Etkinleştir** düğmesine tıklayarak OTOMATIK olarak CORS ilkesini uygulayın.

CORS 'yi el ile de etkinleştirebilirsiniz.

1. Oluşturulan ilke kodunu görmek için **bunu genel düzey bağlantı üzerinde el ile Uygula '** yı seçin.
2. Azure portal API Management hizmetinizin **API 'leri** bölümünde **tüm API** 'lere gidin.
3. **</>** **Gelen işleme** bölümünde simgesini seçin.
4. İlkeyi **<inbound>** XML dosyasının bölümüne ekleyin. **<origin>** Değerin geliştirici portalının etki alanı ile eşleştiğinden emin olun.

> [!NOTE]
> 
> CORS ilkesini, API (ler) kapsamı yerine ürün kapsamında uygularsanız ve API 'niz bir üst bilgi aracılığıyla abonelik anahtarı kimlik doğrulaması kullanıyorsa, konsolunuz çalışmaz.
>
> Tarayıcı, abonelik anahtarı içeren bir üst bilgi içermeyen bir seçenekler HTTP isteği otomatik olarak yayımlar. Eksik abonelik anahtarı nedeniyle API Management, bu seçenek çağrısını bir ürünle ilişkilendiremez, bu nedenle CORS ilkesini uygulayamaz.
>
> Geçici bir çözüm olarak, abonelik anahtarını bir sorgu parametresine geçirebilirsiniz.

> [!NOTE]
> 
> Yalnızca bir CORS ilkesi yürütülür. Birden çok CORS ilkesi belirttiyseniz (örneğin, API düzeyinde ve tümü-API düzeyinde), etkileşimli konsolunuz beklendiği gibi çalışmayabilir.

### <a name="what-permissions-do-i-need-to-edit-the-developer-portal"></a>Geliştirici portalını düzenlemek için hangi izinlere ihtiyacım var?

`Oops. Something went wrong. Please try again later.`Portalı yönetim modunda açtığınızda hata görüyorsanız, gerekli izinlere (Azure RBAC) gerek duyulmayabilir.

Eski portallar, `Microsoft.ApiManagement/service/getssotoken/action` `/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ApiManagement/service/<apim-service-name>` Kullanıcı yöneticisinin portallara erişmesine izin vermek için hizmet kapsamında () izni gerektirdi. Yeni Portal kapsamda izin gerektirir `Microsoft.ApiManagement/service/users/token/action` `/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ApiManagement/service/<apim-service-name>/users/1` .

Gerekli izne sahip bir rol oluşturmak için aşağıdaki PowerShell betiğini kullanabilirsiniz. Parametresini değiştirmeyi unutmayın `<subscription-id>` . 

```powershell
#New Portals Admin Role 
Import-Module Az 
Connect-AzAccount 
$contributorRole = Get-AzRoleDefinition "API Management Service Contributor" 
$customRole = $contributorRole 
$customRole.Id = $null
$customRole.Name = "APIM New Portal Admin" 
$customRole.Description = "This role gives the user ability to log in to the new Developer portal as administrator" 
$customRole.Actions = "Microsoft.ApiManagement/service/users/token/action" 
$customRole.IsCustom = $true 
$customRole.AssignableScopes.Clear() 
$customRole.AssignableScopes.Add('/subscriptions/<subscription-id>') 
New-AzRoleDefinition -Role $customRole 
```
 
Rol oluşturulduktan sonra, Azure portal **Access Control (IAM)** bölümünde herhangi bir kullanıcıya verilebilir. Bu rolün bir kullanıcıya atanması, bu izni hizmet kapsamında atayacaktır. Kullanıcı, hizmette *herhangi bir* Kullanıcı adına SAS belirteçleri üretebilecektir. En azından, bu rolün hizmetin yöneticisine atanması gerekir. Aşağıdaki PowerShell komutu, `user1` kullanıcıya gereksiz izinler vermekten kaçınmak için rolün en düşük kapsamdaki bir kullanıcıya nasıl atanacağını gösterir: 

```powershell
New-AzRoleAssignment -SignInName "user1@contoso.com" -RoleDefinitionName "APIM New Portal Admin" -Scope "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ApiManagement/service/<apim-service-name>/users/1" 
```

İzinler bir kullanıcıya verildikten sonra, yeni izinlerin etkili olması için kullanıcının oturumu kapatıp Azure portal yeniden oturum açması gerekir.

### <a name="im-seeing-the-unable-to-start-the-portal-see-if-settings-are-specified-correctly--error"></a>`Unable to start the portal. See if settings are specified correctly (...)`Hatayı görüyorum

Bu hata, `GET` çağrısı başarısız olduğunda gösterilir `https://<management-endpoint-hostname>/subscriptions/xxx/resourceGroups/xxx/providers/Microsoft.ApiManagement/service/xxx/contentTypes/document/contentItems/configuration?api-version=2018-06-01-preview` . Bu çağrı tarayıcıdan portalın yönetim arabirimi tarafından verilir.

API Management hizmetiniz VNet 'deyse, yukarıdaki VNet bağlantı sorusuna başvurun.

Çağrı hatasına Ayrıca özel bir etki alanına atanan ve tarayıcı tarafından güvenilmeyen bir TLS/SSL sertifikası neden olabilir. Risk azaltma olarak, yönetim uç noktası özel etki alanını kaldırabilirsiniz. API Management, güvenilen bir sertifika ile varsayılan uç noktaya geri döner.

### <a name="whats-the-browser-support-for-the-portal"></a>Portal için tarayıcı desteği nedir?

| Tarayıcı                     | Desteklenir       |
|-----------------------------|-----------------|
| Apple Safari                | Evet<sup>1</sup> |
| Google Chrome               | Evet<sup>1</sup> |
| Microsoft Edge              | Evet<sup>1</sup> |
| Microsoft Internet Explorer | No              |
| Mozilla Firefox             | Evet<sup>1</sup> |

 <small><sup>1</sup> en son iki üretim sürümünde desteklenir.</small>

## <a name="next-steps"></a>Sonraki adımlar

Yeni geliştirici portalı hakkında daha fazla bilgi edinin:

- [Yönetilen geliştirici portalına erişme ve bunları özelleştirme](api-management-howto-developer-portal-customize.md)
- [Portalın şirket içinde barındırılan sürümünü ayarlama][2]
- [Kendi pencere öğesini uygulama][3]

Diğer kaynaklara gözatamazsınız:

- [Kaynak kodlu GitHub deposu][1]

[1]: https://aka.ms/apimdevportal
[2]: https://github.com/Azure/api-management-developer-portal/wiki
[3]: https://aka.ms/apimdevportal/extend
