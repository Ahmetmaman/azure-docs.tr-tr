---
title: Azure Site Recovery ile Azure bölgeleri arasında Azure Iaas sanal makinelerinin olağanüstü durum kurtarma için Azure Site Recovery destek matrisi | Microsoft Docs
description: Desteklenen işletim sistemleri ve yapılandırmalar Azure sanal makineleri (VM'ler), Azure Site Recovery çoğaltması için bir bölgeden diğerine için olağanüstü durum kurtarma (DR) gereksinimlerini özetler.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 01/21/2019
ms.author: raynew
ms.openlocfilehash: 3b41f975b484083dab79f16984e84018b2e830a1
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55497307"
---
# <a name="support-matrix-for-replicating-from-one-azure-region-to-another"></a>Bir Azure bölgesinden diğerine çoğaltma için destek matrisi

Bu makalede, çoğaltma, yük devretme ve kurtarma Azure vm'leri bir Azure bölgesinden diğerine ile olağanüstü durum kurtarma kullanarak dağıttığınızda desteklenen yapılandırmalar ve bileşenleri özetlenir [Azure Site Recovery](site-recovery-overview.md) hizmeti.


## <a name="deployment-method-support"></a>Dağıtım yöntemi desteği

**Dağıtım yöntemi** |  **Desteklenen / desteklenmeyen**
--- | ---
**Azure portal** | Desteklenen
**PowerShell** | [PowerShell ile azure'dan Azure'a çoğaltma](azure-to-azure-powershell.md)
**REST API** | Desteklenen
**CLI** | Şu anda desteklenmiyor


## <a name="resource-support"></a>Kaynak desteği

**Kaynak eylem** | **Ayrıntılar**
--- | --- | ---
**Kasa kaynak grupları arasında taşıma** | Desteklenmiyor
**İşlem/depolama/ağ kaynakları kaynak grupları arasında taşıma** | Desteklenmiyor.<br/><br/> Bir VM veya depolama/ağ gibi ilişkili bileşenleri taşırsanız, sanal Makinenin çoğaltma sonra devre dışı bırakın ve ardından sanal makine için çoğaltmayı yeniden etkinleştirmeniz gerekir.
**İçin olağanüstü durum kurtarma için başka bir aboneliği Azure Vm'lerini çoğaltma** | Aynı Azure Active Directory kiracısı içinde desteklenir.
**Bölge içinde desteklenen coğrafi kümeleri (içinde ve abonelikler arasında) arasında sanal makineleri geçirme** | Aynı Azure Active Directory kiracısı içinde desteklenir.
**Aynı bölge içinde sanal makineleri geçirme** | Desteklenmiyor.

## <a name="region-support"></a>Bölge desteği

Çoğaltma ve aynı coğrafi kümedeki herhangi iki bölgeleri arasında Vm'lere'ı kurtarın. Coğrafi kümeleri, veri gecikme süresi ve özerkliği göz önünde bulundurarak tanımlanır.


**Coğrafi küme** | **Azure bölgeleri**
-- | --
Amerika | Kanada Doğu, Kanada Orta, Güney Orta ABD, Batı Orta ABD, Doğu ABD, Doğu ABD 2, Batı ABD, Batı ABD 2, Orta ABD, Kuzey Orta ABD
Avrupa | UK Batı, UK Güney, Kuzey Avrupa, Batı Avrupa, Fransa Orta, Fransa Güney
Asya | Güney Hindistan, Orta Hindistan, Güneydoğu Asya, Doğu Asya, Japonya Doğu, Kore Orta, Kore Güney Batı, Japonya
Avustralya   | Avustralya Doğu, Avustralya Güneydoğu, Avustralya Orta, Avustralya Orta 2
Azure Kamu    | ABD Devleti Virginia, ABD Devleti Iowa, ABD Devleti Arizona, ABD Devleti Texas, US DOD Doğu, ABD DOD Orta
Almanya | Almanya Orta, Almanya Kuzeydoğu
Çin | Çin Doğu, Kuzey Çin, Çin North2, Çin doğu2

>[!NOTE]
>
> - İçin **Brezilya Güney** bölgeye çoğaltmak ve aşağıdakilerden birini yük devretme: Orta Güney ABD, Batı Orta ABD, Doğu ABD, Doğu ABD 2, Batı ABD, Batı ABD 2 ve orta Kuzey ABD bölgeleri. Site Recovery, Brezilya Güney, burada Vm'leri korunabilir gelen bir kaynak bölgesi olarak kullanılmak üzere yalnızca etkinleştirilmiş olduğu unutulmamalıdır. Bunu **bir hedef DR bölgesindeki kullanılamaz** Orta Güney ABD gibi Azure bölgelerinden birini için. Bunun nedeni gecikme süresi, Brezilya Güney dışındaki tüm diğer Amerika'nın bölgeyi seçmek için önerilen nedeniyle coğrafi uzaklıktan gözlemledik.
> 
> - Eğer **bir bölge görmek karşılaştırılamıyor** istediğiniz **bir kasa oluşturmak için** sonra aboneliğiniz, bu bölgede kaynakları oluşturmak için erişimi olduğundan emin olun. Örneğin: Fransa Güney bölgesinde kasası oluşturmak mümkün değilse, aboneliğinizin Fransa Güney bölgesine erişimi yok. Lütfen "diğer genel sorular" konu sorun türü "abonelik yönetimi" altında dosya destek bileti ve sorun türü "XXX beyaz liste aboneliğinde Azure bölgesi"
> 
> - Kullanıyorsanız **bir bölge görmek karşılaştırılamıyor** coğrafi bir küme içindeki **çoğaltmayı etkinleştirme sırasında** sonra aboneliğiniz, bu bölgede sanal makine oluşturmak için erişimi olduğundan emin olun. Örneğin: Sanal makineler Fransa Orta için Fransa Güney korumaya çalışıyorsanız ve görmüyorsanız Fransa Güney bölgesi altındaki aşağı açılır liste sonra aboneliğiniz bu bölgedeki VM dağıtmak için erişime sahip değil. Lütfen "diğer genel sorular" konu sorun türü "abonelik yönetimi" altında dosya destek bileti ve sorun türü "XXX beyaz liste aboneliğinde Azure bölgesi"
> - Yukarıda belirtilen coğrafi kümeleri arasında bölgeleri seçemezsiniz.


## <a name="cache-storage"></a>Önbellek depolama

Çoğaltma sırasında Site Recovery tarafından kullanılan önbellek depolama hesabı için destek bu tabloda özetlenmiştir.

**Ayar** | **Destek** | **Ayrıntılar**
--- | --- | ---
Genel amaçlı V2 depolama hesaplarının (sık erişimli ve seyrek erişimli katman) | Desteklenmiyor. | V2 ilişkin işlem maliyetlerini V1 depolama hesaplarından daha önemli ölçüde daha yüksek olduğu için önbellek depolama için sınırlama bulunmaktadır.
Sanal ağlar için Azure depolama güvenlik duvarları  | Desteklenen | Güvenlik Duvarı etkin önbellek depolama hesabı veya hedef depolama hesabı kullanıyorsanız, emin ['İzin güvenilen Microsoft Hizmetleri'](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).


## <a name="replicated-machine-operating-systems"></a>Çoğaltılan makinelerin işletim sistemleri

Site Recovery, Azure Bu bölümde listelenen işletim sistemlerini çalıştıran sanal makinelerin çoğaltmasını destekler.

### <a name="windows"></a>Windows

**İşletim sistemi** | **Ayrıntılar**
--- | ---
Windows Server 2016  | Sunucu Çekirdeği, masaüstü deneyimi ile sunucu
Windows Server 2012 R2 |
Windows Server 2012 |
Windows Server 2008 R2 | SP1 çalıştıran veya üzeri

#### <a name="linux"></a>Linux

**İşletim sistemi** | **Ayrıntılar**
--- | ---
Red Hat Enterprise Linux | 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6  
CentOS | 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6
Ubuntu 14.04 LTS Server | [Desteklenen bir çekirdek sürümleri](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)
Ubuntu 16.04 LTS Server | [Desteklenen bir çekirdek sürümü](#supported-ubuntu-kernel-versions-for-azure-virtual-machines)<br/><br/> Ubuntu sunucuları bulut Vm'leri yapılandırmak için parola tabanlı kimlik doğrulaması ve oturum açma ve cloud-init'i paket kullanarak parola tabanlı oturum açma (cloudinit yapılandırması) bağlı olarak yük devretme devre dışı bırakılmış olabilir. Parola tabanlı oturum açma olabilir sanal makinede yeniden etkin desteği parolasını sıfırlayarak > sorun giderme > Ayarlar menüsünden (devredilen VM'nin Azure portalında.
Debian 7 | [Desteklenen bir çekirdek sürümleri](#supported-debian-kernel-versions-for-azure-virtual-machines)
Debian 8 | [Desteklenen bir çekirdek sürümleri](#supported-debian-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 12 | SP1, SP2 SP3. [(Desteklenen çekirdek sürümleri)](#supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines)
SUSE Linux Enterprise Server 11 | SP3<br/><br/> Makineler SP4 ' SP3'ü çoğaltmak yükseltme desteklenmiyor. Çoğaltılmış bir makineden yükseltildiyse, çoğaltmayı devre dışı bırakın ve yükseltmeden sonra çoğaltmayı yeniden etkinleştirmeniz gerekir.
SUSE Linux Enterprise Server 11 | SP4
Oracle Linux | 6.4, 6.5, 6.6, 6.7, 6.8, 6.9, 6.10, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5 <br/><br/> Red Hat uyumlu çekirdek veya kesilemeyen Enterprise çekirdeği sürüm 3 (UEK3) çalışıyor.


#### <a name="supported-ubuntu-kernel-versions-for-azure-virtual-machines"></a>Azure sanal makineleri için desteklenen bir Ubuntu çekirdek sürümleri

**Yayın** | **Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
14.04 LTS | 9.22 | 3.13.0-24-Generic 3.13.0-164-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-21-Generic 4.4.0-140-generic için<br/>4.15.0-1023-Azure 4.15.0-1036-azure için |
14.04 LTS | 9.21 | 3.13.0-24-Generic 3.13.0-163-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-21-Generic 4.4.0-140-generic için<br/>4.15.0-1023-Azure 4.15.0-1035-azure için |
14.04 LTS | 9.20 | 3.13.0-24-Generic 3.13.0-161-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-21-Generic 4.4.0-138-generic için<br/>4.15.0-1023-Azure 4.15.0-1030-azure için |
14.04 LTS | 9.19 | 3.13.0-24-Generic 3.13.0-153-generic için<br/>3.16.0-25-Generic 3.16.0-77-generic için<br/>3.19.0-18-Generic 3.19.0-80-generic için<br/>4.2.0-18-Generic 4.2.0-42-generic için<br/>4.4.0-21-Generic 4.4.0-131-generic için |
|||
16.04 LTS | 9.22 | 4.4.0-21-Generic 4.4.0-140-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-14-Generic 4.10.0-42-generic için<br/>4.11.0-13-Generic 4.11.0-14-generic için<br/>4.13.0-16-Generic 4.13.0-45-generic için<br/>4.15.0-13-Generic 4.15.0-43-generic için<br/>4.11.0-1009-Azure 4.11.0-1016-azure için<br/>4.13.0-1005-Azure 4.13.0-1018-azure için <br/>4.15.0-1012-Azure 4.15.0-1036-azure için|
16.04 LTS | 9.21 | 4.4.0-21-Generic 4.4.0-140-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-14-Generic 4.10.0-42-generic için<br/>4.11.0-13-Generic 4.11.0-14-generic için<br/>4.13.0-16-Generic 4.13.0-45-generic için<br/>4.15.0-13-Generic 4.15.0-42-generic için<br/>4.11.0-1009-Azure 4.11.0-1016-azure için<br/>4.13.0-1005-Azure 4.13.0-1018-azure için <br/>4.15.0-1012-Azure 4.15.0-1035-azure için|
16.04 LTS | 9.20 | 4.4.0-21-Generic 4.4.0-138-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-14-Generic 4.10.0-42-generic için<br/>4.11.0-13-Generic 4.11.0-14-generic için<br/>4.13.0-16-Generic 4.13.0-45-generic için<br/>4.15.0-13-Generic 4.15.0-38-generic için<br/>4.11.0-1009-Azure 4.11.0-1016-azure için<br/>4.13.0-1005-Azure 4.13.0-1018-azure için <br/>4.15.0-1012-Azure 4.15.0-1030-azure için|
16.04 LTS | 9.19 | 4.4.0-21-Generic 4.4.0-131-generic için<br/>4.8.0-34-Generic 4.8.0-58-generic için<br/>4.10.0-14-Generic 4.10.0-42-generic için<br/>4.11.0-13-Generic 4.11.0-14-generic için<br/>4.13.0-16-Generic 4.13.0-45-generic için<br/>4.15.0-13-Generic 4.15.0-30-generic için<br/>4.11.0-1009-Azure 4.11.0-1016-azure için<br/>4.13.0-1005-Azure 4.13.0-1018-azure için <br/>4.15.0-1012-Azure 4.15.0-1019-azure için|


#### <a name="supported-debian-kernel-versions-for-azure-virtual-machines"></a>Azure sanal makineleri için desteklenen Debian çekirdek sürümleri

**Yayın** | **Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
Debian 7 | 9.18,9.19,9.20,9.21 | 3.2.0-4-AMD64 3.2.0-6-amd64 için 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | 9.20, 9.21 | 3.16.0-4-AMD64 3.16.0-7-amd64, 4.9.0-0.bpo.4-amd64 4.9.0-0.bpo.8-amd64 için için |
Debian 8 | 9.19 | 3.16.0-4-AMD64 3.16.0-6-amd64, 4.9.0-0.bpo.4-amd64 4.9.0-0.bpo.7-amd64 için için |
Debian 8 | 9.18 | 3.16.0-4-AMD64 3.16.0-6-amd64, 4.9.0-0.bpo.4-amd64 4.9.0-0.bpo.6-amd64 için için |

#### <a name="supported-suse-linux-enterprise-server-12-kernel-versions-for-azure-virtual-machines"></a>Azure sanal makineleri için desteklenen bir SUSE Linux Enterprise Server 12 çekirdek sürümleri

**Yayın** | **Mobility hizmeti sürümü** | **Çekirdek sürümü** |
--- | --- | --- |
SUSE Linux Enterprise Server (SP1, SP2 SP3) 12 | 9.21 | SP1 3.12.49-11-default 3.12.74-60.64.40-default için</br></br> SP1(LTSS) 3.12.74-60.64.45-default 3.12.74-60.64.107-default için</br></br> SP2 4.4.21-69-default 4.4.120-92.70-default için</br></br>SP2(LTSS) 4.4.121-92.73-default 4.4.121-92.98-default için</br></br>SP3 4.4.73-5-default 4.4.162-94.72-default için |
SUSE Linux Enterprise Server (SP1, SP2 SP3) 12 | 9.20 | SP1 3.12.49-11-default 3.12.74-60.64.40-default için</br></br> SP1(LTSS) 3.12.74-60.64.45-default 3.12.74-60.64.107-default için</br></br> SP2 4.4.21-69-default 4.4.120-92.70-default için</br></br>SP2(LTSS) 4.4.121-92.73-default 4.4.121-92.98-default için</br></br>SP3 4.4.73-5-default 4.4.162-94.69-default için |
SUSE Linux Enterprise Server (SP1, SP2 SP3) 12 | 9.19 | SP1 3.12.49-11-default 3.12.74-60.64.40-default için</br></br> SP1(LTSS) 3.12.74-60.64.45-default 3.12.74-60.64.93-default için</br></br> SP2 4.4.21-69-default 4.4.120-92.70-default için</br></br>SP2(LTSS) 4.4.121-92.73-default 4.4.121-92.80-default için</br></br>SP3 4.4.73-5-default 4.4.140-94.42-default için |
SUSE Linux Enterprise Server (SP1, SP2 SP3) 12 | 9.18 | SP1 3.12.49-11-default 3.12.74-60.64.40-default için</br></br> SP1(LTSS) 3.12.74-60.64.45-default 3.12.74-60.64.93-default için</br></br> SP2 4.4.21-69-default 4.4.120-92.70-default için</br></br>SP2(LTSS) 4.4.121-92.73-default 4.4.121-92.80-default için</br></br>SP3 4.4.73-5-default 4.4.138-94.39-default için |


## <a name="replicated-machines---linux-file-systemguest-storage"></a>Çoğaltılan makineler - Linux dosya sistemi/Konuk depolama

* Dosya sistemleri: ext3, ext4, ReiserFS (yalnızca Suse Linux Enterprise Server), XFS BTRFS
* Birim Yöneticisi: LVM2
* Çok yollu yazılım: Cihaz Eşleyici


## <a name="replicated-machines---compute-settings"></a>Çoğaltılan makineler - işlem ayarları

**Ayar** | **Destek** | **Ayrıntılar**
--- | --- | ---
Boyut | Herhangi bir Azure VM boyutu en az 2 CPU Çekirdeği ve 1 GB RAM | Doğrulama [Azure sanal makine boyutları](../virtual-machines/windows/sizes.md).
Kullanılabilirlik kümeleri | Desteklenen | Varsayılan seçeneklerle bir Azure sanal makine için çoğaltmayı etkinleştirmek, bir kullanılabilirlik kümesinde kaynak bölge ayarlarını göre otomatik olarak oluşturulur. Bu ayarları değiştirebilirsiniz.
Kullanılabilirlik alanları | Desteklenen |
Hibrit kullanım teklifi (HUB) | Desteklenen | Kaynak VM etkin bir HUB lisans yük devretme testi veya yük devretme varsa VM, ayrıca HUB lisansı kullanır.
VM ölçek kümeleri | Desteklenmiyor |
Microsoft Azure galeri görüntüleri - yayımlandı | Desteklenen | Sanal Makineyi desteklenen bir işletim sisteminde çalışıyorsa desteklenmiyor.
Azure Galerisi'ndeki görüntüleri - üçüncü taraf yayımlandı | Desteklenen | Sanal Makineyi desteklenen bir işletim sisteminde çalışıyorsa desteklenmiyor.
Özel görüntüleri - üçüncü taraf yayımlandı | Desteklenen | Sanal Makineyi desteklenen bir işletim sisteminde çalışıyorsa desteklenmiyor.
Site RECOVERY'yi kullanarak VM'lerin geçişi | Desteklenen | Bir VMware VM veya fiziksel makine için Azure Site Recovery kullanılarak geçirilmiş, makinede çalışan mobilite hizmetinin eski sürümü kaldırın ve makineyi başka bir Azure bölgesine çoğaltmak önce yeniden gerekebilir.

## <a name="replicated-machines---disk-actions"></a>Çoğaltılan makineler - disk eylemleri

**Eylem** | **Ayrıntılar**
-- | ---
Çoğaltılmış sanal diski yeniden boyutlandırma | Desteklenen
Çoğaltılmış bir VM'ye disk ekleme | Desteklenmiyor.<br/><br/> Sanal makine için çoğaltmayı devre dışı bırakmak için disk ekleyin ve ardından çoğaltmayı yeniden etkinleştirin.

## <a name="replicated-machines---storage"></a>Çoğaltılan makineler - depolama

Azure VM'nin işletim sistemi diski, veri diski ve geçici disk desteği bu tabloda özetlenmiştir.

- Sanal makine disk limitleri ve hedefler için gözleme önemlidir [Linux](../virtual-machines/linux/disk-scalability-targets.md) ve [Windows](../virtual-machines/windows/disk-scalability-targets.md) herhangi bir performans sorunlarından kaçınmak için sanal makineleri.
- Varsayılan ayarlarla dağıtırsanız, Site Recovery diskler ve depolama hesapları kaynak ayarlara göre otomatik olarak oluşturur.
- Özelleştirirseniz, yönergeleri izleyin emin olun.

**Bileşen** | **Destek** | **Ayrıntılar**
--- | --- | ---
İşletim sistemi disk maksimum boyutu | 2048 GB | [Daha fazla bilgi edinin](../virtual-machines/windows/about-disks-and-vhds.md#disks-used-by-vms) VM diskleri hakkında.
Geçici disk | Desteklenmiyor | Geçici disk her zaman çoğaltmadan dışlandı.<br/><br/> Geçici disk üzerinde herhangi bir kalıcı veri yok. [Daha fazla bilgi edinin](../virtual-machines/windows/about-disks-and-vhds.md#temporary-disk).
Veri diski maksimum boyutu | 4095 GB |
Veri diski sayısı | En fazla 64 içinde belge belirli bir Azure VM boyutu için destek | [Daha fazla bilgi edinin](../virtual-machines/windows/sizes.md) VM boyutları hakkında.
Veri disk değişim hızı | Premium depolama için disk başına 10 MB/sn sayısı. Standart depolama için disk başına 2 MB/sn sayısı. | Üzerindeki ortalama veri değişim oranı disk sürekli olarak en yüksek değerden yüksek olduğundan, çoğaltma catch olmaz.<br/><br/>  Ancak, en fazla tutularak aşılıyorsa, çoğaltma yakalayabilir, ancak biraz Gecikmeli kurtarma noktalarını görebilirsiniz.
Veri diski - standart depolama hesabı | Desteklenen |
Veri diski - premium depolama hesabı | Desteklenen | Bir VM, premium ve standart depolama hesapları arasında yayılabilir disk varsa, aynı depolama yapılandırması hedef bölgede olduğundan emin olmak için her disk için farklı bir hedef depolama hesabı seçebilirsiniz.
Yönetilen disk - standart | Azure Site Recovery, desteklenen Azure bölgelerinde desteklenir. |
Yönetilen disk - premium | Azure Site Recovery, desteklenen Azure bölgelerinde desteklenir. |
Standart SSD | Desteklenen |
Yedeklilik | LRS ve GRS desteklenir.<br/><br/> ZRS desteklenmez.
Seyrek erişimli ve sık erişimli depolama | Desteklenmiyor | VM diskleri seyrek erişimli ve sık erişimli depolama alanı desteklenmez.
Depolama alanları | Desteklenen |
(SSE) bekleyen şifreleme | Desteklenen | SSE, depolama hesapları varsayılan ayardır.   
Windows işletim sistemi için Azure Disk şifrelemesi (ADE) | VM'ler için etkin [şifrelemesi ile Azure AD uygulaması](https://aka.ms/ade-aad-app) desteklenir |
Linux işletim sistemi için Azure Disk şifrelemesi (ADE) | Desteklenmiyor |
Sık erişimli Ekle/Kaldır disk | Desteklenmiyor | VM veri diski ekleyip, çoğaltmayı devre dışı bırakın ve yeniden sanal Makineye yönelik çoğaltmayı etkinleştirmek gerekir.
Diski hariç tutma | Desteklenmiyor|   Geçici disk, varsayılan olarak çıkarılır.
Doğrudan Erişimli Depolama Alanları  | Kilitlenme tutarlı kurtarma noktaları için desteklenmiyor. Uygulama tutarlı kurtarma noktalarına desteklenmez. |
Genişleme dosya sunucusu  | Kilitlenme tutarlı kurtarma noktaları için desteklenmiyor. Uygulama tutarlı kurtarma noktalarına desteklenmez. |
LRS | Desteklenen |
GRS | Desteklenen |
RA-GRS | Desteklenen |
ZRS | Desteklenmiyor |
Seyrek erişimli ve sık erişimli depolama | Desteklenmiyor | Seyrek erişimli ve sık erişimli depolama alanı sanal makine diskleri desteklenmez
Sanal ağlar için Azure depolama güvenlik duvarları  | Desteklenen | Depolama hesapları için sanal ağ erişimini kısıtlama, emin olmanız ['İzin güvenilen Microsoft Hizmetleri'](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).
Genel amaçlı V2 depolama hesaplarının (hem sık erişimli ve seyrek erişimli Katmanlar) | Hayır | İşlem maliyetleri artırmak için genel amaçlı V1 depolama hesaplarında önemli ölçüde karşılaştırılır.

>[!IMPORTANT]
> VM disk ölçeklenebilirlik ve performans hedefleri için dikkate aldığınızdan emin olun [Linux](../virtual-machines/linux/disk-scalability-targets.md) veya [Windows](../virtual-machines/windows/disk-scalability-targets.md) herhangi bir performans sorunlarından kaçınmak için sanal makineler. Varsayılan ayarları izlerseniz, Site Recovery gerekli diskler ve depolama hesapları kaynak yapılandırmaya göre oluşturun. Özelleştirme ve kendi ayarlarınızı seçin, kaynak için VM'lerin disk ölçeklenebilirlik ve performans hedefleri izleyin emin olun.

## <a name="azure-site-recovery-limits-to-replicate-data-change-rates"></a>Verileri çoğaltmak için azure Site Recovery limitleri değişim oranları
Aşağıdaki tablo, Azure Site Recovery sınırlarını sağlar. Bu limitler yaptığımız testleri temel alsa da mümkün olan tüm uygulama G/Ç birleşimlerini kapsamamaktadır. Gerçek sonuçlar, uygulamanızın G/Ç karışımına göre değişebilir. Biz de, veri değişim sıklığı ve sanal makinenin veri değişim sıklığı disk başına dikkate alınması gereken iki sınır olmadığını unutmamalısınız.
Örneğin, P20 Premium disk baktığımızda aşağıdaki tabloda, Site Recovery ile en fazla beş tür disk VM başına 25 MB/sn toplam değişim sıklığı, VM sınırı nedeniyle disk başına 5 MB/sn karmaşası başa çıkabilir.

**Çoğaltma depolama hedefi** | **Ortalama kaynak disk G/Ç boyutu** |**Ortalama kaynak disk veri değişim sıklığı** | **Günlük toplam kaynak disk veri değişim sıklığı**
---|---|---|---
Standart depolama | 8 KB | 2 MB/sn | Disk başına 168 GB
Premium P10 veya P15 disk | 8 KB  | 2 MB/sn | Disk başına 168 GB
Premium P10 veya P15 disk | 16 KB | 4 MB/sn |  Disk başına 336 GB
Premium P10 veya P15 disk | 32 KB veya daha büyük | 8 MB/sn | Disk başına 672 GB
Premium P20 veya P30 veya P40 veya P50 disk | 8 KB    | 5 MB/sn | Disk başına 421 GB
Premium P20 veya P30 veya P40 veya P50 disk | 16 KB veya daha büyük |10 MB/sn | Disk başına 842 GB
## <a name="replicated-machines---networking"></a>Çoğaltılan makineler - ağ
**Yapılandırma** | **Destek** | **Ayrıntılar**
--- | --- | ---
NIC | Belirli bir Azure VM boyutu için desteklenen en büyük sayı | VM yük devretme sırasında oluşturulduğunda NIC oluşturulur.<br/><br/> Çoğaltma etkinleştirildiğinde kaynak VM üzerindeki NIC sayısı VM yük devretmesi NIC sayısına bağlıdır. Çoğaltmayı etkinleştirdikten sonra bir NIC ekleyip, yük devretme sonrasında çoğaltılmış sanal makine üzerindeki NIC sayısı etkisi yoktur.
İnternet Yük Dengeleyici | Desteklenen | Bir kurtarma planında bir Azure Otomasyonu betik kullanarak önceden yapılandırılmış bir yük dengeleyici ile ilişkilendirin.
İç Load balancer | Desteklenen | Bir kurtarma planında bir Azure Otomasyonu betik kullanarak önceden yapılandırılmış bir yük dengeleyici ile ilişkilendirin.
Genel IP adresi | Desteklenen | Var olan bir genel IP adresini NIC ile ilişkilendirin Veya, bir genel IP adresi oluşturun ve bir kurtarma planında bir Azure Otomasyonu betik kullanarak NIC ile ilişkilendirin.
NIC'de NSG | Desteklenen | NSG, bir Azure Otomasyonu komut dosyası kullanarak bir kurtarma planında NIC ile ilişkilendirin.
Alt ağda NSG | Desteklenen | NSG, bir kurtarma planında bir Azure Otomasyonu betik kullanarak alt ağı ile ilişkilendirin.
Ayrılmış (statik) IP adresi | Desteklenen | Kaynak VM NIC'i bir statik IP adresi varsa ve hedef alt ağ aynı IP adresi, atanan devredilen VM'nin için.<br/><br/> Hedef alt ağ, aynı IP adresi yoksa, alt ağdaki kullanılabilir IP adreslerinden oluşan bir VM için ayrılmıştır.<br/><br/> Bir sabit IP adresini ve alt ağ olarak da belirtebilirsiniz **çoğaltılan öğeler** > **ayarları** > **işlem ve ağ**  >  **Ağ arabirimleri**.
Dinamik IP adresi | Desteklenen | Kaynak NIC dinamik IP adresi varsa, yük devredilen VM NIC de varsayılan olarak dinamiktir.<br/><br/> Bunu sabit bir IP adresi için gerekirse değiştirebilirsiniz.
Traffic Manager     | Desteklenen | Traffic Manager, trafiğin uç noktasına düzenli olarak kaynak bölgede ve uç noktaya yük devretme durumunda hedef bölgede yönlendirilmesi önceden yapılandırabilirsiniz.
Azure DNS | Desteklenen |
Özel DNS  | Desteklenen |
Kimliği doğrulanmamış Proxy | Desteklenen | Başvurmak [ağ rehberi belgesi.](site-recovery-azure-to-azure-networking-guidance.md)    
Kimliği doğrulanmış Proxy | Desteklenmiyor | VM için giden bağlantı kimliği doğrulanmış bir ara sunucu kullanıyorsa, Azure Site Recovery kullanarak yinelenemez.    
Siteden siteye VPN ile şirket içi (ile veya olmadan ExpressRoute)| Desteklenen | Udr ve Nsg'ler Site kurtarma trafiği şirket içi yönlendirilmemesidir şekilde yapılandırıldığından emin olun. Başvurmak [ağ rehberi belgesi.](site-recovery-azure-to-azure-networking-guidance.md)  
VNET'ten VNET'e bağlantı | Desteklenen | Başvurmak [ağ rehberi belgesi.](site-recovery-azure-to-azure-networking-guidance.md)  
Sanal Ağ Hizmeti Uç Noktaları | Desteklenen | Depolama hesapları için sanal ağ erişimini kısıtlama, güvenilen Microsoft hizmetlerinin depolama hesabına erişim izni verildiğini emin olun.
Hızlandırılmış Ağ | Desteklenen | Kaynak VM üzerinde hızlandırılmış ağ etkin olması gerekir. [Daha fazla bilgi edinin](azure-vm-disaster-recovery-with-accelerated-networking.md).



## <a name="next-steps"></a>Sonraki adımlar
- Okuma [Kılavuzu, Azure Vm'lerini çoğaltma için ağı](site-recovery-azure-to-azure-networking-guidance.md).
- Olağanüstü durum kurtarma dağıtma [Azure Vm'lerini çoğaltma](site-recovery-azure-to-azure.md).
