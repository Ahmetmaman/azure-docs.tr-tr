---
title: '& sorgusu oluşturma Azure Data Lake Analytics-Azure CLı'
description: Azure Data Lake Analytics bir hesap oluşturmak ve U-SQL işini göndermek için Azure komut satırı arabirimini nasıl kullanacağınızı öğrenin.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 06/18/2017
ms.custom: devx-track-azurecli
ms.openlocfilehash: 9123ca6a1bfa90737bb1ce83ee365d1ecf514e1f
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92221001"
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-cli"></a>Azure CLI kullanarak Azure Data Lake Analytics ile çalışmaya başlama

[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Bu makalede, Azure Data Lake Analytics hesapları oluşturmak, USQL işleri ve katalogları göndermek için Azure CLı komut satırı arabirimi 'nin nasıl kullanılacağı açıklanır. İş, sekmeyle ayrılmış değerler (TSV) dosyasını okur ve bu dosyayı virgülle ayrılmış değerler (CSV) dosyasına dönüştürür.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki öğelerin olması gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* Bu makalede, Azure CLı sürüm 2,0 veya üstünü çalıştırıyor olmanız gerekir. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure aboneliğinizde oturum açmak için:

```azurecli
az login
```

Bir URL’ye gitmeniz ve bir kimlik doğrulama kodu girmeniz istenir.  Sonra yönergeleri izleyerek kimlik bilgilerinizi girin.

Oturum açtıktan sonra, oturum komutu aboneliklerinizi listeler.

Belirli bir aboneliği kullanmak için:

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-data-lake-analytics-account"></a>Data Lake Analytics hesabı oluşturma

Herhangi bir işi çalıştırmadan önce bir Data Lake Analytics hesabına sahip olmanız gerekir. Bir Data Lake Analytics hesabı oluşturmak için aşağıdaki öğeleri belirtmeniz gerekir:

* **Azure Kaynak grubu**. Azure Kaynak Grubu'nda bir Data Lake Analytics hesabı oluşturulmalıdır. [Azure Resource Manager](../azure-resource-manager/management/overview.md) , uygulamanızdaki kaynaklarla bir grup olarak çalışmanıza olanak sağlar. Uygulamanıza ilişkin tüm kaynakları tek ve eşgüdümlü bir işlem ile dağıtabilir, güncelleştirebilir veya silebilirsiniz.  

Aboneliğiniz altındaki mevcut kaynak gruplarını listelemek için:

```azurecli
az group list
```

Yeni bir kaynak grubu oluşturmak için:

```azurecli
az group create --name "<Resource Group Name>" --location "<Azure Location>"
```

* **Data Lake Analytics hesap adı**. Her Data Lake Analytics hesabının bir adı vardır.
* **Konum**. Data Lake Analytics'i destekleyen Azure veri merkezlerinden birini kullanın.
* **Varsayılan Data Lake Store hesabı**: her Data Lake Analytics hesabının varsayılan bir Data Lake Store hesabı vardır.

Mevcut bir Data Lake Store hesabını listelemek için:

```azurecli
az dls account list
```

Yeni bir Data Lake Store hesabı oluşturmak için:

```azurecli
az dls account create --account "<Data Lake Store Account Name>" --resource-group "<Resource Group Name>"
```

Bir Data Lake Analytics hesabı oluşturmak için aşağıdaki söz dizimini kullanın:

```azurecli
az dla account create --account "<Data Lake Analytics Account Name>" --resource-group "<Resource Group Name>" --location "<Azure location>" --default-data-lake-store "<Default Data Lake Store Account Name>"
```

Hesap oluşturduktan sonra aşağıdaki komutları kullanarak hesapları listeleyebilir ve hesap ayrıntılarını görüntüleyebilirsiniz:

```azurecli
az dla account list
az dla account show --account "<Data Lake Analytics Account Name>"
```

## <a name="upload-data-to-data-lake-store"></a>Data Lake Store'a veri yükleme

Bu eğiticide, bazı arama günlüklerini işleyeceksiniz.  Arama günlüğü, Data Lake Store veya Azure Blob depolama alanında depolanabilir.

Azure portalı, bir arama günlüğü dosyası içeren bazı örnek veri dosyalarını varsayılan Data Lake Store hesabına kopyalamak için bir kullanıcı arabirimi sağlar. Verileri varsayılan Data Lake Store hesabına yüklemek için bkz. [Kaynak verileri hazırlama](data-lake-analytics-get-started-portal.md).

Azure CLı kullanarak dosyaları karşıya yüklemek için aşağıdaki komutları kullanın:

```azurecli
az dls fs upload --account "<Data Lake Store Account Name>" --source-path "<Source File Path>" --destination-path "<Destination File Path>"
az dls fs list --account "<Data Lake Store Account Name>" --path "<Path>"
```

Data Lake Analytics ayrıca Azure Blob depolama alanına da erişebilir.  Verileri Azure Blob depolama alanına yüklemek için bkz. [Azure CLI'yı Azure Storage ile kullanma](../storage/blobs/storage-quickstart-blobs-cli.md).

## <a name="submit-data-lake-analytics-jobs"></a>Data Lake Analytics işlerini gönderme

Data Lake Analytics işleri, U-SQL dilinde yazılır. U-SQL hakkında daha fazla bilgi için bkz. [U-SQL dili ile çalışmaya başlama](data-lake-analytics-u-sql-get-started.md) ve [U-SQL dili başvurusu](/u-sql/).

### <a name="to-create-a-data-lake-analytics-job-script"></a>Bir Data Lake Analytics işi betiği oluşturmak için

Aşağıdaki U-SQL betiği ile bir metin dosyası oluşturun ve metin dosyasını iş istasyonunuza kaydedin:

```usql
@a  =
    SELECT * FROM
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

Bu U-SQL betiği, **Extractors.Tsv()** öğesini kullanarak kaynak veri dosyasını okur ve ardından **Outputters.Csv()** öğesini kullanarak bir csv dosyası oluşturur.

Kaynak dosyayı farklı bir konuma kopyalamadıkça bu iki yolu değiştirmeyin.  Data Lake Analytics, mevcut olmaması halinde çıkış klasörünü oluşturur.

Varsayılan Data Lake Store hesaplarında depolanan dosyalar için göreli yolların kullanılması daha basittir. Mutlak yol da kullanabilirsiniz.  Örnek:

```usql
adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
```

Mutlak yolları, bağlı Storagehesaplarındaki dosyalara erişmek için kullanmanız gerekir.  Bağlı Azure Storage hesabında depolanan dosyalar için söz dizimi şu şekildedir:

```usql
wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv
```

> [!NOTE]
> Genel bloblar ile Azure Blob kapsayıcısı desteklenmiyor.
> Genel kapsayıcılar ile Azure Blob kapsayıcısı desteklenmiyor.
>

### <a name="to-submit-jobs"></a>İşleri göndermek için

Bir işi göndermek için aşağıdaki söz dizimini kullanın.

```azurecli
az dla job submit --account "<Data Lake Analytics Account Name>" --job-name "<Job Name>" --script "<Script Path and Name>"
```

Örnek:

```azurecli
az dla job submit --account "myadlaaccount" --job-name "myadlajob" --script @"C:\DLA\myscript.txt"
```

### <a name="to-list-jobs-and-show-job-details"></a>İşleri listelemek ve iş ayrıntılarını görüntülemek için

```azurecli
az dla job list --account "<Data Lake Analytics Account Name>"
az dla job show --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

### <a name="to-cancel-jobs"></a>İşleri iptal etmek için

```azurecli
az dla job cancel --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

## <a name="retrieve-job-results"></a>İş sonuçlarını alma

Bir iş tamamlandıktan sonra, aşağıdaki cmdlet'leri kullanarak çıkış dosyalarını listeleyebilir ve dosyaları indirebilirsiniz:

```azurecli
az dls fs list --account "<Data Lake Store Account Name>" --source-path "/Output" --destination-path "<Destination>"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv" --length 128 --offset 0
az dls fs download --account "<Data Lake Store Account Name>" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destination-path "<Destination Path and File Name>"
```

Örnek:

```azurecli
az dls fs download --account "myadlsaccount" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destination-path "C:\DLA\myfile.csv"
```

## <a name="next-steps"></a>Sonraki adımlar

* Data Lake Analytics Azure CLı başvuru belgesini görmek için bkz. [Data Lake Analytics](/cli/azure/dla).
* Data Lake Store Azure CLı başvuru belgesini görmek için bkz. [Data Lake Store](/cli/azure/dls).
* Daha karmaşık bir sorgu görmek için [Azure Data Lake Analytics'i kullanarak Web sitesi günlüklerini çözümleme](data-lake-analytics-analyze-weblogs.md) makalesine bakın.