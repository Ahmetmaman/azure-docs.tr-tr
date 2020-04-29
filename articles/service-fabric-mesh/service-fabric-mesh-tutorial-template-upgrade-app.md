---
title: Öğretici-Azure Service Fabric ağı 'nda çalışan bir uygulamayı yükseltme
description: Bu öğreticide, Service Fabric Mesh’te çalışan Service Fabric uygulamasını yükseltmeyi öğreneceksiniz.
author: dkkapur
ms.topic: tutorial
ms.date: 01/11/2019
ms.author: dekapur
ms.custom: mvc, devcenter
ms.openlocfilehash: 42db17fa6474d3230bc523d0cf65b375cf01276e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75351741"
---
# <a name="tutorial-upgrade-a-service-fabric-application-running-in-service-fabric-mesh"></a>Öğretici: Service Fabric Mesh’te çalışan Service Fabric uygulamasını yükseltme

Bu öğretici, bir serinin üçüncü bölümüdür. Ayrılan CPU kaynaklarını artırarak, [daha önce Service Fabric Mesh'te dağıtılmış](service-fabric-mesh-tutorial-template-deploy-app.md) Service Fabric uygulamasını yükseltmeyi öğreneceksiniz.  İşiniz bittiğinde, daha yüksek CPU kaynaklarıyla çalışan bir Web ön uç hizmetiniz olur.

Serinin üçüncü bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uygulama yapılandırmalarını değiştirme
> * Service Fabric Mesh’te çalışan bir uygulamayı yükseltme

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [Şablon kullanarak Service Fabric Mesh’e uygulama dağıtma](service-fabric-mesh-tutorial-template-deploy-app.md)
> * [Service Fabric Mesh’te çalışan bir uygulamayı ölçeklendirme](service-fabric-mesh-tutorial-template-scale-services.md)
> * Service Fabric Mesh’te çalışan bir uygulamayı yükseltme
> * [Uygulamayı kaldırma](service-fabric-mesh-tutorial-template-remove-app.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturabilirsiniz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) .

* [Azure Cloud Shell](service-fabric-mesh-howto-setup-cli.md)’i açın veya [Azure CLI ve Service Fabric Mesh CLI’sini yerel olarak yükleyin](service-fabric-mesh-howto-setup-cli.md#install-the-azure-service-fabric-mesh-cli).

## <a name="upgrade-application-configurations"></a>Uygulama yapılandırmalarını yükseltme

Service Fabric Mesh’e uygulamaları dağıtmanın ana avantajlarından biri, uygulama yapılandırmanızı kolayca yükseltebilmenizdir.  Örneğin, hizmetleriniz için CPU veya bellek kaynakları yükseltilebilir.

Bu öğreticide, örnek olarak, [önceden dağıtılmış olan](service-fabric-mesh-tutorial-template-deploy-app.md) ve şimdi çalışıyor olması gereken Yapılacaklar Listesi örneği kullanılmaktadır. Uygulamada iki hizmet vardır: WebFrontEnd ve ToDoService. Her hizmet başlangıçta 0.5 CPU kaynağıyla dağıtılmıştır.  WebFrontEnd hizmetinin CPU kaynaklarını görüntülemek için aşağıdakileri çalıştırın:

```azurecli
az mesh service show --resource-group myResourceGroup --name WebFrontEnd --app-name todolistapp
```

Uygulama kaynağının dağıtım şablonunda, her hizmetin istenen CPU kaynaklarını ayarlamak için kullanılabilen bir *cpu* özelliği vardır. Uygulama, birden çok hizmetten oluşabilir; her hizmet, birlikte dağıtılan ve yönetilen benzersiz bir *cpu* ayarına sahiptir. Web ön uç hizmetinin CPU kaynaklarını artırmak için dağıtım şablonu veya parametreler dosyasındaki *cpue* değerini değiştirin.  Ardından uygulamayı yükseltin.

### <a name="modify-the-deployment-template-parameters"></a>Dağıtım şablonu parametrelerini değiştirme

Şablonunuzda, uygulama dağıtıldıktan sonra değişeceğini tahmin ettiğiniz değerler olduğunda veya dağıtım bazında değiştirme seçeneğine sahip olmak istediğinizde (diğer dağıtımlar için bu şablonu yeniden kullanmayı planlıyorsanız) en iyi uygulama, değerlerin parametrelerini oluşturmaktır.

Önceden uygulama, [mesh_rp.windows.json dağıtım şablonu](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.json) ve [mesh_rp.windows.parameter.json parametreleri](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.parameters.json) kullanılarak dağıtılıyordu.

[mesh_rp.windows.parameter.json parametreleri](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.parameters.json) dosyasını yerel olarak açın ve *frontEndCpu* değerini 1 olarak ayarlayın:

```json
      "frontEndCpu":{
        "value": "1"
      }
```

Parametreler dosyasına değişikliklerinizi kaydedin.  

*frontEndCpu* parametresi, [mesh_rp.windows.json dağıtım şablonunun](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/todolist/mesh_rp.windows.json)*parametreler* bölümünde bildirilir:

```json
"frontEndCpu": {
    "defaultValue": "0.5",
    "type": "string",
    "metadata": {
        "description": "The CPU resources for the front end web service."
    }
}
```

WebFrontEnd hizmeti *codePackages->resources->requests->cpu* özelliği *frontEndCpu* parametresine başvurur:

```json
    "services": [
          {
            "name": "WebFrontEnd",
            "properties": {
              "description": "WebFrontEnd description.",
              "osType": "Windows",
              "codePackages": [
                {
                  "name": "WebFrontEnd",
                  "image": "[parameters('frontEndImage')]",
                  ...
                  "resources": {
                    "requests": {
                      "cpu": "[parameters('frontEndCpu')]",
                      "memoryInGB": "[parameters('frontEndMemory')]"
                    }
                  },
                  "imageRegistryCredential": {
                    "server": "[parameters('registryServer')]",
                    "username": "[parameters('registryUserName')]",
                    "password": "[parameters('registryPassword')]"
                  }
                }
              ],
              ...
```

### <a name="upgrade-your-application"></a>Uygulamanızı yükseltme

Uygulama çalışırken, şablonu ve güncelleştirilmiş parametreler dosyasını yeniden dağıtarak uygulamayı yükseltebilirsiniz:

```azurecli
az mesh deployment create --resource-group myResourceGroup --template-file c:\temp\mesh_rp.windows.json --parameters c:\temp\mesh_rp.windows.parameters.json
```

Böylece uygulamanıza yönelik sıralı yükseltme başlatılır ve birkaç dakika içinde CPU kaynaklarının arttığını görmeniz gerekir.  WebFrontEnd hizmetinin CPU kaynaklarını görüntülemek için aşağıdakileri çalıştırın:

```azurecli
az mesh service show --resource-group myResourceGroup --name WebFrontEnd --app-name todolistapp
```

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Uygulama yapılandırmalarını değiştirme
> * Service Fabric Mesh’te çalışan bir uygulamayı yükseltme

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Service Fabric Mesh uygulamasını kaldırma](service-fabric-mesh-tutorial-template-remove-app.md)
