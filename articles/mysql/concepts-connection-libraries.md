---
title: Bağlantı kitaplıkları-MySQL için Azure veritabanı
description: Bu makalede, istemci programlarının MySQL için Azure veritabanı 'na bağlanırken kullanabileceği her bir kitaplık veya sürücü listelenir.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: conceptual
ms.date: 8/3/2020
ms.openlocfilehash: ac6e5ff2ce775b8ca273ce31a9a35a0e8e37bc07
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "94542635"
---
# <a name="connection-libraries-for-azure-database-for-mysql"></a>MySQL için Azure veritabanı bağlantı kitaplıkları
Bu makalede, istemci programlarının MySQL için Azure veritabanı 'na bağlanırken kullanabileceği her bir kitaplık veya sürücü listelenir.

## <a name="client-interfaces"></a>İstemci arabirimleri
MySQL, sektör standartları ODBC ve JDBC ile uyumlu uygulamalar ve araçlarla MySQL kullanmak için standart veritabanı sürücüsü bağlantısı sunar. ODBC veya JDBC ile birlikte çalışarak herhangi bir sistem MySQL kullanabilir.

| **Dil** | **Platform** | **Ek kaynak** | **İndir** |
| :----------- | :------------| :-----------------------| :------------|
| PHP | Windows, Linux | [PHP için MySQL Native Driver-mysqlnd](https://dev.mysql.com/downloads/connector/php-mysqlnd/) | [İndir](https://secure.php.net/downloads.php) |
| ODBC | Windows, Linux, Mac OS X ve UNIX platformları | [MySQL Bağlayıcısı/ODBC Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-odbc/en/) | [İndir](https://dev.mysql.com/downloads/connector/odbc/) |
| ADO.NET | Windows | [MySQL Bağlayıcısı/NET Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-net/en/) | [İndir](https://dev.mysql.com/downloads/connector/net/) |
| JDBC | Platformdan bağımsız | [MySQL Bağlayıcısı/J 5,1 Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-j/5.1/en/) | [İndir](https://dev.mysql.com/downloads/connector/j/) |
| Node.js | Windows, Linux, Mac OS X | [sıdorares/node-mysql2](https://github.com/sidorares/node-mysql2/tree/master/documentation) | [İndir](https://github.com/sidorares/node-mysql2) |
| Python | Windows, Linux, Mac OS X | [MySQL Bağlayıcısı/Python Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-python/en/) | [İndir](https://dev.mysql.com/downloads/connector/python/) |
| C++ | Windows, Linux, Mac OS X | [MySQL Bağlayıcısı/C++ Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-cpp/en/) | [İndir](https://dev.mysql.com/downloads/connector/python/) |
| C | Windows, Linux, Mac OS X | [MySQL Bağlayıcısı/C Geliştirici Kılavuzu](https://dev.mysql.com/doc/c-api/8.0/en/) | [İndir](https://dev.mysql.com/downloads/connector/c/)
| Perl | Windows, Linux, Mac OS X ve UNIX platformları | [DBD:: MySQL](https://metacpan.org/pod/DBD::mysql) | [İndir](https://metacpan.org/pod/DBD::mysql) |


## <a name="next-steps"></a>Sonraki adımlar
Tercih ettiğiniz dili kullanarak MySQL için Azure veritabanı 'na bağlanma ve sorgu oluşturma konusunda bu hızlı başlangıçlardan birini okuyun:

- [PHP](./connect-php.md)
- [Java](./connect-java.md)
- [.NET (C#)](./connect-csharp.md)
- [Python](./connect-python.md)
- [Node.JS](./connect-nodejs.md)
- [Ruby](./connect-ruby.md)
- [C++](connect-cpp.md)
- [Git](./connect-go.md)
