---
title: Tablo depolamaya giriş - Azure’da nesne depolama | Microsoft Docs
description: Bir NoSQL veri deposu olan Azure Table Storage kullanarak bulutta yapılandırılmış veri depolayın.
services: storage
author: SnehaGunda
ms.service: storage
ms.devlang: dotnet
ms.topic: overview
ms.date: 04/23/2018
ms.author: sngun
ms.component: tables
ms.openlocfilehash: 316e10d56e5c235c4d64dde0fde4dcb9429629c8
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39527796"
---
# <a name="introduction-to-table-storage-in-azure"></a>Azure’da Tablo depolamaya giriş

[!INCLUDE [storage-table-cosmos-db-tip-include](../../../includes/storage-table-cosmos-db-tip-include.md)]

Azure Tablo Depolama, yapılandırılmış NoSQL verilerini bulutta depolayan ve şemasız bir tasarım ile anahtar/öznitelik deposu sağlayan bir hizmettir. Table Storage şemasız olduğu için uygulamanızın ihtiyaçları geliştikçe verilerinizi kolayca uyarlayabilirsiniz. Tablo Depolama verilerine erişim, çoğu uygulama türü için hızlı ve ekonomik olmanın yanı sıra benzer veri hacimleri için geleneksel SQL’e kıyasla genellikle daha düşük maliyetlidir.

Web uygulamaları için kullanıcı verileri, adres defterleri, cihaz bilgileri veya hizmetiniz için gerekli olan tüm diğer meta veri türleri gibi esnek veri kümelerini Tablo Depolama ile depolayabilirsiniz. Bir tabloda istediğiniz kadar varlık depolayabilirsiniz ve bir depolama hesabı kapasite limitini dolduracak kadar tablo içerebilir.

[!INCLUDE [storage-table-concepts-include](../../../includes/storage-table-concepts-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.

* [.NET’te Azure Tablo Depolama kullanmaya başlama](../../cosmos-db/table-storage-how-to-use-dotnet.md)

* Kullanılabilir API’ler ile ilgili eksiksiz bilgiler için Tablo hizmeti başvuru belgelerini görüntüleyin:

    * [.NET başvurusu için Depolama İstemci Kitaplığı](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)

    * [REST API başvurusu](http://msdn.microsoft.com/library/azure/dd179355)
