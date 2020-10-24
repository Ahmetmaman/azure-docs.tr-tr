---
title: Azure Cosmos SQL API veritabanı ve kapsayıcısı için kaynak kilidi oluşturmak üzere PowerShell betiği
description: Azure Cosmos SQL API veritabanı ve kapsayıcısı için kaynak kilidi oluşturma
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 06/12/2020
ms.openlocfilehash: 66352cd298d83e6ce5b2616644d9c80e628c2d7c
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92481882"
---
# <a name="create-a-resource-lock-for-azure-cosmos-sql-api-database-and-container-using-azure-powershell"></a>Azure PowerShell kullanarak Azure Cosmos SQL API veritabanı ve kapsayıcısı için bir kaynak kilidi oluşturma

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../../../includes/sample-powershell-install-no-ssh.md)]

> [!IMPORTANT]
> Kaynak kilitleri, Cosmos DB hesabı etkinleştirilmiş özellik ile ilk kez kilitlenmediği takdirde, herhangi bir Cosmos DB SDK 'yı, hesap anahtarları aracılığıyla bağlanan araçları veya Azure portalını kullanarak bağlanan kullanıcılar tarafından yapılan değişiklikler için çalışmaz `disableKeyBasedMetadataWriteAccess` . Bu özelliğin nasıl etkinleştirileceği hakkında daha fazla bilgi edinmek için bkz. [SDK 'lardan değişiklikleri engellemeyi](../../../role-based-access-control.md#prevent-sdk-changes).

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/sql/ps-sql-lock.ps1 "Create, list, and remove resource locks")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
|**Azure kaynağı**| |
| [New-AzResourceLock](/powershell/module/az.resources/new-azresourcelock) | Kaynak kilidi oluşturur. |
| [Get-AzResourceLock](/powershell/module/az.resources/get-azresourcelock) | Bir kaynak kilidi alır veya kaynak kilitlerini listeler. |
| [Remove-AzResourceLock](/powershell/module/az.resources/remove-azresourcelock) | Kaynak kilidini kaldırır. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/).