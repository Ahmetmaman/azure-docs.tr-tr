---
title: Kapsayıcı örneği üzerinde lizleştirme araştırması ayarlama
description: Azure Container Instances ' de uygun olmayan kapsayıcıları yeniden başlatmak için Lida araştırmaların nasıl yapılandırılacağını öğrenin
ms.topic: article
ms.date: 07/02/2020
ms.openlocfilehash: befe9693be1413abf455d915814c53aab20db53c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "86169706"
---
# <a name="configure-liveness-probes"></a>Canlılık yoklaması yapılandırma

Kapsayıcılı uygulamalar uzun süre çalışabilir, bu da kapsayıcıyı yeniden başlatarak onarılması gerekebilecek hatalı durumlara neden olabilir. Azure Container Instances, kapsayıcı grubunuzdaki Kapsayıcılarınızı, kritik işlevler çalışmıyorsa yeniden başlatılacak şekilde yapılandırabileceğiniz şekilde, lilik araştırmalarını destekler. Limek araştırması bir [Kubernetes araştırması](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)gibi davranır.

Bu makalede, sanal sağlıksız bir kapsayıcının otomatik olarak yeniden başlatılmasını gösteren, limize Me araştırması içeren bir kapsayıcı grubunun nasıl dağıtılacağı açıklanır.

Azure Container Instances Ayrıca, trafiğin yalnızca kendisine hazır olduğu zaman bir kapsayıcıya ulaşmasını sağlamak için yapılandırabileceğiniz [hazırlık araştırmalarını](container-instances-readiness-probe.md)destekler.

> [!NOTE]
> Şu anda bir sanal ağa dağıtılan bir kapsayıcı grubunda bir lizleştirme araştırması kullanamazsınız.

## <a name="yaml-deployment"></a>YAML dağıtımı

`liveness-probe.yaml`Aşağıdaki kod parçacığına sahip bir dosya oluşturun. Bu dosya, sonunda sağlıksız hale gelen NGNX kapsayıcısından oluşan bir kapsayıcı grubunu tanımlar.

```yaml
apiVersion: 2019-12-01
location: eastus
name: livenesstest
properties:
  containers:
  - name: mycontainer
    properties:
      image: nginx
      command:
        - "/bin/sh"
        - "-c"
        - "touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600"
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      livenessProbe:
        exec:
            command:
                - "cat"
                - "/tmp/healthy"
        periodSeconds: 5
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Yukarıdaki YAML yapılandırmasıyla Bu kapsayıcı grubunu dağıtmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az container create --resource-group myResourceGroup --name livenesstest -f liveness-probe.yaml
```

### <a name="start-command"></a>Başlat komutu

Dağıtım, `command` kapsayıcının ilk kez çalışmaya başladığı zaman çalışan bir başlangıç komutu tanımlayan bir özelliği içerir. Bu özellik bir dize dizisini kabul eder. Bu komut, uygun olmayan bir durum girerek kapsayıcının benzetimini yapar.

İlk olarak, bir bash oturumu başlatır ve dizin içinde adlı bir dosya oluşturur `healthy` `/tmp` . Daha sonra dosyayı silmeden önce 30 saniye boyunca uykuya geçer ve 10 dakikalık bir uyku moduna girer:

```bash
/bin/sh -c "touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600"
```

### <a name="liveness-command"></a>Lida komutu

Bu dağıtım `livenessProbe` `exec` , lietler denetimi olarak davranan bir lietler komutunu destekleyen bir tanımlar. Bu komut sıfır olmayan bir değerle çıkış alıyorsa, kapsayıcı sonlandırılır ve yeniden başlatılır ve `healthy` Dosya bulunamadığına işaret edilir. Bu komut, çıkış kodu 0 ile başarılı bir şekilde çıkılırken hiçbir işlem yapılmaz.

`periodSeconds`Özelliği, ebilmelidir komutunun her 5 saniyede bir yürütülmesi gerektiğini gösterir.

## <a name="verify-liveness-output"></a>Libir çıktıyı doğrula

İlk 30 saniye içinde, `healthy` Start komutu tarafından oluşturulan dosya vardır. Liki komutu `healthy` dosyanın varlığını denetlediğinde, durum kodu 0 döndürür, başarılı olarak sinyal verir ve yeniden başlatma gerçekleşmez.

30 saniye sonra, `cat /tmp/healthy` komut başarısız olur, sağlıksız ve olayları sonlandırma olayları oluşmasına neden olur.

Bu olaylar Azure portal veya Azure CLı 'dan görüntülenebilir.

![Portalın sağlıksız olayı][portal-unhealthy]

Azure portal olayları görüntüleyerek, türündeki olaylar, uygun `Unhealthy` olmayan komut başarısız olduğunda tetiklenir. Sonraki olay, `Killing` bir yeniden başlatmanın başlaması için bir kapsayıcı silmeyi belirten türdür. Bu olay gerçekleştiğinde kapsayıcının yeniden başlatma sayısı artar.

Yeniden başlatmalar, genel IP adresleri ve düğüme özgü içerikler gibi kaynakların korunması için yerinde tamamlanır.

![Portal yeniden başlatma sayacı][portal-restart]

Destekleneme araştırması sürekli olarak başarısız olur ve çok fazla yeniden başlatma tetiklerse, Kapsayıcınız bir üstel geri dönme gecikmesi girer.

## <a name="liveness-probes-and-restart-policies"></a>Lizleştirme araştırmaları ve yeniden başlatma ilkeleri

Yeniden başlatma ilkeleri, lilezleştirme araştırmaları tarafından tetiklenen yeniden başlatma davranışının yerini alır Örneğin, bir `restartPolicy = Never` *ve* bir lizleştirme araştırması ayarlarsanız, başarısız bir denetim nedeniyle kapsayıcı grubu yeniden başlatmaz. Bunun yerine kapsayıcı grubu, kapsayıcı grubunun öğesinin yeniden başlatma ilkesine uyar `Never` .

## <a name="next-steps"></a>Sonraki adımlar

Önkoşul olmayan bir işlev düzgün çalışmıyorsa, görev tabanlı senaryolar otomatik yeniden başlatmaları etkinleştirmek için bir araştırma gerektirebilir. Görev tabanlı kapsayıcılar çalıştırma hakkında daha fazla bilgi için, bkz. [Azure Container Instances Kapsayıcılı görevleri çalıştırma](container-instances-restart-policy.md).

<!-- IMAGES -->
[portal-unhealthy]: ./media/container-instances-liveness-probe/unhealthy-killing.png
[portal-restart]: ./media/container-instances-liveness-probe/portal-restart.png
