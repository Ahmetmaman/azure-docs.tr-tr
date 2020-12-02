---
title: Azure Güvenlik Merkezi'nde güvenlik puanı
description: Azure Güvenlik Merkezi 'nin güvenli puanı ve güvenlik denetimlerinin açıklaması
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetd: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/10/2020
ms.author: memildin
ms.openlocfilehash: 3cd536d051f3e227ba86429ae3f1633bf6c2e82f
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96490542"
---
# <a name="secure-score-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik puanı

## <a name="introduction-to-secure-score"></a>Güvenli puana giriş

Azure Güvenlik Merkezi 'nin iki ana hedefi vardır: 

- Geçerli güvenlik durumunuzu anlamanıza yardımcı olması için
- güvenliği verimli ve etkili bir şekilde iyileştirmenize yardımcı olmak için

Güvenlik Merkezi 'nde, bu hedeflere ulaşmanızı sağlayan merkezi özellik **güvenli puanı**.

Güvenlik Merkezi, güvenlik sorunları için kaynaklarınızı, aboneliklerinizi ve kuruluşunuzu sürekli olarak değerlendirir. Daha sonra her türlü bulguları tek bir puanın içinde toplar, böylece bir bakışta geçerli güvenlik durumunuza göre daha yüksek puan, belirtilen risk düzeyini azaltır.

Güvenli puan, Azure portal sayfalarında yüzde değeri olarak gösterilir, ancak temel alınan değerler de açıkça sunulur:

:::image type="content" source="./media/secure-score-security-controls/single-secure-score-via-ui.png" alt-text="Portalda gösterildiği gibi genel güvenli puan":::

Güvenliğinizi artırmak için, puanınızı yükseltmek için gereken bekleyen eylemler için Güvenlik Merkezi 'nin öneriler sayfasını gözden geçirin. Her öneri, belirli bir sorunu düzeltmenizi sağlayacak yönergeler içerir.

Öneriler, **güvenlik denetimleri** halinde gruplandırılır. Her denetim, ilgili güvenlik önerilerinin mantıksal grubudur ve güvenlik açığı bulunan saldırı yüzeylerinizi yansıtır. Puanınız yalnızca bir denetim içindeki tek bir kaynağın *Tüm* önerilerini düzeltdiğinde geliştirilir. Kuruluşunuzun her bir saldırı yüzeyi için ne kadar iyi güvenlik altına alınacağını öğrenmek için her bir güvenlik denetiminin puanlarını gözden geçirin.

Daha fazla bilgi için bkz. [güvenli puanınızın aşağıda nasıl hesaplandığı](secure-score-security-controls.md#how-your-secure-score-is-calculated) . 


## <a name="access-your-secure-score"></a>Güvenli puanınızı erişin

Aşağıdaki bölümlerde açıklandığı gibi, genel olarak güvenli puanınızı ve abonelik başına puanınızı Azure portal veya program aracılığıyla aracılığıyla bulabilirsiniz:

- [Portaldan güvenli puanınızı alın](#get-your-secure-score-from-the-portal)
- [REST API güvenli puanınızı alın](#get-your-secure-score-from-the-rest-api)
- [Azure Kaynak Grafınızdan (ARG) güvenli puanınızı alın](#get-your-secure-score-from-azure-resource-graph-arg)

### <a name="get-your-secure-score-from-the-portal"></a>Portaldan güvenli puanınızı alın

Güvenlik Merkezi, puanınızı portalda göze çarpacak şekilde görüntüler: Güvenlik Merkezi 'ne genel bakış sayfası ilk ana kutucuğudur. Bu kutucuğu seçtiğinizde, puanı abonelik tarafından kesilen puanı görebileceğiniz adanmış güvenli puan sayfasına götürür. Önceliklendirilmiş önerilerin ayrıntılı listesini görmek için tek bir abonelik seçin ve aboneliğin puanına sahip olacağını belirten olası etkileri görebilirsiniz.

Daha sonra, güvenli puanınız Güvenlik Merkezi 'nin Portal sayfalarında aşağıdaki konumlarda gösterilmiştir.

- Güvenlik Merkezi 'nin **genel bakış** (ana pano) kutucuğunda:

    :::image type="content" source="./media/secure-score-security-controls/score-on-main-dashboard.png" alt-text="Güvenlik Merkezi 'ndeki panoda güvenli puan":::

- Adanmış **güvenli Puanlama** sayfasında:

    :::image type="content" source="./media/secure-score-security-controls/score-on-dedicated-dashboard.png" alt-text="Güvenlik Merkezi 'nin güvenli Puanlama sayfasında güvenli puan":::

- **Öneriler** sayfasının en üstünde:

    :::image type="content" source="./media/secure-score-security-controls/score-on-recommendations-page.png" alt-text="Güvenlik Merkezi 'nin öneriler sayfasında güvenli puan":::

### <a name="get-your-secure-score-from-the-rest-api"></a>REST API güvenli puanınızı alın

Puanınızı güvenli Puanlama API 'SI (Şu anda önizleme aşamasında) aracılığıyla erişebilirsiniz. API yöntemleri, verileri sorgulama ve zaman içinde güvenli Puanlarınızın kendi raporlama mekanizmanızı oluşturma esnekliğini sağlar. Örneğin, belirli bir aboneliğin Puanını almak için [güvenli puanlar API](/rest/api/securitycenter/securescores) 'sini kullanabilirsiniz. Ayrıca, güvenlik denetimlerini ve aboneliklerinizin geçerli Puanını listelemek için [güvenli puan DENETIMLERI API](/rest/api/securitycenter/securescorecontrols) 'sini de kullanabilirsiniz.

![API aracılığıyla tek bir güvenli puan alma](media/secure-score-security-controls/single-secure-score-via-api.png)

Güvenli Puanlama API 'sinin üzerine inşa eden araçların örnekleri için bkz. [GitHub topluluğumuz güvenli Puanlama alanı](https://github.com/Azure/Azure-Security-Center/tree/master/Secure%20Score). 

### <a name="get-your-secure-score-from-azure-resource-graph-arg"></a>Azure Kaynak Grafınızdan (ARG) güvenli puanınızı alın

Azure Kaynak Grafiği, güçlü filtreleme, gruplama ve sıralama özelliklerine sahip bulut ortamlarınızdaki kaynak bilgilerine anında erişim sağlar. Azure aboneliklerindeki bilgileri programlı olarak veya Azure portal içinden sorgulamak için hızlı ve verimli bir yoldur. [Azure Kaynak Grafiği hakkında daha fazla bilgi edinin](../governance/resource-graph/index.yml).

Bağımsız değişken ile birden çok aboneliğin güvenli puanına erişmek için:

1. Azure portal **Azure Kaynak Grafiği Gezgini**' ni açın.

    :::image type="content" source="./media/security-center-identity-access/opening-resource-graph-explorer.png" alt-text="Azure Kaynak Grafiği Gezgini * * öneri sayfası başlatılıyor" :::

1. Kusto sorgunuzu girin (kılavuz için aşağıdaki örnekleri kullanarak).

    - Bu sorgu, abonelik KIMLIĞINI, geçerli puanı punto ve yüzde olarak ve abonelik için en fazla puanı döndürür. 

        ```kusto
        SecurityResources 
        | where type == 'microsoft.security/securescores' 
        | extend current = properties.score.current, max = todouble(properties.score.max)
        | project subscriptionId, current, max, percentage = ((current / max)*100)
        ```

    - Bu sorgu tüm güvenlik denetimlerinin durumunu döndürür. Her denetim için, sağlıksız kaynak sayısını, geçerli puanı ve en fazla puanı elde edersiniz. 

        ```kusto
        SecurityResources 
        | where type == 'microsoft.security/securescores/securescorecontrols'
        | extend SecureControl = properties.displayName, unhealthy = properties.unhealthyResourceCount, currentscore = properties.score.current, maxscore = properties.score.max
        | project SecureControl , unhealthy, currentscore, maxscore
        ```

1. **Sorguyu Çalıştır**' ı seçin.




## <a name="tracking-your-secure-score-over-time"></a>Zaman içinde güvenli puanınızı izleme

Pro hesabıyla Power BI bir Kullanıcı kullanıyorsanız, zaman içinde güvenli puanı izlemek ve tüm değişiklikleri araştırmak için **zaman Içinde güvenli puan** Power BI panosunu kullanabilirsiniz.

> [!TIP]
> GitHub 'daki Azure Güvenlik Merkezi topluluğunun ayrılmış alanında bu panoyu ve güvenli Puanlama ile program aracılığıyla çalışmaya yönelik diğer araçları bulabilirsiniz: https://github.com/Azure/Azure-Security-Center/tree/master/Secure%20Score

Pano, güvenlik durumunuzu çözümlemenize yardımcı olacak aşağıdaki iki raporu içerir:

- **Kaynaklar Özeti** -kaynaklarınızın sistem durumuyla ilgili özetlenen verileri sağlar.
- **Güvenli puan özeti** -puanınızın ilerleme durumuyla ilgili özetlenmiş veriler sağlar. Puandaki değişiklikleri görüntülemek için "abonelik başına süre içinde güvenli puan" grafiğini kullanın. Puanınızın önemli bir değişikliği fark ederseniz, değişikliğe neden olabilecek değişiklikler için "güvenli puanınızı etkileyebilecek algılanan değişiklikler" tablosunu kontrol edin. Bu tablo, önerilerden biri için silinen kaynakları, yeni dağıtılan kaynakları veya güvenlik durumunun değiştiği kaynakları gösterir.

:::image type="content" source="./media/secure-score-security-controls/power-bi-secure-score-dashboard.png" alt-text="Zaman içinde güvenli puanınızı izlemek ve değişiklikleri araştırmak için zaman Içinde isteğe bağlı güvenli puan":::





## <a name="how-your-secure-score-is-calculated"></a>Güvenli puanınızın hesaplanması 

Her güvenlik denetiminin genel güvenli puana doğru katkısı, öneriler sayfasında net bir şekilde gösterilir.

[![Gelişmiş güvenli skor güvenlik denetimlerini tanıtır](media/secure-score-security-controls/security-controls.png)](media/secure-score-security-controls/security-controls.png#lightbox)

Bir güvenlik denetimine yönelik tüm olası noktaları almak için, tüm kaynaklarınızın güvenlik denetimindeki tüm güvenlik önerilerine uyması gerekir. Örneğin, güvenlik merkezi 'nin Yönetim bağlantı noktalarınızı güvenli hale getirmeye yönelik birden çok önerisi vardır. Güvenli puanınız için fark yapmak üzere tümünü düzeltmeniz gerekir.

Örneğin, "sistem güncelleştirmelerini Uygula" adlı güvenlik denetiminde en fazla altı puan bulunur ve bu, denetimin olası artış değerindeki araç ipucunda görebileceğiniz şekilde:

[!["Sistem güncelleştirmelerini Uygula" güvenlik denetimi](media/secure-score-security-controls/apply-system-updates-control.png)](media/secure-score-security-controls/apply-system-updates-control.png#lightbox)

Bu denetim için en yüksek puan, sistem güncelleştirmelerini Uygula, her zaman 6 ' dır. Bu örnekte, 50 kaynak vardır. Bu nedenle, maksimum puanı 50 ile bölyoruz ve sonuç her kaynağın 0,12 puntodur. 

* **Olası artış** (0,12 x 8 sağlıksız kaynak = 0,96)-denetim içinde sizin için kullanılabilir olan diğer noktaları. Bu denetimdeki tüm önerileri düzeltmeniz durumunda puanınız %2 oranında artar (Bu durumda 0,96 nokta, 1 noktaya yuvarlanır). 
* **Geçerli skor** (0,12 x 42 sağlıklı kaynaklar = 5,04)-Bu denetimin geçerli puanı. Her denetim toplam puanta katkıda bulunur. Bu örnekte, denetim 5,04, geçerli güvenli toplamın işaret eder.
* **Maksimum puan** -denetim içindeki tüm önerileri tamamlayarak elde edebilmeniz gereken en yüksek puan sayısıdır. Bir denetim için maksimum puan, bu denetimin göreli anlam olduğunu gösterir. İlk olarak çalışma sorunlarını önceliklendirme için maksimum puan değerlerini kullanın. 


### <a name="calculations---understanding-your-score"></a>Hesaplamalar-puanınızı anlama

|Ölçüm|Formül ve örnek|
|-|-|
|**Güvenlik denetiminin geçerli puanı**|<br>![Güvenlik denetiminin Puanını hesaplama denklemi](media/secure-score-security-controls/secure-score-equation-single-control.png)<br><br>Her bireysel güvenlik denetimi güvenlik puanına katkıda bulunur. Denetim içindeki bir öneriden etkilenen her kaynak, denetimin geçerli puanına doğru katkıda bulunur. Her denetim için geçerli *puan, denetimdeki kaynakların durumunun* ölçüsüdür.<br>![Güvenlik denetiminin geçerli puanı hesaplanırken kullanılan değerleri gösteren araç ipuçları](media/secure-score-security-controls/security-control-scoring-tooltips.png)<br>Bu örnekte, sağlıklı ve sağlıksız kaynakların toplamı olduğundan, 6 ' nın en fazla puanı 78 olarak bölünür.<br>6/78 = 0,0769<br>Sağlıklı kaynak sayısına (4) göre çarpılması geçerli puanın sonucunu elde ediyor:<br>0,0769 * 4 = **0,31**<br><br>|
|**Güvenlik puanı**<br>Tek abonelik|<br>![Aboneliğin güvenli Puanını hesaplama denklemi](media/secure-score-security-controls/secure-score-equation-single-sub.png)<br><br>![Tüm denetimler etkin olan tek abonelik güvenli puanı](media/secure-score-security-controls/secure-score-example-single-sub.png)<br>Bu örnekte, kullanılabilen tüm güvenlik denetimlerine sahip tek bir abonelik vardır (en fazla 60 punto puanı). Puan, olası bir 60 28 noktayı gösterir ve kalan 32 noktaları, güvenlik denetimlerinin "potansiyel puan artışı" biçiminde yansıtılır.<br>![Denetimlerin listesi ve olası puan artışı](media/secure-score-security-controls/secure-score-example-single-sub-recs.png)|
|**Güvenlik puanı**<br>Birden çok abonelik|<br>![Birden çok abonelik için güvenli puanı hesaplama denklemi](media/secure-score-security-controls/secure-score-equation-multiple-subs.png)<br><br>Birden çok abonelik için Birleşik puanı hesaplarken, güvenlik merkezi her abonelik için bir *Ağırlık* içerir. Abonelikleriniz için göreli ağırlıklar, Güvenlik Merkezi tarafından, kaynak sayısı gibi faktörlere göre belirlenir.<br>Her aboneliğin geçerli puanı, tek bir abonelikle aynı şekilde hesaplanır, ancak daha sonra ağırlığa, denklemde gösterildiği gibi uygulanır.<br>Birden çok abonelik görüntülenirken, güvenli puan tüm etkin ilkelerin içindeki tüm kaynakları değerlendirir ve her bir güvenlik denetiminin en fazla puanı üzerinde Birleşik etkileri gruplandırır.<br>![Tüm denetimler etkin olan birden çok abonelik için güvenli puan](media/secure-score-security-controls/secure-score-example-multiple-subs.png)<br>Birleşik puan bir ortalama **değil** ; Bunun yerine, tüm aboneliklerdeki tüm kaynakların durumunun değerlendirilmiş bir şekilde olduğunu gösterir.<br>Burada, öneriler sayfasına giderseniz ve kullanılabilir olası noktaları eklerseniz, geçerli puan (24) ile kullanılabilir maksimum puan (60) arasındaki fark olduğunu göreceksiniz.|
||||

### <a name="which-recommendations-are-included-in-the-secure-score-calculations"></a>Hangi öneriler, güvenli puan hesaplamalarına dahildir?

Yalnızca yerleşik önerilerin güvenli puanı üzerinde etkisi vardır.

**Önizleme** olarak işaretlenen öneriler, güvenli puanınızın hesaplamalarına dahil değildir. Önizleme dönemi sona erdiğinde puanınızın altına katkıda bulunduklarında, bunlar mümkün olduğunda yine de düzeltilmelidir.

Önizleme önerisi örneği:

:::image type="content" source="./media/secure-score-security-controls/example-of-preview-recommendation.png" alt-text="Önizleme bayrağıyla ilgili öneri":::

## <a name="improve-your-secure-score"></a>Secure score’unuzu iyileştirme

Güvenli puanınızı iyileştirmek için, öneriler listenizden güvenlik önerilerini düzeltin. Her bir kaynak için veya **hızlı düzeltme** kullanarak her öneriyi el ile düzeltebilirsiniz! bir kaynak grubuna hızlıca bir öneri için düzeltme uygulamak üzere (kullanılabilir olduğunda) seçeneği. Daha fazla bilgi için bkz. [önerileri](security-center-remediate-recommendations.md)düzeltme.

Puanınızı artırmanın ve kullanıcılarınızın puanınızı olumsuz yönde etkileyen kaynaklar oluşturmadığından ve ilgili önerilerden zorla ve reddetme seçeneklerini yapılandıracağından emin olmanın bir diğer yolu. [Zorla/reddetme önerilerini kullanarak yanlış yapılandırma önleme konusunda](prevent-misconfigurations.md)daha fazla bilgi edinin.

## <a name="security-controls-and-their-recommendations"></a>Güvenlik denetimleri ve önerileri

Aşağıdaki tabloda, Azure Güvenlik Merkezi 'ndeki güvenlik denetimleri listelenmiştir. Her denetim için, denetimde listelenen *Tüm* önerileri, *Tüm* kaynaklarınız için düzeltmeniz durumunda, güvenli puanlarınıza ekleyebileceğiniz en fazla puan sayısını görebilirsiniz. 

Güvenlik Merkezi ile sağlanan güvenlik önerileri kümesi, her bir kuruluşun ortamındaki kullanılabilir kaynaklara göre tasarlanmıştır. Öneriler, [ilkeleri devre dışı bırakarak](tutorial-security-policy.md#disable-security-policies-and-disable-recommendations) ve [belirli kaynakları bir öneriden muaf](exempt-resource.md)bırakarak daha da özelleştirilebilir. 
 
Her kuruluşun, atanan Azure Ilke girişimlerini dikkatle gözden geçirmesini öneririz. 

> [!TIP]
> Girişimlerinizi İnceleme ve düzenlemeyle ilgili ayrıntılar için bkz. [güvenlik Ilkeleriyle çalışma](tutorial-security-policy.md). 

Güvenlik Merkezi 'nin varsayılan güvenlik girişimi sektörde en iyi uygulamaları ve standartları temel alsa da, aşağıda listelenen yerleşik önerilerin kuruluşunuza tamamen uymamasının bir senaryosu vardır. Sonuç olarak, kuruluşunuzun kendi ilkeleriyle uyumlu olduğundan emin olmak için, güvenliği tehlikeye atmadan, bazen varsayılan girişimi ayarlamanız gerekir. endüstri standartları, mevzuat standartları ve buluşmak üzere olduğunuz kıyaslamalar.<br><br>
<div class="foo">

<style type="text/css"> . TG {Border-daraltma: daraltma; kenarlık-boşluk: 0;}. TG TD {Border-Color: Siyah; kenarlık stili: Solid; Border-width: 1px; yazıtipi-Family: Arial, sans-serif; font-size: 14px; overflow: Hidden; Padding: 10px 5px; sözcük-Break: normal;}. TG TH {Border-Color: Siyah; kenarlık stili: Solid; kenarlık genişliği: 1px; yazı tipi-ailesi: Arial, sans-serif; font-size: 18px; yazı tipi-ağırlığı: normal; taşma: gizli; doldurma: 10px 5px; sözcük-Break: normal;}. TG. TG-cly1 {text-align: Left; dikey-align: Middle}. TG. TG-lboı {Border-Color: Inherit; metin-Hizala: Left; Dikey-Hizala: Orta} </style>
<table class="tg">
<thead>
  <tr>
    <th class="tg-cly1"><b>Güvenlik denetimi, skor ve açıklama</b><br></th>
    <th class="tg-cly1"><b>Öneriler</b></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">MFA 'yı etkinleştir (en fazla puan 10)</p></strong>Yalnızca bir kullanıcının kimliğini doğrulamak için parola kullanıyorsanız, bir saldırı vektörünü açık bırakır. Parola zayıfmıdır veya başka bir yerde sunulduktan sonra Kullanıcı Kullanıcı adı ve parolayla oturum açmasını gerçekten istiyor musunuz?<br><a href="https://www.microsoft.com/security/business/identity/mfa">MFA</a> etkinken hesaplarınız daha güvenlidir ve kullanıcılar, tek oturum açma (SSO) ile neredeyse her uygulama için kimlik doğrulaması yapabilir.</td>
    <td class="tg-lboi"; width=55%>- MFA, aboneliğinizde sahip izinleri olan hesaplarda etkinleştirilmelidir<br>- MFA, aboneliğinizde yazma izinleri olan hesaplar etkinleştirilmelidir</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Güvenli yönetim bağlantı noktaları (en fazla 8 puan)</p></strong>Deneme yanılma saldırısı, bir VM 'ye erişim kazanmak için hedef yönetim bağlantı noktaları sağlar. Bağlantı noktalarının her zaman açık olması gerektiğinden, tek bir risk azaltma stratejisi, tam zamanında ağ erişim denetimleri, ağ güvenlik grupları ve sanal makine bağlantı noktası yönetimi kullanarak bağlantı noktalarında etkilenme olasılığını azaltmaktır.<br>Birçok BT kurumundan giden SSH iletişimini engellemediğinden saldırganlar, virüslü sistemlerdeki RDP bağlantı noktalarının sunucuları denetlemek için saldırgan komutuna geri iletişim kurmasına izin veren şifreli tüneller oluşturabilir. Saldırganlar Windows Uzaktan Yönetimi alt sistemini kullanarak ortamınızda geçici bir şekilde hareket edebilir ve bir ağdaki diğer kaynaklara erişmek için çalınmış kimlik bilgilerini kullanabilir.</td>
    <td class="tg-lboi"; width=55%>- Sanal makinelerin yönetim bağlantı noktaları, tam zamanında ağ erişim denetimiyle korunmalıdır<br>- Sanal makineler bir ağ güvenlik grubuyla ilişkilendirilmelidir<br>- Yönetim bağlantı noktaları sanal makinelerinizde kapatılmalıdır</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Sistem güncelleştirmelerini Uygula (en fazla 6 puan)</p></strong>Sistem güncelleştirmeleri, kuruluşlara operasyonel verimliliği koruma, güvenlik açıklarını azaltma ve son kullanıcılar için daha kararlı bir ortam sağlama olanağı sunar. Güncelleştirmelerin uygulanması, düzeltme eki yüklenmemiş güvenlik açıklarını ve saldırılara açık olan ortamlarda sonuçları bırakır. Bu güvenlik açıklarına yararlanılabilir ve veri kaybına, veri ayıklanmasına, fidye ve kaynak kötüye kullanılmasına yol açabilir. Sistem güncelleştirmelerini dağıtmak için, sanal makinelerinize yönelik <a href="/azure/automation/update-management/overview">düzeltme eklerini ve güncelleştirmeleri yönetmek üzere güncelleştirme yönetimi çözümünü</a> kullanabilirsiniz. Güncelleştirme yönetimi, yazılım sürümlerinin dağıtımını ve bakımını denetleme işlemidir.</td>
    <td class="tg-lboi"; width=55%>- İzleme Aracısı sistem durumu sorunları makinelerinizde çözümlenmelidir<br>- İzleme Aracısı sanal makine ölçek kümelerine yüklenmelidir<br>- İzleme Aracısı makinelerinizde yüklü olmalıdır<br>- Bulut hizmeti rolleriniz için işletim sistemi sürümü güncellenmelidir<br>- Sanal makine ölçek kümelerindeki sistem güncelleştirmeleri yüklenmelidir<br>- Sistem güncelleştirmelerinin makinelerinizde yüklü olması gerekir<br>- Sistem güncelleştirmelerinin uygulanabilmesi için makineleriniz yeniden başlatılmalıdır<br>- Kubernetes Hizmetleri, güvenlik açığı olmayan bir Kubernetes sürümüne yükseltilmelidir<br>- İzleme Aracısı sanal makinelerinizde yüklü olmalıdır<br>- Log Analytics Aracısı Windows tabanlı Azure Arc makinelerinizde yüklü olmalıdır (Önizleme)<br>- Log Analytics Aracısı, Linux tabanlı Azure yay makinelerinizde yüklü olmalıdır (Önizleme)</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Güvenlik açıklarını düzelt (en fazla 6 puan)</p></strong>Bir güvenlik açığı, bir tehdit aktörün bir kaynağın gizliliğini, kullanılabilirliğini veya bütünlüğünü tehlikeye atabilecek bir zayıflığa sahiptir. <a href="/windows/security/threat-protection/microsoft-defender-atp/next-gen-threat-and-vuln-mgt">Güvenlik açıklarını yönetmek</a> kuruluş pozlamasını, Hardens uç nokta yüzey alanını azaltır, kurumsal esnekliği artırır ve kaynaklarınızın saldırı yüzeyini azaltır. Tehdit ve güvenlik açığı yönetimi, yazılım ve güvenlik yapılandırması hataları hakkında görünürlük sağlar ve azaltmaları için öneriler sağlar.</td>
    <td class="tg-lboi"; width=55%>- Gelişmiş veri güvenliği SQL veritabanında etkinleştirilmelidir<br>- Azure Container Registry görüntülerdeki güvenlik açıkları düzeltilmelidir<br>- SQL veritabanlarınızdaki güvenlik açıkları düzeltilmelidir<br>- Güvenlik açıkları bir güvenlik açığı değerlendirme çözümü tarafından düzeltilmelidir<br>- Güvenlik açığı değerlendirmesi SQL yönetilen örneği üzerinde etkinleştirilmelidir<br>- Güvenlik açığı değerlendirmesi SQL sunucularınızda etkinleştirilmelidir<br>- Güvenlik açığı değerlendirme çözümünün sanal makinelerinizde yüklü olması gerekir<br>- Kapsayıcı görüntüleri yalnızca güvenilen kayıt defterlerinden dağıtılmalıdır (Önizleme)<br>- Kubernetes için Azure Ilke eklentisi, kümelerinizde yüklü ve etkin olmalıdır (Önizleme)</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Bekleyen şifrelemeyi etkinleştir (en fazla skor 4)</p></strong><a href="/azure/security/fundamentals/encryption-atrest">Bekleyen şifreleme</a> , depolanan veriler için veri koruması sağlar. Bekleyen verilere yönelik saldırılar, verilerin depolandığı donanıma fiziksel erişim sağlamaya çalışır. Azları, bekleyen büyük miktarlarda verileri şifrelemek ve şifrelerini çözmek için simetrik şifrelemeyi kullanır. Verileri depolamaya yazıldığı şekilde şifrelemek için bir simetrik şifreleme anahtarı kullanılır. Bu şifreleme anahtarı, bellekte kullanım için yeniden kullanıma hazır olduğundan bu verilerin şifresini çözmek için de kullanılır. Anahtarlar kimlik tabanlı erişim denetimi ve denetim ilkeleriyle güvenli bir yerde depolanmalıdır. Bu tür bir güvenli konum Azure Key Vault. Bir saldırgan şifreli verileri alırsa, şifreleme anahtarlarını yoksa, saldırgan şifrelemeyi bozmadan verilere erişemez.</td>
    <td class="tg-lboi"; width=55%>- Disk şifrelemesi sanal makinelere uygulanmalıdır<br>- SQL veritabanı 'nda Saydam Veri Şifrelemesi etkinleştirilmelidir<br>- Otomasyon hesabı değişkenleri şifrelenmelidir<br>- Service Fabric kümelerinde ClusterProtectionLevel özelliği EncryptAndSign olarak ayarlanmalıdır<br>- SQL Server TDE koruyucusu kendi anahtarınızla şifrelenmelidir</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Yoldaki verileri şifreleme (en fazla puan 4)</p></strong>Veriler, bileşenler, konumlar veya programlar arasında aktarıldığında "geçiş sırasında" olur. Yoldaki verileri koruyamadığı kuruluşlar, ortadaki adam saldırıları, gizlice dinleme ve oturum ele geçirme işlemleri için açıktır. SSL/TLS protokolleri, verileri alışverişi için kullanılmalıdır ve bir VPN önerilir. Bir Azure sanal makinesi ve şirket içi konum arasında şifrelenmiş veriler gönderirken, internet üzerinden şifreli trafik göndermek için <a href="/azure/vpn-gateway/vpn-gateway-about-vpngateways">azure VPN Gateway</a> gibi bir sanal ağ geçidi kullanabilirsiniz.</td>
    <td class="tg-lboi"; width=55%>- API uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır<br>- İşlev Uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır<br>- Yalnızca Redis Cache güvenli bağlantılar etkinleştirilmelidir<br>- Depolama hesaplarına Güvenli aktarım etkinleştirilmelidir<br>- Web uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır<br>- PostgreSQL sunucuları için özel uç nokta etkinleştirilmelidir<br>- PostgreSQL veritabanı sunucuları için SSL bağlantısını zorla etkinleştirilmelidir<br>- MySQL veritabanı sunucuları için SSL bağlantısını zorla etkinleştirilmelidir<br>- TLS, API uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>- TLS, işlev uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>- TLS, Web uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>- API uygulamanızda FTPS gerekli olmalıdır<br>- İşlev uygulamanızda FTPS gerekli olmalıdır<br>- Web uygulamanızda FTPS gerekli olmalıdır</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Erişimi ve izinleri yönetme (en fazla 5 puan)</p></strong>Bir güvenlik programının temel bir bölümü, kullanıcılarınızın işlerini yapmak için gereken erişimi, ancak <a href="/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models">en az ayrıcalık erişim modelini</a>sağlamalarını sağlamaktır.<br><a href="/azure/role-based-access-control/overview">Azure rol tabanlı erişim denetimi (Azure RBAC)</a>ile rol atamaları oluşturarak kaynaklarınıza erişimi denetleyin. Rol ataması üç öğeden oluşur:<br>- <strong>Güvenlik sorumlusu</strong>: kullanıcının erişim istediği nesne<br>- <strong>Rol tanımı</strong>: izinleri<br>- <strong>Kapsam</strong>: izinlerin uygulandığı kaynak kümesi</td>
    <td class="tg-lboi"; width=55%>- Kullanım dışı bırakılan hesaplar aboneliğinizden çıkarılmalıdır (Önizleme)<br>- Sahip izinleri olan kullanım dışı hesaplar aboneliğinizden kaldırılmalı (Önizleme)<br>- Sahip dış hesapların aboneliğinizden kaldırılması gerekir (Önizleme)<br>- Yazma izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekir (Önizleme)<br>- Aboneliğinize birden fazla sahip atanmalıdır<br>- Azure rol tabanlı erişim denetimi (Azure RBAC), Kubernetes hizmetlerinde (Önizleme) kullanılmalıdır<br>- Service Fabric kümeler yalnızca istemci kimlik doğrulaması için Azure Active Directory kullanmalıdır<br>- Hizmet sorumluları, yönetim sertifikaları yerine aboneliklerinizi korumak için kullanılmalıdır<br>- Kapsayıcılar için en az ayrıcalıklı Linux özellikleri zorlanmalıdır (Önizleme)<br>- Kapsayıcılar için sabit (salt okunurdur) kök dosya sistemi zorunlu kılınmalıdır (Önizleme)<br>- Ayrıcalık yükseltme ile kapsayıcının önlenmesi gerekir (Önizleme)<br>- Kapsayıcıları kök kullanıcı olarak çalıştırma, kaçınılmalıdır (Önizleme)<br>- Gizli ana bilgisayar ad alanlarını paylaşan kapsayıcıların önlenmesi gerekir (Önizleme)<br>- Pod HostPath birimi takmaları kullanımı bilinen bir listeyle sınırlandırılmalıdır (Önizleme)<br>- Ayrıcalıklı kapsayıcılar önlenebilir (Önizleme)<br>- Kubernetes için Azure Ilke eklentisi, kümelerinizde yüklü ve etkin olmalıdır (Önizleme)<br>- Web uygulamalarının tüm gelen istekler için bir SSL sertifikası istemesi gerekir<br>- Yönetilen kimlik API uygulamanızda kullanılmalıdır<br>- Yönetilen kimlik, işlev uygulamanızda kullanılmalıdır<br>- Yönetilen kimliğin Web uygulamanızda kullanılması gerekir</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Güvenlik yapılandırmasını düzeltme (maks. puan 4)</p></strong>Yanlış yapılandırılmış BT varlıklarının saldırıya uğramakta olma riski daha yüksektir. Varlıklar dağıtıldığında ve son tarihler karşılanıyorsa, temel sağlamlaştırma eylemleri genellikle unutulur. Güvenlik yapılandırması hataları altyapıdaki herhangi bir düzeyde olabilir: işletim sistemleri ve ağ gereçlerden bulut kaynaklarına.<br>Azure Güvenlik Merkezi, kaynaklarınızın yapılandırmalarını endüstri standartları, yönetmelikler ve kıyaslamalar ile sürekli olarak karşılaştırır. Kuruluşunuza göre ilgili "uyumluluk paketlerini" (standartlar ve temeller) yapılandırdığınızda, tüm boşluklar CCEıD ve olası güvenlik etkisi hakkında bir açıklama içeren güvenlik önerilerine neden olur.<br>Yaygın olarak kullanılan paketler, <a href="/azure/security/benchmarks/introduction">Azure Güvenlik kıyaslaması</a> ve <a href="https://www.cisecurity.org/benchmark/azure/">CIS Microsoft Azure temelleri kıyaslama sürümü 1.1.0</a></td>
    <td class="tg-lboi"; width=55%>- Kapsayıcı güvenlik yapılandırmalarında güvenlik açıkları düzeltilmelidir<br>- Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir<br>- Sanal makine ölçek kümelerinizin güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir<br>- İzleme Aracısı sanal makinelerinizde yüklü olmalıdır<br>- İzleme Aracısı makinelerinizde yüklü olmalıdır<br>- Log Analytics Aracısı Windows tabanlı Azure Arc makinelerinizde yüklü olmalıdır (Önizleme)<br>- Log Analytics Aracısı, Linux tabanlı Azure yay makinelerinizde yüklü olmalıdır (Önizleme)<br>- İzleme Aracısı sanal makine ölçek kümelerine yüklenmelidir<br>- İzleme Aracısı sistem durumu sorunları makinelerinizde çözümlenmelidir<br>- Kapsayıcılar AppArmor profilinin geçersiz kılınması veya devre dışı bırakılması sınırlandırılmalıdır (Önizleme)<br>- Kubernetes için Azure Ilke eklentisi, kümelerinizde yüklü ve etkin olmalıdır (Önizleme)</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Yetkisiz ağ erişimini kısıtla (maks. puan 4)</p></strong>Kuruluş içindeki uç noktalar, sanal ağınızdan desteklenen Azure hizmetlerine doğrudan bağlantı sağlar. Bir alt ağdaki sanal makineler tüm kaynaklarla iletişim kurabilir. Bir alt ağ içindeki kaynaklarla ve bu kaynaklardan gelen iletişimi sınırlandırmak için bir ağ güvenlik grubu oluşturun ve alt ağ ile ilişkilendirin. Kuruluşlar, gelen ve giden kurallar oluşturarak yetkisiz trafiğe karşı sınırlayabilir ve koruyabilir.</td>
    <td class="tg-lboi"; width=55%>- Sanal makinenizde IP iletimi devre dışı bırakılmalıdır<br>- Yetkili IP aralıkları, Kubernetes hizmetlerinde tanımlanmalıdır (Önizleme)<br>- Kullanım DıŞı Uygulama hizmetlerine erişim kısıtlı olmalıdır (Önizleme)<br>- Kullanım DıŞı IaaS NSG 'lerdeki Web uygulamalarına yönelik kurallar sağlamlaştırılmış olmalıdır<br>- Sanal makineler bir ağ güvenlik grubuyla ilişkilendirilmelidir<br>- CORS, her kaynağın API uygulamanıza erişmesine izin vermemelidir<br>- CORS, her kaynağın İşlev Uygulaması erişmesine izin vermemelidir<br>- CORS, her kaynağın Web uygulamanıza erişmesine izin vermemelidir<br>- API uygulaması için uzaktan hata ayıklama kapatılmalıdır<br>- İşlev Uygulaması için uzaktan hata ayıklama kapatılmalıdır<br>- Web uygulaması için uzaktan hata ayıklama kapatılmalıdır<br>- Internet 'e yönelik VM 'lerle izin veren ağ güvenlik grupları için erişim kısıtlanması gerekir<br>- Internet 'e yönelik sanal makineler için ağ güvenlik grubu kuralları sağlamlaştırılmış olmalıdır<br>- Kubernetes için Azure Ilke eklentisi, kümelerinizde yüklü ve etkin olmalıdır (Önizleme)<br>- Kapsayıcılar yalnızca izin verilen bağlantı noktalarında dinleme yapılmalıdır (Önizleme)<br>- Hizmetlerin yalnızca izin verilen bağlantı noktalarını dinlemesi gerekir (Önizleme)<br>- Konak ağ ve bağlantı noktalarının kullanımı sınırlandırılmalıdır (Önizleme)<br>- Sanal ağlar Azure Güvenlik Duvarı (Önizleme) ile korunmalıdır<br>- MariaDB sunucuları için özel uç nokta etkinleştirilmelidir<br>- MySQL sunucuları için özel uç nokta etkinleştirilmelidir<br>- PostgreSQL sunucuları için özel uç nokta etkinleştirilmelidir</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Uyarlamalı uygulama denetimi Uygula (en fazla skor 3)</p></strong>Uyarlamalı uygulama denetimi (AAC), Azure ve Azure dışı makinelerde hangi uygulamaların çalıştırılacağını denetlemenize olanak tanıyan akıllı, otomatik, uçtan uca bir çözümdür. Ayrıca makinelerinizi kötü amaçlı yazılımlara karşı sağlamlaştırmanıza de yardımcı olur.<br>Güvenlik Merkezi, bir makine grubu için bilinen güvenli uygulamaların bir listesini oluşturmak üzere makine öğrenimini kullanır.<br>Onaylanan uygulama listesine yönelik bu yenilikçi yaklaşım, yönetim karmaşıklığı olmadan güvenlik avantajlarını sağlar.<br>AAC, özellikle belirli bir uygulama kümesini çalıştırması gereken amaç oluşturulmuş sunucular için geçerlidir.</td>
    <td class="tg-lboi"; width=55%>- Uyarlamalı uygulama denetimleri sanal makinelerde etkinleştirilmelidir<br>- İzleme Aracısı sanal makinelerinizde yüklü olmalıdır<br>- İzleme Aracısı makinelerinizde yüklü olmalıdır<br>- Log Analytics Aracısı Windows tabanlı Azure Arc makinelerinizde yüklü olmalıdır (Önizleme)<br>- Log Analytics Aracısı, Linux tabanlı Azure yay makinelerinizde yüklü olmalıdır (Önizleme)<br>- İzleme Aracısı sistem durumu sorunları makinelerinizde çözümlenmelidir</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Veri sınıflandırması Uygula (maks. puan 2)</p></strong>Kuruluşunuzun verilerini duyarlığa göre sınıflandırın ve iş etkisi, verilere değer belirlemenize ve bu verileri atamanıza olanak tanır ve idare için strateji ve temel sağlar.<br><a href="/azure/information-protection/what-is-information-protection">Azure Information Protection</a> , veri sınıflandırmasına yardımcı olabilir. Verileri korumak ve veri erişimini kısıtlamak için şifreleme, kimlik ve yetkilendirme ilkelerini kullanır. Microsoft tarafından kullanılan bazı sınıflandırmalar iş dışı, genel, genel, gizli ve çok gizli.</td>
    <td class="tg-lboi"; width=55%>- SQL veritabanlarınızdaki hassas verilerin sınıflandırılmış olması gerekir (Önizleme)</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Uygulamaları DDoS saldırılarına karşı koruma (maks. puan 2)</p></strong>Dağıtılmış hizmet reddi (DDoS) saldırıları, kaynakları ve işleme uygulamalarını kullanılamaz hale getirir. <a href="/azure/virtual-network/ddos-protection-overview">Azure DDoS koruma standardı</a> 'nı kullanarak kuruluşunuzu üç ana DDoS saldırısı türünden savunabilirsiniz:<br>- <strong>Volumetric saldırıları</strong> , ağı yasal trafikle taşacak. DDoS koruma standardı, bu saldırıları bir bütün olarak veya otomatik olarak temizleyerek bu saldırıları azaltır.<br>- <strong>Protokol saldırıları</strong> , katman 3 ve katman 4 protokol yığınındaki zayıf yanları kötüye vererek bir hedefi işlemez. DDoS koruması standardı, kötü amaçlı trafiği engelleyerek bu saldırıları azaltır.<br>- <strong>Kaynak (uygulama) katmanı saldırıları</strong> Web uygulaması paketlerini hedef. Web uygulaması güvenlik duvarı ve DDoS koruma standardı ile bu türe karşı savunın.</td>
    <td class="tg-lboi"; width=55%>- DDoS koruma standardı etkinleştirilmelidir<br>- Kapsayıcı CPU ve bellek sınırları zorunlu kılınmalıdır (Önizleme)<br>- Kubernetes için Azure Ilke eklentisi, kümelerinizde yüklü ve etkin olmalıdır (Önizleme)</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Endpoint Protection 'ı etkinleştir (en fazla puan 2)</p></strong>Uç noktalarınızın kötü amaçlı yazılımlardan korunmasını sağlamak için, davranış algılayıcıları, uç noktalarınızın işletim sistemlerinden veri toplayıp işleyin ve bu verileri analiz için özel buluta gönderir. Güvenlik analizi, tehditlere yönelik yanıtları önermek için büyük veri, makine öğrenimi ve diğer kaynaklardan faydalanır. Örneğin, <a href="/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection">Microsoft Defender ATP</a> , saldırı bilgilerini belirlemek ve güvenlik uyarıları oluşturmak için tehdit zekasını kullanır.<br>Güvenlik Merkezi aşağıdaki Endpoint Protection çözümlerini destekler: Windows Defender, System Center Endpoint Protection, Trend Micro, Symantec v 12.1.1.1100, Windows için McAfee ile v10 arasındaki, Linux için McAfee ile v10 arasındaki ve Linux için Sophos v9. Güvenlik Merkezi bu çözümlerin herhangi birini algılarsa, Endpoint Protection 'ı yüklemenin önerisi artık görünmez.</td>
    <td class="tg-lboi"; width=55%>- Endpoint Protection sistem durumu hatalarının sanal makine ölçek kümelerinde düzeltilmelidir<br>- Endpoint Protection sistem durumu sorunları makinelerinizde çözümlenmelidir<br>- Uç nokta koruma çözümü, sanal makine ölçek kümelerine yüklenmelidir<br>- Uç nokta koruma çözümünü sanal makinelere yükler<br>- İzleme Aracısı sistem durumu sorunları makinelerinizde çözümlenmelidir<br>- İzleme Aracısı sanal makine ölçek kümelerine yüklenmelidir<br>- İzleme Aracısı makinelerinizde yüklü olmalıdır<br>- İzleme Aracısı sanal makinelerinizde yüklü olmalıdır<br>- Log Analytics Aracısı Windows tabanlı Azure Arc makinelerinizde yüklü olmalıdır (Önizleme)<br>- Log Analytics Aracısı, Linux tabanlı Azure yay makinelerinizde yüklü olmalıdır (Önizleme)<br>- Makinelerinize Endpoint Protection çözümünü yükler</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Denetim ve günlüğü etkinleştir (en fazla puan 1)</p></strong>Günlüğe kaydetme verileri, geçmiş sorunlara yönelik öngörüler sağlar, olası olanları önler, uygulama performansını iyileştirebilir ve aksi takdirde el ile gerçekleştirilen eylemleri otomatikleştirebilme yeteneği sağlar.<br>- <strong>Denetim ve yönetim günlükleri</strong> <a href="/azure/azure-resource-manager/management/overview">Azure Resource Manager</a> işlemler hakkında bilgi sağlar.<br>- <strong>Veri düzlemi günlükleri</strong> , Azure Kaynak kullanımının bir parçası olarak oluşturulan olaylar hakkında bilgi sağlar.<br>- <strong>İşlenen olaylar</strong> , işlenmiş olan çözümlenen olaylar/uyarılar hakkında bilgi sağlar.</td>
    <td class="tg-lboi"; width=55%>- SQL Server üzerinde denetim etkinleştirilmelidir<br>- Uygulama hizmetlerindeki tanılama günlükleri etkinleştirilmelidir<br>- Azure Data Lake Store tanılama günlükleri etkinleştirilmelidir<br>- Azure Stream Analytics tanılama günlükleri etkinleştirilmelidir<br>- Batch hesaplarındaki tanılama günlükleri etkinleştirilmelidir<br>- Data Lake Analytics tanılama günlükleri etkinleştirilmelidir<br>- Olay Hub 'ındaki tanılama günlükleri etkinleştirilmelidir<br>- IoT Hub tanılama günlükleri etkinleştirilmelidir<br>- Key Vault tanılama günlükleri etkinleştirilmelidir<br>- Logic Apps tanılama günlükleri etkinleştirilmelidir<br>- Arama hizmetindeki tanılama günlükleri etkinleştirilmelidir<br>- Service Bus tanılama günlükleri etkinleştirilmelidir<br>- Sanal makine ölçek kümelerindeki tanılama günlükleri etkinleştirilmelidir<br>- Toplu Iş hesaplarında ölçüm uyarısı kuralları yapılandırılmalıdır<br>- SQL denetim ayarları, kritik etkinlikleri yakalamak için yapılandırılmış Action-Groups sahip olmalıdır<br>- SQL Server 'lar 90 günden daha uzun denetim bekletme günlerindeki şekilde yapılandırılmalıdır.</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">Gelişmiş tehdit korumasını etkinleştir (en fazla puan 0)</p></strong>Azure Güvenlik Merkezi 'nin isteğe bağlı Azure Defender tehdit koruması planları, ortamınız için kapsamlı savunma sağlar. Güvenlik Merkezi, ortamınızın herhangi bir alanında bir tehdit algıladığında bir uyarı oluşturur. Bu uyarılar, etkilenen kaynakların ayrıntılarını, önerilen düzeltme adımlarını ve bazı durumlarda bir mantıksal uygulamayı yanıt olarak tetiklemeye yönelik bir seçenek anlatmaktadır.<br>Her Azure Defender planı, bu güvenlik denetimindeki ilgili öneriyi kullanarak etkinleştirebileceğiniz ayrı, isteğe bağlı bir tekliftir.<br><a href="/azure/security-center/threat-protection">Güvenlik Merkezi 'nde tehdit koruması hakkında daha fazla bilgi edinin</a>.</td>
    <td class="tg-lboi"; width=55%>- Azure SQL veritabanı sunucularında gelişmiş veri güvenliği etkinleştirilmelidir<br>- Makinelerdeki SQL sunucularında gelişmiş veri güvenliği etkinleştirilmelidir<br>- Gelişmiş tehdit koruması sanal makinelerde etkinleştirilmelidir<br>- Gelişmiş tehdit koruması Azure App Service planlarda etkinleştirilmelidir<br>- Gelişmiş tehdit koruması, Azure depolama hesaplarında etkinleştirilmelidir<br>- Gelişmiş tehdit koruması, Azure Kubernetes hizmet kümelerinde etkinleştirilmelidir<br>- Gelişmiş tehdit koruması Azure Container Registry kayıt defterlerinde etkinleştirilmelidir<br>- Gelişmiş tehdit koruması Azure Key Vault kasaları üzerinde etkinleştirilmelidir</td>
  </tr>
  <tr>
    <td class="tg-lboi"><strong><p style="font-size: 16px">En iyi güvenlik uygulamalarını uygulayın (en fazla puan 0)</p></strong>Modern güvenlik uygulamaları, ağ çevre ' ün ihlal olduğunu varsayar. Bu nedenle, bu denetimdeki en iyi uygulamalardan birçoğu kimlikleri yönetmeye odaklanmaktadır.<br>Anahtar ve kimlik bilgilerinin kaybolması yaygın bir sorundur. Anahtarları,. pfx dosyalarını ve parolaları şifreleyerek anahtarları ve gizli dizileri <a href="/azure/key-vault/key-vault-overview">Azure Key Vault</a> korur.<br>Sanal özel ağlar (VPN 'Ler), sanal makinelerinize erişmenin güvenli bir yoludur. VPN 'Ler kullanılamıyorsa, karmaşık parola ve <a href="/azure/active-directory/authentication/concept-mfa-howitworks">Azure AD Multi-Factor Authentication</a>gibi iki öğeli kimlik doğrulaması kullanın. İki öğeli kimlik doğrulaması, yalnızca Kullanıcı adları ve parolalara bağlı olan zayıf yanları önler.<br>Güçlü kimlik doğrulama ve yetkilendirme platformlarının kullanılması, başka bir en iyi uygulamadır. Federal kimliklerin kullanılması, kuruluşların yetkili kimliklerin yönetimini temsilciliğini sağlar. Bu, çalışanlar sonlandırıldığı ve erişiminin iptal edilmesi gereken durumlarda da önemlidir.</td>
    <td class="tg-lboi"; width=55%>- Aboneliğiniz için en fazla 3 sahip belirtilmelidir<br>- Okuma izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekir<br>- MFA, aboneliğinizde okuma izinleri olan hesaplarda etkinleştirilmelidir<br>- Güvenlik Duvarı ve sanal ağ yapılandırmalarına sahip depolama hesaplarına erişim kısıtlı olmalıdır<br>- RootManageSharedAccessKey hariç tüm yetkilendirme kuralları, Olay Hub 'ı ad alanından kaldırılmalıdır<br>- SQL sunucuları için bir Azure Active Directory Yöneticisi sağlanmalıdır<br>- Yönetilen örneklerinizdeki gelişmiş veri güvenliği etkinleştirilmelidir<br>- Olay Hub 'ı örneğindeki yetkilendirme kuralları tanımlanmalıdır<br>- Depolama hesaplarının yeni Azure Resource Manager kaynaklarına geçirilmesi gerekir<br>- Sanal makinelerin yeni Azure Resource Manager kaynaklara geçirilmesi gerekir<br>- Alt ağlar bir ağ güvenlik grubuyla ilişkilendirilmelidir<br>- Önizle Windows Exploit Guard etkinleştirilmelidir <br>- Önizle Konuk yapılandırma aracısının yüklü olması gerekir<br>- İnternet 'e yönelik olmayan sanal makineler ağ güvenlik gruplarıyla korunmalıdır<br>- Sanal makineler için Azure Backup etkinleştirilmelidir<br>- MariaDB için Azure veritabanı 'nda coğrafi olarak yedekli yedekleme etkinleştirilmelidir<br>- MySQL için Azure veritabanı 'nda coğrafi olarak yedekli yedekleme etkinleştirilmelidir<br>- PostgreSQL için Azure veritabanı için coğrafi olarak yedekli yedekleme etkinleştirilmelidir<br>- PHP, API uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>- PHP, Web uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>- Java, API uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>- Java, işlev uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>- Java, Web uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>- Python, API uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>- Python, işlev uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>- Python, Web uygulamanız için en son sürüme güncelleştirilmeleri gerekir<br>- SQL sunucuları için denetim bekletme en az 90 gün olarak ayarlanmalıdır</td>
  </tr>
</tbody>
</table>


</div>




## <a name="secure-score-faq"></a>Güvenli puan SSS

### <a name="if-i-address-only-three-out-of-four-recommendations-in-a-security-control-will-my-secure-score-change"></a>Bir güvenlik denetimindeki dört önerimin yalnızca üç tanesi ele alıyorsa, güvenli puanımı değişmem gerekir mi?
Hayır. Tek bir kaynağın tüm önerilerini düzeltene kadar değişmez. Bir denetim için en fazla puanı almak üzere tüm kaynaklar için tüm önerileri düzeltmeniz gerekir.

### <a name="if-a-recommendation-isnt-applicable-to-me-and-i-disable-it-in-the-policy-will-my-security-control-be-fulfilled-and-my-secure-score-updated"></a>Öneri bana uygulanabilir değilse ve ilkede devre dışı bıraktıktan sonra güvenlik denetimin yerine getirilmesi ve güvenli puanım güncellenmem gerekir mi?
Evet. Ortamınızda uygulandıklarında önerilerin devre dışı bırakılmasını öneririz. Belirli bir öneriyi devre dışı bırakma hakkında yönergeler için bkz. [güvenlik Ilkelerini devre dışı bırakma](./tutorial-security-policy.md#disable-security-policies-and-disable-recommendations).

### <a name="if-a-security-control-offers-me-zero-points-towards-my-secure-score-should-i-ignore-it"></a>Güvenlik denetimi, güvenli puanma doğru bir şekilde sıfır puan sunduğunda, bunu yoksaymalıdır mi?
Bazı durumlarda, en yüksek puanı sıfırdan büyük bir denetim görürsünüz, ancak etki sıfırdır. Kaynakları düzeltmeye yönelik artımlı puan göz ardı edildiğinde sıfıra yuvarlanır. Bu önerileri, hala güvenlik geliştirmeleri getirdiklerinde yok saymayın. Tek özel durum, "ek En Iyi Yöntem" denetimidir. Bu önerilerin yapılması puanınızı artırmaz, ancak genel güvenliğinizi geliştirir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, güvenli puanı ve tarafından sunulan güvenlik denetimleri açıklanmaktadır. İlgili malzemeler için aşağıdaki makalelere bakın:

- [Önerinin farklı öğeleri hakkında bilgi edinin](security-center-recommendations.md)
- [Önerileri nasıl düzelteceğinizi öğrenin](security-center-remediate-recommendations.md)
- [Güvenli puanla programlama yoluyla çalışmaya yönelik GitHub tabanlı araçları görüntüleyin](https://github.com/Azure/Azure-Security-Center/tree/master/Secure%20Score)