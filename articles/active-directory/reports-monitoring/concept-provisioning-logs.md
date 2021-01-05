---
title: Azure Active Directory portalında günlükleri sağlama (Önizleme) | Microsoft Docs
description: Azure Active Directory portalında günlük raporlarını sağlamaya giriş
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 12/28/2020
ms.author: markvi
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 56818862b6bc4eb38b819185aceb121e6e78488e
ms.sourcegitcommit: 7e97ae405c1c6c8ac63850e1b88cf9c9c82372da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/29/2020
ms.locfileid: "97803536"
---
# <a name="provisioning-reports-in-the-azure-active-directory-portal-preview"></a>Azure Active Directory portalında raporları sağlama (Önizleme)

Azure Active Directory (Azure AD) içindeki raporlama mimarisi aşağıdaki bileşenlerden oluşur:

- **Etkinlik** 
    - **Oturum açma** bilgileri – yönetilen uygulamaların kullanımı ve Kullanıcı oturum açma etkinlikleri hakkında bilgiler.
    - **Denetim günlükleri**  -  [Denetim günlükleri](concept-audit-logs.md) , kullanıcılar ve Grup Yönetimi, yönetilen uygulamalar ve Dizin etkinlikleri hakkında sistem etkinliği bilgilerini sağlar.
    - **Günlükleri sağlama** -Azure AD sağlama hizmeti tarafından sağlanan Kullanıcı, Grup ve roller hakkında sistem etkinliği sağlar. 

- **Güvenlik** 
    - **Riskli oturum** açma işlemleri- [riskli oturum](../identity-protection/overview-identity-protection.md) açma, bir kullanıcı hesabının meşru sahibi olmayan birisi tarafından gerçekleştirilmiş olabilecek oturum açma girişimine yönelik bir göstergedir.
    - **Risk için Işaretlenen kullanıcılar** - [riskli bir Kullanıcı](../identity-protection/overview-identity-protection.md) , tehlikeye girmiş olabilecek bir kullanıcı hesabı göstergesidir.

Bu konu, sağlama raporuna genel bir bakış sağlar.

## <a name="prerequisites"></a>Önkoşullar

### <a name="who-can-access-the-data"></a>Verilere kimler erişebilir?
* Uygulama sahipleri, sahip oldukları uygulamalar için günlükleri görüntüleyebilir
* Güvenlik Yöneticisi, güvenlik okuyucu, rapor okuyucu, uygulama Yöneticisi ve bulut uygulaması yönetici rolleri kullanıcıları
* [Provisioninglogs iznine](https://docs.microsoft.com/azure/active-directory/roles/custom-enterprise-app-permissions#full-list-of-permissions) sahip özel bir roldeki kullanıcılar
* Genel Yöneticiler


### <a name="what-azure-ad-license-do-you-need-to-access-provisioning-activities"></a>Sağlama etkinliklerine erişmek için hangi Azure AD lisansınızın olması gerekir?

Tüm sağlama etkinliği raporunu görmek için kiracınızın kendisiyle ilişkili bir Azure AD Premium lisansı olması gerekir. Azure Active Directory sürümünüzü yükseltmek için bkz. [Azure Active Directory Premium kullanmaya](../fundamentals/active-directory-get-started-premium.md) başlama. 

## <a name="provisioning-logs"></a>Sağlama günlükleri

Sağlama günlükleri aşağıdaki soruların yanıtlarını sağlar:

* ServiceNow 'da hangi gruplar başarıyla oluşturuldu?
* Hangi kullanıcılar Adobe 'dan başarıyla kaldırıldı?
* DropBox 'ta başarısız olan kullanıcılar nelerdir?

Sağlama günlüklerine, [Azure portal](https://portal.azure.com) **Azure Active Directory** dikey penceresinin **izleme** bölümünde **sağlama günlükleri** ' ni seçerek erişebilirsiniz. Bazı sağlama kayıtlarının portalda gösterilmesi iki saate kadar sürebilir.

![Sağlama günlükleri](./media/concept-provisioning-logs/access-provisioning-logs.png "Sağlama günlükleri")


Bir sağlama günlüğü, şunu gösteren bir varsayılan liste görünümüne sahiptir:

- Kimlik
- Eylem
- Kaynak sistem
- Hedef sistem
- Durum
- Tarih


![Varsayılan sütunlar](./media/concept-provisioning-logs/default-columns.png "Varsayılan sütunlar")

Araç çubuğunda **Sütunlar**’a tıklayarak liste görünümünü özelleştirebilirsiniz.

![Sütun seçici](./media/concept-provisioning-logs/column-chooser.png "Sütun seçici")

Bu sayede ek alanları görüntüleyebilir ya da zaten görüntülenen alanları kaldırabilirsiniz.

![Kullanılabilir sütunlar](./media/concept-provisioning-logs/available-columns.png "Kullanılabilir sütunlar")

Daha ayrıntılı bilgi edinmek için liste görünümünde bir öğe seçin.

![Ayrıntılı bilgiler](./media/concept-provisioning-logs/steps.png "Filtre")


## <a name="filter-provisioning-activities"></a>Sağlama etkinliklerini filtrele

Sağlama verilerinizi filtreleyebilirsiniz. Bazı filtre değerleri, kiracınıza göre dinamik olarak doldurulur. Örneğin, kiracınızda herhangi bir oluşturma olayınız yoksa, Create için bir filtre seçeneği olmayacaktır.
Varsayılan görünümde, aşağıdaki filtreleri seçebilirsiniz:

- Kimlik
- Tarih
- Durum
- Eylem


![Filtre ekleme](./media/concept-provisioning-logs/default-filter.png "Filtre")

**Kimlik** filtresi, ilgilendiğiniz adı veya kimliği belirtmenize olanak sağlar. Bu kimlik bir Kullanıcı, Grup, rol ya da başka bir nesne olabilir. Nesnenin adına veya KIMLIğINE göre arama yapabilirsiniz. KIMLIK senaryoya göre farklılık gösterir. Örneğin, Azure AD 'den SalesForce 'a bir nesne sağlarken, kaynak KIMLIĞI Azure AD 'deki kullanıcının nesne KIMLIĞIDIR, ancak targetID, Salesforce 'daki kullanıcının KIMLIĞIDIR. Workday 'den Active Directory 'e sağlanırken kaynak KIMLIĞI iş günü çalışan çalışan KIMLIĞIDIR. Kullanıcı adının Kimlik sütununda her zaman mevcut olamayacağını unutmayın. Her zaman bir KIMLIK olur. 


**Tarih** filtresi, döndürülen veriler için bir zaman çerçevesi tanımlamanıza olanak sağlar.  
Olası değerler şunlardır:

- 1 ay
- 7 gün
- 30 gün
- 24 saat
- Özel zaman aralığı

Özel bir zaman çerçevesini seçtiğinizde, bir başlangıç tarihi ve bitiş tarihi yapılandırabilirsiniz.


**Durum** filtresi şunları seçmenizi sağlar:

- Tümü
- Başarılı
- Hata
- Atlandı



**Eylem** filtresi aşağıdakileri filtrelemenizi sağlar:

- Oluştur 
- Güncelleştirme
- Sil
- Devre Dışı Bırak
- Diğer

Ayrıca, varsayılan görünümün filtrelerine aşağıdaki filtreleri de ayarlayabilirsiniz:

- İş kimliği
- Geçiş KIMLIĞI
- Değişiklik KIMLIĞI
- Kaynak KIMLIĞI
- Hedef KIMLIĞI
- Uygulama


![Bir alan seçin](./media/concept-provisioning-logs/add-filter.png "Bir alan seçin")


- **Iş kimliği** -benzersiz BIR iş kimliği, sağlamayı etkinleştirdiğiniz her uygulamayla ilişkilendirilir.   

- **Cycle ID** -sağlama döngüsünü benzersiz şekilde tanımlar. Bu KIMLIĞI, bu olayın gerçekleştiği döngüyü aramak için desteklemek üzere paylaşabilirsiniz.

- **Kimliği Değiştir** -sağlama olayı için benzersiz tanımlayıcı. Bu KIMLIĞI, sağlama olayını aramak için desteklemek üzere paylaşabilirsiniz.   


- **Kaynak sistem** -kimliğin nereden sağlandığını belirtmenize olanak sağlar. Örneğin, Azure AD 'den ServiceNow 'a bir nesne sağlanırken kaynak sistem Azure AD ' dir. 

- **Hedef sistem** -kimliğin nerede sağlandığını belirtmenize olanak sağlar. Örneğin, Azure AD 'den ServiceNow 'a bir nesne sağlanırken, hedef sistem ServiceNow olur. 

- **Uygulama** -yalnızca belirli bir dizeyi içeren bir görünen ada sahip uygulama kayıtlarını göstermenizi sağlar.

 

## <a name="provisioning-details"></a>Sağlama ayrıntıları 

Sağlama listesi görünümünde bir öğe seçtiğinizde, bu öğe hakkında daha fazla ayrıntı alırsınız.
Ayrıntılar aşağıdaki kategorilere göre gruplandırılır:

- Adımlar

- Sorun giderme ve öneriler

- Değiştirilen özellikler

- Özet


![Sağlama ayrıntıları](./media/concept-provisioning-logs/provisioning-tabs.png "Sekmeler")



### <a name="steps"></a>Adımlar

**Adımlar** sekmesi, bir nesne sağlamak için kullanılan adımları özetler. Bir nesnenin sağlanması dört adımdan oluşabilir: 

- Nesne içeri aktar
- Nesnenin kapsam içinde olup olmadığını belirleme
- Kaynak ve hedef arasında nesne Eşleştir
- Nesne sağla (işlem yapın, bu bir oluşturma, güncelleştirme, silme veya devre dışı bırakma olabilir)



![Ekran görüntüsü, sağlama adımlarını gösteren adımlar sekmesini gösterir.](./media/concept-provisioning-logs/steps.png "Filtre")


### <a name="troubleshoot-and-recommendations"></a>Sorun giderme ve öneriler


**Sorun giderme ve öneriler** sekmesi, hata kodunu ve nedenini sağlar. Hata bilgileri yalnızca bir hata durumunda kullanılabilir. 


### <a name="modified-properties"></a>Değiştirilen özellikler

**Değiştirilen özellikler** eski değeri ve yeni değeri gösterir. Eski değer olmadığı durumlarda eski değer sütunu boştur. 


### <a name="summary"></a>Özet

**Özet** sekmesi, kaynak ve hedef sistemdeki nesne için ne olduğunu ve tanımlayıcılara genel bakış sağlar. 

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

- Azure portal, bir Premium sürümünüz varsa ve ücretsiz sürümünüz varsa 7 gün sonra, bildirilen sağlama verilerini 30 gün boyunca depolar. Sağlama günlükleri, 30 günden daha fazla bekletme için [Log Analytics](../app-provisioning/application-provisioning-log-analytics.md) 'e yayımlanabilir. 

- Değişiklik KIMLIĞI özniteliğini benzersiz tanımlayıcı olarak kullanabilirsiniz. Bu örneğin, ürün desteğiyle etkileşim kurarken faydalı olur.

- Şu anda bir CSV dosyası olarak sağlama verileri indirme seçeneği yoktur, ancak [Microsoft Graph](/graph/api/provisioningobjectsummary-list?tabs=http&view=graph-rest-beta)kullanarak verileri dışarı aktarabilirsiniz.

- Kapsamda olmayan kullanıcılar için Atlanan olayları görebilirsiniz. Özellikle eşitleme kapsamı tüm kullanıcılar ve gruplar olarak ayarlandığında bu beklenmektedir. Hizmetimiz, Kiracıdaki tüm nesneleri, kapsam dışı olsalar da değerlendirmeyecektir. 

- Sağlama günlükleri Şu anda kamu bulutu 'nda kullanılamıyor. Sağlama günlüklerine erişemedik, lütfen geçici bir geçici çözüm olarak denetim günlüklerini kullanın. 

- Sağlama günlükleri rol içeri aktarmaları göstermez (AWS, SalesForce ve ZenDesk için geçerlidir). Rol içeri aktarmaları için Günlükler denetim günlüklerinde bulunabilir. 

## <a name="error-codes"></a>Hata Kodları

Sağlama günlüklerinde bulabileceğiniz hataların nasıl çözümleneceğini daha iyi anlamak için aşağıdaki tabloyu kullanın. Eksik olan herhangi bir hata kodu için, bu sayfanın en altındaki bağlantıyı kullanarak geri bildirim sağlayın. 

|Hata Kodu|Açıklama|
|---|---|
|Çakışma, EntryConflict|Çakışan Kullanıcı hesabının eşleştirilmek ve üzerinde alınması gerekiyorsa, Azure AD 'de veya uygulamada çakışan öznitelik değerlerini düzeltin ya da eşleşen öznitelik yapılandırmanızı gözden geçirin. Eşleşen öznitelikleri yapılandırma hakkında daha fazla bilgi için aşağıdaki [belgeleri](../app-provisioning/customize-application-attributes.md) gözden geçirin.|
|TooManyRequests|Hedef uygulama, aşırı yüklenmiş ve çok fazla istek aldığından kullanıcıyı güncelleştirme girişimini reddetti. Yapılacak bir şey yok. Bu deneme otomatik olarak kullanımdan kaldırılacak. Microsoft bu sorunla aynı zamanda bilgilendirildi.|
|InternalServerError |Hedef uygulama beklenmeyen bir hata döndürdü. Hedef uygulamayla ilgili olarak çalışmasını engelleyen bir hizmet sorunu olabilir. Bu deneme, 40 dakika içinde otomatik olarak kullanımdan kaldırılacaktır.|
|InsufficientRights, MethodNotAllowed, NotAllowed, yetkisiz| Azure AD, hedef uygulamayla kimlik doğrulaması yapabiliyor, ancak güncelleştirmeyi gerçekleştirme yetkisi yoktu. Lütfen hedef uygulamanın sunduğu yönergeleri ve ilgili uygulama [öğreticisini](../saas-apps/tutorial-list.md)gözden geçirin.|
|UnprocessableEntity|Hedef uygulama beklenmeyen bir yanıt döndürdü. Hedef uygulamanın yapılandırması doğru olmayabilir veya hedef uygulamayla bunun çalışmasını engelleyen bir hizmet sorunu olabilir.|
|WebExceptionProtocolError |Hedef uygulamaya bağlanılırken bir HTTP protokol hatası oluştu. Yapılacak bir şey yok. Bu deneme, 40 dakika içinde otomatik olarak kullanımdan kaldırılacaktır.|
|Invalidanchor|Sağlama hizmeti tarafından daha önce oluşturulmuş veya eşleştirilmiş bir Kullanıcı artık yok. Kullanıcının mevcut olduğundan emin olmak için denetleyin. Tüm kullanıcıların yeniden eşleşmesini zorlamak için, [işi yeniden başlatmak](/graph/api/synchronization-synchronizationjob-restart?tabs=http&view=graph-rest-beta)üzere MS Graph API kullanın. Yeniden başlatmanın sağlanması, tamamlanması zaman alabilir ve bir başlangıç döngüsünü tetikleyeceğini unutmayın. Ayrıca, sağlama hizmetinin çalışması için kullandığı önbelleği de siler, yani Kiracıdaki tüm kullanıcılar ve gruplar yeniden değerlendirilmek ve belirli sağlama olayları bırakılamaz.|
|NotImplemented | Hedef uygulama beklenmeyen bir yanıt döndürdü. Uygulamanın yapılandırması doğru olmayabilir veya hedef uygulamayla bunun çalışmasını engelleyen bir hizmet sorunu olabilir. Lütfen hedef uygulamanın sunduğu yönergeleri ve ilgili uygulama [öğreticisini](../saas-apps/tutorial-list.md)gözden geçirin. |
|MandatoryFieldsMissing, MissingValues |Gerekli değerler eksik olduğundan kullanıcı oluşturulamadı. Kaynak kayıttaki eksik öznitelik değerlerini düzeltin veya gerekli alanların atlanmadığından emin olmak için eşleşen öznitelik yapılandırmanızı gözden geçirin. Eşleşen öznitelikleri yapılandırma hakkında [daha fazla bilgi edinin](../app-provisioning/customize-application-attributes.md) .|
|SchemaAttributeNotFound |Hedef uygulamada mevcut olmayan bir öznitelik belirtildiğinden, işlem gerçekleştirilemedi. Öznitelik özelleştirme hakkındaki [belgelere](../app-provisioning/customize-application-attributes.md) bakın ve yapılandırmanızın doğru olduğundan emin olun.|
|InternalError |Azure AD sağlama hizmeti içinde bir iç hizmet hatası oluştu. Yapılacak bir şey yok. Bu deneme 40 dakika içinde otomatik olarak yeniden denenecek.|
|Invaliddomain |Geçersiz bir etki alanı adı içeren bir öznitelik değeri nedeniyle işlem gerçekleştirilemedi. Kullanıcının etki alanı adını güncelleştirin veya hedef uygulamadaki izin verilen listeye ekleyin. |
|Zaman aşımı |Hedef uygulamanın yanıt vermesi çok uzun sürdüğü için işlem tamamlanamadı. Yapılacak bir şey yok. Bu deneme 40 dakika içinde otomatik olarak yeniden denenecek.|
|Licenselimitexceıbaşında|Bu Kullanıcı için kullanılabilir lisans olmadığından, Kullanıcı hedef uygulamada oluşturulamadı. Hedef uygulama için ek lisanslar temin edebilir ya da doğru özniteliklerin doğru özniteliklerle atandığından emin olmak için Kullanıcı atamalarınızı ve öznitelik eşleme yapılandırmanızı gözden geçirin.|
|DuplicateTargetEntries  |Hedef uygulamada yapılandırılmış eşleşen özniteliklerle birden fazla kullanıcı bulunduğundan işlem tamamlanamadı. Hedef uygulamadan yinelenen kullanıcıyı kaldırın ya da öznitelik eşlemelerinizi [burada](../app-provisioning/customize-application-attributes.md)açıklandığı gibi yeniden yapılandırın.|
|DuplicateSourceEntries | Yapılandırılmış eşleşen özniteliklerle birden fazla kullanıcı bulunduğundan işlem tamamlanamadı. Yinelenen kullanıcıyı kaldırın ya da öznitelik eşlemelerinizi [burada](../app-provisioning/customize-application-attributes.md)açıklandığı gibi yeniden yapılandırın.|
|Importatlandı | Her kullanıcı değerlendirildiğinde, kullanıcıyı kaynak sistemden içeri aktarmayı deneiyoruz. Bu hata genellikle, içeri aktarılan Kullanıcı öznitelik eşlemelerinizde tanımlanan eşleşen özellik eksikse oluşur. Eşleşen öznitelik için Kullanıcı nesnesinde bir değer olmadan kapsam, eşleme veya dışarı aktarma değişikliklerini değerlendiremedik. Not, bu hatanın varlığı kullanıcının kapsam içinde olduğunu göstermez, ancak kullanıcı için kapsamları henüz değerlendirdik.|
|Entrysynchronizationatlandı | Sağlama Hizmeti, kaynak sistemi başarıyla sorguladı ve kullanıcıyı tanımladı. Kullanıcı üzerinde başka bir eylem yapılmadı ve bunlar atlandı. Atlama, kullanıcının kapsam dışına çıkmasına veya daha fazla değişiklik gerekmeden hedef sistemde zaten mevcut olan kullanıcıdan kaynaklanıyor olabilir.|
|Systemforcrossdomainıdentitymanagementmultipleentriesınresponse| Kullanıcı veya Grup almak için bir GET isteği gerçekleştirirken, yanıtta birden çok kullanıcı veya grup aldık. Yanıtta yalnızca bir kullanıcı veya grup almayı bekliyorduk. [Örneğin](../app-provisioning/use-scim-to-provision-users-and-groups.md#get-group), bir grubu almak ve üyeleri hariç tutmak için bir filtre sağlamak üzere bir get ISTEğI ve SCIM uç noktanız üyeleri döndürürse, bu hatayı oluşturacağız.|

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanıcı hazırlama durumunu denetleme](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md)
* [Azure AD Galeri uygulamasına kullanıcı sağlamayı yapılandırma sorunu](../app-provisioning/application-provisioning-config-problem.md)
* [Sağlama günlükleri Graph API 'SI](/graph/api/resources/provisioningobjectsummary?view=graph-rest-beta)
