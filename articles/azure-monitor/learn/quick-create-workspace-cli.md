---
title: Azure CLI kullanarak Log Analytics çalışma alanı oluşturma | Microsoft Docs
description: Azure CLI ile Bulut ve şirket içi ortamınızı gelen yönetim çözümleri ve veri toplamayı etkinleştirmek için Log Analytics çalışma alanı oluşturmayı öğrenin.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: magoedte
ms.openlocfilehash: 76ddbe34650e72b12344f78bc74280f114e5d26c
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2019
ms.locfileid: "56592443"
---
# <a name="create-a-log-analytics-workspace-with-azure-cli-20"></a>Azure CLI 2.0 ile Log Analytics çalışma alanı oluşturma

Azure CLI 2.0, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıçta Azure CLI 2.0, Azure İzleyici'de bir Log Analytics çalışma alanı dağıtım yapma gösterilmektedir. Bir Log Analytics çalışma alanı, Azure İzleyici günlük verileri için benzersiz bir ortamdır. Kendi veri deposu ve yapılandırma her çalışma alanına sahiptir ve veri kaynakları ve çözümleri belirli bir çalışma alanında, verilerini depolamak için yapılandırılır. Aşağıdaki kaynaklardan veri toplama işlemini düşünüyorsanız, Log Analytics çalışma gerektirir:

* Aboneliğinizdeki Azure kaynakları  
* Şirket içi bilgisayarlar System Center Operations Manager tarafından izlenen  
* System Center Configuration Manager cihaz koleksiyonları  
* Azure depolama biriminden tanılama veya günlük verileri  
 
Azure sanal makinelerini ve Windows veya Linux Vm'leri, ortamınızda gibi diğer kaynakları için aşağıdaki konulara bakın:

* [Azure sanal makinelerden veri toplama](../learn/quick-collect-azurevm.md)
* [Karma Linux bilgisayarından verileri toplama](../learn/quick-collect-linux-computer.md)
* [Karma Windows bilgisayardan veri topla](quick-collect-windows-computer.md)

Azure aboneliğiniz yoksa, oluşturma [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) başlamadan önce.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.30 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
Bir çalışma alanı ile [az grubu dağıtım oluşturma](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create). Aşağıdaki örnekte adlı bir çalışma alanı oluşturur *TestWorkspace* kaynak grubundaki *Laboratuvar* içinde *eastus* yerel bir Resource Manager şablonu kullanarak konumu Makine. JSON şablonunu, çalışma alanının adı için yalnızca isteyecek şekilde yapılandırılmış ve büyük olasılıkla ortamınızdaki standart bir yapılandırma olarak kullanılacak diğer parametreler için varsayılan bir değer belirtir. Veya, kuruluşunuzda paylaşılan erişim için bir Azure depolama hesabında şablonu depolayabilirsiniz. Şablonları ile çalışma hakkında daha fazla bilgi için bkz: [kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-cli.md)

Aşağıdaki parametreleri varsayılan değeri ayarlayın:

* Konum - Doğu ABD için varsayılanları
* SKU - Nisan 2018 fiyatlandırma modelinde yayımlanan yeni GB başına fiyatlandırma katmanı varsayılan olarak

>[!WARNING]
>Oluşturma veya yeni Nisan 2018 fiyatlandırma modelini tercih bir Abonelikteki Log Analytics çalışma alanını yapılandırma, yalnızca geçerli Log Analytics fiyatlandırma katmanı ise **PerGB2018**. 
>

### <a name="create-and-deploy-template"></a>Şablon oluşturma ve dağıtma

1. Aşağıdaki JSON söz dizimini kopyalayıp dosyanıza yapıştırın:

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "String",
            "metadata": {
              "description": "Specifies the name of the workspace."
            }
        },
        "location": {
            "type": "String",
            "allowedValues": [
              "eastus",
              "westus"
            ],
            "defaultValue": "eastus",
            "metadata": {
              "description": "Specifies the location in which to create the workspace."
            }
        },
        "sku": {
            "type": "String",
            "allowedValues": [
              "Standalone",
              "PerNode",
              "PerGB2018"
            ],
            "defaultValue": "PerGB2018",
            "metadata": {
            "description": "Specifies the service tier of the workspace: Standalone, PerNode, Per-GB"
        }
          }
    },
    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2015-11-01-preview",
            "location": "[parameters('location')]",
            "properties": {
                "sku": {
                    "Name": "[parameters('sku')]"
                },
                "features": {
                    "searchVersion": 1
                }
            }
          }
       ]
    }
    ```

2. Gereksinimlerinizi karşılayacak şekilde şablonunu düzenleyin.  Gözden geçirme [Microsoft.OperationalInsights/workspaces şablon](https://docs.microsoft.com/azure/templates/microsoft.operationalinsights/workspaces) başvuru hangi özellikler ve değerler desteklendiğini öğrenin. 
3. Bu dosyayı farklı Kaydet **deploylaworkspacetemplate.json** yerel bir klasöre.   
4. Bu şablonu dağıtmaya hazırsınız. Şablonu içeren klasörden aşağıdaki komutları kullanın:

    ```azurecli
    az group deployment create --resource-group <my-resource-group> --name <my-deployment-name> --template-file deploylaworkspacetemplate.json
    ```

Dağıtımın tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonuç içeren aşağıdakine benzer bir ileti görürsünüz:

![Dağıtım tamamlandığında örnek sonucu](media/quick-create-workspace-cli/template-output-01.png)

## <a name="next-steps"></a>Sonraki adımlar
Bir çalışma alanı kullanılabilir olduğuna göre telemetri izleme koleksiyonunu yapılandırma, bu verileri çözümlemek için günlük aramaları çalıştıran ve ek veriler ve hakkında analitik bilgiler sağlamak için bir yönetim çözümünü ekleyin.  

* Azure Tanılama veya Azure depolama ile Azure kaynaklarından veri toplamayı etkinleştirmek için bkz: [toplamak Azure hizmeti günlükleri ve Log analytics'teki kullanım ölçümlerini](../platform/collect-azure-metrics-logs.md).  
* Ekleme [System Center Operations Manager veri kaynağı olarak](../platform/om-agents.md) , Operations Manager yönetim grubuna bildirimde bulunan aracılardan veri toplamak ve Log Analytics çalışma alanınızda depolamak için.  
* Connect [Configuration Manager](../platform/collect-sccm.md) hiyerarşideki koleksiyona üye olan bilgisayarlara aktarmak için.  
* Gözden geçirme [izleme çözümleri](../insights/solutions.md) kullanılabilir ve ekleme veya bir çözüm çalışma alanınızdan kaldırın.
