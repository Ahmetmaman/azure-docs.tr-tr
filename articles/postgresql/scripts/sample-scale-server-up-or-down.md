---
title: Azure CLı betiği-PostgreSQL için Azure veritabanı 'nı ölçeklendirme ve izleme
description: Örnek Azure CLI Betiği - Ölçümleri sorguladıktan sonra PostgreSQL için Azure Veritabanı sunucusunu farklı bir performans düzeyinde olacak şekilde ölçeklendirin.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.devlang: azurecli
ms.custom: mvc, devx-track-azurecli
ms.topic: sample
ms.date: 08/07/2019
ms.openlocfilehash: a848e14f854385ed1603918ab7e7a274667f2324
ms.sourcegitcommit: 6906980890a8321dec78dd174e6a7eb5f5fcc029
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92427548"
---
# <a name="monitor-and-scale-a-single-postgresql-server-using-azure-cli"></a>Azure CLI kullanarak tek bir PostgreSQL sunucusunu izleme ve ölçeklendirme
Bu örnek CLı betiği, ölçümleri sorguladıktan sonra, tek bir PostgreSQL için Azure veritabanı sunucusu için işlem ve depolamayı ölçeklendirir. İşlem ölçeği yukarı veya aşağı olabilir. Depolama alanı yalnızca ölçeği değiştirebilir. 

> [!IMPORTANT] 
> Depolama yalnızca yukarı ölçeklenebilen, aşağı doğru değil.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

CLI aracını yerel olarak çalıştırmayı tercih ederseniz bu makale için Azure CLI aracının 2.0 veya sonraki bir sürümü gerekir. `az --version` komutunu çalıştırarak sürümü denetleyin. Azure CLI aracını yüklemek veya sürümünüzü yükseltmek için bkz. [Azure CLI’yi Yükleme]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik
Betiği abonelik KIMLIĞINIZLE güncelleştirin.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/scale-postgresql-server.sh "Create and scale Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Betik çalıştırıldıktan sonra aşağıdaki komutu kullanarak kaynak grubunu ve bununla ilişkili tüm kaynakları kaldırın. 
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/delete-postgresql.sh "Delete the resource group.")]

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, aşağıdaki tabloda ana hatları verilen komutları kullanır:

| **Komut** | **Notlar** |
|---|---|
| [az group create](/cli/azure/group) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az postgres server create](/cli/azure/postgres/server#az-postgres-server-create) | Veritabanlarını barındıran bir PostgreSQL sunucusu oluşturur. |
| [az Postgres Server Update](/cli/azure/postgres/server#az-postgres-server-update) | PostgreSQL sunucusunun özelliklerini güncelleştirir. |
| [az monitor metrics list](/cli/azure/monitor/metrics) | Kaynaklar için ölçüm değerini listeleyin. |
| [az group delete](/cli/azure/group) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar
- [PostgreSQL Için Azure veritabanı işlem ve depolama](../concepts-pricing-tiers.md) hakkında daha fazla bilgi edinin
- Ek betikleri deneyin: [PostgreSQL için Azure Veritabanı Azure CLI örnekleri](../sample-scripts-azure-cli.md)
- [Azure CLI](/cli/azure) hakkında daha fazla bilgi edinin
