---
title: Azure Site Recovery kullanarak VMware Vm'lerini ve fiziksel sunucuları azure'a olağanüstü durum kurtarma için çoğaltma sorunlarını giderme | Microsoft Docs
description: Bu makalede VMware vm'lerinin olağanüstü durum kurtarma sırasında yaygın çoğaltma sorunlarına yönelik sorun giderme bilgileri ve fiziksel sunucuları Azure'a Azure Site Recovery kullanarak sağlar.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/7/2019
ms.author: mayg
ms.openlocfilehash: 71c07d93d75ee372a50ec4ff5fc81e92926d329b
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55964790"
---
# <a name="troubleshoot-replication-issues-for-vmware-vms-and-physical-servers"></a>VMware Vm'lerini ve fiziksel sunucular için çoğaltma sorunlarını giderme

Azure Site Recovery kullanarak VMware sanal makineleri veya fiziksel sunucuları koruduğunuzda, belirli bir hata iletisi görebilirsiniz. Bu makalede, şirket içi VMware Vm'leri ve fiziksel sunucuları Azure'a kullanarak çoğaltma yaptığınızda çalıştırdığınızca bazı yaygın sorunlar [Site Recovery](site-recovery-overview.md).

## <a name="monitor-process-server-health-to-avoid-replication-issues"></a>Çoğaltma sorunlarını önlemek için işlem sunucusunun sistem durumunu izleme

İlişkili kaynak makineleriniz için çoğaltmanın ilerlediğini emin olmak için portal üzerindeki işlem sunucusu (PS) durumunu izlemek için önerilir. Kasada Yönet gidin > Site Recovery altyapısı > yapılandırma sunucuları. Yapılandırma sunucusu dikey penceresinde, ilişkili bölümünde işlem sunucusunu tıklayın. Kendi sistem durumu istatistikleriyle işlem sunucu dikey penceresi açılır. CPU kullanımı, bellek kullanımı, çoğaltma, sertifika sona erme tarihi ve kullanılabilir boş alanı gerekli PS hizmetlerinin durumunu izleyebilirsiniz. Tüm istatistiklerin durum yeşil olmalıdır. 

**Bellek ve CPU kullanımı % 70'in altında olan ve % 25 yukarıdaki alanı boş önerilen**. Boş alan Azure'a yüklemeden önce kaynak makinelerden çoğaltma verilerini depolamak için kullanılan işlem sunucusu önbellek diski alanı ifade eder. %20 değerinden azaltır, çoğaltma için tüm ilişkili kaynak makinelerden kısıtlanır. İzleyin [kapasite Kılavuzu](./site-recovery-plan-capacity-vmware.md#capacity-considerations) kaynak makinelerden çoğaltma için gerekli yapılandırmayı öğrenin.

PS makinede şu hizmetlerin çalıştığından emin olun. Veya çalışmadığından herhangi bir hizmeti yeniden başlatın.

**Yerleşik bir işlem sunucusu**

* cxprocessserver
* Inmage Pushınstall
* Günlük karşıya yükleme hizmeti (LogUpload)
* Inmage Scout uygulama hizmeti
* Microsoft Azure kurtarma Hizmetleri Aracısı (obengine)
* Inmage Scout VX Aracısı - Sentinel/Outpost (svagents)
* tmansvc
* World Wide Web Yayımlama Hizmeti (W3SVC)
* MySQL
* Microsoft Azure Site Recovery hizmeti (dra)

**Genişleme işlem sunucusu**

* cxprocessserver
* Inmage Pushınstall
* Günlük karşıya yükleme hizmeti (LogUpload)
* Inmage Scout uygulama hizmeti
* Microsoft Azure kurtarma Hizmetleri Aracısı (obengine)
* Inmage Scout VX Aracısı - Sentinel/Outpost (svagents)
* tmansvc

**Azure'da yeniden çalışma için işlem sunucusu**

* cxprocessserver
* Inmage Pushınstall
* Günlük karşıya yükleme hizmeti (LogUpload)

Tüm hizmetlerin StartType ayarlandığından emin olun **otomatik veya Otomatik (Gecikmeli Başlatma)**. Microsoft Azure kurtarma Hizmetleri Aracısı (obengine) hizmet olarak yukarıda kendi StartType sahip olması gerekmez.

## <a name="initial-replication-issues"></a>İlk çoğaltma sorunları

İlk çoğaltma hatalarını genellikle işlem sunucusu ile Azure arasında veya kaynak sunucu ile işlem sunucusu arasında bağlantı sorunları nedeniyle oluşup. Çoğu durumda, aşağıdaki bölümlerde yer alan adımları tamamlayarak bu sorunları giderebilirsiniz.

### <a name="check-the-source-machine"></a>Kaynak makine denetleyin

Kaynak makine denetleyebilirsiniz aşağıdaki listeyi gösterir yolları:

*  Kaynak sunucuda komut satırında aşağıdaki komutu çalıştırarak HTTPS bağlantı noktası üzerinden işlem sunucusu ping Telnet kullanın. HTTPS bağlantı noktası 9443, çoğaltma trafiğini gönderip için işlem sunucusu tarafından kullanılan varsayılan değerdir. Kayıt zamanında Bu bağlantı noktasını değiştirebilirsiniz. Aşağıdaki komut, sorunları o blok güvenlik duvarı bağlantı noktası ve ağ bağlantısı sorunları için denetler.


   `telnet <process server IP address> <port>`


   > [!NOTE]
   > Telnet bağlantısını test etmek için kullanın. Kullanmayın `ping`. Telnet yüklü değilse, listelenen adımları tamamlamak [Telnet istemcisi yükleme](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx).

   Telnet PS bağlantı noktasına başarıyla bağlanabilmek için ise, boş bir ekran görülebilir.

   İşlem sunucusuna bağlanamıyorsa, işlem sunucusunun gelen bağlantı noktası 9443'e izin verin. Örneğin, bir çevre ağına ağınız varsa, işlem sunucusunun gelen bağlantı noktası 9443'e izin vermeniz gereken veya denetimli alt ağ. Ardından, sorunun nerede oluştuğunu görmek için kontrol edin.

*  Telnet başarılı ise ve henüz kaynak makine PS erişilebilir olmadığını bildiriyor. kaynak makinedeki web tarayıcısı açın ve adres https://<PS_IP>:<PS_Data_Port>/ erişilebilir olup olmadığını denetleyin.

    HTTPS sertifika hatası bu adresi ulaşmaktan bekleniyor. Sertifika hatası yok sayılıyor ve devam etmeden 400 – Hatalı istek, sunucu başka bir deyişle, tarayıcının isteği yanıt veremez ve sunucuya standart HTTPS bağlantı da ve iyi çalışıp çalışmadığını bitmelidir.

    Bu başarısız olursa, tarayıcı hata iletisinde ayrıntıları rehberlik sağlar. İçin örneğin Ara sunucu kimlik doğrulaması yanlışsa, proxy sunucusu 407 – Proxy kimlik doğrulaması gerekli birlikte gerekli eylemleri hata iletisi döndürür. 

*  Kaynak VM ağına ilgili hataları aşağıdaki günlükleri karşıya yükleme hataları kontrol edin:

       C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\svagents*.log 

### <a name="check-the-process-server"></a>İşlem sunucusu denetleyin

İşlem sunucusu denetleyebilirsiniz aşağıdaki listeyi gösterir yolları:

> [!NOTE]
> NAT IP üzerinde yapılandırılmamış olması ve işlem sunucusu statik bir IPv4 adresi olmalıdır.

* **Kaynak makine ve işlem sunucusu arasındaki bağlantıyı denetleyin**
1. Telnet kaynak makineden mümkün olan ve henüz PS kaynak sunucudan erişilebilir değil durumunda, kaynak VM üzerinde cxpsclient aracını çalıştırarak cxprocessserver kaynak VM ile uçtan uca bağlantıyı denetleyin:

       <install folder>\cxpsclient.exe -i <PS_IP> -l <PS_Data_Port> -y <timeout_in_secs:recommended 300>

    PS karşılık gelen hatalar hakkında ayrıntılı bilgi için aşağıdaki dizinde oluşturulan günlüklerini kontrol edin:

       C:\ProgramData\ASR\home\svsystems\transport\log\cxps.err
       and
       C:\ProgramData\ASR\home\svsystems\transport\log\cxps.xfer
2. PS gelen hiçbir sinyal yok durumda PS aşağıdaki günlükleri kontrol edin:

       C:\ProgramData\ASR\home\svsystems\eventmanager*.log
       and
       C:\ProgramData\ASR\home\svsystems\monitor_protection*.log

*  **İşlem sunucusu etkin bir şekilde veri Azure'a gönderme olup olmadığını denetleyin**.

   1. İşlem sunucusu, Görev Yöneticisi'ni (Ctrl + Shift + Esc tuşlarına basın) açın.
   2. Seçin **performans** sekmesine tıklayın ve ardından **açık Kaynak İzleyicisi** bağlantı. 
   3. Üzerinde **Kaynak İzleyicisi** sayfasında **ağ** sekmesi. Altında **ağ etkinliği işlemlerle**, kontrol olup olmadığını **cbengine.exe** etkin bir şekilde büyük miktarlarda veri gönderiyor.

        ![Ağ etkinliği ile işlem birimlerini gösteren ekran görüntüsü](./media/vmware-azure-troubleshoot-replication/cbengine.png)

   Büyük bir veri hacmi cbengine.exe gönderme değil, aşağıdaki bölümlerde yer alan adımları tamamlayın.

*  **İşlem sunucusu Azure Blob depolama alanına bağlanıp bağlanamadığınızı denetleyin**.

   Seçin **cbengine.exe**. Altında **TCP bağlantılarını**, Azure blogunda depolama URL'si işlem sunucusundan bağlantı olup olmadığını denetleyin.

   ![Cbengine.exe ve Azure Blob Depolama URL'si arasındaki bağlantıları gösteren ekran görüntüsü](./media/vmware-azure-troubleshoot-replication/rmonitor.png)

   İşlem sunucusundan bağlantı Denetim Masası ' nda Azure blogunda depolama URL'sine yoksa seçin **Hizmetleri**. Aşağıdaki hizmetlerin çalışır durumda olup olmadığını denetleyin:

   *  cxprocessserver
   *  Inmage Scout VX Aracısı-Sentinel/Outpost
   *  Microsoft Azure Kurtarma Hizmetleri Aracısı
   *  Microsoft Azure Site Recovery Hizmeti
   *  tmansvc

   Veya çalışmadığından herhangi bir hizmeti yeniden başlatın. Sorunun nerede oluştuğunu görmek için kontrol edin.

*  **İşlem sunucusu bağlantı noktası 443'ü kullanarak Azure genel IP adresine bağlanıp bağlanamadığınızı denetleyin**.

   %ProgramFiles%\Microsoft Azure kurtarma Hizmetleri Agent\Temp, en son CBEngineCurr.errlog dosyasını açın. Dosyada arayın **443** veya dizesi için **başarısız bağlantı denemesi**.

   ![Hata gösteren ekran görüntüsü Temp klasörü günlüğe kaydeder.](./media/vmware-azure-troubleshoot-replication/logdetails1.png)

   Sorunları gösteriliyorsa işlem sunucusu, komut satırında (IP adresi, önceki görüntüde maskelenir), Azure genel IP adresine ping atmayı Telnet kullanın. Azure genel IP adresi, bağlantı noktası 443'ü kullanarak, bir CBEngineCurr.currLog dosyasında bulabilirsiniz:

   `telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443`

   Bağlanamıyorsanız, erişim sorununu sonraki adımda açıklandığı gibi güvenlik duvarı veya proxy ayarları nedeniyle olup olmadığını denetleyin.

*  **IP adresi tabanlı güvenlik duvarı işlem sunucusu üzerindeki erişimi engeller olup olmadığını denetleyin**.

   Sunucuda IP adresi tabanlı güvenlik duvarı kurallarını kullanırsanız, tam listesini indirin [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). Güvenlik Duvarı yapılandırmanızda Güvenlik Duvarı'nın azure'a (ve varsayılan HTTPS bağlantı noktası, 443) iletişimine izin verdiğinden emin olmak için IP adresi aralıklarını ekleyin. (Erişim denetimi ve kimlik yönetimi için kullanılır) Azure Batı ABD bölgesinde ve aboneliğinizin Azure bölgesi için IP adresi aralıklarına izin verin.

*  **İşlem sunucusu üzerindeki URL tabanlı bir güvenlik duvarı erişimi engelliyor olup olmadığını denetleyin**.

   Sunucuda bir URL tabanlı güvenlik duvarı kuralı kullanırsanız, güvenlik duvarı yapılandırması için aşağıdaki tabloda listelenen URL'leri ekleyin:

[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  

*  **İşlem sunucusu üzerindeki ara sunucu ayarları erişimi engellemek olup olmadığını denetleyin**.

   Bir ara sunucu kullanıyorsanız, proxy sunucusu adı DNS sunucusu tarafından çözümlenir emin olun. Yapılandırma sunucusunu ayarladıktan sağladığınız değeri denetleyin, kayıt defteri anahtarına gidin **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure Site Recovery\ProxySettings**.

   Ardından, veri göndermek için aynı ayarları Azure Site Recovery aracısı tarafından kullanıldığından emin olun: 
      
   1. Arama **Microsoft Azure yedekleme**. 
   2. Açık **Microsoft Azure Backup**ve ardından **eylem** > **özelliklerini değiştirme**. 
   3. Üzerinde **Proxy Yapılandırması** sekmesinde proxy adresi görmeniz gerekir. Proxy adresi kayıt defteri ayarlarında gösterilen proxy adresi ile aynı olmalıdır. Aksi durumda, aynı adrese değiştirin.

*  **İşlem sunucusu üzerindeki bant genişliğini kısıtlama kısıtlı olup olmadığını denetleyin**.

   Bant genişliğini artırın ve ardından Sorun oluşmaya devam edip etmediğini denetleyin.

## <a name="source-machine-isnt-listed-in-the-azure-portal"></a>Kaynak makine Azure Portalı'nda listelenmez

Site RECOVERY'yi kullanarak çoğaltmayı etkinleştirmek için kaynak makine seçmeye çalıştığınızda, makine aşağıdaki nedenlerden biri için kullanılabilir olmayabilir:

* **Aynı içeren iki sanal makine örnek UUİD'si**: VCenter altında iki sanal makine aynı örneğini UUID varsa, Azure portalında ilk sanal makine yapılandırma sunucusu tarafından bulunan gösterilir. Bu sorunu çözmek için iki sanal makine aynı örneğini UUID sahip olun. Bu senaryo, burada VM yedekleme etkin hale gelir ve bulma Kayıtlarımız günlüğe örneklerinde görülen yaygındır. Başvurmak [Azure Site Recovery Azure'a VMware: Yinelenen veya eski girdilerin temizlenmesini nasıl](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx) çözümlenecek.
* **Yanlış vCenter kullanıcı kimlik bilgilerini**: OVF şablonu veya birleşik Kurulumu kullanarak yapılandırma sunucusunu ayarladıktan ayarladığınızda, doğru vCenter kimlik bilgilerini emin olun. Kurulum sırasında eklenen kimlik bilgilerini doğrulamak için bkz: [otomatik bulma için kimlik bilgilerini değiştirme](vmware-azure-manage-configuration-server.md#modify-credentials-for-automatic-discovery).
* **vCenter ayrıcalıklar yetersiz**: VCenter erişmek için sağlanan izinler gerekli izinlere sahip değilseniz, sanal makineleri keşfetmek için hatası gerçekleşebilir. İçinde açıklanan izinleri olun [otomatik bulma için bir hesap hazırlama](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) vCenter kullanıcı hesabına eklenir.
* **Azure Site Recovery yönetim sunucuları**: Sanal Makine Yönetim sunucusu bir veya daha fazla aşağıdaki rollerin - altında kullanılıyorsa, yapılandırma sunucusu /scale-out işlem sunucusu / ana hedef sunucusu, sonra da sanal makineyi portaldan seçme mümkün olmayacaktır. Yönetim sunucuları yinelenemez.
* **Zaten korumalı/başarısız üzerinden Azure Site Recovery Hizmetleri aracılığıyla**: Sanal makine zaten korumalı veya Site Recovery yük devretti, sanal makineyi Portalı'nda koruma için işaretleyin kullanılamaz. Portalda aradığınız sanal makineyi başka bir kullanıcı tarafından veya farklı bir abonelikte zaten korunmuyor emin olun.
* **bağlı vCenter**: VCenter bağlı durumda olup olmadığını denetleyin. Doğrulamak için kurtarma Hizmetleri Kasası'na gidin > Site Recovery altyapısı > yapılandırma sunucuları >'a tıklayın, ilgili yapılandırma sunucusunda > hakkınız ilişkili sunucular ilgili ayrıntıları içeren bir dikey pencere açar. VCenter bağlı olup olmadığını denetleyin. "Bağlanmadı" durumda değilse, sorunu çözün ve ardından [yapılandırma sunucusunu Yenile](vmware-azure-manage-configuration-server.md#refresh-configuration-server) portalında. Bundan sonra sanal makine portalda listelenir.
* **ESXi güç kapalı**: Altında sanal makinenin bulunduğu ESXi ana bilgisayar ise, kapalı sonra sanal makine listede bulunmayan veya Azure portalında seçilebilir olmayacak. ESXi konağı Aç [yapılandırma sunucusunu Yenile](vmware-azure-manage-configuration-server.md#refresh-configuration-server) portalında. Bundan sonra sanal makine portalda listelenir.
* **Yeniden başlatma bekleniyor**: Sanal makine üzerinde bekleyen bir yeniden başlatma yoksa, daha sonra Azure portalında makine seçmek mümkün olmayacaktır. Yeniden başlatma bekleniyor etkinliklerini tamamlamak için olun [yapılandırma sunucusunu Yenile](vmware-azure-manage-configuration-server.md#refresh-configuration-server). Bundan sonra sanal makine portalda listelenir.
* **Nebyl nalezen IP**: Sanal makine ile ilişkili geçerli bir IP adresi yoksa, daha sonra Azure portalında makine seçmek mümkün olmayacaktır. Sanal makine için geçerli bir IP adresi atamak için olun [yapılandırma sunucusunu Yenile](vmware-azure-manage-configuration-server.md#refresh-configuration-server). Bundan sonra sanal makine portalda listelenir.

## <a name="protected-virtual-machines-are-greyed-out-in-the-portal"></a>Korumalı sanal makineleri dışarı portalda gri

Sistemdeki yinelenen girişler varsa altında Site Recovery çoğaltılan sanal makineler, Azure portalında mevcut değildir. Eski girişleri silmek ve sorunu çözmek öğrenmek için bkz [Azure Site Recovery VMware-Azure: Yinelenen veya eski girdilerin temizlenmesini nasıl](https://social.technet.microsoft.com/wiki/contents/articles/32026.asr-vmware-to-azure-how-to-cleanup-duplicatestale-entries.aspx).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla yardıma ihtiyacınız varsa, sorunuzu gönderin [Azure Site Recovery Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). Etkin bir topluluk sunuyoruz ve biri, Mühendislerimiz yardımcı olabilir.
