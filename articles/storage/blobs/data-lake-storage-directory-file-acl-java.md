---
title: "& ACL 'Ler için Java SDK Azure Data Lake Storage 2."
description: Hiyerarşik ad alanı (HNS) etkin olan depolama hesaplarında dizinleri ve dosya ve Dizin erişim denetim listelerini (ACL) yönetmek için Java için Azure depolama kitaplıklarını kullanın.
author: normesta
ms.service: storage
ms.date: 09/10/2020
ms.custom: devx-track-java
ms.author: normesta
ms.topic: how-to
ms.subservice: data-lake-storage-gen2
ms.reviewer: prishet
ms.openlocfilehash: f6e8219f744a91628f9860f0af133c07eddb4253
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95913394"
---
# <a name="use-java-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. içinde dizinleri, dosyaları ve ACL 'Leri yönetmek için Java kullanın

Bu makalede, Java kullanarak hiyerarşik ad alanı (HNS) etkinleştirilmiş depolama hesaplarında Dizin, dosya ve izinleri oluşturma ve bunları yönetme işlemi gösterilmektedir. 

[Paket (Maven)](https://search.maven.org/artifact/com.azure/azure-storage-file-datalake)  |  [Örnekler](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake)  |  [API başvurusu](/java/api/overview/azure/storage-file-datalake-readme)  |  [Gen1 to Gen2 Mapping](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake/GEN1_GEN2_MAPPING.md)  |  [Geri bildirimde](https://github.com/Azure/azure-sdk-for-java/issues) bulunun

## <a name="prerequisites"></a>Önkoşullar

> [!div class="checklist"]
> * Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
> * Hiyerarşik ad alanı (HNS) etkin olan bir depolama hesabı. Bir tane oluşturmak için [Bu](../common/storage-account-create.md) yönergeleri izleyin.

## <a name="set-up-your-project"></a>Projenizi ayarlama

Başlamak için [Bu sayfayı](https://search.maven.org/artifact/com.azure/azure-storage-file-datalake) açın ve Java kitaplığı 'nın en son sürümünü bulun. Sonra, *pom.xml* dosyasını metin düzenleyicinizde açın. Bu sürüme başvuran bir bağımlılık öğesi ekleyin.

İstemci uygulamanızın kimlik doğrulamasını Azure Active Directory (AD) kullanarak ve sonra Azure gizli Istemci kitaplığına bir bağımlılık ekleyin. Bkz.  [projenize gizli Istemci kitaplığı paketi ekleme](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity#adding-the-package-to-your-project).

Ardından, bu Imports deyimlerini kod dosyanıza ekleyin.

```java
import com.azure.core.credential.TokenCredential;
import com.azure.storage.common.StorageSharedKeyCredential;
import com.azure.storage.file.datalake.DataLakeDirectoryClient;
import com.azure.storage.file.datalake.DataLakeFileClient;
import com.azure.storage.file.datalake.DataLakeFileSystemClient;
import com.azure.storage.file.datalake.DataLakeServiceClient;
import com.azure.storage.file.datalake.DataLakeServiceClientBuilder;
import com.azure.storage.file.datalake.models.ListPathsOptions;
import com.azure.storage.file.datalake.models.PathAccessControl;
import com.azure.storage.file.datalake.models.PathAccessControlEntry;
import com.azure.storage.file.datalake.models.PathItem;
import com.azure.storage.file.datalake.models.PathPermissions;
import com.azure.storage.file.datalake.models.RolePermissions;
```

## <a name="connect-to-the-account"></a>Hesaba Bağlan 

Bu makaledeki kod parçacıklarını kullanmak için depolama hesabını temsil eden bir **DataLakeServiceClient** örneği oluşturmanız gerekir. 

### <a name="connect-by-using-an-account-key"></a>Hesap anahtarı kullanarak bağlan

Bu, bir hesaba bağlanmanın en kolay yoludur. 

Bu örnek, bir hesap anahtarı kullanarak bir **DataLakeServiceClient** örneği oluşturur.

```java

static public DataLakeServiceClient GetDataLakeServiceClient
(String accountName, String accountKey){

    StorageSharedKeyCredential sharedKeyCredential =
        new StorageSharedKeyCredential(accountName, accountKey);

    DataLakeServiceClientBuilder builder = new DataLakeServiceClientBuilder();

    builder.credential(sharedKeyCredential);
    builder.endpoint("https://" + accountName + ".dfs.core.windows.net");

    return builder.buildClient();
}      
```

### <a name="connect-by-using-azure-active-directory-azure-ad"></a>Azure Active Directory kullanarak bağlanma (Azure AD)

Uygulamanızı Azure AD ile doğrulamak için [Azure kimlik istemci kitaplığı](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) 'nı kullanabilirsiniz.

Bu örnek, bir istemci KIMLIĞI, bir istemci parolası ve bir kiracı KIMLIĞI kullanarak bir **DataLakeServiceClient** örneği oluşturur.  Bu değerleri almak için bkz. [bir istemci uygulamasından istekleri yetkilendirmek Için Azure AD 'den belirteç alma](../common/storage-auth-aad-app.md).

```java
static public DataLakeServiceClient GetDataLakeServiceClient
    (String accountName, String clientId, String ClientSecret, String tenantID){

    String endpoint = "https://" + accountName + ".dfs.core.windows.net";
        
    ClientSecretCredential clientSecretCredential = new ClientSecretCredentialBuilder()
    .clientId(clientId)
    .clientSecret(ClientSecret)
    .tenantId(tenantID)
    .build();
           
    DataLakeServiceClientBuilder builder = new DataLakeServiceClientBuilder();
    return builder.credential(clientSecretCredential).endpoint(endpoint).buildClient();
 } 
```

> [!NOTE]
> Daha fazla örnek için bkz. [Java Için Azure kimlik istemci kitaplığı](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) belgeleri.


## <a name="create-a-container"></a>Kapsayıcı oluşturma

Bir kapsayıcı dosyalarınız için bir dosya sistemi görevi görür. **DataLakeServiceClient. createFileSystem** metodunu çağırarak bir tane oluşturabilirsiniz.

Bu örnek adlı bir kapsayıcı oluşturur `my-file-system` . 

```java
static public DataLakeFileSystemClient CreateFileSystem
(DataLakeServiceClient serviceClient){

    return serviceClient.createFileSystem("my-file-system");
}
```

## <a name="create-a-directory"></a>Dizin oluşturma

**Datalakefilesystemclient. createDirectory** metodunu çağırarak bir dizin başvurusu oluşturun.

Bu örnek, bir kapsayıcıya adlı bir dizin ekler `my-directory` ve sonra adlı bir alt dizin ekler `my-subdirectory` . 

```java
static public DataLakeDirectoryClient CreateDirectory
(DataLakeServiceClient serviceClient, String fileSystemName){
    
    DataLakeFileSystemClient fileSystemClient =
    serviceClient.getFileSystemClient(fileSystemName);

    DataLakeDirectoryClient directoryClient =
        fileSystemClient.createDirectory("my-directory");

    return directoryClient.createSubDirectory("my-subdirectory");
}
```

## <a name="rename-or-move-a-directory"></a>Bir dizini yeniden adlandırma veya taşıma

**Datalakedirectoryclient. Rename** metodunu çağırarak bir dizini yeniden adlandırın veya taşıyın. İstenen dizinin yolunu bir parametre olarak geçirin. 

Bu örnek, bir alt dizini ada yeniden adlandırır `my-subdirectory-renamed` .

```java
static public DataLakeDirectoryClient
    RenameDirectory(DataLakeFileSystemClient fileSystemClient){

    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory/my-subdirectory");

    return directoryClient.rename(
        fileSystemClient.getFileSystemName(),"my-subdirectory-renamed");
}
```

Bu örnek adlı dizini adlı bir `my-subdirectory-renamed` dizinin alt dizinine taşır `my-directory-2` . 

```java
static public DataLakeDirectoryClient MoveDirectory
(DataLakeFileSystemClient fileSystemClient){

    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory/my-subdirectory-renamed");

    return directoryClient.rename(
        fileSystemClient.getFileSystemName(),"my-directory-2/my-subdirectory-renamed");                
}
```

## <a name="delete-a-directory"></a>Bir dizini silme

**Datalakedirectoryclient. deleteWithResponse** yöntemini çağırarak bir dizini silin.

Bu örnek adlı bir dizini siler `my-directory` .   

```java
static public void DeleteDirectory(DataLakeFileSystemClient fileSystemClient){
        
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory");

    directoryClient.deleteWithResponse(true, null, null, null);
}
```

## <a name="upload-a-file-to-a-directory"></a>Dizine dosya yükleme

İlk olarak, **Datalakefileclient** sınıfının bir örneğini oluşturarak hedef dizinde bir dosya başvurusu oluşturun. **Datalakefileclient. Append** metodunu çağırarak bir dosyayı karşıya yükleyin. **Datalakefileclient. FlushAsync** yöntemini çağırarak karşıya yüklemeyi tamamladığınızdan emin olun.

Bu örnek, bir metin dosyasını adlı bir dizine yükler `my-directory` . '

```java
static public void UploadFile(DataLakeFileSystemClient fileSystemClient) 
    throws FileNotFoundException{
        
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory");

    DataLakeFileClient fileClient = directoryClient.createFile("uploaded-file.txt");

    File file = new File("C:\\mytestfile.txt");

    InputStream targetStream = new BufferedInputStream(new FileInputStream(file));

    long fileSize = file.length();

    fileClient.append(targetStream, 0, fileSize);

    fileClient.flush(fileSize);
}
```

> [!TIP]
> Dosya boyutunuz büyükse, kodunuzun **Datalakefileclient. Append** yöntemine birden çok çağrı yapması gerekir. Bunun yerine **Datalakefileclient. uploadFromFile** metodunu kullanmayı düşünün. Bu şekilde, tüm dosyayı tek bir çağrıda karşıya yükleyebilirsiniz. 
>
> Bir örnek için sonraki bölüme bakın.

## <a name="upload-a-large-file-to-a-directory"></a>Bir dizine büyük bir dosya yükleme

**Datalakefileclient. Append** yöntemine birden çok çağrı yapmak zorunda kalmadan büyük dosyaları karşıya yüklemek Için **Datalakefileclient. uploadfromfile** yöntemini kullanın.

```java
static public void UploadFileBulk(DataLakeFileSystemClient fileSystemClient) 
    throws FileNotFoundException{
        
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory");

    DataLakeFileClient fileClient = directoryClient.getFileClient("uploaded-file.txt");

    fileClient.uploadFromFile("C:\\mytestfile.txt");

    }

```

## <a name="download-from-a-directory"></a>Bir dizinden indir

İlk olarak, indirmek istediğiniz dosyayı temsil eden bir **Datalakefileclient** örneği oluşturun. Dosyayı okumak için **Datalakefileclient. Read** metodunu kullanın. Akıştan bir dosyaya bayt kaydetmek için herhangi bir .NET dosya işleme API 'sini kullanın. 

```java
static public void DownloadFile(DataLakeFileSystemClient fileSystemClient)
    throws FileNotFoundException, java.io.IOException{

    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory");

    DataLakeFileClient fileClient = 
        directoryClient.getFileClient("uploaded-file.txt");

    File file = new File("C:\\downloadedFile.txt");

    OutputStream targetStream = new FileOutputStream(file);

    fileClient.read(targetStream);

    targetStream.close();
      
}

```

## <a name="list-directory-contents"></a>Dizin içeriğini listeleme

Bu örnek, adlı bir dizinde bulunan her bir dosyanın adını yazdırır `my-directory` .

```java
static public void ListFilesInDirectory(DataLakeFileSystemClient fileSystemClient){
        
    ListPathsOptions options = new ListPathsOptions();
    options.setPath("my-directory");
     
    PagedIterable<PathItem> pagedIterable = 
    fileSystemClient.listPaths(options, null);

    java.util.Iterator<PathItem> iterator = pagedIterable.iterator();
       
    PathItem item = iterator.next();

    while (item != null)
    {
        System.out.println(item.getName());


        if (!iterator.hasNext())
        {
            break;
        }
            
        item = iterator.next();
    }

}
```

## <a name="manage-access-control-lists-acls"></a>Erişim denetim listelerini (ACL 'Ler) yönetme

Dizinler ve dosyalar için erişim izinlerini alabilir, ayarlayabilir ve güncelleştirebilirsiniz.

> [!NOTE]
> Erişimi yetkilendirmek için Azure Active Directory (Azure AD) kullanıyorsanız, güvenlik sorumlusuna [Depolama Blobu veri sahibi rolü](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunların nasıl değiştirileceği hakkında daha fazla bilgi edinmek için  [Azure Data Lake Storage 2. erişim denetimi](./data-lake-storage-access-control.md)' ne bakın.

### <a name="manage-a-directory-acl"></a>Dizin ACL 'sini yönetme

Bu örnek, adlı bir dizinin ACL 'sini alır ve ayarlar `my-directory` . Bu örnek, sahip olan kullanıcıya okuma, yazma ve yürütme izinlerini verir, sahip olan gruba yalnızca okuma ve yürütme izinleri verir ve diğerlerinin tüm okuma erişimini sağlar.

> [!NOTE]
> Uygulamanız Azure Active Directory (Azure AD) kullanarak erişim yetkisi alıyorsa, uygulamanızın erişim yetkisi vermek için kullandığı güvenlik sorumlusuna [Depolama Blobu veri sahibi rolü](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunların nasıl değiştirileceği hakkında daha fazla bilgi edinmek için  [Azure Data Lake Storage 2. erişim denetimi](./data-lake-storage-access-control.md)' ne bakın.

```java
static public void ManageDirectoryACLs(DataLakeFileSystemClient fileSystemClient){

    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory");

    PathAccessControl directoryAccessControl =
        directoryClient.getAccessControl();

    List<PathAccessControlEntry> pathPermissions = directoryAccessControl.getAccessControlList();
       
    System.out.println(PathAccessControlEntry.serializeList(pathPermissions));
             
    RolePermissions groupPermission = new RolePermissions();
    groupPermission.setExecutePermission(true).setReadPermission(true);
  
    RolePermissions ownerPermission = new RolePermissions();
    ownerPermission.setExecutePermission(true).setReadPermission(true).setWritePermission(true);
  
    RolePermissions otherPermission = new RolePermissions();
    otherPermission.setReadPermission(true);
  
    PathPermissions permissions = new PathPermissions();
  
    permissions.setGroup(groupPermission);
    permissions.setOwner(ownerPermission);
    permissions.setOther(otherPermission);

    directoryClient.setPermissions(permissions, null, null);

    pathPermissions = directoryClient.getAccessControl().getAccessControlList();
     
    System.out.println(PathAccessControlEntry.serializeList(pathPermissions));

}

```

Ayrıca, bir kapsayıcının kök dizininin ACL 'sini de alabilir ve ayarlayabilirsiniz. Kök dizini almak için `""` **Datalakefilesystemclient. getdirectoryclient** metoduna boş bir dize () geçirin.

### <a name="manage-a-file-acl"></a>Dosya ACL 'sini yönetme

Bu örnek, adlı bir dosyanın ACL 'sini alır ve ayarlar `upload-file.txt` . Bu örnek, sahip olan kullanıcıya okuma, yazma ve yürütme izinlerini verir, sahip olan gruba yalnızca okuma ve yürütme izinleri verir ve diğerlerinin tüm okuma erişimini sağlar.

> [!NOTE]
> Uygulamanız Azure Active Directory (Azure AD) kullanarak erişim yetkisi alıyorsa, uygulamanızın erişim yetkisi vermek için kullandığı güvenlik sorumlusuna [Depolama Blobu veri sahibi rolü](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunların nasıl değiştirileceği hakkında daha fazla bilgi edinmek için  [Azure Data Lake Storage 2. erişim denetimi](./data-lake-storage-access-control.md)' ne bakın.

```java
static public void ManageFileACLs(DataLakeFileSystemClient fileSystemClient){

    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-directory");

    DataLakeFileClient fileClient = 
        directoryClient.getFileClient("uploaded-file.txt");

    PathAccessControl fileAccessControl =
        fileClient.getAccessControl();

    List<PathAccessControlEntry> pathPermissions = fileAccessControl.getAccessControlList();
     
    System.out.println(PathAccessControlEntry.serializeList(pathPermissions));
           
    RolePermissions groupPermission = new RolePermissions();
    groupPermission.setExecutePermission(true).setReadPermission(true);

    RolePermissions ownerPermission = new RolePermissions();
    ownerPermission.setExecutePermission(true).setReadPermission(true).setWritePermission(true);

    RolePermissions otherPermission = new RolePermissions();
    otherPermission.setReadPermission(true);

    PathPermissions permissions = new PathPermissions();

    permissions.setGroup(groupPermission);
    permissions.setOwner(ownerPermission);
    permissions.setOther(otherPermission);

    fileClient.setPermissions(permissions, null, null);

    pathPermissions = fileClient.getAccessControl().getAccessControlList();
   
    System.out.println(PathAccessControlEntry.serializeList(pathPermissions));

}
```

### <a name="set-an-acl-recursively"></a>ACL 'yi yinelemeli olarak ayarlama

Bu değişiklikleri her bir alt öğe için ayrı ayrı yapmak zorunda kalmadan, bir üst dizinin varolan alt öğelerinde ACL 'Leri yinelemeli olarak ekleyebilir, güncelleştirebilir ve kaldırabilirsiniz. Daha fazla bilgi için bkz. [Azure Data Lake Storage 2. için erişim denetim listelerini (ACL 'ler) yinelemeli olarak ayarlama](recursive-access-control-lists.md).

## <a name="see-also"></a>Ayrıca bkz.

* [API başvuru belgeleri](/java/api/overview/azure/storage-file-datalake-readme)
* [Paket (Maven)](https://search.maven.org/artifact/com.azure/azure-storage-file-datalake)
* [Örnekler](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake)
* [Gen1 to Gen2 Mapping](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake/GEN1_GEN2_MAPPING.md)
* [Bilinen sorunlar](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Geri bildirimde bulunun](https://github.com/Azure/azure-sdk-for-java/issues)