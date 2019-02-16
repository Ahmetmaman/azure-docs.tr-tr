---
title: Azure veri fabrikası veri akışı koşullu bölme dönüştürme eşlemesi
description: Azure Data Factory veri akışı koşullu bölme dönüştürme
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 9fe650465160ab837d8c8c191887d0f952976682
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56272146"
---
# <a name="azure-data-factory-mapping-data-flow-conditional-split-transformation"></a>Azure veri fabrikası veri akışı koşullu bölme dönüştürme eşlemesi

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Koşullu bölünmüş dönüşümü, veri içeriğini bağlı olarak farklı akışlara veri satırları yönlendirebilirsiniz. Koşullu bölünmüş dönüşümü uygulaması, programlama dilinde büyük/küçük harf karar yapısına benzer. Dönüştürme ifadeleri değerlendirir ve sonuçlarına göre belirtilen akış veri satırına yönlendirir. Herhangi bir ifade bir satır eşleşiyorsa varsayılan çıkışa yönlendirilir, bu dönüşümü bir varsayılan çıkış de sağlar.

![Koşullu bölünmüş](media/data-flow/cd1.png "koşullu Böl")

Ek koşullar eklemek için "Ekle Stream" alt yapılandırma bölmesinde seçin ve ifadeniz derlemek için ifade oluşturucu metin kutusuna tıklayın.
