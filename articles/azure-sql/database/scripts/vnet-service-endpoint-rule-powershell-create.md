---
title: Tek ve havuza alınmış veritabanlarının VNet uç noktaları ve kuralları için PowerShell
description: Azure SQL veritabanınız ve Azure SYNAPSE için sanal hizmet uç noktaları oluşturup yönetmek üzere PowerShell betikleri sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.devlang: PowerShell
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: vanto
ms.date: 04/17/2019
ms.custom: sqldbrb=1
tags: azure-synapse
ms.openlocfilehash: 76a1d3aaadcbd1b15966a84f5dd2fe876f82c43a
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102177632"
---
# <a name="powershell-create-a-virtual-service-endpoint-and-vnet-rule-for-azure-sql-database"></a>PowerShell: Azure SQL veritabanı için sanal hizmet uç noktası ve VNet kuralı oluşturma
[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

*Sanal ağ kuralları* , Azure [SQL veritabanı](../sql-database-paas-overview.md) veritabanları, elastik havuzlarınız veya [Azure](../../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) 'daki veritabanları için [mantıksal SQL Server](../logical-servers.md) 'ın sanal ağlardaki belirli alt ağlardan gönderilen iletişimleri kabul edip etmediğini denetleyen bir güvenlik duvarı güvenlik özelliğidir.

> [!IMPORTANT]
> Bu makale, Azure Synapse (eski adıyla SQL DW) dahil olmak üzere Azure SQL veritabanı için geçerlidir. Kolaylık olması için, bu makaledeki Azure SQL veritabanı, Azure SQL veritabanı veya Azure SYNAPSE ait veritabanları için geçerlidir. Bu makalede ilişkili bir hizmet uç noktası olmadığından, bu makale Azure SQL yönetilen örneği *için uygulanmıyor.*

Bu makalede, aşağıdaki eylemleri alan bir PowerShell betiği gösterilmektedir:

1. Alt ağınızda Microsoft Azure *sanal hizmet uç noktası* oluşturur.
2. Bir *sanal ağ kuralı* oluşturmak için, uç noktasını sunucunuzun güvenlik duvarına ekler.

Daha fazla arka plan için bkz. [Azure SQL veritabanı Için sanal hizmet uç noktaları][sql-db-vnet-service-endpoint-rule-overview-735r].

> [!TIP]
> Tüm ihtiyacınız varsa, Azure SQL veritabanı için sanal hizmet uç noktası *türü adını* , alt ağınız için değerlendirmek veya eklemek istiyorsanız, daha [doğrudan PowerShell betiğimize](#a-verify-subnet-is-endpoint-ps-100)atlayabilirsiniz.

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

> [!IMPORTANT]
> PowerShell Azure Resource Manager modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler [ `Az.Sql` cmdlet 'ler](/powershell/module/az.sql)içindir. Eski modül için bkz. [Azurerd. SQL](/powershell/module/AzureRM.Sql/). Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır.

## <a name="major-cmdlets"></a>Ana cmdlet 'ler

Bu makale, alt ağ uç noktasını sunucunuzun erişim denetim listesine (ACL) ekleyen [ **New-AzSqlServerVirtualNetworkRule** cmdlet 'ini](/powershell/module/az.sql/new-azsqlservervirtualnetworkrule) vurgular ve böylece bir kural oluşturur.

Aşağıdaki listede, **New-AzSqlServerVirtualNetworkRule** çağrınıza hazırlanmanız için çalıştırmanız gereken diğer *ana* cmdlet 'lerin sırası gösterilmektedir. Bu makalede, [komut dosyası 3 "sanal ağ kuralı"](#a-script-30)içinde bu çağrılar oluşur:

1. [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig): bir alt ağ nesnesi oluşturur.
2. [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork): Sanal ağınızı, alt ağ vererek oluşturur.
3. [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/Set-azVirtualNetworkSubnetConfig): alt ağınız Için bir sanal hizmet uç noktası atar.
4. [Set-AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork): sanal ağınızda yapılan güncelleştirmeler devam ettirir.
5. [New-AzSqlServerVirtualNetworkRule](/powershell/module/az.sql/new-azsqlservervirtualnetworkrule): alt ağınız bir uç nokta olduktan sonra, alt ağınızı bir sanal ağ kuralı olarak sunucunuzun ACL 'sine ekler.
   - Bu cmdlet, Azure RM PowerShell modülü sürüm 5.1.1 'dan başlayarak **-ıgnoremissingvnetserviceendpoint** parametresini sunar.

## <a name="prerequisites-for-running-powershell"></a>PowerShell çalıştırmaya yönelik önkoşullar

- [Azure Portal][http-azure-portal-link-ref-477t]gibi Azure 'da zaten oturum açabilirsiniz.
- PowerShell betiklerini zaten çalıştırabilirsiniz.

> [!NOTE]
> Lütfen sunucunuza eklemek istediğiniz VNet/alt ağ için hizmet uç noktalarının açık olduğundan emin olun, aksi takdirde VNet güvenlik duvarı kuralının oluşturulması başarısız olur.

## <a name="one-script-divided-into-four-chunks"></a>Dört Öbekle bölünmüş bir betik

Tanıtım PowerShell betiğimiz, daha küçük betikler dizisine bölünmüştür. Bölüm öğrenimi kolaylaştırır ve esneklik sağlar. Betikler, belirtilen sıralamadaki çalıştırılmalıdır. Artık betikleri çalıştırmak için zaman yoksa, gerçek test çıktımız komut dosyası 4 ' ün ardından görüntülenir.

<a name="a-script-10"></a>

### <a name="script-1-variables"></a>Betik 1: değişkenler

Bu ilk PowerShell betiği, değişkenlere değerler atar. Sonraki betikler bu değişkenlere bağımlıdır.

> [!IMPORTANT]
> Bu betiği çalıştırmadan önce, isterseniz değerleri düzenleyebilirsiniz. Örneğin, zaten bir kaynak grubunuz varsa, kaynak grubu adınızı atanan değer olarak düzenlemek isteyebilirsiniz.
>
> Abonelik adınız betikte düzenlenmelidir.

### <a name="powershell-script-1-source-code"></a>PowerShell betiği 1 kaynak kodu

```powershell
######### Script 1 ########################################
##   LOG into to your Azure account.                     ##
##   (Needed only one time per powershell.exe session.)  ##
###########################################################

$yesno = Read-Host 'Do you need to log into Azure (only one time per powershell.exe session)?  [yes/no]'
if ('yes' -eq $yesno) { Connect-AzAccount }

###########################################################
##  Assignments to variables used by the later scripts.  ##
###########################################################

# You can edit these values, if necessary.
$SubscriptionName = 'yourSubscriptionName'
Select-AzSubscription -SubscriptionName $SubscriptionName

$ResourceGroupName = 'RG-YourNameHere'
$Region = 'westcentralus'

$VNetName = 'myVNet'
$SubnetName = 'mySubnet'
$VNetAddressPrefix = '10.1.0.0/16'
$SubnetAddressPrefix = '10.1.1.0/24'
$VNetRuleName = 'myFirstVNetRule-ForAcl'

$SqlDbServerName = 'mysqldbserver-forvnet'
$SqlDbAdminLoginName = 'ServerAdmin'
$SqlDbAdminLoginPassword = 'ChangeYourAdminPassword1'

$ServiceEndpointTypeName_SqlDb = 'Microsoft.Sql'  # Official type name.

Write-Host 'Completed script 1, the "Variables".'
```

<a name="a-script-20"></a>

### <a name="script-2-prerequisites"></a>Betik 2: Önkoşullar

Bu betik, uç nokta eyleminin olduğu sonraki betiği hazırlar. Bu betik, yalnızca henüz yoksa, aşağıdaki listelenen öğeler için oluşturulur. Bu öğelerin zaten mevcut olduğundan eminseniz, betik 2 ' i atlayabilirsiniz:

- Azure kaynak grubu
- Mantıksal SQL Server

### <a name="powershell-script-2-source-code"></a>PowerShell betiği 2 kaynak kodu

```powershell
######### Script 2 ########################################
##   Ensure your Resource Group already exists.          ##
###########################################################

Write-Host "Check whether your Resource Group already exists."

$gottenResourceGroup = $null
$gottenResourceGroup = Get-AzResourceGroup -Name $ResourceGroupName -ErrorAction SilentlyContinue

if ($null -eq $gottenResourceGroup) {
    Write-Host "Creating your missing Resource Group - $ResourceGroupName."
    New-AzResourceGroup -Name $ResourceGroupName -Location $Region
} else {
    Write-Host "Good, your Resource Group already exists - $ResourceGroupName."
}

$gottenResourceGroup = $null

###########################################################
## Ensure your server already exists. ##
###########################################################

Write-Host "Check whether your server already exists."

$sqlDbServer = $null
$azSqlParams = @{
    ResourceGroupName = $ResourceGroupName
    ServerName        = $SqlDbServerName
    ErrorAction       = 'SilentlyContinue'
}
$sqlDbServer = Get-AzSqlServer @azSqlParams

if ($null -eq $sqlDbServer) {
    Write-Host "Creating the missing server - $SqlDbServerName."
    Write-Host "Gather the credentials necessary to next create a server."

    $sqlAdministratorCredentials = [pscredential]::new($SqlDbAdminLoginName,(ConvertTo-SecureString -String $SqlDbAdminLoginPassword -AsPlainText -Force))

    if ($null -eq $sqlAdministratorCredentials) {
        Write-Host "ERROR, unable to create SQL administrator credentials.  Now ending."
        return
    }

    Write-Host "Create your server."

    $sqlSrvParams = @{
        ResourceGroupName           = $ResourceGroupName
        ServerName                  = $SqlDbServerName
        Location                    = $Region
        SqlAdministratorCredentials = $sqlAdministratorCredentials
    }
    New-AzSqlServer @sqlSrvParams
} else {
    Write-Host "Good, your server already exists - $SqlDbServerName."
}

$sqlAdministratorCredentials = $null
$sqlDbServer = $null

Write-Host 'Completed script 2, the "Prerequisites".'
```

<a name="a-script-30"></a>

## <a name="script-3-create-an-endpoint-and-a-rule"></a>Betik 3: uç nokta ve kural oluşturma

Bu betik, bir alt ağa sahip bir sanal ağ oluşturur. Sonra betik, **Microsoft. SQL** uç noktası türünü alt ağa atar. Son olarak, komut alt ağını erişim denetim listesine (ACL) ekler ve böylece bir kural oluşturur.

### <a name="powershell-script-3-source-code"></a>PowerShell betiği 3 kaynak kodu

```powershell
######### Script 3 ########################################
##   Create your virtual network, and give it a subnet.  ##
###########################################################

Write-Host "Define a subnet '$SubnetName', to be given soon to a virtual network."

$subnetParams = @{
    Name            = $SubnetName
    AddressPrefix   = $SubnetAddressPrefix
    ServiceEndpoint = $ServiceEndpointTypeName_SqlDb
}
$subnet = New-AzVirtualNetworkSubnetConfig @subnetParams

Write-Host "Create a virtual network '$VNetName'.`nGive the subnet to the virtual network that we created."

$vnetParams = @{
    Name              = $VNetName
    AddressPrefix     = $VNetAddressPrefix
    Subnet            = $subnet
    ResourceGroupName = $ResourceGroupName
    Location          = $Region
}
$vnet = New-AzVirtualNetwork @vnetParams

###########################################################
##   Create a Virtual Service endpoint on the subnet.    ##
###########################################################

Write-Host "Assign a Virtual Service endpoint 'Microsoft.Sql' to the subnet."

$vnetSubParams = @{
    Name            = $SubnetName
    AddressPrefix   = $SubnetAddressPrefix
    VirtualNetwork  = $vnet
    ServiceEndpoint = $ServiceEndpointTypeName_SqlDb
}
$vnet = Set-AzVirtualNetworkSubnetConfig @vnetSubParams

Write-Host "Persist the updates made to the virtual network > subnet."

$vnet = Set-AzVirtualNetwork -VirtualNetwork $vnet

$vnet.Subnets[0].ServiceEndpoints  # Display the first endpoint.

###########################################################
##   Add the Virtual Service endpoint Id as a rule,      ##
##   into SQL Database ACLs.                             ##
###########################################################

Write-Host "Get the subnet object."

$vnet = Get-AzVirtualNetwork -ResourceGroupName $ResourceGroupName -Name $VNetName

$subnet = Get-AzVirtualNetworkSubnetConfig -Name $SubnetName -VirtualNetwork $vnet

Write-Host "Add the subnet .Id as a rule, into the ACLs for your server."

$ruleParams = @{
    ResourceGroupName      = $ResourceGroupName
    ServerName             = $SqlDbServerName
    VirtualNetworkRuleName = $VNetRuleName
    VirtualNetworkSubnetId = $subnet.Id
}
New-AzSqlServerVirtualNetworkRule @ruleParams 

Write-Host "Verify that the rule is in the SQL Database ACL."

$rule2Params = @{
    ResourceGroupName      = $ResourceGroupName
    ServerName             = $SqlDbServerName
    VirtualNetworkRuleName = $VNetRuleName
}
Get-AzSqlServerVirtualNetworkRule @rule2Params

Write-Host 'Completed script 3, the "Virtual-Network-Rule".'
```

<a name="a-script-40"></a>

## <a name="script-4-clean-up"></a>Betik 4: Temizleme

Bu son betik, önceki betiklerin tanıtım için oluşturduğu kaynakları siler. Ancak, komut dosyası aşağıdakileri silmeden önce onay ister:

- Mantıksal SQL Server
- Azure Kaynak Grubu

Betik 1 tamamlandıktan sonra istediğiniz zaman betiği 4 ' ü çalıştırabilirsiniz.

### <a name="powershell-script-4-source-code"></a>PowerShell betiği 4 kaynak kodu

```powershell
######### Script 4 ########################################
##   Clean-up phase A:  Unconditional deletes.           ##
##                                                       ##
##   1. The test rule is deleted from SQL Database ACL.        ##
##   2. The test endpoint is deleted from the subnet.    ##
##   3. The test virtual network is deleted.             ##
###########################################################

Write-Host "Delete the rule from the SQL Database ACL."

$removeParams = @{
    ResourceGroupName      = $ResourceGroupName
    ServerName             = $SqlDbServerName
    VirtualNetworkRuleName = $VNetRuleName
    ErrorAction            = 'SilentlyContinue'
}
Remove-AzSqlServerVirtualNetworkRule @removeParams

Write-Host "Delete the endpoint from the subnet."

$vnet = Get-AzVirtualNetwork -ResourceGroupName $ResourceGroupName -Name $VNetName

Remove-AzVirtualNetworkSubnetConfig -Name $SubnetName -VirtualNetwork $vnet

Write-Host "Delete the virtual network (thus also deletes the subnet)."

$removeParams = @{
    Name              = $VNetName
    ResourceGroupName = $ResourceGroupName
    ErrorAction       = 'SilentlyContinue'
}
Remove-AzVirtualNetwork @removeParams

###########################################################
##   Clean-up phase B:  Conditional deletes.             ##
##                                                       ##
##   These might have already existed, so user might     ##
##   want to keep.                                       ##
##                                                       ##
##   1. Logical SQL server                        ##
##   2. Azure resource group                             ##
###########################################################

$yesno = Read-Host 'CAUTION !: Do you want to DELETE your server AND your resource group?  [yes/no]'
if ('yes' -eq $yesno) {
    Write-Host "Remove the server."

    $removeParams = @{
        ServerName        = $SqlDbServerName
        ResourceGroupName = $ResourceGroupName
        ErrorAction       = 'SilentlyContinue'
    }
    Remove-AzSqlServer @removeParams

    Write-Host "Remove the Azure Resource Group."
    
    Remove-AzResourceGroup -Name $ResourceGroupName -ErrorAction SilentlyContinue
} else {
    Write-Host "Skipped over the DELETE of SQL Database and resource group."
}

Write-Host 'Completed script 4, the "Clean-Up".'
```

<a name="a-actual-output"></a>

<a name="a-verify-subnet-is-endpoint-ps-100"></a>

## <a name="verify-your-subnet-is-an-endpoint"></a>Alt ağınızın bir uç nokta olduğunu doğrulama

Zaten bir sanal hizmet uç noktası olduğu anlamına gelen, **Microsoft. SQL** tür adı atanmış bir alt ağa sahip olabilirsiniz. Uç noktadan bir sanal ağ kuralı oluşturmak için [Azure Portal][http-azure-portal-link-ref-477t] kullanabilirsiniz.

Ya da alt ağınızın **Microsoft. SQL** tür adına sahip olup olmadığından emin olabilirsiniz. Bu işlemleri gerçekleştirmek için aşağıdaki PowerShell betiğini çalıştırabilirsiniz:

1. Alt ağınızın **Microsoft. SQL** tür adına sahip olup olmadığını yokerin.
2. İsteğe bağlı olarak, varsa tür adını atayın.
    - Komut dosyası, eksik tür adını uygulamadan önce *onaylamanızı* ister.

### <a name="phases-of-the-script"></a>Betiğin aşamaları

PowerShell betiğinin aşamaları aşağıda verilmiştir:

1. Her PS oturumunda yalnızca bir kez gerekli olan Azure hesabınızda oturum açın.  Değişkenler atayın.
2. Sanal ağınızı ve ardından alt ağınız için arama yapın.
3. Alt ağınız **Microsoft. SQL** Endpoint Server türü olarak etiketlendi mu?
4. Alt ağınız üzerinde **Microsoft. SQL** ad türünde bir sanal hizmet uç noktası ekleyin.

> [!IMPORTANT]
> Bu betiği çalıştırmadan önce, komut dosyasının en üstüne yakın olan $-Variables atanan değerleri düzenlemeniz gerekir.

### <a name="direct-powershell-source-code"></a>Doğrudan PowerShell kaynak kodu

Bu PowerShell betiği, sizden onay isterse, yanıt vermediğiniz takdirde hiçbir şeyi güncelleştirmez. Betik, **Microsoft. SQL** tür adını alt ağa ekleyebilir. Ancak, alt ağınız tür adı yoksa, komut dosyası eklemeyi dener.

```powershell
### 1. LOG into to your Azure account, needed only once per PS session.  Assign variables.
$yesno = Read-Host 'Do you need to log into Azure (only one time per powershell.exe session)?  [yes/no]'
if ('yes' -eq $yesno) { Connect-AzAccount }

# Assignments to variables used by the later scripts.
# You can EDIT these values, if necessary.

$SubscriptionName = 'yourSubscriptionName'
Select-AzSubscription -SubscriptionName "$SubscriptionName"

$ResourceGroupName = 'yourRGName'
$VNetName = 'yourVNetName'
$SubnetName = 'yourSubnetName'
$SubnetAddressPrefix = 'Obtain this value from the Azure portal.' # Looks roughly like: '10.0.0.0/24'

$ServiceEndpointTypeName_SqlDb = 'Microsoft.Sql'  # Do NOT edit. Is official value.

### 2. Search for your virtual network, and then for your subnet.
# Search for the virtual network.
$vnet = $null
$vnet = Get-AzVirtualNetwork -ResourceGroupName $ResourceGroupName -Name $VNetName

if ($vnet -eq $null) {
    Write-Host "Caution: No virtual network found by the name '$VNetName'."
    return
}

$subnet = $null
for ($nn = 0; $nn -lt $vnet.Subnets.Count; $nn++) {
    $subnet = $vnet.Subnets[$nn]
    if ($subnet.Name -eq $SubnetName) { break }
    $subnet = $null
}

if ($null -eq $subnet) {
    Write-Host "Caution: No subnet found by the name '$SubnetName'"
    Return
}

### 3. Is your subnet tagged as 'Microsoft.Sql' endpoint server type?
$endpointMsSql = $null
for ($nn = 0; $nn -lt $subnet.ServiceEndpoints.Count; $nn++) {
    $endpointMsSql = $subnet.ServiceEndpoints[$nn]
    if ($endpointMsSql.Service -eq $ServiceEndpointTypeName_SqlDb) {
        $endpointMsSql
        break
    }
    $endpointMsSql = $null
}

if ($null -eq $endpointMsSql) {
    Write-Host "Good: Subnet found, and is already tagged as an endpoint of type '$ServiceEndpointTypeName_SqlDb'."
    return
} else {
    Write-Host "Caution: Subnet found, but not yet tagged as an endpoint of type '$ServiceEndpointTypeName_SqlDb'."

    # Ask the user for confirmation.
    $yesno = Read-Host 'Do you want the PS script to apply the endpoint type name to your subnet?  [yes/no]'
    if ('no' -eq $yesno) { return }
}

### 4. Add a Virtual Service endpoint of type name 'Microsoft.Sql', on your subnet.
$setParams = @{
    Name            = $SubnetName
    AddressPrefix   = $SubnetAddressPrefix
    VirtualNetwork  = $vnet
    ServiceEndpoint = $ServiceEndpointTypeName_SqlDb
}
$vnet = Set-AzVirtualNetworkSubnetConfig @setParams

# Persist the subnet update.
$vnet = Set-AzVirtualNetwork -VirtualNetwork $vnet

for ($nn = 0; $nn -lt $vnet.Subnets.Count; $nn++) {
    $vnet.Subnets[0].ServiceEndpoints # Display.
}
```

<!-- Link references: -->
[sql-db-vnet-service-endpoint-rule-overview-735r]:../vnet-service-endpoint-rule-overview.md
[http-azure-portal-link-ref-477t]: https://portal.azure.com/
