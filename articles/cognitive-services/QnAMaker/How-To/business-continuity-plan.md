---
title: İş sürekliliği planları - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: İş sürekliliği planları birincil amacı, arıza süresi olmadan Bot veya uygulamanın hangi olun bir esnek Bilgi Bankası uç noktası oluşturmaktır.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 01/14/2019
ms.author: tulasim
ms.openlocfilehash: ad4f10b3b59e71ca31f1b27879c4b60157f0a46c
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55881367"
---
# <a name="create-a-business-continuity-plan-for-your-qna-maker-service"></a>Soru-cevap Oluşturucu hizmetiniz için bir iş süreklilik planı oluşturma

İş sürekliliği planları birincil amacı, arıza süresi olmadan Bot veya uygulamanın hangi olun bir esnek Bilgi Bankası uç noktası oluşturmaktır.

![Soru-cevap Oluşturucu bcp planı](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

Üst düzey yukarıda gösterildiği gibi olur:

1. İki paralel kümesi [soru-cevap Oluşturucu Hizmetleri](../How-To/set-up-qnamaker-service-azure.md) içinde [Azure eşleştirilmiş bölgeleri](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

2. Birincil ve ikincil bir Azure search dizinlerini eşitlenmiş halde tutun. GitHub örneği [burada](https://github.com/pchoudhari/QnAMakerBackupRestore) Azure dizinleri yedekleme geri yükleme yapılacağını görmek için.

3. Application Insights kullanarak yedekleme [sürekli dışarı aktarma](https://docs.microsoft.com/azure/application-insights/app-insights-export-telemetry).

4. Birincil ve ikincil yığınları ayarlandıktan sonra kullanın [traffic manager](https://docs.microsoft.com/azure/traffic-manager/) iki uç nokta yapılandırmak ve bir yönlendirme yöntemi ayarlamak için.

5. Traffic manager uç noktanız için bir SSL sertifikası oluşturmanız gerekir. [SSL sertifikası bağlama](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl) uygulama hizmetlerinizdeki.

6. Son olarak, traffic manager uç nokta Bot veya uygulamalarda kullanın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-cevap Oluşturucu dağıtımınız için kapasite seçin](../Tutorials/choosing-capacity-qnamaker-deployment.md)
