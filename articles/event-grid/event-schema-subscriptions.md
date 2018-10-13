---
title: Azure Event Grid aboneliği olay şeması
description: Abonelik olayları Azure Event Grid ile sağlanan özellikleri tanımlar
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: reference
ms.date: 10/12/2018
ms.author: tomfitz
ms.openlocfilehash: ae6513c503b930d9c953f5245a9c98ea096109bb
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2018
ms.locfileid: "49310247"
---
# <a name="azure-event-grid-event-schema-for-subscriptions"></a>Abonelikler için Azure Event Grid olay şeması

Bu makalede, Azure aboneliği olayları için şema ve özellikleri sağlar. Olay şemaları için bir giriş için bkz [Azure Event Grid olay şeması](event-schema.md).

Azure Abonelikleriniz ve kaynak grupları, aynı olay türleri gösterin. Olay türlerini, kaynak değişiklikleri veya Eylemler ilgilidir. Birincil olayları kaynak grubu içindeki kaynaklar için kaynak grubu yayma ve Azure abonelikleri abonelik kaynaklarla ilgili olayları yayma farktır.

Kaynak olayları için PUT, PATCH, GÖNDERİ oluşturulur ve silme işlemleri gönderilen `management.azure.com`. ALMA işlemleri, olayları oluşturmayın. İşlemler için veri düzlemi gönderilen (gibi `myaccount.blob.core.windows.net`) olayları oluşturmayın. Eylem olaylar, bir kaynak için anahtarları listeleme gibi işlemler için olay verilerini sağlar.

Bir Azure aboneliği için olaylara abone olduğunuzda, uç noktanızı Bu abonelik için tüm olayları alır. Olaylar, olay, bir sanal makine güncelleştiriliyor gibi görmek istediğiniz zamanda belki de yeni bir giriş dağıtım geçmişini yazma gibi sizin için önemli olmayan olaylar içerebilir. Tüm olayları, uç noktada almak ve kullanmak istediğiniz olayları işleyen kod yazın. Veya olay aboneliği oluştururken bir filtre ayarlayabilirsiniz.

Program aracılığıyla olayları işlemek için olayları bakarak sıralayabilirsiniz `operationName` değeri. Örneğin, olay uç noktanızı eşit olan işlemleri için olayları yalnızca işleyebilir `Microsoft.Compute/virtualMachines/write` veya `Microsoft.Storage/storageAccounts/write`.

Etkinlik konusu işlemin hedef kaynağın kaynak kimliğidir. Bir kaynak için olayları filtrelemek için bu kaynak sağlayan olay aboneliği oluştururken kimliği. Bir kaynak türüne göre filtre uygulamak için aşağıdaki biçimde bir değer kullanın: `/subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.Compute/virtualMachines`

Örnek betikler ve öğreticiler listesi için bkz: [Azure aboneliği olay kaynağı](event-sources.md#azure-subscriptions).

## <a name="available-event-types"></a>Kullanılabilir olay türleri

Bir VM oluşturulduğunda veya bir depolama hesabı silinmiş gibi azure abonelikleri Yönetimi olayları Azure Kaynak Yöneticisi'nden gösterin.

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Resources.ResourceActionCancel | Kaynak üzerinde işlem iptal edildiğinde oluşturulur. |
| Microsoft.Resources.ResourceActionFailure | Kaynak eylem başarısız olduğunda oluşturulur. |
| Microsoft.Resources.ResourceActionSuccess | Kaynak üzerinde işlem başarılı olduğunda oluşturulur. |
| Microsoft.Resources.ResourceDeleteCancel | Arandığında silme işlemi iptal edildi. Bu olay, bir şablon dağıtımı iptal edildiğinde gerçekleşir. |
| Microsoft.Resources.ResourceDeleteFailure | Arandığında delete işlemi başarısız oluyor. |
| Microsoft.Resources.ResourceDeleteSuccess | Arandığında silme işlemi başarılı olur. |
| Microsoft.Resources.ResourceWriteCancel | Arandığında oluşturma veya güncelleştirme işlemi iptal edildi. |
| Microsoft.Resources.ResourceWriteFailure | Arandığında oluşturma veya güncelleştirme işlemi başarısız olur. |
| Microsoft.Resources.ResourceWriteSuccess | Arandığında oluşturma veya güncelleştirme işlemi başarılı olur. |

## <a name="example-event"></a>Örnek olay

İçin şemayı aşağıdaki örnekte bir **ResourceWriteSuccess** olay. Aynı şemaya kullanılan **ResourceWriteFailure** ve **ResourceWriteCancel** olayları için farklı değerlerle `eventType`.

```json
[{
  "subject": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
  "eventType": "Microsoft.Resources.ResourceWriteSuccess",
  "eventTime": "2018-07-19T18:38:04.6117357Z",
  "id": "4db48cba-50a2-455a-93b4-de41a3b5b7f6",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
      "action": "Microsoft.Storage/storageAccounts/write",
      "evidence": {
        "role": "Subscription Admin"
      }
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "_claim_names": "{\"groups\":\"src1\"}",
      "_claim_sources": "{\"src1\":{\"endpoint\":\"{URI}\"}}",
      "http://schemas.microsoft.com/claims/authnclassreference": "1",
      "aio": "{token}",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "{ID}",
      "e_exp": "{expiration}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "{last-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "{first-name}",
      "ipaddr": "{IP-address}",
      "name": "{full-name}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "onprem_sid": "{ID}",
      "puid": "{ID}",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",
      "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "{user-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "{user-name}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "resourceProvider": "Microsoft.Storage",
    "resourceUri": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
    "operationName": "Microsoft.Storage/storageAccounts/write",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}"
}]
```

İçin şemayı aşağıdaki örnekte bir **ResourceDeleteSuccess** olay. Aynı şemaya kullanılan **ResourceDeleteFailure** ve **ResourceDeleteCancel** olayları için farklı değerlerle `eventType`.

```json
[{
  "subject": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
  "eventType": "Microsoft.Resources.ResourceDeleteSuccess",
  "eventTime": "2018-07-19T19:24:12.763881Z",
  "id": "19a69642-1aad-4a96-a5ab-8d05494513ce",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
      "action": "Microsoft.Storage/storageAccounts/delete",
      "evidence": {
        "role": "Subscription Admin"
      }
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "_claim_names": "{\"groups\":\"src1\"}",
      "_claim_sources": "{\"src1\":{\"endpoint\":\"{URI}\"}}",
      "http://schemas.microsoft.com/claims/authnclassreference": "1",
      "aio": "{token}",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "{ID}",
      "e_exp": "262800",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "{last-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "{first-name}",
      "ipaddr": "{IP-address}",
      "name": "{full-name}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "onprem_sid": "{ID}",
      "puid": "{ID}",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",
      "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "{user-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "{user-name}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "httpRequest": {
      "clientRequestId": "{ID}",
      "clientIpAddress": "{IP-address}",
      "method": "DELETE",
      "url": "https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2018-02-01"
    },
    "resourceProvider": "Microsoft.Storage",
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
    "operationName": "Microsoft.Storage/storageAccounts/delete",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}"
}]
```

İçin şemayı aşağıdaki örnekte bir **ResourceActionSuccess** olay. Aynı şemaya kullanılan **ResourceActionFailure** ve **ResourceActionCancel** olayları için farklı değerlerle `eventType`.

```json
[{   
  "subject": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{namespace}/AuthorizationRules/RootManageSharedAccessKey",
  "eventType": "Microsoft.Resources.ResourceActionSuccess",
  "eventTime": "2018-10-08T22:46:22.6022559Z",
  "id": "{ID}",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{namespace}/AuthorizationRules/RootManageSharedAccessKey",
      "action": "Microsoft.EventHub/namespaces/AuthorizationRules/listKeys/action",
      "evidence": {
        "role": "Contributor",
        "roleAssignmentScope": "/subscriptions/{subscription-id}",
        "roleAssignmentId": "{ID}",
        "roleDefinitionId": "{ID}",
        "principalId": "{ID}",
        "principalType": "ServicePrincipal"
      }     
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "aio": "{token}",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/identity/claims/identityprovider": "{URL}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",       "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "httpRequest": {
      "clientRequestId": "{ID}",
      "clientIpAddress": "{IP-address}",
      "method": "POST",
      "url": "https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{namespace}/AuthorizationRules/RootManageSharedAccessKey/listKeys?api-version=2017-04-01"
    },
    "resourceProvider": "Microsoft.EventHub",
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{namespace}/AuthorizationRules/RootManageSharedAccessKey",
    "operationName": "Microsoft.EventHub/namespaces/AuthorizationRules/listKeys/action",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}" 
}]
```

## <a name="event-properties"></a>Olay Özellikleri

Bir olay aşağıdaki üst düzey veri vardır:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| konu başlığı | dize | Olay kaynağı tam kaynak yolu. Bu alan, yazılabilir değil. Event Grid, bu değeri sağlar. |
| Konu | dize | Yayımcı tarafından tanımlanan olay konu yolu. |
| olay türü | dize | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | dize | Olayın oluşturulduğu zamandan, sağlayıcının UTC saatini temel alan. |
| id | dize | Olayın benzersiz tanımlayıcısı. |
| veri | object | Abonelik olay verileri. |
| dataVersion | dize | Veri nesnesinin şema sürümü. Yayımcı, şema sürümü tanımlar. |
| metadataVersion | dize | Olay meta verilerinin şema sürümü. Event Grid, şemanın en üst düzey özellikleri tanımlar. Event Grid, bu değeri sağlar. |

Veri nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Yetkilendirme | object | İşlem için istenen yetkilendirme. |
| Talep | object | Talep özellikleri. Daha fazla bilgi için [JWT belirtimi](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html). |
| correlationId | dize | Sorun giderme için bir işlem kimliği. |
| HTTP isteği | object | İşlem ayrıntıları. Bu nesne yalnızca olan mevcut bir kaynağı güncelleştirirken dahil ya da bir kaynak siliniyor. |
| ResourceProvider | dize | İşlemi kaynak sağlayıcı. |
| resourceUri | dize | İşlemi kaynak URI'si. |
| operationName | dize | Alınan işlemi. |
| durum | dize | İşlemin durumu. |
| subscriptionId | dize | Kaynak abonelik kimliği. |
| Kiracı kimliği | dize | Kaynak Kiracı kimliği. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Event grid'e giriş için bkz [Event Grid nedir?](overview.md).
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid aboneliği şema](subscription-creation-schema.md).
