---
title: Azure Resource Manager vCPU kota artışı istekleri
description: Azure Resource Manager vCPU kota artışı istekleri
author: sowmyavenkat86
ms.author: svenkat
ms.date: 01/27/2020
ms.topic: how-to
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 2580da2a4ac7b943dee3e5e6ff8bdbd49664505b
ms.sourcegitcommit: ad83be10e9e910fd4853965661c5edc7bb7b1f7c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2020
ms.locfileid: "96745256"
---
# <a name="quota-increase-requests"></a>Kota artışı istekleri

Sanal makineler ve sanal makine ölçek kümeleri için Kaynak Yöneticisi vCPU kotaları, her bir bölgede her bir abonelik için iki katmanda zorlanır.

İlk katman, tüm VM serileri genelinde toplam bölgesel vCPU sayısı sınırı. İkinci katman, D serisi vCPU 'Lar gibi sanal makine başına vCPU sayısı sınırı. Yeni bir sanal makine dağıtılması her zaman, bu VM Serisi için yeni ve var olan vCPU 'ların toplamı, söz konusu VM Serisi için onaylanan vCPU kotasını aşmamalıdır. Ayrıca, tüm VM dizileri arasında dağıtılan toplam yeni ve var olan vCPU sayısı, abonelik için onaylanan toplam bölgesel vCPU kotasını aşmamalıdır. Bu kotalardan biri aşılırsa, VM dağıtımına izin verilmez.
Azure portal VM Serisi için vCPU kota sınırı artışı isteyebilirsiniz. VM Serisi kotasının bir artışı toplam bölgesel vCPU sayısını aynı tutara göre otomatik olarak arttırır.

Yeni bir abonelik oluşturulduğunda, varsayılan toplam bölgesel sanal CPU 'Lar, tüm tek VM serileri için varsayılan vCPU kotaları toplamına eşit olmayabilir. Bu olgu, dağıtmak istediğiniz her bir VM Serisi için yeterince kota içeren bir aboneliğe neden olabilir. Tüm dağıtımlar için toplam bölgesel sanal CPU 'ların yeterli kotası olmayabilir. Bu durumda, toplam bölgesel vCPU sınırını açıkça artırmak için bir istek göndermeniz gerekir. Toplam bölgesel vCPU sayısı, bölgenin tüm VM serileri genelinde onaylanan kotanın toplamını aşamaz.

> [!NOTE]
> Sınırı veya kotayı varsayılan sınırın üzerinde yükseltmek istiyorsanız, [ücretsiz bir çevrimiçi müşteri destek isteği açın](../../azure-resource-manager/templates/error-resource-quota.md#solution).

Kotalar hakkında daha fazla bilgi için bkz. [sanal makine vCPU kotaları](../../virtual-machines/windows/quotas.md) ve [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../../azure-resource-manager/management/azure-subscription-service-limits.md).