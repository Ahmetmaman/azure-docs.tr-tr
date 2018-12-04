---
title: Azure Kubernetes Hizmetleri (AKS) Azure CLI kullanarak sanal düğümler oluştur
description: Pod'ları çalıştırmak için sanal düğümü kullanan Azure Kubernetes Hizmetleri (AKS) kümesini oluşturmak için Azure CLI'yı kullanmayı öğrenin.
services: container-service
author: iainfoulds
ms.service: container-service
ms.date: 12/03/2018
ms.author: iainfou
ms.openlocfilehash: ee16165352edbacddac0c91f1ff68109982577de
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856755"
---
# <a name="create-and-configure-an-azure-kubernetes-services-aks-cluster-to-use-virtual-nodes-using-the-azure-cli"></a>Oluşturma ve Azure CLI kullanarak sanal düğümü kullanmak için Azure Kubernetes Hizmetleri (AKS) kümesi yapılandırma

Uygulama iş yüklerini bir Azure Kubernetes Service (AKS) kümesi içinde hızlı bir şekilde ölçeklendirmek için sanal düğümü kullanabilirsiniz. Sanal düğüm ile pod'ların hızlı sağlama sahip ve yalnızca yürütme zamanları için saniye başına ödeme yaparsınız. Ek pod'lar çalıştırmak için VM hesaplama düğümlerini dağıtmak için Kubernetes küme ölçeklendiriciyi için beklemenize gerek yoktur. Bu makalede oluşturmak ve AKS kümesi ve sanal ağ kaynaklarını yapılandırmak ve ardından sanal düğümü etkinleştirme gösterilmektedir.

> [!IMPORTANT]
> AKS için sanal düğümü şu anda **Önizleme**. Önizlemeler, [ek kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="before-you-begin"></a>Başlamadan önce

Sanal düğümler ACI çalıştırma pod'ların ve AKS kümesi arasındaki ağ iletişimini etkinleştirin. Bu iletişim sağlamak için bir sanal ağ alt ağı oluşturulur ve temsilci izinleri atanır. Yalnızca iş ile AKS küme kullanılarak oluşturulan sanal düğümü *Gelişmiş* ağ. Varsayılan olarak, AKS kümeleri ile oluşturulan *temel* ağ. Bu makalede, bir sanal ağ oluşturup alt ağları ve ağ Gelişmiş kullanan bir AKS kümesi dağıtma işlemini göstermektedir.

## <a name="launch-azure-cloud-shell"></a>Azure Cloud Shell'i başlatma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır.

Cloud Shell'i açmak için seçmeniz **deneyin** bir kod bloğunun sağ üst köşesinde öğesinden. İsterseniz [https://shell.azure.com/bash](https://shell.azure.com/bash) adresine giderek Cloud Shell'i ayrı bir tarayıcı sekmesinde de başlatabilirsiniz. **Kopyala**’yı seçerek kod bloğunu kopyalayın, Cloud Shell’e yapıştırın ve Enter tuşuna basarak çalıştırın.

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz, bu makalede Azure CLI Sürüm 2.0.49 gerektirir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir gruptur. [az group create][az-group-create] komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek *westus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Kullanarak bir sanal ağ oluşturma [az ağ sanal ağ oluşturma] [ az-network-vnet-create] komutu. Aşağıdaki örnek, bir sanal ağ adı oluşturur *myVnet* bir adres ön eki ile *10.0.0.0/8*ve adlı bir alt ağ *myAKSSubnet*. Varsayılan olarak bu alt ağ adres ön eki *10.240.0.0/16*:

```azurecli-interactive
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefixes 10.0.0.0/8 \
    --subnet-name myAKSSubnet \
    --subnet-prefix 10.240.0.0/16
```

Şimdi kullanarak sanal düğümler için ek bir alt ağı oluşturmak [az ağ sanal ağ alt ağı oluşturma] [ az-network-vnet-subnet-create] komutu. Aşağıdaki örnekte adlı bir alt ağ oluşturulmaktadır *myVirtualNodeSubnet* adres ön eki ile *10.241.0.0/16*.

```azurecli-interactive
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name myVirtualNodeSubnet \
    --address-prefix 10.241.0.0/16
```

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Bir AKS kümesinin diğer Azure kaynaklarıyla etkileşime geçmesini sağlamak için bir Azure Active Directory hizmet sorumlusu kullanılır. Bu hizmet sorumlusu Azure CLI veya portal ile otomatik olarak oluşturulabilir veya kendiniz önceden bir tane oluşturup ek izinler atayabilirsiniz.

Kullanarak bir hizmet sorumlusu oluşturma [az ad sp create-for-rbac] [ az-ad-sp-create-for-rbac] komutu. `--skip-assignment` parametresi, ek izinlerin atanmasını engeller.

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

Çıktı aşağıdaki örneğe benzer:

```
{
  "appId": "bef76eb3-d743-4a97-9534-03e9388811fc",
  "displayName": "azure-cli-2018-11-21-18-42-00",
  "name": "http://azure-cli-2018-11-21-18-42-00",
  "password": "1d257915-8714-4ce7-a7fb-0e5a5411df7f",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

*appId* ve *password* değerlerini not edin. Bu değerler aşağıdaki adımlarda kullanılacaktır.

## <a name="assign-permissions-to-the-virtual-network"></a>Sanal ağ için izinler atama

Kümeniz sanal ağı yönetmek ve kullanmak izin vermek için ağ kaynaklarını kullanmak için doğru haklara AKS hizmet sorumlusu vermeniz gerekir.

İlk olarak, sanal ağın kaynak Kimliğini kullanarak alın [az ağ vnet show][az-network-vnet-show]:

```azurecli-interactive
az network vnet show --resource-group myResourceGroup --name myVnet --query id -o tsv
```

Sanal ağ kullanmak AKS kümesi için doğru erişim vermek için kullanarak bir rol ataması oluşturma [az rol ataması oluşturma] [ az-role-assignment-create] komutu. `<appId`> ve `<vnetId>` yerine önceki iki adımda topladığınız değerleri yazın.

```azurecli-interactive
az role assignment create --assignee <appId> --scope <vnetId> --role Contributor
```

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

Bir önceki adımda oluşturduğunuz AKS alt ağa bir AKS kümesi dağıtırsınız. Bu alt ağ kullanımının Kimliğini alın [az ağ sanal ağ alt ağı show][az-network-vnet-subnet-show]:

```azurecli-interactive
az network vnet subnet show --resource-group myResourceGroup --vnet-name myVnet --name myAKSSubnet --query id -o tsv
```

AKS kümesi oluşturmak için [az aks create][az-aks-create] komutunu kullanın. Aşağıdaki örnekte, bir düğüm ile *myAKSCluster* adlı bir küme oluşturulmuştur. Değiştirin `<subnetId>` önceki adımda elde edilen Kimliğine sahip ve ardından `<appId>` ve `<password>` ile 

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --network-plugin azure \
    --service-cidr 10.0.0.0/16 \
    --dns-service-ip 10.0.0.10 \
    --docker-bridge-address 172.17.0.1/16 \
    --vnet-subnet-id <subnetId> \
    --service-principal <appId> \
    --client-secret <password>
```

Birkaç dakika sonra komut tamamlanır ve küme hakkında JSON tarafından biçimlendirilmiş bilgiler gösterilir.

## <a name="enable-virtual-nodes"></a>Sanal düğümler etkinleştir

Ek işlevsellik sağlamak için bir Azure CLI uzantısı sanal düğümü Bağlayıcısı'nı kullanır. Sanal düğümler Bağlayıcısı'nı etkinleştirmeden önce ilk uzantısını kullanarak yükleme [az uzantısı ekleme] [ az-extension-add] komutu:

```azurecli-interactive
az extension add --source https://aksvnodeextension.blob.core.windows.net/aks-virtual-node/aks_virtual_node-0.2.0-py2.py3-none-any.whl
```

Sanal düğümler etkinleştirmek için artık kullanın [az aks enable-addons] [ az-aks-enable-addons] komutu. Aşağıdaki örnekte adlı alt *myVirtualNodeSubnet* bir önceki adımda oluşturduğunuz:

```azurecli-interactive
az aks enable-addons \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --addons virtual-node \
    --subnet-name myVirtualNodeSubnet
```

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

`kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks get-credentials][az-aks-get-credentials] komutunu kullanın. Bu adım kimlik bilgilerini indirir ve Kubernetes CLI’yi bunları kullanacak şekilde yapılandırır.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın.

```console
kubectl get nodes
```

Linux için oluşturulan tek VM düğümünden'i ve ardından sanal düğümü aşağıdaki örnek çıktı gösterilmektedir *sanal düğümü-aci-linux*:

```
$ kubectl get nodes

NAME                          STATUS    ROLES     AGE       VERSION
virtual-node-aci-linux        Ready     agent     28m       v1.11.2
aks-agentpool-14693408-0      Ready     agent     32m       v1.11.2
```

## <a name="deploy-a-sample-app"></a>Örnek bir uygulama dağıtma

Adlı bir dosya oluşturun `virtual-node.yaml` aşağıdaki YAML'ye kopyalayın. Düğümde, kapsayıcı zamanlamak için bir [nodeSelector] [ node-selector] ve [toleration] [ toleration] tanımlanır.

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
        image: microsoft/aci-helloworld
        ports:
        - containerPort: 80
      nodeSelector:
        kubernetes.io/role: agent
        beta.kubernetes.io/os: linux
        type: virtual-kubelet
      tolerations:
      - key: virtual-kubelet.io/provider
        operator: Exists
      - key: azure.com/aci
        effect: NoSchedule
```

Uygulamayı çalıştırın [kubectl uygulamak] [ kubectl-apply] komutu.

```console
kubectl apply -f virtual-node.yaml
```

Kullanım [kubectl pod'ları alma] [ kubectl-get] komutunu `-o wide` pod'ların ve zamanlanmış düğüm listesini çıkarmak için bağımsız değişken. Dikkat `aci-helloworld` pod zamanlandı `virtual-node-aci-linux` düğümü.

```
$ kubectl get pods -o wide

NAME                            READY     STATUS    RESTARTS   AGE       IP           NODE
aci-helloworld-9b55975f-bnmfl   1/1       Running   0          4m        10.241.0.4   virtual-node-aci-linux
```

Pod sanal düğümü ile kullanmak için temsilci Azure sanal ağ alt ağından iç IP adresi atanır.

## <a name="test-the-virtual-node-pod"></a>Sanal düğüm pod test

Sanal düğüm üzerinde çalışan bir pod test etmek için demo uygulamasını bir web istemcisi ile göz atın. Pod iç IP adresi atandığı gibi bu bağlantıyı başka bir AKS kümesi pod'u hızlıca test edebilirsiniz. Bir test pod oluşturun ve terminal oturumu ekler:

```console
kubectl run -it --rm virtual-node-test --image=debian
```

Yükleme `curl` pod kullanarak `apt-get`:

```console
apt-get update && apt-get install -y curl
```

Artık erişim pod kullanmanın adresi `curl`, gibi *http://10.241.0.4*. Önceki gösterilen kendi iç IP adresi verin `kubectl get pods` komutu:

```console
curl -L http://10.241.0.4
```

Demo uygulamayı aşağıdaki sıkıştırılmış örneğe çıktıda gösterildiği gibi görüntülenir:

```
$ curl -L 10.241.0.4

<html>
<head>
  <title>Welcome to Azure Container Instances!</title>
</head>
[...]
```

Test pod ile terminal oturumunu kapatın `exit`. Oturumunuz sona pod silinmiş olur.

## <a name="remove-virtual-nodes"></a>Sanal düğüm kaldırma

Artık sanal düğümü kullanmak istemiyorsanız, bunları devre dışı bırakabilirsiniz kullanarak [az aks devre dışı bırak-addons] [ az aks disable-addons] komutu. Aşağıdaki örnek, Linux sanal düğümü devre dışı bırakır:

```azurecli-interactive
az aks disable-addons --resource-group myResourceGroup --name myAKSCluster --addons virtual-node
```

Şimdi, kaynak grubunu ve sanal ağ kaynakları kaldırın:

```azurecli-interactive
# Change the name of your resource group and network resources as needed
RES_GROUP=myResourceGroup

# Get network profile ID
NETWORK_PROFILE_ID=$(az network profile list --resource-group $RES_GROUP --query [0].id --output tsv)

# Delete the network profile
az network profile delete --id $NETWORK_PROFILE_ID -y

# Get the service association link (SAL) ID
SAL_ID=$(az network vnet subnet show --resource-group $RES_GROUP --vnet-name myVnet --name myAKSSubnet --query id --output tsv)/providers/Microsoft.ContainerInstance/serviceAssociationLinks/default

# Delete the default SAL ID for the subnet
az resource delete --ids $SAL_ID --api-version 2018-07-01

# Delete the subnet delegation to Azure Container Instances
az network vnet subnet update --resource-group $RES_GROUP --vnet-name myVnet --name myAKSSubnet --remove delegations 0
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir pod sanal düğümde zamanlanmış ve bir özel, iç IP adresi atanır. Bunun yerine, bir yük dengeleyici veya giriş denetleyicisine üzerinden pod için bir hizmet dağıtımı ve trafiği yönlendirme oluşturabilirsiniz. Daha fazla bilgi için [AKS içinde bir temel giriş denetleyicisine oluşturma][aks-basic-ingress].

Sanal düğümler genellikle bir AKS ölçeklendirme bir çözümde bileşenidir. Ölçeklendirme çözümleri hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Kubernetes yatay pod otomatik ölçeklendiricinin kullanın][aks-hpa]
- [Kubernetes küme ölçeklendiriciyi kullanmak][aks-cluster-autoscaler]

<!-- LINKS - external -->
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/

<!-- LINKS - internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[az-group-create]: /cli/azure/group#az-group-create
[az-network-vnet-create]: /cli/azure/network/vnet#az-network-vnet-create
[az-network-vnet-subnet-create]: /cli/azure/network/vnet/subnet#az-network-vnet-subnet-create
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-network-vnet-show]: /cli/azure/network/vnet#az-network-vnet-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet#az-network-vnet-subnet-show
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-enable-addons]: /cli/azure/aks#az-aks-enable-addons
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az aks disable-addons]: /cli/azure/aks#az-aks-disable-addons
[aks-hpa]: tutorial-kubernetes-scale.md
[aks-cluster-autoscaler]: autoscaler.md
[aks-basic-ingress]: ingress-basic.md
