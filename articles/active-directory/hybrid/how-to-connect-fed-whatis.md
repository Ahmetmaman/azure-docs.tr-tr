---
title: Azure AD Connect ve Federasyon | Microsoft Docs
description: Bu sayfa, Azure AD Connect kullanan AD FS işlemleri ile ilgili tüm belgeler için merkezi konumdur.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: f9107cf5-0131-499a-9edf-616bf3afef4d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/09/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 54aab6e9718b5a4a55cf2a3f9c472e9e02a53ea7
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56191087"
---
# <a name="azure-ad-connect-and-federation"></a>Azure AD Connect ve federasyon
Azure Active Directory (Azure AD) Connect sağlar ile Federasyonu yapılandırma şirket içi Active Directory Federasyon Hizmetleri (AD FS) ve Azure AD. Federasyon oturum açma ile kullanıcılar parolalarını yeniden girmek zorunda kalmadan şirket ağ üzerindeyken, Azure AD tabanlı hizmetler ile şirket içi parolalarını--ve için oturum açabilir etkinleştirebilirsiniz. AD FS ile Federasyon seçeneğini kullanarak yeni bir AD FS yüklemesini dağıtabilir veya bir Windows Server 2012 R2 grubunda var olan bir yüklemesini belirtebilirsiniz.

Bu konu Azure AD Connect'i Federasyon ile ilgili işlevler hakkında bilgi için platformdur. Tüm ilgili konulara bağlantılar listeler. Azure AD Connect bağlantıları için bkz [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md).

## <a name="azure-ad-connect-federation-topics"></a>Azure AD Connect: Federasyon konuları
| Konu | Neleri kapsar ve okumak ne zaman |
|:--- |:--- |
| **Azure AD Connect kullanıcı oturumu açma seçenekleri** | |
| [Kullanıcı oturum açma seçeneklerini anlama](plan-connect-user-signin.md) |Azure oturum açma kullanıcı deneyimini nasıl etkilediklerini ve çeşitli kullanıcı oturum açma seçenekleri hakkında bilgi edinin. |
| **Azure AD Connect kullanarak AD FS'yi yükleyin** | |
| [Önkoşullar](how-to-connect-install-custom.md#ad-fs-configuration-pre-requisites) |Azure AD Connect aracılığıyla başarılı bir AD FS yükleme önkoşullarını bakın. |
| [AD FS grubu Yapılandır](how-to-connect-install-custom.md#configuring-federation-with-ad-fs) |Yeni bir AD FS grubu, Azure AD Connect kullanarak yükleyin. |
| [Alternatif bir oturum açma Kimliğini kullanarak Azure AD ile federasyona ](how-to-connect-fed-management.md#alternateid) | Alternatif oturum açma kimliği kullanarak Federasyonu yapılandırma  |
| **AD FS yapılandırmasını değiştirme** | |
| [Güveni onarın](how-to-connect-fed-management.md#repairthetrust) |Onarım geçerli güven arasında AD FS ve Office 365/Azure şirket içi. |
| [Yeni bir AD FS Sunucusu Ekle](how-to-connect-fed-management.md#addadfsserver) |İlk yüklemeden sonra ek AD FS sunucusu ile bir AD FS grubunu genişletin. |
| [Yeni bir AD FS WAP sunucusu ekleme](how-to-connect-fed-management.md#addwapserver) |İlk yüklemeden sonra ek bir Web uygulaması Ara sunucusu (WAP) sunucusu ile bir AD FS grubunu genişletin. |
| [Yeni bir Federasyon etki alanına ekleme](how-to-connect-fed-management.md#addfeddomain) |Azure AD ile federasyona eklenmesi için başka bir etki alanına ekleyin. |
| [SSL sertifikasını güncelleştirme](how-to-connect-fed-ssl-update.md)| Bir AD FS grubu için SSL sertifikasını güncelleştirin. |
| [Office 365 ve Azure AD için Federasyon sertifikalarını yenileme](how-to-connect-fed-o365-certs.md)|Azure AD ile O365 sertifikanızı yenileyin.|
| **Başka bir Federasyon yapılandırma** | |
| [Azure AD’nin birden çok örneğini tek bir AD FS örneği ile birleştirme](how-to-connect-fed-single-adfs-multitenant-federation.md) | Birden çok Azure AD'yi tek bir AD FS grubunu ile federasyona eklemek| 
| [Bir özel şirket logosu/gösterim Ekle](how-to-connect-fed-management.md#customlogo) |Oturum açma deneyimi, AD FS oturum açma sayfasında gösterilen özel logo belirterek değiştirin. |
| [Oturum açma bir açıklama ekleyin](how-to-connect-fed-management.md#addsignindescription) |AD FS oturum açma sayfasında oturum açma açıklamayı değiştirin. |
| [AD FS talep kurallarını değiştirme](how-to-connect-fed-management.md#modclaims) |Değiştirin veya talep kuralları karşılık gelen AD FS için Azure AD Connect eşitleme yapılandırmasını ekleyin. |


## <a name="additional-resources"></a>Ek kaynaklar
* [Federasyon iki Azure AD'yi tek AD FS ile](how-to-connect-fed-single-adfs-multitenant-federation.md)
* [Azure'da AD FS dağıtımı](how-to-connect-fed-azure-adfs.md)
* [Azure Traffic Manager ile azure'da yüksek kullanılabilirlik çapraz coğrafi AD FS dağıtımı](../active-directory-adfs-in-azure-with-azure-traffic-manager.md)
