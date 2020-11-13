---
title: Azure Izleyici uyarıları için ortak uyarı şeması
description: Ortak uyarı şemasını anlama, neden kullanılması gerekir ve nasıl etkinleştirilir?
ms.topic: conceptual
ms.subservice: alerts
ms.date: 03/14/2019
ms.openlocfilehash: aa619976c8fa03b925d66e884ad03fc4e385693e
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94565611"
---
# <a name="common-alert-schema"></a>Ortak uyarı şeması

Bu makalede ortak uyarı şemasının ne olduğu, kullanmanın avantajları ve nasıl etkinleştirileceği açıklanır.

## <a name="what-is-the-common-alert-schema"></a>Ortak uyarı şeması nedir?

Ortak uyarı şeması, bugün Azure 'daki uyarı bildirimleri için tüketim deneyimini standartlaştırır. Geçmişte, bugün Azure 'daki üç uyarı türü (ölçüm, günlük ve etkinlik günlüğü) kendi e-posta şablonlarına, Web kancası şemalarına vb. sahip. Ortak uyarı şeması ile, artık tutarlı bir şema ile uyarı bildirimleri alabilirsiniz.

Herhangi bir uyarı örneği, **etkilenen kaynağı** ve **uyarının nedenini** açıklar ve bu örnekler aşağıdaki bölümlerde ortak şemada açıklanmıştır:
* **Temel bilgiler** : uyarının diğer yaygın uyarı meta verileri (örneğin, önem derecesi veya açıklama) ile birlikte **hangi kaynakla** ilgili olduğunu açıklayan tüm uyarı türlerinde ortak olan **standartlaştırılmış alanlar** kümesi. 
* **Uyarı bağlamı** : Uyarı **türüne göre** değişen alanlarla, **uyarının nedenini** açıklayan bir alan kümesi. Örneğin, bir ölçüm uyarısında, uyarı bağlamındaki ölçüm adı ve ölçüm değeri gibi alanlar olabilir, ancak bir etkinlik günlüğü uyarısı uyarıyı oluşturan olay hakkında bilgi sahibi olur. 

Müşterilerden aldığımız tipik tümleştirme senaryoları, uyarı örneğinin, sorumlu ekibin üzerinde çalışmaya başladığı bir Özet (örneğin, kaynak grubu) temelinde ilgili takıma yönlendirilmesini içerir. Ortak uyarı şeması sayesinde, önemli alanları kullanarak uyarı türlerinde standartlaştırılmış yönlendirme mantığınızı kullanabilirsiniz ve bağlam alanlarını ilgili takımların daha fazla araştırmasını sağlar.

Diğer bir deyişle, _çok_ daha basit bir görevi yönetmek ve sürdürmek için daha az tümleştirmelere sahip olabilirsiniz. Ayrıca, gelecekteki uyarı yükü zenginleştirmelerinin (örneğin, özelleştirme, tanı zenginleştirme vb.) yalnızca ortak şemada çalışır.

## <a name="what-enhancements-does-the-common-alert-schema-bring"></a>Ortak uyarı şeması hangi geliştirmeleri getirir?

Ortak uyarı şeması öncelikle uyarı bildirimlerinde kendisini bildirir. Göreceğiniz geliştirmeler aşağıda listelenmiştir:

| Eylem | sekmesinden |
|:---|:---|
| SMS | Tüm Uyarı türleri için tutarlı bir SMS şablonu. |
| E-posta | Tutarlı ve ayrıntılı bir e-posta şablonu, sorunları bir bakışta kolayca tanılamanıza olanak sağlar. Katıştırılmış ayrıntılı-portalda uyarı örneğine ve etkilenen kaynağa yönelik bağlantılar, düzeltme sürecine hızlıca geçebilmenizi sağlar. |
| Web kancası/mantıksal uygulama/Azure Işlevi/Otomasyon Runbook 'U | Farklı Uyarı türleri genelinde kolayca tümleştirmeler oluşturmanıza olanak sağlayan tüm uyarı türleri için tutarlı bir JSON yapısı. |

Yeni şema aynı zamanda hem Azure portal hem de hemen gelecekte Azure mobil uygulaması daha zengin bir uyarı tüketim deneyimi sağlayacaktır. 

[Web kancaları/Logic Apps/Azure Işlevleri/Otomasyonu runbook 'Ları için şema tanımları hakkında daha fazla bilgi edinin.](./alerts-common-schema-definitions.md)

> [!NOTE]
> Aşağıdaki eylemler ortak uyarı şemasını desteklemez: ITSM Bağlayıcısı.

## <a name="how-do-i-enable-the-common-alert-schema"></a>Ortak uyarı şemasını etkinleştirmek Nasıl yaparım? istiyor musunuz?

Eylem grupları aracılığıyla, hem portalda hem de REST API aracılığıyla ortak uyarı şemasını kabul edebilir veya devre dışı bırakabilirsiniz. Yeni şemaya geçiş yapılacak geçiş, bir eylem düzeyinde bulunur. Örneğin, bir e-posta eylemi ve bir Web kancası eylemi için bağımsız olarak kabul etmeniz gerekir.

> [!NOTE]
> 1. Aşağıdaki uyarı türleri varsayılan olarak ortak şemayı destekler (zorunlu kabul etme gerekmez):
>     * Akıllı algılama uyarıları
> 1. Aşağıdaki uyarı türleri şu anda ortak şemayı desteklemiyor:
>     * [VM'ler için Azure izleyici](../insights/vminsights-overview.md) tarafından oluşturulan uyarılar
>     * [Azure maliyet yönetimi](../../cost-management-billing/manage/cost-management-budget-scenario.md) tarafından oluşturulan uyarılar

### <a name="through-the-azure-portal"></a>Azure portal aracılığıyla

![Ortak uyarı şeması kabul etme](media/alerts-common-schema/portal-opt-in.png)

1. Bir eylem grubunda varolan veya yeni bir eylemi açın. 
1. Gösterilen ortak uyarı şemasını etkinleştirmek için ' Evet ' seçeneğini belirleyin.

### <a name="through-the-action-groups-rest-api"></a>Eylem grupları aracılığıyla REST API

Ortak uyarı şemasını kabul etmek için [eylem GRUPLARı API](/rest/api/monitor/actiongroups) 'sini de kullanabilirsiniz. [Oluşturma veya güncelleştirme](/rest/api/monitor/actiongroups/createorupdate) REST API çağrısı yaparken, aşağıdaki eylemlerden herhangi biri Için "useCommonAlertSchema" bayrağını ' true ' olarak (kabul etmek için) veya ' false ' (devre dışı) olarak ayarlayabilirsiniz (e-posta/Web kancası/mantıksal uygulama/Azure işlevi/Otomasyon Runbook 'u).

Örneğin, [Create veya update](/rest/api/monitor/actiongroups/createorupdate) REST API için yapılan aşağıdaki istek gövdesi şunları yapacaktır:

* "John tikan 'un e-postası" e-posta eyleminin ortak uyarı şemasını etkinleştirin
* "Gamze Smith 'in e-postası" e-posta eyleminin ortak uyarı şemasını devre dışı bırakın
* Web kancası eylemi "örnek Web kancası" için ortak uyarı şemasını etkinleştirin

```json
{
  "properties": {
    "groupShortName": "sample",
    "enabled": true,
    "emailReceivers": [
      {
        "name": "John Doe's email",
        "emailAddress": "johndoe@email.com",
        "useCommonAlertSchema": true
      },
      {
        "name": "Jane Smith's email",
        "emailAddress": "janesmith@email.com",
        "useCommonAlertSchema": false
      }
    ],
    "smsReceivers": [
      {
        "name": "John Doe's mobile",
        "countryCode": "1",
        "phoneNumber": "1234567890"
      },
      {
        "name": "Jane Smith's mobile",
        "countryCode": "1",
        "phoneNumber": "0987654321"
      }
    ],
    "webhookReceivers": [
      {
        "name": "Sample webhook",
        "serviceUri": "http://www.example.com/webhook",
        "useCommonAlertSchema": true
      }
    ]
  },
  "location": "Global",
  "tags": {}
}
```





## <a name="next-steps"></a>Sonraki adımlar

- [Web kancaları/Logic Apps/Azure Işlevleri/Otomasyonu runbook 'Ları için ortak uyarı şeması tanımları.](./alerts-common-schema-definitions.md)
- [Tüm uyarılarınızı işlemek için ortak uyarı şemasıyla yararlanan bir mantıksal uygulama oluşturmayı öğrenin.](./alerts-common-schema-integrations.md)
