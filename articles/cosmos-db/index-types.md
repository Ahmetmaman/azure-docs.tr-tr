---
title: Azure Cosmos DB'de dizin türleri
description: Azure Cosmos DB'de dizin türlerine genel bakış
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 3/13/2019
ms.author: mjbrown
ms.openlocfilehash: 56c0fcb24ac5d255c6a36bcffd327df76f459963
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57990547"
---
# <a name="index-types-in-azure-cosmos-db"></a>Azure Cosmos DB'de dizin türleri

Yolu için dizin oluşturma ilkesini yapılandırdığınız birden çok seçenek vardır. Her yol için bir veya daha fazla dizin tanımları belirtebilirsiniz:

- **Veri türü:** Dize, sayı, nokta, çokgen veya LineString (yol başına veri türü başına yalnızca bir girdi içerebilir).

- **Dizin türü:** Aralık (eşitlik, aralığı veya ORDER BY sorguları) veya uzamsal (uzamsal sorgular).

- **Duyarlık:** Bir aralık için en yüksek duyarlık değeri -1, aynı zamanda varsayılan değer olan dizinidir.

## <a name="index-kind"></a>Dizin türü

Azure Cosmos DB, dize veya sayı veri türleri için yapılandırılmış her yolun veya her ikisi de aralık dizini destekler.

- **Aralık dizini** verimli eşitlik sorguları, birleştirme sorgular, aralık sorguları destekler (kullanarak >, <>, =, < =,! =) ve ORDER BY sorgular. ORDER By sorguları varsayılan olarak, ayrıca maksimum dizin duyarlık (-1) gerektirir. Veri türü dize veya sayı olabilir.

- **Uzamsal dizin** destekler verimli uzamsal (içinde ve uzaklık) sorgular. Veri türü, nokta, çokgen veya LineString olabilir. Azure Cosmos DB, nokta, çokgen veya LineString veri türleri için belirtilebilir her yol için uzamsal dizin türü de destekler. Değer belirtilen yolda gibi geçerli bir GeoJSON parçası olmalıdır {"type": "Nokta", "koordinatları": [0.0, 10.0]}. Azure Cosmos DB, otomatik dizin oluşturma noktası Çokgen ve LineString veri türlerini destekler.

Uzaysal dizinler, hizmet için kullanılabilir ve aralık sorguların örnekleri aşağıda verilmiştir:

| **Dizin türü** | **Açıklama/kullanım örneği** |
| ---------- | ---------------- |
| Aralık      | / Prop / aralığı? (veya /) aşağıdaki sorguları verimli bir şekilde sunmak için kullanılabilir:<br><br>SELECT FROM koleksiyon c WHERE c.prop = "değer"<br><br>SELECT FROM koleksiyon c WHERE c.prop > 5<br><br>Koleksiyon c ORDER BY c.prop seçin<br><br>Aralık / özellikler / [] /? (veya / veya/Özellikler /) aşağıdaki sorguları verimli bir şekilde sunmak için kullanılabilir:<br><br>WHERE etiketi seçin etiketi koleksiyon c birleşim etiketi IN c.props = 5  |
| Uzamsal    | / Prop / aralığı? (veya /) aşağıdaki sorguları verimli bir şekilde sunmak için kullanılabilir:<br><br>Burada ST_DISTANCE(c.prop, {"type": Koleksiyon C'den seçin "Nokta", "koordinatları": [0.0, 10.0]}) < 40<br><br>Burada ST_WITHIN(c.prop, {"type": Koleksiyon C'den seçin "",...} Noktası) --Etkin noktalarında dizin ile<br><br>Burada ST_WITHIN({"type": Koleksiyon C'den seçin "Çokgen",...}, c.prop)--üzerinde çokgenler etkin dizin ile. |

## <a name="default-behavior-of-index-kinds"></a>Dizin türleri varsayılan davranışı

- Sinyal bir tarama sorgu böyle bir durumda, varsayılan olarak, hizmet vermek gerekli olabilir herhangi kesinlik aralığı dizin yok ise sorgularla işleçler gibi aralığı için bir hata döndürülür > =.

- "X-ms-documentdb-enable-tarama" üst bilgisinde REST API veya "EnableScanInQuery" istek seçeneği .NET SDK'sı kullanarak olmadan bir aralık dizini aralık sorguları gerçekleştirilebilir. Azure Cosmos DB dizine göre filtrelemek için kullanabileceğiniz sorgusunda herhangi bir filtre varsa, hata döndürülür.

- Uzamsal dizin veya dizinden sunulabilen diğer filtrelerle değilse, varsayılan olarak, uzamsal sorgular için bir hata döndürülür. Bir tarama sorgularını x-ms-documentdb-enable-tarama veya EnableScanInQuery kullanarak gerçekleştirilebilir.

## <a name="index-precision"></a>Dizin duyarlık

> [!NOTE]
> Azure Cosmos kapsayıcıları, en yüksek duyarlık value(-1) dışında özel dizin kesinliği artık gerektiren yeni bir dizin düzenini destekler. Bu yöntemle yolları ile verilen duyarlık her zaman dizine eklenir. Dizin oluşturma ilkesi duyarlık değeri belirtirseniz, kapsayıcılar bir CRUD istekleri duyarlık değeri sessizce yoksayar ve kapsayıcı yanıttan yalnızca en yüksek duyarlık value(-1) içerir.  Tüm yeni Cosmos kapsayıcılar, varsayılan olarak yeni dizin düzeni kullanın.

- Dizin duyarlık dizin depolama yükü ve sorgu performansı arasında bir denge sağlamak için kullanabilirsiniz. Sayılar için varsayılan duyarlık yapılandırma-1 (maksimum) kullanmanızı öneririz. Sayı 8 baytlık JSON olduğundan, bu 8 baytlık bir yapılandırmaya eşdeğerdir. Bazı aralıklar dahilinde değerleri aynı eşleyin anlamına gelir, duyarlığı, 1 ile 7 arasında gibi daha düşük bir değer seçme giriş dizini. Bu nedenle, dizin depolama alanı azaltabilir, ancak sorgu yürütme daha fazla öğe işlemek zorunda kalabilirsiniz. Sonuç olarak, daha fazla aktarım hızı/RU tüketir.

- Dizin duyarlık dize aralıklarıyla daha pratik uygulama vardır. Dizeleri herhangi bir rastgele uzunluktaki olabileceğinden, dizin duyarlık seçimi dize aralığı sorguların performansını etkileyebilir. Ayrıca, gerekli olan dizin depolama alanı miktarı da etkileyebilir. Dizinleri dizesi aralık 1 ile 100 veya -1 (maksimum) arasında bir dizin duyarlıkla yapılandırılabilir. Dize özellikleri ORDER BY sorguları gerçekleştirmek istiyorsanız, bir duyarlık karşılık gelen yollarla için-1 belirtmeniz gerekir.

- Uzaysal dizinler, her zaman varsayılan dizini duyarlık tüm türleri için (Point, LineString ve Çokgen) kullanın. Uzaysal dizinler için varsayılan dizin duyarlık geçersiz kılınamaz.

Bir sorgu ORDER BY kullanır ancak karşı en yüksek duyarlık sorgulanan yoluyla bir aralık dizini yok, azure Cosmos DB, bir hata döndürür.

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'de dizinleme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Dizin oluşturma genel bakış](index-overview.md)
- [Dizin oluşturma ilkesi](indexing-policies.md)
- [Dizin yolları](index-paths.md)

