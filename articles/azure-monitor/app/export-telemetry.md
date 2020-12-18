---
title: Application Insights 'ten Telemetriyi sürekli dışa aktarma | Microsoft Docs
description: Tanılama ve kullanım verilerini Microsoft Azure depolama alanına aktarın ve buradan indirin.
ms.topic: conceptual
ms.date: 05/26/2020
ms.openlocfilehash: a6f636ce9fe30c666f08935d5830eb0c12e6cb5e
ms.sourcegitcommit: d79513b2589a62c52bddd9c7bd0b4d6498805dbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2020
ms.locfileid: "97674146"
---
# <a name="export-telemetry-from-application-insights"></a>Application Insights’tan telemetriyi dışarı aktarma
Telemetrinizi standart saklama süresinden daha uzun süre tutmak mı istiyorsunuz? Ya da özel bir şekilde işlesin mi? Sürekli dışa aktarma bu için idealdir. Application Insights portalında gördüğünüz olaylar JSON biçiminde Microsoft Azure depoya aktarılabilir. Buradan, verilerinizi indirebilir ve işlemek için gereken her kodu yazabilirsiniz.  

> [!NOTE]
> Sürekli dışarı aktarma yalnızca klasik Application Insights kaynakları için desteklenir. [Çalışma alanı tabanlı Application Insights kaynakları](./create-workspace-resource.md) [tanılama ayarlarını](./create-workspace-resource.md#export-telemetry) kullanmalıdır.
>

Sürekli dışarı aktarmayı ayarlamadan önce şunları göz önünde bulundurmanız isteyebileceğiniz bazı alternatifler vardır:

* Ölçüm veya arama sekmesinin en üstündeki dışarı aktar düğmesi, tabloları ve grafikleri bir Excel elektronik tablosuna aktarmanıza olanak tanır.

* [Analytics](../log-query/log-query-overview.md) , telemetri için güçlü bir sorgu dili sağlar. Ayrıca sonuçları dışarı aktarabilir.
* [Verilerinizi Power BI araştırmak](./export-power-bi.md)istiyorsanız, bu Işlemi sürekli dışarı aktarma kullanmadan yapabilirsiniz.
* [Veri erişimi REST API](https://dev.applicationinsights.io/) , telemetrinize programlı bir şekilde erişmenizi sağlar.
* Ayrıca, [PowerShell aracılığıyla sürekli dışarı aktarmayı](/powershell/module/az.applicationinsights/new-azapplicationinsightscontinuousexport)ayarla ' da erişebilirsiniz.

Sürekli dışa aktarma Işlemi, verilerinizi depolama alanına kopyaladıktan sonra (istediğiniz kadar uzun sürebileceği), her zamanki [Bekletme dönemi](./data-retention-privacy.md)için Application Insights de kullanılabilir.

## <a name="continuous-export-advanced-storage-configuration"></a>Sürekli dışa aktarma Gelişmiş depolama yapılandırması

Sürekli dışarı aktarma, aşağıdaki Azure depolama özelliklerini/yapılandırmasını **desteklemez** :

* Azure Blob depolama ile birlikte [VNET/Azure Storage güvenlik duvarları](../../storage/common/storage-network-security.md) kullanımı.

* [Azure Data Lake Storage 2.](../../storage/blobs/data-lake-storage-introduction.md).

## <a name="create-a-continuous-export"></a><a name="setup"></a> Sürekli dışarı aktarma oluşturma

> [!NOTE]
> Uygulama, günde 3TB 'den fazla veri dışa aktarabilir. Günde 3TB 'den fazla dışa aktarıldığında dışarı aktarma devre dışı bırakılır. Sınır olmadan dışarı aktarmak için [Tanılama ayarları tabanlı dışarı aktarmayı](#diagnostic-settings-based-export)kullanın.

1. Soldaki yapılandırma bölümündeki uygulamanızın Application Insights kaynağında, sürekli dışarı aktarmayı açın ve **Ekle**' yi seçin:

2. Dışarı aktarmak istediğiniz telemetri verileri türlerini seçin.

3. Verileri depolamak istediğiniz bir [Azure depolama hesabı](../../storage/common/storage-introduction.md) oluşturun veya seçin. Depolama fiyatlandırma seçenekleri hakkında daha fazla bilgi için [resmi fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/storage/)ziyaret edin.

     Ekle, hedef ver, depolama hesabı ' na tıklayın ve ardından yeni bir mağaza oluşturun ya da mevcut bir depoyu seçin.

    > [!Warning]
    > Varsayılan olarak, depolama konumu Application Insights kaynağınız ile aynı coğrafi bölgeye ayarlanır. Farklı bir bölgede depolukarşılaşırsanız, aktarım ücretlerine tabi olabilirsiniz.

4. Depolama alanında bir kapsayıcı oluşturun veya seçin.

> [!NOTE]
> Dışarı aktarma işlemini oluşturduktan sonra, yeni alınan veriler Azure Blob depolama alanına akacak. Sürekli dışarı aktarma işlemi, yalnızca sürekli dışarı aktarma etkinleştirildikten sonra oluşturulan/alınan yeni telemetri iletir. Sürekli dışarı aktarma etkinleştirilmeden önce mevcut olan veriler dışarı aktarılmaz ve sürekli dışarı aktarmayı kullanarak daha önce oluşturulmuş olan verileri geçmişe dönük olarak dışarı aktarmanın desteklenen bir yolu yoktur.

Verilerin depolamada görünmesi için bir saat yaklaşık bir gecikme olabilir.

İlk dışa aktarma işlemi tamamlandıktan sonra, Azure Blob depolama kapsayıcısında aşağıdakine benzer bir yapı bulacaksınız: (Bu, topladığınız verilere bağlı olarak farklılık gösterir.)

|Ad | Açıklama |
|:----|:------|
| [Kullanılabilirlik](export-data-model.md#availability) | [Kullanılabilirlik Web testlerini](./monitor-web-app-availability.md)raporlar.  |
| [Olay](export-data-model.md#events) | [Trackevent ()](./api-custom-events-metrics.md#trackevent)tarafından oluşturulan özel olaylar. 
| [Özel durumlar](export-data-model.md#exceptions) |Sunucudaki ve tarayıcıdaki [özel durumları](./asp-net-exceptions.md) raporlar.
| [İletiler](export-data-model.md#trace-messages) | [Tracktrace](./api-custom-events-metrics.md#tracktrace)tarafından ve [günlük bağdaştırıcıları](./asp-net-trace-logs.md)tarafından gönderilir.
| [Ölçümler](export-data-model.md#metrics) | Ölçüm API çağrıları tarafından oluşturulur.
| [PerformanceCounters](export-data-model.md) | Application Insights tarafından toplanan performans sayaçları.
| [İstekler](export-data-model.md#requests)| [Trackrequest](./api-custom-events-metrics.md#trackrequest)tarafından gönderildi. Standart modüller bunu, sunucuda ölçülen sunucu yanıt süresini raporlar olarak kullanır.| 

### <a name="to-edit-continuous-export"></a>Sürekli dışarı aktarmayı düzenlemek için

Sürekli dışarı aktar ' a tıklayın ve düzenlenecek depolama hesabını seçin.

### <a name="to-stop-continuous-export"></a>Sürekli dışarı aktarmayı durdurmak için

Dışarı aktarmayı durdurmak için devre dışı bırak ' a tıklayın. Yeniden etkinleştir ' e tıkladığınızda, dışarı aktarma işlemi yeni verilerle yeniden başlatılır. Dışarı aktarma devre dışı bırakıldığı sürece portalda gelen verileri almazsınız.

Dışarı aktarmayı kalıcı olarak durdurmak için silin. Bunu yapmak, verilerinizi depolamadan silmez.

### <a name="cant-add-or-change-an-export"></a>Dışarı aktarma ekleyemez veya değiştirilemez mi?
* Dışarı aktarmaları eklemek veya değiştirmek için Sahip, Katkıda Bulunan veya Application Insights Katkıda Bulunanı erişim izinleriniz olmalıdır. [Roller hakkında bilgi edinin][roles].

## <a name="what-events-do-you-get"></a><a name="analyze"></a> Hangi olayları alırsınız?
Bu veriler, istemci IP adresinden hesapladığımız konum verilerini eklememiz dışında, uygulamanızdan aldığımız ham telemetri olur.

[Örnekleme](./sampling.md) tarafından atılan veriler, verilmiş verilere dahil değildir.

Diğer hesaplanan ölçümler dahil değildir. Örneğin, ortalama CPU kullanımını dışa aktarmıyoruz, ancak ortalamasının hesaplandığı ham Telemetriyi dışarı aktarıyoruz.

Veriler ayrıca, ayarlamış olduğunuz tüm [kullanılabilirlik Web testlerinin](./monitor-web-app-availability.md) sonuçlarını da içerir.

> [!NOTE]
> **Aşağıdakine.** Uygulamanız çok fazla veri gönderiyorsa, örnekleme özelliği oluşturulan telemetrinin yalnızca bir alt kümesini çalıştırabilir ve gönderebilir. [Örnekleme hakkında daha fazla bilgi edinin.](./sampling.md)
>
>

## <a name="inspect-the-data"></a><a name="get"></a> Verileri İnceleme
Depolamayı doğrudan portalda inceleyebilirsiniz. En soldaki menüdeki giriş ' e tıklayın, en üstte "Azure Services" **depolama hesapları**' nı seçin, depolama hesabı adını seçin, Genel Bakış sayfasında hizmetler ' ın altında **Bloblar** ' ı seçin ve son olarak kapsayıcı adını seçin.

Visual Studio 'da Azure Storage 'ı denetlemek için **Görünüm**, **bulut Gezgini**' ni açın. (Bu menü komutu yoksa, Azure SDK 'sını yüklemeniz gerekir: **Yeni proje** Iletişim kutusunu açın, Visual C#/Cloud öğesini genişletin ve **.NET için Microsoft Azure SDK al**' ı seçin.)

Blob deponuzu açtığınızda blob dosyaları kümesine sahip bir kapsayıcı görürsünüz. Application Insights kaynak adı, izleme anahtarı, telemetri türü/tarih/saat öğesinden türetilen her bir dosyanın URI 'SI. (Kaynak adı tümüyle küçük ve izleme anahtarı kesik çizgileri atlar.)

![Uygun bir araçla blob deposunu inceleyin](./media/export-telemetry/04-data.png)

Tarih ve saat UTC olur ve bunun oluşturulduğu zamandan değil, her zaman telemetri depoya yatırtı. Bu nedenle, verileri indirmek için kod yazarsanız veriler üzerinden doğrusal bir şekilde hareket edebilir.

Yolun biçimi şöyledir:

```console
$"{applicationName}_{instrumentationKey}/{type}/{blobDeliveryTimeUtc:yyyy-MM-dd}/{ blobDeliveryTimeUtc:HH}/{blobId}_{blobCreationTimeUtc:yyyyMMdd_HHmmss}.blob"
```

Konum

* `blobCreationTimeUtc` blob 'un iç hazırlama deposunda oluşturulduğu zaman
* `blobDeliveryTimeUtc` blob 'un dışarı aktarma hedef depolamasına kopyalandığı zaman

## <a name="data-format"></a><a name="format"></a> Veri biçimi
* Her blob, birden çok ' \n ' ayrılmış satır içeren bir metin dosyasıdır. Yaklaşık bir dakikalık bir zaman diliminde işlenen Telemetriyi içerir.
* Her satır, istek veya sayfa görünümü gibi bir telemetri veri noktasını temsil eder.
* Her satır biçimlendirilmemiş bir JSON belgesidir. Satırları görüntülemek istiyorsanız, blobu Visual Studio 'da açın ve   >  **Gelişmiş**  >  **Biçim dosyasını** Düzenle ' yi seçin:

   ![Uygun bir araçla telemetri görüntüleme](./media/export-telemetry/06-json.png)

Zaman süreleri, 10 000 ticks = 1 MS olduğunda yer işaretleri içinde bulunur. Örneğin, bu değerler tarayıcıdan bir istek göndermek için 1 MS, bunu alacak 3 MS ve tarayıcıdaki sayfayı işlemek için 1,8 s saatini gösterir:

```json
"sendRequest": {"value": 10000.0},
"receiveRequest": {"value": 30000.0},
"clientProcess": {"value": 17970000.0}
```

[Özellik türleri ve değerleri için ayrıntılı veri modeli başvurusu.](export-data-model.md)

## <a name="processing-the-data"></a>Verileri işleme
Küçük ölçekte, verilerinizi çekmek, bir elektronik tabloya okumak ve bu şekilde devam etmek için kod yazabilirsiniz. Örnek:

```csharp
private IEnumerable<T> DeserializeMany<T>(string folderName)
{
   var files = Directory.EnumerateFiles(folderName, "*.blob", SearchOption.AllDirectories);
   foreach (var file in files)
   {
      using (var fileReader = File.OpenText(file))
      {
         string fileContent = fileReader.ReadToEnd();
         IEnumerable<string> entities = fileContent.Split('\n').Where(s => !string.IsNullOrWhiteSpace(s));
         foreach (var entity in entities)
         {
            yield return JsonConvert.DeserializeObject<T>(entity);
         }
      }
   }
}
```

Daha büyük bir kod örneği için bkz. [çalışan rolü kullanma][exportasa].

## <a name="delete-your-old-data"></a><a name="delete"></a>Eski verilerinizi silme
Depolama kapasitenizi yönetmek ve gerekirse eski verileri silmek sizin sorumluluğunuzdadır.

## <a name="if-you-regenerate-your-storage-key"></a>Depolama anahtarınızı yeniden oluşturursanız...
Depolamanızın anahtarını değiştirirseniz sürekli dışarı aktarma çalışmayı durdurur. Azure hesabınızda bir bildirim görürsünüz.

Sürekli Dışarı Aktarma sekmesini açın ve dışarı aktarmanızı düzenleyin. Dışarı Aktarma Hedefi'ni düzenleyin ama aynı depolamayı seçili durumda bırakın. Onaylamak için Tamam'a tıklayın.

Sürekli dışarı aktarma yeniden başlatılır.

## <a name="export-samples"></a>Örnekleri dışarı aktar

* [Stream Analytics kullanarak SQL’ye aktarma][exportasa]
* [Stream Analytics örnek 2](export-stream-analytics.md)

Daha büyük ölçeklendirilirken, buluttaki [HDInsight](https://azure.microsoft.com/services/hdinsight/) -Hadoop kümelerini göz önünde bulundurun. HDInsight, büyük verilerin yönetilmesi ve çözümlenmesi için kullanabileceğiniz çeşitli teknolojiler sağlar ve bunu, Application Insights aktarılmış verileri işlemek için kullanabilirsiniz.

## <a name="q--a"></a>Soru-Cevap
* *Ancak istediğim tek seferlik bir grafik indirmesi.*  

    Evet, bunu yapabilirsiniz. Sekmenin en üstünde, **verileri dışarı aktar**' a tıklayın.
* *Bir dışarı aktarma ayarladım, ancak depolamamda hiç veri yok.*

    Dışarı aktarmayı ayarlarken uygulamanızdaki telemetri almak Application Insights muydunuz? Yalnızca yeni veriler alacaksınız.
* *Bir dışarı aktarma ayarlamaya çalıştım, ancak erişim reddedildi*

    Hesap kuruluşunuza aitse, sahipler veya katkıda bulunanlar gruplarının bir üyesi olmanız gerekir.
* *Doğrudan kendi şirket içi mağazama dışarı aktarabilir miyim?*

    Hayır, Üzgünüz. Verme altyapımız Şu anda yalnızca Azure depolama ile birlikte çalışıyor.  
* *Mağazama yerleştirdiğiniz veri miktarına yönelik herhangi bir sınırlama var mı?*

    Hayır. Dışarı aktarma işlemi tamamlanana kadar verileri gönderme işlemini yapacağız. BLOB depolama için dış sınırlara ulaşdığımızda duracağız, ancak bu oldukça büyük. Bu, ne kadar depolama alanı kullanacağınızı denetlemenizi sağlar.  
* *Depolamada kaç blob görmem gerekir?*

  * Dışarı aktarmak üzere seçtiğiniz her veri türü için, her dakikada yeni bir blob oluşturulur (veriler varsa).
  * Ayrıca, yüksek trafikli uygulamalar için ek bölüm birimleri ayrılır. Bu durumda, her birim dakikada bir blob oluşturur.
* *Anahtarı depolama alanım olarak yeniden oluşturdum veya kapsayıcının adını değiştirdi ve şimdi dışarı aktarma çalışmıyor.*

    Dışarı aktarmayı düzenleyin ve hedefi dışarı aktar sekmesini açın. Aynı depolama alanını daha önce olarak bırakın ve onaylamak için Tamam ' ı tıklatın. Dışarı aktarma yeniden başlatılır. Değişiklik son birkaç gün içinde olduysa, verileri kaybetmezsiniz.
* *Dışarı aktarmayı duraklatabilir miyim?*

    Evet. Devre dışı bırak'ı seçin.

## <a name="code-samples"></a>Kod örnekleri

* [Stream Analytics örneği](export-stream-analytics.md)
* [Stream Analytics kullanarak SQL’ye aktarma][exportasa]
* [Özellik türleri ve değerleri için ayrıntılı veri modeli başvurusu.](export-data-model.md)

## <a name="diagnostic-settings-based-export"></a>Tanılama ayarları tabanlı dışarı aktarma

Tanılama ayarları tabanlı dışarı aktarma, sürekli dışarı aktarmanın farklı bir şemasını kullanır. Ayrıca sürekli dışarı aktarmanın şu şekilde olmadığı özellikleri destekler:

* VNET, güvenlik duvarları ve özel bağlantılar içeren Azure depolama hesapları.
* Olay Hub 'ına aktar.

Tanılama ayarları tabanlı dışarı aktarmaya geçiş yapmak için:

1. Geçerli sürekli dışarı aktarmayı devre dışı bırak.
2. [Uygulamayı çalışma alanı tabanlı olarak geçirin](convert-classic-resource.md).
3. [Tanılama ayarlarını dışarı aktarmayı etkinleştirin](create-workspace-resource.md#export-telemetry). Tanılama ayarlarını seçin > Application Insights kaynağınız içinden **Tanılama ayarı ekleyin** .

<!--Link references-->

[exportasa]: ./code-sample-export-sql-stream-analytics.md
[roles]: ./resources-roles-access-control.md

