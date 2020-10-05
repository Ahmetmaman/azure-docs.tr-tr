---
title: 'Hızlı başlangıç: Azure Blob depolama kitaplığı v12-.NET'
description: Bu hızlı başlangıçta, blob (nesne) deposunda bir kapsayıcı ve BLOB oluşturmak üzere .NET için Azure Blob depolama istemci kitaplığı sürüm 12 ' yi nasıl kullanacağınızı öğrenirsiniz. Ardından, blob’u yerel bilgisayarınıza indirmeyi ve bir kapsayıcıdaki tüm blobların listesini görüntülemeyi öğreneceksiniz.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 07/24/2020
ms.service: storage
ms.subservice: blobs
ms.topic: quickstart
ms.custom: devx-track-csharp
ms.openlocfilehash: fa37db7a5c63f7a5e8a84d98afbb81e007904974
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "89001442"
---
# <a name="quickstart-azure-blob-storage-client-library-v12-for-net"></a>Hızlı Başlangıç: .NET için Azure Blob depolama istemci kitaplığı v12

.NET için Azure Blob depolama istemci kitaplığı V12 ile çalışmaya başlayın. Azure Blob depolama, Microsoft’un buluta yönelik nesne depolama çözümüdür. Paketi yüklemek ve temel görevler için örnek kodu denemek için adımları izleyin. Blob depolama, çok miktarda yapılandırılmamış veriyi depolamak için iyileştirilmiştir.

.NET için Azure Blob depolama istemci kitaplığı V12 ' nı kullanarak şunları yapın:

* Bir kapsayıcı oluşturma
* Azure Depolama'ya blob yükleme
* Bir kapsayıcıdaki tüm Blobları listeleme
* Blobu yerel bilgisayarınıza indirme
* Kapsayıcı silme

Ek kaynaklar:

* [API başvuru belgeleri](/dotnet/api/azure.storage.blobs)
* [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs)
* [Paket (NuGet)](https://www.nuget.org/packages/Azure.Storage.Blobs)
* [Örnekler](https://docs.microsoft.com/azure/storage/common/storage-samples-dotnet?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-samples)

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* Azure depolama hesabı- [depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)
* İşletim sisteminiz için geçerli [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) . Çalışma zamanını değil, SDK 'Yı aldığınızdan emin olun.

## <a name="setting-up"></a>Ayarlanıyor

Bu bölümde, bir projeyi .NET için Azure Blob depolama istemci kitaplığı V12 ile çalışacak şekilde hazırlama adımları gösterilmektedir.

### <a name="create-the-project"></a>Proje oluşturma

*BlobQuickstartV12*adlı bir .NET Core uygulaması oluşturun.

1. Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` *BlobQuickstartV12*adlı yeni bir konsol uygulaması oluşturmak için komutunu kullanın. Bu komut, tek bir kaynak dosyası olan basit bir "Merhaba Dünya" C# projesi oluşturur: *program.cs*.

   ```console
   dotnet new console -n BlobQuickstartV12
   ```

1. Yeni oluşturulan *BlobQuickstartV12* dizinine geçiş yapın.

   ```console
   cd BlobQuickstartV12
   ```

1. *BlobQuickstartV12* dizininde, *veri*adlı başka bir dizin oluşturun. Blob veri dosyalarının oluşturulup depolanacağı yerdir.

    ```console
    mkdir data
    ```

### <a name="install-the-package"></a>Paketi yükler

Hala uygulama dizininde, komutunu kullanarak .NET için Azure Blob Storage istemci kitaplığı 'nı yükleyebilirsiniz `dotnet add package` .

```console
dotnet add package Azure.Storage.Blobs
```

### <a name="set-up-the-app-framework"></a>Uygulama çerçevesini ayarlama

Proje dizininden:

1. *Program.cs* dosyasını Düzenleyicinizde açın
1. İfadeyi Kaldır `Console.WriteLine("Hello World!");`
1. `using`Yönergeler ekleme
1. `Main`Zaman uyumsuz kodu desteklemek için yöntem bildirimini güncelleştirme

Kod şu şekildedir:

```csharp
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Models;
using System;
using System.IO;
using System.Threading.Tasks;

namespace BlobQuickstartV12
{
    class Program
    {
        static async Task Main()
        {
        }
    }
}
```

[!INCLUDE [storage-quickstart-credentials-include](../../../includes/storage-quickstart-credentials-include.md)]

## <a name="object-model"></a>Nesne modeli

Azure Blob depolama, büyük miktarlarda yapılandırılmamış verileri depolamak için iyileştirilmiştir. Yapılandırılmamış veriler, metin veya ikili veriler gibi belirli bir veri modeline veya tanıma bağlı olmayan verilerdir. BLOB depolama üç tür kaynak sunar:

* Depolama hesabı
* Depolama hesabındaki bir kapsayıcı
* Kapsayıcıda bir blob

Aşağıdaki diyagramda bu kaynaklar arasındaki ilişki gösterilmektedir.

![BLOB depolama mimarisi diyagramı](./media/storage-blobs-introduction/blob1.png)

Şu kaynaklarla etkileşim kurmak için aşağıdaki .NET sınıflarını kullanın:

* [BlobServiceClient](/dotnet/api/azure.storage.blobs.blobserviceclient): `BlobServiceClient` sınıfı, Azure depolama kaynaklarını ve BLOB kapsayıcılarını değiştirmenize olanak sağlar.
* [Blobcontainerclient](/dotnet/api/azure.storage.blobs.blobcontainerclient): `BlobContainerClient` sınıfı, Azure depolama kapsayıcılarını ve bloblarını değiştirmenize olanak sağlar.
* [Blobclient](/dotnet/api/azure.storage.blobs.blobclient): `BlobClient` sınıfı, Azure Storage bloblarını değiştirmenize izin verir.
* [Blobdownloadınfo](/dotnet/api/azure.storage.blobs.models.blobdownloadinfo): `BlobDownloadInfo` sınıfı, blob indirilmeden döndürülen özellikleri ve içeriği temsil eder.

## <a name="code-examples"></a>Kod örnekleri

Bu örnek kod parçacıkları, .NET için Azure Blob depolama istemci kitaplığı ile aşağıdakilerin nasıl gerçekleştirileceğini göstermektedir:

* [Bağlantı dizesini alma](#get-the-connection-string)
* [Bir kapsayıcı oluşturma](#create-a-container)
* [Blobları bir kapsayıcıya yükleme](#upload-blobs-to-a-container)
* [Kapsayıcıdaki blobları listeleme](#list-the-blobs-in-a-container)
* [Blob’ları indirme](#download-blobs)
* [Kapsayıcı silme](#delete-a-container)

### <a name="get-the-connection-string"></a>Bağlantı dizesini alma

Aşağıdaki kod, depolama [bağlantı dizesini yapılandırma](#configure-your-storage-connection-string) bölümünde oluşturulan ortam değişkeninden depolama hesabının bağlantı dizesini alır.

Bu kodu metodun içine ekleyin `Main` :

```csharp
Console.WriteLine("Azure Blob storage v12 - .NET quickstart sample\n");

// Retrieve the connection string for use with the application. The storage
// connection string is stored in an environment variable on the machine
// running the application called AZURE_STORAGE_CONNECTION_STRING. If the
// environment variable is created after the application is launched in a
// console or with Visual Studio, the shell or application needs to be closed
// and reloaded to take the environment variable into account.
string connectionString = Environment.GetEnvironmentVariable("AZURE_STORAGE_CONNECTION_STRING");
```

### <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Yeni kapsayıcı için bir ad belirleyin. Aşağıdaki kod, benzersiz olduğundan emin olmak için kapsayıcı adına bir GUID değeri ekler.

> [!IMPORTANT]
> Kapsayıcı adlarının küçük harfle yazılması gerekir. Kapsayıcıları ve blobları adlandırma hakkında daha fazla bilgi için bkz. [Kapsayıcıları, Blobları ve Meta Verileri Adlandırma ve Bunlara Başvurma](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

[BlobServiceClient](/dotnet/api/azure.storage.blobs.blobserviceclient) sınıfının bir örneğini oluşturun. Ardından, depolama hesabınızda kapsayıcıyı oluşturmak için [Createblobcontainsısync](/dotnet/api/azure.storage.blobs.blobserviceclient.createblobcontainerasync) yöntemini çağırın.

Bu kodu yönteminin sonuna ekleyin `Main` :

```csharp
// Create a BlobServiceClient object which will be used to create a container client
BlobServiceClient blobServiceClient = new BlobServiceClient(connectionString);

//Create a unique name for the container
string containerName = "quickstartblobs" + Guid.NewGuid().ToString();

// Create the container and return a container client object
BlobContainerClient containerClient = await blobServiceClient.CreateBlobContainerAsync(containerName);
```

### <a name="upload-blobs-to-a-container"></a>Blobları bir kapsayıcıya yükleme

Aşağıdaki kod parçacığı:

1. Yerel *veri* dizininde bir metin dosyası oluşturur.
1. Kapsayıcı [oluşturma](#create-a-container) bölümünde, kapsayıcıda [getblobclient](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobclient) yöntemini çağırarak bir [blobclient](/dotnet/api/azure.storage.blobs.blobclient) nesnesine bir başvuru alır.
1. [Uploadasync](/dotnet/api/azure.storage.blobs.blobclient.uploadasync#Azure_Storage_Blobs_BlobClient_UploadAsync_System_IO_Stream_System_Boolean_System_Threading_CancellationToken_) yöntemini çağırarak yerel metin dosyasını bloba yükler. Bu yöntem, daha önce oluşturulmadıysa bir blob oluşturur, aksi takdirde üzerine yazar.

Bu kodu yönteminin sonuna ekleyin `Main` :

```csharp
// Create a local file in the ./data/ directory for uploading and downloading
string localPath = "./data/";
string fileName = "quickstart" + Guid.NewGuid().ToString() + ".txt";
string localFilePath = Path.Combine(localPath, fileName);

// Write text to the file
await File.WriteAllTextAsync(localFilePath, "Hello, World!");

// Get a reference to a blob
BlobClient blobClient = containerClient.GetBlobClient(fileName);

Console.WriteLine("Uploading to Blob storage as blob:\n\t {0}\n", blobClient.Uri);

// Open the file and upload its data
using FileStream uploadFileStream = File.OpenRead(localFilePath);
await blobClient.UploadAsync(uploadFileStream, true);
uploadFileStream.Close();
```

### <a name="list-the-blobs-in-a-container"></a>Kapsayıcıdaki blobları listeleme

[Getblobsasync](/dotnet/api/azure.storage.blobs.blobcontainerclient.getblobsasync) yöntemini çağırarak kapsayıcıdaki Blobları listeleyin. Bu durumda, kapsayıcıya yalnızca bir blob eklenmiş, bu nedenle listeleme işlemi yalnızca bir BLOB döndürüyor.

Bu kodu yönteminin sonuna ekleyin `Main` :

```csharp
Console.WriteLine("Listing blobs...");

// List all blobs in the container
await foreach (BlobItem blobItem in containerClient.GetBlobsAsync())
{
    Console.WriteLine("\t" + blobItem.Name);
}
```

### <a name="download-blobs"></a>Blob’ları indirme

[Downloadasync](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.downloadasync) yöntemini çağırarak önceden oluşturulmuş blobu indirin. Örnek kod, yerel dosya sisteminde her iki dosyayı da görebilmeniz için dosya adına "INDIRILMIŞ" bir sonek ekler.

Bu kodu yönteminin sonuna ekleyin `Main` :

```csharp
// Download the blob to a local file
// Append the string "DOWNLOADED" before the .txt extension 
// so you can compare the files in the data directory
string downloadFilePath = localFilePath.Replace(".txt", "DOWNLOADED.txt");

Console.WriteLine("\nDownloading blob to\n\t{0}\n", downloadFilePath);

// Download the blob's contents and save it to a file
BlobDownloadInfo download = await blobClient.DownloadAsync();

using (FileStream downloadFileStream = File.OpenWrite(downloadFilePath))
{
    await download.Content.CopyToAsync(downloadFileStream);
    downloadFileStream.Close();
}
```

### <a name="delete-a-container"></a>Kapsayıcı silme

Aşağıdaki kod, [DeleteAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.deleteasync)kullanarak tüm kapsayıcıyı silerek uygulamanın oluşturduğu kaynakları temizler. Ayrıca, uygulama tarafından oluşturulan yerel dosyaları da siler.

Uygulama, `Console.ReadLine` BLOB, kapsayıcı ve yerel dosyaları silmeden önce çağırarak kullanıcı girişi için duraklatılır. Bu, kaynakların silinmeden önce gerçekten doğru şekilde oluşturulduğunu doğrulamak iyi bir şansınız olur.

Bu kodu yönteminin sonuna ekleyin `Main` :

```csharp
// Clean up
Console.Write("Press any key to begin clean up");
Console.ReadLine();

Console.WriteLine("Deleting blob container...");
await containerClient.DeleteAsync();

Console.WriteLine("Deleting the local source and downloaded files...");
File.Delete(localFilePath);
File.Delete(downloadFilePath);

Console.WriteLine("Done");
```

## <a name="run-the-code"></a>Kodu çalıştırma

Bu uygulama yerel *veri* klasörünüzde bir sınama dosyası oluşturur ve BLOB depolamaya yükler. Örnek daha sonra kapsayıcıdaki Blobları listeler ve eski ve yeni dosyaları karşılaştırabilmeniz için dosyayı yeni bir adla indirir.

Uygulama dizininize gidip uygulamayı derleyin ve çalıştırın.

```console
dotnet build
```

```console
dotnet run
```

Uygulamanın çıktısı aşağıdaki örneğe benzer:

```output
Azure Blob storage v12 - .NET quickstart sample

Uploading to Blob storage as blob:
         https://mystorageacct.blob.core.windows.net/quickstartblobs60c70d78-8d93-43ae-954d-8322058cfd64/quickstart2fe6c5b4-7918-46cb-96f4-8c4c5cb2fd31.txt

Listing blobs...
        quickstart2fe6c5b4-7918-46cb-96f4-8c4c5cb2fd31.txt

Downloading blob to
        ./data/quickstart2fe6c5b4-7918-46cb-96f4-8c4c5cb2fd31DOWNLOADED.txt

Press any key to begin clean up
Deleting blob container...
Deleting the local source and downloaded files...
Done
```

Temizleme işlemine başlamadan önce, iki dosya için *veri* klasörünüzü denetleyin. Dosyaları açarak aynı olduklarını görebilirsiniz.

Dosyaları doğruladıktan sonra, test dosyalarını silmek ve tanıtımı sona almak için **ENTER** tuşuna basın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta .NET kullanarak blobları karşıya yükleme, indirme ve listeleme hakkında bilgi edindiniz.

BLOB depolama örnek uygulamalarını görmek için devam edin:

> [!div class="nextstepaction"]
> [Azure Blob Storage SDK v12 .NET örnekleri](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Azure.Storage.Blobs/samples)

* Öğreticiler, örnekler, hızlı ve diğer belgelerde, [.net ve .NET Core geliştiricileri Için Azure](/dotnet/azure/)' u ziyaret edin.
* .NET Core hakkında daha fazla bilgi için bkz. [10 dakika içinde .NET kullanmaya başlama](https://www.microsoft.com/net/learn/get-started/).
