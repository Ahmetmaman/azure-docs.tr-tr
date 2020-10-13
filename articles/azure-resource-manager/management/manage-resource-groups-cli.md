---
title: Kaynak gruplarını yönetme-Azure CLı
description: Kaynak gruplarınızı Azure Resource Manager aracılığıyla yönetmek için Azure CLı 'yi kullanın. Kaynak grupları oluşturma, listeleme ve silme işlemlerinin nasıl yapılacağını gösterir.
author: mumian
ms.topic: conceptual
ms.date: 09/01/2020
ms.author: jgao
ms.custom: devx-track-azurecli
ms.openlocfilehash: abab1a6a65f3cc45bcd88dc7aed89e18e4e28d5b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91371244"
---
# <a name="manage-azure-resource-manager-resource-groups-by-using-azure-cli"></a>Azure CLı kullanarak Azure Resource Manager kaynak gruplarını yönetme

Azure Kaynak gruplarınızı yönetmek için [Azure Resource Manager](overview.md) Ile Azure CLI 'yi nasıl kullanacağınızı öğrenin. Azure kaynaklarını yönetmek için bkz. Azure [CLI kullanarak Azure kaynaklarını yönetme](manage-resources-cli.md).

Kaynak gruplarını yönetme hakkında diğer makaleler:

- [Azure portal kullanarak Azure kaynak gruplarını yönetme](manage-resources-portal.md)
- [Azure PowerShell kullanarak Azure kaynak gruplarını yönetme](manage-resources-powershell.md)

## <a name="what-is-a-resource-group"></a>Kaynak grubu nedir?

Kaynak grubu, bir Azure çözümüne ilişkin kaynakları tutan bir kapsayıcıdır. Kaynak grubu bir çözümün tüm kaynaklarını veya yalnızca grup olarak yönetmek istediğiniz kaynakları içerebilir. Kuruluş için önemli olan faktörleri temel alarak kaynakları kaynak gruplarına nasıl ayıracağınıza siz karar verirsiniz. Genellikle, bunları bir grup olarak kolayca dağıtabilmeniz, güncelleştirebilmeniz ve silebilmeniz için aynı yaşam döngüsünü paylaşan kaynakları ekleyin.

Kaynak grubu, kaynaklarla ilgili meta verileri depolar. Bu nedenle, kaynak grubu için bir konum belirttiğinizde meta verilerin nereye depolanacağını belirtirsiniz. Uyumluluk nedeniyle verilerinizin belirli bir bölgeye depolandığından emin olmanız gerekebilir.

Kaynak grubu, kaynaklarla ilgili meta verileri depolar. Kaynak grubu için bir konum belirttiğinizde, meta verilerin nerede depolandığını belirtirsiniz.

## <a name="create-resource-groups"></a>Kaynak grupları oluşturma

Aşağıdaki CLı komutu bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name demoResourceGroup --location westus
```

## <a name="list-resource-groups"></a>Kaynak gruplarını listeleme

Aşağıdaki CLı betiği, aboneliğinizin altındaki kaynak gruplarını listeler.

```azurecli-interactive
az group list
```

Bir kaynak grubu almak için:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group show --name $resourceGroupName
```

## <a name="delete-resource-groups"></a>Kaynak gruplarını silme

Aşağıdaki CLı betiği bir kaynak grubunu siler:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName
```

Kaynakların silinmesini Azure Resource Manager nasıl sipariş Azure Resource Manager hakkında daha fazla bilgi için bkz. [kaynak grubu silme](delete-resource-group.md).

## <a name="deploy-resources-to-an-existing-resource-group"></a>Kaynakları var olan bir kaynak grubuna dağıtma

Bkz. [kaynakları var olan bir kaynak grubuna dağıtma](manage-resources-cli.md#deploy-resources-to-an-existing-resource-group).

## <a name="deploy-a-resource-group-and-resources"></a>Kaynak grubu ve kaynakları dağıtma

Kaynak Yöneticisi şablonu kullanarak bir kaynak grubu oluşturabilir ve gruba kaynak dağıtabilirsiniz. Daha fazla bilgi için bkz. [kaynak grubu oluşturma ve kaynakları dağıtma](../templates/deploy-to-subscription.md#resource-groups).

## <a name="redeploy-when-deployment-fails"></a>Dağıtım başarısız olduğunda yeniden Dağıt

Bu özellik *hata durumunda geri alma*olarak da bilinir. Daha fazla bilgi için bkz. [dağıtım başarısız olduğunda yeniden dağıtma](../templates/rollback-on-error.md).

## <a name="move-to-another-resource-group-or-subscription"></a>Başka bir kaynak grubuna veya aboneliğe taşıma

Gruptaki kaynakları başka bir kaynak grubuna taşıyabilirsiniz. Daha fazla bilgi için bkz. [kaynakları taşıma](manage-resources-cli.md#move-resources).

## <a name="lock-resource-groups"></a>Kaynak gruplarını kilitle

Kilitleme, kuruluşunuzdaki diğer kullanıcıların Azure aboneliği, kaynak grubu veya kaynak gibi önemli kaynakları yanlışlıkla silmesini veya değiştirmelerini engeller. 

Aşağıdaki betik bir kaynak grubunu kilitleyerek kaynak grubu silinemez.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az lock create --name LockGroup --lock-type CanNotDelete --resource-group $resourceGroupName  
```

Aşağıdaki betik, bir kaynak grubu için tüm kilitleri alır:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az lock list --resource-group $resourceGroupName  
```

Aşağıdaki betik bir kilidi siler:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
echo "Enter the lock name:" &&
read lockName &&
az lock delete --name $lockName --resource-group $resourceGroupName
```

Daha fazla bilgi için bkz. [Azure Resource Manager ile kaynakları kilitleme](lock-resources.md).

## <a name="tag-resource-groups"></a>Kaynak gruplarını etiketleme

Varlıklarınızı mantıksal olarak düzenlemek için kaynak gruplarına ve kaynaklarına Etiketler uygulayabilirsiniz. Daha fazla bilgi için bkz. [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](tag-resources.md#azure-cli).

## <a name="export-resource-groups-to-templates"></a>Kaynak gruplarını şablonlara aktarma

Kaynak grubunuzu başarıyla ayarladıktan sonra, kaynak grubunun Kaynak Yöneticisi şablonunu görüntülemek isteyebilirsiniz. Şablonu dışarı aktarmak iki avantaj sunar:

- Şablon tüm bütün altyapıyı içerdiğinden çözümün gelecekteki dağıtımlarını otomatikleştirin.
- Çözümünüzü temsil eden JavaScript Nesne Gösterimi (JSON) bakarak Şablon sözdizimini öğrenin.

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group export --name $resourceGroupName  
```

Betik, şablonu konsolunda görüntüler.  JSON ' ı kopyalayın ve dosya olarak kaydedin.

Şablonu dışarı aktar özelliği Azure Data Factory kaynaklarının dışarı aktarılmasını desteklemez. Data Factory kaynaklarını nasıl dışarı aktarabilirsiniz hakkında bilgi edinmek için bkz. [Azure Data Factory veri fabrikasını kopyalama veya](https://aka.ms/exportTemplateViaAdf)kopyalama.

Klasik dağıtım modeliyle oluşturulan kaynakları dışarı aktarmak için, [bunları Kaynak Yöneticisi dağıtım modeline geçirmeniz](https://aka.ms/migrateclassicresourcetoarm)gerekir.

Daha fazla bilgi için, [Azure Portal ' de tek ve çoklu kaynak verme şablonuna](../templates/export-template-portal.md)bakın.

## <a name="manage-access-to-resource-groups"></a>Kaynak gruplarına erişimi yönetme

Azure [rol tabanlı erişim denetimi (Azure RBAC)](../../role-based-access-control/overview.md) , Azure 'daki kaynaklara erişimi yönetme yöntemidir. Daha fazla bilgi için bkz. [Azure CLI kullanarak Azure rol atamaları ekleme veya kaldırma](../../role-based-access-control/role-assignments-cli.md).

## <a name="next-steps"></a>Sonraki adımlar

- Azure Resource Manager öğrenmek için bkz. [Azure Resource Manager genel bakış](overview.md).
- Kaynak Yöneticisi Şablon sözdizimini öğrenmek için bkz. [Azure Resource Manager şablonlarının yapısını ve sözdizimini anlayın](../templates/template-syntax.md).
- Şablon geliştirmeyi öğrenmek için [adım adım öğreticiler](../index.yml)bölümüne bakın.
- Azure Resource Manager şablonu şemalarını görüntülemek için bkz. [şablon başvurusu](/azure/templates/).
