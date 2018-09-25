---
title: Kullanıcılar, MariaDB sunucusu için Azure veritabanı'nda oluşturma
description: Bu makalede, MariaDB server için Azure veritabanı ile etkileşim kurmak için yeni kullanıcı hesaplarını nasıl oluşturabileceğiniz açıklanır.
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 50154a7fee63eb3ff9e08155123f9e5962bbfcf0
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46946128"
---
# <a name="create-users-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı'nda kullanıcıları oluşturun 
Bu makalede nasıl kullanıcılar Azure veritabanı'nda MariaDB için oluşturabileceğiniz açıklanır.

MariaDB için Azure veritabanınızı ilk oluşturduğunuzda, bir Sunucu Yöneticisi oturum açma kullanıcı adı ve parola sağlanan. Daha fazla bilgi için izleyebileceğiniz [hızlı](quickstart-create-mariadb-server-database-using-azure-portal.md). Sunucu Yöneticisi oturum açma kullanıcı adını Azure portalında bulabilirsiniz.

Sunucu yönetici kullanıcısı belirli ayrıcalıklara sunucunuzun listelendiği gibi alır: seçin, Ekle, Güncelleştir, Sil, oluşturma, bırakma, yeniden yükleme, işlem, başvuruları, dizin, ALTER, Göster veritabanları, geçici tablolar oluşturmak, kilit tablolar, yürütme, çoğaltma bağımlı, çoğaltma İSTEMCİ, GÖRÜNÜMÜ OLUŞTURMAK, GÖRÜNÜMÜ GÖSTER, RUTİN OLUŞTURMA, YORDAM ALTER, KULLANICI, OLAY TETİKLEYİCİSİ OLUŞTURMA

MariaDB için Azure veritabanı oluşturulduktan sonra ek kullanıcılar oluşturma ve yönetici erişim vereceğinizi ilk sunucu yöneticisi kullanıcı hesabını kullanabilirsiniz. Ayrıca, sunucu yöneticisi hesabı, tek veritabanı şemalarını erişimi daha az ayrıcalıklı kullanıcı oluşturmak için kullanılabilir.

## <a name="create-additional-admin-users"></a>Ek yönetici kullanıcılara oluşturma
1. Bağlantı bilgileri ve yönetici kullanıcı adını alın.
   Veritabanı sunucusuna bağlanmak için tam sunucu adı ve yönetici oturum açma kimlik bilgileri gerekir. Sunucu adını ve sunucu oturum açma bilgilerini kolayca bulabilirsiniz **genel bakış** sayfası veya **özellikleri** Azure portalında sayfası. 

2. Veritabanı sunucunuza bağlanmak için yönetici hesabı ve parolayı kullanın. MySQL Workbench, mysql.exe, HeidiSQL veya diğerleri gibi tercih edilen istemci aracını kullanın. 
   Bağlanmak nasıl emin değilseniz bkz [bağlanmak ve veri sorgulamak için kullanım MySQL Workbench](./connect-workbench.md)

3. Düzenleyin ve aşağıdaki SQL kodu çalıştırın. Yer tutucu değerini için yeni kullanıcı adınızı değiştirin `new_master_user`. Bu söz dizimi listelenen tüm veritabanı şemalarını ayrıcalıklar verir (*.*) kullanıcı adına (Bu örnekte new_master_user). 

   ```sql
   CREATE USER 'new_master_user'@'%' IDENTIFIED BY 'StrongPassword!';
   
   GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER ON *.* TO 'new_master_user'@'%' WITH GRANT OPTION; 
   
   FLUSH PRIVILEGES;
   ```

4. Verir doğrulayın 
   ```sql
   USE sys;
   
   SHOW GRANTS FOR 'new_master_user'@'%';
   ```

## <a name="create-database-users"></a>Veritabanı kullanıcıları oluşturma

1. Bağlantı bilgileri ve yönetici kullanıcı adını alın.
   Veritabanı sunucusuna bağlanmak için tam sunucu adı ve yönetici oturum açma kimlik bilgileri gerekir. Sunucu adını ve sunucu oturum açma bilgilerini kolayca bulabilirsiniz **genel bakış** sayfası veya **özellikleri** Azure portalında sayfası. 

2. Veritabanı sunucunuza bağlanmak için yönetici hesabı ve parolayı kullanın. MySQL Workbench, mysql.exe, HeidiSQL veya diğerleri gibi tercih edilen istemci aracını kullanın. 
   Bağlanmak nasıl emin değilseniz bkz [bağlanmak ve veri sorgulamak için kullanım MySQL Workbench](./connect-workbench.md)

3. Düzenleyin ve aşağıdaki SQL kodu çalıştırın. Yer tutucu değerini değiştirin `db_user` hedeflenen yeni bir kullanıcı adı ve yer tutucu değerini `testdb` veritabanınızın adı ile.

   Bu sql kod söz dizimi testdb örneklerde adlı yeni bir veritabanı oluşturur. MariaDB hizmeti için Azure veritabanı'nda yeni bir kullanıcı oluşturur ve yeni veritabanı şemasına tüm ayrıcalıkları verir (testdb.\*) söz konusu kullanıcı için. 

   ```sql
   CREATE DATABASE testdb;
   
   CREATE USER 'db_user'@'%' IDENTIFIED BY 'StrongPassword!';
   
   GRANT ALL PRIVILEGES ON testdb . * TO 'db_user'@'%';
   
   FLUSH PRIVILEGES;
   ```

4. Veritabanı içinde bir onayları doğrulayın.
   ```sql
   USE testdb;
   
   SHOW GRANTS FOR 'db_user'@'%';
   ```

5. Yeni kullanıcı adı ve parola kullanarak belirli bir veritabanı belirtme sunucuya oturum açın. Bu örnek, mysql komut satırını gösterir. Bu komutla birlikte, kullanıcı adı için parola istenir. Kendi sunucu adı, veritabanı adı ve kullanıcı adını değiştirin.

   ```bash
   mysql --host mydemoserver.mariadb.database.azure.com --database testdb --user db_user@mydemoserver -p
   ```
MariaDB belgeleri için kullanıcı hesabı yönetimi hakkında daha fazla bilgi için bkz. [kullanıcı hesabı Yönetimi](https://mariadb.com/kb/en/library/user-account-management/), [verme söz dizimi](https://mariadb.com/kb/en/library/grant/), ve [ayrıcalıkları](https://mariadb.com/kb/en/library/grant/#privilege-levels).

## <a name="next-steps"></a>Sonraki adımlar
Bunların bağlanmasına imkan yeni kullanıcılar Makine IP adresleri için güvenlik duvarını açmak: [oluşturun ve Azure portalını kullanarak Azure veritabanı MariaDB için güvenlik duvarı kurallarını yönetme](howto-manage-firewall-portal.md)  

<!--or [Azure CLI](howto-manage-firewall-using-cli.md).-->
