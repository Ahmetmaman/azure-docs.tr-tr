---
title: İçerik çeviri - Translator metin çevirisi API'si engelle
titlesuffix: Azure Cognitive Services
description: Çeviri ile içerik önlemek Translator metin çevirisi API'si.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 11/20/2018
ms.author: v-jansko
ms.openlocfilehash: b012201bcb9e0aece3d7e1ddd4f771643088af76
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52683381"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator-text-api"></a>Translator metin çevirisi API'si ile içeriğin çeviri engelleme

Böylece çevrilmiş değil Translator metin çevirisi API'si, etiketi içeriği sağlar. Örneğin, kod, marka adı veya yerelleştirilmiş doesn't make Sense bir sözcük/tümcecik etiket isteyebilirsiniz. 

## <a name="methods-for-preventing-translation"></a>Çeviri önleme yöntemleri
1. Çıkış için bir Twitter etiketi @somethingtopassthrough veya #somethingtopassthrough. Kaldırma kaçış çeviri sonra.

2. İçeriğinizi etiketlemek `notranslate`.

   Örnek:

   ```html
   <div class="notranslate">This will not be translated.</div>
   <div>This will be translated. </div>
   ```

3. Kullanım [dinamik sözlük](dynamic-dictionary.md) belirli bir çeviri olarak açıklamayı amaçlamamaktadır.

4. Translator metin çevirisi API'si çeviri için dize geçirmeyin.

5. Özel Translator: Kullanan bir [özel Translator sözlüğünde](custom-translator/what-is-dictionary.md) bir ifade çevirisi ile % 100 olasılık olarak açıklamayı amaçlamamaktadır.


## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Çeviri, Translator API çağrısında kaçının](reference/v3-0-translate.md)
