---
title: Öğretici - Azure Active Directory B2C kiracısı oluştur | Microsoft Docs
description: Azure portalını kullanarak bir Azure Active Directory B2C kiracısı oluşturarak, uygulamalarınızı kaydetmek için hazırlamayı öğrenin.
services: B2C
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: b8878eccb079bf78c45ff9c1e4040659d109b1ab
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55152944"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Öğretici: Azure Active Directory B2C kiracısı oluşturma

Uygulamalarınızı Azure Active Directory (Azure AD) B2C ile etkileşim kurabilmesi yönettiğiniz bir kiracıda kayıtlı olmaları gerekir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD B2C kiracısı oluşturma
> * Kiracı aboneliğinize bağlayın

Sonraki öğreticide bir uygulamayı kaydetme öğrenin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-an-azure-ad-b2c-tenant"></a>Azure AD B2C kiracısı oluşturma

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Tıklayarak aboneliğinizi içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve onu içeren dizine seçme. Bu dizin, Azure AD B2C kiracınızı içerecek hesaptan farklıdır.

    ![Abonelik dizinine geçin](./media/tutorial-create-tenant/switch-directory-subscription.png)

3. Seçin **kaynak Oluştur** Azure portalının sol üst köşedeki içinde.
4. Arayın ve seçin **Active Directory B2C**ve ardından **Oluştur**.
5. Seçin **yeni bir Azure AD B2C Kiracısı oluşturma**bir kuruluş adı ve Kiracı adı kullanılan ilk etki alanı adı girin (bunu daha sonra değiştirilemez) ülkeyi seçin ve ardından **Oluştur**.

    ![Kiracı oluşturma](./media/tutorial-create-tenant/create-tenant.png)

    Bu örnekte, contoso0926Tenant.onmicrosoft.com Kiracı adıdır

6. Üzerinde **yeni B2C Kiracısı oluşturun veya mevcut bir Kiracınız için bağlantı** sayfasında **Azure Aboneliğimi bağlantı var olan bir Azure AD B2C Kiracısına**, oluşturduğunuz Kiracı seçin, aboneliğinizi seçin ve ardından tıklayın **Yeni Oluştur**.
7. Kiracı içeren, konumu seçin ve ardından kaynak grubu için bir ad girin **Oluştur**.
8. Yeni Kiracı kullanmaya başlamak için Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve onu içeren dizine seçme.

    ![Dizin Kiracı anahtarı](./media/tutorial-create-tenant/switch-directories.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, öğrendiğiniz nasıl yapılır:

> [!div class="checklist"]
> * Azure AD B2C kiracısı oluşturma
> * Kiracı aboneliğinize bağlayın

> [!div class="nextstepaction"]
> [Uygulamalarınızı kaydetme](tutorial-register-applications.md)
