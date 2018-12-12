---
title: İleti - Azure Event Hubs'a göndermek ve almak için Python kullanma | Microsoft Docs
description: Olayları almak ve Python kullanarak Event Hubs ile akış olaylarını yakalamak için olayları gönderirsiniz öğrenin.
keywords: ''
documentationcenter: ''
services: event-hubs
author: ShubhaVijayasarathy
manager: ''
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/26/2018
ms.author: shvija
ms.openlocfilehash: 4a0e2cd7e0c768512e1aafc042fe55338fdc206e
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53084983"
---
# <a name="how-to-use-azure-event-hubs-from-a-python-application"></a>Bir Python uygulamasından Azure Event Hubs'ı kullanma
Azure Event Hubs saniyede milyonlarca olay alıp işleme kapasitesine sahip olan bir Büyük Veri akış platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Daha fazla bilgi için [Event hubs'a giriş](event-hubs-what-is-event-hubs.md). 

Bu makalede aşağıdaki görevlerin dilinde yazılmış bir uygulamadan nasıl yapılacağını gösteren makalelerin bağlantıları sağlanmaktadır **Python**:

- [Olay hub'ına olayları gönderme](#send-events-to-event-hubs)
- [Bir olay hub'ından olayları alma](#receive-events-from-event-hubs)
- [Okuma yakalanan olay verileri bir Azure depolama biriminden](#capture-event-hubs-data). 

## <a name="prerequisites"></a>Önkoşullar
- Bu hızlı başlangıçlara birini izleyerek bir olay hub'ı oluşturma: [Azure portalında](event-hubs-create.md), [Azure CLI](event-hubs-quickstart-cli.md), [Azure PowerShell](event-hubs-quickstart-powershell.md), [AzureResourceManagerşablonu](event-hubs-resource-manager-namespace-event-hub.md). 
- Python 3.4 veya üzerinin yüklü makinenizde.

## <a name="install-python-package"></a>Python paketini yükle

Event Hubs için Python paketini yüklemek için Python alt yolu olan bir komut istemi açın ve ardından bu komutu çalıştırın: 

```bash
pip install azure-eventhub
```

## <a name="send-events-to-event-hubs"></a>Event Hubs’a olaylar gönderme
Aşağıdaki kod, olayları olay hub'ına bir Python uygulamasından gönderme işlemini göstermektedir: 

1. Olay hub'ı, anahtar adını ve anahtar değeri URL'sini tutması için değişkenleri oluşturun. 

    ```python
    # Import classes from Event Hubs python package
    from azure.eventhub import EventHubClient, Receiver, Offset
    
    # Address can be in either of these formats:
    # "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<mynamespace>.servicebus.windows.net/myeventhub"
    # "amqps://<mynamespace>.servicebus.windows.net/myeventhub"
    # For example:
    ADDRESS = "amqps://<MyEventHubNamspaceName>.servicebus.windows.net/<MyEventHubName>"
    
    # SAS policy and key are not required if they are encoded in the URL
    USER = "<Name of the access key. Default name: RootManageSharedAccessKey>"
    KEY = "<The access key>"
    ```

2. Event Hubs istemcisi oluşturma, bir gönderici ekleyin, istemcisini çalıştıran, gönderen kullanarak olayı gönderme ve işiniz bittiğinde sonra istemci durdurun. 

    ```python
    # Create an Event Hubs client
    client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
    
    # Add a sender to the client
    sender = client.add_sender(partition="0")
    
    # Run the Event Hub client
    client.run()
    
    # Send event to the event hub
    sender.send(EventData("<MyEventData>"))
    
    # Stop the Event Hubs client
    client.stop()
    
    ```

Python'da yazılmış bir uygulamadan bir olay hub'ına olay gönderme konusunda tam öğretici için bkz: [bu makalede](event-hubs-python-get-started-send.md).

## <a name="receive-events-from-event-hubs"></a>Event Hubs'dan olayları alma
Aşağıdaki kod, bir Python uygulamasındaki bir olay hub'ından olay alma işlemini göstermektedir: 

```python

# Create an Event Hubs client
client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)

# Add a receiver to the client
receiver = client.add_receiver(CONSUMER_GROUP, PARTITION, prefetch=5000, offset=OFFSET)

# Run the Event Hubs client
client.run()

# Receive event data from the event hub
for event_data in receiver.receive(timeout=100):
    last_offset = event_data.offset
    last_sn = event_data.sequence_number
    print("Received: {}, {}".format(last_offset, last_sn))

# Stop the Event Hubs client
client.stop()
```

Python'da yazılmış bir uygulamadan bir olay hub'ından olay alma konusunda tam öğretici için bkz: [bu makalede](event-hubs-python-get-started-receive.md)

## <a name="read-capture-event-data-from-azure-storage"></a>Azure Depolama'dan yakalama olay veri okuma
Aşağıdaki kodu depolanan yakalanan olay verileri okuma işlemini gösterir bir **Azure blob depolama** bir Python uygulamasından: etkinleştirme **yakalama** özelliği olay hub'ı yönergeleri izleyerek: [Event Hubs Azure portalını kullanarak yakalama özelliğini etkinleştirme](event-hubs-capture-enable-through-portal.md). Ardından, bazı olayları, kodu test etmeden önce olay hub'ına gönderin. 

```python
import os
import string
import json
import avro.schema
from avro.datafile import DataFileReader, DataFileWriter
from avro.io import DatumReader, DatumWriter
from azure.storage.blob import BlockBlobService

def processBlob(filename):
    reader = DataFileReader(open(filename, 'rb'), DatumReader())
    dict = {}
    for reading in reader:
        parsed_json = json.loads(reading["Body"])
        if not 'id' in parsed_json:
            return
        if not dict.has_key(parsed_json['id']):
            list = []
            dict[parsed_json['id']] = list
        else:
            list = dict[parsed_json['id']]
            list.append(parsed_json)
    reader.close()
    for device in dict.keys():
        deviceFile = open(device + '.csv', "a")
        for r in dict[device]:
            deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\n')

def startProcessing(accountName, key, container):
    print 'Processor started using path: ' + os.getcwd()
    block_blob_service = BlockBlobService(account_name=accountName, account_key=key)
    generator = block_blob_service.list_blobs(container)
    for blob in generator:
        #content_length == 508 is an empty file, so only process content_length > 508 (skip empty files)
        if blob.properties.content_length > 508:
            print('Downloaded a non empty blob: ' + blob.name)
            cleanName = string.replace(blob.name, '/', '_')
            block_blob_service.get_blob_to_path(container, blob.name, cleanName)
            processBlob(cleanName)
            os.remove(cleanName)
        block_blob_service.delete_blob(container, blob.name)
startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'capture')

```

Python'da yazılmış bir uygulamadan Azure blob depolama alanındaki yakalanan Event Hubs verilerini okumak hakkında eksiksiz bir öğretici için bkz: [bu makalede](event-hubs-capture-python.md)

## <a name="github-samples"></a>GitHub örnekleri
Daha fazla Python örnekleri bulabilirsiniz [azure-event-hubs-python Git deposu](https://github.com/Azure/azure-event-hubs-python/).

## <a name="next-steps"></a>Sonraki adımlar
Başlangıç kavramları bölümdeki makaleleri inceleyin [Event Hubs özelliklerine genel bakış](event-hubs-features.md).