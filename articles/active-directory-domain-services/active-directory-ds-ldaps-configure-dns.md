---
title: LDAPS internet üzerinden kullanarak yönetilen etki alanına erişmek için DNS'yi yapılandırma | Microsoft Docs
description: LDAPS internet üzerinden kullanarak bir Azure AD Domain Services yönetilen etki alanına erişmek için DNS yapılandırma
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: a47f0f3e-2578-422a-a421-034f66de38f5
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: ergreenl
ms.openlocfilehash: 205e41727d9e6e360c34e7ac2a99b9ceec74361b
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54846175"
---
# <a name="configure-dns-to-access-an-azure-ad-domain-services-managed-domain-using-secure-ldap-ldaps"></a>Güvenli LDAP (LDAPS) kullanarak bir Azure AD Domain Services yönetilen etki alanına erişmek için DNS yapılandırma

## <a name="before-you-begin"></a>Başlamadan önce
Tam [görev 3: Azure portalını kullanarak yönetilen etki alanı için güvenli LDAP'yi etkinleştirme](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)

## <a name="task-4-configure-dns-to-access-the-managed-domain-from-the-internet"></a>Görev 4: Yönetilen etki alanı internet'ten erişmek için DNS yapılandırma
> [!TIP]
> **İsteğe bağlı görev** - planlıyor musunuz, internet üzerinden LDAPS'ı kullanarak yönetilen etki alanına erişmek için bu yapılandırma görevi atla.
>
>

Bu göreve başlamadan önce tam adımlar bölümünde özetlenen [görev 3](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md).

İnternet üzerinden güvenli LDAP erişimi etkinleştirdikten sonra istemci bilgisayarları Bu yönetilen etki alanı bulabilmesi için DNS güncelleştirmeniz gerekiyor. Dış IP adresi gördüğünüz **özellikleri** sekmesinde **dış IP adresi için LDAPS erişim**.

Bu dış IP adresine (örneğin, ' ldaps.contoso100.com') yönetilen etki alanı DNS adını işaret eden dış DNS sağlayıcınızda yapılandırın. Örneğin, aşağıdaki DNS girişini oluşturun:

    ldaps.contoso100.com  -> 52.165.38.113

İşte bu kadar! Artık internet üzerinden güvenli LDAP kullanarak yönetilen etki alanına bağlanmak hazırsınız.

> [!WARNING]
> İstemci bilgisayarları LDAPS'ı kullanarak yönetilen etki alanı başarıyla bağlanabilmesi için LDAPS sertifika verenin güvenmelidir unutmayın. Genel olarak güvenilir bir sertifika kullanıyorsanız, istemci bilgisayarlar bu sertifikayı verenler güven bu yana hiçbir şey yapmanız gerekmez. Kendinden imzalı bir sertifika kullanıyorsanız, otomatik olarak imzalanan sertifika ortak bölümünü istemci bilgisayarda güvenilen sertifika deposuna yükleyin.
>
>

## <a name="next-step"></a>Sonraki adım
[5. Görev: Yönetilen etki alanını bağlama ve güvenli LDAP erişimini kilitleme](active-directory-ds-ldaps-bind-lockdown.md)
