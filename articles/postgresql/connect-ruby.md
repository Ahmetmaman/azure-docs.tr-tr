---
title: Ruby ile bağlanın - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu hızlı başlatma, PostgreSQL - Single Server için Azure Veritabanı'ndan veri bağlamak ve sorgulamak için kullanabileceğiniz bir Ruby kodu örneği sağlar.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.custom: mvc
ms.devlang: ruby
ms.topic: quickstart
ms.date: 5/6/2019
ms.openlocfilehash: 5dd4cfe9bee0db6f14f736c79fe91770641008fb
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74766914"
---
# <a name="azure-database-for-postgresql---single-server-use-ruby-to-connect-and-query-data"></a>PostgreSQL için Azure Veritabanı - Tek Sunucu: Verileri bağlamak ve sorgulamak için Ruby'yi kullanın
Bu hızlı başlangıçta, [Ruby](https://www.ruby-lang.org) uygulaması kullanılarak PostgreSQL için Azure Veritabanı’na nasıl bağlanılacağı gösterilmiştir. Ayrıca veritabanında veri sorgulamak, eklemek, güncelleştirmek ve silmek için SQL deyimlerini nasıl kullanacağınız da gösterilmiştir. Bu makaledeki adımlarda, Ruby kullanarak geliştirmeyle ilgili bilgi sahibi olduğunuz ve PostgreSQL için Azure Veritabanı ile çalışmaya yeni başladığınız varsayılır.

## <a name="prerequisites"></a>Ön koşullar
Bu hızlı başlangıçta, başlangıç noktası olarak şu kılavuzlardan birinde oluşturulan kaynaklar kullanılmaktadır:
- [DB Oluşturma - Portal](quickstart-create-server-database-portal.md)
- [DB Oluşturma - Azure CLI](quickstart-create-server-database-azure-cli.md)

Ayrıca yüklü olması gerekir:
- [Ruby](https://www.ruby-lang.org/en/downloads/)
- Ruby pg, Ruby için PostgreSQL modülü

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma
PostgreSQL için Azure Veritabanı'na bağlanmak üzere gereken bağlantı bilgilerini alın. Tam sunucu adına ve oturum açma kimlik bilgilerine ihtiyacınız vardır.

1. [Azure portalına](https://portal.azure.com/)giriş yapın.
2. Azure portalında sol taraftaki menüden **Tüm kaynaklar**'a tıklayın ve oluşturduğunuz sunucuyu (örneğin, **mydemoserver**) arayın.
3. Sunucunun adına tıklayın.
4. Sunucunun **Genel Bakış** panelinden **Sunucu adı** ile **Sunucu yöneticisi oturum açma adı**’nı not alın. Parolanızı unutursanız, bu panelden parolayı da sıfırlayabilirsiniz.
 ![PostgreSQL için Azure Veritabanı sunucu adı](./media/connect-ruby/1-connection-string.png)

> [!NOTE]
> Azure `@` Postgres kullanıcı adındaki sembol, tüm bağlantı `%40` dizelerinde olduğu gibi url olarak kodlanmıştır. 

## <a name="connect-and-create-a-table"></a>Bağlanma ve tablo oluşturma
**CREATE TABLE** SQL deyimini kullanarak bir tabloyu bağlamak ve oluşturmak ve ardından **INSERT INTO** SQL deyimlerini kullanarak tabloya satırlar eklemek için aşağıdaki kodu kullanın.

Kod, PostgreSQL için Azure Veritabanı’na bağlanmak amacıyla [new()](https://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) oluşturucusuna sahip [PG::Connection](https://www.rubydoc.info/gems/pg/PG/Connection) nesnesini kullanır. Ardından DROP, CREATE TABLE ve INSERT INTO komutlarını çalıştırmak için [exec()](https://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) yöntemini çağırır. Kod, [PG::Error](https://www.rubydoc.info/gems/pg/PG/Error) sınıfını kullanarak hataları kontrol eder. Ardından bağlantıyı sonlandırılmadan önce kapatmak için [close()](https://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) yöntemini çağırır.

`host`, `database`, `user` ve `password` dizelerini kendi değerlerinizle değiştirin. 


```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mydemoserver.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin%40mydemoserver')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database'

    # Drop previous table of same name if one exists
    connection.exec('DROP TABLE IF EXISTS inventory;')
    puts 'Finished dropping table (if existed).'

    # Drop previous table of same name if one exists.
    connection.exec('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);')
    puts 'Finished creating table.'

    # Insert some data into table.
    connection.exec("INSERT INTO inventory VALUES(1, 'banana', 150)")
    connection.exec("INSERT INTO inventory VALUES(2, 'orange', 154)")
    connection.exec("INSERT INTO inventory VALUES(3, 'apple', 100)")
    puts 'Inserted 3 rows of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="read-data"></a>Verileri okuma
Bağlanmak ve **SELECT** SQL deyimi kullanarak verileri okumak için aşağıdaki kodu kullanın. 

Kod, PostgreSQL için Azure Veritabanı’na bağlanmak amacıyla [new()](https://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) oluşturucusuna sahip [PG::Connection](https://www.rubydoc.info/gems/pg/PG/Connection) nesnesini kullanır. Ardından sonuçları sonuç kümesinde tutarak SELECT komutunu çalıştırmak için [exec()](https://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) yöntemini çağırır. Sonuç kümesi koleksiyonu, `resultSet.each do` döngüsü kullanılarak ve geçerli satır değerleri `row` değişkeninde tutularak yinelenir. Kod, [PG::Error](https://www.rubydoc.info/gems/pg/PG/Error) sınıfını kullanarak hataları kontrol eder. Ardından bağlantıyı sonlandırılmadan önce kapatmak için [close()](https://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) yöntemini çağırır.

`host`, `database`, `user` ve `password` dizelerini kendi değerlerinizle değiştirin. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mydemoserver.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin%40mydemoserver')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :database => dbname, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    resultSet = connection.exec('SELECT * from inventory;')
    resultSet.each do |row|
        puts 'Data row = (%s, %s, %s)' % [row['id'], row['name'], row['quantity']]
    end

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="update-data"></a>Verileri güncelleştirme
**UPDATE** SQL deyimini kullanarak bağlanmak ve verileri güncelleştirmek için aşağıdaki kodu kullanın.

Kod, PostgreSQL için Azure Veritabanı’na bağlanmak amacıyla [new()](https://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) oluşturucusuna sahip [PG::Connection](https://www.rubydoc.info/gems/pg/PG/Connection) nesnesini kullanır. Ardından UPDATE komutunu çalıştırmak için [exec()](https://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) yöntemini çağırır. Kod, [PG::Error](https://www.rubydoc.info/gems/pg/PG/Error) sınıfını kullanarak hataları kontrol eder. Ardından bağlantıyı sonlandırılmadan önce kapatmak için [close()](https://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) yöntemini çağırır.

`host`, `database`, `user` ve `password` dizelerini kendi değerlerinizle değiştirin. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mydemoserver.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin%40mydemoserver')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    # Modify some data in table.
    connection.exec('UPDATE inventory SET quantity = %d WHERE name = %s;' % [200, '\'banana\''])
    puts 'Updated 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```


## <a name="delete-data"></a>Verileri silme
Bağlanmak ve **DELETE** SQL deyimi kullanarak verileri okumak için aşağıdaki kodu kullanın. 

Kod, PostgreSQL için Azure Veritabanı’na bağlanmak amacıyla [new()](https://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) oluşturucusuna sahip [PG::Connection](https://www.rubydoc.info/gems/pg/PG/Connection) nesnesini kullanır. Ardından UPDATE komutunu çalıştırmak için [exec()](https://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) yöntemini çağırır. Kod, [PG::Error](https://www.rubydoc.info/gems/pg/PG/Error) sınıfını kullanarak hataları kontrol eder. Ardından bağlantıyı sonlandırılmadan önce kapatmak için [close()](https://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) yöntemini çağırır.

`host`, `database`, `user` ve `password` dizelerini kendi değerlerinizle değiştirin. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mydemoserver.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin%40mydemoserver')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection to database.'

    # Modify some data in table.
    connection.exec('DELETE FROM inventory WHERE name = %s;' % ['\'orange\''])
    puts 'Deleted 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Dışarı Aktarma ve İçeri Aktarma seçeneğini kullanarak veritabanınızı geçirme](./howto-migrate-using-export-and-import.md)
