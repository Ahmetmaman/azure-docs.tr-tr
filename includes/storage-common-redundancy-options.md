---
title: dosya dahil etme
description: dosya dahil etme
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 01/14/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 73c2b742ede21a4e86d717d994f8ebc4f16389c9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "77157236"
---
Depolama hesabı için artıklık seçenekleri şunlardır:

* Yerel olarak yedekli depolama (LRS): basit, düşük maliyetli bir artıklık stratejisi. Veriler, birincil bölge içinde zaman uyumlu olarak üç kez kopyalanır.
* Bölgesel olarak yedekli depolama (ZRS): yüksek kullanılabilirlik gerektiren senaryolar için artıklık. Veriler, birincil bölgedeki üç Azure kullanılabilirlik bölgesi arasında zaman uyumlu olarak kopyalanır.
* Coğrafi olarak yedekli depolama (GRS): bölgesel kesintilere karşı korumak için çapraz bölgesel yedeklilik. Veriler birincil bölgede zaman uyumlu olarak üç kez kopyalanır ve ardından zaman uyumsuz olarak ikincil bölgeye kopyalanır. İkincil bölgedeki verilere yönelik okuma erişimi için Okuma Erişimli Coğrafi olarak yedekli depolamayı (RA-GRS) etkinleştirin.
* Coğrafi bölge yedekli depolama (GZRS) (Önizleme): hem yüksek kullanılabilirlik hem de maksimum dayanıklılık gerektiren senaryolar için artıklık. Veriler, birincil bölgedeki üç Azure kullanılabilirlik bölgesi arasında zaman uyumlu olarak kopyalanır ve sonra zaman uyumsuz olarak ikincil bölgeye kopyalanır. İkincil bölgedeki verilere yönelik okuma erişimi için, Okuma Erişimli Coğrafi bölge yedekli depolamayı (RA-GZRS) etkinleştirin.

Azure depolama 'daki artıklık seçenekleri hakkında daha fazla bilgi için bkz. [Azure Storage yedekliği](../articles/storage/common/storage-redundancy.md).
