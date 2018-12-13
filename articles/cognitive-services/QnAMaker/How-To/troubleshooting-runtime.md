---
title: Sorun giderme - QnAMaker
titlesuffix: Azure Cognitive Services
description: Kullanıcının Azure hesabında barındırılan bileşenlerinin QnAMaker oluşur. Hata ayıklama QnAMaker Azure kaynaklarını yönetmek veya QnAMaker destek ekibi kendi kurulumu hakkında ek bilgi sağlamak kullanıcıların gerektirebilir.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 065b6551098a39fb737b7eface17d78b111d31b6
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53074154"
---
# <a name="troubleshooting-tips-to-support-the-qna-maker-service-and-runtime"></a>Çalışma zamanı ve soru-cevap Oluşturucu hizmetini desteklemek için sorun giderme ipuçları
Kullanıcının Azure hesabında barındırılan bileşenlerinin QnAMaker oluşur. Hata ayıklama QnAMaker Azure kaynaklarını yönetmek veya QnAMaker destek ekibi kendi kurulumu hakkında ek bilgi sağlamak kullanıcıların gerektirebilir.

## <a name="how-to-get-latest-qnamaker-runtime-updates"></a>QnAMaker çalışma zamanı güncelleştirmeleri alma
QnAMaker çalışma zamanı parçası olan Azure App Service'in ne zaman dağıtılacağını, [QnAMaker hizmeti oluşturma](./set-up-qnamaker-service-azure.md) Azure portalında. Güncelleştirmeler çalışma zamanı için düzenli aralıklarla hale getirilir. QnAMaker kurulumunuzu uygulamak için en son güncelleştirmeleri uygulamak için uygulama hizmetini yeniden başlatmanız gerekir.
1. QnAMaker hizmetinize (kaynak grubu) Git [Azure portalı](https://portal.azure.com)

    ![QnAMaker Azure kaynak grubu](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

2. App Service üzerinde tıklayın ve genel bakış bölümünü açın

     ![QnAMaker App Service](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

3. App service'ı yeniden başlatın. Bu işlem birkaç saniye içinde tamamlanmalıdır. Aşağı akış uygulamaları/robotlarınızın bu QnAMaker hizmeti oluşturma Not Bu yeniden başlatma süresi boyunca son kullanıcılar için kullanılamaz.

    ![QnAMaker appservice yeniden başlatma](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="how-to-get-the-qnamaker-service-hostname"></a>QnAMaker hizmet ana bilgisayar adını alma
QnAMaker desteği veya UserVoice başvurduğunuzda QnAMaker hizmeti ana bilgisayar adı hata ayıklama amaçları için yararlıdır. Bu form URL'sini adıdır: https://*{hostname}*. azurewebsites.net.
    
1. QnAMaker hizmetinize (kaynak grubu) Git [Azure portalı](https://portal.azure.com)

    ![Azure portalında QnAMaker Azure kaynak grubu](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

2. Uygulama hizmeti

     ![QnAMaker uygulama hizmetini seçin](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

3. Konak adı URL genel bakış bölümünde kullanılabilir

    ![QnAMaker ana bilgisayar adı](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-gethostname.png)
    

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [QnAMaker API kullanın](./upgrade-qnamaker-service.md)
