---
title: Azure Service Fabric ters proxy güvenli iletişim | Microsoft Docs
description: Güvenli uçtan uca iletişimi etkinleştirmek için ters proxy yapılandırın.
services: service-fabric
documentationcenter: .net
author: kavyako
manager: vipulm
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 08/10/2017
ms.author: kavyako
ms.openlocfilehash: 0558a5647267dda26890ba3a6dc1af326fae94f6
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39308172"
---
# <a name="connect-to-a-secure-service-with-the-reverse-proxy"></a>Güvenli hizmet ters proxy ile bağlanma

Bu makalede, ters proxy ve bu nedenle bir uçtan uca güvenli kanal Etkinleştirme Hizmetleri arasında güvenli bağlantı kurmak açıklanmaktadır.

Ters proxy HTTPS üzerinde dinleyecek şekilde yapılandırıldığında güvenli hizmetlere bağlanma desteklenir. Belgenin geri kalanında, bu durumda varsayar.
Başvurmak [ters proxy Azure Service fabric'te](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy) Service Fabric ters Proxy'yi yapılandırmak için.

## <a name="secure-connection-establishment-between-the-reverse-proxy-and-services"></a>Ters proxy ile hizmetler arasında güvenli bağlantı kurma 

### <a name="reverse-proxy-authenticating-to-services"></a>Kimlik doğrulama hizmetleri için ters proxy:
Kendisini tanımlayan ters proxy ile belirtilen sertifikasını kullanarak hizmetlere ***reverseProxyCertificate*** özelliğinde **küme** [kaynak türü bölümüne](../azure-resource-manager/resource-group-authoring-templates.md). Hizmetleri ters proxy tarafından sunulan sertifika doğrulamak için mantığı uygulayabilir. Hizmetler, yapılandırma ayarlarında yapılandırma paketi olarak kabul edilen istemci sertifikası ayrıntıları belirtebilirsiniz. Bu çalışma zamanında okunabilir ve ters proxy tarafından sunulan sertifika doğrulamak için kullanılır. Başvurmak [uygulama parametrelerini yönetme](service-fabric-manage-multiple-environment-app-configuration.md) yapılandırma ayarlarını eklemek için. 

### <a name="reverse-proxy-verifying-the-services-identity-via-the-certificate-presented-by-the-service"></a>Ters proxy hizmeti tarafından sunulan sertifika aracılığıyla hizmet kimliği doğrulanıyor:
Ters proxy hizmetleri tarafından sunulan sertifikaların sunucu sertifikası doğrulamayı gerçekleştirmek için aşağıdaki ilkeleri destekler: hiçbiri, ServiceCommonNameAndIssuer ve ServiceCertificateThumbprints.
Ters proxy'nin kullanması ilkeyi seçmek için **ApplicationCertificateValidationPolicy** içinde **Applicationgateway'inin/Http** bölümüne [fabricSettings](service-fabric-cluster-fabric-settings.md).

Sonraki bölümde bu seçeneklerin her biri için yapılandırma ayrıntılarını gösterir.

### <a name="service-certificate-validation-options"></a>Hizmet sertifika doğrulama seçenekleri 

- **Hiçbiri**: ters proxy proxy hizmet sertifikası doğrulamasını atlar ve güvenli bir bağlantı kurar. Bu varsayılan davranıştır.
Belirtin **ApplicationCertificateValidationPolicy** değerle **hiçbiri** içinde [ **Applicationgateway'inin/Http** ](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) bölümü.

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http",
               "parameters": [
                 {
                   "name": "ApplicationCertificateValidationPolicy",
                   "value": "None"
                 }
               ]
             }
           ],
           ...
   }
   ```

- **ServiceCommonNameAndIssuer**: ters proxy, sertifikanın ortak adı ve hemen verenin parmak izi bağlı hizmeti tarafından sunulan sertifika doğrular: belirtin **ApplicationCertificateValidationPolicy** değerle **ServiceCommonNameAndIssuer** içinde [ **Applicationgateway'inin/Http** ](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) bölümü.

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http",
               "parameters": [
                 {
                   "name": "ApplicationCertificateValidationPolicy",
                   "value": "ServiceCommonNameAndIssuer"
                 }
               ]
             }
           ],
           ...
   }
   ```

   Hizmet ortak adı ve verenin parmak izleri listesi belirtmek için bir [ **Applicationgateway'inin/Http/ServiceCommonNameAndIssuer** ](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttpservicecommonnameandissuer) bölümüne **fabricSettings**, Aşağıda gösterildiği gibi. Birden çok sertifika ortak adı ve verenin parmak izi çiftleri eklenebilir **parametreleri** dizisi. 

   Uç nokta ters proxy sunar için ortak bir sertifika bağlanılıyorsa burada belirtilen değerlerden herhangi birini adı ve verenin parmak iziyle eşleştiğinden, SSL kanalı kurulur. 
   Sertifika ayrıntıları eşleştirmek için başarısızlık durumunda, ters proxy, istemcinin isteği 502 (hatalı ağ geçidi) durum kodu ile başarısız olur. HTTP durum satırı da "Geçersiz SSL sertifikası" ifadesini içerir 

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http/ServiceCommonNameAndIssuer",
               "parameters": [
                 {
                   "name": "WinFabric-Test-Certificate-CN1",
                   "value": "b3 44 9b 01 8d 0f 68 39 a2 c5 d6 2b 5b 6c 6a c8 22 b4 22 11"
                 },
                 {
                   "name": "WinFabric-Test-Certificate-CN2",
                   "value": "b3 44 9b 01 8d 0f 68 39 a2 c5 d6 2b 5b 6c 6a c8 22 11 33 44"
                 }
               ]
             }
           ],
           ...
   }
   ```

- **ServiceCertificateThumbprints**: ters proxy, parmak izinden yola çıkarak proxy hizmet sertifika doğrulama. Hizmetleri ile kendi kendine yapılandırıldığında bu yolu imzalı sertifikaları Git seçebilirsiniz: belirtin **ApplicationCertificateValidationPolicy** değerle **ServiceCertificateThumbprints** içinde [ **Applicationgateway'inin/Http** ](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) bölümü.

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http",
               "parameters": [
                 {
                   "name": "ApplicationCertificateValidationPolicy",
                   "value": "ServiceCertificateThumbprints"
                 }
               ]
             }
           ],
           ...
   }
   ```

   Ayrıca parmak belirtin bir **ServiceCertificateThumbprints** girişi **Applicationgateway'inin/Http** bölümü. Birden çok parmak izleri aşağıda gösterildiği gibi değeri alanına, virgülle ayrılmış bir liste olarak belirtilebilir:

   ```json
   {
   "fabricSettings": [
             ...
             {
               "name": "ApplicationGateway/Http",
               "parameters": [
                   ...
                 {
                   "name": "ServiceCertificateThumbprints",
                   "value": "78 12 20 5a 39 d2 23 76 da a0 37 f0 5a ed e3 60 1a 7e 64 bf,78 12 20 5a 39 d2 23 76 da a0 37 f0 5a ed e3 60 1a 7e 64 b9"
                 }
               ]
             }
           ],
           ...
   }
   ```

   Sunucu sertifikasının parmak izini bu yapılandırma girişi listeleniyorsa, ters proxy SSL bağlantı başarılı olur. Aksi takdirde, bağlantıyı sonlandırır ve istemcinin isteği bir 502 (hatalı ağ geçidi) ile başarısız olur. HTTP durum satırı da "Geçersiz SSL sertifikası" ifadesini içerir

## <a name="endpoint-selection-logic-when-services-expose-secure-as-well-as-unsecured-endpoints"></a>Hizmetleri güvenli olarak güvenli olmayan uç noktaları kullanıma sunun, uç nokta seçme mantığı
Service fabric, bir hizmet için birden fazla uç nokta yapılandırma destekler. Daha fazla bilgi için [bir hizmet bildiriminde kaynakları belirtme](service-fabric-service-manifest-resources.md).

Ters proxy seçer göre isteği iletmek için uç noktalardan biri **ListenerName** sorgu parametresi olarak [URI hizmet](./service-fabric-reverseproxy.md#uri-format-for-addressing-services-by-using-the-reverse-proxy). Varsa **ListenerName** parametresi belirtilmezse, ters proxy, uç noktalar listesinden herhangi bir uç noktaya seçim yapabilirsiniz. Hizmet için yapılandırılmış Uç noktalara bağlı olarak, seçili uç noktaya bir HTTP veya HTTPS uç noktası olabilir. Olabilir senaryoları veya gereksinimleri, bir "yalnızca güvenli modda"; çalışması için ters proxy istediğiniz diğer bir deyişle, güvenli olmayan uç noktaları isteklerini iletmek için güvenli ters proxy istemezsiniz. Ters proxy için salt güvenli moda ayarlanacak belirtin **SecureOnlyMode** yapılandırma girişi değerle **true** içinde [ **Applicationgateway'inin/Http** ](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) bölümü.   

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
                ...
              {
                "name": "SecureOnlyMode",
                "value": true
              }
            ]
          }
        ],
        ...
}
```

> [!NOTE]
> İçinde çalışırken **SecureOnlyMode**, bir istemci belirtilmişse bir **ListenerName** bir HTTP(unsecured) uç noktasına karşılık gelen, ters proxy istek bir 404 (bulunamadı) HTTP durum kodu ile başarısız olur.

## <a name="setting-up-client-certificate-authentication-through-the-reverse-proxy"></a>İstemci sertifikası kimlik doğrulaması ters proxy aracılığıyla ayarlama
Ters proxy SSL sonlandırma olur ve tüm istemci sertifikası verileri kaybolur. İstemci sertifikası kimlik doğrulaması gerçekleştirmek hizmetler için belirtme **ForwardClientCertificate** ayarı [ **Applicationgateway'inin/Http** ](./service-fabric-cluster-fabric-settings.md#applicationgatewayhttp) bölümü.

1. Zaman **ForwardClientCertificate** ayarlanır **false**ters proxy değil istek istemci sertifikası, SSL anlaşması sırasında istemciyle.
Bu varsayılan davranıştır.

2. Zaman **ForwardClientCertificate** ayarlanır **true**ters proxy istemcinin, SSL anlaşması sırasında istemci sertifikasını ister.
Ardından sertifika verileri, adlı özel bir HTTP üst bilgisinde istemci iletir **X istemci sertifikası**. Üst bilgi değeri istemci sertifikasını base64 kodlu PEM biçimi dizesidir. Hizmet başarılı/istek uygun durum kodu ile sertifika verileri inceleyerek sonra başarısız.
İstemci sertifika sunmuyorsa ters proxy boş bir üst bilgisi iletir ve durumu işlemek service gerisini halleder.

> [!NOTE]
> Ters proxy boyutundaydı iletici ' dir. İstemci sertifikasının tüm doğrulaması gerçekleştirmez.


## <a name="next-steps"></a>Sonraki adımlar
* Başvurmak [güvenli hizmetlerine bağlanmak için ters proxy yapılandırma](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) için Azure Resource Manager şablonu örnekleri yapılandırmak için farklı hizmet sertifikası ile ters proxy doğrulama seçenekleri güvenli.
* HTTP iletişim hizmetleri arasında bir örneğini görmek bir [GitHub üzerinde örnek proje](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).
* [Reliable Services uzaktan iletişimi ile uzak yordam çağrıları](service-fabric-reliable-services-communication-remoting.md)
* [OWIN güvenilir hizmetler kullanan Web](service-fabric-reliable-services-communication-webapi.md)
* [Küme sertifikalarını yönetme](service-fabric-cluster-security-update-certs-azure.md)
