---
title: Azure Standard Load Balancer tanılama | Microsoft Docs
description: Kullanılabilir Ölçümler ve sistem durumu bilgileri tanılama için Azure Standard Load Balancer için kullanın.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/9/2018
ms.author: Kumud
ms.openlocfilehash: 0711b52b76a22e32d05f27e6aae6c981bd2c148a
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902612"
---
# <a name="metrics-and-health-diagnostics-for-standard-load-balancer"></a>Standard Load Balancer ölçümleri ve sistem durumu tanılama

Azure Standard Load Balancer Azure Standard Load Balancer verir kaynaklarınızı aşağıdaki tanılama özellikleri sunar:
* **Çok boyutlu ölçümler**: üzerinden çok boyutlu yeni tanılama özellikleri sağlar [Azure İzleyici](https://docs.microsoft.com/azure/azure-monitor/overview) hem genel hem de iç için yük dengeleyici yapılandırmalarının. İzleme, yönetme ve sorun giderme, yük dengeleyici kaynakları.

* **Kaynak durumu**: Azure portalında yük dengeleyici sayfası ve kaynak durumu sayfası (izleme altında) standart Load Balancer'ın herkese açık yük dengeleyici yapılandırması için kaynak durumu bölümünde kullanıma sunar.

Bu makalede bu özelliklerin hızlı bir tura sağlar ve standart Load Balancer için kullanılacak yol sunar.

## <a name = "MultiDimensionalMetrics"></a>Çok boyutlu ölçümleri

Azure Load Balancer'ı Azure Portal'daki yeni Azure ölçümler (Önizleme) aracılığıyla yeni bir çok boyutlu ölçümler sağlar ve gerçek zamanlı tanılama Öngörüler yük dengeleyici kaynakları almanıza yardımcı olur. 

Çeşitli Standard Load Balancer yapılandırmaları aşağıdaki ölçümler sağlar:

| Ölçüm | Kaynak türü | Açıklama | Önerilen toplama |
| --- | --- | --- | --- |
| VIP kullanılabilirlik (veri yolu kullanılabilirlik) | Herkese açık yük dengeleyici | Standart Load Balancer veri yolundan bir bölgede yük dengeleyici ön ucuna, tüm sanal makinenizin destekler SDN yığınını için sürekli olarak uygular. Sağlıklı örnekleri kaldığı sürece, ölçüm olarak uygulamanızın yük dengeli trafik aynı yolu izler. Müşterilerinizin kullandığı veri yolu ayrıca doğrulanır. Ölçüm uygulamanıza görünmez ve diğer işlemlerle etkilemez.| Ortalama |
| DIP kullanılabilirlik (sistem durumu araştırma durumu) |  Genel ve iç yük dengeleyici | Standart Load Balancer, uygulama uç noktasının yapılandırma ayarlarınıza göre izler bir dağıtılmış sistem durumu yoklaması hizmeti kullanır. Bu ölçüm, bir toplama veya uç nokta başına sağlar filtrelenmiş görünüm load balancer Havuzu'ndaki her örneğinin uç noktası. Load Balancer, uygulama durumunu nasıl görüntülediğine, sistem durumu araştırması yapılandırması tarafından belirtildiği şekilde görebilirsiniz. |  Ortalama |
| SYN (eşitleme) paketleri |  Herkese açık yük dengeleyici | Standart Load Balancer İletim Denetimi Protokolü (TCP) bağlantılarını sonlandırmak veya TCP veya UDP paket akışları ile etkileşim desteklemez. Akışlar ve bunların el sıkışmaları her zaman kaynağı ile sanal makine örneği arasındadır. TCP protokolü senaryolarınızı daha iyi gidermek için SYN kullanmak yapabileceğiniz kaç TCP bağlantısı anlamak için paket sayaçları denemesi yapıldı. Ölçüm alınan TCP SYN paketlerin sayısını raporlar.| Ortalama |
| SNAT bağlantıları |  Herkese açık yük dengeleyici |Standart yük dengeleyici genel IP adresi ön ucu verdiğinizi giden akışlar sayısını raporlar. Kaynak ağ adresi çevirisi (SNAT) bağlantı noktaları exhaustible bir kaynaktır. Bu ölçüm, ne kadar yoğun olarak uygulamanız üzerinde SNAT giden kaynaklı akışlar için güvenmektedir bir göstergesini verebilirsiniz. Başarılı ve başarısız giden SNAT akışlar için sayaçları bildirilir ve sorun giderme ve giden akış durumunu anlamak için kullanılabilir.| Ortalama |
| Bayt sayaçları |  Genel ve iç yük dengeleyici | Standart yük dengeleyici ön uç işlenen veri bildirir.| Ortalama |
| Paket sayaçları |  Genel ve iç yük dengeleyici | Standart yük dengeleyici ön uç işlenen paket bildirir.| Ortalama |

### <a name="view-your-load-balancer-metrics-in-the-azure-portal"></a>Azure portalında yük dengeleyici ölçümlerinizi görüntüleyin

Azure Portalı aracılığıyla her iki yük dengeleyici kaynak sayfasında belirli bir kaynak için ve Azure İzleyici sayfasında kullanılabilir ölçümler (Önizleme) sayfanın load balancer ölçümleri sunar. 

Standard Load Balancer kaynaklarınız için ölçümleri görüntülemek için:
1. Ölçümler (Önizleme) sayfasına gidin ve aşağıdakilerden birini yapın:
   * Yük Dengeleyici kaynak sayfasında ölçüm türü açılan listesinde seçin.
   * Azure İzleyici sayfasında, yük dengeleyici kaynağını seçin.
2. Uygun toplama türünü ayarlayın.
3. İsteğe bağlı olarak, gerekli filtreleme ve gruplandırma yapılandırın.

![Standard Load Balancer ölçümleri Önizleme](./media/load-balancer-standard-diagnostics/LBMetrics1.png)

*Şekil: Standard Load Balancer için kullanılabilirlik ve sistem durumu araştırma durumu ölçümü DIP*

### <a name="retrieve-multi-dimensional-metrics-programmatically-via-apis"></a>API'ler aracılığıyla programlama çok boyutlu ölçümleri alma

API Kılavuzu çok boyutlu ölçüm tanımlarını ve değerleri almak için bkz. [Azure izleme REST API Kılavuzu](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough#retrieve-metric-definitions-multi-dimensional-api).

### <a name = "DiagnosticScenarios"></a>Tanılama senaryoları ve önerilen görünümleri

#### <a name="is-the-data-path-up-and-available-for-my-load-balancer-vip"></a>Benim için yük dengeleyici VIP veri yolu yukarı ve kullanılabilir mi?

VIP kullanılabilirlik ölçümü Vm'lerinizi bulunduğu bölgede işlem ana veri yolu durumunu açıklar. Sistem durumu Azure altyapısının bir yansıma unsurdur. Ölçüm için kullanabilirsiniz:
- Dış hizmetinizin kullanılabilirliğini izleyin
- Derinlemesine ve hizmetinizin dağıtıldığı platform sağlıklı olup veya konuk işletim sistemi veya uygulama örneği sağlıklı olup anlayın.
- Bir olay, hizmet veya temel alınan veri düzlemi ilgili kaynaklandığını ayırmak. Bu ölçüm sistem durumu araştırması durum ("DIP kullanılabilirlik") ile karıştırmayın.

Standard Load Balancer kaynaklarınız için VIP kullanılabilirlik almak için:
1. Doğru yük dengeleyici kaynağını seçili olduğundan emin olun. 
2. İçinde **ölçüm** aşağı açılan listesinden **VIP kullanılabilirlik**. 
3. İçinde **toplama** aşağı açılan listesinden **ortalama**. 
4. Ayrıca, VIP adresi veya VIP bağlantı noktası gerekli ön uç IP adresi ile boyut olarak veya ön uç bağlantı noktası üzerinde bir filtre ekleyin ve sonra bunları seçili boyutuna göre gruplayın.

![Yoklama VIP](./media/load-balancer-standard-diagnostics/LBMetrics-VIPProbing.png)

*Şekil: Yük dengeleyici VIP'sine algılama ayrıntıları*

Ölçüm etkin, bant dışı bir ölçüye göre oluşturulur. Bölge içinde bir yoklama hizmeti, ölçüm için trafiğin kaynaklandığı. Hizmet genel ön uç ile bir dağıtımını oluşturun ve ön uç kaldırana kadar devam sürede etkin hale gelir. 

>[!NOTE]
>İç ön uçlar, şu anda desteklenmiyor. 

Dağıtımınızın ön uç ve kural eşleşen bir paket düzenli olarak oluşturulur. Bu bölge kaynaktan konağa arka uç havuzundaki bir VM'nin bulunduğu erişir. Diğer tüm trafik için yaptığı gibi yük dengeleyici altyapı aynı Yük Dengeleme ve çeviri işlemleri gerçekleştirir. Bu araştırma bant dışı yük dengeli uç nokta üzerinde. Arka uç havuzunda iyi durumda VM bulunduğu, işlem konak üzerinde araştırma ulaştıktan sonra işlem konak araştırma hizmetine bir yanıt oluşturur. Bu trafik, sanal Makinenizin görmez.

VIP kullanılabilirlik aşağıdaki nedenlerle başarısız olur:
- Dağıtımınız, arka uç havuzunda kalan yok durumu iyi olan VM'lerin sahiptir. 
- Altyapı kesinti oluştu.

Tanılama amacıyla kullanabileceğiniz [sistem durumu araştırması durumuyla birlikte VIP kullanılabilirlik ölçümü](#vipavailabilityandhealthprobes).

Kullanım **ortalama** çoğu senaryo için toplama olarak.

#### <a name="are-the-back-end-instances-for-my-vip-responding-to-probes"></a>My VIP için arka uç örneklerinin yoklamalara yanıt vermeye misiniz?

Sistem durumu araştırma durumu ölçümü, yük dengeleyici durum araştırması yapılandırıldığında, sizin tarafınızdan yapılandırılan uygulama dağıtımınızın durumunu açıklar. Yük Dengeleyici durum araştırması durumunu yeni akışlar gönderileceği belirlemek için kullanır. Sistem durumu araştırmalarının kaynağı olan bir Azure altyapı adresinden ve VM konuk işletim sistemi içinde görülebilir.

Standard Load Balancer kaynaklarınız için DIP kullanılabilirlik almak için:
1. Seçin **DIP kullanılabilirlik** ölçüm ile **ortalama** toplama türü. 
2. Gerekli VIP IP adres veya bağlantı noktası (veya her ikisi de) bir filtre uygulayacaksınız.

![DIP kullanılabilirlik](./media/load-balancer-standard-diagnostics/LBMetrics-DIPAvailability.png)

*Şekil: Yük dengeleyici VIP kullanılabilirlik*

Sistem durumu araştırmaları, aşağıdaki nedenlerden dolayı başarısız:
- Durum araştırması dinlemiyor veya yanıt vermeyen bir bağlantı noktası yapılandırın veya yanlış protokolünü kullanır. Hizmetinizi doğrudan sunucu döndürmeyi (DSR veya kayan IP) kullanıyorsa, kuralları, NIC'ın IP yapılandırmasının IP adresinde dinleyen bir hizmet emin olun ve üzerinde yalnızca ön uç IP adresiyle yapılandırılmış geri döngü adresi.
- Araştırma ağ güvenlik grubu, sanal makinenin konuk işletim sistemi güvenlik duvarı veya uygulama katmanı filtreler tarafından izin verilmiyor.

Kullanım **ortalama** çoğu senaryo için toplama olarak.

#### <a name="how-do-i-check-my-outbound-connection-statistics"></a>My giden bağlantı istatistikleri nasıl kontrol edebilirim? 

Başarılı ve başarısız bağlantılar için hacmi SNAT bağlantıları ölçüm açıklar [giden akışlar](https://aka.ms/lboutbound).

Başarısız bağlantılar hacmi sıfırdan büyük SNAT bağlantı noktası tükenmesi gösterir. Bu hatalar neden olabileceğini belirlemek için daha fazla araştırmanız gerekir. Bağlantı noktası tükenmesi SNAT bildirimleri oluşturmak için bir hata olarak bir [giden akış](https://aka.ms/lboutbound). Giden bağlantılar senaryoları ve iş mekanizmaları anlamak için nasıl azaltılacağını öğrenmek ve SNAT bağlantı noktası tükenmesi önlemeye yönelik tasarım ile ilgili makaleyi gözden geçirin. 

SNAT bağlantı istatistiklerini almak için:
1. Seçin **SNAT bağlantıları** ölçüm türü ve **toplam** toplama olarak. 
2. Gruplandırma ölçütü **bağlantı durumu** farklı çizgilerle gösterilir başarılı ve başarısız SNAT bağlantı sayısı için. 

![SNAT bağlantı](./media/load-balancer-standard-diagnostics/LBMetrics-SNATConnection.png)

*Şekil: Yük dengeleyici SNAT bağlantısı sayısı*


#### <a name="how-do-i-check-inboundoutbound-connection-attempts-for-my-service"></a>Gelen/giden bağlantı girişimleri için Hizmetimi nasıl denetlerim?

Birimin gelmiş veya gönderilen TCP SYN paketlerin SYN paketleri ölçüm açıklar (için [giden akışlar](https://aka.ms/lboutbound)) belirli bir ön uç ile ilişkili olan. Bu ölçüm, TCP bağlantı girişimleri hizmetinize anlamak için kullanabilirsiniz.

Kullanım **toplam** çoğu senaryo için toplama olarak.

![SYN bağlantı](./media/load-balancer-standard-diagnostics/LBMetrics-SYNCount.png)

*Şekil: Yük dengeleyici SYN sayısı*


#### <a name="how-do-i-check-my-network-bandwidth-consumption"></a>My ağ bant genişliği kullanımını nasıl kontrol edebilirim? 

Bayt ve paket sayaçları ölçüm bayt ve gönderilen veya alınan her-front-end temelinde hizmetinizi tarafından paket hacmine açıklar.

Kullanım **toplam** çoğu senaryo için toplama olarak.

Bayt veya paket sayısı istatistiklerini almak için:
1. Seçin **bayt sayısını** ve/veya **paket sayısı** ölçüm türü ile **ortalama** toplama olarak. 
2. Aşağıdakilerden birini yapın:
   * Belirli bir ön uç IP, ön uç bağlantı noktasını, arka uç IP veya arka uç bağlantı noktası üzerinde bir filtre uygulayacaksınız.
   * Herhangi bir filtreleme olmadan bir yük dengeleyici kaynak genel istatistiklerini Al

![Bayt sayısı](./media/load-balancer-standard-diagnostics/LBMetrics-ByteCount.png)

*Şekil: Yük dengeleyici bayt sayısı*

#### <a name = "vipavailabilityandhealthprobes"></a>Yük Dengeleyici dağıtımım nasıl Tanılama?

Tek bir grafikte VIP kullanılabilirlik ve sistem durumu araştırması ölçümleri bir bileşimini kullanarak sorun için arama ve sorunu çözmek nereye tanımlayabilirsiniz. Azure'nın düzgün çalıştığını güvence elde edin ve yaratacağı yapılandırma veya uygulamanın kök neden olduğunu belirlemek için bu bilgileri kullanın.

Azure'nın sağladığınız yapılandırma göre dağıtımınızın durumunu nasıl görüntülediğine anlamak için sistem durumu araştırması ölçümleri kullanabilirsiniz. Sistem durumu araştırmaları sırasında isteyen her zaman izleme veya bir neden belirleyen bir harika ilk adımdır.

Bir adım daha atın ve Azure'nın belirli bir dağıtım için sorumlu olduğu temel alınan veri düzlemine durumunu nasıl görüntülediğine konusunda fikir sahibi olun için VIP kullanılabilirlik ölçümlerini kullanın. Her iki ölçüm birleştirdiğinizde, bu örnekte gösterildiği gibi hata nerede olabilir, ayırabilirsiniz:

![VIP tanılama](./media/load-balancer-standard-diagnostics/LBMetrics-DIPnVIPAvailability.png)

*Şekil: Birleştirme DIP ve VIP kullanılabilirlik ölçümlerini*

Grafik, aşağıdaki bilgileri görüntüler:
- Altyapı sağlıklı sanal makinelerinizin barındırma altyapısını erişilebilir ve arka uçta birden çok VM'in yerleştirildiği. Bu bilgileri yüzde 100 VIP kullanılabilirlik mavi izleme tarafından belirtilir. 
- Ancak, sistem durumu araştırma durumu (DIP kullanılabilirlik) grafik başına yüzde 0 turuncu izleme tarafından belirtildiği şekilde altındadır. Yeşil vurgular (DIP kullanılabilirlik) durumu sağlıklı, ve hangi müşterinin dağıtım noktasının nerede dönüştü daire içinde alanında yeni akışlar kabul edemiyor.

Grafik, müşterilerin kendi dağıtım sorunlarını giderme tahmin veya diğer sorunları ortaya çıkan destek sormak zorunda kalmadan olanak tanır. Sistem durumu araştırmaları, yanlış yapılandırma veya bir uygulamayı nedeniyle başarısız olduğu için hizmet kullanılamıyor.

### <a name = "Limitations"></a>Sınırlamaları

VIP kullanılabilirlik şu anda yalnızca genel ön uçları için kullanılabilir.

## <a name = "ResourceHealth"></a>Kaynak sistem durumu

Standard Load Balancer kaynaklar için sistem durumu mevcut aracılığıyla sunulan **kaynak durumu** altında **İzleyici > Hizmet durumu**.

>[!NOTE]
>Yük Dengeleyici için kaynak sistem durumu şu anda standart yük dengeleyici yapılandırması yalnızca genel için kullanılabilir. Kaynak durumu, iç yük dengeleyici veya temel SKU'ları, yük dengeleyici kaynaklar sunmaz.

Genel bir Standard Load Balancer kaynaklarınızın durumunu görüntülemek için:
1. Seçin **İzleyici** > **hizmet durumunu**.

   ![İzlenecekler sayfası](./media/load-balancer-standard-diagnostics/LBHealth1.png)

   *Şekil: Azure İzleyicisi hizmet durumu bağlantısı*

2. Seçin **kaynak durumu**ve ardından emin olun **abonelik kimliği** ve **kaynak türünün yük dengeleyici =** seçilir.

   ![Kaynak durumu](./media/load-balancer-standard-diagnostics/LBHealth3.png)

   *Şekil: sistem durumu görüntüleme için kaynağı seçin*

3. Listede, geçmiş sistem durumunu görüntülemek için yük dengeleyici kaynağı seçin.

    ![Load Balancer sistem durumu](./media/load-balancer-standard-diagnostics/LBHealth4.png)

   *Şekil: Yük dengeleyici kaynak durumu görünümünden*
 
Aşağıdaki tabloda çeşitli kaynak sistem durumları ve açıklamalarının listelenmiştir: 

| Kaynak durumu | Açıklama |
| --- | --- |
| Kullanılabilir | Genel standart yük dengeleyici kaynağınızı sağlıklı ve kullanılabilir. |
| Kullanılamaz | Genel standart yük dengeleyici kaynağınızı iyi durumda değil. Seçerek sağlığını tanılamanız **Azure İzleyici** > **ölçümleri**.<br>(*Kullanılamıyor* durumu anlamına da kaynak genel standard load balancer'ınız ile bağlı değil.) |
| Bilinmeyen | Kaynak sistem durumu, genel standart yük dengeleyici kaynağı için henüz güncelleştirilmemiş.<br>(*Bilinmeyen* durumu anlamına da kaynak genel standard load balancer'ınız ile bağlı değil.)  |

## <a name="next-steps"></a>Sonraki adımlar

- [Standart Yük Dengeleyici](load-balancer-standard-overview.md) hakkında daha fazla bilgi edinin.
- Daha fazla bilgi edinin, [yük dengeleyici giden bağlantı](https://aka.ms/lboutbound).
- Hakkında bilgi edinin [Azure İzleyici](https://docs.microsoft.com/azure/azure-monitor/overview).
- Hakkında bilgi edinin [Azure İzleyici ölçümleri REST API](https://docs.microsoft.com/rest/api/monitor/metrics/).


