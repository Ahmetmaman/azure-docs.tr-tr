---
title: Birden fazla Azure sanal makinesi için güncelleştirmeleri yönetme
description: Bu makalede, Azure sanal makinesi için güncelleştirmeleri yönetme açıklar.
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 08/29/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 231a9876c7a84953a7d9a88b761a1da9475d1f48
ms.sourcegitcommit: 2b2129fa6413230cf35ac18ff386d40d1e8d0677
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43248150"
---
# <a name="manage-updates-for-multiple-machines"></a>Birden çok makine için güncelleştirmeleri yönetme

Güncelleştirme yönetimi çözümü, Windows ve Linux sanal makineleriniz için güncelleştirme ve düzeltme eklerini yönetmek için kullanabilirsiniz. [Azure Otomasyonu](automation-offering-get-started.md) hesabınızdan şunları yapabilirsiniz:

- Sanal makine ekleme
- Kullanılabilir güncelleştirmelerin durumunu değerlendirebilir
- Gerekli güncelleştirmelerin yüklemesini zamanlayabilir
- Güncelleştirme yönetimi etkin olduğu tüm sanal makineleri için güncelleştirmelerin başarıyla uygulandığını doğrulamak için dağıtım sonuçlarını gözden geçirin

## <a name="prerequisites"></a>Önkoşullar

Güncelleştirme yönetimini kullanmak için gerekir:

- Azure Otomasyonu Farklı Çalıştır hesabı. Nasıl oluşturacağınızı öğrenmek için bkz: [Azure Otomasyonu ile çalışmaya başlama](automation-offering-get-started.md).
- Desteklenen işletim sistemlerinden birinin yüklü olduğu bir sanal makine veya bilgisayar.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Güncelleştirme yönetimi aşağıdaki işletim sistemlerinde desteklenir:

|İşletim sistemi  |Notlar  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Destekler, yalnızca değerlendirme güncelleştirin.         |
|Windows Server 2008 R2 SP1 ve üzeri     |Windows PowerShell 4.0 veya üzeri gereklidir. ([WMF 4.0 indirme](https://www.microsoft.com/download/details.aspx?id=40855))</br> Windows PowerShell 5.1, daha fazla güvenilirlik için önerilir. ([İndirme WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616))         |
|CentOS 6 (x86/x64) ve 7 (x64)      | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.        |
|Red Hat Enterprise 6 (x86/x64) ve 7 (x64)     | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.        |
|SUSE Linux Enterprise Server 11 (x86/x64) ve 12 (x64)     | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.        |
|Ubuntu 12.04 LTS, 14.04 LTS ve 16.04 LTS (x86/x64)      |Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.         |

> [!NOTE]
> Güncelleştirmelerin Ubuntu'daki bakım penceresinin dışında uygulanmasının önüne geçmek için Katılımsız Yükseltme paketini otomatik güncelleştirmeler devre dışı bırakılacak şekilden yeniden yapılandırın. Daha fazla bilgi için bkz. [Ubuntu Server Kılavuzu'ndaki Otomatik Güncelleştirmeler konu başlığı](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.

Bu çözüm, birden çok Azure Log Analytics çalışma alanı için rapor için yapılandırılmış Linux için Operations Management Suite (OMS) aracısını desteklemez.

## <a name="enable-update-management-for-azure-virtual-machines"></a>Azure sanal makineler için güncelleştirme yönetimini etkinleştirme

Azure portalında Otomasyon hesabınızı açın ve ardından **güncelleştirme yönetimi**.

Seçin **Azure Vm'leri Ekle**.

![Azure VM Ekle sekmesi](./media/manage-update-multi/update-onboard-vm.png)

Eklemek için bir sanal makine seçin. 

Altında **güncelleştirme yönetimini etkinleştirme**seçin **etkinleştirme** eklemek için sanal makine.

![Güncelleştirme Yönetimini Etkinleştir iletişim kutusu](./media/manage-update-multi/update-enable.png)

Güncelleştirme yönetimi, onboarding tamamlandığında, sanal makineniz için etkinleştirilir.

## <a name="enable-update-management-for-non-azure-virtual-machines-and-computers"></a>Azure olmayan sanal makineler ve bilgisayarlar için güncelleştirme yönetimini etkinleştirme

Azure Windows sanal makineleri ve bilgisayarlar için güncelleştirme yönetimini etkinleştirme hakkında bilgi için bkz: [Azure Log Analytics hizmetine bağlama Windows bilgisayarlara](../log-analytics/log-analytics-windows-agent.md).

Azure olmayan Linux sanal makineleri ve bilgisayarlar için güncelleştirme yönetimini etkinleştirme hakkında bilgi için bkz: [azure'da Linux bilgisayarlarını Log Analytics'e bağlama](../log-analytics/log-analytics-agent-linux.md).

## <a name="view-computers-attached-to-your-automation-account"></a>Otomasyon hesabınıza bağlı bilgisayarları görüntüleme

Makineleriniz için güncelleştirme yönetimini etkinleştirdikten sonra seçerek makine bilgilerini görüntüleyebilirsiniz **bilgisayarlar**. Bilgi bulabilirsiniz *makine adı*, *uyumluluk durumu*, *ortam*, *işletim sistemi türü*, *kritik ve güvenlik güncelleştirmeleri yüklü*, *diğer güncelleştirmelerin yüklü*, ve *Güncelleştirme Aracısı hazırlığı* bilgisayarlarınız için.

  ![Bilgisayarları görüntüle sekmesi](./media/manage-update-multi/update-computers-tab.png)

Güncelleştirme yönetimi için yakın zamanda etkinleştirilmiş bilgisayarlar henüz değerlendirilmemiş değil. Bu bilgisayarların uyumluluk durumu durumu **değerlendirilmeyen**. Uyumluluk durumu için olası değerler listesi aşağıda verilmiştir:

- **Uyumlu**: eksik kritik güncelleştirmeleri veya güvenlik güncelleştirmeleri olan bilgisayarlar.

- **Uyumlu olmayan**: en az bir kritik eksik olan bilgisayarlar veya güvenlik güncelleştirmesi.

- **Değerlendirilmedi**: güncelleştirme Değerlendirme verileri beklenen zaman çerçevesi içinde bilgisayardan alınan edilmemiş. Linux bilgisayarlar için beklenen zaman çerçevesi içinde son 3 saat ' dir. Windows bilgisayarlar, son 12 saat içinde beklenen zaman çerçevesi içindir.

Aracı durumunu görüntülemek için bağlantıyı seçin **güncelleştirme ARACISI hazırlığı** sütun. Bu seçeneğin belirlenmesi açılır **karma çalışanı** bölmesinde ve karma çalışanı durumunu gösterir. Aşağıdaki görüntüde, uzun bir süre için güncelleştirme yönetimini üzere bağlanmamış bir aracı örneği gösterilmektedir:

![Bilgisayarları görüntüle sekmesi](./media/manage-update-multi/update-agent-broken.png)

## <a name="view-an-update-assessment"></a>Güncelleştirme değerlendirmesini görüntüleme

Güncelleştirme Yönetimi etkinleştirildikten sonra **Güncelleştirme yönetimi** bölmesi açılır. **Eksik güncelleştirmeler** sekmesinde eksik güncelleştirmelerin bir listesini görebilirsiniz.

## <a name="collect-data"></a>Veri toplama

Sanal makine ve bilgisayarlarda yüklü aracılar, güncelleştirmelerle ilgili verileri toplar. Aracılar, verileri Azure güncelleştirme yönetimine gönderir.

### <a name="supported-agents"></a>Desteklenen aracılar

Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır:

| Bağlı kaynak | Desteklenen | Açıklama |
| --- | --- | --- |
| Windows aracıları |Evet |Güncelleştirme yönetimi, Windows aracılarından sistem güncelleştirme bilgilerini toplar ve ardından gerekli güncelleştirmelerin yüklemesini başlatır. |
| Linux aracıları |Evet |Güncelleştirme yönetimi, Linux aracılarından sistem güncelleştirme bilgilerini toplar ve ardından desteklenen dağıtımlarda gerekli güncelleştirmelerin yüklemesini başlatır. |
| Operations Manager yönetim grubu |Evet |Güncelleştirme yönetimi, bağlı Yönetim grubundaki aracılardan sistem güncelleştirmeleri hakkında bilgi toplar. |
| Azure Storage hesabı |Hayır |Azure depolama, sistem güncelleştirme bilgilerini içermez. |

### <a name="collection-frequency"></a>Toplama sıklığı

Bir tarama yönetilen her Windows bilgisayarı için günde iki kez çalıştırır. Her 15 dakikada bir Windows API'si çağrılarak son güncelleştirme zamanı durumu değişip değişmediğini belirlemek için sorgulanamıyor. Durum değiştiyse, bir Uyumluluk taraması başlatılır. Yönetilen her Linux bilgisayarı için 3 saatte bir tarama çalıştırır.

30 dakika ve Panoda yönetilen bilgisayarlardan gelen güncelleştirilmiş verilerin görüntülenmesi için 6 saat arasında sürebilir.

## <a name="schedule-an-update-deployment"></a>Güncelleştirme dağıtımı zamanlama

Güncelleştirmeleri yüklemek için yayın zamanlamanızı ve hizmet pencerenizi ile hizalar bir dağıtım zamanlayın. Dağıtıma hangi güncelleştirme türlerinin dahil edileceğini seçebilirsiniz. Örneğin, kritik güncelleştirmeleri veya güvenlik güncelleştirmelerini dahil edip güncelleştirme paketlerini dışlayabilirsiniz.

Altında bir veya daha fazla sanal makineler için yeni bir güncelleştirme dağıtımı zamanlama **güncelleştirme yönetimi**seçin **güncelleştirme dağıtımı zamanla**.

İçinde **yeni güncelleştirme dağıtım** bölmesinde aşağıdaki bilgileri belirtin:

- **Ad**: güncelleştirme dağıtımını tanımlamak için benzersiz bir ad girin.
- **İşletim sistemi**: seçin **Windows** veya **Linux**.
- **Güncelleştirilecek makineler**: kayıtlı arama, içeri aktarılan grubu seçin ya da güncelleştirmek istediğiniz makineleri seçin için makineleri seçin. Seçerseniz **makineler**, makinenin hazır olma gösterilen **güncelleştirme ARACISI hazırlığı** sütun. Güncelleştirme dağıtımı zamanlayabilirsiniz önce bilgisayarın sistem durumunu görebilirsiniz. Log Analytics'te bilgisayar grupları oluşturmak için farklı yöntemler hakkında bilgi edinmek için bkz: [Log analytics'te bilgisayar grupları](../log-analytics/log-analytics-computer-groups.md)

  ![Yeni güncelleştirme dağıtım bölmesi](./media/manage-update-multi/update-select-computers.png)

- **Güncelleştirme sınıflandırması**: güncelleştirme dağıtımına dahil edilecek yazılım türlerini seçin. Sınıflandırma türleri açıklaması için bkz: [güncelleştirme sınıflandırmaları](automation-update-management.md#update-classifications). Sınıflandırma türleri şunlardır:
  - Kritik güncelleştirmeler
  - Güvenlik güncelleştirmeleri
  - Güncelleştirme paketleri
  - Özellik paketleri
  - Hizmet paketleri
  - Tanım güncelleştirmeleri
  - Araçlar
  - Güncelleştirmeler

- **Hariç tutulacak güncelleştirmeler**: Bu seçeneğin belirlenmesi açılır **hariç** sayfası. BB makalelerine veya hariç tutmak için paket adları girin.

- **Zamanlama ayarları** - Geçerli saatten 30 dakika sonrası olan varsayılan tarih ve saati kabul edebilirsiniz. Farklı bir saat de belirtebilirsiniz.

   Ayrıca, dağıtımın bir kez veya yinelenen bir zamanlamaya göre gerçekleşeceğini belirtebilirsiniz. Yinelenen bir zamanlama ayarlamak altında için **yinelenme**seçin **yinelenen**.

   ![Zamanlama Ayarları iletişim kutusu](./media/manage-update-multi/update-set-schedule.png)
- **Bakım penceresi (dakika)**: güncelleştirme dağıtımının gerçekleşmesini istediğiniz süreyi belirtin. Bu ayar, değişikliklerin sizin tanımladığınız hizmet pencereleri içinde gerçekleştirilmesini sağlar.

- **Denetim yeniden** -Bu ayar, yeniden başlatmalar güncelleştirme dağıtımı için nasıl işleneceğini belirler.

   |Seçenek|Açıklama|
   |---|---|
   |Gerekirse yeniden başlatma| **(Varsayılan)**  Bakım penceresi izin veriyorsa yeniden başlatma gerekirse başlatılır.|
   |Her zaman yeniden başlat|Yeniden başlatma, bir gerekip gerekmediğine bakılmaksızın başlatılır. |
   |Hiçbir zaman yeniden başlatma|Bağımsız olarak, yeniden başlatma gerekirse yeniden başlatmalar görüntülenmez.|
   |Yalnızca yeniden başlatma - güncelleştirmeleri yüklemez|Bu seçenek, güncelleştirmelerin yoksayar ve yalnızca yeniden başlatır.|

Zamanlamayı yapılandırmayı tamamladığınızda, seçin **Oluştur** düğmesi ve durum panosuna dönün. **Zamanlanmış** tablo oluşturduğunuz dağıtım zamanlaması gösterilir.

## <a name="view-results-of-an-update-deployment"></a>Güncelleştirme dağıtımının sonuçlarını görüntüleme

Zamanlanmış dağıtım başladıktan sonra, bu dağıtımın durumunu **Güncelleştirme yönetimi** bölümündeki **Güncelleştirme dağıtımları** sekmesinde görebilirsiniz.

Dağıtım o anda çalışıyorsa, **Sürüyor** durumundadır. Dağıtım başarıyla tamamlandıktan sonra durumu değişerek **başarılı**.

Dağıtımdaki bir veya daha fazla güncelleştirme başarısız olursa durum **Kısmen başarısız** olur.

![Güncelleştirme dağıtımının durumu](./media/manage-update-multi/update-view-results.png)

Bir güncelleştirme dağıtımının panosunu görmek için tamamlanan dağıtımı seçin.

**Güncelleştirme sonuçları** bölmesinde toplam güncelleştirme sayısı ve sanal makine için dağıtım sonuçları gösterilir. Sağdaki tabloda her güncelleştirmenin ve yükleme sonuçları ayrıntılı bir dökümü verir. Yükleme sonuçları aşağıdaki değerlerden biri olabilir:

- **Denenmedi**: tanımlanan bakım penceresi süresine göre yeterli süre kullanılabilir olmadığından güncelleştirme yüklenmedi.
- **Başarılı**: Güncelleştirme başarılı oldu.
- **Başarısız**: Güncelleştirme başarısız oldu.

Dağıtımın oluşturduğu tüm günlük girişlerini görmek için **Tüm günlükler**’i seçin.

Hedef sanal makinede güncelleştirme dağıtımını yöneten runbook'un iş akışını görmek için çıkış döşemesini seçin.

Dağıtımla ilgili her türlü hata hakkında ayrıntılı bilgi için **Hatalar**’ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Günlükler, çıktı ve hata dahil olmak üzere güncelleştirme yönetimi hakkında daha fazla bilgi için bkz. [güncelleştirme yönetimi çözümünü azure'da](../operations-management-suite/oms-solution-update-management.md).
