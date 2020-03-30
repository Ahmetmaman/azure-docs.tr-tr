---
title: 'Hızlı Başlangıç: Python ile Cassandra API’si - Azure Cosmos DB'
description: Bu hızlı başlangıçta Python ile profil uygulaması oluşturmak için Azure Cosmos DB Apache API’sinin nasıl kullanılacağı gösterilmektedir.
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.devlang: python
ms.topic: quickstart
ms.date: 09/24/2018
ms.openlocfilehash: 0b432653c452b6763e746f61b86e881c9cee62cc
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77134608"
---
# <a name="quickstart-build-a-cassandra-app-with-python-sdk-and-azure-cosmos-db"></a>Quickstart: Python SDK ve Azure Cosmos DB ile cassandra uygulaması oluşturun

> [!div class="op_single_selector"]
> * [.NET](create-cassandra-dotnet.md)
> * [Java](create-cassandra-java.md)
> * [Node.js](create-cassandra-nodejs.md)
> * [Python](create-cassandra-python.md)
>  

Bu hızlı başlangıçta, bir Azure Cosmos DB Cassandra API hesabı oluşturursunuz ve Bir Cassandra veritabanı ve kapsayıcı oluşturmak için GitHub'dan klonlanmış bir Cassandra Python uygulaması kullanırsınız. Azure Cosmos DB, belge, tablo, anahtar değeri ve grafik veritabanlarını küresel dağıtım ve yatay ölçek özelliklerine sahip hızlı bir şekilde oluşturmanıza ve sorgulamanıza olanak tanıyan çok modelli bir veritabanı hizmetidir.

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) Veya Azure aboneliği olmadan [Azure Cosmos DB'yi ücretsiz olarak deneyin.](https://azure.microsoft.com/try/cosmosdb/)
- [Python 2.7.14+ veya 3.4+](https://www.python.org/downloads/).
- [Git.](https://git-scm.com/downloads)
- [Apache Cassandra için Python Sürücüsü](https://github.com/datastax/python-driver).

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

Bir belge veritabanı oluşturmadan önce Azure Cosmos DB ile bir Cassandra hesabı oluşturmanız gerekir.

[!INCLUDE [cosmos-db-create-dbaccount-cassandra](../../includes/cosmos-db-create-dbaccount-cassandra.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir Cassandra API uygulamasını klonlayalım, bağlantı dizesini ayarlayalım ve çalıştıralım. Verilerle programlı bir şekilde çalışmanın ne kadar kolay olduğunu görüyorsunuz. 

1. Bir komut istemi açın. `git-samples` adlı yeni bir klasör oluşturun. Ardından, komut istemini kapatın.

    ```bash
    md "C:\git-samples"
    ```

2. Git Bash gibi bir Git terminal penceresi açın ve örnek uygulamayı yüklemek üzere yeni bir klasör olarak değiştirmek için `cd` komutunu kullanın.

    ```bash
    cd "C:\git-samples"
    ```

3. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulamanın bir kopyasını oluşturur.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-cassandra-python-getting-started.git
    ```

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Kodun veritabanı kaynaklarını nasıl oluşturduğunu öğrenmek istiyorsanız aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Parçacıkların tümü *pyquickstart.py* dosyasından alınır. Aksi durumda, [Bağlantı dizenizi güncelleştirme](#update-your-connection-string) bölümüne atlayabilirsiniz. 

* Kullanıcı adı ve parola değerleri, Azure portalındaki bağlantı dizesi sayfası kullanılarak ayarlanmıştır. `path\to\cert` bir X509 sertifikasının yolunu sağlar. 

   ```python
    ssl_opts = {
            'ca_certs': 'path\to\cert',
            'ssl_version': ssl.PROTOCOL_TLSv1_2
            }
    auth_provider = PlainTextAuthProvider( username=cfg.config['username'], password=cfg.config['password'])
    cluster = Cluster([cfg.config['contactPoint']], port = cfg.config['port'], auth_provider=auth_provider, ssl_options=ssl_opts)
    session = cluster.connect()
   
   ```

* `cluster`, contactPoint bilgileriyle başlatılır. ContactPoint, Azure portalından alınır.

    ```python
   cluster = Cluster([cfg.config['contactPoint']], port = cfg.config['port'], auth_provider=auth_provider)
    ```

* `cluster`, Azure Cosmos DB Cassandra API’sine bağlanır.

    ```python
    session = cluster.connect()
    ```

* Yeni bir anahtar alanı oluşturulur.

    ```python
   session.execute('CREATE KEYSPACE IF NOT EXISTS uprofile WITH replication = {\'class\': \'NetworkTopologyStrategy\', \'datacenter1\' : \'1\' }')
    ```

* Yeni bir tablo oluşturulur.

   ```
   session.execute('CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)');
   ```

* Anahtar/değer varlıkları eklenir.

    ```Python
    insert_data = session.prepare("INSERT INTO  uprofile.user  (user_id, user_name , user_bcity) VALUES (?,?,?)")
    session.execute(insert_data, [1,'Lybkov','Seattle'])
    session.execute(insert_data, [2,'Doniv','Dubai'])
    session.execute(insert_data, [3,'Keviv','Chennai'])
    session.execute(insert_data, [4,'Ehtevs','Pune'])
    session.execute(insert_data, [5,'Dnivog','Belgaum'])
    ....
    
    ```

* Tüm anahtar değerlerini almak için sorgu.

    ```Python
    rows = session.execute('SELECT * FROM uprofile.user')
    ```  
    
* Bir anahtar-değeri almak için sorgu.

    ```Python
    
    rows = session.execute('SELECT * FROM uprofile.user where user_id=1')
    ```  

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin. Bağlantı dizesi, uygulamanızın barındırılan veritabanıyla iletişim kurmasına olanak tanır.

1. [Azure portalındaki](https://portal.azure.com/)Azure Cosmos DB hesabınızda **Bağlantı Dizesini**seçin. 

    En üstteki USERNAME değerini kopyalamak için ekranın sağ tarafındaki ![Kopyala düğmesini](./media/create-cassandra-python/copy.png) düğmesini kullanın.

    ![Azure portalında erişim için kullanıcı adı, parola ve erişim noktasını görüntüleme ve kopyalama, bağlantı dizesi dikey penceresi](./media/create-cassandra-python/keys.png)

2. *config.py* dosyasını açın. 

3. Portaldan CONTACT POINT değerini 10. satırda `<FILLME>` üzerine yapıştırın.

    10. satır şuna benzer şekilde görünmelidir: 

    `'contactPoint': 'cosmos-db-quickstarts.cassandra.cosmosdb.azure.com:10350'`

4. Portaldan USERNAME değerini kopyalayın ve 6. satırda `<FILLME>` üzerine yapıştırın.

    6. satır şuna benzer şekilde görünmelidir: 

    `'username': 'cosmos-db-quickstart',`
    
5. Portaldan PASSWORD değerini kopyalayın ve 8. satırda `<FILLME>` üzerine yapıştırın.

    8. satır şuna benzer şekilde görünmelidir:

    `'password' = '2Ggkr662ifxz2Mg==`';`

6. *config.py* dosyasını kaydedin.
    
## <a name="use-the-x509-certificate"></a>X509 sertifikası kullanma

1. Baltimore CyberTrust Root sertifikasını [https://cacert.omniroot.com/bc2025.crt](https://cacert.omniroot.com/bc2025.crt)yerel olarak indirin. Dosya uzantısı *.cer'i*kullanarak dosyayı yeniden adlandırın.

   Sertifika `02:00:00:b9` seri numarasına ve `d4🇩🇪20:d0:5e:66:fc:53:fe:1a:50:88:2c:78:db:28:52:ca:e4:74` SHA1 parmak izine sahiptir.

2. *pyquickstart.py* açın ve `path\to\cert` yeni sertifikanızı işaret edin.

3. *pyquickstart.py*kaydedin.

## <a name="run-the-python-app"></a>Python uygulamasını çalıştırma

1. `azure-cosmos-db-cassandra-python-getting-started` klasörüne geçmek için git terminalindeki cd komutunu kullanın. 

2. Gerekli modülleri yüklemek için aşağıdaki komutları çalıştırın:

    ```python
    python -m pip install cassandra-driver
    python -m pip install prettytable
    python -m pip install requests
    python -m pip install pyopenssl
    ```

2. Python uygulamanızı başlatmak için aşağıdaki komutu çalıştırın:

    ```
    python pyquickstart.py
    ```

3. Sonuçların beklendiği gibi olduğunu komut satırından kontrol edin.

    Programın yürütülmesini durdurmak ve konsol penceresini kapatmak için CTRL+C tuşuna basın. 

    ![Çıktıyı görüntüleme ve doğrulama](./media/create-cassandra-python/output.png)
    
4. Azure portalında bu yeni verileri sorgulamak, değiştirmek ve birlikte çalışmak için **Veri Gezgini**'ni açın. 

    ![Veri Gezgini’nde verileri görüntüleme](./media/create-cassandra-python/data-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Cassandra API ile Azure Cosmos DB hesabı oluşturmayı ve Cassandra veritabanı ve kapsayıcı oluşturan bir Cassandra Python uygulamasını çalıştırmayı öğrendiniz. Artık Azure Cosmos DB hesabınıza ek veri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Cassandra verilerini Azure Cosmos DB’ye aktarma](cassandra-import-data.md)

