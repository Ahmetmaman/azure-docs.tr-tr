---
title: Azure Izleyici günlüklerine akış Azure Active Directory günlükleri | Microsoft Docs
description: Azure Izleyici günlükleriyle Azure Active Directory günlüklerini tümleştirme hakkında bilgi edinin
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 2c3db9a8-50fa-475a-97d8-f31082af6593
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: f70d1caacfd655c956d4fcc36e3f0d3848d8f0fe
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89230577"
---
# <a name="integrate-azure-ad-logs-with-azure-monitor-logs"></a>Azure AD günlüklerini Azure Izleyici günlükleriyle tümleştirme

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Azure Izleyici günlükleri belirli olayları bulmak, eğilimleri analiz etmek ve çeşitli veri kaynakları arasında bağıntı gerçekleştirmek için verileri sorgulamanızı sağlar. Azure Izleyici günlüklerinde Azure AD etkinlik günlüklerinin tümleştirmesiyle, şu gibi görevleri gerçekleştirebilirsiniz:

 * Azure AD oturum açma günlüklerinizi Azure Güvenlik Merkezi tarafından yayınlanan güvenlik günlükleriyle karşılaştırın

 * Uygulama performansı verilerinin Azure Application Insights ile ilişkilendirilmesi için uygulamanızın oturum açma sayfasında performans sorunlarını giderin.  

Bir Ignite oturumundan alınan aşağıdaki videoda, pratik Kullanıcı senaryolarında Azure AD günlükleri için Azure Izleyici günlüklerini kullanmanın avantajları gösterilmektedir.

> [!VIDEO https://www.youtube.com/embed/MP5IaCTwkQg?start=1894]

Bu makalede, Azure Izleyici ile Azure Active Directory (Azure AD) günlüklerinin nasıl tümleştirileceğini öğreneceksiniz.

## <a name="supported-reports"></a>Desteklenen raporlar

Daha fazla analiz için denetim etkinlik günlüklerini ve oturum açma etkinliği günlüklerini Azure Izleyici günlüklerine yönlendirebilirsiniz. 

* **Denetim günlükleri**: [Denetim günlükleri etkinlik raporu](concept-audit-logs.md), kiracınızda gerçekleştirilen her görevin geçmişine erişmenizi sağlar.
* **Oturum açma günlükleri**: [Oturum açma işlemleri etkinlik raporuyla](concept-sign-ins.md), denetim günlüklerinde bildirilen görevleri kimlerin gerçekleştirdiğini saptayabilirsiniz.

> [!NOTE]
> B2C ile ilgili denetim ve oturum açma işlemleri etkinlik günlükleri şu an için desteklenmemektedir.
>

## <a name="prerequisites"></a>Ön koşullar 

Bu özelliği kullanmak için şunlara ihtiyacınız vardır:

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz denemeye kaydolabilirsiniz](https://azure.microsoft.com/free/).
* Azure AD kiracısı.
* Azure AD kiracısında *genel yönetici* veya *güvenlik yöneticisi* olan bir kullanıcı.
* Azure aboneliğinizdeki bir Log Analytics çalışma alanı. [Log Analytics çalışma alanı oluşturmayı](../../azure-monitor/learn/quick-create-workspace.md)öğrenin.

## <a name="licensing-requirements"></a>Lisanslama gereksinimleri

Bu özelliğin kullanılması için Azure AD Premium P1 veya P2 lisansı gerekir. Gereksinimlerinize uygun lisansı bulmak için bkz. [Ücretsiz, Temel ve Premium sürümlerinin genel olarak sağlanan özelliklerini karşılaştırma](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="send-logs-to-azure-monitor"></a>Günlükleri Azure Izleyici 'ye gönderme

1. [Azure portalında](https://portal.azure.com) oturum açın. 

2. **Azure Active Directory**  >  **Tanılama ayarlarını**Azure Active Directory  ->  **Tanılama ayarı Ekle**' yi seçin. Tanılama ayarları yapılandırma sayfasına ulaşmak için **Denetim günlükleri** veya **oturum açma** sayfasından **ayarları dışarı aktar** ' ı da seçebilirsiniz.  
    
3. **Tanılama ayarları** menüsünde **Log Analytics çalışma alanına gönder** onay kutusunu seçin ve ardından **Yapılandır**' ı seçin.

4. Günlükleri göndermek istediğiniz Log Analytics çalışma alanını seçin veya belirtilen iletişim kutusunda yeni bir çalışma alanı oluşturun.  

5. Aşağıdakilerden birini veya ikisini birden yapın:
    * Log Analytics çalışma alanına denetim günlükleri göndermek için **auditlogs** onay kutusunu seçin. 
    * Log Analytics çalışma alanına oturum açma günlükleri göndermek için **Signınlogs** onay kutusunu seçin.

6. Ayarları kaydetmek için **Kaydet**’i seçin.

    ![Tanılama ayarları](./media/howto-integrate-activity-logs-with-log-analytics/Configure.png)

7. 15 dakika sonra, olayların Log Analytics çalışma alanınıza akışı olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Izleyici günlükleri ile Azure AD etkinlik günlüklerini çözümleme](howto-analyze-activity-logs-log-analytics.md)
* [Azure Active Directory için Log Analytics görünümlerini yükleyip kullanın](howto-install-use-log-analytics-views.md)