---
title: Azure kuyruklara giriş-Azure depolama
description: Çok sayıda iletiyi depolamaya yönelik bir hizmet olan Azure sıralarına giriş konusuna bakın. Kuyruk hizmeti bir URL biçimi, depolama hesabı, kuyruk ve ileti içerir.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 03/18/2020
ms.service: storage
ms.subservice: queues
ms.topic: overview
ms.reviewer: dineshm
ms.openlocfilehash: d1ec251edf384e8032a67dc66982787d17c75dbd
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92781473"
---
# <a name="what-are-azure-queues"></a>Azure kuyrukları nedir?

Azure Kuyruk Depolama, çok sayıda iletiyi depolamaya yönelik bir hizmettir. HTTP veya HTTPS kullanarak kimliği doğrulanmış çağrılar aracılığıyla dünyanın her yerinden iletilere erişirsiniz. Kuyruk iletisi boyutu 64 KB 'ye kadar olabilir. Bir kuyruk, depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti içerebilir. Kuyruklar genellikle zaman uyumsuz olarak işlenecek iş biriktirme listesi oluşturmak için kullanılır.

## <a name="queue-service-concepts"></a>Kuyruk hizmeti kavramlar

Kuyruk hizmetinde şu bileşenler bulunur:

![Bir depolama hesabı, kuyruklar ve iletiler arasındaki ilişkiyi gösteren diyagram](./media/storage-queues-introduction/queue1.png)

* **URL biçimi:** Kuyruklar şu URL biçimi kullanılarak adreslenebilir:

    `https://<storage account>.queue.core.windows.net/<queue>`
  
    Aşağıdaki URL diyagramdaki bir kuyruğun adresini belirtir:  
  
    `https://myaccount.queue.core.windows.net/images-to-download`

* **Depolama hesabı:** Tüm Azure depolama erişimi bir depolama hesabı üzerinden yapılır. Depolama hesabı kapasitesi hakkında daha fazla bilgi için bkz. [Standart depolama hesapları Için ölçeklenebilirlik ve performans hedefleri](../common/scalability-targets-standard-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).

* **Kuyruk:** Kuyrukta bir dizi ileti vardır. Sıra **adı tamamen küçük harfle yazılmalıdır.** Kuyrukların adlandırılması hakkında daha fazla bilgi için bkz. [Kuyrukları ve Meta Verileri Adlandırma](/rest/api/storageservices/Naming-Queues-and-Metadata).

* **İleti:** İleti, biçimi ne olursa olsun en çok 64 KB büyüklüktedir. Sürüm 2017-07-29 ' den önce, izin verilen maksimum yaşam süresi yedi gündür. Sürüm 2017-07-29 veya üzeri için, en fazla yaşam süresi herhangi bir pozitif sayı veya iletinin süresinin dolmadığını belirten-1 olabilir. Bu parametre atlanırsa, varsayılan yaşam süresi yedi gündür.

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama hesabı oluşturma](../common/storage-account-create.md?toc=%252fazure%252fstorage%252fqueues%252ftoc.json)
* [.NET kullanarak kuyruklar ile çalışmaya başlama](storage-dotnet-how-to-use-queues.md)