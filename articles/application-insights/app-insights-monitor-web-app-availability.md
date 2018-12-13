---
title: Web sitelerinin kullanılabilirlik ve yanıt hızını izleme | Microsoft Docs
description: Application Insights’ta web testleri ayarlayın. Web sitesi kullanılamaz duruma gelirse veya yavaş yanıt verirse uyarı alın.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/30/2018
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: a5177293b24ec400714d8f87be4198a76d59214a
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52878729"
---
# <a name="monitor-availability-and-responsiveness-of-any-web-site"></a>Web sitelerinin kullanılabilirlik ve yanıt hızını izleme
Web uygulamanızı veya web sitenizi herhangi bir sunucuya dağıttıktan sonra kullanılabilirlik ve yanıt hızını izlemeye yönelik testler ayarlayabilirsiniz. [Azure Application Insights](app-insights-overview.md), dünyanın her yerindeki noktalarından uygulamanıza düzenli aralıklarla web istekleri gönderir. Uygulamanız yanıt vermezse veya yavaş yanıt verirse sizi uyarır.

Genel İnternet'ten erişilebilen herhangi bir HTTP veya HTTPS uç noktası için kullanılabilirlik testleri ayarlayabilirsiniz. Test ettiğiniz web sitesine eklemeniz gereken bir şey yoktur. Kendi siteniz olması bile gerekmez. Kullandığınız bir REST API hizmetini test edebilirsiniz.

İki tür kullanılabilirlik testi vardır:

* [URL ping testi](#create): Azure portalında oluşturabileceğiniz basit bir test.
* [Çok adımlı web testi](#multi-step-web-tests): Visual Studio Enterprise’da oluşturup portala yüklediğiniz test.

Her uygulama kaynağı için 100’e kadar kullanılabilirlik testi oluşturabilirsiniz.


## <a name="create"></a>Kullanılabilirlik testi raporlarınız için kaynak açma

Web uygulamanız için **Application Insights’ı zaten yapılandırdıysanız**, Application Insights kaynağını [Azure portalında](https://portal.azure.com) açın.

**Veya raporlarınızı yeni bir kaynakta görmek istiyorsanız,** [Azure portalı](https://portal.azure.com)’na gidin ve bir Application Insights kaynağı oluşturun.

![Yeni > Application Insights](./media/app-insights-monitor-web-app-availability/11-new-app.png)

Yeni kaynağa ait Genel Bakış dikey penceresini açmak için **Tüm kaynaklar**’a tıklayın.

## <a name="setup"></a>URL ping testi oluşturma
Kullanılabilirlik dikey penceresini açın ve bir kullanılabilirlik testi ekleyin.

![En azından web sitenizin URL'sini doldurma](./media/app-insights-monitor-web-app-availability/001-create-test.png)

* **URL**, test etmek istediğiniz herhangi bir web sayfası olabilir, ancak ortak İnternet’te görünür olmalıdır. URL bir sorgu dizesi içerebilir. Bu nedenle, örneğin, veritabanınızla biraz alıştırma yapabilirsiniz. URL yeniden yönlendirme adresine çözümlenirse, en fazla 10 yeniden yönlendirmeyi izleriz.
* **Bağımlı istekleri ayrıştır**: Bu seçenek işaretlenirse test, test kapsamındaki web sitesine ait görüntü, betik, stil dosyaları ve diğer dosyaları ister. Kayıtlı yanıt süresi, bu dosyaları almak için geçen süreyi içerir. Testin tamamının zaman aşımı süresi içinde tüm bu kaynaklar sorunsuz yüklenemezse, test başarısız olur. Seçenek işaretlenmezse, test yalnızca belirttiğiniz URL’deki dosyayı ister.

* **Yeniden denemeyi etkinleştir**: Bu seçenek işaretlenirse, test başarısız olduğunda kısa bir süre sonra yeniden denenir. Art arda üç deneme başarısız olursa bir hata bildirilir. Sonraki testler bundan sonra her zamanki test sıklığında gerçekleştirilir. Bir sonraki başarılı olana kadar yeniden deneme geçici olarak askıya alınır. Bu kural her test konuma bağımsız olarak uygulanır. Bu ayar önerilir. Ortalama olarak hataların yaklaşık %80’i yeniden deneme sırasında kaybolur.

* **Test sıklığı**: Her test konumdan testin ne sıklıkta çalıştırılacağını ayarlar. Beş dakikalık varsayılan sıklıkta ve beş test konumuyla, siteniz ortalama olarak dakikada bir test edilir.

* **Test konumları**, sunucularımızın URL’nize web istekleri gönderdiği yerlerdir. Bizim en az önerilen test konumları beş, sorunları, Web sitenizdeki ağ sorunlarını ayırt edebilirsiniz emin olmak amacıyla sayısıdır. En fazla 16 konum seçebilirsiniz.

> [!NOTE]
> * Birden çok konumlardan beş konumlardan en az test kesinlikle öneririz. Bu geçici bir sorun belirli bir konum neden olabilir yanlış alarm önlemek içindir. Buna ek olarak en uygun yapılandırma için uyarı konumu eşiği + 2 eşit sayıda test konumları sahip olduğunu bulduk. 
> * Katı bir onay "bağımlı istekleri Ayrıştır" seçeneğinin sonuçları etkinleştiriliyor. Test için el ile site göz atarken belirgin olmayabilir çalışmaları başarısız olabilir.

* **Başarı ölçütleri**:

    **Test zaman aşımı**: Yavaş yanıtlar hakkında uyarı almak için bu değeri azaltın. Yanıtlar sitenizden bu süre içinde alınmadıysa test başarısız sayılır. **Bağımlı istekleri ayrıştır**’ı seçtiyseniz; tüm görüntüler, stil dosyaları, betikler ve diğer bağımlı kaynaklar bu süre içinde alınmış olmalıdır.

    **HTTP yanıtı**: Başarılı sayılan döndürüldü durum kodu. 200, normal web sayfası döndürüldüğünü belirten koddur.

    **İçerik eşleşmesi**: "Hoş geldiniz!" gibi bir dize. Her yanıtta büyük küçük harfe duyarlı bir tam eşleşme oluştuğunu test edebiliriz. Joker karakter bulunmayan düz bir dize olmalıdır. Sayfanızın içeriği değişirse bunu güncelleştirmeniz gerektiğini unutmayın.

* **Uyarı konumu eşiği**: en az 3/5 konumları öneririz. Uyarı konumu eşiği ve test konumları sayısı arasındaki en iyi ilişki **uyarı konumu eşiği** = **test konumları sayısı** - 2, en az beş ile test konumları.

## <a name="multi-step-web-tests"></a>Çok adımlı web testleri
Bir dizi URL'nin bulunduğu bir senaryoyu izleyebilirsiniz. Örneğin, bir satış web sitesi izliyorsanız, öğelerin alışveriş sepetine doğru eklendiğini test edebilirsiniz.

> [!NOTE]
> Çok adımlı web testleri ücrete tabidir. [Fiyatlandırma düzeni](https://azure.microsoft.com/pricing/details/application-insights/).
> 

Çok adımlı bir test oluşturmak için Visual Studio Enterprise kullanarak senaryoyu kaydedin ve kaydı Application Insights'a yükleyin. Application Insights, senaryoyu aralıklarla yeniden yürütür ve yanıtları doğrular.

> [!NOTE]
> * Testlerinizde kodlanmış işlevler veya döngüler kullanamazsınız. Test tamamen .webtest betiğinde yer almalıdır. Ancak, standart eklentiler kullanabilirsiniz.
> * Çok adımlı web testlerinde yalnızca İngilizce karakterler desteklenir. Visual Studio’yu diğer dillerde kullanıyorsanız lütfen İngilizce olmayan karakterleri çevirmek/hariç tutmak için web testi tanımı dosyasını güncelleştirin.
>

#### <a name="1-record-a-scenario"></a>1. Senaryo kaydetme
Web oturumu kaydetmek için Visual Studio Enterprise kullanın.

1. Web performans testi projesi oluşturun.

    ![Visual Studio Enterprise sürümünde, Web Performansı ve Yük Testi şablonundan bir proje oluşturun.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-create.png)

 * *Web Performansı ve Yük Testi şablonunu görmüyor musunuz?* - Visual Studio Enterprise’ı kapatın. **Visual Studio Yükleyicisi**’ni açarak Visual Studio Enterprise yüklemesini değiştirin. **Tek Bileşenler** altında **Web Performansı ve yük testi araçları**’nı seçin.

2. .webtest dosyasını açın ve kaydı başlatın.

    ![.webtest dosyasını açın ve Kaydet’e tıklayın.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-start.png)
3. Testinizde benzetimini yapmak istediğiniz kullanıcı işlemlerini yapın: web sitenizi açın, sepete ürün ekleyin ve bunlara devam edin. Sonra testinizi durdurun.

    ![Internet Explorer'da web testi kaydedicisi çalışır.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-record.png)

    Uzun bir senaryo oluşturmayın. 100 adımlık ve 2 dakikalık bir sınır vardır.
4. Testi düzenleme nedenleri:

   * Alınan metin ve yanıt kodlarını denetlemek için doğrulama ekleme.
   * Gereksiz tüm etkileşimleri kaldırma. Ayrıca, resim veya reklam için ya da sitelerin izlenmesi için bağımlı istekleri kaldırabilirsiniz.

     Yalnızca test betiğini düzenleyebildiğinizi, özel kod veya başka web testlerinden çağrı ekleyemediğinizi unutmayın. Testlere döngü eklemeyin. Standart web testi eklentileri kullanabilirsiniz.
5. Çalıştığından emin olmak için testi Visual Studio’da çalıştırın.

    Web test çalıştırıcısı bir web tarayıcısı açar ve kaydettiğiniz eylemleri yineler. Beklediğiniz gibi çalıştığından emin olun.

    ![Visual Studio’da .webtest dosyasını açın ve Çalıştır’a tıklayın.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-run.png)

#### <a name="2-upload-the-web-test-to-application-insights"></a>2. Web testi Application Insights'a yükleme
1. Application Insights portalında bir web testi oluşturun.

    ![Web testleri dikey penceresinde Ekle'yi seçin.](./media/app-insights-monitor-web-app-availability/16-another-test.png)
2. Çok adımlı testi seçip .webtest dosyasını yükleyin.

    ![Çok adımlı web testini seçin.](./media/app-insights-monitor-web-app-availability/appinsights-71webtestUpload.png)

    Test konumları, sıklığı ve uyarı parametrelerini aynı ping testlerinde olduğu gibi aynı şekilde ayarlayın.

### <a name="plugging-time-and-random-numbers-into-your-multi-step-test"></a>Çok adımlı testinizde bağlı kalma süresi ve rasgele rakamlar
Dış bir kaynağa ait stoklar gibi zamana bağımlı veriler alan bir aracı test ettiğinizi varsayalım. Web testinizi kaydettiğinizde, belirli zamanları kullanmanız gerekse de, bunları testin parametreleri (StartTime ve EndTime) olarak ayarlarsınız.

![Parametrelere sahip web testi.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-parameters.png)

Testi çalıştırdığınızda, EndTime her zaman geçerli zaman, StartTime da 15 dakika öncesi olmalıdır.

Web Testi Eklentileri, zamanları parametreleme yolunu sağlar.

1. İstediğiniz her değişken parametre değeri için bir web testi eklentisi ekleyin. Web testi araç çubuğunda, **Web Testi Eklentisi Ekle**’yi seçin.

    ![Web Testi Eklentisi Ekle’yi, sonra da bir türü seçin.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugins.png)

    Bu örnekte, Tarih Saat Eklentisinin iki örneğini kullanacağız. Bir örnek "15 dakika önce" için, bir örnek de "şimdi" için.
2. Her eklentinin özelliklerini açın. Buna bir ad verip geçerli saat olarak kullanılmak üzere ayarlayın. Bunlardan birini Dakika Ekle = 15 olarak ayarlayın.

    ![Adı, Geçerli Saati Kullan’ı ve Dakika Ekle’yi ayarlayın.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-parameters.png)
3. Web testi parametrelerinde, eklenti adına başvurmak için {{plug-in name}} kullanın.

    ![Test parametresinde {{plug-in name}} kullanın.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-name.png)

Artık testi portala yükleyin. Testin her çalıştırılışında dinamik değerler kullanılır.


## <a name="monitor"></a>Kullanılabilirlik testi sonuçlarınızı görme

Birkaç dakika sonra, test sonuçlarını görmek için **Yenile**’ye tıklayın. 

![Giriş dikey penceresinde özet sonuçları](./media/app-insights-monitor-web-app-availability/14-availSummary-3.png)

Dağılım grafiği, tanılama testi adım ayrıntılarını içeren örnek test sonuçlarını gösterir. Test altyapısı, hata içeren testler için tanılama ayrıntılarını depolar. Başarılı testlerde, yürütmelerin bir alt kümesi için tanılama ayrıntıları depolanır. Test zaman damgası, test süresi, konum ve test adını görmek için yeşil/kırmızı noktalardan herhangi birinin üzerine gelin. Test sonucunun ayrıntılarını görmek için dağılım grafiğinde noktalara tıklayarak gezinin.  

Belirli bir testi veya konumu seçin ya da ilgilendiğiniz dönemle ilgili daha fazla sonuç görmek için zaman dilimini küçültün. Arama Gezgini’ni kullanarak tüm yürütmelerden alınan sonuçları görün veya Analytics sorgularını kullanarak bu veriler üzerinde özel raporlar çalıştırın.

Ölçüm Gezgini’nde ham sonuçlara ek olarak iki Kullanılabilirlik ölçümü vardır: 

1. Kullanılabilirlik: Tüm test yürütmelerinde başarılı olan testlerin yüzdelik oranı. 
2. Test Süresi: Tüm test yürütmelerinde ortalama test süresi.

Belirli bir testin ve/veya konumun eğilimlerini analiz etmek için test adına ve konumuna göre filtre uygulayabilirsiniz.

## <a name="edit"></a> Testleri inceleme ve düzenleme

Özet sayfasından belirli bir test seçin. Burada özel sonuçları görebilir ve düzenleyebilir ya da geçici olarak devre dışı bırakabilirsiniz.

![Web testini düzenleme veya devre dışı bırakma](./media/app-insights-monitor-web-app-availability/19-availEdit-3.png)

Hizmetinizde bakım gerçekleştirdiğiniz sırada kullanılabilirlik testlerini veya bunlarla ilişkili uyarıları devre dışı bırakmak isteyebilirsiniz. 

## <a name="failures"></a>Hata görürseniz
Kırmızı noktaya tıklayın.

![Kırmızı noktaya tıklama](./media/app-insights-monitor-web-app-availability/open-instance-3.png)

Bir kullanılabilirlik testi sonucundan tüm bileşenler genelinde işlem ayrıntılarını görebilirsiniz. Burada şunları yapabilirsiniz:

* Sunucunuzdan alınan yanıtı denetleme.
* Başarısız bir kullanılabilirlik testi işlenirken toplanan ilişkili sunucu tarafı telemetrisi ile hata tanılayın.
* Bir sorun oturum veya bir sorunu izlemek için Git veya Azure panoları iş öğesi. Hata, bu olayın bir bağlantısını içerir.
* Web testi sonucunu Visual Studio’da açın.

Deneyimi uçtan uca işlem tanılamaları hakkında daha fazla bilgi edinin [burada](app-insights-transaction-diagnostics.md).

Sentetik kullanılabilirlik testin başarısız olmasına neden olan sunucu tarafı özel durumun ayrıntılarını görmek için özel durum satırına tıklayın. Ayrıca Al [hata ayıklama anlık görüntüsünü](app-insights-snapshot-debugger.md) daha zengin kod düzeyi tanılama için.

![Sunucu tarafı tanılamalarını](./media/app-insights-monitor-web-app-availability/open-instance-4.png)

## <a name="alerts"></a> Kullanılabilirlik uyarıları
Klasik uyarılar deneyimini kullanmanın kullanılabilirlik veri uyarı kuralları aşağıdaki türde olabilir:
1. Bir zaman dönemi içindeki hataları raporlama Y konumları dışında X
2. Toplama kullanılabilirlik yüzdesi düşme bir eşiğin altında
3. Ortalama test süresi arttıkça bir eşiğini aşan

### <a name="alert-on-x-out-of-y-locations-reporting-failures"></a>Hata Raporlama Y konumları dışında X uyar
Varsayılan olarak etkin uyarı kuralı Y konumları dışında X [birleştirilmiş yeni uyarılar deneyimini](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts), yeni bir kullanılabilirlik testi oluşturun. "Klasik" seçeneğini belirleyerek veya uyarı kuralı devre dışı bırakmak belirleyerek çevirme.

![Deneyimi oluşturun](./media/app-insights-monitor-web-app-availability/appinsights-71webtestUpload.png)

**Önemli**: ile [yeni birleştirilmiş uyarılar](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts), uyarı kuralının önem derecesi ve bildirim tercihleri ile [Eylem grupları](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) **olmalıdır** yapılandırılmış Uyarılar deneyimini yaşayın. Aşağıdaki adımlar olmadan, yalnızca portal bildirim alırsınız. 

1. Kullanılabilirlik testi kaydettikten sonra ayrıntılara gitmek için yeni test adına tıklayın. "Uyarı düzenleme" tıklayın ![Düzenle kaydedildikten sonra](./media/app-insights-monitor-web-app-availability/editaftersave.png)

2. İstenen önem derecesini, kural açıklaması ve en önemlisi - bu uyarı kuralı için kullanmak istediğiniz bildirim tercihleri olan eylem grubu ayarlayın.
![Düzen kaydedildikten sonra](./media/app-insights-monitor-web-app-availability/setactiongroup.png)


> [!NOTE]
> * Yukarıdaki adımları izleyerek uyarıyı tetikleyecek çıktığında bildirimleri almak için Eylem grupları yapılandırın. Kural tetiklendiğinde bu adım, yalnızca portal bildirim alırsınız.
>

### <a name="alert-on-availability-metrics"></a>Kullanılabilirlik ölçümler üzerinde uyarı
Kullanarak [yeni birleştirilmiş uyarılar](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts), uyarı segmentli toplama kullanılabilirliğine ve süresi ölçümleri test edebilirsiniz:

1. Ölçüm deneyimi bir Application Insights kaynağını seçin ve bir kullanılabilirlik ölçümü seçin: ![kullanılabilirlik ölçümlerini seçimi](./media/app-insights-monitor-web-app-availability/selectmetric.png)

2. Menü seçeneğinden belirli testleri veya uyarı kuralı ayarlamak için konumları seçebileceğiniz yeni deneyime sürer uyarıları yapılandırın. Bu uyarı kuralı buraya Eylem grupları da yapılandırabilirsiniz.
    ![Kullanılabilirlik uyarıları yapılandırma](./media/app-insights-monitor-web-app-availability/availabilitymetricalert.png)

### <a name="alert-on-custom-analytics-queries"></a>Özel analytics sorgularına göre uyarı
Kullanarak [yeni birleştirilmiş uyarılar](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts), sizi uyarabilir [özel günlük sorguları](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log). Özel sorgular ile kullanılabilirlik sorunları en güvenilir sinyal elde etmenize yardımcı olan rastgele koşula göre uyarabilir. Bu ayrıca TrackAvailability SDK'sını kullanarak özel kullanılabilirlik sonuçları gönderiyorsanız özellikle geçerlidir. 

> [!Tip]
> * Ölçümleri kullanılabilirliği verileri TrackAvailability SDK'mız çağırarak gönderdiğiniz herhangi bir özel kullanılabilirlik sonucunu içerir. Uyarı özel kullanılabilirlik sonuçları ölçümleri desteği hakkında uyarı kullanabilirsiniz.
>

## <a name="dealing-with-sign-in"></a>Oturum açmayla ilgilenme
Kullanıcılarınız uygulamanızda oturum açarsa, oturum açma benzetimi için bir dizi seçeneğiniz vardır; böylece, oturum açmanın ötesinde sayfaları test edebilirsiniz. Kullandığınız yaklaşım, uygulamanın sağladığı güvenlik türüne bağlıdır.

Her durumda, uygulamanızda yalnızca test amacıyla bir hesap oluşturmalısınız. Mümkünse, web testlerinin gerçek kullanıcıları etkileme olasılığını önlemek için test hesabının izinlerini kısıtlayın.

### <a name="simple-username-and-password"></a>Basit kullanıcı adı ve parola
Web testini normal şekilde kaydedin. Önce tanımlama bilgilerini silin.

### <a name="saml-authentication"></a>SAML kimlik doğrulaması
Web testlerinde kullanıma uygun SAML eklentisini kullanın.

### <a name="client-secret"></a>Gizli anahtar
Uygulamanızda gizli anahtar içeren bir oturum açma yolu varsa bu yolu kullanın. Azure Active Directory (AAD), gizli anahtarla oturum açmayı sağlayan bir hizmet örneğidir. AAD’de gizli anahtar, Uygulama Anahtarı’dır.

Aşağıda uygulama anahtarı kullanan bir Azure web uygulaması için web testi örneği verilmiştir:

![Gizli anahtar örneği](./media/app-insights-monitor-web-app-availability/110.png)

1. Gizli anahtar (AppKey) kullanarak AAD’den belirteç alın.
2. Yanıttan taşıyıcı belirteci ayıklayın.
3. Yetkilendirme üst bilgisinde taşıyıcı belirteç kullanarak API çağırın.

Web testinin gerçek bir istemci olduğundan, yani AAD’de kendi uygulamasına sahip olduğundan emin olun ve bu istemcinin clientId’si ile appkey’ini kullanın. Test edilen hizmetiniz, AAD içinde kendi uygulamasına sahiptir: bu uygulamanın appID URI’si, web testinin “kaynak” alanında yansıtılır.

### <a name="open-authentication"></a>Açık Kimlik Doğrulaması
Microsoft veya Google hesabınızla oturum açma, bir açık kimlik doğrulaması örneğidir. OAuth kullanan çok sayıda uygulama, alternatif gizli anahtar da sağlar; bu nedenle ilk taktiğiniz bu olasılığın incelenmesi olmalıdır.

Testinizde OAuth kullanılarak oturum açılması gerekiyorsa, genel yaklaşım şöyledir:

* Web tarayıcınız, kimlik doğrulama sitesi ve uygulamanız arasındaki trafiği incelemek için Fiddler gibi bir araç kullanın.
* Farklı makineler veya tarayıcılar kullanarak veya uzun aralıklarla (süresi dolacak şekilde belirteçleri izin vermek için) iki veya daha fazla oturum açın.
* Farklı oturumları karşılaştırarak, kimlik doğrulama sitesinden geri geçirilen belirteci tanımlayın; başka bir deyişle oturum açıldıktan sonra uygulama sunucunuza geçirilen belirteç.
* Visual Studio’yu kullanarak web testini kaydetme
* Belirteçleri parametreleyin; belirteç kimlik doğrulayıcıdan döndürüldüğünde ve sitede sorgu sırasında kullanıldığında parametre ayarı.
  (Visual Studio testi parametrelemeyi dener, ancak belirteçleri doğru parametrelemez.)

## <a name="performance-tests"></a>Performans testleri
Web sitenizde bir yük testi çalıştırabilirsiniz. Kullanılabilirlik testinde olduğu gibi dünyanın dört bir yanındaki noktalarımızdan basit istekler ya da çok adımlı istekler gönderebilirsiniz. Kullanılabilirlik testinden farklı olarak eşzamanlı birden fazla kullanıcıyı benzeten çok sayıda istek gönderilir.

Genel Bakış dikey penceresinde **Ayarlar**, **Performans Testleri**’ni açın. Bir test oluşturduğunuzda, bağlanma veya Azure DevOps hesabı oluşturmak için davetlidir.

Test tamamlandığında yanıt süreleri ve başarı oranları gösterilir.


![Performans testi](./media/app-insights-monitor-web-app-availability/perf-test.png)

> [!TIP]
> Performans testi etkilerini gözlemlemek için [Canlı Akış](app-insights-live-stream.md)'ı ve [Profil Oluşturucu](app-insights-profiler.md)'yu kullanmanız gerekir.
>

## <a name="automation"></a>Otomasyon
* Otomatik olarak [kullanılabilirlik testi ayarlamak için PowerShell betiklerini kullanın](app-insights-powershell.md#add-an-availability-test).
* Bir uyarı ortaya çıktığında çağrılan bir [web kancası](../monitoring-and-diagnostics/insights-webhooks-alerts.md) ayarlayın.

## <a name="qna"></a> SSS

* *Site sorunsuz görünüyor ancak görüyorum test hataları? Application Insights bana neden uyarı olduğu?*

    * "Etkin ayrıştırma bağımlı istekleri" testiniz var mı? Komut dosyaları gibi kaynakları üzerinde katı denetimi sonuçlarını vb. görüntüler. Bu tür hataları tarayıcıda belirgin olmayabilir. Tüm görüntüleri, betikleri, stil sayfalarını ve sayfa tarafından yüklenen diğer dosyaları denetleyin. Herhangi biri başarısızsa, ana html sayfası Tamam olarak yüklense bile test başarısız olarak raporlanır. Bu tür kaynak hatalarına karşı testin hassasiyetini ortadan kaldırmak için test yapılandırmasında "Bağımlı İstekleri Ayrıştır" seçeneğinin işaretini kaldırın. 

    * Geçici ağ sinyalleri vb. kaynaklı gürültü olasılığını azaltmak için "Test hataları için yeniden denemeyi etkinleştir" yapılandırmasının işaretlendiğinden emin olun. Ayrıca, uygunsuz uyarılara neden olan konuma özgü sorunları önlemek için daha fazla konumdan test yapabilir ve uyarı kuralı eşiğini uygun şekilde yönetebilirsiniz.

    * Herhangi bir kullanılabilirlik deneyiminden kırmızı nokta veya neden biz bildirilen hata ayrıntılarını görmek için tüm kullanılabilirlik hatasından arama Gezgini'ni tıklatın. Test sonucu (etkinse) bağlantılı sunucu tarafı telemetrisi ile birlikte testin neden başarısız anlamanıza yardımcı olacaktır. Sık karşılaşılan nedenleri geçici bir sorun, ağ veya bağlantı sorunlarıdır. 

    * Test zaman aşımı mı? Biz, 2 dakika sonra testleri durdurur. Ping veya çok adımlı bir test 2 dakikadan uzun sürerse, biz hata olarak raporlanır. Kısa süre içinde tamamlayabilmeniz için birden fazla olanlar test parçalamak göz önünde bulundurun.

    * Tüm Konumlar raporu hata ya da yalnızca bazılarını mı? Yalnızca bazı hataları bildirilen ağ/CDN sorunlarından kaynaklanıyor olabilir. Yeniden üzerinde kırmızı noktaya tıklayarak neden konum bir başarısızlık bildirilmedi anlamanıza yardımcı olacaktır.

* *Ben, uyarıyı tetikleyen veya çözümlendiğinde bir e-posta veya her ikisini de almadığını?*

    E-postanızı doğrudan listelenen veya bildirimleri almak için yapılandırılmış olan bir dağıtım listesi onaylamak için Klasik uyarılar yapılandırmasını denetleyin. Daha sonra ise, dış e-postalar alabilirsiniz onaylamak için dağıtım listesi yapılandırmasını denetleyin. Ayrıca, posta yöneticiniz bu soruna neden yapılandırılmış ilkeleri olup olmadığını kontrol edin.

* *Web kancası bildirim almadı?*

    Web kancası bildirim alma uygulama kullanılabilir ve Web kancası isteği başarıyla işler emin olun. Bkz: [bu](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) daha fazla bilgi için.

* *Protokol ihlali hatası ile aralıklı test hatası?*

    Hata ("protokol ihlali..CR’den sonra LF gelmelidir") sunucu (veya bağımlılıklar) ile ilgili bir sorun olduğunu gösterir. Bu durum, yanıtta hatalı biçimlendirilmiş üst bilgiler ayarlandığında meydana gelir. Yük dengeleyiciler veya CDN'lerden kaynaklanabilir. Özellikle bazı üst bilgiler satır sonunu belirtmek için CRLF kullanmıyor olabilir; bu durum HTTP belirtimini ihlal eder ve bu nedenle .NET WebRequest düzeyinde doğrulama başarısız olur. İhlal edici olabilecek nokta üst bilgilerine yanıtı inceleyin.
    
    Not: URL, HTTP üst bilgilerinin gevşek doğrulaması yapılmış tarayıcılarda başarısız olmayabilir. Bu sorunun ayrıntılı bir açıklaması için bu blog gönderisine bakın: http://mehdi.me/a-tale-of-debugging-the-linkedin-api-net-and-http-protocol-violations/  
    
* *Test hatalarını tanılamak için herhangi bir ilgili sunucu tarafı telemetrisi görmüyorum?*
    
    Sunucu tarafı uygulamanız için Application Insights ayarlanmışsa, bunun nedeni [örnekleme](app-insights-sampling.md) işleminin devam ediyor olması olabilir. Farklı kullanılabilirlik sonucu seçin.

* *Web testimden kod çağırabilir miyim?*

    Hayır. Test adımları .webtest dosyasında olmalıdır. Bu nedenle, başka web testlerini çağıramaz, döngüleri kullanamazsınız. Ancak yararlı bulabileceğiniz bir dizi eklenti vardır.

* *HTTPS destekleniyor mu?*

    TLS 1.1 ve TLS 1.2 desteklenir.
* *"Web testleri" ve "kullanılabilirlik testleri" arasında bir fark var mı?*

    Bu iki terim birbirlerinin yerine kullanılabilir. Kullanılabilirlik testleri, çok adımlı web testlerine ek olarak tek URL ping testlerini de içeren daha genel bir terimdir.
    
* *Kullanılabilirlik testlerini, güvenlik duvarının arkasında çalışan kendi iç sunucumuzda kullanmak istiyorum.*

    İki olası çözümü vardır:
    
    * Güvenlik duvarınızı, [Web testi aracılarımızın IP adreslerinden](app-insights-ip-addresses.md) gelen isteklere izin verecek şekilde yapılandırın.
    * İç sunucunuzu düzenli olarak test etmek için kendi kodunuzu yazın. Kodu, güvenlik duvarınızın arkasındaki bir test sunucusunda arka plan işlemi olarak çalıştırın. Test işleminiz, temel SDK paketindeki [TrackAvailability()](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) API’sini kullanarak sonuçları Application Insights’a gönderebilir. Bunun için test sunucunuzun Application Insights alım uç noktası ile giden bağlantısının olması gerekir, ancak bu, gelen isteklere izin vermeye göre çok daha küçük bir güvenlik riski oluşturur. Sonuçlar kullanılabilirlik web testi dikey pencerelerinde görünür, ancak Analytics, Search ve Ölçüm Gezgini’nde kullanılabilirlik sonuçları olarak görüntülenir.

* *Çok adımlı web testi karşıya yüklenemiyor*

    Bu durum bazı nedenler:
    * 300 K boyut sınırı vardır.
    * Döngüler desteklenmez.
    * Başka web testlerine başvurular desteklenmez.
    * Veri kaynakları desteklenmez.

* *Çok adımlı testim tamamlanmıyor*

    Test başına 100 istek sınırı var. Ayrıca, iki dakikadan uzun çalışırsa test durduruldu.

* *İstemci sertifikalarıyla testi nasıl çalıştırırım?*

    Üzgünüz, bunu desteklemiyoruz.


## <a name="next"></a>Sonraki adımlar
[Tanılama günlüklerinde arama yapma][diagnostic]

[Sorun giderme][qna]

[Web testi aracılarının IP adresleri](app-insights-ip-addresses.md)

<!--Link references-->

[azure-availability]: ../insights-create-web-tests.md
[diagnostic]: app-insights-diagnostic-search.md
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md
