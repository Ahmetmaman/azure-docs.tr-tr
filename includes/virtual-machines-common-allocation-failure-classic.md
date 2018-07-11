---
title: include dosyası
description: include dosyası
services: virtual-machines-windows, azure-resource-manager
author: genlin
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 04/14/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: f403e060859df6d1de96a3c0d478d57df2677eee
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "31531064"
---
Sabitlenmelidir ayırma isteği neden ayırma yaygın senaryolar aşağıda verilmiştir. Bu makalenin devamındaki her bir senaryo olarak ele alacağız.

- VM'yi yeniden boyutlandırma veya var olan bir bulut hizmetine Vm'lerinizdeki veya rol örneklerinizdeki Ekle
- Kısmen durduruldu (serbest bırakıldı) Vm'leri yeniden başlatma
- Tam olarak durduruldu (serbest bırakıldı) Vm'leri yeniden başlatma
- Hazırlama ve üretim dağıtımları (yalnızca bir hizmet olarak platform)
- Benzeşim grubu (VM veya hizmet yakınlık)
- Sanal ağ benzeşim – grup tabanlı

Ayırma hatası aldığınızda, listelenen senaryolar için hata atanamadığı olup olmadığını denetleyin. Karşılık gelen senaryo tanımlamak için Azure platformu tarafından döndürülen ayırma hatası kullanın. İsteğiniz sabitlenmiş değilse bazı daha kümeleri, böylece başarılı ayırma olasılığını artırma isteğinizi açmak için sabitleme kısıtlamalarını kaldırın.
Genel olarak, hata "istenen VM boyutu desteklenmediğini" durum, daha sonra her zaman yeniden deneyebilir. Bu durum, yeterli kaynakları isteği karşılamak için kümedeki serbest bırakılmış olabilir çünkü. İstenen VM boyutu desteklenmiyor sorun olması durumunda, başka bir VM boyutu deneyin. Aksi takdirde, yalnızca seçeneğin sabitleme kısıtlaması kaldırmaktır.

İki yaygın hata senaryoları için benzeşim grupları ilgilidir. Geçmişte, yakın Vm'leri ve hizmet örnekleri sağlamak için kullanılan bir benzeşim grubu veya bir sanal ağ oluşturmayı etkinleştirmek için kullanılan. Bölgesel sanal ağlara sunulmasıyla birlikte, benzeşim grupları artık bir sanal ağ oluşturmak için gerekli değildir. Azure altyapı ağ gecikme süresi azaltma ile VM veya hizmet yakınlık için benzeşim gruplarını kullanması için Önerideki değişti.

Aşağıdaki diyagramda, sınıflandırma (sabitlenmiş) ayırma senaryolar sunar. 

![Sabitlenmiş ayırma sınıflandırma](./media/virtual-machines-common-allocation-failure/Allocation3.png)

## <a name="resize-a-vm-or-add-vms-or-role-instances-to-an-existing-cloud-service"></a>VM'yi yeniden boyutlandırma veya var olan bir bulut hizmetine Vm'lerinizdeki veya rol örneklerinizdeki Ekle
**Hata**

Upgrade_VMSizeNotSupported veya GeneralError

**Küme sabitleme nedeni**

VM'yi yeniden boyutlandırma veya bir sanal makine veya rol örneği var olan bir bulut hizmetine eklemek için bir istek barındıran mevcut bulut hizmetine özgün kümeyi yeniden denenmesi gerekir. Yeni bir bulut hizmeti oluşturma kaynakları serbest bırakmak veya destekler, istenen VM boyutu olan başka bir kümesini bulmak üzere Azure platformunun sağlar.

**Geçici çözüm**

Hata Upgrade_VMSizeNotSupported * ise başka bir VM boyutu deneyin. Başka bir VM boyutu kullanarak bir seçenek değilse, ancak farklı bir sanal IP adresi (VIP) kullanmak için kabul edilebilir ise, yeni sanal makine konak ve var olan sanal makinelerin çalıştığı bölgesel sanal ağı için yeni bir bulut hizmeti eklemek için yeni bir bulut hizmeti oluşturun. Mevcut bulut hizmetiniz bir bölgesel sanal ağı kullanmıyorsa, yine de yeni bir bulut hizmeti için yeni bir sanal ağ oluşturun ve ardından bağlanmak, [yeni bir sanal ağ mevcut bir sanal ağa](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Hakkında daha fazla bilgi [bölgesel sanal ağlara](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

Hata GeneralError * ise (örneğin, belirli bir VM boyutu) kaynak türü, küme tarafından desteklenir, ancak küme, şu anda ücretsiz kaynak yok olasıdır. Yukarıdaki senaryoya benzer yeni bir bulut hizmeti (farklı bir VIP kullanmak yeni bir bulut hizmeti olduğunu unutmayın) oluşturma aracılığıyla istenen işlem kaynağı ekleyin ve, bulut hizmetlerini bağlamak üzere bölgesel sanal ağ'ı kullanın.

## <a name="restart-partially-stopped-deallocated-vms"></a>Kısmen durduruldu (serbest bırakıldı) Vm'leri yeniden başlatma
**Hata**

GeneralError *

**Küme sabitleme nedeni**

Kısmi ayırmayı kaldırma (serbest bırakıldı) bir veya daha fazlasını ancak değil tüm VM'lerin bir bulut hizmetinde durduruldu anlamına gelir. Ne zaman durdurun (serbest bırakın) bir VM, ilişkili kaynakları serbest bırakılır. VM durduruldu (serbest bırakıldı), yeniden başlatma, bu nedenle yeni bir ayırma isteği gereklidir. Kısmen serbest bulut hizmetinde Vm'leri yeniden başlatma var olan bir bulut hizmeti için Vm'leri ekleme ile eşdeğerdir. Ayırma isteğinin barındıran mevcut bulut hizmetine özgün kümeyi yeniden denenmesi gerekir. Farklı bir bulut hizmeti oluşturmak, ücretsiz bir kaynak veya destekler, istenen VM boyutu olan başka bir kümesini bulmak üzere Azure platformunun sağlar.

**Geçici çözüm**

Farklı bir VIP kullanır, durduruldu (serbest bırakıldı) sanal makinelerini silmek (ancak ilişkili diskler tutmak için) kabul edilebilir ve eklerseniz farklı bir bulut hizmeti ile Vm'leri yedekleyin. Bulut hizmetlerini bağlamak üzere bölgesel sanal ağ kullanın:

* Mevcut bulut hizmetiniz bir bölgesel sanal ağ kullanıyorsa, yeni bir bulut hizmeti aynı sanal ağa eklemeniz yeterlidir.
* Mevcut bulut hizmetiniz bir bölgesel sanal ağı kullanmıyorsa, yeni bir bulut hizmeti için yeni bir sanal ağ oluşturun ve ardından [var olan sanal ağınızda yeni sanal ağına bağlama](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Hakkında daha fazla bilgi [bölgesel sanal ağlara](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

## <a name="restart-fully-stopped-deallocated-vms"></a>Tam olarak durduruldu (serbest bırakıldı) Vm'leri yeniden başlatma
**Hata**

GeneralError *

**Küme sabitleme nedeni**

Durduruldu tam ayırmayı kaldırma anlamına gelir (tüm sanal makineler bir bulut hizmetinden serbest bırakıldı). Bulut hizmetini barındıran konumundaki özgün küme denenmesi bu VM'lerin yeniden ayırma isteklerini sahip. Yeni bir bulut hizmeti oluşturma kaynakları serbest bırakmak veya destekler, istenen VM boyutu olan başka bir kümesini bulmak üzere Azure platformunun sağlar.

**Geçici çözüm**

Farklı bir VIP kullanır, özgün durduruldu (serbest bırakıldı) sanal makinelerini silmek (ancak ilişkili diskler tutmak için) kabul edilebilir ve karşılık gelen bulut hizmetini silme (ilişkili işlem kaynakları, durduruldu (serbest bırakıldı) olduğunda zaten yayımlanmış olan Vm'leri). Vm'leri yeniden eklemek için yeni bir bulut hizmeti oluşturun.

## <a name="stagingproduction-deployments-platform-as-a-service-only"></a>Hazırlama veya üretim dağıtımları (yalnızca bir hizmet olarak platform)
**Hata**

New_General * veya New_VMSizeNotSupported *

**Küme sabitleme nedeni**

Hazırlama dağıtımı ve bir bulut hizmeti üretim dağıtımını aynı kümede barındırılır. İkinci dağıtımı eklediğinizde, karşılık gelen ayırma isteğinin ilk dağıtım barındıran aynı kümede denenecek.

**Geçici çözüm**

İlk dağıtım ve özgün bulut hizmetini silin ve bulut hizmeti yeniden dağıtın. Bu eylem, her iki dağıtımın uyacak şekilde ücretsiz yeterli kaynaklara sahip olan bir kümede mi istediğiniz VM boyutlarını destekler, bir kümedeki ilk dağıtımda kavuşmak.

## <a name="affinity-group-vmservice-proximity"></a>Benzeşim grubu (VM/hizmet yakınlık)
**Hata**

New_General * veya New_VMSizeNotSupported *

**Küme sabitleme nedeni**

Herhangi bir benzeşim grubuna atanan kaynak bir kümeye bağlı işlem. Benzeşim grubu çalıştı mevcut kaynakların barındırıldığı kümede, yeni bilgi işlem kaynağına ister. Yeni kaynakları bir bulut hizmetiniz veya yeni bir bulut hizmeti aracılığıyla oluşturulan bu durum geçerlidir.

**Geçici çözüm**

Bir benzeşim grubu gerekli değilse değil bir benzeşim grubu kullanın veya işlem kaynaklarınızı birden çok benzeşim grupları gruplandırabilirsiniz.

## <a name="affinity-group-based-virtual-network"></a>Grup tabanlı benzeşim sanal ağ
**Hata**

New_General * veya New_VMSizeNotSupported *

**Küme sabitleme nedeni**

Bölgesel sanal ağlara sunulmadan önce bir sanal ağ benzeşim grubu ile ilişkilendirilecek gerekirdi. Sonuç olarak açıklandığı bir benzeşim grubuna yerleştirilen kaynaklar ile aynı kısıtlamalara bağlı işlem "ayırma senaryo: benzeşim grubu (VM/hizmet yakınlık)" Yukarıdaki bölümde. İşlem kaynakları için bir küme bağlıdır.

**Geçici çözüm**

Bir benzeşim grubu gerekmez, eklediğiniz, yeni kaynaklar için yeni bir bölgesel sanal ağ oluşturun ve ardından [var olan sanal ağınızda yeni sanal ağına bağlama](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Hakkında daha fazla bilgi [bölgesel sanal ağlara](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

Alternatif olarak, [benzeşim grubuna bağlı sanal ağınızda bir bölgesel sanal ağa geçirmeniz](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/)ve ardından istenen kaynakları tekrar ekleyin.
