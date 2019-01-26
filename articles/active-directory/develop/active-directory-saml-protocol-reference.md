---
title: Azure AD, SAML protokolü kullanan nasıl | Microsoft Docs
description: Bu makalede, Azure Active Directory çoklu oturum açma ve çoklu oturum kapatma SAML profillerinde genel bir bakış sağlar.
services: active-directory
documentationcenter: .net
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 88125cfc-45c1-448b-9903-a629d8f31b01
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: hirsin
ms.openlocfilehash: ba87af33eead5155beb419914cd715dfba836fa9
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2019
ms.locfileid: "55077548"
---
# <a name="how-azure-ad-uses-the-saml-protocol"></a>Azure AD, SAML protokolünü nasıl kullanır?

Uygulamaların kullanıcılarına çoklu oturum açma deneyimini sağlamak üzere etkinleştirmek için azure Active Directory (Azure AD) kullanan SAML 2.0 protokolü. [Çoklu oturum açma](single-sign-on-saml-protocol.md) ve [çoklu oturum kapatmak](single-sign-out-saml-protocol.md) Azure AD'nin SAML profilleri SAML onaylamalarını, protokoller ve bağlamaları kimlik sağlayıcı hizmetine nasıl kullanıldığı açıklanmaktadır.

SAML protokolü, kimlik sağlayıcısı (Azure AD) ve hizmet sağlayıcısı (kendilerini hakkında bilgi alışverişi uygulaması) gerekir.

Bir uygulamayı Azure AD ile kaydettiğinizde, uygulama geliştiricisi Azure AD ile Federasyon ile ilgili bilgileri kaydeder. Bu bilgiler içerir **yeniden yönlendirme URI'si** ve **meta veri URI'sini** uygulama.

Azure AD kullanan bulut hizmetin **meta veri URI'sini** imzalama anahtarı ve oturum kapatma URI almanızı sağlar. Müşteri, uygulamada açabilir **Azure AD uygulama kaydı ->** ve ardından **ayarlar -> Özellikler**, bunlar oturum kapatma URL'si güncelleştirebilirsiniz. Bu şekilde Azure AD, doğru URL'yi yanıta gönderebilirsiniz. 

Azure Active Directory kiracıya özel ve ortak (Kiracı bağımsız) tek oturum açma ve çoklu oturum kapatma uç noktalarını kullanıma sunar. Bu URL'ler adreslenebilir konumlarını temsil--meta verileri okuyabilmesi için uç noktaya dönebilmesi yalnızca tanımlayıcıları--değildirler.

* Kiracıya özgü uç nokta şu konumdadır `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`. *<TenantDomainName>* Yer tutucusu kaydedilmiş bir etki alanı adı veya Azure AD kiracısı, Tenantıd GUID temsil eder. Örneğin, contoso.com kiracısını Federasyon meta verilerini şöyledir: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

* Kiracı bağımsız uç noktası şu konumdadır `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. Bu uç nokta adresini **ortak** Kiracı etki alanı adı veya kimliği yerine görüntülenir.

Azure AD yayımladığı Federasyon meta veri belgelerini hakkında daha fazla bilgi için bkz. [Federasyon meta verileri](azure-ad-federation-metadata.md).
