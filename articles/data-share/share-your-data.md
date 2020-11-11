---
title: 'Öğretici: kuruluşunuz dışında paylaşma-Azure veri paylaşımınız'
description: Öğretici-Azure veri paylaşımından yararlanarak müşterilerle ve iş ortaklarıyla veri paylaşma
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: tutorial
ms.date: 08/28/2020
ms.openlocfilehash: 232f50c05182799c93a636baa2aec8ed93419be8
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2020
ms.locfileid: "94489480"
---
# <a name="tutorial-share-data-using-azure-data-share"></a>Öğretici: Azure Veri Paylaşımı’nı kullanarak verileri paylaşma  

Bu öğreticide, yeni bir Azure veri paylaşımı ayarlamayı ve verilerinizi Azure kuruluşunuzun dışındaki müşteriler ve iş ortaklarıyla paylaşmaya nasıl başlayacağınızı öğreneceksiniz. 

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Veri Paylaşımı oluşturun.
> * Veri Paylaşımınıza veri kümelerini ekleyin.
> * Veri paylaşımınız için bir anlık görüntü zamanlaması etkinleştirin. 
> * Veri Paylaşımınıza alıcıları ekleyin. 

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği: bir Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Alıcının Azure oturum açma e-posta adresi (e-posta diğer adlarını kullanarak çalışmaz).
* Kaynak Azure veri deposu, veri paylaşma kaynağı oluşturmak için kullanacağınız sunucudan farklı bir Azure aboneliğinde yer alıyorsa, [Microsoft. DataShare kaynak sağlayıcısını](concepts-roles-permissions.md#resource-provider-registration) Azure veri deposunun bulunduğu abonelikte kaydedin. 

### <a name="share-from-a-storage-account"></a>Bir depolama hesabından paylaşma:

* Azure depolama hesabı: henüz yoksa bir [Azure depolama hesabı](../storage/common/storage-account-create.md) oluşturabilirsiniz
* *Microsoft. Storage/storageAccounts/Write* içinde bulunan depolama hesabına yazma izni. Bu izin Katkıda Bulunan rolünde vardır.
* *Microsoft. Authorization/role atamalar/Write* ' de bulunan depolama hesabına rol ataması ekleme izni. Bu izin Sahip rolünde vardır. 


### <a name="share-from-a-sql-based-source"></a>SQL tabanlı bir kaynaktan paylaşma:

* Paylaşmak istediğiniz tablolar ve görünümler içeren bir Azure SQL veritabanı veya Azure SYNAPSE Analytics (eski adıyla SQL veri ambarı).
* *Microsoft. SQL/Servers/veritabanları/Write* 'TA bulunan SQL Server 'da veritabanlarına yazma izni. Bu izin Katkıda Bulunan rolünde vardır.
* Veri ambarına erişmek için veri paylaşımının izni. Bu, aşağıdaki adımlarla yapılabilir: 
    1. Kendinizi SQL Server için Azure Active Directory yöneticisi olarak ayarlayın.
    1. Azure Active Directory kullanarak Azure SQL veritabanı/veri ambarına bağlanın.
    1. Veri paylaşımının kaynak yönetilen kimliğini bir db_datareader olarak eklemek için aşağıdaki betiği yürütmek üzere sorgu Düzenleyicisi 'ni (Önizleme) kullanın. SQL Server kimlik doğrulaması değil Active Directory kullanarak bağlanmanız gerekir. 
    
        ```sql
        create user "<share_acct_name>" from external provider;     
        exec sp_addrolemember db_datareader, "<share_acct_name>"; 
        ```                   
       *<share_acc_name>* veri paylaşma kaynağınızın adı olduğunu unutmayın. Henüz bir veri paylaşma kaynağı oluşturmadıysanız, bu ön koşul daha sonra geri dönebilirsiniz.  

* ' Db_datareader ' erişimine sahip bir Azure SQL veritabanı kullanıcısı, paylaşmak istediğiniz tablolar ve/veya görünümler üzerinde gezinmek ve bunları seçmek için. 

* İstemci IP SQL Server güvenlik duvarı erişimi. Bu, aşağıdaki adımlarla yapılabilir: 
    1. Azure portal 'deki SQL Server 'da *güvenlik duvarları ve sanal ağlar* ' a gidin.
    1. Azure hizmetlerine erişime izin vermek **için Aç düğmesine** tıklayın.
    1. **+ ISTEMCI IP Ekle** ' ye tıklayın ve **Kaydet** ' e tıklayın. İstemci IP adresi değişebilir. Bu işlemin bir sonraki Azure portal SQL verilerini paylaşışınızda tekrarlanması gerekebilir. Ayrıca, bir IP aralığı ekleyebilirsiniz. 

### <a name="share-from-azure-data-explorer"></a>Azure Veri Gezgini'nden paylaşma
* Paylaşmak istediğiniz veritabanlarına sahip bir Azure Veri Gezgini kümesi.
* *Microsoft. kusto/kümeler/Write* Içinde bulunan Azure Veri Gezgini kümesine yazma izni. Bu izin Katkıda Bulunan rolünde vardır.
* *Microsoft. Authorization/role atamalar/Write* ' de bulunan Azure Veri Gezgini kümesine rol ataması ekleme izni. Bu izin Sahip rolünde vardır.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/) oturum açın.

## <a name="create-a-data-share-account"></a>Veri paylaşma hesabı oluşturma

Azure Kaynak grubunda bir Azure veri paylaşma kaynağı oluşturun.

1. Portalın sol üst köşesindeki menü düğmesini seçin ve ardından **kaynak oluştur** (+) seçeneğini belirleyin.

1. *Veri paylaşımında* arama yapın.

1. Veri paylaşma ' yı seçin ve **Oluştur** ' u seçin.

1. Aşağıdaki bilgilerle Azure veri paylaşma kaynağınızın temel ayrıntılarını doldurun. 

     **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Abonelik | Aboneliğiniz | Veri paylaşma hesabınız için kullanmak istediğiniz Azure aboneliğini seçin.|
    | Kaynak grubu | *test-resource-group* | Mevcut bir kaynak grubunu kullanın veya yeni bir kaynak grubu oluşturun. |
    | Konum | *Doğu ABD 2* | Veri paylaşma hesabınız için bir bölge seçin.
    | Ad | *datashareaccount* | Veri paylaşma hesabınız için bir ad belirtin. |
    | | |

1. Veri paylaşma hesabınızı sağlamak için **gözden geçir + oluştur** ' u seçin ve **Oluştur** ' a tıklayın. Yeni bir veri paylaşma hesabının sağlanması genellikle yaklaşık 2 dakika veya daha kısa sürer. 

1. Dağıtım tamamlandığında **Kaynağa Git** ' i seçin.

## <a name="create-a-share"></a>Bir paylaşma oluşturun

1. Veri paylaşımında genel bakış sayfasına gidin.

    ![Verilerinizi paylaşma](./media/share-receive-data.png "Verilerinizi paylaşma") 

1. **Verilerinizi paylaşmayı Başlat** ' ı seçin.

1. **Oluştur** ’u seçin.   

1. Paylaşımınızın ayrıntılarını doldurun. Bir ad, paylaşma türü, içerik paylaşma açıklaması ve kullanım koşulları (isteğe bağlı) belirtin. 

    ![EnterShareDetails](./media/enter-share-details.png "Paylaşma ayrıntılarını girin") 

1. **Devam** ’ı seçin.

1. Paylaşımınıza veri kümeleri eklemek için **veri kümesi Ekle** ' yi seçin. 

    ![Paylaşımınıza veri kümeleri ekleme](./media/datasets.png "Veri kümeleri")

1. Eklemek istediğiniz veri kümesi türünü seçin. Önceki adımda seçtiğiniz paylaşma türüne (anlık görüntü veya yerinde) bağlı olarak farklı bir veri kümesi türleri listesi görürsünüz. Bir Azure SQL veritabanından veya Azure SYNAPSE Analytics 'ten paylaşıyorsanız bazı SQL kimlik bilgileri istenir. Önkoşulların bir parçası olarak oluşturduğunuz kullanıcıyı kullanarak kimlik doğrulaması yapın.

    ![Adddataset 'ler](./media/add-datasets.png "Veri kümesi Ekle")    

1. Paylaşmak istediğiniz nesneye gidin ve ' veri kümesi Ekle ' seçeneğini belirleyin. 

    ![Selectdataset 'ler](./media/select-datasets.png "Veri kümelerini seçin")    

1. Alıcılar sekmesinde, ' + Alıcı Ekle ' seçeneğini belirleyerek veri tüketicinizin e-posta adreslerini girin. 

    ![AddRecipients](./media/add-recipient.png "Alıcı ekleme") 

1. **Devam** ’ı seçin.

1. Anlık görüntü paylaşma türü ' nu seçtiyseniz, veri tüketicisine verilerinizin güncelleştirmelerini sağlamak için anlık görüntü zamanlamasını yapılandırabilirsiniz. 

    ![EnableSnapshots](./media/enable-snapshots.png "Anlık görüntüleri etkinleştir") 

1. Bir başlangıç saati ve yinelenme aralığı seçin. 

1. **Devam** ’ı seçin.

1. Gözden geçir + Oluştur sekmesinde paket Içeriklerinizi, ayarlarınızı, alıcılarını ve eşitleme ayarlarınızı gözden geçirin. **Oluştur** ’u seçin.

Azure veri paylaşımınız artık oluşturulmuştur ve veri paylaşımınızın alıcısı artık davetinizi kabul etmeye hazırdır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak artık gerekli olmadığında, **veri paylaşımının genel bakış** sayfasına gidin ve **Sil** ' i seçerek kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure veri paylaşımının nasıl oluşturulacağını ve alıcıların nasıl davet alınacağını öğrenirsiniz. Bir veri tüketicisinin bir veri paylaşımının kabul edip alabileceği hakkında bilgi edinmek için, verileri kabul etme ve alma öğreticisine geçin.

> [!div class="nextstepaction"]
> [Öğretici: Azure Veri Paylaşımı’nı kullanarak veri kabul etme ve alma](subscribe-to-data-share.md)
