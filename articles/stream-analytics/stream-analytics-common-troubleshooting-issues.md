---
title: Azure Stream Analytics'te sorunlarını gidermek için genel sorunlar
description: Bu makalede, Azure Stream Analytics ve adımları bu sorunları gidermek için bazı yaygın sorunları açıklar.
services: stream-analytics
author: jasonwhowell
manager: kfile
ms.author: jasonh
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/12/2018
ms.openlocfilehash: d3b01e75a9b34ce4e38138816935bdae2e0ea778
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42061740"
---
# <a name="common-issues-in-stream-analytics-and-steps-to-troubleshoot"></a>Sık karşılaşılan sorunları gidermek için Stream Analytics ve adımları

## <a name="troubleshoot-malformed-input-events"></a>Hatalı giriş olaylarını sorun giderme

 Stream Analytics işinizin Giriş akışı yanlış biçimlendirilmiş iletiler içerdiğinde, serileştirme sorunlarına neden olur. Örneğin, hatalı bir ileti nedeni parantezin eksik olması veya bir JSON nesnesinde ayraç eksik veya yanlış olabilir zaman damgası biçiminde saat alanı. 
 
 Bir Stream Analytics işi girdi hatalı bir ileti aldığında, iletiyi bırakır ve kullanıcıya bir uyarı bildirir. Bir uyarı sembolü gösterilir **girişleri** kutucuk Stream Analytics işinizin (Bu uyarı işareti var. işi çalışır durumda olduğu sürece):

![Girişleri kutucuğu](media/stream-analytics-malformed-events/inputs_tile.png)

Daha fazla bilgi için uyarı ayrıntılarını görüntülemek için tanılama günlüklerini etkinleştirin. Giriş yanlış biçimlendirilmiş olaylar için yürütme günlüklerini şuna benzer bir ileti ile bir giriş içeriyor: "ileti: kaynağından giriş olayları seri durumdan çıkarılamadı <blob URI> json olarak". 

### <a name="troubleshooting-steps"></a>Sorun giderme adımları

1. Giriş kutucuğuna gidin ve tıklayarak uyarıları görüntüleyin.

2. Giriş Ayrıntıları kutucuğu, sorun hakkında ayrıntılar uyarılarla bir kümesini görüntüler. Aşağıdaki örnek bir uyarı iletisi, bölüm, uzaklığı ve sıra numaraları uyarı iletisini gösteren hatalı biçimlendirilmiş JSON verilerini olduğu. 

   ![Uzaklığı olan uyarı iletisi](media/stream-analytics-malformed-events/warning_message_with_offset.png)

3. Yanlış biçimde olduğu JSON verilerini almak için CheckMalformedEvents.cs kodu çalıştırın. Bu örnekte kullanılabilir [GitHub örnekleri depomuzdan](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/CheckMalformedEventsEH). Bu kod okuma bölüm kimliği, uzaklığı ve bu uzaklık içinde bulunan veri yazdırır. 

4. Verileri okuduktan sonra, serileştirme biçimini analiz edebilir ve düzeltebilirsiniz.

5. Ayrıca [olayları bir Service Bus Explorer ile IOT Hub'ından okumak](https://code.msdn.microsoft.com/How-to-read-events-from-an-1641eb1b).

## <a name="delayed-output"></a>Gecikmeli çıkış

### <a name="first-output-is-delayed"></a>İlk çıkış ertelendi
Bir Stream Analytics işi başladığında, giriş olayları okumak, ancak bazı durumlarda üretilen çıktıda bir gecikme olabilir.

Zamana bağlı sorgu öğeleri büyük saat değerleri için çıkış gecikmesi katkıda bulunabilir. Büyük zaman pencereleri doğru çıktı oluşturmak için iş akışında zaman penceresi doldurmak için en son zaman mümkün (en fazla yedi gün önce) verilerini okuyarak başlatılır. Bekleyen giriş olaylarını yakalama okuma işlemi tamamlanana kadar bu süre boyunca hiçbir çıktı üretilmiştir. Sistem, böylece iş yeniden başlatma akış işi, yükseltildiğinde bu sorun ortaya çıkabilir. Bu tür yükseltmeler, genellikle bir kez her birkaç ay oluşur. 

Bu nedenle, Stream Analytics sorgunuz tasarlarken dikkatli olun. Büyük bir zaman penceresinde'nı kullanıyorsanız (birden fazla birkaç saat ayarlama için yedi gün) iş başlatıldığında veya yeniden başlatıldığında işin sorgu söz dizimi, zamana bağlı öğeleri için bunu bir gecikme için ilk çıktı yol açabilir.  

İlk çıkış gecikme bu tür için bir risk azaltma, sorgunun paralelleştirme teknikleri (veriyi bölümlendirme) kullanın veya daha fazla akış işi arayı kapatıncaya kadar işleme oranını artırmak için birim ekleyin sağlamaktır.  Daha fazla bilgi için [Stream Analytics işleri oluştururken dikkat edilecek noktalar](stream-analytics-concepts-checkpoint-replay.md)

Bu etkenler oluşturulan ilk çıkışın dakikliğini etkiler:

1. Pencereli toplamlar (grup tarafından atlaması ve windows kayan atlayan,) kullanımı
   - Atlayan veya atlamalı pencere toplamlar için sonuçları penceresi süre sonunda oluşturulur. 
   - Bir olay girer veya kayan pencere çıkar, kayan bir pencere için sonuçları üretilir. 
   - Büyük pencere boyutu (> 1 saat) kullanmayı planlıyorsanız, böylece çıkışı daha sık bakın atlamalı veya kayan bir pencere seçmek idealdir.

2. Zamana bağlı birleşimler (DATEDIFF katılma) kullanımı
   - Eşleşen her iki tarafında eşleşen olaylar geldiğinde hemen sonra oluşturulur.
   - Bir eşleşme (sol dış birleştirme) eksik veri sonunda, DATEDIFF pencerenin sol tarafındaki her bir olay göre oluşturulur.

3. Zamana bağlı analitik işlevler (ISFİRST, LAST ve GECİKME süresi sınırı) kullanımı
   - Analitik İşlevler, her olay için bir çıktı oluşturulur, gecikme.

### <a name="output-falls-behind"></a>Çıkış geride
İşin normal işlem sırasında işin çıktısını (uzun ve daha uzun gecikme), geride kalıyor fark ederseniz bu faktörleri inceleyerek nedenlerini saptayabilirler:
- Aşağı Akış havuz olup kısıtlandı
- Yukarı Akış kaynağı olup olmadığını kısıtlandı
- Sorgu işleme mantığı yoğun işlem gücü kullanımlı olup olmadığı

Azure portalında ayrıntılarını görmek için iş akışında seçip **iş diyagramı**. Her bir giriş var olan bir bölüm biriktirme listesi olay ölçüm. Biriktirme listesi olay ölçümü artmaya devam ederse, sistem kaynaklarının sınırlı olduğu bir göstergesidir. Potansiyel olarak verilecek çıkış havuzu kısıtlama veya yüksek CPU olmasıdır. İş diyagramı kullanma hakkında daha fazla bilgi için bkz. [veri odaklı iş diyagramı kullanarak hata ayıklama](stream-analytics-job-diagram-with-metrics.md).

## <a name="handle-duplicate-records-in-azure-sql-database-output"></a>Azure SQL veritabanı çıkışında yinelenen kayıtları işleme

Azure SQL veritabanı için bir Stream Analytics işi çıktı olarak yapılandırdığınızda, yığın kayıtları hedef tabloya ekler. Genel olarak, Azure stream analytics garanti eder [en az bir kere teslim]( https://msdn.microsoft.com/azure/stream-analytics/reference/event-delivery-guarantees-azure-stream-analytics) bir çıkış havuzuna yine de [tam olarak elde-kez teslim]( https://blogs.msdn.microsoft.com/streamanalytics/2017/01/13/how-to-achieve-exactly-once-delivery-for-sql-output/) SQL tablosu, tanımlı bir kısıtlama olduğunda SQL çıktı. 

Azure Stream Analytics, benzersiz anahtar kısıtlamaları SQL tablosunda ayarlanır ve SQL tablosuna eklenen yinelenen kayıt sonra yinelenen kayıt kaldırır. Toplu işleri ve yinelemeli olarak tek bir yinelenen kayıt bulunana kadar toplu ekleme verileri ayırır. Akış işi önemli sayıda yinelenen satır, bu bölme olduğundan ve işlem eklemek daha az verimli ve zaman alıcı olan tek, yinelenenleri yok saymak vardır. Etkinlik günlüğü'nde son bir saat içinde birden çok anahtar ihlali uyarı iletisi görürseniz, SQL çıkışınızı tüm işin yavaşlattığını olasıdır. 

Bu sorunu gidermek için şunları yapmalısınız [dizini yapılandırma]( https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql) , neden anahtar ihlali IGNORE_DUP_KEY seçeneğini etkinleştirerek. Bu seçeneğin etkinleştirilmesi, SQL toplu ekleme sırasında yoksayılacak yinelenen değerler sağlar ve SQL Azure, yalnızca bir uyarı iletisi yerine bir hata üretir. Azure Stream Analytics artık birincil anahtar ihlali hatalarını üretmez.

IGNORE_DUP_KEY dizin çeşitli türleri için yapılandırırken aşağıdaki gözlemlere unutmayın:

* Bir birincil anahtar veya ALTER INDEX kullanan benzersiz kısıtlama IGNORE_DUP_KEY ayarlayamazsınız, bırakın ve dizini yeniden oluşturmanız gerekir.  
* BİRİNCİL anahtar benzersiz kısıtlamasından farklıdır ve CREATE INDEX veya dizin tanımı kullanılarak oluşturulan benzersiz bir dizin için ALTER INDEX kullanarak IGNORE_DUP_KEY seçeneği ayarlayabilirsiniz.  
* Bu dizinlerin benzersizlik olamaz çünkü IGNORE_DUP_KEY sütun deposu dizinleri için geçerli değildir.  

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
