---
title: CSV Blobları üzerinde arama
titleSuffix: Azure Cognitive Search
description: DelimitedText ayrıştırma modunu kullanarak CSV 'yi Azure Blob depolamadan ayıklayın ve içeri aktarın.
manager: nitinme
author: mgottein
ms.author: magottei
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 07/11/2020
ms.openlocfilehash: f9c01e8e31e78c277a7a3ec1e5d8d0c32b58f8bc
ms.sourcegitcommit: dc68a2c11bae2e9d57310d39fbed76628233fd7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2020
ms.locfileid: "91403662"
---
# <a name="how-to-index-csv-blobs-using-delimitedtext-parsing-mode-and-blob-indexers-in-azure-cognitive-search"></a>Azure Bilişsel Arama ile aynı Demitedmetin ayrıştırma modunu ve BLOB Dizin oluşturucularını kullanarak CSV bloblarını dizin oluşturma

Varsayılan olarak, [Azure bilişsel arama blob Indexer](search-howto-indexing-azure-blob-storage.md) , ayrılmış metin bloblarını tek bir metin öbeği olarak ayrıştırır. Ancak, CSV verileri içeren bloblarla, genellikle blobdaki her satırı ayrı bir belge olarak değerlendirmek istersiniz. Örneğin, aşağıdaki sınırlandırılmış metin verildiğinde, her biri "ID", "Dateyayınlanan" ve "Tags" alanlarını içeren iki belgeye ayrıştırın: 

```text
id, datePublished, tags
1, 2016-01-12, "azure-search,azure,cloud"
2, 2016-07-07, "cloud,mobile"
```

Bu makalede, ayrıştırma modunu ayarlayarak CSV bloblarını bir Azure Bilişsel Arama blob Dizin Oluşturucu ile ayrıştırmayı öğreneceksiniz `delimitedText` . 

> [!NOTE]
> Birden çok arama belgesini bir Azure blobundan çıkarmak için [bire çok dizin oluşturma](search-howto-index-one-to-many-blobs.md) içindeki Dizin Oluşturucu yapılandırma önerilerini izleyin.

## <a name="setting-up-csv-indexing"></a>CSV dizinlemeyi ayarlama
CSV bloblarını indekslemek için, bir Dizin Oluşturucu `delimitedText` [oluşturma](/rest/api/searchservice/create-indexer) isteğinde ayrıştırma moduyla bir Dizin Oluşturucu tanımı oluşturun veya güncelleştirin:

```http
    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }
```

`firstLineContainsHeaders` Her Blobun ilk (boş olmayan) satırın üstbilgiler içerdiğini belirtir.
Blob 'lar bir başlangıç üst bilgisi satırı içermiyorsa, üst bilgiler Dizin Oluşturucu yapılandırmasında belirtilmelidir: 

```http
"parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 
```

Yapılandırma ayarını kullanarak sınırlayıcı karakteri özelleştirebilirsiniz `delimitedTextDelimiter` . Örneğin:

```http
"parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextDelimiter" : "|" } }
```

> [!NOTE]
> Şu anda yalnızca UTF-8 kodlaması desteklenir. Diğer kodlamalar için desteğe ihtiyacınız varsa [UserVoice](https://feedback.azure.com/forums/263029-azure-search)üzerinde oy verin.

> [!IMPORTANT]
> Ayrılmış metin ayrıştırma modunu kullandığınızda Azure Bilişsel Arama, veri kaynağınızdaki tüm Blobların CSV olacağını varsayar. Aynı veri kaynağında CSV ve CSV olmayan Blobların bir karışımını desteketmeniz gerekiyorsa lütfen [UserVoice](https://feedback.azure.com/forums/263029-azure-search)üzerinde oy verin.
> 
> 

## <a name="request-examples"></a>İstek örnekleri
Tümünü bir araya getirmek, tüm yük örnekleri aşağıda verilmiştir. 

DataSource 

```http
    POST https://[service name].search.windows.net/datasources?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   
```

Dizinleyic

```http
    POST https://[service name].search.windows.net/indexers?api-version=2020-06-30
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }
```

## <a name="help-us-make-azure-cognitive-search-better"></a>Azure Bilişsel Arama daha iyi hale getirmemize yardımcı olun
Geliştirmeler için özellik istekleriniz veya fikirler varsa, bu girişi [UserVoice](https://feedback.azure.com/forums/263029-azure-search/)üzerinde belirtin. Mevcut özelliği kullanarak yardıma ihtiyacınız varsa sorunuzu [Stack Overflow](https://stackoverflow.microsoft.com/questions/tagged/18870)gönderin.