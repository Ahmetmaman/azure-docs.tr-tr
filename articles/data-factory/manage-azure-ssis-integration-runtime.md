---
title: Azure-SSIS tümleştirme çalışma zamanı yeniden | Microsoft Docs
description: Azure Data factory'de bir Azure-SSIS tümleştirme çalışma zamanı zaten sağlanmış sonra yeniden öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/22/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 7d4accf7958b93addd1b6d99e2d40a447c32dba9
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956600"
---
# <a name="reconfigure-the-azure-ssis-integration-runtime"></a>Azure-SSIS tümleştirme çalışma zamanı yeniden yapılandırın
Bu makalede, mevcut bir Azure-SSIS tümleştirme çalışma zamanı yapılandırılacağını açıklar. Azure Data Factory'de bir Azure-SSIS tümleştirme çalışma zamanı (IR) oluşturmak için bkz [bir Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md).  

## <a name="data-factory-ui"></a>Data Factory Kullanıcı Arabirimi (UI) 
Data Factory kullanıcı Arabirimi, Durdur, Düzenle/yeniden yapılandırın veya bir Azure-SSIS IR'yi silmek için kullanabilirsiniz 

1. İçinde **Data Factory kullanıcı arabirimini**, geçiş **Düzenle** sekmesi. Data Factory kullanıcı arabirimini başlatmak için tıklayın **yazar ve İzleyici** veri fabrikanızın giriş sayfasında.
2. Sol bölmede **bağlantıları**.
3. Sağ bölmede, geçiş **tümleştirme çalışma zamanları**. 
4. Eylemler sütunundaki düğmelerini kullanabilirsiniz **Durdur**, **Düzenle**, veya **Sil** tümleştirme çalışma zamanı. **Kod** düğmesine **eylemleri** sütun, tümleştirme çalışma zamanı ile ilişkilendirilen JSON tanımı görüntülemenizi sağlar.  
    
    ![Azure SSIS IR için Eylemler](./media/manage-azure-ssis-integration-runtime/actions-for-azure-ssis-ir.png)

### <a name="to-reconfigure-an-azure-ssis-ir"></a>Bir Azure-SSIS IR yeniden yapılandırmak için
1. Tümleştirme çalışma zamanının tıklayarak durdurun **Durdur** içinde **eylemleri** sütun. Liste görünümü yenilemek için şuna tıklayın **Yenile** araç. IR durdurulduktan sonra ilk eylemi IR başlatmanıza olanak tanır bakın 

    ![İçin durduruldu sonra Azure SSIS IR - Eylemler](./media/manage-azure-ssis-integration-runtime/actions-after-ssis-ir-stopped.png)
2. Düzenleme/tıklayarak RECONFIGURE IR **Düzenle** düğmesine **eylemleri** sütun. İçinde **tümleştirme çalışma zamanı Kurulumu** penceresinde (örneğin, düğümlerin veya düğüm başına en fazla Paralel yürütme sayısı, düğüm boyutu) ayarlarını değiştirin. 
3. IR yeniden başlatmak için **Başlat** düğmesine **eylemleri** sütun.     

## <a name="azure-powershell"></a>Azure PowerShell
Sağlama ve Azure-SSIS tümleştirme çalışma zamanı örneğini Başlat sonra bunu bir dizi çalıştırarak yeniden yapılandırabilirsiniz `Stop`  -  `Set`  -  `Start` PowerShell cmdlet öğelerini ardışık. Örneğin, aşağıdaki PowerShell betiğini beş Azure-SSIS tümleştirme çalışma zamanı örneği için ayrılmış düğüm sayısını değiştirir.

### <a name="reconfigure-an-azure-ssis-ir"></a>Bir Azure-SSIS IR yeniden yapılandırın

1. İlk olarak kullanarak Azure-SSIS tümleştirme çalışma zamanını durdurmak [Stop-AzureRmDataFactoryV2IntegrationRuntime](/powershell/module/azurerm.datafactoryv2/stop-azurermdatafactoryv2integrationruntime?view=azurermps-4.4.1) cmdlet'i. Bu komut, tüm alt düğümleri serbest bırakır ve faturalama durdurur.

    ```powershell
    Stop-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName 
    ```
2. Ardından, Azure-SSIS IR kullanarak yeniden [kümesi AzureRmDataFactoryV2IntegrationRuntime](/powershell/module/azurerm.datafactoryv2/set-azurermdatafactoryv2integrationruntime?view=azurermps-4.4.1) cmdlet'i. Aşağıdaki örnek komut, bir Azure-SSIS tümleştirme çalışma zamanı beş düğüm için çıkış ölçeklendirir.

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -NodeCount 5
    ```  
3. Ardından, Azure-SSIS tümleştirme çalışma zamanını kullanarak başlatın [başlangıç AzureRmDataFactoryV2IntegrationRuntime](/powershell/module/azurerm.datafactoryv2/start-azurermdatafactoryv2integrationruntime?view=azurermps-4.4.1) cmdlet'i. Bu komut tüm düğümlerinin SSIS paketlerini çalıştırmak için ayırır.   

    ```powershell
    Start-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName
    ```

### <a name="delete-an-azure-ssis-ir"></a>Bir Azure-SSIS IR Sil
1. İlk olarak, veri fabrikanızın altındaki tüm mevcut Azure SSIS Ir'ler listeleyin.

    ```powershell
    Get-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName -Status
    ```
2. Ardından, tüm mevcut Azure SSIS Ir'ler, data factory'de durdurun.

    ```powershell
    Stop-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    ```
3. Ardından, veri fabrikanızın tek tek tüm mevcut Azure SSIS Ir'ler kaldırın.

    ```powershell
    Remove-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    ```
4. Son olarak, veri fabrikanızın kaldırın.

    ```powershell
    Remove-AzureRmDataFactoryV2 -Name $DataFactoryName -ResourceGroupName $ResourceGroupName -Force
    ```
5. Yeni bir kaynak grubu oluşturduysanız, kaynak grubunu kaldırın.

    ```powershell
    Remove-AzureRmResourceGroup -Name $ResourceGroupName -Force 
    ```

## <a name="next-steps"></a>Sonraki adımlar
Azure-SSIS çalışma zamanı hakkında daha fazla bilgi için aşağıdaki konulara bakın: 

- [Azure-SSIS tümleştirme çalışma zamanı](concepts-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede Azure-SSIS IR'yi genel dahil tümleştirme çalışma zamanları hakkında kavramsal bilgiler sağlar 
- [Öğretici: SSIS paketlerini Azure’a dağıtma](tutorial-create-azure-ssis-runtime-portal.md). Bu makale bir Azure-SSIS IR oluşturmaya ilişkin adım adım yönergeler sağlar ve SSIS kataloğunu barındırmak için bir Azure SQL veritabanı kullanır. 
- [Nasıl yapılır: Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md). Bu makale öğreticiyi genişletip ve Azure SQL veritabanı yönetilen örneği kullanma ve IR'yi bir sanal ağa ekleme hakkında yönergeler sağlar. 
- [Azure-SSIS IR’yi bir sanal ağa ekleyin](join-azure-ssis-integration-runtime-virtual-network.md). Bu makale Azure-SSIS IR’yi bir Azure sanal ağına ekleme hakkında kavramsal bilgiler sağlar. Ayrıca, Azure portalını kullanarak Azure-SSIS IR’nin sanal ağa katılmasını sağlayacak şekilde sanal ağı yapılandırma adımlarını da sunar. 
- [Azure-SSIS IR’yi izleme](monitor-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede bir Azure-SSIS IR ile ilgili bilgileri ve döndürülen bilgilerdeki durumların açıklamalarını alma işlemi gösterilmektedir. 
 
