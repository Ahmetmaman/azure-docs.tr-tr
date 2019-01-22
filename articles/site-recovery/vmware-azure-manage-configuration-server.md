---
title: VMware ve fiziksel sunucuya Azure Site Recovery ile olağanüstü durum kurtarma için yapılandırma sunucusunu yönetme | Microsoft Docs
description: Bu makalede VMware vm'lerinin olağanüstü durum kurtarma için mevcut yapılandırma sunucusu ve fiziksel sunucuları Azure Site Recovery ile azure'a nasıl yönetileceği açıklanır.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: ramamill
ms.openlocfilehash: 81f775d8deccb9fb8b23e811a6ca89886576f55f
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54431648"
---
# <a name="manage-the-configuration-server-for-vmware-vm-disaster-recovery"></a>VMware VM'LERİNDE olağanüstü durum kurtarma için yapılandırma sunucusunu yönetme

Kullanırken bir şirket içi yapılandırma sunucusu ayarlama [Azure Site Recovery](site-recovery-overview.md) VMware Vm'lerini ve fiziksel sunucuları azure'a olağanüstü durum kurtarma için. Yapılandırma sunucusu arasındaki iletişimi düzenler şirket içi VMware ve Azure ve veri çoğaltma işlemlerini yönetir. Bu makalede dağıtıldıktan sonra yapılandırma sunucusunu yönetmek için ortak görevler özetlenir.

## <a name="access-configuration-server"></a>Erişimi yapılandırma sunucusu

Yapılandırma sunucusu gibi erişebilirsiniz:

* VM, dağıtıldığı ve başlangıç oturum **Azure Site Recovery Yapılandırma Yöneticisi** masaüstü kısayolu.
* Alternatif olarak, yapılandırma sunucusunu uzaktan https:// erişebilirsiniz*ConfigurationServerName*/:44315 /. Yönetici kimlik bilgilerinizle oturum açın.

## <a name="modify-vmware-server-settings"></a>VMware sunucu ayarlarını değiştirin

1. Sonra farklı bir VMware sunucusunu yapılandırma sunucusu ile ilişkilendirmek için [oturum](#access-configuration-server)seçin **vCenter sunucusu/vSphere ESXi Sunucusu Ekle**.
2. Ayrıntıları girin ve ardından **Tamam**.

## <a name="modify-credentials-for-automatic-discovery"></a>Otomatik bulma için kimlik bilgilerini değiştirme

1. Sonra VMware vm'lerinin otomatik bulma için VMware sunucusuna bağlanmak için kullanılan kimlik bilgilerini güncelleştirmek için [oturum](#access-configuration-server), hesabı seçin ve tıklayın **Düzenle**.
2. Yeni kimlik bilgilerini girin ve ardından **Tamam**.

    ![VMware değiştirme](./media/vmware-azure-manage-configuration-server/modify-vmware-server.png)

CSPSConfigtool.exe aracılığıyla kimlik bilgilerini de değiştirebilirsiniz.

1. Cspsconfigtool.exe'yi başlatın ve yapılandırma sunucusu için oturum açma
2. ' A tıklayın ve değiştirmek istediğiniz hesabı seçin **Düzenle**.
3. Değiştirilen kimlik bilgilerini girin ve tıklayın **Tamam**

## <a name="modify-credentials-for-mobility-service-installation"></a>Mobility hizmeti yüklemesi için kimlik bilgilerini değiştirme

VMware Vm'leri için çoğaltmayı etkinleştirme Mobility hizmetini otomatik olarak yüklemek için kullanılan kimlik bilgilerini değiştirin.

1. Sonra [oturum](#access-configuration-server)seçin **sanal makine kimlik bilgilerini yönetme**
2. ' A tıklayın ve değiştirmek istediğiniz hesabı seçin **Düzenle**
3. Yeni kimlik bilgilerini girin ve ardından **Tamam**.

    ![Mobility hizmet kimlik bilgilerini değiştirme](./media/vmware-azure-manage-configuration-server/modify-mobility-credentials.png)

CSPSConfigtool.exe üzerinden kimlik bilgileri de değiştirebilirsiniz.

1. Cspsconfigtool.exe'yi başlatın ve yapılandırma sunucusu için oturum açma
2. ' A tıklayın ve değiştirmek istediğiniz hesabı seçin **Düzenle**
3. Yeni kimlik bilgilerini girin ve tıklayın **Tamam**.

## <a name="add-credentials-for-mobility-service-installation"></a>Mobility hizmeti yüklemesi için kimlik bilgilerini ekleyin

Yapılandırma sunucusu, OVF dağıtımı sırasında kimlik bilgilerini ekleyerek kaçırdıysanız

1. Sonra [oturum](#access-configuration-server)seçin **sanal makine kimlik bilgilerini yönetme**.
2. Tıklayarak **sanal makine kimlik bilgileri ekleme**.
    ![Ekle-mobility-credentials](media/vmware-azure-manage-configuration-server/add-mobility-credentials.png)
3. Yeni kimlik bilgilerini girin ve tıklayın **Ekle**.

CSPSConfigtool.exe üzerinden kimlik bilgileri de ekleyebilirsiniz.

1. Cspsconfigtool.exe'yi başlatın ve yapılandırma sunucusu için oturum açma
2. Tıklayın **Ekle**, yeni kimlik bilgilerini girin ve tıklayın **Tamam**.

## <a name="modify-proxy-settings"></a>Proxy ayarlarını değiştirme

Azure'da internet erişimi için yapılandırma sunucusu makine tarafından kullanılan proxy ayarları değiştirin. Varsayılan işlem sunucusunun yapılandırma sunucusu makinesinde çalışan yanı sıra işlem sunucunuz varsa, her iki makinede ayarlarını değiştirin.

1. Sonra [oturum](#access-configuration-server) yapılandırma sunucusuna seçin **bağlantıları Yönet**.
2. Proxy değerlerini güncelleştirin. Ardından **Kaydet** ayarları güncelleştirilemiyor.

## <a name="add-a-network-adapter"></a>Bir ağ bağdaştırıcısı ekleyin

Open Virtualization Format (OVF) şablonu, tek bir ağ bağdaştırıcısı ile VM yapılandırma sunucusu dağıtır.

- Yapabilecekleriniz [VM'ye ek bağdaştırıcı ekleme](vmware-azure-deploy-configuration-server.md#add-an-additional-adapter), ancak yapılandırma sunucusunu kasaya kaydetmeden önce eklemeniz gerekir.
- Yapılandırma sunucusunu kasaya kaydetmek sonra bir bağdaştırıcı eklemek için VM özelliklerinde bağdaştırıcı ekleyin. Ayarını yapmanız gerekir [yeniden kaydettirin](#reregister-a-configuration-server-in-the-same-vault) kasadaki sunucusu.


## <a name="reregister-a-configuration-server-in-the-same-vault"></a>Bir yapılandırma sunucusunda aynı kasaya yeniden kaydettirin

Gerekirse yapılandırma sunucusunu aynı kasaya yeniden kaydettirin. Varsayılan işlem sunucusunun yapılandırma sunucusu makinesinde çalışan yanı sıra bir ek işlem sunucusu makinesi varsa, her iki makine yeniden kaydettirin.


  1. Kasası'nda açın **Yönet** > **Site Recovery altyapısı** > **Configuration Servers**.
  2. İçinde **sunucuları**seçin **indirme kayıt anahtarı** kasa kimlik bilgileri dosyası indirilemedi.
  3. Configuration server makinesinde oturum açın.
  4. İçinde **%ProgramData%\ASR\home\svsystems\bin**açın **cspsconfigtool.exe**.
  5. Üzerinde **kasa kaydı** sekmesinde **Gözat**, indirdiğiniz kasa kimlik bilgileri dosyası bulun.
  6. Gerekiyorsa, proxy sunucusu ayrıntıları sağlayın. Ardından **Kaydet**’i seçin.
  7. Bir yönetici PowerShell komut penceresi açın ve aşağıdaki komutu çalıştırın:
   ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
   ```

      >[!NOTE]
      >Şunları **son sertifikaları çekme** genişleme işlem sunucusu yapılandırma sunucusundan komutu yürütün *"< yükleme Drive\Microsoft Azure Site Recovery\agent\cdpcli.exe >"--registermt'yi*

  8. Son olarak, aşağıdaki komutu yürüterek obengine yeniden başlatın.
  ```
          net stop obengine
          net start obengine
   ```


## <a name="register-a-configuration-server-with-a-different-vault"></a>Yapılandırma sunucusunu farklı bir kasaya kaydetme

> [!WARNING]
> Geçerli kasa yapılandırma sunucusundan aşağıdaki adımı ayırır ve yapılandırma sunucusu altında tüm korumalı sanal makinelerin çoğaltma durdurulur.

1. Yapılandırma sunucusuna oturum açın.
2. Bir yönetici PowerShell komut penceresi açın ve aşağıdaki komutu çalıştırın:

    ```
    reg delete "HKLM\Software\Microsoft\Azure Site Recovery\Registration"
    net stop dra
    ```
3. Masaüstü kısayolunu kullanarak yapılandırma sunucusu Gereci tarayıcı portalını başlatın.
4. Yeni bir yapılandırma sunucusu için benzer kayıt adımları [kayıt](vmware-azure-tutorial.md#register-the-configuration-server).

## <a name="upgrade-the-configuration-server"></a>Yapılandırma sunucusunu yükseltme

Yapılandırma sunucusunu güncelleştirmek için güncelleştirme paketleri çalıştırdığınız. Güncelleştirmeleri kadar N-4 sürümleri için uygulanabilir. Örneğin:

- 9.7, 9.8, 9.9 veya 9.10 çalıştırırsanız, doğrudan 9.11 için yükseltebilirsiniz.
- 9.6 veya önceki bir sürümünü çalıştırın ve 9.11 için yükseltmek istiyorsanız 9.7 sürümüne yükseltmeniz gerekir. 9.11 önce.

Güncelleştirme paketleri yapılandırma sunucusundaki tüm sürümlerine yükseltme için bağlantılar kullanılabilir [Azure güncelleştirmeleri sayfası](https://azure.microsoft.com/updates/?product=site-recovery).

> [!IMPORTANT]
> Her yeni sürümü ile yayınlanan, bir Azure Site Recovery bileşeni 'N-4 altındaki tüm sürümleri 'N'' destek kapsamı dışında olarak kabul edilir. Her zaman kullanılabilir en son sürümlerine yükseltmek için önerilir.

Sunucuyu aşağıdaki gibi yükseltin:

1. Kasaya Git **Yönet** > **Site Recovery altyapısı** > **Configuration Servers**.
2. Bir güncelleştirme varsa, bir bağlantı görünür **aracı sürümü** > sütun.
    ![Güncelleştirme](./media/vmware-azure-manage-configuration-server/update2.png)
3. Güncelleştirme yükleyicisi dosya yapılandırma sunucusuna indirin.

    ![Güncelleştirme](./media/vmware-azure-manage-configuration-server/update1.png)

4. Yükleyiciyi çalıştırmak için çift tıklayın.
5. Yükleyici, makine üzerinde çalışan geçerli sürümünü algılar. Tıklayın **Evet** yükseltmeyi başlatmak için.
6. Yükseltme tamamlandığında sunucu yapılandırmasını doğrular.

    ![Güncelleştirme](./media/vmware-azure-manage-configuration-server/update3.png)

7. Tıklayın **son** yükleyici kapatın.

## <a name="delete-or-unregister-a-configuration-server"></a>Silme veya kaydını iptal yapılandırma sunucusu

1. [Korumayı devre dışı](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) yapılandırma sunucusu altında yer alan tüm VM'ler için.
2. [İlişkisini](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) ve [Sil](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) yapılandırma sunucusundan tüm çoğaltma ilkeleri.
3. [Silme](vmware-azure-manage-vcenter.md#delete-a-vcenter-server) yapılandırma sunucusuyla ilişkili tüm vCenter sunucuları/vSphere konakları.
4. Kasası'nda açın **Site Recovery altyapısı** > **Configuration Servers**.
5. Kaldırmak istediğiniz yapılandırma sunucusunu seçin. Ardından **ayrıntıları** sayfasında **Sil**.

    ![Yapılandırma sunucusunu silme](./media/vmware-azure-manage-configuration-server/delete-configuration-server.png)


### <a name="delete-with-powershell"></a>PowerShell ile silme

İsteğe bağlı olarak, PowerShell kullanarak yapılandırma sunucusunu silebilirsiniz.

1. [Yükleme](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps?view=azurermps-4.4.0) Azure PowerShell modülü.
2. Bu komutu kullanarak Azure hesabınızda oturum açın:

    `Connect-AzureRmAccount`
3. Kasa aboneliğini seçin.

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Kasa bağlamını ayarlayın.

    ```
    $vault = Get-AzureRmRecoveryServicesVault -Name <name of your vault>
    Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault
    ```
4. Yapılandırma sunucusunu alır.

    `$fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. Yapılandırma sunucusu silme.

    `Remove-AzureRmSiteRecoveryFabric -Fabric $fabric [-Force] `

> [!NOTE]
> Kullanabileceğiniz **-Force** AzureRmSiteRecoveryFabric Kaldır seçeneği için yapılandırma sunucusunu zorla silme işlemi.

## <a name="generate-configuration-server-passphrase"></a>Yapılandırma sunucusu parolası oluşturma

1. Yapılandırma sunucunuzda oturum açın ve ardından yönetici olarak bir komut istemi penceresi açın.
2. Bin klasörüne erişmeye dizini değiştirmek için komutu yürütün **cd %ProgramData%\ASR\home\svsystems\bin**
3. Parola dosyası oluşturmak için yürütme **genpassphrase.exe - v > MobSvc.passphrase**.
4. Parolanız konumundaki dosyasında depolanan **%ProgramData%\ASR\home\svsystems\bin\MobSvc.passphrase**.

## <a name="renew-ssl-certificates"></a>SSL sertifikalarını yenileme

Mobility hizmeti, işlem sunucusu ve ona bağlı olan ana hedef sunucusu etkinliklerini düzenleyen bir yerleşik web sunucusuna yapılandırma sunucusu vardır. Web sunucusu istemcilerin kimliğini doğrulamak için bir SSL sertifikası kullanır. Sertifika üç yıl sonra süresi dolar ve herhangi bir zamanda yenilenebilir.

### <a name="check-expiry"></a>Süre sonu denetleyin

Mayıs 2016 önce yapılandırma sunucusu dağıtımları için sertifika süre sonu bir yıla ayarlanır. Sona erecek bir sertifika varsa, aşağıdakiler gerçekleşir:

- Ne zaman sona erme tarihi olan iki ay veya daha az (Site Recovery bildirimlerine abone olursa) portalında ve e-posta bildirimleri gönderme hizmetini başlatır.
- Kasa kaynak sayfasında bir bildirim başlığı görüntülenir. Daha fazla bilgi için başlığı seçin.
- Görürseniz bir **Şimdi Yükselt** düğmesini gösteren 9.4.xxxx.x veya üzeri sürümler için bazı bileşenler, ortamınızdaki yükseltilmemiş. Sertifikayı yenilemek önce bileşenleri yükseltin. Eski sürümlerine yeniden yenilenemiyor.

### <a name="renew-the-certificate"></a>Sertifikayı Yenile

1. Kasası'nda açın **Site Recovery altyapısı** > **yapılandırma sunucusu**. Gerekli yapılandırma sunucusunu seçin.
2. Sona erme tarihi altında görünür **Configuration Server sistem durumu**.
3. Seçin **sertifikalarını yenilemesi**.

## <a name="refresh-configuration-server"></a>Yapılandırma sunucusunu Yenile

1. Azure portalında gidin **kurtarma Hizmetleri kasası** > **Yönet** > **Site Recovery altyapısı**  >   **VMware ve fiziksel makineler için** > **yapılandırma sunucusu**
2. Yenilemek istediğiniz yapılandırma sunucusuna tıklayın.
3. Seçtiğiniz yapılandırma sunucusu ayrıntıları ile dikey penceresinde tıklayın **daha fazla** > **sunucusunu Yenile eylemi**.
4. Altında işinin ilerleme durumunu izlemek **kurtarma Hizmetleri kasası** > **izleme** > **Site Recovery işleri**.

## <a name="update-windows-license"></a>Windows lisansı güncelleştirme

OVF şablonu ile sağlanan lisans 180 gün boyunca geçerli bir değerlendirme lisanstır. Kesintisiz olarak kullanım için Windows ile tedarik edilen lisans etkinleştirmeniz gerekir.

## <a name="failback-requirements"></a>Yeniden çalışma gereksinimleri

Yeniden koruma ve yeniden çalışma sırasında şirket içi yapılandırma sunucusunun çalıştığından ve bağlı durumda olması gerekir. Başarıyla yeniden çalışma için geri başarısız sanal makine yapılandırma sunucusu veritabanında mevcut olmalıdır.

Normal zamanlanmış yedeklemeleri yapılandırma sunucunuzun olması emin olun. Bir olağanüstü durum oluşursa ve yapılandırma sunucusu kaybolur, öncelikle yapılandırma sunucusunu bir yedek kopyadan geri yükleyin ve geri yüklenen yapılandırma sunucusu ile kasaya kaydedilen aynı IP adresi olduğundan emin olun. Yeniden çalışma için geri yüklenen yapılandırma sunucusunu farklı bir IP adresi kullanıldığında çalışmaz.

## <a name="next-steps"></a>Sonraki adımlar

Olağanüstü durum kurtarma işlemini ayarlama öğreticileri gözden [VMware Vm'lerini](vmware-azure-tutorial.md) azure'a.
