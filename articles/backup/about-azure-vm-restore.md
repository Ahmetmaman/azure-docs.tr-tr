---
title: Azure sanal makine geri yükleme işlemi hakkında
description: Azure Backup hizmetinin Azure sanal makinelerini nasıl geri yüklediği hakkında bilgi edinin
ms.topic: conceptual
ms.date: 05/20/2020
ms.openlocfilehash: 62d1ff7973693f29c77c77fe2ad4fbbb598a5fa4
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101738421"
---
# <a name="about-azure-vm-restore"></a>Azure VM geri yükleme hakkında

Bu makalede, [Azure Backup hizmetinin](./backup-overview.md) Azure sanal makinelerini (VM) nasıl geri yüklediği açıklanır. Birkaç geri yükleme seçeneği vardır. Destekledikleri çeşitli senaryolar ele alınacaktır.

## <a name="concepts"></a>Kavramlar

- **Kurtarma noktası** ( **geri yükleme noktası** olarak da bilinir): kurtarma noktası Yedeklenen özgün verilerin bir kopyasıdır.

- **Katman (anlık görüntü ve kasa)**: Azure VM yedeklemesi iki aşamada gerçekleşir:

  - 1. aşamada, alınan anlık görüntü diskle birlikte depolanır. Bu, **anlık görüntü katmanı** olarak adlandırılır. Anlık görüntü katmanı geri yüklemeleri daha hızlıdır (kasadan geri yükleme) ve geri yüklemeyi tetiklemeden önce anlık görüntülerin kasaya kopyalaması için bekleme süresini ortadan kaldırırlar. Bu nedenle anlık görüntü katmanından geri yükleme de [anında geri yükleme](./backup-instant-restore-capability.md)olarak adlandırılır.
  - 2. aşamada, anlık görüntü aktarılır ve Azure Backup hizmeti tarafından yönetilen kasada depolanır. Bu, **kasa katmanı** olarak adlandırılır.

- **Özgün konum kurtarma (OLR)**: geri yükleme noktasından, yedeklemelerin alındığı kaynak Azure VM 'sine yapılan kurtarma, kurtarma noktasında depolanan durum ile değiştiriliyor. Bu, işletim sistemi diskinin ve kaynak VM 'nin veri disklerindeki (ler) yerini alır.

- **Alternatif Konum Kurtarma (ALR)**: kurtarma noktasından, yedeklemelerin alındığı orijinal sunucu dışında bir sunucuya yapılan bir kurtarma.

- **Öğe düzeyinde geri yükleme (ıLR):** VM 'nin içindeki tek tek dosyaları veya klasörleri kurtarma noktasından geri yükleme

- **Kullanılabilirlik (çoğaltma türleri)**: Azure Backup depolama/verilerinizi yüksek oranda kullanılabilir tutmak için iki tür çoğaltma sunar:
  - [Yerel olarak yedekli depolama (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) verilerinizi üç kez çoğaltır (verilerinizin üç kopyasını oluşturur) bir veri merkezinde bulunan bir depolama ölçek birimi. Verilerin tüm kopyaları aynı bölgenin içinde yer alır. LRS, verilerinizi yerel donanım hatalarına karşı korumak için düşük maliyetli bir seçenektir.
  - [Coğrafi olarak yedekli depolama (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage) , varsayılan ve önerilen çoğaltma seçeneğidir. GRS, verilerinizi ikincil bir bölgeye (kaynak verilerin birincil konumundan yüzlerce kilometre uzakta) kopyalar. GRS 'nin maliyeti LRS 'den fazladır, ancak bölgesel bir kesinti olsa bile, verileriniz için GRS daha yüksek düzeyde dayanıklılık sağlar.
  - Bölgesel olarak [yedekli depolama (ZRS)](../storage/common/storage-redundancy.md#zone-redundant-storage) , verilerinizi [kullanılabilirlik bölgelerinde](../availability-zones/az-overview.md#availability-zones)çoğaltır, bu da aynı bölgedeki veri fazlalığını ve dayanıklılığı garanti ediyor. ZRS 'nin kapalı kalma süresi yoktur. Bu nedenle, [veri](https://azure.microsoft.com/resources/achieving-compliant-data-residency-and-security-with-azure/)ve kapalı kalma süresi olmaması gereken kritik iş yükleriniz, ZRS 'de yedeklenebilir.

- **Çapraz bölge geri yükleme (CRR)**: çapraz bölge geri yükleme (CRR [), Azure](./backup-azure-arm-restore-vms.md#restore-options)sanal makinelerini Azure eşlenmiş bir bölge olan ikincil bir bölgede geri yüklemenize olanak tanır. Bu, [Azure eşlenmiş](../best-practices-availability-paired-regions.md#what-are-paired-regions) bir bölgedir. verilerinizi ikincil bölgede dilediğiniz zaman, kısmi veya tam kesintiler sırasında veya seçtiğiniz diğer herhangi bir zamanda geri yükleyebilirsiniz. 

## <a name="restore-scenarios"></a>Geri yükleme senaryoları

![Geri yükleme senaryoları ](./media/about-azure-vm-restore/recovery-scenarios.png)

| **Senaryo**                                                 | **Ne yapılır?**                                             | **Kullanılması gereken durumlar**                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [Yeni bir sanal makine oluşturmak için geri yükleme](./backup-azure-arm-restore-vms.md) | Tüm VM 'yi OLR 'ye geri yükler (kaynak VM hala varsa) veya ALR | <li> Kaynak VM kaybolursa veya bozuksa, tüm VM 'yi geri yükleyebilirsiniz  <li> VM 'nin bir kopyasını oluşturabilirsiniz  <li> Denetim veya uyumluluk için geri yükleme detayına gidebilirsiniz  <li> Bu seçenek Market görüntülerinden oluşturulan Azure VM 'Leri (yani, lisansın süre dolmadığı için kullanılabilir değilse) için çalışmaz. |
| [VM 'nin disklerini geri yükleme](./backup-azure-arm-restore-vms.md#restore-disks) | Sanal makineye bağlı diskleri geri yükleme                             |  Tüm diskler: Bu seçenek, şablonu oluşturur ve diski geri yükler. Gereksinimlerinizi karşılamak için bu şablonu özel yapılandırmalarda (örneğin, kullanılabilirlik kümeleri) düzenleyebilir ve ardından hem şablonu hem de VM 'yi yeniden oluşturmak için diski geri yükleyebilirsiniz. |
| [Sanal makine içindeki belirli dosyaları geri yükleme](./backup-azure-restore-files-from-vm.md) | Geri yükleme noktası ' nı seçin, dosyalar ' ı seçin ve yedeklenen VM ile aynı (veya uyumlu) işletim sistemine geri yükleyin. |  Geri yüklenecek belirli dosyaları biliyorsanız, tüm VM 'yi geri yüklemek yerine bu seçeneği kullanın. |
| [Şifrelenmiş bir VM 'yi geri yükleme](./backup-azure-vms-encryption.md) | Portaldan, diskleri geri yükleyin ve ardından VM 'yi oluşturmak için PowerShell 'i kullanın | <li> [Azure Active Directory ile şifrelenmiş VM](../virtual-machines/windows/disk-encryption-windows-aad.md)  <li> [Azure AD olmadan şifrelenmiş VM](../virtual-machines/windows/disk-encryption-windows.md) <li> [Azure AD *ile* *Azure AD 'ye geçirilmiş* şifreli VM](../virtual-machines/windows/disk-encryption-faq.md#can-i-migrate-vms-that-were-encrypted-with-an-azure-ad-app-to-encryption-without-an-azure-ad-app) |
| [Çapraz bölge geri yükleme](./backup-azure-arm-restore-vms.md#cross-region-restore) | Yeni bir VM oluşturma veya diskleri ikincil bir bölgeye geri yükleme (Azure eşleştirilmiş bölge) | <li> **Tam kesinti**: çapraz bölge geri yükleme özelliğiyle, İkincil bölgedeki verileri kurtarmak için bir bekleme süresi yoktur. Azure 'un bir kesinti bildirmemesine karşın, ikincil bölgede geri yükleme işlemini başlatabilirsiniz. <li> **Kısmi kesinti**: Azure Backup, yedeklenen verilerinizi ve hatta ağ içinde, yedeklenen verilerle ilişkili Azure Backup ve depolama kümelerini depolayan belirli depolama kümelerinde kapalı kalma süresi oluşabilir. Çapraz bölge geri yükleme ile ikincil bölgede yedeklenen verilerin bir çoğaltmasını kullanarak ikincil bölgede geri yükleme gerçekleştirebilirsiniz. <li> **Kesinti yok**: ikincil bölge verileriyle denetim veya uyumluluk amaçlarıyla iş sürekliliği ve olağanüstü durum kurtarma (BCDR) detaylarını gerçekleştirebilirsiniz. Bu, iş sürekliliği ve olağanüstü durum kurtarma detaylarının birincil bölgesinde tam veya kısmi bir kesinti olmasa bile, yedeklenen verileri ikincil bölgede geri yüklemeyi gerçekleştirmenize olanak tanır.  |

## <a name="next-steps"></a>Sonraki adımlar

- [VM geri yükleme hakkında sık sorulan sorular](/azure/backup/backup-azure-vm-backup-faq#restore)
- [Desteklenen geri yükleme yöntemleri](./backup-support-matrix-iaas.md#supported-restore-methods)
- [Geri yükleme sorunlarını giderme](./backup-azure-vms-troubleshoot.md#restore)
