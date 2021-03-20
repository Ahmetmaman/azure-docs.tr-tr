---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/11/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: c0ce4e882f270f5e0c789a608aaada5c6c9cba92
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "67188230"
---
Bağlanmakta sorun yaşıyorsanız, aşağıdaki öğeleri kontrol edin:

- **Sertifika dışarı aktarma Sihirbazı** ile bir istemci sertifikası dışarı aktardıysanız, bunu bir. pfx dosyası olarak dışarı aktardığınızdan ve **Mümkünse sertifika yolundaki tüm sertifikaları** içerdiğinden emin olun. Bu değerle dışarı aktardığınızda, kök sertifika bilgileri de dışarı aktarılabilir. Sertifikayı istemci bilgisayara yükledikten sonra,. pfx dosyasındaki kök sertifika da yüklenir. Kök sertifikanın yüklendiğini doğrulamak için **Kullanıcı sertifikalarını Yönet** ' i açın ve **Güvenilen kök sertifika yetkilisi sertifikaları**' nı seçin. Kimlik doğrulamasının çalışması için mevcut olması gereken kök sertifikanın listelendiğini doğrulayın.

- Bir kuruluş CA çözümü tarafından verilen bir sertifika kullandıysanız ve kimlik doğrulayamıyorum, istemci sertifikasındaki kimlik doğrulama sırasını doğrulayın. İstemci sertifikasına çift tıklayarak, **Ayrıntılar** sekmesini seçerek ve ardından **Gelişmiş anahtar kullanımı**' nı seçerek kimlik doğrulama listesi sırasını kontrol edin. *Istemci kimlik doğrulamasının* listedeki ilk öğe olduğundan emin olun. Değilse, listedeki ilk öğe olarak *Istemci kimlik doğrulamasına* sahip Kullanıcı şablonuna dayalı bir istemci sertifikası verin.

- P2S hakkında ek sorun giderme bilgileri için bkz. [P2S bağlantılarının sorunlarını giderme](../articles/vpn-gateway/vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).