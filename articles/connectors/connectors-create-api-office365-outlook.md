---
title: Office 365 Outlook - Azure Logic Apps'i bağlama | Microsoft Docs
description: E-posta, kişiler ve Takvim Office 365 REST API'lerini ve Azure Logic Apps ile yönetme
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 10/18/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 0355f121a09e1ba89f98a8af5037eb1371db2242
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39215647"
---
# <a name="get-started-with-the-office-365-outlook-connector"></a>Office 365 Outlook Bağlayıcısı ile çalışmaya başlama
Office 365 Outlook Bağlayıcısı, Office 365 Outlook'ta etkileşimine olanak tanır. Bu Bağlayıcıyı oluşturmak, düzenlemek ve kişiler ve takvim öğeleri, güncelleştirme ve ayrıca Al, Gönder ve e-postayı yanıtlamak için kullanın.

Office 365 Outlook:

* Office 365'te e-posta ve Takvim özelliklerini kullanarak, iş akışınızı oluşturun. 
* Tetikleyiciler, bir Takvim öğesi güncelleştirilirken bir yeni e-posta ve daha fazla olduğunda, iş akışınızı başlatmak için kullanın.
* Eylemler bir e-posta gönderin, yeni bir takvim etkinliği ve daha fazlasını oluşturmak için kullanın. Örneğin, (tetikleyici) Salesforce'ta yeni bir nesne olmadığında, Office 365 Outlook (bir eylem) bir e-posta gönderin. 

Bu makalede, bir mantıksal uygulama Office 365 Outlook Bağlayıcısı'nı kullanma işlemini gösterir ve ayrıca Tetikleyicileri ve eylemleri listeler.

> [!NOTE]
> Makalenin bu sürümü Logic Apps genel kullanılabilirlik (GA) için geçerlidir.
> 
> 

Logic Apps hakkında daha fazla bilgi için bkz: [mantıksal uygulamalar nedir](../logic-apps/logic-apps-overview.md) ve [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-office-365"></a>Office 365’e bağlanın
Mantıksal uygulamanızı herhangi bir hizmete erişebilmeniz için önce ilk oluşturmak bir *bağlantı* hizmeti. Bir bağlantı, bir mantıksal uygulama ile başka bir hizmet arasında bağlantı sağlar. Örneğin, Office 365 Outlook'a bağlanma için önce bir Office 365 ihtiyacınız *bağlantı*. Bir bağlantı oluşturmak için normalde bağlanmak istediğiniz hizmete erişmek için kullandığınız kimlik bilgilerini girin. Bu nedenle Office 365 Outlook ile bağlantı oluşturmak için Office 365 hesabınıza kimlik bilgilerini girin.

## <a name="create-the-connection"></a>Bağlantı oluşturma
> [!INCLUDE [Steps to create a connection to Office 365](../../includes/connectors-create-api-office365-outlook.md)]
> 
> 

## <a name="use-a-trigger"></a>Tetikleyici kullanma
Bir tetikleyici bir mantıksal uygulamada tanımlanan iş akışını başlatmak için kullanılan bir olaydır. Tetikleyiciler "hizmet bir aralığı ve istediğiniz sıklığı yoklama". [Tetikleyiciler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Mantıksal uygulama içinde "Tetikleyiciler, bir listesini almak için office 365" yazın:  
   
    ![](./media/connectors-create-api-office365-outlook/office365-trigger.png)
2. Seçin **yaklaşan bir etkinlik yakında başlıyor, Office 365 Outlook -**. Bir bağlantı zaten varsa, bir Takvim açılır listeden seçin.
   
    ![](./media/connectors-create-api-office365-outlook/sample-calendar.png)
   
    Oturum açmanız istenirse, oturum bağlantısı oluşturmak için ayrıntıları girin. [Bağlantı oluşturma](connectors-create-api-office365-outlook.md#create-the-connection) bu konudaki adımları listelenir. 
   
   > [!NOTE]
   > Bu örnekte, bir takvim etkinliği güncelleştirildiğinde mantıksal uygulama çalıştırmaları. Bu tetikleyiciyi sonuçlarını görmek için bir kısa mesaj gönderen başka bir eylem ekleyin. Örneğin, Twilio ekleyin *ileti gönder* eylemi, metinler, 15 dakika içinde bir takvim etkinliği Başlarken. 
   > 
   > 
3. Seçin **Düzenle** ayarlayın ve düğme **sıklığı** ve **aralığı** değerleri. Örneğin, her 15 dakikada yoklamak için tetikleyici istiyorsanız ayarlayın **sıklığı** için **dakika**, ayarlayıp **aralığı** için **15**. 
   
    ![](./media/connectors-create-api-office365-outlook/calendar-settings.png)
4. **Kaydet** değişikliklerinizi (sol üst köşesindeki araç). Mantıksal uygulamanızı kaydedilir ve otomatik olarak etkinleştirilir.

## <a name="use-an-action"></a>Bir eylem kullanın
Bir eylem, bir mantıksal uygulamada tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. [Eylemler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Artı işaretini seçin. Birkaç seçenek görürsünüz: **Eylem Ekle**, **koşul Ekle**, veya biri **daha fazla** seçenekleri.
   
    ![](./media/connectors-create-api-office365-outlook/add-action.png)
2. Seçin **Eylem Ekle**.
3. Metin kutusuna "kullanılabilir tüm eylemlerin bir listesini almak için office 365" yazın.
   
    ![](./media/connectors-create-api-office365-outlook/office365-actions.png) 
4. Bizim örneğimizde seçin **Office 365 Outlook - ilgili kişi oluşturma**. Bir bağlantı zaten varsa, ardından **klasör kimliği**, **verilen ad**ve diğer özellikleri:  
   
    ![](./media/connectors-create-api-office365-outlook/office365-sampleaction.png)
   
    Ardından bağlantı bilgileri istenirse, bağlantı oluşturmak için ayrıntıları girin. [Bağlantı oluşturma](connectors-create-api-office365-outlook.md#create-the-connection) Bu konu başlığında bu özellikleri açıklar. 
   
   > [!NOTE]
   > Bu örnekte biz Office 365 Outlook'ta yeni kişi oluşturun. Başka bir tetikleyici çıkışı kişi oluşturmak için kullanabilirsiniz. Örneğin, SalesForce ekleyin *bir nesne oluşturulduğunda* tetikleyici. Office 365 Outlook eklersiniz *kişi oluştur* SalesForce alanlar Office 365'te yeni kişi oluşturmak için kullandığı eylem. 
   > 
   > 
5. **Kaydet** değişikliklerinizi (sol üst köşesindeki araç). Mantıksal uygulamanızı kaydedilir ve otomatik olarak etkinleştirilir.

## <a name="connector-specific-details"></a>Özel bağlayıcı ayrıntıları

Tetikleyiciler ve eylemlerin swagger'da tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınırlama Bkz [bağlayıcı ayrıntıları](/connectors/office365connector/). 

## <a name="next-steps"></a>Sonraki Adımlar
[Mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Logic Apps diğer kullanılabilir bağlayıcılar keşfedin bizim [API listesi](apis-list.md).

