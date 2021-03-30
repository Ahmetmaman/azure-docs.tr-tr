---
title: Azure API Management ürün şablonları | Microsoft Docs
description: Azure API Management Geliştirici Portalında ürün sayfalarının içeriğini özelleştirmeyi öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 49f9254c-4c5f-4ed4-9c8d-798f44e805ee
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/04/2019
ms.author: apimpm
ms.openlocfilehash: 4c8cd4aa3e91c5d69c40e47683818ed8bc9be338
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "86249912"
---
# <a name="product-templates-in-azure-api-management"></a>Azure API Management ürün şablonları

Azure API Management, içeriğini yapılandıran bir dizi şablon kullanarak geliştirici portalı sayfalarının içeriğini özelleştirmenizi sağlar. Bu şablonları kullanarak uygun gördüğünüz şekilde, sayfanın içeriğini yapılandırmak için [dotsıvı](http://dotliquidmarkup.org/) Syntax ve seçtiğiniz düzenleyiciyi ( [Örneğin, tasarımcılar için dotlikit](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers)) ve sağlanan bir yerelleştirilmiş [dize kaynakları](api-management-template-resources.md#strings), [glif kaynakları](api-management-template-resources.md#glyphs)ve [sayfa denetimleri](api-management-page-controls.md)ile birlikte kullanabilirsiniz.  
  
 Bu bölümdeki şablonlar, geliştirici portalındaki ürün sayfalarının içeriğini özelleştirmenizi sağlar.  
  
-   [Ürün listesi](#ProductList)  
  
-   [Ürün](#Product)  
  
> [!NOTE]
>  Örnek varsayılan şablonlar aşağıdaki belgelere dahil edilmiştir, ancak sürekli iyileştirmeler nedeniyle değişikliğe tabidir. Canlı varsayılan şablonları, istenen ayrı şablonlara giderek Geliştirici Portalında görüntüleyebilirsiniz. Şablonlarla çalışma hakkında daha fazla bilgi için bkz. [şablonları kullanarak API Management Geliştirici Portalını Özelleştirme](./api-management-developer-portal-templates.md).  

[!INCLUDE [api-management-portal-legacy.md](../../includes/api-management-portal-legacy.md)]

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]
  
##  <a name="product-list"></a><a name="ProductList"></a> Ürün listesi  
 **Ürün listesi** şablonu, geliştirici portalındaki ürün listesi sayfasının gövdesini özelleştirmenizi sağlar.  
  
 ![Ürün listesi](./media/api-management-product-templates/APIM_ProductsListTemplatePage.png "APIM_ProductsListTemplatePage")  
  
### <a name="default-template"></a>Varsayılan şablon  
  
```xml  
<search-control></search-control>  
<div class="row">  
    <div class="col-md-9">  
        <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>  
    </div>  
</div>  
<div class="row">  
    <div class="col-md-12">  
    {% if products.size > 0 %}  
    <ul class="list-unstyled">  
    {% for product in products %}  
        <li>  
            <h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>  
            {{product.description}}  
        </li>     
    {% endfor %}  
    </ul>  
    <paging-control></paging-control>  
    {% else %}  
    {% localized "CommonResources|NoItemsToDisplay" %}  
    {% endif %}  
    </div>  
</div>  
```  
  
### <a name="controls"></a>Denetimler  
 `Product list`Şablon aşağıdaki [sayfa denetimlerini](api-management-page-controls.md)kullanabilir.  
  
-   [sayfalama denetimi](api-management-page-controls.md#paging-control)  
  
-   [arama denetimi](api-management-page-controls.md#search-control)  
  
### <a name="data-model"></a>Veri modeli  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Sayfalama|[Sayfalama](api-management-template-data-model-reference.md#Paging) varlığı.|Ürünler koleksiyonu için sayfalama bilgileri.|  
|Filtreleme|Varlık [filtreleme](api-management-template-data-model-reference.md#Filtering) .|Ürün listesi sayfası için filtreleme bilgileri.|  
|Ürünler|[Ürün](api-management-template-data-model-reference.md#Product) varlıklarının koleksiyonu.|Geçerli kullanıcıya görünür olan ürünler.|  
  
### <a name="sample-template-data"></a>Örnek şablon verileri  
  
```json  
{  
    "Paging": {  
        "Page": 1,  
        "PageSize": 10,  
        "TotalItemCount": 2,  
        "ShowAll": false,  
        "PageCount": 1  
    },  
    "Filtering": {  
        "Pattern": null,  
        "Placeholder": "Search products"  
    },  
    "Products": [  
        {  
            "Id": "56f9445ffaf7560049060001",  
            "Title": "Starter",  
            "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",  
            "Terms": "",  
            "ProductState": 1,  
            "AllowMultipleSubscriptions": false,  
            "MultipleSubscriptionsCount": 1  
        },  
        {  
            "Id": "56f9445ffaf7560049060002",  
            "Title": "Unlimited",  
            "Description": "Subscribers have completely unlimited access to the API. Administrator approval is required.",  
            "Terms": null,  
            "ProductState": 1,  
            "AllowMultipleSubscriptions": false,  
            "MultipleSubscriptionsCount": 1  
        }  
    ]  
}  
```  
  
##  <a name="product"></a><a name="Product"></a> Ürünüyle  
 **Ürün** şablonu, geliştirici portalındaki ürün sayfasının gövdesini özelleştirmenizi sağlar.  
  
 ![Geliştirici portalı ürün sayfası](./media/api-management-product-templates/APIM_ProductPage.png "APIM_ProductPage")  
  
### <a name="default-template"></a>Varsayılan şablon  
  
```xml  
<h2>{{Product.Title}}</h2>  
<p>{{Product.Description}}</p>  
  
{% assign replaceString0 = '{0}' %}  
  
{% if Limits and Limits.size > 0 %}  
<h3>{% localized "ProductDetailsStrings|WebProductsUsageLimitsHeader"%}</h3>  
<ul>  
  {% for limit in Limits %}  
  <li>{{limit.DisplayName}}</li>  
  {% endfor %}  
</ul>  
{% endif %}  
  
{% if apis.size > 0 %}  
<p>  
  <b>  
    {% if apis.size == 1 %}  
    {% capture apisCountText %}{% localized "ProductDetailsStrings|TextblockSingleApisCount" %}{% endcapture %}  
    {% else %}  
    {% capture apisCountText %}{% localized "ProductDetailsStrings|TextblockMultipleApisCount" %}{% endcapture %}  
    {% endif %}  
  
    {% capture apisCount %}{{apis.size}}{% endcapture %}  
    {{ apisCountText | replace : replaceString0, apisCount }}  
  </b>  
</p>  
  
<ul>  
  {% for api in Apis %}  
  <li>  
    <a href="/docs/services/{{api.Id}}">{{api.Name}}</a>  
  </li>  
  {% endfor %}  
</ul>  
{% endif %}  
  
{% if subscriptions.size > 0 %}  
<p>  
  <b>  
    {% if subscriptions.size == 1 %}  
    {% capture subscriptionsCountText %}{% localized "ProductDetailsStrings|TextblockSingleSubscriptionsCount" %}{% endcapture %}  
    {% else %}  
    {% capture subscriptionsCountText %}{% localized "ProductDetailsStrings|TextblockMultipleSubscriptionsCount" %}{% endcapture %}  
    {% endif %}  
  
    {% capture subscriptionsCount %}{{subscriptions.size}}{% endcapture %}  
    {{ subscriptionsCountText | replace : replaceString0, subscriptionsCount }}  
  </b>  
</p>  
  
<ul>  
  {% for subscription in subscriptions %}  
  <li>  
    <a href="/developer#{{subscription.Id}}">{{subscription.DisplayName}}</a>  
  </li>  
  {% endfor %}  
</ul>  
{% endif %}  
{% if CannotAddBecauseSubscriptionNumberLimitReached %}  
<b>{% localized "ProductDetailsStrings|TextblockSubscriptionLimitReached" %}</b>  
{% elsif CannotAddBecauseMultipleSubscriptionsNotAllowed == false %}  
<subscribe-button></subscribe-button>  
{% endif %}  
```  
  
### <a name="controls"></a>Denetimler  
 `Product list`Şablon aşağıdaki [sayfa denetimlerini](api-management-page-controls.md)kullanabilir.  
  
-   [abone ol-düğme](api-management-page-controls.md#subscribe-button)  
  
### <a name="data-model"></a>Veri modeli  
  
|Özellik|Tür|Açıklama|  
|--------------|----------|-----------------|  
|Ürün|[Ürün](api-management-template-data-model-reference.md#Product)|Belirtilen ürün.|  
|IsDeveloperSubscribed|boolean|Geçerli kullanıcının bu ürüne abone olup olmadığı.|  
|SubscriptionState|sayı|Aboneliğin durumu. Olası durumlar şunlardır:<br /><br /> -   `0 - suspended` – abonelik engellenir ve abone ürünün herhangi bir API 'sini çağıramaz.<br />-   `1 - active` – Abonelik etkin.<br />-   `2 - expired` – abonelik sona erme tarihine ulaştı ve devre dışı bırakıldı.<br />-   `3 - submitted` – abonelik isteği geliştirici tarafından yapıldı, ancak henüz onaylanmamış veya reddedildi.<br />-   `4 - rejected` – abonelik isteği bir yönetici tarafından reddedildi.<br />-   `5 - cancelled` – abonelik, geliştirici veya yönetici tarafından iptal edildi.|  
|Sınırlar|array|Bu özellik kullanım dışıdır ve kullanılmamalıdır.|  
|DelegatedSubscriptionEnabled|boolean|Bu abonelik için [temsilcinin](./api-management-howto-setup-delegation.md) etkin olup olmadığı.|  
|DelegatedSubscriptionUrl 'Si|string|Temsilci etkin ise, temsil edilen abonelik URL 'SI.|  
|Isınmış|boolean|Ürünün koşulları varsa, geçerli kullanıcının koşulları kabul etmediğini belirtir.|  
|Abonelikler|[Abonelik Özeti](api-management-template-data-model-reference.md#SubscriptionSummary) varlıklarının koleksiyonu.|Ürüne yönelik abonelikler.|  
|GetVersionEx|[API](api-management-template-data-model-reference.md#API) varlıkları koleksiyonu.|Bu üründeki API 'Ler.|  
|CannotAddBecauseSubscriptionNumberLimitReached|boolean|Geçerli kullanıcının bu ürüne abonelik limitine göre abone olmaya uygun olup olmadığı.|  
|CannotAddBecauseMultipleSubscriptionsNotAllowed|boolean|Mevcut kullanıcının bu ürüne abone olmaya uygun olup olmadığı, birden fazla aboneliğe izin verilmesi veya bu ürüne izin verilmediğine bakılmaksızın.|  
  
### <a name="sample-template-data"></a>Örnek şablon verileri  
  
```json  
{  
    "Product": {  
        "Id": "56f9445ffaf7560049060001",  
        "Title": "Starter",  
        "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",  
        "Terms": "",  
        "ProductState": 1,  
        "AllowMultipleSubscriptions": false,  
        "MultipleSubscriptionsCount": 1  
    },  
    "IsDeveloperSubscribed": true,  
    "SubscriptionState": 1,  
    "Limits": [],  
    "DelegatedSubscriptionEnabled": false,  
    "DelegatedSubscriptionUrl": null,  
    "IsAgreed": false,  
    "Subscriptions": [  
        {  
            "Id": "56f9445ffaf7560049070001",  
            "DisplayName": "Starter  (default)"  
        }  
    ],  
    "Apis": [  
        {  
            "id": "56f9445ffaf7560049040001",  
            "name": "Echo API",  
            "description": null,  
            "serviceUrl": "http://echoapi.cloudapp.net/api",  
            "path": "echo",  
            "protocols": [  
                2  
            ],  
            "authenticationSettings": null,  
            "subscriptionKeyParameterNames": null  
        }  
    ],  
    "CannotAddBecauseSubscriptionNumberLimitReached": false,  
    "CannotAddBecauseMultipleSubscriptionsNotAllowed": true  
}  
```

## <a name="next-steps"></a>Sonraki adımlar
Şablonlarla çalışma hakkında daha fazla bilgi için bkz. [şablonları kullanarak API Management Geliştirici Portalını Özelleştirme](api-management-developer-portal-templates.md).
