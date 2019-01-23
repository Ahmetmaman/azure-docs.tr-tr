---
title: Linux VM'den - Azure veri diski çıkarma | Microsoft Docs
description: Azure CLI veya Azure portalını kullanarak azure'da bir sanal makineden veri diski çıkarma öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.topic: article
ms.date: 07/18/2018
ms.author: cynthn
ms.component: disks
ms.openlocfilehash: 5a0c2f91c5290e679cc2a3ac03cbf226ceb902da
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54477711"
---
# <a name="how-to-detach-a-data-disk-from-a-linux-virtual-machine"></a>Nasıl bir Linux sanal makinesinden veri diski çıkarma

Sanal makineye bağlı bir veri diskine ihtiyacınız olmadığında bunu kolayca ayırabilirsiniz. Bu diski sanal makineden kaldırır, ancak depolama alanından kaldırmaz. Bu makalede, bir Ubuntu LTS, 16.04 dağıtım çalışıyoruz. Başka bir dağıtım kullanıyorsanız, diski çıkarma yönergelerini farklı olabilir.

> [!WARNING]
> Disk ayırma, otomatik olarak silinmez. Premium depolamaya abone olduğunuz, disk için depolama ücretleri uygulanmaya devam edecektir. Daha fazla bilgi için [fiyatlandırma ve Premium depolama kullanırken faturalama](../windows/premium-storage.md#pricing-and-billing). 
> 
> 

Disk üzerinde var olan verileri yeniden kullanmak isterseniz bu verileri aynı sanal makineye veya başka birine yeniden ekleyebilirsiniz.  


## <a name="connect-to-the-vm-to-unmount-the-disk"></a>Diski çıkarın VM'ye bağlanma

CLI veya portalı kullanarak disk ayırmadan önce disk kaldırılması gerekiyor ve fstab dosyanızı, başvuruları kaldırılır.

VM’ye bağlanın. Bu örnekte, sanal makinenin genel IP adresidir *10.0.1.4* kullanıcı *azureuser*: 

```bash
ssh azureuser@10.0.1.4
```

İlk olarak, ayırmak istediğiniz veri diski bulun. Aşağıdaki örnek dmesg SCSI diskler üzerinde filtrelemek için kullanır:

```bash
dmesg | grep SCSI
```

Çıktı aşağıdaki örneğe benzer:

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

Burada, *sdc* ayırmak için istediğimiz disktir. Ayrıca, diskin UUID'sini alın.

```bash
sudo -i blkid
```

Çıktı aşağıdaki örneğe benzer:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```


Düzen */etc/fstab* disk başvuruları kaldırın. 

> [!NOTE]
> Yanlış düzenleme **/etc/fstab** dosya yapılamamasına bir sistemde neden olabilir. Emin değilseniz, düzgün bir şekilde bu dosya düzenleme hakkında daha fazla bilgi için ait dağıtım belgelerine bakın. Ayrıca düzenlemeden önce /etc/fstab dosyasının yedek bir kopyası oluşturulur önerilir.

Açık */etc/fstab* gibi bir metin düzenleyicisinde dosya:

```bash
sudo vi /etc/fstab
```

Bu örnekte, aşağıdaki satırı öğesinden silinmesi gereken */etc/fstab* dosyası:

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

Kullanım `umount` disk çıkaramadı. Aşağıdaki örnek çıkarır */dev/sdc1* gelen bölüm */datadrive* bağlama noktası:

```bash
sudo umount /dev/sdc1 /datadrive
```


## <a name="detach-a-data-disk-using-azure-cli"></a>Azure CLI kullanarak veri diski çıkarma 

Bu örnekte ayırır *myDataDisk* adlı VM'den diski *myVM* içinde *myResourceGroup*.

```azurecli
az vm disk detach \
    -g myResourceGroup \
    --vm-name myVm \
    -n myDataDisk
```

Disk depolama alanında kalır, ancak artık bir sanal makineye bağlı değildir.


## <a name="detach-a-data-disk-using-the-portal"></a>Portalı kullanarak veri diski çıkarma

1. Sol menüde **sanal makineler**.
2. Önce ayırmak istediğiniz veri diskinin bulunduğu sanal makineyi seçin **Durdur** için VM'yi serbest bırakın.
3. Sanal makine bölmesinde seçin **diskleri**.
4. Üst kısmındaki **diskleri** bölmesinde **Düzenle**.
5. İçinde **diskleri** bölmesinde, en sağdaki ayırmak için istediğiniz veri diskinin ![Ayır düğmesi](./media/detach-disk/detach.png) düğmesi ayırma.
5. Disk kaldırıldıktan sonra Bölmenin üst kısmındaki Kaydet'e tıklayın.
6. Sanal makine bölmesinden **genel bakış** ve ardından **Başlat** VM'yi yeniden başlatmak için bölmenin üstünde düğme.

Disk depolama alanında kalır, ancak artık bir sanal makineye bağlı değildir.



## <a name="next-steps"></a>Sonraki adımlar
Veri diskini yeniden kullanmak istiyorsanız, eklediğiniz [, başka bir VM'ye](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

