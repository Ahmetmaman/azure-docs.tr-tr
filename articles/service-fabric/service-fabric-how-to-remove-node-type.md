---
title: Azure Service Fabric düğüm türünü kaldırma | Microsoft Docs
description: Azure 'da çalışan bir Service Fabric kümesinden düğüm türünü kaldırmayı öğrenin.
author: inputoutputcode
manager: sridmad
ms.topic: conceptual
ms.date: 08/11/2020
ms.author: chrpap
ms.openlocfilehash: ede999bee9ce1a4a9dd10652a2c52a840d5b24be
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "88163586"
---
# <a name="how-to-remove-a-service-fabric-node-type"></a>Service Fabric düğüm türünü kaldırma
Bu makalede, var olan bir düğüm türünü kümeden kaldırarak bir Azure Service Fabric kümesinin nasıl ölçeklendiriyapılacağı açıklanır. Service Fabric küme, mikro hizmetlerinizin dağıtıldığı ve yönetildiği, ağa bağlı bir sanal veya fiziksel makine kümesidir. Bir kümenin parçası olan makine veya VM, düğüm olarak adlandırılır. Sanal Makine Ölçek Kümeleri, bir sanal makine koleksiyonunu bir küme olarak dağıtmak ve yönetmek için kullandığınız bir Azure işlem kaynağıdır. Bir Azure kümesinde tanımlanan her düğüm türü [ayrı bir ölçek kümesi olarak ayarlanır](service-fabric-cluster-nodetypes.md). Her düğüm türü ayrıca yönetilebilir. Bir Service Fabric kümesi oluşturduktan sonra, düğüm türünü (sanal makine ölçek kümesi) ve tüm düğümlerini kaldırarak bir kümeyi yatay olarak ölçeklendirebilirsiniz.  Küme üzerinde iş yükleri çalışırken bile kümeyi istediğiniz zaman ölçeklendirebilirsiniz.  Küme ölçeklenirken uygulamalarınız da otomatik olarak ölçeklendirilir.

> [!WARNING]
> Bir üretim kümesinden düğüm türünü kaldırmak için bu yaklaşımı kullanmak, sıklıkla kullanılması önerilmez. Düğüm türünün arkasındaki sanal makine ölçek kümesi kaynağını sildiği için bu, tehlikeli bir komuttur. 

## <a name="durability-characteristics"></a>Dayanıklılık özellikleri
Remove-AzServiceFabricNodeType kullanılırken güvenlik, hızlara göre önceliklendirilir. Düğüm türü gümüş veya altın [dayanıklılık düzeyi](./service-fabric-cluster-capacity.md#durability-characteristics-of-the-cluster)olmalıdır, nedeni:
- Bronz durum bilgilerini kaydetme hakkında garanti vermez.
- Gümüş ve altın dayanıklılık, ölçek kümesinde yapılan değişiklikleri yakalar.
- Altın Ayrıca ölçek kümesi altında Azure güncelleştirmeleri üzerinde denetim elde etmenizi sağlar.

Verilerin kaybolmaması için temel değişiklikleri ve güncelleştirmeleri "düzenler" Service Fabric. Ancak, bronz dayanıklılık ile bir düğüm türünü kaldırdığınızda durum bilgilerini kaybedebilirsiniz. Birincil düğüm türünü kaldırıyorsanız ve uygulamanız durum bilgisdeyse, bronz kabul edilebilir. Üretim için durum bilgisi olan iş yüklerini çalıştırdığınızda, en düşük yapılandırma gümüş olmalıdır. Benzer şekilde, üretim senaryolarında birincil düğüm türü her zaman gümüş veya altın olmalıdır.

### <a name="more-about-bronze-durability"></a>Bronz dayanıklılık hakkında daha fazla bilgi

Bronz olan bir düğüm türü kaldırılırken, düğüm türündeki tüm düğümler hemen aşağı gider. Service Fabric, hiçbir bronz düğüm ölçek kümesi güncelleştirmesini yakalamaz, bu nedenle tüm VM 'Ler hemen aşağı gider. Bu düğümlerde durum bilgisi olan herhangi bir şey varsa, veriler kaybolur. Artık durum bilgisiz olsa bile Service Fabric tüm düğümler halkaya katılır, bu nedenle tüm bir komşu kaybolabilir ve bu da kümenin kendisinin kararlılığını bozabilir.

## <a name="remove-a-node-type"></a>Düğüm türünü kaldırma

1. İşleme başlamadan önce lütfen bu önkoşulların önüne danışın.

    - Küme sağlıklı.
    - Düğüm türü kaldırıldıktan sonra yine yeterli kapasite olacaktır, örn. gerekli çoğaltma sayısının yerleştirileceği düğüm sayısı.

2. Düğüm türünü kullanmak için yerleştirme kısıtlamalarına sahip tüm hizmetleri taşıyın.

    - Uygulama/hizmet bildirimini artık düğüm türüne başvurmayacak şekilde değiştirin.
    - Değişikliği dağıtın.

    Ardından şunları doğrulayın:
    - Yukarıda değiştirilen tüm hizmetler artık düğüm türüne ait düğüm üzerinde çalışmıyor.
    - Tüm hizmetler sağlıklı.

3. Düğüm türünü birincil olmayan olarak işaretini kaldır (birincil düğüm olmayan türler için atla)

    - Dağıtım için kullanılan Azure Resource Manager şablonunu bulun.
    - Service Fabric bölümündeki düğüm türüyle ilgili bölümü bulun.
    - IsPrimary özelliğini false olarak değiştirin. * * Bu görevdeki düğüm türüyle ilgili bölümü kaldırmayın.
    - Değiştirilen Azure Resource Manager şablonunu dağıtın. * * Küme yapılandırmasına bağlı olarak bu adım biraz zaman alabilir.
    
    Ardından şunları doğrulayın:
    - Portalın Service Fabric bölümü, kümenin hazırlandığını gösterir.
    - Küme sağlıklı.
    - Düğüm türüne ait düğümlerin hiçbiri çekirdek düğüm olarak işaretlenmiş.

4. Düğüm türündeki her düğümü devre dışı bırakın.

    PowerShell kullanarak kümeye bağlanın ve ardından aşağıdaki adımı çalıştırın.
    
    ```powershell
    $nodeType = "" # specify the name of node type
    $nodes = Get-ServiceFabricNode
    
    foreach($node in $nodes)
    {
      if ($node.NodeType -eq $nodeType)
      {
        $node.NodeName
     
        Disable-ServiceFabricNode -Intent RemoveNode -NodeName $node.NodeName -Force
      }
    }
    ```

    - Bronz dayanıklılık için tüm düğümlerin devre dışı durumuna gelmesini bekleyin
    - Gümüş ve altın dayanıklılık için bazı düğümler devre dışı bırakılacak ve geri kalan durum devre dışı olacaktır. Devre dışı bırakma durumundaki düğümlerin Ayrıntılar sekmesini kontrol edin. Bunlar, altyapı hizmeti bölümleri için çekirdek sağlamaya açık durumdaysa devam etmek güvenli hale gelir.

5. Düğüm türü için verileri durdurun.

    PowerShell kullanarak kümeye bağlanın ve ardından aşağıdaki adımı çalıştırın.
    
    ```powershell
    foreach($node in $nodes)
    {
      if ($node.NodeType -eq $nodeType)
      {
        $node.NodeName
     
        Start-ServiceFabricNodeTransition -Stop -OperationId (New-Guid) -NodeInstanceId $node.NodeInstanceId -NodeName $node.NodeName -StopDurationInSeconds 10000
      }
    }
    ```
    
    Düğüm türü için tüm düğümlerin aşağı işaretlendiğinden bekleyin.

6. Özgün sanal makine ölçek kümesindeki düğümleri serbest bırakma
    
    Ölçek kümesinin dağıtıldığı Azure aboneliğinde oturum açın ve sanal makine ölçek kümesini kaldırın. 

    ```powershell
    $scaleSetName="myscaleset"
    $scaleSetResourceType="Microsoft.Compute/virtualMachineScaleSets"
    
    Remove-AzResource -ResourceName $scaleSetName -ResourceType $scaleSetResourceType -ResourceGroupName $resourceGroupName -Force
    ```

    
7. Düğüm türü için verileri kaldırın.

    PowerShell kullanarak kümeye bağlanın ve ardından aşağıdaki adımı çalıştırın.
    
    ```powershell
    foreach($node in $nodes)
    {
      if ($node.NodeType -eq $nodeType)
      {
        $node.NodeName
     
        Remove-ServiceFabricNodeState -NodeName $node.NodeName -Force
      }
    }
    ```

    Tüm düğümlerin kümeden çıkarılana kadar bekleyin. Düğümler SFX içinde gösterilmemelidir.

8. Service Fabric bölümden düğüm türünü kaldırın.

    - Dağıtım için kullanılan Azure Resource Manager şablonunu bulun.
    - Service Fabric bölümündeki düğüm türüyle ilgili bölümü bulun.
    - Düğüm türüne karşılık gelen bölümü kaldırın.
    - Yalnızca gümüş ve daha yüksek dayanıklılık kümelerinde, şablondaki küme kaynağını güncelleştirin ve sistem durumu ilkelerini, `applicationDeltaHealthPolicies` aşağıda verilen şekilde küme kaynağı altına ekleyerek doku:/sistem uygulama durumunu yoksayacak şekilde yapılandırın `properties` . Aşağıdaki ilke var olan hataları yoksayacak, ancak yeni sistem durumu hatalarına izin vermez. 
 
 
     ```json
    "upgradeDescription":  
    { 
      "forceRestart": false, 
      "upgradeReplicaSetCheckTimeout": "10675199.02:48:05.4775807", 
      "healthCheckWaitDuration": "00:05:00", 
      "healthCheckStableDuration": "00:05:00", 
      "healthCheckRetryTimeout": "00:45:00", 
      "upgradeTimeout": "12:00:00", 
      "upgradeDomainTimeout": "02:00:00", 
      "healthPolicy": { 
        "maxPercentUnhealthyNodes": 100, 
        "maxPercentUnhealthyApplications": 100 
      }, 
      "deltaHealthPolicy":  
      { 
        "maxPercentDeltaUnhealthyNodes": 0, 
        "maxPercentUpgradeDomainDeltaUnhealthyNodes": 0, 
        "maxPercentDeltaUnhealthyApplications": 0, 
        "applicationDeltaHealthPolicies":  
        { 
            "fabric:/System":  
            { 
                "defaultServiceTypeDeltaHealthPolicy":  
                { 
                        "maxPercentDeltaUnhealthyServices": 0 
                } 
            } 
        } 
      } 
    },
    ```

    - Değiştirilen Azure Resource Manager şablonunu dağıtın. * * Bu adım genellikle iki saate kadar sürer. Bu yükseltme ayarları InfrastructureService olarak değiştirecek, bu nedenle bir düğümün yeniden başlatılması gerekiyor. Bu durumda `forceRestart` yok sayılır. 
    Parametresi, `upgradeReplicaSetCheckTimeout` Service Fabric bir bölümün güvenli durumda olmasını bekleyeceği en uzun süreyi belirtir, daha önceden güvenli bir durumda değildir. Güvenlik denetimleri bir düğümdeki tüm bölümler için başarılı olduktan sonra, bu düğümdeki yükseltmeye devam eder Service Fabric.
    Parametrenin değeri `upgradeTimeout` 6 saate indirgenmiş olabilir, ancak maxhayvan güvenliği 12 saat kullanılmalıdır.

    Ardından şunları doğrulayın:
    - Portalda Service Fabric kaynak, Ready olarak görünür.

9. ARM şablonundan düğüm türüyle ilişkili kaynaklara yönelik tüm başvuruları kaldırın.

    - Dağıtım için kullanılan Azure Resource Manager şablonunu bulun.
    - Şablondaki düğüm türüyle ilişkili sanal makine ölçek kümesini ve diğer kaynakları kaldırın.
    - Değişiklikleri dağıtın.

    Ardından:
    - Dağıtımın tamamlanmasını bekleyin.
    
10. Artık kullanımda olmayan düğüm türüyle ilgili kaynakları kaldırın. Örnek Load Balancer ve genel IP. 

    - Bu kaynakları kaldırmak için, belirli kaynak türünü ve API sürümünü belirten 6. adımda kullanılan PowerShell komutunu kullanabilirsiniz. 

> [!Note]
> Aynı Load Balancer ve IP, düğüm türleri arasında yeniden kullanılırsa bu adım isteğe bağlıdır.

## <a name="next-steps"></a>Sonraki adımlar
- Küme [dayanıklılığı özellikleri](./service-fabric-cluster-capacity.md#durability-characteristics-of-the-cluster)hakkında daha fazla bilgi edinin.
- [Düğüm türleri ve sanal makine ölçek kümeleri](service-fabric-cluster-nodetypes.md)hakkında daha fazla bilgi edinin.
- [Service Fabric kümesi ölçeklendirme](service-fabric-cluster-scaling.md)hakkında daha fazla bilgi edinin.
