---
title: Bulut veritabanlarında dağıtılmış işlemler
description: Azure SQL veritabanı elastik veritabanı işlemleri'ne genel bakış
services: sql-database
ms.service: sql-database
ms.subservice: elastic-scale
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: 3147061f527621ba98dee84f4d347a6e883d61c0
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166478"
---
# <a name="distributed-transactions-across-cloud-databases"></a>Bulut veritabanlarında dağıtılmış işlemler
Elastik veritabanı işlemleri için Azure SQL veritabanı (SQL DB) SQL DB'de birkaç veritabanlarına yayılan işlemler çalıştırmanıza olanak tanır. SQL veritabanı için elastik veritabanı işlem ADO .NET kullanarak .NET uygulamaları için kullanılabilir ve tanıdık programlama deneyimi kullanarak ile tümleştirme [System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx) sınıfları. Kitaplık almak için bkz. [.NET Framework 4.6.1 (Web Yükleyicisi)](https://www.microsoft.com/download/details.aspx?id=49981).

Şirket içi Microsoft Dağıtılmış İşlem Düzenleyicisi (MSDTC) çalıştıran böyle bir senaryo genellikle gereklidir. MSDTC azure'da hizmet olarak Platform uygulaması için kullanılabilir olmadığından, dağıtılmış işlemler koordine özelliği artık SQL Veritabanına doğrudan tümleştirilmiştir. Uygulamalar, dağıtılmış işlemler başlatmak için bir SQL veritabanına bağlanabilir ve veritabanlarından birini şeffaf bir şekilde dağıtılmış işlem aşağıdaki şekilde gösterildiği gibi koordine. 

  ![Elastik veritabanı işlemleri kullanarak Azure SQL veritabanı ile dağıtılmış işlemler ][1]

## <a name="common-scenarios"></a>Genel senaryolar
SQL veritabanı için elastik veritabanı işlem uygulamaları birkaç farklı SQL veritabanlarında depolanan verileri atomik değişiklik yapmak etkinleştirin. C# ve .NET istemci tarafı geliştirme deneyimlerinde Önizleme odaklanır. T-SQL kullanarak sunucu tarafı deneyimi, daha sonraki bir zamana planlanmaktadır.  
Elastik veritabanı işlemleri, aşağıdaki senaryolarda hedefler:

* Azure'da çok veritabanı uygulamaları: farklı veri türleri farklı veritabanları üzerinde bulunan gibi bu senaryoyla veriler dikey olarak SQL DB çeşitli veritabanları arasında bölümlenirken. Bazı işlemler, iki veya daha fazla veritabanı içinde tutulan verilerde yapılan değişiklikleri gerektirir. Uygulama, elastik veritabanı işlem veritabanlarında değişikliklerini koordine etmek ve kararlılık emin olmak için kullanır.
* Azure uygulamalarında parçalı veritabanlarını: Bu senaryo ile veri katmanı kullanan [elastik veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md) veya yatay SQL veritabanında çok sayıda veritabanı arasında verileri bölümlemek için self-parçalama. Bir tanınmış bir kullanım örneği değişiklikleri kiracıya yayılabilir, parçalı çok kiracılı bir uygulama için atomik değişiklikleri gerçekleştirmek için gerekli değildir. Örneğin bir kiracıdaki bir aktarım hem bulunan farklı veritabanları üzerinde başka bir düşünün. Sırayla genellikle aynı Kiracı için kullanılan çeşitli veritabanları arasında esnetme bazı atomik işlemler gerektiği anlamına gelir, büyük bir kiracı için kapasite gereksinimlerini karşılamak için ayrıntılı parçalama ikinci durumdur. Veritabanları arasında çoğaltılan verilerini başvurmak için atomik güncelleştirmeleri buna üçüncü bir durumdur. Bu satırlar boyunca atomik, hizmetteki, işlemleri artık Önizleme kullanarak birçok veritabanı arasında Eşgüdümlü.
  Elastik veritabanı işlem iki aşamalı tamamlama işlem kararlılık veritabanlarında emin olmak için kullanın. Sadece tek bir işlem içinde her defasında 100 veritabanları gerektiren işlemler için uygun değil. Bu sınırlar zorlanmamış, ancak bir performans ve başarı oranları bu sınırlar aşıldığında yaşanmaya elastik veritabanı işlemleri için beklemeniz gerekir.

## <a name="installation-and-migration"></a>Yükleme ve geçiş
SQL veritabanı elastik veritabanı işlemleri için özellikler System.Data.dll ve System.Transactions.dll .NET kitaplıklarına güncelleştirmeler sağlanır. DLL'leri emin olun, iki aşamalı tamamlama kararlılık emin olmak gerektiğinde kullanılır. Elastik veritabanı işlemleri kullanarak uygulamalar geliştirmeye başlamak için Yükle [.NET Framework 4.6.1](https://www.microsoft.com/download/details.aspx?id=49981) veya sonraki bir sürümü. .NET framework'ün önceki bir sürümünde çalıştırıldığı sırada işlemler için Dağıtılmış bir işlem yükseltme başarısız olur ve bir özel durum oluşturulur.

Yükleme sonrasında, SQL DB bağlantılarla içinde System.Transactions API'leri dağıtılmış işlem kullanabilirsiniz. Bu API'leri kullanarak mevcut MSDTC uygulamalar varsa, yalnızca .NET 4.6 için mevcut uygulamalarınızı 4.6.1 yükledikten sonra yeniden Framework. Projelerinizi .NET 4.6 hedefliyorsanız, otomatik olarak yeni Framework sürümünün güncelleştirilmiş DLL'leri kullanır ve SQL DB bağlantılarla birlikte API'sini çağırır dağıtılmış işlem artık başarılı olur.

Elastik veritabanı işlem MSDTC yükleme gerektirmez unutmayın. Bunun yerine, elastik veritabanı işlemleri, doğrudan tarafından ve SQL DB içinde yönetilir. MSDTC dağıtımını dağıtılmış işlemler SQL DB ile kullanmak için gerekli olmadığından bu bulut senaryolarına önemli ölçüde basitleştirir. 4. Bölüm elastik veritabanı işlemleri ve bulut uygulamalarınıza Azure ile birlikte gerekli .NET framework dağıtma konusunda daha ayrıntılı olarak açıklanmaktadır.

## <a name="development-experience"></a>Geliştirme deneyimi
### <a name="multi-database-applications"></a>Birden çok veritabanı uygulamaları
Aşağıdaki örnek kod ile .NET System.Transactions tanıdık programlama deneyimi kullanır. . NET'te bir ortam işlem TransactionScope sınıfı oluşturur. (Bir "ortam" geçerli iş parçacığında yer alan bir işlemdir.) İşlemde tüm bağlantıları TransactionScope içinde açılır. Farklı veritabanlarındaki katılırsanız, işlem otomatik olarak dağıtılmış bir işlem için yükseltilmiş. İşlem sonucunu bir işleme belirtmek için tamamlamak için kapsamı ayarlama denetlenir.

    using (var scope = new TransactionScope())
    {
        using (var conn1 = new SqlConnection(connStrDb1))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = new SqlConnection(connStrDb2))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T2 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }

### <a name="sharded-database-applications"></a>Parçalı veritabanı uygulamaları
SQL veritabanı için elastik veritabanı işlemleri de destekler, kullandığınız OpenConnectionForKey yöntemi elastik veritabanı istemci Kitaplığı'nın ölçeği genişletilmiş veri bağlantılarını açmak için dağıtılmış işlemleri koordine katmanı. Birkaç farklı parçalama anahtarı değerleri arasında değişiklikler için işlem tutarlılığı güvence altına almak için gereken durumları göz önünde bulundurun. Farklı bir parçalama anahtarı değerleri barındırma parçalara bağlantıları OpenConnectionForKey kullanarak aracılı. Dağıtılmış bir işlem gerektiren işlem garantileri sağlama, genel durumda farklı parçalara bağlantıları olabilir. Aşağıdaki kod örneği, bu yaklaşımı gösterir. Bu, shardmap adında bir değişkene bir parça eşlemesi elastik veritabanı istemci Kitaplığı'ndan temsil etmek için kullanıldığını varsayar:

    using (var scope = new TransactionScope())
    {
        using (var conn1 = shardmap.OpenConnectionForKey(tenantId1, credentialsStr))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = shardmap.OpenConnectionForKey(tenantId2, credentialsStr))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T1 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }


## <a name="net-installation-for-azure-cloud-services"></a>Azure Cloud Services için .NET yükleme
Azure .NET uygulamalarını barındırmak için çeşitli teklifleri sağlar. Farklı teklifleri karşılaştırması kullanılabilir [Azure App Service, Cloud Services ve Virtual Machines karşılaştırması](../app-service/choose-web-site-cloud-service-vm.md). ' % S'konuk işletim sistemi teklifin .NET 4.6.1 elastik işlemler için gerekli daha küçükse 4.6.1 için konuk işletim sistemi yükseltme gerekir. 

Azure uygulama hizmetleri için konuk işletim sistemi yükseltmeleri şu anda desteklenmemektedir. Azure sanal makineleri için yalnızca VM'de oturum açın ve en son .NET framework için yükleyiciyi çalıştırın. Azure bulut Hizmetleri için başlangıç görevleri dağıtımınızın daha yeni bir .NET sürümü yüklemesine eklemeniz gerekir. Kavramlar ve adımlar bölümünde belgelendirilen [.NET bulut hizmeti rolündeki yükleme](../cloud-services/cloud-services-dotnet-install-dotnet.md).  

Yükleyici'den Azure bulut Hizmetleri .NET 4.6 için önyükleme işlemi sırasında yükleyiciyi .NET 4.6.1 için daha fazla geçici depolama alanı gerektirebilir unutmayın. Başarılı bir yükleme için Azure bulut hizmeti, ServiceDefinition.csdef dosyasında LocalResources bölümü ve ortam ayarlarına başlangıç göreviniz için geçici depolama alanını artırmak aşağıdaki örnekte gösterildiği gibi gerekir:

    <LocalResources>
    ...
        <LocalStorage name="TEMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
        <LocalStorage name="TMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
        <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
            <Environment>
        ...
                <Variable name="TEMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TEMP']/@path" />
                </Variable>
                <Variable name="TMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TMP']/@path" />
                </Variable>
            </Environment>
        </Task>
    </Startup>

## <a name="transactions-across-multiple-servers"></a>Birden fazla sunucuya işlemleri
Elastik veritabanı işlemleri, Azure SQL veritabanı'nda mantıksal farklı sunucular arasında desteklenir. İşlem mantıksal sunucu sınırları geçtiğinde katılan sunucular ilk bir karşılıklı iletişim ilişkisi girilmesi gerekir. İletişim ilişki kurulduktan sonra iki sunucuların herhangi bir veritabanında başka bir sunucudan veritabanları ile elastik işlemler katılabilir. İkiden fazla mantıksal sunucuları kapsayan işlemler ile bir iletişim ilişkisi çiftinde mantıksal sunucu için yerinde olması gerekir.

Elastik veritabanı işlemleri için çapraz-sunucu iletişimi ilişkileri yönetmek için aşağıdaki PowerShell cmdlet'lerini kullanın:

* **Yeni AzureRmSqlServerCommunicationLink**: Azure SQL DB'de iki mantıksal sunucuları arasında yeni bir iletişim ilişkisi oluşturmak için bu cmdlet'i kullanın. Her iki sunucuyu diğer sunucu işlemleri başlatabilirsiniz yani simetrik ilişkidir.
* **Get-AzureRmSqlServerCommunicationLink**: var olan iletişimi ilişkileri ve özellikleri almak için bu cmdlet'i kullanın.
* **Remove-AzureRmSqlServerCommunicationLink**: Use this cmdlet to remove an existing communication relationship. 

## <a name="monitoring-transaction-status"></a>İşlem durumunu izleme
Dinamik Yönetim görünümlerini (Dmv'ler) İzleyici durumunu ve ilerlemesini, devam eden bir elastik veritabanı işlem için SQL DB kullanın. SQL veritabanında dağıtılmış işlemler için işlem ilişkili tüm Dmv'leri ilgilidir. Karşılık gelen listesini Dmv'leri burada bulabilirsiniz: [işlem ilgili dinamik yönetimi görünümleri ve işlevleri (Transact-SQL)](https://msdn.microsoft.com/library/ms178621.aspx).

Bu Dmv'leri özellikle yararlı olur:

* **sys.DM\_tran\_etkin\_işlemleri**: geçerli durumda etkin işlemlerin ve durumlarını listeler. UOW (iş birimi) sütunu için aynı dağıtılmış işlem ait farklı alt işlemlerin tanımlayabilirsiniz. Tüm işlemler aynı dağıtılmış işlem içinde aynı UOW değeri getirir. Bkz: [DMV belgeleri](https://msdn.microsoft.com/library/ms174302.aspx) daha fazla bilgi için.
* **sys.DM\_tran\_veritabanı\_işlemleri**: işlem günlüğünde yerleştirme gibi işlemleri hakkında ek bilgi sağlar. Bkz: [DMV belgeleri](https://msdn.microsoft.com/library/ms186957.aspx) daha fazla bilgi için.
* **sys.DM\_tran\_kilitleri**: şu anda devam eden işlemler tarafından tutulan kilitleri hakkında bilgi sağlar. Bkz: [DMV belgeleri](https://msdn.microsoft.com/library/ms190345.aspx) daha fazla bilgi için.

## <a name="limitations"></a>Sınırlamalar
SQL DB, esnek veritabanı işlem şu anda aşağıdaki sınırlamalar geçerlidir:

* Yalnızca SQL DB'de veritabanlarında işlemler desteklenir. Diğer [X / açık XA](https://en.wikipedia.org/wiki/X/Open_XA) kaynak sağlayıcıları ve SQL DB dışında veritabanlarını esnek veritabanı işlemleri katılamaz. Elastik veritabanı işlem şirket içi SQL Server ve Azure SQL veritabanı arasında şekilde esnetemezsiniz anlamına gelir. Şirket içinde dağıtılmış işlemler için MSDTC kullanmaya devam edin. 
* Yalnızca istemci uyumlu işlemler bir .NET uygulamasından desteklenir. T-SQL BEGIN TRANSACTION dağıtılmış gibi sunucu tarafı desteği, planlanan ancak henüz kullanılamıyor. 
* WCF hizmetlerinde işlemler desteklenmez. Örneğin, bir işlem yürütür bir WCF hizmeti yöntemi vardır. Çağrısı bir işlem kapsam içinde kapsayan olarak yapılamayacak bir [System.ServiceModel.ProtocolException](https://msdn.microsoft.com/library/system.servicemodel.protocolexception).

## <a name="next-steps"></a>Sonraki adımlar
Sorularınız varsa lütfen bize ulaşın [SQL veritabanının Forumu](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) ve özellik istekleri için lütfen bunları Ekle [SQL veritabanı geri bildirim Forumu](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-transactions-overview/distributed-transactions.png



