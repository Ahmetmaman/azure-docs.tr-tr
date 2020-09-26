---
title: Öğretici-BLOB depolama ile yüksek oranda kullanılabilir bir uygulama oluşturma
titleSuffix: Azure Storage
description: Uygulama verilerinizi yüksek oranda kullanılabilir hale getirmek için Read-Access coğrafi bölge-yedekli (RA-GZRS) depolama alanını kullanın.
services: storage
author: tamram
ms.service: storage
ms.topic: tutorial
ms.date: 04/16/2020
ms.author: tamram
ms.reviewer: artek
ms.custom: mvc, devx-track-python, devx-track-js, devx-track-csharp
ms.subservice: blobs
ms.openlocfilehash: 90b3135174d06b6e896d50e0db13fa7747e882c0
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91295456"
---
# <a name="tutorial-build-a-highly-available-application-with-blob-storage"></a>Öğretici: BLOB depolama ile yüksek oranda kullanılabilir bir uygulama oluşturma

Bu öğretici, bir dizinin birinci bölümüdür. Burada, uygulama verilerinizi Azure 'da yüksek oranda kullanılabilir hale getirme hakkında bilgi edineceksiniz.

Bu öğreticiyi tamamladığınızda, [Okuma Erişimli Coğrafi bölge-yedekli](../common/storage-redundancy.md) (ra-GZRS) depolama hesabından bir blobu yükleyen ve alan bir konsol uygulamasına sahip olursunuz.

Azure depolamada coğrafi yedeklilik, işlemleri birincil bir bölgeden zaman uyumsuz olarak yüzlerce mil olan ikincil bir bölgeye çoğaltır. Bu çoğaltma işlemi, ikincil bölgedeki verilerin nihai olarak tutarlı olmasını sağlar. Konsol uygulaması, hangi uç noktanın bağlanacağı için [devre kesici](/azure/architecture/patterns/circuit-breaker) modelini kullanır, otomatik olarak, başarısızlık ve kurtarmalar arasında uç noktalar arasında geçiş yapılır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

Serinin birinci bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Depolama hesabı oluşturma
> * Bağlantı dizesini ayarlama
> * Konsol uygulamasını çalıştırma

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

# <a name="net"></a>[.NET](#tab/dotnet)

* **Azure geliştirme** iş yüküyle [Visual Studio 2019](https://www.visualstudio.com/downloads/) ' ü yükler.

  ![Azure geliştirme (Web ve Bulut altında)](media/storage-create-geo-redundant-storage/workloads.png)

# <a name="python"></a>[Python](#tab/python)

* [Python](https://www.python.org/downloads/) 'ı yükler
* [Python Için Azure depolama SDK 'sını](https://github.com/Azure/azure-storage-python) indirme ve yükleme

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

* [Node.js](https://nodejs.org)'i yükler.

---

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Depolama hesabı, Azure Storage veri nesnelerinizi depolamak ve erişmek için benzersiz bir ad alanı sağlar.

Okuma Erişimli Coğrafi bölge-yedekli (RA-GZRS) depolama hesabı oluşturmak için aşağıdaki adımları izleyin:

1. Azure portal **kaynak oluştur** düğmesini seçin.
2. **Yeni** sayfadan **depolama hesabı-blob, dosya, tablo, kuyruk ' ı** seçin.
4. Aşağıdaki bilgileri kullanarak depolama hesabı formunu alttaki resimde gösterildiği gibi doldurun ve **Oluştur**’u seçin:

   | Ayar       | Örnek değer | Açıklama |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Abonelik** | *Aboneliğim* | Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.azure.com/Subscriptions). |
   | **adlı yönetilen örnek,** | *myResourceGroup* | Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming). |
   | **Ad** | *mystorageaccount* | Depolama hesabınız için benzersiz bir ad. |
   | **Konum** | *Doğu ABD* | Konum seçin. |
   | **Performans** | *Standart* | Standart performans, örnek senaryo için iyi bir seçenektir. |
   | **Hesap türü** | *StorageV2* | Genel amaçlı v2 depolama hesabı kullanılması önerilir. Azure depolama hesabı türleri hakkında daha fazla bilgi için bkz. [depolama hesabına genel bakış](../common/storage-account-overview.md). |
   | **Çoğaltma**| *Okuma erişimli coğrafi alanlar arası yedekli depolama (RA-GZRS)* | Birincil bölge, bölgesel olarak yedekli olur ve ikincil bölgeye okuma erişimi olan ikincil bir bölgeye çoğaltılır. |
   | **Erişim katmanı**| *Sık Erişimli* | Sık erişilen veriler için sık kullanılan katmanı kullanın. |

    ![depolama hesabı oluşturma](media/storage-create-geo-redundant-storage/createragrsstracct.png)

## <a name="download-the-sample"></a>Örneği indirme

# <a name="net"></a>[.NET](#tab/dotnet)

[Örnek projeyi indirin](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip) ve storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.zip dosyasını ayıklayın (sıkıştırmasını açın). Geliştirme ortamına uygulamanın bir kopyasını indirmek için [git](https://git-scm.com/) de kullanılabilir. Örnek proje bir konsol uygulaması içerir.

```bash
git clone https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.git
```

# <a name="python"></a>[Python](#tab/python)

[Örnek projeyi indirin](https://github.com/Azure-Samples/storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip) ve storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs.zip dosyasını ayıklayın (sıkıştırmasını açın). Geliştirme ortamına uygulamanın bir kopyasını indirmek için [git](https://git-scm.com/) de kullanılabilir. Örnek proje temel bir Python uygulaması içerir.

```bash
git clone https://github.com/Azure-Samples/storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs.git
```

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

[Örnek projeyi indirin](https://github.com/Azure-Samples/storage-node-v10-ha-ra-grs) ve dosyayı ayıklayın. Geliştirme ortamına uygulamanın bir kopyasını indirmek için [git](https://git-scm.com/) de kullanılabilir. Örnek proje, temel bir Node.js uygulaması içerir.

```bash
git clone https://github.com/Azure-Samples/storage-node-v10-ha-ra-grs
```

---

## <a name="configure-the-sample"></a>Örneği yapılandırma

# <a name="net"></a>[.NET](#tab/dotnet)

Bu uygulamada, depolama hesabınız için bağlantı dizesi sağlamanız gerekir. Bu bağlantı dizesini uygulamayı çalıştıran yerel makine üzerindeki bir ortam değişkeninde depolayabilirsiniz. Ortam değişkenini oluşturmak için İşletim Sisteminize bağlı olarak aşağıdaki örneklerden birini izleyin.

Azure portalında depolama hesabınıza gidin. Depolama hesabınızdaki **Ayarlar** bölümünde **Erişim anahtarları**’nı seçin. Birincil veya ikincil anahtardaki **bağlantı dizesini** kopyalayın. İşletim sisteminize bağlı olarak, \<yourconnectionstring\> gerçek bağlantı dizenizle değiştirerek aşağıdaki komutlardan birini çalıştırın. Bu komut, yerel makinede bir ortam değişkeni kaydeder. Windows 'da, kullanmakta olduğunuz **komut istemi** veya kabuğu yeniden yükleyene kadar ortam değişkeni kullanılamaz.

### <a name="linux"></a>Linux

```
export storageconnectionstring=<yourconnectionstring>
```

### <a name="windows"></a>Windows

```powershell
setx storageconnectionstring "<yourconnectionstring>"
```

# <a name="python"></a>[Python](#tab/python)

Uygulamada, depolama hesabı kimlik bilgilerinizi sağlamanız gerekir. Bu bilgileri, uygulamayı çalıştıran yerel makinedeki ortam değişkenlerine kaydedebilirsiniz. Ortam değişkenlerini oluşturmak için Işletim sisteminize bağlı olarak aşağıdaki örneklerden birini izleyin.

Azure portalında depolama hesabınıza gidin. Depolama hesabınızdaki **Ayarlar** bölümünde **Erişim anahtarları**’nı seçin. **Depolama hesabı adını** ve **anahtar** değerlerini aşağıdaki komutlara yapıştırın \<youraccountname\> ve \<youraccountkey\> yer tutucuları değiştirin. Bu komut, ortam değişkenlerini yerel makineye kaydeder. Windows 'da, kullanmakta olduğunuz **komut istemi** veya kabuğu yeniden yükleyene kadar ortam değişkeni kullanılamaz.

### <a name="linux"></a>Linux

```
export accountname=<youraccountname>
export accountkey=<youraccountkey>
```

### <a name="windows"></a>Windows

```powershell
setx accountname "<youraccountname>"
setx accountkey "<youraccountkey>"
```

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

Bu örneği çalıştırmak için, depolama hesabı kimlik bilgilerinizi `.env.example` dosyaya eklemeniz ve sonra olarak yeniden adlandırmanız gerekir `.env` .

```
AZURE_STORAGE_ACCOUNT_NAME=<replace with your storage account name>
AZURE_STORAGE_ACCOUNT_ACCESS_KEY=<replace with your storage account access key>
```

Bu bilgileri, depolama hesabınıza giderek ve **Ayarlar** bölümünde **erişim anahtarları** ' nı seçerek Azure Portal bulabilirsiniz.

Gerekli bağımlılıkları yükler. Bunu yapmak için bir komut istemi açın, örnek klasöre gidin ve girin `npm install` .

---

## <a name="run-the-console-application"></a>Konsol uygulamasını çalıştırma

# <a name="net"></a>[.NET](#tab/dotnet)

Visual Studio 'da **F5** tuşuna basın veya uygulamada hata ayıklamaya başlamak için **Başlat** ' ı seçin. Visual Studio, yapılandırıldıysa eksik NuGet paketlerini otomatik olarak geri yükler, daha fazla bilgi edinmek için [paketleri yükleme ve yeniden yükleme paketini geri yükleyin](https://docs.microsoft.com/nuget/consume-packages/package-restore#package-restore-overview) .

Bir konsol penceresi açılır ve uygulama çalışmaya başlar. Uygulama, çözümdeki **HelloWorld.png** resmini depolama hesabına yükler. Uygulama, görüntünün ikincil RA-GZRS uç noktasına çoğaltıldığından emin olmak için kontrol eder. Ardından, resmi 999 kereye kadar indirmeye başlar. Her okuma bir **P** veya **S**tarafından temsil edilir. Burada **P** , birincil uç noktayı temsil eder ve **S** ikincil uç noktayı temsil eder.

![Çalışan konsol uygulaması](media/storage-create-geo-redundant-storage/figure3.png)

Örnek kodda, [DownloadToFileAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.downloadtofileasync) yöntemini kullanarak depolama hesabından bir resim indirmek için `Program.cs` dosyasındaki `RunCircuitBreakerAsync` görevi kullanılmaktadır. İndirilmesinden önce, bir [OperationContext](/dotnet/api/microsoft.azure.cosmos.table.operationcontext) tanımlanmıştır. İşlem bağlamı, indirme işlemi başarıyla tamamlandığında veya indirme işlemi başarısız olup yeniden denendiğinde başlatılan olay işleyicilerini tanımlar.

# <a name="python"></a>[Python](#tab/python)

Uygulamayı bir terminalde veya komut isteminde çalıştırmak için **circuitbreaker.py** dizinine gidip `python circuitbreaker.py` komutunu girin. Uygulama, çözümdeki **HelloWorld.png** resmini depolama hesabına yükler. Uygulama, görüntünün ikincil RA-GZRS uç noktasına çoğaltıldığından emin olmak için kontrol eder. Ardından, resmi 999 kereye kadar indirmeye başlar. Her okuma bir **P** veya **S**tarafından temsil edilir. Burada **P** , birincil uç noktayı temsil eder ve **S** ikincil uç noktayı temsil eder.

![Çalışan konsol uygulaması](media/storage-create-geo-redundant-storage/figure3.png)

Örnek kodda, [get_blob_to_path](https://docs.microsoft.com/python/api/azure-storage-blob/azure.storage.blob.baseblobservice.baseblobservice?view=azure-python-previous#get-blob-to-path-container-name--blob-name--file-path--open-mode--wb---snapshot-none--start-range-none--end-range-none--validate-content-false--progress-callback-none--max-connections-2--lease-id-none--if-modified-since-none--if-unmodified-since-none--if-match-none--if-none-match-none--timeout-none-) yöntemini kullanarak depolama hesabından bir resim indirmek için `circuitbreaker.py` dosyasındaki `run_circuit_breaker` yöntemi kullanılmaktadır.

Depolama nesnesi yeniden deneme işlevi, doğrusal bir yeniden deneme ilkesine ayarlıdır. Yeniden deneme işlevi, isteklerin yeniden denenip denenmeyeceğini belirler ve isteği yeniden denemeden önce kaç saniye bekleneceğini belirtir. Birincile yapılan istek başarısız olursa aynı isteğin ikincile yeniden denenmesini istiyorsanız **retry\_to\_secondary** değerini true olarak ayarlayın. Örnek uygulamada depolama nesnesinin `retry_callback` işlevinde özel bir yeniden deneme ilkesi tanımlanmıştır.

İndirilmadan önce, Service Object [retry_callback](https://docs.microsoft.com/python/api/azure-storage-common/azure.storage.common.storageclient.storageclient?view=azure-python) ve [response_callback](https://docs.microsoft.com/python/api/azure-storage-common/azure.storage.common.storageclient.storageclient?view=azure-python) işlevi tanımlanmıştır. Bu işlevler, indirme işlemi başarıyla tamamlandığında veya indirme işlemi başarısız olup yeniden denendiğinde başlatılan olay işleyicilerini tanımlar.

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

Örneği çalıştırmak için bir komut istemi açın, örnek klasöre gidin ve girin `node index.js` .

Örnek, BLOB depolama hesabınızda bir kapsayıcı oluşturur, kapsayıcıya **HelloWorld.png** yükler ve sonra kapsayıcının ve görüntünün ikincil bölgeye çoğaltılıp çoğaltılmadığını sürekli olarak denetler. Çoğaltma sonrasında, indirmek veya çıkmak için **D** veya **Q** (sonra ENTER ile) girmenizi ister. Çıktın aşağıdaki örneğe benzer şekilde görünmesi gerekir:

```
Created container successfully: newcontainer1550799840726
Uploaded blob: HelloWorld.png
Checking to see if container and blob have replicated to secondary region.
[0] Container has not replicated to secondary region yet: newcontainer1550799840726 : ContainerNotFound
[1] Container has not replicated to secondary region yet: newcontainer1550799840726 : ContainerNotFound
...
[31] Container has not replicated to secondary region yet: newcontainer1550799840726 : ContainerNotFound
[32] Container found, but blob has not replicated to secondary region yet.
...
[67] Container found, but blob has not replicated to secondary region yet.
[68] Blob has replicated to secondary region.
Ready for blob download. Enter (D) to download or (Q) to quit, followed by ENTER.
> D
Attempting to download blob...
Blob downloaded from primary endpoint.
> Q
Exiting...
Deleted container newcontainer1550799840726
```

---

## <a name="understand-the-sample-code"></a>Örnek kodu anlama

### <a name="net"></a>[.NET](#tab/dotnet)

### <a name="retry-event-handler"></a>Yeniden deneme olay işleyicisi

`OperationContextRetrying` olay işleyicisi, resmin indirilmesi işlemi başarısız olmuşsa ve yeniden denenecek şekilde ayarlanmışsa çağrılır. Uygulamada tanımlı yeniden deneme sayısı üst sınırına ulaşılmışsa isteğin [LocationMode](/dotnet/api/microsoft.azure.storage.blob.blobrequestoptions.locationmode) değeri `SecondaryOnly` olarak değiştirilir. Bu ayar, uygulamayı resmi ikincil uç noktadan indirmeyi denemeye zorlar. Birincil uç nokta süresiz olarak yeniden denenmediği için bu yapılandırma resmin istenmesinde harcanan süreyi azaltmış olur.

```csharp
private static void OperationContextRetrying(object sender, RequestEventArgs e)
{
    retryCount++;
    Console.WriteLine("Retrying event because of failure reading the primary. RetryCount = " + retryCount);

    // Check if we have had more than n retries in which case switch to secondary.
    if (retryCount >= retryThreshold)
    {

        // Check to see if we can fail over to secondary.
        if (blobClient.DefaultRequestOptions.LocationMode != LocationMode.SecondaryOnly)
        {
            blobClient.DefaultRequestOptions.LocationMode = LocationMode.SecondaryOnly;
            retryCount = 0;
        }
        else
        {
            throw new ApplicationException("Both primary and secondary are unreachable. Check your application's network connection. ");
        }
    }
}
```

### <a name="request-completed-event-handler"></a>İstek tamamlandı olay işleyicisi

`OperationContextRequestCompleted` olay işleyicisi, resmin indirilmesi işlemi başarılı olduğunda çağrılır. Uygulama ikincil uç noktayı kullanıyorsa bu uç noktayı 20 kereye kadar daha kullanmaya devam eder. 20 kereden sonra uygulama [LocationMode](/dotnet/api/microsoft.azure.storage.blob.blobrequestoptions.locationmode) değerini yeniden `PrimaryThenSecondary` olarak ayarlar ve birincil uç noktayı tekrar dener. Bir istek başarılı olursa uygulama birincil uç noktadan okumaya devam eder.

```csharp
private static void OperationContextRequestCompleted(object sender, RequestEventArgs e)
{
    if (blobClient.DefaultRequestOptions.LocationMode == LocationMode.SecondaryOnly)
    {
        // You're reading the secondary. Let it read the secondary [secondaryThreshold] times,
        //    then switch back to the primary and see if it's available now.
        secondaryReadCount++;
        if (secondaryReadCount >= secondaryThreshold)
        {
            blobClient.DefaultRequestOptions.LocationMode = LocationMode.PrimaryThenSecondary;
            secondaryReadCount = 0;
        }
    }
}
```

### <a name="python"></a>[Python](#tab/python)

### <a name="retry-event-handler"></a>Yeniden deneme olay işleyicisi

`retry_callback` olay işleyicisi, resmin indirilmesi işlemi başarısız olmuşsa ve yeniden denenecek şekilde ayarlanmışsa çağrılır. Uygulamada tanımlı yeniden deneme sayısı üst sınırına ulaşılmışsa isteğin [LocationMode](https://docs.microsoft.com/python/api/azure-storage-common/azure.storage.common.models.locationmode?view=azure-python) değeri `SECONDARY` olarak değiştirilir. Bu ayar, uygulamayı resmi ikincil uç noktadan indirmeyi denemeye zorlar. Birincil uç nokta süresiz olarak yeniden denenmediği için bu yapılandırma resmin istenmesinde harcanan süreyi azaltmış olur.

```python
def retry_callback(retry_context):
    global retry_count
    retry_count = retry_context.count
    sys.stdout.write(
        "\nRetrying event because of failure reading the primary. RetryCount= {0}".format(retry_count))
    sys.stdout.flush()

    # Check if we have more than n-retries in which case switch to secondary
    if retry_count >= retry_threshold:

        # Check to see if we can fail over to secondary.
        if blob_client.location_mode != LocationMode.SECONDARY:
            blob_client.location_mode = LocationMode.SECONDARY
            retry_count = 0
        else:
            raise Exception("Both primary and secondary are unreachable. "
                            "Check your application's network connection.")
```

### <a name="request-completed-event-handler"></a>İstek tamamlandı olay işleyicisi

`response_callback` olay işleyicisi, resmin indirilmesi işlemi başarılı olduğunda çağrılır. Uygulama ikincil uç noktayı kullanıyorsa bu uç noktayı 20 kereye kadar daha kullanmaya devam eder. 20 kereden sonra uygulama [LocationMode](https://docs.microsoft.com/python/api/azure-storage-common/azure.storage.common.models.locationmode?view=azure-python) değerini yeniden `PRIMARY` olarak ayarlar ve birincil uç noktayı tekrar dener. Bir istek başarılı olursa uygulama birincil uç noktadan okumaya devam eder.

```python
def response_callback(response):
    global secondary_read_count
    if blob_client.location_mode == LocationMode.SECONDARY:

        # You're reading the secondary. Let it read the secondary [secondaryThreshold] times,
        # then switch back to the primary and see if it is available now.
        secondary_read_count += 1
        if secondary_read_count >= secondary_threshold:
            blob_client.location_mode = LocationMode.PRIMARY
            secondary_read_count = 0
```

### <a name="nodejs"></a>[Node.js](#tab/nodejs)

Node.js Ile v10 arasındaki SDK ile geri çağırma işleyicileri gereksizdir. Bunun yerine, örnek, yeniden deneme seçenekleriyle yapılandırılmış bir işlem hattı ve ikincil bir uç nokta oluşturur. Bu, birincil işlem hattı aracılığıyla verilerinize ulaşamazsa uygulamanın otomatik olarak ikincil işlem hattına geçiş yapmasına olanak tanır.

```javascript
const accountName = process.env.AZURE_STORAGE_ACCOUNT_NAME;
const storageAccessKey = process.env.AZURE_STORAGE_ACCOUNT_ACCESS_KEY;
const sharedKeyCredential = new SharedKeyCredential(accountName, storageAccessKey);

const primaryAccountURL = `https://${accountName}.blob.core.windows.net`;
const secondaryAccountURL = `https://${accountName}-secondary.blob.core.windows.net`;

const pipeline = StorageURL.newPipeline(sharedKeyCredential, {
  retryOptions: {
    maxTries: 3,
    tryTimeoutInMs: 10000,
    retryDelayInMs: 500,
    maxRetryDelayInMs: 1000,
    secondaryHost: secondaryAccountURL
  }
});
```

---

## <a name="next-steps"></a>Sonraki adımlar

Serinin birinci bölümünde, RA-GZRS depolama hesaplarıyla bir uygulamayı yüksek oranda kullanılabilir hale getirme hakkında bilgi edindiniz.

Bir hatanın benzetimini yapmayı öğrenmek ve uygulamanızı ikincil RA-GZRS uç noktasını kullanmak üzere zorlamak için serinin ikinci bölümüne ilerleyin.

> [!div class="nextstepaction"]
> [Birincil bölgeden okurken hata benzetimi yap](simulate-primary-region-failure.md)
