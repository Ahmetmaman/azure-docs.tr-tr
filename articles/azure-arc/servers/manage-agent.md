---
title: Azure Arc etkin sunucular Aracısı 'nı yönetme
description: Bu makalede, Azure Arc etkin sunucular bağlı makine aracısının yaşam döngüsü boyunca genellikle gerçekleştirdiğiniz farklı yönetim görevleri açıklanır.
ms.date: 09/09/2020
ms.topic: conceptual
ms.openlocfilehash: af020d0ca586b950b444f2a3149ad207b5696050
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92108941"
---
# <a name="managing-and-maintaining-the-connected-machine-agent"></a>Bağlı makine aracısını yönetme ve sürdürme

Windows veya Linux için, Azure Arc etkinleştirilmiş sunuculara bağlı makine aracısının ilk dağıtımından sonra, aracıyı yeniden yapılandırmanız, yükseltmeniz veya yaşam döngüsünün kullanımdan kaldırılması aşamasına ulaştıysa bilgisayardan kaldırmanız gerekebilir. Bu rutin bakım görevlerini el ile veya Otomasyon aracılığıyla kolayca yönetebilirsiniz, bu da hem işlemsel hata hem de giderleri azaltır.

## <a name="before-uninstalling-agent"></a>Aracıyı kaldırmadan önce

Bağlı makine aracısını Arc etkin sunucunuzda kaldırmadan önce, Azure faturanızda beklenmeyen sorunları veya maliyetleri önlemek için aşağıdakileri göz önünde bulundurun:

* Azure VM uzantılarını etkin bir sunucuya dağıttıysanız ve bağlı makine aracısını kaldırırsanız veya kaynak grubundaki yay etkin sunucuyu temsil eden kaynağı silerseniz, bu uzantılar çalışmaya devam eder ve normal işlemlerini gerçekleştirir.

* Kaynak grubunuzda yay etkin sunucuyu temsil eden kaynağı silerseniz, ancak VM uzantılarını kaldırmazsanız, makineyi yeniden kaydettiğinizde, yüklü VM uzantılarını yönetemezsiniz.

Artık Azure Arc etkin sunucularıyla yönetmek istemediğiniz sunucular veya makineler için, yönetimi başarıyla durdurmak üzere bu adımları izlemeniz gerekir:

1. VM uzantılarını makineden veya sunucudan kaldırın. Adımlar aşağıda verilmiştir.

2. Aşağıdaki yöntemlerden birini kullanarak makinenin Azure Arc bağlantısını kesin:

    * `azcmagent disconnect`Makinede veya sunucuda komut çalıştırılıyor.

    * Azure portal, üst çubuktan **Sil** ' i seçerek seçili kayıtlı yay etkin sunucusundan.

    * [Azure CLI](../../azure-resource-manager/management/delete-resource-group.md?tabs=azure-cli#delete-resource) veya [Azure PowerShell](../../azure-resource-manager/management/delete-resource-group.md?tabs=azure-powershell#delete-resource)kullanma. Parametresi için `ResourceType` kullanın `Microsoft.HybridCompute/machines` .

3. Aracıyı makineden veya sunucudan kaldırın. Aşağıdaki adımları izleyin.

## <a name="upgrading-agent"></a>Aracı yükseltiliyor

Windows ve Linux için Azure bağlı makine Aracısı, gereksinimlerinize bağlı olarak, en son sürüme el ile veya otomatik olarak yükseltilebilir. Aşağıdaki tabloda, aracı yükseltmesini gerçekleştirmek için desteklenen yöntemler açıklanmıştır.

| İşletim sistemi | Yükseltme yöntemi |
|------------------|----------------|
| Windows | El ile<br> Windows Update |
| Ubuntu | [Apt](https://help.ubuntu.com/lts/serverguide/apt.html) |
| SUSE Linux Enterprise Server | [Zypper](https://en.opensuse.org/SDB:Zypper_usage_11.3) |
| RedHat Enterprise, Amazon, CentOS Linux | [yum](https://wiki.centos.org/PackageManagement/Yum) |

### <a name="windows-agent"></a>Windows aracısı

Windows için bağlı makine aracısının güncelleştirme paketi şuradan edinilebilir:

* Microsoft Update

* [Microsoft Update Kataloğu](https://www.catalog.update.microsoft.com/Home.aspx)

* Microsoft Indirme merkezi 'nden [Windows agent Windows Installer paketi](https://aka.ms/AzureConnectedMachineAgent) .

Aracı, yazılım güncelleştirme yönetimi işleminizi desteklemek üzere çeşitli yöntemler izleyerek yükseltilebilir. Microsoft Update 'tan alma dışında, komut Isteminden, komut Isteminden veya diğer otomasyon çözümünden veya yürüterek kullanıcı arabirimi sihirbazından el ile indirebilirsiniz ve çalıştırabilirsiniz `AzureConnectedMachine.msi` .

> [!NOTE]
> * Aracıyı yükseltmek için *yönetici* izinlerinizin olması gerekir.
> * El ile yükseltmek için, önce yükleyici paketini indirmeniz ve hedef sunucudaki bir klasöre veya paylaşılan bir ağ klasöründen kopyalamanız gerekir. 

Windows Installer paketlerine yönelik komut satırı seçeneklerini tanımıyorsanız, [msiexec standart komut satırı seçeneklerini](/windows/win32/msi/standard-installer-command-line-options) ve [msiexec komut satırı seçeneklerini](/windows/win32/msi/command-line-options)gözden geçirin.

#### <a name="to-upgrade-using-the-setup-wizard"></a>Kurulum Sihirbazı 'Nı kullanarak yükseltmek için

1. Yönetici haklarına sahip bir hesapla bilgisayarda oturum açın.

2. Kurulum sihirbazını başlatmak için **AzureConnectedMachineAgent.msi** yürütün.

Kurulum Sihirbazı önceki bir sürümün mevcut olup olmadığını bulur ve sonra otomatik olarak aracının yükseltmesini gerçekleştirir. Yükseltme tamamlandığında, Kurulum Sihirbazı otomatik olarak kapanır.

#### <a name="to-upgrade-from-the-command-line"></a>Komut satırından yükseltmek için

1. Yönetici haklarına sahip bir hesapla bilgisayarda oturum açın.

2. Aracıyı sessizce yükseltmek ve klasörde bir kurulum günlük dosyası oluşturmak için `C:\Support\Logs` aşağıdaki komutu çalıştırın.

    ```dos
    msiexec.exe /i AzureConnectedMachineAgent.msi /qn /l*v "C:\Support\Logs\Azcmagentupgradesetup.log"
    ```

### <a name="linux-agent"></a>Linux Aracısı

Bir Linux makinesindeki aracıyı en son sürüme güncelleştirmek için, iki komut içerir. Yerel paket dizinini depolardan en son kullanılabilir paketler listesiyle ve yerel paketi yükseltmek için bir komuttan güncelleştiren bir komut.

En son Aracı paketini Microsoft 'un [paket deposundan](https://packages.microsoft.com/)indirebilirsiniz.

> [!NOTE]
> Aracıyı yükseltmek için, *kök* erişim Izinleriniz veya sudo kullanılarak yükseltilmiş haklara sahip bir hesapla olması gerekir.

#### <a name="upgrade-ubuntu"></a>Ubuntu 'ı yükselt

1. Yerel paket dizinini depolarda yapılan en son değişikliklerle güncelleştirmek için aşağıdaki komutu çalıştırın:

    ```bash
    apt update
    ```

2. Sisteminizi yükseltmek için aşağıdaki komutu çalıştırın:

    ```bash
    apt upgrade
    ```

Paketlerin yüklenmesi ve kaldırılması gibi [apt](https://help.ubuntu.com/lts/serverguide/apt.html) komutunun eylemleri `/var/log/dpkg.log` günlük dosyasında günlüğe kaydedilir.

#### <a name="upgrade-red-hatcentosamazon-linux"></a>Red Hat/CentOS/Amazon Linux 'u yükselt

1. Yerel paket dizinini depolarda yapılan en son değişikliklerle güncelleştirmek için aşağıdaki komutu çalıştırın:

    ```bash
    yum check-update
    ```

2. Sisteminizi yükseltmek için aşağıdaki komutu çalıştırın:

    ```bash
    yum update
    ```

Paketlerin yüklenmesi ve kaldırılması gibi, [yıum](https://access.redhat.com/articles/yum-cheat-sheet) komutunun eylemleri `/var/log/yum.log` günlük dosyasında günlüğe kaydedilir. 

#### <a name="upgrade-suse-linux-enterprise"></a>SUSE Linux Enterprise 'ı yükseltme

1. Yerel paket dizinini depolarda yapılan en son değişikliklerle güncelleştirmek için aşağıdaki komutu çalıştırın:

    ```bash
    zypper refresh
    ```

2. Sisteminizi yükseltmek için aşağıdaki komutu çalıştırın:

    ```bash
    zypper update
    ```

Paketlerin yüklenmesi ve kaldırılması gibi [zypper](https://en.opensuse.org/Portal:Zypper) komutunun eylemleri `/var/log/zypper.log` günlük dosyasında günlüğe kaydedilir. 

## <a name="about-the-azcmagent-tool"></a>Azcmagent aracı hakkında

Azcmagent Aracı (Azcmagent.exe), yükleme sırasında Azure Arc etkin sunucular bağlı makine aracısını yapılandırmak veya yükleme sonrasında aracının başlangıç yapılandırmasını değiştirmek için kullanılır. Azcmagent.exe aracıyı özelleştirmek ve durumunu görüntülemek için komut satırı parametreleri sağlar:

* **Bağlan** -makineyi Azure yaya bağlamak için

* **Bağlantıyı kes** -makinenin Azure Arc bağlantısını kesmek için

* **Yeniden bağlan** -bağlantısı kesilen bir makineyi Azure yaya yeniden bağlamak için

* Aracıda bir sorunu gidermeye yardımcı olabilecek aracı durumunu ve yapılandırma özelliklerini (kaynak grubu adı, abonelik KIMLIĞI, sürüm vb. **) görüntüleyin.**

* **-h veya--help** -kullanılabilir komut satırı parametrelerini gösterir

    Örneğin, **reconnect** parametresinin ayrıntılı yardımını görmek için, yazın `azcmagent reconnect -h` . 

* **-v veya--verbose** -ayrıntılı günlüğü etkinleştir

Etkileşimli olarak oturum açtığınızda bir **bağlanma**, **bağlantısını kesme**ve el ile **yeniden bağlanma** gerçekleştirebilir veya birden çok aracı eklemek için kullandığınız hizmet sorumlusunu veya Microsoft Identity Platform [erişim belirtecini](../../active-directory/develop/access-tokens.md)kullanarak otomatik hale getirebilirsiniz. Makineyi Azure Arc etkin sunucularla kaydetmek için bir hizmet sorumlusu kullanmıyorsanız, hizmet sorumlusu oluşturmak için aşağıdaki [makaleye](onboard-service-principal.md#create-a-service-principal-for-onboarding-at-scale) bakın.

>[!NOTE]
>**Azcmagent**çalıştırmak için Linux makinelerde *kök* erişim izinlerine sahip olmanız gerekir.

### <a name="connect"></a>Bağlan

Bu parametre, makinenin Azure 'da oluşturulduğunu temsil eden Azure Resource Manager bir kaynak belirtir. Kaynak, belirtilen abonelikte ve kaynak grubunda bulunur ve makineyle ilgili veriler, bu ayar tarafından belirtilen Azure bölgesinde saklanır `--location` . Varsayılan kaynak adı, belirtilmemişse makinenin ana bilgisayar adıdır.

Makinenin sistem tarafından atanan kimliğine karşılık gelen bir sertifika indirilir ve yerel olarak depolanır. Bu adım tamamlandıktan sonra Azure bağlı makine Metadata Service ve konuk yapılandırma Aracısı, Azure Arc etkin sunucularıyla eşitlemeye başlar.

Hizmet sorumlusu kullanarak bağlanmak için aşağıdaki komutu çalıştırın:

`azcmagent connect --service-principal-id <serviceprincipalAppID> --service-principal-secret <serviceprincipalPassword> --tenant-id <tenantID> --subscription-id <subscriptionID> --resource-group <ResourceGroupName> --location <resourceLocation>`

Erişim belirteci kullanarak bağlanmak için aşağıdaki komutu çalıştırın:

`azcmagent connect --access-token <> --subscription-id <subscriptionID> --resource-group <ResourceGroupName> --location <resourceLocation>`

Yükseltilmiş oturum açma kimlik bilgilerinizle (etkileşimli) bağlanmak için aşağıdaki komutu çalıştırın:

`azcmagent connect --tenant-id <TenantID> --subscription-id <subscriptionID> --resource-group <ResourceGroupName> --location <resourceLocation>`

### <a name="disconnect"></a>Bağlantıyı kes

Bu parametre, Azure 'da makinenin silindiğini temsil eden Azure Resource Manager bir kaynak belirtir. Aracıyı makineden silmez, bu işlem ayrı bir adım olarak yapılmalıdır. Makinenin bağlantısı kesildikten sonra, Azure Arc etkin sunucularıyla yeniden kaydetmek istiyorsanız, `azcmagent connect` Azure 'da bunun için yeni bir kaynak oluşturulmasını kullanın.

> [!NOTE]
> Arc etkin sunucunuza bir veya daha fazla Azure VM Uzantısı dağıttıysanız ve kaydını Azure 'da sildiğinizde, uzantılar hala yüklüdür. Yüklü uzantıya bağlı olarak, kendi işlevini etkin bir şekilde gerçekleştiriyor olduğunu anlamak önemlidir. Devre dışı bırakılması veya artık Arc etkin sunucularla yönetilmemesi amaçlanan makineler, Azure 'dan kayıt kaldırılmadan önce uzantıları kaldırmalıdır.

Hizmet sorumlusu kullanarak bağlantıyı kesmek için aşağıdaki komutu çalıştırın:

`azcmagent disconnect --service-principal-id <serviceprincipalAppID> --service-principal-secret <serviceprincipalPassword> --tenant-id <tenantID>`

Erişim belirteci kullanarak bağlantıyı kesmek için aşağıdaki komutu çalıştırın:

`azcmagent disconnect --access-token <accessToken>`

Yükseltilmiş oturum açma kimlik bilgilerinizle (etkileşimli) bağlantıyı kesmek için aşağıdaki komutu çalıştırın:

`azcmagent disconnect --tenant-id <tenantID>`

### <a name="reconnect"></a>Yeniden bağlan

> [!WARNING]
> `reconnect`Komut kullanım dışıdır ve kullanılmamalıdır. Komut gelecekteki bir aracı sürümünde kaldırılacak ve mevcut aracılar yeniden bağlanma isteğini tamamlayamayacak. Bunun yerine makinenizin [bağlantısını kesip](#disconnect) yeniden [bağlayın](#connect) .

Bu parametre, zaten kayıtlı veya bağlı olan makinenin Azure Arc etkin sunucularıyla yeniden bağlanmasını ister. Bu, makinenin süresi en az 45 gün, sertifikanın süresi dolduğunda gerekli olabilir. Bu parametre, bu makineyi temsil eden Azure Resource Manager kaynağına karşılık gelen yeni kimlik bilgilerini almak için belirtilen kimlik doğrulama seçeneklerini kullanır.

Bu komut, [Azure bağlı makine ekleme](agent-overview.md#required-permissions) rolünden daha yüksek ayrıcalıklar gerektirir.

Hizmet sorumlusu kullanarak yeniden bağlanmak için aşağıdaki komutu çalıştırın:

`azcmagent reconnect --service-principal-id <serviceprincipalAppID> --service-principal-secret <serviceprincipalPassword> --tenant-id <tenantID>`

Erişim belirteci kullanarak yeniden bağlanmak için aşağıdaki komutu çalıştırın:

`azcmagent reconnect --access-token <accessToken>`

Yükseltilmiş oturum açma kimlik bilgilerinizle (etkileşimli) yeniden bağlanmak için aşağıdaki komutu çalıştırın:

`azcmagent reconnect --tenant-id <tenantID>`

## <a name="remove-the-agent"></a>Aracıyı Kaldırma

Windows veya Linux bağlı makine aracısını makineden kaldırmak için aşağıdaki yöntemlerden birini gerçekleştirin. Aracının kaldırılması, sanal makinenin Arc özellikli sunucularla kaydını kaldırmaz veya yüklü Azure VM uzantılarını kaldırmaz. Bu adımları, artık Azure 'da makineyi yönetmeniz gerekmiyorsa ayrı olarak gerçekleştirmeniz gerekir ve aracı kaldırılmadan önce bunların tamamlanması gerekir.

### <a name="windows-agent"></a>Windows aracısı

Aşağıdaki yöntemlerin her ikisi de aracıyı kaldırır, ancak makinede *C:\Program Files\AzureConnectedMachineAgent* klasörünü kaldırmaz.

#### <a name="uninstall-from-control-panel"></a>Denetim masasından kaldır

1. Windows aracısını makineden kaldırmak için aşağıdakileri yapın:

    a. Yönetici izinlerine sahip bir hesapla bilgisayarda oturum açın.  
    b. **Denetim Masası**'nda **Programlar ve Özellikler**' i seçin.  
    c. **Programlar ve Özellikler**' de **Azure bağlı makine Aracısı**' nı seçin, **Kaldır**' ı seçin ve **Evet**' i seçin.  

    >[!NOTE]
    > Aracı Kurulum sihirbazını **AzureConnectedMachineAgent.msi** yükleyicisi paketine çift tıklayarak da çalıştırabilirsiniz.

#### <a name="uninstall-from-the-command-line"></a>Komut satırından kaldır

Aracıyı komut Isteminden el ile kaldırmak veya betik gibi otomatikleştirilmiş bir yöntemi kullanmak için aşağıdaki örneği kullanabilirsiniz. İlk olarak, işletim sisteminden uygulama paketinin asıl tanımlayıcısı olan bir GUID olan ürün kodunu almanız gerekir. Kaldırma işlemi Msiexec.exe komut satırı kullanılarak gerçekleştirilir `msiexec /x {Product Code}` .

1. Kayıt defteri düzenleyicisini açın.

2. Kayıt defteri anahtarı altında `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Uninstall` ürün kodu GUID 'sini bulup kopyalayın.

3. Daha sonra aşağıdaki örnekleri kullanarak, msiexec 'i kullanarak aracıyı kaldırabilirsiniz:

   * Komut satırı türünden:

       ```dos
       msiexec.exe /x {product code GUID} /qn
       ```

   * PowerShell kullanarak aynı adımları gerçekleştirebilirsiniz:

       ```powershell
       Get-ChildItem -Path HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall | `
       Get-ItemProperty | `
       Where-Object {$_.DisplayName -eq "Azure Connected Machine Agent"} | `
       ForEach-Object {MsiExec.exe /x "$($_.PsChildName)" /qn}
       ```

### <a name="linux-agent"></a>Linux Aracısı

> [!NOTE]
> Aracıyı kaldırmak için, *kök* erişim Izinleriniz veya sudo kullanılarak yükseltilmiş haklara sahip bir hesapla olması gerekir.

Linux aracısını kaldırmak için, kullanılacak komut Linux işletim sistemine bağlıdır.

- Ubuntu için aşağıdaki komutu çalıştırın:

    ```bash
    sudo apt purge azcmagent
    ```

- RHEL, CentOS ve Amazon Linux için aşağıdaki komutu çalıştırın:

    ```bash
    sudo yum remove azcmagent
    ```

- SLES için aşağıdaki komutu çalıştırın:

    ```bash
    sudo zypper remove azcmagent
    ```

## <a name="unregister-machine"></a>Makinenin kaydını sil

Makinenin Azure 'da destekleyici hizmetler ile yönetilmesini durdurmayı planlıyorsanız, yayın etkin sunucularla makinenin kaydını silmek için aşağıdaki adımları gerçekleştirin. Bu adımları, bağlı makine aracısını makineden kaldırdıktan önce veya sonra yapabilirsiniz.

1. [Azure Portal](https://aka.ms/hybridmachineportal)giderek Azure Arc etkin sunucularını açın.

2. Listeden makineyi seçin, üç nokta (**...**) simgesini seçin ve **Sil**' i seçin.

## <a name="update-or-remove-proxy-settings"></a>Proxy ayarlarını Güncelleştir veya Kaldır

Aracıyı bir ara sunucu aracılığıyla hizmetle iletişim kuracak şekilde yapılandırmak veya dağıtımdan sonra bu yapılandırmayı kaldırmak için ya da bu görevi gerçekleştirmek için aşağıdaki yöntemlerden birini kullanın.

### <a name="windows"></a>Windows

Proxy sunucusu ortam değişkenini ayarlamak için aşağıdaki komutu çalıştırın:

```powershell
# If a proxy server is needed, execute these commands with the proxy URL and port.
[Environment]::SetEnvironmentVariable("https_proxy","http://{proxy-url}:{proxy-port}","Machine")
$env:https_proxy = [System.Environment]::GetEnvironmentVariable("https_proxy","Machine")
# For the changes to take effect, the agent service needs to be restarted after the proxy environment variable is set.
Restart-Service -Name himds
```

Aracıyı bir ara sunucu üzerinden iletişimi durduracak şekilde yapılandırmak için, proxy sunucusu çevresel değişkenini kaldırmak ve aracı hizmetini yeniden başlatmak üzere aşağıdaki komutu çalıştırın:

```powershell
[Environment]::SetEnvironmentVariable("https_proxy",$null,"Machine")
$env:https_proxy = [System.Environment]::GetEnvironmentVariable("https_proxy","Machine")
# For the changes to take effect, the agent service needs to be restarted after the proxy environment variable removed.
Restart-Service -Name himds
```

### <a name="linux"></a>Linux

Proxy sunucusunu ayarlamak için, aracı yükleme paketini indirdiğiniz dizinden aşağıdaki komutu çalıştırın:

```bash
# Reconfigure the connected machine agent and set the proxy server.
bash ~/Install_linux_azcmagent.sh --proxy "{proxy-url}:{proxy-port}"
```

Aracıyı bir ara sunucu üzerinden iletişimi durduracak şekilde yapılandırmak için, proxy yapılandırmasını kaldırmak üzere aşağıdaki komutu çalıştırın:

```bash
sudo azcmagent_proxy remove
```

## <a name="next-steps"></a>Sonraki adımlar

* Sorun giderme bilgileri, [bağlı makine Aracısı sorunlarını giderme kılavuzunda](troubleshoot-agent-onboard.md)bulunabilir.

* VM [Konuk yapılandırması](../../governance/policy/concepts/guest-configuration.md), makinenin beklenen Log Analytics çalışma alanına rapor olduğunu doğrulama, [VM 'lerle Azure izleyici](../../azure-monitor/insights/vminsights-enable-policy.md)ile izlemeyi etkinleştirme ve çok daha birçok şey için [Azure ilkesi](../../governance/policy/overview.md)'ni kullanarak makinenizi yönetmeyi öğrenin.

* [Log Analytics Aracısı](../../azure-monitor/platform/log-analytics-agent.md)hakkında daha fazla bilgi edinin. Windows ve Linux için Log Analytics Aracısı, işletim sistemi ve iş yükü izleme verilerini toplamak, Otomasyon Runbook 'larını veya Güncelleştirme Yönetimi gibi özellikleri kullanarak yönetmek ya da [Azure Güvenlik Merkezi](../../security-center/security-center-introduction.md)gibi diğer Azure hizmetlerini kullanmak istediğinizde gereklidir.