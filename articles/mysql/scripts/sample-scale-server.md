---
title: CLı betiği-ölçek sunucusu-MySQL için Azure veritabanı
description: Bu örnek CLI betiği, ölçümleri sorguladıktan sonra MySQL için Azure Veritabanı sunucusunu farklı bir performans düzeyinde olacak şekilde ölçeklendirir.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc, devx-track-azurecli
ms.date: 12/02/2019
ms.openlocfilehash: 318e41089f096f22d17faaa77863fe6af54a3553
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87483775"
---
# <a name="monitor-and-scale-an-azure-database-for-mysql-server-using-azure-cli"></a>Azure CLI kullanarak MySQL için Azure Veritabanı sunucusunu izleme ve ölçeklendirme
Bu örnek CLı betiği, ölçümleri sorguladıktan sonra, tek bir MySQL için Azure veritabanı sunucusu için işlem ve depolamayı ölçeklendirir. İşlem ölçeği yukarı veya aşağı olabilir. Depolama alanı yalnızca ölçeği değiştirebilir.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

CLI aracını yerel olarak çalıştırmayı tercih ederseniz bu makale için Azure CLI aracının 2.0 veya sonraki bir sürümü gerekir. `az --version` komutunu çalıştırarak sürümü denetleyin. Azure CLI aracını yüklemek veya sürümünüzü yükseltmek için bkz. [Azure CLI’yi Yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik
Betiği abonelik KIMLIĞINIZLE güncelleştirin.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh "Create and scale Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Betik çalıştırıldıktan sonra aşağıdaki komutu kullanarak kaynak grubunu ve bununla ilişkili tüm kaynakları kaldırın. 
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, aşağıdaki tabloda ana hatları verilen komutları kullanır:

| **Komut** | **Notlar** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az mysql server create](/cli/azure/mysql/server#az-mysql-server-create) | Veritabanlarını barındıran bir MySQL sunucusu oluşturur. |
| [az MySQL Server Update](/cli/azure/mysql/server#az-mysql-server-update) | MySQL sunucusunun özelliklerini güncelleştirir. |
| [az monitor metrics list](/cli/azure/monitor/metrics#az-monitor-metrics-list) | Kaynaklar için ölçüm değerini listeleyin. |
| [az group delete](/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar
- [MySQL Için Azure veritabanı işlem ve depolama](../concepts-pricing-tiers.md) hakkında daha fazla bilgi edinin
- Ek betikleri deneyin: [MySQL için Azure Veritabanı Azure CLI örnekleri](../sample-scripts-azure-cli.md)
- [Azure CLI](/cli/azure) hakkında daha fazla bilgi edinin
