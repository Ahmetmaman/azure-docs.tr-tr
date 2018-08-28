---
title: Excel Online - Azure Logic Apps bağlayın | Microsoft Docs
description: Excel Online REST API'leri ve Azure Logic Apps ile verileri yönetme
ms.service: logic-apps
services: logic-apps
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.topic: article
ms.date: 08/23/2018
ms.openlocfilehash: ceef6c5f32372bb69f6ce789e755bc540cb12ba1
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43051958"
---
# <a name="manage-excel-online-data-with-azure-logic-apps"></a>Azure Logic Apps ile Excel Online veri yönetme

Azure Logic Apps ve Excel Online Bağlayıcısı ile otomatik görevler ve iş veya OneDrive için Excel Online'da verilerinizi temel iş akışları oluşturabilirsiniz. Bu bağlayıcı yardımcı Eylemler sağlar, verileri ile çalışma ve elektronik tablolar, örneğin yönetebilirsiniz: 

* Yeni çalışma sayfaları ve tabloları oluşturun.
* Alma ve çalışma, tabloları ve satırları yönetin.
* Tek satır ve anahtar sütunlarını ekleyin.

Bu eylemler çıkışları diğer hizmetler için Eylemler ile kullanın. Örneğin, her hafta çalışma oluşturan bir eylem kullanın, onay e-posta, Office 365 Outlook bağlayıcısını kullanarak gönderir, başka bir eylem kullanın.

Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

> [!NOTE]
> [Excel Online iş](/connectors/excelonlinebusiness/) ve [Excel Online OneDrive için](/connectors/excelonline/) bağlayıcıları Azure Logic Apps ile çalışma ve farklı [powerapps Excel bağlayıcı](/connectors/excel/).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* Bir [Office 365 hesabı](https://www.office.com/) iş hesabı veya kişisel Microsoft hesabı 

  Excel verilerinizi storage klasöründeki virgülle ayrılmış değer (CSV) dosyası olarak Örneğin, Onedrive'da bulunabilir. 
  Aynı CSV dosyası ile de kullanabileceğiniz [düz dosya bağlayıcı](../logic-apps/logic-apps-enterprise-integration-flatfile.md).

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Excel Online verilerinize erişmek için istediğiniz mantıksal uygulaması. Bu bağlayıcı, bu nedenle, mantıksal uygulamanızı başlatmak için yalnızca eylem seçebilir ayrı bir tetikleyici örneğin sağlar, **yinelenme** tetikleyici.

## <a name="add-excel-action"></a>Excel Eylem Ekle

1. İçinde [Azure portalında](https://portal.azure.com), mantıksal Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Tetikleyici altında seçin **yeni adım**.

1. Arama kutusuna filtreniz olarak "excel" girin. Eylemler listesinde, istediğiniz eylemi seçin.

1. Office 365 hesabınızda oturum açmanız istenirse seçin **oturum**. 

   Mantıksal uygulamanızı Excel Online bağlantı oluşturun ve verilerinize erişmek için kimlik bilgilerinizi yetkilendirin.

1. Seçili eylem için gerekli bilgileri sağlayarak ve mantıksal uygulamanızın iş akışı oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Eylemler ve bağlayıcılar Swagger dosyaları tarafından açıklanan sınırları gibi teknik ayrıntılar için bu bağlayıcı başvuru sayfalarına bakın:

* [İş için Excel Online'da](/connectors/excelonlinebusiness/) 
* [Excel Online'da OneDrive için](/connectors/excelonline/) 

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](http://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
