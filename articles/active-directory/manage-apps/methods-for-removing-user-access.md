---
title: Bir kullanıcının uygulamaya erişimini kaldırmak nasıl | Microsoft Docs
description: Bir kullanıcının uygulamaya erişimini kaldırmak öğrenme
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/17/2018
ms.author: celested
ms.openlocfilehash: 9e0c7db1ecdcdd0f21e65f53a167d29786a58b92
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55963412"
---
# <a name="how-to-remove-a-users-access-to-an-application"></a>Bir uygulama için kullanıcı erişimi kaldırma

Bu makalede, bir kullanıcının uygulamaya erişimini kaldırmak nasıl anlamanıza yardımcı olur.

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a>Bir uygulama için belirli bir kullanıcının veya grubun atamasını kaldırmak istiyorum

Bir kullanıcı veya uygulamaya Grup atamasını kaldırmak için listelenen adımları izleyin. [kurumsal bir uygulamayı Azure Active Directory'de bir kullanıcı veya grup atamasını kaldırmak](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) makalesi.

## <a name="i-want-to-disable-all-access-to-an-application-for-every-user"></a>Her kullanıcı için tüm uygulama erişimi devre dışı bırakmak istiyorum

Bir uygulama için tüm kullanıcı oturum açma işlemleri devre dışı bırakmak için listelenen adımları izleyin. [kullanıcı oturum açma işlemleri için kurumsal bir uygulamayı Azure Active Directory'de devre dışı bırak](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) makalesi.

## <a name="i-want-to-delete-an-application-entirely"></a>Bir uygulamayı tamamen silmek istiyorum

İçin **bir uygulamayı silmek**, bu yönergeleri izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6.  Silmek istediğiniz uygulamayı seçin.

7.  Uygulama yüklendikten sonra tıklayın **Sil** üst uygulamanın simgesinden **genel bakış** bölmesi.

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a>Herhangi bir uygulama için tüm gelecek kullanıcı onayı işlemleri devre dışı bırakmak istiyorum

Tüm dizininizin engellemek için son kullanıcıların herhangi bir uygulama konusunda çekince kullanıcı onayı devre dışı bırakılıyor. Yöneticiler, yine de kullanıcının behalves üzerinde onay verebilir. Uygulama hakkında daha fazla bilgi için onay ve olabilir ya da bunu yapmak istiyor neden okuma [anlama kullanıcı ve yönetici onayı](../develop/howto-convert-app-to-be-multi-tenant.md#understand-user-and-admin-consent). Ayrıca bkz [izinler ve onay](../develop/v2-permissions-and-consent.md).

İçin **tüm dizininizdeki tüm gelecek kullanıcı onayı işlemleri devre dışı**, bu yönergeleri izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** 

3.  tıklayın **kurumsal uygulamalar** Gezinti menüsünde.

5.  Tıklayın **kullanıcı ayarları**.

6.  Ayarlama **kullanıcılar, uygulamalara kendileri adına şirket verilerine erişmesine izin verebilir** geç **Hayır** ve Kaydet düğmesine tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

[Uygulamalara erişimi yönetme](what-is-access-management.md)
