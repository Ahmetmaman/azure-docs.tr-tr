---
title: Konuşma Öğrenici sahip bir Model - Microsoft Bilişsel hizmetler sürümü etiketleme kullanma | Microsoft Docs
titleSuffix: Azure
description: Sürüm oluşturma ve konuşma Öğrenici modeliyle etiketleme kullanmayı öğrenin.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: d596c4c614119caa4078df1ed569e685031e9c7c
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/27/2018
ms.locfileid: "53796849"
---
# <a name="how-to-use-version-tagging"></a>Sürüm etiketleme kullanma

Bu öğreticide, hangi sürümün "canlı" konuşma Öğrenici modelinizi sürümleri etiketleme ve ayarlamak nasıl gösterir.  

## <a name="requirements"></a>Gereksinimler
Bu öğretici, günlük iletişim kutuları, olmayan günlük iletişim Web kullanıcı arabirimini oluşturmak için Bot Framework öykünücüyü kullanarak gerektirir.  

Bu öğreticide, genel öğretici Bot çalışıyor olması gerekir:

    npm run tutorial-general

## <a name="details"></a>Ayrıntılar

Model etiketlenmiş sürümlerini statik; düzenleyemez veya bunları değiştirme. Modelinizi düzenlerken, her zaman ana düzenlediğiniz sürümü. Yeni bir etiket eklediğinizde, konuşma Öğrenici zamandaki o noktada Model anlık görüntüsünü yakalar. 

Botunuzun "Canlı" bir sürüm olarak seçtiğiniz Model sürümünü kullanır, ancak yalnızca "Etiket düzenleme" "Ana" olarak ayarlandığında, sahip görüşmeler görüntülenebilir durumda olmayacaktır. Dışında bir "Ana" modelin "Etiket düzenleme" özelliği ayarlarsanız, sonra anlık görüntüyü modelinin görüntüleyebilirsiniz, ancak herhangi bir şekilde değiştiremezsiniz.

## <a name="steps"></a>Adımlar

### <a name="install-the-bot-framework-emulator"></a>Bot Framework öykünücüsü'nü yükleyin

1. [https://github.com/Microsoft/BotFramework-Emulator](https://github.com/Microsoft/BotFramework-Emulator) kısmına gidin.
2. İndirin ve öykünücüyü yükleyin.

### <a name="create-a-model"></a>Model oluşturma

1. Model listesi giriş sayfasından tıklayın `New Model` düğmesi.
2. İçinde `Name` alan türü, "Öğreticisi-18-Versioning" ENTER tuşuna basın.
4. Sol panelde, "Ayarlar"'ı tıklatın.
5. CONVERSATION_LEARNER_MODEL_ID alanın içeriğini panoya kopyalayın.

### <a name="configure-the-emulator"></a>Öykünücü yapılandırın

1. Konuşma Öğrenici Kök klasörde, ".env" dosyasını açın.
2. ".Env" dosyasında şöyle bir satır ekleyin:
    - `CONVERSATION_LEARNER_MODEL_ID=[paste-model-id-from-clipboard-here]`
3. Komut İstemi'nden çıkmadan ve yeniden çalıştırılması, konuşma Öğrenici hizmeti yeniden başlatın:
    - `npm run tutorial-general`
4. Bot Framework öykünücüsü, yeni bir bot yapılandırması oluşturun, uç nokta URL'sini ayarlayın `http://localhost:3978/api/messages`

### <a name="version-1"></a>Sürüm 1

Sürüm 1 için tek bir eylem oluşturacağız.

1. Web kullanıcı arabirimini sol bölmede bulunan "Eylemler" öğesine tıklayın `New Action` düğmesi.
2. "Botun yanıt" alanına, "Merhaba vardır (sürüm 1)".
3. `Save` düğmesine tıklayın.

Şimdi Biz bu "Sürüm" modelin 1 etiketlemeniz.

4. Sol bölmede 'ayarları "'nı tıklatın ve ardından tıklayın ![](../media/tutorial18_version_tags.PNG)açığa çıkarmak için"Sürüm etiketleri"simgesi `New Tag` düğmesine tıklamamalıdır tıklatmanız gerekir.
    - "Sürüm 1" olarak adlandırın
4. İçinde "Live etiket" seçin, açılan "sürüm 1".  
    - Artık bu Botunu kullanarak kanalları "1. sürümünü" Modelimizi kullanacaktır.
    - Varlıklar, Eylemler ve eğitme iletişim kutuları bu sürüm 1 modelin artık değiştirilebilir.
    - "Etiket düzenleme" olarak "sürümü 1" seçerseniz yalnızca Model görüntüleme ve düzenleme olmadan mümkün olacaktır.
    - "Ana" düzenleme izni "etiket" ayarlayın, düzenlenebilir model tek sürümü.

Şimdi, "Sürüm 1" "Sürüm etiketleri" kılavuzunda görürsünüz.

### <a name="version-2"></a>Sürüm 2

Şimdi biz Modelimizi sürüm 1'den ayırmak için düzenleyeceksiniz.

1. "Eylemler" Sol paneldeki tıklayın.
2. Eylemler kılavuz tıklayın "Merhaba vardır (sürüm 1)".
3. Değişiklik "Botun yanıt" alanı "Merhaba vardır (sürüm 2)".
4. `Save` düğmesine tıklayın.
5. `New Action` düğmesine tıklayın.
6. Türü, "bye bye (sürüm 2)" içinde "Botun yanıt" alan.

### <a name="confirm-bot-framework-emulator-is-using-version-1"></a>Bot Framework öykünücüsü'nü kullanarak sürüm 1 olduğunu doğrulayın

1. Bot Framework Öykünücüde iletide, "Hey var." yazın.
2. Bot ile yanıt veren bildirimi "Merhaba vardır (sürüm 1)".
    - Bu sürüm 1 "canlı" olduğunu doğrular.

### <a name="view-the-conversation-logs-in-conversation-learner-web-ui"></a>Konuşma Öğrenici Web kullanıcı Arabiriminde konuşma günlüklerini görüntüleme

1. "Günlük iletişim kutuları" üzerinde Sol paneldeki tıklatın
    - İletişim kutularını görmüyorsanız, yenile düğmesine tıklayın.
2. Kılavuzundaki "Sürüm 1" etiketine dikkat edin.
3. Kılavuzda tıklayın "Merhaba vardır (sürüm 1)"

> [!NOTE]
> Düzeltmeleri tüm şu anda kullanılabilir konuşma Öğrenici işlevinden seçerek yapabiliyoruz, bu düzenlemeler sürüm 1 değil de, ana ancak yapılacaktır.

Artık, sürüm oluşturma nasıl çalıştığını ve Bot Framework öykünücüsü'nü Bot ile nasıl etkileşim kurabileceğine gördünüz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Demo - parola sıfırlama](./demo-password-reset.md)
