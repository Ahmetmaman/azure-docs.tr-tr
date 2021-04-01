---
title: Azure Resource Manager şablonları kullanarak Labs oluşturma veya değiştirme
description: Bir DevTest laboratuvarında otomatik olarak laboratuvar oluşturmak veya değiştirmek için PowerShell ile Azure Resource Manager şablonlarını nasıl kullanacağınızı öğrenin
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 645f1d0717514d2c7e7b16844513327127e4e1a8
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "87272649"
---
# <a name="create-or-modify-labs-automatically-using-azure-resource-manager-templates-and-powershell"></a>Azure Resource Manager şablonlarını ve PowerShell 'i kullanarak laboratuvarları otomatik olarak oluşturma veya değiştirme

DevTest Labs, hızla ve otomatik olarak yeni laboratuvarlar oluşturmanıza veya mevcut laboratuvarlara ve ardından bu kaynakları dağıtmanıza yardımcı olabilecek birçok Azure Resource Manager şablon ve PowerShell komut dosyası sağlar.

Bu makale, laboratuvarlarınızın oluşturulmasını, değiştirilmesini ve dağıtımını otomatik hale getirmek için bu şablonları ve betikleri kullanma sürecinde size kılavuzluk eder. Bu makalede ayrıca, PowerShell kullanarak DevTest Labs 'de bazı yaygın görevleri gerçekleştirmek için nasıl daha fazla bilgi bulabileceğiniz gösterilmektedir.

## <a name="step-1-gather-your-templates-and-scripts"></a>Adım 1: şablonlarınızı ve betikleri toplayın
Genel [GitHub deponuzda](https://github.com/Azure/azure-devtestlab)önceden oluşturulmuş [Azure Resource Manager şablonları](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates) ve [PowerShell betikleri](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/Scripts) bulabilirsiniz. Bunları olduğu gibi kullanın veya gereksinimlerinize göre özelleştirin ve kendi [özel git](devtest-lab-add-artifact-repo.md)deponuzda depolayın.

## <a name="step-2-modify-your-azure-resource-manager-template"></a>2. Adım: Azure Resource Manager şablonunuzu değiştirme
Daha önce hiç şablon oluşturmadıysanız [ilk Azure Resource Manager şablonunuzu oluşturma](../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md) bölümündeki adımları izleyebilirsiniz.

Ayrıca, [Azure Resource Manager şablonları oluşturmak Için en iyi yöntemler](../azure-resource-manager/templates/template-best-practices.md) , güvenilir ve kullanımı kolay Azure Resource Manager şablonlar oluşturmanıza yardımcı olacak birçok yönerge ve öneri sunar. Genellikle, belirtilen yaklaşımlardan veya örneklerden birinin varyasyonunu kullanacaksınız ve gereksinimlerinize göre şablonunuzu değiştirirsiniz.

## <a name="step-3-deploy-resources-with-powershell"></a>3. Adım: PowerShell ile kaynak dağıtma
Şablonlarınızı ve betikleri özelleştirdikten sonra, [Kaynak Yöneticisi şablonlar ve Azure PowerShell sahip kaynakları dağıtmak](../azure-resource-manager/templates/deploy-powershell.md)için gereken adımları izleyin. Bu makalede, kaynaklarınızı Azure 'a dağıtmak için Azure Resource Manager şablonlarla Azure PowerShell kullanma hakkında genel bilgiler sağlanmaktadır.


## <a name="common-tasks-you-can-perform-in-devtest-labs-using-powershell"></a>PowerShell kullanarak DevTest Labs 'de gerçekleştirebileceğiniz ortak görevler
PowerShell kullanarak otomatikleştirebileceğiniz diğer birçok ortak görev vardır. Belgelerde aşağıdaki bölümlerde, bu görevleri gerçekleştirmek için gereken adımlar ana hatlarıyla verilmiştir.

* [PowerShell kullanarak bir VHD dosyasından özel görüntü oluşturma](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
* [PowerShell kullanarak VHD dosyasını laboratuvar depolama hesabına yükleyin](devtest-lab-upload-vhd-using-powershell.md)
* [PowerShell kullanarak bir laboratuvara dış Kullanıcı ekleme](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell)
* [PowerShell kullanarak laboratuvar özel rolü oluşturma](devtest-lab-grant-user-permissions-to-specific-lab-policies.md#creating-a-lab-custom-role-using-powershell)

### <a name="next-steps"></a>Sonraki adımlar
* Özelleştirilmiş şablonlarınızı veya betiklerinizi depoladığınız [özel bir git deposu](devtest-lab-add-artifact-repo.md) oluşturmayı öğrenin.
* [Azure hızlı başlangıç şablonu galerisinden Azure Resource Manager şablonlarını](https://github.com/Azure/azure-quickstart-templates)gezin.
