---
title: Azure AD raporlama API'si ile çalışmaya başlama | Microsoft Docs
description: Nasıl Azure Active Directory raporlama API'SİYLE çalışmaya başlama
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 860d602ecba257ed9015d1e080e5dcb1aa5ab872
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624633"
---
# <a name="get-started-with-the-azure-active-directory-reporting-api"></a>Azure Active Directory raporlama API'SİYLE çalışmaya başlama

Azure Active Directory ile çeşitli sağlar [raporları](overview-reports.md), SIEM sistemleri, Denetim ve iş zekası araçları gibi uygulamalar için yararlı bilgiler içeren. 

Azure AD raporlar için Microsoft Graph API'sini kullanarak, bir dizi REST tabanlı API aracılığıyla verilere programlı erişim elde edebilirsiniz. Çeşitli programlama dilleri ve araçlarından bu API'leri çağırabilirsiniz.

Bu makalede erişim yolları dahil olmak üzere Raporlama API'si ile bir genel bakış sağlar.

Sorun yaşarsanız bkz [Azure Active Directory için destek alma](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto).

## <a name="prerequisites"></a>Önkoşullar

Raporlama API'si ile veya kullanıcı müdahalesi olmadan erişmek için gerekir:

1. (Güvenlik okuyucusu, Güvenlik Yöneticisi, genel yönetici) Rolleri Ata
2. Bir uygulamayı kaydetme
3. İzinleri verme
4. Yapılandırma ayarlarını toplayın

Ayrıntılı yönergeler için bkz. [Azure Active Directory raporlama API'SİYLE erişmek için Önkoşullar](howto-configure-prerequisites-for-reporting-api.md). 

## <a name="api-endpoints"></a>API uç noktaları 

Microsoft Graph API uç nokta için denetim günlüklerini `https://graph.microsoft.com/beta/auditLogs/directoryAudits` ve oturum açma işlemleri için Microsoft Graph API uç noktası `https://graph.microsoft.com/beta/auditLogs/signIns`. Daha fazla bilgi için [denetim API'si başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) ve [oturum açma, API Başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signIn).

## <a name="apis-with-graph-explorer"></a>API'leri ile Graph Gezgini

Kullanabileceğiniz [explorer MSGraph](https://developer.microsoft.com/graph/graph-explorer) oturum açma işleminizi doğrulamak ve API veri denetleyebilirsiniz. Oturum açma düğmelerinin her ikisi de Graph Gezgini Arabiriminde kullanarak hesabınızda oturum açtığınızdan emin olun ve ayarlama **AuditLog.Read.All** ve **Directory.Read.All** gösterildiği kiracınız için izinleri.   

![Graph Gezgini](./media/concept-reporting-api/graph-explorer.png)

![UI izinleri değiştirme](./media/concept-reporting-api/modify-permissions.png)

## <a name="use-certificates-to-access-the-azure-ad-reporting-api"></a>Azure AD raporlama API'si erişmek için sertifika kullanma 

Kullanıcı müdahalesi olmadan raporlama verilerini almak planlıyorsanız Azure AD raporlama API'si ile sertifikaları kullanır.

Ayrıntılı yönergeler için bkz. [sertifikalarla Azure AD raporlama API'sini kullanarak veri alma](tutorial-access-api-with-certificates.md).

## <a name="next-steps"></a>Sonraki adımlar

 * [Raporlama API'sini erişmek için Önkoşullar](howto-configure-prerequisites-for-reporting-api.md) 
 * [Sertifikalarla Azure AD raporlama API'sini kullanarak veri alma](tutorial-access-api-with-certificates.md)
 * [Azure AD raporlama API'sini hatalarını giderme](troubleshoot-graph-api.md)


