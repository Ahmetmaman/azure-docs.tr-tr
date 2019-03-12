---
title: İstek sınırları ve azaltma - Azure Resource Manager
description: Abonelik sınırlarına ulaşıldı, Azure Resource Manager istekleri azaltma kullanmayı açıklar.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: e1047233-b8e4-4232-8919-3268d93a3824
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2019
ms.author: tomfitz
ms.custom: seodec18
ms.openlocfilehash: 91a776ba13ffaeeb4f8184371ae45a80d829ae46
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57550653"
---
# <a name="throttling-resource-manager-requests"></a>Resource Manager istekleri azaltma

Her Azure aboneliği ve Kiracı için Resource Manager kadar saat başına istek 12.000 okuma ve yazma istekleri saatte 1.200 sağlar. Bu limitler istekleri yapabilen sorumlu kimliği ve abonelik kimliği için kapsamlı veya Kiracı kimliği. İsteklerinizi birden fazla asıl kimliği geliyorsa, abonelik veya Kiracı genelinde sınırınızı 12.000 ve saatte 1.200 büyüktür.

İstekleri aboneliğiniz veya kiracınız için uygulanır. Abonelik isteği olanları aboneliğinizi geçirme involve kimliği, aboneliğinizdeki kaynak gruplarını almak gibi adı verilir. Kiracı isteklerini, geçerli Azure konumlarının alma gibi abonelik Kimliğinizi dahil değildir.

Bu limitler her bir Azure Resource Manager örneğine uygulayın. Her Azure bölgesi içinde birden çok örneği vardır ve Azure Resource Manager tüm Azure bölgelerine dağıtılır.  Uygulamada, sınırlar etkili bir şekilde bu sınırlardan daha yüksek olacak şekilde, kullanıcı olarak istekler genellikle birçok farklı örnekleri tarafından sunulur.

Bu limitler ulaşırsa, uygulama veya betik, istek kısıtlama gerekir. Bu makalede, sahip olduğunuz sınırına ulaşmadan önce kalan istekler belirleme ve sınırına ulaştınız, yanıt nasıl gösterir.

Sınıra ulaştığınızda, HTTP durum kodu alma **429 çok fazla istek**.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="remaining-requests"></a>Kalan istekler
Yanıt üst bilgilerini inceleyerek, kalan istek sayısını belirleyebilirsiniz. Okuma istekleri, kalan okuma isteklerinin sayısı için üst bilgisindeki bir değer döndürür. İstekler dahil kalan yazma isteklerinin sayısı'için bir değer yazın. Aşağıdaki tabloda, bu değerler için inceleyebilirsiniz yanıt üstbilgilerini açıklanmaktadır:

| Yanıt üst bilgisi | Açıklama |
| --- | --- |
| x-MS-ratelimit-Remaining-Subscription-Reads |Abonelik kapsamı, kalan okur. Bu değer, okuma işlemlerinin döndürülür. |
| x-MS-ratelimit-Remaining-Subscription-Writes |Abonelik kapsamı, kalan yazar. Bu değer, yazma işlemlerinin döndürülür. |
| x-MS-ratelimit-Remaining-tenant-Reads |Kalan kapsamlı kiracı okur |
| x-ms-ratelimit-remaining-tenant-writes |Kalan kapsamlı Kiracı yazar. |
| x-MS-ratelimit-Remaining-Subscription-Resource-Requests |Abonelik, kaynak türü istekleri kalan kapsamı.<br /><br />Bu üst bilgi değeri yalnızca bir hizmet varsayılan sınırı geçersiz kılınmış döndürülür. Resource Manager abonelik okuma ve yazma yerine bu değeri ekler. |
| x-MS-ratelimit-Remaining-Subscription-Resource-entities-Read |Abonelik, kaynak türü toplama isteklerini kalan kapsamı.<br /><br />Bu üst bilgi değeri yalnızca bir hizmet varsayılan sınırı geçersiz kılınmış döndürülür. Bu değer, kalan toplama isteklerini (liste kaynaklar) sayısını sağlar. |
| x-MS-ratelimit-Remaining-tenant-Resource-Requests |Kiracı kaynak türü istekleri kalan kapsamı.<br /><br />Kiracı düzeyinde istek için yalnızca bu üst bilgi eklenir ve bir hizmet, yalnızca varsayılan sınırı geçersiz kılınmış. Kaynak Yöneticisi Kiracı okuma veya yazma yerine bu değeri ekler. |
| x-MS-ratelimit-Remaining-tenant-Resource-entities-Read |Kiracı kaynak türü toplama isteklerini kalan kapsamı.<br /><br />Kiracı düzeyinde istek için yalnızca bu üst bilgi eklenir ve bir hizmet, yalnızca varsayılan sınırı geçersiz kılınmış. |

## <a name="retrieving-the-header-values"></a>Üstbilgi değerlerini alma
Bu kod veya betik üstbilgi değerlerini alma herhangi bir üst bilgi değeri almaktan farklı değildir. 

Örneğin, **C#**, üstbilgi değerini almak bir **HttpWebResponse** adlı nesne **yanıt** aşağıdaki kod ile:

```cs
response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)
```

İçinde **PowerShell**, üst bilgi değeri bir Invoke-WebRequest işlemden alın.

```powershell
$r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
$r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
```

Tam bir PowerShell örnek için bkz: [bir abonelik için Resource Manager sınırları denetle](https://github.com/Microsoft/csa-misc-utils/tree/master/psh-GetArmLimitsViaAPI).

Hata ayıklama için kalan istekler görmek istiyorsanız, sağlayabilir **-hata ayıklama** parametresi, **PowerShell** cmdlet'i.

```powershell
Get-AzResourceGroup -Debug
```

Şu yanıt değeri de dahil olmak üzere birçok değer döndürür:

```powershell
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
OK

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-reads: 11999
```

Yazma sınırları almak için bir yazma işlemi kullanın: 

```powershell
New-AzResourceGroup -Name myresourcegroup -Location westus -Debug
```

Aşağıdaki değerleri dahil olmak üzere birçok değer döndürür:

```powershell
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
Created

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-writes: 1199
```

İçinde **Azure CLI**, daha ayrıntılı seçeneğini kullanarak üstbilgi değeri alınamıyor.

```azurecli
az group list --verbose --debug
```

Aşağıdaki değerleri dahil olmak üzere birçok değer döndürür:

```azurecli
msrest.http_logger : Response status: 200
msrest.http_logger : Response headers:
msrest.http_logger :     'Cache-Control': 'no-cache'
msrest.http_logger :     'Pragma': 'no-cache'
msrest.http_logger :     'Content-Type': 'application/json; charset=utf-8'
msrest.http_logger :     'Content-Encoding': 'gzip'
msrest.http_logger :     'Expires': '-1'
msrest.http_logger :     'Vary': 'Accept-Encoding'
msrest.http_logger :     'x-ms-ratelimit-remaining-subscription-reads': '11998'
```

Yazma sınırları almak için bir yazma işlemi kullanın: 

```azurecli
az group create -n myresourcegroup --location westus --verbose --debug
```

Aşağıdaki değerleri dahil olmak üzere birçok değer döndürür:

```azurecli
msrest.http_logger : Response status: 201
msrest.http_logger : Response headers:
msrest.http_logger :     'Cache-Control': 'no-cache'
msrest.http_logger :     'Pragma': 'no-cache'
msrest.http_logger :     'Content-Length': '163'
msrest.http_logger :     'Content-Type': 'application/json; charset=utf-8'
msrest.http_logger :     'Expires': '-1'
msrest.http_logger :     'x-ms-ratelimit-remaining-subscription-writes': '1199'
```

## <a name="waiting-before-sending-next-request"></a>Bir sonraki istek göndermeden önce bekleniyor
Resource Manager istek sınırına ulaştığında döndürür **429** HTTP durum kodu ve **Retry-After** üst bilgisindeki değerle. **Retry-After** değeri sonraki isteği göndermeden önce uygulamanızı beklemesi gereken saniye (veya uyku) sayısını belirtir. Yeniden deneme değeri dolmadan isteği gönderirseniz, isteğiniz işlenir değil ve yeni bir yeniden deneme değeri döndürülür.

## <a name="next-steps"></a>Sonraki adımlar

* Tam bir PowerShell örnek için bkz: [bir abonelik için Resource Manager sınırları denetle](https://github.com/Microsoft/csa-misc-utils/tree/master/psh-GetArmLimitsViaAPI).
* Limitler ve kotalar hakkında daha fazla bilgi için bkz: [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md).
* Zaman uyumsuz REST istekleri işleme hakkında bilgi edinmek için [Azure zaman uyumsuz işlemleri izleme](resource-manager-async-operations.md).
