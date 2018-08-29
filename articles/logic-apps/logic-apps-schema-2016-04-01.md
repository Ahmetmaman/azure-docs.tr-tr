---
title: Şema güncelleştirmeleri Haziran-1-2016 - Azure Logic Apps | Microsoft Docs
description: Azure Logic apps'te mantıksal uygulama tanımları için güncelleştirilmiş bir şema sürümü 2016-06-01
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
ms.assetid: 349d57e8-f62b-4ec6-a92f-a6e0242d6c0e
ms.topic: article
ms.date: 07/25/2016
ms.openlocfilehash: 43fd52dd04e679b9756c07e8c6e260323469026a
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43126211"
---
# <a name="schema-updates-for-azure-logic-apps---june-1-2016"></a>Şema güncelleştirmeleri Azure Logic Apps için - 1 Haziran 2016

[Şema güncelleştirildi](https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json) ve Azure Logic Apps için API sürümü logic apps, daha güvenilir ve kullanmayı daha kolay hale anahtar geliştirmeleri içerir:

* [Kapsamları](#scopes) , Grup veya Eylemler eylemleri koleksiyonu olarak iç içe olanak tanır.
* [Koşullar ve döngüler](#conditions-loops) artık birinci sınıf eylemlerdir.
* Eylemler ile çalıştırmak için daha kesin sıralama `runAfter` özelliğini değiştirme `dependsOn`

Logic apps, 1 Haziran 2016 şeması, 1 Ağustos 2015 preview şema yükseltmek için [yükseltme bölümü denetleyin](#upgrade-your-schema).

<a name="scopes"></a>

## <a name="scopes"></a>Kapsamlar

Grup eylemleri birlikte ya da birbirlerine içinde iç içe eylemleri izin kapsamları, bu şema içerir. Örneğin, bir koşul başka bir koşul içerebilir. Daha fazla bilgi edinin [kapsam söz dizimi](../logic-apps/logic-apps-loops-and-scopes.md), veya bu temel kapsam örnek gözden geçirin:

```
{
    "actions": {
        "My_Scope": {
            "type": "scope",
            "actions": {                
                "Http": {
                    "inputs": {
                        "method": "GET",
                        "uri": "http://www.bing.com"
                    },
                    "runAfter": {},
                    "type": "Http"
                }
            }
        }
    }
}
```

<a name="conditions-loops"></a>

## <a name="conditions-and-loops-changes"></a>Koşullar ve döngüler değişiklikleri

Önceki şemada, tek bir eylemle ilişkili parametreler sürümleri, koşullar ve döngüler yoktu. Koşullar ve döngüler artık eylem türleri olarak görünmesi için bu sınırlama, bu şema kaldırıncaya. Daha fazla bilgi edinin [döngüler ve kapsamları](../logic-apps/logic-apps-loops-and-scopes.md), veya bu temel örnekte koşul eylemi için gözden geçirin:

```
{
    "If_trigger_is_some-trigger": {
        "type": "If",
        "expression": "@equals(triggerBody(), 'some-trigger')",
        "runAfter": { },
        "actions": {
            "Http_2": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.bing.com"
                },
                "runAfter": {},
                "type": "Http"
            }
        },
        "else": 
        {
            "if_trigger_is_another-trigger": "..."
        }      
    }
}
```

<a name="run-after"></a>

## <a name="runafter-property"></a>'runAfter' özelliği

`runAfter` Özelliğini değiştirir `dependsOn`, önceki eylem durumu eylemler için çalışma sırasını belirttiğinizde daha fazla duyarlık temelinde sağlanması.

`dependsOn` Özelliği "eylemi çalıştırılmadan ile başarılı oldu" eşanlamlı, önceki eylem başarılı olup şirket tabanlı bir eylemi çalıştırmak istediğiniz kaç kez başarısız oldu veya atlandı en önemli. `runAfter` Özelliği nesne sonra çalıştığı tüm eylem adları belirten bir nesne olarak bu esneklik sağlar. Bu özellik ayrıca bir dizi tetikleyici olarak kabul edilir durumları tanımlar. Ayrıca sonraki adım B başarılı veya başarısız bir adım başarılı olur ve sonra çalıştırmak istediyseniz, örneğin, bu işlevi `runAfter` özelliği:

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrade-your-schema"></a>Şemanızı yükseltme

Yükseltilecek [en son şema](https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json), yalnızca birkaç adımda yapması gerekmez. Yükseltme işlemi yükseltme betiği içeren yeni bir mantıksal uygulama kaydetme ve isterseniz, büyük olasılıkla önceki bir mantıksal uygulama üzerine.

1. Azure portalında mantıksal uygulamanızı açın.

2. Git **genel bakış**. Mantıksal uygulama araç çubuğunda **Şemayı Güncelleştir**.
   
    ![Şemayı Güncelleştir'i seçin][1]
   
    Yükseltilmiş tanım, kopyalama ve gerekirse, bir kaynak tanımı yapıştırın döndürülür. 
    Ancak biz **önemle tavsiye** seçtiğiniz **Kaydet** hiçbir bağlantı başvurusunda yükseltilmiş mantıksal uygulamayı geçerli olduğundan emin olmak için.

3. Yükseltme dikey penceresi araç çubuğunda seçin **Kaydet**.

4. Durum ve mantıksal adı girin. Yükseltilmiş mantıksal uygulamanızı dağıtmayı tercih **Oluştur**.

5. Yükseltilmiş mantıksal uygulamanızın beklendiği gibi çalıştığını onaylayın.
   
   > [!NOTE]
   > El ile veya istek tetikleyicisi kullanıyorsanız, yeni mantıksal uygulamanıza geri çağırma URL'sini değiştirir. Uçtan uca deneyim works emin olmak için yeni URL'yi test edin. Önceki URL'leri korumak için var olan mantıksal uygulamanız kopyalayabilirsiniz.

6. *İsteğe bağlı* araç çubuğunda, yeni şema sürümü ile önceki mantıksal uygulamanızı üzerine yazmayı tercih **kopya**yanındaki **Şemayı Güncelleştir**. Bu adım, yalnızca aynı kaynak kimliği veya mantıksal uygulamanızın tetikleyici URL'si isteğinde bulunmak istiyorsanız gereklidir.

### <a name="upgrade-tool-notes"></a>Aracı yükseltme notları

#### <a name="mapping-conditions"></a>Eşleme koşulları

Yükseltilen tanımında, true ve false dal eylemleri kapsamı olarak gruplamak en iyi araç haline getirir. Özellikle, Tasarımcı desenini `@equals(actions('a').status, 'Skipped')` olarak görünmesi gereken bir `else` eylem. Ancak, aracı tanınmayan desenleri algılarsa, araç hem true hem de false dal için ayrı koşulları oluşturabilirsiniz. Gerekirse yükselttikten sonra Eylemler eşleyebilirsiniz.

#### <a name="foreach-loop-with-condition"></a>Koşul ile 'foreach' döngüsü

Yeni şemayı desenini çoğaltmak için filtreleme eylemini kullanabilirsiniz bir `foreach` her öğesi, bir koşul ile döngüsü ancak yükseltme yaptığınızda bu değişikliği otomatik olarak gerçekleştirilmesi. Yalnızca koşulla eşleşen öğelerin bir dizisi döndürme için foreach döngüsü önce Filtre koşulu olur ve bu diziyi foreach eyleme geçirilir. Bir örnek için bkz. [döngüler ve kapsamları](../logic-apps/logic-apps-loops-and-scopes.md).

#### <a name="resource-tags"></a>Kaynak etiketleri

Yükseltmeden sonra yükseltilen iş akışını sıfırlamak gerekir böylece kaynak etiketleri kaldırılır.

## <a name="other-changes"></a>Diğer değişiklikler

### <a name="renamed-manual-trigger-to-request-trigger"></a>'Manual' tetikleyicisine 'request' tetikleyici olarak yeniden adlandırıldı

`manual` Tetikleyici türü kullanım dışı ve olarak yeniden adlandırıldı `request` türüyle `http`. Bu değişiklik desen türünü daha fazla tutarlılık oluşturan tetikleyicisi oluşturmak için kullanılır.

### <a name="new-filter-action"></a>Yeni 'filtresi' eylemi

Uzun bir diziye aşağı öğeleri, daha küçük bir kümesini filtrelemek için yeni `filter` türü bir dizi ve bir koşulu kabul eder, her öğe için koşulu değerlendirir ve koşulu karşılayan öğeleri ile bir dizi döndürür.

### <a name="restrictions-for-foreach-and-until-actions"></a>'Foreach' ve 'until' eylemleri kısıtlamaları

`foreach` Ve `until` döngü için tek bir eylem kısıtlı.

### <a name="new-trackedproperties-for-actions"></a>Eylemler için yeni 'trackedProperties'

Eylemler artık adlı ek bir özellik olan `trackedProperties`, eşdüzeye olduğu `runAfter` ve `type` özellikleri. Bu nesne, belirli bir eylem girişleri veya iş akışının bir parçası yayınlanan Azure tanılama telemetrisi dahil etmek istediğiniz çıkışları belirtir. Örneğin:

```
{                
    "Http": {
        "inputs": {
            "method": "GET",
            "uri": "http://www.bing.com"
        },
        "runAfter": {},
        "type": "Http",
        "trackedProperties": {
            "responseCode": "@action().outputs.statusCode",
            "uri": "@action().inputs.uri"
        }
    }
}
```

## <a name="next-steps"></a>Sonraki Adımlar
* [Logic apps için iş akışı tanımları oluşturma](../logic-apps/logic-apps-author-definitions.md)
* [Mantıksal uygulamalar için dağıtım şablonları oluşturma](../logic-apps/logic-apps-create-deploy-template.md)

<!-- Image references -->
[1]: ./media/logic-apps-schema-2016-04-01/upgradeButton.png
