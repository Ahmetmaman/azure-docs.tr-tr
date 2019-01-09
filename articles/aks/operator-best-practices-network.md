---
title: İşleç en iyi uygulamalar - Azure Kubernetes Hizmetleri (AKS) ağ bağlantısı
description: Sanal ağ kaynakları ve bağlantı Azure Kubernetes Service (AKS) için küme işleci en iyi uygulamaları öğrenin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 12/10/2018
ms.author: iainfou
ms.openlocfilehash: 0ad6ab27a51cf082be71262b887a459f6c7cc906
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54101981"
---
# <a name="best-practices-for-network-connectivity-and-security-in-azure-kubernetes-service-aks"></a>Ağ bağlantısını ve güvenlik Azure Kubernetes Service (AKS) için en iyi uygulamalar

Oluşturma ve Azure Kubernetes Service (AKS) kümelerini yönetme gibi düğümler ve uygulamalar için ağ bağlantısı sağlar. Bu ağ kaynakları, IP adres aralığı, yük Dengeleyiciler ve giriş denetleyicileri içerir. Hizmet, uygulamalarınız için yüksek bir kalitesini korumak için planlama ve ardından bu kaynakları yapılandırma gerekir.

Bu en iyi yöntemler makalesi küme operatörleri için ağ bağlantısını ve güvenlik odaklanır. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * AKS temel ve Gelişmiş Ağ modlarında karşılaştırın
> * Gerekli IP adresi ve bağlantı için planlama
> * Yük Dengeleyiciler, giriş denetleyicileri veya bir web uygulaması güvenlik duvarları (WAF) kullanarak trafiği dağıtma
> * Güvenli bir küme düğümlerine bağlanma

## <a name="choose-the-appropriate-network-model"></a>Uygun ağ modeli seçin

**En iyi uygulama kılavuzunu** - var olan sanal ağları ya da şirket içi ağ ile tümleştirme için AKS ağ Gelişmiş kullanın. Bu ağ modeli, bir kuruluş ortamında kaynakları ve denetimlerin daha fazla ayrılmasını da sağlar.

Sanal ağlar, AKS düğümleri ve uygulamalarınıza erişmek için müşteriler için temel bağlantı sağlar. Sanal ağlar AKS kümeye dağıtmak için iki farklı yolu vardır:

* **Temel ağ** -Azure tarafından yönetilen sanal ağ kaynakları kümesi dağıtılır ve kullandığı [kubernetes] [ kubenet] Kubernetes eklentisi.
* **Gelişmiş Ağ** - mevcut bir sanal ağa dağıtılır ve kullandığı [Azure kapsayıcı ağ arabirimi (CNI)] [ cni-networking] Kubernetes eklentisi. Pod'ların diğer ağ hizmetleri veya şirket kaynaklarına yönlendirebilir tek tek IP'leri alır.

Kapsayıcı ağ arabirimi (CNI) bir ağ sağlayıcısına isteklerde kapsayıcı çalışma zamanı sağlayan bir satıcı nötr protokolüdür. Azure CNI pod'ların ve düğümler için IP adresleri de atar ve mevcut Azure sanal ağlarına bağlanma gibi IP adresi Yönetimi (IPAM) özellikleri sağlar. Her düğüm ve pod kaynak Azure sanal ağında bir IP adresi alır ve diğer kaynakları veya Hizmetleri ile iletişim kurmak için hiçbir ek yönlendirme gereklidir.

![Her tek bir Azure sanal ağa bağlanma köprüleri ile iki düğümü gösteren diyagram](media/operator-best-practices-network/advanced-networking-diagram.png)

Çoğu Üretim dağıtımı için Gelişmiş Ağ kullanmanız gerekir. Bu ağ modeli denetim ayrımı ve kaynaklarının yönetimini olanak sağlar. Güvenlik açısından bakıldığında, yönetmek ve bu kaynakları güvenli hale getirmek için farklı ekipler genellikle istersiniz. Mevcut Azure kaynakları, şirket içi kaynaklara veya diğer hizmetler doğrudan her pod için atanan IP adresleri üzerinden bağlanmanıza Gelişmiş Ağ olanak sağlar.

Gelişmiş Ağ kullandığınızda, bir AKS kümesi için ayrı bir kaynak grubundaki sanal ağ kaynağı olan. AKS hizmet sorumlusuna erişmek ve bu kaynakları yönetmek için izinleri verin. AKS kümesi tarafından kullanılan hizmet sorumlusunun en az olmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md#network-contributor) sanal ağınızdaki bir alt ağ üzerindeki izinleri. Tanımlamak istiyorsanız bir [özel rol](../role-based-access-control/custom-roles.md) yerleşik ağ Katılımcısı rolü kullanmak yerine, aşağıdaki izinler gereklidir:
  * `Microsoft.Network/virtualNetworks/subnets/join/action`
  * `Microsoft.Network/virtualNetworks/subnets/read`

AKS hizmet sorumlusu temsilci seçme hakkında daha fazla bilgi için bkz. [temsilci diğer Azure kaynaklarına erişimi][sp-delegation].

Her düğüm ve pod almak gibi kendi IP adresini, AKS alt ağlar için adres aralıklarını kullanıma planlayın. Alt ağ, her düğüm, pod'ların ve dağıttığınız ağ kaynaklarına IP adresleri sağlamak amacıyla büyük olmalıdır. Her bir AKS kümesi, kendi alt ağına yerleştirilmelidir. Azure'da şirket içi ya da eşlenen ağlara bağlantı izin vermek için çakışan IP adresi aralıkları ile var olan ağ kaynaklarını kullanmayın. Her düğümü ile temel ve Gelişmiş Ağ çalışan pod'ların sayısını için varsayılan limitleri vardır. Ölçeği artırma olayları veya Küme yükseltme işlemleri işlemek için ayrıca ek IP adresleri kullanılmak üzere atanmış bir alt ağ gerekir.

IP adresi hesaplamak için gereken bkz [Gelişmiş Yapılandırma AKS ağ][advanced-networking].

### <a name="basic-networking-with-kubenet"></a>Kubernetes ile temel ağ

Temel Ağ küme dağıtılmadan önce sanal ağları ayarlama gerektirmez ancak dezavantajları vardır:

* Düğümleri ve pod'ların farklı IP alt ağlara yerleştirilir. Kullanıcı tanımlı yönlendirme (UDR) ve IP iletme pod'ların ve düğümler arasındaki trafiği yönlendirmek için kullanılır. Bu ek yönlendirme ağ performansı azaltır.
* Var olan şirket içi ağlara ya da diğer Azure Sanal Ağları için eşleme bağlantıları karmaşıktır.

Temel ağ gibi sanal ağ ve alt ağları AKS kümeden ayrı ayrı oluşturmanız gerekmez, küçük geliştirme ve test iş yükleri için uygundur. Düşük trafiğe sahip ya da kaldırma ve iş yüklerini kapsayıcılarına kaydırma için basit Web siteleri temel ağ ile dağıtılan AKS kümeleri basitliğinin de yararlanabilir. Çoğu Üretim dağıtımı için planlama ve Gelişmiş ağ kullanın.

## <a name="distribute-ingress-traffic"></a>Giriş trafiği dağıtma

**En iyi uygulama kılavuzunu** - uygulamalarınızı HTTP veya HTTPS trafiği dağıtmak için giriş kaynakları ve denetleyicileri kullanın. Giriş denetleyicileri, bir normal Azure load balancer'a göre ek özellikler sağlamak ve yerel Kubernetes kaynakları yönetilebilir.

AKS kümenizde uygulamaları müşteri trafiğini Azure yük dengeleyici dağıtabilirsiniz, ancak ne bu trafiği anladığı içinde sınırlıdır. Bir yük dengeleyici kaynağını 4 katmanında çalışır ve protokol veya bağlantı noktalarında trafiğini dağıtır. HTTP veya HTTPS kullanan çoğu web uygulaması Kuberenetes giriş kaynakları ve 7. katmanda çalışan denetleyiciler kullanmanız gerekir. Giriş uygulama ve tutamacı TLS/SSL sonlandırma URL'sini temel alarak trafiği dağıtabilirsiniz. Bu özelliği, kullanıma ve eşleme IP adresi sayısını da azaltır. Bir yük dengeleyici ile her bir uygulama genellikle atanan ve AKS Küme hizmeti eşlenmiş bir genel bir IP adresi gerekir. Bir giriş kaynağı ile tek bir IP adresi trafiği birden çok uygulama dağıtabilirsiniz.

![Bir AKS kümesinde giriş trafik akışını gösteren diyagram](media/operator-best-practices-network/aks-ingress.png)

 Giriş için iki bileşeni vardır:

 * Bir giriş *kaynak*, ve
 * Bir giriş *denetleyicisi*

Bir YAML bildirimi giriş kaynaktır `kind: Ingress` tanımlayan konak, sertifikalar ve kuralları trafiği yönlendirmek için AKS kümenizde çalışan hizmetler. Aşağıdaki örnek YAML bildirim trafiği için dağıtmanız gerekir *myapp.com* iki hizmet birine *blogservice* veya *storeservice*. Müşterinin bir hizmete yönlendirilir veya diğer erişim URL'sini temel alarak.

```yaml
kind: Ingress
metadata:
 name: myapp-ingress
   annotations: kubernetes.io/ingress.class: "PublicIngress"
spec:
 tls:
 - hosts:
   - myapp.com
   secretName: myapp-secret
 rules:
   - host: myapp.com
     http:
      paths:
      - path: /blog
        backend:
         serviceName: blogservice
         servicePort: 80
      - path: /store
        backend:
         serviceName: storeservice
         servicePort: 80
```

Bir AKS düğümü üzerinde çalışan ve gelen istekler için izleyen bir arka plan programı bir giriş denetleyicisidir. Trafik giriş kaynağında tanımlanan kurallar temel alınarak dağıtılır. En yaygın giriş denetleyicisine dayanır [NGINX]. AKS değil kısıtlamak, belirli bir denetleyiciye gibi diğer denetleyicileri kullanabilmeniz [dağılımı][contour], [HAProxy][haproxy], veya [ Traefik][traefik].

Aşağıdaki nasıl yapılır kılavuzlarını da dahil olmak üzere, giriş için birçok senaryo vardır:

* [Dış ağ bağlantısına sahip bir temel giriş denetleyicisi oluşturun][aks-ingress-basic]
* [Bir özel, iç ağ ve IP adresi kullanan bir giriş denetleyicisini oluşturma][aks-ingress-internal]
* [Kendi TLS sertifikalarını kullanan bir giriş denetleyicisini oluşturma][aks-ingress-own-tls]
* Şimdi şifreleme TLS sertifikalarını otomatik olarak oluşturmak için kullandığı bir giriş denetleyicisine oluşturma [dinamik genel IP adresi ile] [ aks-ingress-tls] veya [bir statik genel IP adresi ile][aks-ingress-static-tls]

## <a name="secure-traffic-with-a-web-application-firewall-waf"></a>Bir web uygulaması Güvenlik Duvarı (WAF) ile trafiğin güvenliğini sağlama

**En iyi uygulama kılavuzunu** - gelen trafik için olası saldırılara karşı taramak için bir web uygulaması Güvenlik Duvarı (WAF) gibi kullanın [Azure için Barracuda WAF] [ barracuda-waf] veya Azure Application Gateway. Bu daha gelişmiş ağ kaynaklarını da yalnızca HTTP ve HTTPS bağlantıları ya da temel SSL sonlandırma ötesinde trafiği yönlendirebilirsiniz.

Hizmetler ve uygulamalar için trafiği dağıtan bir giriş denetleyicisine genellikle bir Kubernetes AKS kümenizde kaynaktır. Denetleyici bir arka plan programı gibi bir AKS düğümü üzerinde çalışan ve CPU, bellek ve ağ bant genişliği gibi düğümün kaynaklardan bazıları kullanır. Daha büyük ortamlarda, genellikle bazı Bu trafik yönlendirme veya TLS sonlandırma AKS kümesi dışında bir ağ kaynağına boşaltma istersiniz. Ayrıca, gelen trafiği için olası saldırılara karşı tarama istiyorsunuz.

![Azure uygulama ağ geçidi gibi web uygulaması Güvenlik Duvarı (WAF) koruyabilir ve AKS kümenizin trafiği dağıtma](media/operator-best-practices-network/web-application-firewall-app-gateway.png)

Bir web uygulaması Güvenlik Duvarı (WAF), gelen trafiği filtreleyerek, ek bir güvenlik katmanı sağlar. Açık Web uygulaması güvenlik Project (OWASP) saldırıları gibi platformlar arası komut dosyası çalıştırma veya tanımlama bilgisinin zehirleme izlemek üzere kurallar kümesi sağlar. [Azure Application Gateway] [ app-gateway] AKS ile tümleştirebileceğiniz bir WAF kümeleri trafiği, AKS kümesi ve uygulamalarınızı ulaşmadan önce bu güvenlik özellikleri sağlamak için olan. Mevcut yatırımlardan veya uzmanlığı, belirli bir üründe kullanmaya devam edebilirsiniz şekilde diğer üçüncü taraf çözümleri de bu işlevler gerçekleştirin.

Yük Dengeleyici veya giriş kaynakları, daha fazla trafik dağılımı iyileştirmek için AKS kümenizin çalışmaya devam eder. Uygulama ağ geçidi olarak bir giriş denetleyicisine kaynak tanımıyla merkezi olarak yönetilebilir. Başlamak için [bir uygulama ağ geçidi giriş denetleyicisine oluşturma][app-gateway-ingress].

## <a name="securely-connect-to-nodes-through-a-bastion-host"></a>Düğümleri Burcu ana bilgisayarı arasında güvenli bir şekilde bağlanma

**En iyi uygulama kılavuzunu** -uzaktan bağlantı, AKS düğümleri sunmayın. Atlama kutusunda Yönetim sanal ağ veya Burcu ana bilgisayarı oluşturun. Kale ana bilgisayarı güvenli bir şekilde, AKS kümesinde uzak yönetim görevleri için trafiği yönlendirmek için kullanın.

Azure Yönetim Araçları'nı kullanarak aks'deki çoğu işlemi tamamlanabilir ya da Kubernetes API sunucusu üzerinden. AKS düğümleri, genel internet'e bağlı olmayan ve yalnızca özel bir ağda kullanılabilir. Düğümlere bağlanma ve bakımının veya sorunlarını gidermek için bağlantılarınızı savunma ana bilgisayar üzerinden yönlendirmek veya kutusunu atlayabilirsiniz. Bu konak, AKS kümesi sanal ağa güvenli bir şekilde eşlenen ayrı Yönetim sanal ağ içinde olmalıdır.

![Kale ana bilgisayarı kullanarak AKS düğümlerine bağlanmak veya atlama kutusunu](media/operator-best-practices-network/connect-using-bastion-host-simplified.png)

Kale ana bilgisayarı için yönetim ağ, çok güvenli hale getirilmelidir. Kullanım bir [Azure ExpressRoute] [ expressroute] veya [VPN ağ geçidi] [ vpn-gateway] bir şirket içi ağa bağlanma ve ağ güvenliği'ni kullanarak erişimi denetleme gruplar.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, ağ bağlantısını ve güvenlik odaklı. Kubernetes temel ağ bilgileri hakkında daha fazla bilgi için bkz: [ağ Azure Kubernetes Service (AKS) uygulamaları için kavramlar][aks-concepts-network]

<!-- LINKS - External -->
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet
[app-gateway-ingress]: https://github.com/Azure/application-gateway-kubernetes-ingress
[Nginx]: https://www.nginx.com/products/nginx/kubernetes-ingress-controller
[contour]: https://github.com/heptio/contour
[haproxy]: https://www.haproxy.org
[traefik]: https://github.com/containous/traefik
[barracuda-waf]: https://www.barracuda.com/products/webapplicationfirewall/models/5

<!-- INTERNAL LINKS -->
[aks-concepts-network]: concepts-network.md
[sp-delegation]: kubernetes-service-principal.md#delegate-access-to-other-azure-resources
[expressroute]: ../expressroute/expressroute-introduction.md
[vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-own-tls]: ingress-own-tls.md
[app-gateway]: ../application-gateway/overview.md
[advanced-networking]: configure-advanced-networking.md
