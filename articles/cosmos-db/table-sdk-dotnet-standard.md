---
title: Azure Cosmos DB tablo API .NET standart SDK'sı & kaynakları
description: Tüm Azure Cosmos DB tablo API'si ve .NET standart SDK'ı yayın tarihleri, sona erme tarihlerini ve her bir sürümü arasında yapılan değişiklikler hakkında bilgi edinin.
author: wmengmsft
ms.author: wmeng
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: dotnet
ms.topic: reference
ms.date: 10/18/2018
ms.openlocfilehash: ce7cc489b107ce4bd95270b9a7f8cb560a2d2398
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55249655"
---
# <a name="azure-cosmos-db-table-net-standard-api-download-and-release-notes"></a>Azure Cosmos DB tablosu .NET standart API: İndirme ve sürüm notları
> [!div class="op_single_selector"]

> * [.NET](table-sdk-dotnet.md)
> * [.NET Standard](table-sdk-dotnet-standard.md)
> * [Java](table-sdk-java.md)
> * [Node.js](table-sdk-nodejs.md)
> * [Python](table-sdk-python.md)

|   |   |
|---|---|
|**SDK'sını indirme**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table)|
|**Geçerli desteklenen çerçevesi**|[Microsoft .NET Standard 2.0](https://www.nuget.org/packages/NETStandard.Library)|

## <a name="release-notes"></a>Sürüm notları

### <a name="a-name0101-preview0101-preview"></a><a name="0.10.1-preview"/>0.10.1-Preview
* SAS belirteci, Azure depolama tablo uç noktalarına karşı TablePermissions, ServiceProperties ve ServiceStats işlemler için destek eklendi. 
   > [!NOTE]
   > Önceki Azure depolama tablo SDK'ları, bazı işlevler henüz, istemci tarafı şifreleme gibi desteklenmez.

### <a name="a-name0100-preview0100-preview"></a><a name="0.10.0-preview"/>0.10.0-Preview
* Temel CRUD, batch ve Azure depolama tablo uç noktalarına karşı sorgu işlemleri için destek eklendi. 
   > [!NOTE]
   > Önceki Azure depolama tablo SDK'ları, bazı işlevler henüz, istemci tarafı şifreleme gibi desteklenmez.

### <a name="a-name091-preview091-preview"></a><a name="0.9.1-preview"/>0.9.1-Preview
* Azure Cosmos DB tablosu .NET standart SDK Cosmos DB tablo veri modeline etkili erişim sağlayan bir platformlar arası .NET kitaplıktır. Bu ilk sürümde eksiksiz bir tablo ve varlık CRUD + benzer API'leri ile sorgu işlevlerini destekler [.NET Framework için Cosmos DB tablosu SDK](table-sdk-dotnet.md). 
   > [!NOTE]
   >  Azure tablo depolama uç noktaları 0.9.1-preview sürümü henüz desteklenmiyor.

## <a name="release-and-retirement-dates"></a>Yayın ve sona erme tarihleri
Microsoft'un sağladığı bildirim en az **12 ay** yeni/desteklenen bir sürüme geçiş hafifletmek için bir SDK'yı devre dışı bırakmadan önce.

| Sürüm | Yayınlanma Tarihi | Sona erme tarihi |
| --- | --- | --- |
| [0.10.1-Preview](#0.10.1-preview) |22 Ocak 2019 |--- |
| [0.10.0-Preview](#0.10.0-preview) |18 Aralık 2018'e |--- |
| [0.9.1-Preview](#0.9.1-preview) |18 Ekim 2018 |--- |


## <a name="faq"></a>SSS

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.
Azure Cosmos DB tablo API'si hakkında daha fazla bilgi için bkz: [Azure Cosmos DB tablo API'sine giriş](table-introduction.md). 
