---
title: Azure 'da bir Linux VM 'de Bash betiğini çalıştırmak için Cloud-init kullanma
description: Azure CLı ile oluşturma sırasında bir Linux sanal makinesinde Bash betiğini çalıştırmak için Cloud-init kullanma
author: rickstercdn
ms.service: virtual-machines
ms.collection: linux
ms.topic: how-to
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: 1dabd7db85d4222ca2e9f8387b716af291ee6a69
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102559435"
---
# <a name="use-cloud-init-to-run-a-bash-script-in-a-linux-vm-in-azure"></a>Azure 'da bir Linux sanal makinesinde Bash betiğini çalıştırmak için Cloud-init kullanma
Bu makalede, Azure 'da sağlama sırasında Linux sanal makinesi (VM) veya sanal makine ölçek kümeleri (VMSS) üzerinde mevcut bir bash betiğini çalıştırmak için [Cloud-init](https://cloudinit.readthedocs.io) ' i nasıl kullanacağınız gösterilmektedir. Bu Cloud-init betikleri, kaynaklar Azure tarafından sağlandıktan sonra ilk önyüklemede çalışır. Cloud-init 'in Azure 'da ve desteklenen Linux korumalar 'daki yerel olarak nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Cloud-init Overview](using-cloud-init.md)

## <a name="run-a-bash-script-with-cloud-init"></a>Cloud-init ile bash betiği çalıştırma
Cloud-init ile mevcut betiklerinizi bir Cloud-config dosyasına dönüştürmeniz gerekmez, Cloud-init, biri Bash betiğiyle birden çok giriş türünü kabul eder.

Betikleri çalıştırmak için Linux özel betik Azure uzantısını kullanıyorsanız, bunları Cloud-init kullanmak üzere geçirebilirsiniz. Ancak, Azure uzantıları, komut dosyası hatalarıyla uyarı vermek için tümleşik raporlamaya sahiptir, komut dosyası başarısız olursa bir Cloud-init görüntü dağıtımı başarısız olmaz.

Bu işlevi eylemde görmek için, test için basit bir bash betiği oluşturun. Cloud-init dosyası gibi `#cloud-config` , bu betiğin sanal makinenizi sağlamak Için AzureCLI komutlarını çalıştıracağınız yerel olması gerekir.  Bu örnekte, dosyayı yerel makinenizde değil Cloud Shell oluşturun. İstediğiniz düzenleyiciyi kullanabilirsiniz. Dosyayı oluşturmak ve kullanılabilir düzenleyicilerin listesini görmek için `sensible-editor simple_bash.sh` adını girin. **Nano** düzenleyiciyi kullanmak için #1 seçin. Tüm Cloud-init dosyalarının, özellikle de ilk satırda doğru şekilde kopyalandığından emin olun.  

```bash
#!/bin/sh
echo "this has been written via cloud-init" + $(date) >> /tmp/myScript.txt
```

Bu görüntüyü dağıtılmadan önce [az Group Create](/cli/azure/group) komutuyla bir kaynak grubu oluşturmanız gerekir. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Şimdi [az VM Create](/cli/azure/vm) Ile bir VM oluşturun ve Bash betik dosyasını `--custom-data simple_bash.sh` aşağıdaki gibi belirtin:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data simple_bash.sh \
  --generate-ssh-keys 
```
## <a name="verify-bash-script-has-run"></a>Bash betiği 'nin çalıştırıldığını doğrula
Önceki komutun çıktısında gösterilen sanal makinenizin genel IP adresine SSH. Kendi **Publicıpaddress** değerini aşağıdaki şekilde girin:

```bash
ssh <publicIpAddress>
```

**/Tmp** dizinine geçin ve myScript.txt dosyanın var olduğunu ve içinde uygun metne sahip olduğunu doğrulayın.  Değilse, daha fazla ayrıntı için **/var/log/Cloud-Init.log dosyasına** bakabilirsiniz.  Aşağıdaki girişi arayın:

```bash
Running config-scripts-user using lock Running command ['/var/lib/cloud/instance/scripts/part-001']
```

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma değişikliklerine yönelik ek Cloud-init örnekleri için aşağıdakilere bakın:
 
- [VM 'ye ek bir Linux kullanıcısı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede var olan paketleri güncelleştirmek için bir paket yöneticisi çalıştırın](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştir](cloudinit-update-vm-hostname.md) 
- [Uygulama paketi yüklemesi, yapılandırma dosyalarını güncelleştirme ve anahtarları ekleme](tutorial-automate-vm-deployment.md)
