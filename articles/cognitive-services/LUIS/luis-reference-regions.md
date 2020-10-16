---
title: Yayımlama bölgeleri & uç noktaları-LUSıS
description: Azure portal belirtilen bölge, LUO uygulamasını yayımlayacağınız yer ve aynı bölge için bir uç nokta URL 'SI oluşturulur.
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 11/19/2019
ms.openlocfilehash: 134f71e957b39cf35b4e45e42c2101fbce233f05
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91277164"
---
# <a name="authoring-and-publishing-regions-and-the-associated-keys"></a>Yazma ve yayımlama bölgeleri ve ilişkili anahtarlar

Üç yazma bölgesi, karşılık gelen LUI portalları tarafından desteklenir. Bir LUIS uygulamasını birden fazla bölgeye yayımlamak için her bölgeye ait en az bir anahtara ihtiyacınız vardır.

<a name="luis-website"></a>

## <a name="luis-authoring-regions"></a>LUSıS yazma bölgeleri
Bölgeye göre üç LUı yazma portalı vardır. Aynı bölgede yazmanız ve yayımlamanız gerekir.

|LUIS|Yazma bölgesi|Azure bölge adı|
|--|--|--|
|[www.luis.ai][www.luis.ai] <br>[previous.luis.ai](https://previous.luis.ai)|ABD<br>Avrupa değil<br>Avustralya değil| `westus`|
|[au.luis.ai][au.luis.ai] <br>[previous.au.luis.ai](https://previous.au.luis.ai)|Avustralya| `australiaeast`|
|[eu.luis.ai][eu.luis.ai] <br>[previous.eu.luis.ai](https://previous.eu.luis.ai)|Avrupa|`westeurope`|

Yazma bölgeleri, [Yük devretme bölgelerini eşleştirilmiştir](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

<a name="regions-and-azure-resources"></a>

## <a name="publishing-regions-and-azure-resources"></a>Yayımlama bölgeleri ve Azure kaynakları
Uygulama, LUIS portalına eklenmiş olan LUIS kaynaklarıyla ilişkilendirilen tüm bölgelerde yayımlanır. Örneğin, [www.Luis.ai][www.luis.ai]üzerinde oluşturulan bir uygulama için, **WESTUS** 'de bir lusıs veya bilişsel hizmet kaynağı oluşturup [bunu bir kaynak olarak uygulamaya eklerseniz](luis-how-to-azure-subscription.md), uygulama o bölgede yayımlanır.

## <a name="public-apps"></a>Ortak uygulamalar
Ortak bir uygulama tüm bölgelerde yayımlanır, böylece bölge tabanlı bir LUTO kaynak anahtarı olan bir Kullanıcı, kaynak anahtarı ile ilişkili olan herhangi bir bölgede uygulamaya erişebilir.

<a name="publishing-regions"></a>

## <a name="publishing-regions-are-tied-to-authoring-regions"></a>Yayımlama bölgeleri yazma bölgelerine bağlıdır

Yazma bölgesi uygulaması yalnızca ilgili yayımlama bölgesine yayımlanabilir. Uygulamanız yanlış yazma bölgesinde bulunuyorsa dışarı aktarıp yayımlama bölgenize uygun yazma bölgesinde içeri aktarın.

Üzerinde oluşturulan LUSıS uygulamaları https://www.luis.ai [Avrupa](#publishing-to-europe) ve [Avustralya](#publishing-to-australia) bölgeleri hariç tüm uç noktalara yayımlanabilir.

## <a name="publishing-to-europe"></a>Avrupa 'da yayımlama

Avrupa bölgelerine yayımlamak için yalnızca LUSıS uygulamaları oluşturursunuz https://eu.luis.ai . Avrupa bölgesinde bir anahtar kullanarak başka bir yerde yayımlamayı denerseniz, Luz bir uyarı mesajı görüntüler. Bunun yerine, kullanın https://eu.luis.ai . Tarafından oluşturulan lusıs uygulamaları [https://eu.luis.ai][eu.luis.ai] diğer bölgelere otomatik olarak geçirilmez. ' İ dışa aktarıp, daha sonra geçirmek için LUO uygulamasını içeri aktarın.

## <a name="europe-publishing-regions"></a>Avrupa yayımlama bölgeleri

 Genel bölge | Yazma API bölgesi & yazma Web sitesi| Bölge & yayımlanıyor<br>`API region name`   |  Uç nokta URL biçimi   |
|-----|------|------|------|
| [Avrupa](#publishing-to-europe)| `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Orta Fransa<br>`francecentral`     | `https://francecentral.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| [Avrupa](#publishing-to-europe)| `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Kuzey Avrupa<br>`northeurope`     | `https://northeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| [Avrupa](#publishing-to-europe) | `westeurope`<br>[eu.luis.ai][eu.luis.ai]| West Europe<br>`westeurope`    |  `https://westeurope.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| [Avrupa](#publishing-to-europe) | `westeurope`<br>[eu.luis.ai][eu.luis.ai]| Güney Birleşik Krallık<br>`uksouth`    |  `https://uksouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |

## <a name="publishing-to-australia"></a>Avustralya 'da yayımlama

Avustralya bölgelerinde yayımlamak için yalnızca LUSıS uygulamaları oluşturursunuz https://au.luis.ai . Avustralya bölgesindeki bir anahtar kullanarak başka bir yerde yayımlamayı denerseniz, Luz bir uyarı mesajı görüntüler. Bunun yerine, kullanın https://au.luis.ai . Tarafından oluşturulan lusıs uygulamaları [https://au.luis.ai][au.luis.ai] diğer bölgelere otomatik olarak geçirilmez. ' İ dışa aktarıp, daha sonra geçirmek için LUO uygulamasını içeri aktarın.

## <a name="australia-publishing-regions"></a>Avustralya yayımlama bölgeleri

 Genel bölge | Yazma API bölgesi & yazma Web sitesi| Bölge & yayımlanıyor<br>`API region name`   |  Uç nokta URL biçimi   |
|-----|------|------|------|
| [Avustralya](#publishing-to-australia) | `australiaeast`<br>[au.luis.ai][au.luis.ai]| Doğu Avustralya<br>`australiaeast`     |  `https://australiaeast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |

## <a name="publishing-to-other-regions"></a>Diğer bölgelere yayımlama

Diğer bölgelere yayımlamak için yalnızca LUSıS uygulamaları oluşturursunuz [https://www.luis.ai](https://www.luis.ai) .

## <a name="other-publishing-regions"></a>Diğer yayımlama bölgeleri

 Genel bölge | Yazma API bölgesi & yazma Web sitesi| Bölge & yayımlanıyor<br>`API region name`   |  Uç nokta URL biçimi   |
|-----|------|------|------|
| Afrika | `westus`<br>[www.luis.ai][www.luis.ai]| Güney Afrika Kuzey<br>`southafricanorth` |  `https://southafricanorth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Asya | `westus`<br>[www.luis.ai][www.luis.ai]| Orta Hindistan<br>`centralindia` |  `https://centralindia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Asya | `westus`<br>[www.luis.ai][www.luis.ai]| Doğu Asya<br>`eastasia`     |  `https://eastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Asya | `westus`<br>[www.luis.ai][www.luis.ai]| Doğu Japonya<br>`japaneast`     |   `https://japaneast.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Asya | `westus`<br>[www.luis.ai][www.luis.ai]| Batı Japonya<br>`japanwest`     |   `https://japanwest.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Asya | `westus`<br>[www.luis.ai][www.luis.ai]| Güney Kore - Orta<br>`koreacentral`     |   `https://koreacentral.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Asya | `westus`<br>[www.luis.ai][www.luis.ai]| Güneydoğu Asya<br>`southeastasia`     |   `https://southeastasia.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Kuzey Amerika |`westus`<br>[www.luis.ai][www.luis.ai] | Orta Kanada<br>`canadacentral`     |   `https://canadacentral.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Kuzey Amerika |`westus`<br>[www.luis.ai][www.luis.ai] | Central US<br>`centralus`     |   `https://centralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Kuzey Amerika |`westus`<br>[www.luis.ai][www.luis.ai] | Doğu ABD<br>`eastus`      |  `https://eastus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Kuzey Amerika | `westus`<br>[www.luis.ai][www.luis.ai] | Doğu ABD 2<br>`eastus2`     |  `https://eastus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Kuzey Amerika | `westus`<br>[www.luis.ai][www.luis.ai] | Orta Kuzey ABD<br>`northcentralus`  |  `https://northcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Kuzey Amerika | `westus`<br>[www.luis.ai][www.luis.ai] | Orta Güney ABD<br>`southcentralus`  |  `https://southcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Kuzey Amerika |`westus`<br>[www.luis.ai][www.luis.ai] | Orta Batı ABD<br>`westcentralus`    |  `https://westcentralus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |
| Kuzey Amerika | `westus`<br>[www.luis.ai][www.luis.ai] | Batı ABD<br>`westus`  |   `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`  |
| Kuzey Amerika |`westus`<br>[www.luis.ai][www.luis.ai] | Batı ABD 2<br>`westus2`    |  `https://westus2.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`  |
| Güney Amerika | `westus`<br>[www.luis.ai][www.luis.ai] | Brezilya Güney<br>`brazilsouth`    |  `https://brazilsouth.api.cognitive.microsoft.com/luis/v2.0/apps/YOUR-APP-ID?subscription-key=YOUR-SUBSCRIPTION-KEY`   |

## <a name="endpoints"></a>Uç Noktalar

[Yazma ve tahmin uç noktaları](developer-reference-resource.md)hakkında daha fazla bilgi edinin.

## <a name="failover-regions"></a>Yük devretme bölgeleri

Her bölgenin yük devretmek için bir ikincil bölgesi vardır. Avrupa, Avrupa ve Avustralya 'Nın içinde devredilmekte ve Avustralya 'Nın içinde devredildi.

Yazma bölgeleri, [Yük devretme bölgelerini eşleştirilmiştir](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Önceden oluşturulmuş varlıklar başvurusu](./luis-reference-prebuilt-entities.md)

 [www.luis.ai]: https://www.luis.ai
 [au.luis.ai]: https://au.luis.ai
 [eu.luis.ai]: https://eu.luis.ai
