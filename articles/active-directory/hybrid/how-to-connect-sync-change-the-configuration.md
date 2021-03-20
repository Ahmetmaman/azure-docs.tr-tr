---
title: 'Azure AD Connect eşitleme: Varsayılan yapılandırmada bir değişiklik yapın'
description: Azure AD Connect eşitlemede yapılandırmada değişiklik yapma konusunda size kılavuzluk eder.
services: active-directory
author: billmath
manager: daveba
ms.assetid: 7b9df836-e8a5-4228-97da-2faec9238b31
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 08/30/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2044653673da10de59d5ff125da44ac1f89e22f9
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96861859"
---
# <a name="azure-ad-connect-sync-make-a-change-to-the-default-configuration"></a>Azure AD Connect eşitleme: Varsayılan yapılandırmada bir değişiklik yapın
Bu makalenin amacı, Azure Active Directory (Azure AD) Connect Sync 'de varsayılan yapılandırmada değişiklik yapma konusunda size yol gösterir. Bazı yaygın senaryolar için adımlar sağlar. Bu bilgi ile kendi iş kurallarınızı temel alarak kendi yapılandırmanızda basit değişiklikler yapabiliyor olmanız gerekir.

> [!WARNING]
> Varsayılan kullanıma hazır eşitleme kurallarında değişiklik yaparsanız, bu değişikliklerin Azure AD Connect bir sonraki güncelleştirilişinde üzerine yazılır, bu da beklenmedik ve olası istenmeyen eşitleme sonuçlarına neden olur.
>
> Varsayılan kullanıma hazır eşitleme kuralları parmak izine sahiptir. Bu kurallarda değişiklik yaparsanız, parmak izi artık eşleşmesiz olur. Azure AD Connect yeni bir sürümünü uygulamaya çalıştığınızda gelecekte sorunlarla karşılaşabilirsiniz. Yalnızca bu makalede açıklanan şekilde değişiklik yapın.

## <a name="synchronization-rules-editor"></a>Eşitleme kuralları Düzenleyicisi
Eşitleme kuralları Düzenleyicisi, varsayılan yapılandırmayı görmek ve değiştirmek için kullanılır. Bunu, **Azure AD Connect** grubunun altındaki **Başlat** menüsünde bulabilirsiniz.  
![Eşitleme kuralı Düzenleyicisi ile Başlat menüsü](./media/how-to-connect-sync-change-the-configuration/startmenu2.png)

Düzenleyiciyi açtığınızda, varsayılan hazır kurallar ' ı görürsünüz.

![Eşitleme kuralı Düzenleyicisi](./media/how-to-connect-sync-change-the-configuration/sre2.png)

### <a name="navigating-in-the-editor"></a>Düzenleyicide gezinme
Düzenleyicinin en üstündeki açılan listeleri kullanarak, belirli bir kuralı hızlıca bulabilirsiniz. Örneğin, proxyAddresses özniteliğinin dahil edildiği kuralları görmek isterseniz, açılan listeleri şu şekilde değiştirebilirsiniz:  
![SRE filtrelemesi](./media/how-to-connect-sync-change-the-configuration/filtering.png)  
Filtrelemeyi sıfırlamak ve yeni bir yapılandırma yüklemek için klavyede F5 tuşuna basın.

Sağ üst köşedeki **Yeni kural ekle** düğmesi vardır. Kendi özel kuralınızı oluşturmak için bu düğmeyi kullanın.

Alt kısımdaki seçili bir eşitleme kuralında hareket eden düğmelerdir. Bunları istediğiniz gibi **düzenleyin** ve **silin** . **Dışarı aktarma** , eşitleme kuralını yeniden oluşturmak Için bir PowerShell betiği oluşturur. Bu yordamla, bir eşitleme kuralını bir sunucudan diğerine taşıyabilirsiniz.

## <a name="create-your-first-custom-rule"></a>İlk özel kuralınızı oluşturma
En yaygın değişiklikler öznitelik akışıdır. Kaynak dizininizdeki veriler Azure AD ile aynı olmayabilir. Bu bölümdeki örnekte, kullanıcının verilen adının her zaman *uygun durumda* olduğundan emin olun.

### <a name="disable-the-scheduler"></a>Zamanlayıcıyı devre dışı bırak
[Zamanlayıcı](how-to-connect-sync-feature-scheduler.md) , varsayılan olarak her 30 dakikada bir çalışır. Değişiklik yaparken ve yeni kurallarınızın sorunlarını giderirken başlatılmadığından emin olun. Zamanlayıcıyı geçici olarak devre dışı bırakmak için PowerShell 'i başlatın ve çalıştırın `Set-ADSyncScheduler -SyncCycleEnabled $false` .

![Zamanlayıcıyı devre dışı bırak](./media/how-to-connect-sync-change-the-configuration/schedulerdisable.png)  

### <a name="create-the-rule"></a>Kuralı oluşturma
1. **Yeni kural ekle**' ye tıklayın.
2. **Açıklama** sayfasında, aşağıdakileri girin:  
   ![Gelen kuralı filtreleme](./media/how-to-connect-sync-change-the-configuration/description2.png)  
   * **Ad**: kurala açıklayıcı bir ad verin.
   * **Açıklama**: başka birinin kuralın ne olduğunu anlayabilmesi için bir açıklama sağlayın.
   * **Bağlı sistem**: Bu, nesnenin bulunduğu sistemdir. Bu durumda **Active Directory Bağlayıcısı**' nı seçin.
   * **Bağlı sistem/meta veri deposu nesne türü**: sırasıyla **Kullanıcı** ve **kişi**' yi seçin.
   * **Bağlantı türü**: Bu değeri **katılacak** şekilde değiştirin.
   * **Öncelik**: sistemde benzersiz bir değer sağlayın. Daha düşük bir sayısal değer, daha yüksek önceliği gösterir.
   * **Etiket**: bunu boş bırakın. Yalnızca Microsoft 'un kullanıma hazır kurallarında bu kutu bir değerle doldurulmalıdır.
3. **Kapsam filtresi** sayfasında, **ıısisnotnull** girin.  
   ![Gelen kural kapsamı filtresi](./media/how-to-connect-sync-change-the-configuration/scopingfilter.png)  
   Bu bölüm, kuralın hangi nesnelere uygulanacağını tanımlamak için kullanılır. Boş bırakılırsa, kural tüm kullanıcı nesnelerine uygulanır. Bununla birlikte, bu, konferans odalarını, hizmet hesaplarını ve diğer kişi olmayan kullanıcı nesnelerini de kapsar.
4. **Kural Birleştir** sayfasında, alanı boş bırakın.
5. **Dönüşümler** sayfasında, **FlowType** ' ı **Expression**' ı değiştirin. **Target özniteliği** için, bir **öğesini seçin.** **Kaynak** Için, **pcase ([II])** girin.
   ![Gelen kural dönüşümleri](./media/how-to-connect-sync-change-the-configuration/transformations.png)  
   Eşitleme altyapısı, hem işlev adı hem de özniteliğin adı için büyük/küçük harfe duyarlıdır. Bir yanlış yazarsanız, kuralı eklediğinizde bir uyarı görürsünüz. Kaydedebilir ve devam edebilirsiniz, ancak kuralı yeniden açıp düzeltmeniz gerekir.
6. Kuralı kaydetmek için **Ekle** ' ye tıklayın.

Yeni özel kuralınız, sistemdeki diğer eşitleme kurallarıyla görünür olmalıdır.

### <a name="verify-the-change"></a>Değişikliği doğrulama
Bu yeni değişiklik ile, beklendiği gibi çalıştığından ve herhangi bir hata oluşturmadığınızdan emin olmak istersiniz. Sahip olduğunuz nesne sayısına bağlı olarak, bu adımı iki şekilde yapabilirsiniz:

- Tüm nesnelerde tam eşitleme çalıştırın.
- Tek bir nesne üzerinde önizleme ve tam eşitleme çalıştırın.

**Başlat** menüsünden **eşitleme hizmetini** açın. Bu bölümdeki adımlar bu araçta bulunur.

**Tüm nesnelerde tam eşitleme**  

   1. En üstteki **Bağlayıcılar** ' ı seçin. Önceki bölümde (Bu durumda Active Directory Domain Services) değiştirdiğiniz bağlayıcıyı belirleyin ve seçin. 
   2. **Eylemler** için **Çalıştır**' ı seçin.
   3. **Tam eşitleme**' yi seçin ve ardından **Tamam**' ı seçin.
   ![Tam eşitleme](./media/how-to-connect-sync-change-the-configuration/fullsync.png)  
   Nesneler artık meta veri deposunda güncelleştirilir. Meta veri deposundaki nesneye bakarak yaptığınız değişiklikleri doğrulayın.

**Tek bir nesne üzerinde önizleme ve tam eşitleme**  

   1. En üstteki **Bağlayıcılar** ' ı seçin. Önceki bölümde (Bu durumda Active Directory Domain Services) değiştirdiğiniz bağlayıcıyı belirleyin ve seçin.
   2. **Arama Bağlayıcısı alanı**' nı seçin. 
   3. Değişikliği test etmek için kullanmak istediğiniz bir nesneyi bulmak için **kapsamı** kullanın. Nesneyi seçin ve **Önizleme**' ye tıklayın. 
   4. Yeni ekranda **yürütme önizlemesi**' yi seçin.  
   ![Önizlemeyi Kaydet](./media/how-to-connect-sync-change-the-configuration/commitpreview.png)  
   Değişiklik artık metadize 'ye kaydedilir.

**Meta veri deposundaki nesneyi görüntüleme**  

1. Değerin beklendiğinden ve kuralın uygulandığından emin olmak için birkaç örnek nesne seçin. 
2. Üstten **Metadize aramasını** seçin. İlgili nesneleri bulmak için ihtiyacınız olan herhangi bir filtre ekleyin. 
3. Arama sonuçlarından bir nesne açın. Öznitelik değerlerine bakın ve ayrıca kuralın beklenen şekilde uygulandığı **eşitleme kuralları** sütununda doğrulayın.  
![Meta veri deposu arama](./media/how-to-connect-sync-change-the-configuration/mvsearch.png)  

### <a name="enable-the-scheduler"></a>Zamanlayıcıyı etkinleştirme
Her şey bekleniyorsa, zamanlayıcıyı yeniden etkinleştirebilirsiniz. PowerShell 'den öğesini çalıştırın `Set-ADSyncScheduler -SyncCycleEnabled $true` .

## <a name="other-common-attribute-flow-changes"></a>Diğer ortak öznitelik akışı değişiklikleri
Önceki bölümde bir öznitelik akışında nasıl değişiklik yapılacağı açıklanmıştır. Bu bölümde bazı ek örnekler sağlanır. Eşitleme kuralını oluşturma adımları kısaltılmıştır, ancak önceki bölümde yer alarak tüm adımları bulabilirsiniz.

### <a name="use-an-attribute-other-than-the-default"></a>Varsayılan değer dışında bir öznitelik kullanın
Bu fabrikam senaryosunda, yerel alfabede verilen ad, soyadı ve görünen ad için kullanılan bir orman vardır. Bu özniteliklerin Latin karakter temsili uzantı özniteliklerinde bulunabilir. Azure AD 'de genel adres listesi oluşturmak ve Microsoft 365 için, kuruluş bunun yerine bu öznitelikleri kullanmak istiyor.

Varsayılan yapılandırmayla, Yerel ormandaki bir nesne şöyle görünür:  
![Öznitelik akışı 1](./media/how-to-connect-sync-change-the-configuration/attributeflowjp1.png)

Diğer öznitelik akışlarıyla bir kural oluşturmak için aşağıdakileri yapın:

1. **Başlangıç** menüsünden **eşitleme kuralları düzenleyicisini** açın.
2. Sol tarafta seçili olan **giriş** seçiliyken **Yeni kural ekle** düğmesine tıklayın.
3. Kurala bir ad ve açıklama verin. Şirket içi Active Directory örneğini ve ilgili nesne türlerini seçin. **Bağlantı türü**' nde, **Birleştir**' i seçin. **Öncelik** için, başka bir kural tarafından kullanılmayan bir sayı seçin. Hazır kurallar 100 ile başlar, bu nedenle 50 değeri bu örnekte kullanılabilir.
  ![Öznitelik akışı 2](./media/how-to-connect-sync-change-the-configuration/attributeflowjp2.png)
4. **Kapsam filtresini** boş bırak. (Yani, ormandaki tüm Kullanıcı nesneleri için geçerlidir.)
5. **JOIN kurallarını** boş bırakın. (Yani, kutudan çıkan kuralın herhangi bir birleştirmeleri işlemesini sağlar.)
6. **Dönüşümler**' de aşağıdaki akışları oluşturun:  
  ![Öznitelik akışı 3](./media/how-to-connect-sync-change-the-configuration/attributeflowjp3.png)
7. Kuralı kaydetmek için **Ekle** ' ye tıklayın.
8. **Synchronization Service Manager** gidin. **Bağlayıcılar**' da, kuralı eklediğiniz bağlayıcıyı seçin. **Çalıştır**' ı seçin ve **tam eşitleme**' yi seçin. Tam eşitleme, tüm nesneleri geçerli kuralları kullanarak yeniden hesaplar.

Bu, bu özel kuralla aynı nesnenin sonucudur:  
![Öznitelik akışı 4](./media/how-to-connect-sync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Özniteliklerin uzunluğu
Dize öznitelikleri varsayılan olarak dizinlenebilir ve en fazla uzunluk 448 karakterdir. Daha fazlasını içerebilen dize öznitelikleriyle çalışıyorsanız, öznitelik akışına aşağıdakileri dahil ettiğinizden emin olun:  
`attributeName` <- `Left([attributeName],448)`.

### <a name="changing-the-userprincipalsuffix"></a>UserPrincipalSuffix 'ı değiştirme
Active Directory ' deki userPrincipalName özniteliği her zaman kullanıcılar tarafından bilinmez ve oturum açma KIMLIĞI olarak uygun olmayabilir. Azure AD Connect eşitleme Yükleme Sihirbazı ile farklı bir öznitelik seçebilirsiniz; Örneğin, *posta*. Ancak bazı durumlarda, öznitelik hesaplanmalıdır.

Örneğin, contoso şirketi, biri üretim ve test için olmak üzere iki Azure AD dizinine sahiptir. Bunlar, test kiracısındaki kullanıcıların oturum açma KIMLIĞINDE başka bir sonek kullanmasını ister:  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`.

Bu ifadede, her şeyi @-sign (Word) ve sabit bir dizeyle Birleştir olarak alın.

### <a name="convert-a-multi-value-attribute-to-single-value"></a>Çok değerli bir özniteliği tek değere Dönüştür
Active Directory içindeki bazı öznitelikler, Active Directory Kullanıcıları ve bilgisayarları 'nda tek değerli görünseler de şemaya çok değerli. Açıklama özniteliği bir örnektir:  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`.

Bu ifadede, özniteliğinde bir değer varsa, özniteliğinde ilk öğeyi (*öğe*) alın, baştaki ve sondaki boşlukları (*trim*) kaldırın ve ardından dizedeki Ilk 448 karakteri (*sol*) tutun.

### <a name="do-not-flow-an-attribute"></a>Bir özniteliği Flow
Bu bölümün senaryosunda arka plan için bkz. [öznitelik akışı Işlemini denetleme](concept-azure-ad-connect-sync-declarative-provisioning.md#control-the-attribute-flow-process).

Bir özniteliği akımanın iki yolu vardır. Birincisi, [Seçilen öznitelikleri kaldırmak](how-to-connect-install-custom.md#azure-ad-app-and-attribute-filtering)için Yükleme Sihirbazı ' nı kullanmaktır. Daha önce özniteliğini hiç eşitlemenizden Bu seçenek işe yarar. Ancak, bu özniteliği eşitlemeyi ve daha sonra bu özelliği kullanarak kaldırmayı başladıysanız, eşitleme altyapısı özniteliğin yönetilmesini durduruyor ve mevcut değerler Azure AD 'de bırakılır.

Bir özniteliğin değerini kaldırmak ve ileride Flow olmadığından emin olmak istiyorsanız, özel bir kural oluşturmanız gerekir.

Bu fabrikam senaryosunda, buluta eşitdiğimiz özniteliklerin bazılarının orada olmaması gerektiğini belirledik. Bu özniteliklerin Azure AD 'den kaldırıldığından emin olmak istiyoruz.  
![Hatalı uzantı öznitelikleri](./media/how-to-connect-sync-change-the-configuration/badextensionattribute.png)

1. Yeni bir gelen eşitleme kuralı oluşturun ve açıklamayı doldurun.
  ![Tanımlarını](./media/how-to-connect-sync-change-the-configuration/syncruledescription.png)
2. **FlowType** için **ifadesiyle** ve **kaynak** için **AuthoritativeNull** ile öznitelik akışları oluşturun. **AuthoritativeNull** değişmez değeri, daha düşük öncelikli bir eşitleme kuralı değeri doldurmayı denese bile, meta veri deposunda değerin boş olması gerektiğini gösterir.
  ![Uzantı öznitelikleri için dönüşüm](./media/how-to-connect-sync-change-the-configuration/syncruletransformations.png)
3. Eşitleme kuralını kaydedin. **Eşitleme hizmetini** başlatın, bağlayıcıyı bulun, **Çalıştır**' ı seçin ve **tam eşitleme**' yi seçin. Bu adım tüm öznitelik akışlarını yeniden hesaplar.
4. Bağlayıcı alanında arama yaparak hedeflenen değişikliklerin verilmek üzere olduğunu doğrulayın.
  ![Aşamalı silme](./media/how-to-connect-sync-change-the-configuration/deletetobeexported.png)

## <a name="create-rules-with-powershell"></a>PowerShell ile kurallar oluşturma
Yalnızca birkaç değişiklik yapmanız durumunda eşitleme kuralı Düzenleyicisi 'nin kullanılması iyi bir şekilde çalışacaktır. Birçok değişiklik yapmanız gerekiyorsa, PowerShell daha iyi bir seçenek olabilir. Bazı gelişmiş özellikler yalnızca PowerShell ile kullanılabilir.

### <a name="get-the-powershell-script-for-an-out-of-box-rule"></a>Hazır olmayan bir kural için PowerShell betiğini alma
Hazır bir kural oluşturan PowerShell betiğini görmek için, eşitleme kuralları Düzenleyicisi ' nde kuralı seçin ve **dışarı aktar**' a tıklayın. Bu eylem, kuralı oluşturan PowerShell betiğini sağlar.

### <a name="advanced-precedence"></a>Gelişmiş öncelik
Hazır olmayan eşitleme kuralları 100 öncelik değeriyle başlar. Birçok ormanınız varsa ve birçok özel değişiklik yapmanız gerekiyorsa, 99 eşitleme kuralları yeterli olmayabilir.

Kullanıma hazır kuralların önüne ek kuralların eklenmesini istediğiniz eşitleme motoruna talimat verebilirsiniz. Bu davranışı almak için aşağıdaki adımları izleyin:

1. Eşitleme kuralları düzenleyicisinde ilk kullanıma hazır eşitleme kuralını (**AD-User birleşimden**) Işaretleyin ve **dışarı aktar**' ı seçin. SR tanımlayıcı değerini kopyalayın.  
![Değişiklikten önce PowerShell](./media/how-to-connect-sync-change-the-configuration/powershell1.png)  
2. Yeni eşitleme kuralını oluşturun. Eşitleme kuralları düzenleyicisini oluşturmak için bunu kullanabilirsiniz. Kuralı bir PowerShell betiğine dışarı aktarın.
3. Daha önce özellik **başında**, kutudan çıkış kuralı ' ndan tanımlayıcı değerini ekleyin. **Önceliği** **0** olarak ayarlayın. Tanımlayıcı özniteliğinin benzersiz olduğundan ve başka bir kuraldan bir GUID 'yi yeniden kullandığınızdan emin olun. Ayrıca **ımmutabletag** özelliğinin ayarlı olmadığından emin olun. Bu özellik yalnızca hazır olmayan bir kural için ayarlanmalıdır.
4. PowerShell betiğini kaydedin ve çalıştırın. Sonuç olarak, özel kuralınıza 100 öncelik değeri atanır ve diğer tüm hazır kurallar artırılır.  
![Değişiklikten sonra PowerShell](./media/how-to-connect-sync-change-the-configuration/powershell2.png)  

Gerektiğinde aynı **Precedencebefore** değerini kullanarak çok sayıda özel eşitleme kuralına sahip olabilirsiniz.

## <a name="enable-synchronization-of-usertype"></a>UserType eşitlemesini etkinleştir
Azure AD Connect, 1.1.524.0 ve sonraki sürümleri içindeki **Kullanıcı** nesneleri Için **UserType** özniteliğinin eşitlenmesini destekler. Daha belirgin olarak, aşağıdaki değişiklikler sunulmuştur:

- Azure AD Bağlayıcısı 'ndaki nesne türü **Kullanıcı** şeması, tür dizesinde olan ve tek değerli olan UserType özniteliğini içerecek şekilde genişletilir.
- Meta veri deposundaki **kişinin** nesne türü şeması, tür dizesinde olan ve tek değerli olan UserType özniteliğini içerecek şekilde genişletilir.

Varsayılan olarak, şirket içi Active Directory karşılık gelen UserType özniteliği olmadığından, UserType özniteliği eşitleme için etkin değildir. Eşitlemeyi el ile etkinleştirmeniz gerekir. Bunu yapmadan önce, Azure AD tarafından zorlanan aşağıdaki davranışı göz önünde bulmalısınız:

- Azure AD, UserType özniteliği için yalnızca iki değer kabul eder: **üye** ve **Konuk**.
- UserType özniteliği Azure AD Connect eşitleme için etkinleştirilmemişse, Dizin eşitleme aracılığıyla oluşturulan Azure AD kullanıcıları, UserType özniteliği **üye** olarak ayarlanmalıdır.
- 1.5.30.0 sürümünden önce Azure AD, mevcut Azure AD kullanıcılarının UserType özniteliğinin Azure AD Connect tarafından değiştirilmesine izin vermedi. Eski sürümlerde, bu yalnızca Azure AD kullanıcılarının oluşturulması sırasında ayarlanabilir ve [PowerShell aracılığıyla değiştirilebilir](/powershell/module/azuread/set-azureaduser).

UserType özniteliğinin eşitlemesini etkinleştirmeden önce, ilk olarak özniteliğin şirket içi Active Directory nasıl türetileceğine karar vermelisiniz. En yaygın yaklaşımlar aşağıda verilmiştir:

- Kaynak öznitelik olarak kullanılmak üzere kullanılmamış bir şirket içi AD özniteliği belirleyin (örneğin, extensionAttribute1). Belirlenen şirket içi AD özniteliği tür **dizesinde** olmalıdır, tek değerli olmalıdır ve **üye** veya **Konuk** değeri içermelidir. 

    Bu yaklaşımı seçerseniz, UserType özniteliğinin eşitlenmesi etkinleştirilmeden önce, belirtilen özniteliğin Azure AD 'ye eşitlenen, şirket içi Active Directory tüm mevcut kullanıcı nesneleri için doğru değerle doldurulduğundan emin olmanız gerekir.

- Alternatif olarak, diğer özelliklerden UserType özniteliğinin değerini de türetebilirsiniz. Örneğin, şirket içi AD userPrincipalName özniteliği etki alanı bölümüyle sonlanıyorsa tüm kullanıcıları **Konuk** olarak eşitlemeniz gerekir <em>@partners.fabrikam123.org</em> . 

    Daha önce belirtildiği gibi Azure AD Connect eski sürümleri, mevcut Azure AD kullanıcılarının UserType özniteliğinin Azure AD Connect tarafından değiştirilmesine izin vermez. Bu nedenle, karar verdiğiniz mantığın, kiracınızdaki tüm mevcut Azure AD kullanıcıları için UserType özniteliğinin zaten nasıl yapılandırıldığı ile tutarlı olduğundan emin olmanız gerekir.

UserType özniteliğinin eşitlemesini etkinleştirme adımları şu şekilde özetlenebilir:

1.  Eşitleme zamanlayıcısını devre dışı bırakın ve devam eden bir eşitleme olmadığından emin olun.
2.  Kaynak özniteliğini şirket içi AD bağlayıcı şemasına ekleyin.
3.  UserType 'ı Azure AD bağlayıcı şemasına ekleyin.
4.  Öznitelik değerini şirket içi Active Directory Flow için bir gelen eşitleme kuralı oluşturun.
5.  Öznitelik değerini Azure AD 'ye Flow bir giden eşitleme kuralı oluşturun.
6.  Tam eşitleme döngüsünü çalıştırın.
7.  Eşitleme zamanlayıcısını etkinleştirin.

>[!NOTE]
> Bu bölümün geri kalanında bu adımlar ele alınmaktadır. Tek ormanlı topolojiyle ve özel eşitleme kuralları olmadan bir Azure AD dağıtımı bağlamında açıklanırlar. Çok ormanlı topolojiniz varsa, özel eşitleme kuralları yapılandırılmışsa veya bir hazırlama sunucusuna sahipseniz, adımları uygun şekilde ayarlamanız gerekir.

### <a name="step-1-disable-the-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>1. Adım: eşitleme zamanlayıcısını devre dışı bırakın ve devam eden bir eşitleme olmadığını doğrulayın
Azure AD 'de istenmeyen değişiklikleri dışarı aktarmayı önlemek için, eşitleme kurallarını güncelleştirme ortasında olduğunuz sırada hiçbir eşitlemenin bulunmadığından emin olun. Yerleşik eşitleme zamanlayıcısını devre dışı bırakmak için:

 1. Azure AD Connect sunucusunda bir PowerShell oturumu başlatın.
 2. Cmdlet 'ini çalıştırarak zamanlanmış eşitlemeyi devre dışı bırakın `Set-ADSyncScheduler -SyncCycleEnabled $false` .
 3.   >  **Eşitleme hizmeti**'ni başlatmak için Synchronization Service Manager açın.
 4. **İşlemler** sekmesine gidin ve *sürmekte* olan bir işlem olmadığından emin olun.

### <a name="step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema"></a>2. Adım: kaynak özniteliğini şirket içi AD bağlayıcı şemasına ekleme
Tüm Azure AD öznitelikleri şirket içi AD Bağlayıcısı alanına aktarılmaz. Kaynak özniteliğini içeri aktarılan özniteliklerin listesine eklemek için:

 1. Synchronization Service Manager **Bağlayıcılar** sekmesine gidin.
 2. Şirket içi AD Bağlayıcısı ' na sağ tıklayın ve **Özellikler**' i seçin.
 3. Açılır iletişim kutusunda, **öznitelikleri Seç** sekmesine gidin.
 4. Kaynak özniteliğinin öznitelik listesinde işaretli olduğundan emin olun.
 5. Kaydetmek için **Tamam** ' ı tıklatın.
![Şirket içi AD bağlayıcı şemasına kaynak özniteliği Ekle](./media/how-to-connect-sync-change-the-configuration/usertype1.png)

### <a name="step-3-add-the-usertype-attribute-to-the-azure-ad-connector-schema"></a>3. Adım: UserType özniteliğini Azure AD bağlayıcı şemasına ekleme
Varsayılan olarak, UserType özniteliği Azure AD Connect alanına aktarılmaz. UserType özniteliğini içeri aktarılan öznitelikler listesine eklemek için:

 1. Synchronization Service Manager **Bağlayıcılar** sekmesine gidin.
 2. **Azure AD Bağlayıcısı** ' na sağ tıklayın ve **Özellikler**' i seçin.
 3. Açılır iletişim kutusunda, **öznitelikleri Seç** sekmesine gidin.
 4. UserType özniteliğinin öznitelik listesinde işaretli olduğundan emin olun.
 5. Kaydetmek için **Tamam** ' ı tıklatın.

![Azure AD bağlayıcı şemasına kaynak özniteliği ekleme](./media/how-to-connect-sync-change-the-configuration/usertype2.png)

### <a name="step-4-create-an-inbound-synchronization-rule-to-flow-the-attribute-value-from-on-premises-active-directory"></a>4. Adım: öznitelik değerini şirket içi Active Directory akıtmak için bir gelen eşitleme kuralı oluşturma
Gelen eşitleme kuralı, öznitelik değerinin şirket içi Active Directory kaynak özniteliğinden metadize 'ye akmasını sağlar:

1. Eşitleme kuralları düzenleyicisini **Başlat**' a giderek eşitleme kuralları düzenleyicisini açın  >  .
2. Arama filtresi **yönünü** **gelen** olarak ayarlayın.
3. Yeni bir gelen kuralı oluşturmak için **Yeni kural ekle** düğmesine tıklayın.
4. **Açıklama** sekmesinde aşağıdaki yapılandırmayı sağlayın:

    | Öznitelik | Değer | Ayrıntılar |
    | --- | --- | --- |
    | Name | *Bir ad belirtin* | Örneğin, *ad 'Den içinde – Kullanıcı UserType* |
    | Description | *Bir açıklama girin* |  |
    | Bağlı sistem | *Şirket içi AD bağlayıcısını seçin* |  |
    | Bağlı sistem nesne türü | **Kullanıcı** |  |
    | Meta veri deposu nesne türü | **Kişi** |  |
    | Bağlantı türü | **Join** |  |
    | Önceliği | *1 – 99 arasında bir sayı seçin* | 1 – 99 özel eşitleme kuralları için ayrılmıştır. Başka bir eşitleme kuralı tarafından kullanılan bir değer seçmeyin. |

5. **Kapsam filtresi** sekmesine gidin ve aşağıdaki yan tümcesiyle **tek bir kapsam filtresi grubu** ekleyin:

    | Öznitelik | İşleç | Değer |
    | --- | --- | --- |
    | adminDescription | NOTSTARTWITH | Kullanıcısını\_ |

    Kapsam filtresi, bu gelen eşitleme kuralının hangi şirket içi AD nesnelerini uygulanacağını belirler. Bu örnekte, Azure AD Kullanıcı geri yazma özelliği ile oluşturulan kullanıcı nesnelerine eşitleme kuralının uygulanmasını önleyen, *ad – Kullanıcı genel* kullanıma hazır eşitleme kuralındaki içinde kullanılan kapsam filtresini kullanırız. Azure AD Connect dağıtımınıza göre kapsam filtresini ince ayar gerekebilir.

6. **Dönüştürme** sekmesine gidin ve istenen dönüştürme kuralını uygulayın. Örneğin, UserType için kaynak özniteliği olarak kullanılmayan bir şirket içi AD özniteliği (extensionAttribute1 gibi) belirlediyseniz, doğrudan öznitelik akışı uygulayabilirsiniz:

    | Akış türü | Target özniteliği | Kaynak | Bir kez Uygula | Birleştirme türü |
    | --- | --- | --- | --- | --- |
    | Direct | UserType | extensionAttribute1 | İşaretlemeyin | Güncelleştir |

    Başka bir örnekte, UserType özniteliğinin değerini diğer özelliklerden türetmek istiyorsunuz. Örneğin, şirket içi AD userPrincipalName özniteliği etki alanı bölümüyle sonlanıyorsa tüm kullanıcıları Konuk olarak eşitlemeniz gerekir <em>@partners.fabrikam123.org</em> . Şunun gibi bir ifade uygulayabilirsiniz:

    | Akış türü | Target özniteliği | Kaynak | Bir kez Uygula | Birleştirme türü |
    | --- | --- | --- | --- | --- |
    | Expression | UserType | IıF (ısun ([userPrincipalName]), ııF (CBool (Instr (LCase ([userPrincipalName]), " @partners.fabrikam123.org ") = 0), "üye", "Konuk"), hata ("UserType belirleme Için userPrincipalName yok")) | İşaretlemeyin | Güncelleştir |

7. Gelen kuralını oluşturmak için **Ekle** ' ye tıklayın.

![Gelen eşitleme kuralı oluştur](./media/how-to-connect-sync-change-the-configuration/usertype3.png)

### <a name="step-5-create-an-outbound-synchronization-rule-to-flow-the-attribute-value-to-azure-ad"></a>5. Adım: öznitelik değerini Azure AD 'ye Flow bir giden eşitleme kuralı oluşturma
Giden eşitleme kuralı, öznitelik değerinin meta veri kaynağından Azure AD 'deki UserType özniteliğine akmasını sağlar:

1. Eşitleme kuralları düzenleyicisine gidin.
2. Arama filtresi **yönünü** **giden** olarak ayarlayın.
3. **Yeni kural ekle** düğmesine tıklayın.
4. **Açıklama** sekmesinde aşağıdaki yapılandırmayı sağlayın:

    | Öznitelik | Değer | Ayrıntılar |
    | ----- | ------ | --- |
    | Name | *Bir ad belirtin* | Örneğin, *AAD 'ye kadar – Kullanıcı UserType* |
    | Description | *Bir açıklama girin* ||
    | Bağlı sistem | *AAD bağlayıcısını seçin* ||
    | Bağlı sistem nesne türü | **Kullanıcı** ||
    | Meta veri deposu nesne türü | **Kişi** ||
    | Bağlantı türü | **Join** ||
    | Önceliği | *1 – 99 arasında bir sayı seçin* | 1 – 99 özel eşitleme kuralları için ayrılmıştır. Başka bir eşitleme kuralı tarafından kullanılan bir değer seçmeyin. |

5. **Kapsam filtresi** sekmesine gidin ve iki yan tümce içeren **tek bir kapsam filtresi grubu** ekleyin:

    | Öznitelik | İşleç | Değer |
    | --- | --- | --- |
    | sourceObjectType | SıFıRA | Kullanıcı |
    | Cloudana kopyalı | Not QUAL | Doğru |

    Kapsam filtresi, bu giden eşitleme kuralının hangi Azure AD nesnelerine uygulanacağını belirler. Bu örnekte, *Çıkış IÇIN ad – Kullanıcı kimliği* olmayan eşitleme kuralına göre aynı kapsam filtresini kullanırız. Eşitleme kuralının şirket içi Active Directory eşitlenmemiş Kullanıcı nesnelerine uygulanmasını önler. Azure AD Connect dağıtımınıza göre kapsam filtresini ince ayar gerekebilir.

6. **Dönüştürme** sekmesine gidin ve aşağıdaki dönüştürme kuralını uygulayın:

    | Akış türü | Target özniteliği | Kaynak | Bir kez Uygula | Birleştirme türü |
    | --- | --- | --- | --- | --- |
    | Direct | UserType | UserType | İşaretlemeyin | Güncelleştir |

7. Giden kuralı oluşturmak için **Ekle** ' ye tıklayın.

![Giden eşitleme kuralı oluştur](./media/how-to-connect-sync-change-the-configuration/usertype4.png)

### <a name="step-6-run-a-full-synchronization-cycle"></a>6. Adım: tam eşitleme döngüsünü çalıştırma
Genel olarak, hem Active Directory hem de Azure AD Bağlayıcısı şemalarına yeni öznitelikler ekledik ve özel eşitleme kuralları sunduğumuz için tam eşitleme çevrimi gereklidir. Azure AD 'ye vermeden önce değişiklikleri doğrulamak istiyorsunuz. 

Tam eşitleme döngüsünü oluşturan adımları el ile çalıştırırken değişiklikleri doğrulamak için aşağıdaki adımları kullanabilirsiniz.

1. Şirket **ıçı ad bağlayıcısında** **tam içeri aktarma** çalıştırın:

   1. Synchronization Service Manager **Bağlayıcılar** sekmesine gidin.
   2. Şirket **ıçı ad Bağlayıcısı** ' na sağ tıklayın ve **Çalıştır**' ı seçin.
   3. Açılır iletişim kutusunda **tam Içeri aktar** ' ı seçin ve ardından **Tamam**' a tıklayın.
   4. İşlemin bitmesini bekleyin.

      > [!NOTE]
      > Kaynak özniteliği içeri aktarılan öznitelikler listesine zaten dahil edil, şirket içi AD bağlayıcısında tam içeri aktarma işlemini atlayabilirsiniz. Diğer bir deyişle, [2. Adım: kaynak özniteliğini şirket ıçı ad bağlayıcı şemasına ekleme konusunda](#step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema)herhangi bir değişiklik yapmanız gerekmez.

2. **Azure AD Bağlayıcısı**'nda **tam içeri aktarma** çalıştırın:

   1. **Azure AD Bağlayıcısı** ' na sağ tıklayın ve **Çalıştır**' ı seçin.
   2. Açılır iletişim kutusunda **tam Içeri aktar** ' ı seçin ve ardından **Tamam**' a tıklayın.
   3. İşlemin bitmesini bekleyin.

3. Mevcut bir kullanıcı nesnesindeki eşitleme kuralı değişikliklerini doğrulayın:

    Şirket içi Active Directory kaynak özniteliği ve Azure AD 'den UserType, ilgili bağlayıcı alanlarına içeri aktarılmalıdır. Tam eşitlemeye devam etmeden önce, şirket içi AD Bağlayıcısı alanında mevcut bir kullanıcı nesnesi üzerinde **Önizleme** yapın. Seçtiğiniz nesnenin kaynak özniteliği doldurulmuş olmalıdır.
    
    Meta veri deposunda doldurulmuş UserType ile başarılı bir **Önizleme** , eşitleme kurallarını doğru bir şekilde yapılandırdığınıza yönelik iyi bir göstergedir. **Önizlemenin** nasıl yapılacağı hakkında daha fazla bilgi için, bkz. [değişikliği doğrulama](#verify-the-change)bölümü.

4. Şirket **ıçı ad Bağlayıcısı** üzerinde **tam eşitleme** çalıştırın:

   1. Şirket **ıçı ad Bağlayıcısı** ' na sağ tıklayın ve **Çalıştır**' ı seçin.
   2. Açılır iletişim kutusunda **tam eşitleme** ' yi seçin ve ardından **Tamam**' a tıklayın.
   3. İşlemin bitmesini bekleyin.

5. Azure AD 'ye **bekleyen dışarı aktarmaları** doğrulayın:

   1. **Azure AD Bağlayıcısı** ' na sağ tıklayın ve **bağlayıcı alanı ara**' yı seçin.
   2. **Bağlayıcı alanını ara** açılır iletişim kutusunda:

      - **Kapsamı** **bekleyen dışarı aktarma** olarak ayarlayın.
      - Üç onay kutusunu seçin: **Ekle**, **Değiştir** ve **Sil**.
      - Aktarılacak değişikliklerle birlikte nesnelerin listesini almak için **Ara** düğmesine tıklayın. Belirli bir nesne için değişiklikleri incelemek için, nesnesine çift tıklayın.
      - Değişikliklerin beklendiğini doğrulayın.

6. **Azure AD Bağlayıcısı**'Nda **dışarı aktarmayı** Çalıştır:

   1. **Azure AD Bağlayıcısı** ' na sağ tıklayın ve **Çalıştır**' ı seçin.
   2. **Bağlayıcıyı Çalıştır** açılır Iletişim kutusunda **dışarı aktar** ' ı seçin ve ardından **Tamam**' a tıklayın.
   3. Azure AD 'ye dışarı aktarma işleminin bitmesini bekleyin.

> [!NOTE]
> Bu adımlar, Azure AD Bağlayıcısı üzerinde tam eşitleme ve dışarı aktarma adımlarını içermez. Bu adımlar gerekli değildir çünkü öznitelik değerleri şirket içi Active Directory yalnızca Azure AD 'ye akar.

### <a name="step-7-re-enable-the-sync-scheduler"></a>7. Adım: eşitleme zamanlayıcısını yeniden etkinleştirin
Yerleşik eşitleme zamanlayıcısını yeniden etkinleştirin:

1. Bir PowerShell oturumu başlatın.
2. Cmdlet 'ini çalıştırarak zamanlanmış eşitlemeyi yeniden etkinleştirin `Set-ADSyncScheduler -SyncCycleEnabled $true` .


## <a name="next-steps"></a>Sonraki adımlar
* [Bildirim temelli sağlamayı anlamak](concept-azure-ad-connect-sync-declarative-provisioning.md)için yapılandırma modeli hakkında daha fazla bilgi edinin.
* [Bildirim temelli sağlama Ifadelerini anlama](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md)bölümünde ifade dili hakkında daha fazla bilgi edinin.

**Genel Bakış konuları**

* [Azure AD Connect eşitleme: eşitlemeyi anlama ve özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
