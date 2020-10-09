---
title: Azure Data Box Gateway sınırları | Microsoft Docs
description: Microsoft Azure Data Box Gateway için sistem sınırlarını ve önerilen boyutları açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/20/2019
ms.author: alkohli
ms.openlocfilehash: d160b0ead1ceb8c41bede20cb389a360d24258f9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "82561377"
---
# <a name="azure-data-box-gateway-limits"></a>Azure Data Box Gateway sınırları

Microsoft Azure Data Box Gateway çözümünüzü dağıtırken ve işletirken bu limitleri göz önünde bulundurun.

## <a name="data-box-gateway-service-limits"></a>Hizmet sınırlarını Data Box Gateway

[!INCLUDE [data-box-edge-gateway-service-limits](../../includes/data-box-edge-gateway-service-limits.md)]

## <a name="data-box-gateway-device-limits"></a>Cihaz sınırlarını Data Box Gateway

Aşağıdaki tabloda Data Box Gateway cihazının sınırları açıklanmaktadır.

| Açıklama | Değer |
|---|---|
|Hayır. cihaz başına dosya |100.000.000 <br> Eklenmekte olan her 25.000.000 dosya için (en fazla 100.000.000 ' de maksimum sınıra sahip), 2 TB disk alanı, 8 GB RAM ve 4 çekirdek CPU eklemeniz gerekir. |
|Hayır. cihaz başına paylaşım sayısı |24 |
|Hayır. Azure depolama kapsayıcısı başına paylaşım sayısı |1 |
|Bir paylaşıma yazılan en büyük dosya boyutu|2 TB 'lik bir sanal cihaz için maksimum dosya boyutu 500 GB 'tır. <br> En büyük dosya boyutu, en fazla 5 TB 'ye ulaşana kadar önceki orandaki veri diski boyutuyla artar. |

## <a name="azure-storage-limits"></a>Azure depolama sınırları

[!INCLUDE [data-box-edge-gateway-storage-limits](../../includes/data-box-edge-gateway-storage-limits.md)]

## <a name="data-upload-caveats"></a>Veri yükleme uyarıları

[!INCLUDE [data-box-edge-gateway-storage-data-upload-caveats](../../includes/data-box-edge-gateway-storage-data-upload-caveats.md)]

## <a name="azure-storage-account-size-and-object-size-limits"></a>Azure depolama hesabı boyutu ve nesne boyutu sınırları

[!INCLUDE [data-box-edge-gateway-storage-acct-limits](../../includes/data-box-edge-gateway-storage-acct-limits.md)]

## <a name="azure-object-size-limits"></a>Azure nesne boyutu sınırları

[!INCLUDE [data-box-edge-gateway-storage-object-limits](../../includes/data-box-edge-gateway-storage-object-limits.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Box Gateway'i dağıtmaya hazırlanma](data-box-gateway-deploy-prep.md)
