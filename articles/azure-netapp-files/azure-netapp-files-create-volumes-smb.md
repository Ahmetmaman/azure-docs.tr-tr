---
title: Azure NetApp Files için bir SMB birimi oluşturun | Microsoft Docs
description: Bu makalede, Azure NetApp Files içinde nasıl SMBv3 birimi oluşturacağınız gösterilmektedir. Active Directory bağlantıları ve etki alanı Hizmetleri için gereksinimler hakkında bilgi edinin.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 09/24/2020
ms.author: b-juche
ms.openlocfilehash: e2c487b62813bc4480786daa08666fe6471bd18d
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91325717"
---
# <a name="create-an-smb-volume-for-azure-netapp-files"></a>Azure NetApp Files için SMB birimi oluşturma

Azure NetApp Files, NFS (NFSv3 ve NFSv 4.1), SMBv3 veya Dual Protocol (NFSv3 ve SMB) kullanarak birim oluşturmayı destekler. Birimin kapasite kullanımı, havuzunun sağlanan kapasitesinden sayılır. Bu makalede, SMBv3 birimi oluşturma konusu gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce 
Zaten bir kapasite havuzu ayarlamış olmalısınız.   
[Kapasite havuzu ayarlama](azure-netapp-files-set-up-capacity-pool.md)   
Azure NetApp Files için bir alt ağ atanmış olmalıdır.  
[Azure NetApp Files için bir alt ağı temsilci olarak belirleme](azure-netapp-files-delegate-subnet.md)

## <a name="requirements-for-active-directory-connections"></a>Active Directory bağlantıları için gereksinimler

 SMB birimi oluşturmadan önce Active Directory bağlantı oluşturmanız gerekir. Active Directory bağlantıları için gereksinimler şunlardır: 

* Kullandığınız yönetici hesabının, belirttiğiniz kuruluş birimi (OU) yolunda makine hesapları oluşturma yeteneği olmalıdır.  

* Uygun bağlantı noktaları geçerli Windows Active Directory (AD) sunucusunda açık olmalıdır.  
    Gerekli bağlantı noktaları aşağıdaki gibidir: 

    |     Hizmet           |     Bağlantı noktası     |     Protokol     |
    |-----------------------|--------------|------------------|
    |    AD Web Hizmetleri    |    9389      |    TCP           |
    |    DNS                |    53        |    TCP           |
    |    DNS                |    53        |    UDP           |
    |    Icmpv4             |    Yok       |    Yankı yanıtı    |
    |    Kerberos           |    464       |    TCP           |
    |    Kerberos           |    464       |    UDP           |
    |    Kerberos           |    88        |    TCP           |
    |    Kerberos           |    88        |    UDP           |
    |    LDAP               |    389       |    TCP           |
    |    LDAP               |    389       |    UDP           |
    |    LDAP               |    3268      |    TCP           |
    |    NetBIOS adı       |    138       |    UDP           |
    |    SAM/LSA            |    445       |    TCP           |
    |    SAM/LSA            |    445       |    UDP           |
    |    W32Time            |    123       |    UDP           |

* Hedeflenen Active Directory Domain Services yönelik site topolojisi, özel olarak Azure NetApp Files dağıtıldığı Azure VNet 'te yönergelere uymalıdır.  

    Azure NetApp Files dağıtıldığı sanal ağın adres alanı yeni veya var olan bir Active Directory sitesine eklenmelidir (Azure NetApp Files tarafından erişilebilen bir etki alanı denetleyicisi). 

* Belirtilen DNS sunucularına, Azure NetApp Files için [temsilci alt ağından](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-delegate-subnet) ulaşılabilir olmalıdır.  

    Desteklenen ağ topolojileri için [Azure NetApp Files ağ planlama yönergelerine](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-network-topologies) bakın.

    Ağ güvenlik grupları (NSG 'ler) ve güvenlik duvarları, Active Directory ve DNS trafiği isteklerine izin vermek için uygun şekilde yapılandırılmış kurallara sahip olmalıdır. 

* Azure NetApp Files atanmış alt ağ, tüm yerel ve uzak etki alanı denetleyicileri dahil olmak üzere etki alanındaki tüm Active Directory Domain Services (ekleme) etki alanı denetleyicilerine erişebilmelidir. Aksi takdirde, hizmet kesintisi meydana gelebilir.  

    Azure NetApp Files atanmış alt ağ tarafından erişilemeyen etki alanı denetleyicileriniz varsa, Active Directory bağlantısını oluştururken bir Active Directory sitesi belirtebilirsiniz.  Azure NetApp Files, yalnızca Azure NetApp Files atanmış alt ağ adres alanının olduğu sitedeki etki alanı denetleyicileriyle iletişim kurması gerekir.

    Bkz. AD siteleri ve hizmetleriyle ilgili [site topolojisini tasarlama](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/designing-the-site-topology) . 
    
* Bir SMB birimi için, [birleşimi Active Directory](#create-an-active-directory-connection) penceresindeki **AES şifreleme** kutusunu işaretleyerek, AES şifrelemesini etkinleştirebilirsiniz. Azure NetApp Files DES, Kerberos AES 128 ve Kerberos AES 256 şifreleme türlerini (en az güvenli olan en az güvenli) destekler. AES şifrelemesini etkinleştirirseniz Active Directory birleştirmek için kullanılan Kullanıcı kimlik bilgileri, Active Directory için etkinleştirilen yetenekler ile eşleşen en yüksek karşılık gelen hesap seçeneğine sahip olmalıdır.    

    Örneğin, Active Directory yalnızca AES-128 özelliğine sahipse, Kullanıcı kimlik bilgileri için AES-128 hesabı seçeneğini etkinleştirmeniz gerekir. Active Directory AES-256 özelliğine sahipse, AES-256 hesabı seçeneğini etkinleştirmeniz gerekir (Bu da AES-128 ' i destekler). Active Directory herhangi bir Kerberos şifreleme özelliği yoksa, Azure NetApp Files varsayılan olarak DES kullanır.  

    Active Directory Kullanıcıları ve Bilgisayarları Microsoft Yönetim Konsolu (MMC) ' nin özelliklerinde Hesap seçeneklerini etkinleştirebilirsiniz:   

    ![Active Directory Kullanıcıları ve bilgisayarları MMC](../media/azure-netapp-files/ad-users-computers-mmc.png)

* Azure NetApp Files, Azure NetApp Files hizmeti ve hedeflenen [Active Directory etki alanı denetleyicileri](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)arasında LDAP trafiğinin güvenli aktarımını sağlayan [LDAP imzalamayı](https://docs.microsoft.com/troubleshoot/windows-server/identity/enable-ldap-signing-in-windows-server)destekler. LDAP imzalama için Microsoft Danışmanlık [ADV190023](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/ADV190023) ' nin kılavuzunu takip ediyorsanız, [Birleştir Active Directory](#create-an-active-directory-connection) penceresindeki **LDAP imzalama** kutusunu işaretleyerek Azure NetApp Files ' de LDAP imzalama özelliğini etkinleştirmeniz gerekir. 

    [LDAP kanalı bağlama](https://support.microsoft.com/help/4034879/how-to-add-the-ldapenforcechannelbinding-registry-entry) yapılandırmasının Azure NetApp Files hizmeti üzerinde hiçbir etkisi yoktur. 

Ek AD bilgileri hakkında Azure NetApp Files [SMB SSS](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-faqs#smb-faqs) bölümüne bakın. 

## <a name="decide-which-domain-services-to-use"></a>Hangi etki alanı hizmetlerinin kullanılacağını belirleyin 

Azure NetApp Files, AD bağlantıları için hem [Active Directory Domain Services](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/understanding-active-directory-site-topology) (eklemeleri) hem Azure Active Directory Domain Services (aeklemeleri) destekler.  Bir AD bağlantısı oluşturmadan önce, ekleme veya AEKLEMESINE göre ' yi kullanıp kullanmayacağınızı belirlemeniz gerekir.  

Daha fazla bilgi için bkz. [otomatik olarak yönetilen Active Directory Domain Services, Azure Active Directory ve yönetilen Azure Active Directory Domain Services karşılaştırma](https://docs.microsoft.com/azure/active-directory-domain-services/compare-identity-solutions). 

### <a name="active-directory-domain-services"></a>Active Directory Domain Services

Azure NetApp Files için tercih edilen [Active Directory Siteleri ve Hizmetleri](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/understanding-active-directory-site-topology) kapsamını kullanabilirsiniz. Bu seçenek, [Azure NetApp Files tarafından erişilebilen](azure-netapp-files-network-topologies.md)etki alanı denetleyicilerini ACTIVE DIRECTORY DOMAIN SERVICES (ekler) okuma ve yazma işlemleri sağlar. Ayrıca, hizmetin belirtilen Active Directory Siteleri ve Hizmetleri sitesinde olmayan etki alanı denetleyicileriyle iletişim kurmasını da engeller. 

EKLEME kullandığınızda sitenizin adını bulmak için, kuruluşunuzda Active Directory Domain Services sorumlu yönetim grubuyla iletişim sağlayabilirsiniz. Aşağıdaki örnekte, site adının görüntülendiği Active Directory Siteleri ve Hizmetleri eklentisi gösterilmektedir: 

![Active Directory Siteleri ve Hizmetleri](../media/azure-netapp-files/azure-netapp-files-active-directory-sites-services.png)

Azure NetApp Files için bir AD bağlantısı yapılandırdığınızda, **ad site adı** alanı için kapsam içinde site adını belirtirsiniz.

### <a name="azure-active-directory-domain-services"></a>Azure Active Directory Domain Services 

Azure Active Directory Domain Services (AEKLEYEN) yapılandırma ve kılavuz için bkz. [Azure AD Domain Services belgeleri](https://docs.microsoft.com/azure/active-directory-domain-services/).

Azure NetApp Files için ek AEKLEYEN konular geçerlidir: 

* AEKLEMELERI 'nin dağıtıldığı VNet veya alt ağın Azure NetApp Files dağıtımıyla aynı Azure bölgesinde olduğundan emin olun.
* Azure NetApp Files dağıtıldığı bölgede başka bir VNet kullanırsanız, iki sanal ağ arasında bir eşleme oluşturmanız gerekir.
* Azure NetApp Files `user` ve `resource forest` türlerini destekler.
* Eşitleme türü için, `All` veya seçebilirsiniz `Scoped` .   
    Seçeneğini belirlerseniz `Scoped` , SMB paylaşımlarına erişmek için doğru Azure AD grubunun seçildiğinden emin olun.  Emin değilseniz, `All` eşitleme türünü kullanabilirsiniz.
* Enterprise veya Premium SKU kullanımı gereklidir. Standart SKU desteklenmiyor.

Bir Active Directory bağlantısı oluşturduğunuzda, AEKLEMELERI için aşağıdaki özellikleri aklınızda bulabilirsiniz:

* **BIRINCIL DNS**, **Ikincil DNS**ve **ad DNS etki alanı ADıNA** ilişkin bilgileri, aeklemeleri menüsünde bulabilirsiniz.  
DNS sunucuları için, Active Directory bağlantısını yapılandırmak için iki IP adresi kullanılacaktır. 
* **Kuruluş birimi yolu** `OU=AADDC Computers` .  
Bu ayar, **NetApp hesabı**altındaki **Active Directory bağlantılarında** yapılandırılır:

  ![Kuruluş birimi yolu](../media/azure-netapp-files/azure-netapp-files-org-unit-path.png)

* **Kullanıcı adı** kimlik bilgileri, Azure AD grubunun **Azure AD DC yöneticileri**üyesi olan herhangi bir kullanıcı olabilir.


## <a name="create-an-active-directory-connection"></a>Active Directory bağlantısı oluşturma

1. NetApp hesabınızdan **Active Directory bağlantılar**' a ve ardından **Birleştir**' e tıklayın.  

    ![Active Directory bağlantıları](../media/azure-netapp-files/azure-netapp-files-active-directory-connections.png)

2. Active Directory Birleştir penceresinde, kullanmak istediğiniz etki alanı hizmetlerine bağlı olarak aşağıdaki bilgileri sağlayın:  

    Kullandığınız etki alanı hizmetlerine özgü bilgiler için bkz. [hangi etki alanı hizmetlerinin kullanılacağını belirleyin](#decide-which-domain-services-to-use). 

    * **Birincil DNS**  
        Bu, Active Directory etki alanına ekleme ve SMB kimlik doğrulama işlemleri için gereken DNS 'dir. 
    * **İkincil DNS**   
        Bu, gereksiz ad hizmetleri sağlamak için ikincil DNS sunucusudur. 
    * **AD DNS etki alanı adı**  
        Bu, birleştirmek istediğiniz Active Directory Domain Services etki alanı adıdır.
    * **AD site adı**  
        Bu, etki alanı denetleyicisi bulmanın sınırlandırılacak site adıdır.
    * **SMB sunucusu (bilgisayar hesabı) ön eki**  
        Bu, Azure NetApp Files yeni hesapların oluşturulması için kullanacağı Active Directory makine hesabının adlandırma ön ekidir.

        Örneğin, kuruluşunuzun dosya sunucuları için kullandığı adlandırma standardı, NAS-01, NAS-02..., NAS-045 ise, ön ek için "NAS" girersiniz. 

        Hizmet, gerektiğinde Active Directory daha fazla makine hesabı oluşturur.

        > [!IMPORTANT] 
        > Active Directory bağlantısını oluşturduktan sonra SMB sunucusu ön ekinin yeniden adlandırılması kesintiye uğramasıdır. SMB sunucu önekini yeniden adlandırdıktan sonra mevcut SMB paylaşımlarını yeniden bağlamanız gerekecektir.

    * **Kuruluş birimi yolu**  
        Bu, SMB sunucu makinesi hesaplarının oluşturulacağı kuruluş birimi (OU) için LDAP yoludur. Diğer bir deyişle, OU = ikinci düzey, OU = ilk düzey. 

        Azure Active Directory Domain Services ile Azure NetApp Files kullanıyorsanız, kuruluş birimi yolu, `OU=AADDC Computers` NetApp hesabınız için Active Directory yapılandırdığınızda olur.

    ![Active Directory Birleştir](../media/azure-netapp-files/azure-netapp-files-join-active-directory.png)

    * **AES şifrelemesi**   
        SMB birimi için AES şifrelemesini etkinleştirmek üzere bu onay kutusunu seçin. Gereksinimler için [Active Directory bağlantılara yönelik gereksinimlere](#requirements-for-active-directory-connections) bakın. 

        ![Active Directory AES şifrelemesi](../media/azure-netapp-files/active-directory-aes-encryption.png)

        **AES şifreleme** özelliği şu anda önizlemededir. Bu özelliği ilk kez kullanıyorsanız, özelliği kullanmadan önce kaydedin: 

        ```azurepowershell-interactive
        Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFAesEncryption
        ```

        Özellik kaydının durumunu denetleyin: 

        > [!NOTE]
        > **Registrationstate** , ' a `Registering` değiştirilmeden önce 60 dakikaya kadar bir durumda olabilir `Registered` . Devam etmeden önce durum **kaydoluncaya** kadar bekleyin.

        ```azurepowershell-interactive
        Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFAesEncryption
        ```
        
        Ayrıca, [Azure CLI komutlarını](https://docs.microsoft.com/cli/azure/feature?view=azure-cli-latest&preserve-view=true) kullanarak `az feature register` `az feature show` özelliği kaydedebilir ve kayıt durumunu görüntüleyebilirsiniz. 

    * **LDAP Imzalama**   
        LDAP imzalamayı etkinleştirmek için bu onay kutusunu seçin. Bu işlev, Azure NetApp Files hizmeti ve Kullanıcı tarafından belirtilen [Active Directory Domain Services etki alanı denetleyicileri](https://docs.microsoft.com/windows/win32/ad/active-directory-domain-services)arasında güvenli LDAP aramaları sunar. Daha fazla bilgi için bkz. [ADV190023 | LDAP kanalı bağlamayı ve LDAP IMZALAMAYı etkinleştirmek için Microsoft Kılavuzu](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/ADV190023).  

        ![LDAP imzalamayı Active Directory](../media/azure-netapp-files/active-directory-ldap-signing.png) 

        **LDAP imzalama** özelliği şu anda önizlemededir. Bu özelliği ilk kez kullanıyorsanız, özelliği kullanmadan önce kaydedin: 

        ```azurepowershell-interactive
        Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFLdapSigning
        ```

        Özellik kaydının durumunu denetleyin: 

        > [!NOTE]
        > **Registrationstate** , ' a `Registering` değiştirilmeden önce 60 dakikaya kadar bir durumda olabilir `Registered` . Devam etmeden önce durum **kaydoluncaya** kadar bekleyin.

        ```azurepowershell-interactive
        Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFLdapSigning
        ```
        
        Ayrıca, [Azure CLI komutlarını](https://docs.microsoft.com/cli/azure/feature?view=azure-cli-latest&preserve-view=true) kullanarak `az feature register` `az feature show` özelliği kaydedebilir ve kayıt durumunu görüntüleyebilirsiniz. 

     * **Yedekleme ilkesi kullanıcıları**  
        Azure NetApp Files ile kullanım için oluşturulan bilgisayar hesabına yükseltilmiş ayrıcalıklar gerektiren ek hesaplar ekleyebilirsiniz. Belirtilen hesapların dosya veya klasör düzeyinde NTFS izinlerini değiştirmesine izin verilir. Örneğin, Azure NetApp Files bir SMB dosya paylaşımında veri geçirmek için kullanılan ayrıcalıklı olmayan bir hizmet hesabı belirtebilirsiniz.  

        ![Active Directory yedekleme ilkesi kullanıcıları](../media/azure-netapp-files/active-directory-backup-policy-users.png)

        **Yedekleme ilkesi kullanıcıları** özelliği şu anda önizlemededir. Bu özelliği ilk kez kullanıyorsanız, özelliği kullanmadan önce kaydedin: 

        ```azurepowershell-interactive
        Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFBackupOperator
        ```

        Özellik kaydının durumunu denetleyin: 

        > [!NOTE]
        > **Registrationstate** , ' a `Registering` değiştirilmeden önce 60 dakikaya kadar bir durumda olabilir `Registered` . Devam etmeden önce durum **kaydoluncaya** kadar bekleyin.

        ```azurepowershell-interactive
        Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFBackupOperator
        ```
        
        Ayrıca, [Azure CLI komutlarını](https://docs.microsoft.com/cli/azure/feature?view=azure-cli-latest&preserve-view=true) kullanarak `az feature register` `az feature show` özelliği kaydedebilir ve kayıt durumunu görüntüleyebilirsiniz. 

    * **Kullanıcı adınız** ve **Parolanız** dahil kimlik bilgileri

        ![Active Directory kimlik bilgileri](../media/azure-netapp-files/active-directory-credentials.png)

3. **Katıl**’a tıklayın.  

    Oluşturduğunuz Active Directory bağlantı görüntülenir.

    ![Active Directory bağlantıları oluşturuldu](../media/azure-netapp-files/azure-netapp-files-active-directory-connections-created.png)

## <a name="add-an-smb-volume"></a>SMB birimi ekleme

1. Kapasite havuzları dikey penceresinden **birimler** dikey penceresine tıklayın. 

    ![Birimlere git](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png)

2. Birim oluşturmak için **+ Birim ekle**'ye tıklayın.  
    Birim oluştur penceresi görüntülenir.

3. Birim Oluştur penceresinde **Oluştur** ' a tıklayın ve temel bilgiler sekmesinde aşağıdaki alanlar için bilgi sağlayın:   
    * **Birim adı**      
        Oluşturmakta olduğunuz birim için ad belirtin.   

        Birim adı her bir kapasite havuzu içinde benzersiz olmalıdır. En az üç karakter uzunluğunda olmalıdır. Herhangi bir alfasayısal karakter kullanabilirsiniz.   

        `default` `bin` Birim adı olarak veya kullanamazsınız.

    * **Kapasite havuzu**  
        Birimin oluşturulmasını istediğiniz kapasite havuzunu belirtin.

    * **Kota**  
        Birime ayrılmış mantıksal depolama miktarını belirtin.  

        **Kullanılabilir kota** alanı, yeni birimi oluştururken kullanabildiğiniz, seçilen kapasite havuzundaki kullanılmamış alan miktarını gösterir. Yeni birimin boyutu kullanılabilir kotayı aşamaz.  

    * **Aktarım hızı (MIB/S)**   
        Birim el ile QoS kapasite havuzunda oluşturulduysa, birim için istediğiniz aktarım hızını belirtin.   

        Birim bir otomatik QoS kapasite havuzunda oluşturulduysa, bu alanda görünen değer (Kota x hizmet düzeyi aktarım hızı) olur.   

    * **Sanal ağ**  
        Birime erişmek istediğiniz Azure sanal ağını (VNet) belirtin.  

        Belirttiğiniz VNet Azure NetApp Files için bir alt ağa sahip olmalıdır. Azure NetApp Files hizmetine yalnızca aynı VNet 'ten veya VNet eşlemesi ile aynı bölgedeki bir VNet 'ten erişilebilir. Ayrıca, hızlı rota aracılığıyla şirket içi ağınızdan birime da erişebilirsiniz.   

    * **Alt ağ**  
        Birim için kullanmak istediğiniz alt ağı belirtin.  
        Belirttiğiniz alt ağ Azure NetApp Files için temsilci atanmış olmalıdır. 
        
        Bir alt ağ temsilcisi yoksa, birim oluştur sayfasında **Yeni oluştur** ' a tıklayabilirsiniz. Sonra alt ağ oluştur sayfasında alt ağ bilgilerini belirtin ve alt ağın Azure NetApp Files için temsilci olarak **Microsoft. NetApp/birimler** ' i seçin. Her VNet 'te Azure NetApp Files için yalnızca bir alt ağ atanabilir.   
 
        ![Birim oluşturun](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Alt ağ oluşturma](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

    * Birime var olan bir anlık görüntü ilkesi uygulamak istiyorsanız, genişletmek için **Gelişmiş bölümü göster** ' e tıklayın, anlık görüntü yolunu gizlemek isteyip istemediğinizi belirtin ve açılır menüden bir anlık görüntü ilkesi seçin. 

        Anlık görüntü ilkesi oluşturma hakkında daha fazla bilgi için bkz. [anlık görüntü Ilkelerini yönetme](azure-netapp-files-manage-snapshots.md#manage-snapshot-policies).

        ![Gelişmiş seçimi göster](../media/azure-netapp-files/volume-create-advanced-selection.png)

4. **Protokol** ' e tıklayın ve aşağıdaki bilgileri doldurun:  
    * Birimin protokol türü olarak **SMB** ' yi seçin. 
    * Açılır listeden **Active Directory** bağlantınızı seçin.
    * **Paylaşım adı**' nda paylaşılan birimin adını belirtin.

    ![SMB protokolünü belirtin](../media/azure-netapp-files/azure-netapp-files-protocol-smb.png)

5. Birim ayrıntılarını gözden geçirmek için **gözden geçir + oluştur** ' a tıklayın.  Ardından, SMB birimini oluşturmak için **Oluştur** ' a tıklayın.

    Oluşturduğunuz birim birimler sayfasında görünür. 
 
    Birim, kapasite havuzundan aboneliği, kaynak grubunu ve konum özniteliklerini devralır. Birimin dağıtım durumunu izlemek için Bildirimler sekmesini kullanabilirsiniz.

## <a name="control-access-to-an-smb-volume"></a>SMB birimine erişimi denetleme  

SMB birimine erişim izinler aracılığıyla yönetilir.  

### <a name="share-permissions"></a>İzinleri paylaşma  

Varsayılan olarak, yeni bir birimde **Everyone/Full denetim** Share izinleri vardır. Domain Admins grubunun üyeleri, Azure NetApp Files birimi için kullanılan bilgisayar hesabında Bilgisayar Yönetimi ' ni kullanarak paylaşma izinlerini değiştirebilir.

![SMB bağlama yolu ](../media/azure-netapp-files/smb-mount-path.png) 
 ![ kümesi paylaşma izinleri](../media/azure-netapp-files/set-share-permissions.png) 

### <a name="ntfs-file-and-folder-permissions"></a>NTFS dosya ve klasör izinleri  

Windows SMB istemcisinde nesnenin özelliklerinin **güvenlik** sekmesini kullanarak bir dosya veya klasör için izinleri ayarlayabilirsiniz.
 
![Dosya ve klasör izinlerini ayarlama](../media/azure-netapp-files/set-file-folder-permissions.png) 

## <a name="next-steps"></a>Sonraki adımlar  

* [Windows veya Linux sanal makineleri için birimi bağlama veya ayırma](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Azure NetApp Files için kaynak sınırları](azure-netapp-files-resource-limits.md)
* [SMB hakkında SSS](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-faqs#smb-faqs)
* [Azure hizmetleri için sanal ağ tümleştirmesi hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)
* [Azure CLı kullanarak yeni bir Active Directory ormanı yüklemesi](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/virtual-dc/adds-on-azure-vm)
