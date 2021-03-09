---
title: Azure Kubernetes hizmetlerinde (AKS) portalı kullanarak sanal düğümler oluşturma
description: Bir Azure Kubernetes hizmeti (AKS) kümesi oluşturmak için Azure portal kullanarak pods 'yi çalıştırmak için sanal düğümleri kullanan bir Azure
services: container-service
ms.topic: conceptual
ms.date: 05/06/2019
ms.custom: references_regions, devx-track-azurecli
ms.openlocfilehash: 06a3e7263b2e03cfc37f7ba3c733e07536b5d473
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102501813"
---
# <a name="create-and-configure-an-azure-kubernetes-services-aks-cluster-to-use-virtual-nodes-in-the-azure-portal"></a>Azure portal sanal düğümleri kullanmak için bir Azure Kubernetes hizmeti (AKS) kümesi oluşturma ve yapılandırma

Bu makalede, sanal düğümlerin etkinleştirildiği sanal ağ kaynaklarını ve AKS kümesini oluşturmak ve yapılandırmak için Azure portal nasıl kullanılacağı gösterilmektedir.

> [!NOTE]
> [Bu makale](virtual-nodes.md) , sanal düğümleri kullanan bölge kullanılabilirliğine ve sınırlamalara genel bir bakış sunar.

## <a name="before-you-begin"></a>Başlamadan önce

Sanal düğümler Azure Container Instances (aci) ve aks kümesinde çalışan Pod 'ler arasında ağ iletişimini sağlar. Bu iletişim sağlamak için bir sanal ağ alt ağı oluşturulur ve temsilci izinleri atanır. Sanal düğümler yalnızca *Gelişmiş* ağ (Azure CNI) kullanılarak oluşturulan aks kümeleriyle çalışır. Varsayılan olarak, AKS kümeleri *temel* ağ (kubenet) ile oluşturulur. Bu makalede, bir sanal ağ ve alt ağ oluşturma ve ardından Gelişmiş ağ kullanan bir AKS kümesi dağıtma işlemleri gösterilir.

Daha önce ACI kullandıysanız, hizmet sağlayıcısını aboneliğiniz ile kaydedin. Aşağıdaki örnekte gösterildiği gibi [az Provider List][az-provider-list] komutunu kullanarak aci sağlayıcı kaydının durumunu denetleyebilirsiniz:

```azurecli-interactive
az provider list --query "[?contains(namespace,'Microsoft.ContainerInstance')]" -o table
```

Aşağıdaki örnek çıktıda gösterildiği gibi, *Microsoft. Containerınstance* sağlayıcısı *kayıtlı* olarak rapor etmelidir:

```output
Namespace                    RegistrationState    RegistrationPolicy
---------------------------  -------------------  --------------------
Microsoft.ContainerInstance  Registered           RegistrationRequired
```

Sağlayıcı *Notregistered* olarak gösteriyorsa, aşağıdaki örnekte gösterildiği gibi [az Provider Register][az-provider-register] kullanarak sağlayıcıyı kaydedin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerInstance
```

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

Azure Portal sol üst köşesinde **kaynak oluştur**  >  **Kubernetes hizmeti**' ni seçin.

**Temel Bilgiler** sayfasında aşağıdaki seçenekleri yapılandırın:

- *PROJE AYRINTILARI*: Bir Azure aboneliği seçtikten sonra bir Azure kaynak grubu seçin veya *myResourceGroup* adıyla yeni bir tane oluşturun. **Kubernetes kümesi adı** alanına *myAKSCluster* gibi bir ad girin.
- *KÜME AYRINTILARI*: AKS kümesi için bölge, Kubernetes sürümü ve DNS adı ön eki seçin.
- *BIRINCIL düğüm havuzu*: aks düğümleri IÇIN bir VM boyutu seçin. AKS kümesi dağıtıldıktan sonra, sanal makine boyutu **değiştirilemez**.
     - Kümeye dağıtılacak düğüm sayısını seçin. Bu makalede, **düğüm sayısını** *1* olarak ayarlayın. Küme dağıtıldıktan sonra düğüm sayısı **ayarlanabilir**.

**İleri: ölçek** öğesine tıklayın.

**Ölçek** sayfasında, **sanal düğümler** altında *etkin* ' i seçin.

![AKS kümesi oluşturma ve sanal düğümleri etkinleştirme](media/virtual-nodes-portal/enable-virtual-nodes.png)

Varsayılan olarak, bir küme kimliği oluşturulur. Bu küme kimliği, küme iletişimi ve diğer Azure hizmetleriyle tümleştirme için kullanılır. Varsayılan olarak, bu küme kimliği yönetilen bir kimliktir. Daha fazla bilgi için bkz. [yönetilen kimlikleri kullanma](use-managed-identity.md). Ayrıca, bir hizmet sorumlusunu küme kimliğiniz olarak kullanabilirsiniz.

Küme, Gelişmiş ağ için de yapılandırılır. Sanal düğümler kendi Azure sanal ağ alt ağını kullanacak şekilde yapılandırılır. Bu alt ağ, AKS kümesi arasında Azure kaynaklarını bağlama izinlerine temsilci seçti. Henüz temsilci alt ağınız yoksa, Azure portal Azure sanal ağını ve alt ağını sanal düğümlerle kullanılmak üzere oluşturur ve yapılandırır.

**Gözden geçir ve oluştur**’u seçin. Doğrulama tamamlandıktan sonra **Oluştur**' u seçin.

AKS kümesinin oluşturulması ve kullanıma hazır olması birkaç dakika sürer.

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bir Kubernetes kümesini yönetmek için Kubernetes komut satırı istemcisi [kubectl][kubectl]’i kullanın. `kubectl` istemcisi Azure Cloud Shell’de önceden yüklüdür.

Cloud Shell açmak için, bir kod bloğunun sağ üst köşesinden **dene** ' yi seçin. Ayrıca, ' a giderek ayrı bir tarayıcı sekmesinde Cloud Shell de başlatabilirsiniz [https://shell.azure.com/bash](https://shell.azure.com/bash) . **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

[az aks get-credentials][az-aks-get-credentials] komutunu kullanarak, `kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırın. Aşağıdaki örnek *myResourceGroup* adlı kaynak grubu içindeki *myAKSCluster* adlı kümenin kimlik bilgilerini alır:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın.

```console
kubectl get nodes
```

Aşağıdaki örnek çıktıda, oluşturulan tek VM düğümü ve Linux için sanal düğüm, *sanal düğüm-aci-Linux* sanal düğümü gösterilmektedir:

```output
NAME                           STATUS    ROLES     AGE       VERSION
virtual-node-aci-linux         Ready     agent     28m       v1.11.2
aks-agentpool-14693408-0       Ready     agent     32m       v1.11.2
```

## <a name="deploy-a-sample-app"></a>Örnek uygulama dağıtma

Azure Cloud Shell, `virtual-node.yaml` aşağıdaki YAML 'de adlı bir dosya oluşturun ve kopyalayın. Düğüm üzerinde kapsayıcıyı zamanlamak için bir [Nodeselector][node-selector] ve [toleranation][toleration] tanımlanmıştır. Bu ayarlar, Pod 'ın sanal düğümde zamanlanmasını sağlar ve özelliğin başarıyla etkinleştirildiğini doğrulayın.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aci-helloworld
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aci-helloworld
  template:
    metadata:
      labels:
        app: aci-helloworld
    spec:
      containers:
      - name: aci-helloworld
        image: mcr.microsoft.com/azuredocs/aci-helloworld
        ports:
        - containerPort: 80
      nodeSelector:
        kubernetes.io/role: agent
        beta.kubernetes.io/os: linux
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Exists
```

Uygulamayı [kubectl Apply][kubectl-apply] komutuyla çalıştırın.

```azurecli-interactive
kubectl apply -f virtual-node.yaml
```

[Kubectl Get Pod][kubectl-get] komutunu bağımsız değişkenle birlikte kullanarak `-o wide` bir pod ve zamanlanan düğüm listesini çıkış. `virtual-node-helloworld`Pod 'ın düğüm üzerinde zamanlandığına dikkat edin `virtual-node-linux` .

```console
kubectl get pods -o wide
```

```output
NAME                                     READY     STATUS    RESTARTS   AGE       IP           NODE
virtual-node-helloworld-9b55975f-bnmfl   1/1       Running   0          4m        10.241.0.4   virtual-node-aci-linux
```

Pod 'a sanal düğümlerle kullanılmak üzere atanan Azure sanal ağ alt ağından bir iç IP adresi atanır.

> [!NOTE]
> Azure Container Registry depolanan görüntüleri kullanıyorsanız, [bir Kubernetes gizli anahtarını yapılandırın ve kullanın][acr-aks-secrets]. Sanal düğümlerin geçerli sınırlaması, tümleşik Azure AD hizmet sorumlusu kimlik doğrulamasını kullanamıyoruz. Gizli anahtar kullanmıyorsanız, sanal düğümlerde zamanlanan Pod 'ler başlatılamaz ve hatayı bildirebilir `HTTP response status code 400 error code "InaccessibleImage"` .

## <a name="test-the-virtual-node-pod"></a>Sanal düğüm Pod 'u test etme

Sanal düğümde çalışan Pod 'u test etmek için, bir web istemcisiyle tanıtım uygulamasına gidin. Pod 'a bir iç IP adresi atandığında, bu bağlantıyı AKS kümesindeki başka bir pod 'tan hızlıca test edebilirsiniz. Bir test Pod oluşturun ve buna bir terminal oturumu ekleyin:

```console
kubectl run -it --rm virtual-node-test --image=debian
```

Şunu `curl` kullanarak Pod 'a yüklensin `apt-get` :

```console
apt-get update && apt-get install -y curl
```

Artık, kullanarak Pod 'nizin adresine erişin `curl` *http://10.241.0.4* . Önceki komutta gösterilen kendi iç IP adresini belirtin `kubectl get pods` :

```console
curl -L http://10.241.0.4
```

Demo uygulaması aşağıdaki sıkıştırılmış örnek çıktıda gösterildiği gibi görüntülenir:

```output
<html>
<head>
  <title>Welcome to Azure Container Instances!</title>
</head>
[...]
```

İle test Pod 'unuzla Terminal oturumunu kapatın `exit` . Oturumunuz sona erdikten sonra Pod silinir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, sanal düğümde bir pod zamanlandı ve özel, dahili bir IP adresi atandı. Bunun yerine, bir yük dengeleyici veya giriş denetleyicisi aracılığıyla bir hizmet dağıtımı oluşturup Pod 'inize trafik yönlendirebilirsiniz. Daha fazla bilgi için bkz. [AKS 'de temel giriş denetleyicisi oluşturma][aks-basic-ingress].

Sanal düğümler, AKS 'teki bir ölçeklendirme çözümünün bir bileşenidir. Çözümleri ölçeklendirme hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Kubernetes yatay Pod otomatik Scaler 'ı kullanma][aks-hpa]
- [Kubernetes kümesi otomatik Scaler 'ı kullanma][aks-cluster-autoscaler]
- [Sanal düğümler için otomatik ölçeklendirme örneğine göz atın][virtual-node-autoscale]
- [Sanal Kubelet açık kaynak kitaplığı hakkında daha fazla bilgi edinin][virtual-kubelet-repo]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[aks-github]: https://github.com/azure/aks/issues]
[virtual-node-autoscale]: https://github.com/Azure-Samples/virtual-node-autoscale
[virtual-kubelet-repo]: https://github.com/virtual-kubelet/virtual-kubelet
[acr-aks-secrets]: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

<!-- LINKS - internal -->
[aks-network]: ./configure-azure-cni.md
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[aks-hpa]: tutorial-kubernetes-scale.md
[aks-cluster-autoscaler]: cluster-autoscaler.md
[aks-basic-ingress]: ingress-basic.md
[az-provider-list]: /cli/azure/provider#az-provider-list
[az-provider-register]: /cli/azure/provider#az-provider-register
