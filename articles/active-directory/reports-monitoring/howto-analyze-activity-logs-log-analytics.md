---
title: Log Analytics (Önizleme) kullanarak Azure Active Directory etkinlik günlüklerini çözümleme | Microsoft Docs
description: Log Analytics (Önizleme) kullanarak Azure Active Directory etkinlik günlükleri analiz etmeyi öğrenin
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 4535ae65-8591-41ba-9a7d-b7f00c574426
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7ea13d08af924427b9e7dc5def72c19d560525b8
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56188265"
---
# <a name="analyze-azure-ad-activity-logs-with-log-analytics-preview"></a>Azure AD çözümleme etkinlik günlüklerini Log Analytics (Önizleme) ile

Çalıştırdıktan sonra [tümleştirme Azure AD etkinlik günlüklerini Log Analytics ile](howto-integrate-activity-logs-with-log-analytics.md), ortamınız hakkında Öngörüler elde etmek için günlük analizinin gücünü kullanabilirsiniz. Ayrıca yükleyebilirsiniz [Log Analytics görünümleri için Azure AD etkinlik günlüklerini](howto-install-use-log-analytics-views.md) ortamınıza erişmek için Denetim ve oturum açma olaylarını önceden oluşturulmuş raporları.

Bu makalede, Azure analiz etmeyi öğrenin AD etkinlik günlüklerini Log Analytics çalışma alanınızda. 

## <a name="prerequisites"></a>Önkoşullar 

Örneği takip etmek için ihtiyacınız vardır:

* Azure aboneliğinizdeki bir Log Analytics çalışma. Bilgi edinmek için nasıl [Log Analytics çalışma alanı oluşturma](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace).
* İlk olarak, adımları tamamlamak [rota Azure AD etkinlik günlüklerini Log Analytics çalışma alanınıza](howto-integrate-activity-logs-with-log-analytics.md).

## <a name="navigate-to-the-log-analytics-workspace"></a>Log Analytics çalışma alanına gidin

1. [Azure Portal](https://portal.azure.com) oturum açın. 

2. Seçin **Azure Active Directory**ve ardından **günlükleri** gelen **izleme** bölümü, Log Analytics çalışma alanını açın. Çalışma alanı bir varsayılan sorgu ile açılır.

    ![Varsayılan sorgu](./media/howto-analyze-activity-logs-log-analytics/defaultquery.png)


## <a name="view-the-schema-for-azure-ad-activity-logs"></a>Azure AD etkinlik şemasını görüntülemek günlükleri

Günlükleri depoya **AuditLogs** ve **SigninLogs** çalışma alanında Tablo. Bu tablo şemasını görüntülemek için:

1. Önceki bölümde varsayılan sorgu görünümünde seçin **şema** ve çalışma alanını genişletin. 

2. Genişletin **günlük yönetimi** bölümünde ve ya da genişletin **AuditLogs** veya **SignInLogs** günlük şemasını görüntülemek için.
    ![Denetim günlükleri](./media/howto-analyze-activity-logs-log-analytics/auditlogschema.png) ![Signın günlükleri](./media/howto-analyze-activity-logs-log-analytics/signinlogschema.png)

## <a name="query-the-azure-ad-activity-logs"></a>Sorgu Azure AD etkinlik günlükleri

Çalışma alanınızda günlükleri sahip olduğunuza göre artık sorguları bunlara karşı çalıştırabilirsiniz. Örneğin, son bir hafta içinde kullanılan en iyi uygulamaları almak için varsayılan sorguyu seçin ve aşağıdaki ile değiştirin **çalıştırın**

```
SigninLogs 
| where CreatedDateTime >= ago(7d)
| summarize signInCount = count() by AppDisplayName 
| sort by signInCount desc 
```

Geçen haftaya göre üst denetim olaylarını almak için aşağıdaki sorguyu kullanın:

```
AuditLogs 
| where TimeGenerated >= ago(7d)
| summarize auditCount = count() by OperationName 
| sort by auditCount desc 
```
## <a name="alert-on-azure-ad-activity-log-data"></a>Azure AD etkinlik günlüğü verileri temelinde uyar

Sorgunuzda uyarıları da ayarlayabilirsiniz. Örneğin, 10'dan fazla olduğunda bir uyarı yapılandırmak için uygulamaları son bir hafta içinde olarak kullanılmıştır:

1. Çalışma alanından seçin **uyarı ayarlama** açmak için **oluşturma kuralı** sayfası. 
    ![Uyarı ayarlama](./media/howto-analyze-activity-logs-log-analytics/setalert.png)

2. Varsayılan seçin **Uyarı ölçütleri** uyarı ve güncelleştirme oluşturulan **eşiği** 10 varsayılan ölçümündeki. 
    ![Uyarı ölçütleri](./media/howto-analyze-activity-logs-log-analytics/alertcriteria.png)

3. Bir ad ve uyarı için bir açıklama girin ve önem derecesini seçin. Bizim örneğimizde, biz ayarlayabilirsiniz **bilgilendirici**.

4. Seçin **eylem grubu** , uyarılmak sinyali oluştuğunda. Web kancaları, Azure işlevleri veya mantıksal uygulamalar'ı kullanarak eyleme otomatikleştirmek veya e-posta veya SMS mesajı aracılığıyla ekibinizi bilgilendirin seçebilirsiniz. Daha fazla bilgi edinin [grupları Azure portalında uyarı oluşturma ve yönetme](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups).

5. Uyarı yapılandırıldıktan sonra seçin **Oluştur uyarı** etkinleştirin. 

## <a name="install-and-use-pre-built-views-for-azure-ad-activity-logs"></a>Yükleyip önceden oluşturulmuş görünümler için Azure AD etkinlik günlükleri

Azure AD etkinlik için önceden oluşturulmuş Log Analytics görünümleri de indirebilirsiniz günlükleri. Görünümler, Denetim ve oturum açma olayları ile ilgili yaygın senaryolar için ilgili çeşitli raporlar sağlar. Ayrıca tüm raporların, sağlanan verilerin önceki bölümde açıklanan adımları kullanarak uyarabilir.

* **Azure AD hesabı olayları sağlama**: Bu görünümde sağlanan yeni kullanıcı sayısı gibi sağlama etkinliği denetim ile ilgili raporlar gösterilir ve sağlama hataları, kullanıcı sayısı güncelleştirildi ve güncelleştirme hataları ve kullanıcılar da sağlanan ve karşılık gelen hataları sayısı.    
* **Oturum açma olayları**: Bu görünümde, oturum açma etkinliği, uygulama, kullanıcı, cihaz yanı sıra oturum açma sayısı zamanla izleme Özet görünümünü tarafından oturum açma işlemleri gibi izleme ile ilgili en ilgili raporlar gösterilir.
* **Kullanıcı onayı gerçekleştirme**: Bu görünüm, gibi tüm onay tabanlı uygulamalar için uygulama tarafından oturum açma yanı sıra kullanıcı, oturum açma işlemleri tarafından izin verilen kullanıcılar tarafından izin verir. kullanıcı onayı için ilgili raporları gösterir. 

[Azure AD etkinlik günlükleri için Log Analytics görünümlerini yüklemeyi ve kullanmayı](howto-install-use-log-analytics-views.md) öğrenin. 


## <a name="next-steps"></a>Sonraki adımlar

* [Log Analytics sorguları kullanmaya başlama](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries)
* [Azure portalında uyarı gruplarını oluşturma ve yönetme](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups)
* [Yükleme ve Azure Active Directory için Log Analytics görünümleri kullanma](howto-install-use-log-analytics-views.md)
