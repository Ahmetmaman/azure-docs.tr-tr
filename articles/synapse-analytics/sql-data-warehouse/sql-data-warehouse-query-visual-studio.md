---
title: VSTS ile bağlanma
description: Azure SYNAPSE Analytics 'i Visual Studio ile sorgulayın.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 08/15/2019
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 59e0aed3ceda369909c6a4aecd03637a82a2dd1b
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93305379"
---
# <a name="connect-to-azure-synapse-analytics-with-visual-studio-and-ssdt"></a>Visual Studio ve SSDT ile Azure SYNAPSE Analytics 'e bağlanma

> [!div class="op_single_selector"]
> * [Azure Data Studio](../sql/get-started-azure-data-studio.md)
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](../sql/get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Yalnızca birkaç dakika içinde Azure SYNAPSE içinde adanmış bir SQL havuzunu sorgulamak için Visual Studio 'Yu kullanın. Bu yöntem, Visual Studio 2019 ' de SQL Server Veri Araçları (SSDT) uzantısını kullanır. 

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi kullanmak için şunlar gerekir:

* Mevcut bir adanmış SQL Havuzu. Bir tane oluşturmak için bkz. [adanmış BIR SQL havuzu oluşturma](create-data-warehouse-portal.md).
* Visual Studio için SSDT. Visual Studio kullanıyorsanız büyük olasılıkla Visual Studio için SSDT zaten var. Yükleme yönergeleri ve seçenekleri için bkz. [Visual Studio’yu ve SSDT’yi yükleme](sql-data-warehouse-install-visual-studio.md).
* Tam SQL server adı. Bu bilgileri bulmak için bkz. [adanmış BIR SQL havuzuna bağlanma](../sql/connect-overview.md).

## <a name="1-connect-to-your-dedicated-sql-pool"></a>1. adanmış SQL havuzunuza bağlanın
1. Visual Studio 2019 ' i açın.
2. SQL Server Nesne Gezgini SQL Server Nesne Gezgini **göster** ' i seçerek açın  >  **SQL Server Object Explorer**.
   
    ![SQL Server Nesne Gezgini](./media/sql-data-warehouse-query-visual-studio/open-ssdt.png)
3. **SQL Server ekle** simgesine tıklayın.
   
    ![SQL Server ekleme](./media/sql-data-warehouse-query-visual-studio/add-server.png)
4. Sunucuya Bağlan penceresindeki alanları doldurun.
   
    ![Sunucuya bağlanma](./media/sql-data-warehouse-query-visual-studio/connection-dialog.png)
   
   * **Sunucu adı**. Önceden tanımlanmış olan **sunucu adını** girin.
   * **Kimlik doğrulaması**. **SQL Server Kimlik Doğrulaması** 'nı veya **Active Directory Tümleşik Kimlik Doğrulaması** 'nı seçin.
   * **Kullanıcı Adı** ve **Parola**. Yukarıda SQL Server Kimlik Doğrulaması seçiliyse kullanıcı adını ve parolayı girin.
   * **Bağlan** 'a tıklayın.
5. Araştırmak için Azure SQL sunucunuzu genişletin. Sunucuyla ilişkili veritabanlarını görüntüleyebilirsiniz. Örnek veritabanınızdaki tabolaları görmek için AdventureWorksDW'yi genişletin.
   
    ![AdventureWorksDW'yi araştırma](./media/sql-data-warehouse-query-visual-studio/explore-sample.png)

## <a name="2-run-a-sample-query"></a>2. örnek bir sorgu çalıştırın
Artık veritabanınızla bağlantı kurulduğuna göre bir sorgu yazalım.

1. SQL Server Nesne Gezgini'nde veritabanınıza sağ tıklayın.
2. **Yeni Sorgu** ’yu seçin. Yeni bir sorgu penceresi açılır.
   
    ![Yeni sorgu](./media/sql-data-warehouse-query-visual-studio/new-query2.png)
3. Aşağıdaki T-SQL sorgusunu sorgu penceresine kopyalayın:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Yeşil oka tıklayarak sorguyu çalıştırın veya şu kısayolu kullanın: `CTRL` + `SHIFT` + `E` .
   
    ![Sorgu çalıştırma](./media/sql-data-warehouse-query-visual-studio/run-query.png)
5. Sorgu sonuçlarına bakın. Bu örnekte FactInternetSales tablosunda 60398 satır var.
   
    ![Sorgu sonuçları](./media/sql-data-warehouse-query-visual-studio/query-results.png)

## <a name="next-steps"></a>Sonraki adımlar
Artık bağlanıp sorgulama yaptığınızda, [Power BI verileri görselleştirmeyi](sql-data-warehouse-get-started-visualize-with-power-bi.md)deneyin.

Ortamınızı Azure Active Directory kimlik doğrulaması için yapılandırmak üzere bkz. [ADANMıŞ SQL havuzunda kimlik doğrulama](sql-data-warehouse-authentication.md).
