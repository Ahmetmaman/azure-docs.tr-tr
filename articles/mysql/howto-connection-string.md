---
title: Bağlantı dizeleri-MySQL için Azure veritabanı
description: Bu belgede, ADO.NET (C#), JDBC, Node.js, ODBC, PHP, Python ve Ruby dahil olmak üzere MySQL için Azure veritabanı 'na bağlanmak üzere uygulamalar için şu anda desteklenen bağlantı dizeleri listelenmektedir.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: how-to
ms.date: 3/18/2020
ms.custom: devx-track-python, devx-track-js
ms.openlocfilehash: 40af2660f0052a876ef9310bc2426295ba67558b
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "94541496"
---
# <a name="how-to-connect-applications-to-azure-database-for-mysql"></a>MySQL için Azure Veritabanına nasıl uygulama bağlanır
Bu konu, MySQL için Azure veritabanı tarafından şablonlar ve örneklerle birlikte desteklenen bağlantı dizesi türlerini listeler. Bağlantı dizeniz içinde farklı parametrelere ve ayarlara sahip olabilirsiniz.

- Sertifikayı almak için bkz. [SSL 'yi yapılandırma](./howto-configure-ssl.md).
- {your_host} = \<servername> . MySQL.Database.Azure.com
- {your_user} @ {ServerName} = kimlik doğrulaması için Kullanıcı kimliği biçimi doğru.  Yalnızca Kullanıcı kimliğini kullanıyorsanız kimlik doğrulaması başarısız olur.

## <a name="adonet"></a>ADO.NET
```ado.net
Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password};[SslMode=Required;]
```

Bu örnekte sunucu adı, `mydemoserver` veritabanı adı, `wpdb` Kullanıcı adı `WPAdmin` ve parola olur `mypassword!2` . Sonuç olarak, bağlantı dizesi şöyle olmalıdır:

```ado.net
Server= "mydemoserver.mysql.database.azure.com"; Port=3306; Database= "wpdb"; Uid= "WPAdmin@mydemoserver"; Pwd="mypassword!2"; SslMode=Required;
```

## <a name="jdbc"></a>JDBC
```jdbc
String url ="jdbc:mysql://%s:%s/%s[?verifyServerCertificate=true&useSSL=true&requireSSL=true]",{your_host},{your_port},{your_database}"; myDbConn = DriverManager.getConnection(url, {username@servername}, {your_password}";
```

## <a name="nodejs"></a>Node.js
```node.js
var conn = mysql.createConnection({host: {your_host}, user: {username@servername}, password: {your_password}, database: {your_database}, Port: {your_port}[, ssl:{ca:fs.readFileSync({ca-cert filename})}}]);
```

## <a name="odbc"></a>ODBC
```odbc
DRIVER={MySQL ODBC 5.3 UNICODE Driver};Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password}; [sslca={ca-cert filename}; sslverify=1; Option=3;]
```

## <a name="php"></a>PHP
```php
$con=mysqli_init(); [mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL);] mysqli_real_connect($con, {your_host}, {username@servername}, {your_password}, {your_database}, {your_port});
```

## <a name="python"></a>Python
```python
cnx = mysql.connector.connect(user={username@servername}, password={your_password}, host={your_host}, port={your_port}, database={your_database}[, ssl_ca={ca-cert filename}, ssl_verify_cert=true])
```

## <a name="ruby"></a>Ruby
```ruby
client = Mysql2::Client.new(username: {username@servername}, password: {your_password}, database: {your_database}, host: {your_host}, port: {your_port}[, sslca:{ca-cert filename}, sslverify:false, sslcipher:'AES256-SHA'])
```

## <a name="get-the-connection-string-details-from-the-azure-portal"></a>Azure portal bağlantı dizesi ayrıntılarını alın
[Azure Portal](https://portal.azure.com), MySQL Için Azure veritabanı sunucusuna gidin ve ardından **bağlantı dizeleri** ' ne tıklayarak örneğinizin dize listesini alın: :::image type="content" source="./media/howto-connection-strings/connection-strings-on-portal.png" alt-text="Azure Portal bağlantı dizeleri bölmesi":::

Dize, sürücü, sunucu ve diğer veritabanı bağlantı parametreleri gibi ayrıntılar sağlar. Bu örnekleri, veritabanı adı, parola vb. gibi kendi parametrelerinizi kullanacak şekilde değiştirin. Daha sonra bu dizeyi, kodunuzun ve uygulamalarınızdan sunucuya bağlanmak için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- Bağlantı kitaplıkları hakkında daha fazla bilgi için bkz. [Kavramlar-bağlantı kitaplıkları](./concepts-connection-libraries.md).
