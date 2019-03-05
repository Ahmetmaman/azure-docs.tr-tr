---
title: SQL veritabanı için 1433 dışındaki bağlantı noktaları | Microsoft Docs
description: Azure SQL veritabanı için ADO.NET'ten istemci bağlantılarını, proxy sunucusunu atlamak ve doğrudan 1433 dışındaki bağlantı noktaları kullanarak veritabanıyla etkileşim.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: sstein
manager: craigg
ms.date: 11/07/2018
ms.openlocfilehash: 96b6b4866b17e15f544a10124d07e651d747b58b
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2019
ms.locfileid: "57306451"
---
# <a name="ports-beyond-1433-for-adonet-45"></a>ADO.NET 4.5 için 1433 dışındaki bağlantı noktaları

Bu konuda, ADO.NET 4.5 veya sonraki bir sürümünü kullanan istemciler için Azure SQL veritabanı bağlantı davranışları açıklanmaktadır.

> [!IMPORTANT]
> Bağlantı mimarisi hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı bağlantı mimarisi](sql-database-connectivity-architecture.md).
>

## <a name="outside-vs-inside"></a>Dış ve iç karşılaştırması

Azure SQL veritabanı bağlantıları için önce istemci programınızın çalışıp çalışmayacağını isteriz gerekir *dışında* veya *içinde* Azure bulut sınırları. Alt bölümlerde iki yaygın senaryo açıklanmaktadır.

### <a name="outside-client-runs-on-your-desktop-computer"></a>*Dışında:* İstemcisi, masaüstü bilgisayarınızda çalıştırır.

1433 numaralı bağlantı noktası, SQL veritabanı istemci uygulamanızı barındıran, masaüstü bilgisayarınızda açık olması gereken tek bağlantı noktasıdır.

### <a name="inside-client-runs-on-azure"></a>*İçinde:* İstemci, Azure üzerinde çalışır.

İstemciniz bir Azure bulut sınırları içinde çalıştığında, dediğimiz kullandığı bir *doğrudan rota* SQL veritabanı sunucusu ile etkileşim. Daha fazla bağlantı kurulduktan sonra veritabanı ve istemci arasındaki etkileşimler Azure SQL veritabanı ağ geçidi gerektirir.

Sırası aşağıdaki gibidir:

1. ADO.NET 4.5 (veya üzeri), kısa bir Azure bulut etkileşim başlatır ve dinamik olarak tanımlanan bağlantı noktası numarasını alır.

   * Dinamik olarak tanımlanan bağlantı noktası numarası 11000 11999 veya 14000 14999 aralığıdır.
2. ADO.NET ardından SQL veritabanı sunucusuna doğrudan hiçbir Ara yazılımla arasında bağlanır.
3. Sorgular, doğrudan veritabanına gönderilir ve sonuçları doğrudan istemciye döndürülür.

Bağlantı noktası aralıkları 11000 11999 ve 14000 14999 Azure İstemci makinenizde SQL veritabanı ile ADO.NET 4.5 istemci etkileşimleri kullanılabilir bırakılır emin olun.

* Özellikle, bağlantı noktaları aralığındaki herhangi diğer giden blockers boş olmalıdır.
* Azure vm'nizdeki **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı** bağlantı noktası ayarlarını denetler.
  
  * Kullanabileceğiniz [güvenlik duvarının kullanıcı arabirimi](https://msdn.microsoft.com/library/cc646023.aspx) belirtmek için bir kural eklemek üzere **TCP** protokol söz dizimi ile bir bağlantı noktası aralığı birlikte ister **11000 11999**.

## <a name="version-clarifications"></a>Sürüm açıklamalar

Bu bölümde, ürün sürümleri başvuran adlar açıklar. Ayrıca, bazı eşleştirmeleri ürünleri arasındaki sürümleri listelenir.

### <a name="adonet"></a>ADO.NET

* ADO.NET 4.0 TDS 7.3 protokolü, ancak değil 7.4 destekler.
* ADO.NET 4.5 ve sonrası TDS 7.4 protokolünü destekler.

### <a name="odbc"></a>ODBC

* Microsoft SQL Server ODBC 11 veya üstü

### <a name="jdbc"></a>JDBC

* Microsoft SQL Server JDBC 4.2 veya üzeri (aslında TDS 7.4 destekler ancak "yeniden yönlendirme" uygulamıyor JDBC 4.0)

## <a name="related-links"></a>İlgili bağlantılar

* ADO.NET 4.6 20 Temmuz 2015 tarihinde yayınlanmıştır. Bir .NET ekibi blog duyurusuna kullanılabilir [burada](https://blogs.msdn.com/b/dotnet/archive/20../../announcing-net-framework-4-6.aspx).
* ADO.NET 4.5 15 Ağustos 2012 tarihinde yayınlanmıştır. Bir .NET ekibi blog duyurusuna kullanılabilir [burada](https://blogs.msdn.com/b/dotnet/archive/20../../announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).
  * Bir ADO.NET 4.5.1 blog yazısı kullanılabilir [burada](https://blogs.msdn.com/b/dotnet/archive/20../../announcing-the-net-framework-4-5-1-preview.aspx).

* Microsoft® ODBC sürücüsü 17 SQL Server® - Windows, Linux ve macOS için https://www.microsoft.com/download/details.aspx?id=56567

* Azure SQL veritabanı V12 yeniden yönlendirmesi aracılığıyla bağlanma https://techcommunity.microsoft.com/t5/DataCAT/Connect-to-Azure-SQL-Database-V12-via-Redirection/ba-p/305362

* [TDS Protokolü sürüm listesi](http://www.freetds.org/userguide/tdshistory.htm)
* [SQL veritabanı geliştirmeye genel bakış](sql-database-develop-overview.md)
* [Azure SQL veritabanı güvenlik duvarı](sql-database-firewall-configure.md)
* [Nasıl yapılır: SQL Veritabanı’nda güvenlik duvarı ayarlarını yapılandırma](sql-database-configure-firewall-settings.md)


