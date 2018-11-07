---
title: Azure Service Bus kaynaklarını yönetmek için PowerShell kullanma | Microsoft Docs
description: Hizmet veri yolu kaynakları oluşturup yönetmek için PowerShell modülü kullanın
services: service-bus-messaging
documentationcenter: .NET
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2018
ms.author: spelluru
ms.openlocfilehash: 61869787304d8acaff00e13b52e557b878a795a4
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51250382"
---
# <a name="use-powershell-to-manage-service-bus-resources"></a>Hizmet veri yolu kaynaklarını yönetmek için PowerShell'i kullanma

Microsoft Azure PowerShell, denetlemek ve dağıtım ve yönetimini Azure hizmetlerinin otomatikleştirmek için kullanabileceğiniz bir komut dosyası ortamıdır. Bu makalede nasıl kullanılacağını [Service Bus Resource Manager PowerShell Modülü](/powershell/module/azurerm.servicebus) sağlamak ve Service Bus varlıklarına (ad alanları, kuyruklar, konular ve abonelikler) yönetmek için bir yerel Azure PowerShell konsolunda veya komut dosyası kullanarak.

Ayrıca, Azure Resource Manager şablonlarını kullanarak Service Bus varlıklarına yönetebilirsiniz. Daha fazla bilgi için bkz [Azure Resource Manager şablonlarını kullanarak Service Bus'ı oluşturma kaynakları](service-bus-resource-manager-overview.md).

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki önkoşulları karşılamanız gerekir:

* Azure aboneliği. Bir aboneliği edinme hakkında daha fazla bilgi için bkz. [satın alma seçenekleri][purchase options], [üye tekliflerimizi][member offers], veya [ücretsiz Hesap][free account].
* Azure PowerShell ile bir bilgisayar. Yönergeler için [Azure PowerShell cmdlet'lerini kullanmaya başlama](/powershell/azure/get-started-azureps).
* PowerShell betikleri, NuGet paketlerini ve .NET Framework genel yaklaşım.

## <a name="get-started"></a>başlarken

İlk adım, Azure hesabınızı ve Azure aboneliği için oturum açmak için PowerShell kullanmaktır. Bölümündeki yönergeleri [Azure PowerShell cmdlet'lerini kullanmaya başlama](/powershell/azure/get-started-azureps) Azure hesabınızda oturum açın ve almak ve Azure aboneliğinizde kaynaklara erişmek için.

## <a name="provision-a-service-bus-namespace"></a>Service Bus ad alanı sağlama

Service Bus ad alanları ile çalışırken kullanabileceğiniz [Get-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespace), [yeni AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespace), [Remove-AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespace), ve [kümesi AzureRmServiceBusNamespace](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespace) cmdlet'leri.

Bu örnek betikte birkaç yerel değişkenler oluşturur; `$Namespace` ve `$Location`.

* `$Namespace` Service Bus ad alanı ile çalışmak istiyoruz adıdır.
* `$Location` Biz ad alanı sağlama veri merkezini tanımlar.
* `$CurrentNamespace` Biz almak (veya oluşturma) başvuru ad alanı depolar.

Gerçek bir betikte `$Namespace` ve `$Location` parametre olarak geçirilebilir.

Bu komut dosyasının parçası şunları yapar:

1. Belirtilen ada sahip bir Service Bus ad alanı almaya çalışır.
2. Ad alanı bulunursa bulunanları bildirir.
3. Ad alanı bulunamazsa, ad alanı oluşturur ve ardından yeni oluşturulan ad alanı alır.
   
    ``` powershell
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
   
    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Host "The namespace $Namespace already exists in the $Location region:"
        # Report what was found
        Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "The $Namespace namespace does not exist."
        Write-Host "Creating the $Namespace namespace in the $Location region..."
        New-AzureRmServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzureRMServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
        Write-Host "The $Namespace namespace in Resource Group $ResGrpName in the $Location region has been successfully created."
                
    }
    ```

### <a name="create-a-namespace-authorization-rule"></a>Bir ad alanı yetkilendirme kuralı oluştur

Aşağıdaki örnek, ad alanı yetkilendirme kurallarını kullanarak yönetmek gösterilmektedir [New-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/new-azurermservicebusnamespaceauthorizationrule), [Get-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/get-azurermservicebusnamespaceauthorizationrule), [Set-AzureRmServiceBusNamespaceAuthorizationRule](/powershell/module/azurerm.servicebus/set-azurermservicebusnamespaceauthorizationrule), ve [Remove-AzureRmServiceBusNamespaceAuthorizationRule cmdlet'leri](/powershell/module/azurerm.servicebus/remove-azurermservicebusnamespaceauthorizationrule).

```powershell
# Query to see if rule exists
$CurrentRule = Get-AzureRmServiceBusNamespaceAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

# Check if the rule already exists or needs to be created
if ($CurrentRule)
{
    Write-Host "The $AuthRule rule already exists for the namespace $Namespace."
}
else
{
    Write-Host "The $AuthRule rule does not exist."
    Write-Host "Creating the $AuthRule rule for the $Namespace namespace..."
    New-AzureRmServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule -Rights @("Listen","Send")
    $CurrentRule = Get-AzureRmServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
    Write-Host "The $AuthRule rule for the $Namespace namespace has been successfully created."

    Write-Host "Setting rights on the namespace"
    $authRuleObj = Get-AzureRmServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

    Write-Host "Remove Send rights"
    $authRuleObj.Rights.Remove("Send")
    Set-AzureRmServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Add Send and Manage rights to the namespace"
    $authRuleObj.Rights.Add("Send")
    Set-AzureRmServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj
    $authRuleObj.Rights.Add("Manage")
    Set-AzureRmServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Show value of primary key"
    $CurrentKey = Get-AzureRmServiceBusKey -ResourceGroup $ResGrpName -NamespaceName $Namespace -Name $AuthRule
        
    Write-Host "Remove this authorization rule"
    Remove-AzureRmServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -Name $AuthRule
}
```

## <a name="create-a-queue"></a>Bir kuyruk oluşturma

Bir kuyruk veya konuda oluşturmak için önceki bölümde komut dosyası kullanarak bir ad alanı denetimi gerçekleştirin. Ardından, kuyruk oluşturun:

```powershell
# Check if queue already exists
$CurrentQ = Get-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName

if($CurrentQ)
{
    Write-Host "The queue $QueueName already exists in the $Location region:"
}
else
{
    Write-Host "The $QueueName queue does not exist."
    Write-Host "Creating the $QueueName queue in the $Location region..."
    New-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -EnablePartitioning $True
    $CurrentQ = Get-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName
    Write-Host "The $QueueName queue in Resource Group $ResGrpName in the $Location region has been successfully created."
}
```

### <a name="modify-queue-properties"></a>Kuyruk özelliklerini değiştirme

Kullanabileceğiniz önceki bölümde komut dosyası yürütme sonrasında [Set-AzureRmServiceBusQueue](/powershell/module/azurerm.servicebus/set-azurermservicebusqueue) aşağıdaki örnekteki gibi bir sıranın özelliklerini güncelleştirmek için cmdlet:

```powershell
$CurrentQ.DeadLetteringOnMessageExpiration = $True
$CurrentQ.MaxDeliveryCount = 7
$CurrentQ.MaxSizeInMegabytes = 2048
$CurrentQ.EnableExpress = $True

Set-AzureRmServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -QueueObj $CurrentQ
```

## <a name="provisioning-other-service-bus-entities"></a>Diğer Service Bus varlık sağlama

Kullanabileceğiniz [Service Bus PowerShell modülünü](/powershell/module/azurerm.servicebus) konu başlıkları ve abonelikler gibi diğer varlıklar sağlamak için. Bu cmdlet'ler, önceki bölümde gösterilen kuyruk oluşturma cmdlet'leri sözdizimsel olarak benzerdir.

## <a name="next-steps"></a>Sonraki adımlar

- Tüm Service Bus Resource Manager PowerShell modülü belgelerine bakın [burada](/powershell/module/azurerm.servicebus). Tüm kullanılabilir cmdlet'lerin bu sayfada listelenir.
- Azure Resource Manager şablonlarını kullanma hakkında daha fazla bilgi için bkz [Azure Resource Manager şablonlarını kullanarak Service Bus'ı oluşturma kaynakları](service-bus-resource-manager-overview.md).
- Hakkında bilgi [hizmet veri yolu .NET Yönetim kitaplıklarını](service-bus-management-libraries.md).

Bu blog gönderilerinde açıklandığı gibi hizmet veri yolu varlıklarını yönetmek için bazı alternatif yolu vardır:

* [Service Bus kuyrukları, konular ve abonelikler bir PowerShell betiğini kullanarak oluşturma](https://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Bir Service Bus Namespace ve bir PowerShell betiğini kullanarak bir olay hub'ı oluşturma](https://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)
* [Service Bus PowerShell betikleri](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[purchase options]: http://azure.microsoft.com/pricing/purchase-options/
[member offers]: http://azure.microsoft.com/pricing/member-offers/
[free account]: http://azure.microsoft.com/pricing/free-trial/
