---
title: HBv2-Series-Azure sanal makineleri
description: HBv2 serisi VM 'Ler için Özellikler.
author: vermagit
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 10/09/2020
ms.author: amverma
ms.reviewer: jushiman
ms.openlocfilehash: 13fc9d3574243c2403f93489a398a461c5392de7
ms.sourcegitcommit: 436518116963bd7e81e0217e246c80a9808dc88c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98918763"
---
# <a name="hbv2-series"></a>HBv2 serisi

HBv2 serisi VM 'Ler, akıcı Dynamics, sınırlı öğe analizi ve rezervoır benzetimi gibi bellek bant genişliğine göre çalışan uygulamalar için iyileştirilmiştir. HBv2 VM 'Ler özelliği 120 AMD EPIC 7742 işlemci çekirdekleri, CPU çekirdeği başına 4 GB RAM ve eşzamanlı çoklu iş parçacığı yok. Her HBv2 VM, en fazla 340 GB/sn bellek bant genişliği ve en fazla 4 işlem FP64 işlem sağlar.

HBv2 serisi VM 'Ler özelliği 200 GB/sn Mellanox HDR InfiniBand. Bu VM 'Ler, iyileştirilmiş ve tutarlı RDMA performansı için engelleyici olmayan bir FAT ağacına bağlanır. Bu VM 'Ler, uyarlamalı yönlendirmeyi ve dinamik bağlı taşımayı (Standart RC ve UD aktarımlarında ek olarak) destekler. Bu özellikler uygulama performansı, ölçeklenebilirliği ve tutarlılığı geliştirir ve bunların kullanımı kesinlikle önerilir.

[Premium Depolama](premium-storage-performance.md): desteklenir<br>
[Premium depolama önbelleği](premium-storage-performance.md): desteklenir<br>
[Dinamik geçiş](maintenance-and-updates.md): desteklenmiyor<br>
[Güncelleştirmeleri koruyan bellek](maintenance-and-updates.md): desteklenmiyor<br>
[VM oluşturma desteği](generation-2.md): 1. nesil<br>
[Hızlandırılmış ağ](../virtual-network/create-vm-accelerated-networking-cli.md): desteklenir<br>
<br>

| Boyut | Sanal işlemci | İşlemci | Bellek (GiB) | Bellek bant genişliği GB/sn | Taban CPU sıklığı (GHz) | Tüm çekirdekler sıklığı (GHz, tepe) | Tek çekirdekli sıklık (GHz, tepe) | RDMA performansı (GB/sn) | MPı desteği | Geçici depolama (GiB) | Maksimum veri diskleri | En fazla Ethernet vNIC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HB120rs_v2 | 120 | AMD EPYıC 7V12 | 480 | 350 | 2.45 | 3,1 | 3.3 | 200 | Tümü | 480 + 960 | 8 | 8 |


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaçlı](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Sanal makinelerinizi yapılandırma](./workloads/hpc/configure.md)hakkında daha fazla bilgi edinin, [InfiniBand 'Yi etkinleştirir](./workloads/hpc/enable-infiniband.md) [ve Azure](./workloads/hpc/setup-mpi.md) için HPC uygulamalarını [HPC iş yüklerinde](./workloads/hpc/overview.md)optimize edin.
- En son duyurular ve bazı HPC örnekleri hakkında bilgi edinin ve [Azure Işlem teknik topluluk bloglarında](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute)bu sonuçları elde edin.
- Çalıştırılan HPC iş yüklerinin daha yüksek düzey mimari görünümü için bkz. [Azure 'Da yüksek performanslı bilgi işlem (HPC)](/azure/architecture/topics/high-performance-computing/).
- Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.
