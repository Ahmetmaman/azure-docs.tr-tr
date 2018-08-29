---
title: API geçişi Kılavuzu v1'den v2 | Microsoft Docs
titleSuffix: Azure
description: Bilgi geçiş son API'sine nasıl ayarlayın.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/01/2018
ms.author: diberry
ms.openlocfilehash: e7460a0eaf51e839accf08b6f8b07a112033af17
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43130551"
---
# <a name="api-v2-migration-guide"></a>API v2 Geçiş Kılavuzu
Sürüm 1 [uç nokta](https://aka.ms/v1-endpoint-api-docs) ve [yazma](https://aka.ms/v1-authoring-api-docs) API'leri kullanımdan kaldırılacaktır. 2. sürüme geçirme anlamak için bu kılavuzu kullanın [uç nokta](https://aka.ms/luis-endpoint-apis) ve [yazma](https://aka.ms/luis-authoring-apis) API'leri. 

## <a name="new-azure-regions"></a>Yeni Azure bölgeleri
LUIS sahip yeni [bölgeleri](https://aka.ms/LUIS-regions) LUIS API'leri için sağlanan. LUIS, bölge grupları için farklı bir Web sitesi sağlar. Uygulama sorgu beklediğiniz aynı bölgede yazılması gerekir. Uygulamaları bölgeleri otomatik olarak geçirilmez. Tek bir bölge sonra almak üzere başka bir yeni bölgede kullanılabilir olması için uygulamanın verilecek.

## <a name="authoring-route-changes"></a>Rota değişiklikleri yazma
Yazma API rota değiştirilen kullanımından **prog** kullanımına yol **API** rota.


| sürüm | yol |
|--|--|
|1|/luis/V1.0/**prog**/apps|
|2|/luis/**API**/v2.0/apps|


## <a name="endpoint-route-changes"></a>Uç nokta rota değişiklikleri
API uç noktası, farklı bir yanıt yanı sıra yeni querystring parametreleri vardır. Ayrıntılı bayrağı true ise, puanı, bağımsız olarak tüm hedefleri topScoringIntent yanı sıra bir ıntents adlı bir dizi döndürülür.

| sürüm | Rota Al |
|--|--|
|1|/luis/v1/Application? kimliği {AppID} = & q = {q}|
|2|/ luis/v2.0/apps/{appId}?q={q} [& timezoneOffset] [& ayrıntılı] [& Yazım denetimi] [& hazırlama] [& bing-yazım-onay-subscription-key] [& günlük]|


V1 uç noktası başarılı yanıt:
```JSON
{
  "odata.metadata":"https://dialogice.cloudapp.net/odata/$metadata#domain","value":[
    {
      "id":"bccb84ee-4bd6-4460-a340-0595b12db294","q":"turn on the camera","response":"[{\"intent\":\"OpenCamera\",\"score\":0.976928055},{\"intent\":\"None\",\"score\":0.0230718572}]"
    }
  ]
}
```

v2 uç noktası başarılı yanıt:
```JSON
{
  "query": "forward to frank 30 dollars through HSBC",
  "topScoringIntent": {
    "intent": "give",
    "score": 0.3964121
  },
  "entities": [
    {
      "entity": "30",
      "type": "builtin.number",
      "startIndex": 17,
      "endIndex": 18,
      "resolution": {
        "value": "30"
      }
    },
    {
      "entity": "frank",
      "type": "frank",
      "startIndex": 11,
      "endIndex": 15,
      "score": 0.935219169
    },
    {
      "entity": "30 dollars",
      "type": "builtin.currency",
      "startIndex": 17,
      "endIndex": 26,
      "resolution": {
        "unit": "Dollar",
        "value": "30"
      }
    },
    {
      "entity": "hsbc",
      "type": "Bank",
      "startIndex": 36,
      "endIndex": 39,
      "resolution": {
        "values": [
          "BankeName"
        ]
      }
    }
  ]
}
```

## <a name="key-management-no-longer-in-api"></a>Artık API anahtarı yönetimi
Abonelik uç noktası anahtarı API'leri bırakılmıştır 410 GONE döndürüyor.

| sürüm | yol |
|--|--|
|1|/luis/V1.0/prog/Subscriptions|
|1|/ luis/v1.0/prog/subscriptions/{subscriptionKey}|

Azure [uç nokta anahtarları](luis-how-to-azure-subscription.md) Azure portalında oluşturulur. Anahtara bir LUIS uygulaması üzerinde atadığınız **[Yayımla](luis-how-to-manage-keys.md)** sayfası. Gerçek bir anahtar değer kattığını bilmek gerekmez. LUIS, atama yapmak için abonelik adını kullanır. 

## <a name="new-versioning-route"></a>Yeni sürüm oluşturma yolu
V2 modeli şimdi bulunan bir [sürüm](luis-how-to-manage-versions.md). Sürüm 10 karakter rota adıdır. Varsayılan "0.1" sürümüdür.

| sürüm | yol |
|--|--|
|1|/luis/V1.0/**prog**/apps/ {AppID} / varlıklar|
|2|/luis/**API**/v2.0/apps/{appId}/**sürümleri**/ {VersionID} / varlıklar|

## <a name="metadata-renamed"></a>Meta veri olarak yeniden adlandırıldı
LUIS meta verileri döndürmek API'leri yeni adları vardır.

| V1 rota adı | v2 rota adı |
|--|--|
|PersonalAssistantApps |Yardımcıları|
|applicationcultures|kültürler|
|applicationdomains|etki alanları|
|applicationusagescenarios|usagescenarios|


## <a name="sample-renamed-to-suggest"></a>"Önermek için" "Örnek" olarak yeniden adlandırıldı
LUIS önerir mevcut konuşma [konuşma uç noktası](luis-how-to-review-endoint-utt.md) modeli geliştirmek. Önceki sürümde bu taşıyordu **örnek**. Yeni sürüm için örnek adı değiştirilirse **Öner**. Bu adlandırılır **[gözden geçirin, konuşma uç noktası](luis-how-to-review-endoint-utt.md)** LUIS Web sitesinde.

| sürüm | yol |
|--|--|
|1|/luis/V1.0/**prog**/apps/ {AppID} /entities/ {Entityıd} /**örnek**|
|1|/luis/V1.0/**prog**/apps/ {AppID} /intents/ {intentId} /**örnek**|
|2|/luis/**API**/v2.0/apps/{appId}/**sürümleri**/ {VersionID} /entities/ {Entityıd} /**önerin**|
|2|/luis/**API**/v2.0/apps/{appId}/**sürümleri**/ {VersionID} /intents/ {intentId} /**önerin**|


## <a name="create-app-from-prebuilt-domains"></a>Önceden oluşturulmuş etki alanlarından uygulaması oluşturma
[Önceden oluşturulmuş etki alanları](luis-how-to-use-prebuilt-domains.md) bir önceden tanımlanmış bir etki alanı modeli sağlar. Önceden oluşturulmuş etki alanları ortak etki alanları için LUIS uygulamanızı hızla geliştirmenizi sağlar. Bu API, önceden oluşturulmuş bir etki alanını temel alarak yeni bir uygulama oluşturmanıza olanak sağlar. Yeni AppID yanıttır.

|v2 yol|Fiili|
|--|--|
|/luis/api/v2.0/Apps/customprebuiltdomains  |GET, post|
|/ luis/api/v2.0/apps/customprebuiltdomains/{culture}  |Al|

## <a name="importing-1x-app-into-2x"></a>1.x uygulama 2.x alma
Dışarı aktarılan 1.x uygulamanın JSON içine aktarmadan önce değiştirmeniz gereken bazı alanlar vardır [LUIS] [ LUIS] 2.0. 

### <a name="prebuilt-entities"></a>Önceden oluşturulmuş varlıklar 
[Önceden oluşturulmuş varlıklarla](luis-prebuilt-entities.md) değiştirilmiştir. V2 kullandığınızdan emin olun önceden oluşturulmuş varlıklar. Bu kullanarak içerir [datetimeV2](luis-prebuilt-entities.md#use-a-prebuilt-datetimev2-entity), datetime yerine. 

### <a name="actions"></a>Eylemler
Actions özelliği artık geçerli değil. Boş olmalıdır 

### <a name="labeled-utterances"></a>Etiketli konuşma
V1 boşluk başlangıcında veya sonunda bir sözcük veya tümcecik içeren etiketli konuşma izin verilir. Boşluklar kaldırıldı. 

## <a name="common-reasons-for-http-response-status-codes"></a>HTTP yanıtı durum kodları için yaygın nedenler
Bkz: [LUIS API'si yanıt kodları](luis-reference-response-codes.md).

## <a name="next-steps"></a>Sonraki adımlar

Var olan geri KALAN güncelleştirilecek v2 API'si belgeleri çağırır LUIS için kullanım [uç nokta](https://aka.ms/luis-endpoint-apis) ve [yazma](https://aka.ms/luis-authoring-apis) API'leri. 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions