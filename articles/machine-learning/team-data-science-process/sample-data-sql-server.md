---
title: Azure'da - Team Data Science Process'i SQL Server verileri örneklendirme
description: Örnek verileri SQL veya Python programlama dili kullanarak azure'da SQL Server'da depolanan ve sonra Azure Machine Learning ile taşıyın.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 71a2ec9dc4d644fb8739db3817e2cd1d09913da7
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76717650"
---
# <a name="heading"></a>Azure 'da SQL Server örnek veriler

Bu makalede, SQL veya Python programlama dili kullanarak Azure SQL Server'da depolanan verileri örnek kullanmayı gösterir. Ayrıca bir dosyaya kaydetme, bir Azure blobuna yüklemeyi ve ardından Azure Machine Learning Studio'ya okuma tarafından Azure Machine Learning içine örneklenen verileri taşıma işlemini de gösterir.

Python örneklemesi [pyodbc](https://code.google.com/p/pyodbc/) ODBC kitaplığını kullanarak örnekleme yapmak için Azure 'daki SQL Server ve [Pandas](https://pandas.pydata.org/) kitaplığı 'na bağlanır.

> [!NOTE]
> Örnek SQL kodunu bu belgedeki verileri azure'da bir SQL Server'da olduğunu varsayar. Aksi takdirde, verilerinizi Azure 'da SQL Server taşımaya ilişkin yönergeler için [Azure 'da SQL Server verileri taşıma](move-sql-server-virtual-machine.md) makalesine başvurun.
> 
> 

**Verileriniz neden örnekleyebilirsiniz?**
Veri kümesini analiz etmek için planlama büyükse, genellikle aşağı örnek veriler için daha küçük ancak temsilcisi ve daha kolay yönetilebilir bir boyutunu azaltmak için iyi bir fikir olduğunu. Örnekleme, veri anlama, araştırma ve özellik mühendisliğini kolaylaştırır. [Ekip veri bilimi işlemindeki (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/) rolü, veri işleme işlevlerinin ve makine öğrenimi modellerinin hızlı prototipini etkinleştirmektir.

Bu örnekleme görevi, [ekip veri bilimi işlemindeki (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/)bir adımdır.

## <a name="SQL"></a>SQL kullanma
Bu bölümde veritabanındaki verilere yönelik basit rastgele örnekleme gerçekleştirmek için SQL kullanarak çeşitli yöntemler açıklanmaktadır. Veri boyutu ve onun dağıtım göre bir yöntem seçin.

Aşağıdaki iki öğe, örnekleme işlemini gerçekleştirmek için SQL Server `newid` nasıl kullanacağınızı gösterir. Seçtiğiniz yöntem, örneğin, örnek (Aşağıdaki örnek kodda pk_id, bir otomatik olarak oluşturulan birincil anahtar olarak kabul edilir) ne kadar rastgele olmasını istediğinize bağlıdır.

1. Daha az katı rastgele örnek
   
        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())
2. Daha fazla rastgele örnek 
   
        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

Tablesample verileri de örnekleme için kullanılabilir. Bu seçenek, veri boyutunuz büyükse (farklı sayfalardaki verilerin bağıntılı olmadığı varsayıldığında) ve sorgunun makul bir süre içinde tamamlanacağı daha iyi bir yaklaşım olabilir.

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

> [!NOTE]
> Keşfedin ve yeni bir tablo depolayarak örneklenen bu verilerden Özellikleri Oluştur
> 
> 

### <a name="sql-aml"></a>Azure Machine Learning bağlanılıyor
[Verileri Içeri aktarma][import-data] modülünde Azure Machine Learning yukarıdaki örnek sorguları doğrudan kullanarak, verileri anında örnekleyebilirsiniz ve bir Azure Machine Learning denemesine taşıyabilirsiniz. Örnek verileri okumak için okuyucu modülünü kullanmanın ekran görüntüsü burada gösterilmektedir:

![Okuyucu sql][1]

## <a name="python"></a>Python programlama dilini kullanma
Bu bölümde, Python 'da bir SQL Server veritabanına ODBC bağlantısı kurmak için [pyodbc kitaplığı](https://code.google.com/p/pyodbc/) kullanılması gösterilmektedir. Veritabanı bağlantı dizesi şu şekildedir: (ServerName, dbname, username ve Password öğesini yapılandırmanızla değiştirin):

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Python 'daki [Pandas](https://pandas.pydata.org/) kitaplığı, Python programlamasına yönelik veri işleme için zengin veri yapıları ve veri çözümleme araçları sağlar. Aşağıdaki kod, Azure SQL veritabanındaki bir tablodaki verilerin% 0,1 örneğini bir Pandas verilerine okur:

    import pandas as pd

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, column2... from <table_name> tablesample (0.1 percent)''', conn)

Artık, örneklenen verileri Pandas veri çerçevesi ile çalışabilirsiniz. 

### <a name="python-aml"></a>Azure Machine Learning bağlanılıyor
Aşağıdaki örnek kod, alt örneklenen verileri bir dosyaya kaydedin ve uygulamayı bir Azure blobuna yüklemek için kullanabilirsiniz. Blob 'daki veriler, [verileri Içeri aktarma][import-data] modülünü kullanarak doğrudan bir Azure Machine Learning denemesine okunabilir. Adımlar aşağıdaki gibidir: 

1. Pandas veri çerçevesi yerel bir dosyaya yazma
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. Azure blobuna yerel dosya karşıya yükleme
   
        from azure.storage import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
   
        try:
   
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
   
        except:            
            print ("Something went wrong with uploading blob:"+BLOBNAME)
3. Aşağıdaki ekranda gösterildiği gibi Azure Machine Learning [veri alma][import-data] modülünü kullanarak Azure Blob 'dan verileri okuyun:

![Okuyucu blob][2]

## <a name="the-team-data-science-process-in-action-example"></a>Team Data Science Process eylem örnekte
Ortak veri kümesi kullanan bir ekip veri bilimi Işlemi örneğini görmek için, bkz. [Team Data Science Process ın Action: using SQL Server](sql-walkthrough.md).

[1]: ./media/sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/sample-sql-server-virtual-machine/reader_blob.png

[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
