---
title: SCP ile Azure Linux VM 'lerine/dosyalarından dosya taşıma
description: SCP 'yi ve SSH anahtar çiftini kullanarak Azure 'da Linux VM 'ye ve bu sanal makineye dosyaları güvenle taşıyın.
author: cynthn
ms.service: virtual-machines
ms.collection: linux
ms.workload: infrastructure
ms.topic: how-to
ms.date: 07/12/2017
ms.author: cynthn
ms.subservice: disks
ms.openlocfilehash: 83b57055ee7a3fedab014abeab96520c3877b843
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2021
ms.locfileid: "102558449"
---
# <a name="move-files-to-and-from-a-linux-vm-using-scp"></a>SCP kullanarak bir Linux VM 'ye veya buradan dosya taşıma

Bu makalede, güvenli kopya (SCP) kullanarak iş istasyonunuzdan bir Azure Linux VM 'sine veya bir Azure Linux VM 'sinden iş istasyonunuza kadar dosya taşıma işlemlerinin nasıl yapılacağı gösterilir. Hızlı ve güvenli bir şekilde iş istasyonunuz ve Linux VM arasında dosya taşımak, Azure altyapınızı yönetmek için önemlidir. 

Bu makalede, [SSH ortak ve özel anahtar dosyaları](mac-create-ssh-keys.md)kullanılarak Azure 'da dağıtılan BIR Linux sanal makinesine ihtiyacınız vardır. Ayrıca yerel bilgisayarınız için bir SCP istemcisine ihtiyacınız vardır. SSH üzerinde oluşturulmuştur ve çoğu Linux ve Mac bilgisayarın ve bazı Windows kabukların varsayılan bash kabuğu 'nda bulunur.

## <a name="quick-commands"></a>Hızlı komutlar

Linux VM 'ye bir dosya kopyalama

```bash
scp file azureuser@azurehost:directory/targetfile
```

Linux VM 'den bir dosyayı aşağı kopyalama

```bash
scp azureuser@azurehost:directory/file targetfile
```

## <a name="detailed-walkthrough"></a>Ayrıntılı kılavuz

Örnek olarak, bir Azure yapılandırma dosyasını bir Linux sanal makinesine taşıdık ve hem SCP hem de SSH anahtarlarını kullanarak bir günlük dosyası dizini çekeceğiz.   

## <a name="ssh-key-pair-authentication"></a>SSH anahtar çifti kimlik doğrulaması

SCP, aktarım katmanı için SSH kullanır. SSH, hedef konaktaki kimlik doğrulamasını işler ve dosyayı SSH ile varsayılan olarak belirtilen şifrelenmiş bir tünelde alır. SSH kimlik doğrulaması için, Kullanıcı adları ve parolalar kullanılabilir. Ancak, SSH ortak ve özel anahtar kimlik doğrulaması, en iyi güvenlik uygulaması olarak önerilir. SSH bağlantıyı doğrulandıktan sonra, SCP dosyayı kopyalamaya başlar. Düzgün şekilde yapılandırılmış `~/.ssh/config` ve SSH ortak ve özel anahtarları kullanarak, yalnızca bir sunucu adı (veya IP adresi) KULLANıLARAK SCP bağlantısı oluşturulabilir. Yalnızca bir SSH anahtarınız varsa, SCP `~/.ssh/` dizinde arar ve sanal makinede oturum açmak için varsayılan olarak kullanır.

`~/.ssh/config`Ve SSH ortak ve özel anahtarlarınızı yapılandırma hakkında daha fazla bilgi için bkz. [SSH anahtarları oluşturma](mac-create-ssh-keys.md).

## <a name="scp-a-file-to-a-linux-vm"></a>SCP 'yi Linux sanal makinesine bir dosya

İlk örnekte, Otomasyonu dağıtmak için kullanılan bir Linux sanal makinesine bir Azure yapılandırma dosyası kopyalayacağız. Bu dosya gizli dizileri içeren Azure API kimlik bilgilerini içerdiğinden güvenlik önemlidir. SSH tarafından sunulan şifreli tünel, dosyanın içeriğini korur.

Aşağıdaki komut yerel *. Azure/config* dosyasını FQDN *myserver.eastus.cloudapp.Azure.com* ile bir Azure VM 'sine kopyalar. Azure VM 'deki Yönetici Kullanıcı adı *azureuser*. Dosya */Home/azureuser/* dizinine yöneliktir. Bu komutta kendi değerlerinizi yerine koyun.

```bash
scp ~/.azure/config azureuser@myserver.eastus.cloudapp.com:/home/azureuser/config
```

## <a name="scp-a-directory-from-a-linux-vm"></a>SCP bir Linux VM 'den bir dizin

Bu örnekte, Linux VM 'sinden bir günlük dosyası dizinini iş istasyonunuza kopyalayacağız. Bir günlük dosyası gizli veya gizli veriler içermeyebilir veya içermeyebilir. Ancak SCP 'nin kullanılması, günlük dosyalarının içeriklerinin şifrelenmesini sağlar. Dosyaları aktarmak için SCP 'nin kullanılması, günlük dizinini ve dosyalarını iş istasyonunuza daha da güvenli hale getirmenin en kolay yoludur.

Aşağıdaki komut, Azure VM 'deki */Home/azureuser/logs/* dizinindeki dosyaları yerel/tmp dizinine kopyalar:

```bash
scp -r azureuser@myserver.eastus.cloudapp.com:/home/azureuser/logs/. /tmp/
```

`-r`Bayrak, SCP 'nin dosyaları ve dizinleri komutta listelenen dizin noktasından yinelemeli olarak kopyalamasını ister.  Ayrıca, komut satırı sözdiziminin bir kopyalama komutuna benzer olduğuna dikkat edin `cp` .

## <a name="next-steps"></a>Sonraki adımlar

* [VMAccess uzantısını kullanarak Azure Linux VM 'lerinde kullanıcıları, SSH 'yi yönetme ve diskleri denetleme veya onarma](../extensions/vmaccess.md?toc=/azure/virtual-machines/linux/toc.json)
