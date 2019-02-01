---
title: Azure Backup Sunucusu sorunlarını giderme
description: Azure Backup sunucusu yedekleme ve kayıt ve uygulama iş yükleri, geri yükleme sorunlarını giderin.
services: backup
author: kasinh
manager: vvithal
ms.service: backup
ms.topic: conceptual
ms.date: 11/24/2017
ms.author: kasinh
ms.openlocfilehash: 830bf8603a495d1f2708f73cf090695f1b7a7c48
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55493941"
---
# <a name="troubleshoot-azure-backup-server"></a>Azure Backup Sunucusu sorunlarını giderme

Bilgileri aşağıdaki tablolarda Azure Backup sunucusu kullanırken karşılaştığınız hataları gidermek için kullanın.

## <a name="invalid-vault-credentials-provided"></a>Sağlanan kasa kimlik bilgileri geçersiz 

Bu sorunu çözümlemek için izlediği [Bu sorun giderme adımlarını](https://docs.microsoft.com/azure/backup/backup-azure-mabs-troubleshoot#registration-and-agent-related-issues).

## <a name="the-agent-operation-failed-because-of-a-communication-error-with-the-dpm-agent-coordinator-service-on-the-server"></a>Sunucusundaki DPM aracı Düzenleyicisi hizmeti ile iletişim hatası nedeniyle aracı işlemi başarısız oldu 

Bu sorunu çözümlemek için izlediği [Bu sorun giderme adımlarını](https://docs.microsoft.com/azure/backup/backup-azure-mabs-troubleshoot#registration-and-agent-related-issues). 

## <a name="setup-could-not-update-registry-metadata"></a>Kurulum, kayıt defteri meta verilerini güncelleştiremedi

Bu sorunu çözümlemek için izlediği [Bu sorun giderme adımlarını](https://docs.microsoft.com/azure/backup/backup-azure-mabs-troubleshoot#installation-issues).




## <a name="installation-issues"></a>Yükleme sorunları

| İşlem | Hata Ayrıntıları | Geçici çözüm |
|-----------|---------------|------------|
|Yükleme | Kurulum, kayıt defteri meta verilerini güncelleştiremedi. Bu güncelleştirme hatası, depolama alanı tüketiminin overusage için neden olabilir. Bunu önlemek için ReFS Trimming kayıt defteri girdisini güncelleştirin. | Kayıt defteri anahtarı ayarlamak **SYSTEM\CurrentControlSet\Control\FileSystem\RefsEnableInlineTrim**. Dword değerini 1 olarak ayarlayın. |
|Yükleme | Kurulum, kayıt defteri meta verilerini güncelleştiremedi. Bu güncelleştirme hatası, depolama alanı tüketiminin overusage için neden olabilir. Bunu önlemek için Volume SnapOptimization kayıt defteri girdisini güncelleştirin. | Kayıt defteri anahtarı oluşturma **SOFTWARE\Microsoft veri koruma Manager\Configuration\VolSnapOptimization\WriteIds** boş bir dize değerine sahip. |
## <a name="registration-and-agent-related-issues"></a>Kayıt ve aracı ile ilgili sorunlar
| İşlem | Hata Ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Bir kasaya kaydetme | Sağlanan kasa kimlik bilgileri geçersiz. Dosya bozuk veya mu değil sahip en son kimlik bilgilerini kurtarma hizmeti ile ilişkili. | Bu hatayı düzeltmek için: <ul><li> Kasadan yeni kimlik bilgileri dosyası indirin ve yeniden deneyin. <br>(VEYA)</li> <li> Önceki eylemi işe yaramadı, farklı bir yerel dizin için kimlik bilgilerini indirme deneyin veya yeni bir kasa oluşturun. <br>(VEYA)</li> <li> Tarih güncelleştirmeyi deneyin ve saat ayarlarını anlatıldığı gibi [bu blog](https://azure.microsoft.com/blog/troubleshooting-common-configuration-issues-with-azure-backup/). <br>(VEYA)</li> <li> C:\windows\temp birden fazla 65000 dosyaları olup olmadığını denetleyin. Eski dosyaları başka bir konuma taşıyın veya öğeleri Temp klasörü silin. <br>(VEYA)</li> <li> Sertifikaların durumunu denetleyin. <br> a. Açık **bilgisayar sertifikalarını yönetme** (Denetim Masasında). <br> b. Genişletin **kişisel** düğümü ile onun alt düğümü **sertifikaları**.<br> c.  Sertifikayı kaldırın **Windows Azure Araçları**. <br> d. Azure Backup istemci kaydını yeniden deneyin. <br> (VEYA) </li> <li> Her Grup İlkesi yerinde olup olmadığını denetleyin. </li></ul> |
| Aracı korunan sunucuya gönderme | Aracı işlemi başarısız oldu DPM Agent Coordinator hizmeti ile iletişim hatası nedeniyle \<ServerName >. | **Ürününde görüntülenen önerilen eylemi işe yaramazsa, aşağıdaki adımları uygulayın**: <ul><li> Güvenilmeyen etki alanındaki bir bilgisayarı bağlıyorsanız izleyin [adımları](https://technet.microsoft.com/library/hh757801(v=sc.12).aspx). <br> (VEYA) </li><li> Güvenilen bir etki alanındaki bir bilgisayardan bağlıyorsanız, özetlenen adımları kullanarak sorun giderme [bu blog](https://blogs.technet.microsoft.com/dpm/2012/02/06/data-protection-manager-agent-network-troubleshooting/). <br>(VEYA)</li><li> Sorun giderme adımı virüsten koruma devre dışı bırakmayı deneyin. Sorunu çözmesi önerildiği virüsten koruma ayarlarını değiştirmek [bu makalede](https://technet.microsoft.com/library/hh757911.aspx).</li></ul> |
| Aracı korunan sunucuya gönderme | Sunucu için belirtilen kimlik bilgileri geçersiz. | **Ürününde görüntülenen önerilen eylemi işe yaramazsa, aşağıdaki adımları uygulayın**: <br> Koruma aracısını, belirtilen üretim sunucusu üzerinde el ile yüklemeyi deneyin [bu makalede](https://technet.microsoft.com/library/hh758186(v=sc.12).aspx#BKMK_Manual).|
| Azure Backup Aracısı Azure Backup hizmetine bağlanamıyor (Kimliği: 100050) | Azure Backup Aracısı Azure Backup hizmetine bağlanamadı. | **Ürününde görüntülenen önerilen eylemi işe yaramazsa, aşağıdaki adımları uygulayın**: <br>1. Yükseltilmiş isteminden aşağıdaki komutu çalıştırın: **psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe**. Bu, Internet Explorer penceresi açılır. <br/> 2. Git **Araçları** > **Internet Seçenekleri** > **bağlantıları** > **LAN Ayarları**. <br/> 3. Sistem hesabı için proxy ayarlarını doğrulayın. Proxy IP ve bağlantı noktası ayarlayın. <br/> 4. Internet Explorer'ı kapatın.|
| Azure Backup Aracısı yüklemesi başarısız oldu | Microsoft Azure kurtarma Hizmetleri yüklemesi başarısız oldu. Microsoft Azure kurtarma Hizmetleri yüklemesi tarafından sisteme yapılan tüm değişiklikler geri alındı. (KİMLİK: 4024) | Azure aracısını el ile yükleyin.


## <a name="configuring-protection-group"></a>Koruma grubunu yapılandırma
| İşlem | Hata Ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Koruma gruplarını yapılandırma | DPM, korumalı uygulama bileşenini oluşturamadı bilgisayar (korunan bilgisayar adı). | Seçin **Yenile** yapılandırma koruma grubu kullanıcı Arabirimi ekranında ilgili veri kaynağı/bileşen düzeyinde. |
| Koruma gruplarını yapılandırma | Koruma yapılandırılamıyor | Korumalı sunucu bir SQL server ise, sysadmin rolü izinleri korumalı bilgisayar üzerindeki sistem hesabına (ntauthority\system adlı) açıklanan şekilde sağlandığını doğrulamak [bu makalede](https://technet.microsoft.com/library/hh757977(v=sc.12).aspx).
| Koruma gruplarını yapılandırma | Bu koruma grubu için depolama havuzunda yeterli boş alan yok. | Depolama havuzuna eklenen disklerden [bir bölüm içermemelidir](https://technet.microsoft.com/library/hh758075(v=sc.12).aspx). Disk üzerinde mevcut olan birimleri silin. Daha sonra bunları depolama havuzuna ekleyin.|
| İlke değişikliği |Yedekleme İlkesi değiştirilemedi. Hata: Geçerli işlem bir [0x29834] iç hizmet hatası nedeniyle başarısız oldu. Lütfen bir süre geçtikten sonra işlemi yeniden deneyin. Sorun devam ederse Microsoft desteğine başvurun. |**Neden:**<br/>Bu hata koşulları altında üç oluşur: güvenlik ayarları etkinleştirildiğinde, önceden belirtilen minimum değerleri aşağıda bekletme aralığını azaltabilir çalıştığınızda ve desteklenmeyen bir sürümünde olduğunda. (Microsoft Azure Backup sunucusu sürümü olarak 2.0.9052 ve Azure Backup sunucusu güncelleştirmesi 1 altındaki desteklenmeyen sürümleri vardır.) <br/>**Önerilen eylem:**<br/> İlke ile ilgili güncelleştirme ile devam etmek için yukarıda belirtilen en düşük bekletme saklama süresi ayarlayın. (En düşük bekletme süresi yedi günlük, haftalık, üç hafta için dört hafta için aylık veya bir yıl için yıllık gündür.) <br><br>İsteğe bağlı olarak, başka bir yedekleme aracısı ve Azure Backup sunucusu tüm güvenlik güncelleştirmelerini yararlanmak için güncelleştirilecek bir yaklaşımdır tercih edilir. |

## <a name="backup"></a>Backup
| İşlem | Hata Ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Backup | Çoğaltma tutarsız | Koruma grubu Sihirbazı'nda Otomatik tutarlılık denetimi seçeneği açık olduğunu doğrulayın. Çoğaltma tutarsızlık ilgili öneriler ve nedenler hakkında daha fazla bilgi için Microsoft TechNet makalesini inceleyin [çoğaltma tutarsız](https://technet.microsoft.com/library/cc161593.aspx).<br> <ol><li> Sistem durumu/BMR yedekleme olması durumunda, Windows Server Yedekleme korunan sunucuda yüklü olduğunu doğrulayın.</li><li> DPM/Microsoft Azure Backup sunucusu DPM depolama havuzunda yer ile ilgili sorunları olup olmadığını denetleyin ve depolamayı gerektiği gibi ayırır.</li><li> Korumalı sunucu üzerindeki birim gölge kopyası hizmeti durumunu denetleyin. Devre dışı durumda ise, el ile başlayacak şekilde ayarlayın. Sunucusunda hizmeti başlatın. Ardından DPM/Microsoft Azure Backup sunucusu konsola geri dönün ve tutarlılık denetimiyle birlikte bir eşitleme başlatın.</li></ol>|
| Backup | İş çalışırken beklenmeyen bir hata oluştu. Cihaz hazır değil. | **Ürününde görüntülenen önerilen eylemi işe yaramazsa, aşağıdaki adımları uygulayın:** <br> <ul><li>Gölge kopya depolama alanı koruma grubundaki öğeleri üzerinde sınırsız ayarlayın ve ardından tutarlılık denetimi çalıştırın.<br></li> (VEYA) <li>Mevcut koruma silmeyi deneyin grubu ve birden çok yeni grup oluşturma. Her yeni koruma grubu içinde ayrı bir öğe olmalıdır.</li></ul> |
| Backup | Yalnızca sistem durumu yedekleme yapıyorsanız, korunan bilgisayarda Sistem Durumu yedeğini depolamak için yeterli boş alan olduğundan emin olun. | <ol><li>Windows Server Yedekleme korunan makinede yüklü olduğunu doğrulayın.</li><li>Yeterli alan olduğunu sistem durumu için korumalı bilgisayardaki doğrulayın. Bu korunan bilgisayara gidin ait doğrulamak için en kolay yolu Windows Server Yedekleme'yi açın, Seçimlerinizde tıklayın ve BMR'ı seçin. Kullanıcı arabirimini daha sonra ne kadar alan gereklidir bildirir. Açık **WSB** > **yerel yedekleme** > **yedekleme zamanlaması** > **yedekleme yapılandırması seçin**  >  **Tam sunucu** (boyutu görüntülenir). Bu boyut, doğrulama için kullanın.</li></ol>
| Backup | Çevrimiçi kurtarma noktası oluşturma başarısız oldu | Hata iletisi diyorsa "Windows Azure Backup Aracısı seçilen birimin anlık görüntüsünü oluşturamadı" çoğaltma ve kurtarma noktası birim alanı artırmayı deneyin.
| Backup | Çevrimiçi kurtarma noktası oluşturma başarısız oldu | Hata iletisi "Windows Azure Backup Aracısı OBEngine hizmetine bağlanamıyor" derse, OBEngine Hizmetleri çalıştıran bilgisayarda listesi içinde bulunduğunu doğrulayın. OBEngine hizmetine çalışmıyorsa, OBEngine hizmetini başlatmak için "OBEngine net start" komutunu kullanın.
| Backup | Çevrimiçi kurtarma noktası oluşturma başarısız oldu | "Bu sunucunun şifreleme parolası ayarlanmamış. hata iletisi diyorsa Lütfen bir şifreleme parolası yapılandırın"bir şifreleme parolası yapılandırma deneyin. Başarısız olursa, aşağıdaki adımları uygulayın: <br> <ol><li>Karalama konumu bulunduğunu doğrulayın. Bu kayıt defterinde belirtilen konumdur **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config**, adıyla **ScratchLocation** mevcut olmalıdır.</li><li> Karalama konumu varsa, eski parolayı kullanarak yeniden deneyin. *Bir şifreleme parolası yapılandırdığınız zaman, güvenli bir konuma kaydedin.*</li><ol>
| Backup | BMR için yedekleme hatası | BMR boyutu büyükse, bazı uygulama dosyalarını işletim sistemi sürücüye taşıyın ve yeniden deneyin. |
| Backup | Yeni bir Microsoft Azure Backup sunucusu bir VMware VM yeniden koruma seçeneğini eklemek kullanılabilir olarak göstermez. | VMware özellikleri, eski, devre dışı bırakılan örneğini Microsoft Azure Backup sunucusu işaret ettiği. Bu sorunu çözmek için:<br><ol><li>VCenter (SC-VMM eşdeğeri), Git **özeti** sekmesini ve sonra **özel öznitelikler**.</li>  <li>Eski Microsoft Azure Backup sunucusu adından Sil **DPMServer** değeri.</li>  <li>Yeni Microsoft Azure Backup sunucusu geri dönün ve Syf değiştirme  Seçtikten sonra **Yenile** düğme VM koruma eklemek kullanılabilir olarak onay kutusu belirir.</li></ol> |
| Backup | Dosyaları ve paylaşılan klasörleri erişirken hata oluştu | TechNet makalesinde önerildiği virüsten koruma ayarlarını değiştirmeyi deneyin [DPM sunucusunda virüsten koruma yazılımı çalıştırma](https://technet.microsoft.com/library/hh757911.aspx).|
| Backup | VMware sanal makinesi için çevrimiçi kurtarma noktası oluşturma işleri başarısız. DPM, ChangeTracking bilgilerini almaya çalışırken vmware'den bir hatayla karşılaştı. Hata kodu - FileFaultFault (kimliği 33621) |  <ol><li> Etkilenen VM'ler için VMware üzerinde CTK sıfırlayın.</li> <li>Bu bağımsız disk yerinde VMware olmadığını denetleyin.</li> <li>Etkilenen VM'ler için korumayı durdurun ve yeniden koruma **Yenile** düğmesi. </li><li>Etkilenen VM'ler için bir bilgi çalıştırın.</li></ol>|


## <a name="change-passphrase"></a>Parola Değiştir
| İşlem | Hata Ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Parola Değiştir |Güvenlik girildiği PIN yanlış. Bu işlemi tamamlamak için PIN doğru güvenliğini sağlayın. |**Neden:**<br/> Bu hata meydana gelir geçersiz girin veya (örneğin, bir parola değiştirme) kritik bir işlem gerçekleştirirken güvenlik PIN'i süresi dolmuş. <br/>**Önerilen eylem:**<br/> İşlemi tamamlamak için geçerli bir güvenlik PIN'i girmeniz gerekir. PIN almak için Azure portalında oturum açın ve kurtarma Hizmetleri Kasası'na gidin. Ardından **ayarları** > **özellikleri** > **güvenlik PIN'i Oluştur**. Bu PIN, parolayı değiştirmek için kullanın. |
| Parola Değiştir |İşlem başarısız oldu. Kimlik: 120002 |**Neden:**<br/>Güvenlik ayarları etkinleştirildiğinde veya desteklenmeyen bir sürümünü kullanırken, parola değiştirmeye çalıştığınızda bu hata oluşur.<br/>**Önerilen eylem:**<br/> Parolayı değiştirmek için Yedekleme aracısı 2.0.9052 olan en düşük sürüme güncelleştirmeniz gerekir. Ayrıca Azure Backup sunucusu en düşük güncelleştirme 1 için güncelleştirme ve geçerli bir güvenlik PIN'i girmeniz gerekir. PIN almak için Azure portalında oturum açıp kurtarma Hizmetleri Kasası'na gidin. Ardından **ayarları** > **özellikleri** > **güvenlik PIN'i Oluştur**. Bu PIN, parolayı değiştirmek için kullanın. |


## <a name="configure-email-notifications"></a>E-posta bildirimlerini yapılandırma
| İşlem | Hata Ayrıntıları | Geçici çözüm |
| --- | --- | --- |
| Bir Office 365 hesabınızı kullanarak e-posta bildirimlerini ayarlama |Hata Kimliği: 2013| **Neden:**<br> Office 365 hesabınız kullanılmaya çalışılıyor <br>**Önerilen eylem:**<ol><li> Emin olmak için ilk şey, Exchange'de "İzin ver anonim geçişi üzerinde bir alma bağlayıcısında" DPM sunucunuz için ayarlandığını ' dir. Bu yapılandırma hakkında daha fazla bilgi için bkz. [izin anonim geçişi üzerinde bir alma bağlayıcısında](https://technet.microsoft.com/library/bb232021.aspx) TechNet'teki.</li> <li> Bir iç SMTP geçiş kullanın ve Office 365 sunucunuzu kullanarak ayarlamanız gerekir, bir geçiş olması için IIS ayarlayabilirsiniz. DPM sunucusuna yapılandırma [IIS kullanarak SMTP o365'e geçiş](https://technet.microsoft.com/library/aa995718(v=exchg.65).aspx).<br><br> **ÖNEMLİ:** Kullandığınızdan emin olun user@domain.com biçimi ve *değil* etki alanı\kullanıcı.<br><br><li>SMTP sunucusu olarak yerel sunucu adını kullanacak şekilde noktası DPM bağlantı noktası 587. Bu e-postaları alınması gereken kullanıcı e-posta gelin.<li> Kullanıcı adı ve parola DPM SMTP Kurulumu sayfasında DPM açıktır etki alanındaki bir etki alanı hesabı olmalıdır. </li><br> **NOT**: SMTP sunucu adresini değiştirirken, yeni ayarlarında değişiklik yapmak, ayarlar kutusunu kapatın ve yeni değeri yansıtır emin olmak için yeniden açın.  Her zaman bu şekilde test en iyi uygulama, bu nedenle, yeni ayarların etkili, yalnızca değiştirme ve test neden.<br><br>Bu işlem sırasında herhangi bir zamanda DPM Konsolu kapatmak ve aşağıdaki kayıt defteri anahtarlarını düzenleyerek bu ayarları silebilirsiniz: **HKLM\SOFTWARE\Microsoft\Microsoft Data Protection Manager\Notification\ <br/> SMTPPassword silin ve SMTPUserName anahtarları**. Yeniden başlattıktan sonra kullanıcı Arabirimine geri ekleyebilirsiniz.
