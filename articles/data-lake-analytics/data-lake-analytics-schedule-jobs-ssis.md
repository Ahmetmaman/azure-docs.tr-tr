---
title: SSIS kullanarak Azure Data Lake Analytics U-SQL işleri zamanlama
description: Satır içi betik veya U-SQL sorgu dosyalarından U-SQL işlerini zamanlamak için SQL Server Integration Services nasıl kullanacağınızı öğrenin.
ms.reviewer: jasonh
ms.service: data-lake-analytics
ms.topic: how-to
ms.date: 07/17/2018
ms.openlocfilehash: b080b433f5af49e970faba02003fb68e21a08365
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "92221460"
---
# <a name="schedule-u-sql-jobs-using-sql-server-integration-services-ssis"></a>SQL Server Integration Services kullanarak U-SQL işlerini zamanlama (SSIS)

Bu belgede, SQL Server tümleştirme hizmeti 'ni (SSIS) kullanarak U-SQL işlerini nasıl düzenleyeceğinizi ve oluşturacağınızı öğreneceksiniz. 

## <a name="prerequisites"></a>Önkoşullar

[Tümleştirme Hizmetleri Için Azure Özellik paketi](/sql/integration-services/azure-feature-pack-for-integration-services-ssis#scenario-managing-data-in-the-cloud) , Azure Data Lake Analytics hizmetine bağlanmasına yardımcı olan [Azure Data Lake Analytics görevi](/sql/integration-services/control-flow/azure-data-lake-analytics-task) ve [Azure Data Lake Analytics bağlantı yöneticisini](/sql/integration-services/connection-manager/azure-data-lake-analytics-connection-manager) sağlar. Bu görevi kullanmak için, ' yi yüklediğinizden emin olun:

- [Visual Studio için SQL Server Veri Araçları indirin ve yükleyin (SSDT)](/sql/ssdt/download-sql-server-data-tools-ssdt)
- [Tümleştirme Hizmetleri için Azure Feature Pack 'i (SSIS) yükler](/sql/integration-services/azure-feature-pack-for-integration-services-ssis)

## <a name="azure-data-lake-analytics-task"></a>Azure Data Lake Analytics görev

Azure Data Lake Analytics görevi, kullanıcıların U-SQL işlerini Azure Data Lake Analytics hesabına göndermesine izin verir. 

[Azure Data Lake Analytics görevi yapılandırmayı öğrenin](/sql/integration-services/control-flow/azure-data-lake-analytics-task).

![SSIS 'de Azure Data Lake Analytics görevi](./media/data-lake-analytics-schedule-jobs-ssis/data-lake-analytics-azure-data-lake-analytics-task-in-ssis.png)

U-SQL betiğini farklı yerlerden, SSIS yerleşik işlevleri ve görevleri kullanarak alabilirsiniz. aşağıdaki senaryolar, farklı Kullanıcı durumları için U-SQL betiklerini nasıl yapılandırabilirim göstermektedir.

## <a name="scenario-1-use-inline-script-call-tvfs-and-stored-procs"></a>Senaryo 1-iç satır betiği kullan TVFs ve depolanan procs

Azure Data Lake Analytics görev Düzenleyicisi ' nde, **SourceType** öğesini **DirectInput** olarak yapılandırın ve U-SQL deyimlerini **ustointo** içine koyun.

Kolay bakım ve kod yönetimi için, yalnızca kısa U-SQL betiğini satır içi betikler olarak Yerleştir (örneğin, var olan tablo değerli işlevleri ve saklı yordamları U-SQL veritabanlarınızda çağırabilirsiniz). 

![SSIS görevinde satır içi U-SQL betiğini Düzenle](./media/data-lake-analytics-schedule-jobs-ssis/edit-inline-usql-script-in-ssis.png)

İlgili makale: [parametreleri saklı yordamlara geçirme](#scenario-6-pass-parameters-to-u-sql-script)

## <a name="scenario-2-use-u-sql-files-in-azure-data-lake-store"></a>Senaryo 2-Azure Data Lake Store içindeki U-SQL dosyalarını kullanma

Ayrıca, Azure Feature Pack 'te **Azure Data Lake Store dosya sistemi görevi** ' ni kullanarak Azure Data Lake Store U-SQL dosyalarını da kullanabilirsiniz. Bu yaklaşım, bulutta depolanan betikleri kullanmanıza olanak sağlar.

Azure Data Lake Store dosya sistemi görevi ve Azure Data Lake Analytics görevi arasındaki bağlantıyı ayarlamak için aşağıdaki adımları izleyin.

### <a name="set-task-control-flow"></a>Görev denetim akışını ayarla

SSIS paket tasarımı görünümünde, Foreach döngüsü kapsayıcısına bir **Azure Data Lake Store dosya sistemi görevi**, bir **foreach döngüsü kapsayıcısı** ve bir **Azure Data Lake Analytics görevi** ekleyin. Azure Data Lake Store dosya sistemi görevi, ADLS hesabınızdaki U-SQL dosyalarını geçici bir klasöre indirmeye yardımcı olur. Foreach döngüsü kapsayıcısı ve Azure Data Lake Analytics görevi, bir U-SQL işi olarak geçici klasörün altındaki her U-SQL dosyasını Azure Data Lake Analytics hesabına göndermeye yardımcı olur.

![Bir foreach döngüsü kapsayıcısına eklenen Azure Data Lake Store dosya sistemi görevinin gösterildiği diyagram.](./media/data-lake-analytics-schedule-jobs-ssis/use-u-sql-files-in-azure-data-lake-store.png)

### <a name="configure-azure-data-lake-store-file-system-task"></a>Azure Data Lake Store dosya sistemi görevini yapılandırma

1. **Işlemi** **Copyfromadls** olarak ayarlayın.
2. **AzureDataLakeConnection** kurma [Azure Data Lake Store bağlantı Yöneticisi](/sql/integration-services/connection-manager/azure-data-lake-store-connection-manager)hakkında daha fazla bilgi edinin.
3. **AzureDataLakeDirectory** ayarlayın. U-SQL komut dosyalarınızı depolayan klasöre işaret edin. Azure Data Lake Store hesabı kök klasörüyle ilişkili göreli yolu kullanın.
4. **Hedefi** , indirilen U-SQL betiklerini önbelleğe alan bir klasöre ayarlayın. Bu klasör yolu, U-SQL işi gönderimi için Foreach döngü kapsayıcısında kullanılacaktır. 

![Azure Data Lake Store dosya sistemi görevini yapılandırma](./media/data-lake-analytics-schedule-jobs-ssis/configure-azure-data-lake-store-file-system-task.png)

[Azure Data Lake Store dosya sistemi görevi hakkında daha fazla bilgi edinin](/sql/integration-services/control-flow/azure-data-lake-store-file-system-task).

### <a name="configure-foreach-loop-container"></a>Foreach döngüsü kapsayıcısını Yapılandır

1. **Koleksiyon** sayfasında, **Numaralandırıcı** öğesini **Foreach dosya numaralandırıcısı** olarak ayarlayın.

2. **Numaralandırıcı yapılandırma** grubu altındaki **klasörü** , indirilen U-SQL betiklerini içeren geçici klasöre ayarlayın.

3. Döngü kapsayıcısının yalnızca ile biten dosyaları **yakaladığı şekilde, Numaralandırıcı yapılandırması** altındaki **dosyaları** olarak ayarlayın `*.usql` `.usql` .

    !["Collection" seçili ve Numaralandırıcı ve Numaralandırıcı yapılandırma bölümlerinin vurgulandığı foreach döngüsü düzenleyicisini gösteren ekran görüntüsü.](./media/data-lake-analytics-schedule-jobs-ssis/configure-foreach-loop-container-collection.png)

4. **Değişken eşlemeleri** sayfasında, her bir U-SQL dosyası için dosya adını almak üzere Kullanıcı tanımlı bir değişken ekleyin. Dosya adını almak için **dizini** 0 olarak ayarlayın. Bu örnekte, adlı bir değişken tanımlayın `User::FileName` . Bu değişken, Azure Data Lake Analytics görevinde u-SQL betik dosyası bağlantısını dinamik olarak almak ve U-SQL iş adını ayarlamak için kullanılacaktır.

    ![Dosya adını almak için Foreach döngüsü kapsayıcısını yapılandırma](./media/data-lake-analytics-schedule-jobs-ssis/configure-foreach-loop-container-variable-mapping.png)

### <a name="configure-azure-data-lake-analytics-task"></a>Azure Data Lake Analytics görevi yapılandırma 

1. **SourceType** öğesini **FileConnection** olarak ayarlayın.

2. Dosya bağlantısına işaret eden, Foreach döngüsü kapsayıcısından döndürülen dosya nesnelerine işaret eden dosya bağlantısıyla **Fıleconnection** değerini ayarlayın.
    
    Bu dosya bağlantısını oluşturmak için:

   1. **\<New Connection...>** FileConnection ayarında öğesini seçin.
   2. **Kullanım türünü** **mevcut dosya** olarak ayarlayın ve **dosyayı** varolan herhangi bir dosyanın dosya yoluna ayarlayın.

       !["Kullanım türü" için "var olan dosya" seçiliyken dosya Bağlantı Yöneticisi düzenleyicisini gösteren ekran görüntüsü.](./media/data-lake-analytics-schedule-jobs-ssis/configure-file-connection-for-foreach-loop-container.png)

   3. **Bağlantı yöneticileri** görünümünde, hemen şimdi oluşturulan dosya bağlantısına sağ tıklayın ve **Özellikler**' i seçin.

   4. **Özellikler** penceresinde, **ifadeler**' i genişletin ve **ConnectionString** öğesini foreach döngüsü kapsayıcısında tanımlanan değişkene ayarlayın (örneğin,) `@[User::FileName]` .

       ![Foreach döngüsü kapsayıcısını Yapılandır](./media/data-lake-analytics-schedule-jobs-ssis/configure-file-connection-property-for-foreach-loop-container.png)

3. **AzureDataLakeAnalyticsConnection** ' i, işleri göndermek istediğiniz Azure Data Lake Analytics hesaba ayarlayın. [Azure Data Lake Analytics bağlantı Yöneticisi](/sql/integration-services/connection-manager/azure-data-lake-analytics-connection-manager)hakkında daha fazla bilgi edinin.

4. Diğer iş yapılandırmasını ayarlayın. [Daha Fazla Bilgi Edinin](/sql/integration-services/control-flow/azure-data-lake-analytics-task).

5. U-SQL iş adını dinamik olarak ayarlamak için **ifadeler** kullanın:

    1. **İfadeler** sayfasında, **JobName** için yeni bir anahtar-değer çifti ekleyin.
    2. JobName değerini Foreach döngü kapsayıcısında tanımlanan değişkene ayarlayın, örneğin, `@[User::FileName]` .
    
        ![U-SQL iş adı için SSIS Ifadesini yapılandırma](./media/data-lake-analytics-schedule-jobs-ssis/configure-expression-for-u-sql-job-name.png)

## <a name="scenario-3-use-u-sql-files-in-azure-blob-storage"></a>Senaryo 3-Azure Blob depolamada U-SQL dosyaları kullanma

Azure Feature Pack 'te Azure **BLOB Indirme görevini** kullanarak, Azure Blob depolamada U-SQL dosyalarını kullanabilirsiniz. Bu yaklaşım, bulutta betikleri kullanmanıza izin vermez.

Bu adımlar [Senaryo 2 ile benzerdir: Azure Data Lake Store Içindeki U-SQL dosyalarını kullanın](#scenario-2-use-u-sql-files-in-azure-data-lake-store). Azure Data Lake Store dosya sistemi görevini Azure Blob Indirme göreviyle değiştirin. [Azure Blob Indirme görevi hakkında daha fazla bilgi edinin](/sql/integration-services/control-flow/azure-blob-download-task).

Denetim akışı aşağıdaki gibidir.

![Azure Data Lake Store 'de U-SQL dosyaları kullanma](./media/data-lake-analytics-schedule-jobs-ssis/use-u-sql-files-in-azure-blob-storage.png)

## <a name="scenario-4-use-u-sql-files-on-the-local-machine"></a>Senaryo 4-yerel makinede U-SQL dosyalarını kullanma

Bulutta depolanan U-SQL dosyalarını kullanmanın yanı sıra, yerel makinenizde veya SSIS paketleriniz ile dağıtılan dosyalardaki dosyaları da kullanabilirsiniz.

1. SSIS projesinde **bağlantı yöneticileri** ' ne sağ tıklayın ve **Yeni bağlantı Yöneticisi**' ni seçin.

2. **Dosya** türünü seçin ve **Ekle...** öğesine tıklayın.

3. **Kullanım türünü** **mevcut dosya** olarak ayarlayın ve **dosyayı** yerel makinedeki dosyaya ayarlayın.

    ![Yerel dosyaya dosya bağlantısı ekle](./media/data-lake-analytics-schedule-jobs-ssis/configure-file-connection-for-foreach-loop-container.png)

4. **Azure Data Lake Analytics** görev ekleme ve:
    1. **SourceType** öğesini **FileConnection** olarak ayarlayın.
    2. Şimdi oluşturulan dosya bağlantısına **FileConnection** ayarlayın.

5. Azure Data Lake Analytics görev için diğer konfigürasyonları sona erdirin.

## <a name="scenario-5-use-u-sql-statement-in-ssis-variable"></a>Senaryo 5-SSIS değişkeninde U-SQL ifadesini kullanma

Bazı durumlarda, U-SQL deyimlerini dinamik olarak oluşturmanız gerekebilir. U-SQL deyimini dinamik olarak oluşturmanıza yardımcı olması için **SSIS ifadesi** ve betik görevi gıbı diğer SSIS görevleri ile **SSIS değişkenini** kullanabilirsiniz.

1. **Ssıs > Variables** üst düzey menü aracılığıyla değişkenler araç penceresini açın.

2. Bir SSIS değişkeni ekleyin ve değeri doğrudan ayarlayın ya da değeri oluşturmak için **ifadeyi** kullanın.

3. **Azure Data Lake Analytics görev** ekleme ve:
    1. **SourceType** **olarak ayarlayın**.
    2. **SourceVariable** öğesini ŞIMDI oluşturulan SSIS değişkenine ayarlayın.

4. Azure Data Lake Analytics görev için diğer konfigürasyonları sona erdirin.

## <a name="scenario-6-pass-parameters-to-u-sql-script"></a>Senaryo 6-parametreleri U-SQL betiğe geçir

Bazı durumlarda, U-SQL betiğinden U-SQL değişken değerini dinamik olarak ayarlamak isteyebilirsiniz. Azure Data Lake Analytics görevinde **parametre eşleme** özelliği bu senaryoyla ilgili yardım. Genellikle iki tipik Kullanıcı durumu vardır:

- Giriş ve çıkış dosyası yolu değişkenlerini geçerli tarih ve saate göre dinamik olarak ayarlayın.
- Saklı yordamlar için parametresini ayarlayın.

[U-SQL betiği için parametreleri ayarlama hakkında daha fazla bilgi edinin](/sql/integration-services/control-flow/azure-data-lake-analytics-task#parameter-mapping-page-configuration).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure’da SSIS paketlerini çalıştırma](../data-factory/how-to-invoke-ssis-package-ssis-activity.md)
- [Tümleştirme Hizmetleri için Azure Özellik paketi (SSIS)](/sql/integration-services/azure-feature-pack-for-integration-services-ssis#scenario-managing-data-in-the-cloud)
- [Azure Data Factory kullanarak U-SQL işlerini zamanlama](../data-factory/transform-data-using-data-lake-analytics.md)