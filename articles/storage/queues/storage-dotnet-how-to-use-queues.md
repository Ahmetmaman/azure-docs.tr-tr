---
title: .NET - Azure Depolama'yı kullanarak Azure Kuyruk depolamasına başlayın
description: Azure Queues, uygulama bileşenleri arasında güvenilir ve zaman uyumsuz mesajlaşma sağlar. Bulut mesajlaşma özelliği uygulama bileşenlerinizin bağımsız olarak ölçeklendirilmesini sağlar.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 05/21/2019
ms.service: storage
ms.subservice: queues
ms.topic: conceptual
ms.reviewer: cbrooks
ms.openlocfilehash: 0806c1101c0bc93a1b917cb2d18709721ff0c6d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75968289"
---
# <a name="get-started-with-azure-queue-storage-using-net"></a>.NET kullanarak Azure Kuyruk Depolamaya başlayın

[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../../includes/storage-check-out-samples-dotnet.md)]

## <a name="overview"></a>Genel Bakış

Azure Queue depolama birimi, uygulama bileşenleri arasında bulut mesajlaşma özelliği sağlar. Ölçeklendirmek üzere uygulama tasarlarken, uygulama bileşenleri birbirinden bağımsız şekilde ölçeklenebilmek için genellikle birbirinden ayrılır. Kuyruk depolama bulutta, masaüstünde, şirket içi sunucuda veya mobil bir cihazda çalışan uygulama bileşenleri arasındaki iletişim için zaman uyumsuz mesajlaşma sunar. Kuyruk depolama ayrıca zaman uyumsuz görevlerin yönetilmesini ve süreç iş akışlarının oluşturulmasını destekler.

### <a name="about-this-tutorial"></a>Bu öğretici hakkında

Bu öğreti, Azure kuyruk depolama kullanarak bazı genel senaryolar için .NET kodunun nasıl yazılacağını göstermektedir. Kapsanan senaryolara kuyruk oluşturma ve silme ile kuyruk iletileri ekleme, okuma ve silme dahildir.

**Tahmini tamamlanma süresi:** 45 dakika

### <a name="prerequisites"></a>Ön koşullar

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [.NET için Azure Depolama ortak istemci kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.Common/)
* [.NET için Azure Depolama Sırası istemci kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.Queue/)
* [.NET için Azure Yapılandırma Yöneticisi](https://www.nuget.org/packages/Microsoft.Azure.ConfigurationManager/)
* Azure [depolama hesabı](../common/storage-account-create.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

Ardından, geliştirme ortamınızı Visual Studio’da ayarlayın; böylece bu kılavuzdaki kod örneklerini denemeye hazır olursunuz.

### <a name="create-a-windows-console-application-project"></a>Windows konsol uygulaması projesi oluşturma

Visual Studio'da yeni bir Windows konsol uygulaması oluşturun. Aşağıdaki adımlar Visual Studio 2019'da nasıl bir konsol uygulaması oluşturabileceğinizi gösterir. Adımlar Visual Studio’nun diğer sürümlerinde de benzerdir.

1. **Dosya** > **Yeni** > **Proje** seçin
2. **Platform** > **Pencerelerini** Seçin
3. **Konsol Uygulaması (.NET Framework)** öğesini seçin
4. **Sonraki'ni** Seçin
5. Proje **adı** alanına, başvurunuz için bir ad girin
6. **Oluştur**’u seçin

Bu öğreticideki tüm kod örnekleri konsol uygulamanızın **Program.cs** dosyasının **Main()** yöntemine eklenebilir.

Azure Depolama istemci kitaplıklarını, Azure bulut hizmeti veya web uygulaması ve masaüstü ve mobil uygulamalar da dahil olmak üzere her türlü .NET uygulamasında kullanabilirsiniz. Bu kılavuzda, sadeleştirmek için konsol uygulaması kullanmaktayız.

### <a name="use-nuget-to-install-the-required-packages"></a>Gereken paketleri yüklemek için NuGet kullanma

Bu öğreticiyi tamamlamak için projenizdeki aşağıdaki üç pakete başvurmanız gerekir:

* [.NET için Microsoft Azure Depolama Ortak İstemci Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.Common/): Bu paket, depolama hesabınızdaki veri kaynaklarına programlı erişim sağlar.
* [.NET için Microsoft Azure Depolama Sırası Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.Queue/): Bu istemci kitaplığı, istemci tarafından erişilebilen iletileri depolamak için Microsoft Azure Depolama Kuyruğu hizmetiyle çalışmayı sağlar.
* [.NET için Microsoft Azure Configuration Manager Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.ConfigurationManager/): Bu paket, uygulamanızın nerede çalıştığına bakmaksızın yapılandırma dosyasından bağlantı dizesini ayrıştırmak için bir sınıf sağlar.

Bu paketleri elde etmek için NuGet'i kullanabilirsiniz. Şu adımları uygulayın:

1. **Solution Explorer'da**projenize sağ tıklayın ve **NuGet Paketlerini Yönet'i**seçin.
2. **Gözat'ı** seçin
3. "Microsoft.Azure.Storage.Queue" için çevrimiçi arama yapın ve Depolama istemcikitaplığını ve bağımlılıklarını yüklemek için **Yükle'yi** seçin. Bu, sıra kitaplığı bağımlılığı olan Microsoft.Azure.Storage.Common kitaplığını da yükler.
4. "Microsoft.Azure.ConfigurationManager" için çevrimiçi arama yapın ve Azure Configuration Manager'ı yüklemek için **Yükle'yi** seçin.

> [!NOTE]
> Depolama istemci kitaplıkları paketleri de [.NET için Azure SDK'ya](https://azure.microsoft.com/downloads/)dahildir. Ancak, her zaman en son sürümlere sahip olduğundan emin olmak için NuGet'deki Depolama istemci kitaplıklarını da yüklemenizi öneririz.
>
> .NET için Depolama istemci kitaplıklarında oDataLib bağımlılıkları, WCF Veri Hizmetleri'nden değil, NuGet'de bulunan ODataLib paketleri tarafından çözülür. ODataLib kitaplıkları NuGet aracılığıyla doğrudan indirilebilir veya kod projenizle başvurulabilir. Depolama istemcikitaplıkları tarafından kullanılan belirli ODataLib paketleri [OData](https://nuget.org/packages/Microsoft.Data.OData/), [Edm](https://nuget.org/packages/Microsoft.Data.Edm/)ve [Uzamsal](https://nuget.org/packages/System.Spatial/). Bu kitaplıklar Azure Tablo depolama sınıfları tarafından kullanılırken, Depolama istemci kitaplıklarıyla programlama için gereken bağımlılıklardır.

### <a name="determine-your-target-environment"></a>Hedef ortamınızı saptama

Bu kılavuzdaki örnekleri çalıştırmak için iki ortam seçeneğiniz vardır:

* Kodunuzu buluttaki bir Azure Storage hesabına karşı çalıştırabilirsiniz.
* Kodunuzu Azure Storage öykünücüsüne karşı çalıştırabilirsiniz. Depolama öykünücüsü, buluttaki Azure Storage hesabına öykünen bir yerel ortamdır. Öykünücü, uygulamanız geliştirildiği sırada kodunuzu test etmek ve hatalarını ayıklamak için bağımsız bir seçenektir. Öykünücü iyi bilinen hesabı ve anahtarı kullanır. Daha fazla bilgi için bkz. Geliştirme [ve sınama için Azure depolama emülatörü kullanın.](../common/storage-use-emulator.md)

Buluttaki bir depolama hesabını hedefliyorsanız, depolama hesabınız için birincil erişim anahtarını Azure portalından kopyalayın. Daha fazla bilgi için [bkz.](../common/storage-account-keys-manage.md)

> [!NOTE]
> Azure Storage ile ilişkili maliyetlerin oluşmasını önlemek için depolama öykünücüsünü hedefleyebilirsiniz. Ancak, buluttaki bir Azure Storage hesabını hedeflemeyi seçerseniz, bu öğreticiyi gerçekleştirme maliyetleri göz ardı edilecektir.

### <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma

Depolama hizmetlerine erişmek için uç noktaları ve kimlik bilgilerini yapılandırmak için bir depolama bağlantı dizesi kullanarak .NET desteği için Azure Depolama istemcikitaplıkları. Depolama bağlantı dizenizi korumanın en iyi yolu bir yapılandırma dosyasında tutmaktır.

Bağlantı dizeleri hakkında daha fazla bilgi için [bkz.](../common/storage-configure-connection-string.md)

> [!NOTE]
> Depolama hesabı anahtarınız depolama hesabınızın kök parolasına benzer. Depolama hesabı anahtarınızı korumak için her zaman özen gösterin. Diğer kullanıcılara dağıtmaktan, sabit kodlamaktan ve başkalarının erişebileceği düz metin dosyasına kaydetmekten kaçının. Anahtarınızın tehlikede olduğunu düşünüyorsanız, Azure portalını kullanarak hesap anahtarınızı yeniden oluşturun.

Bağlantı dizenizi yapılandırmak için Visual Studio'daki Solution Explorer'ın **app.config** dosyasını açın. Aşağıda gösterilen ** \<appSettings\> ** öğesinin içeriğini ekleyin. *Hesap adını* depolama hesabınızın adıyla ve *hesap anahtarınızı* hesap erişim anahtarınızla değiştirin:

```xml
<configuration>
    <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
    </startup>
    <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key" />
    </appSettings>
</configuration>
```

Örneğin, yapılandırma ayarınız şunun gibi görünür:

```xml
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=GMuzNHjlB3S9itqZJHHCnRkrokLkcSyW7yK9BRbGp0ENePunLPwBgpxV1Z/pVo9zpem/2xSHXkMqTHHLcx8XRA==" />
```

Depolama öykünücüsünü hedeflemek için iyi bilinen hesap adıyla ve anahtarıyla eşleşen bir kısayolu kullanabilirsiniz. Bu durumda, bağlantı dizesi ayarı şöyle olur:

```xml
<add key="StorageConnectionString" value="UseDevelopmentStorage=true;" />
```

### <a name="add-using-directives"></a>Using yönergeleri ekleme

Aşağıdaki `using` yönergelerini `Program.cs` dosyasının üst tarafına ekleyin:

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.Azure.Storage; // Namespace for CloudStorageAccount
using Microsoft.Azure.Storage.Queue; // Namespace for Queue storage types
```

### <a name="copy-your-credentials-from-the-azure-portal"></a>Azure portalından kimlik bilgilerinizi kopyalama

Örnek kodun, depolama hesabınıza erişim için yetki vermesi gerekir. Yetki vermek için, depolama hesabınızın kimlik bilgilerini bir bağlantı dizesi şeklinde uygulamaya sağlarsınız. Depolama hesabınızın kimlik bilgilerini görüntülemek için:

1. [Azure portalına](https://portal.azure.com) gidin.
2. Depolama hesabınızı bulun.
3. Depolama hesabına genel bakışın **Ayarlar** bölümünde **Erişim anahtarları**’nı seçin. Hesap erişim anahtarlarınız ve her bir anahtar için tam bağlantı dizesi görüntülenir.
4. **key1** bölümünde **Bağlantı dizesi** değerini bulun ve **Kopyala** düğmesine tıklayarak bağlantı dizesini kopyalayın. Sonraki adımda bir ortam değişkenine bağlantı dizesini ekleyeceksiniz.

    ![Azure portalından bağlantı dizesinin kopyalanmasını gösteren ekran görüntüsü](media/storage-dotnet-how-to-use-queues/portal-connection-string.png)

### <a name="parse-the-connection-string"></a>Bağlantı dizesini ayrıştırma

[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-queue-service-client"></a>Kuyruk hizmeti istemcisi oluşturma

[CloudQueueClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueueclient?view=azure-dotnet) sınıfı, Kuyruk depolamada depolanan kuyrukları almanızı sağlar. Hizmet istemcisini oluşturma yöntemlerinden biri aşağıda verilmiştir:

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
```

Artık Kuyruk depolamadan veri okuyan ve bu depolamaya veri yazan kodu yazmaya hazırsınız.

## <a name="create-a-queue"></a>Bir kuyruk oluşturma

Bu örnek, zaten yoksa, nasıl bir kuyruk oluşturulacağını gösterir:

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a container.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Create the queue if it doesn't already exist
queue.CreateIfNotExists();
```

## <a name="insert-a-message-into-a-queue"></a>Kuyruğa bir ileti yerleştirme

Varolan bir sıraya bir ileti yerleştirmek için ilk olarak yeni bir [CloudQueueMessage](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage?view=azure-dotnet) oluşturun. Ardından [AddMessage](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueue.addmessage?view=azure-dotnet) yöntemini çağırın. **CloudQueueMessage** bir dizeden (UTF-8 biçiminde) veya bir **bayt** dizisinden oluşturulabilir. Burada, bir kuyruk oluşturan (eğer yoksa) ve 'Hello, World' iletisini yerleştiren bir kod yer almaktadır:

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Create the queue if it doesn't already exist.
queue.CreateIfNotExists();

// Create a message and add it to the queue.
CloudQueueMessage message = new CloudQueueMessage("Hello, World");
queue.AddMessage(message);
```

## <a name="peek-at-the-next-message"></a>Sonraki iletiye gözatın

[PeekMessage](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueue.peekmessage?view=azure-dotnet) yöntemini çağırarak iletiyi kuyruktan kaldırmadan kuyruğun önündeki iletiye göz atabilirsiniz.

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Peek at the next message
CloudQueueMessage peekedMessage = queue.PeekMessage();

// Display message.
Console.WriteLine(peekedMessage.AsString);
```

## <a name="change-the-contents-of-a-queued-message"></a>Kuyruğa alınan iletinin içeriğini değiştirme

Kuyrukta yer alan bir iletinin içeriğini değiştirebilirsiniz. Eğer ileti bir iş görevini temsil ediyorsa, bu özelliği kullanarak iş görevinin durumunu güncelleştirebilirsiniz. Aşağıdaki kod kuyruk iletisini yeni içeriklerle güncelleştirir ve görünürlük zaman aşımını 60 saniye daha uzatır. Bu, ileti ile ilişkili işin durumunu kaydeder ve istemciye ileti üzerinde çalışmaya devam etmesi için bir dakika daha zaman verir. Bir işleme adımı donanım veya yazılım arızasından dolayı başarısız olursa baştan başlamanıza gerek kalmadan kuyruk iletilerindeki çok adımlı iş akışlarını izlemek için bu yöntemi kullanabilirsiniz. Genellikle bir yeniden deneme sayacı tutmanı gerekir ve bir ileti *n* seferden daha fazla yeniden denenirse, silebilirsiniz. Bu, her işlendiğinde bir uygulama hatası tetikleyen bir iletiye karşı koruma sağlar.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Get the message from the queue and update the message contents.
CloudQueueMessage message = queue.GetMessage();
message.SetMessageContent2("Updated contents.", false);
queue.UpdateMessage(message,
    TimeSpan.FromSeconds(60.0),  // Make it invisible for another 60 seconds.
    MessageUpdateFields.Content | MessageUpdateFields.Visibility);
```

## <a name="de-queue-the-next-message"></a>Sonraki iletiyi sıradan çıkarmak

Kodunuz, bir iletiyi bir kuyruktan iki adımda çıkarır. [GetMessage](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueue.getmessage?view=azure-dotnet)’ı çağırdığınzda, bir kuyruktaki bir sonraki iletiyi alırsınız. **GetMessage**’dan dönen bir ileti bu kuyruktaki kod okuyan iletilere karşı görünmez olur. Varsayılan olarak bu ileti 30 saniye görünmez kalır. İletiyi kuyruktan kaldırmayı tamamlamak için ayrıca [DeleteMessage](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueue.deletemessage?view=azure-dotnet)’ı çağırmanız gerekir. Bir iletinin iki adımlı kaldırılma süreci, donanım veya yazılım arızasından dolayı kodunuzun bir iletiyi işleyememesi durumunda kodunuzun başka bir örneğinin aynı iletiyi alıp yeniden denemesini sağlar. Kodunuz ileti işlendikten hemen sonra **DeleteMessage**’ı çağırır.

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Get the next message
CloudQueueMessage retrievedMessage = queue.GetMessage();

//Process the message in less than 30 seconds, and then delete the message
queue.DeleteMessage(retrievedMessage);
```

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a>Genel Kuyruk depolama API’leri ile Zaman Uyumsuz-Bekleme yöntemini kullanma

Bu örnek, genel Kuyruk depolama API’leri ile Zaman Uyumsuz-Bekleme yönteminin nasıl kullanılacağını gösterir. Örnek, her yönteme eklenen *Async* soneki ile belirtildiği şekilde kullanılan yöntemlerin her birinin zaman uyumsuz sürümlerini çağırır. Zaman uyumsuz bir yöntem kullanıldığında, zaman uyumsuz-bekleme yöntemi çağrı tamamlanana kadar yerel çalıştırmayı askıya alır. Bu davranış geçerli iş parçacığının başka işler yapmasını sağlar ve böylece performans sorunlarını engellemeye yardımcı olur, uygulamanızın genel yanıt hızını iyileştirir. .NET’te Zaman Uyumsuz-Bekleme yönteminin kullanılması ile ilgili daha fazla ayrıntı için bkz. [Zaman Uyumsuz ve Bekleme (C# ve Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

```csharp
// Create the queue if it doesn't already exist
if(await queue.CreateIfNotExistsAsync())
{
    Console.WriteLine("Queue '{0}' Created", queue.Name);
}
else
{
    Console.WriteLine("Queue '{0}' Exists", queue.Name);
}

// Create a message to put in the queue
CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

// Async enqueue the message
await queue.AddMessageAsync(cloudQueueMessage);
Console.WriteLine("Message added");

// Async dequeue the message
CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

// Async delete the message
await queue.DeleteMessageAsync(retrievedMessage);
Console.WriteLine("Deleted message");
```

## <a name="leverage-additional-options-for-de-queuing-messages"></a>İletilerin kuyruktan çıkarılması için ek seçenekleri kullanma

İletilerin bir kuyruktan alınma şeklini iki yöntemle özelleştirebilirsiniz. İlk olarak toplu iletiler alabilirsiniz (en fazla 32). İkinci olarak daha uzun veya daha kısa bir görünmezlik süresi ayarlayarak kodunuzun her iletiyi tamamen işlemesi için daha az veya daha fazla zaman tanıyabilirsiniz. Aşağıdaki kod örneğinde tek çağrıda 20 ileti almak için [GetMessages](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueue.getmessages?view=azure-dotnet) yöntemi kullanılmıştır. Ardından her ileti bir **foreach** döngüsü ile işlenir. Ayrıca her ileti için görünmezlik zaman aşımı beş dakika olarak ayarlanır. 5 dakikalık sürenin tüm iletiler için aynı zamanda başladığını unutmayın, bu nedenle **GetMessages** çağrısından itibaren 5 dakika geçtikten sonra silinmeyen tüm iletiler görünür olacaktır.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
{
    // Process all messages in less than 5 minutes, deleting each message after processing.
    queue.DeleteMessage(message);
}
```

## <a name="get-the-queue-length"></a>Kuyruk uzunluğu alma

Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz. [FetchAttributes](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueue.fetchattributes?view=azure-dotnet) yöntemi, ileti sayısı dahil olmak üzere Kuyruk hizmetinden kuyruk özniteliklerini almasını ister. [ApproximateMessageCount](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueue.approximatemessagecount?view=azure-dotnet) özelliği Kuyruk hizmetini çağırmadan **FetchAttributes** yöntemi tarafından alınan en son değeri döndürür.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Fetch the queue attributes.
queue.FetchAttributes();

// Retrieve the cached approximate message count.
int? cachedMessageCount = queue.ApproximateMessageCount;

// Display number of messages.
Console.WriteLine("Number of messages in queue: " + cachedMessageCount);
```

## <a name="delete-a-queue"></a>Bir kuyruk silme

Bir kuyruğu ve içinde yer alan tüm iletileri silmek için kuyruk nesnesindeki [Sil](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueue.delete?view=azure-dotnet) yöntemini çağırın.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Delete the queue.
queue.Delete();
```

## <a name="next-steps"></a>Sonraki adımlar

Kuyruk depolamanın temellerini öğrendiğinize göre, daha karmaşık depolama görevleri hakkında daha fazla bilgi edinmek için bu bağlantıları takip edin:

* Kullanılabilir API’ler ile ilgili eksiksiz bilgiler için Kuyruk hizmeti başvuru belgelerini görüntüleyin:
  * [.NET başvurusu için Depolama İstemci Kitaplığı](https://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
  * [REST API başvurusu](https://msdn.microsoft.com/library/azure/dd179355)
* [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki) kullanarak Azure Storage ile birlikte çalışmak üzere yazdığınız kodları nasıl sadeleştireceğinizi öğrenin.
* Azure’da veri depolama ile ilgili ek seçenekler hakkında daha fazla bilgi edinmek için daha fazla özellik kılavuzu görüntüleyin.
  * Yapılandırılmış verileri depolamak için [.NET kullanarak Azure Table Storage’ı kullanmaya başlayın](../../cosmos-db/table-storage-how-to-use-dotnet.md).
  * Yapılandırılmamış verileri depolamak için [.NET kullanarak Azure Blob Storage’ı kullanmaya başlayın](../blobs/storage-dotnet-how-to-use-blobs.md).
  * İlişkisel verileri depolamak için [.NET (C#) kullanarak SQL Veritabanı'na bağlanın](../../sql-database/sql-database-connect-query-dotnet-core.md).

[Download and install the Azure SDK for .NET]: /develop/net/
[.NET client library reference]: https://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
[Creating an Azure Project in Visual Studio]: https://msdn.microsoft.com/library/azure/ee405487.aspx
[Azure Storage Team Blog]: https://blogs.msdn.com/b/windowsazurestorage/
[OData]: https://nuget.org/packages/Microsoft.Data.OData/5.0.2
[Edm]: https://nuget.org/packages/Microsoft.Data.Edm/5.0.2
[Spatial]: https://nuget.org/packages/System.Spatial/5.0.2
