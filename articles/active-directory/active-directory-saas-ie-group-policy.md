---
title: Azure erişim paneli uzantısını dağıtmak için bir GPO kullanarak IE | Microsoft Docs
description: Internet Explorer eklenti için uygulamalarım portalında dağıtmak için Grup İlkesi kullanma
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/09/2018
ms.author: barbkess
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ce27f51dc5e80b4ce2bd83b5f9f6c75853a2ea73
ms.sourcegitcommit: 465ae78cc22eeafb5dfafe4da4b8b2138daf5082
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44325286"
---
# <a name="how-to-deploy-the-access-panel-extension-for-internet-explorer-using-group-policy"></a>Grup İlkesi'ni kullanarak Internet Explorer için erişim paneli uzantısını dağıtma
Bu öğreticide, uzaktan erişim paneli uzantısını Internet Explorer için kullanıcılarınızın makinelerde yüklemek için Grup İlkesi kullanımı gösterilmektedir. Bu uzantıyı kullanarak yapılandırılan uygulamalarında oturum açmak için gereken Internet Explorer kullanıcılar için gerekli olan [parola tabanlı çoklu oturum açma](manage-apps/what-is-single-sign-on.md#password-based-single-sign-on).

Yöneticiler'in bu uzantının dağıtımı otomatik hale getirmek önerilir. Aksi takdirde, kullanıcılar indirin ve uzantı kendilerinin yüklemesi kullanıcı hataya ve yönetici izinleri gerektiren gerekir. Bu öğretici, bir Grup İlkesi'ni kullanarak yazılım dağıtımlarını otomatikleştirme yöntemi içerir. [Grup İlkesi hakkında daha fazla bilgi edinin.](https://technet.microsoft.com/windowsserver/bb310732.aspx)

Erişim paneli uzantısı için de kullanılabilir olan [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) ve [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), hangi hiçbiri yüklemek için yönetici izinleri gerektirir.

## <a name="prerequisites"></a>Önkoşullar
* Ayarlamış olduğunuz [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), ve kullanıcılarınızın makineleri etki alanınıza katılmış.
* Grup İlkesi nesnesi (GPO) düzenlemek için "Ayarları düzenleme" izniniz olmalıdır. Varsayılan olarak, şu güvenlik gruplarının üyeleri bu izne sahip: etki alanı yöneticileri, kuruluş yöneticileri ve Group Policy Creator Owners. [Daha fazla bilgi edinin.](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

## <a name="step-1-create-the-distribution-point"></a>1. adım: dağıtım noktası oluşturma
İlk olarak, uzaktan üzerinde uzantıyı yüklemek istediğiniz makineleri tarafından erişilebilen bir ağ konumu üzerinde Yükleyici paketini yerleştirmeniz gerekir. Bunu yapmak için şu adımları uygulayın:

1. İçin sunucuda yönetici olarak oturum açın
2. İçinde **Sunucu Yöneticisi'ni** penceresinde, Git **dosya ve depolama hizmetleri**.
   
    ![Açık dosyalar ve depolama hizmetleri](./media/active-directory-saas-ie-group-policy/files-services.png)
3. Git **paylaşımları** sekmesi. Ardından **görevleri** > **yeni paylaşım...**
   
    ![Açık dosyalar ve depolama hizmetleri](./media/active-directory-saas-ie-group-policy/shares.png)
4. Tamamlamak **yeni paylaşım Sihirbazı** ve kullanıcılarınızın makinelerden erişilebildiğinden emin olmak için izinleri ayarlayın. [Paylaşımları hakkında daha fazla bilgi edinin.](https://technet.microsoft.com/library/cc753175.aspx)
5. Aşağıdaki Microsoft Windows Installer paketi (.msi dosyası) indirin: [erişim paneli Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)
6. Yükleyici paket paylaşımında istenen bir konuma kopyalayın.
   
    ![.Msi dosyasını paylaşıma kopyalayın.](./media/active-directory-saas-ie-group-policy/copy-package.png)
7. İstemci makineleriniz Yükleyici paketini paylaşımından erişmek mümkün olduğunu doğrulayın. 

## <a name="step-2-create-the-group-policy-object"></a>2. adım: Grup İlkesi nesnesi oluşturma
1. Active Directory etki alanı Hizmetleri (AD DS) yüklemenizi barındıran sunucuda oturum açın.
2. Sunucu Yöneticisi'nde Git **Araçları** > **Grup İlkesi Yönetimi**.
   
    ![Araçlar'a gidin > Grup İlkesi Yönetimi](./media/active-directory-saas-ie-group-policy/tools-gpm.png)
3. Sol bölmesinde **Grup İlkesi Yönetimi** penceresinde, kuruluş birimi (OU) hiyerarşisini görüntülemek ve hangi kapsamda grup ilkesini uygulamak istediğiniz belirleyin. Örneğin, test etmek için birkaç kullanıcılara dağıtmak için küçük bir OU çekme karar verebilir ya da kuruluşunuzun tamamıyla dağıtmak için bir en üst düzey OU seçin.
   
   > [!NOTE]
   > Oluşturma veya, kuruluş birimine (OU) Düzenle, Sunucu Yöneticisi'ne geri geçmek ve gitmek istiyorsanız, **Araçları** > **Active Directory Kullanıcıları ve Bilgisayarları**.
   > 
   > 
4. Bir OU seçtikten sonra sağ tıklatın ve seçin **bu etki alanında GPO oluştur ve buraya bağla...**
   
    ![Yeni bir GPO oluşturma](./media/active-directory-saas-ie-group-policy/create-gpo.png)
5. İçinde **yeni GPO** istemi, yeni Grup İlkesi nesnesi için bir ad yazın.
   
    ![Yeni GPO adı](./media/active-directory-saas-ie-group-policy/name-gpo.png)
6. Grup İlkesi nesnesini, oluşturduğunuz ve seçin sağ **Düzenle**.
   
    ![Yeni GPO düzenleme](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

## <a name="step-3-assign-the-installation-package"></a>3. adım: yükleme paketini atama
1. Temel uzantısını dağıtmak isteyip istemediğinizi belirleyin **Bilgisayar Yapılandırması** veya **Kullanıcı Yapılandırması**. Kullanırken [Bilgisayar Yapılandırması](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), uzantıyı hangi kullanıcıların oturum açın, bilgisayara yüklenir. İle [Kullanıcı Yapılandırması](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), kullanıcılar, bunları bağımsız olarak hangi bilgisayarların oturum açmak için yüklü uzantılıdır.
2. Sol bölmesinde **Grup İlkesi Yönetimi Düzenleyicisi** penceresi, seçtiğiniz yapılandırma türüne aşağıdaki klasör yolları birini gidin:
   
   * `Computer Configuration/Policies/Software Settings/`
   * `User Configuration/Policies/Software Settings/`
3. Sağ **yazılım yüklemesi**, ardından **yeni** > **paket...**
   
    ![Yeni bir yazılım yükleme paketi oluşturun.](./media/active-directory-saas-ie-group-policy/new-package.png)
4. Yükleyici paketi içeren paylaşılan klasöre gidin [1. adım: dağıtım noktası oluştur](#step-1-create-the-distribution-point).msi dosyasını seçin ve tıklayın **açık**.
   
   > [!IMPORTANT]
   > Paylaşım bu aynı sunucuda bulunuyorsa, yerel dosya yolu yerine bir ağ dosya yolu .msi eriştiğiniz doğrulayın.
   > 
   > 
   
    ![Paylaşılan klasörden yükleme paketini seçin.](./media/active-directory-saas-ie-group-policy/select-package.png)
5. İçinde **yazılım dağıtma** anında, select **atanan** dağıtım yönteminize için. Daha sonra, **Tamam**'a tıklayın.
   
    ![Atanan seçin ve ardından Tamam'a tıklayın.](./media/active-directory-saas-ie-group-policy/deployment-method.png)

Uzantı artık, seçtiğiniz kuruluş dağıtılır. [Grup İlkesi Yazılım yükleme hakkında daha fazla bilgi edinin.](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

## <a name="step-4-auto-enable-the-extension-for-internet-explorer"></a>4. adım: Otomatik etkinleştirmek için Internet Explorer uzantısı
Kullanmadan önce yükleyici çalıştırmanın yanı sıra, her uzantısını Internet Explorer için açıkça etkinleştirilmesi gerekir. Grup İlkesi kullanarak erişim panelinde uzantıyı etkinleştirmek için aşağıdaki adımları izleyin:

1. İçinde **Grup İlkesi Yönetimi Düzenleyicisi** penceresinde, yapılandırma türüne seçerseniz, aşağıdaki yollardan birini gidin [3. adım: Ata yükleme paketini](#step-3-assign-the-installation-package):
   
   * `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
2. Sağ **eklenti listesi**seçip **Düzenle**.
    ![Eklenti Listesi düzenleyin.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)
3. İçinde **eklenti listesi** penceresinde **etkin**. Ardından, altında **seçenekleri** bölümünde **göster...** .
   
    ![Etkinleştir'i tıklatın ve ardından göster...](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)
4. İçinde **içeriğini göster** penceresinde aşağıdaki adımları gerçekleştirin:
   
   1. İlk sütun için ( **değer adı** alan), kopyalama ve aşağıdaki sınıf kimliği: `{030E9A3F-7B18-4122-9A60-B87235E4F59E}`
   2. İkinci sütun için ( **değer** alan), aşağıdaki değer: `1`
   3. Tıklayın **Tamam** kapatmak için **içeriğini göster** penceresi.
      
      ![Yukarıda belirtildiği gibi değerleri doldurun.](./media/active-directory-saas-ie-group-policy/show-contents.png)
5. Tıklayın **Tamam** yaptığınız değişiklikleri uygulamak ve kapatmak için **eklenti listesi** penceresi.

Seçili OU'ya makineler için şimdi Uzantının etkinleştirilmesi gerekir. [Etkinleştirmek veya Internet Explorer eklentileri devre dışı bırakmak için Grup İlkesi'ni kullanma hakkında daha fazla bilgi edinin.](https://technet.microsoft.com/library/dn454941.aspx)

## <a name="step-5-optional-disable-remember-password-prompt"></a>5. adım (isteğe bağlı): "Parolayı anımsa" istemi devre dışı bırak
Kullanıcılar Web sitelerine erişim paneli uzantısını kullanarak oturum açtığınızda, Internet Explorer "parolanızı depolamak istiyorsunuz?" isteyen aşağıdaki istemi gösterebilir

![Parola istemi](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

Ardından kullanıcılarınız bu istemi görüntülemesini engellemek istiyorsanız, otomatik tamamlama parolaları anımsamaya çalışmaktan gelen önlemek için aşağıdaki adımları izleyin:

1. İçinde **Grup İlkesi Yönetimi Düzenleyicisi** penceresi, aşağıda listelenen yoluna gidin. Bu yapılandırma ayarı yalnızca altında kullanılabilir **Kullanıcı Yapılandırması**.
   
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`
2. Adlı ayarı bulmak **kullanıcı adları ve parolalar formlarında Otomatik Tamamlama özelliği açmak**.
   
   > [!NOTE]
   > Active Directory önceki sürümlerinde bu ayar adıyla listesinde **parolaları kaydetmek için otomatik tamamlama izin verme**. Bu öğreticide açıklanan ayarı için bu ayarı yapılandırmanın farklıdır.
   > 
   > 
   
    ![Kullanıcı Ayarları altında şunun unutmayın.](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)
3. Yukarıdaki ayar sağ tıklatın ve seçin **Düzenle**.
4. Başlıklı penceresinde **kullanıcı adları ve parolalar formlarında Otomatik Tamamlama özelliği açmak**seçin **devre dışı bırakılmış**.
   
    ![Devre seçin](./media/active-directory-saas-ie-group-policy/disable-passwords.png)
5. Tıklayın **Tamam** bu değişiklikleri uygulamak ve penceresini kapatın.

Kullanıcılar artık kendi kimlik bilgilerini veya kullanım otomatik tamamlama daha önce depolanan kimlik bilgilerine depolamak mümkün olacaktır. Ancak, bu ilke otomatik tamamlama, arama alanlar gibi form alanlarını diğer türleri için kullanmaya devam etmek kullanıcılara izin vermez.

> [!WARNING]
> Bazı kimlik bilgileri, bu ilke depolamak kullanıcıları seçtikten sonra bu ilke etkinleştirilirse *değil* zaten depolanmış kimlik bilgilerini temizleyin.
> 
> 

## <a name="step-6-testing-the-deployment"></a>6. adım: test etme ve dağıtım
Uzantı dağıtımı başarılı olup olmadığını doğrulamak için aşağıdaki adımları izleyin:

1. Kullanarak dağıttıysanız **Bilgisayar Yapılandırması**, seçtiğiniz OU'ya ait bir istemci makinesi oturum [2. adım: Grup İlkesi nesnesi oluşturmak](#step-2-create-the-group-policy-object). Kullanarak dağıttıysanız **Kullanıcı Yapılandırması**, ilgili OU'ya ait bir kullanıcı olarak oturum açtığınızdan emin olun.
2. Bu bileşenler tamamen Grup İlkesi için değişiklikleri için güncelleştirme birkaç oturum sürebilir bu makineyle. Güncelleştirmeyi zorlamak için açık bir **komut istemi** penceresi ve şu komutu çalıştırın: `gpupdate /force`
3. Yüklemenin gerçekleşmesi makineyi yeniden başlatmanız gerekir. Önyükleme sırasında uzantı normal yükler önemli ölçüde daha uzun sürebilir.
4. Yeniden başlattıktan sonra açın **Internet Explorer**. Pencerenin sağ üst köşesindeki üzerinde tıklayın **Araçları** (dişli simgesi) seçip **eklentileri yönetme**.
   
    ![Araçlar'a gidin > eklentileri yönetme](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)
5. İçinde **Eklentileri Yönet** penceresinde doğrulayın **erişim paneli uzantısını** yüklü olduğundan ve kendi **durumu** ayarlanmış **etkin**.
   
    ![Erişim paneli uzantısı yüklü ve etkin olduğunu doğrulayın.](./media/active-directory-saas-ie-group-policy/verify-install.png)

## <a name="related-articles"></a>İlgili makaleler
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
* [Uygulama erişimi ve Azure Active Directory ile çoklu oturum açma](manage-apps/what-is-single-sign-on.md)
* [Erişim paneli uzantısını Internet Explorer için sorun giderme](active-directory-saas-ie-troubleshooting.md)

