---
title: Varlık algılamayı geliştirmek için deyim listeler
titleSuffix: Azure Cognitive Services
description: Bu kategori ve desenleri algılama veya hedefleri ve varlıkların tahmin iyileştirebilir uygulama özelliklerini eklemek için Language Understanding (LUIS) kullanın
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: feb51cd55801addaf5ce2486e5527542f794bbc5
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52580970"
---
# <a name="use-phrase-lists-to-boost-signal-of-word-list"></a>Kullanım deyimi word listesinin boost sinyale listeler

LUIS uygulamanızı kendi doğruluğunu artırmak için özellikler ekleyebilirsiniz. Bu belirli kelimeleri ipuçları sağlayarak Yardım LUIS özellikleri ve ifadeleri bir uygulama etki alanı sözlüğü bir parçasıdır. 

A [tümcecik listesi](luis-concept-feature.md) aynı sınıfa ait benzer şekilde (örneğin, şehirler ya da ürün adlarını) işlenmesi gereken değerleri (sözcük ve tümcecikleri) bir grup içerir. LUIS bunları biri hakkında ne öğrenir otomatik olarak başkaları için de uygulanır. Bu liste bir kapalı listesi varlık (tam metinle eşleşen) eşleşen bir kelimelerin değildir.

Bir ifade listesi uygulama etki alanının sözlüğü LUIS için ikinci bir sinyal sözcükleri ilgili olarak ekler.

## <a name="add-phrase-list"></a>İfade listesi ekleme

1. Adını tıklayarak uygulamanızı açın **uygulamalarım** sayfasında ve ardından **derleme**, ardından **tümcecik listeleri** uygulamanızın sol panelde. 

2. Üzerinde **tümcecik listeleri** sayfasında **oluştur yeni ifade listesi**. 
 
3. İçinde **tümcecik listesine Ekle** iletişim kutusunda, ifade listesi adı olarak "Şehir" yazın. İçinde **değer** tümcecik listesi değerlerini yazın. Bir saat veya virgülle ayrılmış değerler kümesi tek bir değer yazın ve sonra basın **Enter**.

    ![İfade listesi şehirler Ekle](./media/luis-add-features/add-phrase-list-cities.png)

4. LUIS, tümcecik listenize eklemek için ilgili değerleri önerebilirsiniz. Tıklayın **önerilir** bir grup için added value(s) anlamsal olarak ilişkili önerilen değerleri almak için. Önerilen değerlerden herhangi birini tıklatın veya tıklatın **Ekle** bunları eklemek için tüm.

    ![Önerilen değerler ifade listesi](./media/luis-add-features/related-values.png)

5. Tıklayın **birbirinin yerine bu değerleri** birbirlerinin yerine kullanılabilir seçenekleri eklendi ifade listesi değerler.

    ![Önerilen değerler ifade listesi](./media/luis-add-features/interchangeable.png)

6. **Kaydet**’e tıklayın. "Şehir" tümceciği listeye eklenen **tümcecik listeleri** sayfası.

<a name="edit-phrase-list"></a>
<a name="delete-phrase-list"></a>
<a name="deactivate-phrase-list"></a>

> [!Note]
> Silme veya üzerinde bağlamsal araç tümcecik listeden devre dışı bırakmak **tümcecik listeleri** sayfası.

## <a name="next-steps"></a>Sonraki adımlar

Sonra ekleme, düzenleme, silme veya bir ifade listesi devre dışı bırakma [eğitme ve uygulamayı test etme](luis-interactive-test.md) performansı artıran yeniden görmek için.
