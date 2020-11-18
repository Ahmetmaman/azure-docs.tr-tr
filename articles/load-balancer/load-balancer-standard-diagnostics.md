---
title: Ölçümler, uyarılar ve kaynak durumu ile tanılama-Azure Standart Load Balancer
description: Azure Standart Load Balancer tanımak için kullanılabilir ölçümleri, uyarıları ve kaynak durumu bilgilerini kullanın.
services: load-balancer
documentationcenter: na
author: asudbring
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/14/2019
ms.author: allensu
ms.openlocfilehash: 9c322620e1d66182937be41bb02d48fd1469f459
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94697569"
---
# <a name="standard-load-balancer-diagnostics-with-metrics-alerts-and-resource-health"></a>Ölçümler, uyarılar ve kaynak durumu ile Standart Load Balancer

Azure Standart Load Balancer aşağıdaki tanılama yeteneklerini kullanıma sunar:

* **Çok boyutlu ölçümler ve uyarılar**: Standart yük dengeleyici yapılandırmalarına yönelik [Azure izleyici](../azure-monitor/overview.md) aracılığıyla çok boyutlu tanılama özellikleri sağlar. Standart yük dengeleyici kaynaklarınızı izleyebilir, yönetebilir ve sorun giderebilirsiniz.

* **Kaynak sistem durumu**: Load Balancer kaynak durumu durumu Monitor altındaki kaynak durumu sayfasında bulunur. Bu otomatik denetim, Load Balancer kaynağınızın geçerli kullanılabilirliğini bildirir.

Bu makalede, bu yetenekler için hızlı bir tura yer verilmiştir ve bunları Standart Load Balancer için kullanmanın yolları sunulmaktadır. 

## <a name="multi-dimensional-metrics"></a><a name = "MultiDimensionalMetrics"></a>Çok boyutlu ölçümler

Azure Load Balancer, Azure portal Azure ölçümleri aracılığıyla çok boyutlu ölçümler sağlar ve yük dengeleyici kaynaklarınıza gerçek zamanlı tanılama öngörüleri almanıza yardımcı olur. 

Çeşitli Standart Load Balancer yapılandırmalarında aşağıdaki ölçümler sağlanır:

| Ölçüm | Kaynak türü | Description | Önerilen toplama |
| --- | --- | --- | --- |
| Veri yolu kullanılabilirliği | Genel ve iç yük dengeleyici | Standart Load Balancer bir bölgedeki veri yolunu sürekli olarak yük dengeleyicinin ön ucuna (VM’nizi destekleyen SDN yığınına kadar) aktarır. Sağlıklı örnekler kaldığı sürece ölçüm, uygulamanızın yük dengeli trafiğiyle aynı yolu izler. Müşterilerinizin kullandığı veri yolu da doğrulanır. Ölçümler uygulamanızda görünmez ve diğer işlemleri engellemez.| Ortalama |
| Sistem durumu yoklama durumu | Genel ve iç yük dengeleyici | Standart Load Balancer, yapılandırma ayarlarınıza göre uygulama uç noktanızın sistem durumunu izleyen dağıtılmış bir sistem durumu algılama hizmeti kullanır. Bu ölçüm, yük dengeleyici havuzundaki her örnek uç noktası için toplu veya uç noktası başına filtrelenmiş bir görünüm sunar. Durum yoklaması yapılandırmanızın belirttiği gibi Load Balancer’ın uygulamanızın durumunu nasıl görüntülediğini görebilirsiniz. |  Ortalama |
| SYN (eşitleme) paketleri | Genel ve iç yük dengeleyici | Standart Load Balancer, İletim Denetimi Protokolü (TCP) bağlantılarını sonlandırmaz veya TCP ya da UDP paket akışlarıyla etkileşime geçmez. Akışlar ve el sıkışmaları her zaman kaynak ve VM örneği arasında gerçekleşir. TCP protokolü senaryolarınızdaki sorunları daha iyi şekilde gidermek amacıyla kaç tane TCP bağlantısı denemesinin yapıldığını öğrenmek için SYN paketi sayaçlarını kullanabilirsiniz. Ölçüm alınan TCP SYN paketi sayısını bildirir.| Ortalama |
| SNAT bağlantıları | Genel yük dengeleyici |Standart Load Balancer, farklı bir kimlikle Genel IP adresi uç noktasına aktarılan giden akış sayısını bildirir. Kaynak ağ adresi çevirisi (SNAT) bağlantı noktaları tükenebilen kaynaklardır. Bu ölçüm, uygulamanızın kaynak alınan giden akışlar için SNAT’ye ne kadar bağlı olduğunu belirtebilir. Başarılı ve başarısız giden SNAT akışlarına yönelik sayaçlar bildirilir. Bu sayaçlar, giden akışlarınızda sorun gidermek ve bunların durumunu anlamak için kullanılabilir.| Ortalama |
| Ayrılan SNAT bağlantı noktaları | Genel yük dengeleyici | Standart Load Balancer, arka uç örneği başına ayrılan SNAT bağlantı noktalarının sayısını raporlar | Ortalama. |
| Kullanılan SNAT bağlantı noktaları | Genel yük dengeleyici | Standart Load Balancer, arka uç örneği başına kullanılan SNAT bağlantı noktalarının sayısını raporlar. | Ortalama | 
| Bayt sayaçları |  Genel ve iç yük dengeleyici | Standart Load Balancer, her uç noktasının işlediği veri miktarını bildirir. Baytların arka uç örneklerinde eşit olarak dağıtılmadığını görebilirsiniz. Azure 'un Load Balancer algoritması akışlara dayalı olduğu için bu beklenmektedir | Ortalama |
| Paket sayaçları |  Genel ve iç yük dengeleyici | Standart Load Balancer, her ön ucun işlediği paket sayısını bildirir.| Ortalama |

  >[!NOTE]
  >Bir NVA veya güvenlik duvarı SYN paketi, bayt sayacı ve paket sayacı ölçümleri aracılığıyla iç yük dengeleyiciden trafik dağıtılması kullanılırken, sıfır olarak görünür. 
  
### <a name="view-your-load-balancer-metrics-in-the-azure-portal"></a>Yük dengeleyici ölçülerinizi Azure portal görüntüleyin

Azure portal, yük dengeleyici ölçümlerini, belirli bir kaynak için hem yük dengeleyici kaynak sayfasında hem de Azure Izleyici sayfasında bulunan ölçümler sayfası aracılığıyla kullanıma sunar. 

Standart Load Balancer kaynaklarınızın ölçümlerini görüntülemek için:
1. Ölçümler sayfasına gidin ve aşağıdakilerden birini yapın:
   * Yük dengeleyici kaynağı sayfasında, açılan listeden ölçüm türünü seçin.
   * Azure Izleyici sayfasında, yük dengeleyici kaynağını seçin.
2. Uygun ölçüm toplama türünü ayarlayın.
3. İsteğe bağlı olarak, gerekli filtreleme ve gruplandırmayı yapılandırın.
4. İsteğe bağlı olarak, zaman aralığını ve toplamayı yapılandırın. Varsayılan olarak saat UTC 'de görüntülenir.

  >[!NOTE] 
  >Veriler dakikada bir kez örneklendiği için belirli ölçümleri yorumlarken zaman toplama önemlidir. Eğer toplama işlemi beş dakikaya ayarlanırsa ve SNAT ayırma gibi ölçümler için ölçüm toplama türü toplamı kullanılırsa, grafiğiniz toplam ayrılan SNAT bağlantı noktalarından beş kat daha görüntülenir. 

![Standart Load Balancer ölçümleri](./media/load-balancer-standard-diagnostics/lbmetrics1anew.png)

*Şekil: Standart Load Balancer için veri yolu kullanılabilirliği ölçümü*

### <a name="retrieve-multi-dimensional-metrics-programmatically-via-apis"></a>API 'Ler aracılığıyla çok boyutlu ölçümleri program aracılığıyla alma

Çok boyutlu ölçüm tanımlarını ve değerlerini almaya yönelik API Kılavuzu için bkz. [Azure izleme REST API izlenecek yol](../azure-monitor/platform/rest-api-walkthrough.md#retrieve-metric-definitions-multi-dimensional-api). Bu ölçümler yalnızca ' tüm ölçümler ' seçeneği aracılığıyla bir depolama hesabına yazılabilir. 

### <a name="configure-alerts-for-multi-dimensional-metrics"></a>Çok boyutlu ölçümler için uyarıları yapılandırma ###

Azure Standart Load Balancer, çok boyutlu ölçümler için kolayca yapılandırılabilir uyarıları destekler. Bir Touchless kaynak izleme deneyimini güçlendiren şekilde değişen önem düzeyleri ile uyarıları tetiklemek için belirli ölçümler için özel eşikler yapılandırın.

Uyarıları yapılandırmak için:
1. Yük Dengeleyici için uyarı alt dikey penceresine git
1. Yeni uyarı kuralı oluşturma
    1.  Uyarı koşulunu yapılandırma
    1.  Seçim Otomatik onarım için eylem grubu Ekle
    1.  Sezgisel yeniden eyleme izin veren uyarı önem derecesi, ad ve açıklama atayın

  >[!NOTE]
  >Uyarı koşulu yapılandırma penceresinde, sinyal geçmişi için zaman serisi gösterilir. Bu zaman serisini, arka uç IP gibi boyutlara göre filtrelemeye yönelik bir seçenek vardır. Bu **, zaman** serisi grafiğini filtreleyip uyarının kendisini filtreleyecek. Belirli arka uç IP adresleri için uyarıları yapılandıramazsınız.

### <a name="common-diagnostic-scenarios-and-recommended-views"></a><a name = "DiagnosticScenarios"></a>Yaygın tanılama senaryoları ve önerilen görünümler

#### <a name="is-the-data-path-up-and-available-for-my-load-balancer-frontend"></a>Veri yolu, Load Balancer ön ucu için hazır ve kullanılabilir mi?
<details><summary>Genişlet</summary>

Veri yolu kullanılabilirliği ölçümü, sanal makinelerinizin bulunduğu işlem konağının bölge içindeki veri yolunun sistem durumunu açıklar. Ölçüm, Azure altyapısının sistem durumunun bir yansımasıyla aynıdır. Ölçüyü şu şekilde kullanabilirsiniz:
- Hizmetinizin dış kullanılabilirliğini izleyin
- Daha ayrıntılı bilgi edinin ve hizmetinizin dağıtıldığı platformun sağlıklı olup olmadığını veya Konuk işletim sistemi ya da uygulama örneğinizin sağlıklı olup olmadığını anlayın.
- Bir olayın hizmetinize mı yoksa temel alınan veri düzlemine mi ilgili olduğunu yalıtın. Bu ölçüyü durum araştırma durumu ("arka uç örneği kullanılabilirliği") ile karıştırmayın.

Standart Load Balancer kaynaklarınız için veri yolu kullanılabilirliğini almak için:
1. Doğru yük dengeleyici kaynağının seçildiğinden emin olun. 
2. **Ölçüm** açılan listesinde, **veri yolu kullanılabilirliği**' ni seçin. 
3. **Toplama** açılan listesinde, **Ort**' ı seçin. 
4. Ayrıca, ön uç IP adresi veya ön uç bağlantı noktası için gerekli ön uç IP adresi veya ön uç bağlantı noktasıyla boyut olarak bir filtre ekleyin ve ardından bunları seçilen boyuta göre gruplayın.

![VIP yoklama](./media/load-balancer-standard-diagnostics/LBMetrics-VIPProbing.png)

*Şekil: Load Balancer ön uç araştırma ayrıntıları*

Ölçüm, etkin ve bant içi ölçüm tarafından oluşturulur. Bölge içindeki bir yoklama hizmeti, ölçüm için trafik kaynağı. Hizmet, genel ön uç ile bir dağıtım oluşturduktan hemen sonra etkinleştirilir ve ön ucu kaldırana kadar devam eder. 

Dağıtımınızın ön ucu ve kuralıyla eşleşen bir paket düzenli aralıklarla oluşturulur. Kaynak bölgeden, arka uç havuzundaki bir VM 'nin bulunduğu konağa geçer. Yük dengeleyici altyapısı, diğer tüm trafikle aynı yük dengelemeyi ve çeviri işlemlerini gerçekleştirir. Bu araştırma, yük dengeli uç noktanıza bant içinde yer alır. Arka uç havuzunda iyi durumda olan bir VM 'nin bulunduğu işlem konağına araştırma yapıldıktan sonra, işlem Konağı yoklama hizmetine bir yanıt üretir. VM 'niz bu trafiği görmez.

Veri yolu kullanılabilirliği aşağıdaki nedenlerden dolayı başarısız olur:
- Dağıtımınızda arka uç havuzunda kalan sağlıklı VM yok. 
- Bir altyapı kesintisi oluştu.

Tanılama amacıyla, [veri yolu kullanılabilirlik ölçüsünü sistem durumu araştırma durumuyla birlikte](#vipavailabilityandhealthprobes)kullanabilirsiniz.

Çoğu senaryo için toplama olarak **Ortalama** kullanın.
</details>

#### <a name="are-the-backend-instances-for-my-load-balancer-responding-to-probes"></a>Load Balancer arka uç örnekleri yoklamalara yanıt veriyor mu?
<details>
  <summary>Genişlet</summary>
Durum araştırma durumu ölçümü, yük dengeleyicinizin sistem durumu araştırmasını yapılandırırken sizin tarafınızdan yapılandırılan uygulama dağıtımınızın sistem durumunu açıklar. Yük dengeleyici, yeni akışların nereye gönderileceğini öğrenmek için sistem durumu araştırmasının durumunu kullanır. Sistem durumu araştırmaları bir Azure altyapı adresinden kaynaklanmakta ve VM 'nin Konuk işletim sistemi içinde görülebilir.

Standart Load Balancer kaynaklarınızın durum araştırma durumunu almak için:
1. **Ortalama** toplama türü Ile **sistem durumu araştırma durumu** ölçümünü seçin. 
2. Gerekli ön uç IP adresine veya bağlantı noktasına (veya her ikisine) bir filtre uygulayın.

Durum araştırmaları aşağıdaki nedenlerden dolayı başarısız olur:
- Bir sistem durumu araştırması, dinlemede olmayan veya yanıt vermeyen bir bağlantı noktasına veya yanlış protokol kullanılarak yapılandırılır. Hizmetiniz doğrudan sunucu dönüşü (DSR veya kayan IP) kuralları kullanıyorsa, hizmetin yalnızca ön uç IP adresiyle yapılandırılmış geri döngüyle değil, NIC 'nin IP yapılandırmasının IP adresini dinlediğinden emin olun.
- Ağ güvenlik grubu, sanal makinenin Konuk işletim sistemi güvenlik duvarı veya uygulama katmanı filtreleri tarafından araştırmanız buna izin verilmez.

Çoğu senaryo için toplama olarak **Ortalama** kullanın.
</details>

#### <a name="how-do-i-check-my-outbound-connection-statistics"></a>Giden bağlantı istatistiklerimi denetlemek Nasıl yaparım? mı? 
<details>
  <summary>Genişlet</summary>
SNAT bağlantı ölçümü, [giden akışlar](./load-balancer-outbound-connections.md)için başarılı ve başarısız bağlantıların hacmini açıklar.

Sıfırdan büyük bir hatalı bağlantı birimi, SNAT bağlantı noktası tükenmesi olduğunu gösterir. Bu hatalara neyin neden olabileceğini belirlemek için daha fazla araştırma yapmanız gerekir. SNAT bağlantı noktası Tükenme bildirimleri, [giden akış](./load-balancer-outbound-connections.md)oluşturma hatası olarak. İş üzerindeki senaryoları ve mekanizmaları anlamak için giden bağlantılar hakkındaki makaleyi gözden geçirin ve SNAT bağlantı noktası tükenmesi olmaması için nasıl azaltacağınızı ve tasarlayacağınızı öğrenin. 

SNAT bağlantı istatistiklerini almak için:
1. **SNAT bağlantıları** ölçüm türünü ve toplama olarak **Sum** ' ı seçin. 
2. Farklı satırlarla temsil edilen başarılı ve başarısız SNAT bağlantı sayıları için **bağlantı durumuna** göre gruplandırın. 

![SNAT bağlantısı](./media/load-balancer-standard-diagnostics/LBMetrics-SNATConnection.png)

*Şekil: Load Balancer SNAT bağlantı sayısı*
</details>


#### <a name="how-do-i-check-my-snat-port-usage-and-allocation"></a>SNAT bağlantı noktası kullanımımı ve ayırma Nasıl yaparım? kontrol edilsin mi?
<details>
  <summary>Genişlet</summary>
Kullanılan SNAT bağlantı noktaları ölçümü, giden akışları sürdürmek için kaç adet SNAT bağlantı noktasının tüketildiğini izler. Bu, bir internet kaynağı ile arka uç VM veya bir yük dengeleyicinin arkasındaki sanal makine ölçek kümesi arasında kaç tane benzersiz akış yapıldığını ve genel IP adresine sahip olmadığını gösterir. Kullanmakta olduğunuz SNAT bağlantı noktası sayısını ayrılan SNAT bağlantı noktaları ölçümüyle karşılaştırarak, hizmetinizin, SNAT tükenmesi ve elde edilen giden akış arızası riski olup olmadığını belirleyebilirsiniz. 

Ölçümleriniz [giden akış](./load-balancer-outbound-connections.md) hatası riskini gösteriyorsa, makaleye başvurun ve hizmetin sistem durumunu sağlamak için bunu azaltmaya yönelik adımları uygulayın.

SNAT bağlantı noktası kullanımını ve ayırmayı görüntülemek için:
1. İstenen verilerin görüntülendiğinden emin olmak için grafiğin zaman toplamasını 1 dakika olarak ayarlayın.
1. Ölçüm türü ve toplama olarak **Ortalama** IÇIN **kullanılan SNAT bağlantı noktalarını** ve/veya **ayrılmış SNAT bağlantı noktalarını** seçin
    * Varsayılan olarak, bu ölçümler, TCP ve UDP üzerinden toplanan Load Balancer eşlenen tüm ön uç genel IP 'lerine karşılık gelen, her bir arka uç VM 'si veya VMSS tarafından ayrılan veya kullanılan toplam SNAT bağlantı noktası sayısıdır.
    * Tarafından kullanılan veya yük dengeleyici için ayrılan toplam SNAT bağlantı noktalarını görüntülemek için ölçüm toplama **toplamını** kullanın
1. Belirli bir **protokol türüne**, bir **arka uç IP** kümesine ve/veya **ön uç IP**'lerine filtre uygulayın.
1. Arka uç veya ön uç örneği başına sistem durumunu izlemek için bölme uygulayın. 
    * Notun bölünmesi yalnızca tek bir ölçümün tek seferde görüntülenmesine izin verir. 
1. Örneğin, makine başına TCP akışı için SNAT kullanımını izlemek için, **Ortalama** olarak toplayın, **arka uç IP** 'Lerine göre ayrılır ve **protokol türüne** göre filtreleyin. 

![SNAT ayırma ve kullanım](./media/load-balancer-standard-diagnostics/snat-usage-and-allocation.png)

*Şekil: bir arka uç VM kümesi için Ortalama TCP SNAT bağlantı noktası ayırma ve kullanımı*

![Arka uç örneğine göre SNAT kullanımı](./media/load-balancer-standard-diagnostics/snat-usage-split.png)

*Şekil: arka uç örneği başına TCP SNAT bağlantı noktası kullanımı*
</details>

#### <a name="how-do-i-check-inboundoutbound-connection-attempts-for-my-service"></a>Hizmetme gelen/giden bağlantı girişimlerini kontrol Nasıl yaparım? mı?
<details>
  <summary>Genişlet</summary>
Bir SYN paketleri ölçümü, belirli bir ön uçla ilişkili olan veya gönderilen ( [giden akışlar](./load-balancer-outbound-connections.md)IÇIN) TCP SYN paketlerinin hacmini açıklar. Bu ölçümü, hizmetinize yönelik TCP bağlantısı girişimlerini anlamak için kullanabilirsiniz.

Çoğu senaryo için toplama olarak **Toplam** ' i kullanın.

![SYN bağlantısı](./media/load-balancer-standard-diagnostics/LBMetrics-SYNCount.png)

*Şekil: Load Balancer SYN sayısı*
</details>


#### <a name="how-do-i-check-my-network-bandwidth-consumption"></a>Nasıl yaparım? ağ bant genişliği tüketimi kontrol edilsin mi? 
<details>
  <summary>Genişlet</summary>
Bayt ve paket sayaçları ölçümü, hizmetiniz tarafından ön uç başına gönderilen veya alınan bayt ve paketlerin hacmini açıklar.

Çoğu senaryo için toplama olarak **Toplam** ' i kullanın.

Byte veya Packet Count istatistiklerini almak için:
1. Toplam **bayt sayısını** ve/veya **paket sayısı** ölçüm türünü seçin. **Avg** 
2. Aşağıdakilerden birini yapın:
   * Belirli bir ön uç IP, ön uç bağlantı noktası, arka uç IP veya arka uç bağlantı noktası üzerine filtre uygulayın.
   * Herhangi bir filtreleme yapmadan yük dengeleyici kaynağınızın genel istatistiklerini alın.

![Bayt sayısı](./media/load-balancer-standard-diagnostics/LBMetrics-ByteCount.png)

*Şekil: Load Balancer bayt sayısı*
</details>

#### <a name="how-do-i-diagnose-my-load-balancer-deployment"></a><a name = "vipavailabilityandhealthprobes"></a>Yük dengeleyici dağıtımımı Tanıla Nasıl yaparım??
<details>
  <summary>Genişlet</summary>
Tek bir grafikteki veri yolu kullanılabilirliği ve durum araştırma durumu ölçümlerinin bir birleşimini kullanarak, sorunun nerede arandığını tanımlayabilir ve sorunu çözebilirsiniz. Azure 'un doğru şekilde çalıştığını güvence altına alabilir ve yaratacağı için bu bilgiyi kullanarak yapılandırmanın veya uygulamanın kök nedenin olduğunu belirleyebilirsiniz.

Azure 'un dağıtımınızın sistem durumunu, verdiğiniz yapılandırma uyarınca nasıl görüntülemelerini anlamak için sistem durumu araştırma ölçümlerini kullanabilirsiniz. Sistem durumu araştırmalarının, her zaman izlemenin veya bir nedeni belirlemede harika bir ilk adımdır.

Bu adımları daha fazla alabilir ve veri yolu kullanılabilirliği ölçüsünü kullanarak Azure 'un, belirli dağıtımınızdan sorumlu olan temel alınan veri düzleminin sistem durumunu nasıl görünümüyle ilgili Öngörüler elde edebilirsiniz. Her iki ölçümü de birleştirdiğinizde, bu örnekte gösterildiği gibi hatanın nerede olabileceğini yalıtabilirsiniz:

![Veri yolu kullanılabilirliği ve durum araştırma durumu ölçümlerini birleştirme](./media/load-balancer-standard-diagnostics/lbmetrics-dipnvipavailability-2bnew.png)

*Şekil: veri yolu kullanılabilirliği ve durum araştırma durumu ölçümlerini birleştirme*

Grafik aşağıdaki bilgileri görüntüler:
- VM 'lerinizi barındıran altyapı, grafiğin başlangıcında kullanım dışı ve yüzde 0 ' dan fazla. Daha sonra, altyapı sağlıklı ve VM 'Ler erişilebilir ve arka uca birden fazla VM yerleştirildi. Bu bilgiler, daha sonra yüzde 100 olan veri yolu kullanılabilirliği için mavi izleme tarafından belirtilir. 
- Mor izleme tarafından belirtilen durum araştırma durumu, grafiğin başlangıcında yüzde 0 ' dır. Yeşil daire içinde, durum araştırma durumunun sağlıklı hale geldiğini ve müşterinin dağıtımının yeni akışları kabul edebildiği noktada vurgulanmıştır.

Grafik, müşterilerin, diğer sorunların oluşup oluşmadığını tahmin etmek veya destek istemek zorunda kalmadan, kendi üzerinde dağıtım sorunlarını gidermelerini sağlar. Yanlış yapılandırma veya hatalı uygulama nedeniyle sistem durumu araştırmaları başarısız olduğu için hizmet kullanılamıyor.
</details>

## <a name="resource-health-status"></a><a name = "ResourceHealth"></a>Kaynak sistem durumu

Standart Load Balancer kaynaklarının sistem durumu, **izleme > hizmeti sistem durumu** altında mevcut **kaynak sistem durumu** aracılığıyla gösterilir.

Genel Standart Load Balancer kaynaklarınızın durumunu görüntülemek için:
1. **Monitor**  >  **Hizmet durumunu** İzle ' yi seçin.

   ![Sayfayı izle](./media/load-balancer-standard-diagnostics/LBHealth1.png)

   *Şekil: Azure Izleyici 'de hizmet durumu bağlantısı*

2. **Kaynak durumu**' yi seçin ve ardından **abonelik kimliği** ve **kaynak türü = Load Balancer** ' nın seçildiğinden emin olun.

   ![Kaynak sistem durumu](./media/load-balancer-standard-diagnostics/LBHealth3.png)

   *Şekil: sistem durumu görünümü için kaynak seçin*

3. Listede, geçmiş sistem durumunu görüntülemek için Load Balancer kaynağını seçin.

    ![Load Balancer sistem durumu](./media/load-balancer-standard-diagnostics/LBHealth4.png)

   *Şekil: Load Balancer kaynak durumu görünümü*
 
Genel kaynak sistem durumu açıklaması [RHC belgelerinde](../service-health/resource-health-overview.md)bulunabilir. Azure Load Balancer için belirli durumlar için aşağıdaki tabloda listelenmiştir: 

| Kaynak sistem durumu | Description |
| --- | --- |
| Kullanılabilir | Standart yük dengeleyici kaynağınız sağlıklı ve kullanılabilir durumda. |
| Düzeyi düşürüldü | Standart yük dengeleyiciye, performansı etkileyen platform veya Kullanıcı tarafından başlatılan olaylar vardır. Veri yolu kullanılabilirlik ölçümü, en az iki dakika boyunca %90 ' den az, ancak %25 ' ten büyük bir durum bildirdi. Orta derecede önemli performans etkisi yaşayacaktır. [Veri yolu kullanılabilirliği Kılavuzu ' nu izleyerek, kullanılabilirliğinden etkilenmesine neden olan kullanıcı tarafından başlatılan olaylar olup olmadığını saptayın.
| Kullanılamaz | Standart yük dengeleyici kaynağınız sağlıklı değil. Veri yolu kullanılabilirlik ölçümü, en az iki dakika boyunca %25 sistem durumunu daha az raporladı. Gelen bağlantı için önemli bir performans etkisi veya kullanılabilirlik eksikliği yaşanacaktır. Kullanılamaz duruma neden olan kullanıcı veya platform olayları olabilir. [Veri yolu kullanılabilirliği Kılavuzu] sorunlarını etkileyen Kullanıcı tarafından başlatılan olaylar olup olmadığını belirleme. |
| Bilinmiyor | Standart yük dengeleyici kaynağınızın kaynak sistem durumu henüz güncelleştirilmemiş veya son 10 dakika boyunca veri yolu kullanılabilirliği bilgilerini almamış. Bu durum geçici olmalıdır ve veriler alındıktan hemen sonra doğru durum yansıtılacaktır. |

## <a name="next-steps"></a>Sonraki adımlar

- [Standart Yük Dengeleyici](./load-balancer-overview.md) hakkında daha fazla bilgi edinin.
- [Yük dengeleyici giden bağlantınız](./load-balancer-outbound-connections.md)hakkında daha fazla bilgi edinin.
- [Azure izleyici](../azure-monitor/overview.md)hakkında bilgi edinin.
- [Azure izleyici REST API](/rest/api/monitor/) ve [ölçümleri REST API aracılığıyla alma](/rest/api/monitor/metrics/list)hakkında bilgi edinin.