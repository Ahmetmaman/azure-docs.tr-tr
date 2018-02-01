---
title: "Azure Data Factory ile birden fazla tabloyu artımlı olarak kopyalama | Microsoft Docs"
description: "Bu öğreticide, değişim verileri şirket içi SQL Server veritabanındaki birden çok tablodan Azure SQL veritabanına artımlı olarak kopyalayan bir Azure Data Factory işlem hattı oluşturacaksınız."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/20/2018
ms.author: jingwang
ms.openlocfilehash: c79bce401b0f1d67d7955f4c97a5dfac5008be0d
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="incrementally-load-data-from-multiple-tables-in-sql-server-to-an-azure-sql-database"></a>SQL Server’daki birden fazla tablodan Azure SQL veritabanı’na artımlı olarak veri yükleme
Bu öğreticide, değişim verileri şirket içi SQL Server’daki birden çok tablodan Azure SQL Veritabanına yükleyen bir Azure veri fabrikası işlem hattı oluşturacaksınız.    

Bu öğreticide aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Kaynak ve hedef veri depolarını hazırlayın.
> * Veri fabrikası oluşturma.
> * Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma.
> * Tümleştirme çalışma zamanını yükleyin. 
> * Bağlı hizmet oluşturma. 
> * Kaynak, havuz ve eşik veri kümeleri oluşturun.
> * İşlem hattını oluşturma, çalıştırma ve izleme.
> * Sonuçları gözden geçirin.
> * Kaynak tablolarına veri ekleyin veya bu verileri güncelleştirin.
> * İşlem hattını yeniden çalıştırın ve izleyin.
> * Son sonuçları gözden geçirin.

> [!NOTE]
> Bu makale, şu anda önizleme sürümünde olan Azure Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 belgeleri](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) konusunu inceleyin.

## <a name="overview"></a>Genel Bakış
Bu çözümü oluşturmak için önemli adımlar şunlardır: 

1. **Eşit sütununu seçin**.
    
    Kaynak veri deposunda her çalıştırma için yeni veya güncelleştirilmiş kayıtları tanımlamak için her tablodan kullanılabilen bir sütun seçin. Normalde, satırlar oluşturulduğunda veya güncelleştirildiğinde seçilen bu sütundaki veriler (örneğin, last_modify_time veya kimlik) artmaya devam eder. Bu sütundaki en büyük değer eşik olarak kullanılır.

2. **Eşik değerini depolamak için veri deposunu hazırlayın**.   
    
    Bu öğreticide, eşik değerini bir SQL veritabanında depolayacaksınız.

3. **Aşağıdaki eylemler ile bir işlem hattı oluşturun**: 
    
    a. İşlem hattına parametre olarak geçen kaynak tablosu adlarının bir listesi üzerinden yinelenen bir ForEach eylemi oluşturun. Her kaynak tablosunda, bu tabloya yönelik olarak yüklenen değişiklikleri gerçekleştirmek için aşağıdaki eylemleri çağırır.

    b. İki arama etkinliği oluşturun. Son eşik değerini almak için ilk Arama etkinliğini kullanın. Yeni eşik değerini almak için ikinci Arama etkinliğini kullanın. Bu eşik değerleri, Kopyalama etkinliğine geçirilir.

    c. Eşik sütununun değeri eski eşik değerinden büyük ve yeni eşik değerinden küçük olacak şekilde, satırları kaynak veri deposundan kopyalayan bir Kopyalama etkinliği oluşturun. Ardından, delta veriler kaynak veri deposundan Azure Blob depolama alanına yeni bir dosya olarak kopyalanır.

    d. Sonraki seferde çalışan işlem hattı için eşik değerini güncelleştiren bir StoredProcedure etkinliği oluşturun. 

    Yüksek düzeyli çözüm diyagramı aşağıdaki gibidir: 

    ![Artımlı olarak veri yükleme](media\tutorial-incremental-copy-multiple-tables-portal\high-level-solution-diagram.png)


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar
* **SQL Server**. Bu öğreticide şirket içi SQL Server veritabanını kaynak veri deposu olarak kullanırsınız. 
* **Azure SQL Veritabanı**. SQL veritabanını havuz veri deposu olarak kullanırsınız. SQL veritabanınız yoksa, oluşturma adımları için bkz. [Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started-portal.md). 

### <a name="create-source-tables-in-your-sql-server-database"></a>SQL Server veritabanınızda kaynak tabloları oluşturma

1. SQL Server Management Studio’yu açın ve şirket içi SQL Server veritabanınıza bağlanın.

2. **Sunucu Gezgini**’nde veritabanına sağ tıklayın ve **Yeni Sorgu**’yu seçin.

3. `customer_table` ve `project_table` adlı tabloları oluşturmak için aşağıdaki SQL komutunu veritabanınızda çalıştırın:

    ```sql
    create table customer_table
    (
        PersonID int,
        Name varchar(255),
        LastModifytime datetime
    );
    
    create table project_table
    (
        Project varchar(255),
        Creationtime datetime
    );
        
    INSERT INTO customer_table
    (PersonID, Name, LastModifytime)
    VALUES
    (1, 'John','9/1/2017 12:56:00 AM'),
    (2, 'Mike','9/2/2017 5:23:00 AM'),
    (3, 'Alice','9/3/2017 2:36:00 AM'),
    (4, 'Andy','9/4/2017 3:21:00 AM'),
    (5, 'Anny','9/5/2017 8:06:00 AM');
    
    INSERT INTO project_table
    (Project, Creationtime)
    VALUES
    ('project1','1/1/2015 0:00:00 AM'),
    ('project2','2/2/2016 1:23:00 AM'),
    ('project3','3/4/2017 5:16:00 AM');
    
    ```

### <a name="create-destination-tables-in-your-azure-sql-database"></a>Azure SQL veritabanınızda hedef tablo oluşturma
1. SQL Server Management Studio’yu açın ve Azure SQL veritabanınıza bağlanın.

2. **Sunucu Gezgini**’nde veritabanına sağ tıklayın ve **Yeni Sorgu**’yu seçin.

3. `customer_table` ve `project_table` adlı tabloları oluşturmak için aşağıdaki SQL komutunu SQL veritabanınızda çalıştırın:  
    
    ```sql
    create table customer_table
    (
        PersonID int,
        Name varchar(255),
        LastModifytime datetime
    );
    
    create table project_table
    (
        Project varchar(255),
        Creationtime datetime
    );

    ```

### <a name="create-another-table-in-the-sql-database-to-store-the-high-watermark-value"></a>Üst eşik değerini depolamak için SQL veritabanında başka bir tablo oluşturma
1. SQL veritabanınızda aşağıdaki SQL komutunu çalıştırarak eşik değerini depolamak için `watermarktable` adlı bir tablo oluşturun: 
    
    ```sql
    create table watermarktable
    (
    
        TableName varchar(255),
        WatermarkValue datetime,
    );
    ```
2. Her iki kaynak tablonun ilk eşik değerlerini eşik tablosuna ekleyin.

    ```sql

    INSERT INTO watermarktable
    VALUES 
    ('customer_table','1/1/2010 12:00:00 AM'),
    ('project_table','1/1/2010 12:00:00 AM');
    
    ```

### <a name="create-a-stored-procedure-in-the-sql-database"></a>SQL veritabanında bir saklı yordam oluşturma 

SQL veritabanınızda bir saklı yordam oluşturmak için aşağıdaki komutu çalıştırın. Bu saklı yordam, her işlem hattı çalıştırmasından sonra eşik değerini güncelleştirir. 

```sql
CREATE PROCEDURE sp_write_watermark @LastModifiedtime datetime, @TableName varchar(50)
AS

BEGIN

    UPDATE watermarktable
    SET [WatermarkValue] = @LastModifiedtime 
WHERE [TableName] = @TableName

END

```

### <a name="create-data-types-and-additional-stored-procedures"></a>Veri türleri ve ek saklı yordamlar oluşturma
SQL veritabanınızda iki saklı yordam ve iki veri türü oluşturmak için aşağıdaki sorguyu çalıştırın. Bunlar, kaynak tablodaki verileri hedef tablolarla birleştirmek için kullanılır.

```sql
CREATE TYPE DataTypeforCustomerTable AS TABLE(
    PersonID int,
    Name varchar(255),
    LastModifytime datetime
);

GO

CREATE PROCEDURE sp_upsert_customer_table @customer_table DataTypeforCustomerTable READONLY
AS

BEGIN
  MERGE customer_table AS target
  USING @customer_table AS source
  ON (target.PersonID = source.PersonID)
  WHEN MATCHED THEN
      UPDATE SET Name = source.Name,LastModifytime = source.LastModifytime
  WHEN NOT MATCHED THEN
      INSERT (PersonID, Name, LastModifytime)
      VALUES (source.PersonID, source.Name, source.LastModifytime);
END

GO

CREATE TYPE DataTypeforProjectTable AS TABLE(
    Project varchar(255),
    Creationtime datetime
);

GO

CREATE PROCEDURE sp_upsert_project_table @project_table DataTypeforProjectTable READONLY
AS

BEGIN
  MERGE project_table AS target
  USING @project_table AS source
  ON (target.Project = source.Project)
  WHEN MATCHED THEN
      UPDATE SET Creationtime = source.Creationtime
  WHEN NOT MATCHED THEN
      INSERT (Project, Creationtime)
      VALUES (source.Project, source.Creationtime);
END

```

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Soldaki menüde **Yeni**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın. 
   
   ![Yeni->DataFactory](./media/tutorial-incremental-copy-multiple-tables-portal/new-azure-data-factory-menu.png)
2. **Yeni veri fabrikası** sayfasında **ad** için **ADFMultiIncCopyTutorialDF** adını girin. 
      
     ![Yeni veri fabrikası sayfası](./media/tutorial-incremental-copy-multiple-tables-portal/new-azure-data-factory.png)
 
   Azure data factory adı **küresel olarak benzersiz** olmalıdır. Aşağıdaki hatayı alırsanız veri fabrikasının adını değiştirin (örneğin adınızADFMultiIncCopyTutorialDF) ve oluşturmayı yeniden deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](naming-rules.md) makalesine bakın.
  
       `Data factory name ADFMultiIncCopyTutorialDF is not available`
3. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
      - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
      - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
        Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
4. **Sürüm** için **V2 (Önizleme)** öğesini seçin.
5. Data factory için **konum** seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.
6. **Panoya sabitle**’yi seçin.     
7. **Oluştur**’a tıklayın.      
8. Panoda şu kutucuğu ve üzerinde şu durumu görürsünüz: **Veri fabrikası dağıtılıyor**. 

    ![veri fabrikası dağıtılıyor kutucuğu](media/tutorial-incremental-copy-multiple-tables-portal/deploying-data-factory.png)
9. Oluşturma işlemi tamamlandıktan sonra, resimde gösterildiği gibi **Data Factory** sayfasını görürsünüz.
   
   ![Data factory giriş sayfası](./media/tutorial-incremental-copy-multiple-tables-portal/data-factory-home-page.png)
10. Azure Data Factory kullanıcı arabirimini (UI) ayrı bir sekmede açmak için **Yazar ve İzleyici** kutucuğuna tıklayın.
11. Azure Data Factory kullanıcı arabiriminin başlangıç sayfasında **İşlem hattı oluştur**’a tıklayın (veya) **Düzenle** sekmesine geçin. 

   ![Başlarken sayfası](./media/tutorial-incremental-copy-multiple-tables-portal/get-started-page.png)

## <a name="create-self-hosted-integration-runtime"></a>Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma
Özel bir ağda yer alan (şirket içi) bir veri deposundaki verileri bir Azure veri deposuna taşırken, şirket içi ortamınıza şirket içinde barındırılan bir tümleştirme çalışma zamanı (IR) yükleyin. Şirket içinde barındırılan IR, özel ağınız ile Azure arasında veri taşır. 

1. Sol bölmenin altından **Bağlantılar**’a tıklayın ve **Bağlantılar** penceresinde **Tümleştirme Çalışma Zamanları**’na geçin. 

   ![Bağlantılar sekmesi](./media/tutorial-incremental-copy-multiple-tables-portal/connections-tab.png)
2. **Tümleştirme Çalışma Zamanları** sekmesinde **+ Yeni**’ye tıklayın. 

   ![Yeni tümleştirme çalışma zamanı - düğme](./media/tutorial-incremental-copy-multiple-tables-portal/new-integration-runtime-button.png)
3. **Tümleştirme Çalışma Zamanı Kurulumu** penceresinde **Dış işlemlere veri taşıma ve dağıtım etkinlikleri gerçekleştir**’i seçip **İleri**’ye tıklayın. 

   ![Tümleştirme çalışma zamanı türünü seçme](./media/tutorial-incremental-copy-multiple-tables-portal/select-integration-runtime-type.png)
4. ** Özel Ağ** seçeneğini belirleyip **İleri**’ye tıklayın. 

   ![Özel ağı seçin](./media/tutorial-incremental-copy-multiple-tables-portal/select-private-network.png)
5. **Ad** için **MySelfHostedIR** adını girip **İleri**’ye tıklayın. 

   ![Şirket içinde barındırılan IR adı](./media/tutorial-incremental-copy-multiple-tables-portal/self-hosted-ir-name.png)
10. **1. Seçenek: Hızlı kurulum** bölümünde **Bu bilgisayarda hızlı kurulumu başlatmak için buraya tıklayın** seçeneğine tıklayın. 

   ![Hızlı kurulum bağlantısına tıklayın](./media/tutorial-incremental-copy-multiple-tables-portal/click-exress-setup.png)
11. **Tümleştirme Çalışma Zamanı (Şirket İçinde Barındırılan) Hızlı Kurulum** penceresinde **Kapat**’a tıklayın. 

   ![Tümleştirme çalışma zamanı kurulumu - başarılı](./media/tutorial-incremental-copy-multiple-tables-portal/integration-runtime-setup-successful.png)
12. Web tarayıcısında, **Tümleştirme Çalışma Zamanı Kurulumu** penceresinde **Son**’a tıklayın. 

   ![Tümleştirme çalışma zamanı kurulumu - son](./media/tutorial-incremental-copy-multiple-tables-portal/click-finish-integration-runtime-setup.png)
17. Tümleştirme çalışma zamanları listesinde **MySelfHostedIR** öğesini gördüğünüzü doğrulayın.

       ![Tümleştirme çalışma zamanları - liste](./media/tutorial-incremental-copy-multiple-tables-portal/integration-runtimes-list.png)

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma
Veri depolarınızı ve işlem hizmetlerinizi veri fabrikasına bağlamak için veri fabrikasında bağlı hizmetler oluşturursunuz. Bu bölümde, şirket içi SQL Server veritabanı ve SQL veritabanı hesabınızla bağlı hizmetler oluşturacaksınız. 

### <a name="create-the-sql-server-linked-service"></a>SQL Server bağlı hizmet oluşturma
Bu adımda, şirket içi SQL Server veritabanınızı veri fabrikasına bağlarsınız.

1. **Bağlantılar** penceresinde **Tümleştirme Çalışma Zamanları** sekmesinden **Bağlı Hizmetler** sekmesine geçip **+ Yeni**’ye tıklayın.

    ![Yeni Bağlı Hizmet düğmesi](./media/tutorial-incremental-copy-multiple-tables-portal/new-sql-server-linked-service-button.png)
2. **Yeni Bağlı Hizmet** penceresinde **SQL Server**’ı seçip **Devam**’a tıklayın. 

    ![SQL Server'ı seçin](./media/tutorial-incremental-copy-multiple-tables-portal/select-sql-server.png)
3. **Yeni Bağlı Hizmet** penceresinde aşağıdaki adımları izleyin:

    1. **Ad** için **SqlServerLinkedService** adını girin. 
    2. **Tümleştirme çalışma zamanı aracılığıyla bağlan** için **MySelfHostedIR** seçeneğini belirleyin. Bu **önemli** bir adımdır. Varsayılan tümleştirme çalışma zamanı bir şirket içi veri deposuna bağlanamaz. Daha önce oluşturduğunuz şirket içinde barındırılan tümleştirme çalışma zamanını kullanın. 
    3. **Sunucu adı** olarak SQL Server veritabanını içeren bilgisayarınızın adını girin.
    4. **Veritabanı adı** olarak SQL Server’ınızdaki kaynak verileri içeren veritabanının adını girin. Ön koşulların bir parçası olarak bir tablo oluşturdunuz ve bu veritabanına veri eklediniz. 
    5. **Kimlik doğrulama türü** alanına veritabanına bağlanmak için kullanmak istediğiniz **kimlik doğrulama türünü** seçin. 
    6. **Kullanıcı adı** alanına SQL Server veritabanına erişimi olan kullanıcının adını girin. Kullanıcı hesabınızda veya sunucu adında eğik çizgi karakteri (`\`) kullanmanız gerekirse kaçış karakterini (`\`) kullanın. `mydomain\\myuser` bunun bir örneğidir.
    7. **Parola** alanına kullanıcının **parolasını** girin. 
    8. Data Factory’nin SQL Server veritabanınıza bağlanıp bağlanamadığını test etmek için **Bağlantıyı sına**’ya tıklayın. Bağlantı başarılı olana kadar tüm hataları düzeltin. 
    9. Bağlı hizmeti kaydetmek için **Kaydet**’e tıklayın.

        ![SQL Server bağlı hizmeti - ayarlar](./media/tutorial-incremental-copy-multiple-tables-portal/sql-server-linked-service-settings.png)

### <a name="create-the-azure-sql-database-linked-service"></a>Azure SQL Veritabanı bağlı hizmetini oluşturun
Son adımda, kaynak SQL Server veritabanınızı veri fabrikasına bağlamak için bağlı bir hizmet oluşturursunuz. Bu adımda, hedef/havuz Azure SQL veritabanınızı veri fabrikasına bağlarsınız. 

1. **Bağlantılar** penceresinde **Tümleştirme Çalışma Zamanları** sekmesinden **Bağlı Hizmetler** sekmesine geçip **+ Yeni**’ye tıklayın.

    ![Yeni Bağlı Hizmet düğmesi](./media/tutorial-incremental-copy-multiple-tables-portal/new-sql-server-linked-service-button.png)
2. **Yeni Bağlı Hizmet** penceresinde **Azure SQL Veritabanı**’nı seçip **Devam**’a tıklayın. 
3. **Yeni Bağlı Hizmet** penceresinde aşağıdaki adımları izleyin:

    1. **Ad** için **AzureSqlDatabaseLinkedService** adını girin. 
    3. **Sunucu adı** alanına açılan listeden Azure SQL Server’ınızın adını seçin. 
    4. **Veritabanı adı** alanına, ön koşulların bir parçası olarak içinde customer_table ve project_table tablolarını oluşturduğunuz Azure SQL veritabanını girin. 
    6. **Kullanıcı adı** alanına SQL Server veritabanına erişimi olan kullanıcının adını girin. 
    7. **Parola** alanına kullanıcının **parolasını** girin. 
    8. Data Factory’nin SQL Server veritabanınıza bağlanıp bağlanamadığını test etmek için **Bağlantıyı sına**’ya tıklayın. Bağlantı başarılı olana kadar tüm hataları düzeltin. 
    9. Bağlı hizmeti kaydetmek için **Kaydet**’e tıklayın.

        ![Azure SQL bağlı hizmeti - ayarlar](./media/tutorial-incremental-copy-multiple-tables-portal/azure-sql-linked-service-settings.png)
10. Listede iki bağlı hizmet gördüğünüzü doğrulayın. 
   
    ![İki bağlı hizmet](./media/tutorial-incremental-copy-multiple-tables-portal/two-linked-services.png) 

## <a name="create-datasets"></a>Veri kümeleri oluşturma
Bu adımda veri kaynağı, veri hedefi ve eşiğin depolanacağı yeri temsil eden veri kümeleri oluşturacaksınız.

### <a name="create-a-source-dataset"></a>Kaynak veri kümesi oluşturma

1. Sol bölmede, **+ (artı)** düğmesine ve sonra **Veri Kümesi**’ne tıklayın.

   ![Yeni Veri Kümesi menüsü](./media/tutorial-incremental-copy-multiple-tables-portal/new-dataset-menu.png)
2. **Yeni Veri Kümesi** penceresinde **SQL Server**’ı seçip **Son**’a tıklayın. 

   ![SQL Server'ı seçin](./media/tutorial-incremental-copy-multiple-tables-portal/select-sql-server-for-dataset.png)
3. Web tarayıcısında veri kümesinin yapılandırılması için yeni bir sekme açıldığını görürsünüz. Ayrıca, ağaç görünümünde de bir veri kümesi görürsünüz. Alttaki Özellikler penceresinin **Genel** sekmesinde **Ad** için **SourceDataset** adını girin. 

   ![Kaynak veri kümesi - ad](./media/tutorial-incremental-copy-multiple-tables-portal/source-dataset-general.png)
4. Özellikler penceresinin **Bağlantı** sekmesine geçin ve **Bağlı hizmet** için **SqlServerLinkedService** hizmetini seçin. Burada bir tablo seçmezsiniz. İşlem hattındaki Kopyalama etkinliği, tüm tabloyu yüklemek yerine verileri yüklemek için bir SQL sorgusu kullanır.

   ![Kaynak veri kümesi - bağlantı](./media/tutorial-incremental-copy-multiple-tables-portal/source-dataset-connection.png)


### <a name="create-a-sink-dataset"></a>Havuz veri kümesi oluşturma
1. Sol bölmede, **+ (artı)** düğmesine ve sonra **Veri Kümesi**’ne tıklayın.

   ![Yeni Veri Kümesi menüsü](./media/tutorial-incremental-copy-multiple-tables-portal/new-dataset-menu.png)
2. **Yeni Veri Kümesi** penceresinde **Azure SQL Veritabanı**’nı seçin ve **Son**’a tıklayın. 

   ![Azure SQL Veritabanı’nı seçin](./media/tutorial-incremental-copy-multiple-tables-portal/select-azure-sql-database.png)
3. Web tarayıcısında veri kümesinin yapılandırılması için yeni bir sekme açıldığını görürsünüz. Ayrıca, ağaç görünümünde de bir veri kümesi görürsünüz. Alttaki Özellikler penceresinin **Genel** sekmesinde, **Ad** için **SinkDataset** adını girin.

   ![Havuz Veri Kümesi - genel](./media/tutorial-incremental-copy-multiple-tables-portal/sink-dataset-general.png)
4. Özellikler penceresinin **Bağlantı** sekmesine geçin ve **Bağlı hizmet** için **AzureSqlLinkedService** hizmetini seçin. 

   ![Havuz Veri Kümesi - bağlantı](./media/tutorial-incremental-copy-multiple-tables-portal/sink-dataset-connection.png)
5. Özellikler penceresinin **Parametreler** sekmesine geçin ve aşağıdaki adımları uygulayın: 

    1. **Parametre oluştur/güncelleştir** bölümünde **+ Yeni**’ye tıklayın. 
    2. **Ad** alanına **SinkTableName**, **tür** alanına **String** değerini girin. Bu veri kümesi, **SinkTableName** değerini bir parametre olarak alır. SinkTableName parametresi, çalışma zamanında dinamik olarak işlem hattı tarafından ayarlanır. İşlem hattındaki ForEach etkinliği, tablo adlarının bir listesi üzerinden yinelenir ve her yinelemede tablo adını bu veri kümesine geçirir.
    3. **Parametreleştirilebilir özellikler** bölümünde, **tableName** özelliği için `@{dataset().SinkTableName}` girin. Veri kümesinin **tableName** özelliğini başlatmak için **SinkTableName** parametresine geçirilen değeri kullanırsınız. 

       ![Havuz Veri Kümesi - özellikler](./media/tutorial-incremental-copy-multiple-tables-portal/sink-dataset-parameters.png)

### <a name="create-a-dataset-for-a-watermark"></a>Eşik için veri kümesi oluşturma
Bu adımda üst eşik değerini depolamak için bir veri kümesi oluşturacaksınız. 

1. Sol bölmede, **+ (artı)** düğmesine ve sonra **Veri Kümesi**’ne tıklayın.

   ![Yeni Veri Kümesi menüsü](./media/tutorial-incremental-copy-multiple-tables-portal/new-dataset-menu.png)
2. **Yeni Veri Kümesi** penceresinde **Azure SQL Veritabanı**’nı seçin ve **Son**’a tıklayın. 

   ![Azure SQL Veritabanı’nı seçin](./media/tutorial-incremental-copy-multiple-tables-portal/select-azure-sql-database.png)
3. Alttaki Özellikler penceresinin **Genel** sekmesinde, **Ad** için **WatermarkDataset** adını girin.
4. **Bağlantı** sekmesine geçin ve aşağıdaki adımları uygulayın: 

    1. **Bağlı hizmet** için **AzureSqlDatabaseLinkedService** hizmetini seçin.
    2. **Tablo** için **[dbo].[watermarktable]** seçeneğini belirleyin.

       ![Filigran Veri Kümesi - bağlantı](./media/tutorial-incremental-copy-multiple-tables-portal/watermark-dataset-connection.png)

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma
Bu işlem hattı parametre olarak tablo adları listesini alır. ForEach etkinliği, tablo adları listesi üzerinden yinelenir ve aşağıdaki işlemleri gerçekleştirir: 

1. Eski eşik değerini (ilk değer veya son yinelemede kullanılan değer) almak için Arama etkinliğini kullanın.

2. Yeni eşik değerini (kaynak tablodaki eşik sütununda bulunan en yüksek değer) almak için Arama etkinliğini kullanın.

3. Bu iki eşik değeri arasında kaynak veritabanından hedef veritabanına veri kopyalamak için Kopyalama etkinliğini kullanın.

4. Bir sonraki yinelemede kullanılacak eski eşik değerini güncelleştirmek için StoredProcedure etkinliğini kullanın. 

### <a name="create-the-pipeline"></a>İşlem hattını oluşturma

1. Sol bölmede, **+ (artı)** düğmesine ve sonra da **İşlem Hattı**’na tıklayın.

    ![Yeni İşlem Hattı - menü](./media/tutorial-incremental-copy-multiple-tables-portal/new-pipeline-menu.png)
2. **Özellikler** penceresinin **Genel** sekmesinde **Ad** için **IncrementalCopyPipeline** adını girin. 

    ![İşlem hattı adı](./media/tutorial-incremental-copy-multiple-tables-portal/pipeline-name.png)
3. **Özellikler** penceresinde aşağıdaki adımları uygulayın: 

    1. **+ Yeni** öğesine tıklayın. 
    2. **name** parametresi için **tableList** girin. 
    3. Parametre **türü** olarak **Nesne** seçeneğini belirleyin.

    ![İşlem hattı parametreleri](./media/tutorial-incremental-copy-multiple-tables-portal/pipeline-parameters.png) 
4. **Etkinlikler** araç kutusundan **ForEach** etkinliğini sürükleyerek işlem hattı tasarımcısının yüzeyine bırakın. **Özellikler** penceresinin **Genel** sekmesinde **IterateSQLTables** girin. 

    ![ForEach etkinliği - ad](./media/tutorial-incremental-copy-multiple-tables-portal/foreach-name.png)
5. **Özellikler** penceresinin **Ayarlar** sekmesine geçin ve **Öğeler** için `@pipeline().parameters.tableList` girin. ForEach etkinliği, bir tablo listesi üzerinden yinelenir ve artımlı kopyalama işlemini gerçekleştirir. 

    ![ForEach etkinliği - ayarlar](./media/tutorial-incremental-copy-multiple-tables-portal/foreach-settings.png)
6. İşlem hattında **ForEach** etkinliği zaten seçili değilse bunu seçin. **Düzenle (Kalem simgesi)** düğmesine tıklayın.

    ![ForEach etkinliği - düzenleme](./media/tutorial-incremental-copy-multiple-tables-portal/edit-foreach.png)
7. **Etkinlikler** araç kutusundan **Lookup** etkinliğini sürükleyip bırakın ve **Ad** için **LookupOldWaterMarkActivity** adını girin.

    ![İlk Arama Etkinliği - ad](./media/tutorial-incremental-copy-multiple-tables-portal/first-lookup-name.png)
8. **Özellikler** penceresinin **Ayarlar** sekmesine geçin ve aşağıdaki adımları uygulayın: 

    1. **Kaynak Veri Kümesi** için **WatermarkDataset**’i seçin.
    2. **Sorgu Kullan** için **Sorgu**’yu seçin. 
    3. **Sorgu** için aşağıdaki SQL sorgusunu girin. 

        ```sql
        select * from watermarktable where TableName  =  '@{item().TABLE_NAME}'
        ```

        ![İlk Arama Etkinliği - ayarlar](./media/tutorial-incremental-copy-multiple-tables-portal/first-lookup-settings.png)
9. **Etkinlikler** araç kutusundan **Arama** etkinliğini sürükleyip bırakın ve **Ad** için **LookupNewWaterMarkActivity** adını girin.
        
    ![İkinci Arama Etkinliği - ad](./media/tutorial-incremental-copy-multiple-tables-portal/second-lookup-name.png)
10. **Ayarlar** sekmesine geçin.

    1. **Kaynak Veri Kümesi** için **SourceDataset**’i seçin. 
    2. **Sorgu Kullan** için **Sorgu**’yu seçin.
    3. **Sorgu** için aşağıdaki SQL sorgusunu girin.

        ```sql    
        select MAX(@{item().WaterMark_Column}) as NewWatermarkvalue from @{item().TABLE_NAME}
        ```
    
        ![İkinci Arama Etkinliği - ayarlar](./media/tutorial-incremental-copy-multiple-tables-portal/second-lookup-settings.png)
11. **Etkinlikler** araç kutusundan **Kopyalama** etkinliğini sürükleyip bırakın ve **Ad** için **IncrementalCopyActivity** adını girin. 

    ![Kopyalama Etkinliği - ad](./media/tutorial-incremental-copy-multiple-tables-portal/copy-activity-name.png)
12. **Arama** etkinliklerini tek tek **Kopyalama** etkinliğine bağlayın. Bağlanmak için **Arama** etkinliğine bağlı **yeşil** kutuyu sürüklemeye başlayın ve **Kopyalama** etkinliğinin üzerine başlayın. Kopyalama etkinliğinin kenarlık rengi **mavi** olduğunda fare düğmesini bırakın.

    ![Arama etkinliklerini Kopyalama etkinliğine bağlama](./media/tutorial-incremental-copy-multiple-tables-portal/connect-lookup-to-copy.png)
13. İşlem hattında **Kopyalama** etkinliğini seçin. **Özellikler** penceresinin **Kaynak** sekmesine geçin. 

    1. **Kaynak Veri Kümesi** için **SourceDataset**’i seçin. 
    2. **Sorgu Kullan** için **Sorgu**’yu seçin. 
    3. **Sorgu** için aşağıdaki SQL sorgusunu girin.

        ```sql
        select * from @{item().TABLE_NAME} where @{item().WaterMark_Column} > '@{activity('LookupOldWaterMarkActivity').output.firstRow.WatermarkValue}' and @{item().WaterMark_Column} <= '@{activity('LookupNewWaterMarkActivity').output.firstRow.NewWatermarkvalue}'        
        ```

        ![Kopyalama Etkinliği - kaynak ayarları](./media/tutorial-incremental-copy-multiple-tables-portal/copy-source-settings.png)
14. **Havuz** sekmesine geçin ve **Havuz Veri Kümesi** alanı için **SinkDataset**’i seçin. 
        
    ![Kopyalama Etkinliği - havuz ayarları](./media/tutorial-incremental-copy-multiple-tables-portal/copy-sink-settings.png)
15. **Parametreler** sekmesine geçin ve aşağıdaki adımları uygulayın:

    1. **Havuz Saklı Yordam Adı** özelliği için `@{item().StoredProcedureNameForMergeOperation}` adını girin.
    2. **Havuz Tablo Türü** özelliği için `@{item().TableType}` değerini girin.
    3. **Havuz Veri Kümesi** bölümünde **SinkTableName** parametresi için `@{item().TABLE_NAME}` değerini girin.

        ![Kopyalama Etkinliği - parametreler](./media/tutorial-incremental-copy-multiple-tables-portal/copy-activity-parameters.png)
16. **Etkinlikler** araç kutusundan **Saklı Yordam** etkinliğini sürükleyerek işlem hattı tasarımcısının yüzeyine bırakın. **Kopyalama** etkinliğini **Saklı Yordam** etkinliğine bağlayın. 

    ![Kopyalama Etkinliği - parametreler](./media/tutorial-incremental-copy-multiple-tables-portal/connect-copy-to-sproc.png)
17. İşlem hattında **Saklı Yordam** etkinliğini seçin ve **Özellikler** penceresinin **Genel** sekmesinde **Ad** alanına **StoredProceduretoWriteWatermarkActivity** adını girin. 

    ![Saklı Yordam Etkinliği - ad](./media/tutorial-incremental-copy-multiple-tables-portal/sproc-activity-name.png)
18. **SQL Hesabı** sekmesine geçin ve **Bağlı Hizmet** için **AzureSqlDatabaseLinkedService** seçeneğini belirleyin.

    ![Saklı Yordam Etkinliği - SQL Hesabı](./media/tutorial-incremental-copy-multiple-tables-portal/sproc-activity-sql-account.png)
19. **Saklı Yordam** sekmesine geçin ve aşağıdaki adımları uygulayın:

    1. **Saklı yordam adı** alanına `sp_write_watermark` adını girin. 
    2. **Yeni** düğmesini kullanarak aşağıdaki parametreleri ekleyin: 

        | Adı | Tür | Değer | 
        | ---- | ---- | ----- |
        | LastModifiedtime | datetime | `@{activity('LookupNewWaterMarkActivity').output.firstRow.NewWatermarkvalue}` |
        | TableName | Dize | `@{activity('LookupOldWaterMarkActivity').output.firstRow.TableName}` |
    
        ![Saklı Yordam Etkinliği - saklı yordam ayarları](./media/tutorial-incremental-copy-multiple-tables-portal/sproc-activity-sproc-settings.png)
20. Sol bölmede **Yayımla**'ya tıklayın. Bu eylem, oluşturduğunuz varlıkları Data Factory hizmetinde yayımlar. 

    ![Yayımla düğmesi](./media/tutorial-incremental-copy-multiple-tables-portal/publish-button.png)
21. **Başarıyla yayımlandı** iletisini görene kadar bekleyin. Bildirimleri görmek için **Bildirimleri Göster** bağlantısına tıklayın. **X** simgesine tıklayarak bildirim penceresini kapatın.

    ![Bildirimleri Göster](./media/tutorial-incremental-copy-multiple-tables-portal/notifications.png)

 
## <a name="run-the-pipeline"></a>İşlem hattını çalıştırma

1. İşlem hattının araç çubuğunda **Tetikle**’ye tıklayıp **Şimdi Tetikle**’ye tıklayın.     

    ![Şimdi tetikle](./media/tutorial-incremental-copy-multiple-tables-portal/trigger-now.png)
2. **İşlem Hattı Çalıştırma** penceresinde **tableList** parametresi için aşağıdaki değeri girip **Son**’a tıklayın. 

    ```
    [
        {
            "TABLE_NAME": "customer_table",
            "WaterMark_Column": "LastModifytime",
            "TableType": "DataTypeforCustomerTable",
            "StoredProcedureNameForMergeOperation": "sp_upsert_customer_table"
        },
        {
            "TABLE_NAME": "project_table",
            "WaterMark_Column": "Creationtime",
            "TableType": "DataTypeforProjectTable",
            "StoredProcedureNameForMergeOperation": "sp_upsert_project_table"
        }
    ]
    ```

    ![İşlem Hattı Çalıştırma bağımsız değişkenleri](./media/tutorial-incremental-copy-multiple-tables-portal/pipeline-run-arguments.png)

## <a name="monitor-the-pipeline"></a>İşlem hattını izleme

1. Soldaki **İzleyici** sekmesine geçin. **El ile tetikleme** yoluyla tetiklenen işlem hattı çalıştırmasını görürsünüz. Listeyi yenilemek için **Yenile** düğmesine tıklayın. **Eylemler** sütunundaki bağlantılar, işlem hattı çalıştırmasıyla ilişkili etkinlik çalıştırmalarını görüntülemenize ve işlem hattını yeniden çalıştırmanıza imkan tanır. 

    ![İşlem hattı çalıştırmaları](./media/tutorial-incremental-copy-multiple-tables-portal/pipeline-runs.png)
2. **Eylemler** sütunundaki **Etkinlik Çalıştırmalarını Görüntüle** bağlantısına tıklayın. Seçili işlem hattı çalıştırmasıyla ilişkili tüm etkinlik çalıştırmalarını görürsünüz. 

    ![Etkinlik çalıştırmaları](./media/tutorial-incremental-copy-multiple-tables-portal/activity-runs.png)

## <a name="review-the-results"></a>Sonuçları gözden geçirin
Verilerin kaynak tablolardan hedef tablolara kopyalandığını doğrulamak için, SQL Server Management Studio’da SQL veritabanında aşağıdaki sorguları çalıştırın: 

**Sorgu** 
```sql
select * from customer_table
```

**Çıktı**
```
===========================================
PersonID    Name    LastModifytime
===========================================
1           John    2017-09-01 00:56:00.000
2           Mike    2017-09-02 05:23:00.000
3           Alice   2017-09-03 02:36:00.000
4           Andy    2017-09-04 03:21:00.000
5           Anny    2017-09-05 08:06:00.000
```

**Sorgu**

```sql
select * from project_table
```

**Çıktı**

```
===================================
Project     Creationtime
===================================
project1    2015-01-01 00:00:00.000
project2    2016-02-02 01:23:00.000
project3    2017-03-04 05:16:00.000
```

**Sorgu**

```sql
select * from watermarktable
```

**Çıktı**

```
======================================
TableName       WatermarkValue
======================================
customer_table  2017-09-05 08:06:00.000
project_table   2017-03-04 05:16:00.000
```

Her iki tablonun da eşik değerlerinin güncelleştirildiğine dikkat edin. 

## <a name="add-more-data-to-the-source-tables"></a>Kaynak tablolara daha fazla veri ekleme

customer_table içerisindeki mevcut bir satırı güncelleştirmek için aşağıdaki sorguyu kaynak SQL Server veritabanında çalıştırın. Project_table içine yeni bir satır ekleyin. 

```sql
UPDATE customer_table
SET [LastModifytime] = '2017-09-08T00:00:00Z', [name]='NewName' where [PersonID] = 3

INSERT INTO project_table
(Project, Creationtime)
VALUES
('NewProject','10/1/2017 0:00:00 AM');
``` 

## <a name="rerun-the-pipeline"></a>İşlem hattını yeniden çalıştırma
1. Web tarayıcısı penceresinde, soldaki **Düzenle** sekmesine geçin. 
2. İşlem hattının araç çubuğunda **Tetikle**’ye tıklayıp **Şimdi Tetikle**’ye tıklayın.   

    ![Şimdi tetikle](./media/tutorial-incremental-copy-multiple-tables-portal/trigger-now.png)
1. **İşlem Hattı Çalıştırma** penceresinde **tableList** parametresi için aşağıdaki değeri girip **Son**’a tıklayın. 

    ```
    [
        {
            "TABLE_NAME": "customer_table",
            "WaterMark_Column": "LastModifytime",
            "TableType": "DataTypeforCustomerTable",
            "StoredProcedureNameForMergeOperation": "sp_upsert_customer_table"
        },
        {
            "TABLE_NAME": "project_table",
            "WaterMark_Column": "Creationtime",
            "TableType": "DataTypeforProjectTable",
            "StoredProcedureNameForMergeOperation": "sp_upsert_project_table"
        }
    ]
    ```

## <a name="monitor-the-pipeline"></a>İşlem hattını izleme

1. Soldaki **İzleyici** sekmesine geçin. **El ile tetikleme** yoluyla tetiklenen işlem hattı çalıştırmasını görürsünüz. Listeyi yenilemek için **Yenile** düğmesine tıklayın. **Eylemler** sütunundaki bağlantılar, işlem hattı çalıştırmasıyla ilişkili etkinlik çalıştırmalarını görüntülemenize ve işlem hattını yeniden çalıştırmanıza imkan tanır. 

    ![İşlem hattı çalıştırmaları](./media/tutorial-incremental-copy-multiple-tables-portal/pipeline-runs.png)
2. **Eylemler** sütunundaki **Etkinlik Çalıştırmalarını Görüntüle** bağlantısına tıklayın. Seçili işlem hattı çalıştırmasıyla ilişkili tüm etkinlik çalıştırmalarını görürsünüz. 

    ![Etkinlik çalıştırmaları](./media/tutorial-incremental-copy-multiple-tables-portal/activity-runs.png) 

## <a name="review-the-final-results"></a>Son sonuçları gözden geçirme
Güncelleştirilen/yeni verilerin kaynak tablolardan hedef tablolara kopyalandığını doğrulamak için, SQL Server Management Studio’da veritabanında aşağıdaki sorguları çalıştırın. 

**Sorgu** 
```sql
select * from customer_table
```

**Çıktı**
```
===========================================
PersonID    Name    LastModifytime
===========================================
1           John    2017-09-01 00:56:00.000
2           Mike    2017-09-02 05:23:00.000
3           NewName 2017-09-08 00:00:00.000
4           Andy    2017-09-04 03:21:00.000
5           Anny    2017-09-05 08:06:00.000
```

3 numaraya ait **PersonID** için yeni **Name** ve **LastModifytime** değerlerine dikkat edin. 

**Sorgu**

```sql
select * from project_table
```

**Çıktı**

```
===================================
Project     Creationtime
===================================
project1    2015-01-01 00:00:00.000
project2    2016-02-02 01:23:00.000
project3    2017-03-04 05:16:00.000
NewProject  2017-10-01 00:00:00.000
```

**NewProject** girişinin project_table tablosuna eklendiğine dikkat edin. 

**Sorgu**

```sql
select * from watermarktable
```

**Çıktı**

```
======================================
TableName       WatermarkValue
======================================
customer_table  2017-09-08 00:00:00.000
project_table   2017-10-01 00:00:00.000
```

Her iki tablonun da eşik değerlerinin güncelleştirildiğine dikkat edin.
     
## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide aşağıdaki adımları gerçekleştirdiniz: 

> [!div class="checklist"]
> * Kaynak ve hedef veri depolarını hazırlayın.
> * Veri fabrikası oluşturma.
> * Şirket içinde barındırılan tümleştirme çalışma (IR) zamanı oluşturun.
> * Tümleştirme çalışma zamanını yükleyin.
> * Bağlı hizmet oluşturma. 
> * Kaynak, havuz ve eşik veri kümeleri oluşturun.
> * İşlem hattını oluşturma, çalıştırma ve izleme.
> * Sonuçları gözden geçirin.
> * Kaynak tablolarına veri ekleyin veya bu verileri güncelleştirin.
> * İşlem hattını yeniden çalıştırın ve izleyin.
> * Son sonuçları gözden geçirin.

Azure üzerinde bir Spark kümesi kullanarak veri dönüştürme hakkında bilgi edinmek için aşağıdaki öğreticiye geçin:

> [!div class="nextstepaction"]
>[Değişiklik İzleme teknolojisini kullanarak Azure SQL Veritabanından Azure Blob depolama alanına verileri artımlı olarak yükleme](tutorial-incremental-copy-change-tracking-feature-portal.md)


