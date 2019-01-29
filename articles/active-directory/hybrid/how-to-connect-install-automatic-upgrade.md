---
title: 'Azure AD Connect: Otomatik yükseltme | Microsoft Docs'
description: Bu konu, Azure AD Connect Eşitleme'deki yerleşik Otomatik yükseltme özelliğini açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 6b395e8f-fa3c-4e55-be54-392dd303c472
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/26/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 208ef56f05eb8f9c6730e2390e428e6b8bda0592
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55182090"
---
# <a name="azure-ad-connect-automatic-upgrade"></a>Azure AD Connect: Otomatik yükseltme
Bu özellik, derleme ile tanıtılan [(Şubat 2016'da yayımlanan) 1.1.105.0](reference-connect-version-history.md#111050).  Bu özellik, güncelleştirildiği [1.1.561 derleme](reference-connect-version-history.md#115610) ve artık daha önce desteklenmiyordu ek senaryoları destekler.

## <a name="overview"></a>Genel Bakış
Azure AD Connect yüklemenizin güncel olduğundan her zaman emin olamazdı ile daha kolay **otomatik yükseltme** özelliği. Bu özellik, express yüklemeleri için varsayılan olarak etkindir ve DirSync yükseltir. Yeni bir sürümü yayımlandığında, yükleme otomatik olarak yükseltilir.
Otomatik yükseltme, aşağıdakiler için varsayılan olarak etkindir:

* Ayarları yükleme ve DirSync yükseltmeleri express.
* SQL Express LocalDB kullanımıyla, hangi her zaman hızlı ayarları kullan olduğu. DirSync ile SQL Express LocalDB de kullanabilirsiniz.
* Hızlı ayarları ve DirSync tarafından oluşturulan varsayılan MSOL_ hesabı AD hesaptır.
* 100. 000'den az nesneler meta veri deposunda sahip.

Otomatik yükseltme geçerli durumu PowerShell cmdlet'iyle görüntülenebilir `Get-ADSyncAutoUpgrade`. Bunu, aşağıdaki durumlara sahiptir:

| Durum | Açıklama |
| --- | --- |
| Etkin |Otomatik yükseltme etkinleştirilir. |
| Askıya Alındı |Yalnızca sistem tarafından ayarlayın. Sistem **şu anda** otomatik güncelleştirmeleri almak uygun. |
| Devre dışı |Otomatik yükseltme devre dışı bırakıldı. |

Arasında değiştirebilirsiniz **etkin** ve **devre dışı bırakılmış** ile `Set-ADSyncAutoUpgrade`. Sistem durumu ayarlamalıdır yalnızca **askıya alındı**.

Otomatik yükseltme, Azure AD Connect Health için yükseltme altyapı kullanıyor. İş, emin olmak otomatik yükseltme için proxy sunucusu URL'leri açtığınız **Azure AD Connect Health** açıklandığı gibi [Office 365 URL'leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).


Varsa **Eşitleme Hizmeti Yöneticisi** kullanıcı Arabirimi, sunucu üzerinde çalıştığını ve ardından yükseltme UI kapatılana kadar askıya alındı.

## <a name="troubleshooting"></a>Sorun giderme
Connect yüklemenizi kendisini beklendiği gibi değil yükseltirseniz, ardından neyin yanlış olabilir ve çıkış bulmak için aşağıdaki adımları izleyin.

İlk olarak, yeni bir sürüm yayımlandıktan ilk gününden denenmesi için Otomatik yükseltme beklememeniz gerekir. Yükseltme yüklemenizi hemen yükselttiyseniz değil paniğe denenmeden önce kasıtlı bir rastgele olma durumu yoktur.

Doğru değildir düşünüyorsanız, ilk çalıştırma `Get-ADSyncAutoUpgrade` için Otomatik yükseltme etkinleştirildiğinden emin olun.

Ardından, Ara sunucunuzda veya güvenlik duvarı içinde gerekli URL'leri açtığınız emin olun. Otomatik güncelleştirme açıklandığı gibi Azure AD Connect Health'i kullanarak [genel bakış](#overview). Bir ara sunucu kullanırsanız, sistem durumu kullanmak için yapılandırılmış emin olun. bir [proxy sunucusu](how-to-connect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). Ayrıca, test [sistem bağlantı](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) Azure AD'ye.

Azure AD'de doğrulanmış bir bağlantıyla, bunu eventlogs bırakma zamanı geldi. Olay Görüntüleyicisi'ni başlatın ve konum **uygulama** eventlog. Kaynağı için bir olay günlüğü Filtresi Ekle **Azure AD Connect Yükseltmesi** ve olay kimliği aralığını **300-399**.  
![Otomatik yükseltme için olay günlüğü filtresi](./media/how-to-connect-install-automatic-upgrade/eventlogfilter.png)  

Artık, Otomatik yükseltme durumu ile ilişkili eventlogs görebilirsiniz.  
![Otomatik yükseltme için olay günlüğü filtresi](./media/how-to-connect-install-automatic-upgrade/eventlogresult.png)  

Sonuç kodu durumunun genel bir bakış ile bir ön eki vardır.

| Sonuç kodu öneki | Açıklama |
| --- | --- |
| Başarılı |Yükleme başarıyla yükseltildi. |
| UpgradeAborted |Geçici bir durum yükseltme durduruldu. Yeniden denenecek ve daha sonra başarılı beklenir. |
| UpgradeNotSupported |Sistem otomatik olarak yükseltilmekte olan sistemin engelleyen bir yapılandırması vardır. Durum değiştirme ancak sistemin el ile yükseltilmesi gereken beklenir görmek için denenecek. |

Bulduğunuz en yaygın iletilerinin bir listesi aşağıda verilmiştir. Tüm listelenmez, ancak sonuç iletisi Temizle hangi sorun olmalıdır.

| Sonuç iletisi | Açıklama |
| --- | --- |
| **UpgradeAborted** | |
| UpgradeAbortedCouldNotSetUpgradeMarker |Kayıt defterine yazılamadı. |
| UpgradeAbortedInsufficientDatabasePermissions |Yerleşik Yöneticiler grubunun veritabanı izinleri yok. El ile bu sorunu gidermek için Azure AD Connect'in en son sürüme yükseltin. |
| UpgradeAbortedInsufficientDiskSpace |Yükseltme desteklemek için yeterli disk alanı yok. |
| UpgradeAbortedSecurityGroupsNotPresent |Değil bulun ve tüm güvenlik grupları eşitleme altyapısı tarafından kullanılan çözümleyin. |
| UpgradeAbortedServiceCanNotBeStarted |NT hizmetinin **Microsoft Azure AD eşitleme** başlatılamadı. |
| UpgradeAbortedServiceCanNotBeStopped |NT hizmetinin **Microsoft Azure AD eşitleme** durdurulamadı. |
| UpgradeAbortedServiceIsNotRunning |NT hizmetinin **Microsoft Azure AD eşitleme** çalışmıyor. |
| UpgradeAbortedSyncCycleDisabled |SyncCycle seçeneğinde [Zamanlayıcı](how-to-connect-sync-feature-scheduler.md) devre dışı bırakıldı. |
| UpgradeAbortedSyncExeInUse |[Eşitleme Hizmeti Yöneticisi kullanıcı Arabirimi](how-to-connect-sync-service-manager-ui.md) sunucuda açılır. |
| UpgradeAbortedSyncOrConfigurationInProgress |Yükleme Sihirbazı'nı çalıştıran veya eşitleme dışında Zamanlayıcı zamanlandı. |
| **UpgradeNotSupported** | |
| UpgradeNotSupportedAdfsSignInMethod | Adfs oturum açma yöntemi olarak seçtiniz. | 
| UpgradeNotSupportedCustomizedSyncRules |Kendi özel kurallarınızı yapılandırmaya ekledik. |
| UpgradeNotSupportedDeviceWritebackEnabled |Etkinleştirdiğiniz [cihaz geri yazmayı](how-to-connect-device-writeback.md) özelliği. |
| UpgradeNotSupportedGroupWritebackEnabled |Etkinleştirdiğiniz [grup geri yazma](how-to-connect-preview.md#group-writeback) özelliği. |
| UpgradeNotSupportedInvalidPersistedState |Kurulum, hızlı ayarları veya Dırsync yükseltmesi değil. |
| UpgradeNotSupportedMetaverseSizeExceeeded |Meta veri deposunda 100. 000'den fazla nesne var. |
| UpgradeNotSupportedMultiForestSetup |Birden fazla ormana bağlanırsınız. Hızlı Kurulum yalnızca tek bir ormana bağlanır. |
| UpgradeNotSupportedNonLocalDbInstall |SQL Server Express LocalDB veritabanına kullanmıyorsunuz demektir. |g
| UpgradeNotSupportedNonMsolAccount |[AD DS bağlayıcı hesabı](reference-connect-accounts-permissions.md#ad-ds-connector-account) varsayılan MSOL_ hesabı artık değil. |
| UpgradeNotSupportedNotConfiguredSignInMethod | AAD Connect ayarlarken seçtiğiniz *yapılandırmazsanız* oturum açma yöntemi seçilirken. | 
| UpgradeNotSupportedPtaSignInMethod | Geçişli kimlik doğrulaması oturum açma yöntemi olarak seçtiniz. |
| UpgradeNotSupportedStagingModeEnabled |Sunucu olması ayarlandığından [hazırlama modunda](how-to-connect-sync-operations.md#staging-mode). |
| UpgradeNotSupportedUserWritebackEnabled |Etkinleştirdiğiniz [kullanıcı geri yazma](how-to-connect-preview.md#user-writeback) özelliği. |

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
