---
title: Verileri bir SQL havuzuna alma
description: Azure SYNAPSE Analytics 'te bir SQL havuzuna veri alma hakkında bilgi edinin
services: synapse-analytics
author: djpmsft
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: daperlov
ms.reviewer: jrasnick
ms.openlocfilehash: 02efaf3f0382a7af63717e777036637de2bbec25
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90033209"
---
# <a name="ingest-data-into-a-sql-pool"></a>Verileri bir SQL havuzuna alma

Bu makalede, Azure SYNAPSE Analytics kullanarak bir Azure Data Lake Gen 2 depolama hesabından bir SQL havuzuna veri alma hakkında bilgi edineceksiniz.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* **Azure depolama hesabı**: Azure Data Lake Storage Gen 2 ' nı *kaynak* veri deposu olarak kullanırsınız. Depolama hesabınız yoksa, oluşturma adımları için bkz. [Azure depolama hesabı oluşturma](../../storage/blobs/data-lake-storage-quickstart-create-account.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) .
* **Azure SYNAPSE Analytics**: bir SQL havuzunu *Havuz* veri deposu olarak kullanırsınız. Azure SYNAPSE Analytics örneğiniz yoksa, oluşturma adımları için bkz. [SQL havuzu oluşturma](../../azure-sql/database/single-database-create-quickstart.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) .

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma

Azure SYNAPSE Analytics 'te, bağlı bir hizmet, bağlantı bilgilerinizi diğer hizmetlere tanımladığınız yerdir. Bu bölümde, bir Azure SYNAPSE Analytics ve bağlı hizmet Azure Data Lake Storage 2. ekleyeceksiniz.

1. Azure SYNAPSE Analytics UX ' i açın ve **Yönet** sekmesine gidin.
1. **Dış bağlantılar**altında **bağlı hizmetler**' i seçin.
1. Bağlı bir hizmet eklemek için **Yeni**' yi seçin.
1. Listeden Azure Data Lake Storage 2. kutucuğunu seçin ve **devam**' ı seçin.
1. Kimlik doğrulama kimlik bilgilerinizi girin. Hesap anahtarı, hizmet sorumlusu ve yönetilen kimlik, şu anda desteklenen kimlik doğrulama türleridir. Kimlik bilgilerinizin doğru olduğunu doğrulamak için Bağlantıyı Sına ' yı seçin. Bittiğinde **Oluştur**’u seçin.
1. 3-5 arası adımları yineleyin, ancak Azure Data Lake Storage 2. yerine Azure SYNAPSE Analytics kutucuğunu seçin ve ilgili bağlantı kimlik bilgilerini girin. Azure SYNAPSE Analytics, SQL kimlik doğrulaması, yönetilen kimlik ve hizmet sorumlusu için şu anda desteklenmektedir.

## <a name="create-pipeline"></a>İşlem hattı oluşturma

İşlem hattı, bir dizi etkinliğin yürütülmesi için mantıksal akışı içerir. Bu bölümde, verileri bir SQL havuzuna ADLS 2. veren bir kopyalama etkinliği içeren bir işlem hattı oluşturacaksınız.

1. **Orchestrate** sekmesine gidin. Ardışık düzen üstbilgisinin yanındaki artı simgesini seçin ve işlem **hattı**' nı seçin.
1. Etkinlikler bölmesinde **taşıma ve dönüştürme** ' nın altında, verileri ardışık düzen tuvaline **Kopyala** ' yı sürükleyin.
1. Kopyalama etkinliği ' ni seçin ve **kaynak** sekmesine gidin. Yeni bir kaynak veri kümesi oluşturmak için **Yeni** ' yi seçin.
1. Veri depolama alanı olarak Azure Data Lake Storage Gen2 seçin ve devam ' ı seçin.
1. Biçim olarak DelimitedText ' i seçin ve devam ' ı seçin.
1. Özellikleri ayarla bölmesinde, oluşturduğunuz ADLS bağlı hizmeti ' ni seçin. Kaynak verilerinizin dosya yolunu belirtin ve ilk satırın bir üst bilgisine sahip olup olmadığını belirtin. Şemayı dosya deposundan veya örnek bir dosyadan içeri aktarabilirsiniz. Bittiğinde Tamam ' ı seçin.
1. **Havuz** sekmesine gidin. Yeni bir havuz veri kümesi oluşturmak için **Yeni** ' yi seçin.
1. Veri depolduğunuz Azure SYNAPSE Analytics ' i seçin ve devam ' ı seçin.
1. Özellikleri ayarla bölmesinde, oluşturduğunuz Azure SYNAPSE Analytics bağlı hizmetini seçin. Varolan bir tabloya yazıyorsanız, açılan listeden seçin. Aksi takdirde, **Düzenle** ' yi işaretleyin ve yeni tablo adınızı girin. Bittiğinde Tamam 'ı seçin
1. Tablo oluşturuyorsanız tablo seçenek alanında **otomatik oluşturma tablosu** ' nu etkinleştirin.

## <a name="debug-and-publish-pipeline"></a>Hata ayıklama ve yayımlama işlem hattı

İşlem hattınızı yapılandırmayı tamamladıktan sonra, yapılarınızı yayımlamadan önce, her şeyin doğru olduğunu doğrulamak için bir hata ayıklama çalıştırması gerçekleştirebilirsiniz.

1. İşlem hattında hata ayıklamak için araç çubuğunda **Hata Ayıkla**'yı seçin. Pencerenin altındaki **Çıkış** sekmesinde işlem hattı çalıştırmasının durumu görüntülenir. 
1. İşlem hattı başarıyla çalıştırıldığında, üstteki araç çubuğunda **Tümünü Yayımla**' yı seçin. Bu eylem, oluşturduğunuz varlıkları (veri kümeleri ve işlem hatları) SYNAPSE Analytics hizmetinde yayımlar.
1. **Başarıyla yayımlandı** iletisini görene kadar bekleyin. Bildirim iletilerini görmek için sağ üst köşedeki zil düğmesini seçin. 


## <a name="trigger-and-monitor-the-pipeline"></a>İşlem hattını tetikleme ve izleme

Bu adımda, önceki adımda yayınlanan işlem hattını el ile tetiklersiniz. 

1. Araç çubuğunda **tetikleyici Ekle** ' yi seçin ve sonra **Şimdi Tetikle**' yi seçin. **İşlem Hattı Çalıştırma** sayfasında **Son**’u seçin.  
1. Sol kenar çubuğunda bulunan **izleyici** sekmesine gidin. El ile tetikleme tarafından tetiklenmiş bir işlem hattı çalıştırması görürsünüz. Etkinlik ayrıntılarını görüntülemek ve Işlem hattını yeniden çalıştırmak için **Eylemler** sütunundaki bağlantıları kullanabilirsiniz.
1. İşlem hattı çalıştırmalarıyla ilişkili etkinlik çalıştırmalarını görmek için **Eylemler** sütunundaki **Etkinlik Çalıştırmalarını Görüntüle** bağlantısını seçin. Bu örnekte yalnızca bir etkinlik bulunur, bu nedenle listede yalnızca bir giriş görürsünüz. Kopyalama işlemiyle ilgili ayrıntılar için **Eylemler** sütunundaki **Ayrıntılar** bağlantısını (gözlük simgesi) seçin. İşlem hattı çalıştırmaları görünümüne dönmek için üstteki işlem **hattı çalıştırmalarını** seçin. Görünümü yenilemek için **Yenile**’yi seçin.
1. Verilerinizin SQL havuzunda doğru yazıldığından emin olun.


## <a name="next-steps"></a>Sonraki adımlar

SYNAPSE Analytics için veri tümleştirmesi hakkında daha fazla bilgi için bkz. [verileri Azure Data Lake Storage 2. 'a dönüştürme ](data-integration-data-lake.md) makalesi.
