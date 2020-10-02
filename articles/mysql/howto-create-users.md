---
title: Veritabanları ve kullanıcılar oluşturma-MySQL için Azure veritabanı
description: Bu makalede, MySQL için Azure veritabanı sunucusu ile etkileşim kurmak üzere nasıl yeni kullanıcı hesapları oluşturabileceğiniz açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: how-to
ms.date: 10/1/2020
ms.openlocfilehash: ed653ffb6fc24a75170d51d345c0c64724ff90f1
ms.sourcegitcommit: b4f303f59bb04e3bae0739761a0eb7e974745bb7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2020
ms.locfileid: "91651030"
---
# <a name="create-databases-and-users-in-azure-database-for-mysql-server"></a>MySQL için Azure veritabanı sunucusu 'nda veritabanları ve kullanıcılar oluşturma

[!INCLUDE[applies-to-single-flexible-server](includes/applies-to-single-flexible-server.md)]

Bu makalede, MySQL için Azure veritabanı sunucusunda nasıl Kullanıcı oluşturabileceğiniz açıklanır.

> [!NOTE]
> Sapma ücretsiz iletişim
>
> Microsoft, farklı ve üçlü ortamları destekler. Bu makale, _İkincil_sözcüğe başvurular içerir. Kullanım açısından [ücretsiz iletişim Için Microsoft Stil Kılavuzu](https://github.com/MicrosoftDocs/microsoft-style-guide/blob/master/styleguide/bias-free-communication.md) bunu bir exclusionword olarak tanır. Bu makalede, şu anda yazılımda görüntülenen sözcük olduğundan, bu makalede tutarlılık için kullanılır. Yazılım, sözcüğü kaldıracak şekilde güncelleniyorsa, bu makale hizalamayla olacak şekilde güncelleştirilir.
>

MySQL için Azure veritabanınızı ilk oluşturduğunuzda, bir Sunucu Yöneticisi oturum açma Kullanıcı adı ve parolası sağladınız. Daha fazla bilgi için [hızlı](quickstart-create-mysql-server-database-using-azure-portal.md)başlangıcı izleyebilirsiniz. Sunucu Yöneticisi oturum açma kullanıcı adınızı Azure portal bulabilirsiniz.

Sunucu Yöneticisi kullanıcısı, sunucunuzda listelenen belirli ayrıcalıkları alır: 

   SEÇME, EKLEME, GÜNCELLEŞTIRME, SILME, OLUŞTURMA, BıRAKMA, YENIDEN YÜKLEME, IŞLEME, BAŞVURULARı, DIZIN, DEĞIŞTIRME, VERITABANLARıNı GÖSTERME, GEÇICI TABLOLAR OLUŞTURMA, TABLOLARı KILITLEME, YÜRÜTME, ÇOĞALTMA BAĞıMLı, ÇOĞALTMA ISTEMCISI, GÖRÜNÜM OLUŞTURMA, GÖRÜNÜMÜ GÖSTERME, RUTIN OLUŞTURMA, RUTIN YORDAM, KULLANıCı OLUŞTURMA, OLAY, TETIKLEYICI


MySQL sunucusu için Azure veritabanı oluşturulduktan sonra, ek kullanıcılar oluşturmak ve bunlara yönetici erişimi vermek için ilk sunucu yöneticisi Kullanıcı hesabını kullanabilirsiniz. Ayrıca, sunucu yöneticisi hesabı ayrı veritabanı şemalarına erişimi olan daha az ayrıcalıklı kullanıcı oluşturmak için kullanılabilir.

> [!NOTE]
> Süper ayrıcalık ve DBA rolü desteklenmez. Hizmette Nelerin desteklenmediğini anlamak için sınırlamalar makalesindeki [ayrıcalıkları](concepts-limits.md#privileges--data-manipulation-support) gözden geçirin.<br><br>
> "Validate_password" ve "caching_sha2_password" gibi parola eklentileri hizmet tarafından desteklenmez.

## <a name="how-to-create-database-with-non-admin-user-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda yönetici olmayan kullanıcı ile veritabanı oluşturma

1. Bağlantı bilgilerini ve yönetici kullanıcı adını alın.
   Veritabanı sunucusuna bağlanmak için tam sunucu adı ve yönetici oturum açma kimlik bilgileri gerekir. Sunucu adını ve oturum açma bilgilerini sunucuya **genel bakış** sayfasından veya Azure Portal **Özellikler** sayfasından kolayca bulabilirsiniz.

2. Veritabanı sunucunuza bağlanmak için yönetici hesabı ve parolasını kullanın. MySQL çalışma ekranı, mysql.exe, HeidiSQL veya diğerleri gibi tercih ettiğiniz istemci aracını kullanın.
   Nasıl bağlanacağınızdan emin değilseniz, bkz. MySQL çalışma ekranı kullanarak [tek sunucuya yönelik verileri bağlama ve sorgulama](./connect-workbench.md) veya [esnek sunucu için verileri bağlama ve sorgulama](./flexible-server/connect-workbench.md)

3. Aşağıdaki SQL kodunu düzenleyin ve çalıştırın. Yer tutucu değerini `db_user` amaçlanan Yeni Kullanıcı adınızla ve yer tutucu değerini `testdb` kendi veritabanı adınızla değiştirin.

   Bu SQL kod sözdizimi, örnek olarak TestDB adlı yeni bir veritabanı oluşturur. Ardından MySQL hizmetinde yeni bir kullanıcı oluşturur ve bu kullanıcı için yeni veritabanı şemasına (TestDB.) tüm ayrıcalıklar verir \* .

   ```sql
   CREATE DATABASE testdb;

   CREATE USER 'db_user'@'%' IDENTIFIED BY 'StrongPassword!';

   GRANT ALL PRIVILEGES ON testdb . * TO 'db_user'@'%';

   FLUSH PRIVILEGES;
   ```

4. Veritabanının içindeki izni doğrulayın.

   ```sql
   USE testdb;

   SHOW GRANTS FOR 'db_user'@'%';
   ```

5. Yeni Kullanıcı adı ve parolayı kullanarak, belirlenen veritabanını belirterek sunucuda oturum açın. Bu örnekte MySQL komut satırı gösterilmektedir. Bu komutla Kullanıcı adı için parola istenir. Kendi sunucu adı, veritabanı adı ve Kullanıcı adınızı değiştirin.

# <a name="single-server"></a>[Tek sunucu](#tab/single-server)

   ```azurecli-interactive
   mysql --host mydemoserver.mysql.database.azure.com --database testdb --user db_user@mydemoserver -p
   ```
# <a name="flexible-server"></a>[Esnek sunucu](#tab/flexible-server)

   ```azurecli-interactive
   mysql --host mydemoserver.mysql.database.azure.com --database testdb --user db_user -p
   ```
 ---

## <a name="how-to-create-additional-admin-users-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda ek yönetici kullanıcılar oluşturma

1. Bağlantı bilgilerini ve yönetici kullanıcı adını alın.
   Veritabanı sunucusuna bağlanmak için tam sunucu adı ve yönetici oturum açma kimlik bilgileri gerekir. Sunucu adını ve oturum açma bilgilerini sunucuya **genel bakış** sayfasından veya Azure Portal **Özellikler** sayfasından kolayca bulabilirsiniz.

2. Veritabanı sunucunuza bağlanmak için yönetici hesabı ve parolasını kullanın. MySQL çalışma ekranı, mysql.exe, HeidiSQL veya diğerleri gibi tercih ettiğiniz istemci aracını kullanın.
   Nasıl bağlanacağınızdan emin değilseniz, bkz. [bağlanmak ve veri sorgulamak Için MySQL çalışma ekranı kullanma](./connect-workbench.md)

3. Aşağıdaki SQL kodunu düzenleyin ve çalıştırın. Yeni Kullanıcı adınızı yer tutucu değeri için değiştirin `new_master_user` . Bu sözdizimi, tüm veritabanı şemalarında (*.*) listelenen ayrıcalıkları Kullanıcı adına (bu örnekte new_master_user) verir.

   ```sql
   CREATE USER 'new_master_user'@'%' IDENTIFIED BY 'StrongPassword!';

   GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER ON *.* TO 'new_master_user'@'%' WITH GRANT OPTION;

   FLUSH PRIVILEGES;
   ```

4. İzni doğrula

   ```sql
   USE sys;

   SHOW GRANTS FOR 'new_master_user'@'%';
   ```

## <a name="azure_superuser"></a>azure_superuser

MySQL için Azure veritabanı sunucuları, "azure_superuser" adlı bir kullanıcıyla oluşturulur. Bu, Microsoft tarafından, izleme, yedeklemeler ve diğer normal bakım işlemlerini gerçekleştirmek üzere sunucuyu yönetmek için oluşturulan bir sistem hesabıdır. Çağrı tabanlı mühendisler Ayrıca, sertifika kimlik doğrulamasıyla bir olay sırasında sunucuya erişmek için bu hesabı kullanabilir ve tam zamanında (JıT) süreçler kullanarak erişim istemesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bağlantı kurmasını sağlamak için yeni kullanıcıların makinelerinin IP adresleri için güvenlik duvarını açın:
- [Tek bir sunucuda güvenlik duvarı kuralları oluşturma ve yönetme](howto-manage-firewall-using-portal.md) 
- [ Esnek sunucuda güvenlik duvarı kuralları oluşturma ve yönetme](flexible-server/how-to-connect-tls-ssl.md)

Kullanıcı hesabı yönetimi hakkında daha fazla bilgi için bkz. [Kullanıcı hesabı yönetimi](https://dev.mysql.com/doc/refman/5.7/en/access-control.html)için MySQL ürün belgeleri, [sözdizimi verme](https://dev.mysql.com/doc/refman/5.7/en/grant.html)ve [ayrıcalıklar](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html).
