---
title: Dağıtım için mantıksal uygulama şablonları oluşturma
description: Azure Logic Apps içinde dağıtımı otomatikleştirmek için Azure Resource Manager şablonları oluşturmayı öğrenin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 07/26/2019
ms.openlocfilehash: 4535e6bf11f8c2abf20b1b323925c3fc3299d362
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "90971781"
---
# <a name="create-azure-resource-manager-templates-to-automate-deployment-for-azure-logic-apps"></a>Azure Logic Apps dağıtımını otomatikleştirmek için Azure Resource Manager şablonu oluşturma

Mantıksal uygulamanızı oluşturma ve dağıtma işlemlerini otomatik hale getirmenize yardımcı olmak için, bu makalede mantıksal uygulamanız için [Azure Resource Manager şablonu](../azure-resource-manager/management/overview.md) oluşturabileceğiniz yollar açıklanmaktadır. İş akışı tanımınızı ve dağıtım için gereken diğer kaynakları içeren bir şablonun yapısı ve sözdizimi hakkında genel bakış için bkz. [genel bakış: Azure Resource Manager şablonlarıyla Logic Apps için dağıtımı otomatikleştirme](logic-apps-azure-resource-manager-templates-overview.md).

Azure Logic Apps, yalnızca mantıksal uygulamalar oluşturmak için değil, yeniden kullanabileceğiniz [önceden oluşturulmuş bir mantıksal uygulama Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json) sağlar, ancak dağıtım için kullanılacak kaynakları ve parametreleri de tanımlayabilir. Bu şablonu kendi iş senaryolarınız için kullanabilir veya şablonu gereksinimlerinize uyacak şekilde özelleştirebilirsiniz.

> [!IMPORTANT]
> Şablonunuzda bulunan bağlantıların mantıksal uygulamanızla aynı Azure kaynak grubunu ve konumunu kullandığınızdan emin olun.

Azure Resource Manager şablonları hakkında daha fazla bilgi için şu konulara bakın:

* [Azure Resource Manager şablon yapısı ve sözdizimi](../azure-resource-manager/templates/template-syntax.md)
* [Azure Resource Manager şablonları yaz](../azure-resource-manager/templates/template-syntax.md)
* [Bulut tutarlılığı için Azure Resource Manager şablonları geliştirme](../azure-resource-manager/templates/templates-cloud-consistency.md)

<a name="visual-studio"></a>

## <a name="create-templates-with-visual-studio"></a>Visual Studio ile şablon oluşturma

Dağıtım için en kolay geçerli parametreli mantıksal uygulama şablonları oluşturmanın en kolay yolu için Visual Studio 'Yu (ücretsiz Community Edition veya üzeri) ve Visual Studio için Azure Logic Apps araçları 'nı kullanın. Daha sonra [Visual Studio 'da mantıksal uygulamanızı oluşturabilir](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md) veya [var olan bir mantıksal uygulamayı visual Studio 'ya Azure Portal bulup indirebilirsiniz](../logic-apps/manage-logic-apps-with-visual-studio.md).

Mantıksal uygulamanızı indirerek, mantıksal uygulamanızın tanımlarını ve bağlantılar gibi diğer kaynakları içeren bir şablon alırsınız. Şablon, mantıksal uygulamanızı ve diğer kaynaklarınızı dağıtmak için kullanılan değerleri de *parametreleştirir* veya parametrelerini tanımlar. Bu parametrelerin değerlerini ayrı Parametreler dosyasında sağlayabilirsiniz. Bu şekilde, bu değerleri dağıtım gereksinimlerinize göre daha kolay bir şekilde değiştirebilirsiniz. Daha fazla bilgi için şu konulara bakın:

* [Visual Studio ile mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md)
* [Visual Studio ile mantıksal uygulamaları yönetme](../logic-apps/manage-logic-apps-with-visual-studio.md)

<a name="azure-powershell"></a>

## <a name="create-templates-with-azure-powershell"></a>Azure PowerShell ile şablonlar oluşturma

[Logicapptemplate modülüyle](https://github.com/jeffhollan/LogicAppTemplateCreator)Azure PowerShell kullanarak kaynak yöneticisi şablonlar oluşturabilirsiniz. Bu açık kaynaklı modül öncelikle mantıksal uygulamanızı ve mantıksal uygulamanın kullandığı tüm bağlantıları değerlendirir. Modül daha sonra dağıtım için gerekli parametrelerle şablon kaynakları oluşturur.

Örneğin, bir Azure Service Bus kuyruğundan ileti alan ve Azure SQL veritabanı 'na veri yükleyen bir mantıksal uygulamanız olduğunu varsayalım. Modül, tüm düzenleme mantığını korur ve SQL ve Service Bus bağlantı dizelerini parametreleştirir ve bu değerleri dağıtım gereksinimlerinize göre sağlayabilmeniz ve değiştirebilmenizi sağlar.

Bu örnekler Azure Resource Manager şablonları kullanarak Logic Apps oluşturmayı ve dağıtmayı, Azure DevOps 'da Azure Pipelines ve Azure PowerShell şunları gösterir:

* [Örnek: Azure Logic Apps Azure Service Bus kuyruklara bağlanma](/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-azure-service-bus-queues-from-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Örnek: Azure Logic Apps Azure Storage hesaplarına bağlanma](/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-azure-storage-accounts-from-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Örnek: Azure Logic Apps için bir işlev uygulama eylemi ayarlama](/samples/azure-samples/azure-logic-apps-deployment-samples/set-up-an-azure-function-app-action-for-azure-logic-apps-and-deploy-with-azure-devops-pipelines/)
* [Örnek: Azure Logic Apps bir tümleştirme hesabına bağlanma](/samples/azure-samples/azure-logic-apps-deployment-samples/connect-to-an-integration-account-from-azure-logic-apps-and-deploy-by-using-azure-devops-pipelines/)

### <a name="install-powershell-modules"></a>PowerShell modüllerini yükler

1. Henüz yapmadıysanız, [Azure PowerShell](/powershell/azure/install-az-ps)' yi daha sonra yükleyebilirsiniz.

1. [PowerShell Galerisi](https://www.powershellgallery.com/packages/LogicAppTemplate)LogicAppTemplate modülünü yüklemenin en kolay yolu için şu komutu çalıştırın:

   ```powershell
   Install-Module -Name LogicAppTemplate
   ```

   En son sürüme güncelleştirmek için şu komutu çalıştırın:

   ```powershell
   Update-Module -Name LogicAppTemplate
   ```

Veya el ile yüklemek için, [mantıksal uygulama şablonu Oluşturucu](https://github.com/jeffhollan/LogicAppTemplateCreator)için GitHub 'daki adımları izleyin.

### <a name="install-azure-resource-manager-client"></a>Azure Resource Manager istemcisi 'ni yükler

LogicAppTemplate modülünün herhangi bir Azure kiracı ve abonelik erişim belirteciyle çalışması için, Azure Resource Manager API 'sini çağıran basit bir komut satırı aracı olan [Azure Resource Manager istemci aracını](https://github.com/projectkudu/ARMClient)yükleyebilirsiniz.

`Get-LogicAppTemplate`Bu araçla komutunu çalıştırdığınızda, komut önce ARMClient aracı aracılığıyla bir erişim belirteci alır, belirteci PowerShell betiğine yönelttir ve şablonu BIR JSON dosyası olarak oluşturur. Araç hakkında daha fazla bilgi için [Azure Resource Manager istemci aracıyla ilgili bu makaleye](https://blog.davidebbo.com/2015/01/azure-resource-manager-client.html)bakın.

### <a name="generate-template-with-powershell"></a>PowerShell ile şablon oluşturma

LogicAppTemplate modülünü ve [Azure CLI](/cli/azure/)yükledikten sonra şablonunuzu oluşturmak Için Şu PowerShell komutunu çalıştırın:

```powershell
$parameters = @{
    Token = (az account get-access-token | ConvertFrom-Json).accessToken
    LogicApp = '<logic-app-name>'
    ResourceGroup = '<Azure-resource-group-name>'
    SubscriptionId = $SubscriptionId
    Verbose = $true
}

Get-LogicAppTemplate @parameters | Out-File C:\template.json
```

[Azure Resource Manager istemci aracından](https://github.com/projectkudu/ARMClient)bir belirteçte boru 'a yönelik öneriyi Izlemek için `$SubscriptionId` Azure abonelik kimliğiniz olduğu yerine bu komutu çalıştırın:

```powershell
$parameters = @{
    LogicApp = '<logic-app-name>'
    ResourceGroup = '<Azure-resource-group-name>'
    SubscriptionId = $SubscriptionId
    Verbose = $true
}

armclient token $SubscriptionId | Get-LogicAppTemplate @parameters | Out-File C:\template.json
```

Ayıkladıktan sonra, bu komutu çalıştırarak şablonınızdan bir parametreler dosyası oluşturabilirsiniz:

```powershell
Get-ParameterTemplate -TemplateFile $filename | Out-File '<parameters-file-name>.json'
```

Azure Key Vault başvurularla ayıklama için (yalnızca statik), şu komutu çalıştırın:

```powershell
Get-ParameterTemplate -TemplateFile $filename -KeyVault Static | Out-File $fileNameParameter
```

| Parametreler | Gerekli | Açıklama |
|------------|----------|-------------|
| TemplateFile | Yes | Şablon dosyanızın dosya yolu |
| KeyVault | No | Olası Anahtar Kasası değerlerini nasıl işleyeceğinizi açıklayan bir sabit listesi. Varsayılan değer: `None`. |
||||

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Mantıksal uygulama şablonları dağıtma](../logic-apps/logic-apps-deploy-azure-resource-manager-templates.md)
