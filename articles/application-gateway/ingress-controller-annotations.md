---
title: Application Gateway giriş denetleyicisi ek açıklamaları
description: Bu makalede, Application Gateway giriş denetleyicisine özgü ek açıklamaların belgelerini bulabilirsiniz.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 4f570f9f18f9c9d484a9bc9c1a5c64d42dbdc714
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93397459"
---
# <a name="annotations-for-application-gateway-ingress-controller"></a>Application Gateway giriş denetleyicisi için ek açıklamalar 

## <a name="introductions"></a>Senaryoya

Kubernetes giriş kaynağına, rastgele anahtar/değer çiftleri ile açıklama eklenebilir. AGIC, program Application Gateway özellikleriyle ilgili ek açıklamaları kullanır ve bu, giriş YAML aracılığıyla yapılandırılamaz. Giriş ek açıklamaları, bir giriş kaynağından türetilmiş tüm HTTP ayarına, arka uç havuzlarına ve dinleyicilerine uygulanır.

## <a name="list-of-supported-annotations"></a>Desteklenen ek açıklamaların listesi

Bir giriş kaynağının AGIC tarafından gözlenecek olması için, ile **açıklanmalıdır** `kubernetes.io/ingress.class: azure/application-gateway` . Bu durumda yalnızca AGIC, söz konusu giriş kaynağıyla birlikte çalışır.

| Ek açıklama anahtarı | Değer Türü | Varsayılan değer | İzin Verilen Değerler
| -- | -- | -- | -- |
| [appgw.ingress.kubernetes.io/backend-path-prefix](#backend-path-prefix) | `string` | `nil` | |
| [appgw.ingress.kubernetes.io/ssl-redirect](#tls-redirect) | `bool` | `false` | |
| [appgw.ingress.kubernetes.io/connection-draining](#connection-draining) | `bool` | `false` | |
| [appgw.ingress.kubernetes.io/connection-draining-timeout](#connection-draining) | `int32` saniyeden | `30` | |
| [appgw.ingress.kubernetes.io/cookie-based-affinity](#cookie-based-affinity) | `bool` | `false` | |
| [appgw.ingress.kubernetes.io/request-timeout](#request-timeout) | `int32` saniyeden | `30` | |
| [appgw.ingress.kubernetes.io/use-private-ip](#use-private-ip) | `bool` | `false` | |
| [appgw.ingress.kubernetes.io/backend-protocol](#backend-protocol) | `string` | `http` | `http`, `https` |

## <a name="backend-path-prefix"></a>Arka uç yolu ön eki

Bu ek açıklama, bir giriş kaynağında belirtilen arka uç yolunun bu ek açıklamada belirtilen önekle yeniden yazılabilir olmasını sağlar. Bu, kullanıcıların uç noktaları bir hizmet kaynağında kullanıma sunmak için kullanılan uç nokta adlarından farklı olan hizmetleri kullanıma sunmasına olanak tanır.

### <a name="usage"></a>Kullanım

```yaml
appgw.ingress.kubernetes.io/backend-path-prefix: <path prefix>
```

### <a name="example"></a>Örnek

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: go-server-ingress-bkprefix
  namespace: test-ag
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/backend-path-prefix: "/test/"
spec:
  rules:
  - http:
      paths:
      - path: /hello/
        backend:
          serviceName: go-server-service
          servicePort: 80
```
Yukarıdaki örnekte, ek açıklama ile adlı bir giriş kaynağı tanımlandık `go-server-ingress-bkprefix` `appgw.ingress.kubernetes.io/backend-path-prefix: "/test/"` . Ek açıklama, uygulama ağ geçidine, yolu için bir yol öneki geçersiz kılması olacak bir HTTP ayarı oluşturmasını söyler `/hello` `/test/` .

> [!NOTE] 
> Yukarıdaki örnekte yalnızca bir kural tanımlanmış. Ancak, ek açıklamalar tüm giriş kaynağına uygulanabilir, bu nedenle, bir Kullanıcı birden çok kural tanımlıysa, belirtilen yolların her biri için arka uç yolu ön eki ayarlanır. Bu nedenle, bir kullanıcı farklı yol önekleri (aynı hizmet için bile) ile farklı kurallar istiyorsa farklı giriş kaynakları tanımlamanız gerekir.

## <a name="tls-redirect"></a>TLS yeniden yönlendirme

Application Gateway [,](./redirect-overview.md) http url 'lerinin OTOMATIK olarak https karşılıklarına yönlendirileceği şekilde yapılandırılabilir. Bu ek açıklama mevcut olduğunda ve TLS düzgün yapılandırıldığında, Kubernetes giriş denetleyicisi [yeniden yönlendirme yapılandırması olan bir yönlendirme kuralı](./redirect-http-to-https-portal.md#add-a-routing-rule-with-a-redirection-configuration) oluşturur ve değişiklikleri Application Gateway uygular. Oluşturulan yeniden yönlendirme HTTP olur `301 Moved Permanently` .

### <a name="usage"></a>Kullanım

```yaml
appgw.ingress.kubernetes.io/ssl-redirect: "true"
```

### <a name="example"></a>Örnek

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: go-server-ingress-redirect
  namespace: test-ag
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/ssl-redirect: "true"
spec:
  tls:
   - hosts:
     - www.contoso.com
     secretName: testsecret-tls
  rules:
  - host: www.contoso.com
    http:
      paths:
      - backend:
          serviceName: websocket-repeater
          servicePort: 80
```

## <a name="connection-draining"></a>Bağlantı boşaltma

`connection-draining`: Bu ek açıklama kullanıcıların bağlantı boşaltma 'nın etkinleştirilip etkinleştirilmeyeceğini belirlemesine izin verir.
`connection-draining-timeout`: Bu ek açıklama, kullanıcıların boşaltma arka uç uç noktasına istekleri sonlandırmak için Application Gateway bir zaman aşımı belirtmesini sağlar.

### <a name="usage"></a>Kullanım

```yaml
appgw.ingress.kubernetes.io/connection-draining: "true"
appgw.ingress.kubernetes.io/connection-draining-timeout: "60"
```

### <a name="example"></a>Örnek

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: go-server-ingress-drain
  namespace: test-ag
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/connection-draining: "true"
    appgw.ingress.kubernetes.io/connection-draining-timeout: "60"
spec:
  rules:
  - http:
      paths:
      - path: /hello/
        backend:
          serviceName: go-server-service
          servicePort: 80
```

## <a name="cookie-based-affinity"></a>Tanımlama bilgisi tabanlı benzeşim

Bu ek açıklama, tanımlama bilgisi tabanlı Benzeşimin etkinleştirilip etkinleştirilmeyeceğini belirtmenizi sağlar.

### <a name="usage"></a>Kullanım

```yaml
appgw.ingress.kubernetes.io/cookie-based-affinity: "true"
```

### <a name="example"></a>Örnek

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: go-server-ingress-affinity
  namespace: test-ag
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/cookie-based-affinity: "true"
spec:
  rules:
  - http:
      paths:
      - path: /hello/
        backend:
          serviceName: go-server-service
          servicePort: 80
```

## <a name="request-timeout"></a>İstek Zaman Aşımı

Bu ek açıklama, yanıt alınmadığında Application Gateway isteğin başarısız olacağı saniye cinsinden istek zaman aşımını belirtmenizi sağlar.

### <a name="usage"></a>Kullanım

```yaml
appgw.ingress.kubernetes.io/request-timeout: "20"
```

### <a name="example"></a>Örnek

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: go-server-ingress-timeout
  namespace: test-ag
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/request-timeout: "20"
spec:
  rules:
  - http:
      paths:
      - path: /hello/
        backend:
          serviceName: go-server-service
          servicePort: 80
```

## <a name="use-private-ip"></a>Özel IP kullan

Bu ek açıklama, Application Gateway özel IP 'si üzerinde bu uç noktanın kullanıma sunulmayacağını belirtmemizi sağlar.

> [!NOTE]
> * Application Gateway aynı bağlantı noktasında birden çok IP 'yi desteklemez (örnek: 80/443). Ek açıklama `appgw.ingress.kubernetes.io/use-private-ip: "false"` ve ile diğeri ile giriş `appgw.ingress.kubernetes.io/use-private-ip: "true"` `HTTP` , Application Gateway güncelleştirmede AGC 'nin başarısız olmasına neden olur.
> * Özel IP 'si olmayan Application Gateway için ile birlikte giriş `appgw.ingress.kubernetes.io/use-private-ip: "true"` yok sayılır. Bu işlem, uyarı ile ilgili giriş olayları için denetleyici günlüklerine ve giriş olaylarına yansıtılır `NoPrivateIP` .


### <a name="usage"></a>Kullanım
```yaml
appgw.ingress.kubernetes.io/use-private-ip: "true"
```

### <a name="example"></a>Örnek
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: go-server-ingress-timeout
  namespace: test-ag
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/use-private-ip: "true"
spec:
  rules:
  - http:
      paths:
      - path: /hello/
        backend:
          serviceName: go-server-service
          servicePort: 80
```

## <a name="backend-protocol"></a>Arka uç Protokolü

Bu ek açıklama, Application Gateway pods ile görüşülürken kullanması gereken Protokolü belirtmemizi sağlar. Desteklenen protokoller: `http` , `https`

> [!NOTE]
> * Application Gateway ' de otomatik olarak imzalanan sertifikalar desteklenirken, şu anda AGIC yalnızca, `https` iyi bilinen BIR CA tarafından imzalanan sertifikayı kullanırken desteklenir.
> * 80 numaralı bağlantı noktasını 443 HTTPS ile kullandığınızdan emin olun.

### <a name="usage"></a>Kullanım
```yaml
appgw.ingress.kubernetes.io/backend-protocol: "https"
```

### <a name="example"></a>Örnek
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: go-server-ingress-timeout
  namespace: test-ag
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/backend-protocol: "https"
spec:
  rules:
  - http:
      paths:
      - path: /hello/
        backend:
          serviceName: go-server-service
          servicePort: 443
```