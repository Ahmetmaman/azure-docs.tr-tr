---
title: Azure hizmetleri için önerilen ilkeler
description: Azure sanal makineleri gibi Azure hizmetleri için önerilen ilkelerin nasıl bulunacağını ve uygulanacağını açıklar.
ms.date: 09/02/2020
ms.topic: conceptual
ms.customer: generated
ms.openlocfilehash: 019541f034ce8f0c3728c38d2ae4425308b4e2a1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "96350288"
---
# <a name="recommended-policies-for-azure-services"></a>Azure hizmetleri için önerilen ilkeler

Azure Ilkesi 'ne yeni olan müşteriler genellikle kaynaklarını yönetmek ve yönetmek için ortak ilke tanımlarını bulmaya bakar. Azure Ilkesinin **Önerilen ilkeleri** , ile başlamak için ortak ilke tanımlarının odaklanmış bir listesini sağlar. Desteklenen kaynaklar için **Önerilen ilkeler** deneyimi, söz konusu kaynak için Portal deneyimine katıştırılır.

Yerleşik ek Azure Ilkesi için bkz. [Azure ilkesi yerleşik tanımları](../samples/built-in-policies.md).

## <a name="azure-virtual-machines"></a>Azure Sanal Makineler

[Azure sanal makineleri](../../../virtual-machines/index.yml) için **Önerilen ilkeler** , sanal makineler için **genel bakış** sayfasında ve **yetenekler** sekmesinde bulunur. _Azure ilke_ kartında, önerilen ilkelerle bir yan bölme açmak için "Yapılandırılmadı" veya "# Assigned" metnini seçin. Sanal makine üyesi olan bir kapsama atanmış olan herhangi bir ilke tanımı gri renkte. Bu sanal makineye uygulamak için önerilen ilkeleri seçin ve her biri için bir atama oluşturmak üzere **Ilke ata** ' yı seçin.

Bir kuruluş, [kaynaklarını ve kaynak hiyerarşisini düzenlemeyle](/azure/cloud-adoption-framework/ready/azure-best-practices/organize-subscriptions)ilgili olarak bu ilke atamalarını kaynak başına bir abonelik veya [Yönetim grubu](../../management-groups/index.yml) düzeyine geçirmeye önerilir.

### <a name="azure-virtual-machines-recommended-policies"></a>Azure sanal makineler önerilen ilkeler

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Olağanüstü durum kurtarma yapılandırması olmadan sanal makineleri denetleme](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0015ea4d-51ff-4ce3-8d8c-f3f8f0179a56) |Olağanüstü durum kurtarma yapılandırması olmayan sanal makineleri denetleyin. Olağanüstü durum kurtarma hakkında daha fazla bilgi edinmek için adresini ziyaret edin [https://aka.ms/asr-doc](../../../site-recovery/index.yml) . |Auditınotexists |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Compute/RecoveryServices_DisasterRecovery_Audit.json) |
|[Yönetilen diskleri kullanmayan VM 'Leri denetleme](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F06a78e20-9358-41c9-923c-fb736d382a4d) |Bu ilke yönetilen diskleri kullanmayan VM 'Leri denetler |denetlenmesini |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Compute/VMRequireManagedDisk_Audit.json) |
|[Sanal makineler için Azure Backup etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F013e242c-8828-4970-87b3-ab247555486d) |Bu ilke, tüm sanal makineler için Azure Backup hizmetinin etkin olup olmadığını denetlemeye yardımcı olur. Azure Backup uygun maliyetli bir yedekleme çözümü, veri kurtarmayı basitleştirir ve diğer bulut yedekleme hizmetlerinden daha kolay bir şekilde etkinleştirilir. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Backup/VirtualMachines_EnableAzureBackup_Audit.json) |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure ilke örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [İlkenin etkilerini anlama](./effects.md) konusunu gözden geçirin.
- [Uyumlu olmayan kaynakları nasıl düzelteceğinizi](../how-to/remediate-resources.md)öğrenin.