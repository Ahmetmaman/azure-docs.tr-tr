---
title: dosya dahil etme
description: dosya dahil etme
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 08/28/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 7d0286b63703c165dda6cd12bb625fc64272aac1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91376514"
---
Azure dosyaları, kendi senaryolarınızın performans ve fiyat gereksinimlerine yönelik olarak paylaşımlarınızı uyarlamanızı sağlamak için dört farklı depolama, Premium, işlem için iyileştirilmiş, sık erişimli ve seyrek katman katmanları sunar:

- **Premium**: Premium dosya paylaşımları katı hal sürücüleri (SSD 'ler) tarafından desteklenir ve **FileStorage depolama hesabı** türüne dağıtılır. Premium dosya paylaşımları, GÇ yoğun iş yükleri için çoğu GÇ işlemleri için tek basamaklı milisaniye içinde tutarlı yüksek performans ve düşük gecikme sağlar. Premium dosya paylaşımları veritabanları, Web sitesi barındırma ve geliştirme ortamları gibi çok çeşitli iş yükleri için uygundur. Premium dosya paylaşımları, sunucu Ileti bloğu (SMB) ve ağ dosya sistemi (NFS) protokolleriyle birlikte kullanılabilir.
- **İşlem için iyileştirilmiş**: işlem için iyileştirilmiş dosya paylaşımları Premium dosya paylaşımları tarafından sunulan gecikmeye gerek olmayan işlem ağır iş yüklerini etkinleştirir. İşlem için iyileştirilmiş dosya paylaşımları, sabit disk sürücüleri (HDD 'Ler) tarafından desteklenen standart depolama donanımında sunulur ve **genel amaçlı sürüm 2 (GPv2) depolama hesabı** türünde dağıtılır. İşlem için iyileştirilmiş, "standart" olarak adlandırılmıştı, ancak bu, katmanın kendisi yerine depolama medyası türü anlamına gelir (Standart depolama donanımında olduklarından, sık ve seyrek da "standart" katmanlarıdır).
- **Sık**erişimli: sık erişimli dosya paylaşımları, takım paylaşımları ve Azure dosya eşitleme gibi genel amaçlı dosya paylaşım senaryolarında en iyi duruma getirilmiş depolama sunar. Etkin dosya paylaşımları, HDD 'Ler tarafından desteklenen standart depolama donanımında sunulur ve **genel amaçlı sürüm 2 (GPv2) depolama hesabı** türünde dağıtılır.
- Seyrek **erişimli: seyrek**erişimli dosya paylaşımları, çevrimiçi arşiv depolama senaryoları için iyileştirilmiş uygun maliyetli depolama sunar. Azure Dosya Eşitleme, daha düşük dalgalanma iş yükleri için de uygun olabilir. Seyrek Erişimli dosya paylaşımları, HDD 'Ler tarafından desteklenen standart depolama donanımında sunulur ve **genel amaçlı sürüm 2 (GPv2) depolama hesabı** türünde dağıtılır.

Premium dosya paylaşımları yalnızca sağlanan faturalandırma modelinde kullanılabilir. Premium dosya paylaşımları için sağlanan faturalandırma modeli hakkında daha fazla bilgi için bkz. [Premium dosya paylaşımları için sağlamayı anlama](../articles/storage/files/storage-files-planning.md#understanding-provisioning-for-premium-file-shares). İşlem için iyileştirilmiş, sık ve seyrek erişimli dosya paylaşımları dahil olmak üzere standart dosya paylaşımları, Kullandıkça Öde faturalandırmasıyla sunulmaktadır.

Sık erişimli ve seyrek erişimli dosya paylaşımları, tüm Azure Kamu ve Azure Kamu bölgelerinde kullanılabilir. İşlem için iyileştirilmiş dosya paylaşımları, Azure Çin ve Azure Almanya bölgeleri dahil olmak üzere tüm Azure bölgelerinde kullanılabilir.

> [!Important]  
> GPv2 depolama hesabı türleri içindeki katmanlar arasında dosya paylaşımlarını taşıyabilirsiniz (işlem için iyileştirilmiş, sık erişimli ve seyrek erişimli). Katmanlar arasında paylaşma işlemleri işlemler arası işlemler: bir Hotter katmanından daha soğuk bir katmana geçiş yapmak, daha soğuk bir katmandan bir dosya için daha soğuk katman yazma işlemi ücreti uygular, ancak daha soğuk bir katmanda bulunan her dosya için seyrek katman okuma işlemi ücreti olur.