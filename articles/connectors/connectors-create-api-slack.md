---
title: Azure Logic Apps bolluk 'e bağlanma
description: Azure Logic Apps kullanarak,, dosyaları izleyen ve bolluk hesabınızdaki kanalları, grupları ve iletileri yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/25/2018
tags: connectors
ms.openlocfilehash: 75f4cea0e5cd2b0bf42c28cbd81e199115834e11
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "87283971"
---
# <a name="monitor-and-manage-slack-with-azure-logic-apps"></a>Azure Logic Apps ile bolluğu izleyin ve yönetin

Azure Logic Apps ve bolluk Bağlayıcısı sayesinde, bolluk dosyalarınızı izleyen ve bolluk kanallarınızı, iletilerinizi, gruplarınızı ve benzerlerini yönettiğiniz otomatikleştirilmiş görevler ve iş akışları oluşturabilirsiniz. Örneğin:

* Yeni dosyalar oluşturulduğunda izleyin.
* Kanalları oluşturun, listeleyin ve birleştirin 
* İleti gönderin.
* Grupları oluşturun ve rahatsız mayın olarak ayarlayın.

Bolluk hesabınızdan yanıt alan Tetikleyicileri kullanabilir ve çıktıyı diğer eylemler için kullanılabilir hale getirebilirsiniz. Bolluk hesabınızla görevleri gerçekleştiren eylemleri kullanabilirsiniz. Ayrıca, başka eylemlere de bolluk eylemlerinin çıktısını kullanabilirsiniz. Örneğin, yeni bir dosya oluşturulduğunda Office 365 Outlook Bağlayıcısı ile e-posta gönderebilirsiniz. Logic Apps 'e yeni başladıysanız [ne Azure Logic Apps](../logic-apps/logic-apps-overview.md) olduğunu gözden geçirin.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* [Bolluk](https://slack.com/) hesabınız ve Kullanıcı kimlik bilgileriniz

  Kimlik bilgileriniz, mantıksal uygulamanızı bir bağlantı oluşturmak ve bolluk hesabınıza erişmek için yetkilendirin.

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgi

* Bolluk hesabınıza erişmek istediğiniz mantıksal uygulama. Bir bolluk tetikleyicisi ile başlamak için [boş bir mantıksal uygulama oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md). Bir bolluk eylemi kullanmak için, mantıksal uygulamanızı bolluk tetikleyicisi veya **yineleme** tetikleyicisi gibi başka bir tetikleyici gibi bir tetikleyici ile başlatın.

## <a name="connect-to-slack"></a>Bolluk 'e Bağlan

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure Portal](https://portal.azure.com)oturum açın ve daha önce açık değilse mantıksal uygulama Tasarımcısı 'nda mantıksal uygulamanızı açın.

1. Boş Logic Apps için, arama kutusuna filtreniz olarak "bolluk" yazın. Tetikleyiciler listesinde istediğiniz tetikleyiciyi seçin. 

   -veya-

   Mevcut Logic Apps için, eylem eklemek istediğiniz son adım altında **yeni adım**' ı seçin. 
   Arama kutusuna filtreniz olarak "bolluk" yazın. 
   Eylemler listesi altında istediğiniz eylemi seçin.

   Adımlar arasında bir eylem eklemek için, işaretçinizi adımlar arasındaki oka taşıyın. 
   Görüntülenen artı işaretini ( **+** ) seçin ve ardından **Eylem Ekle**' yi seçin.

1. Bolluk ' te oturum açmanız istenirse, bolluk çalışma alanınızda oturum açın. 

   ![Bolluk çalışma alanında oturum aç](./media/connectors-create-api-slack/slack-sign-in-workspace.png)

1. Mantıksal uygulamanız için erişim yetkisi verin.

   ![Bolluk 'e erişim yetkisi verme](./media/connectors-create-api-slack/slack-authorize-access.png)

1. Seçtiğiniz tetikleyici veya eyleminiz için gerekli ayrıntıları sağlayın. Mantıksal uygulamanızın iş akışını oluşturmaya devam etmek için daha fazla eylem ekleyin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının Openapı (eski adıyla Swagger) açıklaması tarafından tanımlanan Tetikleyiciler, Eylemler ve limitlerle ilgili teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/slack/)gözden geçirin.

## <a name="get-support"></a>Destek alın

* Sorular için, [Azure Logic Apps Için Microsoft Q&soru sayfasını](/answers/topics/azure-logic-apps.html)ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin

