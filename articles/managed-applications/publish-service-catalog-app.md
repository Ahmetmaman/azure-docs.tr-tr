---
title: Azure hizmet kataloğu yönetilen uygulaması oluşturma ve yayımlama | Microsoft Docs
description: Kuruluşunuzun üyelerine yönelik bir Azure yönetilen uygulaması oluşturmayı gösterir.
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.date: 06/08/2018
ms.author: tomfitz
ms.openlocfilehash: 3b1da6e9068be3c96cce5973f29344fe7e4b4872
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47095849"
---
# <a name="publish-a-managed-application-for-internal-consumption"></a>Yönetilen bir uygulamayı dahili tüketim için yayımlama

Kuruluşunuzun üyelerine yönelik Azure [yönetilen uygulamaları](overview.md) oluşturup yayımlayabilirsiniz. Örneğin, BT departmanı kurumsal standartlara uymak için yönetilen uygulamalar yayımlayabilir. Bu yönetilen uygulamalara Azure marketten değil, hizmet kataloğu üzerinden erişilebilir.

Yönetilen bir uygulamayı hizmet kataloğu için yayımlamak istiyorsanız şunları yapmanız gerekir:

* Yönetilen uygulama ile dağıtılacak kaynakları tanımlayan bir şablon oluşturun.
* Yönetilen uygulamayı dağıtırken portal için kullanıcı arabirimi öğeleri tanımlayın.
* Gerekli şablon dosyalarını içeren bir .zip paketi oluşturun.
* Kullanıcının aboneliğindeki kaynak grubuna hangi kullanıcı, grup veya uygulamanın erişmesi gerektiğine karar verin.
* .zip paketini işaret eden ve kimlik için erişim isteyen yönetilen uygulama tanımını oluşturun.

Bu makale için, yönetilen uygulamanız yalnızca bir depolama hesabı içerir. Yönetilen bir uygulamayı yayımlama adımlarını göstermeye yöneliktir. Tam örnekler için bkz. [Azure yönetilen uygulamaları için örnek projeler](sample-projects.md).

Bu makaledeki PowerShell örnekleri Azure PowerShell’in 6.2 veya üzerini gerektirir. Gerekirse [sürümünüzü güncelleştirin](/powershell/azure/install-azurerm-ps).

## <a name="create-the-resource-template"></a>Kaynak şablonunu oluşturma

Her yönetilen uygulama tanımı **mainTemplate.json** adlı bir dosya içerir. Bu dosyanın içinde, dağıtılacak Azure kaynaklarını tanımlarsınız. Şablon normal bir Resource Manager şablonundan farklı değildir.

**mainTemplate.json** adlı bir dosya oluşturun. Bu ad büyük/küçük harfe duyarlıdır.

Aşağıdaki JSON’u dosyanıza ekleyin. Depolama hesabı oluşturma parametrelerini tanımlar ve depolama hesabının özelliklerini belirtir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountNamePrefix": {
            "type": "string"
        },
        "storageAccountType": {
            "type": "string"
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "variables": {
        "storageAccountName": "[concat(parameters('storageAccountNamePrefix'), uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccountName')]",
            "apiVersion": "2016-01-01",
            "location": "[parameters('location')]",
            "sku": {
                "name": "[parameters('storageAccountType')]"
            },
            "kind": "Storage",
            "properties": {}
        }
    ],
    "outputs": {
        "storageEndpoint": {
            "type": "string",
            "value": "[reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
        }
    }
}
```

mainTemplate.json dosyasını kaydedin.

## <a name="create-the-user-interface-definition"></a>Kullanıcı arabirimi tanımı oluşturma

Azure portalı, yönetilen uygulamayı oluşturan kullanıcılar için kullanıcı arabirimini oluşturmak üzere **createUiDefinition.json** dosyasını kullanır. Her bir parametre için kullanıcıların nasıl giriş sağladığını siz tanımlarsınız. Açılır liste, metin kutusu, parola kutusu ve diğer giriş araçları gibi seçenekleri kullanabilirsiniz. Yönetilen bir uygulamaya ait bir kullanıcı arabirimi tanım dosyası oluşturma hakkında bilgi için [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md) konusunu inceleyin.

**createUiDefinition.json** adlı bir dosya oluşturun. Bu ad büyük/küçük harfe duyarlıdır.

Aşağıdaki JSON’u dosyaya ekleyin.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
    "handler": "Microsoft.Compute.MultiVm",
    "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {}
        ],
        "steps": [
            {
                "name": "storageConfig",
                "label": "Storage settings",
                "subLabel": {
                    "preValidation": "Configure the infrastructure settings",
                    "postValidation": "Done"
                },
                "bladeTitle": "Storage settings",
                "elements": [
                    {
                        "name": "storageAccounts",
                        "type": "Microsoft.Storage.MultiStorageAccountCombo",
                        "label": {
                            "prefix": "Storage account name prefix",
                            "type": "Storage account type"
                        },
                        "defaultValue": {
                            "type": "Standard_LRS"
                        },
                        "constraints": {
                            "allowedTypes": [
                                "Premium_LRS",
                                "Standard_LRS",
                                "Standard_GRS"
                            ]
                        }
                    }
                ]
            }
        ],
        "outputs": {
            "storageAccountNamePrefix": "[steps('storageConfig').storageAccounts.prefix]",
            "storageAccountType": "[steps('storageConfig').storageAccounts.type]",
            "location": "[location()]"
        }
    }
}
```

createUiDefinition.json dosyasını kaydedin.

## <a name="package-the-files"></a>Dosyaları paketleme

İki dosyayı app.zip adlı bir .zip dosyasına ekleyin. İki dosya, .zip dosyasının kök düzeyinde olmalıdır. Dosyaları bir klasöre yerleştirirseniz, yönetilen uygulama tanımını oluştururken gerekli dosyaların mevcut olmadığını belirten bir hata alırsınız. 

Paketi, tüketilebileceği erişilebilir bir konuma yükleyin. 

```powershell
New-AzureRmResourceGroup -Name storageGroup -Location eastus
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName storageGroup `
  -Name "mystorageaccount" `
  -Location eastus `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context

New-AzureStorageContainer -Name appcontainer -Context $ctx -Permission blob

Set-AzureStorageBlobContent -File "D:\myapplications\app.zip" `
  -Container appcontainer `
  -Blob "app.zip" `
  -Context $ctx 
```

## <a name="create-the-managed-application-definition"></a>Yönetilen uygulama tanımı oluşturma

### <a name="create-an-azure-active-directory-user-group-or-application"></a>Azure Active Directory kullanıcı grubu veya uygulaması oluşturma

Sonraki adım, müşteri adına kaynakları yönetmeye yönelik bir kullanıcı grubu ya da uygulama seçmektir. Bu kullanıcı grubu veya uygulama, atanan role göre yönetilen kaynak grubu üzerinde izinlere sahiptir. Rol, Sahip veya Katkıda Bulunan gibi herhangi bir yerleşik Rol Tabanlı Erişim Denetimi (RBAC) rolü olabilir. Kaynakları yönetmek için tek bir kullanıcı izni de verebilirsiniz ancak genellikle bu izni bir kullanıcı grubuna atarsınız. Yeni bir Active Directory kullanıcı grubu oluşturmak için bkz. [Azure Active Directory’de grup oluşturma ve üye ekleme](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

Kaynakları yönetmek için kullanılacak kullanıcı grubunun nesne kimliği gerekir. 

```powershell
$groupID=(Get-AzureRmADGroup -DisplayName mygroup).Id
```

### <a name="get-the-role-definition-id"></a>Rol tanımı kimliği oluşturma

Bundan sonra kullanıcıya, kullanıcı grubuna ya da uygulamaya erişim izni vermek istediğiniz RBAC yerleşik rolünün rol tanımı kimliği gerekir. Genellikle, Sahip, Katkıda Bulunan veya Okuyucu rolünü kullanırsınız. Aşağıdaki komut, Sahip rolünün rol tanımı kimliğinin nasıl alınacağını gösterir:

```powershell
$ownerID=(Get-AzureRmRoleDefinition -Name Owner).Id
```

### <a name="create-the-managed-application-definition"></a>Yönetilen uygulama tanımı oluşturma

Yönetilen uygulama tanımınızı depolamak için henüz bir kaynak grubunuz yoksa şimdi bir tane oluşturun:

```powershell
New-AzureRmResourceGroup -Name appDefinitionGroup -Location westcentralus
```

Şimdi, yönetilen uygulama tanımı kaynağını oluşturun.

```powershell
$blob = Get-AzureStorageBlob -Container appcontainer -Blob app.zip -Context $ctx

New-AzureRmManagedApplicationDefinition `
  -Name "ManagedStorage" `
  -Location "westcentralus" `
  -ResourceGroupName appDefinitionGroup `
  -LockLevel ReadOnly `
  -DisplayName "Managed Storage Account" `
  -Description "Managed Azure Storage Account" `
  -Authorization "${groupID}:$ownerID" `
  -PackageFileUri $blob.ICloudBlob.StorageUri.PrimaryUri.AbsoluteUri
```

### <a name="make-sure-users-can-see-your-definition"></a>Kullanıcıların tanımınızı görebilmesini sağlama

Yönetilen uygulama tanımına eriştiniz ama kuruluşunuzdaki diğer kullanıcıların da erişebildiğinden emin olmak istiyorsunuz. Onlara tanım üzerinde en azından Okuyucu rolü verin. Bu erişim düzeyini abonelikten veya kaynak grubunda devralmış olabilirler. Tanıma kimlerin erişimi olduğunu denetlemek ve kullanıcıları veya grupları eklemek için bkz. [Azure abonelik kaynaklarınıza erişimi yönetmek için Rol Tabanlı Erişim Denetimi kullanma](../role-based-access-control/role-assignments-portal.md).

## <a name="create-the-managed-application"></a>Yönetilen uygulamayı oluşturma

Yönetilen uygulamayı portal, PowerShell veya Azure CLI üzerinden dağıtabilirsiniz.

### <a name="powershell"></a>PowerShell

İlk olarak, yönetilen uygulamayı dağıtmak için PowerShell kullanalım.

```powershell
# Create resource group
New-AzureRmResourceGroup -Name applicationGroup -Location westcentralus

# Get ID of managed application definition
$appid=(Get-AzureRmManagedApplicationDefinition -ResourceGroupName appDefinitionGroup -Name ManagedStorage).ManagedApplicationDefinitionId

# Create the managed application
New-AzureRmManagedApplication `
  -Name storageApp `
  -Location westcentralus `
  -Kind ServiceCatalog `
  -ResourceGroupName applicationGroup `
  -ManagedApplicationDefinitionId $appid `
  -ManagedResourceGroupName "InfrastructureGroup" `
  -Parameter "{`"storageAccountNamePrefix`": {`"value`": `"demostorage`"}, `"storageAccountType`": {`"value`": `"Standard_LRS`"}}"
```

Yönetilen uygulamanız ve yönetilen altyapı artık abonelikte mevcuttur.

### <a name="portal"></a>Portal

Şimdi de yönetilen uygulamayı dağıtmak için portalı kullanalım. Pakette oluşturduğunuz kullanıcı arabirimini görürsünüz.

1. Azure portalına gidin. **+ Kaynak oluştur**’u seçin ve **hizmet kataloğu** araması yapın.

   ![Hizmet kataloğu arama](./media/publish-service-catalog-app/create-new.png)

1. **Hizmet Kataloğu Yönetilen Uygulaması**’nı seçin.

   ![Hizmet kataloğu seçme](./media/publish-service-catalog-app/select-service-catalog-managed-app.png)

1. **Oluştur**’u seçin.

   ![Oluştur’u seçin](./media/publish-service-catalog-app/select-create.png)

1. Kullanılabilir çözümler listesinden oluşturmak istediğiniz yönetilen uygulamayı bulup seçin. **Oluştur**’u seçin.

   ![Yönetilen uygulamayı bulma](./media/publish-service-catalog-app/find-application.png)

   Yönetilen uygulama tanımını portal üzerinden göremiyorsanız, portal ayarlarınızı değiştirmeniz gerekebilir. **Dizin ve Abonelik filtresi**'ni seçin.

   ![Abonelik filtresini seçme](./media/publish-service-catalog-app/select-filter.png)

   Genel abonelik filtresinin yönetilen uygulama tanımının bulunduğu aboneliği içerip içermediğini denetleyin.

   ![Abonelik filtresini denetleme](./media/publish-service-catalog-app/check-global-filter.png)

   Aboneliği seçtikten sonra, baştan başlayıp hizmet kataloğu yönetilen uygulamasını oluşturun. Bunu artık görüyor olmalısınız.

1. Yönetilen uygulama için gerekli olan temel bilgileri sağlayın. Aboneliği ve yönetilen uygulamayı içerecek yeni kaynak grubunu belirtin. **ABD Orta Batı** konumunu seçin. İşiniz bittiğinde **Tamam**’ı seçin.

   ![Yönetilen uygulama parametrelerini sağlama](./media/publish-service-catalog-app/add-basics.png)

1. Yönetilen uygulamadaki kaynaklara özel değerleri sağlayın. İşiniz bittiğinde **Tamam**’ı seçin.

   ![Kaynak parametreleri sağlama](./media/publish-service-catalog-app/add-storage-settings.png)

1. Şablon, sağladığınız değerleri doğrular. Doğrulama başarılı olursa, dağıtımı başlatmak için **Tamam**’ı seçin.

   ![Yönetilen uygulamayı doğrulama](./media/publish-service-catalog-app/view-summary.png)

Dağıtım tamamlandıktan sonra yönetilen uygulama applicationGroup adlı bir kaynak grubunda mevcut olur. Depolama hesabı, applicationGroup adlı ve karma dize değerli bir kaynak grubunda mevcut olur.

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Yönetilen uygulamalara genel bakış](overview.md) konusunu inceleyin.
* Örnek projeler için bkz. [Azure yönetilen uygulamaları için örnek projeler](sample-projects.md).
* Yönetilen bir uygulamaya ait bir kullanıcı arabirimi tanım dosyası oluşturma hakkında bilgi için [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md) konusunu inceleyin.
