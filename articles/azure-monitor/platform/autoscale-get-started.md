---
title: Azure'da otomatik ölçeklendirmeyi kullanmaya başlama
description: Azure 'da kaynak Web uygulamanızı, bulut hizmetinizi, sanal makineyi veya sanal makine ölçek kümesini ölçeklendirmeyi öğrenin.
ms.topic: conceptual
ms.date: 07/07/2017
ms.subservice: autoscale
ms.openlocfilehash: b8d16b4e112c9aebe86c60dc01d380d591fc7624
ms.sourcegitcommit: a07a01afc9bffa0582519b57aa4967d27adcf91a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91743531"
---
# <a name="get-started-with-autoscale-in-azure"></a>Azure 'da otomatik ölçeklendirme ile çalışmaya başlama
Bu makalede, Microsoft Azure portal kaynağınız için otomatik ölçeklendirme ayarlarınızı nasıl ayarlayabileceğinizi açıklar.

Azure Izleyici otomatik ölçeklendirme yalnızca [Sanal Makine Ölçek Kümeleri](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloud Services](https://azure.microsoft.com/services/cloud-services/), [App Service-Web Apps](https://azure.microsoft.com/services/app-service/web/)ve [API Management Hizmetleri](../../api-management/api-management-key-concepts.md)için geçerlidir.

## <a name="discover-the-autoscale-settings-in-your-subscription"></a>Aboneliğinizdeki otomatik ölçeklendirme ayarlarını bulma

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4u7ts]

Azure Izleyici 'de otomatik ölçeklendirme için geçerli olan tüm kaynakları bulabilirsiniz. Adım adım yönergeler için aşağıdaki adımları kullanın:

1. Azure portal açın [.][1]
1. Sol bölmedeki Azure Izleyici simgesine tıklayın.
  ![Azure Izleyici 'yi açın][2]
1. Otomatik ölçeklendirme için geçerli olan tüm kaynakları görüntülemek için otomatik **ölçeklendirme** ' a tıklayın.
  ![Azure Izleyici 'de otomatik ölçeklendirmeyi bulma][3]

Belirli bir kaynak grubundaki kaynakları, belirli kaynak türlerini veya belirli bir kaynağı seçmek için listenin üst kısmındaki Filtre bölmesini kullanabilirsiniz.

Her kaynak için geçerli örnek sayısını ve otomatik ölçeklendirme durumunu bulacaksınız. Otomatik ölçeklendirme durumu şu olabilir:

- **Yapılandırılmadı**: Bu kaynak Için otomatik ölçeklendirmeyi henüz etkinleştirmediyseniz.
- **Etkin**: Bu kaynak Için otomatik ölçeklendirmeyi etkinleştirdiniz.
- **Devre dışı**: Bu kaynak Için otomatik ölçeklendirmeyi devre dışı bırakmış olabilirsiniz.

## <a name="create-your-first-autoscale-setting"></a>İlk otomatik ölçeklendirme ayarınızı oluşturma

Şimdi ilk otomatik ölçeklendirme ayarınızı oluşturmak için basit bir adım adım izlenecek yol ile başlayalım.

1. Azure Izleyici 'de **Otomatik ölçeklendirme** dikey penceresini açın ve ölçeklendirmek istediğiniz kaynağı seçin. (Aşağıdaki adımlarda bir Web uygulamasıyla ilişkili bir App Service planı kullanılır. [İlk ASP.NET Web uygulamanızı, 5 dakikada Azure 'da oluşturabilirsiniz.][4])
1. Geçerli örnek sayısının 1 olduğunu unutmayın. **Otomatik ölçeklendirmeyi etkinleştir**' e tıklayın.
  ![Yeni Web uygulaması için ölçek ayarı][5]
1. Ölçek ayarı için bir ad girin ve ardından **Kural Ekle**' ye tıklayın. Sağ tarafta bağlam bölmesi olarak açan ölçek kuralı seçeneklerine dikkat edin. Bu, varsayılan olarak, kaynağın CPU yüzdesi yüzde 70 ' ü aşarsa, örnek sayınız 1 ' i ölçeklendirmeye yönelik seçeneği ayarlar. Varsayılan değerlerinde bırakın ve **Ekle**' ye tıklayın.
  ![Web uygulaması için ölçek ayarı oluşturma][6]
1. Şimdi ilk ölçek kuralınızı oluşturdunuz. UX en iyi uygulamaları önerdiğine ve "kuralda en az bir ölçeğe sahip olmanız önerilir." Bunun için:

    a. **Kural ekle**'ye tıklayın.

    b. **İşleci** **küçüktür olarak ayarlayın**.

    c. **Eşiği** **20**olarak ayarlayın.

    d. **Sayıyı sayısını azaltmak**için **işlemi** ayarlayın.

   Artık CPU kullanımına göre ölçeklendirilmesi/ölçeklendirmeye yönelik bir ölçek ayarınız olmalıdır.
   ![CPU 'ya göre ölçeklendirin][8]
1. **Kaydet**’e tıklayın.

Tebrikler! Artık, Web uygulamanızı CPU kullanımına göre otomatik olarak ölçeklendirmek için ilk ölçek ayarınızı başarıyla oluşturdunuz.

> [!NOTE]
> Aynı adımlar, bir sanal makine ölçek kümesi veya bulut hizmeti rolü ile çalışmaya başlamak için geçerlidir.

## <a name="other-considerations"></a>Diğer önemli noktalar
### <a name="scale-based-on-a-schedule"></a>Zamanlamaya göre ölçeklendirin
CPU 'ya göre Ölçeklendirmeye ek olarak, ölçeklendirmenizi haftanın belirli günlerinde farklı şekilde ayarlayabilirsiniz.

1. **Ölçek koşulu Ekle**' ye tıklayın.
1. Ölçek modunu ve kuralları ayarlamak, varsayılan koşulla aynıdır.
1. Zamanlama için **belirli günleri Yinele** ' yi seçin.
1. Ölçek koşulunun uygulanması gereken günleri ve başlangıç/bitiş saatini seçin.

![Zamanlamayı zamanlamaya göre ölçeklendirin][9]
### <a name="scale-differently-on-specific-dates"></a>Belirli tarihlerde farklı ölçeklendirin
CPU 'ya göre Ölçeklendirmeye ek olarak, ölçeklendirmenizi belirli tarihler için farklı şekilde ayarlayabilirsiniz.

1. **Ölçek koşulu Ekle**' ye tıklayın.
1. Ölçek modunu ve kuralları ayarlamak, varsayılan koşulla aynıdır.
1. Zamanlama için **Başlangıç/bitiş tarihlerini belirt** ' i seçin.
1. Ölçek koşulunun uygulanması gereken başlangıç/bitiş tarihlerini ve başlangıç/bitiş saatini seçin.

![Tarihlere göre ölçeği Ölçeklendir][10]

### <a name="view-the-scale-history-of-your-resource"></a>Kaynağınızın ölçek geçmişini görüntüleyin
Kaynağınız yukarı veya aşağı ölçeklendirildiğinde, etkinlik günlüğüne bir olay kaydedilir. **Çalışma geçmişi** sekmesine geçerek, son 24 saat için kaynağınızın ölçek geçmişini görüntüleyebilirsiniz.

![Çalıştırma geçmişi][11]

Tüm ölçek geçmişini (90 güne kadar) görüntülemek istiyorsanız, **daha fazla ayrıntı görmek için buraya tıklayın ' ı**seçin. Etkinlik günlüğü açılır ve kaynak ve kategorim için otomatik ölçeklendirme önceden seçilir.

### <a name="view-the-scale-definition-of-your-resource"></a>Kaynağınızın ölçek tanımını görüntüleyin
Otomatik ölçeklendirme bir Azure Resource Manager kaynağıdır. **JSON sekmesine geçerek** , JSON 'da ölçek tanımını görüntüleyebilirsiniz.

![Ölçek tanımı][12]

Gerekirse JSON 'da doğrudan değişiklik yapabilirsiniz. Bu değişiklikler, kaydettikten sonra yansıtılır.

### <a name="disable-autoscale-and-manually-scale-your-instances"></a>Otomatik ölçeklendirmeyi devre dışı bırakma ve örneklerinizi el ile ölçeklendirme
Geçerli ölçek ayarınızı devre dışı bırakmak ve kaynağınızı el ile ölçeklendirmek istediğiniz zamanlar olabilir.

Üstteki **Otomatik ölçeklendirmeyi devre dışı bırak** düğmesine tıklayın.
![Otomatik ölçeklendirmeyi devre dışı bırak][13]

> [!NOTE]
> Bu seçenek yapılandırmanızı devre dışı bırakır. Ancak otomatik ölçeklendirmeyi yeniden etkinleştirdikten sonra geri dönebilirsiniz.

Artık ölçeklendirmek istediğiniz örneklerin sayısını el ile ayarlayabilirsiniz.

![El ile ölçek ayarla][14]

**Otomatik ölçeklendirmeyi etkinleştir** ' i ve sonra **Kaydet**' e tıklayarak her zaman otomatik ölçeklendirmeyi geri dönebilirsiniz.

## <a name="route-traffic-to-healthy-instances-app-service"></a>Trafiği sağlıklı örneklere yönlendir (App Service)

Birden çok örneğe ölçeklendirilen zaman, App Service yalnızca sağlıklı örneklere trafik yönlendirmek için örneklerinizi üzerinde sistem durumu denetimleri gerçekleştirebilir. Bunu yapmak için, portalı App Service açın ve ardından **izleme**altında **sistem durumu denetimi** ' ni seçin. **Etkinleştir** ' i seçin ve uygulamanızda, veya gibi GEÇERLI bir URL yolu sağlayın `/health` `/api/health` . **Kaydet**’e tıklayın.

Özelliği ARM şablonlarıyla etkinleştirmek için, `healthcheckpath` `Microsoft.Web/sites` kaynağın özelliğini sitenizdeki sistem durumu denetim yolu olarak ayarlayın, örneğin: `"/api/health/"` . Özelliği devre dışı bırakmak için, özelliğini boş dizeye geri ayarlayın `""` .

### <a name="health-check-path"></a>Sistem durumu denetim yolu

Yol, 200 ve 299 (dahil) arasında bir durum kodu ile iki dakika içinde yanıt vermelidir. Yol iki dakika içinde yanıt vermezse veya aralığın dışında bir durum kodu döndürürse, örnek "sağlıksız" olarak değerlendirilir. Sistem durumu denetimi, App Service kimlik doğrulaması ve yetkilendirme özellikleriyle tümleşir, bu özellik özellikleri etkin olsa bile sistem uç noktaya ulaşacaktır. Kendi kimlik doğrulama sisteminizi kullanıyorsanız, sistem durumu denetimi yolu anonim erişime izin vermelidir. Site yalnızca HTTP**s**etkinse, healthcheck Isteği http**s**aracılığıyla gönderilir.

Sistem durumu denetim yolu, uygulamanızın kritik bileşenlerini denetlemelidir. Örneğin, uygulamanız bir veritabanına ve bir mesajlaşma sistemine bağımlıysa, sistem durumu denetimi uç noktasının bu bileşenlere bağlanması gerekir. Uygulama kritik bir bileşene bağlanamıyorsa, uygulamanın sağlıksız olduğunu göstermek için yol 500 düzeyinde bir yanıt kodu döndürmelidir.

#### <a name="security"></a>Güvenlik 

Büyük kuruluşlarda bulunan geliştirme ekipleri, genellikle sunulan API 'Ler için güvenlik gereksinimlerine bağlı kalmalıdır. Healthcheck uç noktasının güvenliğini sağlamak için öncelikle, uygulamaya erişimi kısıtlamak üzere [IP kısıtlamaları](../../app-service/app-service-ip-restrictions.md#adding-ip-address-rules), [istemci sertifikaları](../../app-service/app-service-ip-restrictions.md#adding-ip-address-rules)veya bir sanal ağ gibi özellikleri kullanmanız gerekir. Gelen isteğin eşleşmesini isteyerek healthcheck uç noktasının kendisini güvenli hale getirebilirsiniz `User-Agent` `ReadyForRequest/1.0` . İstek önceki güvenlik özellikleri tarafından zaten güvenli hale getirildiğinden Kullanıcı Aracısı taklit edilemez.

### <a name="behavior"></a>Davranış

Sistem durumu denetim yolu sağlandığında, App Service tüm örneklerdeki yolu ping yapar. Başarılı bir yanıt kodu 5 pingden sonra alınmıyorsa, bu örnek "sağlıksız" olarak değerlendirilir. Sağlıksız örnek, yük dengeleyici dönüşünün dışında bırakılacak. Ayrıca, ölçeği büyütme veya küçültme sırasında, yeni örneklerin istekler için hazır olduğundan emin olmak için App Service sistem durumu Denetim yolunu ping yapar.

Kalan sağlıklı örnekler daha fazla yük yaşayabilir. Kalan örneklerin aşırı bir kısmını ortadan kaldırmak için, örneklerinizin yarısını hariç tutulamayacak. Örneğin, bir App Service planı 4 örneğe ölçeklenirse ve 3 ' ü sağlıksız olan 3 tanesi, yük dengeleyici dönüşüyle dışarıda bırakılır. Diğer 2 örnek (1 sağlıklı ve 1 sağlıksız) istekleri almaya devam edecektir. Tüm örneklerin sağlıksız olduğu en kötü durum senaryosunda, hiçbiri dışlanacaktır.

Bir örnek bir saat için sağlıksız kalırsa, yeni örnekle birlikte değişir. En fazla bir örnek, App Service plan başına günde en fazla üç örnek olacak şekilde saat başına değiştirilmeyecektir.

### <a name="monitoring"></a>İzleme

Uygulamanızın sistem durumu Denetim yolunu sağladıktan sonra, Azure Izleyici 'yi kullanarak sitenizin sistem durumunu izleyebilirsiniz. Portaldaki **sistem durumu denetimi** dikey penceresinde, üst araç çubuğundan **ölçümler** ' e tıklayın. Bu, sitenin geçmiş sistem durumunu görebileceğiniz ve yeni bir uyarı kuralı oluşturabileceğiniz yeni bir dikey pencere açar. Sitelerinizi izleme hakkında daha fazla bilgi için [bkz. Azure izleyici Kılavuzu](../../app-service/web-sites-monitor.md).

## <a name="next-steps"></a>Sonraki adımlar
- [Aboneliğinizdeki tüm otomatik ölçeklendirme motoru işlemlerini izlemek için bir etkinlik günlüğü uyarısı oluşturun](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [Aboneliğinizde başarısız olan tüm otomatik ölçeklendirme ölçeği Ölçeklendirmesi/genişleme işlemlerini izlemek için bir etkinlik günlüğü uyarısı oluşturun](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)

<!--Reference-->
[1]:https://portal.azure.com
[2]: ./media/autoscale-get-started/azure-monitor-launch.png
[3]: ./media/autoscale-get-started/discover-autoscale-azure-monitor.png
[4]: ../../app-service/quickstart-dotnetcore.md
[5]: ./media/autoscale-get-started/scale-setting-new-web-app.png
[6]: ./media/autoscale-get-started/create-scale-setting-web-app.png
[7]: ./media/autoscale-get-started/scale-in-recommendation.png
[8]: ./media/autoscale-get-started/scale-based-on-cpu.png
[9]: ./media/autoscale-get-started/scale-condition-schedule.png
[10]: ./media/autoscale-get-started/scale-condition-dates.png
[11]: ./media/autoscale-get-started/scale-history.png
[12]: ./media/autoscale-get-started/scale-definition-json.png
[13]: ./media/autoscale-get-started/disable-autoscale.png
[14]: ./media/autoscale-get-started/set-manualscale.png
