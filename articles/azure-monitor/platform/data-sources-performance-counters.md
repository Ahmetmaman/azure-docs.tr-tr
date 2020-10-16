---
title: Azure Izleyici 'de performans sayaçlarını toplayın ve çözümleyin | Microsoft Docs
description: Performans sayaçları, Windows ve Linux aracılarındaki performansı çözümlemek için Azure Izleyici tarafından toplanır.  Bu makalede, hem Windows hem de Linux aracıları için performans sayaçları koleksiyonunun nasıl yapılandırılacağı, bunların ayrıntılarının çalışma alanında saklandıkları ve Azure portal nasıl çözümleneceği açıklanır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/28/2018
ms.openlocfilehash: 49f944aa98bf0bf8090b10d2feeb50af4a2d42b2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "85955497"
---
# <a name="windows-and-linux-performance-data-sources-in-azure-monitor"></a>Azure Izleyici 'de Windows ve Linux performansı veri kaynakları
Windows ve Linux 'ta performans sayaçları, donanım bileşenlerinin, işletim sistemlerinin ve uygulamaların performansına ilişkin öngörüler sağlar.  Azure Izleyici, daha uzun süreli analiz ve raporlama için performans verilerinin toplanmasının yanı sıra, neredeyse gerçek zamanlı (NRT) analiz için sık aralıklarla performans sayaçlarını toplayabilir.

![Performans sayaçları](media/data-sources-performance-counters/overview.png)

## <a name="configuring-performance-counters"></a>Performans sayaçlarını yapılandırma
[Gelişmiş ayarlar 'Daki veri menüsünden](agent-data-sources.md#configuring-data-sources)performans sayaçlarını yapılandırın.

Yeni bir çalışma alanı için Windows veya Linux performans sayaçlarını ilk kez yapılandırdığınızda, birkaç ortak sayacı hızlı bir şekilde oluşturma seçeneği sunulur.  Her birinin yanında bir onay kutusu görüntülenir.  Başlangıçta oluşturulmasını istediğiniz tüm sayaçların işaretli olduğundan emin olun ve ardından **Seçili performans sayaçlarını Ekle**' ye tıklayın.

Windows performans sayaçları için her performans sayacı için belirli bir örnek seçebilirsiniz. Linux performans sayaçları için, seçtiğiniz her sayacın örneği, üst sayacın tüm alt sayaçları için geçerlidir. Aşağıdaki tabloda hem Linux hem de Windows performans sayaçları için kullanılabilen ortak örnekler gösterilmektedir.

| Örnek adı | Açıklama |
| --- | --- |
| \_Toplam |Tüm örneklerin toplamı |
| \* |Tüm örnekler |
| (/&#124;/var) |Şu adlı örneklerle eşleşir:/veya/var |

### <a name="windows-performance-counters"></a>Windows performans sayaçları

![Windows performans sayaçlarını yapılandırma](media/data-sources-performance-counters/configure-windows.png)

Toplanacak yeni bir Windows performans sayacı eklemek için bu yordamı izleyin.

1. *Nesne (örnek) \Sayaç*biçiminde metin kutusuna sayacın adını yazın.  Yazmaya başladığınızda, size ortak sayaçların eşleşen bir listesi sunulur.  Listeden bir sayaç seçebilir veya kendi listenizden birini yazabilirsiniz.  Ayrıca, *object\counter*belirterek belirli bir sayaca ait tüm örnekleri döndürebilirsiniz.  

    Adlandırılmış örneklerden SQL Server performans sayaçlarını toplarken, tüm adlandırılmış örnek sayaçları *MSSQL $* ile başlar ve ardından örnek adı gelir.  Örneğin, INST2 adlı SQL örneği için veritabanı performans nesnesinden tüm veritabanları için günlük önbelleği Isabet oranı sayacını toplamak için, belirtin `MSSQL$INST2:Databases(*)\Log Cache Hit Ratio` .

2. **+** Sayacı listeye eklemek **Enter** için tıklayın veya ENTER tuşuna basın.
3. Bir sayaç eklediğinizde, **örnek aralığı**için varsayılan 10 saniye kullanır.  Toplanan performans verilerinin depolama gereksinimlerini azaltmak istiyorsanız, bu değeri 1800 saniyeye (30 dakika) daha yüksek bir değere dönüştürebilirsiniz.
4. Sayaç eklemeyi tamamladığınızda, yapılandırmayı kaydetmek için ekranın üst kısmındaki **Kaydet** düğmesine tıklayın.

### <a name="linux-performance-counters"></a>Linux performans sayaçları

![Linux performans sayaçlarını yapılandırma](media/data-sources-performance-counters/configure-linux.png)

Toplanacak yeni bir Linux performans sayacı eklemek için bu yordamı izleyin.

1. Varsayılan olarak, tüm yapılandırma değişiklikleri otomatik olarak tüm aracılara gönderilir.  Linux aracıları için bir yapılandırma dosyası, Floentd veri toplayıcısına gönderilir.  Bu dosyayı her bir Linux aracısında el ile değiştirmek istiyorsanız, *Linux makinelerime aşağıdaki yapılandırmayı Uygula* onay kutusunun işaretini kaldırın ve aşağıdaki yönergeleri izleyin.
2. *Nesne (örnek) \Sayaç*biçiminde metin kutusuna sayacın adını yazın.  Yazmaya başladığınızda, size ortak sayaçların eşleşen bir listesi sunulur.  Listeden bir sayaç seçebilir veya kendi listenizden birini yazabilirsiniz.  
3. **+** Sayacı, nesne **Enter** için diğer sayaçlar listesine eklemek için tıklayın veya ENTER tuşuna basın.
4. Bir nesne için tüm sayaçlar aynı **örnek aralığı**kullanır.  Varsayılan değer 10 saniyedir.  Toplanan performans verilerinin depolama gereksinimlerini azaltmak istiyorsanız bunu 1800 saniyeye (30 dakika) daha yüksek bir değere değiştirirsiniz.
5. Sayaç eklemeyi tamamladığınızda, yapılandırmayı kaydetmek için ekranın üst kısmındaki **Kaydet** düğmesine tıklayın.

#### <a name="configure-linux-performance-counters-in-configuration-file"></a>Yapılandırma dosyasında Linux performans sayaçlarını yapılandırma
Linux performans sayaçlarını Azure portal kullanarak yapılandırmak yerine, Linux aracısında yapılandırma dosyalarını düzenlemeyle ilgili seçeneğiniz vardır.  Toplanacak performans ölçümleri, **/etc/seçenek/Microsoft/omsagent/ \<workspace id\> /conf/omsagent.exe**içindeki yapılandırma tarafından denetlenir.

Toplanacak performans ölçümlerinin her nesnesi veya kategorisi yapılandırma dosyasında tek bir öğe olarak tanımlanmalıdır `<source>` . Sözdizimi aşağıdaki kalıbı izler.

```xml
<source>
    type oms_omi  
    object_name "Processor"
    instance_regex ".*"
    counter_name_regex ".*"
    interval 30s
</source>
```


Bu öğedeki parametreler aşağıdaki tabloda açıklanmıştır.

| Parametreler | Açıklama |
|:--|:--|
| nesne \_ adı | Koleksiyonun nesne adı. |
| örnek \_ Regex |  Toplanacak örnekleri tanımlayan bir *normal ifade* . Değer: `.*` tüm örnekleri belirtir. Yalnızca toplam örnek için işlemci ölçümlerini toplamak üzere \_ belirtebilirsiniz `_Total` . Yalnızca crond veya SSHD örnekleri için işlem ölçümlerini toplamak üzere şunları belirtebilirsiniz: `(crond\|sshd)` . |
| Sayaç \_ adı \_ Regex | Hangi sayaçların (nesne için) toplanacağını tanımlayan bir *normal ifade* . Nesnenin tüm sayaçlarını toplamak için şunu belirtin: `.*` . Yalnızca bellek nesnesi için takas alanı sayaçlarını toplamak için, örneğin şunları belirtebilirsiniz: `.+Swap.+` |
| interval | Nesnenin sayaçlarının toplandığı sıklık. |


Aşağıdaki tabloda, yapılandırma dosyasında belirtebileceğiniz nesneler ve sayaçlar listelenmektedir.  [Azure izleyici 'de Linux uygulamaları için performans sayaçlarını toplama](data-sources-linux-applications.md)bölümünde açıklandığı gibi belirli uygulamalar için kullanılabilir ek sayaçlar vardır.

| Nesne adı | Sayaç Adı |
|:--|:--|
| Mantıksal disk | % Boş ınomdes |
| Mantıksal disk | % Boş alan |
| Mantıksal disk | % Kullanılan ınomdes |
| Mantıksal disk | % Kullanılan alan |
| Mantıksal disk | Disk okuma bayt/sn |
| Mantıksal disk | Disk Okuma/sn |
| Mantıksal disk | Disk aktarımı/sn |
| Mantıksal disk | Disk yazma bayt/sn |
| Mantıksal disk | Disk yazma/sn |
| Mantıksal disk | Boş megabayt |
| Mantıksal disk | Mantıksal disk bayt/sn |
| Bellek | % Kullanılabilir bellek |
| Bellek | % Kullanılabilir takas alanı |
| Bellek | % Kullanılan bellek |
| Bellek | % Kullanılan takas alanı |
| Bellek | Kullanılabilir MBayt belleği |
| Bellek | Kullanılabilir MBayt takas |
| Bellek | Sayfa Okuma/sn |
| Bellek | Sayfa yazma/sn |
| Bellek | Sayfa/sn |
| Bellek | Kullanılan MBayt değiştirme alanı |
| Bellek | Kullanılan bellek MBayt |
| Ağ | Aktarılan toplam bayt sayısı |
| Ağ | Alınan toplam bayt |
| Ağ | Toplam bayt sayısı |
| Ağ | Aktarılan toplam paket sayısı |
| Ağ | Alınan toplam paket sayısı |
| Ağ | Toplam RX hatası sayısı |
| Ağ | Toplam TX hatası sayısı |
| Ağ | Toplam çakışma sayısı |
| Fiziksel disk | Ortalama Disk sn/okuma |
| Fiziksel disk | Ortalama Disk sn/Aktarım |
| Fiziksel disk | Ortalama Disk sn/yazma |
| Fiziksel disk | Fiziksel disk bayt/sn |
| İşleme | Ayrıcalıklı zaman yüzdesi |
| İşleme | Kullanım süresi yüzdesi |
| İşleme | Kullanılan bellek Kbayt |
| İşleme | Sanal paylaşılan bellek |
| İşlemci | % DPC Zamanı |
| İşlemci | % Boşta kalma süresi |
| İşlemci | Kesme zamanı yüzdesi |
| İşlemci | % GÇ bekleme süresi |
| İşlemci | % İyi zaman |
| İşlemci | % Ayrıcalıklı zaman |
| İşlemci | % İşlemci zamanı |
| İşlemci | % Kullanıcı Zamanı |
| Sistem | Boş fiziksel bellek |
| Sistem | Disk belleği dosyalarındaki boş alan |
| Sistem | Boş sanal bellek |
| Sistem | İşlemler |
| Sistem | Disk belleği dosyalarında depolanan boyut |
| Sistem | Çalışma süresi |
| Sistem | Kullanıcılar |


Performans ölçümlerinin varsayılan yapılandırması aşağıda verilmiştir.

```xml
<source>
    type oms_omi
    object_name "Physical Disk"
    instance_regex ".*"
    counter_name_regex ".*"
    interval 5m
</source>

<source>
    type oms_omi
    object_name "Logical Disk"
    instance_regex ".*
    counter_name_regex ".*"
    interval 5m
</source>

<source>
    type oms_omi
    object_name "Processor"
    instance_regex ".*
    counter_name_regex ".*"
    interval 30s
</source>

<source>
    type oms_omi
    object_name "Memory"
    instance_regex ".*"
    counter_name_regex ".*"
    interval 30s
</source>
```

## <a name="data-collection"></a>Veri toplama
Azure Izleyici, bu sayacın yüklü olduğu tüm aracıların belirtilen örnek aralığındaki tüm belirtilen performans sayaçlarını toplar.  Veriler toplanmaz ve ham veriler, Log Analytics çalışma alanınız tarafından belirtilen süre için tüm günlük sorgu görünümlerinde kullanılabilir.

## <a name="performance-record-properties"></a>Performans kaydı özellikleri
Performans kayıtları bir **perf** türüne sahiptir ve aşağıdaki tabloda bulunan özelliklere sahiptir.

| Özellik | Açıklama |
|:--- |:--- |
| Bilgisayar |Olayın toplandığı bilgisayar. |
| CounterName |Performans sayacının adı |
| Onay yolu |Form \\ \\ \<Computer> \\ nesnesi (örnek) sayacında sayacın tam yolu \\ . |
| Onay değeri |Sayacın sayısal değeri. |
| InstanceName |Olay örneğinin adı.  Örnek yoksa boştur. |
| ObjectName |Performans nesnesinin adı |
| SourceSystem |Verilerin toplandığı aracının türü. <br><br>OpsManager – Windows Aracısı, doğrudan bağlantı ya da SCOM <br> Linux – tüm Linux aracıları  <br> AzureStorage – Azure Tanılama |
| TimeGenerated |Verilerin örneklendiği tarih ve saat. |

## <a name="sizing-estimates"></a>Boyutlandırma tahminleri
 10 saniyelik aralıklarda belirli bir sayacın toplanması için kabaca bir tahmin, örnek başına günlük 1 MB 'dir.  Belirli bir sayacın depolama gereksinimlerini aşağıdaki formül ile tahmin edebilirsiniz.

> 1 MB x (sayaç sayısı) x (aracı sayısı) x (örnek sayısı)

## <a name="log-queries-with-performance-records"></a>Performans kayıtlarıyla günlük sorguları
Aşağıdaki tabloda, performans kayıtlarını alan günlük sorgularının farklı örnekleri verilmiştir.

| Sorgu | Açıklama |
|:--- |:--- |
| Perf |Tüm performans verileri |
| Perf &#124; Computer = = "Bilgisayarım" |Belirli bir bilgisayardaki tüm performans verileri |
| Performans &#124; CounterName = = "geçerli disk kuyruğu uzunluğu" |Belirli bir sayaç için tüm performans verileri |
| Performans &#124;; ObjectName = = "Processor" ve CounterName = = "% Işlemci zamanı" ve InstanceName = = "_Total" &#124;, bilgisayar tarafından AVGCPU = Ort (CounterValue) öğesini özetleme |Tüm bilgisayarlar genelinde ortalama CPU kullanımı |
| &#124; CounterName = = "% Işlemci zamanı" &#124;, bilgisayara göre Aggreg, value = Max (CounterValue) özetleme |Tüm bilgisayarlar genelinde en fazla CPU kullanımı |
| Performans &#124;; ObjectName = = "MantıksalDisk" ve CounterName = = "geçerli disk kuyruğu uzunluğu" ve bilgisayar = = "MyComputerName" &#124;, InstanceName 'e göre Aggregtutulma değeri = AVG (CounterValue) |Belirli bir bilgisayarın tüm örnekleri genelinde ortalama geçerli disk sırası uzunluğu |
| &#124; CounterName = = "disk aktarımı/sn" &#124; Aggregve değer = yüzdebirlik (CounterValue, 95) bilgisayar bazında özetlendiği performans |tüm bilgisayarlarda yüzde 95 disk aktarımı/sn |
| &#124; CounterName = = "% Işlemci zamanı" ve InstanceName = = "_Total" &#124; Özetleme ölçütü (TimeGenerated, 1h), bilgisayar |Tüm bilgisayarlarda saatlik ortalama CPU kullanımı |
| Bilgisayar = = "Bilgisayarım" ve CounterName startswith_cs "%" ve InstanceName = = "_Total" &#124; Özetleme ölçütü, Aggregx değer = yüzdebirlik (CounterValue, 70) for bin (TimeGenerated, 1h), CounterName &#124; | Belirli bir bilgisayar için saatlik 70 yüzdebirlik her% yüzde sayacı |
| &#124; CounterName = = "% Işlemci zamanı" ve InstanceName = = "_Total" ve Computer = = "Bilgisayarım" &#124; özetleme ["min (CounterValue)"] = MIN (CounterValue), ["AVG (CounterValue)"] = AVG (CounterValue), ["percentile75 (CounterValue)"] = yüzdebirlik (CounterValue, 75), ["Max (CounterValue)"] = Max (CounterValue) for bin (TimeGenerated, 1h), bilgisayar |Belirli bir bilgisayar için saatlik ortalama, en düşük, en yüksek ve 75-yüzdebirlik CPU kullanımı |
| Perf &#124;; NesneAdı = = "MSSQL $ INST2: Databases" ve InstanceName = = "Master" | Ana veritabanı için veritabanı performans nesnesinden tüm performans verileri, INST2 adlı SQL Server örneğinden.  




## <a name="next-steps"></a>Sonraki adımlar
* MySQL ve Apache HTTP sunucusu dahil [Linux uygulamalarından performans sayaçlarını toplayın](data-sources-linux-applications.md) .
* Veri kaynaklarından ve çözümlerinden toplanan verileri analiz etmek için [günlük sorguları](../log-query/log-query-overview.md) hakkında bilgi edinin.  
* Toplanan verileri, ek görselleştirmeler ve analizler için [Power BI](powerbi.md) dışa aktarın.
