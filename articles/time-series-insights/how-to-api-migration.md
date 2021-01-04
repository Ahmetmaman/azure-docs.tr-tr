---
title: Yeni Azure Time Series Insights Gen2 API sürümlerine geçiş | Microsoft Docs
description: Yeni genel kullanıma sunulan sürümleri kullanmak için Azure Time Series Insights Gen2 ortamlarını güncelleştirme.
ms.service: time-series-insights
services: time-series-insights
author: shreyasharmamsft
ms.author: shresha
manager: dpalled
ms.workload: big-data
ms.topic: conceptual
ms.date: 10/01/2020
ms.custom: shresha
ms.openlocfilehash: 15f1a814b302611029cf6459b8d2df93a32a2d36
ms.sourcegitcommit: 44844a49afe8ed824a6812346f5bad8bc5455030
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/23/2020
ms.locfileid: "97740564"
---
# <a name="migrating-to-new-azure-time-series-insights-gen2-api-versions"></a>Yeni Azure Time Series Insights Gen2 API sürümlerine geçiş

## <a name="overview"></a>Genel Bakış

Ortak Önizlemedeki bir Azure Time Series Insights Gen2 ortamı oluşturduysanız (16 Temmuz 2020 ' den önce), bu makalede açıklanan adımları izleyerek, lütfen TSI ortamınızı, API 'lerin yeni genel sürümlerini kullanacak şekilde güncelleştirin. Bu değişiklik, Azure Time Series Insights Gen1 sürümünü kullanan kullanıcıları etkilemez.

> [!IMPORTANT]
> Bu makalede açıklanan güncelleştirmeler yalnızca, TSI ortamınızın kullandığı API sürümlerini yükseltir. Bu değişiklik, Gen2 ortamları için sunulan yeni [JSON düzleştirme ve kaçış kurallarından](./concepts-json-flattening-escaping-rules.md) bağımsız değildir.

Yeni API sürümü `2020-07-31` ve güncelleştirilmiş bir [zaman serisi ifade sözdizimi](/rest/api/time-series-insights/reference-time-series-expression-syntax)kullanır.

Kullanıcıların, API uç noktalarına çağrılar yapan ortam [zaman serisi model değişkenlerini](./concepts-variables.md), kayıtlı sorguları, Power BI sorguları ve özel araçları geçirmeli. Geçiş işlemiyle ilgili sorularınız veya endişeleriniz varsa Azure portal aracılığıyla bir destek bileti iletin ve bu belgeden bahsedin.

> [!IMPORTANT]
> Preview API sürümü `2018-11-01-preview` 31 ekim 2020 tarihine kadar desteklenmeye devam edecektir. Lütfen hizmette kesintiye uğramadan önce bu geçişin tüm ilgili adımlarını doldurun.

## <a name="migrate-time-series-model-and-saved-queries"></a>Zaman serisi modeli ve kaydedilen sorguları geçirme

Kullanıcıların [zaman serisi modeli değişkenlerini](./concepts-variables.md) ve kayıtlı sorguları geçirmelerini sağlamak için, [Azure Time Series Insights Gezgini](https://insights.timeseries.azure.com)aracılığıyla sunulan yerleşik bir araç vardır. Geçirmek istediğiniz ortama gidin ve aşağıdaki adımları izleyin. **Geçişi kısmen tamamlayabilir ve daha sonra tamamlanacak şekilde geri dönebilirsiniz, ancak güncelleştirmelerden hiçbiri geri alınamaz.**

> [!NOTE]
> Zaman serisi modelinde ve kayıtlı sorgularda güncelleştirme yapmak için ortama katkıda bulunan olmanız gerekir. Katkıda buluna değilseniz yalnızca kişisel kaydedilmiş sorgularınızı geçirebileceksiniz. Devam etmeden önce lütfen [ortam erişim ilkelerini](./concepts-access-policies.md) ve erişim düzeyinizi gözden geçirin.

1. Zaman serisi model değişkenleriniz ve kayıtlı sorgular tarafından kullanılan söz dizimini güncelleştirmek için gezgin tarafından istenir.

    [![İstem](media/api-migration/ux-prompt.png)](media/v2-update-overview/overview-one.png#lightbox)

    Bildirimi yanlışlıkla kapatırsanız, bildirim panelinde bulunabilir.

1. Geçiş aracını açmak için **güncelleştirmeleri göster** ' e tıklayın.

1. **Yükleme türleri**' ne tıklayın. Geçiş, değişken sözdizimini değiştirmek için geçerli türlerinizin üzerine yazacak olduğundan, geçerli türlerinizin bir kopyasını kaydetmeniz gerekir. Türler indirildiyse araç sizi bilgilendirir.

    [![İndirme türleri](media/api-migration/ux-migration-tool.png)](media/v2-update-overview/overview-one.png#lightbox)

1. **Değişkenleri Güncelleştir**' e tıklayın. Bu araç, değişkenler güncelleştirildiği zaman sizi bilgilendirir.

    > [!IMPORTANT]
    > Türlerinizi güncelleştirirseniz, daha eski bir API sürümü () kullanan özel uygulamaların `2018-11-01-preview` çalışmaya devam etmesi için yenı API sürümünü () kullanması gerekir `2020-07-31` . Hangi API sürümünün kullandığınızdan emin değilseniz, güncelleştirmeden önce yöneticinizle görüşün. Geçiş aracını kapatabilir ve bu adımlara daha sonra geri dönebilirsiniz. [Özel uygulamaları geçirme](#migrate-custom-applications)hakkında daha fazla bilgi edinin.

    [![Değişkenleri Güncelleştir](media/api-migration/ux-migration-tool-downloaded-types.png)](media/v2-update-overview/overview-one.png#lightbox)

1. **Kaydedilmiş sorguları Güncelleştir**' e tıklayın. Bu araç, Kaydedilen sorgular güncelleştirildiği zaman sizi bilgilendirir.

    [![Kaydedilmiş sorguları Güncelleştir](media/api-migration/ux-migration-tool-updated-variables.png)](media/v2-update-overview/overview-one.png#lightbox)

1. **Bitti**’ye tıklayın.

    [![Geçiş tamamlandı](media/api-migration/ux-migration-tool-updated-saved-queries.png)](media/v2-update-overview/overview-one.png#lightbox)

Yeni oluşturulan değişkenlerin ve kaydedilmiş sorguların bazılarını çizerek güncelleştirilmiş ortamınızı gözden geçirin. Grafik oluştururken beklenmedik bir davranış görürseniz lütfen gezgin 'deki geri bildirim aracını kullanarak bize geri bildirim gönderin.

## <a name="migrate-power-bi-queries"></a>Power BI sorgularını geçir

Power BI bağlayıcısını kullanarak sorgular oluşturduysanız, önizleme API 'SI sürümünü ve eski zaman serisi Ifadesi sözdizimini kullanarak Azure Time Series Insights çağrı yapırlar. Bu sorgular, önizleme API 'SI kullanım dışı olana kadar verileri başarıyla almaya devam edecektir.

Sorguları yeni API sürümünü ve yeni zaman serisi Ifade sözdizimini kullanacak şekilde güncelleştirmek için, sorguların Gezginden yeniden oluşturulması gerekir. [Power BI bağlayıcısını kullanarak sorgu oluşturma](./how-to-connect-power-bi.md)hakkında daha fazla bilgi edinin.

> [!NOTE]
> En azından Power BI Desktop Temmuz 2020 sürümünü kullanıyor olmanız gerekir. Bunu yapmazsanız, "geçersiz sorgu yük sürümü" hatası görebilirsiniz.

## <a name="migrate-custom-applications"></a>Özel uygulamaları geçirme

Özel uygulamanız aşağıdaki REST uç noktalarına çağrılar yapıyor ise, API sürümünü URI 'de ' ye güncelleştirmek yeterlidir `2020-07-31` :

- Zaman serisi modeli API 'Leri
  - Model ayarları API 'Leri
    - [Al](/rest/api/time-series-insights/dataaccessgen2/modelsettings/get)
    - [Güncelleştirme](/rest/api/time-series-insights/dataaccessgen2/modelsettings/update)
  - Örnek API 'Leri
    - [Tüm toplu Işlemler](/rest/api/time-series-insights/dataaccessgen2/timeseriesinstances/executebatch)
    - [Liste](/rest/api/time-series-insights/dataaccessgen2/timeseriesinstances/list)
    - [Ara](/rest/api/time-series-insights/dataaccessgen2/timeseriesinstances/search)
    - [Önerme](/rest/api/time-series-insights/dataaccessgen2/timeseriesinstances/suggest)
  - Hiyerarşi API 'Leri
    - [Tüm toplu Işlemler](/rest/api/time-series-insights/dataaccessgen2/timeserieshierarchies/executebatch)
    - [Liste](/rest/api/time-series-insights/dataaccessgen2/timeserieshierarchies/list)
  - Türler API 'Leri
    - [Silme, Işlemleri al](/rest/api/time-series-insights/dataaccessgen2/timeseriestypes/executebatch)
    - [Liste](/rest/api/time-series-insights/dataaccessgen2/timeseriestypes/list)

Aşağıdaki REST uç noktaları için, API sürümünü `2020-07-31` URI 'de olarak güncelleştirmeniz ve özelliğin tüm tekrarlarının `tsx` Güncelleştirilmiş [zaman serisi ifadesi sözdizimini](/rest/api/time-series-insights/reference-time-series-expression-syntax)kullanmasını sağlamak zorundasınız.

- Türler API 'Leri
  - [Put Işlemi](/rest/api/time-series-insights/dataaccessgen2/timeseriestypes/executebatch#typesbatchput)
- Sorgu API'leri
  - [GetEvents](/rest/api/time-series-insights/dataaccessgen2/query/execute#getevents)
  - [GetSeries](/rest/api/time-series-insights/dataaccessgen2/query/execute#getseries)
  - [GetAggregateSeries](/rest/api/time-series-insights/dataaccessgen2/query/execute#aggregateseries)

### <a name="examples"></a>Örnekler

#### <a name="typesbatchput"></a>TypesBatchPut

Eski Istek gövdesi (tarafından kullanılır `2018-11-01-preview` ):

```JSON
{
  "put": [
    {
      "id": "c1cb7a33-ed9b-4cf1-9958-f3162fed8ee8",
      "name": "OutdoorTemperatureSensor",
      "description": "This is an outdoor temperature sensor.",
      "variables": {
        "AverageTemperature": {
          "kind": "numeric",
          "value": {
            "tsx": "$event.[Temperature_Celsius].Double"
          },
          "filter": {
            "tsx": "$event.[Mode].String = 'outdoor'"
          },
          "aggregation": {
            "tsx": "avg($value)"
          }
        }
      }
    }
  ]
}
```

Güncelleştirilmiş Istek gövdesi (tarafından kullanılır `2020-07-31` ):

```JSON
{
  "put": [
    {
      "id": "c1cb7a33-ed9b-4cf1-9958-f3162fed8ee8",
      "name": "OutdoorTemperatureSensor",
      "description": "This is an outdoor temperature sensor.",
      "variables": {
        "AverageTemperature": {
          "kind": "numeric",
          "value": {
            "tsx": "$event['Temperature_Celsius'].Double"
          },
          "filter": {
            "tsx": "$event['Mode'].String = 'outdoor'"
          },
          "aggregation": {
            "tsx": "avg($value)"
          }
        }
      }
    }
  ]
}
```

Alternatif olarak, `filter` de olabilir `$event.Mode.String = 'outdoor'` . `value`Özel karakterden çıkmak için köşeli ayraçları kullanması gerekir ( `_` ).

#### <a name="getevents"></a>GetEvents

Eski Istek gövdesi (tarafından kullanılır `2018-11-01-preview` ):

```JSON
{
  "getEvents": {
    "timeSeriesId": [
      "006dfc2d-0324-4937-998c-d16f3b4f1952",
      "T1"
    ],
    "searchSpan": {
      "from": "2016-08-01T00:00:00Z",
      "to": "2016-08-01T00:16:50Z"
    },
    "filter": {
      "tsx": "($event.[Value].Double != null) OR ($event.[Status].String = 'Good')"
    },
    "projectedProperties": [
      {
        "name": "Temperature",
        "type": "Double"
      }
    ]
  }
}
```

Güncelleştirilmiş Istek gövdesi (tarafından kullanılır `2020-07-31` ):

```JSON
{
  "getEvents": {
    "timeSeriesId": [
      "006dfc2d-0324-4937-998c-d16f3b4f1952",
      "T1"
    ],
    "searchSpan": {
      "from": "2016-08-01T00:00:00Z",
      "to": "2016-08-01T00:16:50Z"
    },
    "filter": {
      "tsx": "($event.Value.Double != null) OR ($event.Status.String = 'Good')"
    },
    "projectedProperties": [
      {
        "name": "Temperature",
        "type": "Double"
      }
    ]
  }
}
```

Alternatif olarak, `filter` de olabilir `($event['Value'].Double != null) OR ($event['Status'].String = 'Good')` .

#### <a name="getseries"></a>GetSeries

Eski Istek gövdesi (tarafından kullanılır `2018-11-01-preview` ):

```JSON
{
  "getSeries": {
    "timeSeriesId": [
      "006dfc2d-0324-4937-998c-d16f3b4f1952"
    ],
    "searchSpan": {
      "from": "2016-08-01T00:00:00Z",
      "to": "2016-08-01T00:16:50Z"
    },
    "inlineVariables": {
      "pressure": {
        "kind": "numeric",
        "value": {
          "tsx": "$event.[Bar-Pressure-Offset]"
        },
        "aggregation": {
          "tsx": "avg($value)"
        }
      }
    },
    "projectedVariables": [
      "pressure"
    ]
  }
}
```

Güncelleştirilmiş Istek gövdesi (tarafından kullanılır `2020-07-31` ):

```JSON
{
  "getSeries": {
    "timeSeriesId": [
      "006dfc2d-0324-4937-998c-d16f3b4f1952"
    ],
    "searchSpan": {
      "from": "2016-08-01T00:00:00Z",
      "to": "2016-08-01T00:16:50Z"
    },
    "inlineVariables": {
      "pressure": {
        "kind": "numeric",
        "value": {
          "tsx": "$event['Bar-Pressure-Offset']"
        },
        "aggregation": {
          "tsx": "avg($value)"
        }
      }
    },
    "projectedVariables": [
      "pressure"
    ]
  }
}
```

Alternatif olarak, `value` de olabilir `$event['Bar-Pressure-Offset'].Double` . Veri türü belirtilmemişse, veri türü her zaman çift olarak kabul edilir. Ayraç gösteriminin özel karakteri () atlamak için kullanılması gerekir `-` .

#### <a name="aggregateseries"></a>AggregateSeries

Eski Istek gövdesi (tarafından kullanılır `2018-11-01-preview` ):

```JSON
{
  "aggregateSeries": {
    "timeSeriesId": [
      "006dfc2d-0324-4937-998c-d16f3b4f1952"
    ],
    "searchSpan": {
      "from": "2016-08-01T00:00:00Z",
      "to": "2016-08-01T00:16:50Z"
    },
    "interval": "PT1M",
    "inlineVariables": {
      "MinTemperature": {
        "kind": "numeric",
        "value": {
          "tsx": "coalesce($event.[Temp].Double, toDouble($event.[Temp].Long))"
        },
        "aggregation": {
          "tsx": "min($value)"
        }
      },
    },
    "projectedVariables": [
      "MinTemperature"
    ]
  }
}
```

Güncelleştirilmiş Istek gövdesi (tarafından kullanılır `2020-07-31` ):

```JSON
  "aggregateSeries": {
    "timeSeriesId": [
      "006dfc2d-0324-4937-998c-d16f3b4f1952"
    ],
    "searchSpan": {
      "from": "2016-08-01T00:00:00Z",
      "to": "2016-08-01T00:16:50Z"
    },
    "interval": "PT1M",
    "inlineVariables": {
      "MinTemperature": {
        "kind": "numeric",
        "value": {
          "tsx": "coalesce($event.Temp.Double, toDouble($event.Temp.Long))"
        },
        "aggregation": {
          "tsx": "min($value)"
        }
      },
    },
    "projectedVariables": [
      "MinTemperature"
    ]
  }
}
```

Alternatif olarak, `value` de olabilir `coalesce($event['Temp'].Double, toDouble($event['Temp'].Long))` .

### <a name="potential-errors"></a>Olası hatalar

#### <a name="invalidinput"></a>Invalidınput

Aşağıdaki hatayı görürseniz yeni API sürümünü () kullanıyorsunuz, `2020-07-31` ancak TSX sözdizimi güncelleştirilmedi. Lütfen yukarıdaki [zaman serisi Ifadesi sözdizimini](/rest/api/time-series-insights/reference-time-series-expression-syntax) ve geçiş örneklerini inceleyin. `tsx`API isteğini yeniden göndermeden önce tüm özelliklerin doğru güncelleştirildiğinden emin olun.

```JSON
{
    "error": {
        "code": "InvalidInput",
        "message": "Unable to parse 'value' time series expression (TSX) in variable 'Temperature'.",
        "target": "projectedVariables.Temperature.value",
        "innerError": {
            "parseErrorDetails": [
                {
                    "pos": [
                        0,
                        5
                    ],
                    "line": 1,
                    "msg": "Unsupported Time Series Expression version TSX01 used instead of TSX00.",
                    "code": "UnsupportedTSXVersionTSX01",
                    "target": "$event"
                }
            ],
            "code": "TsxParseError"
        }
    }
}
```

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure Time Series Insights gezgin](./concepts-ux-panels.md) veya özel uygulamanız aracılığıyla ortamınızı test edin.