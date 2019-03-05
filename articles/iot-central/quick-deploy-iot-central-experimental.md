---
title: Azure IoT Central uygulaması oluşturma | Microsoft Docs
description: Yeni bir Azure IoT Central uygulaması oluşturun. Uygulama şablonu kullanarak bir Deneme veya Kullandıkça Öde uygulaması oluşturun.
author: viv-liu
ms.author: viviali
ms.date: 02/20/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: ae5eedf898b2dd9c766b535c6171b6f2d62210f4
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2019
ms.locfileid: "57306927"
---
# <a name="create-an-azure-iot-central-application"></a>Azure IoT Central uygulaması oluşturma

_Oluşturucu_ olarak, Azure IoT Central kullanıcı arabirimini kullanarak Microsoft Azure IoT Central uygulamanızı tanımlayabilirsiniz. Bu hızlı başlangıç örnek bir _cihaz şablonu_ ve simülasyon _cihazları_ içeren bir Azure IoT Central uygulaması oluşturma adımlarını göstermektedir.

Azure IoT Central [Uygulama Yöneticisi](https://aka.ms/iotcentral) sayfasına gidin. Microsoft kişisel ya da iş veya okul hesabıyla oturum açmalısınız.

Yeni bir Azure IoT Central uygulaması oluşturmaya başlamak için **Yeni Uygulama**'yı seçin. Bu sizi **Uygulama Oluştur** sayfasına götürür.

![Azure IoT Central Uygulama Oluştur sayfası](media/quick-deploy-iot-central-experimental/iotcentralcreate.png)

Yeni bir Azure IoT Central uygulaması oluşturmak için:

1. Ödeme planını seçin:
    - **Deneme** uygulamaları, süresi dolmadan 7 gün ücretsiz kullanılabilir. Süresi dolmadan önce herhangi bir anda Kullandıkça Öde uygulamasına dönüştürülebilir.
    - **Kullandıkça Öde** uygulamaları, ilk 5 cihaz ücretsiz olmak üzere cihaz başına ücretlendirilir.

    [Azure IoT Central fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/iot-central/), fiyatlar hakkında daha fazla bilgi edinin.

1. **Contoso IoT** gibi kolay bir uygulama adı seçin. Azure IoT Central sizin için benzersiz bir URL ön eki oluşturur. Bu URL ön ekini daha akılda kalır bir şeyle değiştirebilirsiniz.

1. Uygulama şablonunu seçin. Uygulama şablonu, başlamanıza yardımcı olacak cihaz şablonları ve panolar gibi önceden tanımlanmış öğeleri içerebilir.

    | Uygulama şablonu | Açıklama |
    | -------------------- | ----------- |
    | Contoso Örneği       | Bir Soğutmalı Otomat için önceden oluşturulmuş cihaz şablonunu içeren bir uygulama oluşturur. Azure IoT Central'ı incelemeye başlamak için bu şablonu kullanın. |
    | Devkits Örneği       | MXChip veya Raspberry Pi cihazını bağlamak amacıyla sizin için hazırlanmış cihaz şablonlarıyla bir uygulama oluşturur. Bu cihazlardan birini deneyen bir cihaz gelişticisiyseniz bu şablonu kullanın. |
    | Özel uygulama   | Kendi cihaz şablonlarınız ve cihazlarınızla doldurabileceğiniz boş bir uygulama oluşturur. |

1. **Kullandıkça Öde** uygulaması oluşturuyorsanız, *Dizin*, *Azure Aboneliği* ve *Bölge* bilgilerinizi seçmeniz gerekir. 

    - *Dizin* Azure Active Directory (uygulamanızı oluşturmak için ad). Kullanıcı kimliklerini, kimlik bilgilerini ve diğer kuruluş bilgilerini içerir. Azure AD yoksa, bir Azure aboneliği oluşturduğunuzda bir sizin için oluşturulur.

    - *Azure Aboneliği*, Azure hizmetlerinin örneklerini oluşturmanıza olanak tanır. IoT Central aboneliğinizde kaynakları sağlayacaktır. Azure aboneliğiniz yoksa, [Azure kayıt sayfasında](https://aka.ms/createazuresubscription) bir abonelik oluşturabilirsiniz. Azure aboneliğini oluşturduktan sonra, **Uygulama Oluştur** sayfasına geri gidin. Yeni aboneliğiniz **Azure Aboneliği** açılan listesinde gösterilir.

    - *Bölge*, uygulamanızı oluşturmak istediğiniz fiziksel konumdur. Normalde, en uygun performansı elde etmek için cihazlarınıza en yakın fiziksel konumu seçmeniz gerekir. Azure IoT Central'ın sağlandığı bölgeleri, [Bölgeye göre sağlanan ürünler](https://azure.microsoft.com/regions/services/) sayfasında görebilirsiniz.

    > [!Note]
    > Bölgeyi bir kez seçtiğinizde, uygulamanızı daha sonra başka bir bölgeye taşıyamazsınız.

1. **Oluştur**’u seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir IoT Central uygulaması oluşturdunuz. Önerilen sonraki adım:

> [!div class="nextstepaction"]
> [IoT Central'da bir tura katılın](https://docs.microsoft.com/azure/iot-central/overview-iot-central-tour)
