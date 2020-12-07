---
title: Azure Kubernetes Service (AKS) kümesini yükseltme
description: En son özellikleri ve güvenlik güncelleştirmelerini almak için bir Azure Kubernetes hizmeti (AKS) kümesini nasıl yükselteceğinizi öğrenin.
services: container-service
ms.topic: article
ms.date: 11/17/2020
ms.openlocfilehash: c5de1a02a077ccb5f46b685572c6c43f5951b224
ms.sourcegitcommit: ea551dad8d870ddcc0fee4423026f51bf4532e19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2020
ms.locfileid: "96751504"
---
# <a name="upgrade-an-azure-kubernetes-service-aks-cluster"></a>Azure Kubernetes Service (AKS) kümesini yükseltme

AKS kümesinin yaşam döngüsünün bir parçası olarak genellikle en son Kubernetes sürümüne yükseltmeniz gerekir. En son Kubernetes güvenlik sürümlerini uygulamanız veya en son özellikleri almak için yükseltmeniz önemlidir. Bu makalede, bir AKS kümesindeki ana bileşenlerin veya tek bir varsayılan düğüm havuzunun nasıl yükseltileceğini gösterir.

Birden çok düğüm havuzu veya Windows Server düğümü kullanan AKS kümelerinde bkz. [aks 'de düğüm havuzunu yükseltme][nodepool-upgrade].

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, Azure CLı sürüm 2.0.65 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

> [!WARNING]
> AKS kümesi yükseltmesi, düğümlerinizin bir eş ve drenajı tetikler. Kullanılabilir düşük bir işlem kotası varsa, yükseltme başarısız olabilir. Daha fazla bilgi için bkz. [kotaları artırma](../azure-portal/supportability/resource-manager-core-quotas-request.md) .

## <a name="check-for-available-aks-cluster-upgrades"></a>Kullanılabilir AKS kümesi yükseltmelerini denetle

Kümeniz için hangi Kubernetes sürümlerinin kullanılabilir olduğunu denetlemek için [az aks Get-yükseltmeler][az-aks-get-upgrades] komutunu kullanın. Aşağıdaki örnek, *Myresourcegroup* adlı kaynak grubunda *Myakscluster* adlı kümede kullanılabilir yükseltmeleri denetler:

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

> [!NOTE]
> Desteklenen bir AKS kümesini yükselttiğinizde, Kubernetes ikincil sürümleri atlanamaz. Örneğin, *1.12. x*  ->  *1.13. x* veya *1.13. x*  ->  *1.14. x* arasındaki yükseltmelere izin verilir, ancak *1.12. x*  ->  *1.14. x* değildir.
>
> Yükseltmek için, *1.12. x*  ->  *1.14. x* sürümünden önce *1.12. x*  ->  *1.13. x* sürümünden yükseltme yapın ve ardından *1.13. x*  ->  *1.14. x*'den yükseltme yapın.
>
> Birden çok sürüm atlanması yalnızca desteklenmeyen bir sürümden desteklenen bir sürüme yükseltilirken yapılabilir. Örneğin, desteklenmeyen bir *1,10. x* sürümünden yükseltme > desteklenen bir *1.15. x* tamamlanabilir.

Aşağıdaki örnek çıktı, kümenin *1.13.9* ve *1.13.10* sürümlerine yükseltileceğini gösterir:

```console
Name     ResourceGroup     MasterVersion    NodePoolVersion    Upgrades
-------  ----------------  ---------------  -----------------  ---------------
default  myResourceGroup   1.12.8           1.12.8             1.13.9, 1.13.10
```
Kullanılabilir bir yükseltme yoksa şunları alacaksınız:
```console
ERROR: Table output unavailable. Use the --query option to specify an appropriate query. Use --debug for more info.
```

## <a name="customize-node-surge-upgrade"></a>Düğüm aşırı gerilim yükseltmesini özelleştirme

> [!Important]
> Düğüm dalgalanmalarına, her yükseltme işlemi için istenen en fazla aşırı gerilim sayısı için abonelik kotası gerekir. Örneğin, 5 düğüm havuzu olan, her birinde 4 düğüm sayısına sahip bir kümede toplam 20 düğüm vardır. Her düğüm havuzunda en fazla %50 bir aşırı gerilim değeri varsa, yükseltmeyi tamamlaması için 10 düğümün ek işlem ve IP kotası (2 düğüm * 5 havuz) gereklidir.
>
> Azure CNı kullanıyorsanız, alt ağda kullanılabilir IP 'Lerin yanı sıra [Azure CNI 'nin IP gereksinimlerini karşılayacak](configure-azure-cni.md)şekilde doğrulayın.

Varsayılan olarak, AKS, bir ek düğüm ile aşırı gerilim yükseltmeleri yapılandırır. En fazla aşırı gerilim ayarı için bir varsayılan değeri, bir önceki sürümlü düğümü değiştirmek için mevcut uygulamaların eşinden önce ek bir düğüm oluşturarak iş yükü kesintisini en aza indirmenize olanak sağlar. Yükseltme hızı ve yükseltme kesintisi arasında bir denge sağlamak için, en fazla dalgalanma değeri düğüm havuzu başına özelleştirilebilir. En fazla dalgalanma değeri arttırılarak, yükseltme işlemi daha hızlı tamamlanır, ancak en fazla aşırı gerilim için büyük bir değer ayarlamak yükseltme işlemi sırasında kesintiler oluşmasına neden olabilir. 

Örneğin, en fazla %100 bir dalgalanma değeri, olası en hızlı yükseltme işlemini (düğüm sayısı katlama) sağlar, ancak düğüm havuzundaki tüm düğümlerin aynı anda bırakılmasına neden olur. Ortamları test etmek için bu gibi daha yüksek bir değer kullanmak isteyebilirsiniz. Üretim düğüm havuzları için %33 max_surge bir ayar önerilir.

AKS, en fazla dalgalanma için hem tamsayı değerlerini hem de yüzde değerini kabul eder. "5" gibi bir tamsayı, aşırı yapılacak beş ek düğümü gösterir. "%50" değeri, havuzdaki geçerli düğüm sayısının yarısını bir dalgalanma değerini gösterir. En fazla dalgalanma yüzdesi değeri en az %1, en fazla %100 olabilir. Yüzde değeri en yakın düğüm sayısına yuvarlanır. En fazla dalgalanma değeri, yükseltme sırasında geçerli düğüm sayısından düşükse, en fazla dalgalanma değeri için geçerli düğüm sayısı kullanılır.

Yükseltme sırasında, en fazla dalgalanma değeri en az 1, düğüm havuzunuzdaki düğüm sayısına eşit bir en büyük değer olabilir. Daha büyük değerler ayarlayabilirsiniz, ancak en fazla dalgalanma için kullanılan en fazla düğüm sayısı, yükseltme sırasında havuzdaki düğüm sayısından daha yüksek olmayacaktır.

CLı sürümü 2.16.0 'e kadar, en fazla aşırı gerilim kullanmak için *aks-Preview* CLI uzantısına ihtiyacınız olacaktır. [Az Extension Add][az-extension-add] komutunu kullanın ve [az Extension Update][az-extension-update] komutunu kullanarak kullanılabilir güncelleştirmeler olup olmadığını denetleyin:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

> [!Important]
> Düğüm havuzundaki en fazla aşırı gerilim ayarı kalıcıdır.  Sonraki Kubernetes yükseltmeleri veya düğüm sürümü yükseltmeleri, bu ayarı kullanır. Düğüm havuzlarınızın en fazla dalgalanma değerini istediğiniz zaman değiştirebilirsiniz. Üretim düğüm havuzları için %33 olan en fazla% ayarını öneririz.

Yeni veya var olan düğüm havuzları için en fazla dalgalanma değerlerini ayarlamak için aşağıdaki komutları kullanın.

```azurecli-interactive
# Set max surge for a new node pool
az aks nodepool add -n mynodepool -g MyResourceGroup --cluster-name MyManagedCluster --max-surge 33%
```

```azurecli-interactive
# Update max surge for an existing node pool 
az aks nodepool update -n mynodepool -g MyResourceGroup --cluster-name MyManagedCluster --max-surge 5
```

## <a name="upgrade-an-aks-cluster"></a>AKS kümesini yükseltme

AKS kümeniz için kullanılabilir sürümlerin bir listesi ile yükseltmek için [az aks Upgrade][az-aks-upgrade] komutunu kullanın. Yükseltme işlemi sırasında AKS, belirtilen Kubernetes sürümünü çalıştıran kümeye yeni bir arabellek düğümü (veya [en fazla aşırı gerilim](#customize-node-surge-upgrade)olarak yapılandırılan sayıda düğüm) ekler. Böylece, çalışan uygulamaların kesintiye uğramasını en aza indirmek için eski [düğümlerden birini eşit][kubernetes-drain] hale gelir (en fazla gerilim kullanıyorsanız, belirtilen arabellek düğümlerinin sayısıyla aynı anda çok sayıda düğüm [olarak çalışır)][kubernetes-drain] . Eski düğüm tamamen boşaltılırsa, yeni sürümü alacak şekilde yeniden görüntülenir ve aşağıdaki düğümün yükseltilmesi için arabellek düğümü olur. Bu işlem, kümedeki tüm düğümler yükseltilene kadar yinelenir. İşlemin sonunda, son arabellek düğümü silinecek, mevcut Aracı düğüm sayısı ve bölge bakiyesi korunacaktır.

```azurecli-interactive
az aks upgrade \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --kubernetes-version KUBERNETES_VERSION
```

Kaç tane düğüme sahip olduğunuza bağlı olarak, kümeyi yükseltmek birkaç dakika sürer.

> [!IMPORTANT]
> Herhangi `PodDisruptionBudgets` bir (pdb 'leri), en az 1 Pod çoğaltmasının tek seferde taşınmasına izin verildiğinden emin olun, aksi takdirde boşalt/çıkar işlemi başarısız olur.
> Boşaltma işlemi başarısız olursa, uygulamaların çökmemesini sağlamak için yükseltme işlemi tasarım tarafından başarısız olur. Lütfen işlemin durmasının ne olduğunu (yanlış pdb 'leri, kotanın olmaması vb.) düzeltin ve işlemi yeniden deneyin.

Yükseltmenin başarılı olduğunu doğrulamak için [az aks Show][az-aks-show] komutunu kullanın:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

Aşağıdaki örnek çıktıda, kümenin artık *1.13.10* çalıştığını gösterilmektedir:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ---------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.13.10               Succeeded            myaksclust-myresourcegroup-19da35-90efab95.hcp.eastus.azmk8s.io
```

## <a name="set-auto-upgrade-channel-preview"></a>Otomatik yükseltme kanalını ayarla (Önizleme)

Bir kümeyi el ile yükseltmenin yanı sıra, kümenizde bir otomatik yükseltme kanalı da ayarlayabilirsiniz. Aşağıdaki yükseltme kanalları kullanılabilir:

* *hiçbiri*, Otomatik yükseltmeleri devre dışı bırakan ve kümeyi geçerli Kubernetes sürümünde tutan yok. Bu varsayılandır ve hiçbir seçenek belirtilmemişse kullanılır.
* ikincil sürümü aynı tutarken, kümeyi desteklenen en son düzeltme eki sürümüne otomatik olarak yükseltecek olan *Düzeltme Eki*. Örneğin, bir küme sürüm *1.17.7* çalıştırıyorsa ve *1.17.9*, *1.18.4*, *1.18.6* ve *1.19.1* sürümleri kullanılabiliyorsa, kümeniz *1.17.9* olarak yükseltilir.
* *kararlı*, kümeyi alt sürüm *N-1*' deki desteklenen en son düzeltme eki sürümüne otomatik olarak yükseltecek, burada *n* desteklenen en son alt sürümdür. Örneğin, bir küme sürüm *1.17.7* çalıştırıyorsa ve *1.17.9*, *1.18.4*, *1.18.6* ve *1.19.1* sürümleri kullanılabiliyorsa, kümeniz *1.18.6* olarak yükseltilir.
* *hızlı*, kümeyi en son desteklenen alt sürümde bulunan en son desteklenen yama sürümüne otomatik olarak yükseltecek. Kümenin en son desteklenen alt sürüm olduğu *n-2* alt *sürümünde yer alan* Kubernetes 'nin bir sürümünde olduğu durumlarda, küme ilk olarak *n-1* alt sürümünde desteklenen en son düzeltme eki sürümüne yükseltir. Örneğin, bir küme sürüm *1.17.7* çalıştırıyorsa ve *1.17.9*, *1.18.4*, *1.18.6* ve *1.19.1* sürümleri kullanılabiliyorsa, kümeniz önce *1.18.6* olarak yükseltilir, sonra *1.19.1* olarak yükseltilir.

> [!NOTE]
> Küme otomatik yükseltme yalnızca Kubernetes 'in GA sürümlerine güncelleştirmeler sağlar ve önizleme sürümleri için güncelleştirmeyecektir.

Bir kümeyi otomatik olarak yükseltme, bir kümeyi el ile yükseltirken aynı işlemi izler. Daha ayrıntılı bilgi için bkz. [AKS kümesini yükseltme][upgrade-cluster].

AKS kümeleri için küme otomatik yükseltmesi bir önizleme özelliğidir.

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

`AutoUpgradePreview`Aşağıdaki örnekte gösterildiği gibi, [az Feature Register][az-feature-register] komutunu kullanarak özellik bayrağını kaydedin:

```azurecli-interactive
az feature register --namespace Microsoft.ContainerService -n AutoUpgradePreview
```

Durumun *kayıtlı* gösterilmesi birkaç dakika sürer. [Az Feature List][az-feature-list] komutunu kullanarak kayıt durumunu doğrulayın:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AutoUpgradePreview')].{Name:name,State:properties.state}"
```

Hazırlandığınızda, [az Provider Register][az-provider-register] komutunu kullanarak *Microsoft. Containerservice* kaynak sağlayıcısı kaydını yenileyin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

*Aks-Preview* uzantısını yüklemek için [az Extension Add][az-extension-add] komutunu kullanın ve [az Extension Update][az-extension-update] komutunu kullanarak kullanılabilir güncelleştirmeleri denetleyin:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

Bir küme oluştururken otomatik yükseltme kanalını ayarlamak için, aşağıdaki örneğe benzer şekilde *otomatik yükseltme kanalı* parametresini kullanın.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myAKSCluster --auto-upgrade-channel stable --generate-ssh-keys
```

Mevcut kümede otomatik yükseltme kanalını ayarlamak için, aşağıdaki örneğe benzer şekilde *otomatik yükseltme kanalı* parametresini güncelleştirin.

```azurecli-interactive
az aks update --resource-group myResourceGroup --name myAKSCluster --auto-upgrade-channel stable
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, mevcut bir AKS kümesini nasıl yükselteceğiniz açıklanır. AKS kümelerini dağıtma ve yönetme hakkında daha fazla bilgi için bkz. öğreticiler kümesi.

> [!div class="nextstepaction"]
> [AKS öğreticileri][aks-tutorial-prepare-app]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az-aks-upgrade]: /cli/azure/aks#az-aks-upgrade
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[az-feature-list]: /cli/azure/feature?view=azure-cli-latest#az-feature-list&preserve-view=true
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-provider-register]: /cli/azure/provider?view=azure-cli-latest#az-provider-register&preserve-view=true
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[upgrade-cluster]:  #upgrade-an-aks-cluster