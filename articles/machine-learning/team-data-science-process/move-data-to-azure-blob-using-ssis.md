---
title: İçin veya SSIS bağlayıcıları kullanarak Azure Blob Depolama veri taşıma | Microsoft Docs
description: Veri veya SSIS bağlayıcıları kullanarak Azure Blob depolama alanından taşıyın.
services: machine-learning,storage
documentationcenter: ''
author: deguhath
manager: jhubbard
editor: cgronlun
ms.assetid: 96a1b5fb-34d1-4b9b-8d99-2bb8289e0398
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: deguhath
ms.openlocfilehash: 5db1e7b9c97a0c19ef5ec0a41ea675c33c4d46fc
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37950925"
---
# <a name="move-data-to-or-from-azure-blob-storage-using-ssis-connectors"></a>İçin veya SSIS bağlayıcıları kullanarak Azure Blob Depolama veri taşıma
[Azure için SQL Server Integration Services özellik paketi](https://msdn.microsoft.com/library/mt146770.aspx) Azure'a bağlanmak için Azure ve şirket içi veri kaynakları ve Azure'da depolanan işlem verileri arasında veri aktarımı bileşenleri sağlar.

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

Müşteriler, şirket içi verileri buluta taşıdığınızda, Azure teknolojileri paketi tam gücünden yararlanarak tüm Azure hizmetinden erişebilir. Bu, örneğin, Azure Machine Learning veya bir HDInsight kümesi üzerinde kullanılabilir.

Bu genellikle olması gereken ilk adım, [SQL](sql-walkthrough.md) ve [HDInsight](hive-walkthrough.md) izlenecek yollar.

Hibrit veri tümleştirme senaryolarında, yaygın işletme ihtiyaçlarını gerçekleştirmek için SSIS kullanan kurallı senaryoları için bkz [Bunu yapmak için Azure SQL Server Integration Services özellik paketi ile çok daha fazlasını](http://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) blogu.

> [!NOTE]
> Azure blob depolama için bir tam giriş için başvurmak [Azure Blob Temelleri](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) ve [Azure Blob hizmeti](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Önkoşullar
Bu makalede açıklanan görevler gerçekleştirmek için bir Azure aboneliği ve ayarlanmış bir Azure depolama hesabınız olması gerekir. Karşıya yükleme veya veri yüklemek için Azure depolama hesabı adını ve hesap anahtarınızı bilmeniz gerekir.

* Ayarlamak için bir **Azure aboneliği**, bkz: [ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).
* Oluşturma yönergeleri için bir **depolama hesabı** ve hesabı ve anahtar bilgilerini almak için bkz: [Azure depolama hesapları hakkında](../../storage/common/storage-create-storage-account.md).

Kullanılacak **SSIS bağlayıcıları**, yüklemeniz gerekir:

* **SQL Server 2014 veya 2016 standart (veya üstü)**: Install SQL Server Integration Services içerir.
* **Microsoft SQL Server 2014 veya 2016 tümleştirme hizmetleri özellik paketi için Azure**: Bu indirilebilir, sırasıyla gelen [SQL Server 2014 Integration Services](http://www.microsoft.com/download/details.aspx?id=47366) ve [SQL Server 2016 tümleştirmesi Hizmetleri](https://www.microsoft.com/download/details.aspx?id=49492) sayfaları.

> [!NOTE]
> SSIS SQL Server ile birlikte yüklenir, ancak Express sürümünde bulunmamaktadır. Hangi uygulamaların çeşitli SQL Server sürümlerinde yer hakkında daha fazla bilgi için bkz: [SQL Server sürümleri](http://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/)
> 
> 

SSIS'ndeki eğitim materyallerine için bkz: [SSIS üzerinde uygulamalı eğitim](https://www.microsoft.com/sql-server/training-certification)

Yukarı süreli alma hakkında daha fazla bilgi için kullanılacak SISS basit ayıklama, dönüştürme ve yükleme (ETL) paketleri için bkz: derleme [SSIS öğretici: basit bir ETL paket oluşturma](https://msdn.microsoft.com/library/ms169917.aspx).

## <a name="download-nyc-taxi-dataset"></a>NYC taksi veri kümesini indirin
Açıklanan örnek burada kullanmak genel kullanıma açık bir veri kümesi-- [NYC taksi Gelişlerin](http://www.andresmh.com/nyctaxitrips/) veri kümesi. Veri kümesi hakkında 173 milyon taksi sürmeye NYC içinde 2013 yılında oluşur. İki tür veri vardır: seyahat veri ve taksi verilerini ayrıntıları. Her ay için bir dosya gibi her biri yaklaşık 2 sıkıştırılmamış GB ise tüm 24 dosyalarında sahibiz.

## <a name="upload-data-to-azure-blob-storage"></a>Azure blob depolama alanına veri yükleme
Örneği kullandığımız SSIS kullanarak verileri şirket içi bir paketi Azure blob depolama alanına özellik taşımak için [ **Azure Blob karşıya yükleme görevi**](https://msdn.microsoft.com/library/mt146776.aspx), burada gösterilen:

![Yapılandırma-data-bilimi-vm](./media/move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)

Görev kullandığı Parametreler aşağıda açıklanmıştır:

| Alan | Açıklama |
| --- | --- |
| **AzureStorageConnection** |Mevcut bir Azure depolama Bağlantı Yöneticisi belirtir veya blob dosyalarını barındırıldığı işaret eden bir Azure depolama hesabına başvuran yeni bir tane oluşturur. |
| **BlobContainer** |Karşıya yüklenen dosyalar bloblar blob kapsayıcının adını belirtir. |
| **BlobDirectory** |Karşıya yüklenen dosya blok blobu olarak depolandığı blob dizini belirtir. Blob, hiyerarşik bir yapısı sanal dizindir. Blob zaten varsa, BT IA değiştirildi. |
| **LocalDirectory** |Karşıya yüklenecek dosyaları içeren yerel dizini belirtir. |
| **Dosya adı** |Belirtilen ada düzendeki dosyaları seçmek için bir ad filtresi belirtir. Örneğin, MySheet\*.xls\* MySheet001.xls ve MySheetABC.xlsx gibi dosyalarını içerir |
| **TimeRangeFrom/TimeRangeTo** |Zaman aralığı filtresi belirtir. Sonra değiştirilen dosyaları *TimeRangeFrom* ve önce *TimeRangeTo* dahil edilir. |

> [!NOTE]
> **AzureStorageConnection** kimlik bilgilerinin doğru olması gerekir ve **BlobContainer** aktarımı denenmeden önce mevcut olması gerekir.
> 
> 

## <a name="download-data-from-azure-blob-storage"></a>Verileri Azure blob Depolama'dan indirme
SSIS ile şirket içi depolama için Azure blob depolama alanından verileri indirmek için örneği kullanın. [Azure Blob karşıya yükleme görevi](https://msdn.microsoft.com/library/mt146779.aspx).

## <a name="more-advanced-ssis-azure-scenarios"></a>Daha gelişmiş Azure SSIS senaryoları
SSIS özellik paketi için daha karmaşık akışlar birlikte paketleme görevleri tarafından işlenecek sağlar. Örneğin, blob verilerini doğrudan bir HDInsight kümesinde, bir blob dönün ve ardından şirket içi depolama çıktısı karşıdan yüklenemedi akışı. SSIS ek SSIS bağlayıcıları kullanarak bir HDInsight kümesinde Hive ve Pig işleri çalıştırabilirsiniz:

* SSIS ile bir Azure HDInsight kümesinde bir Hive betiği çalıştırmak için kullanın [Azure HDInsight Hive görev](https://msdn.microsoft.com/library/mt146771.aspx).
* SSIS ile bir Azure HDInsight kümesinde bir Pig betiği çalıştırmak için kullanın [Azure HDInsight Pig görev](https://msdn.microsoft.com/library/mt146781.aspx).

