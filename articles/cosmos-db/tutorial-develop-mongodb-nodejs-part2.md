---
title: MongoDB için Azure Cosmos DB API 'siyle Node.js Express uygulaması oluşturma (part2)
description: Azure Cosmos DB üzerinde Angular ve Node ile MongoDB için kullandığınız aynı API'leri kullanarak bir MongoDB uygulaması oluşturma öğreticisi dizisinin 2. bölümü.
author: johnpapa
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 12/26/2018
ms.author: jopapa
ms.custom: seodec18
ms.reviewer: sngun
ms.openlocfilehash: a796ae32033dbc09b1f5b888ded9ea5e1f4c457e
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86523494"
---
# <a name="create-an-angular-app-with-azure-cosmos-dbs-api-for-mongodb---create-a-nodejs-express-app"></a>MongoDB için Azure Cosmos DB API 'SI ile angular uygulaması oluşturma-Node.js Express uygulaması oluşturma

Bu çok parçalı öğreticide, Express ve angular ile Node.js yazılmış yeni bir uygulama oluşturma ve ardından bunu [MongoDB için Cosmos DB API 'siyle yapılandırılmış Cosmos hesabınıza](mongodb-introduction.md)bağlama işlemlerinin nasıl yapılacağı gösterilmektedir.

Öğreticinin 2. bölümünde [giriş bölümündeki](tutorial-develop-mongodb-nodejs.md) konular genişletilir ve aşağıdaki görevler yer alır:

> [!div class="checklist"]
> * Angular CLI’yi ve TypeScript’i yükleme
> * Angular kullanarak yeni bir proje oluşturma
> * Express framework kullanarak uygulama oluşturma
> * Postman içinde uygulamayı test etme

## <a name="video-walkthrough"></a>Görüntülü kılavuz

> [!VIDEO https://www.youtube.com/embed/lIwJIYcGSUg]

## <a name="prerequisites"></a>Önkoşullar

Öğreticinin bu bölümüne başlatmadan önce [giriş videosunu](tutorial-develop-mongodb-nodejs.md) izlediğinizden emin olun.

Bu öğretici için aşağıdakiler de gereklidir: 
* [Node.js](https://nodejs.org/) sürüm 8.4.0 veya üzeri.
* [Postman](https://www.getpostman.com/)
* [Visual Studio Code](https://code.visualstudio.com/) veya sık kullandığınız bir kod düzenleyici.

> [!TIP]
> Bu öğretici, uygulamanızı adım adım oluşturmanızı sağlayacak adımlarla size yol gösterir. Tamamlanmış projeyi indirmek isterseniz, tamamlanmış uygulamayı github'daki [angular-cosmosdb deposundan](https://github.com/Azure-Samples/angular-cosmosdb) alabilirsiniz.

## <a name="install-the-angular-cli-and-typescript"></a>Angular CLI’yi ve TypeScript’i yükleme

1. Bir Windows Komut İstemi veya Mac Terminal penceresi açın ve Angular CLI’yi yükleyin.

    ```bash
    npm install -g @angular/cli
    ```

2. İstemde aşağıdaki komutu girerek TypeScript’i yükleyin. 

    ```bash
    npm install -g typescript
    ```

## <a name="use-the-angular-cli-to-create-a-new-project"></a>Yeni bir proje oluşturmak için Angular CLI’yi kullanma

1. Komut isteminde, yeni projenizi oluşturmak istediğiniz klasöre gidin, ardından aşağıdaki komutu çalıştırın. Bu komut, yeni bir klasör ve angular-cosmosdb adlı bir proje oluşturur ve yeni bir uygulama için gerekli Angular bileşenlerini yükler. Minimal kurulumu kullanır (--minimal) ve projenin Sass kullandığını belirtir (--style scss bayrağıyla CSS benzeri bir sözdizimi).

    ```bash
    ng new angular-cosmosdb --minimal --style scss
    ```

2. Komut tamamlandığında, dizinleri src/istemci klasörüyle değiştirin.

    ```bash
    cd angular-cosmosdb
    ```

3. Ardından Visual Studio Code’da klasörü açın.

    ```bash
    code .
    ```

## <a name="build-the-app-using-the-express-framework"></a>Express framework kullanarak uygulamayı oluşturma

1. Visual Studio Code’da **Gezgin** bölmesinde, **src** klasörüne sağ tıklayın, **Yeni Klasör**’e tıklayın ve yeni klasöre *sunucu* adını verin.

2. **Gezgin** bölmesinde, **sunucu** klasörüne sağ tıklayın, **Yeni Dosya**’ya tıklayın ve yeni dosyaya *index.js* adını verin.

3. Komut istemine dönün, gövde ayrıştırıcısını yüklemek için aşağıdaki komutu kullanın. Bu, uygulamamızın API'ler aracılığıyla iletilen JSON verilerini ayrıştırmasına yardımcı olur.

    ```bash
    npm i express body-parser --save
    ```

4. Visual Studio Code’da index.js dosyasına aşağıdaki kodu kopyalayın. Bu kod:
    * Express’e başvurur
    * İsteklerin gövdesine JSON verilerini okumak için gövde ayrıştırıcısını getirir
    * Path adında yerleşik bir özellik kullanır
    * Kodlarımızın bulunduğu yeri bulmayı kolaylaştırmak için kök değişkenleri belirler
    * Bağlantı noktasını ayarlar
    * Express'i başlatır
    * Uygulamaya, sunucuyu sunmak için kullanılacak ara yazılımları kullanmayı anlatır
    * dist klasöründeki her şeyi sunar, bu klasördekiler statik içerik olacaktır
    * Uygulamayı oluşturur ve sunucuda bulunamayan tüm GET istekleri için index.html dosyasını sunar (ayrıntılı bağlantılar için)
    * app.listen ile sunucuyu başlatır
    * Bağlantı noktasının etkin olduğunu günlüğe kaydetmek için bir ok işlevi kullanır
    
   ```node
   const express = require('express');
   const bodyParser = require('body-parser');
   const path = require('path');
   const routes = require('./routes');

   const root = './';
   const port = process.env.PORT || '3000';
   const app = express();

   app.use(bodyParser.json());
   app.use(bodyParser.urlencoded({ extended: false }));
   app.use(express.static(path.join(root, 'dist/angular-cosmosdb')));
   app.use('/api', routes);
   app.get('*', (req, res) => {
     res.sendFile('dist/angular-cosmosdb/index.html', {root});
   });

   app.listen(port, () => console.log(`API running on localhost:${port}`));
   ```

5. Visual Studio Code’da **Gezgin** bölmesinde, **server** klasörüne sağ tıklayın, ardından **Yeni dosya**’ya tıklayın. Yeni dosyaya *routes.js* adını verin. 

6. **routes.js**’ye aşağıdaki kodları kopyalayın: Bu kod:
   * Express yönlendiricisine başvurur
   * Hero’ları alır
   * Tanımlanmış bir kahraman için JSON’u geri gönderir

   ```node
   const express = require('express');
   const router = express.Router();

   router.get('/heroes', (req, res) => {
    res.send(200, [
       {"id": 10, "name": "Starlord", "saying": "oh yeah"}
    ])
   });

   module.exports=router;
   ```

7. Tüm değiştirilmiş dosyalarınızı kaydeder. 

8. Visual Studio Code, **Hata Ayıkla** düğmesine tıklayın :::image type="icon" source="./media/tutorial-develop-mongodb-nodejs-part2/debug-button.png"::: , dişli düğmesine tıklayın :::image type="icon" source="./media/tutorial-develop-mongodb-nodejs-part2/gear-button.png"::: . Yeni Launch.json dosyası Visual Studio Code’da açılır. 

8. launch.json dosyasının 11. satırında `"${workspaceFolder}\\server"` değerini `"program": "${workspaceRoot}/src/server/index.js"` ile değiştirin dosyayı kaydedin.

9. Uygulamayı çalıştırmak için **hata ayıklamayı Başlat** düğmesine tıklayın :::image type="icon" source="./media/tutorial-develop-mongodb-nodejs-part2/start-debugging-button.png"::: .

    Uygulama hatasız çalışmalıdır.

## <a name="use-postman-to-test-the-app"></a>Uygulamayı Postman kullanarak test etme

1. Şimdi Postman’ı açın ve GET kutusuna `http://localhost:3000/api/heroes` adresini girin. 

2. **Gönder** düğmesine tıklayın ve uygulamadan json yanıtını alın. 

    Bu yanıt uygulamanın hazır ve yerel olarak çalışır durumda olduğunu gösterir. 

    :::image type="content" source="./media/tutorial-develop-mongodb-nodejs-part2/azure-cosmos-db-postman.png" alt-text="İsteği ve yanıtı gösteren postman":::


## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde aşağıdakileri yaptınız:

> [!div class="checklist"]
> * Angular CLI kullanarak bir Node.js projesi oluşturdunuz
> * Postman kullanarak uygulamayı test ettiniz

Kullanıcı arabirimini oluşturmak için öğreticinin sonraki bölümüne geçebilirsiniz.

> [!div class="nextstepaction"]
> [Angular ile kullanıcı arabirimini oluşturma](tutorial-develop-mongodb-nodejs-part3.md)
