---
title: Azure Otomasyonu 'Nu kullanarak Azure Cloud Services yönetme | Microsoft Docs
description: Azure Otomasyonu hizmetinin Azure Cloud Services 'ı ölçekte yönetmek için nasıl kullanılabileceği hakkında bilgi edinin.
services: cloud-services, automation
author: jodoglevy
manager: timlt
editor: ''
ms.assetid: 3789810a-2892-4eef-bf29-c781c1b5af48
ms.service: cloud-services
ms.topic: article
ms.date: 06/20/2016
ms.author: timlt
ms.openlocfilehash: 67830f8c00d9f74f62883e0714ffe1c2bbbd6903
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92075629"
---
# <a name="managing-azure-cloud-services-using-azure-automation"></a>Azure Otomasyonu 'Nu kullanarak Azure Cloud Services yönetme
Bu kılavuz size Azure Otomasyonu hizmetini ve Azure bulut hizmetlerinizin yönetimini basitleştirmek için nasıl kullanılabileceğini tanıtacaktır.

## <a name="what-is-azure-automation"></a>Azure Otomasyonu Nedir?
[Azure Otomasyonu](https://azure.microsoft.com/services/automation/) , işlem otomasyonu aracılığıyla bulut yönetimini basitleştirecek bir Azure hizmetidir. Azure Otomasyonu, uzun süre çalışan, el ile, hata açısından açık ve sık tekrarlanan görevler kullanmak, kuruluşunuzun güvenilirliğini, verimliliğini ve değer süresini artırmak için otomatikleştirilebilir.

Azure Otomasyonu, kuruluşunuz büyüdükçe gereksinimlerinizi karşılayacak şekilde ölçeklendirilebilen, yüksek düzeyde güvenilir ve yüksek oranda kullanılabilir bir iş akışı yürütme altyapısı sağlar. Azure Otomasyonu 'nda, süreçler el ile, 3. taraf sistemleri tarafından ya da görevlerin gerektiği zaman tam olarak gerçekleşmesi için zamanlanmış aralıklarla kapatılabilir.

İşlem yükünü düşürün ve BT/DevOps personelini, bulut yönetim görevlerinizi Azure Otomasyonu tarafından otomatik olarak çalışacak şekilde taşıyarak iş değeri ekleyen işlere odaklanmak üzere boşaltın.

## <a name="how-can-azure-automation-help-manage-azure-cloud-services"></a>Azure Otomasyonu, Azure bulut hizmetlerini nasıl yönetmenize yardımcı olabilir?
Azure Cloud Services, Azure Otomasyonu 'nda [Azure PowerShell araçlarında](/powershell/)kullanılabilen PowerShell cmdlet 'leri kullanılarak yönetilebilir. Azure Otomasyonu, hizmet içinde tüm bulut hizmeti yönetim görevlerinizi gerçekleştirebilmeniz için bu bulut hizmeti PowerShell cmdlet 'lerini kullanıma hazır olarak kullanabilir. Ayrıca, Azure hizmetleri ve üçüncü taraf sistemler genelinde karmaşık görevleri otomatikleştirmek için bu cmdlet 'leri diğer Azure hizmetleri cmdlet 'leriyle da eşleştirin.

Azure Cloud Services 'yi yönetmek için Azure Otomasyonu 'nun bazı örnekleri şunlardır:

* [Azure Blob depolama alanında cscfg veya cspkg güncelleştirildiğinde bir bulut hizmetinin sürekli dağıtımı](https://gallery.technet.microsoft.com/scriptcenter/Continuous-Deployment-of-A-eeebf3a6)
* [Bulut hizmeti örneklerini tek seferde bir yükseltme etki alanında paralel olarak yeniden başlatma](https://gallery.technet.microsoft.com/scriptcenter/Reboot-Cloud-Service-PaaS-b337a06d)

## <a name="next-steps"></a>Sonraki Adımlar
Azure Otomasyonu ile ilgili temel bilgileri ve Azure bulut hizmetlerini yönetmek için nasıl kullanılabileceğinizi öğrendiğinize göre, Azure Otomasyonu hakkında daha fazla bilgi edinmek için bu bağlantıları izleyin.

* [Azure Otomasyonu genel bakış](../automation/automation-intro.md)
* [İlk runbook’um](../automation/learn/automation-tutorial-runbook-graphical.md)