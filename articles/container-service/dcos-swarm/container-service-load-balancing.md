---
title: Kullanım DıŞı Azure DC/OS kümesindeki Yük Dengeleme kapsayıcıları
description: Azure Container Service DC/OS kümesindeki birden çok kapsayıcıda yükü dengeleyin.
author: rgardler
ms.service: container-service
ms.topic: tutorial
ms.date: 06/02/2017
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: a8f863f16888e6eca2dbc72c5dd612c38edbe46e
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78273371"
---
# <a name="deprecated-load-balance-containers-in-an-azure-container-service-dcos-cluster"></a>Kullanım DıŞı Azure Container Service DC/OS kümesinde Yük Dengeleme kapsayıcıları

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Bu makalede, DC/OS tarafından yönetilen bir Azure Container Service içinde bir iç yük dengeleyicinin Marathon-LB kullanılarak nasıl oluşturulacağını inceliyoruz. Bu yapılandırma, uygulamalarınızı yatay olarak ölçeklemenize olanak sağlar. Ayrıca yük dengeleyicilerinizi genel kümeye, uygulama kapsayıcılarınızı ise özel kümeye ekleyerek genel ve özel aracı kümelerinden yararlanmanıza da olanak tanır. Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Marathon Yük Dengeleyici'yi yapılandırma
> * Yük dengeleyici kullanarak uygulama dağıtma
> * Azure yük dengeleyiciyi yapılandırma

Bu öğreticideki adımları tamamlamak için bir ACS DC/OS kümesi gerekir. Gerekirse, [bu betik örneği](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) sizin için bir tane oluşturabilir.

Bu öğretici, Azure CLI 2.0.4 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükseltme gerekiyorsa, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="load-balancing-overview"></a>Yük dengelemeye genel bakış

Azure Container Service DC/OS kümesinde iki yük dengeleme katmanı vardır: 

**Azure Load Balancer** iki genel giriş noktası sağlar (kullanıcıların erişeceği giriş noktaları). Azure LB, Azure Container Service tarafından otomatik olarak sağlanır ve varsayılan olarak 80, 443 ve 8080 numaralı bağlantı noktalarını ortaya çıkaracak şekilde yapılandırılır.

**Marathon Yük Dengeleyici (marathon-lb)** gelen istekleri bu isteklere hizmet eden kapsayıcı örneklerine yönlendirir. Web hizmetini sağlayan kapsayıcılar ölçeklendirilirken marathon-lb dinamik olarak uyarlanır. Bu yük dengeleyici, Container Service'te varsayılan olarak sağlanmaz, ancak yüklenmesi çok kolaydır.

## <a name="configure-marathon-load-balancer"></a>Marathon Yük Dengeleyici'yi yapılandırma

Marathon Yük Dengeleyici dağıttığınız kapsayıcılara göre kendini dinamik olarak yeniden yapılandırır. Bu ayrıca kapsayıcı ya da aracı kaybına karşı esnektir; bu meydana gelirse, Apache Mesos başka yerde kapsayıcıyı yeniden başlatır ve marathon-lB buna uyarlanır.

Tarayıcınızda Cloud Shell açmak için [https://shell.azure.com](https://shell.azure.com) gidin.

Genel aracının kümesine Marathon yük dengeleyiciyi yüklemek için aşağıdaki komutu çalıştırın.

```console
dcos package install marathon-lb
```

## <a name="deploy-load-balanced-application"></a>Yük dengeli uygulamayı dağıtma

Marathon-lb paketine sahip olduğumuza göre yük dengeleme işlemi uygulamak istediğimiz bir uygulama kapsayıcısını dağıtabiliriz. 

İlk olarak, genel olarak kullanıma sunulan aracıların FQDN değerini alın.

```azurecli-interactive
az acs list --resource-group myResourceGroup --query "[0].agentPoolProfiles[0].fqdn" --output tsv
```

Ardından, *hello-web.json* adlı bir dosya oluşturun ve aşağıdaki içeriğe kopyalayın. `HAPROXY_0_VHOST` etiketinin DC/OS aracılarının FQDN değeriyle güncelleştirilmesi gerekir. 

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}
```

DC/OS CLI'yi kullanarak uygulamayı çalıştırın. Varsayılan olarak Marathon, uygulamayı özel kümeye dağıtır. Bu da yukarıdaki dağıtıma yalnızca yük dengeleyici üzerinden erişilebileceği anlamına gelir ve çoğunlukla istenen davranış budur.

```console
dcos marathon app add hello-web.json
```

Uygulama dağıtıldıktan sonra, yük dengeli uygulamayı görmek için aracı kümesinin FQDN değerine göz atın.

![Yük dengeli uygulamanın resmi](./media/container-service-load-balancing/lb-app.png)

## <a name="configure-azure-load-balancer"></a>Azure Load Balancer'ı yapılandırma

Varsayılan olarak, Azure Load Balancer 80, 8080 ve 443 bağlantı noktalarını ortaya çıkarır. Bu bağlantı noktalarından birini kullanıyorsanız (yukarıdaki örnekte olduğu gibi) bir şey yapmanıza gerek yoktur. Aracı yük dengeleyicinizin FQDN’sini bulabilmeniz gerekir ve her yenilediğinizde üç web sunucunuzdan birine isabet edersiniz. 

Farklı bir bağlantı noktası kullanıyorsanız, kullandığınız bağlantı noktası için yük dengeleyiciye hepsini bir kez deneme kuralı ve bir araştırma eklemeniz gerekir. Bunu [Azure CLI](../../azure-resource-manager/xplat-cli-azure-resource-manager.md)’dan `azure network lb rule create` ve `azure network lb probe create` komutlarıyla yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki eylemlerle birlikte hem Marathon hem de Azure yük dengeleyicileriyle ACS'de yük dengelemeyi öğrendiniz:

> [!div class="checklist"]
> * Marathon Yük Dengeleyici'yi yapılandırma
> * Yük dengeleyici kullanarak uygulama dağıtma
> * Azure yük dengeleyiciyi yapılandırma

Azure depolamayı Azure'da DC/OS ile tümleştirmeyi öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [DC/OS kümesinde Azure Dosya Paylaşımı'nı bağlama](container-service-dcos-fileshare.md)
