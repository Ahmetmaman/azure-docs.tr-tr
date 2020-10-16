---
title: Azure VM 'lerinde yerel Linux parolasını sıfırlama | Microsoft Docs
description: Azure VM 'de yerel Linux parolasını sıfırlama adımlarını tanıtın
services: virtual-machines-linux
documentationcenter: ''
author: Deland-Han
manager: dcscontentpm
editor: ''
tags: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: troubleshooting
ms.date: 08/20/2019
ms.author: delhan
ms.openlocfilehash: c6bfd5b9ff3626593916533f27c5c2755cebcb13
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87028490"
---
# <a name="how-to-reset-local-linux-password-on-azure-vms"></a>Azure VM’lerinde yerel Linux parolasını sıfırlama

Bu makale, yerel Linux sanal makinesi (VM) parolalarını sıfırlamaya yönelik çeşitli yöntemler sunar. Kullanıcı hesabının Kullanım zamanı dolmuşsa veya yalnızca yeni bir hesap oluşturmak istiyorsanız, yeni bir yerel yönetici hesabı oluşturmak ve VM 'ye yeniden erişim kazanmak için aşağıdaki yöntemleri kullanabilirsiniz.

## <a name="symptoms"></a>Belirtiler

VM 'de oturum açamazsınız ve kullandığınız parolanın yanlış olduğunu belirten bir ileti alırsınız. Ayrıca, Azure portal parolanızı sıfırlamak için VMAgent 'ı kullanamazsınız.

## <a name="manual-password-reset-procedure"></a>El ile parola sıfırlama yordamı

> [!NOTE]
> Aşağıdaki adımlar, sanal makinenin yönetilmeyen diskine uygulanmaz.

1. Etkilenen VM 'nin işletim sistemi diski için bir anlık görüntü alın, anlık görüntüden bir disk oluşturun ve ardından diski bir sorun giderme VM 'sine bağlayın. Daha fazla bilgi için, [Azure Portal kullanarak işletim sistemi diskini bir kurtarma sanal makinesine ekleyerek WINDOWS VM sorunlarını giderme](troubleshoot-recovery-disks-portal-linux.md)bölümüne bakın.

2. Uzak Masaüstü kullanarak sorun giderme sanal makinesine bağlanın.

3.  Bir süper kullanıcı olmak için sorun giderme sanal makinesinde aşağıdaki SSH komutunu çalıştırın.

    ```bash
    sudo su
    ```

4.  Yeni eklenen diski bulmak için **fdisk-l ' y** i çalıştırın veya sistem günlüklerine bakın. Takılacak sürücü adını bulun. Ardından, zamana bağlı VM 'de ilgili günlük dosyasına bakın.

    ```bash
    grep SCSI /var/log/kern.log (ubuntu)
    grep SCSI /var/log/messages (centos, suse, oracle)
    ```

    GREP komutunun örnek çıktısı aşağıda verilmiştir:

    ```bash
    kernel: [ 9707.100572] sd 3:0:0:0: [sdc] Attached SCSI disk
    ```

5.  **Tempmount**adlı bir bağlama noktası oluşturun.

    ```bash
    mkdir /tempmount
    ```

6.  Takma noktasındaki işletim sistemi diskini bağlayın. Genellikle *sdc1* veya *sdc2*bağlamanız gerekir. Bu, bozuk makine diskinden */etc* dizinindeki barındırma bölümüne bağlıdır.

    ```bash
    mount /dev/sdc1 /tempmount
    ```

7.  Herhangi bir değişiklik yapmadan önce çekirdek kimlik bilgileri dosyalarının kopyalarını oluşturun:

    ```bash
    cp /etc/passwd /etc/passwd_orig    
    cp /etc/shadow /etc/shadow_orig    
    cp /tempmount/etc/passwd /etc/passwd
    cp /tempmount/etc/shadow /etc/shadow 
    cp /tempmount/etc/passwd /tempmount/etc/passwd_orig
    cp /tempmount/etc/shadow /tempmount/etc/shadow_orig
    ```

8.  Kullanıcının gereken parolasını sıfırlayın:

    ```bash
    passwd <<USER>> 
    ```

9.  Değiştirilen dosyaları, bozuk makinenin diskindeki doğru konuma taşıyın.

    ```bash
    cp /etc/passwd /tempmount/etc/passwd
    cp /etc/shadow /tempmount/etc/shadow
    cp /etc/passwd_orig /etc/passwd
    cp /etc/shadow_orig /etc/shadow
    ```

10. Köke geri dönün ve diski çıkarın.

    ```bash
    cd /
    umount /tempmount
    ```

11. Azure portal, diski sorun giderme VM 'sinden ayırın.

12. [ETKILENEN VM için işletim sistemi diskini değiştirin](troubleshoot-recovery-disks-portal-linux.md#swap-the-os-disk-for-the-vm).

## <a name="next-steps"></a>Sonraki adımlar

* [Başka bir Azure VM 'ye işletim sistemi diski ekleyerek Azure VM sorunlarını giderme](https://social.technet.microsoft.com/wiki/contents/articles/18710.troubleshoot-azure-vm-by-attaching-os-disk-to-another-azure-vm.aspx)

* [Azure CLı: bir VM 'yi VHD 'den silme ve yeniden dağıtma](/archive/blogs/linuxonazure/azure-cli-how-to-delete-and-re-deploy-a-vm-from-vhd)
