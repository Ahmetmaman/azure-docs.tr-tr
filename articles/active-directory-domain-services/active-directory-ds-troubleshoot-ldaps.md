---
title: 'Azure Active Directory Domain Services: Güvenli LDAP yapılandırma sorunlarını giderme | Microsoft Docs'
description: Güvenli LDAP için Azure AD Domain Services sorunlarını giderme
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: ''
editor: ''
ms.assetid: 81208c0b-8d41-4f65-be15-42119b1b5957
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: ergreenl
ms.openlocfilehash: e3a31749407f9ec0494e8452b602ed9966c5ab83
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39504218"
---
# <a name="azure-ad-domain-services---troubleshooting-secure-ldap-configuration"></a>Azure AD etki alanı Hizmetleri - sorun giderme güvenli LDAP yapılandırma

Bu makale için yaygın kullanılan raporlayabileceği ne zaman sorunları [güvenli LDAP yapılandırılıyor](active-directory-ds-admin-guide-configure-secure-ldap.md) Azure AD Domain Services için.

## <a name="aadds101-secure-ldap-network-security-group-configuration"></a>AADDS101: Güvenli LDAP ağ güvenlik grubu yapılandırma

**Uyarı iletisi:**

*İnternet üzerinden güvenli LDAP, yönetilen etki alanı için etkinleştirilir. Ancak 636 numaralı bağlantı noktasına erişim bir ağ güvenlik grubu kullanılarak kilitlenmemiş kilitli değil. Bu kullanıcı hesaplarını parola deneme yanılma saldırıları için yönetilen etki alanındaki getirebilir.*

### <a name="secure-ldap-port"></a>Güvenli LDAP bağlantı noktası

Güvenli LDAP etkin olduğunda, yalnızca belirli IP adreslerinden gelen LDAPS erişime izin vermek için ek kurallar oluşturmanızı öneririz. Bu kurallar, etki alanı güvenlik tehdidi doğuracak deneme yanılma saldırılarına karşı koruyun. Bağlantı noktası 636 yönetilen etki alanınıza erişim sağlar. Güvenli LDAP için erişime izin vermek için NSG güncelleştirmek nasıl aşağıda verilmiştir:

1. Gidin [ağ güvenlik grupları sekmesini](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) Azure portalında
2. Tablodan, etki alanı ile ilişkili NSG seçin.
3. Tıklayarak **gelen güvenlik kuralları**
4. Bağlantı noktası 636 kuralı oluşturma
   1. Tıklayın **Ekle** üst gezinti çubuğunda.
   2. Seçin **IP adresleri** kaynağı için.
   3. Bu kural için kaynak bağlantı noktası aralıkları belirtin.
   4. Hedef bağlantı noktası aralıkları için giriş "636".
   5. Protokolüdür **TCP**.
   6. Kural, bir uygun adını, açıklamasını ve öncelik verin. Varsa bu kuralın önceliğini "Reddet tüm" kuralının önceliği yüksek olmalıdır.
   7. **Tamam** düğmesine tıklayın.
5. Kural oluşturulduğunu doğrulayın.
6. Adımları doğru şekilde tamamladığınızdan emin olmak için iki saat içinde etki alanınızın sistem durumunu denetleyin.

> [!TIP]
> Bağlantı noktası 636 sorunsuz bir şekilde çalıştırmak Azure AD Domain Services için gereken yalnızca kural değil. Daha fazla bilgi için ziyaret [ağ yönergeleri](active-directory-ds-networking.md) veya [sorun giderme NSG yapılandırmasını](active-directory-ds-troubleshoot-nsg.md) makaleler.
>

## <a name="aadds502-secure-ldap-certificate-expiring"></a>AADDS502: LDAP sertifikanın sona ermesinden güvenli

**Uyarı iletisi:**

*Yönetilen etki alanı için güvenli LDAP sertifikasını [date] dolacak].*

**Çözüm:**

Özetlenen adımları izleyerek yeni bir güvenli LDAP sertifikası oluşturma [güvenli LDAP yapılandırma](active-directory-ds-admin-guide-configure-secure-ldap.md) makalesi.

## <a name="contact-us"></a>Bizimle iletişim kurun
Azure Active Directory Domain Services ürün ekibiyle [geri bildirim paylaşma veya destek](active-directory-ds-contact-us.md).
