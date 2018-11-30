---
title: İşleç en iyi uygulamalar - Gelişmiş Zamanlayıcı Özellikleri Azure Kubernetes Hizmetleri (AKS)
description: İçin Gelişmiş Zamanlayıcı taints ve tolerations, düğüm seçicileri ve benzeşim veya arası pod benzeşimi ve benzeşim karşıtlığı Azure Kubernetes Service (AKS) kullanarak küme işleci en iyi uygulamaları öğrenin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: iainfou
ms.openlocfilehash: 3fa2183a5bb3239059c349e8417aeb52553829cf
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52428987"
---
# <a name="best-practices-for-advanced-scheduler-features-in-azure-kubernetes-service-aks"></a>Gelişmiş Zamanlayıcı Özellikleri Azure Kubernetes Service (AKS) için en iyi uygulamalar

Azure Kubernetes Service (AKS) kümelerini yönetirken, genellikle takımlar ve iş yüklerini yalıtmak gerekir. Kubernetes Zamanlayıcı belirli düğümler üzerinde hangi pod'ların zamanlanması veya nasıl küme genelinde uygulamaların uygun şekilde için çok pod dağıtılması denetlemenize olanak sağlayan gelişmiş özellikler sunar. 

Bu en iyi yöntemler makalesi Gelişmiş Kubernetes küme operatörleri için zamanlama özellikleri ele alınmaktadır. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Kullanım taints ve hangi pod'ların sınırlamak için tolerations düğümlerinde zamanlanabilir
> * Düğüm Seçici veya düğüm benzeşim belirli düğümler üzerinde çalıştırılacak tercih pod'ları için
> * Uzaklıkta veya grup birlikte pod'lar arası pod benzeşim veya benzeşim karşıtlığı ile bölme

## <a name="provide-dedicated-nodes-using-taints-and-tolerations"></a>Adanmış düğümler taints tolerations ile sağlayın

**En iyi uygulama kılavuzunu** -kaynak kullanımı yoğun uygulamalar için giriş denetleyicileri gibi belirli düğümlere erişimi sınırlayın. Onları gerektiren iş yükleri için kullanılabilir düğüm kaynakları saklamak ve diğer iş yüklerinin düğümlerde zamanlama izin verme.

AKS kümenizi yeniden oluşturduğunuzda, GPU desteğine veya çok sayıda güçlü CPU'lara düğümleri dağıtabilirsiniz. Bu düğümler, genellikle machine learning (ML) veya yapay zeka (AI) gibi büyük veri işleme iş yükleri için kullanılır. Bu donanım türü genellikle dağıtmak için bir pahalı düğüm kaynağı olduğundan, bu düğümlere zamanlanabilen iş yüklerinin sınırlayın. Bunun yerine, giriş hizmetleri çalıştırmak ve diğer iş yükleri önlemek için kümedeki bazı düğümler ayırmak isteyebilirsiniz.

Kubernetes Zamanlayıcı taints ve tolerations düğümler üzerinde hangi iş yüklerini çalıştırabilirsiniz kısıtlamak için kullanabilirsiniz.

* A **taint** belirli pod'ların yalnızca bunlara zamanlanabilir belirten bir düğüm uygulanır.
* A **toleration** izin veren bir pod uygulanır *tolere* düğümün taint.

Bir AKS kümesi için bir pod dağıttığınızda, Kubernetes düğümlerinde burada bir toleration taint ile hizalanır pod yalnızca zamanlar. Örneğin, destekleyen bir nodepool AKS kümenizde düğümleri GPU ile sahip olduğunuz varsayılır. Adı gibi tanımladığınız *gpu*, zamanlama için bir değer daha sonra. Bu değeri ayarlamanız *NoSchedule*, pod uygun toleration tanımlamıyorsa, Kubernetes Zamanlayıcı pod'ların düğümde zamanlayamazsınız.

```console
kubectl taint node aks-nodepool1 gpu:NoSchedule
```

Düğümlere uygulanması bir taint ile ardından bir toleration düğümlerde zamanlama izin veren pod belirtiminde tanımlarsınız. Aşağıdaki örnek tanımlar `key: gpu` ve `effect: NoSchedule` önceki adımda düğüme uygulanan taint tolerans:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: tf-mnist
spec:
  containers:
  - name: tf-mnist
    image: microsoft/samples-tf-mnist-demo:gpu
  resources:
    requests:
      cpu: 0.5
      memory: 2Gi
    limits:
      cpu: 4.0
      memory: 16Gi
  tolerations:
  - key: "gpu"
    operator: "Equal"
    value: "value"
    effect: "NoSchedule"
```

Ne zaman bu pod dağıtıldığında kullanmak gibi `kubectl apply -f gpu-toleration.yaml`, Kubernetes düğümlerinde pod başarıyla uygulandı taint ile zamanlayabilirsiniz. Bu mantıksal yalıtımı sayesinde bir küme içindeki kaynaklara erişimi denetler.

Taints uyguladığınızda, uygulama geliştiriciler ve sahipleri izin vermek üzere kendi dağıtımlarda gerekli tolerations tanımlamak için çalışır.

Taints ve tolerations hakkında daha fazla bilgi için bkz. [taints ve tolerations uygulama][k8s-taints-tolerations].

## <a name="control-pod-scheduling-using-node-selectors-and-affinity"></a>Denetim pod düğüm seçicileri ve benzeşimi'ni kullanarak zamanlama

**En iyi uygulama kılavuzunu** - düğüm seçiciler, düğümü benzeşimini kullanarak düğümlerinde pod'ları zamanlama denetlemek veya benzeşim'arası pod. Bu ayarlar mantıksal iş yükleri gibi donanım düğümünde yalıtmak Kubernetes Zamanlayıcı sağlar.

Taints ve tolerations sabit bir sonlandırma - kaynaklarla mantıksal olarak ayırmak için kullanılan bir düğümün taint pod tolere değil, düğüm üzerinde zamanlanmış değil. Alternatif bir yaklaşım, düğüm Seçici kullanmaktır. SSD depolaması yerel olarak bağlı veya büyük miktarda bellek belirtin ve ardından pod belirtiminde düğüm Seçicisi tanımlamak için düğümleri gibi etiketleyin. Kubernetes, ardından bu pod'ların eşleşen bir düğümde zamanlar. Tolerations, eşleşen bir düğüm Seçici olmadan pod'ların etiketli düğümler üzerinde zamanlanabilir. Bu davranış, kullanılmamış kaynakları kullanmak için düğümlerde sağlar ancak eşleşen düğüm Seçicisi tanımlayan pod'ların öncelik verir.

Yüksek miktarda bellek düğümlerinin bir örneğe göz atalım. Bu düğümler, yüksek miktarda bellek isteği pod'ları tercih verebilirsiniz. Boşta kaynakları sit yoksa emin olmak için bunlar ayrıca diğer pod'ların çalışmasına izin verin.

```console
kubectl label node aks-nodepool1 hardware:highmem
```

Ardından bir pod belirtimi ekler `nodeSelector` özellik etiketi ile eşleşen bir düğüm Seçicisi tanımlamak için bir düğümde ayarlayın:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: tf-mnist
spec:
  containers:
  - name: tf-mnist
    image: microsoft/samples-tf-mnist-demo:gpu
    resources:
      requests:
        cpu: 0.5
        memory: 2Gi
      limits:
        cpu: 4.0
        memory: 16Gi
    nodeSelector:
      hardware: highmem
```

Bu zamanlayıcı seçeneklerini kullandığınızda, uygulama geliştiricilerinin ve sahipleri izin vermek üzere doğru şekilde kendi pod özellikleri tanımlamak için çalışır.

Düğüm seçiciler kullanma hakkında daha fazla bilgi için bkz. [atama pod düğümlere][k8s-node-selector].

### <a name="node-affinity"></a>Düğüm benzeşim

Bir düğüm Seçicisi, pod'ların verilen bir düğüme atamak için temel bir yoludur. Daha fazla esneklik aracılığıyla kullanılabilir olan *düğüm benzeşim*. Düğüm benzeşim ile pod bir düğüm ile eşleştirilemiyor, ne olacağını tanımlar. Yapabilecekleriniz *gerektiren* Kubernetes Zamanlayıcı ile etiketlenmiş bir konağa bir pod eşleşir. Veya *tercih* eşleşme kullanılabilir değilse bir eşleşme ancak farklı bir ana bilgisayarda zamanlanmış pod izin verin.

Aşağıdaki örnek düğümü benzeşimini ayarlar *requiredDuringSchedulingIgnoredDuringExecution*. Bu benzeşimi, bir düğüm ile eşleşen bir etiket kullanmayı Kubernetes zamanlama gerektirir. Hiçbir düğüm varsa, devam etmek için zamanlama için beklenecek pod sahiptir. Başka bir düğümde zamanlanması pod izin vermek için bunun yerine değeri ayarlayabilirsiniz *preferredDuringScheduledIgnoreDuringExecution*:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: tf-mnist
spec:
  containers:
  - name: tf-mnist
    image: microsoft/samples-tf-mnist-demo:gpu
    resources:
      requests:
        cpu: 0.5
        memory: 2Gi
      limits:
        cpu: 4.0
        memory: 16Gi
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
      - matchExpressions:
        - key: hardware
          operator: In
          values: highmem
```

*IgnoredDuringExecution* ayarı parça belirtir düğümü değişiklik etiketler, pod düğümden çıkarılacak olmamalıdır. Kubernetes Zamanlayıcı zamanlanması, yeni pod'ların zaten düğümler üzerinde zamanlanmış pod yalnızca güncelleştirilen düğüme etiketleri kullanır.

Daha fazla bilgi için [benzeşimi'ni ve benzeşim karşıtlığı][k8s-affinity].

### <a name="inter-pod-affinity-and-anti-affinity"></a>Arası pod benzeşimi'ni ve benzeşim karşıtlığı

Mantıksal iş yüklerini yalıtmak Kubernetes Zamanlayıcı için son bir yaklaşım arası pod benzeşim veya benzeşim karşıtlığı kullanıyor. Bu pod'ları ayarları tanımlamak *olmamalıdır* zamanlanmış bir mevcut pod eşleşen bir düğüm veya bu bunlar *gereken* zamanlanmış. Varsayılan olarak, bir yineleme düğümleri arasında kümesini birden çok pod'ların zamanlamak Kubernetes Zamanlayıcı çalışır. Bu davranışı daha belirli kurallar tanımlayabilirsiniz.

İyi bir örnek de Redis önbelleği kullanan bir web uygulamasıdır. Kubernetes Zamanlayıcı çoğaltmaları düğümlere dağıtır istemek için pod benzeşim karşıtlığı kuralları kullanabilirsiniz. Bundan sonra her web uygulaması bileşeni, karşılık gelen bir önbellek olarak aynı ana bilgisayardaki zamanlandı emin olmak için işe benzeşim kuralları de yapabilirsiniz. Pod'ların düğümleri arasında dağıtılması, aşağıdaki örnekteki gibi görünür:

| **Düğüm 1** | **Düğüm 2** | **Düğüm 3** |
|------------|------------|------------|
| WebApp-1   | WebApp-2   | WebApp-3   |
| 1. önbellek    | Önbellek-2    | Önbellek 3    |

Daha karmaşık bir dağıtıma kullanımı göre düğüm Seçici veya düğüm benzeşim örnektir. Dağıtım sağlar, Kubernetes düğümlerinde pod'ların nasıl zamanlar üzerinde denetim ve mantıksal kaynakları yalıtma. Bir tam örneği bu web uygulaması için Redis cache örneği ile bkz [birlikte bulundurma, aynı düğümdeki pod'ların][k8s-pod-affinity].

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Gelişmiş Kubernetes Zamanlayıcı özelliklere odaklanan. AKS kümesi işlemleri hakkında daha fazla bilgi için aşağıdaki en iyi bakın:

* [Çok kiracılılık ve küme ayırma][aks-best-practices-scheduler]
* [Temel Kubernetes Zamanlayıcı Özellikleri][aks-best-practices-scheduler]
* [Kimlik doğrulama ve yetkilendirme][aks-best-practices-identity]

<!-- EXTERNAL LINKS -->
[k8s-taints-tolerations]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[k8s-node-selector]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[k8s-affinity]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity
[k8s-pod-affinity]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#always-co-located-in-the-same-node

<!-- INTERNAL LINKS -->
[aks-best-practices-scheduler]: operator-best-practices-scheduler.md
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md
[aks-best-practices-identity]: operator-best-practices-identity.md
