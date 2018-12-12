---
title: Azure AD uygulama önkoşulları (önceki sürüm) ile Azure Disk şifrelemesi
description: Bu makale, Iaas sanal makineleri için Microsoft Azure Disk şifrelemesi kullanılarak önkoşulları sağlar.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: ff9f06f3ff062889c9e77163f66527af97bb527d
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53094501"
---
# <a name="azure-disk-encryption-prerequisites-previous-release"></a>Azure Disk şifrelemesi önkoşulları (önceki sürüm)

**Azure Disk Şifrelemesi'nın yeni sürümüne VM disk şifrelemeyi etkinleştirmek için bir Azure AD uygulama parametresi sağlama gereksinimini ortadan kaldırır. Yeni sürümle birlikte, etkinleştirme şifreleme adımı sırasında Azure AD kimlik bilgilerini sağlamak için artık gerekli değildir. Tüm yeni Vm'lere yeni sürümünü kullanarak Azure AD uygulama parametresiz şifrelenmelidir. Yeni sürümünü kullanarak VM disk şifrelemeyi etkinleştirmek için yönergeleri görüntülemek için bkz: [Azure Disk şifrelemesi önkoşulları](azure-security-disk-encryption-prerequisites.md). Zaten Azure AD uygulama parametreleri ile şifrelenmiş VM'ler hala desteklenmektedir ve AAD sözdizimiyle korunmasını devam etmelidir.**

 Bu makalede, Azure Disk şifrelemesi önkoşulları, Azure Disk şifrelemesi kullanabilmeniz için önce karşılanması gereken öğeleri açıklar. Genel Önkoşullar yanı sıra, Azure Disk şifrelemesi ile tümleşiktir [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/) ve şifreleme anahtarları key vault'ta yönetmek için kimlik doğrulaması sağlamak için bir Azure AD uygulaması kullanır. Ayrıca kullanmak isteyebileceğiniz [Azure PowerShell](/powershell/azure/overview) veya [Azure CLI](/cli/azure/) ayarlamak veya Key Vault ve Azure AD uygulaması'nı yapılandırmak için.

Ele alınan desteklenen senaryolar için Azure Iaas sanal makinelerinde Azure Disk Şifrelemesi'ı etkinleştirmeden önce [Azure Disk Şifrelemesi'ne genel bakış](azure-security-disk-encryption-overview.md) makalesi, önkoşulların sağlandığından emin olun. 

> [!NOTE]
> Bazı öneriler, veri, ağ veya ek lisans ya da abonelik maliyetlerinizi kaynaklanan işlem kaynak kullanımını artırabilir. Desteklenen bölgelerdeki Azure kaynakları oluşturmak için geçerli bir etkin Azure aboneliğinizin olması gerekir.


## <a name="bkmk_OSs"></a> Desteklenen işletim sistemleri
Azure Disk şifrelemesi, aşağıdaki işletim sistemlerinde desteklenir:

- Windows Server sürümleri: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 ve Windows Server 2016.
    - Windows Server 2008 R2 için .NET Framework 4.5, azure'daki şifreleme etkinleştirmeden önce yüklü olması gerekir. Windows Update ile isteğe bağlı bir güncelleştirme Windows Server 2008 R2 x64 tabanlı sistemler için Microsoft .NET Framework 4.5.2'yi yükleme ([KB2901983](https://support.microsoft.com/kb/2901983)).    
- Windows istemci sürümleri: Windows 8 istemcisi ve Windows 10 istemcisi.
- Azure Disk şifrelemesi yalnızca Windows'da desteklenmektedir belirli Azure Galerisi'nde Linux sunucusu dağıtımları ve sürümleri dayalıdır. Şu anda desteklenen sürümlerin listesi için başvurmak [Azure Disk şifrelemesi hakkında SSS](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport).
- Azure Disk şifrelemesi, anahtar kasası ve VM'lerin aynı Azure bölgesindeki ve abonelikte bulunmasını gerektirir. Kaynaklarını ayrı bölge içinde yapılandırma Azure Disk şifreleme özelliği etkinleştirilirken bir hata neden olur.

## <a name="bkmk_LinuxPrereq"></a> Linux Iaas sanal makineleri için ek Önkoşullar 

- Linux için Azure Disk şifrelemesi, 7 GB işletim sistemi disk şifrelemeyi etkinleştirmek için VM üzerindeki RAM gerektirir [desteklenen resimler](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport). İşletim sistemi disk şifreleme işlemi tamamlandıktan sonra VM ile daha az bellek çalıştırmak için yapılandırılabilir.
- Şifreleme etkinleştirilmeden önce şifrelenmiş veri diskleri düzgün /etc/fstab içinde listelenmesi gerekir. Kalıcı blok cihaz adı bu giriş için "/ dev/sdX" biçimindeki adlarını sırasında özellikle şifreleme uygulandıktan sonra yeniden başlatmaları arasında aynı disk ile ilişkilendirilmesi dayanan olamaz cihazı olarak kullanın. Bu davranışı hakkında daha fazla ayrıntı için bkz: [Linux VM sorunlarını giderme cihaz adı değişiklikleri](../virtual-machines/linux/troubleshoot-device-names-problems.md)
- /Etc/fstab ayarlarını, bağlama için düzgün şekilde yapılandırıldığından emin olun. Bu ayarları yapılandırmak için bağlama - bir komut çalıştırın veya VM'yi yeniden başlatın ve bu şekilde onarılmasının tetikleyin. Bu işlem tamamlandıktan sonra istenen sürücü yine de bağlandığını doğrulamak için lsblk komutunun çıkışını kontrol edin. 
    - Azure Disk şifrelemesi, /etc/fstab dosya sürücü şifreleme etkinleştirilmeden önce düzgün bağlarsanız değil, düzgün bir şekilde bağlamak mümkün olmayacaktır.
    - Azure Disk şifreleme işlemi /etc/fstab dışında ve kendi yapılandırma dosyasına bağlama bilgilerini şifreleme işleminin bir parçası olarak taşınır. Tamamlandıktan sonra veri Sürücü Şifrelemesi /etc/fstab eksik giriş görmek için alarmed çekinmeyin.
    -  Yeniden başlatıldıktan sonra yeni şifrelenmiş diskler bağlamak Azure Disk şifrelemesi işlemi için saat sürer. Bunlar hemen yeniden başlatmanın ardından kullanılabilir olmaz. İşlemi başlatmak, kilidini açmak ve ardından erişmek diğer işlemler için kullanılabilir önce şifrelenmiş sürücüleri bağlamak için zaman gerekir. Bu işlem, sistem özelliklerine bağlı olarak yeniden başlatma işleminden sonra birden fazla işlem birkaç dakika sürebilir.

Veri diskleri bağlayın ve gerekli/etc/fstab girişleri oluşturmak için kullanılan komutlar örneği bulunabilir [197 205 bu betik dosyasının satırları](https://github.com/ejarvi/ade-cli-getting-started/blob/master/validate.sh#L197-L205). 


## <a name="bkmk_GPO"></a> Ağ ve Grup İlkesi

**Parametresi söz dizimi, Iaas Vm'leri eski AAD kullanan Azure Disk şifrelemesi özelliği etkinleştirmek için aşağıdaki ağ uç noktası yapılandırması gereksinimleri karşılaması gerekir:** 
  - Anahtar kasanızı bağlanmak için bir belirteç almak üzere Iaas VM bir Azure Active Directory uç noktasına bağlanabilir olmalıdır \[login.microsoftonline.com\].
  - Şifreleme anahtarları için anahtar kasanıza yazmak için Iaas VM anahtar kasası uç noktasına bağlanabilir olmalıdır.
  - Iaas VM Azure uzantısı depoyu ve VHD dosyalarını barındıran Azure depolama hesabı'nı barındıran bir Azure depolama uç noktasına bağlanabiliyor olmanız gerekir.
  -  Güvenlik ilkeniz Azure vm'lerinden Internet erişimi sınırlayan, önceki URI çözmek ve IP'ler giden bağlantıya izin verecek bir kuralı yapılandırın. Daha fazla bilgi için [bir güvenlik duvarının ardındayken Azure anahtar kasası](../key-vault/key-vault-access-behind-firewall.md).
  - Windows üzerinde TLS 1.0 açıkça devre dışı bırakıldı ve .NET sürümünü 4.6 veya üzeri güncelleştirilmemiş ADE daha yeni TLS sürümünü seçmek aşağıdaki kayıt defteri değişikliği olanak tanıyacak:
    
        [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
        "SystemDefaultTlsVersions"=dword:00000001
        "SchUseStrongCrypto"=dword:00000001

        [HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319]
        "SystemDefaultTlsVersions"=dword:00000001
        "SchUseStrongCrypto"=dword:00000001` 
     

 


**Grup İlkesi:**
 - Azure Disk şifrelemesi çözümü, BitLocker dış anahtar koruyucusu Windows Iaas Vm'leri için kullanır. Etki alanına katılmış sanal makineleri, TPM koruyucusu zorlamak için tüm grup ilkeleri anında iletme yok. "Uyumlu TPM'siz BitLocker izin ver" için Grup İlkesi hakkında bilgi için bkz: [BitLocker Grup İlkesi başvurusu](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings#a-href-idbkmk-unlockpol1arequire-additional-authentication-at-startup).

-  Özel Grup İlkesi ile etki alanına katılmış sanal makinelerde BitLocker'ı İlkesi şu ayar içermelidir: [bitlocker kurtarma bilgilerinin kullanıcı depolamasını yapılandırma -> izin 256 bitlik kurtarma anahtarı](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-group-policy-settings). Azure Disk şifrelemesi, Bitlocker için özel Grup İlkesi ayarları uyumsuz olduğunda başarısız olur. Doğru ilkeyi gerekmedi makinelerde yeni ilkeyi uygulamak, (gpupdate.exe/Force) güncelleştirmek için yeni ilke zorlayın ve daha sonra yeniden başlatmak gerekli olabilir.  


## <a name="bkmk_PSH"></a> Azure PowerShell
[Azure PowerShell](/powershell/azure/overview) kullanan cmdlet'leri takımına [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) Azure kaynaklarınızı yönetmek için model. İle tarayıcınızda kullanmak [Azure Cloud Shell](../cloud-shell/overview.md), veya her PowerShell oturumunda kullanmak için aşağıdaki yönergeleri kullanarak yerel makinenize yükleyin. Yerel olarak yüklü zaten varsa, Azure Disk şifrelemesini yapılandırmak için en son Azure PowerShell SDK'sı sürümü kullandığınızdan emin olun. En son sürümünü indirin [Azure PowerShell sürümü](https://github.com/Azure/azure-powershell/releases).

### <a name="install-azure-powershell-for-use-on-your-local-machine-optional"></a>Yerel makinenizde (isteğe bağlı) kullanmak için Azure PowerShell'i yükleyin:  
1. İşletim sisteminiz için bağlantıları'ndaki yönergeleri izleyin sonra devam edebilirsiniz ancak geri kalan adımları.      
    - [Yükleme ve yapılandırma için Azure PowerShell Windows](/powershell/azure/install-azurerm-ps). 
        - PowerShellGet, Azure PowerShell'i yükleme ve AzureRM modülünü yükleyin. 
    - [Yükleme ve Azure PowerShell'i macOS ve Linux'ta yapılandırma](/powershell/azure/install-azurermps-maclinux).
        -  PowerShell Core, .NET Core için Azure PowerShell'i yükleyin ve Az modül yükle.
2. Yükleme [Azure Active Directory PowerShell Modülü](/powershell/azure/active-directory/install-adv2#installing-the-azure-ad-module). 

     ```powershell
     Install-Module AzureAD
     ```

3. Yüklü modülleri sürümleri doğrulayın.
      ```powershell
      Get-Module AzureRM -ListAvailable | Select-Object -Property Name,Version,Path
      Get-Module AzureAD -ListAvailable | Select-Object -Property Name,Version,Path
      ```
4. Oturum açmak için Azure kullanarak [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) cmdlet'i.
     
     ```powershell
     Connect-AzureRmAccount
     # For specific instances of Azure, use the -Environment parameter.
     Connect-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)
    
     <# If you have multiple subscriptions and want to specify a specific one, 
     get your subscription list with Get-AzureRmSubscription and 
     specify it with Set-AzureRmContext.  #>
     Get-AzureRmSubscription
     Set-AzureRmContext -SubscriptionId "xxxx-xxxx-xxxx-xxxx"
     ```

5.  Azure AD'ye bağlanma [Connect-AzureAD](/powershell/module/azuread/connect-azuread).
     
     ```powershell
     Connect-AzureAD
     ```

6. Gözden geçirme [Azure PowerShell'i kullanmaya başlama](/powershell/azure/get-started-azureps) ve [AzureAD](/powershell/module/azuread), gerekirse.

## <a name="bkmk_CLI"></a> Azure CLI

[Azure CLI 2.0](/cli/azure) Azure kaynaklarını yönetmek için bir komut satırı aracıdır. CLI, esnek bir şekilde verileri sorgulamak, engelleyici olmayan süreçler olarak uzun süreli işlemleri desteklemek ve kolay bir komut dosyası yapmak için tasarlanmıştır. [Azure Cloud Shell](../cloud-shell/overview.md) ile tarayıcınızda kullanabilir veya yerel makinenize yükleyip herhangi bir PowerShell oturumunda kullanabilirsiniz.

1. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli) kullanılmak üzere yerel makinenize (isteğe bağlı):

2. Yüklü sürümünü doğrulayın.
     
     ```azurecli-interactive
     az --version
     ``` 

3. Oturum açmak için Azure kullanarak [az login](/cli/azure/authenticate-azure-cli).
     
     ```azurecli
     az login
     
     # If you would like to select a tenant, use: 
     az login --tenant "<tenant>"

     # If you have multiple subscriptions, get your subscription list with az account list and specify with az account set.
     az account list
     az account set --subscription "<subscription name or ID>"
     ```

4. Gözden geçirme [Azure CLI 2.0 ile çalışmaya başlama](/cli/azure/get-started-with-azure-cli) gerekirse. 


## <a name="prerequisite-workflow-for-key-vault-and-the-azure-ad-app"></a>Anahtar kasası ve Azure AD uygulaması için önkoşul iş akışı

Zaten Azure Disk şifrelemesi için Key Vault ve Azure AD önkoşulları alışık olduğunuz, kullanabileceğiniz [Azure Disk şifrelemesi önkoşulları PowerShell Betiği](https://raw.githubusercontent.com/Azure/azure-powershell/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1 ). Önkoşulları betiği kullanma hakkında daha fazla bilgi için bkz. [VM hızlı başlangıç şifrelemek](quick-encrypt-vm-powershell.md) ve [Azure Disk şifrelemesi ek](azure-security-disk-encryption-appendix.md#bkmk_prereq-script). 

1. Bir anahtar kasası oluşturma. 
2. Bir Azure AD uygulaması ve hizmet sorumlusu ayarlayın.
3. Azure AD uygulaması için anahtar kasası erişim ilkesi ayarlayın.
4. Set anahtar kasası erişim ilkeleri Gelişmiş.
 
## <a name="bkmk_KeyVault"></a> Anahtar kasası oluşturma 
Azure Disk şifrelemesi ile tümleşiktir [Azure anahtar kasası](https://azure.microsoft.com/documentation/services/key-vault/) denetlemenize ve disk şifreleme anahtarlarını ve gizli anahtar kasası aboneliğinizi yönetmenize yardımcı olmak için. Anahtar kasası oluşturma veya mevcut bir Azure Disk şifrelemesi kullanın. Anahtar kasası hakkında daha fazla bilgi için bkz: [Azure anahtar kasası ile çalışmaya başlama](../key-vault/key-vault-get-started.md) ve [anahtar kasanızın güvenliğini sağlama](../key-vault/key-vault-secure-your-key-vault.md). Bir anahtar kasası oluşturmak için Resource Manager şablonu, Azure PowerShell veya Azure CLI'yı kullanabilirsiniz. 


>[!WARNING]
>Şifreleme parolaları bölgesel sınırlar arası yoksa emin olmak için Azure Disk şifrelemesi anahtar kasası ve VM'lerin aynı bölgede bulunması gerekir. Oluşturun ve şifrelenmiş VM ile aynı bölgede olan bir anahtar Kasası'nı kullanın. 


### <a name="bkmk_KVPSH"></a> PowerShell ile key vault oluşturma

Azure PowerShell kullanarak bir anahtar kasası oluşturabilirsiniz [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/New-AzureRmKeyVault) cmdlet'i. Key Vault için ek cmdlet'ler için bkz [AzureRM.KeyVault](/powershell/module/azurerm.keyvault/). 

1. Gerekirse, [Azure aboneliğinize bağlanma](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH). 
2. Gerekirse, yeni bir kaynak grubu oluşturma ile [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup).  Veri Merkezi konumlarını listesinde, kullanmak için [Get-AzureRmLocation](/powershell/module/azurerm.resources/get-azurermlocation). 
     
     ```azurepowershell-interactive
     # Get-AzureRmLocation 
     New-AzureRmResourceGroup –Name 'MySecureRG' –Location 'East US'
     ```

3. Kullanarak yeni bir anahtar kasası oluşturma [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/New-AzureRmKeyVault)
    
      ```azurepowershell-interactive
     New-AzureRmKeyVault -VaultName 'MySecureVault' -ResourceGroupName 'MySecureRG' -Location 'East US'
     ```

4. Not **kasa adı**, **kaynak grubu adı**, **kaynak kimliği**, **kasa URI'si**ve **nesne kimliği** diskleri şifreleme, daha sonra kullanmak üzere getirilir. 


### <a name="bkmk_KVCLI"></a> Azure CLI ile anahtar kasası oluşturma
Azure CLI kullanarak anahtar kasanıza yönetebileceğiniz [az keyvault](/cli/azure/keyvault#commands) komutları. Bir anahtar kasası oluşturmak için kullanın [az keyvault oluşturma](/cli/azure/keyvault#az-keyvault-create).

1. Gerekirse, [Azure aboneliğinize bağlanma](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI).
2. Gerekirse, yeni bir kaynak grubu oluşturma ile [az grubu oluşturma](/cli/azure/group#az-group-create). Konumlar listesinde, kullanmak için [az hesabı konumları-Listele](/cli/azure/account#az-account-list) 
     
     ```azurecli-interactive
     # To list locations: az account list-locations --output table
     az group create -n "MySecureRG" -l "East US"
     ```

3. Kullanarak yeni bir anahtar kasası oluşturma [az keyvault oluşturma](/cli/azure/keyvault#az-keyvault-create).
    
     ```azurecli-interactive
     az keyvault create --name "MySecureVault" --resource-group "MySecureRG" --location "East US"
     ```

4. Not **kasa adı** (ad), **kaynak grubu adı**, **kaynak kimliği** (ID) **Vault URI'si**ve **nesnekimliği** , döndürülür kullanılmak üzere daha sonra. 

### <a name="bkmk_KVRM"></a> Resource Manager şablonu ile anahtar kasası oluşturma

Bir anahtar kasası kullanarak oluşturabileceğiniz [Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create).

1. Azure Hızlı Başlangıç şablonu üzerinde **azure'a Dağıt**.
2. Abonelik, kaynak grubu, kaynak grubu konumu, anahtar kasası adını, nesne kimliği, yasal koşulları ve Sözleşmesi'ni seçin ve ardından **satın alma**. 


## <a name="bkmk_ADapp"></a> Bir Azure AD uygulaması ve hizmet sorumlusu ayarlama 
Azure'da çalışan bir VM'de etkinleştirilmesi için şifreleme, ihtiyacınız olduğunda, Azure Disk şifrelemesi oluşturur ve şifreleme anahtarları için anahtar kasanıza yazar. Anahtar kasanızı şifreleme anahtarları yönetme, Azure AD kimlik doğrulaması gerektirir. Bu amaç için bir Azure AD uygulaması oluşturun. Kimlik doğrulama amacıyla ya da istemci gizli anahtarı tabanlı kimlik doğrulaması kullanabilirsiniz veya [istemci Azure AD'ye sertifika tabanlı kimlik doğrulaması](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md).


### <a name="bkmk_ADappPSH"></a> Bir Azure AD uygulaması ve hizmet sorumlusu ile Azure PowerShell ayarlama 
Aşağıdaki komutları yürütün için alma ve [Azure AD PowerShell modülünün](/powershell/azure/active-directory/install-adv2). 

1. Gerekirse, [Azure aboneliğinize bağlanma](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH).
2. Kullanım [yeni AzureRmADApplication](/powershell/module/azurerm.resources/new-azurermadapplication) bir Azure AD uygulaması oluşturmaya yönelik PowerShell cmdlet'i. MyApplicationHomePage ve MyApplicationUri istediğiniz herhangi bir değer olabilir.

     ```azurepowershell-interactive
     $aadClientSecret = "My AAD client secret"
     $aadClientSecretSec = ConvertTo-SecureString -String $aadClientSecret -AsPlainText -Force
     $azureAdApplication = New-AzureRmADApplication -DisplayName "My Application Display Name" -HomePage "https://MyApplicationHomePage" -IdentifierUris "https://MyApplicationUri" -Password $aadClientSecretSec
     $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId
     ```

3. Azure AD ClientID $azureAdApplication.ApplicationId bağlıdır ve $aadClientSecret daha sonra Azure Disk Şifrelemesi'ni etkinleştirmek için kullanacağınız istemci gizli anahtarı. Azure AD gizli uygun şekilde koruyun. Çalışan `$azureAdApplication.ApplicationId` ApplicationId gösterilir.


### <a name="bkmk_ADappCLI"></a> Bir Azure AD uygulaması ve hizmet sorumlusu Azure CLI ile ayarlama

Hizmet sorumluları, Azure CLI kullanarak yönetebileceğiniz [az ad sp](/cli/azure/ad/sp) komutları. Daha fazla bilgi için [bir Azure hizmet sorumlusu oluşturma ](/cli/azure/create-an-azure-service-principal-azure-cli).

1. Gerekirse, [Azure aboneliğinize bağlanma](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI).
2. Yeni bir hizmet sorumlusu oluşturun.
     
     ```azurecli-interactive
     az ad sp create-for-rbac --name "ServicePrincipalName" --password "My-AAD-client-secret" --skip-assignment 
     ```
3.  Azure AD ClientID diğer komutlarında kullanılan AppID döndürdü. Ayrıca, az keyvault set-policy için kullanacağınız SPN bir hizmettir. Daha sonra Azure Disk Şifrelemesi'ni etkinleştirmek için kullanması gereken gizli paroladır. Azure AD gizli uygun şekilde koruyun.
 
### <a name="bkmk_ADappRM"></a> Bir Azure AD uygulaması ve hizmet sorumlusu ancak Azure portalında ayarlama
Adımları uygulayın [Azure Active Directory kaynaklarına erişmek uygulama ve hizmet sorumlusu oluşturmak için portalı kullanma](../active-directory/develop/howto-create-service-principal-portal.md) makale bir Azure AD uygulaması oluşturmak için. Aşağıda listelenen her adımı tamamlamak için doğrudan makale bölümüne sürer. 

1. [Gerekli izinleri doğrulama](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)
2. [Bir Azure Active Directory uygulaması oluşturma](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application) 
     - Herhangi bir ad kullanabilir ve oturum açma URL'si uygulama oluştururken istediğiniz.
3. [Uygulama kimliği ve kimlik doğrulama anahtarını alma](../active-directory/develop/howto-create-service-principal-portal.md#get-application-id-and-authentication-key). 
     - Kimlik doğrulama anahtarı, istemci parolası ve Set-AzureRmVMDiskEncryptionExtension için AadClientSecret kullanılır. 
        - Kimlik doğrulama anahtarı, Azure AD'de oturum açmak için bir kimlik bilgisi olarak bir uygulama tarafından kullanılır. Azure portalında bu gizli anahtarları olarak adlandırılır, ancak anahtar kasalarına ilgisi yoktur. Bu gizli dizi uygun şekilde güvenli hale getirin. 
     - Uygulama kimliği, daha sonra Set-azurermvmdiskencryptionextension Aadclientıd ve Set-AzureRmKeyVaultAccessPolicy için ServicePrincipalName olarak kullanılacaktır. 

## <a name="bkmk_KVAP"></a> Azure AD uygulaması için anahtar kasası erişim ilkesini ayarlama
Belirtilen bir anahtar Kasası'na şifreleme gizli anahtarları yazmak için Azure Disk şifrelemesi istemci kimliği ve gizli anahtar Kasası'na yazmak için izne sahip olan Azure Active Directory uygulamasının istemci gizli anahtarı gerekir. 

> [!NOTE]
> Azure Disk şifrelemesi aşağıdaki erişim ilkeleri, Azure AD İstemci uygulamanıza yapılandırmanızı gerektirir: _WrapKey_ ve _ayarlamak_ izinleri.

### <a name="bkmk_KVAPPSH"></a> Azure PowerShell ile Azure AD uygulaması için anahtar kasası erişim ilkesini ayarlama
Azure AD uygulamanızın ihtiyaç duyduğu kasadaki gizli dizileri ve anahtarları erişim hakları. Kullanım [kümesi AzureKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) (uygulama kaydedildiği zaman oluşturuldu) istemci kimliği olarak kullanarak, uygulamaya izinleri vermek için cmdlet _– ServicePrincipalName_ parametre değeri. Daha fazla bilgi için blog gönderisine bakın [Azure Key Vault - adım adım](https://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx). 

1. Gerekirse, [Azure aboneliğinize bağlanma](azure-security-disk-encryption-appendix.md#bkmk_ConnectPSH).
2. PowerShell ile AD uygulaması için anahtar kasası erişim ilkesi ayarlayın.

     ```azurepowershell-interactive
     $keyVaultName = 'MySecureVault'
     $aadClientID = 'MyAadAppClientID'
     $rgname = 'MySecureRG'
     Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname
     ```

### <a name="bkmk_KVAPCLI"></a> Azure CLI ile Azure AD uygulaması için anahtar kasası erişim ilkesini ayarlama
Kullanım [az keyvault set-policy](/cli/azure/keyvault#az-keyvault-set-policy) erişim ilkesini ayarlama. Daha fazla bilgi için [yönetme CLI 2.0 kullanarak Key Vault](../key-vault/key-vault-manage-with-cli2.md#authorizing-an-application-to-use-a-key-or-secret).

1. Gerekirse, [Azure aboneliğinize bağlanma](azure-security-disk-encryption-appendix.md#bkmk_ConnectCLI).
2. Aşağıdaki komutla gizli dizileri ve kaydırma almak için Azure CLI erişim oluşturulan hizmet sorumlusu anahtarları verin:
 
     ```azurecli-interactive
     az keyvault set-policy --name "MySecureVault" --spn "<spn created with CLI/the Azure AD ClientID>" --key-permissions wrapKey --secret-permissions set
     ```

### <a name="bkmk_KVAPRM"></a> Portal ile Azure AD uygulaması için anahtar kasası erişim ilkesini ayarlama

1. Kaynak grubunu, key vault ile açın.
2. Anahtar kasanızı seçin, Git **erişim ilkeleri**, ardından **yeni Ekle**.
3. Altında **Select sorumlusu**, oluşturduğunuz Azure AD uygulaması için arama yapın ve seçin. 
4. İçin **anahtar izinleri**, kontrol **Wrap Key** altında **şifreleme işlemleri**.
5. İçin **gizli dizi izinleri**, kontrol **ayarlamak** altında **gizli dizi yönetimi işlemleri**.
6. Tıklayın **Tamam** erişim ilkesini kaydedin. 

![Azure Key Vault şifreleme işlemleri - Wrap Key](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Azure Key Vault gizli izinleri - ayarlayın ](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

## <a name="bkmk_KVper"></a> Set anahtar kasası erişim ilkeleri Gelişmiş
Azure platform şifreleme anahtarları veya gizli anahtar kasanızı önyükleme ve birimler şifresini çözmek için VM ayıklanarak erişmesi gerekir. Disk şifrelemeyi etkinleştirme anahtar kasası veya dağıtımları başarısız olur.  

### <a name="bkmk_KVperPSH"></a> Gelişmiş erişim ilkeleri Azure PowerShell ile anahtar kasası ayarlama
 Anahtar kasası PowerShell cmdlet'ini kullanın [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) anahtar kasası disk şifrelemeyi etkinleştirmek için.

  - **Anahtar kasası disk şifrelemesi için etkinleştir:** EnabledForDiskEncryption Azure Disk şifrelemesi için gereklidir.
      
     ```azurepowershell-interactive 
     Set-AzureRmKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MySecureRG' -EnabledForDiskEncryption
     ```

  - **Key Vault gerekirse dağıtım için etkinleştir:** bu anahtar kasası kaynak oluşturma, örneğin bir sanal makine oluşturulurken başvurulduğundan olduğunda bu anahtar kasasından gizli dizilerini alma Microsoft.Compute kaynak sağlayıcısına sağlar.

     ```azurepowershell-interactive
      Set-AzureRmKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MySecureRG' -EnabledForDeployment
     ```

  - **Key Vault şablon dağıtımı için gerekirse etkinleştirme:** bu anahtar kasasına bir şablon dağıtımında başvuru olduğunda bu anahtar kasasından gizli dizileri almak üzere Azure Resource Manager sağlar.

     ```azurepowershell-interactive             
     Set-AzureRmKeyVaultAccessPolicy -VaultName 'MySecureVault' -ResourceGroupName 'MySecureRG' -EnabledForTemplateDeployment`
     ```

### <a name="bkmk_KVperCLI"></a> Gelişmiş erişim ilkeleri Azure CLI kullanarak anahtar kasası ayarlama
Kullanım [az keyvault update](/cli/azure/keyvault#az-keyvault-update) anahtar kasası disk şifrelemeyi etkinleştirmek için. 

 - **Anahtar kasası disk şifrelemesi için etkinleştir:** etkin için disk şifrelemesi gereklidir. 

     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MySecureRG" --enabled-for-disk-encryption "true"
     ```  

 - **Key Vault gerekirse dağıtım için etkinleştir:** kasasından gizli diziler olarak depolanan sertifikaları almak için sanal makineler izin verin.
     ```azurecli-interactive
     az keyvault update --name "MySecureVault" --resource-group "MySecureRG" --enabled-for-deployment "true"
     ``` 

 - **Key Vault şablon dağıtımı için gerekirse etkinleştirme:** gizli dizileri kasadan almak için Resource Manager izin verin.
     ```azurecli-interactive  
     az keyvault update --name "MySecureVault" --resource-group "MySecureRG" --enabled-for-template-deployment "true"
     ```


### <a name="bkmk_KVperrm"></a> Gelişmiş erişim ilkeleri Azure portalı üzerinden anahtar kasası ayarlama

1. Anahtar Kasası'nı seçin, Git **erişim ilkeleri**, ve **Gelişmiş erişim ilkelerini görüntülemek için tıklayın**.
2. Etiketli kutunun seçili olduğunu seçin **Azure Disk şifrelemesi birim şifreleme için erişimi etkinleştirme**.
3. Seçin **dağıtımı için Azure sanal Makineler'e erişimi etkinleştir** ve/veya **erişimi için Azure Resource Manager şablon dağıtımı için Etkinleştir**, gerekirse. 
4. **Kaydet**’e tıklayın.

![Gelişmiş erişim ilkeleri azure anahtar kasası](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)


## <a name="bkmk_KEK"></a> Bir anahtar şifreleme anahtarı (isteğe bağlı) ayarlama
Bir ek şifreleme anahtarları için güvenlik katmanı için bir anahtar şifreleme anahtarı (KEK) kullanmak istiyorsanız bir KEK anahtar kasanızı ekleyin. Kullanım [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) anahtar şifreleme anahtarı anahtar kasasını oluşturmak için cmdlet'i. Ayrıca, şirket içi Anahtar Yönetimi'nden HSM bir KEK içeri aktarabilirsiniz. Daha fazla bilgi için [Key Vault belgeleri](../key-vault/key-vault-hsm-protected-keys.md). Anahtar şifreleme anahtarı belirtildiğinde, Azure Disk şifrelemesi anahtar Kasası'na yazmadan önce şifreleme parolaları sarmalamak için bu anahtarı kullanır. 

* Anahtar kasası gizli dizi ve KEK URL'leri tutulan olmalıdır. Azure, sürüm oluşturma bu kısıtlamayı zorlar. Geçerli bir gizli dizi ve KEK URL'ler için aşağıdaki örneklere bakın:

  * Geçerli bir gizli URL örneği:   *https://contosovault.vault.azure.net/secrets/EncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Geçerli bir KEK URL örneği:   *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Azure Disk şifrelemesi, anahtar kasası gizli dizileri ve KEK URL'leri bir parçası olarak belirten bir bağlantı noktası numaralarını desteklemez. Desteklenen ve desteklenmeyen key vault URL'leri örnekleri için aşağıdaki örneklere bakın:

  * Kabul edilebilir bir key vault URL'si  *https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Kabul edilebilir bir key vault URL'si:   *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

### <a name="bkmk_KEKPSH"></a> Azure PowerShell ile bir anahtar şifreleme anahtarı ayarlama 
PowerShell betiğini kullanmadan önce betiği adımları anlamak için Azure Disk şifrelemesi önkoşulları hakkında bilgi sahibi olmalıdır. Örnek betik, ortamınız için değişiklikleri gerekebilir. Bu betik, tüm Azure Disk şifrelemesi önkoşulları oluşturur ve disk şifreleme anahtarı bir anahtar şifreleme anahtarı kullanılarak sarmalama var olan bir Iaas VM, şifreler. 

 ```powershell
 # Step 1: Create a new resource group and key vault in the same location.
     # Fill in 'MyLocation', 'MySecureRG', and 'MySecureVault' with your values.
     # Use Get-AzureRmLocation to get available locations and use the DisplayName.
     # To use an existing resource group, comment out the line for New-AzureRmResourceGroup
     
     $Loc = 'MyLocation';
     $rgname = 'MySecureRG';
     $KeyVaultName = 'MySecureVault'; 
     New-AzureRmResourceGroup –Name $rgname –Location $Loc;
     New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname -Location $Loc;
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $KeyVaultResourceId = (Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname).ResourceId;
     $diskEncryptionKeyVaultUrl = (Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname).VaultUri;
     
 # Step 2: Create the AD application and service principal.
     # Fill in 'MyAADClientSecret', "<My Application Display Name>", "<https://MyApplicationHomePage>", and "<https://MyApplicationUri>" with your values.
     # MyApplicationHomePage and the MyApplicationUri can be any values you wish.
     
     $aadClientSecret =  'MyAADClientSecret';
     $aadClientSecretSec = ConvertTo-SecureString -String $aadClientSecret -AsPlainText -Force;
     $azureAdApplication = New-AzureRmADApplication -DisplayName "<My Application Display Name>" -HomePage "<https://MyApplicationHomePage>" -IdentifierUris "<https://MyApplicationUri>" -Password $aadClientSecretSec
     $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId;
     $aadClientID = $azureAdApplication.ApplicationId;
     
 #Step 3: Enable the vault for disk encryption and set the access policy for the Azure AD application.
     
     Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $rgname -EnabledForDiskEncryption;
     Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname;
     
 #Step 4: Create a new key in the key vault with the Add-AzureKeyVaultKey cmdlet.
     # Fill in 'MyKeyEncryptionKey' with your value.
     
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName -Destination 'Software';
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;
     
 #Step 5: Encrypt the disks of an existing IaaS VM
     # Fill in 'MySecureVM' with your value. 
     
     $VMName = 'MySecureVM';
     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
```

## <a name="bkmk_Cert"></a> Sertifika tabanlı kimlik doğrulaması (isteğe bağlı)
Sertifika kimlik doğrulaması kullanmak istiyorsanız, anahtar kasanıza karşıya yükleyin ve istemciye dağıtın. PowerShell betiğini kullanmadan önce betiği adımları anlamak için Azure Disk şifrelemesi önkoşulları hakkında bilgi sahibi olmalıdır. Örnek betik, ortamınız için değişiklikleri gerekebilir.

     
 ```powershell

 # Fill in "MySecureRG", "MySecureVault", and 'MyLocation' ('My location' only if needed)

   $rgname = 'MySecureRG'
   $KeyVaultName= 'MySecureVault'

   # Create a key vault and set enabledForDiskEncryption property on it. 
   # Comment out the next three lines if you already have an existing key vault enabled for encryption. No need to set 'My location' in this case.

   $Loc = 'MyLocation'
   New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname -Location $Loc
   Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $rgname -EnabledForDiskEncryption

   #Setting some variables with the key vault information 
   $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname
   $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
   $KeyVaultResourceId = $KeyVault.ResourceId

   # Create the Azure AD application and associate the certificate with it. 
   # Fill in "C:\certificates\mycert.pfx", "Password", "<My Application Display Name>", "<https://MyApplicationHomePage>", and "<https://MyApplicationUri>" with your values.
   # MyApplicationHomePage and the MyApplicationUri can be any values you wish

   $CertPath = "C:\certificates\mycert.pfx"
   $CertPassword = "Password"
   $Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
   $CertValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

   $AzureAdApplication = New-AzureRmADApplication -DisplayName "<My Application Display Name>" -HomePage "<https://MyApplicationHomePage>" -IdentifierUris "<https://MyApplicationUri>" -CertValue $CertValue 
   $ServicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $AzureAdApplication.ApplicationId

   $AADClientID = $AzureAdApplication.ApplicationId
   $aadClientCertThumbprint= $cert.Thumbprint

   Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname
   
   # Upload the pfx file to the key vault. 
   # Fill in "MyAADCert".  

   $KeyVaultSecretName = "MyAADCert"
   $FileContentBytes = get-content $CertPath -Encoding Byte
   $FileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
           $JSONObject = @"
           { 
               "data" : "$filecontentencoded", 
               "dataType" : "pfx", 
               "password" : "$CertPassword" 
           } 
"@

   $JSONObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
   $JSONEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

   #Set the secret and set the key vault policy for -EnabledForDeployment

   $Secret = ConvertTo-SecureString -String $JSONEncoded -AsPlainText -Force
   Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName -SecretValue $Secret
   Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $rgname -EnabledForDeployment

   # Deploy the certificate to the VM
   # Fill in 'MySecureVM' with your value.

   $VMName = 'MySecureVM'
   $CertUrl = (Get-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName).Id
   $SourceVaultId = (Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname).ResourceId
   $VM = Get-AzureRmVM -ResourceGroupName $rgname -Name $VMName 
   $VM = Add-AzureRmVMSecret -VM $VM -SourceVaultId $SourceVaultId -CertificateStore "My" -CertificateUrl $CertUrl
   Update-AzureRmVM -VM $VM -ResourceGroupName $rgname 

   #Enable encryption on the VM using Azure AD client ID and the client certificate thumbprint

   Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId
 ```

## <a name="bkmk_CertKEK"></a> Sertifika tabanlı kimlik doğrulaması ve (isteğe bağlı) bir KEK

Şifreleme anahtarı bir KEK ile kaydırma ve sertifika kimlik doğrulaması kullanmak istiyorsanız, kullanabileceğiniz aşağıdaki örnek betiği. PowerShell betiğini kullanmadan önce tüm betik adımları anlamak için önceki Azure Disk şifrelemesi önkoşulları sahibi olmalısınız. Örnek betik, ortamınız için değişiklikleri gerekebilir.

> [!IMPORTANT]
> Linux Vm'lerinde Azure AD sertifika tabanlı kimlik doğrulaması şu anda desteklenmiyor.



     
 ```powershell
# Fill in 'MySecureRG', 'MySecureVault', and 'MyLocation' (if needed)

   $rgname = 'MySecureRG'
   $KeyVaultName= 'MySecureVault'

   # Create a key vault and set enabledForDiskEncryption property on it. 
   # Comment out the next three lines if you already have an existing key vault enabled for encryption.

   $Loc = 'MyLocation'
   New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname -Location $Loc
   Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $rgname -EnabledForDiskEncryption

   # Create the Azure AD application and associate the certificate with it.  
   # Fill in "C:\certificates\mycert.pfx", "Password", "<My Application Display Name>", "<https://MyApplicationHomePage>", and "<https://MyApplicationUri>" with your values.
   # MyApplicationHomePage and the MyApplicationUri can be any values you wish

   $CertPath = "C:\certificates\mycert.pfx"
   $CertPassword = "Password"
   $Cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($CertPath, $CertPassword)
   $CertValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

   $AzureAdApplication = New-AzureRmADApplication -DisplayName "<My Application Display Name>" -HomePage "<https://MyApplicationHomePage>" -IdentifierUris "<https://MyApplicationUri>" -CertValue $CertValue 
   $ServicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $AzureAdApplication.ApplicationId

   $AADClientID = $AzureAdApplication.ApplicationId
   $aadClientCertThumbprint= $cert.Thumbprint

   ## Give access for setting secrets and wraping keys
   Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname

   # Upload the pfx file to the key vault. 
   # Fill in "MyAADCert". 

   $KeyVaultSecretName = "MyAADCert"
   $FileContentBytes = get-content $CertPath -Encoding Byte
   $FileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
           $JSONObject = @"
           { 
               "data" : "$filecontentencoded", 
               "dataType" : "pfx", 
               "password" : "$CertPassword" 
           } 
"@

   $JSONObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
   $JSONEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

   #Set the secret and set the key vault policy for deployment

   $Secret = ConvertTo-SecureString -String $JSONEncoded -AsPlainText -Force
   Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName -SecretValue $Secret
   Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $rgname -EnabledForDeployment

   #Setting some variables with the key vault information and generating a KEK 
   # FIll in 'KEKName'
   
   $KEKName ='KEKName'
   $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname
   $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri
   $KeyVaultResourceId = $KeyVault.ResourceId
   $KEK = Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name $KEKName -Destination "Software"
   $KeyEncryptionKeyUrl = $KEK.Key.kid



   # Deploy the certificate to the VM
   # Fill in 'MySecureVM' with your value.

   $VMName = 'MySecureVM'
   $CertUrl = (Get-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $KeyVaultSecretName).Id
   $SourceVaultId = (Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname).ResourceId
   $VM = Get-AzureRmVM -ResourceGroupName $rgname -Name $VMName 
   $VM = Add-AzureRmVMSecret -VM $VM -SourceVaultId $SourceVaultId -CertificateStore "My" -CertificateUrl $CertUrl
   Update-AzureRmVM -VM $VM -ResourceGroupName $rgname 

   #Enable encryption on the VM using Azure AD client ID and the client certificate thumbprint

   Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $VMName -AadClientID $AADClientID -AadClientCertThumbprint $AADClientCertThumbprint -DiskEncryptionKeyVaultUrl $DiskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId
```

 
## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Windows için Azure Disk şifrelemesini etkinleştirme](azure-security-disk-encryption-windows-aad.md)

> [!div class="nextstepaction"]
> [Linux için Azure Disk şifrelemesini etkinleştirme](azure-security-disk-encryption-linux-aad.md)
