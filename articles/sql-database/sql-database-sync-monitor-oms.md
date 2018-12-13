---
title: Azure SQL Data Sync'i Log Analytics ile izleme | Microsoft Docs
description: Log Analytics kullanarak Azure SQL Data Sync izleme hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: data sync
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: douglasl
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: d6a1f2829352e8589c8d132a195a425edcbe62c3
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52872958"
---
# <a name="monitor-sql-data-sync-with-log-analytics"></a>SQL Data Sync'i Log Analytics ile izleme 

SQL Data Sync Etkinlik günlüğünü denetleyin ve hataları ve Uyarıları algılamak için daha önce SQL Data Sync, Azure portalında el ile iade etmeniz veya PowerShell veya REST API'sini kullanmanız gerekiyordu. Data Sync izleme deneyimini geliştiren özel bir çözümü yapılandırmak için bu makaledeki adımları izleyin. Bu çözüm, senaryonuza uyacak şekilde özelleştirebilirsiniz.

SQL Data Sync hizmetine genel bakış için bkz. [Azure SQL Data Sync ile birden fazla bulut ve şirket içi veritabanı arasında veri eşitleme](sql-database-sync-data.md).

## <a name="monitoring-dashboard-for-all-your-sync-groups"></a>Tüm eşitleme grupları için izleme Panosu 

Artık, tek tek sorunlar için aramak için her eşitleme grubunun günlükleri göz gerekmez. Özel bir Log Analytics görünümünü kullanarak tek bir yerde aboneliklerinizin herhangi birinden gelen tüm eşitleme gruplarını izleyebilirsiniz. Bu görünüm, SQL Data Sync müşteriler için önemli bilgilerin ortaya çıkarır.

![Veri Eşitleme izleme Panosu](media/sql-database-sync-monitor-oms/sync-monitoring-dashboard.png)

## <a name="automated-email-notifications"></a>Otomatik e-posta bildirimleri

Artık Azure portalında el ile veya PowerShell veya REST API aracılığıyla günlüğünü denetlemek gerekir. İle [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview), hata oluştuğunda görmeniz gereken kişilerin e-posta adreslerini doğrudan Git uyarılar oluşturabilirsiniz.

![Veri Eşitleme e-posta bildirimleri](media/sql-database-sync-monitor-oms/sync-email-notifications.png)

## <a name="how-do-you-set-up-these-monitoring-features"></a>Bu izleme özellikleri nasıl ayarlayabilirim? 

Özel bir Log Analytics çözümü, aşağıdaki işlemleri yaparak bir saatten az SQL Data Sync için izleme uygulayın:

Üç bileşeni yapılandırma yapmanız gerekir:

-   SQL Data Sync günlük verileri Log Analytics'e akışı için bir PowerShell runbook.

-   E-posta bildirimleri için bir Log Analytics uyarısı.

-   Bir Log Analytics izleme görünümü.

### <a name="samples-to-download"></a>Örnekleri indir

Aşağıdaki iki örnekleri indirin:

-   [Veri Eşitleme günlük PowerShell Runbook'u](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogPowerShellRunbook.ps1)

-   [Veri Eşitleme Log Analytics görünümü](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogOmsView.omsview)

### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki işlemleri ayarladığınızdan emin olun:

-   Azure Otomasyonu hesabı

-   Log Analytics Çalışma Alanı

## <a name="powershell-runbook-to-get-sql-data-sync-log"></a>SQL veri eşitleme günlüğü almak için PowerShell Runbook'u 

SQL Data Sync'i log veri çekme ve Log Analytics'e göndermek için Azure Otomasyonu'nda barındırılan bir PowerShell runbook'ı kullanın. Örnek bir betik dahil edilir. Bir önkoşul olarak, Azure Otomasyonu hesabı olması gerekir. Ardından, bir runbook oluşturmak ve çalıştırmak için zamanlamak gerekir. 

### <a name="create-a-runbook"></a>Runbook oluşturma

Bir runbook oluşturma hakkında daha fazla bilgi için bkz. [ilk PowerShell runbook'um](https://docs.microsoft.com/azure/automation/automation-first-runbook-textual-powershell).

1.  Azure Otomasyonu hesabınızı altında seçin **runbook'ları** süreç otomasyonu sekmesinde.

2.  Seçin **Runbook Ekle** runbook'ları sayfanın sol üst köşesindeki konumunda.

3.  Seçin **mevcut bir Runbook'u içeri aktar**.

4.  Altında **Runbook dosyası**, kullanın verilen `DataSyncLogPowerShellRunbook` dosya. Ayarlama **Runbook türü** olarak `PowerShell`. Runbook bir ad verin.

5.  **Oluştur**’u seçin. Artık bir runbook var.

6.  Azure Otomasyonu hesabı altında seçin **değişkenleri** sekmesi altında paylaşılan kaynakları.

7.  Seçin **değişken Ekle** değişkenleri sayfasında. Runbook için son yürütme zamanına depolamak için bir değişken oluşturun. Birden çok runbook varsa, her runbook için bir değişken gerekir.

8.  Değişken adı olarak ayarlamak `DataSyncLogLastUpdatedTime` türünü tarih olarak ayarlayın.

9.  Runbook'u seçin ve sayfanın üstündeki Düzenle düğmesine tıklayın.

10. Hesabınızı ve SQL Data Sync yapılandırmanız için gerekli değişiklikleri yapın. (Daha ayrıntılı bilgi için örnek komut dosyasına bakın.)

    1.  Azure Information.

    2.  Eşitleme grubu bilgileri.

    3.  Analytics bilgileri günlüğe kaydet. Azure Portalı'nda bu bilgi | Ayarları | Bağlı kaynaklar. Verileri Log Analytics'e gönderme hakkında daha fazla bilgi için bkz. [HTTP veri toplayıcı API'sini (Önizleme) ile Log Analytics veri Gönder](../azure-monitor/platform/data-collector-api.md).

11. Runbook'u Test Bölmesi'nde çalıştırın. Başarılı olup olmadığını denetleyin.

    Hatalar varsa, en son PowerShell modülünün yüklü olduğundan emin olun. En son PowerShell modülünde yükleyebileceğiniz **modüller Galerisi** Otomasyon hesabınızdaki.

12. Tıklayın **yayımlama**

### <a name="schedule-the-runbook"></a>Runbook zamanlama

Runbook zamanlama için:

1.  Runbook'u altında seçin **zamanlamaları** sekmesi altında kaynakları.

2.  Seçin **zamanlama Ekle** zamanlamaları sayfasında.

3.  Seçin **bir zamanlamayı runbook'a bağlamak**.

4.  Seçin **yeni bir zamanlama oluşturun.**

5.  Ayarlama **yinelenme** yinelenen ve küme için istediğiniz zaman aralığını. Burada, aynı aralık, betik ve Log Analytics kullanın.

6.  **Oluştur**’u seçin.

### <a name="check-the-automation"></a>Otomasyon denetleyin

Otomasyon altında beklendiği gibi çalışıp çalışmadığını izlemek için **genel bakış** automation hesabınız için **iş istatistikleri** görünümüne **izleme**. Bu görünüm kolay görüntüleme için panonuza sabitleyin. Başarılı çalıştırmalar runbook Göster "Tamamlandı" olarak ve başarısız çalıştırmaları "Başarısız" Göster

## <a name="create-a-log-analytics-reader-alert-for-email-notifications"></a>E-posta bildirimleri için bir Log Analytics okuyucusu uyarısı oluşturma

Log Analytics kullanan bir uyarı oluşturmak için aşağıdaki işlemleri yapabilirsiniz. Bir önkoşul olarak Log Analytics, Log Analytics çalışma alanı ile bağlantılı olması gerekir.

1.  Azure portalında **günlük araması**.

2.  Hataları ve Uyarıları eşitleme grubu tarafından seçmiş aralıkta seçmek için bir sorgu oluşturun. Örneğin:

    `Type=DataSyncLog\_CL LogLevel\_s!=Success| measure count() by SyncGroupName\_s interval 60minute`

3.  Sorgu çalıştırdıktan sonra diyor zil seçin **uyarı**.

4.  Altında **uyarı Oluştur temel alarak**seçin **ölçüm ölçüsü**.

    1.  Toplam değer kümesine **büyüktür**.

    2.  Sonra **büyüktür**, bildirimleri almadan önce geçecek eşiği girin. Geçici hatalar veri eşitlemede beklenmektedir. Gürültüsünü azaltmak için eşiği 5 olarak ayarlayın.

5.  Altında **eylemleri**ayarlayın **e-posta bildirimi** "Yes" İstenen bir e-posta alıcılarını girin.

6.  **Kaydet**’e tıklayın. Hatalar oluştuğunda belirtilen alıcılara e-posta bildirimleri artık alırsınız.

## <a name="create-a-log-analytics-view-for-monitoring"></a>İzleme için bir Log Analytics görünümü oluşturma

Bu adım, tüm belirtilen eşitleme gruplarını görsel olarak izlemek için Log Analytics görünümünü oluşturur. Görünüm çeşitli bileşenleri içerir:

-   Tüm eşitleme gruplarını kaç hatalar, başarılar ve uyarılar sahip gösteren bir genel bakış kutucuğu.

-   Hataları ve Uyarıları eşitleme grubu başına sayısını gösteren bir kutucuk için tüm eşitleme gruplarını. Bu kutucuğa sorun gruplarıyla görünmez.

-   Her grup için grubu Eşitleme hataları, başarı ve uyarıları ve en son hata iletileri sayısını gösteren bir kutucuk.

Log Analytics görünümü yapılandırmak için şunları yapın:

1.  Log Analytics'e giriş sayfasında, artı açmak için sol taraftaki seçin **Görünüm Tasarımcısı**.

2.  Seçin **alma** Görünüm Tasarımcısı'nın üst çubukta. Ardından "DataSyncLogOMSView" örnek dosyasını seçin.

3.  Örnek görünümü, iki eşitleme grubu yönetmek için kullanılır. Bu görünüm kendi senaryonuza uyacak şekilde düzenleyin. Tıklayın **Düzenle** ve aşağıdaki değişiklikleri yapın:

    1.  Yeni bir "& Listeyi halka" nesneler Galeriden gerektiği şekilde oluşturun.

    2.  Her kutucukta sorguları bilgilerinizi güncelleştirin.

        1.  Her bir kutucuğundaki TimeStamp_t aralığı istediğiniz gibi değiştirin.

        2.  Her bir eşitleme grubu için döşeme eşitleme grubu adlarını güncelleştirin.

    3.  Her bir kutucuğundaki başlığı gerektiği gibi güncelleştirin.

4.  Tıklayın **Kaydet** görünümü kullanıma hazırdır.

## <a name="cost-of-this-solution"></a>Bu çözümün maliyeti

Çoğu durumda, bu çözümü ücretsiz olarak kullanılabilir.

**Azure Otomasyonu:** kullanımınıza bağlı olarak bir Azure Otomasyonu hesabı ile gerçekleştirilen bir maliyeti olabilir. İlk 500 dakikalık iş çalıştırma zamanı aylık ücretsizdir. Çoğu durumda, bu çözüm başına aylık 500 dakikadan kısa bir sürede kullanmak için bekleniyor. Ücretlerden kaçınmak için iki saat veya daha fazla aralıklarla çalıştırılmak üzere bir runbook'u zamanlayın. Daha fazla bilgi için bkz. [Otomasyon fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/).

**Log Analytics:** kullanımınıza bağlı Log Analytics ile ilişkili bir maliyeti olabilir. Ücretsiz katmanda günlük içe alınan veri 500 MB içerir. Çoğu durumda, bu çözüm, günde 500 MB alma beklenir. Kullanımını azaltmak için yalnızca hata runbook'ta dahil filtrelemeyi kullanın. 500 MB günlük kullanıyorsanız, sınırlama ulaşıldığında durdurma analytics riskini önlemek için ücretli katmana yükseltin. Daha fazla bilgi için bkz. [Log Analytics fiyatlandırma](https://azure.microsoft.com/pricing/details/log-analytics/).

## <a name="code-samples"></a>Kod örnekleri

Bu makalede aşağıdaki konumlardan açıklanan kod örnekleri indirin:

-   [Veri Eşitleme günlük PowerShell Runbook'u](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogPowerShellRunbook.ps1)

-   [Veri Eşitleme Log Analytics görünümü](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/DataSyncLogOmsView.omsview)

## <a name="next-steps"></a>Sonraki adımlar
SQL Data Sync hakkında daha fazla bilgi için bkz.:

-   Genel Bakış - [verileri Eşitle birden fazla Bulut ve şirket içi veritabanı arasında Azure SQL Data Sync ile](sql-database-sync-data.md)
-   Data Sync'i Ayarla
    - Portalda - [öğretici: Azure SQL veritabanı ve SQL Server arasında verileri eşitlemek amacıyla şirket içi SQL Data Sync'i Ayarla](sql-database-get-started-sql-data-sync.md)
    - PowerShell ile
        -  [PowerShell kullanarak birden çok Azure SQL veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-sql-databases.md)
        -  [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md)
-   Veri Eşitleme Aracısı - [veri Aracısı Azure SQL Data Sync için eşitleme](sql-database-data-sync-agent.md)
-   En iyi uygulamalar - [en iyi uygulamalar için Azure SQL Data Sync](sql-database-best-practices-data-sync.md)
-   Sorun giderme - [Azure SQL Data Sync ile ilgili sorunları giderme](sql-database-troubleshoot-data-sync.md)
-   Eşitleme şemasını güncelleştirmek
    -   Transact-SQL ile- [Azure SQL Data Sync şema değişikliklerinin çoğaltmayı otomatik hale getirme](sql-database-update-sync-schema.md)
    -   PowerShell ile- [var olan bir eşitleme grubunda eşitleme şemasını güncelleştirmek için PowerShell kullanma](scripts/sql-database-sync-update-schema.md)

SQL Veritabanı hakkında daha fazla bilgi için bkz.:

-   [SQL Veritabanı'na Genel Bakış](sql-database-technical-overview.md)
-   [Veritabanı Yaşam Döngüsü Yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
