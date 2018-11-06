---
title: Güvenlik Merkezi'nde Azure disk şifrelemesi Uygula | Microsoft Docs
description: Bu belge Azure Güvenlik Merkezi önerinin nasıl uygulanacağını gösterir **disk şifrelemesi Uygula**.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 6cc7824a-8d6b-4a5f-ab40-e3bbaebc4a91
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 4ae3dd0446c2e2aaa35f3ae1322834881ec2009c
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51012165"
---
# <a name="apply-disk-encryption-in-azure-security-center"></a>Güvenlik Merkezi'nde Azure disk şifrelemesi Uygula
Azure Güvenlik Merkezi, Azure Disk şifrelemesi kullanılarak şifrelenmemiş Windows veya Linux VM disk varsa disk şifrelemesi Uygula önerir. Disk şifrelemesi, Windows ve Linux Iaas sanal makine disklerinizi şifreleyin sağlar.  Şifreleme hem işletim sistemi hem de VM’nizin üzerindeki veri birimleri için önerilir.

Disk şifrelemesi, endüstri standardı kullanan [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) Windows özelliğidir ve [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux özelliğidir. Bu özellikler, korumak ve verilerinizi koruyun ve organizasyonel güvenlik ve uyumluluk yükümlülüklerinizin yerine için işletim sistemi ve veri şifrelemesi sağlar. Disk şifrelemesi ile tümleşiktir [Azure anahtar kasası](https://azure.microsoft.com/documentation/services/key-vault/) denetlemenize ve disk şifreleme anahtarlarını ve gizli anahtarları Key Vault aboneliğinizdeki yönetmenize yardımcı olmak için çalışırken, bölgesinde,bekleyenVMdisklerinitümverilerinşifrelenmesinisağlama[Azure depolama](https://azure.microsoft.com/documentation/services/storage/).

> [!NOTE]
> Azure Disk şifrelemesi, aşağıdaki Windows server işletim sistemlerinde - Windows Server 2008 R2, Windows Server 2012 ve Windows Server 2012 R2 desteklenir. Disk şifrelemesi, aşağıdaki Linux sunucusu işletim sistemlerinde - Ubuntu, CentOS, SUSE ve SUSE Linux Enterprise Server (SLES) desteklenir.
>
>

## <a name="implement-the-recommendation"></a>Önerisini uygulama
1. İçinde **önerileri** dikey penceresinde **disk şifrelemesi Uygula**.
2. İçinde **disk şifrelemesi Uygula** dikey penceresinde, Disk şifreleme önerilir VM'lerin bir listesini görürsünüz.
3. Bu Vm'lere şifre uygulamak için yönergeleri izleyin.

![][1]

Güvenlik Merkezi tarafından şifreleme gerektiği belirlenen Azure Virtual Machines'i şifrelemek için aşağıdaki adımları öneriyoruz:

* Azure PowerShell'i yükleyip yapılandırın. Bu, Azure Virtual Machines şifreleme için gereken önkoşulları ayarlamaya yönelik gerekli PowerShell komutlarını çalıştırmanıza olanak sağlar.
* Edinin ve Azure Disk şifrelemesi önkoşulları Azure PowerShell Betiği çalıştırın.
* Sanal makinelerinizi şifreleyin.

[Azure PowerShell ile bir Windows Iaas VM şifreleme](../security/quick-encrypt-vm-powershell.md) Bu adımlarda size yol gösterir. Bu konuda, disk şifrelemesi'ni yapılandırma bir Windows istemci makinesi kullandığınızı varsayar.

Azure sanal makineler için kullanılabilecek birçok yaklaşım vardır. Azure PowerShell veya Azure CLI konusunda zaten bilgiliyseniz alternatif yaklaşımlar kullanmayı tercih edebilirsiniz. Bu diğer yaklaşımlar hakkında bilgi edinmek için [Azure disk şifrelemesi](../security/azure-security-disk-encryption.md).

## <a name="see-also"></a>Ayrıca bkz.
Bu belgede Güvenlik Merkezi önerisini "Uygula disk şifrelemesi." uygulama nasıl oluşturulacağını gösterir Disk şifrelemesi hakkında daha fazla bilgi için şunlara bakın:

* [Azure Key Vault ile şifreleme ve anahtar yönetimi](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (video 36 dakika 39 saniye)--korumak ve verilerinizi korumanıza yardımcı olması için disk şifreleme Yönetimi Iaas Vm'leri ve Azure anahtar kasası için kullanmayı öğrenin.
* [Azure disk şifrelemesi](../security/azure-security-disk-encryption-overview.md) (belge) Windows ve Linux Vm'leri için disk şifrelemesini etkinleştirmeyi öğrenin.

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) --güvenlik ilkelerinin nasıl yapılandırılacağını öğrenin.
* [Güvenlik durumunu, Azure Güvenlik Merkezi'nde izleme](security-center-monitoring.md) --Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) --önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) --Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.

<!--Image references-->
[1]: ./media/security-center-apply-disk-encryption/apply-disk-encryption.png
