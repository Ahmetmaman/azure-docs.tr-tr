---
title: Yeni bir uygulama oluşturma
titleSuffix: Language Understanding - Azure Cognitive Services
description: Oluşturun ve Language Understanding (LUIS) sayfasındaki yönetin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: 0acb844b8b10ff210985dfcd8c96f15da6669356
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58337485"
---
# <a name="create-a-new-luis-app-in-the-luis-portal"></a>LUIS portalda yeni bir LUIS uygulaması oluşturma
Çeşitli şekillerde LUIS uygulaması oluşturmak için vardır. Bir LUIS uygulaması oluşturabileceğiniz [LUIS](https://www.luis.ai) portal ya da yazma LUIS aracılığıyla [API'leri](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f).

## <a name="using-the-luis-portal"></a>LUIS portalı kullanma
Çeşitli şekillerde LUIS portalında yeni bir uygulama oluşturabilirsiniz:

* Boş bir uygulama ile başlayın ve hedefleri, konuşma ve varlıklar oluşturun.
* Boş bir uygulamayla başlayıp bir [önceden oluşturulmuş etki alanı](luis-how-to-use-prebuilt-domains.md).
* Bir LUIS uygulaması zaten hedefleri ve konuşma varlıklarını içeren bir JSON dosyasından içeri aktarın.

## <a name="using-the-authoring-apis"></a>Yazma API'leri kullanma
Çeşitli şekillerde yazma API'leri ile yeni bir uygulama oluşturabilirsiniz:

* [Başlangıç](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) boş bir uygulama ile ve hedefleri, konuşma ve varlıklar oluşturun.
* [Başlangıç](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/59104e515aca2f0b48c76be5) önceden oluşturulmuş bir etki alanına sahip.  


<a name="export-app"></a>
<a name="import-new-app"></a>
<a name="delete-app"></a>
 

## <a name="create-new-app-in-luis"></a>LUIS yeni uygulama oluştur

1. Üzerinde **uygulamalarım** sayfasında **yeni uygulama oluştur**.

    ![LUIS uygulamalarının listesi](./media/luis-create-new-app/apps-list.png)


2. İletişim kutusunda, "TravelAgent" uygulamanızı adlandırın.

    ![Yeni uygulama iletişim kutusu oluşturma](./media/luis-create-new-app/create-app.png)

3. Uygulama kültürü seçin (TravelAgent bir uygulama için İngilizce seçin) ve ardından **Bitti**. 

    > [!NOTE]
    > Uygulama oluşturduktan sonra kültür değiştirilemez. 

## <a name="import-an-app-from-file"></a>Uygulama, dosyadan içeri aktar

1. Üzerinde **uygulamalarım** sayfasında **alma yeni uygulama**.
1. Açılan iletişim kutusunda, geçerli uygulama JSON dosyasını seçin ve ardından **Bitti**.

### <a name="import-errors"></a>İçeri Aktarma hataları

Olası hatalar şunlardır: 

* Bu ada sahip bir uygulama zaten var. Uygulamayı yeniden içeri aktarın ve ayarlama **isteğe bağlı adı** yeni bir ad. 

## <a name="next-steps"></a>Sonraki adımlar

Uygulamadaki ilk göreviniz [hedef ekleme](luis-how-to-add-intents.md).
