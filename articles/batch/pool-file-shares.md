---
title: Azure Batch havuzları için Azure dosya paylaşma
description: Azure Batch içindeki bir Linux veya Windows havuzundaki işlem düğümlerinden Azure dosyaları paylaşımından bağlama.
ms.topic: how-to
ms.date: 05/24/2018
ms.openlocfilehash: 6bbaba20512a17de563e74ba095057c5c3f71f6b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87986432"
---
# <a name="use-an-azure-file-share-with-a-batch-pool"></a>Bir Batch havuzu ile Azure dosya paylaşma kullanma

[Azure dosyaları](../storage/files/storage-files-introduction.md) , bulutta sunucu ileti bloğu (SMB) protokolü aracılığıyla erişilebilen tam olarak yönetilen dosya paylaşımları sunar. Bu makalede, havuz işlem düğümlerinde bir Azure dosya paylaşımının bağlanması ve kullanılması için bilgi ve kod örnekleri sağlanmaktadır.

## <a name="considerations-for-use-with-batch"></a>Batch ile kullanım konuları

* Premium olmayan Azure dosyaları kullanıyorsanız görece düşük sayıda paralel görevi çalıştıran havuzlarınız varsa Azure dosya paylaşımının kullanılmasını düşünün. Beklenen havuz boyutunuz ve varlık dosyası sayısı verildiğinde, Azure dosyalarının (bir Azure depolama hesabı kullanan) kullanılması gerekip gerekmediğini öğrenmek için [performans ve ölçek hedeflerini](../storage/files/storage-files-scale-targets.md) gözden geçirin. 

* Azure dosya paylaşımları uygun [maliyetli](https://azure.microsoft.com/pricing/details/storage/files/) ve başka bir bölgeye veri çoğaltma ile yapılandırılabilir, böylece küresel olarak yedekli olur. 

* Bir Azure dosya paylaşımından şirket içi bir bilgisayardan aynı anda bağlayabilirsiniz. Ancak, özellikle REST API 'Leri kullanırken [eşzamanlılık etkilerini](../storage/common/storage-concurrency.md) anladığınızdan emin olun.

* Ayrıca bkz. Azure dosya paylaşımları için genel [Planlama konuları](../storage/files/storage-files-planning.md) .


## <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma

Batch hesabınıza veya ayrı bir depolama hesabına bağlı bir depolama hesabında bir [dosya paylaşma oluşturun](../storage/files/storage-how-to-create-file-share.md) .

## <a name="mount-an-azure-file-share-on-a-batch-pool"></a>Bir Batch havuzunda Azure dosya paylaşımından bağlama

Bir [Batch havuzunda sanal dosya sistemi bağlama](virtual-file-mount.md)hakkında bilgi için lütfen belgelere bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Toplu Işteki verileri okuma ve yazma diğer seçenekleri için bkz. [devam eden iş ve görev çıktısı](batch-task-output.md).
* Toplu iş iş yükleri için dosya sistemleri dağıtmak üzere [shipbahçe tariflerini](https://github.com/Azure/batch-shipyard/tree/master/recipes) de Içeren [Batch shipbahçe](https://github.com/Azure/batch-shipyard) araç seti ' ne bakın.
