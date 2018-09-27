---
title: Yerleşik Azure Güvenlik Merkezi için PowerShell kullanın ve ağınızı korumanıza | Microsoft Docs
description: Bu belge ekleme Azure Güvenlik Merkezi'nin PowerShell cmdlet'lerini kullanarak sürecinde yardımcı olur.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: e400fcbf-f0a8-4e10-b571-5a0d0c3d0c67
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 9/20/2018
ms.author: rkarlin
ms.openlocfilehash: 4664a9f84a92b7a223409d764971fda81317bbf0
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47222259"
---
# <a name="automate-onboarding-of-azure-security-center-using-powershell"></a>PowerShell kullanarak Azure Güvenlik Merkezi'nin ekleme otomatikleştirin

Azure Güvenlik Merkezi PowerShell modülünü kullanarak Azure iş yüklerinizi programlı olarak güvenliğini sağlayabilirsiniz.
PowerShell kullanarak, görevleri otomatikleştirme ve el ile gerçekleştirilen görevleri bu onlarca, yüzlerce ve binlerce kaynakların her biri, gelen korunmalıdır – aboneliklerle gerektiren büyük ölçekli dağıtımlarda özellikle yararlı olur, devralınan İnsan hatası kaçınmak sağlar başlangıcı.

PowerShell kullanarak ekleme Azure Güvenlik Merkezi, programlı olarak ekleme ve Azure kaynaklarınızı yönetimini otomatikleştirmek ve gerekli güvenlik denetimleri eklemek sağlar.

Bu makalede, değiştirilebilir ve ortamınızda Güvenlik Merkezi'ne gözatın aboneliklerinizde geri almak için kullanılan bir örnek PowerShell Betiği sağlanır. 

Bu örnekte Güvenlik Merkezi kimliği bir Abonelikteki etkinleştiririz: d07c0080-170c-4c24-861d-9c817742786c ve yüksek düzeyde koruma sağlayan Güvenlik Merkezi standart katmanı uygulayarak sağlayan önerilen ayarları uygula Gelişmiş tehdit koruması ve algılaması özellikleri:

1. Ayarlama [ASC standart koruma düzeyini](https://azure.microsoft.com/pricing/details/security-center/). 
 
2. Mevcut bir kullanıcı tanımlı çalışma (myWorkspace) Vm'lerinde topladığı – Bu örnekte, aboneliği ile ilişkili olduğu Microsoft Monitoring Agent gönderir Log Analytics çalışma alanını ayarlayın.

3. Güvenlik Merkezi'nin otomatik aracı, sağlamayı etkinleştirmek [Microsoft Monitoring Agent'ı dağıtır](security-center-enable-data-collection.md#auto-provision-mma).

5. Kuruluşun ayarlamak [ASC uyarıları ve önemli olayları güvenlik ilgili kişisi olarak CISO](security-center-provide-security-contact-details.md).

6. Ata Güvenlik Merkezi'nin [varsayılan güvenlik ilkeleri](security-center-azure-policy.md).

## <a name="prerequisites"></a>Önkoşullar

Güvenlik Merkezi cmdlet'leri çalıştırmadan önce bu adımlar gerçekleştirilmelidir:

1.  PowerShell'i yönetici olarak çalıştırın
2.  PowerShell'de aşağıdaki komutları çalıştırın:
      
        Install-Module -Name PowerShellGet -Force
        Set-ExecutionPolicy -ExecutionPolicy AllSigned
        Import-Module PowerShellGet
        Install-Module -Name AzureRM.profile -RequiredVersion 5.5.0
6.  PowerShell yeniden Başlat

7. PowerShell'de aşağıdaki komutları çalıştırın:

         Install-Module -Name AzureRM.Security -AllowPrerelease -Force

## <a name="onboard-security-center-using-powershell"></a>PowerShell kullanarak Güvenlik Merkezi'ne ekleme

1.  Abonelikleriniz için Güvenlik Merkezi kaynak sağlayıcısını kaydedin:

        Set-AzureRmContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c”
        Register-AzureRmResourceProvider -ProviderNamespace ‘Microsoft.Security’ 

2.  İsteğe bağlı: abonelikler kapsam düzeyini (fiyatlandırma katmanı) ayarlayın (tanımlı değilse, fiyatlandırma katmanı ücretsiz olarak ayarlanır):

        Set-AzureRmContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c”
        Set-AzureRmSecurityPricing -Name "default" -PricingTier "Standard"

3.  Aracıların rapor eder Log Analytics çalışma alanı yapılandırın. Zaten oluşturduğunuz aboneliğin Vm'leri rapor vereceği bir Log Analytics çalışma alanı olması gerekir. Birden çok abonelik aynı çalışma alanına raporlama yapacak tanımlayabilirsiniz. Tanımlı değilse, varsayılan çalışma alanı kullanılır.

        Set-AzureRmSecurityWorkspaceSetting -Name "default" -Scope
        "/subscriptions/d07c0080-170c-4c24-861d-9c817742786c" -WorkspaceId"/subscriptions/d07c0080-170c-4c24-861d-9c817742786c/resourceGroups/myRg/providers/Microsoft.OperationalInsights/workspaces/myWorkspace"

4.  Microsoft Monitoring Agent'ı Azure sanal makinelerinize otomatik sağlama yüklemesini:
    
        Set-AzureRmContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c”
    
        Set-AzureRmSecurityAutoProvisioningSetting -Name "default" -EnableAutoProvision

    > [!NOTE]
    > Azure sanal makinelerinizi otomatik olarak Azure Güvenlik Merkezi tarafından korunduğundan emin olmak otomatik sağlamayı etkinleştirmek için önerilir.
    >

5.  İsteğe bağlı: Güvenlik ilgili kişisinin bilgilerini ekleme, uyarılar ve bildirimler alıcılarını kullanılacak oluşturduğunuz abonelikler için tanımladığınız Güvenlik Merkezi tarafından önerilir:

        Set-AzureRmSecurityContact -Name "default1" -Email "CISO@my-org.com" -Phone "2142754038" -AlertsAdmin -NotifyOnAlert 

6.  Varsayılan Güvenlik Merkezi ilke girişimi atama:

        Register-AzureRmResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
        $Policy = Get-AzureRmPolicySetDefinition -Name ' [Preview]: Enable Monitoring in Azure Security Center'
        New-AzureRmPolicyAssignment -Name 'ASC Default <d07c0080-170c-4c24-861d-9c817742786c>' -DisplayName 'Security Center Default <subscription ID>' -PolicySetDefinition $Policy -Scope ‘/subscriptions/d07c0080-170c-4c24-861d-9c817742786c’

Başarıyla eklenen Azure Güvenlik Merkezi artık PowerShell ile!

Şimdi, program aracılığıyla abonelikler ve kaynaklar arasında yineleme yapmak için Otomasyon betikleri ile bu PowerShell cmdlet'lerini kullanabilirsiniz. Bu, zaman tasarrufu sağlar ve İnsan hatası olasılığını azaltır. Bu [örnek komut dosyası](https://github.com/Microsoft/Azure-Security-Center/blob/master/quickstarts/ASC-Samples.ps1) başvuru olarak.






## <a name="see-also"></a>Ayrıca bkz.
Güvenlik Merkezi'ne ekleme otomatikleştirmek için PowerShell nasıl kullanabileceğiniz hakkında daha fazla bilgi edinmek için şu makaleye bakın:

* [AzureRM.Security](https://www.powershellgallery.com/packages/AzureRM.Security/0.1.0-preview).

Güvenlik Merkezi hakkında daha fazla bilgi için şu makaleye bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
