---
title: Uygulama yedekleme
description: Azure App Service ' de uygulamalarınızın yedeklerini oluşturmayı öğrenin. El ile veya zamanlanmış yedeklemeler çalıştırın. Ekli veritabanını ekleyerek yedeklemeleri özelleştirin.
ms.assetid: 6223b6bd-84ec-48df-943f-461d84605694
ms.topic: article
ms.date: 10/16/2019
ms.custom: seodec18
ms.openlocfilehash: 933ac96d0cf98e0068575e5a70b0f42a157eb611
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2020
ms.locfileid: "91827462"
---
# <a name="back-up-your-app-in-azure"></a>Uygulamanızı Azure’a yedekleme
[Azure App Service](overview.md) yedekleme ve geri yükleme özelliği, uygulama yedeklemelerini el ile veya bir zamanlamaya göre kolayca oluşturmanızı sağlar. Yedeklemeleri sınırsız bir zaman miktarına kadar tutulacak şekilde yapılandırabilirsiniz. Mevcut uygulamanın üzerine yazarak veya başka bir uygulamaya geri yükleyerek uygulamayı önceki bir anlık görüntüye geri yükleyebilirsiniz.

Bir uygulamayı yedekten geri yükleme hakkında bilgi için bkz. [Azure 'da bir uygulamayı geri yükleme](web-sites-restore.md).

<a name="whatsbackedup"></a>

## <a name="what-gets-backed-up"></a>Yedeklenecek
App Service, uygulamanızı kullanacak şekilde yapılandırdığınız bir Azure depolama hesabına ve kapsayıcısına aşağıdaki bilgileri yedekleyebilir. 

* Uygulama yapılandırması
* Dosya içeriği
* Uygulamanıza bağlı veritabanı

Aşağıdaki veritabanı çözümleri yedekleme özelliği ile desteklenir: 

- [SQL Veritabanı](https://azure.microsoft.com/services/sql-database/)
- [MySQL için Azure Veritabanı](https://azure.microsoft.com/services/mysql)
- [PostgreSQL için Azure Veritabanı](https://azure.microsoft.com/services/postgresql)
- [uygulama içi MySQL](https://azure.microsoft.com/blog/mysql-in-app-preview-app-service/)
 

> [!NOTE]
> Her yedekleme, artımlı güncelleştirme değil, uygulamanızın tamamen çevrimdışı bir kopyasıdır.
>

<a name="requirements"></a>

## <a name="requirements-and-restrictions"></a>Gereksinimler ve kısıtlamalar
* Yedekleme ve geri yükleme özelliği, App Service planının **Standart**, **Premium** veya **yalıtılmış** katmanda olmasını gerektirir. App Service planınızı daha yüksek bir katman kullanmak üzere ölçeklendirme hakkında daha fazla bilgi için bkz. [Azure 'da bir uygulamayı ölçeklendirme](manage-scale-up.md). **Premium** ve **yalıtılmış** katmanlar, **Standart** katmandan daha fazla sayıda günlük arka UPS sağlar.
* Yedeklemek istediğiniz uygulamayla aynı abonelikte bir Azure depolama hesabına ve kapsayıcısına ihtiyacınız vardır. Azure depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../storage/common/storage-account-overview.md).
* Yedeklemeler 10 GB 'a kadar uygulama ve veritabanı içeriği olabilir. Yedekleme boyutu bu sınırı aşarsa bir hata alırsınız.
* MySQL için Azure veritabanı 'nın yedekleri etkinleştirilmiş olması desteklenmiyor. Bir yedekleme yapılandırılmışsa, başarısız yedeklemeler alırsınız.
* PostgreSQL için Azure veritabanı, TLS özellikli yedeklemeler desteklenmez. Bir yedekleme yapılandırılmışsa, başarısız yedeklemeler alırsınız.
* Uygulama içi MySQL veritabanları, herhangi bir yapılandırma olmadan otomatik olarak yedeklenir. Uygulama içi MySQL veritabanları için bağlantı dizeleri ekleme gibi el ile ayarları yaparsanız yedeklemeler düzgün çalışmayabilir.
* Yedeklemeleriniz için hedef olarak Güvenlik Duvarı etkin bir depolama hesabı kullanılması desteklenmez. Bir yedekleme yapılandırılmışsa, başarısız yedeklemeler alırsınız.


<a name="manualbackup"></a>

## <a name="create-a-manual-backup"></a>El ile yedekleme oluşturun
1. [Azure Portal](https://portal.azure.com)uygulamanızın sayfasına gidin, **yedeklemeler**' i seçin. **Yedeklemeler** sayfası görüntülenir.

    ![Yedeklemeler sayfası](./media/manage-backup/access-backup-page.png)

    > [!NOTE]
    > Aşağıdaki iletiyi görürseniz, yedeklemelere devam edebilmeniz için App Service planınızı yükseltmek üzere tıklayın.
    > Daha fazla bilgi için bkz. [Azure 'da bir uygulamayı ölçeklendirme](manage-scale-up.md).
    > :::image type="content" source="./media/manage-backup/upgrade-plan.png" alt-text="Yedekleme ve geri yükleme özelliğine erişmek üzere App Service planını yükseltmek için ileti içeren bir başlığın ekran görüntüsü.":::
    > 
    > 

2. **Yedekleme** sayfasında, yedekleme Yapılandırılmadı ' yi seçin **. Uygulamanıza yönelik yedeklemeyi yapılandırmak için buraya tıklayın**.

    ![Yapılandır ' a tıklayın](./media/manage-backup/configure-start.png)

3. **Yedekleme yapılandırması** sayfasında depolama hesabı yapılandırmak için **yapılandırılmamış** ' a tıklayın.

    :::image type="content" source="./media/manage-backup/configure-storage.png" alt-text="Yedekleme ve geri yükleme özelliğine erişmek üzere App Service planını yükseltmek için ileti içeren bir başlığın ekran görüntüsü.":::

4. Bir **depolama hesabı** ve **kapsayıcı**seçerek yedekleme hedefini seçin. Depolama hesabı, yedeklemek istediğiniz uygulamayla aynı aboneliğe ait olmalıdır. İsterseniz, ilgili sayfalarda yeni bir depolama hesabı veya yeni bir kapsayıcı oluşturabilirsiniz. İşiniz bittiğinde **Seç**' e tıklayın.

5. Hala açık olan **yedekleme yapılandırması** sayfasında, **yedekleme veritabanını**yapılandırabilir ve ardından yedeklemelere eklemek Istediğiniz veritabanlarını (SQL veritabanı veya MySQL) seçip **Tamam**' a tıklayın.

    :::image type="content" source="./media/manage-backup/configure-database.png" alt-text="Yedekleme ve geri yükleme özelliğine erişmek üzere App Service planını yükseltmek için ileti içeren bir başlığın ekran görüntüsü.":::

    > [!NOTE]
    > Bu listede bir veritabanının görünmesi için, uygulamanızın **uygulama ayarları** sayfasının **bağlantı dizeleri** bölümünde bağlantı dizesinin bulunması gerekir. 
    >
    > Uygulama içi MySQL veritabanları, herhangi bir yapılandırma olmadan otomatik olarak yedeklenir. Uygulama içi MySQL veritabanları için bağlantı dizeleri ekleme gibi el ile ayarları yaparsanız yedeklemeler düzgün çalışmayabilir.
    > 
    > 

6. **Yedekleme yapılandırması** sayfasında **Kaydet**' e tıklayın.
7. **Yedeklemeler** sayfasında, **Yedekle**' ye tıklayın.

    ![Şimdi Backupbutton](./media/manage-backup/manual-backup.png)

    Yedekleme işlemi sırasında bir ilerleme durumu iletisi görürsünüz.

Depolama hesabı ve kapsayıcısı yapılandırıldıktan sonra dilediğiniz zaman el ile yedekleme başlatabilirsiniz. El ile yedeklemeler süresiz olarak korunur.

<a name="automatedbackups"></a>

## <a name="configure-automated-backups"></a>Otomatik yedeklemeleri yapılandırma
1. **Yedekleme yapılandırması** sayfasında, **Zamanlanmış yedeklemeyi** **Açık**olarak ayarlayın. 

    ![Otomatik yedeklemeleri etkinleştir](./media/manage-backup/scheduled-backup.png)

2. Yedekleme zamanlamasını istediğiniz gibi yapılandırın ve **Tamam**' ı seçin.

<a name="partialbackups"></a>

## <a name="configure-partial-backups"></a>Kısmi yedeklemeleri yapılandırma
Bazen uygulamanızdaki her şeyi yedeklemek istemezsiniz. İşte birkaç örnek:

* Uygulamanızda, eski blog gönderileri veya görüntüleri gibi hiçbir değişiklik olmayan statik içerik içeren [haftalık yedeklemeler ayarlarsınız](#configure-automated-backups) .
* Uygulamanız 10 GB 'lık içeriğe sahiptir (bir seferde yedekleyebileceğiniz maksimum miktar).
* Günlük dosyalarını yedeklemek istemezsiniz.

Kısmi yedeklemeler, tam olarak yedeklemek istediğiniz dosyaları seçmenize olanak sağlar.

> [!NOTE]
> Yedekteki ayrı veritabanları 4GB'A fazla olabilir, ancak yedeklemenin toplam en büyük boyutu 10 GB 'dir

### <a name="exclude-files-from-your-backup"></a>Dosyaları yedeklemeinizden hariç tut
Bir kez yedekleme ve değişiklik yapamamayan günlük dosyalarını ve statik görüntüleri içeren bir uygulamanız olduğunu varsayalım. Böyle durumlarda, bu klasörleri ve dosyaları gelecekteki yedeklemelerde depolanmak üzere dışlayabilirsiniz. Yedeklemelerinizden dosya ve klasörleri dışlamak için `_backup.filter` uygulamanızın klasöründe bir dosya oluşturun `D:\home\site\wwwroot` . Bu dosyada dışlamak istediğiniz dosya ve klasörlerin listesini belirtin. 

Dosyalarınıza giderek dosyalarınıza erişebilirsiniz `https://<app-name>.scm.azurewebsites.net/DebugConsole` . İstenirse, Azure hesabınızda oturum açın.

Yedeklemelerinizden dışlamak istediğiniz klasörleri belirler. Örneğin, vurgulanan klasör ve dosyaları filtrelemek istiyorsunuz.

![Görüntüler klasörü](./media/manage-backup/kudu-images.png)

Adlı bir dosya oluşturun `_backup.filter` ve önceki listeyi dosyaya yerleştirin, ancak kaldırın `D:\home` . Satır başına bir dizin veya dosya listeleyin. Bu nedenle, dosyanın içeriği şu olmalıdır:

 ```
\site\wwwroot\Images\brand.png
\site\wwwroot\Images\2014
\site\wwwroot\Images\2013
```

`_backup.filter` `D:\home\site\wwwroot\` [FTP](deploy-ftp.md) 'yi veya başka bir yöntemi kullanarak sitenizin dizinine dosyayı yükleyin. İsterseniz, kudu kullanarak dosyayı doğrudan oluşturabilir `DebugConsole` ve buraya içerik ekleyebilirsiniz.

Yedeklemeleri, normalde [el ile](#create-a-manual-backup) veya [otomatik olarak](#configure-automated-backups)yaptığınız gibi çalıştırın. Şimdi, içinde belirtilen tüm dosyalar ve klasörler, `_backup.filter` zamanlanan veya el ile başlatılan yedeklemelerin dışında tutulur. 

> [!NOTE]
> Sitenizin kısmi yedeklerini [düzenli bir yedeklemeyi geri](web-sites-restore.md)yüklediğiniz şekilde geri yükleyebilirsiniz. Geri yükleme işlemi doğru şeyi yapar.
> 
> Tam yedekleme geri yüklendiğinde, sitedeki tüm içerikler, yedeklemedeki herhangi bir şey ile değiştirilmiştir. Bir dosya sitede yer alıyorsa, ancak yedeklemede silinir. Ancak kısmi bir yedekleme geri yüklendiğinde, kısıtlı dizinlerden birinde bulunan içerik veya herhangi bir kısıtlanmış dosya olduğu gibi kalır.
> 


<a name="aboutbackups"></a>

## <a name="how-backups-are-stored"></a>Yedeklemeler nasıl depolanır
Uygulamanız için bir veya daha fazla yedekleme yaptıktan sonra yedeklemeler, depolama hesabınızın **kapsayıcılar** sayfasında ve uygulamanızda görünür. Depolama hesabında, her yedekleme, `.zip` yedekleme verilerini ve `.xml` Dosya içeriklerinin bildirimini içeren bir dosyayı içeren bir dosyadan oluşur `.zip` . Uygulama geri yükleme işlemi yapmadan yedeklemelerinize erişmek istiyorsanız bu dosyaları açabilir ve bunlara gözatamazsınız.

Uygulamanın veritabanı yedeklemesi,. zip dosyasının kökünde saklanır. SQL veritabanı için bu bir BACPAC dosyasıdır (dosya uzantısı yoktur) ve içeri aktarılabilir. BACPAC dışarı aktarma 'ya göre Azure SQL veritabanı 'nda veritabanı oluşturmak için bkz. [Azure SQL veritabanı 'nda veritabanı oluşturmak IÇIN bacpac dosyasını Içeri aktarma](../azure-sql/database/database-import.md).

> [!WARNING]
> **Websitebackups** kabınızda herhangi bir dosyanın değiştirilmesi yedeklemenin geçersiz olmasına ve bu nedenle geri yüklenebilir olmasına neden olabilir.
> 
> 

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

[Azure CLI](/cli/azure/install-azure-cli) veya [Azure PowerShell](/powershell/azure/)kullanarak yedekleme yönetimini betiklerle otomatik hale getirebilirsiniz.

Örnekler için bkz.:

- [Azure CLI örnekleri](samples-cli.md)
- [Azure PowerShell örnekleri](samples-powershell.md)

<a name="nextsteps"></a>

## <a name="next-steps"></a>Sonraki Adımlar
Bir uygulamayı yedekten geri yükleme hakkında bilgi için bkz. [Azure 'da bir uygulamayı geri yükleme](web-sites-restore.md).