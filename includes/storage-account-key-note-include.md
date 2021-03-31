---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/06/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: ba715d510dc296ffa8f9c0ee58841f284416a118
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "86027349"
---
## <a name="protect-your-access-keys"></a>Erişim anahtarlarınızı koruyun

Depolama hesabı erişim anahtarlarınız, depolama hesabınız için bir kök parolaya benzer. Erişim anahtarlarınızı korumak için her zaman dikkatli olun. Anahtarlarınızı güvenli bir şekilde yönetmek ve döndürmek için Azure Key Vault kullanın. Erişim anahtarlarını diğer kullanıcılara dağıtmaktan, sabit kodlamadan veya başkalarının erişebileceği düz metin olarak herhangi bir yere kaydetmekten kaçının. Anahtarların tehlikede olduğunu düşünüyorsanız, anahtarlarınızı döndürün.

> [!NOTE]
> Microsoft, paylaşılan anahtar yerine mümkünse blob ve sıra verilerinde istekleri yetkilendirmek için Azure Active Directory (Azure AD) kullanılmasını önerir. Azure AD, paylaşılan anahtar üzerinde üstün güvenlik ve kullanım kolaylığı sağlar. Azure AD ile verilere erişimi yetkilendirme hakkında daha fazla bilgi için bkz. [Azure Active Directory kullanarak Azure bloblarına ve kuyruklara erişim yetkisi verme](../articles/storage/common/storage-auth-aad.md).
