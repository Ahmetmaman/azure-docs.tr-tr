---
title: Karma makineleri Azure portal Azure 'a bağlama
description: Bu makalede, Azure portal Azure Arc etkin sunucularını kullanarak aracıyı yüklemeyi ve makineleri Azure 'a bağlamayı öğreneceksiniz.
ms.date: 10/21/2020
ms.topic: conceptual
ms.openlocfilehash: 8769a3b76172bc6508b7c52eda359695c01eaa4b
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92370160"
---
# <a name="connect-hybrid-machines-to-azure-from-the-azure-portal"></a>Karma makineleri Azure portal Azure 'a bağlama

El ile bir adım kümesi gerçekleştirerek ortamınızdaki bir veya az sayıda Windows veya Linux makinesi için Azure Arc etkin sunucularını etkinleştirebilirsiniz. Alternatif olarak, sağladığımız bir şablon betiği çalıştırarak otomatikleştirilmiş bir yöntemi kullanabilirsiniz. Bu betik, her iki aracısının de indirilmesini ve yüklenmesini otomatikleştirir.

Bu yöntem, aracıyı yüklemek ve yapılandırmak için makinede yönetici izinlerine sahip olmanızı gerektirir. Linux 'ta, kök hesabı kullanarak ve Windows 'ta, yerel Yöneticiler grubunun üyesi olursunuz.

Başlamadan önce, [önkoşulları](agent-overview.md#prerequisites) gözden geçirdiğinizden ve aboneliğinizin ve kaynaklarınızın gereksinimleri karşıladığından emin olun. Desteklenen bölgeler ve diğer ilgili konular hakkında daha fazla bilgi için bkz. [desteklenen Azure bölgeleri](overview.md#supported-regions).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="generate-the-installation-script-from-the-azure-portal"></a>Azure portal yükleme betiğini oluşturma

İndirme ve yüklemeyi otomatik hale getirmeye ve Azure Arc ile bağlantı kurmaya yönelik betik, Azure portal kullanılabilir. İşlemi gerçekleştirmek için aşağıdakileri yapın:

1. Tarayıcınızdan [Azure Portal](https://portal.azure.com)gidin.

1. **Sunucular-Azure yay** sayfasında sol üst köşedeki **Ekle** ' yi seçin.

1. **Bir yöntem seçin** sayfasında, **etkileşimli betiği kullanarak sunucu Ekle** ' yi seçin ve ardından **betik oluştur**' u seçin.

1. **Betik oluştur** sayfasında, makinenin Azure 'da yönetilmesini istediğiniz aboneliği ve kaynak grubunu seçin. Makine meta verilerinin depolanacağı Azure konumunu seçin. Bu konum, kaynak grubunun konumuyla aynı veya farklı olabilir.

1. **Önkoşullar** sayfasında, bilgileri gözden geçirin ve ardından **İleri: kaynak ayrıntıları**' nı seçin.

1. **Kaynak ayrıntıları** sayfasında, aşağıdakileri sağlayın:

    1. **Kaynak grubu** açılan listesinde, makinenin yönetilecek kaynak grubunu seçin.
    1. **Bölge** açılan listesinde, sunucu meta verilerini depolamak için Azure bölgesini seçin.
    1. **İşletim sistemi** açılan listesinde, betiğin üzerinde çalışmak üzere yapılandırıldığı işletim sistemini seçin.
    1. Makine Internet 'e bağlanmak için bir proxy sunucusu üzerinden iletişim kurduklarında, proxy sunucusu IP adresini veya makinenin proxy sunucusuyla iletişim kurmak için kullanacağı adı ve bağlantı noktası numarasını belirtin. Değeri biçiminde girin `http://<proxyURL>:<proxyport>` .
    1. **Sonraki: Etiketler**' i seçin.

1. **Etiketler** sayfasında, önerilen varsayılan **fiziksel konum etiketlerini** gözden geçirin ve bir değer girin veya standartlarınızı desteklemek için bir veya daha fazla **özel etiket** belirtin.

1. **İleri ' yi seçin: betiği indir ve Çalıştır**.

1. **Betiği indir ve Çalıştır** sayfasında, Özet bilgilerini gözden geçirin ve ardından **İndir**' i seçin. Hala değişiklik yapmanız gerekiyorsa, **önceki**' yi seçin.

## <a name="install-and-validate-the-agent-on-windows"></a>Windows 'da aracıyı yükleyip doğrulama

### <a name="install-manually"></a>El ile yükleme

Bağlı makine aracısını *AzureConnectedMachineAgent.msi*Windows Installer paketini çalıştırarak el ile yükleyebilirsiniz. [Windows agent Windows Installer paketinin](https://aka.ms/AzureConnectedMachineAgent) en son sürümünü Microsoft İndirme Merkezi ' nden indirebilirsiniz.

>[!NOTE]
>* Aracıyı yüklemek veya kaldırmak için *yönetici* izinlerinizin olması gerekir.
>* Önce yükleyici paketini indirmeniz ve hedef sunucudaki bir klasöre veya paylaşılan bir ağ klasöründen kopyalamanız gerekir. Yükleyici paketini herhangi bir seçenek olmadan çalıştırırsanız, aracı etkileşimli olarak yüklemek için izleyebileceğiniz bir Kurulum Sihirbazı başlatılır.

Makinenin bir ara sunucu üzerinden hizmete iletişim kurması gerekiyorsa, aracıyı yükledikten sonra aşağıdaki adımlarda açıklanan bir komut çalıştırmanız gerekir. Bu komut, proxy sunucu sistemi ortam değişkenini ayarlar `https_proxy` .

Windows Installer paketlerine yönelik komut satırı seçeneklerini tanımıyorsanız, [msiexec standart komut satırı seçeneklerini](/windows/win32/msi/standard-installer-command-line-options) ve [msiexec komut satırı seçeneklerini](/windows/win32/msi/command-line-options)gözden geçirin.

Örneğin, `/?` Yardım ve hızlı başvuru seçeneğini gözden geçirmek için yükleme programını parametresiyle çalıştırın.

```dos
msiexec.exe /i AzureConnectedMachineAgent.msi /?
```

1. Aracı sessizce yüklemek ve var olan klasörde bir kurulum günlük dosyası oluşturmak için `C:\Support\Logs` aşağıdaki komutu çalıştırın.

    ```dos
    msiexec.exe /i AzureConnectedMachineAgent.msi /qn /l*v "C:\Support\Logs\Azcmagentsetup.log"
    ```

    Kurulum tamamlandıktan sonra aracı başlatılamazsa, ayrıntılı hata bilgileri için günlüklere bakın. Günlük dizini *%ProgramData%\AzureConnectedMachineAgent\log*' dir.

2. Makinenin bir ara sunucu üzerinden iletişim kurması gerekiyorsa, proxy sunucusu ortam değişkenini ayarlamak için aşağıdaki komutu çalıştırın:

    ```powershell
    [Environment]::SetEnvironmentVariable("https_proxy", "http://{proxy-url}:{proxy-port}", "Machine")
    $env:https_proxy = [System.Environment]::GetEnvironmentVariable("https_proxy","Machine")
    # For the changes to take effect, the agent service needs to be restarted after the proxy environment variable is set.
    Restart-Service -Name himds
    ```

    >[!NOTE]
    >Aracı bu önizlemede proxy kimlik doğrulamasını ayarlamayı desteklemiyor.
    >

3. Aracıyı yükledikten sonra, aşağıdaki komutu çalıştırarak Azure Arc hizmetiyle iletişim kurmasını sağlayacak şekilde yapılandırmanız gerekir:

    ```dos
    "%ProgramFiles%\AzureConnectedMachineAgent\azcmagent.exe" connect --resource-group "resourceGroupName" --tenant-id "tenantID" --location "regionName" --subscription-id "subscriptionID"
    ```

### <a name="install-with-the-scripted-method"></a>Komut dosyalı yöntemle Install

1. Sunucusunda oturum açın.

1. Yükseltilmiş bir PowerShell komut istemi açın.

    >[!NOTE]
    >Betik yalnızca Windows PowerShell 'in 64 bitlik bir sürümünden çalışmayı destekler.
    >

1. Betiği kopyaladığınız klasöre veya paylaşıma değiştirin ve betiği çalıştırarak sunucuda yürütün `./OnboardingScript.ps1` .

Kurulum tamamlandıktan sonra aracı başlatılamazsa, ayrıntılı hata bilgileri için günlüklere bakın. Günlük dizini *%ProgramData%\AzureConnectedMachineAgent\log*' dir.

## <a name="install-and-validate-the-agent-on-linux"></a>Linux 'ta aracıyı yükleyip doğrulama

Linux için bağlı makine Aracısı, dağıtım için tercih edilen paket biçiminde sağlanır (. RPM veya. DEB) Microsoft [paket deposunda](https://packages.microsoft.com/)barındırılır. [Kabuk betik paketi `Install_linux_azcmagent.sh` ](https://aka.ms/azcmagent) aşağıdaki eylemleri gerçekleştirir:

- Packages.microsoft.com adresinden Aracı paketini indirmek için konak makineyi yapılandırır.
- Karma kaynak sağlayıcısı paketini kurar.
- Makineyi Azure Arc 'a kaydeder

İsteğe bağlı olarak, parametresini dahil ederek aracıyı proxy bilgileriniz ile yapılandırabilirsiniz `--proxy "{proxy-url}:{proxy-port}"` .

Betik Ayrıca desteklenen ve desteklenmeyen dağıtımları tanımlayan mantığı içerir ve yüklemeyi gerçekleştirmek için gereken izinleri doğrular.

Aşağıdaki örnek aracıyı indirir ve yükler:

```bash
# Download the installation package.
wget https://aka.ms/azcmagent -O ~/Install_linux_azcmagent.sh

# Install the connected machine agent.
bash ~/Install_linux_azcmagent.sh
```

Aracıyı, `--proxy` proxy sunucunuz üzerinden iletişim kurmak üzere yapılandırma parametresi dahil olmak üzere indirmek ve yüklemek için aşağıdaki komutları çalıştırın:

```bash
# Download the installation package.
wget https://aka.ms/azcmagent -O ~/Install_linux_azcmagent.sh

# Install the connected machine agent. 
bash ~/Install_linux_azcmagent.sh --proxy "{proxy-url}:{proxy-port}"
```

## <a name="verify-the-connection-with-azure-arc"></a>Azure Arc ile bağlantıyı doğrulama

Aracıyı yükledikten ve Azure Arc etkin sunucularına bağlanacak şekilde yapılandırdıktan sonra, sunucunun başarıyla bağlandığını doğrulamak için Azure portal gidin. [Azure portalında](https://aka.ms/hybridmachineportal) makinelerinizi görüntüleyin.

![Başarılı bir sunucu bağlantısı](./media/onboard-portal/arc-for-servers-successful-onboard.png)

## <a name="next-steps"></a>Sonraki adımlar

* Sorun giderme bilgileri, [bağlı makine Aracısı sorunlarını giderme kılavuzunda](troubleshoot-agent-onboard.md)bulunabilir.

* VM [Konuk yapılandırması](../../governance/policy/concepts/guest-configuration.md), makinenin beklenen Log Analytics çalışma alanına rapor olduğunu doğrulama, [VM 'lerle Azure izleyici](../../azure-monitor/insights/vminsights-enable-policy.md)ile izlemeyi etkinleştirme ve çok daha birçok şey için [Azure ilkesi](../../governance/policy/overview.md)'ni kullanarak makinenizi yönetmeyi öğrenin.

* [Log Analytics Aracısı](../../azure-monitor/platform/log-analytics-agent.md)hakkında daha fazla bilgi edinin. Windows ve Linux için Log Analytics Aracısı, işletim sistemi ve iş yükü izleme verilerini toplamak, Otomasyon Runbook 'larını veya Güncelleştirme Yönetimi gibi özellikleri kullanarak yönetmek ya da [Azure Güvenlik Merkezi](../../security-center/security-center-introduction.md)gibi diğer Azure hizmetlerini kullanmak istediğinizde gereklidir.