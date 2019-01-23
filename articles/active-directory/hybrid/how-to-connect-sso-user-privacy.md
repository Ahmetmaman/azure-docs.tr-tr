---
title: Kullanıcı gizliliği ve Azure AD sorunsuz çoklu oturum açma | Microsoft Docs
description: Bu makalede, Azure Active Directory (Azure AD) sorunsuz çoklu oturum açma ve GDPR uyumluluğu ile ilgilidir.
services: active-directory
keywords: GDPR, Azure AD Connect nedir, Azure AD, SSO, bileşenleri gerekli çoklu oturum açma
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 5035fb8d8d144114835fb1202dc9c639b9e5a8e3
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54461137"
---
# <a name="user-privacy-and-azure-ad-seamless-single-sign-on"></a>Kullanıcı gizliliği ve Azure AD sorunsuz çoklu oturum açma

[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview"></a>Genel Bakış


Azure AD sorunsuz çoklu oturum açma kişisel veri içerebilen aşağıdaki günlük türü oluşturur: 

- Azure AD Connect izleme günlüğü dosyaları.

Kullanıcı gizliliği için sorunsuz SSO iki yolla geliştirme:

1.  İstek, bir kişi için verileri ayıklayın ve bu kişiden yüklemeleri veri kaldırın.
2.  Hiçbir veri 48 saat dışında tutulur emin olun.

Uygulamak ve sürdürmek daha kolay olduğu gibi ikinci seçeneği önerilir. Her günlük türü yönergelerini bakın:

### <a name="delete-azure-ad-connect-trace-log-files"></a>Azure AD Connect izleme günlük dosyalarını silin

İçeriğini denetlemek **%ProgramData%\AADConnect** klasörü ve delete izleme günlüğünü içeriği (**izleme -\*.log** dosyaları) Azure AD Connect'i yükleme veya 48 saat içinde bu klasörün ya da bu eylem olarak sorunsuz çoklu oturum açma yapılandırmasını değiştirme veriler GDPR kapsamında oluşturabilir.

>[!IMPORTANT]
>Silme **PersistedState.xml** dosyası bu klasörde, bu dosya, Azure AD Connect'in önceki yükleme durumunu korumak için kullanılır ve bir yükseltme yüklemesi tamamlandığında kullanılır. Bu dosya hiçbir zaman bir kişiyle ilgili tüm verileri içerir ve hiçbir zaman silinmesi gerekir.

Gözden geçirin ve Windows Gezgini'ni kullanarak bu izleme günlük dosyalarını silin veya gerekli eylemleri gerçekleştirmek için aşağıdaki PowerShell betiğini kullanabilirsiniz:

```
$Files = ((Get-Item -Path "$env:programdata\aadconnect\trace-*.log").VersionInfo).FileName 
 
Foreach ($file in $Files) { 
    {Remove-Item -Path $File -Force} 
}
```

Betik bir dosyaya kaydet ". PS1 "uzantısı. Gerektiğinde bu betiği çalıştırın.

İlgili Azure AD Connect GDPR gereksinimleri hakkında daha fazla bilgi edinmek için bkz. [bu makalede](reference-connect-user-privacy.md).

### <a name="note-about-domain-controller-logs"></a>Etki alanı denetleyicisi günlükleri hakkında dikkat edin.

Denetim günlüğü etkinleştirilmişse, bu ürün, etki alanı denetleyicileri için güvenlik günlükleri oluşturabilir. Denetim ilkeleri yapılandırma hakkında daha fazla bilgi edinmek için bu okuma [makale](https://technet.microsoft.com/library/dd277403.aspx).

## <a name="next-steps"></a>Sonraki adımlar
* [Güven Merkezi Microsoft Privacy ilkeyi gözden geçirin](https://www.microsoft.com/trustcenter)
  - [**Sorun giderme** ](tshoot-connect-sso.md) -özelliği ile ilgili yaygın sorunları çözmeyi öğrenin.
  - [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - yeni özellik istekleriniz dosyalama için.
