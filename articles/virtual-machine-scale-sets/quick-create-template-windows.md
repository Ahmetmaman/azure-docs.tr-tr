---
title: Hızlı Başlangıç - Azure şablonuyla Windows sanal makine ölçek kümesi oluşturma | Microsoft Docs
description: Örnek uygulama dağıtan ve otomatik ölçeklendirme kurallarını yöneten bir Azure Resource Manager şablonuyla hızlıca bir Windows sanal makine ölçek kümesi oluşturmayı öğrenin
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/27/2018
ms.author: cynthn
ms.openlocfilehash: 8124f2236b37f72741c16dad8f1649d08963542e
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58316669"
---
# <a name="quickstart-create-a-windows-virtual-machine-scale-set-with-an-azure-template"></a>Hızlı Başlangıç: Azure şablonuyla Windows sanal makine ölçek kümesi oluşturma

Sanal makine ölçek kümesi, birbiriyle aynı ve otomatik olarak ölçeklendirilen sanal makine kümesi dağıtmanızı ve yönetmenizi sağlar. Ölçek kümesi içindeki sanal makine sayısını el ile ölçeklendirebilir veya CPU, bellek talebi ya da ağ trafiği gibi kaynak kullanımını temel alan otomatik ölçeklendirme kuralları tanımlayabilirsiniz. Azure Load Balancer daha sonra ölçek kümesindeki sanal makine örneklerine trafiği dağıtır. Bu hızlı başlangıçta, Azure Resource Manager şablonu ile bir sanal makine ölçek kümesi oluşturur ve örnek uygulama dağıtırsınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]


## <a name="define-a-scale-set-in-a-template"></a>Şablonda ölçek kümesi tanımlama
Azure Resource Manager şablonları, ilgili kaynak gruplarını dağıtmanızı sağlar. Şablonlar JavaScript Nesne Gösterimi (JSON) ile yazılmıştır ve uygulamanıza ait Azure altyapısı ortamının tamamını tanımlar. Tek bir şablonda sanal makine ölçek kümesi oluşturabilir, uygulamaları yükleyebilir ve otomatik ölçeklendirme kurallarını yapılandırabilirsiniz. Değişkenleri ve parametreleri kullanarak bu şablonu var olan ölçek kümelerini güncelleştirme veya yenilerini oluşturma amacıyla tekrar kullanabilirsiniz. Şablonları Azure portalı, Azure CLI veya Azure PowerShell aracılığıyla ya da sürekli tümleştirme/sürekli teslim (CI/CD) işlem hatlarından dağıtabilirsiniz.

Şablonlar hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment). JSON söz dizimi ve özellikler için bkz: [Microsoft.Compute/virtualMachineScaleSets](/azure/templates/microsoft.compute/virtualmachinescalesets) şablon başvurusu.

Şablon, her bir kaynak türü için yapılandırma tanımlar. Sanal makine ölçek kümesi kaynak türü, tek bir VM ile benzerlik gösterir. Sanal makine ölçek kümesi kaynak türünün ana bölümleri şunlardır:

| Özellik                     | Özellik açıklaması                                  | Örnek şablon değeri                    |
|------------------------------|----------------------------------------------------------|-------------------------------------------|
| type                         | Oluşturulacak Azure kaynağı türü                            | Microsoft.Compute/virtualMachineScaleSets |
| ad                         | Ölçek kümesi adı                                       | myScaleSet                                |
| location                     | Ölçek kümesinin oluşturulacağı konum                     | Doğu ABD                                   |
| sku.name                     | Her bir ölçek kümesi örneği için VM boyutu                  | Standard_A1                               |
| sku.capacity                 | Başlangıçta oluşturulacak VM örneği sayısı           | 2                                         |
| upgradePolicy.mode           | Değişiklik yapıldığında kullanılacak VM örneği yükseltme modu              | Automatic                                 |
| imageReference               | VM örnekleri için kullanılacak platform veya özel görüntü | Microsoft Windows Server 2016 Datacenter  |
| osProfile.computerNamePrefix | Her bir VM örneği için ad ön eki                     | myvmss                                    |
| osProfile.adminUsername      | Her bir VM örneği için kullanıcı adı                        | azureuser                                 |
| osProfile.adminPassword      | Her bir VM örneği için parola                        | P@ssw0rd!                                 |

 Aşağıdaki örnekte temel ölçek kümesi kaynak tanımı gösterilmektedir. Bir ölçek kümesi şablonunu özelleştirmek için VM boyutunu veya başlangıç kapasitesini değiştirebilir ya da farklı bir platform veya özel görüntü kullanabilirsiniz.

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US",
  "apiVersion": "2017-12-01",
  "sku": {
    "name": "Standard_A1",
    "capacity": "2"
  },
  "properties": {
    "upgradePolicy": {
      "mode": "Automatic"
    },
    "virtualMachineProfile": {
      "storageProfile": {
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage"
        },
        "imageReference":  {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2016-Datacenter",
          "version": "latest"
        }
      },
      "osProfile": {
        "computerNamePrefix": "myvmss",
        "adminUsername": "azureuser",
        "adminPassword": "P@ssw0rd!"
      }
    }
  }
}
```

 Örneği kısa tutmak için sanal ağ arabirim kartı (NIC) yapılandırılması gösterilmemiştir. Yük dengeleyici gibi ek bileşenler de gösterilmemiştir. Ölçek kümesi şablonunun tamamı [bu makalenin sonunda](#deploy-the-template) gösterilmiştir.


## <a name="add-a-sample-application"></a>Örnek uygulama ekleme
Ölçek kümenizi test etmek için temel web uygulaması yükleyin. Bir ölçek kümesini dağıttığınızda VM uzantıları uygulama yükleme gibi dağıtım sonrası yapılandırma ve otomasyon görevlerini gerçekleştirebilir. Betikler Azure depolama veya GitHub konumlarından indirilebilir ya da Azure portalına uzantı çalışma zamanında iletilebilir. Ölçek kümenize uzantı uygulamak için önceki kaynak örneğine *extensionProfile* bölümünü eklemeniz gerekir. Uzantı profili temelde aşağıdaki özellikleri tanımlar:

- Uzantı türü
- Uzantı yayımcısı
- Uzantı sürümü
- Yapılandırma veya yükleme betiklerinin konumu
- VM örneklerinde yürütülecek komutlar

[ASP.NET application on Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) örnek şablonunda PowerShell DSC uzantısı kullanılarak IIS içinde çalışan bir ASP.NET MVC uygulaması yüklenmektedir. 

*url* ile tanımlandığı üzere GitHub'dan bir yükleme betiği indirilir. Uzantı ardından *function* ve *Script* ile tanımlandığı üzere *IISInstall.ps1* betiğinden *InstallIIS* komutunu çalıştırır. ASP.NET uygulaması da *WebDeployPackagePath* ile tanımlandığı üzere GitHub'dan indirilen bir Web Dağıtımı paketine sahiptir:

```json
"extensionProfile": {
  "extensions": [
    {
      "name": "Microsoft.Powershell.DSC",
      "properties": {
        "publisher": "Microsoft.Powershell",
        "type": "DSC",
        "typeHandlerVersion": "2.9",
        "autoUpgradeMinorVersion": true,
        "forceUpdateTag": "1.0",
        "settings": {
          "configuration": {
            "url": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-windows-webapp-dsc-autoscale/DSC/IISInstall.ps1.zip",
            "script": "IISInstall.ps1",
            "function": "InstallIIS"
          },
          "configurationArguments": {
            "nodeName": "localhost",
            "WebDeployPackagePath": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-windows-webapp-dsc-autoscale/WebDeploy/DefaultASPWebApp.v1.0.zip"
          }
        }
      }
    }
  ]
}
```

## <a name="deploy-the-template"></a>Şablonu dağıtma
[ASP.NET MVC application on Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) şablonunu aşağıdaki **Azure'a dağıtın** düğmesiyle dağıtabilirsiniz. Bu düğme Azure portalını açar, şablonun tamamını yükler ve ölçek kümesi adı, örnek sayısı ve yönetici kimlik bilgileri gibi birkaç parametreyi sorar.

[![Şablonu Azure'a dağıtma](media/virtual-machine-scale-sets-create-template/deploy-button.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-vmss-windows-webapp-dsc-autoscale%2Fazuredeploy.json)

Windows ile ASP.NET uygulaması yüklemek için Azure PowerShell kullanabilirsiniz [yeni AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) gibi:

```azurepowershell-interactive
# Create a resource group
New-AzResourceGroup -Name myResourceGroup -Location EastUS

# Deploy template into resource group
New-AzResourceGroupDeployment `
    -ResourceGroupName myResourceGroup `
    -TemplateFile https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-windows-webapp-dsc-autoscale/azuredeploy.json

# Update the scale set and apply the extension
Update-AzVmss `
    -ResourceGroupName myResourceGroup `
    -VmScaleSetName myVMSS `
    -VirtualMachineScaleSet $vmssConfig
```

VM örnekleri için ölçek kümesi adı ve yönetici kimlik bilgileri istemlerini yanıtlayın. Ölçek kümesinin oluşturulması ve uygulamayı yapılandırmak için uzantının uygulanması 10-15 dakika sürebilir.


## <a name="test-your-scale-set"></a>Ölçek kümenizi test etme
Ölçek kümenizi çalışır halde görmek için bir web tarayıcısında örnek web uygulamasına erişin. İle yük dengeleyicinizin genel IP adresini [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) gibi:

```azurepowershell-interactive
Get-AzPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

Yük dengeleyicinin genel IP adresini bir web tarayıcısına şu biçimde girin *http:\//Publicıpaddress/MyApp*. Aşağıdaki örnekte gösterildiği gibi yük dengeleyici trafiği VM örneklerinizden birine dağıtır:

![Çalışan IIS sitesi](./media/virtual-machine-scale-sets-create-powershell/running-iis-site.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekli değilse [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) kaynak grubunu, Ölçek kümesi. `-Force` parametresi kaynakları ek bir komut istemi olmadan silmek istediğinizi onaylar. `-AsJob` parametresi işlemin tamamlanmasını beklemeden denetimi komut istemine döndürür.

```azurepowershell-interactive
Remove-AzResourceGroup -Name "myResourceGroup" -Force -AsJob
```


## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, Azure şablonuyla bir Windows ölçek kümesi oluşturdunuz ve PowerShell DSC uzantısını kullanarak VM örneklerine basit bir ASP.NET uygulaması yüklediniz. Daha fazla bilgi edinmek için Azure sanal makine ölçek kümelerinin nasıl oluşturulacağı ve yönetileceğine ilişkin öğreticiyle devam edin.

> [!div class="nextstepaction"]
> [Azure sanal makine ölçek kümeleri oluşturma ve yönetme](tutorial-create-and-manage-powershell.md)
