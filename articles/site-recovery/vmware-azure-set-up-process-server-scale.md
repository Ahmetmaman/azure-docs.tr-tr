---
title: Azure'da bir işlem sunucusu VMware VM ve Azure Site Recovery ile fiziksel sunucuda yeniden çalışma için ayarlama | Microsoft Docs
description: Bu makalede, Azure Vm'leri vmware'e yeniden çalışma, azure'da bir işlem sunucusu ayarlamak açıklar.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: 6d3fe519729bd56dafd11720a3662eb00b916a98
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39056618"
---
# <a name="set-up-additional-process-servers-for-scalability"></a>Ölçeklenebilirlik için ek işlem sunucularını ayarlama

Varsayılan olarak, VMware Vm'lerini veya fiziksel sunucuları azure'a çoğaltırken [Site Recovery](site-recovery-overview.md), bir işlem sunucusu yapılandırma sunucusu makineye yüklenir ve Site Recovery arasında veri aktarımını koordine etmek için kullanılır ve Şirket içi altyapınızı. Kapasite ve çoğaltma dağıtımınızın ölçeğini artırmak için ek bağımsız işlem sunucuları ekleyebilirsiniz. Bu makalede bunun nasıl yapılacağı açıklanır.

## <a name="before-you-start"></a>Başlamadan önce

### <a name="capacity-planning"></a>Kapasite planlaması

Gerçekleştirdiğiniz emin [kapasite planlaması](site-recovery-plan-capacity-vmware.md) VMware çoğaltması için. Bu, tanımlamanıza yardımcı olur nasıl ve ne zaman ek işlem sunucusu dağıtmanız gerekir.

### <a name="sizing-requirements"></a>Boyutlandırma gereksinimleri 

Tabloda özetlenen boyutlandırma gereksinimlerini doğrulayın. Genel olarak, dağıtımınızı 200'den fazla kaynak makineye ölçeklendirmek sahip olduğunuz veya günlük 2 TB'den fazla oranını karmaşıklığı toplam varsa, trafik hacmini işlemeye yetecek ek işlem sunucularının gerekir.

| **Ek işlem sunucusu** | **Önbellek diski boyutu** | **Veri değişiklik oranı** | **Korumalı makineler** |
| --- | --- | --- | --- |
|4 Vcpu (2 yuva * 2 Çekirdek \@ 2.5 GHz), 8 GB bellek |300 GB |250 GB veya daha az |85 ya da daha az makineleri çoğaltabilir. |
|8 Vcpu (2 yuva * 4 çekirdek \@ 2.5 GHz), 12 GB bellek |600 GB |250 GB ila 1 TB |85 150 makineler arasında çoğaltılır. |
|12 Vcpu (2 yuva * 6 çekirdek \@ 2.5 GHz) 24 GB bellek |1 TB |1 TB ile 2 TB |150-225 makineler arasında çoğaltılır. |

### <a name="prerequisites"></a>Önkoşullar

Ek işlem sunucusu için Önkoşulları aşağıdaki tabloda özetlenmiştir.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]


## <a name="download-installation-file"></a>Yükleme dosyasını indirin

İşlem sunucusu için yükleme dosyasını aşağıdaki gibi indirin:

1. Azure portalında oturum açın ve kurtarma Hizmetleri kasanıza göz atın.
2. Açık **Site Recovery altyapısı** > **VMWare ve fiziksel makineler** > **yapılandırma sunucusu** (altında VMware ve fiziksel Makineler için).
3. Sunucu ayrıntıları detaya gitmek için yapılandırma sunucusunu seçin. Ardından **+ işlem sunucusu**.
4. İçinde **ekleme işlem sunucusu** >  **seçin, işlem sunucunuzu dağıtmak istediğiniz**seçin **Dağıt bir genişleme işlem sunucusu şirket içi**.

  ![Sunucuları Sayfası Ekle](./media/vmware-azure-set-up-process-server-scale/add-process-server.png)
1. Tıklayın **indirme Microsoft Azure Site Recovery birleşik Kurulumu**. Bu yükleme dosyasının en son sürümünü indirir.

  > [!WARNING]
  Configuration server sürümü çalışıyor olması, işlem sunucusu yükleme sürümü aynı farklı veya daha önceki bir sürümü olmalıdır. Sürüm uyumluluğu sağlamak için basit bir yol, en son yükleme veya yapılandırma sunucunuzu güncelleştirmek için kullanılan aynı yükleyici kullanmaktır.

## <a name="install-from-the-ui"></a>Kullanıcı Arabiriminden yükleyin

Aşağıda gösterildiği gibi yükleyin. Sunucuyu ayarladıktan sonra bunu kullanmak için kaynak makineler geçirin.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-add-process-server.md)]


## <a name="install-from-the-command-line"></a>Komut satırından yükleme

Aşağıdaki komutu çalıştırarak yükleyin:

```
UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
```

Burada komut satırı parametreleri aşağıdaki gibidir:

[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]

Örneğin:

```
MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
cd C:\Temp\Extracted
UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "PS" /InstallLocation "D:\" /EnvType "VMWare" /CSIP "10.150.24.119" /PassphraseFilePath "C:\Users\Administrator\Desktop\Passphrase.txt" /DataTransferSecurePort 443
```
### <a name="create-a-proxy-settings-file"></a>Proxy ayarları dosyası oluşturma

Bir proxy ayarlamak gerekiyorsa ProxySettingsFilePath parametresi bir dosya girdi olarak alır. Dosya şu şekilde oluşturun ve bunu giriş ProxySettingsFilePath parametre olarak geçiriyoruz.

```
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address"
* ProxyPort = "Port"
* ProxyUserName="UserName"
* ProxyPassword="Password"
```

## <a name="next-steps"></a>Sonraki adımlar
Hakkında bilgi edinin [işlem sunucusu ayarlarını yönetme](vmware-azure-manage-process-server.md)
