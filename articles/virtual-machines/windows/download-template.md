---
title: Bir Azure VM için şablon indirme | Microsoft Docs
description: Resource Manager dağıtım modelinde dağıtımları otomatik hale getirmeye yardımcı olmak için bir VM templatefor indirin
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 51ef4f51-0942-4249-afea-4a3f87ce1ff8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 11/17/2017
ms.author: cynthn
ms.openlocfilehash: 574227e010a37340ce7248d2e4657f6a3f231d0a
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2019
ms.locfileid: "55984537"
---
# <a name="download-the-template-for-a-vm"></a>VM için şablon indirme
Portal veya PowerShell kullanarak Azure'da bir VM oluşturduğunuzda, Resource Manager şablonu sizin için otomatik olarak oluşturulur. Bu şablon, bir dağıtımı hızlı bir şekilde çoğaltmak için kullanabilirsiniz. Bu şablon, bir kaynak grubundaki tüm kaynakları hakkında bilgi içerir. Bir sanal makine için bu şablonu VM ağ kaynakları dahil olmak üzere, bu kaynak grubundaki desteklemek üzere oluşturulmuş her şeyi içeren anlamına gelir.

## <a name="download-the-template-using-the-portal"></a>Portalı kullanarak şablon indirme
1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Soldaki menünün seçin **sanal makineler**.
3. Listeden sanal makineyi seçin.
4. Seçin **Otomasyon betiği**.
5. Seçin **indirme** menüsünde üst ve .zip dosyasını yerel bilgisayarınıza kaydedin.
6. .Zip dosyasını açın ve dosyaları bir klasöre ayıklayın. .Zip dosyasını içerir:
   
   * deploy.ps1
   * deploy.sh 
   * deployer.RB
   * DeploymentHelper.cs
   * parameters.json
   * Template.JSON

Template.json dosyasını şablonudur.

## <a name="download-the-template-using-powershell"></a>PowerShell kullanarak şablonu indirin
.Json şablon dosyasını kullanarak da indirebilirsiniz [dışarı aktarma AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup) cmdlet'i. Kullanabileceğiniz `-path` parametresi için bir .json dosyası yolu ve dosya adı sağlayın. Bu örnek adlı kaynak grubu için Şablonu İndirme işlemi açıklanır **myResourceGroup** için **C:\users\public\downloads** yerel bilgisayarınızda bir klasör.

```powershell
    Export-AzResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Sonraki adımlar
Şablonları kullanarak kaynakları dağıtma hakkında daha fazla bilgi edinmek için [Resource Manager şablonu Kılavuzu](../../azure-resource-manager/resource-manager-template-walkthrough.md).

