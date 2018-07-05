---
title: Azure Data Lake Store içinde depolanan verilerin güvenliğini sağlama | Microsoft Docs
description: Grupları kullanarak Azure Data Lake Store, veri güvenliğini sağlamayı öğrenmek ve erişim denetimi listeleri
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ca35e65f-3986-4f1b-bf93-9af6066bb716
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: nitinme
ms.openlocfilehash: 0ac6b90f2efc525cfb9767843c741f1e3cfc6de7
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450389"
---
# <a name="securing-data-stored-in-azure-data-lake-store"></a>Azure Data Lake Store içinde depolanan verilerin güvenliğini sağlama
Azure Data Lake Store, veri güvenliğini sağlamak için üç adımlık bir yaklaşımdır.  Rol tabanlı erişim denetimi (RBAC) ve tam olarak kullanıcılar ve güvenlik grupları için veri erişimini etkinleştirmek için erişim denetim listeleri (ACL'ler) ayarlamanız gerekir.

1. Azure Active Directory (AAD içinde) güvenlik grupları oluşturarak başlayın. Bu güvenlik grupları, Azure portalında rol tabanlı erişim denetimi (RBAC) uygulamak için kullanılır. Daha fazla bilgi için [Microsoft Azure rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md).
2. AAD güvenlik gruplarının Azure Data Lake Store hesabına atayın. Bu erişim denetimleri için portalı veya API portalı ve yönetim işlemleri Data Lake Store hesabı.
3. AAD güvenlik gruplarının, erişim denetim listelerini (ACL'ler) Data Lake Store dosya sistemi atayın.
4. Ayrıca, Data Lake Store verileri erişen istemciler için bir IP adresi aralığı ayarlayabilirsiniz.

Bu makalede yukarıdaki görevleri gerçekleştirmek için Azure portalını kullanma hakkında yönergeler sağlar. Data Lake Store hesabı ve veri düzeyinde güvenliği nasıl uyguladığını hakkında ayrıntılı bilgi için bkz. [Azure Data Lake Store güvenlik](data-lake-store-security-overview.md). Azure Data Lake Store ACL'leri nasıl uygulandığını ayrıntılı bilgi için bkz. [Data Lake Store, erişim denetimi genel bakış](data-lake-store-access-control.md).

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Data Lake Store hesabı**. Bir oluşturma hakkında yönergeler için bkz: [Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Azure Active Directory'de güvenlik grupları oluşturma
AAD güvenlik gruplarının nasıl oluşturulacağını ve gruba kullanıcı ekleme hakkında yönergeler için bkz: [Azure Active Directory'de güvenlik grupları yönetme](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

> [!NOTE] 
> Azure portalını kullanarak Azure AD'de bir grup için hem kullanıcılar hem de diğer grupları ekleyebilirsiniz. Ancak, bir grup için bir hizmet sorumlusu ekleme için kullanılması [Azure AD PowerShell modülünü](../active-directory/users-groups-roles/groups-settings-v2-cmdlets.md).
> 
> ```powershell
> # Get the desired group and service principal and identify the correct object IDs
> Get-AzureADGroup -SearchString "<group name>"
> Get-AzureADServicePrincipal -SearchString "<SPI name>"
> 
> # Add the service principal to the group
> Add-AzureADGroupMember -ObjectId <Group object ID> -RefObjectId <SPI object ID>
> ```
 
## <a name="assign-users-or-security-groups-to-azure-data-lake-store-accounts"></a>Azure Data Lake Store hesaplarına kullanıcıları veya güvenlik grupları atama
Azure Data Lake Store hesaplarına kullanıcıların veya güvenlik gruplarının atadığınızda, Azure portalı ve Azure Resource Manager API'leri kullanarak hesap yönetim işlemlerini erişimi denetleyebilirsiniz. 

1. Bir Azure Data Lake Store hesabını açın. Sol bölmeden tıklayın **tüm kaynakları**ve ardından tüm kaynaklar dikey penceresinden bir kullanıcı veya güvenlik grubu atamak istediğiniz hesabın adına tıklayın.

2. Data Lake Store hesabı dikey pencerenizde, tıklayın **erişim denetimi (IAM)**. Varsayılan olarak dikey abonelik sahipleri sahibi olarak listeler.
   
    ![Azure Data Lake Store hesabı için güvenlik grubu atayın](./media/data-lake-store-secure-data/adl.select.user.icon.png "Azure Data Lake Store hesabı için Güvenlik Grup Ata")

3. İçinde **erişim denetimi (IAM)** dikey penceresinde tıklayın **Ekle** açmak için **izinleri eklemek** dikey penceresi. İçinde **izinleri eklemek** dikey penceresinde bir **rol** kullanıcı/grup için. Azure Active Directory'de daha önce oluşturduğunuz güvenlik grubunu bulun ve seçin. Kullanıcıları ve grupları aramak için çok fazla varsa, **seçin** grup adıyla filtrelemek için metin kutusu. 
   
    ![Bir rol için kullanıcı ekleme](./media/data-lake-store-secure-data/adl.add.user.1.png "bir rol için kullanıcı ekleme")
   
    **Sahibi** ve **katkıda bulunan** rolü data lake hesabı yönetim işlevleri çeşitli erişim sağlar. Veri gölü ancak yine de hesap yönetim bilgilerini görüntülemek için gereksinim verilerle etkileşim kullanıcılar için bunları ekleyebilirsiniz **okuyucu** rol. Azure Data Lake Store hesabıyla ilgili yönetim işlemlerini bu rollerin kapsamı sınırlıdır.
   
    Veri işlemleri için kullanıcıların ne yaptığını tek tek dosya sistemi izinleri tanımlayın. Bu nedenle, okuyucu rolüne sahip bir kullanıcı hesabı ile ilişkili yönetim ayarlarını yalnızca görüntüleme ancak potansiyel olarak okuyabilir ve dosya sistemi izinleri atanmış göre veri yazma. Data Lake Store dosya sistemi izinleri adresindeki açıklanmıştır [Azure Data Lake Store dosya sistemine ACL olarak atama güvenlik grubu](#filepermissions).

    > [!IMPORTANT]
    > Yalnızca **sahibi** rolünü otomatik olarak bir dosya sistemi erişimini etkinleştirir. **Katkıda bulunan**, **okuyucu**, ve diğer tüm rolleri herhangi bir dosya ve klasörleri için erişim düzeyini etkinleştirmek için sistem ACL'lerini gerektirir.  **Sahibi** rolü, süper kullanıcı dosya ve ACL'ler ile geçersiz kılınamaz klasör izinleri sağlar. RBAC ilkelerinin veri erişimi nasıl eşleştiği hakkında daha fazla bilgi için bkz. [hesap yönetimi için RBAC](data-lake-store-security-overview.md#rbac-for-account-management).

4. Listede bir grubu/kullanıcısı eklemek isteyip istemediğimi **izinleri eklemek** dikey penceresinde davet edebildiğiniz bunları kendi e-posta adresini yazarak **seçin** metin kutusuna ve ardından bunları listeden seçerek.
   
    ![Bir güvenlik grubu eklemek](./media/data-lake-store-secure-data/adl.add.user.2.png "bir güvenlik grubu Ekle")
   
5. **Kaydet**’e tıklayın. Aşağıda gösterildiği gibi eklenen güvenlik grubu görmeniz gerekir.
   
    ![Eklenen güvenlik grubunu](./media/data-lake-store-secure-data/adl.add.user.3.png "güvenlik grubuna eklendi")

6. Kullanıcı/güvenlik grubunuz artık Azure Data Lake Store hesabına erişebilir. Belirli kullanıcılara erişim sağlamak istiyorsanız, güvenlik grubuna ekleyebilirsiniz. Benzer şekilde, bir kullanıcı için erişimi iptal etmek istiyorsanız, bunları güvenlik grubundan kaldırabilirsiniz. Ayrıca, birden çok güvenlik grupları için bir hesap atayabilirsiniz. 

## <a name="filepermissions"></a>Kullanıcıları veya güvenlik grupları, Azure Data Lake Store dosya sistemine ACL olarak atama
Azure Data Lake dosya sistemine kullanıcı/güvenlik grupları atayarak, Azure Data Lake Store içinde depolanan veriler üzerinde erişim denetimini ayarlayın.

1. Data Lake Store hesabı dikey pencerenizde, **Veri Gezgini**'ne tıklayın.
   
    ![Veri Gezgini aracılığıyla verileri görüntüleme](./media/data-lake-store-secure-data/adl.start.data.explorer.png "Veri Gezgini aracılığıyla verileri görüntüleme")
2. İçinde **Veri Gezgini** dikey penceresinde, ACL yapılandırmak ve ardından istediğiniz klasörü tıklatın **erişim**. Bir dosyaya ACL'leri atamak için önce önizleme ve ardından dosyaya tıklatmalısınız **erişim** gelen **dosya önizlemesi** dikey penceresi.
   
    ![Data Lake dosya sistemine ACL ayarlayın](./media/data-lake-store-secure-data/adl.acl.1.png "Data Lake dosya sistemi ACL'ler ayarlayın")
3. **Erişim** dikey sahipleri listeler ve kök atanmış izinleri atanmış. Tıklayın **Ekle** simgesini ek erişim ACL'leri ekleyebilir.
    > [!IMPORTANT]
    > Tek bir dosya erişim izinlerini ayarlama mutlaka bu dosyayı bir kullanıcı/Grup erişim izni vermez. Dosya yoluna atanan kullanıcı/Grup erişilebilir olmalıdır. Daha fazla bilgi ve örnekler için bkz. [yaygın senaryoları ilgili izinleri](data-lake-store-access-control.md#common-scenarios-related-to-permissions).
   
    ![Standart ve özel erişim listesinde](./media/data-lake-store-secure-data/adl.acl.2.png "listesinde standart ve özel erişim")
   
   * **Sahipleri** ve **diğer herkes** UNIX stili erişim sağlamak, belirttiğiniz okuma, yazma, (rwx) yürütmek için üç ayrı kullanıcı sınıfları: sahibi, Grup ve diğerleri.
   * **İzinleri atanmış** belirli adlandırılmış kullanıcılar veya gruplar ötesinde dosyanın sahibi veya grup için izinleri sağlayan POSIX ACL karşılık gelir. 
     
     Daha fazla bilgi için [HDFS ACL'leri](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Data Lake Store ACL'leri nasıl uygulandığı hakkında daha fazla bilgi için bkz: [Data Lake Store erişim denetimi](data-lake-store-access-control.md).
4. Tıklayın **Ekle** açmak için simgeyi **izinleri atama** dikey penceresi. Bu dikey pencerede tıklayın **kullanıcı veya grup seçin**ve ardından **kullanıcı veya grup seçin** dikey penceresinde, Azure Active Directory'de daha önce oluşturduğunuz güvenlik grubunu arayın. Grupları aramak için çok fazla varsa, metin kutusunun üstündeki grup adıyla filtrelemek için kullanın. Ekleyin ve ardından istediğiniz grubu seçin **seçin**.
   
    ![Grup ekleme](./media/data-lake-store-secure-data/adl.acl.3.png "grup ekleme")
5. Tıklayın **izinleri seçin**seçin ACL veya her ikisini de varsayılan izinleri, izinleri yinelemeli olarak uygulanması gereken ve bir erişim ACL'si, izinleri atamak istediğiniz. **Tamam**’a tıklayın.
   
    ![Grup için izinleri atayın](./media/data-lake-store-secure-data/adl.acl.4.png "gruplandırmak için izin atama")
   
    Data Lake Store ve varsayılan/erişim ACL'leri izinler hakkında daha fazla bilgi için bkz. [Data Lake Store erişim denetimi](data-lake-store-access-control.md).
6. ' I tıklattıktan sonra **Tamam** içinde **izinleri seçin** dikey penceresinde, yeni eklenen grubu ve ilişkili izinlerini de artık listelenir **erişim** dikey penceresi.
   
    ![Grup için izinleri atayın](./media/data-lake-store-secure-data/adl.acl.5.png "gruplandırmak için izin atama")
   
   > [!IMPORTANT]
   > Geçerli sürümde kadar 28 girişleri altında olabilir **izinleri atanmış**. 28'den fazla kullanıcı eklemek istiyorsanız, oluşturmanız gerektiğini güvenlik grupları, kullanıcı güvenlik gruplarına ekleme, eklemek için Data Lake Store hesabı için güvenlik gruplarına erişim sağlamak.
   > 
   > 
7. Gruba ekledikten sonra gerekli olursa, erişim izinleri değiştirebilirsiniz. Bu izin için güvenlik grubu atamak veya kaldırmak isteyip istemediğinizi üzerinde göre her izin türü (okuma, yazma, yürütme) onay kutusunu seçin veya temizleyin. Tıklayın **Kaydet** değişiklikleri kaydetmek veya **at** değişiklikleri geri almak.

## <a name="set-ip-address-range-for-data-access"></a>Veri erişimi için IP adresi aralığı ayarlayın
Azure Data Lake Store, daha fazla kilitleme veri deponuz ağ düzeyinde erişim sağlar. Güvenlik duvarını etkinleştirmek, bir IP adresi belirtin veya güvenilen istemcileriniz için bir IP adresi aralığı tanımlayın. Etkinleştirildiğinde, yalnızca IP adresleri tanımlı aralığın içinde olan istemciler Store'a bağlanabilirsiniz.

![Güvenlik Duvarı ayarları ve IP erişim](./media/data-lake-store-secure-data/firewall-ip-access.png "Güvenlik Duvarı ayarları ve IP adresi")

## <a name="remove-security-groups-for-an-azure-data-lake-store-account"></a>Bir Azure Data Lake Store hesabı için güvenlik grupları Kaldır
Azure Data Lake Store hesaplarını güvenlik grupları kaldırdığınızda, Azure Portal ve Azure Resource Manager API'leri kullanarak hesap yönetimi işlemleri için yalnızca değiştiriyorsunuz.  

Veri erişimi değiştirilmez ve yine de erişim ACL'leri tarafından yönetilir.  Bunun özel durumu olan sahipleri rolündeki kullanıcılar/gruplar.  Kullanıcılar/Gruplar sahipleri rolden Süper kullanıcılar artık olmayan ve erişimleri erişim ACL'si ayarlarına geri döner. 

1. Data Lake Store hesabı dikey pencerenizde, tıklayın **erişim denetimi (IAM)**. 
   
    ![Azure Data Lake hesabı için güvenlik grubu atayın](./media/data-lake-store-secure-data/adl.select.user.icon.png "Azure Data Lake hesabı için Güvenlik Grup Ata")
2. İçinde **erişim denetimi (IAM)** dikey penceresinde, kaldırmak istediğiniz güvenlik grupları'nı tıklatın. Tıklayın **Kaldır**.
   
    ![Güvenlik grubu kaldırıldı](./media/data-lake-store-secure-data/adl.remove.group.png "güvenlik grubu kaldırıldı")

## <a name="remove-security-group-acls-from-azure-data-lake-store-file-system"></a>Güvenlik grubu ACL'leri Azure Data Lake Store dosya sistemi konumundan Kaldır
Güvenlik grubu ACL'leri Azure Data Lake Store dosya sisteminden kaldırdığınızda, Data Lake Store verilere erişimi değiştirme.

1. Data Lake Store hesabı dikey pencerenizde, **Veri Gezgini**'ne tıklayın.
   
    ![Data Lake hesabında dizinleri oluşturma](./media/data-lake-store-secure-data/adl.start.data.explorer.png "dizinleri Data Lake hesabı oluşturma")
2. İçinde **Veri Gezgini** dikey penceresinde, ACL kaldırın ve ardından istediğiniz klasörü tıklatın **erişim**. Bir dosya için ACL'leri kaldırmak için ilk önizleme ve ardından dosyaya tıklatmalısınız **erişim** gelen **dosya önizlemesi** dikey penceresi. 
   
    ![Data Lake dosya sistemine ACL ayarlayın](./media/data-lake-store-secure-data/adl.acl.1.png "Data Lake dosya sistemi ACL'ler ayarlayın")
3. İçinde **erişim** dikey penceresinde, kaldırmak istediğiniz güvenlik grubunu seçin. İçinde **erişim ayrıntıları** dikey penceresinde tıklayın **Kaldır**.
   
    ![Grup için izinleri atayın](./media/data-lake-store-secure-data/adl.remove.acl.png "gruplandırmak için izin atama")

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake Store'a Genel Bakış](data-lake-store-overview.md)
* [Data Lake Store için Azure depolama Bloblarından veri kopyalama](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
* [PowerShell kullanarak Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-powershell.md)
* [.NET SDK kullanarak Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-net-sdk.md)
* [Data Lake Store'a ilişkin tanılama günlüklerine erişme](data-lake-store-diagnostic-logs.md)

