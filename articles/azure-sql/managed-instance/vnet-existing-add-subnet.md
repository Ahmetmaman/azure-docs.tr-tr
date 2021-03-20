---
title: Var olan bir sanal ağı yapılandırma
titleSuffix: Azure SQL Managed Instance
description: Bu makalede, Azure SQL yönetilen örneğini dağıtabileceğiniz mevcut bir sanal ağın ve alt ağın nasıl yapılandırılacağı açıklanır.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: how-to
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova
ms.date: 03/17/2020
ms.openlocfilehash: 461acc07ee2217a38f7bb59805d4c7e0de4a1e22
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "91617664"
---
# <a name="configure-an-existing-virtual-network-for-azure-sql-managed-instance"></a>Mevcut sanal ağı Azure SQL Yönetilen Örneği için yapılandırma
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL yönetilen örneği, bir Azure [sanal ağı](../../virtual-network/virtual-networks-overview.md) içinde dağıtılmalıdır ve yalnızca yönetilen örnekler için ayrılmış alt ağ olmalıdır. [SQL yönetilen örnek sanal ağ gereksinimlerine](connectivity-architecture-overview.md#network-requirements)göre yapılandırıldıysa, var olan sanal ağı ve alt ağı kullanabilirsiniz.

Aşağıdaki durumlardan biri sizin için geçerliyse, bu makalede açıklanan betiği kullanarak ağınızı doğrulayabilir ve değiştirebilirsiniz:

- Hala yapılandırılmamış yeni bir alt ağa sahipsiniz.
- Alt ağın [gereksinimlere](connectivity-architecture-overview.md#network-requirements)göre hizalandığından emin değilsiniz.
- Değişiklikleri yaptıktan sonra alt ağın [ağ gereksinimleriyle](connectivity-architecture-overview.md#network-requirements) uyumlu olup olmadığını kontrol etmek istiyorsunuz.

> [!Note]
> Yönetilen bir örneği yalnızca Azure Resource Manager dağıtım modeliyle oluşturulan sanal ağlarda oluşturabilirsiniz. Klasik dağıtım modeli aracılığıyla oluşturulan Azure sanal ağları desteklenmez. [SQL yönetilen örneği için alt ağ boyutunu belirleme](vnet-subnet-determine-size.md) makalesindeki yönergeleri izleyerek alt ağ boyutunu hesaplayın. Kaynakları dağıttıktan sonra alt ağı yeniden boyutlandıramazsınız.
>
> Yönetilen örnek oluşturulduktan sonra, örneği veya VNet 'i başka bir kaynak grubuna veya aboneliğe taşımak desteklenmez.

## <a name="validate-and-modify-an-existing-virtual-network"></a>Var olan sanal ağı doğrulama ve değiştirme

Var olan bir alt ağ içinde yönetilen bir örnek oluşturmak istiyorsanız, alt ağı hazırlamak için aşağıdaki PowerShell betiğini öneririz:

```powershell
$scriptUrlBase = 'https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/manage/azure-sql-db-managed-instance/delegate-subnet'

$parameters = @{
    subscriptionId = '<subscriptionId>'
    resourceGroupName = '<resourceGroupName>'
    virtualNetworkName = '<virtualNetworkName>'
    subnetName = '<subnetName>'
    }

Invoke-Command -ScriptBlock ([Scriptblock]::Create((iwr ($scriptUrlBase+'/delegateSubnet.ps1?t='+ [DateTime]::Now.Ticks)).Content)) -ArgumentList $parameters
```

Betik, alt ağı üç adımda hazır duruma getirir:

1. Doğrula: SQL yönetilen örnek ağ gereksinimleri için seçili sanal ağı ve alt ağı doğrular.
2. Onayla: kullanıcıya, SQL yönetilen örnek dağıtımı için alt ağı hazırlamak üzere yapılması gereken bir değişiklik kümesi gösteriliyor. Ayrıca onay ister.
3. Hazırlama: sanal ağı ve alt ağı düzgün şekilde yapılandırır.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [SQL yönetilen örneği nedir?](sql-managed-instance-paas-overview.md).
- Bir sanal ağ oluşturmayı, yönetilen bir örnek oluşturmayı ve bir veritabanını bir veritabanı yedeğinden geri yüklemeyi gösteren bir öğretici için bkz. [yönetilen örnek oluşturma](instance-create-quickstart.md).
- DNS sorunları için bkz. [Özel BIR DNS yapılandırma](custom-dns-configure.md).
