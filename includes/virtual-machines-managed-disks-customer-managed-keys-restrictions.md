---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 08/24/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: b660d3a0d49de80ed85cfbdcdf8e28b9828cbf26
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91545334"
---
- Yalnızca 2.048-bit, 3.072-bit ve 4.096-bit boyutlarının [yazılım ve HSM RSA anahtarları](../articles/key-vault/keys/about-keys.md) desteklenir, başka hiçbir anahtar veya boyut yoktur.
    - [HSM](../articles/key-vault/keys/hsm-protected-keys.md) anahtarları Için Azure Anahtar Kasası 'nın **Premium** katmanı gerekir.
- Sunucu tarafı şifreleme ve müşterinin yönettiği anahtarlar kullanılarak şifrelenen özel görüntülerden oluşturulan diskler, müşteri tarafından yönetilen aynı anahtar kullanılarak şifrelenmelidir ve aynı abonelikte olmalıdır.
- Sunucu tarafı şifreleme ve müşteri tarafından yönetilen anahtarlarla şifrelenen disklerden oluşturulan anlık görüntüler, müşteri tarafından yönetilen aynı anahtarlarla şifrelenmelidir.
- Müşteri tarafından yönetilen anahtarlarınızla ilgili tüm kaynakların (Azure Anahtar kasaları, disk şifreleme kümeleri, VM 'Ler, diskler ve anlık görüntüler) aynı abonelikte ve bölgede olması gerekir.
- Müşteri tarafından yönetilen anahtarlarla şifrelenen diskler, anlık görüntüler ve görüntüler başka bir kaynak grubuna ve aboneliğe taşınamaz.
- Azure disk şifrelemesi kullanılarak Şu anda veya daha önce şifrelenen yönetilen diskler, müşteri tarafından yönetilen anahtarlar kullanılarak şifrelenemez.
- , Her abonelik için bölge başına en fazla 50 disk şifreleme kümesi oluşturabilir.
- Paylaşılan görüntü galerileriyle müşteri tarafından yönetilen anahtarları kullanma hakkında daha fazla bilgi için bkz. [Önizleme: görüntüleri şifrelemek için müşteri tarafından yönetilen anahtarları kullanma](../articles/virtual-machines/image-version-encryption.md).
