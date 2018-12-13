---
title: Azure Service Fabric CLI sfctl kafes hizmet | Microsoft Docs
description: Service Fabric CLI'sını sfctl kafes hizmet komutlarını açıklamaktadır.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: cli
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: 6c40cf2340568e704a5c76ad367c07c85826943b
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284804"
---
# <a name="sfctl-mesh-service"></a>sfctl ağ hizmeti
Hizmet ayrıntıları ve uygulama kaynağı listesi Hizmetleri alın.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| list | Tüm hizmet kaynakları listeler. |
| Show | Belirtilen ada sahip bir hizmet kaynağı alır. |

## <a name="sfctl-mesh-service-list"></a>sfctl kafes hizmet listesi
Tüm hizmet kaynakları listeler.

Uygulama kaynağı tüm hizmetleri hakkında bilgi alır. Bilgi, açıklama ve diğer hizmet özelliklerini içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| ---[gerekli] adı--uygulama adı | Uygulamanın adı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-mesh-service-show"></a>sfctl kafes hizmet Göster
Belirtilen ada sahip bir hizmet kaynağı alır.

Belirtilen ada sahip bir hizmet kaynağı hakkındaki bilgileri alır. Bilgi, açıklama ve diğer hizmet özelliklerini içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız Değişken|Açıklama|
| --- | --- |
| ---[gerekli] adı--uygulama adı | Uygulamanın adı. |
| --ad -n [gerekli] | Hizmetin adı. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız Değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |


## <a name="next-steps"></a>Sonraki adımlar
- [Ayarlanan](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application).