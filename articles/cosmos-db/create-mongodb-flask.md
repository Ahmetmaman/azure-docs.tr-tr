---
title: MongoDB için Azure Cosmos DB API 'sini kullanarak bir Python Flask Web uygulaması oluşturma
description: ", MongoDB için Azure Cosmos DB API 'sini kullanarak bağlanmak ve sorgulamak için kullanabileceğiniz bir Python Flask kodu örneği sunar."
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: python
ms.topic: quickstart
ms.date: 12/26/2018
ms.custom: devx-track-python
ms.openlocfilehash: 58f22a335f4c619a6348e9e127e60f5a79f658b2
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95994579"
---
# <a name="quickstart-build-a-python-app-using-azure-cosmos-dbs-api-for-mongodb"></a>Hızlı başlangıç: Azure Cosmos DB MongoDB için API 'sini kullanarak bir Python uygulaması oluşturma
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

> [!div class="op_single_selector"]
> * [.NET](create-mongodb-dotnet.md)
> * [Java](create-mongodb-java.md)
> * [Node.js](create-mongodb-nodejs.md)
> * [Python](create-mongodb-flask.md)
> * [Xamarin](create-mongodb-xamarin.md)
> * [Golang](create-mongodb-go.md)
>  

Bu hızlı başlangıçta, GitHub 'dan kopyalanmış bir Python Flask To-Do Web uygulaması çalıştırmak için Mongo DB API hesabı veya Azure Cosmos DB öykünücüsü Azure Cosmos DB kullanırsınız. Azure Cosmos DB, genel dağıtım ve yatay ölçeklendirme özellikleri ile belge, tablo, anahtar değer ve grafik veritabanlarını hızlıca oluşturmanıza ve sorgulamanızı sağlayan çok modelli bir veritabanı hizmetidir.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio). Veya Azure aboneliği olmadan [ücretsiz Azure Cosmos DB deneyin](https://azure.microsoft.com/try/cosmosdb/) . Ya da [Azure Cosmos DB öykünücüsünü](local-emulator.md)kullanabilirsiniz. 
- [Python 3.6 +](https://www.python.org/downloads/)
- [Python uzantısıyla](https://marketplace.visualstudio.com/items?itemName=donjayamanne.python) [Visual Studio Code](https://code.visualstudio.com/Download) .

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub 'dan bir Flask-MongoDB uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle programlı bir şekilde çalışmanın ne kadar kolay olduğunu görüyorsunuz.

1. Bir komut istemini açın, git-samples adlı yeni bir klasör oluşturun ve komut istemini kapatın.

    ```bash
    md "C:\git-samples"
    ```

2. Git Bash gibi bir Git terminal penceresi açın ve örnek uygulamayı yüklemek üzere yeni bir klasör olarak değiştirmek için `cd` komutunu kullanın.

    ```bash
    cd "C:\git-samples"
    ```

3. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulamanın bir kopyasını oluşturur.

    ```bash
    git clone https://github.com/Azure-Samples/CosmosDB-Flask-Mongo-Sample.git
    ```
3. Python modüllerini yüklemek için aşağıdaki komutu çalıştırın.

    ```bash 
    pip install -r .\requirements.txt
    ```
4. Klasörü Visual Studio Code'da açın.

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Veritabanı kaynaklarının kodda nasıl oluşturulduğunu öğrenmekle ilgileniyorsanız, aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Aksi takdirde, [Web uygulamasını çalıştırma](#run-the-web-app) konusuna atlayabilirsiniz. 

Aşağıdaki kod parçacıklarının tamamı, *app.py* dosyasından alınır ve yerel Azure Cosmos DB öykünücüsü için bağlantı dizesini kullanır. Başka türlü ayrıştırılamayan ileri eğik çizgiler için aşağıda görüldüğü gibi parolanın bölünmesi gerekir.

* MongoDB istemcisini başlatın, veritabanını alın ve kimlik doğrulaması yapın.

    ```python
    client = MongoClient("mongodb://127.0.0.1:10250/?ssl=true") #host uri
    db = client.test    #Select the database
    db.authenticate(name="localhost",password='C2y6yDjf5' + r'/R' + '+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw' + r'/Jw==')
    ```

* Koleksiyonu alın veya henüz yoksa bir tane oluşturun.

    ```python
    todos = db.todo #Select the collection
    ```

* Uygulama oluşturma

    ```Python
    app = Flask(__name__)
    title = "TODO with Flask"
    heading = "ToDo Reminder"
    ```
    
## <a name="run-the-web-app"></a>Web uygulamasını çalıştırma

1. Azure Cosmos DB Emulator’ın çalışır durumda olduğundan emin olun.

2. Bir terminal penceresi açın ve uygulamanın kayıtlı olduğu dizine `cd` uygulayın.

3. Ardından `set FLASK_APP=app.py` , `$env:FLASK_APP = app.py` PowerShell düzenleyicilerinde veya `export FLASK_APP=app.py` bir Mac kullanıyorsanız Flask uygulaması için ortam değişkenini ayarlayın. 

4. Uygulamayı ile çalıştırın `flask run` ve *http: \/ /127.0.0.1:5000/* konumuna gidin.

5. Görevleri ekleyip kaldırın ve koleksiyona eklenip kaldırıldığına dikkat edin.

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

Kodu canlı bir Azure Cosmos DB hesabına göre test etmek istiyorsanız, bir hesap oluşturmak için Azure portal gidin.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Canlı Azure Cosmos DB hesabıyla kodu test etmek için bağlantı dizesi bilgilerinizi alın. Ardından uygulamaya kopyalayın.

1. Azure portal Azure Cosmos DB hesabınızda, sol gezinti **bağlantı dizesi**' ni seçin ve ardından **okuma-yazma anahtarları**' nı seçin. Kullanıcı adını, bağlantı dizesini ve parolayı kopyalamak için ekranın sağ tarafındaki kopyalama düğmelerini kullanacaksınız. 

2. Kök dizinde *app.py* dosyasını açın.

3. **username** değerini portaldan kopyalayın (kopyalama düğmesini kullanarak) ve *app.py* dosyasına **name** değeri olarak yapıştırın.

4. Sonra **bağlantı dizesi** değerini portaldan kopyalayın ve *app.py* dosyanızdaki **mongoclient** değeri yapın.

5. Son olarak, **password** değerini portaldan kopyalayın ve *app.py* dosyanıza **password** değeri olarak yapıştırın.

Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. Öncekiyle aynı şekilde çalıştırabilirsiniz.

## <a name="deploy-to-azure"></a>Azure’a dağıtın

Bu uygulamayı dağıtmak için Azure 'da yeni bir Web uygulaması oluşturabilir ve bu GitHub deposunun çatalından sürekli dağıtımı etkinleştirebilirsiniz. Azure 'da GitHub ile sürekli dağıtımı ayarlamak için bu [öğreticiyi](../app-service/deploy-continuous-deployment.md) izleyin.

Azure'a dağıtırken uygulama anahtarlarınızı kaldırmanız ve aşağıdaki bölümün açıklama satırı yapılmadığından emin olmanız gerekir:

```python
    client = MongoClient(os.getenv("MONGOURL"))
    db = client.test    #Select the database
    db.authenticate(name=os.getenv("MONGO_USERNAME"),password=os.getenv("MONGO_PASSWORD"))
```

Daha sonra MONGOURL, MONGO_PASSWORD ve MONGO_USERNAME değerlerinizi uygulama ayarlarına eklemeniz gerekir. Azure Web Apps’te Uygulama Ayarları hakkında daha fazla bilgi almak için bu [öğreticiyi](../app-service/configure-common.md#configure-app-settings) izleyebilirsiniz.

Bu deponun bir çatalını oluşturmak istemiyorsanız, aşağıdaki **Azure 'A dağıt** düğmesini de seçebilirsiniz. Daha sonra Azure 'a gitmeniz ve Azure Cosmos DB hesap bilgilerinizden uygulama ayarlarını ayarlamanız gerekir.

<a href="https://deploy.azure.com/?repository=https://github.com/heatherbshapiro/To-Do-List---Flask-MongoDB-Example" target="_blank">
<img src="https://azuredeploy.net/deploybutton.png" alt="Click to Deploy to Azure">
</a>

> [!NOTE]
> Kodunuzun GitHub veya diğer kaynak denetimi seçeneklerinde depolanmasını planlıyorsanız lütfen bağlantı dizelerinizi koddan kaldırmayı unutmayın. Bağlantı dizeleriniz, bunun yerine web uygulamasının uygulama ayarlarıyla ayarlanabilir.

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Mongo DB API hesabı için bir Azure Cosmos DB oluşturma ve GitHub 'dan kopyalanmış bir Python Flask To-Do Web uygulaması çalıştırmak için Azure Cosmos DB öykünücüsünü kullanma hakkında öğrendiniz. Şimdi Azure Cosmos DB hesabınıza ek veriler aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB’ye MongoDB verileri aktarma](../dms/tutorial-mongodb-cosmos-db.md?toc=%252fazure%252fcosmos-db%252ftoc.json%253ftoc%253d%252fazure%252fcosmos-db%252ftoc.json)