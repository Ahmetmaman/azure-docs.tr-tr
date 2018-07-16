---
title: NSG akış günlüklerini okuma | Microsoft Docs
description: Bu makalede, NSG akış günlüklerini ayrıştırmak gösterilir
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: jdial
ms.openlocfilehash: 492a0a63198fe2013cfeac0459fc6da8521a5e6e
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39056809"
---
# <a name="read-nsg-flow-logs"></a>NSG akış günlüklerini okuma

PowerShell ile NSG akış günlükleri girdileri oku öğrenin.

NSG akış günlükleri bir depolama hesabında depolanır [blok blobları](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs.md#about-block-blobs). Blok blobları, daha küçük bloklarını yapılır. Her günlük saatte oluşturulan ayrı bir blok blobudur. Her saat yeni günlükleri üretilir, günlükleri yeni girişlerle birkaç dakikada en son verilerle güncelleştirilir. Bu makaledeki bölümleri akış günlüklerini okuma öğrenin.

## <a name="scenario"></a>Senaryo

Aşağıdaki senaryoda, bir depolama hesabında depolanmış bir örnek akış günlüğü vardır. Biz nasıl seçerek en son olayları NSG akış günlüklerini okuyabilirsiniz adım. Bu makalede PowerShell kullanacağız, ancak makalesinde açıklanan kavramları programlama diline sınırlı değildir ve Azure depolama API'ları tarafından desteklenen tüm dillerde uygulanabilir

## <a name="setup"></a>Kurulum

Başlamadan önce ağ güvenlik grubu akış hesabınızdaki bir veya daha fazla ağ güvenlik grupları etkin günlüğü olması gerekir. Akış günlüklerinizi ağ güvenliği etkinleştirme hakkında yönergeler için aşağıdaki makaleye başvurun: [için ağ güvenlik grubu akış günlüğe kaydetme giriş](network-watcher-nsg-flow-logging-overview.md).

## <a name="retrieve-the-block-list"></a>Engelleme listesi alınamıyor

Aşağıdaki PowerShell değişkenleri NSG akış günlüğü blob sorgu ve içindeki blokları listelemek için gereken ayarlar [CloudBlockBlob](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob?view=azurestorage-8.1.3) blok blobu. Betik, ortamınız için geçerli değerler içerecek şekilde güncelleştirin.

```powershell
function Get-NSGFlowLogBlockList {
    [CmdletBinding()]
    param (
        [string] [Parameter(Mandatory=$true)] $subscriptionId,
        [string] [Parameter(Mandatory=$true)] $NSGResourceGroupName,
        [string] [Parameter(Mandatory=$true)] $NSGName,
        [string] [Parameter(Mandatory=$true)] $storageAccountName,
        [string] [Parameter(Mandatory=$true)] $storageAccountResourceGroup,
        [string] [Parameter(Mandatory=$true)] $macAddress,
        [datetime] [Parameter(Mandatory=$true)] $logTime
    )

    process {
        # Retrieve the primary storage account key to access the NSG logs
        $StorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountResourceGroup -Name $storageAccountName).Value[0]

        # Setup a new storage context to be used to query the logs
        $ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

        # Container name used by NSG flow logs
        $ContainerName = "insights-logs-networksecuritygroupflowevent"

        # Name of the blob that contains the NSG flow log
        $BlobName = "resourceId=/SUBSCRIPTIONS/${subscriptionId}/RESOURCEGROUPS/${NSGResourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/${NSGName}/y=$($logTime.Year)/m=$(($logTime).ToString("MM"))/d=$(($logTime).ToString("dd"))/h=$(($logTime).ToString("HH"))/m=00/macAddress=$($macAddress)/PT1H.json"

        # Gets the storage blog
        $Blob = Get-AzureStorageBlob -Context $ctx -Container $ContainerName -Blob $BlobName

        # Gets the block blog of type 'Microsoft.WindowsAzure.Storage.Blob.CloudBlob' from the storage blob
        $CloudBlockBlob = [Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob] $Blob.ICloudBlob

        # Stores the block list in a variable from the block blob.
        $blockList = $CloudBlockBlob.DownloadBlockList()

        # Return the Block List
        $blockList
    }
}
$blockList = Get-NSGFlowLogBlockList -subscriptionId "00000000-0000-0000-0000-000000000000" -NSGResourceGroupName "resourcegroupname" -storageAccountName "storageaccountname" -storageAccountResourceGroup "sa-rg" -macAddress "000D3AF8196E" -logTime "03/07/2018 22:00"
```

`$blockList` Değişkeni, bir blobun blok listesini döndürür. Her bir blok blobu en az iki blokları içerir.  İlk blok uzunluğunda ve `21` bayt, bu blok json günlük açılan parantezler içerir. Diğer blok kapanış köşeli ayraçlar ve uzunluğu `9` bayt.  Aşağıdaki örnek log gördüğünüz gibi yedi girişleri, her olan tek bir giriş içeriyor. Tüm yeni günlük girişleri hemen önce son blok sonuna eklenir.

```
Name                                         Length Committed
----                                         ------ ---------
ZDk5MTk5N2FkNGE0MmY5MTk5ZWViYjA0YmZhODRhYzY=     12      True
NzQxNDA5MTRhNDUzMGI2M2Y1MDMyOWZlN2QwNDZiYzQ=   2685      True
ODdjM2UyMWY3NzFhZTU3MmVlMmU5MDNlOWEwNWE3YWY=   2586      True
ZDU2MjA3OGQ2ZDU3MjczMWQ4MTRmYWNhYjAzOGJkMTg=   2688      True
ZmM3ZWJjMGQ0ZDA1ODJlOWMyODhlOWE3MDI1MGJhMTc=   2775      True
ZGVkYTc4MzQzNjEyMzlmZWE5MmRiNjc1OWE5OTc0OTQ=   2676      True
ZmY2MjUzYTIwYWIyOGU1OTA2ZDY1OWYzNmY2NmU4ZTY=   2777      True
Mzk1YzQwM2U0ZWY1ZDRhOWFlMTNhYjQ3OGVhYmUzNjk=   2675      True
ZjAyZTliYWE3OTI1YWZmYjFmMWI0MjJhNzMxZTI4MDM=      2      True
```

## <a name="read-the-block-blob"></a>Blok blobu okuma

Sonraki okumak ihtiyacımız `$blocklist` verileri almak üzere değişkeni. Bu örnekte, biz engelleme listesi yineleme her bloğundan okunan bayt sayısı ve bunları bir dizide yazı. Kullandığımız [DownloadRangeToByteArray](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadrangetobytearray?view=azurestorage-8.1.3#Microsoft_WindowsAzure_Storage_Blob_CloudBlob_DownloadRangeToByteArray_System_Byte___System_Int32_System_Nullable_System_Int64__System_Nullable_System_Int64__Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) verileri almak için yöntemi.

```powershell
# Set the size of the byte array to the largest block
$maxvalue = ($blocklist | measure Length -Maximum).Maximum

# Create an array to store values in
$valuearray = @()

# Define the starting index to track the current block being read
$index = 0

# Loop through each block in the block list
for($i=0; $i -lt $blocklist.count; $i++)
{

# Create a byte array object to story the bytes from the block
$downloadArray = New-Object -TypeName byte[] -ArgumentList $maxvalue

# Download the data into the ByteArray, starting with the current index, for the number of bytes in the current block. Index is increased by 3 when reading to remove preceding comma.
$CloudBlockBlob.DownloadRangeToByteArray($downloadArray,0,$index+3,$($blockList[$i].Length-1)) | Out-Null

# Increment the index by adding the current block length to the previous index
$index = $index + $blockList[$i].Length

# Retrieve the string from the byte array

$value = [System.Text.Encoding]::ASCII.GetString($downloadArray)

# Add the log entry to the value array
$valuearray += $value
}
```

Artık `$valuearray` dizi her blok dize değeri içerir. Giriş doğrulamak için ikinci son değerine diziden çalıştırarak alın `$valuearray[$valuearray.Length-2]`. Köşeli ayraç yalnızca son değerdir istemezsiniz.

Bu değer sonuçları, aşağıdaki örnekte gösterilmiştir:

```json
        {
             "time": "2017-06-16T20:59:43.7340000Z",
             "systemId": "5f4d02d3-a7d0-4ed4-9ce8-c0ae9377951c",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/CONTOSORG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/CONTOSONSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_AllowInternetOutBound","flows":[{"mac":"000D3A18077E","flowTuples":["1497646722,10.0.0.4,168.62.32.14,44904,443,T,O,A","1497646722,10.0.0.4,52.240.48.24,45218,443,T,O,A","1497646725,10.
0.0.4,168.62.32.14,44910,443,T,O,A","1497646725,10.0.0.4,52.240.48.24,45224,443,T,O,A","1497646728,10.0.0.4,168.62.32.14,44916,443,T,O,A","1497646728,10.0.0.4,52.240.48.24,45230,443,T,O,A","1497646732,10.0.0.4,168.62.32.14,44922,443,T,O,A","14976
46732,10.0.0.4,52.240.48.24,45236,443,T,O,A","1497646735,10.0.0.4,168.62.32.14,44928,443,T,O,A","1497646735,10.0.0.4,52.240.48.24,45242,443,T,O,A","1497646738,10.0.0.4,168.62.32.14,44934,443,T,O,A","1497646738,10.0.0.4,52.240.48.24,45248,443,T,O,
A","1497646742,10.0.0.4,168.62.32.14,44942,443,T,O,A","1497646742,10.0.0.4,52.240.48.24,45256,443,T,O,A","1497646745,10.0.0.4,168.62.32.14,44948,443,T,O,A","1497646745,10.0.0.4,52.240.48.24,45262,443,T,O,A","1497646749,10.0.0.4,168.62.32.14,44954
,443,T,O,A","1497646749,10.0.0.4,52.240.48.24,45268,443,T,O,A","1497646753,10.0.0.4,168.62.32.14,44960,443,T,O,A","1497646753,10.0.0.4,52.240.48.24,45274,443,T,O,A","1497646756,10.0.0.4,168.62.32.14,44966,443,T,O,A","1497646756,10.0.0.4,52.240.48
.24,45280,443,T,O,A","1497646759,10.0.0.4,168.62.32.14,44972,443,T,O,A","1497646759,10.0.0.4,52.240.48.24,45286,443,T,O,A","1497646763,10.0.0.4,168.62.32.14,44978,443,T,O,A","1497646763,10.0.0.4,52.240.48.24,45292,443,T,O,A","1497646766,10.0.0.4,
168.62.32.14,44984,443,T,O,A","1497646766,10.0.0.4,52.240.48.24,45298,443,T,O,A","1497646769,10.0.0.4,168.62.32.14,44990,443,T,O,A","1497646769,10.0.0.4,52.240.48.24,45304,443,T,O,A","1497646773,10.0.0.4,168.62.32.14,44996,443,T,O,A","1497646773,
10.0.0.4,52.240.48.24,45310,443,T,O,A","1497646776,10.0.0.4,168.62.32.14,45002,443,T,O,A","1497646776,10.0.0.4,52.240.48.24,45316,443,T,O,A","1497646779,10.0.0.4,168.62.32.14,45008,443,T,O,A","1497646779,10.0.0.4,52.240.48.24,45322,443,T,O,A"]}]}
,{"rule":"DefaultRule_DenyAllInBound","flows":[]},{"rule":"UserRule_ssh-rule","flows":[]},{"rule":"UserRule_web-rule","flows":[{"mac":"000D3A18077E","flowTuples":["1497646738,13.82.225.93,10.0.0.4,1180,80,T,I,A","1497646750,13.82.225.93,10.0.0.4,
1184,80,T,I,A","1497646768,13.82.225.93,10.0.0.4,1181,80,T,I,A","1497646780,13.82.225.93,10.0.0.4,1336,80,T,I,A"]}]}]}
        }
```

Bu senaryo, tüm günlük ayrıştırma gerek kalmadan girişleri NSG akış günlüklerini okuma örneğidir. Blok Kimliğini kullanarak veya blok blobu depolanan blokları uzunluğunu izleme yazıldıkları şekilde günlüğüne yeni girişler okuyabilirsiniz. Bu, yalnızca yeni girişler okumanızı sağlar.


## <a name="next-steps"></a>Sonraki adımlar

Ziyaret [açık kaynak araçlar kullanarak Azure Ağ İzleyicisi NSG akış günlüklerini görselleştirme](network-watcher-visualize-nsg-flow-logs-open-source-tools.md) NSG akış günlüklerini görüntülemek için diğer yollar hakkında daha fazla bilgi için.

Ziyaret depolama BLOB'ları hakkında daha fazla bilgi edinmek için: [Azure işlevleri Blob Depolama bağlamaları](../azure-functions/functions-bindings-storage-blob.md)
