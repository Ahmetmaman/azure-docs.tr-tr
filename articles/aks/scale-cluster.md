---
title: Azure Kubernetes Service (AKS) kümesini ölçeklendirme
description: Bir Azure Kubernetes Service (AKS) kümedeki düğüm sayısını ölçeklendirmeyi öğrenin.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 01/10/2019
ms.author: iainfoulds
ms.openlocfilehash: 558a3b6dc15293ab9a0895aa4f9f709ba2d0a51f
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2019
ms.locfileid: "54214632"
---
# <a name="scale-the-node-count-in-an-azure-kubernetes-service-aks-cluster"></a>Bir Azure Kubernetes Service (AKS) kümedeki düğüm sayısını ölçekleyin

Kaynak, gerekiyorsa uygulamalarınızı değiştirmek için farklı sayıda düğüm çalıştırmak için bir AKS kümesi elle ölçeklendirebilirsiniz. Ölçeği Azalt düğümleri dikkatli bir şekilde konusunda [kordonlanır ve boşaltılır] [ kubernetes-drain] çalışan uygulamaların kesintiye en aza indirmek için. Yukarı ölçeklediğinizde `az` komut, düğümlerin işaretlenene kadar bekler `Ready` Kubernetes kümesi tarafından.

## <a name="scale-the-cluster-nodes"></a>Küme düğümlerini ölçeklendirme

İlk olarak, alın *adı* nodepool kullanmanın [az aks show] [ az-aks-show] komutu. Aşağıdaki örnekte adlı Küme için nodepool ad alır *myAKSCluster* içinde *myResourceGroup* kaynak grubu:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --query agentPoolProfiles
```

Aşağıdaki örnek çıktı gösterilmektedir *adı* olduğu *nodepool1*:

```console
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query agentPoolProfiles

[
  {
    "count": 1,
    "maxPods": 110,
    "name": "nodepool1",
    "osDiskSizeGb": 30,
    "osType": "Linux",
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_DS2_v2"
  }
]
```

Küme düğümlerini ölçeklendirmek için `az aks scale` komutunu kullanın. Aşağıdaki örnekte adlı bir küme ölçeklendirir *myAKSCluster* tek bir düğüm. Kendi sağlamak *--nodepool adı* önceki komuttan gibi *nodepool1*:

```azurecli-interactive
az aks scale --resource-group myResourceGroup --name myAKSCluster --node-count 1 --nodepool-name <your node pool name>
```

Aşağıdaki örnek çıktıda kümenin bir düğümü için başarıyla Ölçeklendirildi gösterildiği gösterir *agentPoolProfiles* bölümü:

```json
{
  "aadProfile": null,
  "addonProfiles": null,
  "agentPoolProfiles": [
    {
      "count": 1,
      "maxPods": 110,
      "name": "nodepool1",
      "osDiskSizeGb": 30,
      "osType": "Linux",
      "storageProfile": "ManagedDisks",
      "vmSize": "Standard_DS2_v2",
      "vnetSubnetId": null
    }
  ],
  "dnsPrefix": "myAKSClust-myResourceGroup-19da35",
  "enableRbac": true,
  "fqdn": "myaksclust-myresourcegroup-19da35-0d60b16a.hcp.eastus.azmk8s.io",
  "id": "/subscriptions/<guid>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "kubernetesVersion": "1.9.11",
  "linuxProfile": {
    "adminUsername": "azureuser",
    "ssh": {
      "publicKeys": [
        {
          "keyData": "[...]"
        }
      ]
    }
  },
  "location": "eastus",
  "name": "myAKSCluster",
  "networkProfile": {
    "dnsServiceIp": "10.0.0.10",
    "dockerBridgeCidr": "172.17.0.1/16",
    "networkPlugin": "kubenet",
    "networkPolicy": null,
    "podCidr": "10.244.0.0/16",
    "serviceCidr": "10.0.0.0/16"
  },
  "nodeResourceGroup": "MC_myResourceGroup_myAKSCluster_eastus",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "servicePrincipalProfile": {
    "clientId": "[...]",
    "secret": null
  },
  "tags": null,
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

## <a name="next-steps"></a>Sonraki adımlar

AKS öğreticileri ile AKS dağıtma ve yönetme hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [AKS Öğreticisi][aks-tutorial]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-show]: /cli/azure/aks#az-aks-show
