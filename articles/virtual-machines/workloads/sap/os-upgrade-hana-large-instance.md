---
title: İşletim sistemi yükseltme (büyük örnekler) azure'da SAP HANA için | Microsoft Docs
description: (Büyük örnekler) Azure üzerinde SAP HANA için işletim sistemi yükseltmesi gerçekleştirin
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/28/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d7d451f3831309b4755170915b35a23da8910510
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56100763"
---
# <a name="operating-system-upgrade"></a>İşletim sistemi yükseltmesi
Bu belgede, işletim sistemi yükseltmeleri HANA büyük örnekleri hakkında ayrıntılar açıklanmaktadır.

>[!NOTE]
>İşletim sistemi yükseltme, müşterilerin sorumluluğundadır, Microsoft operations desteğine yükseltme sırasında dikkat edilmesi gereken temel alanlar için rehberlik sağlayabilir. Yükseltme için planlama önce de işletim sistemi satıcınıza danışmalısınız.

Sağlama HLI birim zaman Microsoft Operasyon ekibinin işletim sistemini yükleyin. Zamanla, işletim sistemi tutmaları zorunludur (örnek: Düzeltme eki uygulama, ayarlama, yükseltme vs.) HLI birim üzerinde.

İşletim sistemine (örneğin, SP2'ye yükseltme SP1) değişiklikleri ana önce başvurmanız bir destek bileti açarak Microsoft Operations team başvurmanız gerekir.

Lütfen, anahtar şunlardır:

* HLI abonelik kimliğinizi
* Sunucu adınız.
* Düzeltme eki düzeyi uygulamak için planlama.
* Bu değişiklik planladığınıza tarih. 

En az bir hafta bir üretici yazılımını yükseltme, sunucusu dikey penceresinde gerekli olursa denetimi operasyon ekibinin olması nedeniyle istenen yükseltme tarihinden önce bu bilet öneririz.


Farklı SAP HANA sürümleriyle farklı Linux sürümleri için destek matrisi bkz [SAP notu #2235581](https://launchpad.support.sap.com/#/notes/2235581).


## <a name="known-issues"></a>Bilinen sorunlar

Aşağıda yükseltme sırasında bazı sık karşılaşılan bilinen sorunlar verilmiştir:
- İşletim sistemi yükseltme yaptıktan sonra SKU türü II sınıfı SKU, software foundation yazılımı (SFS) kaldırılır. İşletim sistemi yükseltme yaptıktan sonra uyumlu SFS yeniden yüklemeniz gerekir.
- Ethernet Kart sürücüleri (ENIC ve FNIC) daha eski bir sürümüne geri alındı. Yükseltmeden sonra sürücüleri'nın uyumlu sürümünü yeniden yüklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar
- Başvuru [yedekleme ve geri yükleme](hana-overview-high-availability-disaster-recovery.md) işletim sistemi için yedekleme türü ı SKU sınıfı.
- Başvuru [Type II SKU'lara yönelik işletim sistemi yedeklemesi](os-backup-type-ii-skus.md) türü II SKU sınıfı için.
