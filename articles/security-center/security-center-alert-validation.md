---
title: Azure Güvenlik Merkezi 'nde uyarı doğrulaması (EICAR test dosyası) | Microsoft Docs
description: Bu belge, Azure Güvenlik Merkezi'nde güvenlik uyarılarını doğrulamanıza yardımcı olur.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: f8f17a55-e672-4d86-8ba9-6c3ce2e71a57
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/22/2020
ms.author: memildin
ms.openlocfilehash: ac76ce5d4fb788e6c2fb3dc9ec01c8d88bfb55cb
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91268086"
---
# <a name="alert-validation-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde uyarı doğrulama
Bu belge, sisteminizin Azure Güvenlik Merkezi uyarıları için doğru yapılandırılıp yapılandırılmadığını doğrulamayı öğrenmenize yardımcı olur.

## <a name="what-are-security-alerts"></a>Güvenlik uyarıları nedir?
Uyarılar, Güvenlik Merkezi’nin kaynaklarınızda tehditler algıladığında oluşturduğu bildirimlerdir. Sorunu hızlı bir şekilde araştırmak için gereken bilgilerle birlikte uyarıları önceliklendirir ve listeler. Güvenlik Merkezi ayrıca bir saldırıyı nasıl düzeltebileceğiniz konusunda öneriler sağlar.
Daha fazla bilgi için bkz. Güvenlik [Merkezi 'Nde güvenlik uyarıları](security-center-alerts-overview.md) ve [güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)

## <a name="alert-validation"></a>Uyarı doğrulaması

* [Windows](#validate-windows)
* [Linux](#validate-linux)
* [Kubernetes](#validate-kubernetes)

## <a name="validate-alerts-on-windows-vms"></a>Windows VM 'lerinde uyarıları doğrulama <a name="validate-windows"></a>

Güvenlik Merkezi Aracısı bilgisayarınıza yüklendikten sonra, uyarının saldırıya uğrayan kaynak olmasını istediğiniz bilgisayarda bu adımları izleyin:

1. Bir yürütülebilir dosyayı (örneğin **calc.exe**) bilgisayarın masaüstüne veya sizin rahatınızın diğer dizinine kopyalayın ve **ASC_AlertTest_662jfi039N.exe**olarak yeniden adlandırın.
1. Komut istemi ' ni açın ve bu dosyayı aşağıdaki gibi bir bağımsız değişkenle (yalnızca sahte bir bağımsız değişken adı) yürütün: ```ASC_AlertTest_662jfi039N.exe -foo```
1. 5-10 dakika bekleyin ve Güvenlik Merkezi Uyarılarını açın. Bir uyarı görüntülenmelidir.

> [!NOTE]
> Windows için bu test uyarısını gözden geçirirken, alan **bağımsız değişkenlerinin denetim etkin** **olduğundan emin olun.** **Yanlış**ise, komut satırı bağımsız değişken denetimini etkinleştirmeniz gerekir. Etkinleştirmek için aşağıdaki komutu kullanın:
>
>```reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"```

## <a name="validate-alerts-on-linux-vms"></a>Linux VM 'lerinde uyarıları doğrulama <a name="validate-linux"></a>

Güvenlik Merkezi Aracısı bilgisayarınıza yüklendikten sonra, uyarının saldırıya uğrayan kaynak olmasını istediğiniz bilgisayarda bu adımları izleyin:
1. Bir yürütülebilir dosyayı uygun bir konuma kopyalayın ve **./asc_alerttest_662jfi039n**olarak yeniden adlandırın, örneğin:

    ```cp /bin/echo ./asc_alerttest_662jfi039n```

1. Komut istemi 'ni açın ve bu dosyayı yürütün:

    ```./asc_alerttest_662jfi039n testing eicar pipe```

1. 5-10 dakika bekleyin ve Güvenlik Merkezi Uyarılarını açın. Bir uyarı görüntülenmelidir.


## <a name="validate-alerts-on-kubernetes"></a>Kubernetes üzerinde uyarıları doğrulama <a name="validate-kubernetes"></a>

Azure Kubernetes hizmetini Güvenlik Merkezi ile tümleştirdiyseniz, uyarılarınızın aşağıdaki kubectl komutuyla çalıştığını test edebilirsiniz:

```kubectl get pods --namespace=asc-alerttest-662jfi039n```

Kubernetes düğümlerini ve kümelerini erteleme hakkında daha fazla bilgi için bkz. [Kubernetes Için Azure Defender 'A giriş](defender-for-kubernetes-introduction.md)

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede uyarıları doğrulama işlemine giriş yaptınız. Artık bu doğrulama hakkında bilgi sahibi olduğunuza göre, aşağıdaki makaleleri deneyebilirsiniz:

* [Azure Güvenlik Merkezi 'nde tehdit algılamayı Azure Key Vault doğrulama](https://techcommunity.microsoft.com/t5/azure-security-center/validating-azure-key-vault-threat-detection-in-azure-security/ba-p/1220336)
* [Azure Güvenlik Merkezi 'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -Güvenlik Merkezi 'nde uyarıları yönetme ve güvenlik olaylarına yanıt verme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi 'Nde güvenlik durumu izleme](security-center-monitoring.md) -Azure kaynaklarınızın sistem durumunu izlemeyi öğrenin.
* [Azure Güvenlik Merkezi 'nde güvenlik uyarılarını anlama](security-center-alerts-type.md) -farklı güvenlik uyarısı türleri hakkında bilgi edinin.