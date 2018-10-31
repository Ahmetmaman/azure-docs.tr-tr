---
title: Azure Container ınstances'da ortam değişkenlerini ayarlama
description: Azure Container Instances'da çalıştırdığınız kapsayıcılarında ortam değişkenlerini ayarlama hakkında bilgi edinin
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 07/19/2018
ms.author: danlep
ms.openlocfilehash: 1fcb72d75dca99b6670ca3ff536403a6268554a2
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50238157"
---
# <a name="set-environment-variables"></a>Ortam değişkenlerini belirleme

Kapsayıcı örneklerinizin ortam değişkenlerini ayarlama, uygulama veya betik çalıştırma kapsayıcı tarafından dinamik yapılandırma sağlamanıza olanak verir. Bir kapsayıcıda ortam değişkenlerini ayarlamak için bir kapsayıcı örneği oluşturduğunuzda bunları belirtin. Bir kapsayıcı ile başlattığınızda ortam değişkenlerini ayarlayabilirsiniz [Azure CLI](#azure-cli-example), [Azure PowerShell](#azure-powershell-example)ve [Azure portalında](#azure-portal-example).

Örneğin, çalıştırdığınız [Acı/microsoft-wordcount] [ aci-wordcount] kapsayıcı görüntüsü, aşağıdaki ortam değişkenlerini belirterek davranışını değiştirebilirsiniz:

*NumWords*: STDOUT gönderilen sözcük sayısı.

*MinLength*: en az bir sözcük, sayılması için karakter sayısı. Daha yüksek bir sayı ortak kelimeler gibi "," ve "." yok sayar.

Gizli ortam değişkenleri olarak geçirmek gerekiyorsa, Azure Container Instances destekler [güvenli değerleri](#secure-values) hem Windows hem de Linux kapsayıcıları için.

## <a name="azure-cli-example"></a>Azure CLI örneği

Varsayılan çıktısını görmek için [Acı/microsoft-wordcount] [ aci-wordcount] çalıştırabilmesi önce bu kapsayıcı, [az kapsayıcı oluşturma] [ az-container-create] komut (Hayır ortam değişkenlerini) belirtilen:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Çıkış değiştirmek için ikinci bir kapsayıcı ile Başlat `--environment-variables` eklendi, bağımsız değişkeni için değerleri belirtme *NumWords* ve *MinLength* değişkenleri:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables 'NumWords'='5' 'MinLength'='8'
```

Olarak her iki kapsayıcıları durumunu gösterir. bir kez *kesildi* (kullanın [az container show] [ az-container-show] durumunu denetlemek için), kendi günlükleriyle görüntülemek [az kapsayıcı günlüklerini] [ az-container-logs] çıktıyı görmek için.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer1
az container logs --resource-group myResourceGroup --name mycontainer2
```

Kapsayıcılar çıktısını Göster ortam değişkenlerini ayarlayarak ikinci kapsayıcının komut davranışını nasıl değiştirdiğinizi.

```console
azureuser@Azure:~$ az container logs --resource-group myResourceGroup --name mycontainer1
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]

azureuser@Azure:~$ az container logs --resource-group myResourceGroup --name mycontainer2
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]
```

## <a name="azure-powershell-example"></a>Azure PowerShell örneği

PowerShell'de ortam değişkenlerini ayarlamak için CLI'yı benzer, ancak kullandığı `-EnvironmentVariable` komut satırı bağımsız değişkeni.

İlk olarak, başlatma [Acı/microsoft-wordcount] [ aci-wordcount] kapsayıcı bu, varsayılan yapılandırmasında [New-AzureRmContainerGroup] [ new-azurermcontainergroup]komutu:

```azurepowershell-interactive
New-AzureRmContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer1 `
    -Image microsoft/aci-wordcount:latest
```

Şimdi aşağıdakini çalıştırarak [New-AzureRmContainerGroup] [ new-azurermcontainergroup] komutu. Bu bir belirtir *NumWords* ve *MinLength* ortam değişkenleri, bir dizi değişkenini doldurma sonra `envVars`:

```azurepowershell-interactive
$envVars = @{'NumWords'='5';'MinLength'='8'}
New-AzureRmContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer2 `
    -Image microsoft/aci-wordcount:latest `
    -RestartPolicy OnFailure `
    -EnvironmentVariable $envVars
```

Her iki kapsayıcı durumu olduğunda *kesildi* (kullanın [Get-AzureRmContainerInstanceLog] [ azure-instance-log] durumunu denetlemek için), kendi günlükleriyle çekme [ Get-AzureRmContainerInstanceLog] [ azure-instance-log] komutu.

```azurepowershell-interactive
Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
```

Ortam değişkenlerini ayarlayarak kapsayıcı tarafından çalıştırılacak betiği nasıl değiştirdiğiniz her kapsayıcı için çıktıyı gösterir.

```console
PS Azure:\> Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]

Azure:\
PS Azure:\> Get-AzureRmContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]

Azure:\
```

## <a name="azure-portal-example"></a>Azure portal örneği

Azure portalında bir kapsayıcı başlatma, ortam değişkenlerini ayarlamak için bunları belirtin **yapılandırma** sayfasında kapsayıcısı oluşturduğunuzda.

Portal ile dağıttığınızda, üç değişkenler şu anda sınırlı ve şu biçimde girmeniz gerekir: `"variableName":"value"`

Bir örnek için başlangıç [Acı/microsoft-wordcount] [ aci-wordcount] ile kapsayıcı *NumWords* ve *MinLength* değişkenleri.

1. İçinde **yapılandırma**ayarlayın **yeniden ilke** için *başarısız*
2. Girin `"NumWords":"5"` birinci değişken için olan seçin **Evet** altında **ek ortam değişkenlerini ekleyin**girin `"MinLength":"8"` ikinci değişken için. Seçin **Tamam** doğrulayın ve ardından kapsayıcıya dağıtın.

![Ortam değişkeni etkinleştir düğme ve metin kutularını gösteren portal sayfası][portal-env-vars-01]

Kapsayıcının günlüklerini altında görüntülemek için **ayarları** seçin **kapsayıcıları**, ardından **günlükleri**. Çıktıya benzer önceki CLI ve powershell'dekilerden bölümler, betiğin davranışı ortam değişkenlerini şu yollarla nasıl değiştirilmiş gördüğünüz gösterilir. Yalnızca beş sözcükleri görüntülenir, her biri en az sekiz karakter uzunluğu.

![Portal gösteren kapsayıcının günlük çıktısını][portal-env-vars-02]

## <a name="secure-values"></a>Güvenli değerleri

Nesneleri güvenli değerlerle parolaları veya uygulamanız için anahtarları gibi hassas bilgileri tutmak için tasarlanmıştır. Güvenli değerleri için ortam değişkenlerini kullanarak, hem güvenli hem de, kapsayıcınızın görüntüsünü dahil olmak üzere daha esnektir. Başka bir seçenek açıklanan gizli birimler kullanmaktır [Azure Container ınstances'da bir gizli birimi](container-instances-volume-secret.md).

Güvenli değerlerle ortam değişkenleri görünür değildir, kapsayıcının özelliklerinde--değerlerine yalnızca kapsayıcı içinde erişilebilir. Örneğin, kapsayıcı özellikleri Azure portal veya Azure CLI görüntüde yalnızca güvenli bir değişkenin adını, değeri görüntülenir.

Belirterek bir güvenli bir ortam değişkenini ayarlamak `secureValue` özelliği yerine normal `value` değişkenin türü. Aşağıdaki YAML içinde tanımlanan iki değişken iki değişken türleri gösterilmektedir.

### <a name="yaml-deployment"></a>YAML dağıtım

Oluşturma bir `secure-env.yaml` aşağıdaki kod parçacığı dosyası.

```yaml
apiVersion: 2018-10-01
location: eastus
name: securetest
properties:
  containers:
  - name: mycontainer
    properties:
      environmentVariables:
        - name: 'NOTSECRET'
          value: 'my-exposed-value'
        - name: 'SECRET'
          secureValue: 'my-secret-value'
      image: nginx
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

YAML kapsayıcı grubu dağıtmak için aşağıdaki komutu çalıştırın (gerektiğinde kaynak grubu adını ayarlayın):

```azurecli-interactive
az container create --resource-group myResourceGroup --file secure-env.yaml
```

### <a name="verify-environment-variables"></a>Ortam değişkenlerini doğrulayın

Çalıştırma [az container show] [ az-container-show] komutu, kapsayıcının ortam değişkenlerini sorgulamak için:

```azurecli-interactive
az container show --resource-group myResourceGroup --name securetest --query 'containers[].environmentVariables'
```

JSON yanıtı hem güvenli bir ortam değişkeni anahtar hem de değeri gösterilmektedir ancak yalnızca güvenli bir ortam değişkeninin adı:

```json
[
  [
    {
      "name": "NOTSECRET",
      "secureValue": null,
      "value": "my-exposed-value"
    },
    {
      "name": "SECRET",
      "secureValue": null,
      "value": null
    }
  ]
]
```

İle [az container exec] [ az-container-exec] komutu, çalışan bir kapsayıcıda bir komut yürütülürken sağlayan güvenli bir ortam değişkenini ayarlandığını doğrulayabilirsiniz. Kapsayıcıda bir etkileşimli bir bash oturumu başlatmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az container exec --resource-group myResourceGroup --name securetest --exec-command "/bin/bash"
```

Kapsayıcı içinde etkileşimli bir kabuk açtıktan sonra erişebildiğiniz `SECRET` değişkenin değeri:

```console
root@caas-ef3ee231482549629ac8a40c0d3807fd-3881559887-5374l:/# echo $SECRET
my-secret-value
```

## <a name="next-steps"></a>Sonraki adımlar

Toplu işleme çeşitli kapsayıcıları ile büyük bir veri kümesini gibi görev tabanlı senaryoları özel ortam değişkenleri, çalışma zamanında yararlanabilir. Görev tabanlı kapsayıcı çalıştırma hakkında daha fazla bilgi için bkz. [Azure Container Instances'da kapsayıcılı görevleri çalıştırma](container-instances-restart-policy.md).

<!-- IMAGES -->
[portal-env-vars-01]: ./media/container-instances-environment-variables/portal-env-vars-01.png
[portal-env-vars-02]: ./media/container-instances-environment-variables/portal-env-vars-02.png

<!-- LINKS - External -->
[aci-wordcount]: https://hub.docker.com/r/microsoft/aci-wordcount/

<!-- LINKS Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[azure-cli-install]: /cli/azure/
[azure-instance-log]: /powershell/module/azurerm.containerinstance/get-azurermcontainerinstancelog
[azure-powershell-install]: /powershell/azure/install-azurerm-ps
[new-azurermcontainergroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[portal]: https://portal.azure.com
