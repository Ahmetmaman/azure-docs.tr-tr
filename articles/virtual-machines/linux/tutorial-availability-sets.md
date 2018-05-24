---
title: Öğretici - Azure’da Linux VM’ler için yüksek oranda kullanılabilirlik | Microsoft Docs
description: Bu öğreticide, Kullanılabilirlik Kümelerinde yüksek oranda kullanılabilir sanal makineler dağıtmak için Azure CLI 2.0’ı kullanmayı öğreneceksiniz
documentationcenter: ''
services: virtual-machines-linux
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: tutorial
ms.date: 10/05/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: dc6fba89571515d0d2d7ed3ecc35c3065405056b
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-create-and-deploy-highly-available-virtual-machines-with-the-azure-cli-20"></a>Öğretici: Azure CLI 2.0 ile yüksek oranda kullanılabilir sanal makineler oluşturma ve dağıtma

Bu öğreticide, Kullanılabilirlik Kümeleri adlı bir özellik kullanarak Azure’da Sanal Makine çözümlerinizin kullanılabilirlik ve güvenilirliğini nasıl artıracağınızı öğreneceksiniz. Kullanılabilirlik kümeleri, Azure’da dağıttığınız VM’lerin birden fazla yalıtılmış donanım kümesi arasında dağıtılmasını sağlar. Böylece, Azure’da bir donanım veya yazılım hatası oluşursa yalnızca sanal makinelerinizin bir alt kümesinin etkilenmesi ve genel çözümünüzün kullanılabilir ve çalışır durumda kalması sağlanır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kullanılabilirlik kümesi oluşturma
> * Kullanılabilirlik kümesinde sanal makine oluşturma
> * Kullanılabilir sanal makine boyutlarını denetleme

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.30 veya sonraki bir sürümünü çalıştırmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="availability-set-overview"></a>Kullanılabilirlik kümesine genel bakış

Kullanılabilirlik Kümesi, içine yerleştirdiğiniz sanal makine kaynaklarının, Azure veri merkezinde dağıtıldığında birbirinden yalıtılmasını sağlamak için Azure’da kullanabileceğiniz bir mantıksal gruplama yeteneğidir. Azure, bir Kullanılabilirlik Kümesi içine yerleştirdiğiniz sanal makinelerin birden fazla fiziksel sunucuda, bilgi işlem rafında, depolama biriminde ve ağ anahtarında çalışmasını sağlar. Bir donanım veya Azure yazılım hatası oluşursa, yalnızca sanal makinelerinizin bir alt kümesi etkilenir ve genel uygulama güncel kalıp müşterilerinizin kullanımına sunulmaya devam eder. Kullanılabilirlik Kümesi, güvenilir bulut çözümleri oluşturmak istediğinizde gerekli olan temel bir yetenektir.

Dört adet ön uç web sunucusuna sahip olabileceğiniz ve bir veritabanını barındıran iki adet arka uç sanal makine kullandığınız tipik bir sanal makine tabanlı çözümü düşünelim. Azure ile, sanal makinelerinizi dağıtmadan önce iki kullanılabilirlik kümesi tanımlamak istersiniz: bir kullanılabilirlik kümesi, "web" katmanı için ve bir kullanılabilirlik kümesi de "veritabanı" katmanı için. Yeni bir sanal makine oluşturduğunuzda, az vm create komutuna parametre olarak kullanılabilirlik kümesini belirtebilirsiniz ve Azure da otomatik olarak kullanılabilirlik kümesi içinde oluşturduğunuz sanal makinelerin birden fazla fiziksel donanım kaynağı arasında yalıtılmasını sağlar. Web Sunucusu veya Veritabanı Sunucusu Sanal Makinelerinizden birinin çalıştığı fiziksel donanımda sorun varsa, Web Sunucunuzun ve Veritabanı Sanal Makinelerinizin diğer örneklerinin farklı donanımlarda bulunması nedeniyle çalışmaya devam edeceğini bilirsiniz.

Azure’da güvenilir sanal makine tabanlı çözümleri dağıtmak istediğinizde Kullanılabilirlik Kümelerini kullanın.


## <a name="create-an-availability-set"></a>Kullanılabilirlik kümesi oluşturma

[az vm availability-set create](/cli/azure/vm/availability-set#az_vm_availability_set_create) komutunu kullanarak bir kullanılabilirlik kümesi oluşturabilirsiniz. Bu örnekte, *myResourceGroupAvailability* kaynak grubundaki *myAvailabilitySet* adlı kullanılabilirlik kümesi için güncelleştirme ve hata etki alanları sayısı *2* olarak ayarlanmıştır.

İlk olarak, [az group create](/cli/azure/group#az-group-create) ile bir kaynak grubu oluşturun ve sonra kullanılabilirlik kümesini oluşturun:

```azurecli-interactive
az group create --name myResourceGroupAvailability --location eastus

az vm availability-set create \
    --resource-group myResourceGroupAvailability \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

Kullanılabilirlik Kümeleri, hata etki alanları ve güncelleştirme etki alanları arasında kaynakları yalıtmanıza olanak sağlar. **Hata etki alanı**, yalıtılmış bir sunucu + ağ + depolama kaynakları koleksiyonunu temsil eder. Önceki örnekte, sanal makineleri dağıtıldığında kullanılabilirlik kümesi en az iki hata etki alanı arasında dağıtılmıştır. Kullanılabilirlik kümesi de iki **güncelleme etki alanına** dağıtılmıştır. İki güncelleştirme etki alanı, Azure yazılım güncelleştirmeleri gerçekleştirdiğinde sanal makine kaynaklarının yalıtılmasını sağlayarak sanal makinede çalışan tüm yazılımların aynı anda güncelleştirilmesini önler.


## <a name="create-vms-inside-an-availability-set"></a>Kullanılabilirlik kümesi içinde sanal makineler oluşturma

Donanım arasında doğru şekilde dağıtıldığından emin olmak için sanal makinelerin kullanılabilirlik kümesi içinde oluşturulması gerekir. Mevcut VM, oluşturulduktan sonra kullanılabilirlik kümesine eklenemez.

[az vm create](/cli/azure/vm#az_vm_create) ile yeni bir VM oluşturulduğunda, kullanılabilirlik kümesinin adını belirtmek için `--availability-set` parametresini kullanın.

```azurecli-interactive
for i in `seq 1 2`; do
   az vm create \
     --resource-group myResourceGroupAvailability \
     --name myVM$i \
     --availability-set myAvailabilitySet \
     --size Standard_DS1_v2  \
     --image UbuntuLTS \
     --admin-username azureuser \
     --generate-ssh-keys \
     --no-wait
done
```

Artık kullanılabilirlik kümesi içinde iki sanal makine vardır. Aynı kullanılabilirlik kümesinde olduklarından Azure, sanal makinelerin ve tüm kaynaklarının (veri diskleri dahil), yalıtılmış fiziksel donanım arasında dağıtılmasını sağlar. Bu dağıtım, genel sanal makine çözümünün çok daha yüksek düzeyde kullanılabilir olmasını sağlar.

Kullanılabilirlik kümesi dağıtımı, Kaynak Grupları > myResourceGroupAvailability > myAvailabilitySet öğesine gidilerek portaldan görüntülenebilir. VM’ler aşağıdaki örnekte gösterildiği gibi iki hata ve güncelleme etki alanında dağıtılmıştır:

![Portaldaki kullanılabilirlik kümesi](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Kullanılabilir sanal makine boyutlarını denetleme

Ek VM’ler sonradan VM boyutlarının donanımda bulunduğu kullanılabilirlik kümesine eklenebilir. Kullanılabilirlik kümesi için donanım kümesindeki tüm kullanılabilir boyutları listelemek için [az vm availability-set list-sizes](/cli/azure/availability-set#az_availability_set_list_sizes) komutunu kullanın:

```azurecli-interactive
az vm availability-set list-sizes \
     --resource-group myResourceGroupAvailability \
     --name myAvailabilitySet \
     --output table
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Kullanılabilirlik kümesi oluşturma
> * Kullanılabilirlik kümesinde sanal makine oluşturma
> * Kullanılabilir sanal makine boyutlarını denetleme

Sanal makine ölçek kümeleri hakkında daha fazla bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Sanal makine ölçek kümesi oluşturma](tutorial-create-vmss.md)