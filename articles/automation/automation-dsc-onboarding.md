---
title: Makineleri Azure Otomasyon durum yapılandırması tarafından Yönetim için hazırlama
description: Azure Otomasyonu durum yapılandırması ile yönetim için makineleri ayarlama
services: automation
ms.service: automation
ms.subservice: dsc
author: bobbytreed
ms.author: robreed
ms.topic: conceptual
ms.date: 08/08/2018
manager: carmonm
ms.openlocfilehash: 1a3cfb51cc75c89c5a4580b1b7721eb763078980
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2019
ms.locfileid: "55096713"
---
# <a name="onboarding-machines-for-management-by-azure-automation-state-configuration"></a>Makineleri Azure Otomasyon durum yapılandırması tarafından Yönetim için hazırlama

## <a name="why-manage-machines-with-azure-automation-state-configuration"></a>Neden Azure Otomasyon durum yapılandırması ile makineleri yönetebilir?

Gibi [PowerShell Desired State Configuration](/powershell/dsc/overview), tüm Bulut veya şirket içi veri merkezinde Azure Otomasyon durum yapılandırması, DSC düğümleri (fiziksel ve sanal makineler) için bir basit ama güçlü, yapılandırma yönetimi hizmetidir . Ancak ölçeklenebilirlik binlerce makine arasında hızlı ve kolay bir şekilde Merkezi, güvenli bir konumdan etkinleştirir. Kolayca makine olabilir, bunları bildirim temelli yapılandırmalar ve raporları görüntüleme gösteren her makine Ata belirttiğiniz istenen duruma uyumluluk kullanıcının. Azure Otomasyonu durum yapılandırması yönetimi için DSC PowerShell komut dosyası için bir Azure Otomasyonu yönetim katmanı nedir katmanıdır. Diğer bir deyişle, Azure Otomasyonu PowerShell betikleri yönetmenize yardımcı olan aynı şekilde, ayrıca DSC yapılandırmaları yönetmenize yardımcı olur. Azure Otomasyonu durum yapılandırması kullanmanın avantajları hakkında daha fazla bilgi için bkz: [Azure Otomasyon durum yapılandırmasına genel bakış](automation-dsc-overview.md).

Azure Otomasyonu durum yapılandırması, çeşitli makineleri yönetmek için kullanılabilir:

- Azure sanal makineler (Klasik ve Azure Resource Manager dağıtım modeli dağıtılır)
- Amazon Web Services (AWS) EC2 örnekleri
- Fiziksel/sanal Windows makineleri şirket içinde veya Azure/AWS dışındaki bir bulutta bulunan
- Şirket içi, azure'da veya Azure dışındaki bir bulutta bulunan fiziksel/sanal Linux makineleri

Ayrıca, makine yapılandırması buluttan yönetmek hazır değilseniz, Azure Otomasyonu durum yapılandırması de yalnızca rapor uç noktası olarak kullanılabilir. Bu, DSC şirket içi aracılığıyla (itmeden) istenen yapılandırma ve Azure Otomasyonu istenen durumda düğüm uyum zengin raporlama ayrıntıları görüntülemek sağlar.

> [!NOTE]
> Durum yapılandırması ile Azure Vm'lerini yönetme yüklü sanal makine DSC uzantısı 2.70 büyükse, ek ücret alınmadan dahil edilir. Başvurmak [ **Fiyatlandırma sayfasında Otomasyon** ](https://azure.microsoft.com/pricing/details/automation/) daha fazla ayrıntı için.

Aşağıdaki bölümlerde, her makine için Azure Otomasyon durum yapılandırması türü yerleşik getirmeyi özetler.

## <a name="azure-virtual-machines-classic"></a>Azure sanal makineler (Klasik)

Azure Otomasyonu durum yapılandırması ile yapılandırma yönetimi için Azure portal veya PowerShell kullanarak kolayca yerleşik Azure sanal makineler (Klasik) kullanabilirsiniz. Başlık altında ve yönetici VM uzaktan zorunda olmadan Azure VM Desired State Configuration uzantısı ile Azure Otomasyonu durum yapılandırması VM kaydeder. Azure VM Desired State Configuration uzantısı zaman uyumsuz olarak çalıştığından, ilerleme durumunu izlemek ve bu sorun giderme adımları aşağıda sağlanan [ **sorun giderme Azure sanal makine ekleme** ](#troubleshooting-azure-virtual-machine-onboarding) bölümü.

### <a name="azure-portal"></a>Azure portal

İçinde [Azure portalında](https://portal.azure.com/), tıklayın **Gözat** -> **sanal makineler (Klasik)**. Eklemek istediğiniz Windows VM'yi seçin. Sanal makinenin Pano dikey penceresinde tıklayın **tüm ayarlar** -> **uzantıları** -> **Ekle** -> **Azure Automation DSC** -> **oluşturma**.
Girin [PowerShell DSC Local Configuration Manager değerleri](/powershell/dsc/metaconfig4) kullanım Örneğinize, Otomasyon hesabınıza ait kayıt anahtarını ve kayıt URL'si ve bir düğüm yapılandırması için isteğe bağlı olarak sanal Makineye atamak için gerekli.

![DSC için Azure VM uzantıları](./media/automation-dsc-onboarding/DSC_Onboarding_1.png)

Kayıt URL'si bulmak ve Otomasyon hesabı eklemek için aşağıdaki görmek istediğiniz makineyi anahtar [ **güvenli kayıt** ](#secure-registration) bölümü:

### <a name="powershell"></a>PowerShell

```powershell
# log in to both Azure Service Management and Azure Resource Manager
Add-AzureAccount
Connect-AzureRmAccount

# fill in correct values for your VM/Automation account here
$VMName = ''
$ServiceName = ''
$AutomationAccountName = ''
$AutomationAccountResourceGroup = ''

# fill in the name of a Node Configuration in Azure Automation State Configuration, for this VM to conform to
# NOTE: DSC Node Configuration names are case sensitive in the portal.
$NodeConfigName = ''

# get Azure Automation State Configuration registration info
$Account = Get-AzureRmAutomationAccount -ResourceGroupName $AutomationAccountResourceGroup -Name $AutomationAccountName
$RegistrationInfo = $Account | Get-AzureRmAutomationRegistrationInfo

# use the DSC extension to onboard the VM for management with Azure Automation State Configuration
$VM = Get-AzureVM -Name $VMName -ServiceName $ServiceName

$PublicConfiguration = ConvertTo-Json -Depth 8 @{
    SasToken = ''
    ModulesUrl = 'https://eus2oaasibizamarketprod1.blob.core.windows.net/automationdscpreview/RegistrationMetaConfigV2.zip'
    ConfigurationFunction = 'RegistrationMetaConfigV2.ps1\RegistrationMetaConfigV2'

# update these PowerShell DSC Local Configuration Manager defaults if they do not match your use case.
# See https://docs.microsoft.com/powershell/dsc/metaConfig for more details
    Properties = @{
        RegistrationKey = @{
            UserName = 'notused'
            Password = 'PrivateSettingsRef:RegistrationKey'
        }
        RegistrationUrl = $RegistrationInfo.Endpoint
        NodeConfigurationName = $NodeConfigName
        ConfigurationMode = 'ApplyAndMonitor'
        ConfigurationModeFrequencyMins = 15
        RefreshFrequencyMins = 30
        RebootNodeIfNeeded = $False
        ActionAfterReboot = 'ContinueConfiguration'
        AllowModuleOverwrite = $False
    }
}

$PrivateConfiguration = ConvertTo-Json -Depth 8 @{
    Items = @{
        RegistrationKey = $RegistrationInfo.PrimaryKey
    }
}

$VM = Set-AzureVMExtension `
    -VM $vm `
    -Publisher Microsoft.Powershell `
    -ExtensionName DSC `
    -Version 2.76 `
    -PublicConfiguration $PublicConfiguration `
    -PrivateConfiguration $PrivateConfiguration `
    -ForceUpdate

$VM | Update-AzureVM
```

> [!NOTE]
> Durum yapılandırması düğüm yapılandırması adı portalda büyük küçük harfe duyarlı. Eşleşmeyen durumda düğüm altında gösterilmez **düğümleri** sekmesi.

## <a name="azure-virtual-machines"></a>Azure sanal makineleri

Azure Otomasyonu durum yapılandırması Azure portal, Azure Resource Manager şablonları veya PowerShell kullanarak Azure sanal makine yapılandırma yönetimi için kolayca ekleme sağlar. Başlık altında ve yönetici VM uzaktan zorunda olmadan Azure VM Desired State Configuration uzantısı ile Azure Otomasyonu durum yapılandırması VM kaydeder.
Azure VM Desired State Configuration uzantısı zaman uyumsuz olarak çalıştığından, ilerleme durumunu izlemek ve bu sorun giderme adımları aşağıda sağlanan [ **sorun giderme Azure sanal makine ekleme** ](#troubleshooting-azure-virtual-machine-onboarding) bölümü.

### <a name="azure-portal"></a>Azure portal

İçinde [Azure portalında](https://portal.azure.com/), sanal makine ekleme için istediğiniz Azure Otomasyonu hesabı gidin. Durum Yapılandırması sayfasında ve **düğümleri** sekmesinde **+ Ekle**.

Bir Azure sanal makine eklemek için seçin.

Makine yoksa PowerShell istenen durum uzantısı yüklü ve güç durumunu çalışıyor **Connect**.

Altında **kayıt**, girin [PowerShell DSC Local Configuration Manager değerleri](/powershell/dsc/metaconfig4) , kullanım örneği ve bir düğüm yapılandırması için isteğe bağlı olarak sanal Makineye atamak için gerekli.

![Ekleme](./media/automation-dsc-onboarding/DSC_Onboarding_6.png)

### <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

Azure sanal makinelerine dağıtılabilir ve Azure Resource Manager şablonları aracılığıyla Azure Otomasyonu durumu yapılandırmasına eklenmedi. Bkz: [DSC uzantısı yoluyla bir sanal makine ve Azure Otomasyonu DSC yapılandırma](https://azure.microsoft.com/documentation/templates/dsc-extension-azure-automation-pullserver/) örnek şablonu için ekleme yapan Azure Otomasyon durum yapılandırması mevcut bir VM'ye. Kayıt URL'si geçen ve kayıt anahtarını bulmak için bu şablona giriş olarak aşağıdaki bakın [ **güvenli kayıt** ](#secure-registration) bölümü.

### <a name="powershell"></a>PowerShell

[Register-AzureRmAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode) cmdlet'i, sanal makine PowerShell aracılığıyla Azure portalında kullanılabilir.

## <a name="amazon-web-services-aws-virtual-machines"></a>Amazon Web Services (AWS) sanal makineler

Amazon Web Hizmetleri sanal makine için yapılandırma yönetimi AWS DSC araç setini kullanarak Azure Otomasyon durum yapılandırması tarafından kolayca ekleme yapabilirsiniz. Araç Seti hakkında daha fazla bilgi [burada](https://blogs.msdn.microsoft.com/powershell/2016/04/20/aws-dsc-toolkit/).

## <a name="physicalvirtual-windows-machines-on-premises-or-in-a-cloud-other-than-azureaws"></a>Fiziksel/sanal Windows makineleri şirket içinde veya Azure/AWS dışındaki bir bulutta bulunan

Birkaç basit adımda üzerinden internet giden erişime sahip oldukları sürece şirket içi Windows makineleri ve Azure olmayan bulutlarda (örneğin, Amazon Web Hizmetleri) Windows makineleri Azure Otomasyon durum Yapılandırması'na eklediğinizden de olabilir:

1. En son sürümünü emin [WMF 5](https://aka.ms/wmf5latest) Azure Otomasyon durum yapılandırması için eklemek istediğiniz makineleri yüklenir.
1. Aşağıdaki bölümde yer alan yönergeleri izleyin [ **oluşturma DSC metaconfigurations** ](#generating-dsc-metaconfigurations) gerekli DSC metaconfigurations içeren bir klasör oluşturmak için.
1. Uzaktan PowerShell DSC metaconfiguration eklemek istediğiniz makineler için geçerlidir. **Bu komutu çalıştırmak makine en son sürümüne sahip olmanız gerekir [WMF 5](https://aka.ms/wmf5latest) yüklü**:

   ```powershell
   Set-DscLocalConfigurationManager -Path C:\Users\joe\Desktop\DscMetaConfigs -ComputerName MyServer1, MyServer2
   ```

1. Uzaktan PowerShell DSC metaconfigurations uygulayamazsınız, 2. adımda her makineye metaconfigurations klasörünü, ekleme kopyalayın. Ardından çağırın **Set-DscLocalConfigurationManager** eklemek için her bir makinede yerel olarak.
1. Azure portal veya cmdlet'leri kullanarak, makineler eklemek için artık Azure Otomasyonu hesabınızı kayıtlı durum yapılandırması düğümleri olarak gösterilmediğini denetleyin.

## <a name="physicalvirtual-linux-machines-on-premises-in-azure-or-in-a-cloud-other-than-azure"></a>Şirket içi, azure'da veya Azure dışındaki bir bulutta bulunan fiziksel/sanal Linux makineleri

Birkaç basit adımda üzerinden internet giden erişime sahip oldukları sürece şirket içi Linux makineleri, azure'da Linux makineleri ve Azure olmayan bulutlarda Linux makineleri Azure Otomasyon durum Yapılandırması'na eklediğinizden de olabilir:

1. En son sürümünü emin [Linux için PowerShell Desired State Configuration](https://github.com/Microsoft/PowerShell-DSC-for-Linux) Azure Otomasyon durum yapılandırması için eklemek istediğiniz makineleri yüklenir.
1. Varsa [PowerShell DSC Local Configuration Manager varsayılan](/powershell/dsc/metaconfig4) kullanım Örneğinize uyan ve gibi yerleşik makineleri yapmak istiyorsanız, bunlar **hem** isteneceğini ve Azure Otomasyonu durum yapılandırması için Rapor:

   - Azure Otomasyonu durum yapılandırması eklemek için her Linux makine üzerinde `Register.py` yerleşik PowerShell DSC Local Configuration Manager varsayılan değerleri kullanarak:

     `/opt/microsoft/dsc/Scripts/Register.py <Automation account registration key> <Automation account registration URL>`

   - Automation hesabınız için bir kayıt anahtarı ve kayıt URL'si bulmak için aşağıdakilere bakın [ **güvenli kayıt** ](#secure-registration) bölümü.

     PowerShell DSC Local Configuration Manager varsayılan olarak, **olmayan** eşleşme kullanım Örneğinize veya eklemek istediğiniz makineleri bunlar için Azure Otomasyon durum yapılandırması yalnızca rapor olduğunu, ancak yapılandırma veya PowerShell çekme değil Modüller, 3-6 adımları izleyin. Aksi takdirde, doğrudan 6. adıma geçin.

1. Aşağıdaki yönergeleri izleyin [ **oluşturma DSC metaconfigurations** ](#generating-dsc-metaconfigurations) gerekli DSC metaconfigurations içeren bir klasör oluşturmak için bölüm.
1. Uzaktan PowerShell DSC metaconfiguration eklemek istediğiniz makineler için geçerlidir:

    ```powershell
    $SecurePass = ConvertTo-SecureString -String '<root password>' -AsPlainText -Force
    $Cred = New-Object System.Management.Automation.PSCredential 'root', $SecurePass
    $Opt = New-CimSessionOption -UseSsl -SkipCACheck -SkipCNCheck -SkipRevocationCheck

    # need a CimSession for each Linux machine to onboard
    $Session = New-CimSession -Credential $Cred -ComputerName <your Linux machine> -Port 5986 -Authentication basic -SessionOption $Opt

    Set-DscLocalConfigurationManager -CimSession $Session -Path C:\Users\joe\Desktop\DscMetaConfigs
    ```

Bu komutu çalıştırmak makine en son sürümüne sahip olmanız gerekir [WMF 5](https://aka.ms/wmf5latest) yüklü.

1. PowerShell DSC metaconfigurations uzaktan kişiyle, her bir Linux makine için uyguladığınız olamaz, bu makineye Linux makineye 5. adımında klasöründen karşılık gelen metaconfiguration kopyalayın. Ardından çağırın `SetDscLocalConfigurationManager.py` yerel olarak her bir Linux makinesinde Azure Otomasyon durum yapılandırması için eklemek istediğiniz:

   `/opt/microsoft/dsc/Scripts/SetDscLocalConfigurationManager.py -configurationmof <path to metaconfiguration file>`

1. Azure portal veya cmdlet'leri kullanarak, makineler eklemek için artık Azure Otomasyonu hesabınızı kayıtlı DSC düğümleri olarak gösterilmediğini denetleyin.

## <a name="generating-dsc-metaconfigurations"></a>DSC metaconfigurations oluşturuluyor

Genel ekleme için tüm Azure Otomasyon durum yapılandırması için makine bir [DSC metaconfiguration](/powershell/dsc/metaconfig) olabilir isteneceğini ve/veya Azure Otomasyonu durumu için rapor için makinede DSC aracı söyler, uygulandığında, oluşturulan Yapılandırma. DSC metaconfigurations Azure Otomasyon durum yapılandırması için bir PowerShell DSC yapılandırması ya da Azure Otomasyonu PowerShell cmdlet'lerini kullanarak oluşturulabilir.

> [!NOTE]
> DSC metaconfigurations ekleme bir Otomasyon için bir makine yönetimi için hesap gizli dizileri içerir. Oluşturduğunuz herhangi bir DSC metaconfigurations düzgün bir şekilde korumak emin olun veya kullandıktan sonra silin.

### <a name="using-a-dsc-configuration"></a>DSC Yapılandırması'nı kullanarak

1. Yerel ortamınızda bir makinede yönetici olarak VSCode (veya tercih ettiğiniz düzenleyiciyi) açın. Makinede en son sürümüne sahip olmanız gerekir [WMF 5](https://aka.ms/wmf5latest) yüklü.
1. Aşağıdaki betiği yerel olarak kopyalayın. Bu betik metaconfigurations ve metaconfiguration oluşturmayı istiyorsanız, bir komut oluşturmak için PowerShell DSC yapılandırması içerir.

> [!NOTE]
> Durum yapılandırması düğüm yapılandırması adı portalda büyük küçük harfe duyarlı. Eşleşmeyen durumda düğüm altında gösterilmez **düğümleri** sekmesi.

   ```powershell
   # The DSC configuration that will generate metaconfigurations
   [DscLocalConfigurationManager()]
   Configuration DscMetaConfigs
   {
        param
        (
            [Parameter(Mandatory=$True)]
            [String]$RegistrationUrl,

            [Parameter(Mandatory=$True)]
            [String]$RegistrationKey,

            [Parameter(Mandatory=$True)]
            [String[]]$ComputerName,

            [Int]$RefreshFrequencyMins = 30,

            [Int]$ConfigurationModeFrequencyMins = 15,

            [String]$ConfigurationMode = 'ApplyAndMonitor',

            [String]$NodeConfigurationName,

            [Boolean]$RebootNodeIfNeeded= $False,

            [String]$ActionAfterReboot = 'ContinueConfiguration',

            [Boolean]$AllowModuleOverwrite = $False,

            [Boolean]$ReportOnly
        )

        if(!$NodeConfigurationName -or $NodeConfigurationName -eq '')
        {
            $ConfigurationNames = $null
        }
        else
        {
            $ConfigurationNames = @($NodeConfigurationName)
        }

        if($ReportOnly)
        {
            $RefreshMode = 'PUSH'
        }
        else
        {
            $RefreshMode = 'PULL'
        }

        Node $ComputerName
        {
            Settings
            {
                RefreshFrequencyMins           = $RefreshFrequencyMins
                RefreshMode                    = $RefreshMode
                ConfigurationMode              = $ConfigurationMode
                AllowModuleOverwrite           = $AllowModuleOverwrite
                RebootNodeIfNeeded             = $RebootNodeIfNeeded
                ActionAfterReboot              = $ActionAfterReboot
                ConfigurationModeFrequencyMins = $ConfigurationModeFrequencyMins
            }

            if(!$ReportOnly)
            {
            ConfigurationRepositoryWeb AzureAutomationStateConfiguration
                {
                    ServerUrl          = $RegistrationUrl
                    RegistrationKey    = $RegistrationKey
                    ConfigurationNames = $ConfigurationNames
                }

                ResourceRepositoryWeb AzureAutomationStateConfiguration
                {
                ServerUrl       = $RegistrationUrl
                RegistrationKey = $RegistrationKey
                }
            }

            ReportServerWeb AzureAutomationStateConfiguration
            {
                ServerUrl       = $RegistrationUrl
                RegistrationKey = $RegistrationKey
            }
        }
   }

    # Create the metaconfigurations
    # NOTE: DSC Node Configuration names are case sensitive in the portal.
    # TODO: edit the below as needed for your use case
   $Params = @{
        RegistrationUrl = '<fill me in>';
        RegistrationKey = '<fill me in>';
        ComputerName = @('<some VM to onboard>', '<some other VM to onboard>');
        NodeConfigurationName = 'SimpleConfig.webserver';
        RefreshFrequencyMins = 30;
        ConfigurationModeFrequencyMins = 15;
        RebootNodeIfNeeded = $False;
        AllowModuleOverwrite = $False;
        ConfigurationMode = 'ApplyAndMonitor';
        ActionAfterReboot = 'ContinueConfiguration';
        ReportOnly = $False;  # Set to $True to have machines only report to AA DSC but not pull from it
   }

   # Use PowerShell splatting to pass parameters to the DSC configuration being invoked
   # For more info about splatting, run: Get-Help -Name about_Splatting
   DscMetaConfigs @Params
   ```

1. Eklenecek makinelerin adlarını yanı sıra, Otomasyon hesabınızı için kayıt anahtarı ve URL doldurun. Diğer tüm parametreler isteğe bağlıdır. Automation hesabınız için bir kayıt anahtarı ve kayıt URL'si bulmak için aşağıdakilere bakın [ **güvenli kayıt** ](#secure-registration) bölümü.
1. Azure Otomasyonu durum yapılandırması, ancak çekme yapılandırma veya PowerShell modülleri için DSC durum bilgilerini bildirmek için makineleri istiyorsanız ayarlayın **sadece rapor** parametresi true.
1. Betiği çalıştırın. Şu anda adlı bir klasör olmalıdır **DscMetaConfigs** makineler için PowerShell DSC metaconfigurations çalışma dizininizde içeren (Yönetici) olarak eklenecek:

    ```powershell
    Set-DscLocalConfigurationManager -Path ./DscMetaConfigs
    ```

### <a name="using-the-azure-automation-cmdlets"></a>Azure Otomasyon cmdlet'leri kullanma

PowerShell DSC Local Configuration Manager varsayılan kullanım Örneğinize uyan ve bunlar hem isteneceğini ve Azure Otomasyonu durum yapılandırması için rapor için makine istiyorsanız Azure Otomasyon cmdlet'leri oluşturmanın basitleştirilmiş bir yöntem sağlar. DSC metaconfigurations gerekli:

1. VSCode ve PowerShell konsolunu yönetici olarak bir makine yerel ortamınızda açın.
2. Azure Resource Manager kullanarak bağlanma `Connect-AzureRmAccount`
3. Yerleşik düğümlerine istediğiniz Otomasyon hesabı eklemek istediğiniz makineler için PowerShell DSC metaconfigurations indirin:

   ```powershell
   # Define the parameters for Get-AzureRmAutomationDscOnboardingMetaconfig using PowerShell Splatting
   $Params = @{
       ResourceGroupName = 'ContosoResources'; # The name of the Resource Group that contains your Azure Automation Account
       AutomationAccountName = 'ContosoAutomation'; # The name of the Azure Automation Account where you want a node on-boarded to
       ComputerName = @('web01', 'web02', 'sql01'); # The names of the computers that the meta configuration will be generated for
       OutputFolder = "$env:UserProfile\Desktop\";
   }
   # Use PowerShell splatting to pass parameters to the Azure Automation cmdlet being invoked
   # For more info about splatting, run: Get-Help -Name about_Splatting
   Get-AzureRmAutomationDscOnboardingMetaconfig @Params
   ```

1. Şu anda adlı bir klasör olmalıdır ***DscMetaConfigs***, makineler için PowerShell DSC metaconfigurations içeren (Yönetici) olarak eklenecek:

    ```powershell
    Set-DscLocalConfigurationManager -Path $env:UserProfile\Desktop\DscMetaConfigs
    ```

## <a name="secure-registration"></a>Kayıt güvenliğini sağlama

Makineleri güvenli bir şekilde bir PowerShell DSC çekme veya Raporlama sunucusuna (Azure Otomasyonu durumu yapılandırılması dahil olmak üzere) kimlik doğrulaması bir DSC düğümü sağlar WMF 5 DSC kaydı protokolü aracılığıyla bir Azure Otomasyonu hesabına ekleyebilir. Düğüm sunucusuna kaydeder bir **kayıt URL'si**kimlik doğrulaması kullanarak bir **kayıt anahtarı**. Kayıt sırasında sunucu sonrası kaydı için kimlik doğrulaması kullanmak bu düğüm için benzersiz bir sertifika DSC çekme/raporlama sunucusu ve DSC düğüm anlaşır. Bu işlem, bir düğüm bir gibi başka bir tehlikeye kimliğine bürünmek ve kötü amaçlı olarak davranmakta eklenen düğümleri engeller. Kayıttan sonra kayıt anahtarını yeniden kimlik doğrulaması için kullanılmaz ve düğümden silinir.

Durum yapılandırması kayıt protokolden için gereken bilgileri alabilirsiniz **anahtarları** altında **hesap ayarları** Azure portalında. Anahtar simgesine tıklayarak bu dikey pencereyi açın **Essentials** paneli Otomasyon hesabı için.

![Azure Otomasyonu anahtarları ve URL](./media/automation-dsc-onboarding/DSC_Onboarding_4.png)

- Kayıt, URL alanını yönetme anahtarlar dikey URL'dir.
- Kayıt birincil erişim anahtarı veya ikincil erişim anahtarını yönetin anahtarlar dikey penceresine anahtardır. Her iki anahtar kullanılabilir.

Ek güvenlik için bir Otomasyon hesabının birincil ve ikincil erişim anahtarlarını herhangi bir zamanda yeniden oluşturulabilir (üzerinde **anahtarları Yönet** sayfası) önceki anahtarlar kullanarak gelecekteki düğüm kayıtları önlemek için.

## <a name="troubleshooting-azure-virtual-machine-onboarding"></a>Azure sanal makine ekleme sorunlarını giderme

Azure Otomasyonu durum yapılandırması, yapılandırma yönetimi için Azure Windows VM kolayca ekleme olanak tanır. Altyapı öğeleri, Azure VM Desired State Configuration uzantısı, VM'nin Azure Otomasyonu durum yapılandırması ile kaydetmek için kullanılır. Azure VM Desired State Configuration uzantısı zaman uyumsuz olarak çalıştığından, ilerleme durumunu izleme ve sorun giderme yürütme önemli olabilir.

> [!NOTE]
> Herhangi bir Azure VM Desired State Configuration uzantısı kullanan bir Azure Otomasyonu durumu yapılandırması için bir Azure Windows VM ekleme yöntemi kadar Azure Automation'da kayıtlı gösterilecek düğümü için bir saate kadar sürebilir. Bu ekleme için gerekli olan Azure VM DSC uzantı tarafından VM'deki Windows Management Framework 5.0 yüklenmesi Azure Otomasyonu durum yapılandırması VM kaynaklanır.

Sorun giderme ya da Azure VM Desired State Configuration uzantısı durumunu görüntülemek için Azure portalında eklenen olan sanal Makineye gidin, sonra tıklayın **uzantıları** altında **ayarları**. Ardından **DSC** veya **DSCForLinux** işletim sisteminize bağlı olarak. Daha fazla ayrıntı için tıklayabilirsiniz **ayrıntılı durumu görüntüle**.

## <a name="certificate-expiration-and-reregistration"></a>Sertifika geçerlilik süresi ve olunmasını

Bir Azure Otomasyonu durum yapılandırması DSC düğüm olarak bir makine kaydolduktan sonra pek çok neden bu düğüme gelecekte yeniden kaydettirin gerekebilir vardır:

- Kaydolduktan sonra her düğüme benzersiz bir sertifika bir yıl sonra süresi dolar kimlik doğrulaması için otomatik olarak belirler. Şu anda, bir yılın zamanından sonra düğümleri yeniden kaydettirin gerek Yöneticiler, dolmak üzere olan zaman PowerShell DSC kaydı Protokolü otomatik olarak sertifikalarını yenileyemiyor. NŞu önce her düğümün Windows Management Framework 5.0 RTM çalıştığından emin olun. Düğüm bir düğümün kimlik doğrulama sertifikasının süresi dolmadan ve düğüm yeniden kaydetme değil, Azure Otomasyonu ile iletişim kuramıyor ve 'Unresponsive.' işaretli Olunmasını gerçekleştirilen 90 gün veya daha az sertifika sona erme zamanı ya da sertifika süre sonu zamanı sonra herhangi bir noktada oluşturulan ve kullanılan yeni bir sertifika içinde neden olur.
- Değişiklik yapmak için [PowerShell DSC Local Configuration Manager değerleri](/powershell/dsc/metaconfig4) kümesi düğümünün ConfigurationMode gibi ilk kayıt sırasında. Şu anda bu DSC aracı değerleri yalnızca olunmasını değiştirilebilir. Bunun tek istisnası düğüme atanan düğüm yapılandırmasının--bu doğrudan Azure Automation DSC içinde değiştirilebilir.

Düğüm başlangıçta, bu belgede açıklanan ekleme yöntemlerden birini kullanarak kaydettiğiniz aynı şekilde saklanması gerçekleştirilebilir. Azure Otomasyonu durumu yapılandırma bir düğümde nŞu önce kaydını gerekmez.

## <a name="next-steps"></a>Sonraki adımlar

- Başlamak için bkz: [Azure Otomasyon durum yapılandırması ile çalışmaya başlama](automation-dsc-getting-started.md)
- Hedef düğümleri atayabilirsiniz böylece, DSC yapılandırmaları derleme hakkında bilgi edinmek için [yapılandırmaları Azure Automation durumu yapılandırma derleme](automation-dsc-compile.md)
- PowerShell cmdlet başvurusu için bkz. [Azure Otomasyonu durumu yapılandırma cmdlet'leri](/powershell/module/azurerm.automation/#automation)
- Fiyatlandırma bilgileri için bkz: [Azure Otomasyon durum yapılandırması için fiyatlandırma](https://azure.microsoft.com/pricing/details/automation/)
- Bir sürekli dağıtım işlem hattı, Azure Otomasyonu durum yapılandırmasını kullanarak bir örnek görmek için bkz: [sürekli dağıtımı kullanarak Azure Otomasyon durum yapılandırması ve Chocolatey](automation-dsc-cd-chocolatey.md)
