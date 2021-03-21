---
title: Azure Kubernetes Service 'e (AKS) geçiş
description: Azure Kubernetes hizmetine (AKS) geçiş yapın.
services: container-service
ms.topic: article
ms.date: 02/25/2020
ms.custom: mvc
ms.openlocfilehash: e0c3e331dba08fc95f471e3ad40dfcbb10cc2f0c
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104670639"
---
# <a name="migrate-to-azure-kubernetes-service-aks"></a>Azure Kubernetes Service 'e (AKS) geçiş

Bu makale, Azure Kubernetes Service (AKS) için başarılı bir geçiş planlayıp yürütmenize yardımcı olur. Bu kılavuz, önemli kararlar almanıza yardımcı olmak için, AKS için önerilen geçerli yapılandırmaya ilişkin ayrıntılar sağlar. Bu makale her senaryoyu kapsamamaktadır ve uygun yerlerde, makalenin başarılı bir geçiş planlaması için daha ayrıntılı bilgilere bağlantılar içerir.

Bu belge, aşağıdaki senaryoları desteklemeye yardımcı olmak için kullanılabilir:

* [Azure geçişi](../migrate/migrate-services-overview.md) 'ni kullanarak belirli uygulamaları kapsayıma ve onları aks 'e geçirme
* [Kullanılabilirlik kümeleri](../virtual-machines/windows/tutorial-availability-sets.md) tarafından desteklenen bir aks kümesini [sanal makine ölçek kümelerine](../virtual-machine-scale-sets/overview.md) geçirme
* AKS kümesini [Standart SKU yük dengeleyici](./load-balancer-standard.md) kullanmak üzere geçirme
* [Azure Container Service (ACS)-devre dışı bırakma 31 ocak 2020,](https://azure.microsoft.com/updates/azure-container-service-will-retire-on-january-31-2020/) aks 'e geçiriliyor
* [Aks altyapısından](/azure-stack/user/azure-stack-kubernetes-aks-engine-overview) aks 'e geçiş
* Azure olmayan tabanlı Kubernetes kümelerinden AKS 'e geçiş
* Mevcut kaynakları farklı bir bölgeye taşıma

Geçiş yaparken, hedef Kubernetes sürümünüzün AKS için desteklenen pencere kapsamında olduğundan emin olun. Daha eski bir sürüm kullanıyorsanız, bu desteklenen aralıkta olmayabilir ve yükseltme sürümlerinin AKS tarafından desteklenmesi gerekir. Daha fazla bilgi için bkz. [aks desteklenen Kubernetes sürümleri](./supported-kubernetes-versions.md) .

Kubernetes 'in daha yeni bir sürümüne geçiş yapıyorsanız, [Kubernetes sürümünü ve sürüm eğriltme destek ilkesini](https://kubernetes.io/docs/setup/release/version-skew-policy/#supported-versions)gözden geçirin.

Çeşitli açık kaynaklı araçlar, senaryonuza bağlı olarak geçişinize yardımcı olabilir:

* [Velero](https://velero.io/) (Kubernetes 1.7 + gerektirir)
* [Azure Kuto CLı uzantısı](https://github.com/yaron2/azure-kube-cli)
* [ReShifter](https://github.com/mhausenblas/reshifter)

Bu makalede, için geçiş ayrıntılarını özetliyoruz:

> [!div class="checklist"]
> * Azure geçişi ile uygulama Kapsayıcılı hale getirme 
> * Standart Load Balancer ve sanal makine ölçek kümeleri ile AKS 'ler
> * Mevcut bağlı Azure hizmetleri
> * Geçerli kotalar olduğundan emin olun
> * Yüksek kullanılabilirlik ve iş sürekliliği
> * Durum bilgisi olmayan uygulamalarla ilgili konular
> * Durum bilgisi olan uygulamalarla ilgili konular
> * Küme yapılandırmanızın dağıtımı

## <a name="use-azure-migrate-to-migrate-your-applications-to-aks"></a>Uygulamalarınızı AKS 'e geçirmek için Azure geçişi 'ni kullanma

Azure geçişi, Azure şirket içi sunucuları, altyapısı, uygulamaları ve verileri değerlendirmek ve bu uygulamalara geçiş yapmak için birleştirilmiş bir platform sunar. AKS için Azure geçişi 'ni aşağıdakiler için kullanabilirsiniz:

* [ASP.NET uygulamaları kapsayıklaın ve AKS 'e geçiş](../migrate/tutorial-containerize-aspnet-kubernetes.md)
* [Java Web uygulamalarını containerleştirme ve AKS 'e geçirme](../migrate/tutorial-containerize-java-kubernetes.md)

## <a name="aks-with-standard-load-balancer-and-virtual-machine-scale-sets"></a>Standart Load Balancer ve sanal makine ölçek kümeleri ile AKS 'ler

AKS, daha düşük yönetim yüküyle benzersiz yetenekler sunan bir yönetilen hizmettir. Yönetilen bir hizmet olmanın bir sonucu olarak, AKS 'nin desteklediği bir [bölge](./quotas-skus-regions.md) kümesinden seçim yapmanız gerekir. Mevcut kümenizdeki AKS 'e geçiş, AKS tarafından yönetilen denetim düzleminde sağlıklı kalması için mevcut uygulamalarınızın değiştirilmesini gerektirebilir.

[Birden çok düğüm havuzu](./use-multiple-node-pools.md), [KULLANıLABILIRLIK ALANLARı](../availability-zones/az-overview.md), [yetkilendirilmiş IP aralıkları](./api-server-authorized-ip-ranges.md), [küme otomatik Scaler](./cluster-autoscaler.md), [aks için Azure ilkesi](../governance/policy/concepts/policy-for-kubernetes.md)ve yayımlandıkları gibi diğer yeni özellikler gibi özellikleri almanızı sağlamak için [Sanal Makine Ölçek Kümeleri](../virtual-machine-scale-sets/index.yml) ve [Azure Standart Load Balancer](./load-balancer-standard.md) tarafından desteklenen aks kümelerini kullanmanızı öneririz.

[Sanal makine kullanılabilirlik kümeleri](../virtual-machines/availability.md#availability-sets) tarafından desteklenen aks kümelerinde bu özelliklerin birçoğu için destek yok.

Aşağıdaki örnek, bir sanal makine ölçek kümesi tarafından desteklenen tek düğümlü havuz içeren bir AKS kümesi oluşturur. Standart yük dengeleyici kullanır. Ayrıca küme için düğüm havuzunda küme otomatik Scaler 'ı ve en az *1* ve en fazla *3* düğüm ayarlar:

```azurecli-interactive
# First create a resource group
az group create --name myResourceGroup --location eastus

# Now create the AKS cluster and enable the cluster autoscaler
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --node-count 1 \
  --vm-set-type VirtualMachineScaleSets \
  --load-balancer-sku standard \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

## <a name="existing-attached-azure-services"></a>Mevcut bağlı Azure hizmetleri

Kümeleri geçirirken dış Azure hizmetlerinizi bağlı kalabilirsiniz. Bunlar kaynak yeniden oluşturma gerektirmez, ancak işlevleri sürdürmek için önceki yeni kümelere bağlantıların güncelleştirilmesini gerektirecektir.

* Azure Container Registry
* Log Analytics
* Application Insights
* Traffic Manager
* Depolama Hesabı
* Dış veritabanları

## <a name="ensure-valid-quotas"></a>Geçerli kotalar olduğundan emin olun

Geçiş sırasında aboneliğinize daha fazla sanal makine dağıtılacağından, Kotalarınızın ve limitlerinizin bu kaynaklar için yeterli olduğunu doğrulamanız gerekir. [VCPU kotasında](../azure-portal/supportability/per-vm-quota-requests.md)artış istemeniz gerekebilir.

IP 'Leri tüketmemenizi sağlamak için [ağ kotaları](../azure-portal/supportability/networking-quota-requests.md) artışı istemeniz gerekebilir. Ek bilgi için bkz. [AKS için ağ ve IP aralıkları](./configure-kubenet.md) .

Daha fazla bilgi için bkz. [Azure aboneliği ve hizmet sınırları](../azure-resource-manager/management/azure-subscription-service-limits.md). Geçerli kotalarınızı denetlemek için, Azure portal [abonelikler dikey penceresine](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)gidin, aboneliğinizi seçin ve ardından **kullanım + kotalar**' ı seçin.

## <a name="high-availability-and-business-continuity"></a>Yüksek kullanılabilirlik ve Iş sürekliliği

Uygulamanız kapalı kalma süresini işleyememesi durumunda yüksek kullanılabilirlik geçiş senaryoları için en iyi yöntemleri izlemeniz gerekir.  Karmaşık iş sürekliliği planlama, olağanüstü durum kurtarma ve en yüksek çalışma süresi, bu belgenin kapsamı dışındadır.  Daha fazla bilgi edinmek için [Azure Kubernetes Service 'te (AKS) iş sürekliliği ve olağanüstü durum kurtarma Için en iyi uygulamalar](./operator-best-practices-multi-region.md) hakkında daha fazla bilgi edinin.

Karmaşık uygulamalar için genellikle her seferinde değil zaman içinde geçiş yapabilirsiniz. Bu, eski ve yeni ortamların ağ üzerinden iletişim kurması gerekebilecek anlamına gelir. Daha önce `ClusterIP` iletişim kurmak için Hizmetleri kullanan uygulamaların tür olarak açığa çıkarılması `LoadBalancer` ve güvenli bir şekilde sağlanması gerekebilir.

Geçişi gerçekleştirmek için, istemcileri AKS üzerinde çalışan yeni hizmetlere işaret etmek isteyeceksiniz. DNS 'yi, AKS kümenizin önünde bulunan Load Balancer işaret etmek üzere güncelleştirerek trafiği yeniden yönlendirmenizi öneririz.

[Azure Traffic Manager](../traffic-manager/index.yml) , müşterileri Istenen Kubernetes kümesine ve uygulama örneğine yönlendirebilir.  Traffic Manager, bölgeler arasında ağ trafiği dağıtabilecek DNS tabanlı bir trafik yük dengeleyicidir.  En iyi performans ve artıklık için, tüm uygulama trafiğini AKS kümenize geçmeden önce Traffic Manager aracılığıyla doğrudan yönlendirin.  Birden çok Luster dağıtımında, müşteriler her bir AKS kümesindeki hizmetlere işaret eden bir Traffic Manager DNS adına bağlanmalıdır. Bu hizmetleri Traffic Manager uç noktaları kullanarak tanımlayın. Her uç nokta *hizmet yük DENGELEYICI IP*'dir. Ağ trafiğini bir bölgedeki Traffic Manager uç noktasından farklı bir bölgedeki uç noktaya yönlendirmek için bu yapılandırmayı kullanın.

![Traffic Manager ile AKS](media/operator-best-practices-bc-dr/aks-azure-traffic-manager.png)

[Azure ön kapı hizmeti](../frontdoor/front-door-overview.md) , aks kümelerine yönelik trafiği yönlendirme için başka bir seçenektir.  Azure Front Door Service, yüksek kullanılabilirliğe yönelik en iyi performans ve anında genel yük devretme için iyileştirerek, web trafiğinizin genel yönlendirmesini tanımlamanızı, yönetmenizi ve izlemenizi sağlar. 

### <a name="considerations-for-stateless-applications"></a>Durum bilgisi olmayan uygulamalarla ilgili konular

Durum bilgisiz uygulama geçişi en kolay durumdur. Kaynak tanımlarınızı (YAML veya Held) yeni kümeye uygulayın, her şeyin beklendiği gibi çalıştığından emin olun ve yeni kümenizi etkinleştirmek için trafiği yeniden yönlendirin.

### <a name="considerations-for-stateful-applications"></a>Durum bilgisi olan uygulamalarla ilgili konular

Veri kaybını veya beklenmedik kapalı kalma süresini önlemek için durum bilgisi olan uygulamaların geçişini dikkatle planlayın.

Azure dosyaları kullanıyorsanız, dosya payını yeni kümeye bir birim olarak bağlayabilirsiniz:
* [Statik Azure dosyalarını birim olarak bağlama](./azure-files-volume.md#mount-file-share-as-an-persistent-volume)

Azure yönetilen diskler kullanıyorsanız, diski yalnızca herhangi bir VM 'ye bağlı değilse bağlayabilirsiniz:
* [Statik Azure diskini birim olarak bağlama](./azure-disk-volume.md#mount-disk-as-volume)

Bu yaklaşımlardan hiçbiri işe çalışmadıysanız, yedekleme ve geri yükleme seçeneklerini kullanabilirsiniz:
* [Azure 'da Velero](https://github.com/vmware-tanzu/velero-plugin-for-microsoft-azure/blob/master/README.md)

#### <a name="azure-files"></a>Azure Dosyaları

Disklerden farklı olarak, Azure dosyaları aynı anda birden çok konağa bağlanabilir. AKS kümenizde, Azure ve Kubernetes, ACS kümenizin hala kullandığı bir pod oluşturmanızı engellemez. Veri kaybını ve beklenmedik davranışı engellemek için, kümelerin aynı anda aynı dosyalara yazmayın olduğundan emin olun.

Uygulamanız aynı dosya paylaşımının işaret eden birden çok kopyayı barındırabiliyorsanız, durum bilgisi olmayan geçiş adımlarını izleyin ve YAML tanımlarınızı yeni kümenize dağıtın. Aksi takdirde, olası bir geçiş yaklaşımı aşağıdaki adımları içerir:

* Uygulamanızın düzgün çalıştığını doğrulayın.
* Canlı trafiğinizi yeni AKS kümenize getirin.
* Eski kümenin bağlantısını kesin.

Boş bir paylaşımdan başlamak ve kaynak verilerin bir kopyasını oluşturmak istiyorsanız, bu [`az storage file copy`](/cli/azure/storage/file/copy) komutları kullanarak verilerinizi geçirebilirsiniz.


#### <a name="migrating-persistent-volumes"></a>Kalıcı birimleri geçirme

Mevcut kalıcı birimleri AKS 'e geçiriyorsanız, genellikle bu adımları takip edersiniz:

* Uygulamaya sessiz yazma yazma. (Bu adım isteğe bağlıdır ve kapalı kalma süresi gerektirir.)
* Disklerin anlık görüntülerini alın.
* Anlık görüntülerden yeni yönetilen diskler oluşturun.
* AKS 'de kalıcı birimler oluşturun.
* Pod belirtimlerini, Persistentvolumeclaim (statik sağlama) yerine [mevcut birimleri kullanacak](./azure-disk-volume.md) şekilde güncelleştirin.
* Uygulamanızı AKS 'e dağıtın.
* Uygulamanızın düzgün çalıştığını doğrulayın.
* Canlı trafiğinizi yeni AKS kümenize getirin.

> [!IMPORTANT]
> Yazmaları sessiz değil ' i seçerseniz, verileri yeni dağıtıma çoğaltmanız gerekir. Aksi takdirde, disk anlık görüntülerini aldıktan sonra yazılmış verileri kaçırılırsınız.

Bazı açık kaynaklı araçlar, yönetilen diskler oluşturmanıza ve birimleri Kubernetes kümeleri arasında geçirmeye yardımcı olabilir:

* [Azure CLI disk kopyalama uzantısı](https://github.com/noelbundick/azure-cli-disk-copy-extension) , diskleri kaynak grupları ve Azure bölgeleri arasında kopyalar ve dönüştürür.
* [Azure KUTO CLI uzantısı](https://github.com/yaron2/azure-kube-cli) , ACS Kubernetes birimlerini numaralandırır ve bunları bir aks kümesine geçirir.


### <a name="deployment-of-your-cluster-configuration"></a>Küme yapılandırmanızın dağıtımı

AKS 'e bilinen iyi bir yapılandırma dağıtmak için mevcut sürekli tümleştirme (CI) ve sürekli teslim (CD) işlem hattınızı kullanmanızı öneririz. [Uygulamalarınızı derlemek ve AKS 'e dağıtmak](/azure/devops/pipelines/ecosystems/kubernetes/aks-template)için Azure Pipelines kullanabilirsiniz. Mevcut dağıtım görevlerinizi kopyalayın ve `kubeconfig` Yeni AKS kümesine işaret edin.

Bu mümkün değilse, mevcut Kubernetes kümenizdeki kaynak tanımlarını dışarı aktarın ve ardından bunları AKS 'e uygulayın. `kubectl`Nesneleri dışarı aktarmak için ' i kullanabilirsiniz.

```console
kubectl get deployment -o=yaml --export > deployments.yaml
```

### <a name="moving-existing-resources-to-another-region"></a>Mevcut kaynakları başka bir bölgeye taşıma

AKS kümenizi [aks tarafından desteklenen farklı bir bölgeye][region-availability]taşımak isteyebilirsiniz. Diğer bölgede yeni bir küme oluşturmanızı ve ardından kaynaklarınızı ve uygulamalarınızı yeni kümenize dağıtmanızı öneririz. Ayrıca, AKS kümenizde çalışan [Azure dev Spaces][azure-dev-spaces] gibi bir hizmet varsa, bu hizmetleri yeni bölgedeki kümenize de yüklemeniz ve yapılandırmanız gerekecektir.


Bu makalede, için geçiş ayrıntıları özetlenmektedir:

> [!div class="checklist"]
> * Standart Load Balancer ve sanal makine ölçek kümeleri ile AKS 'ler
> * Mevcut bağlı Azure hizmetleri
> * Geçerli kotalar olduğundan emin olun
> * Yüksek kullanılabilirlik ve iş sürekliliği
> * Durum bilgisi olmayan uygulamalarla ilgili konular
> * Durum bilgisi olan uygulamalarla ilgili konular
> * Küme yapılandırmanızın dağıtımı


[region-availability]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
[azure-dev-spaces]: ../dev-spaces/index.yml
