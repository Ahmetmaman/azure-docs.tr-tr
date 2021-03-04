---
title: Power BI uygulamasıyla Azure maliyetlerini analiz etme
description: Bu makalede Azure Maliyet Yönetimi Power BI uygulamasını yükleme ve kullanma adımları gösterilmektedir.
author: bandersmsft
ms.author: banders
ms.date: 02/19/2021
ms.topic: how-to
ms.service: cost-management-billing
ms.subservice: cost-management
ms.reviewer: benshy
ms.openlocfilehash: b08ff57f964ef7bc3712c930c222a10ed0f89ef4
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102045143"
---
# <a name="analyze-cost-with-the-azure-cost-management-power-bi-app-for-enterprise-agreements-ea"></a>Kurumsal Anlaşmalar (EA) için Azure Maliyet Yönetimi Power BI uygulamasıyla maliyet analizi

Bu makalede Azure Maliyet Yönetimi Power BI uygulamasını yükleme ve kullanma adımları gösterilmektedir. Uygulama, Azure maliyetlerinizi Power BI'da analiz etmenize ve yönetmenize yardımcı olur. Uygulamayı kullanarak maliyetleri ve kullanım eğilimlerini izleyebilir, maliyet iyileştirme seçeneklerini belirleyerek harcamalarınızı azaltabilirsiniz.

Azure maliyet yönetimi Power BI uygulaması şu anda yalnızca bir [Kurumsal Anlaşma](https://azure.microsoft.com/pricing/enterprise-agreement/)sahip müşterileri desteklemektedir.

Uygulama özelleştirme sınırlandırır. Gereksinimlerinize göre özelleştirmek üzere varsayılan filtreleri, görünümleri ve görselleştirmeleri değiştirmek ve genişletmek istiyorsanız, bunun yerine [Power BI Desktop ' de Azure maliyet yönetimi bağlayıcısını](/power-bi/connect-data/desktop-connect-azure-cost-management) kullanın. Azure maliyet yönetimi Bağlayıcısı sayesinde, genel iş maliyetinizi bütünsel görünümlerini almak üzere özelleştirilmiş raporlar oluşturmak için diğer kaynaklardan ek verilere katılabiliyor olabilirsiniz. Bağlayıcı ayrıca Microsoft Müşteri sözleşmelerini destekler.

> [!NOTE]
> Power BI şablon uygulamaları, PBIX dosyasının indirilmesini desteklemez.

## <a name="prerequisites"></a>Ön koşullar

- Uygulamayı yüklemek ve kullanmak için [Power BI Pro lisansı](/power-bi/service-self-service-signup-for-power-bi) gerekir.
- Verilere bağlanmak için bir [Kurumsal Anlaşma](../manage/understand-ea-roles.md) hesabı kullanmanız gerekir. Kuruluş Yöneticisi (salt okunur) rolü desteklenir.

## <a name="installation-steps"></a>Yükleme adımları

Uygulamayı yüklemek için:

1. [Azure Maliyet Yönetimi Power BI uygulamasını](https://aka.ms/costmgmt/ACMApp) açın.
1. Power BI AppSource sayfasında **Şimdi edinin**'i seçin.
1. Kullanım koşullarını ve gizlilik ilkesini kabul etmek için **Devam**'ı seçin.
1. **Bu Power BI uygulamasını yükle** kutusunda **Yükle**'yi seçin.
1. Gerekirse bir çalışma alanı oluşturun ve **Devam**'ı seçin.
1. Yükleme tamamlandıktan sonra yeni uygulamanızın hazır olduğunu belirten bildirim görüntülenir.
1. Yüklediğiniz uygulamayı seçin.
1. Başlarken sayfasında, **verilerinizi bağla**' yı seçin.
    :::image type="content" source="./media/analyze-cost-data-azure-cost-management-power-bi-template-app/connect-your-data.png" alt-text="Veri bağlantısını bağlamayı vurgulayan ekran görüntüsü." lightbox="./media/analyze-cost-data-azure-cost-management-power-bi-template-app/connect-your-data.png" :::
1. Açılan iletişim kutusunda **BillingProfileIdOrEnrollmentNumber** yerine EA kayıt numaranızı girin. Kaç aylık verileri almak istediğinizi belirtin. **Kapsam** için varsayılan **Kayıt Numarası** değerini bırakın ve **İleri**'yi seçin.  
    :::image type="content" source="./media/analyze-cost-data-azure-cost-management-power-bi-template-app/ea-number.png" alt-text="E-kayıt bilgilerini nereye girileceğini gösteren ekran görüntüsü." lightbox="./media/analyze-cost-data-azure-cost-management-power-bi-template-app/ea-number.png" :::
1. Sonraki iletişim kutusu Azure 'a bağlanır ve verileri alır. *Varsayılan değerleri yapılandırılmış olarak bırakın* ve **oturum aç ' ı seçin ve devam edin**.  
    :::image type="content" source="./media/analyze-cost-data-azure-cost-management-power-bi-template-app/autofit.png" alt-text="Varsayılan değerlerle Azure maliyet yönetimi uygulamasına Bağlan iletişim kutusunu gösteren ekran görüntüsü." lightbox="./media/analyze-cost-data-azure-cost-management-power-bi-template-app/autofit.png" :::
1. Son yükleme adımında EA kaydınızla bağlantı kurulur ve bunun için bir [Kuruluş Yöneticisi](../manage/understand-ea-roles.md) hesabı gerekir. Tüm varsayılan değerleri bırakın. **Oturum aç ve Bağlan ' ı** seçin.  
    :::image type="content" source="./media/analyze-cost-data-azure-cost-management-power-bi-template-app/ea-auth.png" alt-text="Azure maliyet yönetimi uygulamasına Bağlan iletişim kutusunu ile bağlanmak için varsayılan değerlerle gösteren ekran görüntüsü." lightbox="./media/analyze-cost-data-azure-cost-management-power-bi-template-app/ea-auth.png" :::
1. EA kaydınıza kimlik doğrulamanız istenir. Power BI kimlik doğrulaması yapın. Kimlik doğrulamasından geçtikten sonra bir Power BI veri yenilemesi başlar.
    > [!NOTE]
    > Veri yenileme işleminin tamamlanması biraz zaman alabilir. Süre, belirtilen ay sayısına ve eşitlenmesi gereken veri miktarına göre değişir.

Veri yenileme işlemi tamamlandıktan sonra, önceden oluşturulmuş raporları görüntülemek için Azure Maliyet Yönetimi uygulamasını seçin.

## <a name="reports-available-with-the-app"></a>Uygulamayla kullanılabilen raporlar

Uygulamada aşağıdaki raporlar mevcuttur.

**Kullanmaya Başlama**: Faydalı olabilecek belgelerin bağlantılarını ve geri bildirimlerinizi paylaşabileceğiniz bağlantıları sunar.

**Hesaba genel bakış**: Raporda aşağıdakiler dahil olmak üzere aylık bilgilerin bir özeti gösterilir:

- Alacaklara karşı ücretler
- Yeni satın almalar
- Azure Market ücretleri
- Fazla kullanımlar ve toplam ücretler

**Aboneliklere ve Kaynak Gruplarına Göre Kullanım**: Aboneliklere ve kaynak grubuna göre maliyeti gösteren zaman içindeki maliyet görünümünü ve grafikleri sağlar.

**Hizmetlere Göre Kullanım**: MeterCategory ölçütüne göre zaman içindeki kullanımı gösterir. Kullanım artışlarını veya düşüşlerini anlamak için kullanım verilerinizi izleyebilir ve tüm anomalilerin ayrıntılarına gidebilirsiniz.

**Kullanımı En Çok Etkileyen 5 Etmen**: Bu rapor, ilk 5 MeterCategory ölçütüne ve MeterName bilgisine göre filtrelenmiş maliyet özeti gösterir.

**Windows Server AHB Kullanımı**: Bu rapor, Azure Hibrit Avantajı'nın etkinleştirilmiş olduğu sanal makine sayısını gösterir. Ayrıca sanal makineler tarafından kullanılan çekirdek/vCPU sayısını da gösterir.

:::image type="content" source="./media/analyze-cost-data-azure-cost-management-power-bi-template-app/ahb-report-full.png" alt-text="Tam Azure hibrit avantajları raporunun gösterildiği ekran görüntüsü." lightbox="./media/analyze-cost-data-azure-cost-management-power-bi-template-app/ahb-report-full.png" :::

Bu rapor ayrıca Hibrit Avantajı'nın **etkin** durumda olduğu ancak vCPU sayısının 8'den _az_ olduğu Windows VM'lerini de tanımlar. Hibrit Avantajı'nın **etkin değil** durumunda olduğu ve vCPU sayısı 8 veya _daha fazla_ olduğu VM'leri de gösterir. Bu bilgiler, Hibrit Avantajınızı tam kapasiteyle kullanmanıza yardımcı olur. Olası tasarruf miktarınızı en üst düzeye çıkarmak için avantajı en pahalı sanal makinelerinize uygulayın.

:::image type="content" source="./media/analyze-cost-data-azure-cost-management-power-bi-template-app/ahb-report.png" alt-text="Azure hibrit avantajlar raporunun 8 ' den az vCPU ve vCPU 'Ları etkin değil alanından gösteren ekran görüntüsü." lightbox="./media/analyze-cost-data-azure-cost-management-power-bi-template-app/ahb-report.png" :::

**RI Ücreti Yansıtma**: Bu rapor bölge, abonelik, kaynak grubu veya kaynak düzeyinde ayrılmış örnek (RI) avantajının ne kadar ve nerede uygulandığını öğrenmenize yardımcı olur. Bu rapor, görünümü oluşturmak için amorti edilmiş kullanım verilerini kullanır.

RI kullanımı az olan verileri görüntülemek için _chargetype_ ölçütüne göre filtre uygulayabilirsiniz.

Amorti edilmiş veriler hakkında daha fazla bilgi için bkz. [Kurumsal Anlaşma rezervasyon maliyetleri ve kullanım bilgilerini alma](../reservations/understand-reserved-instance-usage-ea.md).

**RI Tasarrufu**: Bu rapor abonelik, kaynak grubu ve kaynak düzeyinde rezervasyonlar tarafından tahakkuk edilen tasarrufları gösterir. Şunları görüntüler:

- Rezervasyon ile elde edilen maliyet
- Rezervasyonun kullanıma uygulanmadığı durumlarda tahmini isteğe bağlı kullanım ücreti
- Rezervasyondan tahakkuk edilen maliyet tasarrufları

 Rapor, az kullanılan rezervasyon kaynaklı israfı toplam tasarruf tutarından düşer. İsrafın oluşması rezervasyondan kaynaklanmaktadır.

Verileri geliştirmek için amorti edilmiş kullanım verilerini kullanabilirsiniz.

<a name="shared-recommendation"></a>
**VM RI Kapsamı (paylaşılan öneri)** : Bu rapor, seçilen döneme ait isteğe bağlı VM kullanımını ve RI VM kullanımını gösterir. Paylaşılan kapsamda VM RI satın alma işlemlerine yönelik öneriler sunar.

Raporu kullanmak için detaya gitme filtresini seçin.

:::image type="content" source="./media/analyze-cost-data-azure-cost-management-power-bi-template-app/ri-drill-down2.png" alt-text="VM RI kapsam raporundaki detaya gitme seçimini Seç seçeneğini gösteren ekran görüntüsü." lightbox="./media/analyze-cost-data-azure-cost-management-power-bi-template-app/ri-drill-down2.png" :::

Analiz etmek istediğiniz bölgeyi seçin. Ardından örnek boyutu esnekliği grubunu seçin ve bu şekilde devam edin.

Her detaya gitme düzeyi için rapora aşağıdaki filtreler uygulanır:

- Sağ taraftaki kapsam verileri, isteğe bağlı kullanım ücreti kullanılarak ücretlendirilen kullanımla rezervasyon kapsamına giren kullanımın karşılaştırmasını gösteren filtredir.
- Öneriler de filtrelenir.

Öneriler tablosu, kullanılan VM boyutlarına göre rezervasyon satın alma önerileri sağlar.

_Normalleştirilmiş Boyut_ ve _Normalleştirilmiş Önerilen Miktar_ değerleri, satın almayı örnek boyutu esnekliği grubu için en küçük boyut olarak normalleştirmenize yardımcı olur. Bu bilgiler, örnek boyutu esnekliği grubundaki tüm boyutlar için yalnızca bir ayırma satın almayı planladığınız durumlar için faydalıdır.

:::image type="content" source="./media/analyze-cost-data-azure-cost-management-power-bi-template-app/ri-recommendations.png" alt-text="RI önerileri raporunu gösteren ekran görüntüsü." lightbox="./media/analyze-cost-data-azure-cost-management-power-bi-template-app/ri-recommendations.png" :::

**VM RI Kapsamı (tek öneri)** : Bu rapor, seçilen zaman dilimine ait isteğe bağlı VM kullanımını ve RI VM kullanımını gösterir. Abonelik kapsamında VM RI satın alma işlemlerine yönelik öneriler sunar.

Raporun nasıl kullanılacağına ilişkin ayrıntılı bilgi için [VM RI Kapsamı (paylaşılan öneri)](#shared-recommendation) bölümüne bakın.

**RI satın alma işlemleri**: Bu rapor belirli bir dönem içindeki RI satın alma işlemlerini gösterir.

**Fiyat listesi**: Bu rapor, bir Ödeme hesabına veya EA kaydına özgü fiyatların ayrıntılı listesini gösterir.

## <a name="troubleshoot-problems"></a>Sorunları giderme

Power BI uygulamayla ilgili sorun yaşıyorsanız aşağıdaki sorun giderme bilgileri faydalı olabilir.

### <a name="error-processing-the-data-in-the-dataset"></a>Veri kümesindeki verileri işleme hatası

Şunu belirten bir hata alabilirsiniz:

```
There was an error when processing the data in the dataset.
Data source error: {"error":{"code":"ModelRefresh_ShortMessage_ProcessingError","pbi.error":{"code":"ModelRefresh_ShortMessage_ProcessingError","parameters":{},"details":[{"code":"Message","detail":{"type":1,"value":"We cannot convert the value \"Required Field: 'Enr...\" to type List."}}],"exceptionCulprit":1}}} Table: <TableName>.
```

`<TableName>` yerine bir tablo adı görüntülenir.

#### <a name="cause"></a>Nedeni

Maliyet Yönetimi bağlantısında `Enrollment Number` için varsayılan **Kapsam** değeri değiştirildi.

#### <a name="solution"></a>Çözüm

Maliyet Yönetimi'ne yeniden bağlanın ve **Kapsam** değerini `Enrollment Number` olarak ayarlayın. Kuruluşunuzun kayıt numarasını girmeyin, onun yerine tam olarak aşağıdaki resimde gösterildiği gibi `Enrollment Number` yazın.

:::image type="content" source="./media/analyze-cost-data-azure-cost-management-power-bi-template-app/ea-number-troubleshoot.png" alt-text="Kayıt numarası varsayılan metninin değişmediğini gösteren ekran görüntüsü." lightbox="./media/analyze-cost-data-azure-cost-management-power-bi-template-app/ea-number-troubleshoot.png" :::

### <a name="budgetamount-error"></a>BudgetAmount hatası

Şunu belirten bir hata alabilirsiniz:

```
Something went wrong
There was an error when processing the data in the dataset.
Please try again later or contact support. If you contact support, please provide these details.
Data source error: The 'budgetAmount' column does not exist in the rowset. Table: Budgets.
```

#### <a name="cause"></a>Nedeni

Bu hata, temel alınan meta verilerle ilgili bir hata nedeniyle oluşur. Bu sorun, Azure portalda **Maliyet Yönetimi > Bütçe** bölümünde kullanılabilir bütçe olmadığından meydana gelir. Hata düzeltmesi, Power BI Desktop ve Power BI hizmeti için dağıtım sürecindedir.

#### <a name="solution"></a>Çözüm

- Hata düzeltilene kadar, Azure portalında faturalandırma hesabı/EA kaydı düzeyinde bir test bütçesi ekleyerek sorunu geçici olarak çözebilirsiniz. Test bütçesi, Power BI bağlantısının engelini kaldırır. Bütçe oluşturma hakkında daha fazla bilgi için bkz. [Öğretici: Azure bütçesi oluşturma ve yönetme](tutorial-acm-create-budgets.md).

### <a name="invalid-credentials-for-azureblob-error"></a>AzureBlob için geçersiz kimlik bilgileri hatası

Şunu belirten bir hata alabilirsiniz:

```
Failed to update data source credentials: The credentials provided for the AzureBlobs source are invalid.
```

#### <a name="cause"></a>Nedeni

Veri kaynağı bağlantınızın kimlik doğrulama yöntemini değiştirirseniz bu hata oluşur.

#### <a name="solution"></a>Çözüm

1. Verilerinize bağlanın.
1. EA kaydınızı ve ay sayısını girdikten sonra, Kimlik doğrulama yöntemi için **Anonim** varsayılan değerini değiştirmeden bıraktığınızdan ve Gizlilik düzeyi ayarı için **Yok** seçeneğini belirlediğinizden emin olun.  
  :::image type="content" source="./media/analyze-cost-data-azure-cost-management-power-bi-template-app/autofit-troubleshoot.png" alt-text="Ekran görüntüsü Anonim ve Yok değerlerinin girildiği Azure Maliyet Yönetimi Uygulamasına Bağlan iletişim kutusunu gösterir." lightbox="./media/analyze-cost-data-azure-cost-management-power-bi-template-app/autofit-troubleshoot.png" :::
1. Bir sonraki sayfada, Kimlik doğrulama yöntemi için **OAuth2** seçeneğini ayarlayın ve Gizlilik düzeyi için **Yok** seçeneğini belirleyin. Ardından, kaydınızla kimliğinizi doğrulamak için oturum açın. Bu adım ayrıca Power BI’da veri yenilemeyi de başlatır.

## <a name="data-reference"></a>Veri başvurusu

Aşağıdaki bilgiler, uygulama aracılığıyla kullanılabilen verileri özetler. Ayrıca, veri alanları ve değerler hakkında daha çok ayrıntı sunan API'lerin bağlantıları da verilmiştir.

| **Tablo başvurusu** | **Açıklama** |
| --- | --- |
| **AutoFitComboMeter** | RI önerilerini ve kullanım bilgilerini örnek ailesi grubundaki en küçük boyuta göre normalleştirmek için uygulamaya dahil edilen veriler. |
| [**Bakiye özeti**](/rest/api/billing/enterprise/billing-enterprise-api-balance-summary#response) | Kurumsal Anlaşmalar için bakiye özeti. |
| [**Bütçeler**](/rest/api/consumption/budgets/get#definitions) | Mevcut bütçe hedeflerine göre gerçek maliyetleri veya kullanımı görüntülemek için bütçe ayrıntıları. |
| [**Fiyat listeleri**](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet#see-also) | Belirtilen faturalama profili veya EA kaydı için geçerli ölçüm ücretleri. |
| [**RI ücretleri**](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-charges#response) | Son 24 ay içindeki ayrılmış örneklerinize ilişkin ücretler. |
| [**RI önerileri (paylaşılan)**](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation#response) | Son 7 güne ait tüm abonelik kullanım eğilimlerinize göre ayrılmış örnek satın alma önerileri. |
| [**RI önerileri (tek)**](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation#response-1) | Son 7 güne ait tek abonelik kullanım eğilimlerinize göre ayrılmış örnek satın alma önerileri. |
| [**RI kullanım ayrıntıları**](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage#response) | Mevcut ayrılmış örnekleriniz için önceki aya ait tüketim ayrıntıları. |
| [**RI kullanım özeti**](/rest/api/consumption/reservationssummaries/list) | Günlük Azure rezervasyonu kullanım yüzdesi. |
| [**Kullanım ayrıntıları**](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#usage-details-field-definitions) | EA kaydındaki belirtilen faturalama profili için tüketilen miktarların ve tahmini ücretlerin dökümü. |
| [**Amorti edilmiş kullanım ayrıntıları**](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#usage-details-field-definitions) | EA kaydındaki belirtilen faturalama profili için tüketilen miktarların ve tahmini amorti edilmiş ücretlerin dökümü. |

## <a name="next-steps"></a>Sonraki adımlar

Verileri yapılandırma, yenileme, raporları paylaşma ve özelleştirme hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Zamanlanmış yenilemeyi yapılandırma](/power-bi/refresh-scheduled-refresh)
- [Power BI panolarını ve raporlarını iş arkadaşlarıyla ve başkalarıyla paylaşma](/power-bi/service-share-dashboards)
- [Kendinizi ve başkalarını Power BI hizmetindeki raporlara ve panolara abone yapma](/power-bi/service-report-subscribe)
- [Power BI hizmetinden Power BI Desktop'a rapor indirme](/power-bi/service-export-to-pbix)
- [Power BI hizmetinde ve Power BI Desktop’ta rapor kaydetme](/power-bi/service-report-save)
- [Veri kümesini içeri aktararak Power BI hizmetinde rapor oluşturma](/power-bi/service-report-create-new)