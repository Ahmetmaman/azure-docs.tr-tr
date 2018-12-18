---
title: Azure CLI Betiği - PostgreSQL için Azure Veritabanı oluşturma
description: Örnek Azure CLI Betiği - PostgreSQL için Azure Veritabanı sunucusu oluşturur ve sunucu düzeyinde bir güvenlik duvarı kuralı yapılandırır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.date: 02/28/2018
ms.openlocfilehash: e31a51fe8004c92b629e93333348192668922792
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53536041"
---
# <a name="create-an-azure-database-for-postgresql-server-and-configure-a-firewall-rule-using-the-azure-cli"></a>Azure CLI kullanarak PostgreSQL için Azure Veritabanı sunucusu oluşturun ve bir güvenlik duvarı kuralı yapılandırın
Bu örnek CLI Betiği, PostgreSQL için Azure Veritabanı sunucusu oluşturur ve sunucu düzeyinde bir güvenlik duvarı kuralı yapılandırır. Betik başarılı şekilde çalıştırıldıktan sonra, PostgreSQL sunucusuna tüm Azure hizmetlerinden ve yapılandırılmış IP adresinden erişilebilir.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

CLI aracını yerel olarak çalıştırmayı tercih ederseniz bu makale için Azure CLI aracının 2.0 veya sonraki bir sürümü gerekir. `az --version` komutunu çalıştırarak sürümü denetleyin. Azure CLI aracını yüklemek veya sürümünüzü yükseltmek için bkz. [Azure CLI’yi Yükleme]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik
Bu örnek betikte vurgulanan satırları düzenleyerek yönetici kullanıcı adını ve parolasını kendi değerlerinizle güncelleştirin.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/create-postgresql-server-and-firewall-rule.sh?highlight=15-16 "Create an Azure Database for PostgreSQL, and server-level firewall rule.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Betik çalıştırıldıktan sonra aşağıdaki komutu kullanarak kaynak grubunu ve bununla ilişkili tüm kaynakları kaldırın. 
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/delete-postgresql.sh "Delete the resource group.")]

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, aşağıdaki tabloda ana hatları verilen komutları kullanır:

| **Komut** | **Notlar** |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az postgres server create](/cli/azure/postgres/server#az_postgres_server_create) | Veritabanlarını barındıran bir PostgreSQL sunucusu oluşturur. |
| [az postgres server firewall create](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_create) | Girilen IP adresi aralığından sunucuya ve sunucudaki veritabanlarına erişim imkanı sağlayacak bir güvenlik duvarı kuralı oluşturur. |
| [az group delete](/cli/azure/group#az_group_delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar
- Azure CLI hakkında daha fazla bilgi edinin: [Azure CLI belgeleri](/cli/azure)
- Ek betikleri deneyin: [PostgreSQL için Azure Veritabanı Azure CLI örnekleri](../sample-scripts-azure-cli.md)
