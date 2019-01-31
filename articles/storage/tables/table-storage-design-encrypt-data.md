---
title: Azure depolama tablo verilerini şifrelemek | Microsoft Docs
description: Azure depolama tablosu veri şifreleme hakkında bilgi edinin.
services: storage
author: MarkMcGeeAtAquent
ms.service: storage
ms.topic: article
ms.date: 04/11/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: f56946702011968a0fcb31f6fbecbaacdc89ea42
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55470425"
---
# <a name="encrypt-table-data"></a>Tablo verilerini şifreleme
.NET Azure depolama istemci kitaplığı, yerleştirme için şifreleme dize varlık özelliklerini destekler ve işlemleri değiştirin. Şifrelenmiş dizeleri hizmette ikili özellikleri olarak depolanır ve şifre çözme sonra geri dizelere dönüştürülür.    

Şifreleme İlkesi yanı sıra, tablolar için kullanıcıların şifrelenmiş özelliklerini belirtmeniz gerekir. Bu, ya da (TableEntity türetilen POCO varlık için) bir [EncryptProperty] özniteliği ya da bir şifreleme çözümleyici istek seçenekleri belirterek yapılabilir. Bir şifreleme çözümleyici bölüm anahtarını, satır anahtarını ve özellik adını alır ve bu özellik şifrelenmesi gerekip gerekmediğini belirten bir Boole değeri döndüren bir temsilcidir. Şifreleme sırasında istemci kitaplığı, bir özellik için kablo yazılırken şifrelemek karar vermek için bu bilgileri kullanır. Temsilci özellikleri nasıl şifrelenir etrafında mantıksal olasılığı için de sağlar. (X, örneğin, daha sonra özellik A şifrelemek; Aksi takdirde özellik A ve b şifreleme) Okurken veya varlıkları sorgulayarak bu bilgiyi sağlamak gerekli değildir.

## <a name="merge-support"></a>Destek Birleştir

Birleştirme şu anda desteklenmiyor. Bir özellik alt kümesi daha önce farklı bir anahtar kullanılarak şifrelenmiş çünkü yalnızca birleştirme yeni özellikleri ve meta verilerini güncelleştirme veri kaybı ile sonuçlanır. Özellik başına yeni bir anahtar kullanarak, ikisi için de performansla ilgili nedenlerden dolayı uygun değil veya hizmetten önceden var olan bir varlığa okumak için ek hizmet çağrıları yapma ya da birleştirme gerektirir.     

Tablo verilerini şifreleme hakkında daha fazla bilgi için bkz: [istemci tarafı şifreleme ve Microsoft Azure depolama için Azure anahtar kasası](../common/storage-client-side-encryption.md).  

## <a name="next-steps"></a>Sonraki adımlar

- [Tablo Tasarım desenleri](table-storage-design-patterns.md)
- [İlişkileri modelleme](table-storage-design-modeling.md)
- [İlişkileri modelleme](table-storage-design-modeling.md)
- [Veri değişikliği için Tasarım](table-storage-design-for-modification.md)
