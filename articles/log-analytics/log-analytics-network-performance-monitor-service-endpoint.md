---
title: Ağ Performansı İzleyicisi çözümü Azure Log analytics'te | Microsoft Docs
description: Hizmet Bağlantı İzleyicisi özelliği ağ Performans İzleyicisi'nde açık bir TCP bağlantı noktasına sahip herhangi bir uç noktası için ağ bağlantılarını izlemek için kullanın.
services: log-analytics
documentationcenter: ''
author: abshamsft
manager: carmonm
editor: ''
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/20/2018
ms.author: abshamsft
ms.component: ''
ms.openlocfilehash: 76c8421286633dc3c81a073423a7d9f9ca1e1d85
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50420855"
---
# <a name="service-connectivity-monitor"></a>Hizmet Bağlantısı İzleyicisi

Hizmet Bağlantı İzleyicisi özelliği kullanabileceğiniz [Ağ Performansı İzleyicisi](log-analytics-network-performance-monitor.md) açık bir TCP bağlantı noktasına sahip herhangi bir uç noktası için ağ bağlanabilirliğini izlemek için. Bu uç noktaları, Web siteleri, SaaS uygulamaları, PaaS uygulamalarının ve SQL veritabanlarını içerir. 

Hizmet Bağlantı İzleyicisi ile aşağıdaki işlevleri gerçekleştirebilirsiniz: 

- Uygulamalar ve Ağ Hizmetleri Ağ bağlantısını birden çok şube ofisleri veya konumları izleyin. Office 365, Dynamics CRM, iç iş kolu satır uygulama ve SQL veritabanları, uygulamalar ve ağ hizmetlerini içerir.
- Office 365 ve Dynamics 365 uç noktalarına ağ bağlanabilirliğini izlemek için yerleşik testleri kullanın. 
- Yanıt süresi, ağ gecikme süresi ve paket kaybı deneyimli uç noktasına bağlanırken belirleyin.
- Kötü uygulama performansı nedeniyle ağ veya uygulama sağlayıcısının son bazı sorunu nedeniyle olup olmadığını belirler.
- Ayırma-birleştirme ağ üzerinde yer alan her bir topoloji Haritası durakta tarafından katkıda bulunulan gecikme görüntüleyerek kötü uygulama performansı neden olabilecek belirleyin.


![Hizmet Bağlantısı İzleyicisi](media/log-analytics-network-performance-monitor-service-endpoint/service-endpoint-intro.png)


## <a name="configuration"></a>Yapılandırma 
Yapılandırma için Ağ Performansı İzleyicisi'ni açmak için açık [Ağ Performansı İzleyicisi çözüm](log-analytics-network-performance-monitor.md) seçip **yapılandırma**.

![Ağ Performansı İzleyicisi'ni yapılandırma](media/log-analytics-network-performance-monitor-service-endpoint/npm-configure-button.png)


### <a name="configure-log-analytics-agents-for-monitoring"></a>İzleme için log Analytics aracılarını yapılandırma
Çözüm düğümlerinizi topolojisinden hizmet uç noktası keşfedebilmesi için izlemek için kullanılan düğümlerde aşağıdaki güvenlik duvarı kuralları etkinleştirin: 

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow 
```

### <a name="create-service-connectivity-monitor-tests"></a>Hizmet Bağlantı İzleyicisi sınamaları oluşturun 

Hizmet uç noktalarına ağ bağlanabilirliğini izlemek için testleri oluşturmaya başlayın.

1. Seçin **Hizmet Bağlantı İzleyicisi** sekmesi.
2. Seçin **Test Ekle**, test adı ve açıklama girin. 
3. Testi türünü seçin:<br>

    * Seçin **Web** outlook.office365.com veya bing.com gibi HTTP/S istekleri, yanıt veren bir hizmet bağlantısı izlemek için.<br>
    * Seçin **ağ** TCP isteklerine yanıt verir, ancak SQL server, FTP sunucusu veya SSH bağlantı noktası gibi HTTP/S isteklerine yanıt vermiyor hizmetine bağlantıyı izlemek için. 
4. Topoloji Bulma, ağ gecikme süresi ve paket kaybı gibi ağ ölçümlerini gerçekleştirmek istemiyorsanız Temizle **ağ ölçümlerini gerçekleştirmek** onay kutusu. En fazla özellikten faydalanmak için seçili devam eder. 
5. İçinde **hedef**, ağ bağlantılarını izlemek istediğiniz URL/FQDN/IP adresini girin.
6. İçinde **bağlantı noktası numarası**, hedef hizmet bağlantı noktası numarasını girin. 
7. İçinde **Test sıklığı**, testin çalışmasını istediğiniz için ne sıklıkla bir değer girin. 
8. Hizmet için ağ bağlantılarını izlemek istediğiniz düğümleri seçin. 

    >[!NOTE]
    > Windows server tabanlı düğümler için özelliği istekler TCP tabanlı ağ ölçümlerini gerçekleştirmek için kullanır. Windows istemcisi tabanlı düğümler için ağ ölçümlerini gerçekleştirmek için ICMP tabanlı istekleri özelliği kullanır. Windows istemcisi tabanlı düğümler olduğunda bazı durumlarda, hedef uygulama gelen ICMP tabanlı istekleri engeller. Ağ ölçümlerini gerçekleştirmek çözüm silemiyor. Böyle durumlarda, Windows server tabanlı düğümleri kullanmanızı öneririz. 

9. Öğeler için sistem durumu olayları oluşturmak istemiyorsanız, Temizle seçtiğiniz **hedefler sistem durumu izlemeyi etkinleştir, bu test tarafından kapsanan**. 
10. İzleme koşulları seçin. İçin sistem durumu olayı oluşturma eşiği değerler girerek, özel eşikler ayarlayabilirsiniz. Seçilen ağ veya alt ağ çifti için seçilen eşiğin üzerinde koşul değeri gider her sistem durumu olayı oluşturulur. 
11. Seçin **Kaydet** yapılandırmayı kaydetmek için. 

    ![Hizmet Bağlantı İzleyicisi test yapılandırmaları](media/log-analytics-network-performance-monitor-service-endpoint/service-endpoint-configuration.png)



## <a name="walkthrough"></a>Kılavuz 

Ağ Performansı İzleyicisi Pano görünümüne gidin. Sistem, oluşturduğunuz farklı testlerin bir özetini almak için bakmak **Hizmet Bağlantı İzleyicisi** sayfası. 

![Hizmet bağlantı İzlenecekler sayfası](media/log-analytics-network-performance-monitor-service-endpoint/service-endpoint-blade.png)

Testin ayrıntılarını görüntülemek için kutucuğu seçin **testleri** sayfası. Sol taraftaki tabloda, zaman içinde nokta sistem durumu ve hizmet yanıt süresi, ağ gecikme süresi ve paket kaybı tüm testlerin değerini görüntüleyebilirsiniz. Ağ durumu Kaydedici denetimi ağ anlık görüntü daha önce başka bir zaman görüntülemek için kullanın. Test içinde araştırmak istediğiniz tabloyu seçin. Sağ taraftaki bölmede grafiklerde geçmiş eğilimini ve hataların kaybı, gecikme süresi ve yanıt süresi değerleri görüntüleyebilirsiniz. Seçin **Test ayrıntıları** her düğümden performansını görüntülemek için bağlantı.

![Hizmet Bağlantı İzleyicisi sınamaları](media/log-analytics-network-performance-monitor-service-endpoint/service-endpoint-tests.png)

İçinde **Test düğümleri** görünüm, ağ bağlantısı her düğümden gözlemleyin. Performans düşüşü olan düğümü seçin. Uygulama yavaş çalışıyor olması için burada gözlemlenen düğüm budur.

Kötü uygulama performansı ağ veya uygulama sağlayıcısının son bir sorun nedeniyle uygulama yanıt süresini ve ağ gecikme süresi arasındaki bağıntıyı gözlemleyerek olup olmadığını belirler. 

* **Uygulama sorunu:** söylüyor yanıt süresi bir ani değişiklik ancak ağ gecikme süresi, tutarlılık ağın düzgün çalıştığından ve sorun uygulama ucundaki ilgili bir sorun nedeniyle olabilir. 

    ![Hizmet Bağlantı İzleyicisi uygulama sorunu](media/log-analytics-network-performance-monitor-service-endpoint/service-endpoint-application-issue.png)

* **Ağ sorun:** yanıt süresinde ağ gecikme süresine karşılık gelen bir depo ile birlikte bir ani artış yanıt süresi, ağ gecikme süresi arasında bir artış nedeniyle olabilir önerir. 

    ![Hizmet Bağlantı İzleyicisi ağ sorunu](media/log-analytics-network-performance-monitor-service-endpoint/service-endpoint-network-issue.png)

Sorunu nedeniyle ağ olduğunu belirledikten sonra seçin **topolojisi** topoloji haritasında sorunlu atlama tanımlamak için Görünüm bağlantısı. Aşağıdaki görüntüde bir örnek gösterilir. 105-ms toplam gecikme dışında düğümü ile uygulama uç noktası arasındaki 96 ms kırmızı işaretli atlama nedeniyle olur. Sorunlu atlama tanımladıktan sonra düzeltici eylemi gerçekleştirebilir. 

![Hizmet Bağlantı İzleyicisi sınamaları](media/log-analytics-network-performance-monitor-service-endpoint/service-endpoint-topology.png)

## <a name="diagnostics"></a>Tanılama 

Bir abnormality gözlemlerseniz, şu adımları izleyin:

* Hizmet yanıt süresi, Ağ kaybı ve gecikme süresi kullanılamaz olarak gösteriliyorsa, en az aşağıdakilerden biri neden olmuş olabilir:

    - Uygulama çalışmıyor.
    - Hizmet için ağ bağlantısını denetlemek için kullanılan düğümü çalışmıyor.
    - Testi Yapılandırması'nda girilen hedef doğru değil.
    - Düğüm, herhangi bir ağ bağlantısı yok.

* Geçerli hizmet yanıt süresi gösterilir, ancak gecikme süresi yanı sıra ağ kaybı kullanılamaz olarak gösterilen, aşağıdaki nedenlerden biri veya nedeni olabilir:

    - Bir Windows istemci makine hizmete ağ bağlantısını denetlemek için kullanılan düğümün ise hedef hizmet ICMP istekleri engellediğinde veya bir ağ güvenlik duvarı düğümden kaynaklanan ICMP isteği engelliyor.
    - **Ağ ölçümlerini gerçekleştirmek** testi Yapılandırması'nda onay kutusu boştur. 

* DI hizmet yanıt süresi, ancak gecikme süresi yanı sıra ağ kaybı geçerli, hedef hizmeti bir web uygulaması olmayabilir. Test yapılandırmasını düzenleyin ve test türü olarak seçin **ağ** yerine **Web**. 

* Uygulamayı yavaş çalışıyorsa, kötü uygulama performansı ağ veya uygulama sağlayıcısının son bir sorun nedeniyle olup olmadığını belirler.


## <a name="next-steps"></a>Sonraki adımlar
[Arama günlüklerini](log-analytics-log-searches.md) ayrıntılı ağ performansı verileri kayıtları görüntülemek için.
