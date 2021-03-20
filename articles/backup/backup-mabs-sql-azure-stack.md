---
title: Azure Stack SQL Server iş yüklerini yedekleme
description: Bu makalede, Azure Stack SQL Server veritabanlarını korumak için Microsoft Azure Backup sunucusu (MABS) yapılandırma hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 06/08/2018
ms.openlocfilehash: 80de7913b010fca69c3703e423109f2ede653590
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "91332823"
---
# <a name="back-up-sql-server-on-azure-stack"></a>Azure Stack SQL Server yedekleme

Azure Stack SQL Server veritabanlarını korumak için Microsoft Azure Backup sunucusu (MABS) yapılandırmak için bu makaleyi kullanın.

Azure 'da SQL Server veritabanı yedeklemenin yönetimi ve Azure 'da kurtarma işlemleri üç adımdan oluşur:

1. SQL Server veritabanlarını korumak için bir yedekleme ilkesi oluşturma
2. İsteğe bağlı yedekleme kopyaları oluşturma
3. Veritabanını disklerden ve Azure 'dan kurtarma

## <a name="prerequisites-and-limitations"></a>Önkoşullar ve sınırlamalar

* Uzak bir dosya paylaşımında dosyalar içeren bir veritabanına sahipseniz koruma, Hata Kimliği 104 ile başarısız olur. MABS, uzak bir dosya paylaşımında SQL Server veri korumasını desteklemez.
* MABS, uzak SMB paylaşımlarında depolanan veritabanlarını koruyamaz.
* [Kullanılabilirlik grubu çoğaltmalarının salt okuma olarak yapılandırıldığından](/sql/database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server)emin olun.
* **NTAuthority\System adlı** sistem hesabını SQL Server üzerinde sysadmin grubuna açıkça eklemeniz gerekir.
* Kısmen kapsanan bir veritabanı için alternatif bir konum kurtarma gerçekleştirdiğinizde, hedef SQL örneğinde [Kapsanan veritabanları](/sql/relational-databases/databases/migrate-to-a-partially-contained-database#enable) özelliğinin etkin olduğundan emin olmanız gerekir.
* Bir dosya akışı veritabanı için alternatif bir konum kurtarma gerçekleştirdiğinizde, hedef SQL örneğinde [dosya akışı veritabanı](/sql/relational-databases/blob/enable-and-configure-filestream) özelliğinin etkinleştirildiğinden emin olmanız gerekir.
* SQL Server AlwaysOn için koruma:
  * MABS, koruma grubu oluşturma sırasında sorgu çalıştırırken kullanılabilirlik gruplarını algılar.
  * MABS bir yük devretme algılar ve veritabanını korumaya devam eder.
  * MABS, bir SQL Server örneği için çok siteli küme yapılandırmasını destekler.
* AlwaysOn özelliğini kullanan veritabanlarını koruduğunuzda, MABS 'ler aşağıdaki sınırlamalara sahiptir:
  * MABS, yedekleme tercihlerine göre SQL Server ayarlanan kullanılabilirlik grupları için yedekleme ilkesini aşağıdaki gibi kabul eder:
    * İkincil olanı tercih et - Birincil çoğaltmanın tek çoğaltma olması durumu haricinde yedeklemekler ikincil çoğaltmada gerçekleşmelidir. Kullanılabilir birden fazla ikincil çoğaltma varsa, yedekleme önceliği en yüksek olan düğüm yedekleme için seçilir. Yalnızca birincil çoğaltma kullanılabilirse, yedekleme birincil çoğaltmada gerçekleşmelidir.
    * Yalnızca ikincil - Birincil çoğaltmada yedekleme gerçekleştirilmemelidir. Birincil çoğaltma çevrimiçi olan tek çoğaltmaysa, yedekleme gerçekleşmemelidir.
    * Birincil - Yedeklemeler her zaman birincil çoğaltmada gerçekleşmelidir.
    * Herhangi Bir Çoğaltma - Kullanılabilirlik grubundaki kullanılabilir çoğaltmaların herhangi birinde yedekleme gerçekleşebilir. Yedeklemenin kaynağı olan düğüm, her düğümün yedekleme önceliklerine dayalı olarak belirlenir.
  * Şunlara dikkat edin:
    * Yedeklemeler, tüm okunabilir çoğaltmalardan (birincil, zaman uyumlu ikincil, zaman uyumsuz ikincil) meydana gelebilir.
    * Herhangi bir çoğaltma yedeklemeden dışlanmazsa, örneğin **çoğaltma dışlama** etkinse veya okunamaz olarak işaretlenmişse, bu çoğaltma, herhangi bir seçeneğin altında yedekleme için seçilmeyecek.
    * Kullanılabilir ve okunabilir durumda birden fazla çoğaltma varsa, yedekleme önceliği en yüksek olan düğüm yedekleme için seçilir.
    * Seçili düğümde yedekleme başarısız olursa, yedekleme işlemi başarısız olur.
    * Özgün konuma kurtarma desteklenmez.
* SQL Server 2014 veya üzeri yedekleme sorunları:
  * SQL Server 2014 [, Windows Azure Blob depolama alanında Şirket içi SQL Server için veritabanı](/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure)oluşturmak üzere yeni bir özellik ekledi. MABS bu yapılandırmayı korumak için kullanılamaz.
  * SQL AlwaysOn seçeneği için "ikincil tercih et" yedekleme tercihiyle ilgili bazı bilinen sorunlar vardır. MABS her zaman ikincili bir yedekleme alır. İkincil bulunamazsa yedekleme başarısız olur.

## <a name="before-you-start"></a>Başlamadan önce

[Azure Backup sunucusu yükleyip hazırlayın](backup-mabs-install-azure-stack.md).

## <a name="create-a-backup-policy-to-protect-sql-server-databases-to-azure"></a>SQL Server veritabanlarını Azure 'da korumak için bir yedekleme ilkesi oluşturma

1. Azure Backup Sunucusu Kullanıcı arabiriminde, **koruma** çalışma alanını seçin.

2. Araç **şeridinde yeni ' yi seçerek yeni** bir koruma grubu oluşturun.

    ![Koruma grubu oluştur](./media/backup-azure-backup-sql/protection-group.png)

    Azure Backup Sunucusu, koruma grubu Sihirbazı 'nı başlatır, bu da **koruma grubu** oluşturma konusunda size yol gösterir. **İleri**’yi seçin.

3. **Koruma grubu türünü seçin** ekranında **sunucular**' ı seçin.

    ![Koruma grubu türünü seçin-' sunucular '](./media/backup-azure-backup-sql/pg-servers.png)

4. **Grup üyelerini seçin** ekranında, kullanılabilir Üyeler listesinde çeşitli veri kaynakları görüntülenir. **+** Bir klasörü genişletmeyi ve alt klasörleri açığa çıkarmak için seçin. Bir öğe seçmek için onay kutusunu seçin.

    ![SQL DB 'yi seçin](./media/backup-azure-backup-sql/pg-databases.png)

    Seçili tüm öğeler seçili Üyeler listesinde görünür. Korumak istediğiniz sunucuları veya veritabanlarını seçtikten sonra **İleri**' yi seçin.

5. **Veri koruma yöntemini seçin** ekranında, koruma grubu için bir ad belirtin ve **çevrimiçi koruma** istiyorum onay kutusunu seçin.

    ![Veri koruma yöntemi-kısa süreli disk çevrimiçi & Azure](./media/backup-azure-backup-sql/pg-name.png)

6. **Short-Term hedeflerini belirtin** ekranında, diske yedekleme noktaları oluşturmak için gerekli girişleri ekleyin ve **İleri**' yi seçin.

    Bu örnekte, **bekletme aralığı** **5 gündür** **ve yedekleme** sıklığı, her **15 dakikada** bir yapılır. **Hızlı tam yedekleme** **8:00 P. M** olarak ayarlanır.

    ![Kısa vadeli hedefler](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > Gösterilen örnekte, değiştirilen verilerin önceki günün 8:00 PM Yedekleme noktasından aktarılması için her gün 8:00 PM 'de bir yedekleme noktası oluşturulur. Bu işlem **Hızlı tam yedekleme** olarak adlandırılır. İşlem günlükleri her 15 dakikada bir eşitlenir. Veritabanını 9:00 PM 'de kurtarmanız gerekirse, nokta son hızlı tam yedekleme noktasından (Bu örnekte 8PM) günlüklerden oluşturulur.
   >
   >

7. **Disk ayırmayı İncele** ekranında, kullanılabilir genel depolama alanını ve olası disk alanını doğrulayın. **İleri**’yi seçin.

8. **Çoğaltma oluşturma yöntemini seçin** kısmında ilk kurtarma noktanızı oluşturmayı seçin. Bant genişliği yığmasını veya ağ üzerinden bir kez daha kaçınmak için ilk yedeklemeyi el ile (ağ dışı) aktarabilirsiniz. İlk yedeklemenin aktarılmasını beklemek isterseniz, ilk aktarım için saati belirtebilirsiniz. **İleri**’yi seçin.

    ![İlk çoğaltma yöntemi](./media/backup-azure-backup-sql/pg-manual.png)

    İlk yedekleme kopyası, tüm veri kaynağını (SQL Server veritabanı) üretim sunucusundan (SQL Server bilgisayar) Azure Backup Sunucusu 'e aktarmayı gerektirir. Bu veriler büyük olabilir ve verilerin ağ üzerinden aktarılması bant genişliğini aşabilir. Bu nedenle, ilk yedeklemeyi aktarmayı seçebilirsiniz: bant genişliği tıkanıklığını önlemek için **el ile** (çıkarılabilir medya kullanarak) veya **ağ üzerinden otomatik olarak** (belirli bir zamanda).

    İlk yedekleme tamamlandıktan sonra, yedeklemelerin geri kalanı ilk yedekleme kopyasında artımlı yedeklemelerdir. Artımlı yedeklemeler küçük olma eğilimindedir ve ağ üzerinden kolayca aktarılır.

9. Tutarlılık denetiminin ne zaman çalıştırılmasını istediğinizi seçin ve **İleri ' yi** seçin.

    ![Tutarlılık denetimi](./media/backup-azure-backup-sql/pg-consistent.png)

    Azure Backup Sunucusu, yedekleme noktasının bütünlüğü üzerinde bir tutarlılık denetimi gerçekleştirir. Azure Backup Sunucusu, üretim sunucusundaki (Bu senaryodaki SQL Server bilgisayar) yedekleme dosyasının sağlama toplamını ve bu dosya için yedeklenmiş verileri hesaplar. Bir çakışma varsa, Azure Backup Sunucusu yedeklenen dosyanın bozuk olduğu varsayılır. Azure Backup Sunucusu, sağlama toplamı uyuşmazlığına karşılık gelen blokları göndererek yedeklenen verileri yeniden toplayın. Tutarlılık denetimleri performans açısından yoğun olduğundan, tutarlılık denetimini zamanlayabilir veya otomatik olarak çalıştırabilirsiniz.

10. Veri kaynaklarının çevrimiçi korumasını belirtmek için, Azure 'da korunacak veritabanlarını seçin ve **İleri**' yi seçin.

    ![Veri kaynaklarını seçin](./media/backup-azure-backup-sql/pg-sqldatabases.png)

11. Kuruluş ilkelerine uygun yedekleme zamanlamaları ve bekletme ilkeleri ' ni seçin.

    ![Zamanlama ve bekletme](./media/backup-azure-backup-sql/pg-schedule.png)

    Bu örnekte, yedeklemeler günde 12:00 PM ve 8 PM (ekranın alt kısmı) tarihinde bir kez alınır

    > [!NOTE]
    > Hızlı kurtarma için diskte birkaç kısa süreli kurtarma noktasına sahip olmak iyi bir uygulamadır. Bu kurtarma noktaları, işletimsel kurtarma için kullanılır. Azure, daha yüksek SLA 'Lar ve garantili kullanılabilirliğiyle iyi bir site dışı konum işlevi görür.
    >
    >

    **En Iyi Yöntem**: Azure 'a yedeklemeleri yerel disk yedeklemeleri tamamlandıktan sonra başlatılacak şekilde zamanladıysanız, en son Disk yedeklemeleri her zaman Azure 'a kopyalanır.

12. Bekletme ilkesi zamanlaması ' nı seçin. Bekletme ilkesinin nasıl çalıştığı hakkındaki ayrıntılar, [bant altyapısı makalenizi değiştirmek için Azure Backup kullanılıyor](backup-azure-backup-cloud-as-tape.md)olarak sunulmaktadır.

    ![Bekletme İlkesi](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    Bu örnekte:

    * Yedeklemeler günde bir kez 12:00 PM ve 8 PM (ekranın alt kısmı) ile gerçekleştirilir ve 180 gün boyunca korunur.
    * 12:00 P.M. ' de Cumartesi Cumartesi günü 104 hafta boyunca korunur
    * Son Cumartesi günü 12:00 P.M. ' de yedekleme 60 ay boyunca tutulur
    * Mart 'ın son Cumartesi günü 12:00 P.M. 10 yıl boyunca korunur
13. **İleri ' yi** seçin ve ilk yedekleme kopyasının Azure 'a aktarılması için uygun seçeneği belirleyin. **Ağ üzerinden otomatik olarak** seçim yapabilirsiniz

14. **Özet** ekranında ilke ayrıntılarını gözden geçirdikten sonra, iş akışını gerçekleştirmek Için **Grup Oluştur** ' u seçin. **Kapat** ' ı seçebilir ve izleme çalışma alanında işin ilerlemesini izleyebilirsiniz.

    ![Koruma grubu oluşturma In-Progress](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="on-demand-backup-of-a-sql-server-database"></a>SQL Server veritabanının isteğe bağlı yedeklemesi

Önceki adımlar bir yedekleme ilkesi oluştururken, yalnızca ilk yedekleme gerçekleştiğinde bir "kurtarma noktası" oluşturulur. Aşağıdaki adımlar, Scheduler 'ın başlamasını beklemek yerine, bir kurtarma noktasının el ile oluşturulmasını tetikler.

1. Kurtarma noktasını oluşturmadan önce koruma grubu durumunun veritabanı için **Tamam 'ı** gösterip gösterene kadar bekleyin.

    ![Koruma grubu üyeleri](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
2. Veritabanına sağ tıklayın ve **Kurtarma noktası oluştur**' u seçin.

    ![Çevrimiçi kurtarma noktası oluştur](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
3. Açılan menüden **çevrimiçi koruma** ' yı seçin ve Azure 'da bir kurtarma noktası oluşturmaya başlamak için **Tamam** ' ı seçin.

    ![Kurtarma noktası oluştur](./media/backup-azure-backup-sql/sqlbackup-azure.png)
4. **İzleme** çalışma alanında işin ilerlemesini görüntüleyin.

    ![İzleme konsolu](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>Azure 'dan SQL Server veritabanını kurtarma

Korunan bir varlığı (SQL Server veritabanı) Azure 'dan kurtarmak için aşağıdaki adımlar gereklidir.

1. Azure Backup Sunucusu Yönetim konsolunu açın. Korunan sunucuları görebileceğiniz **Kurtarma** çalışma alanı ' na gidin. Gerekli veritabanına (Bu durumda ReportServer $ MSDPM2012) gözatamazsınız. **Çevrimiçi** nokta olarak belirtilen zamanda bir **Kurtarma** seçin.

    ![Kurtarma noktası seçin](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
2. Veritabanı adına sağ tıklayın ve **kurtar**' ı seçin.

    ![Azure 'dan kurtarma](./media/backup-azure-backup-sql/sqlbackup-recover.png)
3. MABS, kurtarma noktasının ayrıntılarını gösterir. **İleri**’yi seçin. Veritabanının üzerine yazmak için, **SQL Server özgün örneğine kurtar** kurtarma türünü seçin. **İleri**’yi seçin.

    ![Özgün konuma kurtar](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    Bu örnekte, MABS veritabanını başka bir SQL Server örneğine veya tek başına bir ağ klasörüne kurtarır.

4. **Kurtarma seçeneklerini belirtin** ekranında, kurtarma tarafından kullanılan bant genişliğini azaltmak için ağ bant genişliği kullanımı azaltma gibi kurtarma seçeneklerini belirleyebilirsiniz. **İleri**’yi seçin.

5. **Özet** ekranında, şu ana kadar sunulan tüm kurtarma yapılandırmasını görürsünüz. **Kurtar**' ı seçin.

    Kurtarma durumu kurtarılan veritabanını gösterir. Sihirbazı kapatmak için **Kapat** ' ı seçebilirsiniz ve ilerlemeyi **izleme** çalışma alanında görüntüleyebilirsiniz.

    ![Kurtarma işlemini Başlat](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    Kurtarma işlemi tamamlandıktan sonra, geri yüklenen veritabanı uygulamayla tutarlı olur.

## <a name="next-steps"></a>Sonraki adımlar

Bkz. [yedekleme dosyaları ve uygulama](backup-mabs-files-applications-azure-stack.md) makalesi.
Bkz. [SharePoint 'Te yedekleme Azure Stack](backup-mabs-sharepoint-azure-stack.md) makalesi.
