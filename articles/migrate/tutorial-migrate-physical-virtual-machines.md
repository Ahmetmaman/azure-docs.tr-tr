---
title: Azure geçişi ile makineleri fiziksel sunucu olarak Azure 'a geçirin.
description: Bu makalede, Azure geçişi ile fiziksel makinelerin Azure 'a nasıl geçirileceği açıklanır.
author: rahulg1190
ms.author: rahugup
ms.manager: bsiva
ms.topic: tutorial
ms.date: 04/15/2020
ms.custom: MVC
ms.openlocfilehash: af1c321e5c537fbd3af770cb392c538e6056e075
ms.sourcegitcommit: ea551dad8d870ddcc0fee4423026f51bf4532e19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2020
ms.locfileid: "96752881"
---
# <a name="migrate-machines-as-physical-servers-to-azure"></a>Makineleri fiziksel sunucu olarak Azure 'a geçirme

Bu makalede, Azure geçişi: sunucu geçiş aracını kullanarak makineleri fiziksel sunucu olarak Azure 'a nasıl geçirebileceğiniz gösterilmektedir. Makineleri fiziksel sunucu olarak düşünerek, bir dizi senaryoda yararlı olacak şekilde geçirme:

- Şirket içi fiziksel sunucuları geçirin.
- Xen, KVM gibi platformlar tarafından sanallaştırılan VM 'Leri geçirin.
- Hyper-V veya VMware VM 'lerini geçirin, bazı nedenlerle [Hyper-v](tutorial-migrate-hyper-v.md)veya [VMware](server-migrate-overview.md) geçişi için standart geçiş işlemini kullanamazsınız.
- Özel bulutlarda çalışan VM 'Leri geçirin.
- Amazon Web Services (AWS) veya Google Cloud Platform (GCP) gibi genel bulutlarda çalışan VM 'Leri geçirin.


Bu öğretici, fiziksel sunucuların Azure 'a nasıl değerlendirileceğini ve geçirileceğini gösteren bir serideki üçüncü bir seridir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure geçişi ile Azure 'u kullanmaya hazırlanma: sunucu geçişi.
> * Geçirmek istediğiniz makineler için gereksinimleri denetleyin ve Azure geçişi çoğaltma gereci için, makineleri bulma ve Azure 'a geçirmek için kullanılan bir makine hazırlayın.
> * Azure geçiş hub 'ına Azure geçiş sunucusu geçiş aracı 'nı ekleyin.
> * Çoğaltma gerecini ayarlayın.
> * Taşımak istediğiniz makinelere Mobility hizmetini yükler.
> * Çoğaltmayı etkinleştirin.
> * Her şeyin beklendiği gibi çalıştığından emin olmak için bir test geçişi çalıştırın.
> * Azure 'a tam geçiş gerçekleştirin.

> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir, böylece bir kavram kanıtı hızlı bir şekilde ayarlayabilmenizi sağlayabilirsiniz. Öğreticiler mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı yönergeler için Azure geçişi için nasıl yapılır-TOS ' i gözden geçirin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.


## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce karşılamanız gereken ön koşullar şunlardır:

Geçiş mimarisini [gözden geçirin](./agent-based-migration-architecture.md) .

## <a name="prepare-azure"></a>Azure’u hazırlama

Sunucu geçişi ile geçiş için Azure 'u hazırlayın.

**Görev** | **Ayrıntılar**
--- | ---
**Azure Geçişi projesi oluşturma** | [Yeni bir proje oluşturmak](https://docs.microsoft.com/azure/migrate/create-manage-projects)için Azure hesabınızda katkıda bulunan veya sahip izinlerinin olması gerekir.
**Azure hesabınız için izinleri doğrulama** | Azure hesabınızın bir VM oluşturmak ve Azure yönetilen diskine yazmak için izinleri olması gerekir.


### <a name="assign-permissions-to-create-project"></a>Proje oluşturmak için izin atama

1. Azure portalında ilgili aboneliği açın ve **Erişim denetimi (IAM)** öğesini seçin.
2. **Erişimi denetle**' de ilgili hesabı bulun ve izinleri görüntülemek için tıklatın.
3. **Katkıda bulunan** veya **sahip** izinlerinizin olması gerekir.
    - Ücretsiz Azure hesabı oluşturduysanız aboneliğinizin sahibi siz olursunuz.
    - Aboneliğin sahibi siz değilseniz sahibiyle iletişime geçerek gerekli rolün atanmasını sağlayın.


### <a name="assign-azure-account-permissions"></a>Azure hesabı izinleri atama

Sanal makine katılımcısı rolünü Azure hesabına atayın. Bu izinler şunları sağlar:

- Seçilen kaynak grubunda sanal makine oluşturma.
- Seçilen sanal ağda sanal makine oluşturma.
- Azure yönetilen diskine yazın. 

### <a name="create-an-azure-network"></a>Azure ağı oluşturma

Bir Azure sanal ağı (VNet) [ayarlayın](../virtual-network/manage-virtual-network.md#create-a-virtual-network) . Azure 'a çoğaltma yaptığınızda Azure VM 'Ler oluşturulur ve geçişi ayarlarken belirttiğiniz Azure VNet 'e birleştirilir.

## <a name="prepare-for-migration"></a>Geçiş için hazırlanma

Fiziksel sunucu geçişine hazırlanmak için fiziksel sunucu ayarlarını doğrulamanız ve bir çoğaltma gereci dağıtmaya hazırlanmanız gerekir.

### <a name="check-machine-requirements-for-migration"></a>Geçiş için makine gereksinimlerini denetleme

Makinelerin Azure 'a geçiş gereksinimleriyle uyumlu olduğundan emin olun. 

> [!NOTE]
> Fiziksel makineleri geçirirken Azure geçişi: sunucu geçişi, Azure Site Recovery hizmetinde aracı tabanlı olağanüstü durum kurtarma ile aynı çoğaltma mimarisini kullanır ve bazı bileşenler aynı kod tabanını paylaşır. Bazı içerikler Site Recovery belgelerine bağlantı verebilir.

1. Fiziksel sunucu gereksinimlerini [doğrulayın](migrate-support-matrix-physical-migration.md#physical-server-requirements) .
2. Azure 'a çoğaltılan şirket içi makinelerin [Azure VM gereksinimleriyle](migrate-support-matrix-physical-migration.md#azure-vm-requirements)uyumlu olduğunu doğrulayın.
3. Azure 'a geçirmeden önce VM 'lerde gereken bazı değişiklikler vardır.
    - Bazı işletim sistemleri için Azure geçişi bu değişiklikleri otomatik olarak yapar. 
    - Geçişe başlamadan önce bu değişiklikleri yapmak önemlidir. Değişikliği yapmadan önce VM 'yi geçirirseniz, VM Azure 'da önyüklenemeyebilir. Yapmanız gereken [Windows](prepare-for-migration.md#windows-machines) ve [Linux](prepare-for-migration.md#linux-machines) değişikliklerini gözden geçirin.

### <a name="prepare-a-machine-for-the-replication-appliance"></a>Çoğaltma gereci için bir makine hazırlama

Azure geçişi: sunucu geçişi, makineleri Azure 'a çoğaltmak için bir çoğaltma gereci kullanır. Çoğaltma gereci aşağıdaki bileşenleri çalıştırır.

- **Yapılandırma sunucusu**: yapılandırma sunucusu, şirket Içi ve Azure arasındaki iletişimleri koordine eder ve veri çoğaltmasını yönetir.
- **İşlem sunucusu**: işlem sunucusu bir çoğaltma ağ geçidi olarak davranır. Çoğaltma verilerini alır; önbelleğe alma, sıkıştırma ve şifreleme ile en iyi duruma getirir ve Azure 'da bir önbellek depolama hesabına gönderir. 

Gereç dağıtımı için aşağıdaki gibi hazırlanın:

- Çoğaltma gerecini barındırmak için bir makine hazırlarsınız. Makine gereksinimlerini [gözden geçirin](migrate-replication-appliance.md#appliance-requirements) .
- Çoğaltma gereci MySQL kullanır. Gereçte MySQL yükleme [seçeneklerini](migrate-replication-appliance.md#mysql-installation) gözden geçirin.
- Çoğaltma gerecinin [ortak](migrate-replication-appliance.md#url-access) ve [kamu](migrate-replication-appliance.md#azure-government-url-access) bulutları 'Na erişmesi Için gereken Azure URL 'lerini gözden geçirin.
- Çoğaltma gereci için [port] (Migrate-Replication-gereci. MD # Port-Access) erişim gereksinimlerini gözden geçirin.

> [!NOTE]
> Çoğaltma gereci, çoğaltmak istediğiniz bir kaynak makineye veya daha önce yüklemiş olduğunuz Azure geçişi bulma ve değerlendirme gerecine yüklenmelidir.

## <a name="set-up-the-replication-appliance"></a>Çoğaltma gereç ayarı

Geçişin ilk adımı, çoğaltma gerecini ayarlamaya yönelik. Fiziksel sunucu geçişi için gereci ayarlamak üzere gereç için yükleyici dosyasını indirirler ve ardından bunu [hazırladığınız makinede](#prepare-a-machine-for-the-replication-appliance)çalıştırırsınız. Gereci yükledikten sonra Azure geçişi sunucu geçişine kaydetmelisiniz.


### <a name="download-the-replication-appliance-installer"></a>Çoğaltma gereç yükleyicisini indirin

1. Azure geçişi proje > **sunucularında** **Azure geçişi: sunucu geçişi**' nde **bul**' a tıklayın.

    ![VM'leri bulma](./media/tutorial-migrate-physical-virtual-machines/migrate-discover.png)

3. Makinelerde **bulunan makineler**  >  **sanallaştırılmış mi?**, **sanallaştırılmamış/diğer**' e tıklayın.
4. **Hedef bölge**' de, makineleri geçirmek istediğiniz Azure bölgesini seçin.
5. **Geçiş için hedef bölgenin bölge adı olduğunu onaylayın**' i seçin.
6. **Kaynak oluştur**' a tıklayın. Bu, arka planda bir Azure Site Recovery Kasası oluşturur.
    - Azure geçişi sunucu geçişi ile geçiş zaten ayarladıysanız, kaynaklar daha önce ayarlandığı için hedef seçenek yapılandırılamaz.    
    - Bu düğmeye tıkladıktan sonra bu proje için hedef bölgeyi değiştiremezsiniz.
    - Sonraki tüm geçişler bu bölgedir.

7. **Yeni bir çoğaltma gereci yüklemek** istiyor musunuz?, **çoğaltma gereci yüklensin**' i seçin.
9. **Çoğaltma gereci yazılımını indirip yükleyin** bölümünde gereç yükleyicisini ve kayıt anahtarını indirin. Gereci kaydettirmek için anahtar gerekir. Anahtar indirildikten beş gün sonra geçerlidir.

    ![Sağlayıcıyı indir](media/tutorial-migrate-physical-virtual-machines/download-provider.png)

10. Gereç kurulum dosyasını ve anahtar dosyasını gereç için oluşturduğunuz Windows Server 2016 makinesine kopyalayın.
11. Yükleme tamamlandıktan sonra gereç Yapılandırma Sihirbazı otomatik olarak başlatılır (gerecin masaüstünde oluşturulan Cspsconfigtool kısayolunu kullanarak Sihirbazı el ile de başlatabilirsiniz). Mobility hizmetinin göndererek yüklenmesi için kullanılacak hesap ayrıntılarını eklemek için sihirbazın hesapları Yönet sekmesini kullanın. Bu öğreticide, çoğaltılacak kaynak VM 'Lere Mobility hizmetini el ile yükleyeceğiz, bu nedenle bu adımda bir kukla hesap oluşturun ve devam edin. "Konuk" adlı kukla hesabı, Kullanıcı adı olarak "Kullanıcı adı" ve hesabın parolası olarak "parola" olarak oluşturmak için aşağıdaki ayrıntıları sağlayabilirsiniz. Çoğaltmayı etkinleştir aşamasında bu kukla hesabı kullanacaksınız. 

12. Kurulum sonrasında gereç yeniden başlatıldıktan sonra, **makine bul**' da, **yapılandırma sunucusu**' nda yeni gereç ' ı seçin ve **kaydı Sonlandır**' a tıklayın. Kayıt işlemini sonuçlandırma, çoğaltma gerecini hazırlamak için birkaç son görevi gerçekleştirir.

    ![Kaydı Sonlandır](./media/tutorial-migrate-physical-virtual-machines/finalize-registration.png)

Bulunan makineler Azure geçişi sunucu geçişi 'nde görünene kadar kayıt gerçekleştirildikten sonra bu işlem biraz zaman alabilir. VM 'Ler keşfedildiğinde, **bulunan sunucular** sayılır.

![Bulunan sunucular](./media/tutorial-migrate-physical-virtual-machines/discovered-servers.png)


## <a name="install-the-mobility-service"></a>Mobility hizmetini yükleme

Geçirmek istediğiniz makinelerde, Mobility hizmeti aracısını yüklemeniz gerekir. Aracı yükleyicileri, çoğaltma aracısında kullanılabilir. Doğru yükleyiciyi bulur ve geçirmek istediğiniz her makineye aracıyı yüklersiniz. Bunu şu şekilde yapabilirsiniz:

1. Çoğaltma gereci 'nda oturum açın.
2. **%ProgramData%\asr\home\svsystems\pushınstallsvc\repository dizinine** gidin.
3. Makine işletim sistemi ve sürümü için yükleyiciyi bulun. [Desteklenen işletim sistemlerini](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines)gözden geçirin. 
4. Yükleyici dosyasını geçirmek istediğiniz makineye kopyalayın.
5. Gereci dağıtırken oluşturulan parolanın bulunduğundan emin olun.
    - Dosyayı makinedeki geçici bir metin dosyasında depolayın.
    - Parola çoğaltma gereci üzerinde elde edebilirsiniz. Geçerli parolayı görüntülemek için komut satırından **C:\ProgramData\ASR\home\svsystems\bin\genpassphrase.exe-v** ' yi çalıştırın.
    - Parolayı yeniden üretme. Bu, bağlantıyı keser ve çoğaltma gerecini yeniden kaydetmeniz gerekir.


### <a name="install-on-windows"></a>Windows’ta yükleme

1. Yükleyici dosyasının içeriğini makinede yerel bir klasöre (örneğin, C:\Temp) ayıklayın, aşağıdaki gibi:

    ```
    ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
    MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
    cd C:\Temp\Extracted
    ```
2. Mobility hizmeti yükleyicisini çalıştırın:
    ```
   UnifiedAgent.exe /Role "MS" /Platform "VmWare" /Silent
    ```
3. Aracıyı çoğaltma gereci ile kaydedin:
    ```
    cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
    UnifiedAgentConfigurator.exe  /CSEndPoint <replication appliance IP address> /PassphraseFilePath <Passphrase File Path>
    ```

### <a name="install-on-linux"></a>Linux'ta yükleme

1. Yükleyici tarbol içeriğini makinedeki yerel bir klasöre (örneğin/T MP/mobsvcınstaller) ayıklayın ve aşağıdaki gibi:
    ```
    mkdir /tmp/MobSvcInstaller
    tar -C /tmp/MobSvcInstaller -xvf <Installer tarball>
    cd /tmp/MobSvcInstaller
    ```
2. Yükleyici betiğini çalıştırın:
    ```
    sudo ./install -r MS -v VmWare -q
    ```
3. Aracıyı çoğaltma gereci ile kaydedin:
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <replication appliance IP address> -P <Passphrase File Path>
    ```

## <a name="replicate-machines"></a>Makineleri çoğaltma

Şimdi, geçiş için makineler ' i seçin. 

> [!NOTE]
> En fazla 10 makineyi birbirine çoğaltabilirsiniz. Daha fazla çoğaltma yapmanız gerekiyorsa bunları aynı anda 10 ' da çoğaltın.

1. Azure geçişi proje > **sunucularında** **Azure geçişi: sunucu geçişi**' nde **Çoğalt**' a tıklayın.

    ![Azure geçişi-sunucular ekranının, Azure geçişi: geçiş araçları altında sunucu geçişi ' nde seçilen çoğaltma düğmesini gösteren ekran görüntüsü.](./media/tutorial-migrate-physical-virtual-machines/select-replicate.png)

2. **Çoğaltma**' da, makinelerinizde > **kaynak ayarları**  >  **sanallaştırılır mi?**, **sanallaştırılmamış/diğer** seçeneğini belirleyin.
3. **Şirket içi gereç** bölümünde, ayarladığınız Azure geçiş gerecinin adını seçin.
4. **Işlem sunucusu**' nda, çoğaltma gerecinin adını seçin.
6. **Konuk kimlik bilgileri**' nde, Mobility hizmetini el ile yüklemek için lütfen daha önce [çoğaltma yükleyicisi kurulumu](#download-the-replication-appliance-installer) sırasında oluşturulan kukla hesabı seçin (gönderme yüklemesi desteklenmez). Ileri ' ye tıklayın **: sanal makineler**.   

    ![Çoğaltma ekranındaki kaynak ayarları sekmesinin ekran görüntüsü Konuk kimlik bilgileri alanıyla vurgulanır.](./media/tutorial-migrate-physical-virtual-machines/source-settings.png)

7. **Sanal makinelerde** **geçiş ayarlarını bir değerlendirmede içeri aktar** bölümünde, varsayılan ayar Hayır olarak kalsın **, geçiş ayarlarını el ile belirteceğiz**.
8. Geçirmek istediğiniz her VM 'yi denetleyin. Ardından Ileri ' ye tıklayın **: hedef ayarlar**.

    ![VM 'Leri seçin](./media/tutorial-migrate-physical-virtual-machines/select-vms.png)


9. **Hedef ayarları**’nda aboneliği ve geçiş yapacağınız hedef bölgeyi seçin. Daha sonra Azure VM’lerinin geçişten sonra bulunacağı kaynak grubunu belirtin.
10. **Sanal Ağ**’da Azure VM’lerinin geçişten sonra katılacağı Azure sanal ağını/alt ağını seçin.
11. **Kullanılabilirlik seçenekleri**' nde şunları seçin:
    -  Bölge içindeki belirli bir kullanılabilirlik bölgesine geçirilen makineyi sabitlemek için kullanılabilirlik alanı. Kullanılabilirlik Alanları arasında çok düğümlü bir uygulama katmanı oluşturan sunucuları dağıtmak için bu seçeneği kullanın. Bu seçeneği belirlerseniz, Işlem sekmesinde seçilen makinenin her biri için kullanılacak kullanılabilirlik alanını belirtmeniz gerekir. Bu seçenek yalnızca geçiş için seçilen hedef bölge Kullanılabilirlik Alanları destekliyorsa kullanılabilir
    -  Geçirilen makinenin bir kullanılabilirlik kümesine yerleştirileceği kullanılabilirlik kümesi. Bu seçeneği kullanabilmek için seçilen hedef kaynak grubunun bir veya daha fazla kullanılabilirlik kümesi olmalıdır.
    - Geçirilen makineler için bu kullanılabilirlik yapılandırmalarının herhangi birine ihtiyacınız yoksa, altyapı artıklığı gerekli değildir.
12. **Azure Hibrit Avantajı**’nda:

    - Azure Hibrit Avantajı’nı uygulamak istemiyorsanız **Hayır**’ı seçin. Ardından **İleri**'ye tıklayın.
    - Etkin Yazılım Güvencesi veya Windows Server abonelikleri kapsamında olan Windows Server makineleriniz varsa ve avantajı geçirdiğiniz makinelere uygulamak istiyorsanız **Evet**’i seçin. Ardından **İleri**'ye tıklayın.

    ![Hedef ayarları](./media/tutorial-migrate-physical-virtual-machines/target-settings.png)

13. **İşlem** bölümünde VM adı, boyutu, işletim sistemi disk türü ve kullanılabilirlik yapılandırmasını (önceki adımda seçildiyse) gözden geçirin. VM’ler [Azure gereksinimleriyle](migrate-support-matrix-physical-migration.md#azure-vm-requirements)uyumlu olmalıdır.

    - **VM boyutu**: değerlendirme önerilerini KULLANıYORSANıZ, VM boyutu açılan listesi önerilen boyutu gösterir. Aksi takdirde Azure Geçişi, Azure aboneliğindeki en yakın eşleşmeye göre bir boyut seçer. Alternatif olarak **Azure VM boyutu**’nda el ile bir boyut seçin.
    - **Işletim sistemi diski**: VM için işletim sistemi (önyükleme) diskini belirtin. İşletim Sistemi diski, işletim sistemi önyükleyiciye ve yükleyiciye sahip disktir.
    - **Kullanılabilirlik alanı**: kullanılacak kullanılabilirlik bölgesini belirtin.
    - **Kullanılabilirlik kümesi**: kullanılacak kullanılabilirlik kümesini belirtin.

> [!NOTE]
> Bir sanal makine kümesi için farklı bir kullanılabilirlik seçeneği seçmek istiyorsanız 1. adıma gidin ve tek bir sanal makine kümesi için çoğaltma başlattıktan sonra farklı kullanılabilirlik seçeneklerini belirleyerek adımları yineleyin.

   ![İşlem ayarları](./media/tutorial-migrate-physical-virtual-machines/compute-settings.png)

13. **Diskler**' de, VM disklerinin Azure 'da çoğaltılıp çoğaltılmayacağını belirtin ve Azure 'da disk türünü (Standart SSD/HDD veya Premium yönetilen diskler) seçin. Ardından **İleri**'ye tıklayın.
    - Diskleri çoğaltmadan çıkarabilirsiniz.
    - Diskleri çıkarırsanız bu diskler geçişten sonra Azure VM’de bulunmaz. 

    ![Disk ayarları](./media/tutorial-migrate-physical-virtual-machines/disks.png)


14. **Çoğaltmayı gözden geçir ve başlat** bölümünde ayarları gözden geçirin ve sunuculara yönelik ilk çoğaltmayı başlatmak için **Çoğalt** üzerine tıklayın.

> [!NOTE]
> Çoğaltma ayarlarını, çoğaltma başlamadan önce dilediğiniz zaman güncelleştirebilirsiniz, **Manage**  >  **çoğaltılan makineleri** yönetin. Çoğaltma başladıktan sonra ayarlar değiştirilemez.



## <a name="track-and-monitor"></a>İzleme ve izleme

- **Çoğalt** ' a tıkladığınızda çoğaltma Başlat işi başlar. 
- Çoğaltma Başlat işi başarıyla tamamlandığında, makineler ilk çoğaltmasını Azure 'a başlatır.
- İlk çoğaltma tamamlandıktan sonra Delta çoğaltma başlar. Şirket içi disklerde artımlı değişiklikler düzenli aralıklarla Azure 'daki çoğaltma disklerine çoğaltılır.


Portal bildirimlerinde iş durumunu izleyebilirsiniz.

Çoğaltma durumunu, **Azure geçişi: sunucu geçişi**' nde **sunucuları** çoğaltma ' ya tıklayarak izleyebilirsiniz.
![Çoğaltmayı izleme](./media/tutorial-migrate-physical-virtual-machines/replicating-servers.png)

## <a name="run-a-test-migration"></a>Geçiş testi çalıştırma


Delta çoğaltma başladığında, Azure 'a tam geçiş çalıştırmadan önce VM 'Ler için bir test geçişi çalıştırabilirsiniz. Bunu geçirmeden önce her makine için en az bir kez yapmanızı öneririz.

- Bir test geçişi çalıştırmak, geçiş işleminin beklendiği gibi çalıştığını denetler ve bu işlem, çalışır durumda olan şirket içi makineleri etkilemeden, çoğaltmaya devam eder. 
- Test geçişi, çoğaltılan verileri kullanarak bir Azure VM oluşturarak geçişe benzetir (genellikle Azure aboneliğinizdeki bir üretim dışı VNet 'e geçiş yapar).
- Geçişi doğrulamak, uygulama testi gerçekleştirmek ve tam geçişten önce herhangi bir sorunu gidermek için çoğaltılan test Azure VM 'yi kullanabilirsiniz.

Test geçişini aşağıdaki şekilde yapın:


1. **Geçiş hedefleri**  >  **sunucuları**  >  **Azure geçişi: sunucu geçişi**' nde **geçirilen sunucuları test et**' e tıklayın.

     ![Geçirilen sunucuları test etme](./media/tutorial-migrate-physical-virtual-machines/test-migrated-servers.png)

2. Test edilecek VM’yi sağ tıklayıp **Test geçişi** üzerine tıklayın.

    ![Test geçişi](./media/tutorial-migrate-physical-virtual-machines/test-migrate.png)

3. **Test Geçişi** bölümünde, Azure VM’nin geçişten sonra bulunacağı Azure VNet’i seçin. Üretim dışı bir VNet kullanmanızı öneririz.
4. **Test Geçişi** işlemi başlar. Portal bildirimlerinde işi izleyin.
5. Geçiş bittikten sonra geçirilen Azure VM’yi, Azure portalında **Sanal Makineler** bölümünde görüntüleyin. Makine adında **Testi** şeklinde bir son ek vardır.
6. Test tamamlandıktan sonra **Makineleri çoğaltma** bölümünde Azure VM’yi sağ tıklayıp **Test geçişini temizle** üzerine tıklayın.

    ![Geçişi temizleme](./media/tutorial-migrate-physical-virtual-machines/clean-up.png)


## <a name="migrate-vms"></a>VM geçirme

Test geçişinin beklendiği gibi çalışıp çalışmadığını doğruladıktan sonra şirket içi makineleri geçirebilirsiniz.

1. Azure geçişi proje > **sunucuları**  >  **Azure geçişi: sunucu geçişi**' nde, **sunucuları çoğaltma**' ya tıklayın.

    ![Sunucuları çoğaltma](./media/tutorial-migrate-physical-virtual-machines/replicate-servers.png)

2. **Makineleri çoğaltma** bölümünde VM > **Geçir** üzerine sağ tıklayın.
3. **Migrate**  >  **Sanal makineleri Kapat ' a geçiş yapın ve veri kaybı olmadan planlı bir geçiş gerçekleştirin**, **Evet**  >  **Tamam**' ı seçin.
    - VM’yi kapatmak istemiyorsanız, **Hayır** seçeneğini belirleyin
    
    Note: fiziksel sunucu geçişi Için, uygulamayı geçiş penceresinin parçası olarak aşağı getirmek (uygulamaların herhangi bir bağlantıyı kabul etmesine izin vermez) ve ardından geçişi başlatmaları (sunucunun çalışır durumda tutulması gerekir, bu nedenle kalan değişiklikler eşitlenebilir) geçiş tamamlanmadan önce yapılır.

4. VM için bir geçiş işlemi başlar. Azure bildirimlerinde işlemi izleyin.
5. İşlem bittikten sonra **Sanal Makineler** sayfasında VM’yi görüntüleyebilir ve yönetebilirsiniz.

## <a name="complete-the-migration"></a>Geçişi tamamlamayı

1. Geçiş yapıldıktan sonra, **geçişi durdurmak**> VM 'ye sağ tıklayın. Bu, şunları yapar:
    - Şirket içi makine için çoğaltmayı sonlandırır.
    - Makineyi Azure geçişi: sunucu geçişi içindeki **çoğaltma sunucusu** sayısından kaldırır.
    - Makinenin çoğaltma durumu bilgilerini temizler.
2. Geçirilen makinelere Azure VM [Windows](../virtual-machines/extensions/agent-windows.md) veya [Linux](../virtual-machines/extensions/agent-linux.md) Aracısı 'nı yükler.
3. Veritabanı bağlantısı dizelerini ve web sunucusu yapılandırmalarını güncelleştirme gibi herhangi bir geçiş sonrası uygulama ayarı gerçekleştirin.
4. Geçirilen uygulamada son uygulama ve geçiş kabul testi gerçekleştirme işlemi şimdi Azure’da çalıştırılmaktadır.
5. Geçirilen Azure VM örneğine giden trafiği kesin.
6. Yerel sanal makine envanterinizden şirket içi sanal makineleri kaldırın.
7. Yerel yedeklemelerden şirket içi sanal makineleri kaldırın.
8. Azure sanal makinelerinin yeni konumunu ve IP adresini göstermek için herhangi bir iç belgeyi güncelleştirin. 

## <a name="post-migration-best-practices"></a>Geçiş sonrası en iyi uygulamalar

- Daha fazla esneklik için:
    - Azure Backup hizmetini kullanarak Azure sanal makinelerini yedekleyip verileri güvende tutun. [Daha fazla bilgi edinin](../backup/quick-backup-vm-portal.md).
    - Site Recovery ile Azure sanal makinelerini ikincil bölgeye çoğaltarak iş yüklerinin çalışmaya devam etmesini ve sürekli kullanılabilir olmasını sağlayın. [Daha fazla bilgi edinin](../site-recovery/azure-to-azure-tutorial-enable-replication.md).
- Daha fazla güvenlik için:
    - Azure Güvenlik Merkezi ile gelen trafik erişimini kilitleme ve sınırlayın [-tam zamanında yönetim](../security-center/security-center-just-in-time.md).
    - [Ağ Güvenlik Grupları](../virtual-network/network-security-groups-overview.md) ile ağ trafiğini yönetim uç noktaları ile kısıtlayın.
    - [Azure Disk Şifrelemesi](../security/fundamentals/azure-disk-encryption-vms-vmss.md)’ni dağıtarak disklerin güvenliğinin sağlanmasına yardımcı olun ve verileri hırsızlık ve yetkisiz erişime karşı koruyun.
    - [IaaS kaynaklarının güvenliğini sağlama](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/) hakkında daha fazla bilgi edinin ve [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)’ni ziyaret edin.
- İzleme ve yönetim için:
    - [Azure Maliyet Yönetimi](../cost-management-billing/cloudyn/overview.md)’ni dağıtarak kaynak kullanımını ve harcamayı izleyin.


## <a name="next-steps"></a>Sonraki adımlar

Azure bulut benimseme çerçevesindeki [bulut geçiş yolculuğunu](/azure/architecture/cloud-adoption/getting-started/migrate) araştırın.