---
title: 'Azure CLı: tek bir veritabanı oluşturma'
description: Tek bir veritabanı oluşturmak için bu Azure CLı örnek betiğini kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: sqldbrb=1, devx-track-azurecli
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 06/25/2019
ms.openlocfilehash: dc17ab55cb473036d8c40bd1e533bcc20c4c6259
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92742260"
---
# <a name="use-the-azure-cli-to-create-a-single-database-and-configure-a-firewall-rule"></a>Azure CLı kullanarak tek bir veritabanı oluşturun ve bir güvenlik duvarı kuralı yapılandırın

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

Bu Azure CLı betik örneği, Azure SQL veritabanı 'nda tek bir veritabanı oluşturur ve sunucu düzeyinde bir güvenlik duvarı kuralı yapılandırır. Betik başarıyla çalıştırıldıktan sonra, veritabanına tüm Azure hizmetlerinden ve yapılandırılmış IP adresinden erişilebilir.

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>Betiği çalıştırın

[!code-azurecli-interactive[main](../../../../cli_scripts/sql-database/create-and-configure-database/create-and-configure-database.sh "Create SQL Database")]

### <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanın.

```azurecli-interactive
az group delete --name $resource
```

## <a name="sample-reference"></a>Örnek başvuru

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Açıklama |
|---|---|
| [az sql server](/cli/azure/sql/server#az-sql-server-create) | Sunucu komutları |
| [az SQL Server güvenlik duvarı](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-create) | Sunucu güvenlik duvarı komutları. |
| [az SQL DB](/cli/azure/sql/db#az-sql-db-create) | Veritabanı komutları. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek SQL Veritabanı CLI betiği örnekleri, [Azure SQL Veritabanı belgeleri](../az-cli-script-samples-content-guide.md) içinde bulunabilir.
