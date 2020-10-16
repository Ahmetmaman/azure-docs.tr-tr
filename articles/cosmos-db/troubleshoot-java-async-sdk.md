---
title: Zaman uyumsuz Java SDK v2 Azure Cosmos DB tanılama ve sorun giderme
description: Zaman uyumsuz Java SDK v2 'de Azure Cosmos DB sorunları tanımlamak, tanılamak ve sorunlarını gidermek için istemci tarafı günlüğe kaydetme ve diğer üçüncü taraf araçları gibi özellikleri kullanın.
author: anfeldma-ms
ms.service: cosmos-db
ms.date: 05/11/2020
ms.author: anfeldma
ms.devlang: java
ms.subservice: cosmosdb-sql
ms.topic: troubleshooting
ms.reviewer: sngun
ms.custom: devx-track-java
ms.openlocfilehash: 60d73f8b3eae21ab399853e8d05b67b7b431ee5f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87321063"
---
# <a name="troubleshoot-issues-when-you-use-the-azure-cosmos-db-async-java-sdk-v2-with-sql-api-accounts"></a>SQL API hesaplarıyla Azure Cosmos DB zaman uyumsuz Java SDK v2 kullandığınızda oluşan sorunları giderme

> [!div class="op_single_selector"]
> * [Java SDK’sı v4](troubleshoot-java-sdk-v4-sql.md)
> * [Zaman uyumsuz Java SDK v2](troubleshoot-java-async-sdk.md)
> * [.NET](troubleshoot-dot-net-sdk.md)
> 

> [!IMPORTANT]
> Bu, Azure Cosmos DB için en son Java SDK 'Sı *değildir* ! Projenizi [Java SDK v4 Azure Cosmos DB](sql-api-sdk-java-v4.md) yükseltmeniz ve ardından Azure Cosmos DB Java SDK v4 [sorun giderme kılavuzunu](troubleshoot-java-sdk-v4-sql.md)okumanız gerekir. Yükseltmek için [Azure Cosmos DB Java SDK 'sı v4](migrate-java-v4-sdk.md) Kılavuzu ve [reaktör vs rxjava](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/master/reactor-rxjava-guide.md) kılavuzundaki yönergeleri izleyin. 
>
> Bu makalede yalnızca Azure Cosmos DB zaman uyumsuz Java SDK v2 için sorun giderme ele alınmaktadır. Daha fazla bilgi için bkz. Azure Cosmos DB zaman uyumsuz Java SDK v2 [sürüm notları](sql-api-sdk-async-java.md), [Maven deposu](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) ve [Performans ipuçları](performance-tips-async-java.md) .
>

Bu makalede, [Java zaman uyumsuz SDK](sql-api-sdk-async-java.md) 'Yı Azure Cosmos db SQL API hesaplarıyla kullandığınızda yaygın sorunlar, geçici çözümler, Tanılama adımları ve araçlar ele alınmaktadır.
Java Async SDK’sı, Azure Cosmos DB SQL API’sine erişim için istemci tarafı mantıksal temsili sağlar. Bu makalede, sorunla karşılaştığınızda size yardımcı olacak araçlar ve yaklaşımlar açıklanır.

Bu listeyle başla:

* Bu makaledeki [yaygın sorunlar ve geçici çözümler] bölümüne göz atın.
* [GitHub 'da kullanılabilir açık kaynak](https://github.com/Azure/azure-cosmosdb-java)olan SDK 'ya bakın. Etkin olarak izlenen bir [sorunlar bölümü](https://github.com/Azure/azure-cosmosdb-java/issues) vardır. Geçici bir çözüm ile benzer bir sorun olup olmadığını denetleyin.
* [Performans ipuçlarını](performance-tips-async-java.md)gözden geçirin ve önerilen uygulamaları izleyin.
* Çözüm bulamazsanız, bu makalenin geri kalanını okuyun. Sonra bir [GitHub sorunu verin](https://github.com/Azure/azure-cosmosdb-java/issues).

## <a name="common-issues-and-workarounds"></a><a name="common-issues-workarounds"></a>Yaygın sorunlar ve geçici çözümler

### <a name="network-issues-netty-read-timeout-failure-low-throughput-high-latency"></a>Ağ sorunları, netty okuma zaman aşımı hatası, düşük aktarım hızı, yüksek gecikme süresi

#### <a name="general-suggestions"></a>Genel öneriler
* Uygulamanın Azure Cosmos DB hesabınızla aynı bölgede çalıştığından emin olun. 
* Uygulamanın çalıştığı konaktaki CPU kullanımını denetleyin. CPU kullanımı yüzde 90 veya daha fazlaysa, uygulamanızı daha yüksek bir yapılandırmaya sahip bir konakta çalıştırın. Ya da yükü daha fazla makineye dağıtabilirsiniz.

#### <a name="connection-throttling"></a>Bağlantı azaltma
[Bir konak makinesindeki bağlantı sınırının] veya [Azure SNAT (Pat) bağlantı noktası tükenmesi]nedeniyle bağlantı azaltma gerçekleşecektir.

##### <a name="connection-limit-on-a-host-machine"></a><a name="connection-limit-on-host"></a>Bir konak makinesinde bağlantı sınırı
Red hat gibi bazı Linux sistemleri, Toplam açık dosya sayısı için üst sınıra sahiptir. Linux 'daki yuvalar dosya olarak uygulanır, bu nedenle bu sayı toplam bağlantı sayısını da sınırlar.
Aşağıdaki komutu çalıştırın.

```bash
ulimit -a
```
"Nofile" olarak tanımlanan en fazla izin verilen açık dosya sayısı, bağlantı havuzu boyutunuzun en az çift katı olması gerekir. Daha fazla bilgi için bkz. [Performans ipuçları](performance-tips-async-java.md).

##### <a name="azure-snat-pat-port-exhaustion"></a><a name="snat"></a>Azure SNAT (PAT) bağlantı noktası tükenmesi

Uygulamanız Azure sanal makinelerinde genel IP adresi olmadan dağıtılmışsa, varsayılan olarak [Azure SNAT bağlantı noktaları](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports) , sanal makinenizin dışındaki herhangi bir uç noktaya bağlantı kurar. VM 'den Azure Cosmos DB uç noktasına izin verilen bağlantı sayısı [Azure SNAT yapılandırması](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#preallocatedports)ile sınırlıdır.

 Azure SNAT bağlantı noktaları yalnızca sanal makinenizin özel bir IP adresi varsa ve VM 'den bir işlem ortak IP adresine bağlanmaya çalıştığında kullanılır. Azure SNAT sınırlamasından kaçınmak için iki geçici çözüm vardır:

* Azure Cosmos DB hizmeti uç noktanızı Azure sanal makineler sanal ağınızın alt ağına ekleyin. Daha fazla bilgi için bkz. [Azure sanal ağ hizmeti uç noktaları](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview). 

    Hizmet uç noktası etkinleştirildiğinde, istekler artık genel bir IP Azure Cosmos DB ' e gönderilmez. Bunun yerine, sanal ağ ve alt ağ kimliği gönderilir. Yalnızca genel IP 'Lere izin veriliyorsa bu değişiklik güvenlik duvarı düşceye neden olabilir. Bir güvenlik duvarı kullanıyorsanız, hizmet uç noktasını etkinleştirdiğinizde, [sanal ağ ACL 'lerini](https://docs.microsoft.com/azure/virtual-network/virtual-networks-acl)kullanarak güvenlik duvarına bir alt ağ ekleyin.
* Azure sanal makinenize genel IP atayın.

##### <a name="cant-reach-the-service---firewall"></a><a name="cant-connect"></a>Hizmete ulaşılamıyor-güvenlik duvarı
``ConnectTimeoutException`` SDK 'nın hizmete ulaşamayacağını gösterir.
Doğrudan modunu kullanırken aşağıdakine benzer bir hata alabilirsiniz:
```
GoneException{error=null, resourceAddress='https://cdb-ms-prod-westus-fd4.documents.azure.com:14940/apps/e41242a5-2d71-5acb-2e00-5e5f744b12de/services/d8aa21a5-340b-21d4-b1a2-4a5333e7ed8a/partitions/ed028254-b613-4c2a-bf3c-14bd5eb64500/replicas/131298754052060051p//', statusCode=410, message=Message: The requested resource is no longer available at the server., getCauseInfo=[class: class io.netty.channel.ConnectTimeoutException, message: connection timed out: cdb-ms-prod-westus-fd4.documents.azure.com/101.13.12.5:14940]
```

Uygulama makinenizde çalışan bir güvenlik duvarınız varsa, doğrudan mod tarafından kullanılan 10.000 numaralı bağlantı noktası aralığını 20.000 ' i açın.
Ayrıca, [bir konak makinesindeki bağlantı sınırını](#connection-limit-on-host)izleyin.

#### <a name="http-proxy"></a>HTTP proxy 'si

Bir HTTP proxy kullanıyorsanız, SDK 'da yapılandırılan bağlantı sayısını destekleyediğinden emin olun `ConnectionPolicy` .
Aksi halde bağlantı sorunlarıyla karşılaşın.

#### <a name="invalid-coding-pattern-blocking-netty-io-thread"></a>Geçersiz kodlama stili: Netty GÇ iş parçacığını engelleme

SDK, Azure Cosmos DB iletişim kurmak için [Netty](https://netty.io/) GÇ kitaplığını kullanır. SDK 'nın zaman uyumsuz API 'ler vardır ve blok olmayan GÇ API 'Lerini kullanır. SDK 'nın GÇ işi, GÇ ağ parçacıklarında gerçekleştirilir. GÇ Netty iş parçacıklarının sayısı, uygulama makinesinin CPU çekirdekleri sayısıyla aynı olacak şekilde yapılandırılmıştır. 

Netty GÇ iş parçacıklarının yalnızca engelleyici olmayan Netty GÇ işleri için kullanılması amaçlanmıştır. SDK, netty GÇ iş parçacıklarından birindeki API çağırma sonucunu uygulamanın koduna döndürür. Uygulama, netty iş parçacığında sonuçları aldıktan sonra uzun süreli bir işlem gerçekleştiriyorsa, SDK 'nın iç GÇ işlerini gerçekleştirmek için yeterli GÇ iş parçacığı olmayabilir. Bu tür uygulamalar, düşük aktarım hızı, yüksek gecikme süresi ve hatalara yol açabilir `io.netty.handler.timeout.ReadTimeoutException` . Geçici çözüm, işlemin zaman aldığını bildiğiniz zaman iş parçacığını geçeceğdir.

Örneğin, aşağıdaki kod parçacığına göz atın. Netty iş parçacığında birkaç milisaniyeye sahip olan uzun süreli işler gerçekleştirebilirsiniz. Bu durumda, sonunda GÇ işini işlemek için bir Netty GÇ iş parçacığı bulunmayan bir duruma ulaşabilirsiniz. Sonuç olarak, bir ReadTimeoutException hatası alırsınız.

### <a name="async-java-sdk-v2-maven-commicrosoftazureazure-cosmosdb"></a><a id="asyncjava2-readtimeout"></a>Async Java SDK v2 (Maven com. Microsoft. Azure:: Azure-cosmosdb)

```java
@Test
public void badCodeWithReadTimeoutException() throws Exception {
    int requestTimeoutInSeconds = 10;

    ConnectionPolicy policy = new ConnectionPolicy();
    policy.setRequestTimeoutInMillis(requestTimeoutInSeconds * 1000);

    AsyncDocumentClient testClient = new AsyncDocumentClient.Builder()
            .withServiceEndpoint(TestConfigurations.HOST)
            .withMasterKeyOrResourceToken(TestConfigurations.MASTER_KEY)
            .withConnectionPolicy(policy)
            .build();

    int numberOfCpuCores = Runtime.getRuntime().availableProcessors();
    int numberOfConcurrentWork = numberOfCpuCores + 1;
    CountDownLatch latch = new CountDownLatch(numberOfConcurrentWork);
    AtomicInteger failureCount = new AtomicInteger();

    for (int i = 0; i < numberOfConcurrentWork; i++) {
        Document docDefinition = getDocumentDefinition();
        Observable<ResourceResponse<Document>> createObservable = testClient
                .createDocument(getCollectionLink(), docDefinition, null, false);
        createObservable.subscribe(r -> {
                    try {
                        // Time-consuming work is, for example,
                        // writing to a file, computationally heavy work, or just sleep.
                        // Basically, it's anything that takes more than a few milliseconds.
                        // Doing such operations on the IO Netty thread
                        // without a proper scheduler will cause problems.
                        // The subscriber will get a ReadTimeoutException failure.
                        TimeUnit.SECONDS.sleep(2 * requestTimeoutInSeconds);
                    } catch (Exception e) {
                    }
                },

                exception -> {
                    //It will be io.netty.handler.timeout.ReadTimeoutException.
                    exception.printStackTrace();
                    failureCount.incrementAndGet();
                    latch.countDown();
                },
                () -> {
                    latch.countDown();
                });
    }

    latch.await();
    assertThat(failureCount.get()).isGreaterThan(0);
}
```
Geçici çözüm, zaman alan iş parçacığını değiştirmek için kullanılır. Uygulamanız için Scheduler 'ın tek bir örneğini tanımlayın.

### <a name="async-java-sdk-v2-maven-commicrosoftazureazure-cosmosdb"></a><a id="asyncjava2-scheduler"></a>Async Java SDK v2 (Maven com. Microsoft. Azure:: Azure-cosmosdb)

```java
// Have a singleton instance of an executor and a scheduler.
ExecutorService ex  = Executors.newFixedThreadPool(30);
Scheduler customScheduler = rx.schedulers.Schedulers.from(ex);
```
Zaman alan, örneğin, yoğun bir iş veya GÇ 'yi engelleme gibi iş yapmanız gerekebilir. Bu durumda, API 'yi kullanarak iş parçacığını tarafından sağlanmış bir çalışana geçirin `customScheduler` `.observeOn(customScheduler)` .

### <a name="async-java-sdk-v2-maven-commicrosoftazureazure-cosmosdb"></a><a id="asyncjava2-applycustomscheduler"></a>Async Java SDK v2 (Maven com. Microsoft. Azure:: Azure-cosmosdb)

```java
Observable<ResourceResponse<Document>> createObservable = client
        .createDocument(getCollectionLink(), docDefinition, null, false);

createObservable
        .observeOn(customScheduler) // Switches the thread.
        .subscribe(
            // ...
        );
```
Kullanarak `observeOn(customScheduler)` , netty GÇ iş parçacığını serbest bırakın ve özel Zamanlayıcı tarafından sağlanmış olan özel iş parçacığına geçiş yapın. Bu değişiklik sorunu çözer. Artık bir hata almazsınız `io.netty.handler.timeout.ReadTimeoutException` .

### <a name="connection-pool-exhausted-issue"></a>Bağlantı havuzu tükendi sorunu

`PoolExhaustedException` istemci tarafı bir hata. Bu hata, uygulama iş yükünüzün SDK bağlantı havuzunun hizmeti tarafından kullanılabileceği miktardan daha yüksek olduğunu gösterir. Bağlantı havuzu boyutunu artırın veya yükü birden çok uygulama üzerinde dağıtın.

### <a name="request-rate-too-large"></a>İstek hızı çok büyük
Bu hata, sunucu tarafı hatasıdır. Sağlanan aktarım hızını kullandınız olduğunu gösterir. Daha sonra yeniden deneyin. Bu hatayla sık sık karşılaşırsanız, koleksiyon aktarım hızı ' nda bir artış düşünün.

### <a name="failure-connecting-to-azure-cosmos-db-emulator"></a>Azure Cosmos DB öykünücüye bağlanılırken hata oluştu

Azure Cosmos DB öykünücü HTTPS sertifikası otomatik olarak imzalanır. SDK 'nın öykünücü ile çalışması için öykünücü sertifikasını bir Java TrustStore 'a aktarın. Daha fazla bilgi için bkz. [Azure Cosmos DB öykünücü sertifikalarını dışarı aktarma](local-emulator-export-ssl-certificates.md).

### <a name="dependency-conflict-issues"></a>Bağımlılık çakışma sorunları

```console
Exception in thread "main" java.lang.NoSuchMethodError: rx.Observable.toSingle()Lrx/Single;
```

Yukarıdaki özel durum, daha eski bir RxJava lib sürümü (ör. 1.2.2) için bir bağımlılığının olduğunu önerir. SDK 'mız, RxJava 'ın önceki sürümlerinde kullanılamayan RxJava 1.3.8 kullanır. 

Bu tür sorunlar için geçici çözüm olarak, RxJava-1.2.2 ' deki diğer bağımlılığın ne olduğunu ve RxJava-1.2.2 üzerinde geçişli bağımlılığı dışarıda bırakmayı ve CosmosDB SDK 'sının daha yeni sürümü getirmesine izin vermeyi belirlemektir.

RxJava-1.2.2 ' de hangi kitaplığın pom.xml olduğunu belirlemek için, proje dosyanızın yanında aşağıdaki komutu çalıştırın:
```bash
mvn dependency:tree
```
Daha fazla bilgi için bkz. [Maven bağımlılık ağacı Kılavuzu](https://maven.apache.org/plugins/maven-dependency-plugin/examples/resolving-conflicts-using-the-dependency-tree.html).

RxJava-1.2.2 ' ı tanımladıktan sonra, projenizin diğer bağımlılığının geçişli bağımlılığı varsa, Pod dosyanızda söz konusu LIB üzerindeki bağımlılığı değiştirebilir ve RxJava geçişli bağımlılığı hariç bırakabilirsiniz:

```xml
<dependency>
  <groupId>${groupid-of-lib-which-brings-in-rxjava1.2.2}</groupId>
  <artifactId>${artifactId-of-lib-which-brings-in-rxjava1.2.2}</artifactId>
  <version>${version-of-lib-which-brings-in-rxjava1.2.2}</version>
  <exclusions>
    <exclusion>
      <groupId>io.reactivex</groupId>
      <artifactId>rxjava</artifactId>
    </exclusion>
  </exclusions>
</dependency>
```

Daha fazla bilgi için bkz. [geçişli bağımlılığı hariç tutma Kılavuzu](https://maven.apache.org/guides/introduction/introduction-to-optional-and-excludes-dependencies.html).


## <a name="enable-client-sdk-logging"></a><a name="enable-client-sice-logging"></a>İstemci SDK günlük kaydını etkinleştir

Java Async SDK, Log4J ve logback gibi popüler günlük çerçeveleri içinde oturum açmayı destekleyen oturum açma façlata olarak dolayısıyla slf4j kullanır.

Örneğin, Log4J 'yi günlüğe kaydetme çerçevesi olarak kullanmak istiyorsanız, Java sınıfizinizdeki aşağıdaki-BS 'leri ekleyin.

```xml
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-log4j12</artifactId>
  <version>${slf4j.version}</version>
</dependency>
<dependency>
  <groupId>log4j</groupId>
  <artifactId>log4j</artifactId>
  <version>${log4j.version}</version>
</dependency>
```

Ayrıca bir Log4J yapılandırması da ekleyin.
```
# this is a sample log4j configuration

# Set root logger level to DEBUG and its only appender to A1.
log4j.rootLogger=INFO, A1

log4j.category.com.microsoft.azure.cosmosdb=DEBUG
#log4j.category.io.netty=INFO
#log4j.category.io.reactivex=INFO
# A1 is set to be a ConsoleAppender.
log4j.appender.A1=org.apache.log4j.ConsoleAppender

# A1 uses PatternLayout.
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%d %5X{pid} [%t] %-5p %c - %m%n
```

Daha fazla bilgi için bkz. [sfl4j Logging el kitabı](https://www.slf4j.org/manual.html).

## <a name="os-network-statistics"></a><a name="netstats"></a>İşletim sistemi ağ istatistikleri
Ve gibi durumlarda birçok bağlantı olduğunu öğrenmek için netstat komutunu çalıştırın `ESTABLISHED` `CLOSE_WAIT` .

Linux 'ta, aşağıdaki komutu çalıştırabilirsiniz.
```bash
netstat -nap
```
Sonucu yalnızca Azure Cosmos DB uç noktasına bağlantılarla filtreleyin.

Durumdaki Azure Cosmos DB uç noktasına bağlantı sayısı, `ESTABLISHED` yapılandırılmış bağlantı havuzu boyutundan büyük olamaz.

Azure Cosmos DB uç noktasına birçok bağlantı `CLOSE_WAIT` durumunda olabilir. 1.000 ' den fazla olabilir. Yüksek bir sayı, bağlantıların hızlı bir şekilde kurulacağını ve yırtılmış olduğunu gösterir. Bu durum potansiyel olarak soruna neden olur. Daha fazla bilgi için bkz. [genel sorunlar ve geçici çözümler] bölümü.

 <!--Anchors-->
[Yaygın sorunlar ve geçici çözümler]: #common-issues-workarounds
[Enable client SDK logging]: #enable-client-sice-logging
[Bir konak makinesinde bağlantı sınırı]: #connection-limit-on-host
[Azure SNAT (PAT) bağlantı noktası tükenmesi]: #snat


