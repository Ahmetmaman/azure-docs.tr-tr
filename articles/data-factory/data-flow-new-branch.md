---
title: Yeni dal dönüştürme Azure veri fabrikası eşleme veri akışı
description: Yeni dal dönüştürme Azure veri fabrikası eşleme veri akışı
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: c48c1500a31ce8f22bcef7207c0ea67caca24231
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56272179"
---
# <a name="azure-data-factory-mapping-data-flow-new-branch-transformation"></a>Yeni dal dönüştürme Azure veri fabrikası eşleme veri akışı

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Dal seçenekleri](media/data-flow/menu.png "menüsü")

Dallanma, veri akışında geçerli veri akışını almak ve başka bir akışa çoğaltın. İşlemler ve dönüştürmeler aynı veri akışı karşı birden çok gerçekleştirmek için yeni dal ayarlar.

Örnek: Veri akışınızı, sütunları ve veri türü dönüşümleri seçili kümesiyle bir dönüştürme kaynak vardır. Ardından, bu kaynağı takip türetilmiş sütun de yerleştirebilirsiniz. Türetilmiş sütununda olduğunuz oluşturduğunuz yeni bir alan bu birleştirir ad ve Soyadı yeni bir "tam adı" alanını hale getirin.

Yeni bir akışı dönüşümleri ve bir havuz üzerinde bir satır kümesi ile işle ve burada aynı veriye sahip farklı bir dizi dönüştürme dönüştürebilirsiniz. Bu akışın bir kopyasını oluşturmak için yeni bir dalı kullanın. Ayrı bir daldaki kopyalanan bu verileri dönüştürerek, ayrı bir konuma verilerin daha sonra havuz.

> [!NOTE]
> "Yeni dal" yalnızca gösterir bir eylem olarak üzerinde + burada çalıştığınız dalı geçerli konumu aşağıdaki sonraki dönüştürme olduğunda dönüştürme menüsü. başka bir dönüştürme seçin sonra eklediğiniz başka bir deyişle, burada sonunda bir "Dalı" seçeneği göremez

![Dal](media/data-flow/branch2.png "dal 2")
