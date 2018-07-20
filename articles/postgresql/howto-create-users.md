---
title: Kullanıcılar, PostgreSQL sunucusu için Azure veritabanı'nda oluşturma
description: Bu makalede, PostgreSQL sunucusu için Azure veritabanı ile etkileşim kurmak için yeni kullanıcı hesaplarını nasıl oluşturacağınızı açıklar.
services: postgresql
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
manager: jhubbard
ms.service: postgresql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: be3be5769f1d9ad6138fcef851179ddc5cf6c0da
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39144746"
---
# <a name="create-users-in-azure-database-for-postgresql-server"></a>Kullanıcılar, PostgreSQL sunucusu için Azure veritabanı'nda oluşturma 
Bu makalede, PostgreSQL sunucusu için Azure veritabanı kullanıcıları nasıl oluşturabileceğiniz açıklanır.

## <a name="the-server-admin-account"></a>Sunucu yöneticisi hesabı
PostgreSQL için Azure veritabanınızı ilk oluşturduğunuzda, bir sunucu yöneticisi kullanıcı adı ve parola sağlanan. Daha fazla bilgi için izleyebileceğiniz [hızlı](quickstart-create-server-database-portal.md) adım adım bir yaklaşım görmek için. Sunucu Yöneticisi kullanıcı adı özel bir adı olduğundan, seçilen sunucu yöneticisi kullanıcı adını Azure portalında bulabilirsiniz.

PostgreSQL sunucusu için Azure veritabanı, tanımlanan 3 varsayılan rolleri ile oluşturulur. Komutunu çalıştırarak bu rolleri görebilirsiniz: `SELECT rolname FROM pg_roles;`
- azure_pg_admin
- azure_superuser
- Sunucu yönetici kullanıcı

Server yönetici kullanıcı, azure_pg_admin rolünün üyesidir. Ancak, sunucu yöneticisi hesabı azure_superuser rolünün bir parçası değil. Bu hizmet yönetilen bir PaaS hizmeti olduğundan Microsoft yalnızca süper kullanıcı rolü bir parçasıdır. 

PostgreSQL altyapısı ayrıcalıkları veritabanı nesnelerine erişimi denetlemek için açıklandığı gibi kullanır [PostgreSQL ürün belgelerine](https://www.postgresql.org/docs/current/static/sql-createrole.html). PostgreSQL için Azure veritabanı'nda sunucu yönetici kullanıcısı bu ayrıcalıklar verilir: oturum açma, NOSUPERUSER, DEVRALMA, CREATEDB, CREATEROLE, NOREPLICATION

Sunucu Yöneticisi kullanıcı hesabı, ek kullanıcılar oluşturma ve bu kullanıcılara azure_pg_admin rolünde vermek için kullanılabilir. Ayrıca, sunucu yöneticisi hesabı daha az ayrıcalıklı kullanıcıların ve tek veritabanları ve şemaları erişiminiz rolleri oluşturmak için kullanılabilir.

## <a name="how-to-create-additional-admin-users-in-azure-database-for-postgresql"></a>Ek yönetici kullanıcılara PostgreSQL için Azure veritabanı'nda oluşturma
1. Bağlantı bilgileri ve yönetici kullanıcı adını alın.
   Veritabanı sunucusuna bağlanmak için tam sunucu adı ve yönetici oturum açma kimlik bilgileri gerekir. Sunucu adını ve sunucu oturum açma bilgilerini kolayca bulabilirsiniz **genel bakış** sayfası veya **özellikleri** Azure portalında sayfası. 

2. Veritabanı sunucunuza bağlanmak için yönetici hesabı ve parolayı kullanın. PgAdmin veya psql gibi tercih edilen istemci aracını kullanın.
   Bağlanmak nasıl emin değilseniz bkz [Cloud Shell'de psql kullanarak PostgreSQL veritabanına bağlanma](./quickstart-create-server-database-portal.md#connect-to-the-postgresql-database-by-using-psql-in-cloud-shell)

3. Düzenleyin ve aşağıdaki SQL kodu çalıştırın. Yer tutucu değerini < new_user > için yeni kullanıcı adınızı değiştirin ve yer tutucu parola kendi güçlü bir parola ile değiştirin. 

   ```sql
   CREATE ROLE <new_user> WITH LOGIN NOSUPERUSER INHERIT CREATEDB CREATEROLE NOREPLICATION PASSWORD '<StrongPassword!>';
   
   GRANT azure_pg_admin TO <new_user>;
   ```

## <a name="how-to-create-database-users-in-azure-database-for-postgresql"></a>Nasıl PostgreSQL için Azure veritabanı'nda veritabanı kullanıcıları oluşturma

1. Bağlantı bilgileri ve yönetici kullanıcı adını alın.
   Veritabanı sunucusuna bağlanmak için tam sunucu adı ve yönetici oturum açma kimlik bilgileri gerekir. Sunucu adını ve sunucu oturum açma bilgilerini kolayca bulabilirsiniz **genel bakış** sayfası veya **özellikleri** Azure portalında sayfası. 

2. Veritabanı sunucunuza bağlanmak için yönetici hesabı ve parolayı kullanın. PgAdmin veya psql gibi tercih edilen istemci aracını kullanın.

3. Düzenleyin ve aşağıdaki SQL kodu çalıştırın. Yer tutucu değerini değiştirin `<db_user>` hedeflenen yeni bir kullanıcı adı ve yer tutucu değerini `<newdb>` veritabanınızın adı ile. Yer tutucusunu kendi güçlü bir parola ile değiştirin. 

   Bu sql kod söz dizimi testdb, örnek amacıyla adlı yeni bir veritabanı oluşturur. Ardından PostgreSQL hizmetinde yeni bir kullanıcı oluşturur ve bu kullanıcı için yeni veritabanı ayrıcalıkları verir bağlanın. 

   ```sql
   CREATE DATABASE <newdb>;
   
   CREATE ROLE <db_user> WITH LOGIN NOSUPERUSER INHERIT CREATEDB NOCREATEROLE NOREPLICATION PASSWORD '<StrongPassword!>';
   
   GRANT CONNECT ON DATABASE <newdb> TO <db_user>;
   ```

4. Bir yönetici hesabını kullanarak veritabanı nesneleri güvenli hale getirmek için ek ayrıcalıkları gerekebilir. Başvurmak [PostgreSQL belgeleri](https://www.postgresql.org/docs/current/static/ddl-priv.html) veritabanı rolleri ve ayrıcalıkları hakkında daha ayrıntılı bilgi için. Örneğin: 
   ```sql
   GRANT ALL PRIVILEGES ON DATABASE <newdb> TO <db_user>;
   ```

5. Yeni kullanıcı adı ve parola kullanarak belirli bir veritabanı belirtme sunucunuz için oturum açın. Bu örnek psql komut satırını gösterir. Bu komutla birlikte, kullanıcı adı için parola istenir. Kendi sunucu adı, veritabanı adı ve kullanıcı adını değiştirin.

   ```azurecli-interactive
   psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=db_user@mydemoserver --dbname=newdb
   ```

## <a name="next-steps"></a>Sonraki adımlar
Bunların bağlanmasına imkan yeni kullanıcılar Makine IP adresleri için güvenlik duvarını açmak: [oluşturun ve Azure veritabanı Azure portalını kullanarak PostgreSQL için güvenlik duvarı kurallarını yönetme](howto-manage-firewall-using-portal.md) veya [Azure CLI](howto-manage-firewall-using-cli.md).

Kullanıcı hesabı yönetimi hakkında daha fazla bilgi için PostgreSQL ürün belgelerine bakın [veritabanı rolleri ve ayrıcalıkları](https://www.postgresql.org/docs/current/static/user-manag.html), [verme söz dizimi](https://www.postgresql.org/docs/current/static/sql-grant.html), ve [ayrıcalıkları](https://www.postgresql.org/docs/current/static/ddl-priv.html).
