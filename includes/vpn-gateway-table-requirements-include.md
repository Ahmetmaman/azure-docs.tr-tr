---
title: dosya dahil etme
description: dosya dahil etme
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: dc018b5d09c9b33c10cd2d54ac6572537e05ed25
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "67188258"
---
Aşağıdaki tabloda PolicyBased ve RouteBased VPN Gateway gereksinimleri listelenmektedir. Bu tablo hem Resource Manager, hem de klasik dağıtım modellerine uygulanır. Klasik modelde, PolicyBased VPN ağ geçitleri statik ağ geçitleriyle aynıdır ve rota tabanlı ağ geçitleri, dinamik ağ geçitleriyle aynıdır.

|  | **PolicyBased temel VPN Gateway** | **RouteBased temel VPN Gateway** | **RouteBased standart VPN Gateway** | **RouteBased yüksek performans VPN Gateway** |
| --- | --- | --- | --- | --- |
| **Siteden Siteye bağlantı   (S2S)** |PolicyBased VPN yapılandırması |RouteBased VPN yapılandırması |RouteBased VPN yapılandırması |RouteBased VPN yapılandırması |
| **Noktadan Siteye bağlantı (P2S**) |Desteklenmez |Destekleniyor (S2S ile birlikte var olabilir) |Destekleniyor (S2S ile birlikte var olabilir) |Destekleniyor (S2S ile birlikte var olabilir) |
| **Kimlik doğrulama yöntemi** |Önceden paylaşılan anahtar |S2S bağlantısı için önceden paylaşılan anahtar, P2S bağlantısı için sertifikalar |S2S bağlantısı için önceden paylaşılan anahtar, P2S bağlantısı için sertifikalar |S2S bağlantısı için önceden paylaşılan anahtar, P2S bağlantısı için sertifikalar |
| **S2S bağlantılarının en yüksek sayısı** |1 |10 |10 |30 |
| **P2S bağlantılarının en yüksek sayısı** |Desteklenmez |128 |128 |128 |
| **Etkin yönlendirme desteği (BGP)** (*) |Desteklenmez |Desteklenmez |Desteklenir |Desteklenir |

  (*) BGP, klasik dağıtım modeli için desteklenmiyor.
