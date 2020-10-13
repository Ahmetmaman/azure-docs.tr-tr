---
title: Azure Izleyici çalışma kitapları erişim denetimi
description: Rol tabanlı erişim denetimi ile önceden oluşturulmuş ve özel parametreli çalışma kitapları ile karmaşık raporlamayı kolaylaştırın
services: azure-monitor
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.openlocfilehash: 9c0b5dd72cf82649e7f659d23606d23aea62ac82
ms.sourcegitcommit: 50802bffd56155f3b01bfb4ed009b70045131750
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91932558"
---
# <a name="access-control"></a>Erişim denetimi

Çalışma kitaplarında erişim denetimi iki şeyi ifade eder:

* Çalışma kitabındaki verileri okumak için erişim gerekiyor. Bu erişim, çalışma kitabında kullanılan kaynaklarda standart [Azure rolleri](../../role-based-access-control/overview.md) tarafından denetlenir. Çalışma kitapları, bu kaynaklara erişim belirtmez veya yapılandırmaz. Kullanıcılar bu kaynaklara genellikle bu kaynaklara ait [Izleme okuyucu](../../role-based-access-control/built-in-roles.md#monitoring-reader) rolünü kullanarak bu kaynaklara erişir.

* Çalışma kitaplarını kaydetmek için erişim gerekli

    - Özel `("My")` çalışma kitaplarını kaydetme ek ayrıcalık gerektirmez. Tüm kullanıcılar özel çalışma kitaplarını kaydedebilir ve yalnızca bu çalışma kitaplarını görebilirler.
    - Paylaşılan çalışma kitaplarını kaydetme, çalışma kitabını kaydetmek için bir kaynak grubunda yazma ayrıcalıkları gerektirir. Bu ayrıcalıklar genellikle [Izleme katılımcısı](../../role-based-access-control/built-in-roles.md#monitoring-contributor) rolü tarafından belirtilir, ancak *çalışma kitaplarının katkıda* bulunan rolü aracılığıyla da ayarlanabilir.
    
## <a name="standard-roles-with-workbook-related-privileges"></a>Çalışma kitabıyla ilgili ayrıcalıklar içeren standart roller

[Izleme okuyucu](../../role-based-access-control/built-in-roles.md#monitoring-reader) , kaynaklardan verileri okumak için izleme araçları (çalışma kitapları dahil) tarafından kullanılan standart/Read ayrıcalıklarını içerir.

[Katkıda](../../role-based-access-control/built-in-roles.md#monitoring-contributor) bulunanı izlemek, `/write` öğeleri kaydetmek için çeşitli izleme araçları tarafından kullanılan genel ayrıcalıkları içerir ( `workbooks/write` paylaşılan çalışma kitaplarını kaydetme ayrıcalıkları dahil).
"Çalışma kitapları katılımcısı" paylaşılan çalışma kitaplarını kaydetmek için bir nesneye "çalışma kitapları/yazma" ayrıcalıklarını ekler.
Kullanıcıların yalnızca görebileceği özel çalışma kitaplarını kaydedebilmeleri için özel ayrıcalıklar gerekmez.

Özel rol tabanlı erişim denetimi için:

`microsoft.insights/workbooks/write`Paylaşılan çalışma kitaplarını kaydetmek için ekleyin. Daha ayrıntılı bilgi için bkz. [çalışma kitabı katılımcısı](../../role-based-access-control/built-in-roles.md#monitoring-contributor) rolü.

## <a name="next-steps"></a>Sonraki adımlar

* Çok sayıda zengin görselleştirmeler seçeneği hakkında daha fazla [bilgi edinmeye başlayın](workbooks-visualizations.md) .
