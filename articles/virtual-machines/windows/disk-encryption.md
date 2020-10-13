---
title: Azure yönetilen disklerinin sunucu tarafı şifrelemesi-PowerShell
description: Azure depolama, verilerinizi depolama kümelerine kalıcı yapmadan önce Rest durumunda şifreleyerek korur. Yönetilen disklerinizin şifrelenebilmesi için Microsoft tarafından yönetilen anahtarları kullanabilir veya kendi anahtarınızla şifrelemeyi yönetmek için müşteri tarafından yönetilen anahtarları kullanabilirsiniz.
author: roygara
ms.date: 09/23/2020
ms.topic: conceptual
ms.author: rogarana
ms.service: virtual-machines
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 49898f2215139c4460c639dc8caa3d2764082e34
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91274784"
---
# <a name="server-side-encryption-of-azure-disk-storage-for-powershell"></a>PowerShell için Azure Disk Depolama sunucu tarafı şifrelemesi

Sunucu tarafı şifreleme (SSE) verilerinizi korur ve kurumsal güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olur. SSE, Azure yönetilen disklerinde (işletim sistemi ve veri diskleri) depolanan verilerinizi buluta kalıcı hale getirerek varsayılan olarak otomatik olarak şifreler. 

Azure yönetilen disklerindeki veriler, 256 bit [AES şifrelemesi](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)kullanılarak saydam bir şekilde şifrelenir, en güçlü blok şifrelemeleri KULLANILABILIR ve FIPS 140-2 uyumludur. Azure yönetilen diskleri temel alan şifreleme modülleri hakkında daha fazla bilgi için bkz [. şifreleme API 'si: yeni nesil](/windows/desktop/seccng/cng-portal)

Sunucu tarafı şifreleme, yönetilen disklerin performansını etkilemez ve ek bir maliyet yoktur. 

> [!NOTE]
> Geçici diskler yönetilen diskler değildir ve konakta şifrelemeyi etkinleştirmediğiniz takdirde SSE tarafından şifrelenmez.

## <a name="about-encryption-key-management"></a>Şifreleme anahtarı yönetimi hakkında

Yönetilen diskinizin şifrelenmesi için platform tarafından yönetilen anahtarları kullanabilir veya kendi anahtarlarınızı kullanarak şifrelemeyi yönetebilirsiniz. Şifrelemeyi kendi anahtarlarınız ile yönetmeyi seçerseniz, yönetilen disklerdeki tüm verileri şifrelemek ve şifrelerini çözmek için kullanılacak *müşteri tarafından yönetilen bir anahtar* belirtebilirsiniz. 

Aşağıdaki bölümlerde, anahtar yönetimiyle ilgili seçeneklerin her biri daha ayrıntılı olarak açıklanır.

### <a name="platform-managed-keys"></a>Platform tarafından yönetilen anahtarlar

Varsayılan olarak, yönetilen diskler platform tarafından yönetilen şifreleme anahtarlarını kullanır. Mevcut yönetilen disklere yazılan tüm yönetilen diskler, anlık görüntüler, görüntüler ve veriler, platform tarafından yönetilen anahtarlarla otomatik olarak şifrelenir.

### <a name="customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlar

[!INCLUDE [virtual-machines-managed-disks-description-customer-managed-keys](../../../includes/virtual-machines-managed-disks-description-customer-managed-keys.md)]

#### <a name="restrictions"></a>Kısıtlamalar

Şimdilik, müşteri tarafından yönetilen anahtarlar aşağıdaki kısıtlamalara sahiptir:

- Diskiniz için bu özellik etkinleştirilirse, devre dışı bırakılamaz.
    Bu sorunu geçici olarak çözmek için, [tüm verileri](disks-upload-vhd-to-managed-disk-powershell.md#copy-a-managed-disk) , müşteri tarafından yönetilen anahtarları kullanmayan tamamen farklı bir yönetilen diske kopyalamanız gerekir.
[!INCLUDE [virtual-machines-managed-disks-customer-managed-keys-restrictions](../../../includes/virtual-machines-managed-disks-customer-managed-keys-restrictions.md)]

#### <a name="supported-regions"></a>Desteklenen bölgeler

Müşterinin yönettiği anahtarlar, yönetilen disklerin kullanılabildiği tüm bölgelerde kullanılabilir.

## <a name="encryption-at-host---end-to-end-encryption-for-your-vm-data"></a>VM verileriniz için ana bilgisayar uçtan uca şifreleme

Uçtan uca şifreleme, sanal makinenizin ayrıldığı Azure sunucusu olan VM ana bilgisayarında başlar. Geçici disklerinizdeki veriler, kısa ömürlü IŞLETIM sistemi diskleri ve kalıcı işletim sistemi/veri diski önbellekler söz konusu VM ana bilgisayarında depolanır. Uçtan uca şifrelemeyi etkinleştirdiğinizde, tüm bu veriler Rest 'de şifrelenir ve depolama hizmetine şifrelenmiş akışlar, burada kalıcı hale gelir. Uçtan uca şifreleme, sanal makinenizin CPU 'sunu kullanmaz ve VM 'nizin performansını etkilemez. 

Uçtan uca şifrelemeyi etkinleştirdiğinizde, geçici diskler ve kısa ömürlü işletim sistemi diskleri, platformda yönetilen anahtarlarla birlikte şifrelenir. İşletim sistemi ve veri diski önbellekleri, şifreleme türüne bağlı olarak, müşteri tarafından yönetilen ya da platform tarafından yönetilen anahtarlarla geri kalan bilgisayarlarda şifrelenir. Örneğin, bir disk, müşteri tarafından yönetilen anahtarlarla şifrelenirse, disk için önbellek, müşteri tarafından yönetilen anahtarlarla şifrelenir ve bir disk, platform tarafından yönetilen anahtarlarla şifrelenirse, diskin önbelleği, platform tarafından yönetilen anahtarlarla şifrelenir.

### <a name="restrictions"></a>Kısıtlamalar

[!INCLUDE [virtual-machines-disks-encryption-at-host-restrictions](../../../includes/virtual-machines-disks-encryption-at-host-restrictions.md)]

#### <a name="supported-regions"></a>Desteklenen bölgeler

[!INCLUDE [virtual-machines-disks-encryption-at-host-regions](../../../includes/virtual-machines-disks-encryption-at-host-regions.md)]

#### <a name="supported-vm-sizes"></a>Desteklenen VM boyutları

[!INCLUDE [virtual-machines-disks-encryption-at-host-suported-sizes](../../../includes/virtual-machines-disks-encryption-at-host-suported-sizes.md)]

## <a name="double-encryption-at-rest"></a>Rest 'te Çift şifreleme

Herhangi bir şifreleme algoritması, uygulama veya anahtarla ilişkili riskten endişe duyan yüksek güvenlikli duyarlı müşteriler artık, platform yönetilen şifreleme anahtarlarını kullanan altyapı katmanında farklı bir şifreleme algoritması/modu kullanarak ek şifreleme katmanını kabul edebilir. Bu yeni katman, kalıcı işletim sistemi ve veri diskleri, anlık görüntüler ve görüntülere, hepsi de çift şifrelemeyle birlikte şifrelenecek şekilde uygulanabilir.

### <a name="supported-regions"></a>Desteklenen bölgeler

[!INCLUDE [virtual-machines-disks-double-encryption-at-rest-regions](../../../includes/virtual-machines-disks-double-encryption-at-rest-regions.md)]

## <a name="server-side-encryption-versus-azure-disk-encryption"></a>Sunucu tarafı şifrelemesi ile Azure disk şifrelemesi karşılaştırması

[Azure disk şifrelemesi](../../security/fundamentals/azure-disk-encryption-vms-vmss.md) , yönetilen diskleri, Konuk VM içindeki müşteri tarafından yönetilen anahtarlarla şifrelemek için Windows 'un [BitLocker](/windows/security/information-protection/bitlocker/bitlocker-overview) özelliğinden yararlanır. Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme, depolama hizmetindeki verileri şifreleyerek VM 'niz için herhangi bir işletim sistemi türünü ve görüntüsünü kullanmanızı sağlayarak ADE 'yi geliştirir.

> [!IMPORTANT]
> Müşteri tarafından yönetilen anahtarlar, Azure Active Directory (Azure AD) bir özelliği olan Azure kaynakları için yönetilen kimliklere bağımlıdır. Müşteri tarafından yönetilen anahtarları yapılandırırken, bir yönetilen kimlik, kapsamakta olan kaynaklara otomatik olarak atanır. Daha sonra aboneliği, kaynak grubunu veya yönetilen diski bir Azure AD dizininden diğerine taşırsanız, yönetilen disklerle ilişkili yönetilen kimlik yeni kiracıya aktarılmaz, böylelikle müşterinin yönettiği anahtarlar artık çalışmayabilir. Daha fazla bilgi için bkz. [Azure AD dizinleri arasında abonelik aktarma](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories).


## <a name="next-steps"></a>Sonraki adımlar

- [PowerShell](disks-enable-host-based-encryption-powershell.md) veya [Azure Portal](../disks-enable-host-based-encryption-portal.md)ile konaktaki şifrelemeyi kullanarak uçtan uca şifrelemeyi etkinleştirin.
- [PowerShell](disks-enable-double-encryption-at-rest-powershell.md) veya [Azure Portal](../disks-enable-double-encryption-at-rest-portal.md)ile yönetilen diskler için bekleyen çift şifrelemeyi etkinleştirin.
- [PowerShell](disks-enable-customer-managed-keys-powershell.md) veya [Azure Portal](../disks-enable-customer-managed-keys-portal.md)yönetilen diskler için müşteri tarafından yönetilen anahtarları etkinleştirin.
- [Müşteri tarafından yönetilen anahtarlarla şifrelenmiş diskler oluşturmak için Azure Resource Manager şablonlarını keşfet](https://github.com/ramankumarlive/manageddiskscmkpreview)
- [Azure Key Vault nedir?](../../key-vault/general/overview.md)
