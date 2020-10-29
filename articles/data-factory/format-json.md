---
title: Azure Data Factory JSON biçimi
description: Bu konuda Azure Data Factory JSON biçimiyle nasıl ele alınacağını açıklamaktadır.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/29/2020
ms.author: jingwang
ms.openlocfilehash: e0d8d5c833460838c28c7bc20b3ab764ed3ae659
ms.sourcegitcommit: dd45ae4fc54f8267cda2ddf4a92ccd123464d411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2020
ms.locfileid: "92927014"
---
# <a name="json-format-in-azure-data-factory"></a>Azure Data Factory JSON biçimi

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

**JSON dosyalarını ayrıştırmak veya VERILERI JSON biçimine yazmak** istediğinizde bu makaleye uyun. 

JSON biçimi şu bağlayıcılar için desteklenir: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage 1.](connector-azure-data-lake-store.md), [Azure Data Lake Storage 2.](connector-azure-data-lake-storage.md), [Azure dosya depolama](connector-azure-file-storage.md), [dosya sistemi](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md) [,,](connector-hdfs.md), [http](connector-http.md)ve [SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, JSON veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

| Özellik         | Açıklama                                                  | Gerekli |
| ---------------- | ------------------------------------------------------------ | -------- |
| tür             | Veri kümesinin Type özelliği **JSON** olarak ayarlanmalıdır. | Yes      |
| location         | Dosya (ler) in konum ayarları. Her dosya tabanlı bağlayıcının, altında kendi konum türü ve desteklenen özellikleri vardır `location` . **Bağlayıcı makalesi-> veri kümesi özellikleri bölümünde ayrıntılara bakın** . | Yes      |
| encodingName     | Test dosyalarını okumak/yazmak için kullanılan kodlama türü. <br>İzin verilen değerler şunlardır: "UTF-8", "UTF-16", "UTF-16TO", "UTF-32", "UTF-32TO", "US-ASCII", "UTF-7", "BIG5", "EUC-JP", "EUC-KR", "GB2312", "GB18030", "JOHAB", "SHIFT-JıS", "CP875", "CP866", "IBM00858", "IBM037", "IBM273", "IBM437", "IBM500", "IBM737", "IBM775", "IBM850", "IBM852", "IBM855", "IBM857", "IBM860", "" IBM861 "," IBM863 "," IBM864 "," IBM865 "," IBM869 "," IBM870 "," IBM01140 "," IBM01141 "," IBM01142 "," IBM01143 "," IBM01144 "," IBM01145 "," IBM01146 "," IBM01147 "," IBM01148 "," IBM01149 "," ISO-2022-JP "," ISO-2022-KR "," ISO-8859-1 "," ISO-8859-2 "," ISO-8859-3 "," ISO-8859-4 "," ISO-8859-5 "," ISO-8859-6 "," ISO-8859-7 "," ISO-8859-8 "," ISO-8859-9 "," ISO-8859-13 " , "ISO-8859-15", "WINDOWS-874", "WINDOWS-1250", "WINDOWS-1251", "WINDOWS-1252", "WINDOWS-1253", "WINDOWS-1254", "WINDOWS-1255", "WINDOWS-1256", "WINDOWS-1257", "WINDOWS-1258".| No       |
| sıkıştırma | Dosya sıkıştırmayı yapılandırmak için özellik grubu. Etkinlik yürütmesi sırasında sıkıştırma/açma işlemi yapmak istediğinizde bu bölümü yapılandırın. | No |
| tür<br/>( *altında `compression`* ) | JSON dosyalarını okumak/yazmak için kullanılan sıkıştırma codec bileşeni. <br>İzin verilen değerler **şunlardır bzip2** , **gzip** , **söndür** , **zipsöndür** , **targzip** , **tar** , **Snappy** veya **lz4** . Varsayılan değer sıkıştırılmaz.<br>**Şu anda** kopyalama etkinliği "Snappy" & "lz4" desteklemez ve eşleme veri akışı "zipsöndür" "," targzip "ve" tar "değerlerini desteklemez.<br>Örneğin, dosya tabanlı havuz veri deposuna **açılan** dosyaları açmak için kopyalama **etkinliği kullanılırken,** / **TarGzip** / **Tar** varsayılan dosyalar klasörüne çıkarılır: `<path specified in dataset>/<folder named as source compressed file>/` , `preserveZipFileNameAsFolder` / `preserveCompressionFileNameAsFolder` sıkıştırılmış dosyaların adının klasör yapısı olarak korunup korunmayacağını denetlemek için [etkinlik kaynağını kopyalama](#json-as-source) üzerinde kullanın.| Hayır.  |
| düzey<br/>( *altında `compression`* ) | Sıkıştırma oranı. <br>İzin verilen değerler **en iyi** veya **en hızlardır** .<br>- **En hızlı:** Elde edilen dosya en iyi şekilde sıkıştırılmasa bile, sıkıştırma işleminin mümkün olduğunca hızlı bir şekilde tamamlanmalıdır.<br>- **En iyi** : işlemin tamamlanmasını daha uzun sürse bile sıkıştırma işlemi en iyi şekilde sıkıştırılmalıdır. Daha fazla bilgi için bkz. [sıkıştırma düzeyi](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) konusu. | No       |

Azure Blob depolamada JSON veri kümesinin bir örneği aşağıda verilmiştir:

```json
{
    "name": "JSONDataset",
    "properties": {
        "type": "Json",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder",
            },
            "compression": {
                "type": "gzip"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, JSON kaynağı ve havuzu tarafından desteklenen özelliklerin bir listesini sağlar.

JSON dosyalarından veri ayıklama ve havuz veri deposu/biçimine eşleme ya da [şema eşlemesinden](copy-activity-schema-and-type-mapping.md)tam tersi hakkında bilgi edinin.

### <a name="json-as-source"></a>Kaynak olarak JSON

Aşağıdaki özellikler kopyalama etkinliği **_ \_ kaynağı \*** * bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| tür          | Kopyalama etkinliği kaynağının Type özelliği **Jsonsource** olarak ayarlanmalıdır. | Yes      |
| formatSettings | Bir özellik grubu. Aşağıdaki **JSON okuma ayarları** tablosuna bakın. | No       |
| storeSettings | Veri deposundan veri okuma hakkında bir özellik grubu. Her dosya tabanlı bağlayıcının, altında kendi desteklenen okuma ayarları vardır `storeSettings` . **Bağlayıcı makalesi-> kopyalama etkinliği özellikleri bölümünde ayrıntılara bakın** . | No       |

Şu koşullarda desteklenen **JSON okuma ayarları** `formatSettings` :

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| tür          | FormatSettings türü **Jsonreadsettings** olarak ayarlanmalıdır. | Yes      |
| compressionProperties | Belirli bir sıkıştırma codec bileşeni için verileri açmak üzere bir özellik grubu. | No       |
| preserveZipFileNameAsFolder<br>( *`compressionProperties` -> `type` as `ZipDeflateReadSettings`* )  | Giriş veri kümesi **Zipsöndür** sıkıştırma ile yapılandırıldığında geçerlidir. Kaynak ZIP dosya adının kopyalama sırasında klasör yapısı olarak korunup korunmayacağını gösterir.<br>- **True (varsayılan)** olarak ayarlandığında Data Factory daraltılmış dosyaları içine yazar `<path specified in dataset>/<folder named as source zip file>/` .<br>- **False** olarak ayarlandığında Data Factory ZIP dosyalarını doğrudan öğesine yazar `<path specified in dataset>` . Yarış veya beklenmedik davranışlara engel olmak için farklı kaynak ZIP dosyalarında yinelenen dosya adlarında bulunmadığından emin olun.  | No |
| preserveCompressionFileNameAsFolder<br>( *`compressionProperties` -> `type` `TarGZipReadSettings` veya `TarReadSettings` olarak* ) | Giriş veri kümesi **targık** sıkıştırması ile yapılandırıldığında geçerlidir / **Tar** . Kaynak sıkıştırılmış dosya adının kopyalama sırasında klasör yapısı olarak korunup korunmayacağını gösterir.<br>- **True (varsayılan)** olarak ayarlandığında Data Factory, açılan dosyaları içine yazar `<path specified in dataset>/<folder named as source compressed file>/` . <br>- **False** olarak ayarlandığında Data Factory açılan dosyaları doğrudan öğesine yazar `<path specified in dataset>` . Yarış veya beklenmedik davranışlara engel olmak için farklı kaynak dosyalarında yinelenen dosya adlarında bulunmadığından emin olun. | No |

### <a name="json-as-sink"></a>Havuz olarak JSON

Aşağıdaki özellikler kopyalama etkinliği **_ \_ Havuz \*** * bölümünde desteklenir.

| Özellik      | Açıklama                                                  | Gerekli |
| ------------- | ------------------------------------------------------------ | -------- |
| tür          | Kopyalama etkinliği kaynağının Type özelliği **Jsonsink** olarak ayarlanmalıdır. | Yes      |
| formatSettings | Bir özellik grubu. Aşağıdaki **JSON yazma ayarları** tablosuna bakın. | No       |
| storeSettings | Veri deposuna veri yazma hakkında bir özellik grubu. Her dosya tabanlı bağlayıcının altında kendi desteklenen yazma ayarları vardır `storeSettings` . **Bağlayıcı makalesi-> kopyalama etkinliği özellikleri bölümünde ayrıntılara bakın** . | No       |

Altında desteklenen **JSON yazma ayarları** `formatSettings` :

| Özellik      | Açıklama                                                  | Gerekli                                              |
| ------------- | ------------------------------------------------------------ | ----------------------------------------------------- |
| tür          | FormatSettings türü **Jsonwritesettings** olarak ayarlanmalıdır. | Yes                                                   |
| filePattern |Her bir JSON dosyasında depolanan verilerin desenini belirtir. İzin verilen değerler: **Setofobjects** (JSON satırları) ve **arrayofobjects** . **Varsayılan** değer **setOfObjects** olarak belirlenmiştir. Bu desenler hakkında ayrıntılı bilgi için bkz. [JSON dosyası desenleri](#json-file-patterns). |No |

### <a name="json-file-patterns"></a>JSON dosyası desenleri

JSON dosyalarından veri kopyalarken, kopyalama etkinliği aşağıdaki JSON dosyası desenlerini otomatik olarak algılayabilir ve ayrıştırılabilir. JSON dosyalarına veri yazarken kopyalama etkinliği havuzunda dosya modelini yapılandırabilirsiniz.

- **1. Tür: setOfObjects**

    Her dosya tek nesne, JSON satırları veya birleştirilmiş nesneler içerir.

    * **tek nesne JSON örneği**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        ```

    * **JSON satırları (havuz için varsayılan)**

        ```json
        {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
        {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
        {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
        ```

    * **bitiştirilmiş JSON örneği**

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        }
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
        ```

- **2. Tür: arrayOfObjects**

    Her dosya bir nesne dizisi içerir.

    ```json
    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
    ]
    ```

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Veri akışlarını eşleme bölümünde şu veri depolarında JSON biçimini okuyabilir ve yazabilirsiniz: [Azure Blob depolama](connector-azure-blob-storage.md#mapping-data-flow-properties), [Azure Data Lake Storage 1.](connector-azure-data-lake-store.md#mapping-data-flow-properties)ve [Azure Data Lake Storage 2.](connector-azure-data-lake-storage.md#mapping-data-flow-properties).

### <a name="source-properties"></a>Kaynak özellikleri

Aşağıdaki tabloda bir JSON kaynağı tarafından desteklenen özellikler listelenmiştir. Bu özellikleri **kaynak seçenekleri** sekmesinde düzenleyebilirsiniz.

| Ad | Açıklama | Gerekli | İzin verilen değerler | Veri akışı betiği özelliği |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Joker karakter yolları | Joker karakterle eşleşen tüm dosyalar işlenecek. Veri kümesinde ayarlanan klasör ve dosya yolunu geçersiz kılar. | hayır | String [] | Yavaya Cardyolları |
| Bölüm kök yolu | Bölümlenmiş dosya verileri için bölümlenmiş klasörleri sütun olarak okumak üzere bir bölüm kök yolu girebilirsiniz | hayır | Dize | Partitionrootyolu |
| Dosya listesi | Kaynağınızın işlenecek dosyaları listeleyen bir metin dosyasına işaret edip etmediğini belirtir | hayır | `true` veya `false` | Si |
| Dosya adının depolanacak sütun | Kaynak dosya adı ve yolu ile yeni bir sütun oluşturma | hayır | Dize | rowUrlColumn |
| Tamamlandıktan sonra | İşlemden sonra dosyaları silin veya taşıyın. Dosya yolu, kapsayıcı kökünden başlar | hayır | Sil: `true` veya `false` <br> Geçiş `['<from>', '<to>']` | purgeFiles <br> moveFiles |
| Son değiştirme ölçütü | En son değiştirildiklerinde dosyaları filtrelemek için seçin | hayır | Zaman damgası | Modıfıedafter <br> modifiedBefore |
| Tek belge | Veri akışlarını eşleme her dosyadan bir JSON belgesi okur | hayır | `true` veya `false` | singleDocument |
| Tırnak işaretleri olmayan sütun adları | **Tırnak Işaretleri olmayan sütun adları** seçiliyse, eşleme veri akışları tırnak IÇINE alınmış JSON sütunlarını okur. | hayır | `true` veya `false` |  unquotedColumnNames |
| Açıklamalar içeriyor | JSON verilerinde C veya C++ stili yorumu varsa, Select **yorumu vardır** | hayır | `true` veya `false` | asComments |
| Tek tırnaklı | Tırnak işaretleri içine almayan JSON sütunlarını okur | hayır | `true` veya `false` | Singletırnaklı |
| Ters eğik çizgi kaçışı | JSON verilerinde karakter kaçış için ters eğik çizgi kullanılırsa, **ters eğik çizgiden kaçı** seçin | hayır | `true` veya `false` | Biriktirme listesi |
| Dosya bulunamamış izin ver | True ise bir dosya bulunmazsa bir hata oluşturulmaz | hayır | `true` veya `false` | ıgnorenofilesfound |

### <a name="source-format-options"></a>Kaynak Biçim seçenekleri

Veri akışınızda kaynak olarak JSON veri kümesi kullanmak, beş ek ayar ayarlamanıza olanak sağlar. Bu ayarlar, **kaynak seçenekleri** sekmesindeki **JSON ayarları** Accordion altında bulunabilir.  

![JSON ayarları](media/data-flow/json-settings.png "JSON Ayarları")

#### <a name="default"></a>Varsayılan

Varsayılan olarak, JSON verileri aşağıdaki biçimde okunurdur.

```
{ "json": "record 1" }
{ "json": "record 2" }
{ "json": "record 3" }
```

#### <a name="single-document"></a>Tek belge

**Tek belge** seçilirse, eşleme veri akışları her dosyadan bir JSON belgesi okur. 

``` json
File1.json
{
    "json": "record 1"
}
File2.json
{
    "json": "record 2"
}
File3.json
{
    "json": "record 3"
}
```
> [!NOTE]
> Veri akışları JSON verileriniz görüntülenirken "corrupt_record" belirten bir hata oluşturanıyor, veriniz JSON dosyanızda tek bir belge içeriyor olabilir. "Tek belge" ayarı bu hatayı temizlemelidir.

#### <a name="unquoted-column-names"></a>Tırnak işaretleri olmayan sütun adları

**Tırnak Işaretleri olmayan sütun adları** seçiliyse, eşleme veri akışları tırnak IÇINE alınmış JSON sütunlarını okur. 

```
{ json: "record 1" }
{ json: "record 2" }
{ json: "record 3" }
```

#### <a name="has-comments"></a>Açıklamalar içeriyor

JSON verilerinde C veya C++ stili yorum oluşturma varsa, Select **yorumu vardır** .

``` json
{ "json": /** comment **/ "record 1" }
{ "json": "record 2" }
{ /** comment **/ "json": "record 3" }
```

#### <a name="single-quoted"></a>Tek tırnaklı

JSON alanları ve değerleri çift tırnak yerine tek tırnakları kullanıyorsa, **tek tırnak** işareti ' ni seçin.

```
{ 'json': 'record 1' }
{ 'json': 'record 2' }
{ 'json': 'record 3' }
```

#### <a name="backslash-escaped"></a>Ters eğik çizgi kaçışı

JSON verilerinde karakter kaçış için ters eğik çizgi kullanılırsa, **ters eğik çizgiden kaçı** seçin.

```
{ "json": "record 1" }
{ "json": "\} \" \' \\ \n \\n record 2" }
{ "json": "record 3" }
```

### <a name="sink-properties"></a>Havuz özellikleri

Aşağıdaki tabloda bir JSON havuzu tarafından desteklenen özellikler listelenmiştir. Bu özellikleri **Ayarlar** sekmesinde düzenleyebilirsiniz.

| Ad | Açıklama | Gerekli | İzin verilen değerler | Veri akışı betiği özelliği |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Klasörü temizle | Hedef klasör, yazma işleminden önce silinirse | hayır | `true` veya `false` | kesilemedi |
| Dosya adı seçeneği | Yazılan verilerin adlandırma biçimi. Varsayılan olarak, biçimdeki bölüm başına bir dosya `part-#####-tid-<guid>` | hayır | Model: dize <br> Bölüm başına: dize [] <br> Sütunda veri olarak: dize <br> Tek dosyaya çıkış: `['<fileName>']`  | filePattern <br> Partitionbir dosya adı <br> rowUrlColumn <br> Partitionbir dosya adı |

### <a name="creating-json-structures-in-a-derived-column"></a>Türetilmiş bir sütunda JSON yapıları oluşturma

Türetilmiş sütun ifade Oluşturucusu aracılığıyla veri akışınıza karmaşık bir sütun ekleyebilirsiniz. Türetilmiş sütun dönüşümünde yeni bir sütun ekleyin ve mavi kutuya tıklayarak ifade oluşturucuyu açın. Bir sütunu karmaşık hale getirmek için JSON yapısını el ile girebilir veya UX kullanarak alt sütunları etkileşimli olarak ekleyebilirsiniz.

#### <a name="using-the-expression-builder-ux"></a>İfade Oluşturucu UX 'i kullanma

Çıktı şeması yan bölmesinde bir sütunun üzerine gelin ve artı simgesine tıklayın. Sütunu karmaşık bir tür yapmak için **alt sütun Ekle** ' yi seçin.

![Alt sütun Ekle](media/data-flow/derive-add-subcolumn.png "Alt sütun Ekle")

Aynı şekilde ek sütunlar ve alt sütunlar ekleyebilirsiniz. Karmaşık olmayan her bir alan için, ifade düzenleyicisine sağa doğru bir ifade eklenebilir.

![Karmaşık sütun Ekle](media/data-flow/derive-complex-column.png "Sütun ekleme")

#### <a name="entering-the-json-structure-manually"></a>JSON yapısını el ile girme

JSON yapısını el ile eklemek için yeni bir sütun ekleyin ve düzenleyicide ifadeyi girin. İfade aşağıdaki genel biçimi izler:

```
@(
    field1=0,
    field2=@(
        field1=0
    )
)
```

Bu ifade "complexColumn" adlı bir sütun için girilmişse, havuza aşağıdaki JSON olarak yazılır:

```
{
    "complexColumn": {
        "field1": 0,
        "field2": {
            "field1": 0
        }
    }
}
```

#### <a name="sample-manual-script-for-complete-hierarchical-definition"></a>Tüm hiyerarşik tanım için örnek el ile betik
```
@(
    title=Title,
    firstName=FirstName,
    middleName=MiddleName,
    lastName=LastName,
    suffix=Suffix,
    contactDetails=@(
        email=EmailAddress,
        phone=Phone
    ),
    address=@(
        line1=AddressLine1,
        line2=AddressLine2,
        city=City,
        state=StateProvince,
        country=CountryRegion,
        postCode=PostalCode
    ),
    ids=[
        toString(CustomerID), toString(AddressID), rowguid
    ]
)
```

## <a name="next-steps"></a>Sonraki adımlar

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Veri akışını eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)