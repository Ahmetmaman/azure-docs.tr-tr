---
title: Akademik bilgi API'si ilişkisi varlık öznitelikleri
titlesuffix: Azure Cognitive Services
description: Akademik bilgi API'si ilişkisi varlığı ile kullanabileceğiniz öznitelikleri hakkında bilgi edinin.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 82e6a5b66342e58e62da029d617cbd1d74c28149
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55864375"
---
# <a name="affiliation-entity"></a>Bağlantı varlığı

<sub> * Şu öznitelikleri ilişkisi varlığa özgüdür. (Ty = '5') </sub>

Ad    |Açıklama                            |Type       | İşlemler
------- | ------------------------------------- | --------- | ----------------------------
Kimlik      |Varlık Kimliği                              |Int64      |Eşittir
AfN     |Normalleştirilmiş ilişkisi adı        |String     |Eşittir
DAfN    |İlişki görünen adı       |String     |yok
BİLGİ      |İlişki alıntı toplam sayısı           |Int32      |yok  
ECC     |İlişki toplam tahmini alıntı sayısı |Int32      |yok

## <a name="extended-metadata-attributes"></a>Genişletilmiş meta veri öznitelikleri ##

Ad    | Açıklama               
--------|---------------------------    
PC      |Bağlantı'nın kağıt sayısı
