---
title: Azure geçişi bağlantı noktası ayarlarını | Microsoft Docs
description: Azure geçişi bağlantı noktası değerleri hakkında ayrıntılar.
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/26/2018
ms.author: spelluru
ms.openlocfilehash: 3ef08cfc94a029f97250578e9b0366a18770c809
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43696758"
---
# <a name="azure-relay-port-settings"></a>Azure geçişi bağlantı noktası ayarları

Aşağıdaki tabloda Azure geçişi için bağlantı noktası değerleri için gerekli yapılandırmayı açıklar.

## <a name="hybrid-connections"></a>Karma Bağlantılar

Karma bağlantılar WebSockets kullanan temel aktarma mekanizması, SSL ile 443 numaralı bağlantı noktasında kullanan **HTTPS** yalnızca. 

## <a name="wcf-relays"></a>WCF Geçişleri
  
|Bağlama|Aktarım güvenliği|Bağlantı noktası|  
|-------------|------------------------|----------|  
|[BasicHttpRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (istemci)|Evet|HTTPS| 
|" |Hayır|HTTP|  
|[BasicHttpRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (hizmet)|Ya da|9351/HTTP|  
|[NetEventRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (istemci)|Evet|9351/HTTPS|  
|" |Hayır|9350/HTTP|  
|[NetEventRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (hizmet)|Ya da|9351/HTTP|  
|[NetTcpRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.nettcprelaybinding) (istemci/hizmet)|Ya da|9352/5671/HTTP (9352/karma kullanıyorsanız 9353)|  
|[NetOnewayRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (istemci)|Evet|9351/HTTPS|  
|" |Hayır|9350/HTTP|  
|[NetOnewayRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (hizmet)|Ya da|9351/HTTP|  
|[WebHttpRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (istemci)|Evet|HTTPS|  
|" |Hayır|HTTP|  
|[WebHttpRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (hizmet)|Ya da|9351/HTTP|  
|[WS2007HttpRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (istemci)|Evet|HTTPS|  
|" |Hayır|HTTP|  
|[WS2007HttpRelayBinding sınıfı](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (hizmet)|Ya da|9351/HTTP|

## <a name="next-steps"></a>Sonraki adımlar
Azure geçişi hakkında daha fazla bilgi edinmek için şu bağlantıları ziyaret edin:
* [Azure Geçiş nedir?](relay-what-is-it.md)
* [Geçiş hakkında SSS](relay-faq.md)