---
title: IOT Central Bağlayıcısı Azure Logic apps'te iş akışları oluşturmak | Microsoft Docs
description: IOT Central Bağlayıcısı'nı Azure Logic Apps'te tetikleyici iş akışlarına ve oluşturmak, güncelleştirmek ve iş akışlarında cihazları silme.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 1/3/2019
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: 971c01ec8002e73dbc6abfb66a0ede26f90a4d9c
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54453322"
---
# <a name="build-workflows-with-the-iot-central-connector-in-azure-logic-apps"></a>IOT Central Bağlayıcısı Azure Logic apps'te iş akışları oluşturun

*Bu konu, Oluşturucular ve Yöneticiler için geçerlidir.*

Azure Logic Apps, cloud services ve şirket içi sistemler arasında uygulama ve verileri tümleştirin otomatik ölçeklenebilir iş akışlarını oluşturmak için kullanın. Azure Logic Apps, çoğu Azure hizmeti, Microsoft Hizmetleri ve diğer hizmet olarak yazılım-A-(SaaS) ürünleri arasında çok sayıda bağlayıcı vardır. Azure Logic Apps'te IOT Central Bağlayıcısı'nı kullanarak IOT Central içinde bir kuralı tetiklendiğinde iş akışları tetikleyebilirsiniz. Bir cihaz oluşturma, bir cihazın özellikleri ve ayarları güncelleştirin veya bir cihazı silmek için IOT Central Bağlayıcısı eylemleri de kullanabilirsiniz. 

Microsoft Flow IOT Central Bağlayıcısı'nı kullanabilirsiniz. Hem Azure Logic Apps ve Microsoft Flow Integration tasarımcıya öncelik veren, ancak biraz farklı hedef kitlelere hizmetleridir. Flow, ihtiyaç duydukları iş akışlarını oluşturmak için türlü ofis çalışanını güçlendirir. Mantıksal uygulamalar, BT uzmanları, veri bağlanmak için ihtiyaç duydukları tümleştirmeler oluşturmak için güç kazandırır. Flow ve Logic Apps karşılaştırması [burada](https://docs.microsoft.com/azure/azure-functions/functions-compare-logic-apps-ms-flow-webjobs). IOT Central Bağlayıcısı Microsoft Flow ile iş akışları oluşturma hakkında bilgi edinin [burada](howto-add-microsoft-flow.md). 

## <a name="prerequisites"></a>Önkoşullar

- Bir Kullandıkça Öde uygulama
- Bir Azure hesabı ve aboneliği oluşturmak ve mantıksal uygulamaları yönetme

## <a name="trigger-a-workflow-when-a-rule-is-triggered"></a>Bir kuralı tetiklendiğinde bir iş akışı tetikleyicisi

Bu bölümde bir kuralı tetiklendiğinde Microsoft Teams için bir ileti göndermek nasıl gösterir. Akışınıza bir olay, olay hub'ına gönderme gibi şeyler, yeni Azure DevOps iş öğesi oluşturma veya SQL Server'da yeni bir satır eklemek için diğer bağlayıcıları kullanmak için yapılandırabilirsiniz.

1. Başlayın [IOT Central içinde bir kural oluşturma](howto-create-telemetry-rules.md). Kural koşulları kaydettikten sonra tıklayın **Azure Logic Apps** döşeme yeni bir eylem. Tıklayın **Azure portalında oluşturma**. Yeni bir mantıksal uygulama oluşturmak için Azure portalına ulaşabilirsiniz. Azure hesabınızda oturum açmanız gerekebilir.

1. Yeni bir mantıksal uygulama oluşturmak için gerekli bilgileri girin. Yeni mantıksal uygulamanızı sağlamak için bir Azure aboneliği seçebilirsiniz. IOT Central uygulamanızı oluşturulduğu aynı abonelik yok. **Oluştur**’a tıklayın.

    ![Azure portalında mantıksal uygulama oluşturma](./media/howto-build-azure-logic-apps/createinazureportal.PNG)

1. Mantıksal uygulamanızı başarıyla oluşturulduktan sonra Logic Apps Tasarımcısı için otomatik olarak yönlendirilir. Tıklayın **boş mantıksal uygulama**. 

    ![Boş mantıksal uygulama oluşturma](./media/howto-build-azure-logic-apps/blanklogicapp.PNG)

1. Tasarımcıda "IOT için merkezi" araması yapın ve seçin **bir kuralı ne zaman tetiklenir** tetikleyici. Bağlayıcı ile IOT Central uygulamanıza hesabı ile oturum açın. 

    ![IOT Central Connector'a oturum](./media/howto-build-azure-logic-apps/addtrigger.PNG)

1. Başarılı oturum açma işleminden sonra görünen alanları görmeniz gerekir. Seçin **uygulama** ve **kural** pencerelerden.

    ![Uygulama ve kural Seç](./media/howto-build-azure-logic-apps/pickappandrule.PNG)

1. Yeni bir eylem ekleyin. Arama **teams'e ileti gönderin** ve **kanalında bir ileti** Microsoft Teams Bağlayıcıdan. Bağlayıcı Microsoft Teams içinde kullandığınız hesap ile oturum açın. 

1. Eylem seçin **takım** ve **kanal**. Doldurun **ileti** her ileti söylemek istediğiniz alanı. Ekleyebileceğiniz *dinamik içerik* , IOT Central kuraldan boyunca cihaz adı ve zaman damgası gibi önemli bilgiler için bildirim geçirme.
    > [!NOTE]
    > Ölçüm ve kuralını tetikleyen özellik değerlerini almak için dinamik içerik penceresi "daha fazla" metni'ı tıklatın.

    ![Mantıksal uygulama düzenleme eylemi dinamik bölmesini açın](./media/howto-build-azure-logic-apps/buildworkflow.PNG)

1. İşiniz bittiğinde eyleminizi düzenleme, tıklayın **Kaydet**.

1. IOT Central uygulamanıza geri dönün, bu kural, bir Azure Logic Apps eylem eylemleri alanında içeriyor. görürsünüz.

Her zaman, Logic Apps Azure Portal'da IOT Central Bağlayıcısı'nı kullanarak bir iş akışı oluşturmaya başlayabilirsiniz. Ardından hangi IOT Central uygulaması ve bağlanmak için hangi kural seçebilir ve yine de aynı şekilde çalışır. IOT Central kuralları sayfasından her başlattığınızda gerek yoktur.

## <a name="create-update-and-delete-a-device-in-a-workflow"></a>Oluşturma, güncelleştirme ve bir iş akışında bir cihazı silme

Bir iş akışı mantıksal uygulamanızı oluştururken, IOT Central Bağlayıcısı'nı kullanarak bir eylem ekleyebilirsiniz. Size **bir cihaz oluşturma**, **bir cihaz güncelleştirmesi**, ve **bir cihazı silmek**.
> [!NOTE]
> İçin **bir cihaz güncelleştirmesi** ve **bir cihazı silmek**, güncelleştirmek veya silmek istediğiniz var olan cihazın Kimliğine ihtiyacınız olacak. IOT Central cihaz Kimliğini tarayıcı URL'sini alabilirsiniz. Bu **değil** cihaz kimliği Gezgin sayfasında listelenen aynı.

![IOT Central cihaz Gezgini cihaz kimliği](./media/howto-build-azure-logic-apps/iotcdeviceid.PNG)
  

## <a name="next-steps"></a>Sonraki adımlar
İş akışlarını oluşturmak için Microsoft Flow kullanmayı öğrendiniz, önerilen sonraki adım olarak [cihazları yönetme](howto-manage-devices.md).
