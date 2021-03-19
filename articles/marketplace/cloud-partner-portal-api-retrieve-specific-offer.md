---
title: Belirli bir teklif API 'sini alma-Azure Marketi
description: Yayımcı ad alanı içinde belirtilen teklifi almak için API.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: mingshen-ms
ms.author: mingshen
ms.date: 07/14/2020
ms.openlocfilehash: 9f3ba6b2f13b9f2bb1d538db84723e3a9baaef12
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "87271850"
---
# <a name="retrieve-a-specific-offer"></a>Belirli bir teklifi alma

> [!NOTE]
> Bulut İş Ortağı Portalı API 'Leri ile tümleşiktir ve Iş Ortağı Merkezi 'nde çalışmaya devam edecektir. Geçiş küçük değişiklikler sunar. Iş Ortağı Merkezi 'ne geçtikten sonra kodunuzun çalışmaya devam ettiğinden emin olmak için [bulut iş ortağı PORTALı API başvurusunda](./cloud-partner-portal-api-overview.md) listelenen değişiklikleri gözden geçirin. CPP API 'Leri yalnızca Iş Ortağı Merkezi 'ne geçişten önce tümleştirilmiş mevcut ürünler için kullanılmalıdır; Yeni ürünlerin Iş Ortağı Merkezi gönderme API 'Leri kullanması gerekir.

Yayımcı ad alanı içinde belirtilen teklifi alır.  

Teklifin belirli bir sürümünü de alabilir veya teklifi taslak, görünüm veya üretim yuvalarında alabilirsiniz. Bir yuva belirtilmemişse, varsayılan olur `draft` . Önizlenmedi veya yayımlanmayan bir teklifi alma girişimi, bir `404 Not Found` hatayla sonuçlanır.

> [!WARNING]
> Gizli dizi türü alanları için gizli değerler bu API tarafından alınmayacak.

``` http
    GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>?api-version=2017-10-31

    GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/versions/<version>?api-version=2017-10-31

    GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/slot/<slotId>?api-version=2017-10-31

```

## <a name="uri-parameters"></a>URI parametreleri

| **Ad**    | **Açıklama**                                                                          | **Veri türü** |
|-------------|------------------------------------------------------------------------------------------|---------------|
| PublisherId | publisherID. Örneğin, contoso                                                        | Dize        |
| OfferId     | Teklifi benzersiz bir şekilde tanımlayan GUID.                                                 | Dize        |
| sürüm     | Alınan teklifin sürümü. Varsayılan olarak, en son teklif sürümü alınır. | Tamsayı       |
| SlotID      | Teklifin alınacağı yuva aşağıdakilerden biri olabilir:      <br/>  - `Draft` (varsayılan), şu anda taslak içinde olan teklif sürümünü alır.  <br/>  -  `Preview` Şu anda önizleme aşamasında olan teklif sürümünü alır.     <br/>  -  `Production` Şu anda üretimde olan teklif sürümünü alır.          |      enum |
| api-sürümü | En son API sürümü                                                                    | Tarih          |
|  |  |  |

## <a name="header"></a>Üst bilgi

|  **Ad**          |   **Değer**            |
|  ---------------   |  --------------        |
|  İçerik Türü      | `application/json`     |
|  Yetkilendirme     | `Bearer YOUR_TOKEN`    |
|  |  |

## <a name="body-example"></a>Gövde örneği

### <a name="response"></a>Yanıt

``` json
{
    "offerTypeId": "microsoft-azure-virtualmachines",
    "publisherId": "contoso",
    "status": "failed",
    "id": "059afc24-07de-4126-b004-4e42a51816fe",
    "version": 5,
    "definition": {
        "displayText": "Contoso Virtual Machine Offer",
        "offer": {
            "microsoft-azure-marketplace-testdrive.enabled": false,
            "microsoft-azure-marketplace-testdrive.videos": [],
            "microsoft-azure-marketplace.title": "Contoso App",
            "microsoft-azure-marketplace.summary": "Contoso App makes dev ops a breeze",
            "microsoft-azure-marketplace.longSummary": "Contoso App makes dev ops a breeze",
            "microsoft-azure-marketplace.description": "Contoso App makes dev ops a breeze",
            "microsoft-azure-marketplace.offerMarketingUrlIdentifier": "contosoapp",
            "microsoft-azure-marketplace.allowedSubscriptions": [
                "59160c40-2e25-4dcf-a2fd-6514cb08bf08"
            ],
            "microsoft-azure-marketplace.usefulLinks": [
            {
                "linkTitle": "Contoso App for Azure",
                "linkUrl": "https://azuremarketplace.microsoft.com"
            }
            ],
                "microsoft-azure-marketplace.categoryMap": [
                {
                "categoryL1": "analytics",
                "categoryL2-analytics": [
                "visualization-and-reporting"
                ]
                },
                {
                "categoryL1": "ai-plus-machine-learning",
                "categoryL2-ai-plus-machine-learning": [
                "bot-services",
                "cognitive-services",
                "other"
                ]
                }
            ],
            "microsoft-azure-marketplace.smallLogo": "https://publishingapistore.blob.core.windows.net/testcontent/D6191_publishers_contoso/contosovirtualmachine/6218c455-9cbc-450c-9920-f2e7a69ee132.png?sv=2014-02-14&sr=b&sig=6O8MM9dgiJ48VK0MwddkyVbprRAnBszyhVkVHGShhkI%3D&se=2019-03-28T19%3A46%3A50Z&sp=r",
            "microsoft-azure-marketplace.mediumLogo": "https://publishingapistore.blob.core.windows.net/testcontent/D6191_publishers_contoso/contosovirtualmachine/557e714b-2f31-4e12-b0cc-e48dd840edf4.png?sv=2014-02-14&sr=b&sig=NwL67NTQf9Gc9VScmZehtbHXpYmxhwZc2foy3o4xavs%3D&se=2019-03-28T19%3A46%3A49Z&sp=r",
            "microsoft-azure-marketplace.largeLogo": "https://publishingapistore.blob.core.windows.net/testcontent/D6191_publishers_contoso/contosovirtualmachine/142485da-784c-44cb-9523-d4f396446258.png?sv=2014-02-14&sr=b&sig=xaMxhwx%2FlKYfz33mJGIg8UBdVpsOwVvqhjTJ883o0iY%3D&se=2019-03-28T19%3A46%3A49Z&sp=r",
            "microsoft-azure-marketplace.wideLogo": "https://publishingapistore.blob.core.windows.net/testcontent/D6191_publishers_contoso/contosovirtualmachine/48af9013-1df7-4c94-8da8-4626e5039ce0.png?sv=2014-02-14&sr=b&sig=%2BnN7f2tprkrqb45ID6JlT01zXcy1PMTkWXtLKD6nfoE%3D&se=2019-03-28T19%3A46%3A49Z&sp=r",
            "microsoft-azure-marketplace.screenshots": [],
            "microsoft-azure-marketplace.videos": [],
            "microsoft-azure-marketplace.leadDestination": "None",
            "microsoft-azure-marketplace.tableLeadConfiguration": {},
            "microsoft-azure-marketplace.blobLeadConfiguration": {},
            "microsoft-azure-marketplace.salesForceLeadConfiguration": {},
            "microsoft-azure-marketplace.crmLeadConfiguration": {},
            "microsoft-azure-marketplace.httpsEndpointLeadConfiguration": {},
            "microsoft-azure-marketplace.marketoLeadConfiguration": {},
            "microsoft-azure-marketplace.privacyURL": "https://azuremarketplace.microsoft.com",
            "microsoft-azure-marketplace.termsOfUse": "Terms of use",
            "microsoft-azure-marketplace.engineeringContactName": "Jon Doe",
            "microsoft-azure-marketplace.engineeringContactEmail": "jondoe@outlook.com",
            "microsoft-azure-marketplace.engineeringContactPhone": "555-555-5555",
            "microsoft-azure-marketplace.supportContactName": "Jon Doe",
            "microsoft-azure-marketplace.supportContactEmail": "jondoe@outlook.com",
            "microsoft-azure-marketplace.supportContactPhone": "555-555-5555",
            "microsoft-azure-marketplace.publicAzureSupportUrl": "",
            "microsoft-azure-marketplace.fairfaxSupportUrl": ""
            },
            "plans": [
            {
                "planId": "contososkuidentifier",
                "microsoft-azure-virtualmachines.skuTitle": "Contoso App",
                "microsoft-azure-virtualmachines.skuSummary": "Contoso App makes dev ops a breeze.",
                "microsoft-azure-virtualmachines.skuDescription": "This is a description for the Contoso App that makes dev ops a breeze.",
                "microsoft-azure-virtualmachines.hideSKUForSolutionTemplate": false,
                "microsoft-azure-virtualmachines.cloudAvailability": [
                    "PublicAzure"
                ],
                "microsoft-azure-virtualmachines.certificationsFairfax": [],
                "virtualMachinePricing": {
                    "isByol": true,
                    "freeTrialDurationInMonths": 0
                },
                "microsoft-azure-virtualmachines.operatingSystemFamily": "Windows",
                "microsoft-azure-virtualmachines.windowsOSType": "Other",
                "microsoft-azure-virtualmachines.operationSystem": "Contoso App",
                "microsoft-azure-virtualmachines.recommendedVMSizes": [
                    "a0-basic",
                    "a0-standard",
                    "a1-basic",
                    "a1-standard",
                    "a2-basic",
                    "a2-standard"
                ],
                "microsoft-azure-virtualmachines.openPorts": [],
                "microsoft-azure-virtualmachines.vmImages": {
                    "1.0.1": {
                    "osVhdUrl": "http://contosoteststorage.blob.core.windows.net/test/contosoVM.vhd?sv=2014-02-14&sig=WlDo6Q4xwYH%2B5QEJbItPUVdgHhBcrVxPBmntZ2vU96w%3D&st=2016-06-25T18%3A30%3A00Z&se=2017-06-25T18%3A30%3A00Z&sp=rl",
                    "lunVhdDetails": []
                    }
                },
                "regions": [
                    "DZ",
                    "AR"
                ]
            }
            ]
        },
        "changedTime": "2017-06-07T06:15:39.7349221Z"
    }
}
```

### <a name="response-body-properties"></a>Yanıt gövdesi özellikleri

|  **Ad**       |   **Açıklama**                                                                                                               |
|  -------------  |   -----------------------------------------------------------------------------------------------------                         |
|  Offertypeıd    | Teklifin türünü tanımlar                                                                                                    |
|  PublisherId    | Yayımcının benzersiz tanımlayıcısı                                                                                              |
|  durum         | Teklifin durumu. Olası değerler listesi için bkz. [teklif durumu](#offer-status) aşağıdaki.                                  |
|  Id             | Teklifi benzersiz bir şekilde tanımlayan GUID                                                                                         |
|  sürüm        | Teklifin geçerli sürümü. Sürüm özelliği istemci tarafından değiştirilemiyor. Her yayımladıktan sonra artırılır.    |
|  tanım     | İş yükünün gerçek tanımı                                                                                               |
|  changedTime    | Teklifin en son değiştirildiği UTC tarih saati                                                                                   |
|  |  |

### <a name="response-status-codes"></a>Yanıt durum kodları

| **Kod**  | **Açıklama**                                                                                                                 |
|  ------   | ------------------------------------------------------------------------------------------------------------------------------- |
|  200      | `OK` -İstek başarıyla işlendi ve yayımcının altındaki tüm teklifler istemciye geri döndü.               |
|  400      | `Bad/Malformed request` -Hata yanıtı gövdesinde daha fazla bilgi bulunabilir.                                                 |
|  403      | `Forbidden` -İstemcinin belirtilen ad alanına erişimi yok.                                                        |
|  404      | `Not found` -Belirtilen varlık yok. İstemci, PublisherId, OfferId ve sürümü (belirtilmişse) denetlemelidir.      |
|  |  |

### <a name="offer-status"></a>Teklif durumu

|  **Ad**                   |   **Açıklama**                             |
| --------------------------- |  -------------------------------------------- |
|  Neveryayınlandı             | Teklif hiç yayımlanmadı.               |
|  NotStarted                 | Teklif yenidir ancak başlatılmamış.              |
|  WaitingForPublisherReview  | Teklif, yayımcı onayını bekliyor.      |
|  Çalışma                    | Teklif gönderimi işleniyor.          |
|  Başarılı                  | Teklif gönderimi işlemeyi tamamladı.    |
|  İptal edildi                   | Teklif Gönderimi iptal edildi.                |
|  Başarısız                     | Teklif gönderimi başarısız oldu.                      |
|  |  |
