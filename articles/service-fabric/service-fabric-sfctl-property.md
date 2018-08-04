---
title: Azure Service Fabric CLI - sfctl özelliği | Microsoft Docs
description: Service Fabric CLI'sını sfctl özelliği komutlarını açıklamaktadır.
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
ms.date: 07/31/2018
ms.author: bikang
ms.openlocfilehash: f24d273afa47466fe53b93d9c9c22bbcb1fc6959
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39493528"
---
# <a name="sfctl-property"></a>sfctl property
Service Fabric adları altında Store ve sorgu özellikleri.

## <a name="commands"></a>Komutlar

|Komut|Açıklama|
| --- | --- |
| delete | Belirtilen Service Fabric özelliği siler. |
| Al | Belirtilen Service Fabric özelliği alır. |
| liste | Verilen adla tüm Service Fabric özellikleri hakkında bilgi alır. |
| yerleştirme | Oluşturur veya bir Service Fabric özelliğini güncelleştirir. |

## <a name="sfctl-property-delete"></a>sfctl özelliği Sil
Belirtilen Service Fabric özelliği siler.

Belirtilen Service Fabric özelliği verilen adla siler. Bir özellik silinebilmesi için önce oluşturulması gerekir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --ad kimliği [gerekli] | Service Fabric adı, olmadan ' fabric\:' URI düzeni. |
| --[gerekli] özellik adı | Alınacak özelliğin adını belirtir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-property-get"></a>sfctl özelliğini Al
Belirtilen Service Fabric özelliği alır.

Verilen adla belirtilen Service Fabric özelliği alır. Bu her zaman hem değer hem de meta verileri döndürür.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --ad kimliği [gerekli] | Service Fabric adı, olmadan ' fabric\:' URI düzeni. |
| --[gerekli] özellik adı | Alınacak özelliğin adını belirtir. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-property-list"></a>sfctl özellik listesi
Verilen adla tüm Service Fabric özellikleri hakkında bilgi alır.

Bir Service Fabric adı özel bilgileri depolayan bir veya daha fazla adlandırılmış özelliklere sahip olabilir. Bu işlem, Disk bellekli bir listesine bu özellikler hakkında bilgi alır. Bilgi ada, değere ve özelliklerin her biri hakkında meta veriler içerir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --ad kimliği [gerekli] | Service Fabric adı, olmadan ' fabric\:' URI düzeni. |
| --devamlılık belirteci | Devamlılık belirteci parametresi, sonraki sonuç kümesini almak için kullanılır. Sistem sonuçlardan tek bir yanıtta uymayan bir devamlılık belirteci boş olmayan bir değer ile API yanıt olarak dahil edilir. Bu değer geçirilen zaman sonraki API çağrısı, API, sonraki sonuç kümesini döndürür. Daha fazla sonuç varsa, devamlılık belirteci bir değer içermiyor. Bu parametrenin değeri, URL kodlanmış olmamalıdır. |
| --dahil değerleri | Döndürülen özelliklerin değerlerini eklenip eklenmeyeceğini belirtmeye izin verir. Değerleri meta verileriyle döndürülmesi gerekiyorsa true; Yalnızca özellik meta verileri döndürmek için false. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |

## <a name="sfctl-property-put"></a>sfctl özelliğini put
Oluşturur veya bir Service Fabric özelliğini güncelleştirir.

Oluşturur veya belirli bir ada altında belirtilen Service Fabric özelliğini güncelleştirir.

### <a name="arguments"></a>Bağımsız Değişkenler

|Bağımsız değişken|Açıklama|
| --- | --- |
| --ad kimliği [gerekli] | Service Fabric adı, olmadan ' fabric\:' URI düzeni. |
| --[gerekli] özellik adı | Service Fabric özelliğin adı. |
| --Değer [gerekli] | Bir Service Fabric özellik değeri açıklar. Bir JSON dizesi budur. <br><br> Json dizesi verilerin 'Kind' ve 'Value' verilerin iki alan vardır. 'Kind' değeri bir JSON dizesinde görünmesi için ilk öğe olmalıdır ve 'İkili', 'Int64', 'Double', 'String' veya 'Guid' değerleri olabilir. Değeri serileştirmek-sağlanan türler için gerekir. Hem 'Kind' ve 'Veri' değerleri dize olarak sağlanmalıdır. |
| --Özel kimliği türü | Özelliğin özel tür kimliği. Bu özelliği kullanarak, kullanıcı özelliğinin değeri türü etiketi mümkün değil. |
| --zaman aşımı -t | Sunucu zaman aşımı saniye.  Varsayılan\: 60. |

### <a name="global-arguments"></a>Genel bağımsız değişkenleri

|Bağımsız değişken|Açıklama|
| --- | --- |
| --hata ayıklama | Tüm hata ayıklama günlüklerini göster için günlüğün ayrıntı düzeyini artırır. |
| ---h Yardım | Bu yardım iletisini ve çıkış gösterir. |
| --Çıktı -o | Çıkış biçimi.  İzin verilen değerler\: json, jsonc, tablo, tsv.  Varsayılan\: json. |
| --Sorgu | JMESPath sorgu dizesi. HTTP bkz\://jmespath.org/ daha fazla bilgi ve örnekler. |
| --verbose | Günlüğün ayrıntı düzeyini artırır. Kullanımı--tam hata ayıklama günlükleri için hata ayıklama. |


## <a name="next-steps"></a>Sonraki adımlar
- [Ayarlanan](service-fabric-cli.md) Service Fabric CLI.
- Service Fabric CLI kullanarak kullanmayı öğrenin [örnek betikleri](/azure/service-fabric/scripts/sfctl-upgrade-application).