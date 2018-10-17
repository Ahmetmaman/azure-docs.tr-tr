---
title: Konuşmacı Tanıma API’si nedir?
titleSuffix: Azure Cognitive Services
description: Bilişsel Hizmetlerdeki Konuşmacı Tanıma API'si ile konuşmacı doğrulama ve konuşmacı belirleme için gelişmiş algoritmalar kullanın.
services: cognitive-services
author: dwlin
manager: cgronlun
ms.service: cognitive-services
ms.component: speaker-recognition
ms.topic: overview
ms.date: 10/01/2018
ms.author: dwlin
ms.openlocfilehash: c9193a51c677b327e7d799412e389467ac5cc1c0
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48016866"
---
# <a name="speaker-recognition-api"></a>Konuşmacı Tanıma API’si

Azure Bilişsel Hizmetler Konuşmacı Tanıma API'lerine hoş geldiniz. Konuşmacı Tanıma API’leri, konuşmacı doğrulama ve konuşmacı belirleme için en gelişmiş algoritmaları sağlayan bulut tabanlı API’lerdir. Konuşmacı Tanıma, iki kategoriye ayrılabilir: konuşmacı doğrulama ve konuşmacı belirleme.


## <a name="speaker-verification"></a>Konuşmacı Doğrulama

Ses, tıpkı parmak izi gibi bir kişiyi tanımlamak için kullanılabilen benzersiz özelliklere sahiptir.  Erişim denetimi ve kimlik doğrulama senaryoları için sinyal olarak ses kullanımı, yeni bir yenilikçi araç olarak ortaya çıkmış olup temelde müşteriler için kimlik doğrulaması deneyimini kolaylaştıran bir üst düzey güvenlik sunar.

Konuşmacı Doğrulama API’leri, ses veya konuşmalarını kullanarak otomatik şekilde kullanıcıları doğrulayabilir ve kullanıcıların kimliğini doğrulayabilir.

### <a name="enrollment"></a>Kayıt

Konuşmacı doğrulama için kayıt, metne bağımlıdır; başka bir deyişle, konuşmacıların hem kayıt hem de doğrulama aşamalarında kullanılacak belirli bir parolayı seçmesi gerekir.

Kayıt sırasında konuşmacının belirli bir tümceciği söylerken sesi kaydedilir daha sonra birçok özellik ayıklanır ve seçilen tümcecik tanınır. Ayıklanan özellikler ve seçilen tümcecik birlikte benzersiz bir ses imzasını oluşturur.

### <a name="verification"></a>Doğrulama

Doğrulama aşamasında bir giriş sesi ve tümcecik, kaydın ses imzası ve tümceciği ile karşılaştırılarak aynı kişiden olup olmadığı ve doğru tümceciğin söylenip söylenmediğini doğrulanır.

Konuşmacı doğrulama hakkında daha fazla ayrıntı için lütfen [Konuşmacı - Doğrulama](https://westus.dev.cognitive.microsoft.com/docs/services/563309b6778daf02acc0a508/operations/563309b7778daf06340c9652) API’sine bakın.

## <a name="speaker-identification"></a>Konuşmacı Belirleme

Konuşmacı Belirleme API’leri, bir grup olası konuşmacı grubu varken ses dosyasında konuşan kişiyi otomatik olarak belirleyebilir. Giriş sesi, sağlanan konuşmacı grubu ile eşlenir ve bir eşleşme bulunması durumunda, konuşmacının kimliği döndürülür.

Tüm konuşmacıların seslerinin sisteme kaydolması ve ses izlerinin oluşturulması için bir kayıt işleminden geçmesi gerekir.


### <a name="enrollment"></a>Kayıt

Konuşmacı belirleme için kayıt metinden bağımsızdır; başka bir deyişle, konuşmacının ses kaydında söyleyecekleri üzerinde bir kısıtlama yoktur. Konuşmacının sesi kaydedilir ve benzersiz bir ses imzası oluşturmak için birçok özellik ayıklanır.


### <a name="recognition"></a>Tanıma

Tanıma sırasında, olası konuşmacı grubu ile birlikte, bilinmeyen konuşmacının sesi de sağlanır. Sesin kime ait olduğunu belirlemek için giriş sesi, tüm konuşmacılarla karşılaştırılır ve bir eşleşme bulunursa konuşmacının kimliği döndürülür.

Konuşmacı belirleme hakkında daha fazla ayrıntı için lütfen [Konuşmacı - Belirleme](https://westus.dev.cognitive.microsoft.com/docs/services/563309b6778daf02acc0a508/operations/5645c068e597ed22ec38f42e) API’sine bakın.
