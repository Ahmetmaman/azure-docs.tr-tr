---
title: Azure Service Fabric ağı 'nda çalışan bir uygulamayı otomatik ölçeklendirme
description: Bir Service Fabric Mesh uygulamasının Hizmetleri için otomatik ölçeklendirme ilkeleri yapılandırmayı öğrenin.
author: dkkapur
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: fb72806dd7ba838ba7170bda409715bc074e1d99
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75461983"
---
# <a name="create-autoscale-policies-for-a-service-fabric-mesh-application"></a>Bir Service Fabric Mesh uygulaması için otomatik ölçeklendirme ilkeleri oluşturma
Service Fabric kafese uygulama dağıtmanın başlıca avantajlarından biri, hizmetlerinizi kolayca ölçeklendirebilme olanağı sağlar. Bu, hizmetinizdeki farklı yük miktarını işlemek veya kullanılabilirliği iyileştirmek için kullanılmalıdır. El ile veya hizmetlerinizi ölçeklendirmek veya otomatik ölçeklendirme ilkeleri ayarlayın.

[Otomatik ölçeklendirme](service-fabric-mesh-scalability.md#autoscaling-service-instances) (yatay ölçeklendirme), hizmet örneği sayısını dinamik olarak ölçeklendirmenize olanak tanıyor. Otomatik ölçeklendirme, büyük esneklik sağlar ve sağlama veya CPU veya bellek kullanım oranlarına dayalı hizmet örnekleri kaldırılmasını sağlar.

## <a name="options-for-creating-an-auto-scaling-policy-trigger-and-mechanism"></a>Ölçeklendirme İlkesi, tetikleyici ve mekanizması otomatik oluşturmaya yönelik seçenekleri
Otomatik ölçeklendirme İlkesi, ölçeklendirmek istediğiniz her hizmet için tanımlanır. İlke, YAML hizmeti kaynak dosyası veya JSON dağıtım şablonu tanımlanır. Her bir ölçeklendirme İlkesi iki bölümden oluşur: bir tetikleyici ve ölçeklendirme mekanizması.

Tetikleyici, bir otomatik ölçeklendirme İlkesi zaman çağrılır tanımlar.  Tetikleyici (ortalama yük) ve (CPU veya bellek) izlenecek ölçüm türünü belirtin.  Üst ve alt yük eşiklerini yüzde belirtilir. Ölçek aralığı (saniye cinsinden) genellikle belirtilen kullanımı (örneğin, ortalama CPU yükü) tüm şu anda dağıtılmış hizmet örnekleri arasında nasıl kontrol edileceğini tanımlar.  İzlenen ölçüm alt eşiğin altında düşene veya üst eşiğin üstünde artırır mekanizması tetiklenir.  

Ölçeklendirme mekanizması ilke tetiklendiğinde ölçeklendirme işlemi gerçekleştirmek nasıl tanımlar.  Mekanizması (ekleme/kaldırma çoğaltması) türü belirttiğinizde, (tamsayı olarak) en düşük ve en yüksek yineleme sayısı.  Hizmet yineleme sayısı aşağıdaki en düşük sayısı veya en yüksek sayı yukarıda hiçbir zaman ayarlanacaktır.  Ayrıca, eklenen veya kaldırılan bir ölçeklendirme işlemi yineleme sayısı bir tamsayı olarak ölçek artırma belirtin.  

## <a name="define-an-auto-scaling-policy-in-a-json-template"></a>Otomatik ölçeklendirme İlkesi JSON şablonunda tanımlama

Aşağıdaki örnek, bir otomatik ölçeklendirme İlkesi JSON dağıtım şablonu gösterir.  Otomatik ölçeklendirme İlkesi bir özelliğe ölçeklenmesi hizmetinin bildirilir.  Bu örnekte, bir CPU ortalama yük tetikleyici tanımlanır.  Ortalama CPU (% 20) 0.2 veya gelecek 0.8 (% 80) yukarıda aşağıda tüm dağıtılan örnekleri düşme yüklerseniz mekanizması tetiklenir.  CPU yükü, 60 saniyede denetlenir.  Ölçeklendirme mekanizması eklemek veya ilkeyi tetikleniyorsa, örnekleri kaldırmak için tanımlanır.  Hizmet örnekleri eklenebilir veya bir artışlarla kaldırıldı.  Aşağıdakilerden en az örnek sayısını ve en fazla örnek sayısını 40 de tanımlanır.

```json
{
"apiVersion": "2018-09-01-preview",
"name": "WorkerApp",
"type": "Microsoft.ServiceFabricMesh/applications",
"location": "[parameters('location')]",
"dependsOn": [
"Microsoft.ServiceFabricMesh/networks/worker-app-network"
],
"properties": {
"services": [   
    { ... },       
    {
    "name": "WorkerService",
    "properties": {
        "description": "Worker Service",
        "osType": "linux",
        "codePackages": [
        {  ...              }
        ],
        "replicaCount": 1,
        "autoScalingPolicies": [
        {
            "name": "AutoScaleWorkerRule",
            "trigger": {
                "kind": "AverageLoad",
                "metric": {
                    "kind": "Resource",
                    "name": "cpu"
                },
                "lowerLoadThreshold": "0.2",
                "upperLoadThreshold": "0.8",
                "scaleIntervalInSeconds": "60"
            },
            "mechanism": {
                "kind": "AddRemoveReplica",
                "minCount": "1",
                "maxCount": "40",
                "scaleIncrement": "1"
            }
        }
        ],        
        ...
    }
    }
]
}
}
```

## <a name="define-an-autoscale-policy-in-a-serviceyaml-resource-file"></a>Service.yaml kaynak dosyada bir otomatik ölçeklendirme ilkesi tanımlama
Aşağıdaki örnek, bir hizmet kaynağı (YAML) dosyasında bir otomatik ölçeklendirme İlkesi gösterir.  Otomatik ölçeklendirme İlkesi hizmetin ölçeği bir özellik olarak bildirilir.  Bu örnekte, bir CPU ortalama yük tetikleyici tanımlanır.  Ortalama CPU (% 20) 0.2 veya gelecek 0.8 (% 80) yukarıda aşağıda tüm dağıtılan örnekleri düşme yüklerseniz mekanizması tetiklenir.  CPU yükü, 60 saniyede denetlenir.  Ölçeklendirme mekanizması eklemek veya ilkeyi tetikleniyorsa, örnekleri kaldırmak için tanımlanır.  Hizmet örnekleri eklenebilir veya bir artışlarla kaldırıldı.  Aşağıdakilerden en az örnek sayısını ve en fazla örnek sayısını 40 de tanımlanır.

```yaml
## Service definition ##
application:
  schemaVersion: 1.0.0-preview2
  name: WorkerApp
  properties:
    services:
      - name: WorkerService
        properties:
          description: WorkerService description.
          osType: Linux
          codePackages:
            ...
          replicaCount: 1
          autoScalingPolicies:
            - name: AutoScaleWorkerRule
              trigger:
                kind: AverageLoad
                metric:
                  kind: Resource
                  name: cpu
                lowerLoadThreshold: 0.2
                upperLoadThreshold: 0.8
                scaleIntervalInSeconds: 60
              mechanism:
                kind: AddRemoveReplica
                minCount: 1
                maxCount: 40
                scaleIncrement: 1
          ...
```

## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [hizmet el ile ölçeklendirme](service-fabric-mesh-tutorial-template-scale-services.md)
