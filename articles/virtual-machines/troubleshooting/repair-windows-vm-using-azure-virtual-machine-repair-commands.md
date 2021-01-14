---
title: Azure sanal makine onarım komutlarını kullanarak bir Windows VM 'yi onarma | Microsoft Docs
description: Bu makalede, tüm hataları gidermek için Azure sanal makine onarım komutlarının nasıl kullanılacağı ve sonra da ilk VM 'nizi yeniden oluşturmanın ayrıntıları yer aldığı açıklanır.
services: virtual-machines-windows
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: virtual-machines
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.date: 09/10/2019
ms.author: v-miegge
ms.openlocfilehash: 7763c1d856a09acf2523a7073c3d300be6b70b37
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98200711"
---
# <a name="repair-a-windows-vm-by-using-the-azure-virtual-machine-repair-commands"></a>Azure Sanal Makine onarım komutlarını kullanarak Windows VM'sini onarma

Azure 'daki Windows sanal makineniz (VM) bir önyükleme veya disk hatasıyla karşılaşırsa, diskin kendisi üzerinde risk azaltma yapmanız gerekebilir. Ortak bir örnek, VM 'nin başarıyla önyükleme yapabilmesini önleyen başarısız bir uygulama güncelleştirmesidir. Bu makalede, tüm hataları gidermek için Azure sanal makine onarım komutlarının nasıl kullanılacağı ve sonra da ilk VM 'nizi yeniden oluşturmanın ayrıntıları yer aldığı açıklanır.

> [!IMPORTANT]
> * Bu makaledeki betikler yalnızca [Azure Resource Manager](../../azure-resource-manager/management/overview.md)kullanan VM 'ler için geçerlidir.
> * Betiğin çalışması için VM 'den giden bağlantı (bağlantı noktası 443) gereklidir.
> * Tek seferde yalnızca bir betik çalıştırılabilir.
> * Çalışan bir betik iptal edilemez.
> * Bir betiğin çalışacağı en uzun süre 90 dakikadır ve bu süre sonra zaman aşımına uğrar.
> * Azure disk şifrelemesi kullanan VM 'Ler için yalnızca tek bir geçiş şifrelemesiyle şifrelenen yönetilen diskler (KEK ile veya olmadan) desteklenir.


## <a name="repair-process-overview"></a>İşleme genel bakış

Artık bir VM 'nin işletim sistemi diskini değiştirmek için Azure VM onarım komutlarını kullanabilir ve VM 'yi silip yeniden oluşturmanız gerekmez.

VM sorununun giderilmesi için aşağıdaki adımları izleyin:

1. Azure Cloud Shell’i başlatma
2. Az Extension Add/Update çalıştırın.
3. Çalıştır az VM Repair Create.
4. Az VM repamayı Çalıştır ' ı çalıştırın veya risk azaltma adımlarını gerçekleştirin.
5. Çalıştır az VM Repair restore.

Ek belgeler ve yönergeler için bkz. [az VM Repair](/cli/azure/ext/vm-repair/vm/repair).

## <a name="repair-process-example"></a>İşlemi Onar örneği

1. Azure Cloud Shell’i başlatma

   Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Bu, önceden yüklenmiş ve hesabınızla kullanılmak üzere yapılandırılmış ortak Azure araçları içerir.

   Cloud Shell açmak için, bir kod bloğunun sağ üst köşesinden **dene** ' yi seçin. Ayrıca, Cloud Shell ziyaret ederek ayrı bir tarayıcı sekmesinde açabilirsiniz [https://shell.azure.com](https://shell.azure.com) .

   Kod bloklarını kopyalamak için **Kopyala** ' yı seçin, sonra kodu Cloud Shell yapıştırın ve çalıştırmak için **ENTER** ' u seçin.

   CLI'yi yerel olarak yükleyip kullanmayı tercih ediyorsanız bu hızlı başlangıç için Azure CLI 2.0.30 veya sonraki bir sürümü gerekir. Sürümü bulmak için ``az --version`` komutunu çalıştırın. Azure CLı 'nizi yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure CLI 'Yı yüklemek](/cli/azure/install-azure-cli).
   
   Şu anda Azure portal oturum açtığınızdan farklı bir hesapla Cloud Shell oturum açmanız gerekiyorsa ``az login`` [az Login Reference](/cli/azure/reference-index#az-login&preserve-view=true)kullanabilirsiniz.  Hesabınızla ilişkili abonelikler arasında geçiş yapmak için ``az account set --subscription`` [az Account set Reference](/cli/azure/account#az-account-set&preserve-view=true)kullanabilirsiniz.

2. Komutları ilk kez kullandıysanız `az vm repair` VM-REPAIR CLI uzantısını ekleyin.

   ```azurecli-interactive
   az extension add -n vm-repair
   ```

   Komutları daha önce kullandıysanız `az vm repair` , tüm güncelleştirmeleri VM-onarım uzantısına uygulayın.

   ```azurecli-interactive
   az extension update -n vm-repair
   ```

3. `az vm repair create` öğesini çalıştırın. Bu komut, işlevsel olmayan VM için işletim sistemi diskinin bir kopyasını oluşturacak, yeni bir kaynak grubunda bir onarım VM 'si oluşturacak ve işletim sistemi diski kopyasını ekleyecek.  Onarım sanal makinesi, belirtilen işlev olmayan VM ile aynı boyutta ve bölgeye sahip olacaktır. Tüm adımlarda kullanılan kaynak grubu ve VM adı, işlevsel olmayan VM için olacaktır. VM 'niz Azure disk şifrelemesi kullanıyorsa, komut, onarım VM 'sine eklendiğinde erişilebilir olması için şifrelenmiş diskin kilidini açmayı dener.

   ```azurecli-interactive
   az vm repair create -g MyResourceGroup -n myVM --repair-username username --repair-password 'password!234' --verbose
   ```

4. `az vm repair run` öğesini çalıştırın. Bu komut, belirtilen onarım betiğini, sanal makine onarım aracılığıyla bağlı diskte çalıştırır. Kullanmakta olduğunuz sorun giderme kılavuzu bir çalıştırma kimliği belirtse, lütfen burada kullanın, aksi takdirde `az vm repair list-scripts` kullanılabilir onarım betiklerini görmek için kullanabilirsiniz. Burada kullanılan kaynak grubu ve VM adı, adım 3 ' te kullanılan işlevsel olmayan VM içindir. Onarım betikleri hakkında ek bilgiler [onarım betiği kitaplığı](https://github.com/Azure/repair-script-library)'nda bulunabilir.

   ```azurecli-interactive
   az vm repair run -g MyResourceGroup -n MyVM --run-on-repair --run-id win-hello-world --verbose
   ```
   
   İsteğe bağlı olarak, onarım sanal makinesini kullanarak gereken el ile azaltma adımlarını gerçekleştirebilir, ardından 5. adıma geçebilirsiniz.

5. `az vm repair restore` öğesini çalıştırın. Bu komut, onarılan işletim sistemi diskini VM 'nin orijinal işletim sistemi diskiyle takas eder. Burada kullanılan kaynak grubu ve VM adı, adım 3 ' te kullanılan işlevsel olmayan VM içindir.

   ```azurecli-interactive
   az vm repair restore -g MyResourceGroup -n MyVM --verbose
   ```

## <a name="verify-and-enable-boot-diagnostics"></a>Önyükleme tanılamayı doğrulama ve etkinleştirme

Aşağıdaki örnek, adlı kaynak grubunda adlı sanal makinede tanılama uzantısını sunar ``myVMDeployed`` ``myResourceGroup`` :

Azure CLI

```azurecli-interactive
az vm boot-diagnostics enable --name myVMDeployed --resource-group myResourceGroup --storage https://mystor.blob.core.windows.net/
```

## <a name="next-steps"></a>Sonraki adımlar

* Sanal makinenize bağlanırken sorun yaşıyorsanız bkz. [Azure VM 'ye YÖNELIK RDP bağlantılarında sorun giderme](./troubleshoot-rdp-connection.md).
* VM 'niz üzerinde çalışan uygulamalara erişme sorunları için bkz. [Azure 'da sanal makinelerde uygulama bağlantı sorunlarını giderme](./troubleshoot-app-connection.md).
* Kaynak Yöneticisi kullanma hakkında daha fazla bilgi için bkz. [Azure Resource Manager genel bakış](../../azure-resource-manager/management/overview.md).
