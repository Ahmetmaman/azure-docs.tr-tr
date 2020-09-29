---
title: 1433 dışındaki bağlantı noktaları
description: ADO.NET 'den Azure SQL veritabanı 'na istemci bağlantıları, proxy 'yi atlayabilir ve 1433 dışındaki bağlantı noktalarını kullanarak doğrudan veritabanıyla etkileşime geçebilir.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: sqldbrb=1, devx-track-dotnet
ms.devlang: ''
ms.topic: reference
author: stevestein
ms.author: sstein
ms.reviewer: genemi
ms.date: 06/11/2020
ms.openlocfilehash: 0d009522ea0d0986233983f8725549b618ffb537
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91444869"
---
# <a name="ports-beyond-1433-for-adonet-45"></a>ADO.NET 4.5 için 1433’ten sonraki bağlantı noktaları
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Bu konuda, ADO.NET 4,5 veya sonraki bir sürümünü kullanan istemciler için Azure SQL veritabanı bağlantı davranışı açıklanmaktadır.

> [!IMPORTANT]
> Bağlantı mimarisi hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı bağlantı mimarisi](connectivity-architecture.md).
>

## <a name="outside-vs-inside"></a>Dış ve iç

Azure SQL veritabanı bağlantıları için, önce istemci programınızın Azure bulut sınırının *dışında* mı yoksa *içinde* mi çalıştığını sormalıdır. Alt bölümlerde iki yaygın senaryo ele alınmaktadır.

### <a name="outside-client-runs-on-your-desktop-computer"></a>*Dış:* İstemci masaüstü bilgisayarınızda çalışır

Bağlantı noktası 1433, SQL veritabanı istemci uygulamanızı barındıran masaüstü bilgisayarınızda açık olması gereken tek bağlantı noktasıdır.

### <a name="inside-client-runs-on-azure"></a>*İçinde:* İstemci Azure 'da çalışır

İstemciniz Azure bulut sınırının içinde çalıştığında, SQL veritabanı ile etkileşime geçmek için *doğrudan bir yol* arayabileceğiz ' u kullanır. Bir bağlantı kurulduktan sonra, istemci ve veritabanı arasındaki diğer etkileşimler Azure SQL veritabanı ağ geçidi gerektirmez.

Sıra aşağıdaki gibidir:

1. ADO.NET 4,5 (veya üzeri), Azure bulutuyla kısa bir etkileşim başlatır ve dinamik olarak tanımlanan bir bağlantı noktası numarası alır.

   * Dinamik olarak tanımlanan bağlantı noktası numarası 11000-11999 aralığındadır.
2. ADO.NET ardından, arasında bir ara yazılım olmadan doğrudan SQL veritabanına bağlanır.
3. Sorgular doğrudan veritabanına gönderilir ve sonuçlar doğrudan istemciye döndürülür.

Azure istemci makinenizde 11000-11999 numaralı bağlantı noktası aralıklarının SQL veritabanı ile ADO.NET 4,5 istemci etkileşimleri için kullanılabilir olduğundan emin olun.

* Özellikle, aralıktaki bağlantı noktaları diğer giden engelleyicilerin dışında olmalıdır.
* Azure VM 'niz üzerinde, **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı** bağlantı noktası ayarlarını denetler.
  
  * [Güvenlik duvarının Kullanıcı arabirimini](/sql/sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access) , **11000-11999**gibi bir bağlantı noktası aralığıyla birlikte **TCP** protokolünü belirlediğiniz bir kural eklemek için kullanabilirsiniz.

## <a name="version-clarifications"></a>Sürüm hakkında açıklamalar

Bu bölümde ürün sürümlerine başvuran bilinen adlar açıklığa kavuşturulur. Ayrıca, ürünler arasında bazı sürüm eşleştirmeleri de listelenir.

### <a name="adonet"></a>ADO.NET

* ADO.NET 4,0, TDS 7,3 protokolünü destekler, ancak 7,4.
* ADO.NET 4,5 ve üzeri, TDS 7,4 protokolünü destekler.

### <a name="odbc"></a>ODBC

* Microsoft SQL Server ODBC 11 veya üzeri

### <a name="jdbc"></a>JDBC

* Microsoft SQL Server JDBC 4,2 veya üzeri (JDBC 4,0 aslında TDS 7,4 destekliyor ancak "yeniden yönlendirme" uygulamaz)

## <a name="related-links"></a>İlgili bağlantılar

* ADO.NET 4,6, 20 Temmuz 2015 tarihinde yayınlandı. [Burada](https://devblogs.microsoft.com/dotnet/announcing-net-framework-4-6/).net ekibinin bir blog duyurusu bulunur.
* ADO.NET 4,5, 15 Ağustos 2012 tarihinde yayınlandı. [Burada](https://devblogs.microsoft.com/dotnet/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code/).net ekibinin bir blog duyurusu bulunur.
  * ADO.NET 4.5.1 hakkında bir blog gönderisi [burada](https://devblogs.microsoft.com/dotnet/announcing-the-net-framework-4-5-1-preview/)bulunabilir.

* SQL Server için Microsoft ODBC sürücüsü 17 https://aka.ms/downloadmsodbcsql

* Yeniden yönlendirme yoluyla Azure SQL Veritabanı V12 'e bağlanma https://techcommunity.microsoft.com/t5/DataCAT/Connect-to-Azure-SQL-Database-V12-via-Redirection/ba-p/305362

* [TDS protokol sürümü listesi](https://www.freetds.org/)
* [SQL veritabanı geliştirmeye genel bakış](develop-overview.md)
* [Azure SQL veritabanı güvenlik duvarı](firewall-configure.md)
