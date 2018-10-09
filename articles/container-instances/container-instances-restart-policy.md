---
title: Yeniden başlatma ilkeleri ile Azure Container Instances'da kapsayıcılı görevleri çalıştırma
description: Derleme, test veya görüntü işleme işlerini gibi tamamlanmak üzere çalıştırılmasını görevleri yürütmek için Azure Container Instances'ı kullanmayı öğrenin.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 07/26/2018
ms.author: danlep
ms.openlocfilehash: c9e3fadd5164ca0d770f36ba95c30db933efcd39
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48853904"
---
# <a name="run-containerized-tasks-with-restart-policies"></a>Yeniden başlatma ilkeleri ile kapsayıcılı görevleri çalıştırma

Azure Container ınstances'da kapsayıcı dağıtma hızlı ve kolay bir kapsayıcı örneği derleme, test ve görüntü işleme gibi kere çalıştırılacak görevleri yürütmek için ilgi çekici bir platform sağlar.

Yapılandırılabilir yeniden başlatma ilkesi ile işlemlerini tamamladıktan sonra kapsayıcılarınızı durdurulur belirtebilirsiniz. Kapsayıcı örnekleri saniye bazında faturalandırılır olduğundan, yalnızca Görev yürütülürken kapsayıcı çalışırken kullanılan işlem kaynakları için ücretlendirilirsiniz.

Örnek Azure CLI bu makalede kullanımda sunulur. Azure CLI Sürüm 2.0.21 olmalıdır veya büyük [yerel olarak yüklü][azure-cli-install], veya CLI'daki [Azure Cloud Shell](../cloud-shell/overview.md).

## <a name="container-restart-policy"></a>Kapsayıcı yeniden başlatma ilkesi

Azure Container Instances'da bir kapsayıcı oluşturduğunuz zaman, üç yeniden başlatma ilkesi ayarlarından birini belirtebilirsiniz.

| Yeniden başlatma ilkesi   | Açıklama |
| ---------------- | :---------- |
| `Always` | Kapsayıcı grubundaki kapsayıcı her zaman yeniden başlatılır. Bu **varsayılan** container oluşturulması sırasında hiçbir yeniden başlatma ilkesi belirtildiğinde uygulanan ayarı. |
| `Never` | Kapsayıcı grubundaki kapsayıcı hiçbir zaman yeniden başlatılır. Kapsayıcıları en fazla bir kez çalıştırın. |
| `OnFailure` | Kapsayıcı grubundaki kapsayıcı (sıfır olmayan çıkış kodu ile sonlandırıldığında) yalnızca kapsayıcıda yürütülen işlemin başarısız olduğunda başlatılır. Kapsayıcıları en az bir kez çalıştırılır. |

## <a name="specify-a-restart-policy"></a>Yeniden başlatma ilkesi belirtin

Nasıl bir yeniden başlatma İlkesi'ni belirtin, kapsayıcı örneklerinizin gibi Azure PowerShell cmdlet'lerini, Azure CLI veya Azure portalında oluşturma üzerinde bağlıdır. Azure CLI'de belirtin `--restart-policy` çağırdığınızda parametresi [az kapsayıcı oluşturma][az-container-create].

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mycontainerimage \
    --restart-policy OnFailure
```

## <a name="run-to-completion-example"></a>Tamamlama örneği çalıştırma

Yeniden başlatma ilkesi iş başında görmek için bir kapsayıcı örneği oluşturma [Acı/microsoft-wordcount] [ aci-wordcount-image] görüntü ve belirtin `OnFailure` yeniden başlatma ilkesi. Bu örnek kapsayıcı varsayılan olarak, Shakespeare'nın metni inceler, bir Python betiği çalıştıran [Hamlet](http://shakespeare.mit.edu/hamlet/full.html)en yaygın 10 sözcükleri STDOUT yazar ve kapanır.

Örnek kapsayıcı ile aşağıdaki komutu çalıştırın [az kapsayıcı oluşturma] [ az-container-create] komutu:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure
```

Azure Container Instances'a kapsayıcı başladıktan ve kendi uygulama (veya bu durumda betik) çıktığında durdurur. Azure Container Instances, yeniden başlatma ilkesi olan bir kapsayıcı durduğunda `Never` veya `OnFailure`, kapsayıcının durumu kümesine **kesildi**. Bir kapsayıcının durumuyla denetleyebilirsiniz [az container show] [ az-container-show] komutu:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer --query containers[0].instanceView.currentState.state
```

Örnek çıktı:

```bash
"Terminated"
```

Örnek kapsayıcının durumunun sonra *kesildi*, kapsayıcı günlüklerini görüntüleyerek görev çıktısını görebilirsiniz. Çalıştırma [az kapsayıcı günlüklerini] [ az-container-logs] betiğini görüntülemek için komut çıktısı:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

Çıktı:

```bash
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
```

Bu örnek betik STDOUT için gönderilen bir çıktı gösterir. Kapsayıcılı görevlerinizi, sonraki alma için kalıcı depolama için ancak çıktılarını yerine yazabilirsiniz. Örneğin, bir [Azure dosya paylaşımının](container-instances-mounting-azure-files-volume.md).

## <a name="configure-containers-at-runtime"></a>Çalışma zamanında kapsayıcılar'ı yapılandırma

Bir kapsayıcı örneği oluşturduğunuzda ayarlayabilirsiniz kendi **ortam değişkenlerini**, yanı sıra özel belirtin **komut satırı** kapsayıcı başlatıldığında yürütülecek. Her kapsayıcı görev özgü yapılandırma ile hazırlamak için batch işleriniz bu ayarları kullanın.

## <a name="environment-variables"></a>Ortam değişkenleri

Uygulama veya betik çalıştırma kapsayıcı tarafından dinamik olarak yapılandırılmasını sağlamak için kapsayıcınızdaki ortam değişkenlerini ayarlayın. Bu benzer `--env` komut satırı bağımsız değişkeni `docker run`.

Örneğin, bir kapsayıcı örneği oluşturduğunuzda, aşağıdaki ortam değişkenlerini belirterek örnek kapsayıcı betikte davranışını değiştirebilirsiniz:

*NumWords*: STDOUT gönderilen sözcük sayısı.

*MinLength*: en az bir sözcük, sayılması için karakter sayısı. Daha yüksek bir sayı ortak kelimeler gibi "," ve "." yok sayar.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=5 MinLength=8
```

Belirterek `NumWords=5` ve `MinLength=8` kapsayıcının ortam değişkenleri için farklı bir çıkış kapsayıcı günlükleri görüntülemelidir. Sonra kapsayıcı durumu olarak gösterilir *kesildi* (kullanın `az container show` durumunu denetlemek için), yeni çıktıyı görmek için günlükleri görüntüleyin:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer2
```

Çıktı:

```bash
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]
```

## <a name="command-line-override"></a>Komut satırı geçersiz kılma

Kapsayıcı görüntüsüne desteklenmiş komut satırı geçersiz kılmak için bir kapsayıcı örneği oluşturduğunuzda, bir komut satırını belirtin. Bu benzer `--entrypoint` komut satırı bağımsız değişkeni `docker run`.

Metin dışında analizi örnek kapsayıcı örneği için sahip *Hamlet* farklı bir komut satırı belirterek. Kapsayıcı tarafından yürütülen Python betiğini *wordcount.py*, bir URL bağımsız değişken olarak kabul eder ve bu sayfanın içeriğinin yerine varsayılan işlem.

Örneğin, ilk 3 beş harfli sözcükleri belirlemek için *Romeo ve Juliet*:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer3 \
    --image microsoft/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables NumWords=3 MinLength=5 \
    --command-line "python wordcount.py http://shakespeare.mit.edu/romeo_juliet/full.html"
```

Yeniden kapsayıcı başladıktan sonra *kesildi*, kapsayıcının günlüklerini göstererek çıktısını görüntüleyin:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer3
```

Çıktı:

```bash
[('ROMEO', 177), ('JULIET', 134), ('CAPULET', 119)]
```

## <a name="next-steps"></a>Sonraki adımlar

### <a name="persist-task-output"></a>Görev çıktılarını kalıcı hale getirme

Tamamlanmak üzere çalıştırılmasını kapsayıcılarınızı çıktısını kalıcı hale getirmek hakkında ayrıntılı bilgi için bkz. [Azure Container Instances ile Azure dosya paylaşımını bağlama](container-instances-mounting-azure-files-volume.md).

<!-- LINKS - External -->
[aci-wordcount-image]: https://hub.docker.com/r/microsoft/aci-wordcount/

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az-container-create
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az-container-logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az-container-show
[azure-cli-install]: /cli/azure/install-azure-cli
