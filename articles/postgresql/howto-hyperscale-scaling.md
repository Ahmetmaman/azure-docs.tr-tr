---
title: Ölçek sunucu grubu-hiper ölçek (Citus)-PostgreSQL için Azure veritabanı
description: Daha fazla yük ile başa çıkmak için sunucu grubu belleği, disk ve CPU kaynaklarını ayarlayın
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 9/18/2020
ms.openlocfilehash: dd59d0b09a28febfc0afe35d9f008ba0e0ee19ab
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91295723"
---
# <a name="server-group-size"></a>Sunucu grubu boyutu

Hyperscale (Citus) dağıtım seçeneği, sorgu yürütmeyi paralel hale getirmek ve daha fazla veri depolamak için birlikte çalışan veritabanı sunucularını kullanır. "Boyut" sunucu grubu, hem sunucu sayısını hem de her birinin donanım kaynaklarını belirtir.

## <a name="picking-initial-size"></a>Başlangıç boyutu seçme

Bir sunucu grubunun boyutu, düğüm sayısı ve bunların donanım kapasitesi açısından kolayca değiştirilebilir ([aşağıya bakın](#scale-a-hyperscale-citus-server-group)). Ancak yine de yeni bir sunucu grubu için bir başlangıç boyutu seçmeniz gerekir. Makul bir seçenek için bazı ipuçları aşağıda verilmiştir.

### <a name="multi-tenant-saas-use-case"></a>Çok kiracılı SaaS Use-Case

Mevcut bir tek düğümlü PostgreSQL veritabanı örneğinden hiper ölçeğe geçiş yapmak için, toplam çalışan sanal çekirdekleri ve RAM sayısının orijinal örneğe eşit olduğu bir küme seçmeniz önerilir. Bu tür senaryolarda, parça kaynak kullanımını iyileştirtiğinden ve daha küçük bir dizin kullanılmasına izin verirken 2 3x performans iyileştirmeleri gördük.

Düzenleyici düğümü için gereken sanal çekirdek sayısı, mevcut iş yükünüze (yazma/okuma aktarım hızı) bağlıdır. Düzenleyici düğümü, çalışan düğümleri olarak çok RAM gerektirmez, ancak sanal çekirdek sayısı temel alınarak ( [Hiperscale (Citus) yapılandırma seçeneklerinde](concepts-hyperscale-configuration-options.md)açıklandığı gibi), sanal çekirdek sayısının gerçek karardır.

### <a name="real-time-analytics-use-case"></a>Real-Time Analytics Use-Case

Toplam Vçekirdekler: RAM 'e uygun veri miktarı kullanıldığında, hiper ölçekte (Citus) bir doğrusal performans geliştirmesini, çalışan çekirdekleri sayısıyla orantılı olarak bekleyebilir. Gereksinimlerinize göre doğru sanal çekirdek sayısını öğrenmek için, tek düğümlü veritabanınızdaki sorgular için geçerli gecikme süresini ve hiper ölçekte gereken gecikme süresini göz önünde bulundurun (Citus). Geçerli gecikme süresini istenen gecikme süresine bölün ve sonucu yuvarlayın.

Çalışan RAM'i: En iyi seçenek, çalışan kümesinin çoğunun belleğe sığabileceği kadar çok bellek sağlamak olacaktır. Uygulamanızın kullandığı sorguların türü bellek gereksinimlerini etkiler. Sorgu üzerinde analizine ilişkin AÇıKLA ' yı çalıştırarak, gereken bellek miktarını belirleyebilirsiniz. Sanal çekirdekler ve RAM 'in, [Hyperscale (Citus) yapılandırma seçenekleri](concepts-hyperscale-configuration-options.md) makalesinde açıklandığı şekilde birlikte ölçeklendirilmesini unutmayın.

## <a name="scale-a-hyperscale-citus-server-group"></a>Hiper ölçek (Citus) sunucu grubunu ölçeklendirme

PostgreSQL için Azure veritabanı-Hyperscale (Citus), daha fazla yük ile başa çıkmak için self servis Ölçeklendirmesi sağlar. Azure portal, yeni çalışan düğümleri eklemeyi ve mevcut düğümlerin sanal çekirdeğini artırmayı kolaylaştırır. Düğüm eklemek kapalı kalma gerektirmez ve hatta [parçaları](#rebalance-shards)yeni düğümlere (parça olarak adlandırılır), sorguları kesintiye uğramadan hareket ettirilmesine neden olur.

### <a name="add-worker-nodes"></a>Çalışan düğümleri Ekle

Düğüm eklemek için, Hyperscale (Citus) sunucu grubundaki **işlem + depolama** sekmesine gidin.  **Çalışan düğümü sayısı** için kaydırıcıyı sürüklemek değeri değiştirir.

:::image type="content" source="./media/howto-hyperscale-scaling/01-sliders-workers.png" alt-text="Kaynak sürgüleri":::

Değiştirilen değerin etkili olması için **Kaydet** düğmesine tıklayın.

> [!NOTE]
> Arttırıldıktan ve kaydedildikten sonra, çalışan düğümlerinin sayısı kaydırıcı kullanılarak azaltılemez.

#### <a name="rebalance-shards"></a>Parçaları yeniden dengeleme

Yeni eklenen düğümlerin avantajlarından yararlanmak için, [Dağıtılmış tablo parçaları](concepts-hyperscale-distributed-data.md#shards)'nı yeniden dengelemeniz gerekir, bu da bazı parçaları mevcut düğümlerden yeni olanlara taşımak anlamına gelir. İlk olarak, yeni çalışanların sağlamayı başarıyla bitirdiğini doğrulayın. Ardından, psql ile küme Düzenleyicisi düğümüne bağlanarak ve çalıştıran parça yeniden dengeleyiciyi başlatın:

```sql
SELECT rebalance_table_shards('distributed_table_name');
```

`rebalance_table_shards`İşlevi, bağımsız değişkeninde adlı tablonun birlikte bulundurma [colocation](concepts-hyperscale-colocation.md) grubundaki tüm tabloları yeniden dengeler. Bu nedenle, her dağıtılmış tablo için işlevi çağırmanız gerekmez, bunu her bir birlikte bulundurma grubundan temsili bir tabloda çağırmanız yeterlidir.

### <a name="increase-or-decrease-vcores-on-nodes"></a>Düğümlerdeki sanal çekirdekleri artırma veya azaltma

Yeni düğümler eklemenin yanı sıra, mevcut düğümlerin yeteneklerini de artırabilirsiniz. İşlem kapasitesini yukarı ve aşağı ayarlamak, performans denemeleri ve trafik taleplerine yönelik kısa veya uzun süreli değişiklikler için yararlı olabilir.

Tüm çalışan düğümlerinin sanal çekirdeğini değiştirmek için **yapılandırma (çalışan düğümü başına)** altındaki **vçekirdekler** kaydırıcısını ayarlayın. Düzenleyici düğümünün sanal çekirdekleri bağımsız olarak ayarlanabilir. **Yapılandırma (düzenleyici düğümü)** altında **vçekirdekler** kaydırıcısını ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar

- Sunucu grubu [performans seçenekleri](concepts-hyperscale-configuration-options.md)hakkında daha fazla bilgi edinin.
