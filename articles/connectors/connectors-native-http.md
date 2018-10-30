---
title: Azure Logic Apps ile herhangi bir HTTP uç noktasına bağlanma | Microsoft Docs
description: Görevler ve herhangi bir HTTP uç noktası ile iletişim kuran Azure Logic Apps kullanarak iş akışlarını otomatikleştirin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: e11c6b4d-65a5-4d2d-8e13-38150db09c0b
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: 1c30f77c061ec25c88186caee3f60e65f3afb3de
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50232877"
---
# <a name="call-http-or-https-endpoints-with-azure-logic-apps"></a>Azure Logic Apps ile HTTP veya HTTPS uç noktalarına çağrı

Azure Logic Apps ve Köprü Metni Aktarım Protokolü (HTTP) Bağlayıcısı ile mantıksal uygulamalar oluşturarak herhangi bir HTTP veya HTTPS uç noktasıyla iletişim iş akışlarını otomatik hale getirebilirsiniz. Örneğin, Web siteniz için hizmet uç noktası izleyebilirsiniz. Aşağı doğru giden, Web sitesi gibi bu uç noktada bir olay meydana geldiğinde olay mantıksal uygulamanızın iş akışı tetikler ve belirtilen eylemleri çalıştırır. 

HTTP tetikleyicisi ilk adım olarak, iş akışı içinde denetlemek için kullanabileceğiniz veya *yoklama* düzenli bir uç nokta. Her denetimi, bir çağrı tetikleyici gönderir veya *isteği* uç noktası. Uç noktanın yanıt, mantıksal uygulamanızın iş akışı çalıştırır olup olmadığını belirler. Tetikleyici, mantıksal uygulama eylemleri yanıta herhangi bir içerik geçirir. 

HTTP eylem istediğiniz zaman uç noktasını çağırmak için akışınızda herhangi bir adım olarak kullanabilirsiniz. Uç noktanın yanıt kalan iş akışının eylemlerini çalışma şeklini belirler.

Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* Aramak istediğiniz hedef uç nokta URL'si 

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* HTTP tetikleyicisi ile başlatmak için hedef uç noktasını çağırmak istediğiniz mantıksal uygulama [boş mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). HTTP eylem kullanmak için mantıksal uygulamanızın bir tetikleyici ile başlayın.

## <a name="add-http-trigger"></a>HTTP tetikleyicisi Ekle

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda boş mantıksal uygulamanızı açın, açık değilse.

1. Arama kutusuna filtreniz olarak "http" girin. Tetikleyiciler listesinde seçin **HTTP** tetikleyici. 

   ![HTTP tetikleyicisini seçin](./media/connectors-native-http/select-http-trigger.png)

1. Sağlamak [HTTP tetikleyici parametrelerini ve değerlerini](../logic-apps/logic-apps-workflow-actions-triggers.md##http-trigger) hedef uç noktasına çağrıda dahil etmek istediğiniz. Hedef uç noktasını denetlemek için yineleme tetikleyicisi istediğiniz sıklığı için ayarlayın.

   ![HTTP tetikleyicisi parametreleri girin](./media/connectors-native-http/http-trigger-parameters.png)

   HTTP tetikleyicisi, parametreler ve değerler hakkında daha fazla bilgi için bkz: [tetikleyici ve eylem türleri başvurusu](../logic-apps/logic-apps-workflow-actions-triggers.md##http-trigger).

1. Mantıksal uygulamanızın Tetikleyici etkinleştirildiğinde çalıştırılan eylemleri iş akışı oluşturmaya devam edin.

## <a name="add-http-action"></a>HTTP Eylem Ekle

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. HTTP eylem eklemek istediğiniz son adımı altında seçin **yeni adım**. 

   Bu örnekte, mantıksal uygulama ilk adım olarak HTTP tetikleyicisi ile başlatılır.

1. Arama kutusuna filtreniz olarak "http" girin. Eylemler listesinde seçin **HTTP** eylem.

   ![HTTP eylemi seçin](./media/connectors-native-http/select-http-action.png)

   Adımlar arasında bir eylem eklemek için işaretçinizi adımlar arasındaki okun üzerine getirin. 
   Artı işaretini seçin (**+**), görünür ve ardından **Eylem Ekle**.

1. Sağlamak [HTTP eylem parametrelerini ve değerlerini](../logic-apps/logic-apps-workflow-actions-triggers.md##http-action) hedef uç noktasına çağrıda dahil etmek istediğiniz. 

   ![HTTP eylem parametrelerini girin](./media/connectors-native-http/http-action-parameters.png)

1. İşiniz bittiğinde mantıksal uygulamanızı kaydettiğinizden emin olun. Tasarımcı araç çubuğunda **Kaydet**'i seçin. 

## <a name="authentication"></a>Kimlik Doğrulaması

Kimlik doğrulaması koymak için **Gelişmiş Seçenekleri Göster** eylem veya tetikleyici içinde. HTTP Tetikleyicileri ve eylemleri için kullanılabilir kimlik doğrulama türleri hakkında daha fazla bilgi için bkz. [tetikleyici ve eylem türleri başvurusu](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
