---
title: 'Öğretici: Azure için tek bir AD ormanı ortamı federasyona | Microsoft Docs'
description: Federasyon kullanarak bir karma kimlik ortamı Kurulumu gösterilmiştir.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/16/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: cc6d1d53fd292a936c833450fee3af91671eec86
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56171182"
---
# <a name="tutorial-federate-a-single-ad-forest-environment-to-the-cloud"></a>Öğretici: Tek bir AD ormanı ortamını bulutta federasyona ekleme

![Oluştur](media/tutorial-federation/diagram.png)

Aşağıdaki öğreticiye Federasyon kullanarak bir karma kimlik ortamı oluşturma işleminde size yol gösterir.  Bu ortam, test etmek veya bir karma kimlik nasıl çalışır hakkında daha fazla bilgi almak için kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için gereken önkoşulları şunlardır:
- Bir bilgisayarla [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview) yüklü.  Üzerinde ya da bunu yapmak için önerilen bir [Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os) veya [Windows Server 2016](https://docs.microsoft.com/windows-server/virtualization/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) bilgisayar.
- [Azure aboneliği](https://azure.microsoft.com/free)
- - Bir [dış ağ bağdaştırıcısı](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network) sanal makinenin internet ile iletişim kurmasına izin vermek için.
- Windows Server 2016'ın bir kopyalama
- A [özel etki alanı](../../active-directory/fundamentals/add-custom-domain.md) , doğrulanabilir

> [!NOTE]
> Bu öğreticide, hızlı sürede öğretici ortamı oluşturabilmeniz PowerShell betikleri kullanılır.  Her komut betikleri başında bildirilen değişkenleri kullanır.  Yapabilir ve ortamınızı yansıtmak üzere değişkenleri değiştirmeniz gerekir.
>
>Kullanılan komut dosyaları, Azure AD Connect'i yüklemeden önce genel bir Active Directory ortamı oluşturun.  Bunlar tüm öğreticileri için uygundur.
>
> Bu öğreticide kullanılan PowerShell betikleri kopyalarını Github'da bulunmaktadır [burada](https://github.com/billmath/tutorial-phs).

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
Şirket içi Active Directory Sunucumuz kullanılacak bir sanal makine oluşturmak için çalışan yapmak için karma kimlik ortamımızda ürününü ihtiyacımız ve ilk şey.  

>[!NOTE]
>Hiçbir zaman bir betik PowerShell'de, konak makinesi üzerinde çalıştırırsanız çalıştırmanız gerekir `Set-ExecutionPolicy remotesigned` ve Evet, PowerShell betikleri çalıştırılmadan önce söyleyin.

Şunları yapın:

1. Yönetici olarak PowerShell ISE'yi açın.
2. Aşağıdaki betiği çalıştırın.

```powershell
#Declare variables
$VMName = 'DC1'
$Switch = 'External'
$InstallMedia = 'D:\ISO\en_windows_server_2016_updated_feb_2018_x64_dvd_11636692.iso'
$Path = 'D:\VM'
$VHDPath = 'D:\VM\DC1\DC1.vhdx'
$VHDSize = '64424509440'

#Create New Virtual Machine
New-VM -Name $VMName -MemoryStartupBytes 16GB -BootDevice VHD -Path $Path -NewVHDPath $VHDPath -NewVHDSizeBytes $VHDSize  -Generation 2 -Switch $Switch  

#Set the memory to be non-dynamic
Set-VMMemory $VMName -DynamicMemoryEnabled $false

#Add DVD Drive to Virtual Machine
Add-VMDvdDrive -VMName $VMName -ControllerNumber 0 -ControllerLocation 1 -Path $InstallMedia

#Mount Installation Media
$DVDDrive = Get-VMDvdDrive -VMName $VMName

#Configure Virtual Machine to Boot from DVD
Set-VMFirmware -VMName $VMName -FirstBootDevice $DVDDrive 
```

## <a name="complete-the-operating-system-deployment"></a>İşletim sistemi dağıtımını tamamlama
Sanal makine oluşturma işlemini tamamlamak için işletim sistemi yüklemesinin tamamlanması gerekir.

1. Hyper-V Yöneticisi, sanal makinede çift tıklatın
2. Başlat düğmesine tıklayın.
3.  'CD veya DVD'den önyükleme için bir tuşa basın için ' istenir. Devam edin ve bunu yapın.
4. Windows Server başlangıç ekranı dilinizi seçin ve tıklayın **sonraki**.
5. Tıklayın **Şimdi Yükle**.
6. Lisans anahtarınızı girip __iade **sonraki**.
7. Denetleme ** lisans koşullarını kabul edin ve tıklayın **sonraki**.
8. Seçin **özel:  Yalnızca Windows yükleyin (Gelişmiş)**
9. **İleri**’ye tıklayın
10. Yükleme tamamlandıktan sonra VM'yi en güncel olduğundan emin olmak için oturum açma ve çalıştırma Windows güncelleştirmeleri, sanal makineyi yeniden başlatın.  En son güncelleştirmeleri yüklersiniz.

## <a name="install-active-directory-pre-requisites"></a>Active Directory önkoşulları yükle
Bir sanal makine sahibiz, Active Directory yüklemeden önce birkaç şey yapmanız gerekir.  Diğer bir deyişle, sanal makine yeniden adlandırma, bir statik IP adresi ve DNS bilgilerini ayarlayabilir ve Uzak Sunucu Yönetim Araçları'nı yükleme ihtiyacımız var.   Şunları yapın:

1. Yönetici olarak PowerShell ISE'yi açın.
2. Çalıştırma `Set-ExecutionPolicy remotesigned` ve tüm [A] Evet söyleyin.  Enter tuşuna basın.
3. Aşağıdaki betiği çalıştırın.

```powershell
#Declare variables
$ipaddress = "10.0.1.117" 
$ipprefix = "24" 
$ipgw = "10.0.1.1" 
$ipdns = "10.0.1.117"
$ipdns2 = "8.8.8.8" 
$ipif = (Get-NetAdapter).ifIndex 
$featureLogPath = "c:\poshlog\featurelog.txt" 
$newname = "DC1"
$addsTools = "RSAT-AD-Tools" 

#Set static IP address
New-NetIPAddress -IPAddress $ipaddress -PrefixLength $ipprefix -InterfaceIndex $ipif -DefaultGateway $ipgw 

# Set the DNS servers
Set-DnsClientServerAddress -InterfaceIndex $ipif -ServerAddresses ($ipdns, $ipdns2)

#Rename the computer 
Rename-Computer -NewName $newname -force 

#Install features 
New-Item $featureLogPath -ItemType file -Force 
Add-WindowsFeature $addsTools 
Get-WindowsFeature | Where installed >>$featureLogPath 

#Restart the computer 
Restart-Computer
```

## <a name="create-a-windows-server-ad-environment"></a>Windows Server AD ortamı oluşturma
Oluşturulan VM sunuyoruz ve yeniden adlandırıldı ve bir statik IP adresine sahip olduğunuzdan, biz devam edin ve yükleyebilir ve Active Directory Domain Services'ı yapılandırın.  Şunları yapın:

1. Yönetici olarak PowerShell ISE'yi açın.
2. Aşağıdaki betiği çalıştırın.

```powershell 
#Declare variables
$DatabasePath = "c:\windows\NTDS"
$DomainMode = "WinThreshold"
$DomainName = "contoso.com"
$DomaninNetBIOSName = "CONTOSO"
$ForestMode = "WinThreshold"
$LogPath = "c:\windows\NTDS"
$SysVolPath = "c:\windows\SYSVOL"
$featureLogPath = "c:\poshlog\featurelog.txt" 
$Password = "Pass1w0rd"

#Install AD DS, DNS and GPMC 
start-job -Name addFeature -ScriptBlock { 
Add-WindowsFeature -Name "ad-domain-services" -IncludeAllSubFeature -IncludeManagementTools 
Add-WindowsFeature -Name "dns" -IncludeAllSubFeature -IncludeManagementTools 
Add-WindowsFeature -Name "gpmc" -IncludeAllSubFeature -IncludeManagementTools } 
Wait-Job -Name addFeature 
Get-WindowsFeature | Where installed >>$featureLogPath

#Create New AD Forest
Install-ADDSForest -CreateDnsDelegation:$false -DatabasePath $DatabasePath -DomainMode $DomainMode -DomainName $DomainName -SafeModeAdministratorPassword $Password -DomainNetbiosName $DomainNetBIOSName -ForestMode $ForestMode -InstallDns:$true -LogPath $LogPath -NoRebootOnCompletion:$false -SysvolPath $SysVolPath -Force:$true
```

## <a name="create-a-windows-server-ad-user"></a>Bir Windows Server AD kullanıcısı oluşturun
Active Directory ortamımızda sahibiz, bir test hesabına ihtiyacımız var.  Bu hesap, şirket içinde oluşturulacak AD ortamını ve ardından Azure AD'ye eşitlenen.  Şunları yapın:

1. Yönetici olarak PowerShell ISE'yi açın.
2. Aşağıdaki betiği çalıştırın.

```powershell 
#Declare variables
$Givenname = "Allie"
$Surname = "McCray"
$Displayname = "Allie McCray"
$Name = "amccray"
$Password = "Pass1w0rd"
$Identity = "CN=ammccray,CN=Users,DC=contoso,DC=com"
$SecureString = ConvertTo-SecureString $Password -AsPlainText -Force


#Create the user
New-ADUser -Name $Name -GivenName $Givenname -Surname $Surname -DisplayName $Displayname -AccountPassword $SecureString

#Set the password to never expire
Set-ADUser -Identity $Identity -PasswordNeverExpires $true -ChangePasswordAtLogon $false -Enabled $true
```

## <a name="create-a-certificate-for-ad-fs"></a>AD FS için bir sertifika oluşturun
AD FS tarafından kullanılacak bir SSL sertifikası artık oluşturacağız.  Bu işlem, otomatik olarak imzalanan bir sertifika ve yalnızca test amacıyla.  Microsoft, bir üretim ortamında otomatik olarak imzalanan bir sertifika kullanarak önermez. Şunları yapın:

1. Yönetici olarak PowerShell ISE'yi açın.
2. Aşağıdaki betiği çalıştırın.

```powershell 
#Declare variables
$DNSname = "adfs.contoso.com"
$Location = "cert:\LocalMachine\My"

#Create certificate
New-SelfSignedCertificate -DnsName $DNSname -CertStoreLocation $Location
```

## <a name="create-an-azure-ad-tenant"></a>Azure AD kiracısı oluşturma
Artık Azure AD kiracısı oluşturun, böylece biz kullanıcılarımız için bulut eşitlemek ihtiyacımız var.  Yeni bir Azure AD oluşturmak için Kiracı, aşağıdakileri yapın.

1. Gözat [Azure portalında](https://portal.azure.com) ve bir Azure aboneliğine sahip bir hesapla oturum açın.
2. Seçin **simgesini (+) artı** araması **Azure Active Directory**.
3. Seçin **Azure Active Directory** arama sonuçlarında.
4. **Oluştur**’u seçin.</br>
![Oluşturma](media/tutorial-password-hash-sync/create1.png)</br>
5. Sağlayan bir **kuruluş adı** ile birlikte **ilk etki alanı adı**. Ardından **Oluştur**’u seçin. Bu dizin oluşturur.
6. Bu tamamlandıktan sonra tıklayın **burada** dizini yönetmek için bağlantı.

## <a name="create-a-global-administrator-in-azure-ad"></a>Azure AD genel Yöneticisi oluşturma
Azure AD kiracısı sahibiz, genel yönetici hesabını oluşturacağız.  Bu hesap, Azure AD Connect yüklemesi sırasında Azure AD Bağlayıcısı hesabı oluşturmak için kullanılır.  Azure AD Bağlayıcısı hesabı bilgileri Azure AD'ye yazmak için kullanılır.   Genel yönetici oluşturmak için hesabı aşağıdakileri yapın.

1.  **Yönet** bölümünde **Kullanıcılar**’ı seçin.</br>
![Oluşturma](media/tutorial-password-hash-sync/gadmin1.png)</br>
2.  Seçin **tüm kullanıcılar** seçip **+ yeni kullanıcı**.
3.  Bu kullanıcı için bir ad ve kullanıcı adı sağlayın. Bu kiracının genel Yöneticisi olacaktır. Ayrıca değiştirmek isteyeceksiniz **dizin rolü** için **genel yönetici.** Geçici parolayı da gösterebilirsiniz. İşiniz bittiğinde **Oluştur**’u seçin.</br>
![Oluşturma](media/tutorial-password-hash-sync/gadmin2.png)</br>
4. Bu tamamlandıktan sonra yeni bir web tarayıcısı açın ve yeni genel yönetici hesabını ve geçici parolayı kullanarak myapps.microsoft.com için oturum açın.
5. Genel yönetici, hatırlanır bir şey için parolayı değiştirin.

## <a name="add-the-custom-domain-name-to-your-directory"></a>Dizine özel etki alanı adı ekleyin
Bir kiracı ve genel yönetici sahibiz, biz, Azure doğrulayabilmeniz bizim özel etki alanı eklemeniz gerekir.  Şunları yapın:

1. Geri [Azure portalında](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) kapattığınızdan emin olun **tüm kullanıcılar** dikey penceresi.
2. Sol taraftan **Özel etki alanı adları**'nı seçin.
3. **Özel etki alanı ekle**'yi seçin.</br>
![Federasyon](media/tutorial-federation/custom1.png)</br>
4. Üzerinde **özel etki alanı adları**, özel etki alanınızın adını kutuya girin ve tıklayın **etki alanı Ekle**.
5. Özel etki alanı adı ekranda TXT veya MX bilgilerle sunulacaktır.  Bu bilgiler, etki alanındaki etki alanı kayıt şirketi DNS bilgilerinin eklenmesi gerekir.  İçin etki alanı kayıt şirketinizde geçmeniz gerekmez, TXT veya MX bilgileri etki alanınız için DNS ayarlarını girin.  Bu, etki alanını doğrulamak Azure olanak tanır.  Bu, Azure doğrulamak 24 saate kadar sürebilir.  Daha fazla bilgi için [özel bir etki alanı ekleme](../../active-directory/fundamentals/add-custom-domain.md) belgeleri.</br>
![Federasyon](media/tutorial-federation/custom2.png)</br>
6. Doğrulandığından emin olmak için Doğrula düğmesine tıklayın.</br>
![Federasyon](media/tutorial-federation/custom3.png)</br>

## <a name="download-and-install-azure-ad-connect"></a>Azure AD Connect yükleme ve indirme
Artık Azure AD Connect karşıdan yüklenip kurulacak zamanı geldi.  Yüklendikten sonra hızlı yükleme çalıştıracağız.  Şunları yapın:

1. İndirme [Azure AD'ye bağlanma](https://www.microsoft.com/download/details.aspx?id=47594)
2. **AzureADConnect.msi** öğesine gidin ve çift tıklayın.
3. Hoş Geldiniz ekranında, lisans koşullarını kabul ettiğinizi belirten kutuyu seçin ve **Devam**'a tıklayın.  
4. Hızlı ayarlar ekranında tıklayın **Özelleştir**.  
5. Gerekli bileşenleri yükleme ekranında. **Yükle**'ye tıklayın.  
6. Kullanıcı oturum açma ekranında seçin **AD FS ile Federasyon** tıklatıp **sonraki**.
![Federasyon](media/tutorial-federation/fed1.png)

1. Azure AD ekranına bağlanma, kullanıcı ve yukarıda oluşturduğumuz genel yönetici parolasını girin ve tıklatın **sonraki**.
2. Dizinleri ekranınıza Connect üzerinde tıklayın **Dizin Ekle**.  Ardından **yeni AD hesabı oluştur** contoso\Administrator kullanıcı adı ve parola girip __iade **Tamam**.
3. **İleri**’ye tıklayın.
4. Azure AD oturum açma yapılandırması ekranında seçin **tüm UPN soneki, doğrulanmış etki alanlarına eşleme olmadan devam** tıklatıp **sonraki.**
5. Etki alanı ve OU filtreleme ekran'ı **sonraki**.
6. Kullanıcıların ekranın tanıtan üzerinde ' a tıklayın **sonraki**.
7. Filtre kullanıcılar ve cihazlar ekranının tıklayın **sonraki**.
8. İsteğe bağlı özellikler ekranında tıklatın **sonraki**.
9. Etki alanı yöneticisi kimlik bilgileri sayfasında contoso\Administrator kullanıcı adını ve parolasını girin ve tıklayın **sonraki.**
10. AD FS grubu ekranda emin **yeni bir AD FS grubunu yapılandırma** seçilir.
11. Seçin **Federasyon sunucularında yüklü bir sertifika kullanmak** tıklatıp **Gözat**.
12. Arama kutusuna DC1 ve anlaşıldığında seçin.  **Tamam**’a tıklayın.
13. Gelen **sertifika dosyası** açılan listesinde, select **adfs.contoso.com** yukarıda oluşturduğumuz sertifika.  **İleri**’ye tıklayın.
![Federasyon](media/tutorial-federation/fed2.png)

1. AD FS sunucusu ekrana tıklayın **Gözat** ve arama kutusuna DC1 ve anlaşıldığında seçin.  **Tamam**’a tıklayın.  **İleri**’ye tıklayın.
![Federasyon](media/tutorial-federation/fed3.png)

1. Web uygulaması Proxy sunucuları ekrana tıklayın **sonraki**.
2. AD FS hizmet hesabı ekranında, contoso\Administrator kullanıcı adını ve parolasını girin ve tıklayın **sonraki.**
3. Azure AD etki alanı ekranında, açılan listeden doğrulanmış özel etki alanınızı seçin ve **sonraki**.
4. Yapılandırma için hazır ekranında **Yükle**'ye tıklayın.
5. Yükleme tamamlandığında **Çıkış**'a tıklayın.
6. Yükleme tamamlandıktan sonra oturumu kapatın ve Eşitleme Hizmeti Yöneticisi'ni veya Synchronization Rule Editor'ı kullanmadan önce yeniden oturum açın.


## <a name="verify-users-are-created-and-synchronization-is-occurring"></a>Kullanıcılar oluşturulur ve eşitleme gerçekleşen doğrulayın
Artık, şirket içi dizininde vardı kullanıcıları eşitlenmiştir ve artık Azure AD kiracısı mevcut doğrulayacağız.  Bunun tamamlanması birkaç saat sürebileceğini unutmayın.  Kullanıcı eşitlenmişse doğrulamak için aşağıdakileri yapın.


1. Gözat [Azure portalında](https://portal.azure.com) ve bir Azure aboneliğine sahip bir hesapla oturum açın.
2. Sol tarafta, seçin **Azure Active Directory**
3. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
4. Yeni kullanıcılar kendi kiracısında gördüğünüzü doğrulayın ![eşitlemesi](media/tutorial-password-hash-sync/synch1.png)

## <a name="test-signing-in-with-one-of-our-users"></a>Kullanıcılarımızın biriyle oturum açma testi

1.  Göz atın [https://myapps.microsoft.com](httpss://myapps.microsoft.com)
2. Sunduğumuz yeni kiracıda oluşturduğunuz kullanıcı hesabı ile oturum.  Aşağıdaki biçimi kullanarak oturum açmanız gerekir: (user@domain.onmicrosoft.com). Kullanıcının oturum açmak için kullandığı aynı parolayı kullanan şirket içi.
![Doğrulayın](media/tutorial-password-hash-sync/verify1.png)

Şimdi başarıyla test edin ve hangi Azure sunduğu ile kendinizi alıştırın için kullanabileceğiniz bir karma kimlik ortamı vardır.

## <a name="next-steps"></a>Sonraki Adımlar

- [Donanım ve önkoşullar](how-to-connect-install-prerequisites.md) 
- [Özelleştirilmiş ayarlar](how-to-connect-install-custom.md)
- [Azure AD Connect ve federasyon](how-to-connect-fed-whatis.md)

