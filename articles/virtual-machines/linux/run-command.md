---
title: Kabuk betikleri, Azure'da bir Linux VM çalıştırma
description: Bu konu başlığı altında komut dosyalarını çalıştır komutunu kullanarak bir Azure Linux sanal makine içinde çalıştırma işlemi açıklanır
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 850c5ac4df8ff3bd0e35567060b3b90dad7baacc
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37342700"
---
# <a name="run-shell-scripts-in-your-linux-vm-with-run-command"></a>Kabuk betikleri Linux VM'nize ile Çalıştır komutunu çalıştırın.

Komutu kullanan bir Azure Linux VM Kabuk betikleri çalıştırmak için VM Aracısı çalıştırın. Bu betikler genel bir makine ya da uygulama yönetimi için kullanılabilir ve hızlı bir şekilde tanılayın ve VM erişimi ve ağ sorunları çözmek ve VM'ye iyi bir duruma geri almak için kullanılabilir.

## <a name="benefits"></a>Avantajlar

Sanal makinelerinizi erişmek için kullanılan birden çok seçenek vardır. Komutunu çalıştırın, sanal makinelerinizde uzaktan VM Aracısı'nı kullanarak komut dosyalarını çalıştırabilirsiniz. Run komutu Azure Portalı aracılığıyla kullanılabilir [REST API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand), [Azure CLI](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke), veya [PowerShell](/powershell/module/azurerm.compute/invoke-azurermvmruncommand).

Bu yetenek, burada bir sanal makineleri içinde bir betik çalıştırmak istediğiniz ve sorun giderme ve RDP sahip olmayan bir sanal makine düzeltmek için yalnızca yollarından biri veya SSH bağlantı noktası açık hatalı ağ veya yönetici kullanıcının nedeniyle tüm senaryolarda kullanışlıdır yapılandırma.

## <a name="restrictions"></a>Kısıtlamalar

Çalıştır komutu kullanırken mevcut olan kısıtlamaları bir listesi verilmiştir.

* Çıkış son 4096 bayt ile sınırlıdır
* Yaklaşık 20 saniye betik çalıştırmak için minimum süre
* Varsayılan olarak, Linux üzerinde yükseltilmiş bir kullanıcı olarak çalıştırılan betikler
* Aynı anda tek bir betik çalıştırabilir
* (Etkileşimli mod) bilgi isteminde betikleri desteklenmez.
* Çalışan bir betik iptal edilemiyor
* Bir betiği çalıştırabilirsiniz en uzun süreyi sonra hangi BT zaman aşımına uğrar 90 dakika olan
* VM'den giden bağlantı betik sonuçlarını döndürmek için gereklidir.

## <a name="azure-cli"></a>Azure CLI

Bir örneği verilmiştir kullanarak [az vm Çalıştır komutunu](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke) bir kabuk betiği bir Azure Linux VM'de çalıştırmak için komutu.

```azurecli-interactive
az vm run-command invoke -g myResourceGroup -n myVm --command-id RunShellScript --scripts "sudo apt-get update && sudo apt-get install -y nginx"
```

> [!NOTE]
> Farklı bir kullanıcı olarak komutları çalıştırmak için kullanabileceğiniz `sudo -u` kullanılacak bir kullanıcı hesabı belirtmek için.

## <a name="azure-portal"></a>Azure portalına

Bir VM'de gidin [Azure](https://portal.azure.com) seçip **komutu çalıştırın** altında **OPERATIONS**. VM'de çalıştırmak için kullanılabilir komutların listesini ile sunulur.

![Komut listesini çalıştırın.](./media/run-command/run-command-list.png)

Bir komutu çalıştırmak için seçin. Bazı komutlar, isteğe bağlı veya gerekli giriş parametreleri olabilir. Bu komutlar için parametreleri giriş değerleri sağlamasına metin alanları olarak sunulur. Her komut genişleterek çalıştırıldığı betik görüntüleyebilirsiniz **komut dosyasını Göster**. **RunShellScript** kendi özel betik sağlamak sağladığından diğer komutlardan farklıdır. 

> [!NOTE]
> Yerleşik komutlar düzenlenebilir değil.

Komut seçilir bitince **çalıştırma** betiği çalıştırmak için. Betik çalışır ve tamamlandığında, çıktı ve hatalar çıkış penceresinde döndürür. Çalışan bir örnek çıktı aşağıdaki ekran görüntüsünde gösterilmektedir **ifconfig** komutu.

![Komut betiği çıktısını çalıştırın](./media/run-command/run-command-script-output.png)

## <a name="available-commands"></a>Kullanılabilir komutlar

Bu tabloda, Linux Vm'leri için kullanılabilir komutların listesini gösterir. **RunShellScript** komutu istediğiniz herhangi bir özel betik çalıştırmak için kullanılabilir.

|**Ad**|**Açıklama**|
|---|---|
|**RunShellScript**|Bir Linux Kabuk betiği çalıştırır.|
|**ifconfig**| Tüm ağ arabirimlerinin yapılandırmasını alın.|

## <a name="limiting-access-to-run-command"></a>Komutu Çalıştır erişimi sınırlandırma

Çalıştırma komutları listesi veya bir komut ayrıntılarını gösteren gerektiren `Microsoft.Compute/locations/runCommands/read` izni, yerleşik [okuyucu](../../role-based-access-control/built-in-roles.md#reader) rol ve daha yüksek.

Bir komut çalıştırmak gerektirir `Microsoft.Compute/virtualMachines/runCommand/action` izni olan [katkıda bulunan](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) rol ve daha yüksek.

Birini kullanabilirsiniz [yerleşik](../../role-based-access-control/built-in-roles.md) rolleri veya oluşturma bir [özel](../../role-based-access-control/custom-roles.md) rol Çalıştır komutunu kullanmaktır.

## <a name="next-steps"></a>Sonraki adımlar

Bkz, [Linux VM'nize betikleri çalıştırma](run-scripts-in-vm.md) , sanal betikleri ve komutları uzaktan çalıştırmak için diğer yollar hakkında bilgi edinmek için.
