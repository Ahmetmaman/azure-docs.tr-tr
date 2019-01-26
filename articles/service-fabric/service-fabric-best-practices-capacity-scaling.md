---
title: Azure Service Fabric kapasite planlaması ve en iyi ölçeklendirme | Microsoft Docs
description: Planlama ve Service Fabric kümeleri ve uygulamaları ölçeklendirme için en iyi yöntemler.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: jeanpaul.connock
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: ff58cc713a6aba211f9eeb1dc42912d21c5b0bdb
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2019
ms.locfileid: "54914122"
---
# <a name="capacity-planning-and-scaling"></a>Kapasite planlama ve ölçekleme

Herhangi bir Azure Service Fabric kümesi oluşturma veya kümenizi barındırma işlem kaynaklarını ölçeklendirme önce kapasiteyi planlamak üzere önemlidir. Kapasite planlaması hakkında daha fazla bilgi için bkz. [Service Fabric küme kapasitesini planlama](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity). NodeType ve küme özelliklerini düşünmeye ek olarak, eklemekte olduğunuz sanal makine sayısına bakılmaksızın bir üretim ortamı için tamamlamak için bir saatten daha uzun sürmesine operations ölçeklendirme için planlayın.

## <a name="vertical-scaling-considerations"></a>Dikey ölçeklendirme hakkında önemli noktalar

[Dikey ölçeklendirme](https://docs.microsoft.com/azure/service-fabric/virtual-machine-scale-set-scale-node-type-scale-out#upgrade-the-size-and-operating-system-of-the-primary-node-type-vms) Azure Service fabric'te bir düğüm türü bir dizi adımları ve konuları gerektirir. Örneğin:
* Küme ölçeklendirme önce iyi durumda olması gerekir. Aksi durumda yalnızca daha fazla küme kararlılığını.
* **Gümüş veya daha yüksek düzeyde dayanıklılık** tüm Service Fabric kümesi durum bilgisi olan hizmetler barındıran NodeType için gereklidir.

> [!NOTE]
> Durum bilgisi olan Service Fabric sistem hizmetlerinin barındıran, birincil NodeType Gümüş dayanıklılık düzeyi veya daha büyük olmalıdır. Gümüş dayanıklılık, yükseltmeleri gibi küme işlemleri etkinleştirdikten sonra düğümleri ve benzeri ekleyerek veya kaldırarak sistemin veri güvenliği için işlemlerinin hız iyileştirdiği için daha yavaş olur.

Bir sanal makine ölçek kümesi ölçeklendirme dikey zararlı olabilir. Bunun yerine, yatay olarak yeni bir ölçek kümesi istenen SKU ile ekleyerek kümenizi ölçeklendirme ve hizmetlerinizi güvenli dikey ölçeklendirme işlemi tamamlamak için istenen SKU'nuz geçirin. Tüm yerel olarak kalıcı durum kaldıran, ana bilgisayar görüntüsünü yeniden oluşturur çünkü bir sanal makine ölçek kümesi kaynak SKU zararlı bir işlemdir.

Service Fabric [düğümü özellikleri ve yerleştirme kısıtlamaları](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-resource-manager-cluster-description#node-properties-and-placement-constraints) kümeniz tarafından uygulamaları hizmetlerinizin nerede karar vermek için kullanılır. Ne zaman birincil düğüm türünüz dikey olarak ölçeklendirme izin ver bildirmek için aynı özellik değerlerini `"nodeTypeRef"`, sanal makine ölçek kümesi Service Fabric uzantısı içinde bulunamadı. Resource Manager şablonu, aşağıdaki kod parçacığı için ölçeklendirme ve geçici bir durum bilgisi olan kümeniz için yalnızca desteklenen yeni sağlanan ölçek kümeleriniz için aynı değere sahip tanımlayacağınız özelliklerini gösterir:

```json
"settings": {
   "nodeTypeRef": ["[parameters('primaryNodetypeName')]"]
}
```

> [!NOTE]
> Kümeniz ile aynı birden fazla ölçek kümesinde çalışan bırakmayın `nodeTypeRef` başarılı dikey ölçeklendirme işlemi tamamlamak için özellik değeri daha uzun gerekir.
> Her zaman test ortamları işlemlerinde, üretim ortamı değişiklikleri denemeden önce doğrulayın. Varsayılan olarak, Service Fabric küme sistem hizmetlerinin yalnızca birincil nodetype hedeflemek için bir yerleştirme kısıtlamasına sahip.

Düğüm özellikleri ve bildirilen yerleştirme kısıtlamaları aşağıdaki adımlar bir sanal makine örneğinin aynı anda yapın. Bu, Sistem Hizmetleri (ve, durum bilgisi olan hizmetler) başka bir yerde yeni çoğaltmaları oluşturuldukça kaldırmakta olduğunuz VM örneğine göre düzgün biçimde kapatılması izin verir.
1. PowerShell üzerinden çalıştırın `Disable-ServiceFabricNode` düğümü devre dışı bırakmak için ilerlememesi ileride kaldırmak amacıyla 'RemoveNode'. En yüksek olan düğüm türünü kaldırın. Örneğin, bir altı düğüm kümeniz varsa, "MyNodeType_5" sanal makine örneği kaldırın.
2. Çalıştırma `Get-ServiceFabricNode` düğümü devre dışı olarak çözümlemeye geçmiş emin olmak için. Aksi durumda, düğümü devre dışı kadar bekleyin. Bu, her düğüm için birkaç saat sürebilir. Düğümü devre dışı olarak çözümlemeye geçmiş kadar devam yok.
3. Tek düğüm türü VM'lerin sayısını azaltın. En yüksek VM örneği şimdi kaldırılacak.
4. 1 ile gerektiği gibi 3. adımları yineleyin, ancak hiçbir zaman ölçeğini birincil düğüm türlerinde ne gerektirdiğini güvenilirlik katmanını daha az örnek sayısı. Bkz: [Service Fabric küme kapasitesini planlama](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) önerilen örnekleri listesi.

## <a name="horizontal-scaling"></a>Yatay ölçekleme

Service Fabric'te ölçeklendirme yatay ya da yapılabilir [el ile](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-scale-up-down) veya [program aracılığıyla](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-programmatic-scaling).

> [!NOTE]
> Silver veya gold dayanıklılık olan bir düğüm türü'nu ölçeklendirme, ölçeklendirme yavaş olacaktır.

### <a name="scaling-out"></a>Ölçeği genişletme

Bir Service Fabric kümesi belirli bir sanal makine ölçek kümesi için örnek sayısını artırarak ölçeklendirin. Program aracılığıyla kapasitesini artırmak istediğiniz ölçek için AzureClient ve Kimliği'ni kullanarak ölçeği genişletebilirsiniz.

```c#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
```

El ile ölçeğini genişletmek için kapasite SKU özelliği istenen güncelleştirme [sanal makine ölçek kümesi](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile) kaynak.
```json
"sku": {
    "name": "[parameters('vmNodeType0Size')]",
    "capacity": "[parameters('nt0InstanceCount')]",
    "tier": "Standard"
}
```

### <a name="scaling-in"></a>Ölçek artırma

Ölçek artırma, Ölçek genişletme değerinden daha fazla dikkat gerektirir. Örneğin:
* Birincil düğüm türü kümenizde Service Fabric sistem hizmetlerinin çalıştırın. Hiçbir zaman kapatmanız veya örneklerinin düğüm türü için ne gerektirdiğini güvenilirlik katmanını daha az sayıda örneğe sahip böylece ölçeği. 
* Bir durum bilgisi olan hizmet için belirli bir sayıda her zaman kullanılabilirliği sürdürmek ve hizmetinizi durumunu korumak için üst düğüm gerekir. En az bir düğüm sayısını bölüm/hizmet hedef çoğaltma kümesi sayısı eşittir da gerekir.

El ile ölçeklendirme için bu adımları izleyin:

1. PowerShell üzerinden çalıştırın `Disable-ServiceFabricNode` düğümü devre dışı bırakmak için ilerlememesi ileride kaldırmak amacıyla 'RemoveNode'. En yüksek olan düğüm türünü kaldırın. Örneğin, bir altı düğüm kümeniz varsa, "MyNodeType_5" sanal makine örneği kaldırın.
2. Çalıştırma `Get-ServiceFabricNode` düğümü devre dışı olarak çözümlemeye geçmiş emin olmak için. Aksi durumda, düğümü devre dışı kadar bekleyin. Bu, her düğüm için birkaç saat sürebilir. Düğümü devre dışı olarak çözümlemeye geçmiş kadar devam yok.
3. Tek düğüm türü VM'lerin sayısını azaltın. En yüksek VM örneği şimdi kaldırılacak.
4. 1 ile gerektiği gibi 3. adımları yineleyin, ancak hiçbir zaman ölçeğini birincil düğüm türlerinde ne gerektirdiğini güvenilirlik katmanını daha az örnek sayısı. Bkz: [Service Fabric küme kapasitesini planlama](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) önerilen örnekleri listesi.

Kapasite SKU özelliği istenen el ile ölçeklendirme güncelleştirmesi [sanal makine ölçek kümesi](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile) kaynak.

```json
"sku": {
    "name": "[parameters('vmNodeType0Size')]",
    "capacity": "[parameters('nt0InstanceCount')]",
    "tier": "Standard"
}
```

1. İstediğiniz kapasite sağlayana kadar 1 ile 3 arasındaki adımları yineleyin. Güvenilirlik katmanı ne gerektirdiğini değerinden birincil düğüm türü örneği sayısını ölçeği yok. Güvenilirlik katmanı ve bunlar gerekli örnek sayısını hakkında daha fazla ayrıntı için başvurmak [Service Fabric küme kapasitesini planlama](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity).

Program aracılığıyla ölçek artırma için kapatma için düğüm hazırlamanız gerekir. Bu, en yüksek örnek düğümü düğümü kaldırıldı bulma ve devre dışı bırakmadan içerir. Örneğin:

```c#
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n =>
        {
            var instanceIdIndex = n.NodeName.LastIndexOf("_");
            var instanceIdString = n.NodeName.Substring(instanceIdIndex + 1);
            return int.Parse(instanceIdString);
        })
        .FirstOrDefault();
```

Kaldırmak için düğümünü tanımladıktan sonra devre dışı bırakın ve aynı kullanarak kaldırma `FabricClient` örneği (`client` bu durumda) ve düğüm örnek adı (`instanceIdString` bu durumda) Yukarıdaki kod içinde kullanılan:

```c#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove the node from the Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up to a timeout) for the node to gracefully shutdown
var timeout = TimeSpan.FromMinutes(5);
var waitStart = DateTime.Now;
while ((mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Up || mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Disabling) &&
        DateTime.Now - waitStart < timeout)
{
    mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync()).FirstOrDefault(n => n.NodeName == mostRecentLiveNode.NodeName);
    await Task.Delay(10 * 1000);
}

// Decrement VMSS capacity
var newCapacity = (int)Math.Max(MinimumNodeCount, scaleSet.Capacity - 1); // Check min count 

scaleSet.Update().WithCapacity(newCapacity).Apply();
```

## <a name="reliability-levels"></a>Güvenilirlik düzeyi

[Güvenilirlik düzeyi](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) bir Service Fabric kümesi kaynağınızı özelliğidir ve farklı için tek tek NodeType yapılandırılamaz. Sistem Hizmetleri kümesi için çoğaltma faktörü denetler ve küme kaynak düzeyinde bir ayardır. Güvenilirlik düzeyi birincil düğüm türünüz gereken düğüm sayısı alt sınırı belirler. Güvenilirlik katmanı, şu değerleri alabilir:
* Platinum - yedi sayısı hedef çoğaltma kümesi ile sistem hizmetleri çalıştırır
* Altın - yedi sayısı hedef çoğaltma kümesi ile sistem hizmetleri çalıştırır
* Gümüş - sistem hizmetlerini beş sayısı hedef çoğaltma kümesi ile çalışır.
* Bronz - sistem hizmetlerini hedef çoğaltma kümesi sayısını üç ile çalışır.

Önerilen en düşük güvenilirlik, Gümüş düzeyidir.

Özellikler kısmında güvenilirlik düzeyi [Microsoft.ServiceFabric/clusters kaynak](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/2018-02-01/clusters), şöyle:

```json
"properties":{
    "reliabilityLevel": "Silver"
}
```

## <a name="durability-levels"></a>Dayanıklılık düzeyleri

> [!WARNING]
> Çalışan Bronz dayanıklılığa sahip düğüm türleri elde _ayrıcalıkların olmadığı_. Bu durum bilgisiz iş yüklerinizi etkileyen altyapı işler değil durduruldu veya kaldırılacak geciktirilmiş, hangi iş yüklerinizi etkileyebilecek anlamına gelir. Bronz dayanıklılık durum bilgisiz iş yükleri çalıştıran düğüm türleri için kullanın. Üretim iş yükleri için Silver çalıştırın ya da yukarıdaki durumu tutarlılığını sağlamak için. Kılavuzunda göre doğru güvenilirlik seçin [kapasite planlama belgelerini](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity).

Dayanıklılık düzeyi, iki kaynakları ayarlamanız gerekir. Uzantı profili [sanal makine ölçek kümesi kaynak](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile):

```json
"extensionProfile": {
    "extensions":          {
        "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
        "properties": {
            "settings": {
                "durabilityLevel": "Bronze"
            }
        }
    }
}
```

Altında `nodeTypes` içinde [Microsoft.ServiceFabric/clusters kaynak](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/2018-02-01/clusters) 

```json
"nodeTypes": [
    {
        "name": "[variables('vmNodeType0Name')]",
        "durabilityLevel": "Bronze"
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

* Vm'leri veya Windows Server çalıştıran bilgisayarlarda bir küme oluşturun: [Windows Server için Service Fabric kümesi oluşturma](service-fabric-cluster-creation-for-windows-server.md)
* Bir küme sanal makineleri veya Linux çalıştıran bilgisayarlara oluşturun: [Bir Linux kümesi oluşturma](service-fabric-cluster-creation-via-portal.md)
* [Service Fabric destek seçenekleri](service-fabric-support.md) hakkında bilgi edinin

[Image1]: ./media/service-fabric-best-practices/generate-common-name-cert-portal.png