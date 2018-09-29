---
title: Cache ASP.NET çıktı önbelleği sağlayıcısı
description: ASP.NET sayfasını kullanarak Azure Redis Cache çıktı önbellek hakkında bilgi edinin
services: redis-cache
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: tysonn
ms.assetid: 78469a66-0829-484f-8660-b2598ec60fbf
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 02/14/2017
ms.author: wesmc
ms.openlocfilehash: a6c3314a981b46aa6f1cbca1f34392d1e1ae6c9a
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47431653"
---
# <a name="aspnet-output-cache-provider-for-azure-redis-cache"></a>ASP.NET çıktı önbelleği sağlayıcısı için Azure Redis önbelleği
Redis çıktı önbelleği sağlayıcısı, çıkış verileri önbelleğe almak için bir işlem dışı depolama mekanizmasıdır. Özellikle tam HTTP yanıtları için bu verilerdir (çıkış önbelleğe alma sayfası). ASP.NET 4'te kullanılmaya başlanan yeni çıktı önbelleği sağlayıcısı genişletilebilirlik noktasına sağlayıcı yararlanmanıza imkan sağlar.

Redis çıktı önbelleği sağlayıcısı kullanmak için önbelleğinizi önce yapılandırın ve ardından Redis çıktı önbelleği sağlayıcısı NuGet paketini kullanarak ASP.NET uygulamanızı yapılandırın. Bu konu, uygulamanızı Redis çıktı önbelleği sağlayıcısı kullanacak şekilde yapılandırma hakkında rehberlik sağlar. Oluşturma ve bir Azure Redis Cache örneği yapılandırma hakkında daha fazla bilgi için bkz. [bir önbellek oluşturma](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

## <a name="store-aspnet-page-output-in-the-cache"></a>Önbellekte ASP.NET sayfa çıktısını Store
Redis Cache oturum durumu NuGet paketini kullanarak Visual Studio'da bir istemci uygulamasını yapılandırmak için tıklayın **NuGet Paket Yöneticisi**, **Paket Yöneticisi Konsolu** gelen **Araçları**menüsü.

`Package Manager Console` penceresinden aşağıdaki komutu çalıştırın.
    
```
Install-Package Microsoft.Web.RedisOutputCacheProvider
```

Redis çıktı önbelleği sağlayıcısı NuGet paketini StackExchange.Redis.StrongName paketinde bağımlılık vardır. StackExchange.Redis.StrongName paket, projenizde mevcut değilse yüklenir. Redis çıktı önbelleği sağlayıcısı NuGet paketi hakkında daha fazla bilgi için bkz. [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) NuGet sayfası.

>[!NOTE]
>Tanımlayıcı adlı StackExchange.Redis.StrongName paket yanı sıra de olmayan-tanımlayıcı adlı StackExchange.Redis sürümü mevcuttur. Projenizi kaldırmalısınız olmayan-tanımlayıcı adlı StackExchange.Redis sürümü kullanıyorsanız, aksi takdirde, projenizde adlandırma çakışmaları alın. Bu paketler hakkında daha fazla bilgi için bkz. [önbellek istemcilerini yapılandırma .NET](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).
>
>

NuGet paketi indirir ve gerekli derleme başvurularını ekler ve aşağıdaki bölümde, web.config dosyasına ekler. Bu bölümde Redis çıktı önbelleği sağlayıcısı kullanmak ASP.NET uygulamanız için gerekli yapılandırmayı içerir.

```xml
<caching>
  <outputCache defaultProvider="MyRedisOutputCache">
    <providers>
      <!-- For more details check https://github.com/Azure/aspnet-redis-providers/wiki -->
      <!-- Either use 'connectionString' OR 'settingsClassName' and 'settingsMethodName' OR use 'host','port','accessKey','ssl','connectionTimeoutInMilliseconds' and 'operationTimeoutInMilliseconds'. -->
      <!-- 'databaseId' and 'applicationName' can be used with both options. -->
      <!--
      <add name="MyRedisOutputCache" 
        host = "127.0.0.1" [String]
        port = "" [number]
        accessKey = "" [String]
        ssl = "false" [true|false]
        databaseId = "0" [number]
        applicationName = "" [String]
        connectionTimeoutInMilliseconds = "5000" [number]
        operationTimeoutInMilliseconds = "1000" [number]
        connectionString = "<Valid StackExchange.Redis connection string>" [String]
        settingsClassName = "<Assembly qualified class name that contains settings method specified below. Which basically return 'connectionString' value>" [String]
        settingsMethodName = "<Settings method should be defined in settingsClass. It should be public, static, does not take any parameters and should have a return type of 'String', which is basically 'connectionString' value.>" [String]
        loggingClassName = "<Assembly qualified class name that contains logging method specified below>" [String]
        loggingMethodName = "<Logging method should be defined in loggingClass. It should be public, static, does not take any parameters and should have a return type of System.IO.TextWriter.>" [String]
        redisSerializerType = "<Assembly qualified class name that implements Microsoft.Web.Redis.ISerializer>" [String]
      />
      -->
      <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider"
           host=""
           accessKey=""
           ssl="true" />
    </providers>
  </outputCache>
</caching>
```

Açıklamalı bölüm öznitelikleri ve her bir öznitelik için örnek ayarlarını örneği sağlar.

Microsoft Azure Portal'da önbellek dikey pencerenize değerlerle öznitelikleri yapılandırma ve diğer değerleri istediğiniz gibi yapılandırın. Önbellek özelliklerinizi erişme ile ilgili yönergeler için bkz: [Redis önbelleği ayarlarını yapılandırma](cache-configure.md#configure-redis-cache-settings).

* **konak** –, önbellek uç noktasını belirtin.
* **bağlantı noktası** – SSL olmayan bağlantı noktası ya da ssl ayarlarına bağlı olarak, SSL bağlantı noktasını kullanın.
* **accessKey** – önbelleğiniz için birincil veya ikincil anahtarı kullanın.
* **SSL** – önbellek/istemci iletişimleri ssl ile güvenli hale getirmek istiyorsanız true; Aksi takdirde false. Doğru bağlantı noktasını belirttiğinizden emin olun.
  * SSL olmayan bağlantı noktasın yeni önbellekler için varsayılan olarak devre dışı bırakılmıştır. SSL bağlantı noktası kullanmak üzere bu ayarı TRUE belirtin. SSL olmayan bağlantı noktası etkinleştirme hakkında daha fazla bilgi için bkz: [erişim bağlantı noktaları](cache-configure.md#access-ports) konusundaki [bir önbellek yapılandırma](cache-configure.md) konu.
* **Databaseıd** – belirtilen hangi veritabanı önbelleğe yönelik kullanmak için veri çıktı. Belirtilmezse, varsayılan değer olan 0 kullanılır.
* **applicationName** – anahtarları redis depolanır `<AppName>_<SessionId>_Data`. Bu adlandırma şeması aynı anahtarı paylaşan birden çok uygulamaların sağlar. Bu parametre isteğe bağlıdır ve onu belirtmezseniz varsayılan değer kullanılır.
* **connectionTimeoutInMilliseconds** – Bu ayar, StackExchange.Redis istemcisi connectTimeout ayarında geçersiz kılmanıza olanak sağlar. Belirtilmezse, varsayılan connectTimeout ayar 5000 kullanılır. Daha fazla bilgi için [StackExchange.Redis yapılandırma modeli](http://go.microsoft.com/fwlink/?LinkId=398705).
* **operationTimeoutInMilliseconds** – Bu ayar, StackExchange.Redis istemcisi syncTimeout ayarında geçersiz kılmanıza olanak sağlar. Belirtilmezse varsayılan syncTimeout olarak 1000 kullanılır. Daha fazla bilgi için [StackExchange.Redis yapılandırma modeli](http://go.microsoft.com/fwlink/?LinkId=398705).

OutputCache yönergesi çıkışı önbelleğe almak istediğiniz her sayfaya ekleyin.

```
<%@ OutputCache Duration="60" VaryByParam="*" %>
```

Önceki örnekte, önbelleğe alınan sayfa verileri kalır önbellekte 60 saniye ve sayfayı farklı bir sürümünü her parametre bileşimi için önbelleğe alınır. OutputCache yönergesi hakkında daha fazla bilgi için bkz: [ @OutputCache ](http://go.microsoft.com/fwlink/?linkid=320837).

Bu adımların sonra uygulamanızı Redis çıktı önbelleği sağlayıcısı kullanmak üzere yapılandırılmış.

## <a name="next-steps"></a>Sonraki adımlar
Kullanıma [Azure Redis Cache için ASP.NET oturum durumu sağlayıcısı](cache-aspnet-session-state-provider.md).

