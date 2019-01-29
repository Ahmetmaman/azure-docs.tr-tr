---
title: Azure SQL Veritabanı sorgulamak için Python kullanma | Microsoft Docs
description: Bu konu Python kullanarak Azure SQL veritabanına bağlanan bir program oluşturma ve Transact-SQL deyimlerini kullanarak nasıl kullanılacağını gösterir.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: python
ms.topic: quickstart
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 01/28/2019
ms.openlocfilehash: b611eb02203c872e3497b5b7c12acddd9eab14c0
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55188397"
---
# <a name="quickstart-use-python-to-query-an-azure-sql-database"></a>Hızlı Başlangıç: Python kullanarak Azure SQL veritabanı sorgulama

 Bu hızlı başlangıçta [Python](https://python.org) kullanarak bir Azure SQL veritabanına bağlanma ve Transact-SQL deyimleriyle veri sorgulama işlemleri gösterilir. Daha fazla SDK ayrıntıları için kullanıma sunduğumuz [başvuru](https://docs.microsoft.com/python/api/overview/azure/sql) belgeleri, [pyodbc GitHub deposu](https://github.com/mkleehammer/pyodbc/wiki/)ve [pyodbc örnek](https://github.com/mkleehammer/pyodbc/wiki/Getting-started).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakilere sahip olduğunuzdan emin olun:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]
  
- Python ve ilgili yazılımları işletim sisteminiz için:
  
  - **macOS**: Homebrew ve Python yükleyin, ODBC sürücüsünü ve SQLCMD yükleyin ve ardından SQL Server için Python sürücüsü yükleyin. 1.2, 1.3 ve 2.1 adımları görmek [macOS üzerinde SQL Server'ı kullanarak oluşturmak Python uygulamaları](https://www.microsoft.com/sql-server/developer-get-started/python/mac/). Daha fazla bilgi için [Linux ve macOS Microsoft ODBC sürücüsünü yükleme](https://docs.microsoft.com/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server).
    
  - **Ubuntu**: Python ve diğer gerekli paketleri yükleyin `sudo apt-get install python python-pip gcc g++ build-essential`. İndirin ve SQL Server için Python sürücüsü ODBC sürücüsünü ve SQLCMD yükleyin. Yönergeler için [pyodbc Python geliştirme için bir geliştirme ortamı yapılandırma](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#linux).
    
  - **Windows**: SQL Server için Python, ODBC sürücüsünü ve SQLCMD ve Python sürücüsü yükleyin. Yönergeler için [pyodbc Python geliştirme için bir geliştirme ortamı yapılandırma](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#windows).

## <a name="get-sql-server-connection-information"></a>SQL server bağlantı bilgilerini alma

[!INCLUDE [prerequisites-server-connection-info](../../includes/sql-database-connect-query-prerequisites-server-connection-info-includes.md)]
    
## <a name="create-code-to-query-your-sql-database"></a>SQL veritabanınızı sorgulamak için kod oluşturun 

1. Bir metin düzenleyicisinde adlı yeni bir dosya oluşturmak *sqltest.py*.  
   
1. Aşağıdaki kodu ekleyin. İçin kendi değerlerinizi yerleştirin \<server >, \<veritabanı >, \<kullanıcıadı >, ve \<parola >.
   
   >[!IMPORTANT]
   >Bu örnekteki kod kaynağı olarak veritabanınızı oluştururken seçebilirsiniz AdventureWorksLT örnek verileri kullanır. Farklı veri veritabanınız varsa kendi veritabanından tablolar SELECT sorgusunda kullanın. 
   
   ```python
   import pyodbc
   server = '<server>.database.windows.net'
   database = '<database>'
   username = '<username>'
   password = '<password>'
   driver= '{ODBC Driver 17 for SQL Server}'
   cnxn = pyodbc.connect('DRIVER='+driver+';SERVER='+server+';PORT=1433;DATABASE='+database+';UID='+username+';PWD='+ password)
   cursor = cnxn.cursor()
   cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
   row = cursor.fetchone()
   while row:
       print (str(row[0]) + " " + str(row[1]))
       row = cursor.fetchone()
   ```
   

## <a name="run-the-code"></a>Kodu çalıştırma

1. Bir komut isteminde aşağıdaki komutu çalıştırın:

   ```cmd
   python sqltest.py
   ```

1. Üst 20 kategori/ürün satırlar döndürülür ve ardından komut penceresini kapatana doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [İlk Azure SQL veritabanınızı tasarlama](sql-database-design-first-database.md)
- [SQL Server için Microsoft Python sürücüleri](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/?v=17.23h)

