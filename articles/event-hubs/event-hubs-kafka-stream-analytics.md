---
title: Azure Event Hubs-Işlem Apache Kafka olayları
description: "Öğretici: Bu makalede Azure Stream Analytics kullanılarak Olay Hub 'ları aracılığıyla alınan Kafka olaylarının nasıl işlenmesi gösterilmektedir."
ms.topic: tutorial
ms.date: 06/23/2020
ms.openlocfilehash: 328537eebd05391fc4c8138395a9c10f0a5d072f
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92319388"
---
# <a name="tutorial-process-apache-kafka-for-event-hubs-events-using-stream-analytics"></a>Öğretici: Stream Analytics kullanarak Event Hubs olaylar için Işlem Apache Kafka 
Bu makalede, verilerin Event Hubs ' a akışını ve Azure Stream Analytics nasıl işleyeceğini gösterir. Aşağıdaki adımlarda size yol gösterir: 

1. Bir Event Hubs ad alanı oluşturun.
2. Olay Hub 'ına ileti gönderen bir Kafka istemcisi oluşturun.
3. Olay Hub 'ından Azure Blob depolama alanına veri kopyalayan bir Stream Analytics iş oluşturun. 

Bir olay hub 'ı tarafından kullanıma sunulan Kafka uç noktasını kullandığınızda protokol istemcilerinizi değiştirmeniz veya kendi kümelerinizi çalıştırmanız gerekmez. Azure Event Hubs [Apache Kafka sürüm 1.0](https://kafka.apache.org/10/documentation.html)’ı destekler. ve üzeri. 


## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için aşağıdaki önkoşulların karşılandığından emin olun:

* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
* [Java Development Kit (JDK) 1.7 +](/azure/developer/java/fundamentals/java-jdk-long-term-support).
* Bir Maven ikili arşivini [indirin](https://maven.apache.org/download.cgi) ve [yükleyin](https://maven.apache.org/install.html).
* [Git](https://www.git-scm.com/)
* Bir **Azure depolama hesabı**. Yoksa, devam etmeden önce [bir tane oluşturun](../storage/common/storage-account-create.md) . Bu izlenecek yolda Stream Analytics işi, çıktı verilerini bir Azure Blob depolama alanında depolar. 


## <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma
Bir **Standart** katman Event Hubs ad alanı oluşturduğunuzda, ad alanı için Kafka uç noktası otomatik olarak etkinleştirilir. Kafka protokolünü kullanan uygulamalarınızdan olayları Standart katman Event Hubs akışı yapabilirsiniz. **Standart** katman Event Hubs ad alanı oluşturmak için [Azure Portal kullanarak Olay Hub 'ı oluşturma](event-hubs-create.md) bölümünde adım adım yönergeleri izleyin. 

> [!NOTE]
> Kafka için Event Hubs yalnızca **Standart** ve **adanmış** katmanlarda kullanılabilir. **Temel** katman Event Hubs Kafka desteklemez.

## <a name="send-messages-with-kafka-in-event-hubs"></a>Kafka ile ileti gönderme Event Hubs

1. [Azure Event Hubs for Kafka Repository](https://github.com/Azure/azure-event-hubs-for-kafka) 'yi makinenize kopyalayın.
2. Klasöre gidin: `azure-event-hubs-for-kafka/quickstart/java/producer` . 
4. İçindeki Producer yapılandırma ayrıntılarını güncelleştirin `src/main/resources/producer.config` . **Olay Hub 'ı ad alanı**için **adı** ve **bağlantı dizesini** belirtin. 

    ```xml
    bootstrap.servers={EVENT HUB NAMESPACE}.servicebus.windows.net:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{CONNECTION STRING for EVENT HUB NAMESPACE}";
    ```

5. `azure-event-hubs-for-kafka/quickstart/java/producer/src/main/java/`' A gidin ve seçtiğiniz bir düzenleyicide **Testdatareporter. Java** dosyasını açın. 
6. Aşağıdaki kod satırını açıklama:

    ```java
                //final ProducerRecord<Long, String> record = new ProducerRecord<Long, String>(TOPIC, time, "Test Data " + i);
    ```
3. Açıklamalı kodun yerine aşağıdaki kod satırını ekleyin: 

    ```java
                final ProducerRecord<Long, String> record = new ProducerRecord<Long, String>(TOPIC, time, "{ \"eventData\": \"Test Data " + i + "\" }");            
    ```

    Bu kod, olay verilerini **JSON** biçiminde gönderir. Stream Analytics bir iş için giriş yapılandırdığınızda, JSON öğesini giriş verilerinin biçimi olarak belirtirsiniz. 
7. **Üretici** ve akışı Event Hubs ' ye çalıştırın. Bir Windows makinesinde, **Node.js komut istemi**kullanırken, `azure-event-hubs-for-kafka/quickstart/java/producer` Bu komutları çalıştırmadan önce klasöre geçin. 
   
    ```shell
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```

## <a name="verify-that-event-hub-receives-the-data"></a>Olay Hub 'ının verileri aldığını doğrulama

1. **Varlıklar**altında **Event Hubs** seçin. **Test**adlı bir olay hub 'ı görtığınızdan emin olun. 

    ![Olay Hub 'ı-test](./media/event-hubs-kafka-stream-analytics/test-event-hub.png)
2. Olay Hub 'ına gelen iletileri görtığınızdan emin olun. 

    ![Olay Hub 'ı-iletiler](./media/event-hubs-kafka-stream-analytics/confirm-event-hub-messages.png)

## <a name="process-event-data-using-a-stream-analytics-job"></a>Stream Analytics işini kullanarak olay verilerini işleme
Bu bölümde bir Azure Stream Analytics işi oluşturursunuz. Kafka istemcisi olayları Olay Hub 'ına gönderir. Olay verilerini giriş olarak alan ve bunu bir Azure Blob depolama alanına çıkaran bir Stream Analytics işi oluşturursunuz. **Azure depolama hesabınız**yoksa [bir tane oluşturun](../storage/common/storage-account-create.md).

Stream Analytics işteki sorgu, herhangi bir analiz yapmadan verileri geçirir. Giriş verilerini, farklı bir biçimde veya elde edilen öngörülerle çıkış verileri oluşturacak şekilde dönüştüren bir sorgu oluşturabilirsiniz.  

### <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma 

1. Azure portal **+ kaynak oluştur** ' u seçin [Azure portal](https://portal.azure.com).
2. **Azure Marketi** menüsünde **analiz** ' i seçin ve **Stream Analytics iş**' ı seçin. 
3. **Yeni Stream Analytics** sayfasında aşağıdaki işlemleri yapın: 
    1. İş için bir **ad** girin. 
    2. **Aboneliğinizi** seçin.
    3. **Kaynak grubu** Için **Yeni oluştur** ' u seçin ve adı girin. **Var olan bir** kaynak grubunu da kullanabilirsiniz. 
    4. İş için bir **konum** seçin.
    5. İşi oluşturmak için **Oluştur** ' u seçin. 

        ![Yeni Stream Analytics işi](./media/event-hubs-kafka-stream-analytics/new-stream-analytics-job.png)

### <a name="configure-job-input"></a>İş girişi yapılandırma

1. Bildirim iletisinde, **Stream Analytics işi** sayfasını görmek Için **Kaynağa Git** ' i seçin. 
2. Sol menüdeki **Iş topolojisi** bölümünde **girişler** ' i seçin.
3. **Akış girişi Ekle**' yi ve ardından **Olay Hub**'ı ' nı seçin. 

    ![Giriş olarak olay hub 'ı ekleme](./media/event-hubs-kafka-stream-analytics/select-event-hub-input.png)
4. **Olay Hub 'ı giriş** yapılandırması sayfasında, aşağıdaki işlemleri yapın: 

    1. Giriş için bir **diğer ad** belirtin. 
    2. **Azure aboneliğinizi**seçin.
    3. Daha önce oluşturduğunuz **Olay Hub 'ı ad alanını** seçin. 
    4. **Olay Hub 'ı**için **Test** ' i seçin. 
    5. **Kaydet**’i seçin. 

        ![Olay Hub 'ı giriş yapılandırması](./media/event-hubs-kafka-stream-analytics/event-hub-input-configuration.png)

### <a name="configure-job-output"></a>İş çıkışını yapılandırma 

1. Menüdeki **Iş topolojisi** bölümünde yer alan **çıktılar** ' i seçin. 
2. Araç çubuğunda **+ Ekle** ' yi seçin ve **BLOB depolama** ' yı seçin.
3. BLOB depolama çıkış ayarları sayfasında, aşağıdaki işlemleri yapın: 
    1. Çıkış için bir **diğer ad** belirtin. 
    2. Azure **aboneliğinizi**seçin. 
    3. **Azure depolama hesabınızı**seçin. 
    4. Stream Analytics sorgusundan çıkış verilerini depolayan **kapsayıcı için bir ad** girin.
    5. **Kaydet**’i seçin.

        ![BLOB depolama çıkış yapılandırması](./media/event-hubs-kafka-stream-analytics/output-blob-settings.png)
 

### <a name="define-a-query"></a>Sorgu tanımlama
Gelen bir veri akışını okumak için bir Stream Analytics işi ayarladıktan sonraki adım, verileri gerçek zamanlı olarak analiz eden bir dönüştürme oluşturmaktır. Dönüştürme sorgusunu [Stream Analytics sorgu dilini](/stream-analytics-query/stream-analytics-query-language-reference) kullanarak tanımlarsınız. Bu kılavuzda, herhangi bir dönüşüm yapmadan verilerden geçen bir sorgu tanımlarsınız.

1. **Sorgu**seçin.
2. Sorgu penceresinde, `[YourOutputAlias]` daha önce oluşturduğunuz çıktı diğer adıyla değiştirin.
3. `[YourInputAlias]`Daha önce oluşturduğunuz giriş diğer adıyla değiştirin. 
4. Araç çubuğunda **Kaydet**’i seçin. 

    ![Ekran yakalama, giriş ve çıkış değişkenlerinin değerlerini içeren sorgu penceresini gösterir.](./media/event-hubs-kafka-stream-analytics/query.png)


### <a name="run-the-stream-analytics-job"></a>Stream Analytics işini çalıştırma

1. Sol menüdeki **Genel Bakış ' ı** seçin. 
2. **Başlat**'ı seçin. 

    ![Başlat menüsü](./media/event-hubs-kafka-stream-analytics/start-menu.png)
1. **Işi Başlat** sayfasında **Başlat**' ı seçin. 

    ![İşi Başlat sayfası](./media/event-hubs-kafka-stream-analytics/start-job-page.png)
1. İşin durumunun **çalışmaya** **başlamasını** bekleyin. 

    ![İş durumu-çalışıyor](./media/event-hubs-kafka-stream-analytics/running.png)

## <a name="test-the-scenario"></a>Senaryoyu test etme
1. Olayları Olay Hub 'ına göndermek için **Kafka üreticisi** 'ni yeniden çalıştırın. 

    ```shell
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```
1. **Azure Blob depolamada** **çıktı verilerinin** oluşturulduğunu görtığınızdan emin olun. Kapsayıcıda aşağıdaki örnek satırlara benzeyen 100 satırlık bir JSON dosyası görürsünüz: 

    ```
    {"eventData":"Test Data 0","EventProcessedUtcTime":"2018-08-30T03:27:23.1592910Z","PartitionId":0,"EventEnqueuedUtcTime":"2018-08-30T03:27:22.9220000Z"}
    {"eventData":"Test Data 1","EventProcessedUtcTime":"2018-08-30T03:27:23.3936511Z","PartitionId":0,"EventEnqueuedUtcTime":"2018-08-30T03:27:22.9220000Z"}
    {"eventData":"Test Data 2","EventProcessedUtcTime":"2018-08-30T03:27:23.3936511Z","PartitionId":0,"EventEnqueuedUtcTime":"2018-08-30T03:27:22.9220000Z"}
    ```

    Azure Stream Analytics işi, Olay Hub 'ından giriş verileri aldı ve bu senaryodaki Azure Blob depolama alanında depolandı. 



## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, protokol istemcilerinizi değiştirmeden veya kendi kümelerinizi çalıştırmadan Event Hubs nasıl akışa alınacağını öğrendiniz. Apache Kafka Event Hubs hakkında daha fazla bilgi edinmek için bkz. [Azure için geliştirici kılavuzu Apache Kafka Event Hubs](apache-kafka-developer-guide.md).