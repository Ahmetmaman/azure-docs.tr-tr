---
title: Azure Stream Analytics Pencereleme işlevlerine giriş
description: Bu makalede Azure Stream Analytics işlerinde kullanılan dört Pencereleme işlevi (atlayan, atlamalı, kayan, oturum) açıklanmaktadır.
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 09/16/2020
ms.openlocfilehash: 4c8d2143d2b6e18de2669a6b45961e601cc394bb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91707566"
---
# <a name="introduction-to-stream-analytics-windowing-functions"></a>Stream Analytics Pencereleme işlevlerine giriş

Zaman akışı senaryolarında, zamana bağlı Windows 'da bulunan veriler üzerinde işlem yapma ortak bir modeldir. Stream Analytics,, geliştiricilerin en düşük çabayla karmaşık akış işleme işleri yazmalarını sağlayan, Pencereleme işlevleri için yerel destek içerir.

Arasından seçim yapabileceğiniz beş tür zamana bağlı pencere [**vardır: atlayan**](https://docs.microsoft.com/stream-analytics-query/tumbling-window-azure-stream-analytics), [**atlamalı**](https://docs.microsoft.com/stream-analytics-query/hopping-window-azure-stream-analytics), [**kaydırma**](https://docs.microsoft.com/stream-analytics-query/sliding-window-azure-stream-analytics), [**oturum**](https://docs.microsoft.com/stream-analytics-query/session-window-azure-stream-analytics)ve [**anlık görüntü**](https://docs.microsoft.com/stream-analytics-query/snapshot-window-azure-stream-analytics) pencereleri.  Stream Analytics işlerinizde sorgu sözdiziminin [**Group By**](https://docs.microsoft.com/stream-analytics-query/group-by-azure-stream-analytics) yan tümcesinde pencere işlevlerini kullanırsınız. [ **Windows ()** işlevini](https://docs.microsoft.com/stream-analytics-query/windows-azure-stream-analytics)kullanarak olayları birden çok pencere üzerinde de toplayabilirsiniz.

Tüm [Pencereleme](https://docs.microsoft.com/stream-analytics-query/windowing-azure-stream-analytics) işlemleri çıkış, pencerenin **sonunda** oluşur. Bir Stream Analytics işi başlattığınızda, *iş çıkışı başlangıç saatini* belirtebilir ve sistem, gelen akışlardaki önceki olayları, belirtilen zamanda ilk pencereyi çıkış için otomatik olarak getirir; Örneğin, *Now* seçeneğiyle başladığınızda, verileri hemen yayma başlatılır. Pencerenin çıktısı, kullanılan toplama işlevine göre tek olay olacaktır. Çıkış olayı pencerenin sonundaki zaman damgasına sahip olur ve tüm pencere işlevleri sabit bir uzunluğa göre tanımlanır. 

![Stream Analytics pencere işlevleri kavramları](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Atlayan pencere
Atlayan pencere işlevleri, bir veri akışını ayrı zaman kesimlerine bölmek ve bunlara karşı aşağıdaki örnekte olduğu gibi bir işlev gerçekleştirmek için kullanılır. Atlayan pencerenin farkı tekrarlaması, çakışmaması ve bir olayın birden fazla atlayan pencereye ait olamamasıdır.

![Stream Analytics atlayan pencere](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Hopping penceresi
Atlamalı pencere işlevleri, sabit bir süre kadar ileri gider. Bunları, büyük bir pencere olarak, üst üste binebilir ve pencere boyutundan daha sık atılabilen şekilde düşünmek kolay olabilir. Olaylar birden fazla hopping penceresi sonuç kümesine ait olabilir. Bir atlamalı pencereyi, atlayan bir pencereyle aynı yapmak için, pencere boyutuyla aynı olacak atlama boyutunu belirtin. 

![Stream Analytics atlamalı penceresi](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Kayan pencere

Windows, çıkış olayları yalnızca pencerenin içeriğinin değiştiği zaman noktaları için, çıkış olaylarından farklı olarak kayan pencereler. Diğer bir deyişle, bir olay pencereye girdiğinde veya pencereden çıkar. Her pencerede en az bir olay vardır. bu durum, Windows 'un, bir veya daha fazla kayan pencereye ait olabilir

![Stream Analytics kayan pencere](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="session-window"></a>Oturum penceresi
Oturum penceresi işlevleri, benzer zamanlarda gelen olayları, verilerin olmadığı zaman ölçeğini filtreleyerek gruplar. Üç ana parametreye sahiptir: zaman aşımı, maksimum süre ve bölümleme anahtarı (isteğe bağlı).

![Stream Analytics oturum penceresi](media/stream-analytics-window-functions/stream-analytics-window-functions-session-intro.png)

İlk olay gerçekleştiğinde oturum penceresi başlar. Son alınan olaydan belirtilen zaman aşımı süresi içinde başka bir olay oluşursa, pencere yeni olayı içerecek şekilde genişletilir. Aksi takdirde, zaman aşımı içinde herhangi bir olay gerçekleşemez, pencere zaman aşımından sonra kapatılır.

Olaylar belirtilen zaman aşımı süresi içinde gerçekleşirken, en fazla süreye ulaşılana kadar oturum penceresi Genişlemeden kalır. En uzun süre denetim aralıkları, belirtilen maksimum süre ile aynı boyut olarak ayarlanır. Örneğin, en fazla süre 10 ise, pencerenin en uzun süreyi aşması durumunda üzerindeki denetimler t = 0, 10, 20, 30 vb. olur.

Bir bölüm anahtarı sağlandığında, olaylar anahtar tarafından birlikte gruplandırılır ve oturum penceresi her bir gruba bağımsız olarak uygulanır. Bu bölümlendirme, farklı kullanıcılar veya cihazlar için farklı oturum pencerelerinin olması gereken durumlarda faydalıdır.

## <a name="snapshot-window"></a>Anlık görüntü penceresi

Windows grupları, aynı zaman damgasına sahip olayları görüntüler. Özel bir pencere işlevi gerektiren diğer Pencereleme türlerinden farklı olarak ( [Sessionwindow ()](https://docs.microsoft.com/stream-analytics-query/session-window-azure-stream-analytics)gıbı, group by yan tümcesine System. Timestamp () ekleyerek bir anlık görüntü penceresi uygulayabilirsiniz.

![Stream Analytics anlık görüntü penceresi](media/stream-analytics-window-functions/snapshot.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

