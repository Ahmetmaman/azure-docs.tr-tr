---
title: Bir HTTP giriş denetleyicisine statik bir IP adresi Azure Kubernetes Service (AKS) ile oluşturun
description: Yükleme ve bir NGINX giriş denetleyicisine Azure Kubernetes Service (AKS) kümesini bir statik genel IP adresi ile yapılandırma hakkında bilgi edinin.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 08/30/2018
ms.author: iainfou
ms.openlocfilehash: 643fcbd3e2fa4cbd716eff8977197e148cc896ef
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2019
ms.locfileid: "55731249"
---
# <a name="create-an-ingress-controller-with-a-static-public-ip-address-in-azure-kubernetes-service-aks"></a>Bir statik genel IP adresiyle Azure Kubernetes Service (AKS) giriş denetleyicisini oluşturma

Giriş denetleyicisine ters proxy, yapılandırılabilir bir trafik yönlendirme ve TLS sonlandırma Kubernetes hizmetleri sağlayan bir yazılım parçasıdır. Kubernetes giriş kaynakları, giriş kuralları ve rotaları için her bir Kubernetes hizmeti yapılandırmak için kullanılır. Giriş denetleyicisine ve giriş kuralları kullanarak, tek bir IP adresi için bir Kubernetes kümesinde birden çok hizmet trafiği yönlendirmek için kullanılabilir.

Bu makalede nasıl dağıtılacağı gösterilir [NGINX giriş denetleyicisine] [ nginx-ingress] Azure Kubernetes Service (AKS) kümesi içinde. Giriş denetleyicisine statik bir genel IP adresi ile yapılandırılır. [Sertifika Yöneticisi] [ cert-manager] proje otomatik olarak oluşturmak ve yapılandırmak için kullanılan [şimdi şifrelemek] [ lets-encrypt] sertifikaları. Son olarak, iki uygulama, her biri tek bir IP adresi erişilebilir, bir AKS kümesinde çalıştırılır.

Aşağıdakileri de yapabilirsiniz:

- [Dış ağ bağlantısına sahip bir temel giriş denetleyicisi oluşturun][aks-ingress-basic]
- [HTTP uygulama yönlendirme eklentiyi etkinleştir][aks-http-app-routing]
- [Kendi TLS sertifikalarını kullanan bir giriş denetleyicisini oluşturma][aks-ingress-own-tls]
- [Şimdi şifreleme dinamik genel IP adresi ile TLS sertifikalarını otomatik olarak oluşturmak için kullandığı bir giriş denetleyicisini oluşturma][aks-ingress-tls]

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, NGINX giriş denetleyicisine, Sertifika Yöneticisi ve örnek bir web uygulamasını yüklemek için Helm kullanır. AKS kümenizi içinde başlatılan ve bir hizmet hesabı için Tiller kullanarak Helm olması gerekir. Helm en son sürümünü kullandığınızdan emin olun. Yükseltme yönergeleri için bkz. [Helm yükleme docs][helm-install]. Yapılandırma ve Helm kullanma hakkında daha fazla bilgi için bkz. [Azure Kubernetes Service (AKS) Helm ile uygulamaları yükleme][use-helm].

Bu makalede, ayrıca Azure CLI Sürüm 2.0.41 çalıştırdığınız gerektirir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Bir giriş denetleyicisini oluşturma

Varsayılan olarak, bir NGINX giriş denetleyicisine ile yeni ortak IP adresi ataması oluşturulur. Bu genel IP adresi-ömrü giriş denetleyicisine için yalnızca statik bir değer ve denetleyiciye silinip yeniden oluşturulması durumunda kaybolur. Ortak bir yapılandırma gereksinimi, var olan bir statik genel IP adresini NGINX giriş denetleyicisine sağlamaktır. Giriş denetleyicisine silinirse statik genel IP adresini kalır. Bu yaklaşım, var olan DNS kayıtlarını ve ağ yapılandırmaları uygulamalarınızın yaşam döngüsü boyunca tutarlı bir şekilde kullanmanıza olanak sağlar.

Statik genel IP adresi oluşturmanız gerekiyorsa, ilk kaynak grubu adını kullanarak AKS kümesiyle alın [az aks show] [ az-aks-show] komutu:

```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv
```

Ardından, bir genel IP adresiyle oluşturun *statik* ayırma yöntemiyle [az network public-IP oluşturma] [ az-network-public-ip-create] komutu. Aşağıdaki örnekte adlı bir genel IP adresi oluşturur *myAKSPublicIP* AKS kümesi önceki adımda elde edilen kaynak grubu:

```azurecli
az network public-ip create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP --allocation-method static
```

Şimdi Dağıt *ngınx giriş* Helm grafiği. Ekleme `--set controller.service.loadBalancerIP` parametresi ve önceki adımda oluşturulan kendi genel IP adresi belirtin. Eklenen yedeklilik için NGINX giriş denetleyicilerinin iki çoğaltma ile dağıtılan `--set controller.replicaCount` parametresi. Giriş denetleyicisine çoğaltmalarını çalışmasını tam olarak yararlanmak için AKS kümenizde birden fazla düğüm olduğundan emin olun.

> [!TIP]
> Aşağıdaki örnekler giriş denetleyicisine ve sertifikaları Yükle `kube-system` ad alanı. İsterseniz, farklı bir ad alanı için ortamınızda belirtebilirsiniz. AKS kümenizi RBAC etkin değilse, ayrıca, ekleme `--set rbac.create=false` komutlar.

```console
helm install stable/nginx-ingress \
    --namespace kube-system \
    --set controller.service.loadBalancerIP="40.121.63.72"  \
    --set controller.replicaCount=2
```

NGINX giriş denetleyici için Kubernetes Yük Dengeleyici Hizmeti oluşturulduğunda, statik IP adresiniz, aşağıdaki örnek çıktıda gösterildiği gibi atanır:

```
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                        TYPE           CLUSTER-IP    EXTERNAL-IP    PORT(S)                      AGE
dinky-panda-nginx-ingress-controller        LoadBalancer   10.0.232.56   40.121.63.72   80:31978/TCP,443:32037/TCP   3m
dinky-panda-nginx-ingress-default-backend   ClusterIP      10.0.95.248   <none>         80/TCP                       3m
```

Genel IP adresine göz atın NGINX giriş denetleyicisinin varsayılan 404 sayfa görüntülenir bu nedenle hiçbir giriş kuralları henüz oluşturulmadı. Aşağıdaki adımlarda yapılandırılmış giriş kural.

## <a name="configure-a-dns-name"></a>Bir DNS adı yapılandırma

HTTPS sertifikaları düzgün çalışması bir FQDN giriş denetleyicisine IP adresi için yapılandırın. Aşağıdaki komut dosyası, giriş denetleyicisine FQDN için kullanmak istediğiniz benzersiz bir ad ve IP adresiyle güncelleştirin:

```console
#!/bin/bash

# Public IP address of your ingress controller
IP="40.121.63.72"

# Name to associate with public IP address
DNSNAME="demo-aks-ingress"

# Get the resource-id of the public ip
PUBLICIPID=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[id]" --output tsv)

# Update public ip address with DNS name
az network public-ip update --ids $PUBLICIPID --dns-name $DNSNAME
```

Giriş denetleyicisine şimdi FQDN'si erişilebilir.

## <a name="install-cert-manager"></a>Sertifika Yöneticisi'ni yükleyin

NGINX giriş denetleyicisine TLS sonlandırma destekler. Almak ve HTTPS için sertifikaları yapılandırmanız için birkaç yolu vardır. Bu makalede kullanmayı gösterir [Sertifika Yöneticisi][cert-manager], otomatik sağlayan [sağlar şifrelemek] [ lets-encrypt] sertifika oluşturma ve Yönetim işlevselliği.

> [!NOTE]
> Bu makalede `staging` şimdi şifrelemek için ortamı. Üretim dağıtımında kullanmak `letsencrypt-prod` ve `https://acme-v02.api.letsencrypt.org/directory` kaynak tanımlarında ve Helm grafiği yüklerken.

Sertifika Yöneticisi denetleyicisi RBAC özellikli bir kümede yüklemek için aşağıdakileri kullanın `helm install` komutu. Yeniden isterseniz değiştirme `--namespace` dışında bir şey *kube-system*:

```console
helm install stable/cert-manager \
  --namespace kube-system \
  --set ingressShim.defaultIssuerName=letsencrypt-staging \
  --set ingressShim.defaultIssuerKind=ClusterIssuer
```

Bunun yerine, kümenizin RBAC etkin değilse, aşağıdaki komutu kullanın:

```console
helm install stable/cert-manager \
  --namespace kube-system \
  --set ingressShim.defaultIssuerName=letsencrypt-staging \
  --set ingressShim.defaultIssuerKind=ClusterIssuer \
  --set rbac.create=false \
  --set serviceAccount.create=false
```

Sertifika Yöneticisi yapılandırma hakkında daha fazla bilgi için bkz. [Sertifika Yöneticisi proje][cert-manager].

## <a name="create-a-ca-cluster-issuer"></a>Veren CA küme oluşturma

Sertifika Yöneticisi sertifika verilmeden önce gerektiren bir [veren] [ cert-manager-issuer] veya [ClusterIssuer] [ cert-manager-cluster-issuer] kaynak. Bu Kubernetes kaynakları işlevselliği, ancak özdeş `Issuer` tek bir ad alanında çalışır ve `ClusterIssuer` tüm ad alanları geneline çalışır. Daha fazla bilgi için [Sertifika Yöneticisi veren] [ cert-manager-issuer] belgeleri.

Bir küme veren gibi oluşturma `cluster-issuer.yaml`, aşağıdaki örnekte bildirimi kullanarak. Kuruluşunuzun geçerli bir adresten e-posta adresiyle güncelleştirme:

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: ClusterIssuer
metadata:
  name: letsencrypt-staging
spec:
  acme:
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    email: user@contoso.com
    privateKeySecretRef:
      name: letsencrypt-staging
    http01: {}
```

Verici oluşturmak için kullanın `kubectl apply -f cluster-issuer.yaml` komutu.

```
$ kubectl apply -f cluster-issuer.yaml

clusterissuer.certmanager.k8s.io/letsencrypt-staging created
```

## <a name="create-a-certificate-object"></a>Bir sertifika nesnesi oluşturun

Ardından, sertifika kaynak oluşturulması gerekir. Sertifika kaynak istenen X.509 sertifikası tanımlar. Daha fazla bilgi için [Sertifika Yöneticisi sertifika][cert-manager-certificates].

Sertifika kaynak gibi oluşturma `certificates.yaml`, aşağıdaki örnekte bildirime sahip. Güncelleştirme *dnsNames* ve *etki alanları* bir önceki adımda oluşturduğunuz DNS adı. Yalnızca dahili giriş denetleyicisine kullanırsanız, hizmetiniz için iç DNS adını belirtin.

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: Certificate
metadata:
  name: tls-secret
spec:
  secretName: tls-secret
  dnsNames:
  - demo-aks-ingress.eastus.cloudapp.azure.com
  acme:
    config:
    - http01:
        ingressClass: nginx
      domains:
      - demo-aks-ingress.eastus.cloudapp.azure.com
  issuerRef:
    name: letsencrypt-staging
    kind: ClusterIssuer
```

Sertifika kaynak oluşturmak için kullanın `kubectl apply -f certificates.yaml` komutu.

```
$ kubectl apply -f certificates.yaml

certificate.certmanager.k8s.io/tls-secret created
```

Sertifika başarıyla oluşturulduğunu doğrulamak için `kubectl describe certificate tls-secret` komutu.

Sertifika verilmişse, aşağıdakine benzer bir çıktı görürsünüz:
```
Type    Reason          Age   From          Message
----    ------          ----  ----          -------
  Normal  CreateOrder     11m   cert-manager  Created new ACME order, attempting validation...
  Normal  DomainVerified  10m   cert-manager  Domain "demo-aks-ingress.eastus.cloudapp.azure.com" verified with "http-01" validation
  Normal  IssueCert       10m   cert-manager  Issuing certificate...
  Normal  CertObtained    10m   cert-manager  Obtained certificate from ACME server
  Normal  CertIssued      10m   cert-manager  Certificate issued successfully
```

## <a name="run-demo-applications"></a>Tanıtım uygulamaları

Giriş denetleyicisine ve sertifika yönetimi çözümü yapılandırıldı. Şimdi, AKS kümesinde uygulamaları çalıştırma iki deneme şimdi. Bu örnekte, iki basit bir 'Merhaba Dünya' uygulaması örneğini dağıtmak için Helm kullanılır.

Örnek Helm grafikleri yükleyebilmek için önce Azure örnekleri deposu Helm ortamınıza aşağıdaki şekilde ekleyin:

```console
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

Aşağıdaki komutla bir Helm grafiği ilk demo uygulamasını oluşturun:

```console
helm install azure-samples/aks-helloworld
```

Şimdi ikinci bir örneğini demo uygulamasını yükleyin. İkinci örnek için iki uygulama görsel olarak benzersiz olacak şekilde yeni bir başlık belirtin. Ayrıca bir benzersiz bir hizmet ad belirtin:

```console
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-an-ingress-route"></a>Bir giriş yol oluşturma

Türünde bir hizmet yapılandırılmışsa ancak her iki uygulamaları artık Kubernetes kümenizde çalışan `ClusterIP`. Bu nedenle, uygulamaların internet'ten erişilemez. Genel olarak kullanılabilir hale getirmek için bir Kubernetes giriş kaynağı oluşturun. Giriş kaynağı, iki uygulama birine trafik yönlendirme kuralları yapılandırır.

Aşağıdaki örnekte, adres için trafiği `https://demo-aks-ingress.eastus.cloudapp.azure.com/` adlı hizmete yönlendirilir `aks-helloworld`. Trafiği adresine `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` yönlendirilir `ingress-demo` hizmeti. Güncelleştirme *konakları* ve *konak* bir önceki adımda oluşturduğunuz DNS adı.

Adlı bir dosya oluşturun `hello-world-ingress.yaml` ve aşağıdaki örnekte YAML kopyalayın.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    certmanager.k8s.io/cluster-issuer: letsencrypt-staging
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  tls:
  - hosts:
    - demo-aks-ingress.eastus.cloudapp.azure.com
    secretName: tls-secret
  rules:
  - host: demo-aks-ingress.eastus.cloudapp.azure.com
    http:
      paths:
      - path: /
        backend:
          serviceName: aks-helloworld
          servicePort: 80
      - path: /hello-world-two
        backend:
          serviceName: ingress-demo
          servicePort: 80
```

Giriş kullanarak kaynak oluşturma `kubectl apply -f hello-world-ingress.yaml` komutu.

```
$ kubectl apply -f hello-world-ingress.yaml

ingress.extensions/hello-world-ingress created
```

## <a name="test-the-ingress-configuration"></a>Giriş yapılandırmayı test etme

Kubernetes giriş denetleyicinizin FQDN'si için bir web tarayıcısı gibi açın *https://demo-aks-ingress.eastus.cloudapp.azure.com*.

Bu örneklerde gibi `letsencrypt-staging`, tarayıcı tarafından verilen bir SSL sertifikası güvenilir değil. Uygulamanıza devam etmek için herhangi bir uyarıyı kabul edin. Bu sertifika bilgilerini gösterir *sahte LE Ara X1* şimdi şifrelemek tarafından sertifikanın verildiği. Bu sahte bir sertifika gösterir `cert-manager` istek doğru şekilde işlenir ve bir sertifika sağlayıcıdan alınan:

![Şimdi hazırlama sertifika şifreleme](media/ingress/staging-certificate.png)

Şimdi kullanmak için şifrelemek değiştirdiğinizde `prod` yerine `staging`, aşağıdaki örnekte gösterildiği gibi şimdi şifrelemek tarafından verilen, güvenilen bir sertifika kullanılır:

![Şimdi sertifika şifreleme](media/ingress/certificate.png)

Demo uygulamayı web tarayıcısında gösterilmektedir:

![Bir uygulama örneği](media/ingress/app-one.png)

Şimdi ekleyin */hello-world-two* FQDN yolu gibi *https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two*. Özel başlıklı ikinci tanıtım uygulaması gösterilmiştir:

![İki uygulama örneği](media/ingress/app-two.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede, giriş bileşenleri, sertifikalar ve örnek uygulamaları yüklemek için Helm kullanılır. Kubernetes kaynak sayısı, bir Helm grafiği dağıttığınızda oluşturulur. Bu kaynaklar, pod'ları, dağıtımlar ve hizmetleri içerir. Temizlemek için önce sertifika kaynakları kaldırın:

```console
kubectl delete -f certificates.yaml
kubectl delete -f cluster-issuer.yaml
```

Şimdi Helm sürümlerle listesinde `helm list` komutu. Adlı grafiklerde Ara *ngınx giriş*, *Sertifika Yöneticisi*, ve *aks-helloworld*aşağıdaki örnek çıktıda gösterildiği gibi:

```
$ helm list

NAME                    REVISION    UPDATED                     STATUS      CHART                   APP VERSION NAMESPACE
waxen-hamster           1           Tue Oct 16 17:44:28 2018    DEPLOYED    nginx-ingress-0.22.1    0.15.0      kube-system
alliterating-peacock    1           Tue Oct 16 18:03:11 2018    DEPLOYED    cert-manager-v0.3.4     v0.3.2      kube-system
mollified-armadillo     1           Tue Oct 16 18:04:53 2018    DEPLOYED    aks-helloworld-0.1.0                default
wondering-clam          1           Tue Oct 16 18:04:56 2018    DEPLOYED    aks-helloworld-0.1.0                default
```

Sürümlerle Sil `helm delete` komutu. Aşağıdaki örnek, NGINX giriş dağıtım ve Sertifika Yöneticisi iki örnek AKS hello world uygulaması siler.

```
$ helm delete waxen-hamster alliterating-peacock mollified-armadillo wondering-clam

release "billowing-kitten" deleted
release "loitering-waterbuffalo" deleted
release "flabby-deer" deleted
release "linting-echidna" deleted
```

Ardından, AKS Merhaba Dünya uygulaması için Helm deposu kaldırın:

```console
helm repo remove azure-samples
```

Örnek uygulamalara yönelik trafiği yönlendiren giriş rota kaldırın:

```console
kubectl delete -f hello-world-ingress.yaml
```

Son olarak, statik genel IP adresi için giriş denetleyicisini oluşturduğunuz kaldırın. Sağlayın, *MC_* küme kaynak grubu adı bu makalede, ilk adımda gibi elde edilen *MC_myResourceGroup_myAKSCluster_eastus*:

```azurecli
az network public-ip delete --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, AKS dış bazı bileşenleri dahil. Bu bileşenler hakkında daha fazla bilgi edinmek için şu proje sayfalara bakın:

- [Helm CLI][helm-cli]
- [NGINX giriş denetleyicisine][nginx-ingress]
- [Sertifika Yöneticisi][cert-manager]

Aşağıdakileri de yapabilirsiniz:

- [Dış ağ bağlantısına sahip bir temel giriş denetleyicisi oluşturun][aks-ingress-basic]
- [HTTP uygulama yönlendirme eklentiyi etkinleştir][aks-http-app-routing]
- [Bir özel, iç ağ ve IP adresi kullanan bir giriş denetleyicisini oluşturma][aks-ingress-internal]
- [Kendi TLS sertifikalarını kullanan bir giriş denetleyicisini oluşturma][aks-ingress-own-tls]
- [Dinamik genel IP ile bir giriş denetleyicisi oluşturmak ve TLS sertifikalarını otomatik olarak oluşturmak için şimdi şifreleme yapılandırma][aks-ingress-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm
[cert-manager]: https://github.com/jetstack/cert-manager
[cert-manager-certificates]: https://cert-manager.readthedocs.io/en/latest/reference/certificates.html
[cert-manager-cluster-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/clusterissuers.html
[cert-manager-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/issuers.html
[lets-encrypt]: https://letsencrypt.org/
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
[helm-install]: https://docs.helm.sh/using_helm/#installing-helm

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-own-tls]: ingress-own-tls.md
