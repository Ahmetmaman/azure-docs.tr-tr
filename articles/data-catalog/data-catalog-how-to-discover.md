---
title: Azure veri Kataloğu'nda veri kaynaklarını bulma
description: Bu makalede, arama, filtreleme ve Azure veri Kataloğu portalının isabet vurgulama özellikleri kullanarak da dahil olmak üzere, Azure veri Kataloğu ile kayıtlı veri varlıklarını nasıl bulacağınızı vurgulanır.
services: data-catalog
author: steelanddata
ms.author: maroche
ms.assetid: f72ae3a3-6573-4710-89a7-f13555e1968c
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: f8e722073db967752747511e47921aec3c4a281a
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43053608"
---
# <a name="how-to-discover-data-sources-in-azure-data-catalog"></a>Azure veri Kataloğu'nda veri kaynaklarını bulma
## <a name="introduction"></a>Giriş
Azure veri Kataloğu, kayıt ve kurumsal veri kaynakları için bulma sistemi olarak görev yapan tam yönetilen bir bulut hizmetidir. Diğer bir deyişle, keşfedin, anlamak ve veri kaynaklarını kullanan kişiler veri Kataloğu yardımcı olur ve bu kuruluşların var olan verilerden daha fazla değer elde etmesine yardımcı olur. Bir veri kaynağı, veri Kataloğu'na kaydedildikten sonra ihtiyacınız olan verileri bulmak için kolayca arama yapabilmesi meta verileri hizmet tarafından dizine alınır.

## <a name="searching-and-filtering"></a>Arama ve filtreleme
Veri Kataloğu'nda bulma işlemi iki birincil mekanizmayı kullanır: arama ve filtreleme.

Arama hem sezgisel hem de güçlü olacak şekilde tasarlanmıştır. Varsayılan olarak, arama terimleri kullanıcı tarafından ek açıklamalar dahil olmak üzere katalogdaki herhangi bir özellikle eşleştirilir.

Filtreleme, aramayı tamamlamak üzere tasarlanmıştır. Uzmanlar, veri kaynağı türü, nesne türü ve etiketler gibi belirli özellikleri seçebilirsiniz. Yalnızca eşleşen veri varlıklarını görüntülemek ve arama sonuçlarını eşleşen sınırlamak.

Aramayı ve filtrelemeyi, bir birleşimini kullanarak ihtiyacınız veri kaynaklarını bulmak için veri Kataloğu ile kayıtlı veri kaynaklarına hızlıca gidebilirsiniz.

## <a name="search-syntax"></a>Söz dizimi arama
Varsayılan serbest metin arama, basit ve sezgisel olsa da, veri kataloğu arama söz dizimi arama sonuçları üzerinde daha fazla denetim için kullanabilirsiniz. Veri kataloğu arama, aşağıdaki tekniklerden destekler:

| Teknik | Kullanım | Örnek |
| --- | --- | --- |
| Temel arama |Bir veya daha fazla arama terimi kullanan temel arama. Sonuçları herhangi bir özelliği bir veya daha fazla belirtilen koşulları ile eşleşen tüm varlıkları içerir. |`sales data` |
| Özellik kapsamı |Burada arama teriminin belirtilen özellikle eşleştirildiği veri kaynakları döndürür. |`name:finance` |
| Boole işleçleri |Genişletmek veya Boolean işlemlerini kullanarak bir arama daraltın. |`finance NOT corporate` |
| Parantez ile gruplandırma |Özellikle, Boole işleçleri ile birlikte mantıksal ayırma sağlamak için sorgunun bölümlerini gruplandırmak üzere parantez kullanın. |`name:finance AND (tags:Q1 OR tags:Q2)` |
| Karşılaştırma işleçleri |Sayısal ve tarih veri türlerine sahip özellikler için eşitlik dışındaki karşılaştırmaları kullanın. |`modifiedTime > "11/05/2014"` |

Veri kataloğu arama hakkında daha fazla bilgi için bkz: [Azure veri Kataloğu](https://msdn.microsoft.com/library/azure/mt267594.aspx) makalesi.

## <a name="hit-highlighting"></a>İsabet vurgulama
Arama sonuçları, (örneğin, veri varlık adı, açıklama ve etiket) belirtilen arama terimleriyle eşleşen özellikler sağlamak için vurgulanan tüm görüntülenen görüntülediğinizde belirli veri varlığının neden belirleyebilmek için verilen arama tarafından döndürüldü.

> [!NOTE]
> İsabet vurgulama devre dışı bırakmak için kullanmak **vurgulayın** geçin veri Kataloğu Portalı'nda.
>
>

Arama sonuçlarını görüntülerken neden bile isabet etkin vurgulama ile bir veri varlığına dahil olduğunu, her zaman açık olmayabilir. Tüm özellikler varsayılan olarak aranır olduğundan, bir veri varlığı nedeniyle bir eşleşme sütun düzeyinde özellikte döndürülmesi. Ve arama sonuçları listesinde birden fazla kullanıcı, kendi etiketleri ve açıklamaları kayıtlı veri varlıklarına açıklama ekleyebilirsiniz, çünkü tüm meta verileri görüntülenebilir.

Döşeme görünümü varsayılan olarak, arama sonuçlarında görüntülenen her bir kutucuk içeren bir **görünümü arama terimini** simgesi, böylece eşleşir ve konumlarını ve isterseniz bunları atlamak için hızlı bir şekilde görüntüleyebilirsiniz.

 ![İsabet vurgulama ve Azure veri Kataloğu Portalı'nda arama sonuçları](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Özet
Veri Kataloğu ile bir veri kaynağı kaydetme yapısal ve açıklayıcı meta verileri veri kaynağından Kataloğu hizmetine kopyalar için veri kaynağı bulunmasını ve anlaşılmasını kolaylaştırmak hale gelir. Bir veri kaynağı kaydettikten sonra filtreleme kullanarak keşfedin ve veri Kataloğu portalını aramak.

## <a name="next-steps"></a>Sonraki adımlar
* Veri kaynaklarını bulma hakkında adım adım ayrıntıları için bkz. [Azure veri Kataloğu ile çalışmaya başlama](data-catalog-get-started.md).
