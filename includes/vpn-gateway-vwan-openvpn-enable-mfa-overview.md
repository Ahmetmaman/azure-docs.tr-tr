---
title: dosya dahil etme
description: dosya dahil etme
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/14/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 3417bf0bd4ae1e0aa670f9fbfcc1fbbfeb372972
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "77471571"
---
Erişim vermeden önce kullanıcılardan ikinci bir kimlik doğrulama faktörü istenmesini istiyorsanız, Azure Multi-Factor Authentication (MFA) yapılandırabilirsiniz. MFA 'yı her kullanıcı için ayrı ayrı yapılandırabilir veya [koşullu erişim](../articles/active-directory/conditional-access/overview.md)aracılığıyla MFA özelliğinden yararlanabilirsiniz.

* Kullanıcı başına MFA, ek ücret ödemeden etkinleştirilebilir. Kullanıcı başına MFA etkinleştirilirken, kullanıcıdan Azure AD kiracısına bağlı tüm uygulamalarda ikinci faktör kimlik doğrulaması istenir. Adımlar için bkz. [1. Seçenekler](#peruser) .
* Koşullu erişim, ikinci bir faktörün nasıl yükseltilmesi gerektiği konusunda daha ayrıntılı denetim sağlar. MFA 'nın yalnızca VPN 'e atanmasına ve Azure AD kiracısına bağlı diğer uygulamaları dışlayacak şekilde izin verebilir. Adımlar için bkz. [2](#conditional) .