---
title: Tasarımcıda erişilebilirlik özelliklerini kullanma
titleSuffix: Azure Machine Learning
description: Tasarımcıda bulunan klavye kısayolları ve ekran okuyucusu erişilebilirlik özellikleri hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.author: peterlu
author: peterclu
ms.date: 01/09/2020
ms.custom: designer
ms.openlocfilehash: 86cb5260a59f864658fbb7ac1c1da2d943c6253e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90893423"
---
# <a name="use-a-keyboard-to-use-azure-machine-learning-designer"></a>Azure Machine Learning tasarımcısını kullanmak için klavye kullanma

Azure Machine Learning tasarımcısını kullanmak için klavye ve ekran okuyucu kullanmayı öğrenin. Azure portal her yerde çalışan klavye kısayollarının bir listesi için, bkz [. Azure Portal klavye kısayolları](../azure-portal/azure-portal-keyboard-shortcuts.md)

Bu iş akışı, [ekran okuyucusu](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator) ve [Jaw](https://www.freedomscientific.com/products/software/jaws/)ile test edilmiştir, ancak diğer standart ekran okuyucularıyla birlikte çalışmalıdır.

## <a name="navigate-the-pipeline-graph"></a>İşlem hattı grafiğinde gezinme

Ardışık düzen grafiği, iç içe geçmiş bir liste olarak düzenlenir. Dış liste, ardışık düzen grafiğindeki tüm modülleri açıklayan bir modül listesidir. İç liste, belirli bir modülün tüm bağlantılarını açıklayan bir bağlantı listesidir.  

1. Modül listesinde, modülleri değiştirmek için ok tuşunu kullanın.
1. Hedef modülün bağlantı listesini açmak için Tab kullanın.
1. Modülün bağlantı bağlantı noktaları arasında geçiş yapmak için ok tuşunu kullanın.
1. Hedef modüle gitmek için "G" kullanın.

## <a name="edit-the-pipeline-graph"></a>Ardışık düzen grafiğini düzenleme

### <a name="add-a-module-to-the-graph"></a>Grafiğe modül ekleme

1. Odağı tuvalden modül ağacına geçirmek için CTRL + F6 tuşlarını kullanın.
1. Standart TreeView denetimini kullanarak modül ağacında istenen modülü bulun.

### <a name="edit-a-module"></a>Modül düzenleme

Bir modülü başka bir modüle bağlamak için:

1. Modül listesinde bir modülü hedeflerken CTRL + SHIFT + H tuşlarını kullanarak bağlantı yardımcısını açın.
1. Modülün bağlantı bağlantı noktalarını düzenleyin.

Modül özelliklerini ayarlamak için:

1. Modül özelliklerini açmak için bir modülü hedeflerken CTRL + SHIFT + E tuşlarını kullanın.
1. Modül özelliklerini düzenleyin.

## <a name="navigation-shortcuts"></a>Gezinti kısayolları

| U | Açıklama |
|-|-|
| Ctrl + F6 | Tuval ve modül ağacı arasında odağı değiştirme |
| Ctrl + F1   | Modül ağacındaki bir düğüme odaklanan bilgi kartını açın |
| CTRL + SHIFT + H | Odak bir düğüm üzerinde olduğunda bağlantı yardımcısını açın |
| CTRL + SHIFT + E | Odak bir düğüm üzerinde olduğunda modül özelliklerini aç |
| CTRL + G | İşlem hattı çalıştırması başarısız olursa odağı ilk başarısız düğüme taşı |

## <a name="action-shortcuts"></a>Eylem kısayolları

Erişim anahtarıyla aşağıdaki kısayolları kullanın. Erişim anahtarları hakkında daha fazla bilgi için bkz https://en.wikipedia.org/wiki/Access_key ..

| U | Eylem |
|-|-|
| Erişim tuşu + R | Çalıştır |
| Erişim tuşu + P | Yayımla |
| Erişim tuşu + C | Kopyalama |
| Erişim tuşu + D | Dağıtma |
| Erişim tuşu + ı | Çıkarım ardışık düzeni oluştur/güncelleştir |
| Erişim tuşu + B | Batch çıkarım ardışık düzeni oluştur/güncelleştir |
| Erişim tuşu + K | "Çıkarım işlem hattı oluştur" açılan menüsünü aç |
| Erişim tuşu + U | "Güncelleştirme çıkarımı ardışık düzeni" açılan menüsünü açın |
| Erişim tuşu + a | Daha fazla (...) açılan listesini aç |

## <a name="next-steps"></a>Sonraki adımlar

- [Yüksek karşıtlığı açma veya tema değiştirme](../azure-portal/set-preferences.md#choose-a-theme-or-enable-high-contrast)
- [Microsoft 'ta erişilebilirlik ile ilgili araçlar](https://www.microsoft.com/accessibility)
