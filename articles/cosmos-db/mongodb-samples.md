---
title: Node.js uygulaması derlemek için MongoDB için Azure Cosmos DB API 'sini kullanın
description: MongoDB için Azure Cosmos DB API 'sini kullanarak çevrimiçi bir veritabanı oluşturan bir öğretici.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: sample
ms.date: 12/26/2018
author: sivethe
ms.author: sivethe
ms.custom: devx-track-js
ms.openlocfilehash: c2117c535cca679d3a7a3b75491bf60ab9567ceb
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "93098221"
---
# <a name="build-an-app-using-nodejs-and-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Node.js ve Azure Cosmos DB API 'sini kullanarak uygulama oluşturma 
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](./sql-api-get-started.md)
> * [Java](./create-sql-api-java.md)
> * [MongoDB için Node.js](mongodb-samples.md)
> * [Node.js](sql-api-nodejs-get-started.md)
>

Bu örnek, MongoDB için Node.js ve Azure Cosmos DB API 'sini kullanarak bir konsol uygulaması oluşturmayı gösterir.

Bu örneği kullanmak için yapmanız gerekenler:

* MongoDB için Azure Cosmos DB API kullanmak üzere yapılandırılmış bir Cosmos hesabı [oluşturun](create-mongodb-dotnet.md#create-account) .
* [Bağlantı dizesi](connect-mongodb-account.md) bilgilerinizi alın.

## <a name="create-the-app"></a>Uygulama oluşturma

1. Bir *app.js* dosyası oluşturun ve aşağıdaki kodu kopyalayıp yapıştırın.

    ```javascript
    var MongoClient = require('mongodb').MongoClient;
    var assert = require('assert');
    var ObjectId = require('mongodb').ObjectID;
    var url = 'mongodb://<username>:<password>@<endpoint>.documents.azure.com:10255/?ssl=true';

    var insertDocument = function(db, callback) {
    db.collection('families').insertOne( {
            "id": "AndersenFamily",
            "lastName": "Andersen",
            "parents": [
                { "firstName": "Thomas" },
                { "firstName": "Mary Kay" }
            ],
            "children": [
                { "firstName": "John", "gender": "male", "grade": 7 }
            ],
            "pets": [
                { "givenName": "Fluffy" }
            ],
            "address": { "country": "USA", "state": "WA", "city": "Seattle" }
        }, function(err, result) {
        assert.equal(err, null);
        console.log("Inserted a document into the families collection.");
        callback();
    });
    };
    
    var findFamilies = function(db, callback) {
    var cursor =db.collection('families').find( );
    cursor.each(function(err, doc) {
        assert.equal(err, null);
        if (doc != null) {
            console.dir(doc);
        } else {
            callback();
        }
    });
    };
    
    var updateFamilies = function(db, callback) {
    db.collection('families').updateOne(
        { "lastName" : "Andersen" },
        {
            $set: { "pets": [
                { "givenName": "Fluffy" },
                { "givenName": "Rocky"}
            ] },
            $currentDate: { "lastModified": true }
        }, function(err, results) {
        console.log(results);
        callback();
    });
    };
    
    var removeFamilies = function(db, callback) {
    db.collection('families').deleteMany(
        { "lastName": "Andersen" },
        function(err, results) {
            console.log(results);
            callback();
        }
    );
    };
    
    MongoClient.connect(url, function(err, client) {
    assert.equal(null, err);
    var db = client.db('familiesdb');
    insertDocument(db, function() {
        findFamilies(db, function() {
        updateFamilies(db, function() {
            removeFamilies(db, function() {
                client.close();
            });
        });
        });
    });
    });
    ```
    
    **İsteğe bağlı**: **MongoDB Node.js 2.2 sürücüsünü** kullanıyorsanız, aşağıdaki kod parçacığını değiştirin:

    Özgün:

    ```javascript
    MongoClient.connect(url, function(err, client) {
    assert.equal(null, err);
    var db = client.db('familiesdb');
    insertDocument(db, function() {
        findFamilies(db, function() {
        updateFamilies(db, function() {
            removeFamilies(db, function() {
                client.close();
            });
        });
        });
    });
    });
    ```
    
    Şununla değiştirilmelidir:

    ```javascript
    MongoClient.connect(url, function(err, db) {
    assert.equal(null, err);
    insertDocument(db, function() {
        findFamilies(db, function() {
        updateFamilies(db, function() {
            removeFamilies(db, function() {
                db.close();
            });
        });
        });
    });
    });
    ```
    
2. *app.js* dosyasında aşağıdaki değişkenleri hesap ayarlarınıza göre değiştirin ([Bağlantı dizenizi](connect-mongodb-account.md) nasıl bulacağınızı öğrenin):

    > [!IMPORTANT]
    > **MongoDB Node.js 3.0 sürücüsü** için Cosmos DB parolasındaki özel karakterlerin kodlanması gerekir. '=' karakterlerini %3D olarak kodladığınızdan emin olun
    >
    > Örnek: *jm1HbNdLg5zxEuyD86ajvINRFrFCUX0bIWP15ATK3BvSv==* parolası *jm1HbNdLg5zxEuyD86ajvINRFrFCUX0bIWP15ATK3BvSv%3D%3D* olarak kodlanır
    >
    > **MongoDB Node.js 2.2 sürücüsü** için Cosmos DB parolasındaki özel karakterlerin kodlanması gerekmez.
    >
    >
   
    ```javascript
    var url = 'mongodb://<endpoint>:<password>@<endpoint>.documents.azure.com:10255/?ssl=true';
    ```
     
3. Tercih ettiğiniz terminali açın, **npm install mongodb --save** komutunu çalıştırın ve sonra da uygulamanızı **node app.js** ile çalıştırın

## <a name="next-steps"></a>Sonraki adımlar

- [Studio 3T](mongodb-mongochef.md) 'ı Azure Cosmos DB MongoDB IÇIN API 'si ile nasıl kullanacağınızı öğrenin.
- MongoDB için Azure Cosmos DB API 'SI ile [Robo 3T kullanmayı](mongodb-robomongo.md) öğrenin.
- MongoDB için Azure Cosmos DB API 'siyle MongoDB [örneklerini](mongodb-samples.md) gezin.