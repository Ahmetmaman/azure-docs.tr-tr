---
title: Log Analytics'te ağ izleme hakkında | Microsoft Docs
description: Ağ izleme çözümleri, NPM, bulut, şirket içi ve karma ortamlar genelinde ağlarını yönetmek için de dahil olmak üzere genel bakış.
services: monitoring-and-diagnostics
documentationcenter: na
author: agummadi
manager: ''
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2018
ms.author: ajaycode
ms.openlocfilehash: 0656cfcc2dcded284be1a337f797681117f3b313
ms.sourcegitcommit: 974c478174f14f8e4361a1af6656e9362a30f515
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "42062128"
---
# <a name="network-monitoring-solutions"></a>Ağ izleme çözümleri 

Azure ağ varlıklarınızı izlemek için bir konak çözümleri sunar. Azure, çözümler ve ağ bağlantısı, ExpressRoute devreleri durumunu izlemek ve bulut ağ trafiği çözümleme için yardımcı programlar sahiptir.

## <a name="network-performance-monitor-npm"></a>Ağ Performansı İzleyicisi'ni (NPM)

Ağ Performansı İzleyicisi'ni (NPM), her biri ağ bağlantısı ve uygulamalarınıza ağınızın sistem izleme doğru olarak ve ağ performansı sağlar özelliklerine paketidir. NPM, bulut tabanlı ve bir hibrit ağ izleme arasındaki bağlantıyı izleyen çözümü sağlar:
 
* Bulut dağıtımları ve şirket içi konumu
* Birden çok veri merkezi ve şube ofisleri
* Görev açısından kritik çok katmanlı uygulamalar/mikro hizmetler
* Kullanıcı konumlarında ve web tabanlı uygulamalara (HTTP/HTTPs) 

Performans İzleyicisi, ExpressRoute İzleyicisi ve hizmet bağlantı İzleyicisi NPM kullanılabilmesine izleme ve aşağıda açıklanmıştır.

## <a name="performance-monitor"></a>Performans İzleyicisi

Performans İzleyicisi NPM bir parçasıdır ve bulut, karma ve şirket içi ortamlar için ağ izleme. Uzak dalı ve saha ofisleri, mağaza konumları, veri merkezleri ve Bulutlar ağ bağlantısı izleyebilirsiniz. Ağ sorunlarını, kullanıcılarınız şikayet etmeden önce algılayabilir. Önemli avantajlar şunlardır:

* Çeşitli alt ağlar genelinde kaybı ve gecikmeyi izleyin ve uyarılar koyun
* Ağdaki tüm yolları (yedekli yolları dahil) izleme
* Çoğaltması zor olan geçici ve zaman içinde nokta ağ sorunlarını giderme
* Ağdaki performansın düşmesine neden olan segmenti belirleme
* Ağın sistem durumunu SNMP'ye gerek kalmadan izleyin

![NPM topoloji Haritası](./media/network-monitoring-overview/npm-topology-map.png) 

Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Log Analytics'te Ağ Performansı İzleyicisi çözümü yapılandırma](../log-analytics/log-analytics-network-performance-monitor.md) 
* [Kullanım örnekleri](https://blogs.technet.microsoft.com/msoms/2016/08/30/monitor-on-premises-cloud-iaas-and-hybrid-networks-using-oms-network-performance-monitor/)
*  Ürün güncelleştirmeleri: [Şubat 2017](https://blogs.technet.microsoft.com/msoms/2017/02/27/oms-network-performance-monitor-is-now-generally-available/), [Ağustos 2017](https://blogs.technet.microsoft.com/msoms/2017/08/14/improvements-to-oms-network-performance-monitor/)

## <a name="expressroute-monitor"></a>ExpressRoute İzleyicisi

ExpressRoute için NPM izleme Azure özel eşleme ve Microsoft eşleme bağlantıları için kapsamlı ExpressRoute sunar. ExpressRoute üzerinden E2E bağlantıyı ve performansı şubeleriniz ve Azure arasında izleyebilirsiniz. Temel işlevler şunlardır:

* Aboneliğinizle ilişkili ER devreler otomatik algılama
* Ağ topolojisini şirket içi ve bulut uygulamalarınıza algılanması
* Kapasite planlaması, bant genişliği kullanımı analizi
* İzleme ve uyarı birincil ve ikincil yollarında
* Azure'a bağlantısı izleme gibi Office 365, Dynamics 365... ExpressRoute üzerinden Hizmetleri
* Sanal ağlar için bağlantı performansında algılayın

![Bölgeler arasında coğrafi harita gösteren trafiği](./media/network-monitoring-overview/expressroute-topology-map.png) 

Daha fazla bilgi için aşağıdaki makalelere bakın:

* [ExpressRoute için Ağ Performansı İzleyicisini Yapılandırma](../expressroute/how-to-npm.md)
* [Blog gönderisi](https://aka.ms/NPMExRmonitorGA)

## <a name="service-connectivity-monitor"></a>Hizmet Bağlantısı İzleyicisi

Hizmet bağlantı izleme ile artık uygulamaları erişilebilirliğini test ve şirket içi, taşıyıcı ağlar ve bulut/özel veri merkezleri arasında performans sorunlarını algılama.

* Uygulamalar için uçtan uca ağ bağlantılarını izlemek
* Ağ performansı ile uygulama teslimini ilişkilendirin, yol kullanıcı ve uygulama arasındaki performans düşüşü kesin konumunu algılayın
* Dünyanın dört bir yanındaki birden fazla kullanıcı konumdan uygulama erişimi test etme
* İş ve SaaS uygulamalarına kolunuz için ağ gecikme süresi ve paket kaybı belirleyin
* Kötü uygulama performansı neden olabilecek ağ üzerindeki etkin noktaları belirleme
* Microsoft Office 365, Dynamics 365, Skype Kurumsal için yerleşik testleri ve diğer Microsoft hizmetlerini kullanarak Office 365 uygulamaları için erişimi izleme

Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Hizmet uç noktaları izlemek için Ağ Performansı İzleyicisi'ni yapılandırma](https://aka.ms/applicationconnectivitymonitorguide)
* [Blog gönderisi](https://aka.ms/svcendptmonitor)

## <a name="traffic-analytics"></a>Trafik Analizi
Trafik analizi, kullanıcı ve uygulama etkinlik görünürlüğü sağlayarak bulut ağlarınızda sağlayan bir bulut tabanlı bir çözümdür. Öngörüler sağlamak için NSG akış günlüklerini analiz edilir:

* Azure ve Internet, genel bulut bölgeleri, sanal ağlar ve alt ağlar arasında ağlar arasında trafik akışları
* Uygulamalar ve algılayıcılar veya ayrılmış akış Toplayıcı Gereçleri gerek kalmadan, ağ protokolleri
* Popüler konuşmacılar, sık iletişim kuran uygulamaları, bulutta, trafik etkin noktaları VM konuşmaları
* Kaynakları ve hedefleridir ağlarda arası ilişkileri kritik iş Hizmetleri ve uygulamaları arasındaki trafiği
* Güvenlik – kötü amaçlı trafik, bağlantı noktalarını Internet, uygulamaları veya İnternet'e erişmeye çalışan VM'ler için Aç...
* Kapasite kullanımı -, VPN ağ geçitleri ve diğer hizmetleri kullanım eğilimlerini izleyerek, fazladan sağlama veya Tıkanıklığı gibi sorunları ortadan kaldırmanıza yardımcı olur

Trafik Analizi ile kuruluşunuzun ağ etkinliği, güvenli uygulamaları ve verileri, Denetim iş yükü performansı iyileştirmek ve yardımcı uyumluluğu sürdürün olan eyleme dönüştürülebilir bilgiler donatır.

![Bölgeler arasında coğrafi harita gösteren trafiği](../network-watcher/media/traffic-analytics/geo-map-view-showcasing-traffic-distribution-to-countries-and-continents.png) 

İlgili bağlantılar:
* [Blog gönderisi](https://aka.ms/trafficanalytics), [belgeleri](https://aka.ms/trafficanalyticsdocs), [SSS](https://docs.microsoft.com/azure/network-watcher/traffic-analytics-faq)

## <a name="dns-analytics"></a>DNS Analizi
DNS yöneticileri için tasarlanan bu çözüm toplar, çözümler ve güvenlik işlemleri ve performans ile ilgili Öngörüler için DNS günlüklerini ilişkilendirir.  Bazı özellikleri şunlardır:

* Kötü amaçlı etki alanlarına çözümlemeye istemci kimliği
* Eski kaynak kayıtları kimliği
* Sık sık sorgulanan etki alanı adlarını ve DNS sık iletişim kuran istemcileri görünürlük
* DNS sunucularındaki istek yükünü görünürlük
* Dinamik DNS kayıt hatalarının izleme

![DNS analizi Panosu](./media/network-monitoring-overview/dns-analytics-overview.png) 

İlgili bağlantılar:
* [Blog gönderisi](https://blogs.technet.microsoft.com/msoms/2017/04/19/introducing-oms-dns-analytics/), [belgeleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-dns)

## <a name="miscellaneous"></a>Muhtelif Hükümler

* [Yeni fiyatlandırma](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor-pricing-faq)
