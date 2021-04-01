---
title: 'REST API: Azure Data Lake Storage 1. dosya sistemi işlemleri | Microsoft Docs'
description: Azure Data Lake Storage 1. üzerinde dosya sistemi işlemleri gerçekleştirmek için Weblerrest API 'Lerini kullanma
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: how-to
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 51e0fb2ffa7b573ecfeda163d9ad99597ff735a2
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "92109213"
---
# <a name="filesystem-operations-on-azure-data-lake-storage-gen1-using-rest-api"></a>REST API kullanarak Azure Data Lake Storage 1. dosya sistemi işlemleri
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-data-operations-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
> 

Bu makalede, Azure Data Lake Storage 1. dosya sistemi işlemlerini gerçekleştirmek için Weblerrest API 'Lerini ve Data Lake Storage 1. REST API 'Lerini nasıl kullanacağınızı öğreneceksiniz. REST API kullanarak Data Lake Storage 1. hesap yönetimi işlemlerinin nasıl gerçekleştirileceği hakkında yönergeler için [REST API kullanarak Data Lake Storage 1. hesap yönetimi işlemleri](data-lake-store-get-started-rest-api.md)konusuna bakın.

## <a name="prerequisites"></a>Önkoşullar
* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* **Azure Data Lake Storage 1. hesabı**. [Azure Portal kullanarak Azure Data Lake Storage 1. kullanmaya başlama](data-lake-store-get-started-portal.md)yönergelerini izleyin.

* **[kıvır](https://curl.haxx.se/)**. Bu makalede, bir Data Lake Storage 1. hesabına yönelik REST API çağrılarının nasıl yapılacağını göstermek için kıvrımlı kullanılır.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak nasıl kimlik doğrulaması gerçekleştiririm?
Azure Active Directory'yi kullanarak kimlik doğrulaması gerçekleştirmek üzere iki yaklaşımdan faydalanabilirsiniz:

* Uygulamanızda (etkileşimli) Son Kullanıcı kimlik doğrulaması için bkz. [.NET SDK kullanarak Data Lake Storage 1. Ile Son Kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-rest-api.md).
* Uygulamanıza yönelik hizmetten hizmete kimlik doğrulaması için (etkileşimli olmayan), bkz. [.NET SDK kullanarak Data Lake Storage 1. ile hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-rest-api.md).


## <a name="create-folders"></a>Klasör oluşturma
Bu işlem, [burada](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory) tanımlanan WebHDFS REST API çağrısını temel alır.

Aşağıdaki cURL komutunu kullanın. **\<yourstorename>** Data Lake Storage 1. hesap adınızla değiştirin.

```console
curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS'
```

Yukarıdaki komutta, \<`REDACTED`\> öğesini daha önce aldığınız yetkilendirme belirteciyle değiştirin. Bu komut, Data Lake Storage 1. hesabınızın kök klasörü altında **MyTempDir** adlı bir dizin oluşturur.

İşlem başarıyla tamamlanırsa aşağıdaki kod parçacığına benzer bir yanıt görmeniz gerekir:

```output
{"boolean":true}
```

## <a name="list-folders"></a>Klasörleri listeleme
Bu işlem, [burada](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory) tanımlanan WebHDFS REST API çağrısını temel alır.

Aşağıdaki cURL komutunu kullanın. **\<yourstorename>** Data Lake Storage 1. hesap adınızla değiştirin.

```console
curl -i -X GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS'
```

Yukarıdaki komutta, \<`REDACTED`\> öğesini daha önce aldığınız yetkilendirme belirteciyle değiştirin.

İşlem başarıyla tamamlanırsa aşağıdaki kod parçacığına benzer bir yanıt görmeniz gerekir:

```output
{
"FileStatuses": {
    "FileStatus": [{
        "length": 0,
        "pathSuffix": "mytempdir",
        "type": "DIRECTORY",
        "blockSize": 268435456,
        "accessTime": 1458324719512,
        "modificationTime": 1458324719512,
        "replication": 0,
        "permission": "777",
        "owner": "<GUID>",
        "group": "<GUID>"
    }]
}
}
```

## <a name="upload-data"></a>Verileri karşıya yükleme
Bu işlem, [burada](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File) tanımlanan WebHDFS REST API çağrısını temel alır.

Aşağıdaki cURL komutunu kullanın. **\<yourstorename>** Data Lake Storage 1. hesap adınızla değiştirin.

```console
curl -i -X PUT -L -T 'C:\temp\list.txt' -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE'
```

Yukarıdaki söz diziminde **-T** parametresi karşıya yüklediğiniz dosyanın konumudur.

Çıkış aşağıdaki kod parçacığına benzer:
   
```output
HTTP/1.1 307 Temporary Redirect
...
Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE&write=true
...
Content-Length: 0

HTTP/1.1 100 Continue

HTTP/1.1 201 Created
...
```

## <a name="read-data"></a>Verileri okuma
Bu işlem, [burada](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File) tanımlanan WebHDFS REST API çağrısını temel alır.

Data Lake Storage 1. hesabından veri okuma iki adımlı bir işlemdir.

* İlk olarak `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN` uç noktası için bir GET isteği gönderirsiniz. Bu çağrı, sonraki GET isteğini göndermek için bir konum döndürür.
* Ardından `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true` uç noktası için GET isteğini gönderirsiniz. Bu çağrı, dosyanın içeriğini görüntüler.

Ancak giriş parametrelerinde birinci ve ikinci adım arasında fark olmadığından, ilk isteği göndermek için `-L` parametresini kullanabilirsiniz. `-L` seçeneği, temelde iki isteği tek istekte birleştirir ve cURL'nin isteği yeni konumda yeniden gerçekleştirmesini sağlar. Son olarak, aşağıdaki kod parçacığında gösterildiği gibi, tüm istek çağrılarının çıktısı görüntülenir. **\<yourstorename>** Data Lake Storage 1. hesap adınızla değiştirin.

```console
curl -i -L GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN'
```

Aşağıdaki kod parçacığına benzer bir çıktı görmeniz gerekir:

```output
HTTP/1.1 307 Temporary Redirect
...
Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
...

HTTP/1.1 200 OK
...

Hello, Data Lake Store user!
```

## <a name="rename-a-file"></a>Dosyayı yeniden adlandırma
Bu işlem, [burada](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory) tanımlanan WebHDFS REST API çağrısını temel alır.

Bir dosyayı yeniden adlandırmak için aşağıdaki cURL komutunu kullanın. **\<yourstorename>** Data Lake Storage 1. hesap adınızla değiştirin.

```console
curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt'
```

Aşağıdaki kod parçacığına benzer bir çıktı görmeniz gerekir:

```output
HTTP/1.1 200 OK
...

{"boolean":true}
```

## <a name="delete-a-file"></a>Dosyayı silme
Bu işlem, [burada](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory) tanımlanan WebHDFS REST API çağrısını temel alır.

Bir dosyayı silmek için aşağıdaki cURL komutunu kullanın. **\<yourstorename>** Data Lake Storage 1. hesap adınızla değiştirin.

```console
curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE'
```

Aşağıdaki gibi bir çıktı görmeniz gerekir:

```output
HTTP/1.1 200 OK
...

{"boolean":true}
```

## <a name="next-steps"></a>Sonraki adımlar
* [Data Lake Storage 1. REST API kullanarak hesap yönetimi işlemleri](data-lake-store-get-started-rest-api.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Data Lake Storage 1. REST API başvurusu](/rest/api/datalakestore/)
* [Azure Data Lake Storage 1. uyumlu açık kaynak büyük veri uygulamaları](data-lake-store-compatible-oss-other-applications.md)