---
title: SYNAPSE Studio Not defterleri
description: Bu makalede, veri hazırlama ve görselleştirme için Azure SYNAPSE Studio (Önizleme) Not defterleri oluşturmayı ve geliştirmeyi öğreneceksiniz.
services: synapse analytics
author: ruixinxu
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: spark
ms.date: 10/19/2020
ms.author: ruxu
ms.reviewer: ''
ms.custom: devx-track-python
ms.openlocfilehash: ea765ae5ff93625cc6a0ed36776a8925e5fce836
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93311145"
---
# <a name="create-develop-and-maintain-synapse-studio-preview-notebooks-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te SYNAPSE Studio (Önizleme) Not defterleri oluşturma, geliştirme ve bakımını yapma

Bir Synapse Studio (Önizleme) Not defteri, canlı kod, görselleştirme ve anlatım metni içeren dosyalar oluşturmanız için bir Web arabirimidir. Not defterleri, fikirlerinizi doğrulamak ve hızlı denemeleri kullanarak verilerinizden Öngörüler elde etmek için iyi bir yerdir. Not defterleri, veri hazırlama, veri görselleştirme, makine öğrenimi ve diğer büyük veri senaryolarında da yaygın olarak kullanılır.

Bir Azure SYNAPSE Studio not defteriyle şunları yapabilirsiniz:

* Sıfır kurulum çabası ile çalışmaya başlayın.
* Yerleşik kurumsal güvenlik özellikleriyle verileri güvende tutun.
* Ham biçimlerdeki verileri (CSV, txt, JSON, vb.), işlenen dosya biçimlerini (Parquet, Delta Gölü, ORC, vb.) ve SQL tablolu veri dosyalarını Spark ve SQL 'de analiz edin.
* Gelişmiş yazma özellikleri ve yerleşik veri görselleştirmesi ile üretken olun.

Bu makalede, Azure SYNAPSE Studio 'da Not defterlerinin nasıl kullanılacağı açıklanır.

## <a name="create-a-notebook"></a>Not defteri oluşturma

Bir not defteri oluşturmanın iki yolu vardır. Yeni bir not defteri oluşturabilir veya var olan bir not defterini **Nesne Gezgini** bir Azure SYNAPSE çalışma alanına aktarabilirsiniz. Azure SYNAPSE Studio Not defterleri, standart Jupyter Notebook IPYNB dosyalarını algılayabilir.

![İçeri aktarma Not Defteri Oluştur](./media/apache-spark-development-using-notebooks/synapse-create-import-notebook.png)

## <a name="develop-notebooks"></a>Not defterleri geliştirme

Not defterleri, bağımsız olarak veya grup olarak çalıştırılan kod veya metin blokları olan hücrelerden oluşur.

### <a name="add-a-cell"></a>Hücre Ekle

Not defterinize yeni bir hücre eklemenin birden çok yolu vardır.

1. Sol üst **+ hücre** düğmesini genişletin ve **kod hücresi Ekle** ya da **metin hücresi Ekle** ' yi seçin.

    ![Ekle-hücre-düğme](./media/apache-spark-development-using-notebooks/synapse-add-cell-1.png)

2. İki hücre arasındaki alanın üzerine gelin ve **Kod Ekle** veya **metin ekle** ' yi seçin.

    ![boşluk Ekle-boşluk](./media/apache-spark-development-using-notebooks/synapse-add-cell-2.png)

3. [Komut modu altında kısayol tuşlarını](#shortcut-keys-under-command-mode)kullanın. Geçerli hücrenin üstüne bir hücre eklemek için **a** 'ya basın. Geçerli hücrenin altına bir hücre eklemek için **B** tuşuna basın.

### <a name="set-a-primary-language"></a>Birincil dil ayarla

Azure SYNAPSE Studio Not defterleri dört Apache Spark dili destekler:

* pySpark (Python)
* Spark (Scala)
* Mini SQL
* Apache Spark için .NET (C#)

En üstteki komut çubuğundaki açılan listeden yeni eklenen hücreler için birincil dili ayarlayabilirsiniz.

   ![Varsayılan-SYNAPSE-Language](./media/apache-spark-development-using-notebooks/synapse-default-language.png)

### <a name="use-multiple-languages"></a>Birden çok dil kullanma

Bir hücrenin başlangıcında doğru dil Magic komutunu belirterek, bir not defterinde birden çok dil kullanabilirsiniz. Aşağıdaki tabloda, hücre dillerini değiştirmek için Magic komutları listelenmektedir.

|Magic komutu |Dil | Açıklama |  
|---|------|-----|
|%% pyspark| Python | Spark bağlamına karşı bir **Python** sorgusu yürütün.  |
|%% Spark| Scala | Spark bağlamına yönelik bir **Scala** sorgusu yürütün.  |  
|%% SQL| Mini SQL | Spark bağlamına karşı bir **mini SQL** sorgusu yürütün.  |
|%% CSharp | Spark C için .NET # | Spark bağlamına karşı **Spark C# için .net** sorgusu yürütün. |

Aşağıdaki görüntü, bir **Spark (Scala)** not defterinde%% **pyspark** Magic komutunu veya **%% SQL** Magic komutuyla bir mini SQL sorgusunu kullanarak pyspark sorgusunun nasıl yazılacağını gösteren bir örnektir. Not defteri için birincil dilin pySpark olarak ayarlandığını unutmayın.

   ![SYNAPSE Spark Magic komutları](./media/apache-spark-development-using-notebooks/synapse-spark-magics.png)

### <a name="use-temp-tables-to-reference-data-across-languages"></a>Diller arasında verilere başvurmak için geçici tabloları kullanma

Bir Synapse Studio not defterindeki farklı dillerdeki verilere veya değişkenlere doğrudan başvurulamıyor. Spark 'ta, diller arasında geçici bir tabloya başvuru yapılabilir. Bir `Scala` `PySpark` `SparkSQL` geçici çözüm olarak Spark geçici tablosunu kullanarak ve ' de bir veri çerçevesinin nasıl okunmasına ilişkin bir örnek aşağıda verilmiştir.

1. 1. hücrede, bir SQL havuzu bağlayıcısından Scala kullanarak bir veri çerçevesini okuyun ve geçici bir tablo oluşturun.

   ```scala
   %%scala
   val scalaDataFrame = spark.read.sqlanalytics("mySQLPoolDatabase.dbo.mySQLPoolTable")
   scalaDataFrame.createOrReplaceTempView( "mydataframetable" )
   ```

2. 2. hücrede Spark SQL kullanarak verileri sorgulayın.
   
   ```sql
   %%sql
   SELECT * FROM mydataframetable
   ```

3. 3. hücrede, PySpark içindeki verileri kullanın.

   ```python
   %%pyspark
   myNewPythonDataFrame = spark.sql("SELECT * FROM mydataframetable")
   ```

### <a name="ide-style-intellisense"></a>IDE stili IntelliSense

Azure SYNAPSE Studio Not defterleri, IDE stili IntelliSense 'i hücre düzenleyicisine getirmek için Monako düzenleyiciyle tümleşiktir. Sözdizimi vurgusu, hata işaretçisi ve otomatik kod tamamlama, kod yazmanıza ve sorunları daha hızlı belirlemenize yardımcı olur.

IntelliSense özellikleri farklı diller için farklı ölçü düzeyleridir. Desteklenen öğeleri görmek için aşağıdaki tabloyu kullanın.

|Diller| Sözdizimi vurgusu | Söz dizimi hata Işaretleyicisi  | Sözdizimi kodu tamamlama | Değişken kodu tamamlama| Sistem Işlevi kod tamamlama| Kullanıcı Işlevi kod tamamlama| Akıllı Girinti | Kod katlama|
|--|--|--|--|--|--|--|--|--|
|PySpark (Python)|Evet|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Spark (Scala)|Evet|Evet|Evet|Evet|-|-|-|Evet|
|Mini SQL|Evet|Evet|-|-|-|-|-|-|
|Spark için .NET (C#)|Evet|-|-|-|-|-|-|-|

### <a name="format-text-cell-with-toolbar-buttons"></a>Araç çubuğu düğmeleriyle metin hücresini Biçimlendir

Ortak markaşağı eylemleri yapmak için metin hücreleri araç çubuğundaki Biçim düğmelerini kullanabilirsiniz. Bu, kalın metin, italicizing metin, kod parçacıkları ekleme, sırasız liste ekleme, sıralı liste ekleme ve URL 'den görüntü ekleme içerir.

  ![SYNAPSE metin hücresi araç çubuğu](./media/apache-spark-development-using-notebooks/synapse-text-cell-toolbar.png)

### <a name="undo-cell-operations"></a>Hücre işlemlerini geri al
En son hücre işlemini iptal etmek için **geri al** düğmesini seçin veya **CTRL + Z** tuşlarına basın. Artık en son 20 geçmiş hücresi eylemini geri alabilirsiniz. 

   ![SYNAPSE geri alma hücreleri](./media/apache-spark-development-using-notebooks/synapse-undo-cells.png)

### <a name="move-a-cell"></a>Bir hücreyi taşıma

En sağdaki Ek hücre eylemleri menüsüne erişmek için üç nokta (...) simgesini seçin. Ardından, geçerli hücreyi taşımak için **hücreyi yukarı taşı** veya **hücreyi aşağı taşı** ' yı seçin. 

[Komut modu altında kısayol tuşlarını](#shortcut-keys-under-command-mode)da kullanabilirsiniz. Geçerli hücreyi taşımak için **Ctrl + Alt + ↑** tuşlarına basın. Geçerli hücreyi aşağı taşımak için **Ctrl + Alt + ↓** tuşlarına basın.

   ![hücre taşı](./media/apache-spark-development-using-notebooks/synapse-move-cells.png)

### <a name="delete-a-cell"></a>Bir hücreyi silme

Bir hücreyi silmek için, en sağdaki Ek hücre eylemleri menüsüne erişmek üzere üç nokta (...) simgesini seçin ve ardından **hücreyi Sil** ' i seçin. 

[Komut modu altında kısayol tuşlarını](#shortcut-keys-under-command-mode)da kullanabilirsiniz. Geçerli hücreyi silmek için **d, d** tuşuna basın.
  
   ![hücreyi Sil](./media/apache-spark-development-using-notebooks/synapse-delete-cell.png)

### <a name="collapse-a-cell-input"></a>Hücre girişini daraltma
Daraltmak için geçerli hücrenin alt kısmındaki ok düğmesini seçin. Genişletmek için, hücre darallarken ok düğmesini seçin.

   ![daraltma-hücre girişi](./media/apache-spark-development-using-notebooks/synapse-collapse-cell-input.gif)

### <a name="collapse-a-cell-output"></a>Hücre çıkışını daraltma

Daraltmak için geçerli hücre çıktısının sol üst kısmındaki **çıktıyı Daralt** düğmesini seçin. Genişletmek için hücre çıktısı daraltıldığında **hücre çıkışını göster** ' i seçin.

   ![daraltma-hücre çıkışı](./media/apache-spark-development-using-notebooks/synapse-collapse-cell-output.gif)

## <a name="run-notebooks"></a>Not defterlerini çalıştırma

Kod hücrelerini Not defterinizde tek seferde veya tümünde çalıştırabilirsiniz. Her hücrenin durumu ve ilerlemesi not defterinde temsil edilir.

### <a name="run-a-cell"></a>Bir hücre Çalıştır

Bir hücrede kodu çalıştırmanın birkaç yolu vardır.

1. Çalıştırmak istediğiniz hücreye gelin ve **hücreyi Çalıştır** düğmesini seçin veya **CTRL + ENTER** tuşlarına basın.

   ![1. çalışma hücresi](./media/apache-spark-development-using-notebooks/synapse-run-cell.png)


2. En sağdaki Ek hücre eylemleri menüsüne erişmek için üç nokta ( **...** ) simgesini seçin. Ardından, **hücreyi Çalıştır** ' ı seçin.

   ![çalışma-2](./media/apache-spark-development-using-notebooks/synapse-run-cell-2.png)
   
3. [Komut modu altında kısayol tuşlarını](#shortcut-keys-under-command-mode)kullanın. Geçerli hücreyi çalıştırmak için **SHIFT + enter** tuşlarına basın ve aşağıdaki hücreyi seçin. Geçerli hücreyi çalıştırmak ve aşağıya yeni bir hücre eklemek için **Alt + Enter** tuşlarına basın.


### <a name="run-all-cells"></a>Tüm hücreleri Çalıştır
Geçerli not defterindeki tüm hücreleri sırayla çalıştırmak için **Tümünü Çalıştır** düğmesini seçin.

   ![tüm hücreleri Çalıştır](./media/apache-spark-development-using-notebooks/synapse-run-all.png)

### <a name="run-all-cells-above-or-below"></a>Yukarıdaki veya altındaki tüm hücreleri Çalıştır

En sağdaki Ek hücre eylemleri menüsüne erişmek için üç nokta ( **...** ) simgesini seçin. Ardından, geçerli sıradaki tüm hücreleri çalıştırmak için **yukarıdaki hücreleri Çalıştır** ' ı seçin. Sıradaki geçerli olan tüm hücreleri çalıştırmak için **aşağıdaki hücreleri Çalıştır** ' ı seçin.

   ![çalışma--veya-aşağı hücreleri](./media/apache-spark-development-using-notebooks/synapse-run-cells-above-or-below.png)


### <a name="cancel-all-running-cells"></a>Tüm çalışan hücreleri iptal et
Çalışan hücreleri veya sırada bekleyen hücreleri iptal etmek için **Tümünü Iptal et** düğmesini seçin. 
   ![iptal-tümünü-hücreler](./media/apache-spark-development-using-notebooks/synapse-cancel-all.png) 

### <a name="cell-status-indicator"></a>Hücre durumu göstergesi

Bir adım adım hücre yürütme durumu, geçerli ilerlemesini görmenizi sağlayacak şekilde hücrenin altında görüntülenir. Hücre çalıştırma işlemi tamamlandıktan sonra, toplam süre ve bitiş saatine sahip bir yürütme özeti gösterilir ve gelecekte başvuru için orada tutulur.

![hücre-durum](./media/apache-spark-development-using-notebooks/synapse-cell-status.png)

### <a name="spark-progress-indicator"></a>Spark ilerleme göstergesi

Azure SYNAPSE Studio Not defteri yalnızca Spark tabanlıdır. Kod hücreleri, sunucusuz Apache Spark havuzunda uzaktan yürütülür. İş yürütme durumunu anlamanıza yardımcı olmak üzere gerçek zamanlı ilerleme çubuğu içeren bir Spark işi ilerleme göstergesi görünür.
Her iş veya aşama başına görev sayısı, Spark işinizin paralel düzeyini belirlemenize yardımcı olur. Ayrıca, iş (veya aşama) adındaki bağlantıyı seçerek belirli bir işin Spark Kullanıcı arabirimine (veya aşamasına) ayrıntılı olarak gidebilirsiniz.


![Spark-ilerleme-gösterge](./media/apache-spark-development-using-notebooks/synapse-spark-progress-indicator.png)

### <a name="spark-session-config"></a>Spark oturum yapılandırması

**Yapılandırma oturumunda** geçerli Spark oturumuna verilecek yürüticilere zaman aşımı süresini, sayısını ve bunların boyutunu belirtebilirsiniz. Spark oturumunu yeniden başlatmak, yapılandırma değişikliklerinin etkili olması içindir. Tüm önbelleğe alınmış Not defteri değişkenleri temizlenir.

[![oturum yönetimi](./media/apache-spark-development-using-notebooks/synapse-spark-session-management.png)](./media/apache-spark-development-using-notebooks/synapse-spark-session-management.png#lightbox)

Spark oturum öneren, Spark oturum yapılandırma panelinde artık kullanılabilir. Bir Spark havuzunu doğrudan oturum yapılandırma panelinden seçebilir ve kaç düğüm kullandığını ve kaç tane kaç yürütme kullanılabilir olduğunu görebilirsiniz. Bu bilgiler, geri ve ileri değiştirme yerine uygun oturum boyutunu ayarlamanıza yardımcı olabilir.

![oturum-öner](./media/apache-spark-development-using-notebooks/synapse-spark-session-recommender.png)


## <a name="bring-data-to-a-notebook"></a>Verileri bir not defterine getirme

Aşağıdaki kod örneklerinde gösterildiği gibi Azure Blob depolama, Azure Data Lake Store Gen 2 ve SQL havuzundaki verileri yükleyebilirsiniz.

### <a name="read-a-csv-from-azure-data-lake-store-gen2-as-a-spark-dataframe"></a>Spark veri çerçevesi olarak Azure Data Lake Store Gen2 'ten CSV okuma

```python
from pyspark.sql import SparkSession
from pyspark.sql.types import *
account_name = "Your account name"
container_name = "Your container name"
relative_path = "Your path"
adls_path = 'abfss://%s@%s.dfs.core.windows.net/%s' % (container_name, account_name, relative_path)

spark.conf.set("fs.azure.account.auth.type.%s.dfs.core.windows.net" %account_name, "SharedKey")
spark.conf.set("fs.azure.account.key.%s.dfs.core.windows.net" %account_name ,"Your ADLSg2 Primary Key")

df1 = spark.read.option('header', 'true') \
                .option('delimiter', ',') \
                .csv(adls_path + '/Testfile.csv')

```

#### <a name="read-a-csv-from-azure-blob-storage-as-a-spark-dataframe"></a>Azure Blob depolama 'dan bir CSV 'yi Spark veri çerçevesi olarak okuma

```python

from pyspark.sql import SparkSession
from pyspark.sql.types import *

blob_account_name = "Your blob account name"
blob_container_name = "Your blob container name"
blob_relative_path = "Your blob relative path"
blob_sas_token = "Your blob sas token"

wasbs_path = 'wasbs://%s@%s.blob.core.windows.net/%s' % (blob_container_name, blob_account_name, blob_relative_path)
spark.conf.set('fs.azure.sas.%s.%s.blob.core.windows.net' % (blob_container_name, blob_account_name), blob_sas_token)

df = spark.read.option("header", "true") \
            .option("delimiter","|") \
            .schema(schema) \
            .csv(wasbs_path)

```

### <a name="read-data-from-the-primary-storage-account"></a>Birincil depolama hesabından veri okuma

Birincil depolama hesabındaki verilere doğrudan erişebilirsiniz. Gizli anahtarları sağlamanız gerekmez. Veri Gezgini, bir dosyaya sağ tıklayıp **Yeni Not defteri** ' ni seçerek veri ayıklayıcısı otomatik olarak oluşturulur yeni bir not defteri görüntüleyin.

![verilerden hücrelere](./media/apache-spark-development-using-notebooks/synapse-data-to-cell.png)

## <a name="visualize-data-in-a-notebook"></a>Not defterindeki verileri görselleştirme

### <a name="produce-rendered-table-view"></a>İşlenmiş tablo görünümü üret

Bir çubuk grafik, çizgi grafik, pasta grafiği, dağılım grafiği ve alan grafiği oluşturma seçeneğiyle tablolu bir sonuç görünümü sağlanır. Verilerinizi kod yazmak zorunda kalmadan görselleştirebilirsiniz. Grafikler, **grafik seçeneklerinde** özelleştirilebilir. 

**%% SQL** Magic komutlarının çıkışı, işlenen tablo görünümünde varsayılan olarak görünür. <code>display(df)</code>İşlenmiş tablo görünümünü oluşturmak Için Spark dataframe, Pandas Dataframe, List veya dayanıklı Dağıtılmış veri kümeleri (RDD) işlevini çağırabilirsiniz.

   [![yerleşik-grafikler](./media/apache-spark-development-using-notebooks/synapse-builtin-charts.png)](./media/apache-spark-development-using-notebooks/synapse-builtin-charts.png#lightbox)

### <a name="visualize-built-in-charts-from-large-scale-dataset"></a>Büyük ölçekli veri kümesinden yerleşik grafikleri görselleştirin 

Varsayılan olarak, <code>display(df)</code> işlev, grafikleri işlemek için verilerin yalnızca ilk 1000 satırını alır. **Tüm sonuçlar üzerinde toplamayı** denetleyin ve **Uygula** düğmesini seçin, grafik oluşturmayı tüm veri kümesinden uygulayacaksınız. Grafik ayarı değiştiğinde bir Spark işi tetiklenecek, hesaplamayı tamamlayıp grafiği işlemeye yönelik bir süre sürer. 
    [![yerleşik-grafikler-toplama-tümü](./media/apache-spark-development-using-notebooks/synapse-builtin-charts-aggregation-all.png)](./media/apache-spark-development-using-notebooks/synapse-builtin-charts-aggregation-all.png#lightbox)



### <a name="visualize-data-statistic-information"></a>Veri istatistikleri bilgilerini görselleştirin
Sütun <code>display(df, summary = True)</code> adı, sütun türü, benzersiz değerler ve her sütun için eksik değerleri içeren belirli bir Spark veri çerçevesinin istatistik özetini denetlemek için öğesini kullanabilirsiniz. Ayrıca, en az değer, Maxhayvan değeri, ortalama değer ve standart sapmayı görmek için belirli bir sütun üzerinde seçim yapabilirsiniz.
    [![yerleşik-grafikler-Özet ](./media/apache-spark-development-using-notebooks/synapse-builtin-charts-summary.png)](./media/apache-spark-development-using-notebooks/synapse-builtin-charts-summary.png#lightbox)

### <a name="render-html-or-interactive-libraries"></a>HTML veya etkileşimli kitaplıkları işleme

HTML kodunu, **Displayhtml ()** kullanarak **bokeh** gibi JavaScript, CSS, D3 veya etkileşimli kitaplıklar gibi işleyebilirsiniz.

Aşağıdaki resim **bokeh** kullanarak bir harita üzerinde karakter çizme örneğidir.

   ![bokeh-örnek](./media/apache-spark-development-using-notebooks/synapse-bokeh-image.png)
   

Yukarıdaki görüntüyü çizmek için aşağıdaki örnek kodu çalıştırın.

```python
from bokeh.plotting import figure, output_file
from bokeh.tile_providers import get_provider, Vendors
from bokeh.embed import file_html
from bokeh.resources import CDN
from bokeh.models import ColumnDataSource

tile_provider = get_provider(Vendors.CARTODBPOSITRON)

# range bounds supplied in web mercator coordinates
p = figure(x_range=(-9000000,-8000000), y_range=(4000000,5000000),
           x_axis_type="mercator", y_axis_type="mercator")
p.add_tile(tile_provider)

# plot datapoints on the map
source = ColumnDataSource(
    data=dict(x=[ -8800000, -8500000 , -8800000],
              y=[4200000, 4500000, 4900000])
)

p.circle(x="x", y="y", size=15, fill_color="blue", fill_alpha=0.8, source=source)

# create an html document that embeds the Bokeh plot
html = file_html(p, CDN, "my plot1")

# display this html
displayHTML(html)

```

## <a name="save-notebooks"></a>Not defterlerini Kaydet

Çalışma alanınızdaki tek bir not defterini veya tüm not defterlerini kaydedebilirsiniz.

1. Tek bir not defterine yaptığınız değişiklikleri kaydetmek için Not defteri komut çubuğundaki **Yayımla** düğmesini seçin.

   ![Yayımlama-Not defteri](./media/apache-spark-development-using-notebooks/synapse-publish-notebook.png)

2. Çalışma alanınızdaki tüm not defterlerini kaydetmek için çalışma alanı komut çubuğunda **Tümünü Yayımla** düğmesini seçin. 

   ![tümünü Yayımla](./media/apache-spark-development-using-notebooks/synapse-publish-all.png)

Not defteri özelliklerinde, kaydetme sırasında hücre çıkışının eklenip eklenmeyeceğini yapılandırabilirsiniz.

   ![Not defteri-Özellikler](./media/apache-spark-development-using-notebooks/synapse-notebook-properties.png)

## <a name="magic-commands"></a>MAGIC komutları
Azure SYNAPSE Studio not defterlerinde tanıdık Jupyıter Magic komutlarını kullanabilirsiniz. Geçerli kullanılabilir sihirli komutları olarak aşağıdaki listeyi gözden geçirin. İhtiyaçlarınızı karşılamak için daha fazla sihirli komut oluşturmaya devam edebilmemiz için [kullanım durumlarınızı GitHub 'da](https://github.com/MicrosoftDocs/azure-docs/issues/new) bize söyleyin.

Kullanılabilir satır mıknatıcs: [% lsmagic](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-lsmagic), [% Time](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-time), [% Time It](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-timeit)

Kullanılabilir hücre mıknatıcs: [%% Time](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-time), [%% timeIt](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-timeit), [%% Capture](https://ipython.readthedocs.io/en/stable/interactive/magics.html#cellmagic-capture),%% [WriteFile](https://ipython.readthedocs.io/en/stable/interactive/magics.html#cellmagic-writefile),% [% SQL](#use-multiple-languages), [%% pyspark](#use-multiple-languages), [%% Spark](#use-multiple-languages), [%% CSharp](#use-multiple-languages)


## <a name="integrate-a-notebook"></a>Bir not defterini tümleştirme

### <a name="add-a-notebook-to-a-pipeline"></a>İşlem hattına Not defteri ekleme

Mevcut bir işlem hattına bir not defteri eklemek veya yeni bir işlem hattı oluşturmak için sağ üst köşedeki **ardışık düzene Ekle** düğmesini seçin.

![İşlem hattına Not defteri ekleme](./media/apache-spark-development-using-notebooks/add-to-pipeline.png)

### <a name="designate-a-parameters-cell"></a>Bir parametre hücresi belirleme

Not defterinizi parametreleştirin, en sağdaki Ek hücre eylemleri menüsüne erişmek için üç nokta (...) simgesini seçin. Ardından, parametreyi parametre hücresi olarak belirlemek için **parametre hücresini değiştirin** ' i seçin.

![iki durumlu parametre](./media/apache-spark-development-using-notebooks/toggle-parameter-cell.png)

Azure Data Factory, parametreler hücresini arar ve yürütme zamanında geçirilen parametreler için bu hücreyi varsayılanlar olarak değerlendirir. Yürütme altyapısı, varsayılan değerlerin üzerine yazmak için, Parameters hücresinin altına giriş parametreleriyle yeni bir hücre ekler. Bir parametre hücresi belirlenmediğinde, eklenen hücre, Not defterinin en üstüne eklenir.

### <a name="assign-parameters-values-from-a-pipeline"></a>İşlem hattından parametre değerleri atama

Parametrelere sahip bir not defteri oluşturduktan sonra, Azure SYNAPSE Not defteri etkinliğine sahip bir işlem hattından çalıştırabilirsiniz. Etkinliği işlem hattı Tuvalinize ekledikten sonra, **Ayarlar** sekmesindeki **temel parametreler** bölümünde parametre değerlerini ayarlayabilirsiniz. 

![Parametre atama](./media/apache-spark-development-using-notebooks/assign-parameter.png)

Parametre değerlerini atarken, işlem [hattı ifade dilini](../../data-factory/control-flow-expression-language-functions.md) veya [Sistem değişkenlerini](../../data-factory/control-flow-system-variables.md)kullanabilirsiniz.



## <a name="shortcut-keys"></a>Kısayol tuşları

Jupyıter not defterlerine benzer şekilde, Azure SYNAPSE Studio Not defterleri de kalıcı bir kullanıcı arabirimine sahiptir. Klavye, Not defteri hücresinin bulunduğu moda bağlı olarak farklı şeyler yapar. SYNAPSE Studio Not defterleri, belirli bir kod hücresi için aşağıdaki iki modu destekler: komut modu ve düzenleme modu.

1. Bir hücre, yazmanızı isteyen bir metin imleci olmadığında komut modundadır. Bir hücre komut modundayken, Not defterini tek tek hücrelere değil, bir bütün olarak düzenleyebilirsiniz. `ESC`Bir hücrenin düzenleyici alanının dışında seçim yapmak için fare tuşuna basarak veya fareyi kullanarak komut moduna girin.

   ![komut modu](./media/apache-spark-development-using-notebooks/synapse-command-mode2.png)

2. Düzenleme modu, düzenleyici alanına yazmanızı isteyen bir metin imlece belirtilir. Bir hücre düzenleme modundayken, hücreye yazabilirsiniz. `Enter`Fareyle sürükleyerek veya bir hücrenin düzenleyici alanında seçim yaparak düzenleme modunu girin.
   
   ![düzenleme modu](./media/apache-spark-development-using-notebooks/synapse-edit-mode2.png)

### <a name="shortcut-keys-under-command-mode"></a>Komut modu altında kısayol tuşları

Aşağıdaki tuş vuruşu kısayollarını kullanarak Azure SYNAPSE Not defterlerindeki kodu daha kolay bir şekilde gezinebilir ve çalıştırabilirsiniz.

| Eylem |SYNAPSE Studio Not defteri kısayolları  |
|--|--|
|Geçerli hücreyi çalıştırın ve aşağıdan seçin | Shift+Enter |
|Geçerli hücreyi çalıştırın ve Alta ekleyin | Alt+Enter |
|Üstteki hücreyi seçin| Yukarı |
|Aşağıdan hücre seçin| Aşağı |
|Yukarıya hücre ekle| A |
|Aşağıya hücre ekle| B |
|Üstteki seçili hücreleri Genişlet| SHIFT + yukarı |
|Seçili hücreleri aşağıdan Genişlet| SHIFT + aşağı|
|Hücreyi yukarı taşı| Ctrl + alt + ↑ |
|Hücreyi aşağı taşı| Ctrl + alt + ↓ |
|Seçili hücreleri sil| D, D |
|Düzenleme moduna geç| Enter |

### <a name="shortcut-keys-under-edit-mode"></a>Düzenleme modundaki kısayol tuşları

Aşağıdaki tuş vuruşu kısayollarını kullanarak, düzenleme modundayken Azure SYNAPSE Not defterlerindeki kodu daha kolay bir şekilde açabilir ve çalıştırabilirsiniz.

| Eylem |SYNAPSE Studio Not defteri kısayolları  |
|--|--|
|İmleci yukarı taşı | Yukarı |
|İmleci aşağı taşı|Aşağı|
|Geri Al|CTRL + Z|
|Yinele|CTRL + Y|
|Açıklama/açıklama kaldır|CTRL +/|
|Önceki kelimeyi Sil|CTRL + geri al|
|Sonra sözcüğü Sil|Ctrl + Delete|
|Hücre başlangıcına git|Ctrl + Home|
|Hücre sonuna git |CTRL + END|
|Bir kelimeyi sola git|Ctrl + sol|
|Bir sözcüğe sağ git|CTRL + sağ|
|Tümünü seç|CTRL + A|
|Leyebilirsiniz| CTRL +]|
|Girintiyi geri al|CTRL + [|
|Komut moduna geç| Esc |

## <a name="next-steps"></a>Sonraki adımlar
- [SYNAPSE örnek not defterlerine göz atın](https://github.com/Azure-Samples/Synapse/tree/master/Notebooks)
- [Hızlı başlangıç: Web araçlarını kullanarak Azure SYNAPSE Analytics 'te Apache Spark Havuzu (Önizleme) oluşturma](../quickstart-apache-spark-notebook.md)
- [Azure SYNAPSE Analytics 'te Apache Spark nedir?](apache-spark-overview.md)
- [Azure SYNAPSE Analytics ile Apache Spark için .NET kullanın](spark-dotnet.md)
- [Apache Spark belgeleri için .NET](/dotnet/spark?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics)
