---
title: Uç nokta kotasını artırın
titleSuffix: Azure Cognitive Services
description: Language Understanding (LUIS), tek bir anahtarın kota dışında uç nokta isteği Kotayı artırmak olanağı sunar. LUIS için daha fazla anahtarları oluşturma ve bunları LUIS uygulamaya ekleme tarafından yapıldığını **Yayımla** sayfasını **kaynakları ve anahtarları** bölümü.
author: diberry
manager: nitinme
ms.custom: seodec18
services: cognitive-services
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 02/08/2019
ms.author: diberry
ms.openlocfilehash: 89778375c6362007a81eab72663f56492f4fe206
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2019
ms.locfileid: "55997915"
---
# <a name="use-microsoft-azure-traffic-manager-to-manage-endpoint-quota-across-keys"></a>Uç nokta kota anahtarlarını yönetmek için Microsoft Azure Traffic Manager'ı kullanma
Language Understanding (LUIS), tek bir anahtarın kota dışında uç nokta isteği Kotayı artırmak olanağı sunar. LUIS için daha fazla anahtarları oluşturma ve bunları LUIS uygulamaya ekleme tarafından yapıldığını **Yayımla** sayfasını **kaynakları ve anahtarları** bölümü. 

Anahtarlar trafiği yönetmek istemci uygulaması vardır. LUIS, yapmaz. 

Bu makalede, Azure ile anahtarları arasında trafiği yönetmek üzere açıklanmaktadır [Traffic Manager][traffic-manager-marketing]. Önceden eğitilmiş ve yayımlanmış bir LUIS uygulaması olmalıdır. Biri yoksa, önceden oluşturulmuş etki alanı izleyin [hızlı](luis-get-started-create-app.md). 

## <a name="connect-to-powershell-in-the-azure-portal"></a>PowerShell için Azure portalında bağlanma
İçinde [Azure] [ azure-portal] portal, PowerShell penceresi açın. PowerShell penceresi için simge **> _** üst gezinti çubuğunda. Portaldan PowerShell kullanarak en son PowerShell sürümünü alın ve kimlik doğrulaması yapılır. Portal, PowerShell gerektirir bir [Azure depolama](https://azure.microsoft.com/services/storage/) hesabı. 

![Azure portalının ekran görüntüsü ile bir Powershell penceresi açın](./media/traffic-manager/azure-portal-powershell.png)

Aşağıdaki bölümlerde [Traffic Manager PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/?view=azurermps-6.2.0#traffic_manager).

## <a name="create-azure-resource-group-with-powershell"></a>PowerShell ile Azure kaynak grubu oluşturun
Azure kaynakları oluşturmadan önce tüm kaynakları içerecek bir kaynak grubu oluşturun. Kaynak grubunu adlandırın `luis-traffic-manager` ve bölgenin `West US`. Kaynak grubu bölgesi grup hakkındaki meta verileri depolar. Başka bir bölgede olmaları durumunda kaynaklarınızı yavaş olmaz. 

Kaynak grubu oluşturun **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup?view=azurermps-6.2.0)** cmdlet:

```powerShell
New-AzureRmResourceGroup -Name luis-traffic-manager -Location "West US"
```

## <a name="create-luis-keys-to-increase-total-endpoint-quota"></a>LUIS toplam uç nokta Kotayı artırmak için anahtarları oluşturma
1. Azure portalında, iki oluşturma **Language Understanding** anahtarları birinde `West US` ve diğeri `East US`. Adlı önceki bölümde oluşturduğunuz mevcut kaynak grubunu kullanın `luis-traffic-manager`. 

    ![Azure portalının ekran görüntüsü luıs traffic manager kaynak grubunda iki LUIS anahtarları](./media/traffic-manager/luis-keys.png)

2. İçinde [LUIS] [ LUIS] Web sitesi, **Yönet** üzerinde bölümünde **anahtarları ve uç noktaları** sayfasında anahtarları uygulamaya atamak ve uygulama tarafından yeniden yayımlayın seçme **Yayımla** sağ üst menüdeki düğmesi. 

    Örnek URL'yi **uç nokta** sütun uç noktası anahtarı ile bir GET isteği bir sorgu parametresi olarak kullanır. İki yeni anahtarları uç nokta URL'lerini kopyalayın. Bunlar, bu makalenin devamındaki Traffic Manager yapılandırması bir parçası olarak kullanılır.

## <a name="manage-luis-endpoint-requests-across-keys-with-traffic-manager"></a>Traffic Manager ile anahtarları arasında LUIS uç nokta isteklerini yönetme
Traffic Manager uç noktalarınız için yeni bir DNS erişim noktası oluşturur. Bir ağ geçidi veya proxy olarak ancak DNS düzeyinde kesinlikle davranmaz. Bu örnekte, tüm DNS kayıtlarını değiştirmez. Bir DNS kitaplığı konusu istek için doğru uç noktaya almak için Traffic Manager'ile iletişim kurmak için kullanır. _Her_ LUIS ilk kullanmak için hangi LUIS uç noktaya belirlemek için bir Traffic Manager istek doğrulamasında yönelik istek. 

### <a name="polling-uses-luis-endpoint"></a>Yoklama LUIS uç noktası kullanır.
Traffic Manager Uç noktalara düzenli aralıklarla uç nokta hala kullanılabilir olduğundan emin olmak için yoklar. Traffic Manager URL ile bir GET isteği erişilebilir olmasını ve 200 dönüş sorguladı. Uç nokta URL'sini üzerinde **Yayımla** sayfasında bunu yapar. Her uç noktası anahtarı farklı bir yol ve sorgu dizesi parametreleri olduğundan, her bir uç noktası anahtarı farklı yoklama yolu gerekiyor. Traffic Manager'ı yoklar, her zaman bir kota isteği ücreti. Sorgu dizesi parametresi **q** LUIS LUIS için gönderilen utterance uç noktadır. Bir utterance göndermek yerine, bu parametre Traffic Manager yoklama LUIS uç nokta günlüğe Traffic Manager'ın yapılandırılmış alınırken bir hata ayıklama teknik eklemek için kullanılır.

Kendi yolun her LUIS uç nokta gerekir çünkü kendi Traffic Manager profili gerekiyor. Profillerinde yönetmek için oluşturmak bir [ _iç içe geçmiş_ Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-nested-profiles) mimarisi. Bir üst profil alt profillere işaret ve bunların arasında trafiği yönetmek.

Traffic Manager yapılandırıldıktan sonra günlük kullanılacak yol değiştirmeyi unutmayın günlüğünüzün yoklama ile doluyor değil için false sorgu dizesi parametresi =.

## <a name="configure-traffic-manager-with-nested-profiles"></a>Traffic Manager ile iç içe geçmiş profillerini yapılandırma
Aşağıdaki bölümlerde, biri Doğu LUIS anahtar diğeri Batı LUIS anahtarı için iki alt profili oluşturun. Üst profil oluşturulduktan sonra iki alt profilleri üst profiline eklenir. 

### <a name="create-the-east-us-traffic-manager-profile-with-powershell"></a>PowerShell ile Doğu ABD Traffic Manager profili oluşturma
Doğu ABD Traffic Manager profili oluşturmak için birkaç adım vardır: profil oluşturma, uç nokta ekleyin ve uç noktası ayarlayın. Traffic Manager profili fazla uç nokta olabilir, ancak her uç nokta aynı doğrulama yolu vardır. LUIS uç nokta URL'leri Doğu ve Batı abonelikler için bölge ve uç noktası anahtarı nedeniyle farklı olduğundan, her LUIS uç noktası profilindeki tek bir uç nokta olması gerekir. 

1. Profil oluşturma **[New-AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile?view=azurermps-6.2.0)** cmdlet'i

    Profili oluşturmak için aşağıdaki cmdlet'i kullanın. Değiştirdiğinizden emin olun `appIdLuis` ve `subscriptionKeyLuis`. SubscriptionKey için ABD Doğu LUIS anahtardır. Traffic Manager yoklama yolu LUIS uygulama kimliği ve uç noktası anahtarı dahil olmak üzere doğru değil, durumunun ise `degraded` çünkü trafiği yönetmek LUIS uç noktası başarıyla istenemiyor. Değerini emin `q` olduğu `traffic-manager-east` LUIS uç nokta günlüklerinde bu değeri görebilirsiniz.

    ```powerShell
    $eastprofile = New-AzureRmTrafficManagerProfile -Name luis-profile-eastus -ResourceGroupName luis-traffic-manager -TrafficRoutingMethod Performance -RelativeDnsName luis-dns-eastus -Ttl 30 -MonitorProtocol HTTPS -MonitorPort 443 -MonitorPath "/luis/v2.0/apps/<appID>?subscription-key=<subscriptionKey>&q=traffic-manager-east"
    ```
    
    Bu tabloda, her bir değişken cmdlet'inde açıklanmaktadır:
    
    |Yapılandırma parametresi|Değişken adı veya değeri|Amaç|
    |--|--|--|
    |-Ad|luıs profili eastus|Azure portalındaki traffic Manager adı|
    |-ResourceGroupName|luıs traffic manager|Önceki bölümde oluşturduğunuz|
    |-TrafficRoutingMethod|Performans|Daha fazla bilgi için [Traffic Manager yönlendirme yöntemleri][routing-methods]. URL isteği için Traffic Manager, performans kullanıyorsanız, kullanıcının bölgesi gelmelidir. Bir sohbet Robotu veya başka bir uygulama giderek, ' % s'çağrısı için Traffic Manager bölgede taklit etmek için Sohbet botu'nın sorumluluk olur. |
    |-RelativeDnsName|luıs dns eastus|Bu hizmet için bir alt etki alanı olur: luıs dns eastus.trafficmanager.net|
    |-Ttl|30|Yoklama aralığı 30 saniye|
    |-MonitorProtocol<BR>-MonitorPort|HTTPS<br>443|Bağlantı noktası ve protokol LUIS için HTTPS/443'tür|
    |-MonitorPath|`/luis/v2.0/apps/<appIdLuis>?subscription-key=<subscriptionKeyLuis>&q=traffic-manager-east`|Değiştirin <appIdLuis> ve <subscriptionKeyLuis> kendi değerlerinizle.|
    
    Başarılı bir istek, yanıt aldı.

2. Doğu ABD uç noktası ekleme **[Add-AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/add-azurermtrafficmanagerendpointconfig?view=azurermps-6.2.0)** cmdlet'i

    ```powerShell
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName luis-east-endpoint -TrafficManagerProfile $eastprofile -Type ExternalEndpoints -Target eastus.api.cognitive.microsoft.com -EndpointLocation "eastus" -EndpointStatus Enabled
    ```
    Bu tabloda, her bir değişken cmdlet'inde açıklanmaktadır:

    |Yapılandırma parametresi|Değişken adı veya değeri|Amaç|
    |--|--|--|
    |-Uçnoktaadı|luıs Doğu endpoint|Profili altında görüntülenen uç nokta adı|
    |-TrafficManagerProfile|$eastprofile|1. adımda oluşturduğunuz profili nesnesini kullanın|
    |-Type|ExternalEndpoints|Daha fazla bilgi için [Traffic Manager uç noktası][traffic-manager-endpoints] |
    |-Hedef|eastus.api.cognitive.microsoft.com|LUIS uç noktası için etki alanı budur.|
    |-EndpointLocation|"eastus"|Uç nokta bölgesi|
    |-EndpointStatus|Etkin|Oluşturulduğunda, uç noktayı etkinleştirme|

    Başarılı yanıt şu şekilde görünür:

    ```console
    Id                               : /subscriptions/<azure-subscription-id>/resourceGroups/luis-traffic-manager/providers/Microsoft.Network/trafficManagerProfiles/luis-profile-eastus
    Name                             : luis-profile-eastus
    ResourceGroupName                : luis-traffic-manager
    RelativeDnsName                  : luis-dns-eastus
    Ttl                              : 30
    ProfileStatus                    : Enabled
    TrafficRoutingMethod             : Performance
    MonitorProtocol                  : HTTPS
    MonitorPort                      : 443
    MonitorPath                      : /luis/v2.0/apps/<luis-app-id>?subscription-key=f0517d185bcf467cba5147d6260bb868&q=traffic-manager-east
    MonitorIntervalInSeconds         : 30
    MonitorTimeoutInSeconds          : 10
    MonitorToleratedNumberOfFailures : 3
    Endpoints                        : {luis-east-endpoint}
    ```

3. Doğu ABD uç noktası ile Ayarla **[Set-AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/set-azurermtrafficmanagerprofile?view=azurermps-6.2.0)** cmdlet'i

    ```powerShell
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $eastprofile
    ```

    Başarılı bir yanıt aynı yanıt olarak 2. adım olacaktır.

### <a name="create-the-west-us-traffic-manager-profile-with-powershell"></a>PowerShell ile Batı ABD Traffic Manager profili oluşturma
Batı ABD Traffic Manager profili oluşturmak için aynı adımları izleyin: profil oluşturma, uç nokta ekleyin ve uç noktası ayarlayın.

1. Profil oluşturma **[New-AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/New-AzureRmTrafficManagerProfile?view=azurermps-6.2.0)** cmdlet'i

    Profili oluşturmak için aşağıdaki cmdlet'i kullanın. Değiştirdiğinizden emin olun `appIdLuis` ve `subscriptionKeyLuis`. SubscriptionKey için ABD Doğu LUIS anahtardır. Yol LUIS uygulama kimliği ve uç noktası anahtarı dahil olmak üzere doğru değilse, Traffic Manager yoklama durumu olan `degraded` çünkü trafiği yönetmek LUIS uç noktası başarıyla istenemiyor. Değerini emin `q` olduğu `traffic-manager-west` LUIS uç nokta günlüklerinde bu değeri görebilirsiniz.

    ```powerShell
    $westprofile = New-AzureRmTrafficManagerProfile -Name luis-profile-westus -ResourceGroupName luis-traffic-manager -TrafficRoutingMethod Performance -RelativeDnsName luis-dns-westus -Ttl 30 -MonitorProtocol HTTPS -MonitorPort 443 -MonitorPath "/luis/v2.0/apps/<appIdLuis>?subscription-key=<subscriptionKeyLuis>&q=traffic-manager-west"
    ```
    
    Bu tabloda, her bir değişken cmdlet'inde açıklanmaktadır:
    
    |Yapılandırma parametresi|Değişken adı veya değeri|Amaç|
    |--|--|--|
    |-Ad|luıs profili westus|Azure portalındaki traffic Manager adı|
    |-ResourceGroupName|luıs traffic manager|Önceki bölümde oluşturduğunuz|
    |-TrafficRoutingMethod|Performans|Daha fazla bilgi için [Traffic Manager yönlendirme yöntemleri][routing-methods]. URL isteği için Traffic Manager, performans kullanıyorsanız, kullanıcının bölgesi gelmelidir. Bir sohbet Robotu veya başka bir uygulama giderek, ' % s'çağrısı için Traffic Manager bölgede taklit etmek için Sohbet botu'nın sorumluluk olur. |
    |-RelativeDnsName|luıs-dns-westus|Bu hizmet için bir alt etki alanı olur: luıs dns westus.trafficmanager.net|
    |-Ttl|30|Yoklama aralığı 30 saniye|
    |-MonitorProtocol<BR>-MonitorPort|HTTPS<br>443|Bağlantı noktası ve protokol LUIS için HTTPS/443'tür|
    |-MonitorPath|`/luis/v2.0/apps/<appIdLuis>?subscription-key=<subscriptionKeyLuis>&q=traffic-manager-west`|Değiştirin <appId> ve <subscriptionKey> kendi değerlerinizle. Bu uç noktası anahtarı Doğu uç noktası anahtarı farklı olduğunu unutmayın|
    
    Başarılı bir istek, yanıt aldı.

2. Batı ABD uç noktası ekleme **[Add-AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Add-AzureRmTrafficManagerEndpointConfig?view=azurermps-6.2.0)** cmdlet'i

    ```powerShell
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName luis-west-endpoint -TrafficManagerProfile $westprofile -Type ExternalEndpoints -Target westus.api.cognitive.microsoft.com -EndpointLocation "westus" -EndpointStatus Enabled
    ```

    Bu tabloda, her bir değişken cmdlet'inde açıklanmaktadır:

    |Yapılandırma parametresi|Değişken adı veya değeri|Amaç|
    |--|--|--|
    |-Uçnoktaadı|luıs Batı endpoint|Profili altında görüntülenen uç nokta adı|
    |-TrafficManagerProfile|$westprofile|1. adımda oluşturduğunuz profili nesnesini kullanın|
    |-Type|ExternalEndpoints|Daha fazla bilgi için [Traffic Manager uç noktası][traffic-manager-endpoints] |
    |-Hedef|westus.api.cognitive.microsoft.com|LUIS uç noktası için etki alanı budur.|
    |-EndpointLocation|"westus"|Uç nokta bölgesi|
    |-EndpointStatus|Etkin|Oluşturulduğunda, uç noktayı etkinleştirme|

    Başarılı yanıt şu şekilde görünür:

    ```console
    Id                               : /subscriptions/<azure-subscription-id>/resourceGroups/luis-traffic-manager/providers/Microsoft.Network/trafficManagerProfiles/luis-profile-westus
    Name                             : luis-profile-westus
    ResourceGroupName                : luis-traffic-manager
    RelativeDnsName                  : luis-dns-westus
    Ttl                              : 30
    ProfileStatus                    : Enabled
    TrafficRoutingMethod             : Performance
    MonitorProtocol                  : HTTPS
    MonitorPort                      : 443
    MonitorPath                      : /luis/v2.0/apps/c3fc5d1e-5187-40cc-af0f-fbde328aa16b?subscription-key=e3605f07e3cc4bedb7e02698a54c19cc&q=traffic-manager-west
    MonitorIntervalInSeconds         : 30
    MonitorTimeoutInSeconds          : 10
    MonitorToleratedNumberOfFailures : 3
    Endpoints                        : {luis-west-endpoint}
    ```

3. Batı ABD uç noktası ile Ayarla **[Set-AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Set-AzureRmTrafficManagerProfile?view=azurermps-6.2.0)** cmdlet'i

    ```powerShell
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $westprofile
    ```

    Başarılı yanıt aynı yanıt olarak 2. adım:.

### <a name="create-parent-traffic-manager-profile"></a>Üst Traffic Manager profili oluşturma
Üst Traffic Manager profili oluşturun ve iki alt Traffic Manager profili üst öğeye bağlayın.

1. Üst profil oluşturma **[New-AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/New-AzureRmTrafficManagerProfile?view=azurermps-6.2.0)** cmdlet'i

    ```powerShell
    $parentprofile = New-AzureRmTrafficManagerProfile -Name luis-profile-parent -ResourceGroupName luis-traffic-manager -TrafficRoutingMethod Performance -RelativeDnsName luis-dns-parent -Ttl 30 -MonitorProtocol HTTPS -MonitorPort 443 -MonitorPath "/"
    ```

    Bu tabloda, her bir değişken cmdlet'inde açıklanmaktadır:

    |Yapılandırma parametresi|Değişken adı veya değeri|Amaç|
    |--|--|--|
    |-Ad|luıs profili üst|Azure portalındaki traffic Manager adı|
    |-ResourceGroupName|luıs traffic manager|Önceki bölümde oluşturduğunuz|
    |-TrafficRoutingMethod|Performans|Daha fazla bilgi için [Traffic Manager yönlendirme yöntemleri][routing-methods]. URL isteği için Traffic Manager, performans kullanıyorsanız, kullanıcının bölgesi gelmelidir. Bir sohbet Robotu veya başka bir uygulama giderek, ' % s'çağrısı için Traffic Manager bölgede taklit etmek için Sohbet botu'nın sorumluluk olur. |
    |-RelativeDnsName|luıs dns üst|Bu hizmet için bir alt etki alanı olur: luıs dns parent.trafficmanager.net|
    |-Ttl|30|Yoklama aralığı 30 saniye|
    |-MonitorProtocol<BR>-MonitorPort|HTTPS<br>443|Bağlantı noktası ve protokol LUIS için HTTPS/443'tür|
    |-MonitorPath|`/`|Alt uç nokta yolları yerine kullanıldığı için bu yolu önemli değildir.|

    Başarılı bir istek, yanıt aldı.

2. Doğu ABD alt profili sahip üst eklemek **[Add-AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Add-AzureRmTrafficManagerEndpointConfig?view=azurermps-6.2.0)** ve **NestedEndpoints** türü

    ```powerShell
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint-useast -TrafficManagerProfile $parentprofile -Type NestedEndpoints -TargetResourceId $eastprofile.Id -EndpointStatus Enabled -EndpointLocation "eastus" -MinChildEndpoints 1
    ```

    Bu tabloda, her bir değişken cmdlet'inde açıklanmaktadır:

    |Yapılandırma parametresi|Değişken adı veya değeri|Amaç|
    |--|--|--|
    |-Uçnoktaadı|alt uç nokta useast|Doğu profili|
    |-TrafficManagerProfile|$parentprofile|Bu uç noktaya atamak için profili|
    |-Type|NestedEndpoints|Daha fazla bilgi için [Add-AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/Add-AzureRmTrafficManagerEndpointConfig?view=azurermps-6.2.0). |
    |-Targetresourceıd|$eastprofile. Kimliği|Alt profil kimliği|
    |-EndpointStatus|Etkin|Üst öğeye ekledikten sonra uç nokta durumu|
    |-EndpointLocation|"eastus"|[Azure bölgesi adı](https://azure.microsoft.com/global-infrastructure/regions/) kaynağı|
    |-MinChildEndpoints|1|Alt uç nokta sayısı için alt sınır|

    Başarılı yanıt Ara yeni içerir ve aşağıdaki gibi `child-endpoint-useast` uç noktası:    

    ```console
    Id                               : /subscriptions/<azure-subscription-id>/resourceGroups/luis-traffic-manager/providers/Microsoft.Network/trafficManagerProfiles/luis-profile-parent
    Name                             : luis-profile-parent
    ResourceGroupName                : luis-traffic-manager
    RelativeDnsName                  : luis-dns-parent
    Ttl                              : 30
    ProfileStatus                    : Enabled
    TrafficRoutingMethod             : Performance
    MonitorProtocol                  : HTTPS
    MonitorPort                      : 443
    MonitorPath                      : /
    MonitorIntervalInSeconds         : 30
    MonitorTimeoutInSeconds          : 10
    MonitorToleratedNumberOfFailures : 3
    Endpoints                        : {child-endpoint-useast}
    ```

3. Batı ABD alt profili sahip üst eklemek **[Add-AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Add-AzureRmTrafficManagerEndpointConfig?view=azurermps-6.2.0)** cmdlet'i ve **NestedEndpoints** türü

    ```powerShell
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint-uswest -TrafficManagerProfile $parentprofile -Type NestedEndpoints -TargetResourceId $westprofile.Id -EndpointStatus Enabled -EndpointLocation "westus" -MinChildEndpoints 1
    ```

    Bu tabloda, her bir değişken cmdlet'inde açıklanmaktadır:

    |Yapılandırma parametresi|Değişken adı veya değeri|Amaç|
    |--|--|--|
    |-Uçnoktaadı|alt uç nokta uswest|Batı profili|
    |-TrafficManagerProfile|$parentprofile|Bu uç noktaya atamak için profili|
    |-Type|NestedEndpoints|Daha fazla bilgi için [Add-AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/Add-AzureRmTrafficManagerEndpointConfig?view=azurermps-6.2.0). |
    |-Targetresourceıd|$westprofile. Kimliği|Alt profil kimliği|
    |-EndpointStatus|Etkin|Üst öğeye ekledikten sonra uç nokta durumu|
    |-EndpointLocation|"westus"|[Azure bölgesi adı](https://azure.microsoft.com/global-infrastructure/regions/) kaynağı|
    |-MinChildEndpoints|1|Alt uç nokta sayısı için alt sınır|

    Başarılı yanıt Ara hem de önceki içerir ve gibi `child-endpoint-useast` uç noktası ve yeni `child-endpoint-uswest` uç noktası:

    ```console
    Id                               : /subscriptions/<azure-subscription-id>/resourceGroups/luis-traffic-manager/providers/Microsoft.Network/trafficManagerProfiles/luis-profile-parent
    Name                             : luis-profile-parent
    ResourceGroupName                : luis-traffic-manager
    RelativeDnsName                  : luis-dns-parent
    Ttl                              : 30
    ProfileStatus                    : Enabled
    TrafficRoutingMethod             : Performance
    MonitorProtocol                  : HTTPS
    MonitorPort                      : 443
    MonitorPath                      : /
    MonitorIntervalInSeconds         : 30
    MonitorTimeoutInSeconds          : 10
    MonitorToleratedNumberOfFailures : 3
    Endpoints                        : {child-endpoint-useast, child-endpoint-uswest}
    ```

4. Uç noktaları ile ayarlanmış **[Set-AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Set-AzureRmTrafficManagerProfile?view=azurermps-6.2.0)** cmdlet'i 

    ```powerShell
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $parentprofile
    ```

    Başarılı yanıt aynı yanıt olarak 3. adım:.

### <a name="powershell-variables"></a>PowerShell değişkenleri
Önceki bölümlerde üç PowerShell değişkenleri oluşturulan: `$eastprofile`, `$westprofile`, `$parentprofile`. Bu değişkenler, Traffic Manager yapılandırması sonuna doğru kullanılır. Değişkenleri oluşturmamayı seçtiniz veya için veya PowerShell pencerenizi zaman aşımına, PowerShell cmdlet'ini kullanabilirsiniz  **[Get-AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Get-AzureRmTrafficManagerProfile?view=azurermps-6.2.0)**, profili yeniden alın ve atamak için bir değişkene. 

Açılı ayraçlar öğeleri değiştirin `<>`, her gereksinim duyduğunuz üç profil için doğru değerlerle. 

```powerShell
$<variable-name> = Get-AzureRmTrafficManagerProfile -Name <profile-name> -ResourceGroupName luis-traffic-manager
```

## <a name="verify-traffic-manager-works"></a>Trafik Yöneticisi'nin çalıştığını doğrulama
İş Traffic Manager profilleri, Profiller durumunu gerek doğrulamak için `Online` bu durum uç noktaya yoklama yolda temel alır. 

### <a name="view-new-profiles-in-the-azure-portal"></a>Azure portalında yeni profilleri görüntüle
Üç profil kaynakları bakarak oluşturulduğunu doğrulayabilirsiniz `luis-traffic-manager` kaynak grubu.

![Ekran görüntüsü, Azure kaynak grubu luıs-traffic-manager](./media/traffic-manager/traffic-manager-profiles.png)

### <a name="verify-the-profile-status-is-online"></a>Profili durumunun çevrimiçi olduğunu doğrulayın
Traffic Manager çevrimiçi olduğundan emin olmak için her uç nokta yolunu yoklar. Çevrimiçi ise, durumu alt profilleri: `Online`. Bu görüntülenen **genel bakış** her profil. 

![Ekran Azure Traffic Manager profili, izleme durumu çevrimiçi gösteren genel bakış](./media/traffic-manager/profile-status-online.png)

### <a name="validate-traffic-manager-polling-works"></a>Traffic Manager'ın çalıştığı yoklama doğrula
Traffic manager Works yoklama doğrulamak için başka bir LUIS uç nokta günlüklerle yoludur. Üzerinde [LUIS] [ LUIS] Web sitesi uygulamalar listesi sayfasında, uygulama için uç nokta günlüğünü dışarı aktarma. Traffic Manager için iki uç nokta yoklayacağını çünkü girişler vardır günlüklerde bile yalnızca birkaç dakikada silinmiş. Sorgu başladığı ile girdilerini arayın unutmayın `traffic-manager-`.

```console
traffic-manager-west    6/7/2018 19:19  {"query":"traffic-manager-west","intents":[{"intent":"None","score":0.944767}],"entities":[]}
traffic-manager-east    6/7/2018 19:20  {"query":"traffic-manager-east","intents":[{"intent":"None","score":0.944767}],"entities":[]}
```

### <a name="validate-dns-response-from-traffic-manager-works"></a>DNS yanıtı Traffic Manager çalıştığını doğrulama
DNS yanıtı LUIS uç nokta döndürür doğrulamak için bir DNS istemci kitaplığını kullanarak DNS trafiği yönetmek üst profili isteyin. Üst profili için DNS adı `luis-dns-parent.trafficmanager.net`.

Aşağıdaki Node.js kod üst profili için bir istek gönderir ve LUIS uç noktasına döndürür:

```javascript
const dns = require('dns');

dns.resolveAny('luis-dns-parent.trafficmanager.net', (err, ret) => {
  console.log('ret', ret);
});
```

LUIS uç noktasıyla başarılı yanıt şöyledir:

```json
[
    {
        value: 'westus.api.cognitive.microsoft.com', 
        type: 'CNAME'
    }
]
```

## <a name="use-the-traffic-manager-parent-profile"></a>Traffic Manager ana profilini kullanın
Uç noktalar genelinde trafiği yönetmek için Traffic Manager DNS LUIS uç noktasını bulmak için bir çağrı eklemeniz gerekir. Bu çağrı için her LUIS uç nokta isteği yapılır ve coğrafi konum LUIS istemci uygulamasının kullanıcının benzetimini yapmak gerekiyor. LUIS istemci uygulamanız ile istek aralığındaki DNS yanıt kodu için uç nokta tahmin için LUIS ekleyin. 

## <a name="resolving-a-degraded-state"></a>Düzeyi düşürülmüş durumunu çözme

Etkinleştirme [tanılama günlükleri](../../traffic-manager/traffic-manager-diagnostic-logs.md) neden uç nokta durumu düzeyi düşürüldü görmek için Traffic Manager'için.

## <a name="clean-up"></a>Temizleme
İki LUIS uç noktası anahtarı, üç Traffic Manager profillerini ve beş bu kaynakları içeren kaynak grubunu kaldırın. Bu, Azure portalından gerçekleştirilir. Beş kaynakları kaynakları listeden silin. Kaynak grubunu silin. 

## <a name="next-steps"></a>Sonraki adımlar

Gözden geçirme [ara yazılım](https://docs.microsoft.com/azure/bot-service/bot-builder-create-middleware?view=azure-bot-service-4.0&tabs=csaddmiddleware%2Ccsetagoverwrite%2Ccsmiddlewareshortcircuit%2Ccsfallback%2Ccsactivityhandler) Botframework'te v4 seçeneklerinde Bu trafiği yönetim kodu Botframework'te robota nasıl eklenebileceğini öğrenin. 

[traffic-manager-marketing]: https://azure.microsoft.com/services/traffic-manager/
[traffic-manager-docs]: https://docs.microsoft.com/azure/traffic-manager/
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[azure-portal]: https://portal.azure.com/
[azure-storage]: https://azure.microsoft.com/services/storage/
[routing-methods]: https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods
[traffic-manager-endpoints]: https://docs.microsoft.com/azure/traffic-manager/traffic-manager-endpoint-types
