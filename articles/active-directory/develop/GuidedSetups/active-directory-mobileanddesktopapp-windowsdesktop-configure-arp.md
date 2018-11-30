---
title: Azure AD v2 Windows Masaüstü alma başlatıldı - Config | Microsoft Docs
description: Nasıl bir Windows Masaüstü .NET (XAML) uygulama erişim belirteci almak ve Azure Active Directory v2 uç noktası tarafından korunan bir API çağrısı.
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 47d62b5ea49205a5e639d620d78466e7e4a66bca
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52581417"
---
# <a name="add-the-applications-registration-information-to-your-app"></a>Uygulamanın kayıt bilgilerini uygulamanıza ekleme
Bu adımda, uygulama kimliği projenize eklemeniz gerekir.

1.  Açık `App.xaml.cs` içeren satırı değiştirin `ClientId` ile:

```csharp
private static string ClientId = "[Enter the application Id here]";
```

### <a name="what-is-next"></a>Sonraki nedir

[!INCLUDE [Test and Validate](../../../../includes/active-directory-develop-guidedsetup-windesktop-test.md)]
