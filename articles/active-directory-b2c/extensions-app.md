---
title: Azure Active Directory B2C 'de uzantılar uygulaması | Microsoft Docs
description: B2C-Extensions-App ' i geri yükleme.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/06/2017
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: dc536fa4292d794e8d89a2564ad10a3c10dd0a3d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94560869"
---
# <a name="azure-ad-b2c-extensions-app"></a>Azure AD B2C: uzantılar uygulaması

Azure AD B2C bir dizin oluşturulduğunda, adlı bir uygulama `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` Yeni dizin içinde otomatik olarak oluşturulur. **B2C-Extensions-App** olarak anılan bu uygulama *uygulama kayıtları* görünür. Kullanıcılar ve özel öznitelikler hakkında bilgi depolamak için Azure AD B2C hizmeti tarafından kullanılır. Uygulamanın silinmesi halinde Azure AD B2C düzgün çalışmaz ve üretim ortamınız bu durumdan etkilenir.

> [!IMPORTANT]
> Kiracınızı hemen silmeyi planlamadığınız müddetçe B2C-Extensions-App ' i silmeyin. Uygulama 30 günden uzun bir süre sonra silinirse, Kullanıcı bilgileri kalıcı olarak kaybedilir.

## <a name="verifying-that-the-extensions-app-is-present"></a>Uzantı uygulamasının mevcut olduğu doğrulanıyor

B2C-Extensions-uygulamasının mevcut olduğunu doğrulamak için:

1. Azure AD B2C kiracınızda, sol taraftaki gezinti menüsünde **tüm hizmetler** ' e tıklayın.
1. **Uygulama kayıtları** arayın ve açın.
1. **B2C-Extensions-App** ile başlayan bir uygulama arayın

## <a name="recover-the-extensions-app"></a>Uzantılar uygulamasını kurtarma

B2C-Extensions-App ' i yanlışlıkla sildiyseniz, bu uygulamayı kurtarmak için 30 gününüz vardır. Graph API kullanarak uygulamayı geri yükleyebilirsiniz:

1. [https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/) adresine göz atın.
1. İçin silinen uygulamayı geri yüklemek istediğiniz Azure AD B2C dizin için genel yönetici olarak sitede oturum açın. Bu genel yöneticinin aşağıdakine benzer bir e-posta adresi olmalıdır: `username@{yourTenant}.onmicrosoft.com` .
1. `https://graph.windows.net/myorganization/deletedApplications`Api-Version = 1.6 Ile URL 'ye yönelik BIR http get verme. Bu işlem, son 30 gün içinde silinmiş tüm uygulamaların listesini alacak.
1. Adın ' B2C-Extensions-App ' ile başladığı listede uygulamayı bulun ve `objectid` özellik değerini kopyalayın.
1. URL 'ye karşı bir HTTP POST verme `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore` . `{OBJECTID}`URL 'nin bölümünü önceki adımdan bulunan ile değiştirin `objectid` .

Artık Azure portal [geri yüklenen uygulamayı görebilmelisiniz](#verifying-that-the-extensions-app-is-present) .

> [!NOTE]
> Bir uygulama yalnızca son 30 gün içinde siliniyorsa geri yüklenebilir. 30 günden daha uzun bir süredir, veriler kalıcı olarak kaybedilir. Daha fazla yardım için bir destek bileti dosyası yapın.
