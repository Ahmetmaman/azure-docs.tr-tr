---
title: NCas T4 v3-serisi
description: NCas T4 v3 serisi VM 'Leri için belirtim.
services: virtual-machines
ms.subservice: sizes
author: vikancha-MSFT
ms.service: virtual-machines
ms.topic: conceptual
ms.date: 08/10/2020
ms.author: vikancha
ms.openlocfilehash: 17ce5314f58a92158ff4fd187ad0ca46bb14a275
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91320175"
---
# <a name="ncast4_v3-series-in-preview"></a>NCasT4_v3 serisi (önizlemede) 

NCasT4_v3 serisi sanal makineler, [NVIDIA Tesla T4](https://www.nvidia.com/en-us/data-center/tesla-t4/) GPU 'LARı ve AMD Epde 7V12 (Roma) CPU 'lar tarafından desteklenir. VM 'Ler, en fazla 64, çok iş parçacıklı AMD EPIC 7V12 (Roma) işlemci çekirdeği ve 440 GiB 'a kadar, her biri 16 GB bellek içeren 4 KB 'a kadar NVıDıA T4 GPU özelliğine sahiptir. Bu sanal makineler, Kullanıcı tarafından oluşturulan isteklerin gerçek zamanlı olarak kullanılması veya NVıDıA 'ın KıLAVUZ sürücüsü ve sanal GPU teknolojisini kullanan etkileşimli grafikler ve görselleştirme iş yükleri gibi AI hizmetlerini dağıtmak için idealdir. CUDA, TensorRT, Caffe, ONNX ve diğer çerçeveleri temel alan standart GPU hesaplama iş yükleri ya da GPU hızlandırmalı grafik uygulamaları tabanlı OpenGL ve DirectX, NCasT4_v3 serisinde kullanıcılara yakın bir şekilde dağıtılır.

[Bir isteği](https://aka.ms/NCT4v3Preview) , önizleme programının bir parçası olarak gönderebilirsiniz.

<br>

ACU: 230-260

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

Dinamik Geçiş: desteklenmiyor

Güncelleştirmeleri koruyan bellek: desteklenmiyor

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU belleği: GiB | Maksimum veri diskleri | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NC4as_T4_v3 |4 |28 |180 | 1 | 16 | 8 | 2 |
| Standard_NC8as_T4_v3 |8 |56 |360 | 1 | 16 | 16 | 4  |
| Standard_NC16as_T4_v3 |16 |110 |360 | 1 | 16 | 32 | 8  |
| Standard_NC64as_T4_v3 |64 |440 |2880 | 4 | 64 | 32 | 8  |


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

Windows veya Linux çalıştıran Azure NCasT4_v3 serisi VM 'lerin GPU Özellikleri avantajlarından yararlanmak için NVIDIA GPU sürücüleri yüklenmelidir.

NVIDIA GPU sürücülerini el ile yüklemek için desteklenen işletim sistemleri, sürücüler, yükleme ve doğrulama adımları için bkz. [Windows Için N SERISI GPU sürücü kurulumu](./windows/n-series-driver-setup.md) .

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaçlı](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.
