---
title: SQL veritabanı yük devretme için Azure-SSIS Integration Runtime'ı yapılandırma | Microsoft Docs
description: Bu makalede Azure SQL veritabanı coğrafi çoğaltma ve yük devretme için SSISDB veritabanının ile Azure-SSIS tümleştirme çalışma zamanını yapılandırma
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 08/14/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: dea0153b9ca6d8e751fd94cc558abd44b2591907
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57453040"
---
# <a name="configure-the-azure-ssis-integration-runtime-with-azure-sql-database-geo-replication-and-failover"></a>Azure SQL veritabanı coğrafi çoğaltma ve yük devretme ile Azure-SSIS tümleştirme çalışma zamanını yapılandırma

Bu makalede, Azure SQL veritabanı SSISDB veritabanı için coğrafi çoğaltma ile Azure-SSIS tümleştirme çalışma zamanı yapılandırma açıklanır. Bir yük devretme işlemi gerçekleştiğinde, Azure-SSIS IR ikincil veritabanı ile çalışmaya devam eder emin olabilirsiniz.

Coğrafi çoğaltma ve yük devretme için SQL veritabanı hakkında daha fazla bilgi için bkz. [genel bakış: Etkin coğrafi çoğaltma ve otomatik yük devretme grupları](../sql-database/sql-database-geo-replication-overview.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="scenario-1---azure-ssis-ir-is-pointing-to-read-write-listener-endpoint"></a>Senaryo 1 - Azure-SSIS IR işaret okuma-yazma dinleyici uç noktası

### <a name="conditions"></a>Koşullar

Bu bölüm, aşağıdaki koşullar doğru olduğunda geçerlidir:

- Azure-SSIS IR, yük devretme grubunun okuma / yazma dinleyici uç noktaya işaret ediyor.

  VE

- SQL veritabanı sunucusu *değil* sanal ağ hizmet uç noktası kuralı ile yapılandırılmış.

### <a name="solution"></a>Çözüm

Yük devretme gerçekleştiğinde Azure-SSIS IR için saydam Azure-SSIS IR, otomatik olarak yük devretme grubuna yeni birincil siteye bağlanır.

## <a name="scenario-2---azure-ssis-ir-is-pointing-to-primary-server-endpoint"></a>Senaryo 2 - Azure-SSIS IR birincil sunucu uç noktaya işaret

### <a name="conditions"></a>Koşullar

Bu bölüm, aşağıdaki koşullardan biri doğru olduğunda geçerlidir:

- Azure-SSIS IR, yük devretme grubunun birincil sunucu uç noktası işaret ediyor. Bu uç nokta yük devretme gerçekleştiğinde değiştirir.

  OR

- Azure SQL veritabanı sunucusu, sanal ağ hizmet uç noktası kuralı ile yapılandırılır.

  OR

- Veritabanı, SQL veritabanı yönetilen bir sanal ağ ile yapılandırılan örneği sunucusudur.

### <a name="solution"></a>Çözüm

Yük devretme işlemi gerçekleştiğinde, şunları yapmanız gerekir:

1. Azure-SSIS IR'yi durdurma

2. IR yeni bölgedeki bir sanal ağ ve yeni birincil uç noktaya işaret edecek şekilde yeniden yapılandırın.

3. IR yeniden başlatın

Aşağıdaki bölümlerde daha ayrıntılı adımları açıklanmaktadır.

### <a name="prerequisites"></a>Önkoşullar

- Sunucunun aynı anda bir kesinti olması durumunda, olağanüstü durum kurtarma için Azure SQL veritabanı sunucunuzun etkinleştirdiğinizden emin olun. Daha fazla bilgi için bkz. [Azure SQL veritabanı ile iş sürekliliğine genel bakış](../sql-database/sql-database-business-continuity.md).

- Geçerli bölgede bulunan bir sanal ağ'ı kullanıyorsanız, başka bir sanal ağ, Azure-SSIS tümleştirme çalışma zamanınızın bağlanmak için yeni bölgede kullanmanız gerekir. Daha fazla bilgi için bkz. [bir Azure-SSIS tümleştirme çalışma zamanını bir sanal ağa katılın](join-azure-ssis-integration-runtime-virtual-network.md).

- Özel kurulum kullanıyorsanız, bir kesinti sırasında erişilebilir olmaya devam eder, böylece özel kurulum betiğini ve ilişkili dosyalarını depolayan blob kapsayıcısı için başka bir SAS URI'si hazırlama gerekebilir. Daha fazla bilgi için bkz. [özel kurulum bir Azure-SSIS tümleştirme çalışma zamanını yapılandırma](how-to-configure-azure-ssis-ir-custom-setup.md).

### <a name="steps"></a>Adımlar

Azure-SSIS IR durdurmak, IR için yeni bir bölgeye geçiş ve yeniden başlatmak için aşağıdaki adımları izleyin.

1. Özgün bölgede IR durdurun.

2. IR yeni ayarlarla güncelleştirmek için PowerShell'de şu komuta çağrı yapın.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
                    -CatalogServerEndpoint "Azure SQL Database server endpoint" `
                    -CatalogAdminCredential "Azure SQL Database server admin credentials" `
                    -VNetId "new VNet" `
                    -Subnet "new subnet" `
                    -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

    Bu PowerShell komut hakkında daha fazla bilgi için bkz. [Azure Data Factory'de Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md)

3. IR yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Azure-SSIS IR için diğer yapılandırma seçenekleri göz önünde bulundurun:

- [Yüksek performans için Azure-SSIS tümleştirme çalışma zamanı yapılandırma](configure-azure-ssis-integration-runtime-performance.md)

- [Azure-SSIS tümleştirme çalışma zamanı Kurulum özelleştirme](how-to-configure-azure-ssis-ir-custom-setup.md)

- [Azure-SSIS tümleştirme çalışma zamanı için Enterprise Edition sağlama](how-to-configure-azure-ssis-ir-enterprise-edition.md)
