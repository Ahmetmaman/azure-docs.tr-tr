---
title: Erişim sunucusu, Azure CLI kullanarak MariaDB için Azure veritabanı'nda oturum
description: Bu makalede, Azure CLI komut satırı yardımcı programını kullanarak MariaDB için Azure veritabanı'nda sunucu günlüklerini erişmeye açıklar.
author: rachel-msft
ms.author: raagyema
ms.service: mariadb
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 11/10/2018
ms.openlocfilehash: 403f111882b1bd151b26af56fd614355e7ba88e3
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53539220"
---
# <a name="configure-and-access-server-logs-by-using-azure-cli"></a>Yapılandırma ve Azure CLI kullanarak sunucu günlüklerine erişme
Sunucu günlükleri MariaDB için Azure veritabanı Azure CLI, Azure komut satırı yardımcı programını kullanarak indirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda adımlamak için ihtiyacınız vardır:
- [MariaDB için Azure veritabanı](quickstart-create-mariadb-server-database-using-azure-cli.md)
- [Azure CLI](/cli/azure/install-azure-cli) veya tarayıcıda Azure Cloud Shell

## <a name="configure-logging-for-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı için günlük tutmayı yapılandırma
Aşağıdaki adımları izleyerek MariaDB yavaş sorgu günlüğü erişmek için sunucu yapılandırabilirsiniz:
1. Günlük özelliğini açar ayarlayarak **yavaş\_sorgu\_günlük** on parametresi.
2. Diğer parametreler gibi ayarlayın **uzun\_sorgu\_zaman** ve **günlük\_yavaş\_yönetici\_deyimleri**.

Azure CLI aracılığıyla bu parametre değeri hakkında bilgi edinmek için bkz: [sunucu parametrelerini yapılandırma](howto-configure-server-parameters-cli.md).

Örneğin, aşağıdaki CLI komutunu yavaş sorgu oturum açar, uzun sorgu süresi 10 saniye olarak ayarlar ve ardından yavaş yönetici deyiminin günlüğünü kapatır. Son olarak, gözden geçirmeniz için yapılandırma seçeneklerini listeler.
```azurecli-interactive
az mariadb server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
az mariadb server configuration set --name long_query_time --resource-group myresourcegroup --server mydemoserver --value 10
az mariadb server configuration set --name log_slow_admin_statements --resource-group myresourcegroup --server mydemoserver --value OFF
az mariadb server configuration list --resource-group myresourcegroup --server mydemoserver
```

## <a name="list-logs-for-azure-database-for-mariadb-server"></a>MariaDB sunucusu için Azure veritabanı listesi günlükleri
Sunucunuz için kullanılabilir günlük dosyaları için Çalıştır [az mariadb server-logs listesi](/cli/azure/mariadb/server-logs#az-mariadb-server-logs-list) komutu.

Sunucu için günlük dosyalarını listeleyebilirsiniz **mydemoserver.mariadb.database.azure.com** kaynak grubu altında **myresourcegroup**. Ardından adlı bir metin dosyasına günlük dosyalarının listesi doğrudan **günlük\_dosyaları\_list.txt**.
```azurecli-interactive
az mariadb server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-from-the-server"></a>Sunucudan günlükleri indir
İle [az mariadb sunucu günlüklerini indirme](/cli/azure/mariadb/server-logs#az-mariadb-server-logs-download) komutunu sunucunuz için ayrı günlük dosyalarına karşıdan yükleyebilirsiniz.

Sunucu için belirli günlük dosyasını indirmek için aşağıdaki örneği kullanın **mydemoserver.mariadb.database.azure.com** kaynak grubu altında **myresourcegroup** yerel ortamınıza.
```azurecli-interactive
az mariadb server-logs download --name mysql-slow-mydemoserver-2018110800.log --resource-group myresourcegroup --server mydemoserver
```

## <a name="next-steps"></a>Sonraki adımlar
- Hakkında bilgi edinin [sunucu MariaDB için Azure veritabanı'nda oturum](concepts-server-logs.md).
