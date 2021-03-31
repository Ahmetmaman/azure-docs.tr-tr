---
title: Cisco verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Panoları görüntülemek, özel uyarılar oluşturmak ve araştırmayı geliştirmek için Cisco ASA gerecinizi Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 62029b5c-29d3-4336-8a22-a9db8214eb7e
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: e8a64dd3e47384ba2bf7579f8052177252634622
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "85566037"
---
# <a name="connect-cisco-asa-to-azure-sentinel"></a>Cisco ASA 'yi Azure Sentinel 'e bağlama



Bu makalede Cisco ASA gerecinizi Azure Sentinel 'e nasıl bağlayabileceğiniz açıklanır. Cisco ASA veri Bağlayıcısı, Azure Sentinel ile Cisco ASA günlüklerinizi kolayca bağlamanıza olanak tanır, panoları görüntüleyebilir, özel uyarılar oluşturabilir ve araştırmayı geliştirebilirsiniz. Azure Sentinel 'de Cisco ASA kullanmak, kuruluşunuzun Internet kullanımı hakkında daha fazla öngörü sağlar ve güvenlik işlemi yeteneklerini geliştirir. 



## <a name="forward-cisco-asa-logs-to-the-syslog-agent"></a>Cisco ASA günlüklerini Syslog aracısına ilet

Cisco ASA, CEF 'yi desteklemez, bu nedenle Günlükler Syslog olarak gönderilir ve Azure Sentinel Aracısı onları CEF günlükleri gibi nasıl ayrıştırarak olduğunu bilir. Syslog Aracısı aracılığıyla syslog iletilerini Azure çalışma alanınıza iletmek için Cisco ASA 'ı yapılandırın:

1. [Bir dış Syslog sunucusuna Syslog Iletileri gönder](https://aka.ms/asi-syslog-cisco-forwarding)' e gidin ve bağlantıyı kurmak için yönergeleri izleyin. İstendiğinde bu parametreleri kullanın:
    - **Bağlantı noktasını** 514 olarak veya aracıda ayarladığınız bağlantı noktasını ayarlayın.
    - Aracının IP adresine **syslog_ip** ayarlayın.

1. Cisco olayları için Log Analytics ilgili şemayı kullanmak için arama yapın `CommonSecurityLog` .

1. 3. [Adım: bağlantıyı doğrulama adımına](connect-cef-verify.md)geçin.




## <a name="next-steps"></a>Sonraki adımlar
Bu belgede Cisco ASA gereçlerini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .


