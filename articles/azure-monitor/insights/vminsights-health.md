---
title: İzleme sanal makine durumu Azure İzleyici ile VM'ler için (Önizleme) | Microsoft Docs
description: Bu makalede, VM'ler için Azure İzleyici ile temel işletim sistemi ve sanal makine durumunu anlamak nasıl açıklar.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/01/2019
ms.author: magoedte
ms.openlocfilehash: f61f420b6a738a410deed2d68acc06862600104f
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2019
ms.locfileid: "55563346"
---
# <a name="understand-the-health-of-your-azure-virtual-machines-with-azure-monitor-for-vms-preview"></a>VM'ler (Önizleme) için Azure İzleyici ile Azure sanal makinelerinizin durumunu anlama
Azure İzleme alanı ayrı ayrı bir spesifik rol ya da görev gerçekleştiren birden çok hizmet içerir, ancak bir Azure sanal makinelerinde barındırılan işletim sistemi ayrıntılı sistem durumu açısından sağlama kullanılabilir değildi.  Log Analytics veya Azure İzleyicisi'ni kullanarak için farklı koşullar izleyebilir olsa da bunlar model ve sistem durumunu temel bileşenler veya genel sanal makine durumunu temsil eden üzere tasarlanmamıştır.  VM sistem durumu özelliği için Azure İzleyici ile proaktif olarak Windows veya Linux konuk işletim sistemi ile anahtar bileşenleri ve bu durumunu ölçmek nasıl belirten ölçütleri ilişkilerini temsil eden bir model performansını ve kullanılabilirliğini izler bileşenleri ve iyi durumda olmayan bir koşul algılandığında sizi uyarır.  

Azure VM genel sistem durumunu görüntüleme ve işletim sistemi temel Vm'leri sistem durumu, sanal makineden doğrudan veya Azure İzleyici kaynak grubunun içindeki tüm sanal makineleri için Azure İzleyici ile iki yönlerden gösterilebilir.

Bu makalede hızlı bir şekilde değerlendirmek, araştırmanıza ve algılanan sistem durumu sorunları gidermek nasıl anlamanıza yardımcı olur.

VM'ler için Azure İzleyici yapılandırma hakkında daha fazla bilgi için bkz: [VM'ler için Azure İzleyici'ı etkinleştirme](vminsights-onboard.md).

>[!NOTE]
>Başlangıç tarihi: 11 Şubat 2019 sistem tanılama deneyimi yeni bir sistem durumu modeli sürümüne bugün olduğunuzda, görünür olan Vm'leri sistem durumu özelliği için geçerli sistem durumu modeli Azure İzleyici'de geçiş başlayacağız. Bu güncelleştirme, sistem durumu toplaması işleme performansını geliştirir ve sistem durumu tanılama Görünümü'nde sunulan daraltılmış sistem durumu modeli içerir. 
>
>Yeni sistem durumu modeli ile daha hızlı ve sonuç olarak, sistem durumunu daha az gecikme süresiyle istenen veya hedeflenen durumuna üst güncelleştirmeleri varlık başına üst düzey durumu ölçütlerini için alt sistem durumu ölçütlerin toplaması olacaktır. Sistem durumu ölçütleri altında hala filtreleyebilirsiniz **performans** ve **kullanılabilirlik** ya da kategori görünümünde seçmek için önceki sekme tabanlı yöntemi farklı kategorileri.
>
>Sistem durumu tanılama deneyimi hakkında daha fazla ayrıntı için lütfen sistem tanılama bakın [bölümü](#health-diagnostics) bu makaledeki. 
>
>Bu güncelleştirme aşağıdaki iyileştirir: 
>
>- Düşük gecikme süresiyle işleme sistem durumu toplaması  
>- Daha hızlı sağlık durumu değişikliklerinde üzerinde uyarı 
>- Sistem durumu toplu sanal makine görünümünde tüm sanal makineler için daha hızlı güncelleştirme 
>
>Bugün Azure İzleyici sistem durumu özelliği ile sanal makineleri için sunulan tüm işlevlerin gerileme vardır.

>Bu değişikliğe bağlı olarak, hizmet ve sistem durumu geçmişi zaman kısa bir süre için bazı kesintisi olacaktır. Sistem durumu tanılama iki deneyimlerinde etkilenen - durum değişikliği geçmişi sıfırlanır ve durumu ölçütlerini için önceki durum değişikliklerini gözden geçirme için sistem durumu tanılama sayfası durum değişikliği sütununda kullanılabilir olmaz. Herhangi bir geçmiş verilerin ilgileniyorsanız, sistem durumu ölçütlerini verileri ve karşılık gelen durum değişikliklerini görüntüsü başvuru için uygulayabileceğiniz sonra kritik VM görev. 

## <a name="monitoring-configuration-details"></a>İzleme Yapılandırma Ayrıntıları
Bu bölümde, Azure Windows ve Linux sanal makinelerini izlemek için tanımlanan varsayılan sistem durumu ölçütlerini özetlenmektedir. Tüm sistem durumu ölçütlerini, sağlıksız koşul karşılandığında uyarı önceden yapılandırılmış. 

### <a name="windows-vms"></a>Windows VM'leri

- Kullanılabilir megabayt belleği 
- Ortalama Disk yazma (Mantıksal Disk) saniyede
- Ortalama Disk yazma (Disk) saniyede
- Okuma başına ortalama Mantıksal Disk saniye
- Aktarım başına ortalama Mantıksal Disk saniye
- Okuma başına ortalama Disk saniye
- Aktarım başına ortalama Disk saniye
- Geçerli Disk Sırası Uzunluğu (Mantıksal Disk)
- Geçerli Disk Sırası Uzunluğu (Disk)
- Disk yüzde boşta kalma süresi
- Dosya sistemi hatası veya Bozulması
- Mantıksal Disk boş alan (%) Düşük
- Mantıksal Disk boş alan (MB) düşük
- Mantıksal Disk boş kalma süresi yüzdesi
- Saniye başına bellek sayfaları
- Yüzde bant kullanılan okuma
- Toplam yüzde bant kullanılan
- Yüzde bant kullanılan yazma
- Kullanımdaki kaydedilmiş bellek yüzdesi
- Fiziksel Disk yüzde boşta kalma süresi
- DHCP istemci hizmeti durumu
- DNS İstemcisi hizmeti durumu
- RPC hizmeti durumu
- Sunucu hizmeti durumu
- Toplam CPU kullanım yüzdesi
- Windows olay günlüğü hizmeti durumu
- Windows Güvenlik Duvarı hizmeti durumu
- Windows Uzaktan Yönetimi hizmetinin sistem durumu

### <a name="linux-vms"></a>Linux VM'leri
- Disk ortalaması Disk sn/Aktarım 
- Disk ortalaması Disk sn/Okuma 
- Disk ortalaması Disk sn/yazma 
- Disk durumu
- Mantıksal Disk boş alan
- Mantıksal Disk % boş alan
- Mantıksal Disk % boş Inode'ları
- Ağ bağdaştırıcısı durumu
- İşlemci yüzde DPC Zamanı
- İşlemci yüzde işlemci zamanı
- Toplam yüzde işlemci zamanı
- Toplam yüzde DPC Zamanı
- İşletim sistemi kullanılabilir megabayt belleği

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
[Azure Portal](https://portal.azure.com) oturum açın. 

## <a name="introduction-to-health-experience"></a>Sistem durumu deneyimi giriş
Tek sanal makine veya VM için sistem durumu özelliğini kullanarak girmeden önce kısa bir giriş sağladığımız bilgileri nasıl görüntülenir ve görselleştirmeler ne temsil anlamak için önemlidir.  

## <a name="view-health-directly-from-a-virtual-machine"></a>Bir sanal makineden doğrudan durumunu görüntüle 
Bir Azure VM durumunu görüntülemek için seçin **Insights (Önizleme)** sol bölmesinde sanal makinenin. VM içgörüler sayfasında **sistem durumu** varsayılan olarak açıktır ve VM sistem durumu görünümünü gösterir.  

![Vm'leri sistem durumuna genel bakış seçili Azure sanal makine için Azure İzleyici](./media/vminsights-health/vminsights-directvm-health.png)

Üzerinde **sistem durumu** bölümünde sekmesinde **Konuk VM sistem durumu**, tablo, sanal makinenizin geçerli sistem durumunu gösterir ve VM sistem durumu uyarılarını toplam sayısı, iyi durumda olmayan bir bileşen tarafından oluşturuldu. Başvurmak [uyarılar](#alerting-and-alert-management) uyarı deneyimi hakkında ayrıntılı bilgi için.  

Bir VM için tanımlanan sistem durumları aşağıdaki tabloda açıklanmıştır: 

|Simge |Sistem durumu |Anlamı |
|-----|-------------|------------|
| |Sorunsuz |VM için algılanan bir sorun olmadığını gösteren tanımlanan sistem durumu koşulları içinde ve gerektiği gibi çalıştığını sistem durumunu iyi durumda. Üst döküm İzleyicisi söz konusu olduğunda, sistem durumu dökümü yapar artırma ve alt best-case veya iki katına durumunu yansıtır.|
| |Kritik |Sistem durumu, bir veya daha fazla kritik sorunlar, normal işlevlerin geri yüklenmesi için ele alınması gereken göstergelerinin algılandığını belirten tanımlanan sistem durumu koşulu içinde değilse önemlidir. Üst döküm İzleyicisi söz konusu olduğunda, sistem durumu dökümü yapar artırma ve alt best-case veya iki katına durumunu yansıtır.|
| |Uyarı |Sistem durumu uyarı burada gösterir tanımlanan sistem durumu koşulu, iki eşikleri arasında olup olmadığını bir *uyarı* durumu ve diğer gösteren bir *kritik* (üç sağlık durumu eşikleri için durumu yapılandırılabilir), veya ne zaman kritik olmayan bir sorun algılandığında çözümlenmedi, kritik sorunlarına neden. Söz konusu olduğunda bir üst döküm İzleyicisi, bir veya daha alt bir uyarı durumunda, ardından üst yansıtır *uyarı* durumu. İçinde bir alt varsa bir *kritik* ve başka bir alt bir *uyarı* durumu, üst toplama sistem durumu gösterilir *kritik*.|
| |Bilinmeyen |Sistem durumu konusu bir *bilinmeyen* pek çok nedenden dolayı sistem durumu gibi hesaplanamıyor olduğunda durumu verileri, hizmeti başlatılmamış toplamak kullanabilirsiniz. Bu sistem durumu yapılandırılabilir değildir.| 

Seçme **görüntüleme durumu tanılama** VM, ilişkili durumu ölçütlerini, durum değişikliklerini ve diğer önemli sorunları VM'le ilgili bileşenler izlenerek karşılaşıldı tüm bileşenlerini gösteren bir sayfa açar. Daha fazla bilgi için [sistem tanılama](#health-diagnostics). 

Altında **bileşen sistem durumu** bölümü, tablo gösterir sistem durumu toplaması durumunu özellikle bu alanların sistem durumu ölçütlerini tarafından izlenen birincil performans kategorileri **CPU**,  **Bellek**, **Disk**, ve **ağ**.  Bileşenleri herhangi birini seçerek, tüm yönlerini söz konusu bileşen ve her birinin kendi sistem durumu izleme tek tek sistem durumu ölçütü listeleyen bir sayfa açılır.  

Sistem durumu Windows işletim sistemini çalıştıran bir Azure VM'den erişirken, en çok kullanılan 5 çekirdek Windows Hizmetleri sistem durumunu gösterilen bölümü altında **çekirdek sistem durumu Hizmetleri**.  Hizmetlerden herhangi birini seçtiğinizde, söz konusu bileşen ve sistem durumunu izleme durumu ölçütlerini listelendiği bir sayfa açılır.  Sistem durumu ölçütlerini adına tıklayarak özellik bölmesi açılır ve burada tanımlanan karşılık gelen bir Azure İzleyici uyarı durumu ölçütlerini varsa dahil olmak üzere yapılandırma ayrıntılarını gözden geçirebilirsiniz. Daha fazla bilgi için bkz. [sistem durumu tanılama ve çalışma durumu ölçütlerini](#health-diagnostics).  

## <a name="aggregate-virtual-machine-perspective"></a>Toplam sanal makine perspektifi
Portalı Gezinti listeden bir kaynak grubundaki tüm sanal makinelerinizin için sistem durumu toplama görüntülemek için seçin **Azure İzleyici** seçip **sanal makineler (Önizleme)**.  

![Azure İzleyici görünümünden izleme VM öngörüleri](./media/vminsights-health/vminsights-aggregate-health.png)

Gelen **abonelik** ve **kaynak grubu** açılan listeler, sanal makineleri içeren uygun kaynak grubu seçin ilgili gruba bildirilen sistem durumlarını görüntülemek için.  Seçiminiz yalnızca sistem durumu özellik için geçerlidir ve performans ya da harita taşımaz.

Üzerinde **sistem durumu** sekmesinde tarafından aşağıdaki öğrenin:

* Kaç tane Vm'niz ne kadar sağlam veya değil gönderen veri (Bilinmeyen durumu adlandırılır) olduğunu ve kritik veya sağlıksız durumda misiniz?
* Hangi sanal makinelerin işletim sistemi (OS) tarafından düzgün çalışmayan bir durum raporlama ve kaç?
* Kaç tane Vm'niz bir işlemci, disk, bellek veya ağ bağdaştırıcısı, sistem durumuna göre kategorilere algılanan bir sorun nedeniyle sağlıksız misiniz?  
* Kaç tane Vm'niz hizmetiyle sistem durumuna göre kategorilere ayrılmış bir çekirdek işletim sistemi, algılanan bir sorun nedeniyle sağlıksız misiniz?

Burada hızlı, proaktif olarak VM izleme durumu ölçütlerini tarafından algılanan en kritik sorunları belirlemek ve VM sistem durumu uyarı ayrıntılarını gözden geçirin ve tanılama ve düzeltme sorunun yardımcı olmak ilişkili Bilgi Bankası makalesi yöneliktir.  Açmak için önem derecelerinin birini seçin [tüm uyarıları](../../azure-monitor/platform/alerts-overview.md#all-alerts-page) sayfası, önem derecesine göre filtrelendi.

**İşletim sistemine göre VM Dağıtım** listede Windows sürümünü veya sürümlerini birlikte Linux dağıtımı tarafından listelenen VM'ler gösterilmektedir. Her işletim sistemi kategorisinde, Vm'leri başka ayrılmıştır VM Durumu'na göre. 

![VM Insights sanal makine dağıtım perspektifi](./media/vminsights-health/vminsights-vmdistribution-by-os.png)

Herhangi bir sütun öğe üzerinde - tıklayabilirsiniz **VM sayısı**, **kritik**, **uyarı**, **sağlıklı** veya **bilinmeyen** için Detaya **sanal makineler** sayfa seçilen sütunun eşleşen filtrelenmiş sonuç listesini görürsünüz. Örneğin, çalışan tüm sanal makineler gözden geçirmek istiyorsanız **Red Hat Enterprise Linux sürüm 7.5**, tıklayarak **VM sayısı** değeri, işletim sistemi ve şu sayfaya eşleşen sanal makineler listesi açılır Bu filtre ve şu anda bilinen sistem durumu.  

![Red Hat Linux sanal makinelerinin örnek paketi](./media/vminsights-health/vminsights-rollup-vm-rehl-01.png)
 
Üzerinde **sanal makineler** sayfa adı sütunu altında bir sanal Makineyi seçerseniz **VM adı**, uyarıların daha fazla ayrıntı içeren VM örneği sayfasına yönlendirilir ve sistem durumu ölçütlerini sorunlar tanımlandı olan Seçili VM etkileyen.  Buradan, sistem durumu ayrıntıları tıklayarak filtreleyebilirsiniz **sistem durumu** hangi bileşenlerin sağlıksız olduğunu görmek için sayfayı veya üst sol köşesindeki simgeyi sağlıksız bir bileşen tarafından gerçekleştirilen VM sistem durumu uyarılarını görüntüleyebilirsiniz Uyarı önem derecesine göre kategorilere ayrılmıştır.    

Bir sanal makine adına tıklayarak VM liste görünümünde açılır **sistem durumu** seçtiyseniz, sayfa için VM, benzer şekilde seçildi. **Insights (Önizleme)** VM'den doğrudan.

![Seçili Azure sanal makinesinin VM öngörüleri](./media/vminsights-health/vminsights-directvm-health.png)

Burada, bir toplama gösterir **sistem durumu** sanal makine için ve **uyarılar**, temsil eden sistem durumu için sağlıksız için sistem durumu değiştiğinde harekete geçirilen VM sistem durumu Uyarıları önem derecesine göre kategorilere ayrılmış bir Sistem durumu ölçütleri.  Seçme **kritik durumdaki VM'ler** bir kritik sağlık durumunda bir veya daha fazla VM'lerin listesini içeren bir sayfa açılır.  Listedeki Vm'lerden birinin göstermek için durumunu tıklayarak **sistem tanılama** VM görünümü.  Burada, hangi durumu ölçütlerini yansıtan çıkış bir sistem durumu sorunu bulabilirsiniz. Zaman **sistem tanılama** sayfası açıldığında, VM ve bunların ilişkili sistem durumu ölçütlerini geçerli bir sağlık durumundaki tüm bileşenleri gösterir.  Başvurmak [sistem durumu tanı](#health-diagnostics) ayrıntılı bilgi için.  

Seçme **tüm sistem durumu ölçütlerini görüntülemek** bu özellik ile kullanılabilir tüm sistem durumu ölçütlerini listesini gösteren bir sayfa açar.  Bilgileri daha fazla bağlı olarak aşağıdaki seçenekleri filtrelenebilir:

* **Tür** – koşulları değerlendirin ve izlenen VM genel sistem durumu geri ölçüt türü üç tür durum vardır.  
    a. **Birim** – sanal makine bazı yönlerini ölçer. Bu sistem durumu ölçüt türü bileşeninin performansı belirlemek için bir performans sayacı yapay bir işlem gerçekleştirmek veya hata veren bir olayı izlemek için bir betik çalıştırarak denetimi yapıyor olabilir.  Varsayılan olarak, filtre birimine ayarlanır.  
    b. **Bağımlılık** -farklı varlıklar arasındaki sistem durumu toplaması sağlar. Bu durumu ölçütlerini başka bir tür için başarılı bir işlem kullanır varlık durumunu bağımlı bir varlık durumunu sağlar.  
    c. **Toplama** -benzer durumu ölçütlerini birleşik sistem durumu sağlar. Birim ve bağımlılık sistem durumu ölçütü, genellikle bir toplama sistem durumu ölçütü altında yapılandırılır. Bir varlıkta hedeflenen birçok farklı durumu ölçütlerini daha iyi bir genel düzenleme sağlamanın yanı sıra toplama sistem durumu ölçütü varlıkların farklı kategorileri için benzersiz bir sistem durumu sağlar.

* **Kategori** -durumu ölçütlerini raporlama amacıyla benzer bir tür ölçütünü gruplamak için kullanılan tür.  Bunlar **kullanılabilirlik** veya **performans**.

Hangi örneklerinin altındaki bir değere tıklayarak sağlıksız olduğunu görmek için aşağıya inebilir **sağlıksız bileşen** sütun.  Sayfasında, bir tablo bir kritik sağlık durumunda bileşenlerini listeler.    

## <a name="health-diagnostics"></a>Sistem durumu tanılama
Thge **sistem tanılama** sayfası, sistem durumu modeli, sanal makinenin tüm bileşenleri listeleyen bir sanal makinenin görselleştirmenizi durumu ölçütlerini, durum değişikliklerini ilişkili ve ilgili bileşenler tarafından tanımlanan diğer önemli sorunları izlenen sağlar VM.

![Bir sanal makine için sistem durumu tanılama sayfası örneği](./media/vminsights-health/health-diagnostics-page-01.png)

Sistem durumu tanılama aşağıdaki yollarla başlatabilirsiniz.

* Azure İzleyici'de toplama VM açısından tüm sanal makineler için toplama sistem durumuna göre.  Üzerinde **sistem durumu** sayfasında, simgesine tıklayarak **kritik**, **uyarı**, **sağlıklı**, veya **bilinmeyen** Sistem durumu bölümünün altında **Konuk VM sistem durumu** ve tüm sanal makineleri listeler sayfanın Veriler'i eşleşen filtrelenmiş kategori.  Değer tıklayarak **sistem durumu** sütun, sistem durumu, belirli bir VM için kapsamlı tanılama açılır.      

* Toplam VM açısından Azure İzleyici'de işletim sistemi tarafından. Altında **VM Dağıtım**, sütun değerlerini herhangi birini seçerek açılır **sanal makineler** sayfasında ve filtrelenmiş kategori eşleşen tabloda bir liste döndürür.  Değerin altında tıklayarak **sistem durumu** sütun, seçili VM için sistem durumu tanılama açılır.    
 
* Konuk sanal makinesinde sanal makineler için Azure İzleyici'den **sistem durumu** sekmesini seçerek **görüntüleme durumu tanılama** 

Sistem durumu tanılama sistem durumu bilgilerini şu kategorilere göre düzenler: 

* Kullanılabilirlik
* Performans
 
Mantıksal disk, CPU gibi belirli bir bileşen için tanımlanan tüm sistem durumu ölçütlerini vs. Ayrıca, İzleyici kategorisini eklentisini yanındaki görülebilir **durumu ölçütlerini** sütun.  

Durumu ölçütlerini durumunu tanımlanan dört durumun – biri tarafından *kritik*, *uyarı*, *sağlıklı*, ve *bilinmeyen*. İlk üç eşik değerleri kullanarak izleyicilerin değiştirebileceğiniz anlamına gelir, yapılandırılabilir [iş yükü İzleyicisi API](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/monitors/update). *Bilinmeyen* yapılandırılabilir ve belirli senaryolar için ayrılmış değil.  

Sistem durumu tanılama sayfası üç ana bölümü vardır:

* Bileşen Modeli 
* Durum Ölçütleri
* Durum Değişiklikleri 

![Sistem durumu tanılama sayfası bölümleri](./media/vminsights-health/health-diagnostics-page-02.png)

### <a name="component-model"></a>Bileşen modeli
Sistem durumu tanılama sayfası en soldaki sütunda bileşen modelidir. VM ile ilişkili tüm bileşenleri bu sütun geçerli sistem durumları ile birlikte görüntülenir. 

Aşağıdaki örnekte bulunan disk, mantıksal disk, işlemci, bellek ve işletim sistemi bileşenlerdir. Bu bileşenlerin birden çok örneği bulundu ve bu sütunda görüntülenir. Örneğin, aşağıdaki görüntüde VM mantıksal disk - sistem durumu iyi olan C: ve D:, üç örnek olduğunu gösterir.  

![Sistem durumu tanılamada sunulan örnek bileşen modeli](./media/vminsights-health/health-diagnostics-page-component.png)

### <a name="health-criteria"></a>Sistem durumu ölçütü
Sistem durumu tanılama sayfası merkezi sütunda **durumu ölçütlerini** sütun. VM için tanımlanan sistem durumu modeli, hiyerarşik bir ağaç şeklinde görüntülenir. Bir sanal makine için sistem durumu modeli, birim ve toplama durumu ölçütlerini oluşur.  

![Sistem durumu tanılamada sunulan örnek durumu ölçütü](./media/vminsights-health/health-diagnostics-page-healthcriteria.png)

Sistem durumu ölçüt bir varlık, vb. durum Eşiği değeri olabilecek bazı ölçütleri ile izlenen örnek sağlığını ölçer. Bir sistem durumu ölçütü, daha önce açıklandığı gibi iki veya üç yapılandırılabilir sağlık durumu eşikleri sahiptir. Belirli bir noktada, sistem durumu ölçütü potansiyel durumlarının sadece birinde olabilir. 

Bir hedef genel durumunu her sistem durumu modeli içerisinde belirlenen sistem durumu ölçütlerini sistem durumuna göre belirlenir. Bu hedefte doğrudan hedeflenen durumu ölçütlerini durumu ölçütlerini hedef toplama sistem durumu ölçütü aracılığıyla çalışırken bileşenleri hedeflenmiş bir birleşimi olacaktır. Bu hiyerarşi gösterilmiştir **durumu ölçütlerini** sistem tanılama sayfası bölümü. Sistem durumu Toplama İlkesi toplama durumu ölçütlerini yapılandırmasının bir parçasıdır (varsayılan ayarı *en kötü,*). Bölümü altında bu özelliği bir parçası olarak çalışan sistem durumu ölçütlerini varsayılan kümesini listesini bulabilirsiniz [izleme yapılandırma ayrıntılarını](#monitoring-configuration-details).  

**Birim** sistem durumu ölçüt türü yapılandırmalarını kadar doğru ve seçme elips bağlantısına tıklayarak değiştirilmiş olabilir **ayrıntıları göster** yapılandırma bölmesini açmak için. 

![Bir sistem durumu ölçütlerini örnek yapılandırma](./media/vminsights-health/health-diagnostics-vm-example-02.png)

Seçili durumu ölçütlerini yapılandırma bölmesinde örneğinde **Disk başına saniyede ortalama yazma**, eşiğini farklı bir sayısal değer ile yapılandırılabilir. Bu, yalnızca değişikliklerden sağlıklı uyarı anlamına gelir, bir iki durumlu İzleyici olur. Diğer sistem durumu ölçütü üç durumu, uyarı ve kritik sistem durumu durum eşiğinin değeri yapılandırabileceğiniz olabilir.  

>[!NOTE]
>Bir örneği için sistem durumu ölçütlerini yapılandırma değişikliklerini uygulama, izlenen tüm örneklerine uygulanır.  Örneğin, **-1 D: Disk** ve değiştirme **Disk başına saniyede ortalama yazma** eşik, yalnızca bu örneği için geçerli değildir, ancak diğer tüm disk örnekleri bulunan ve sanal makinede izlenen.
>

![Bir sistem durumu ölçütlerini bir birim İzleyicisi örneği yapılandırma](./media/vminsights-health/health-diagnostics-criteria-config-01.png)

Bilgi Bankası makaleleri, sistem durumu göstergesi hakkında daha fazla bilgi edinmek istiyorsanız, sorunları, nedenleri ve çözümlemeleri tanımlamanıza yardımcı olması için dahil edilir. Tıklayarak **bilgilerini görüntüleyin** sayfasındaki bağlantısını ve belirli bir Bilgi Bankası makalesi gösteren, tarayıcınızda yeni bir sekmede açılır. Herhangi bir zamanda tüm Vm'leri sistem durumu özelliği için Azure İzleyici ile dahil sistem durumu ölçütü Bilgi Bankası makalelerini inceleyebilirsiniz [burada](https://docs.microsoft.com/azure/monitoring/infrastructure-health/).
  
### <a name="state-changes"></a>Durum değişiklikleri
Sistem durumu Tanılama sayfasında en sağdaki sütunda **durum değişikliklerini**. Seçili durumu ölçütlerini ilişkilendirilmiş tüm durum değişikliklerini listeler **durumu ölçütlerini** bölüm veya bir VM'ye gelen seçildiyse sanal makinenin durumu değişikliği **bileşen modeli** veya **Durumu ölçütlerini** tablosunda sütun. 

![Sistem durumu tanılamada sunulan örnek durum değişiklikleri](./media/vminsights-health/health-diagnostics-page-statechanges.png)

Bu bölümde durumu ölçütlerini ve üstte en son durumuna göre sıralanmış ilişkili zaman oluşur.   

### <a name="association-of-component-model-health-criteria-and-state-change-columns"></a>Bileşen modeli, durumu ölçütlerini ve durumu ilişkiyi değiştirmek sütunları 
Üç sütun birbirleri ile birbirine bağlıdır. Bulunan bir örneğinde seçtiğinizde **bileşen modeli** bölümünde **durumu ölçütlerini** bölüm o bileşen görünüme filtre ve gelenlere **durum değişikliği**bölümü, seçilen sistem durumu ölçütlere göre güncelleştirilir. 

![İzlenen örneği ve sonuçları seçme örneği](./media/vminsights-health/health-diagnostics-vm-example-01.png)

Yukarıdaki örnekte seçtiğinizde **Disk - 1 D:**, durumu ölçütlerini ağaç filtrelenmiştir **Disk - 1 D:**. **Durum değişikliği** sütun kullanılabilirliğine bağlı durum değişikliği gösteren **Disk - 1 D:**. 

Güncelleştirilmiş bir durumu görmek için sistem durumu tanılama sayfası tıklayarak yenileyebilirsiniz **Yenile** bağlantı.  Önceden tanımlanmış bir yoklama aralığı temel sistem durumu ölçütü'nın sistem durumu için bir güncelleştirme varsa, bu görevi bekleme sürelerinden kurtulun sağlar ve en son sistem durumu yansıtır.  **Durumu ölçütlerini** filtre vermektir - seçili sistem durumuna bağlıdır sonuçları kapsam için *sağlıklı*, *uyarı*, *kritik*, *Bilinmeyen*, ve *tüm*.  **Son güncelleştirilen** sağ üst köşedeki zaman sistem durumu tanılama sayfası ne zaman yenilendiğini son zamanı temsil eder.  

## <a name="alerts"></a>Uyarılar
VM sistem durumu özelliği için Azure İzleyici ile tümleştirilir [Azure uyarıları](../../azure-monitor/platform/alerts-overview.md) ve koşul algılandığında önceden tanımlanmış durumu ölçütlerini sağlıklıdan için kötü bir durum değiştiğinde bir uyarı başlatır. Uyarı önem derecesi - önem derecesi 0 ile 4, önem derecesi en yüksek önem derecesine temsil eden 0 ile tarafından kategorilere ayrılmıştır.  

VM sistem durumu Uyarıları önem derecesine göre kategorilere toplam sayısı, üzerinde kullanılabilir **sistem durumu** bölümünde Pano **uyarılar**. Toplam uyarı sayısını veya bir önem derecesi düzeyine karşılık gelen sayısı seçtiğinizde **uyarılar** sayfası açılır ve seçiminizi eşleşen tüm uyarıları listeler.  Örneğin, satır karşılık gelen seçtiyseniz, **önem derecesi düzeyi 1**, aşağıdaki görmek sonra:

![Tüm önem düzeyi 1 uyarı örneği](./media/vminsights-health/vminsights-sev1-alerts-01.png)

Üzerinde **uyarılar** sayfası, yalnızca sizin seçiminiz eşleşen uyarıları göstermek için kapsamında değil, ancak göre filtrelenir **kaynak türü** yalnızca sanal makine kaynak tarafından gerçekleştirilen sistem durumu uyarılarını göstermek için.  Bu sütunu altında bir uyarı listesinden yansıtılan **hedef kaynak**, burada Azure uyarı tetiklendi için özel durumu ölçütlerini 's sağlıksız koşul sağlandığında VM gösterir.  

Bu görünümde dahil edilecek diğer kaynak türlerini veya hizmetler uyarılardan amaçlanmayan, günlük uyarıları bağlı Log Analytics temelinde sorguları veya ölçüm uyarıları gibi Azure İzleyici varsayılan olarak normal şekilde görüntülediğiniz [tüm uyarıları](../../azure-monitor/platform/alerts-overview.md#all-alerts-page) sayfası. 

Bu görünümde, sayfanın üst kısmındaki açılan menüler, değerleri seçerek filtreleyebilirsiniz.

|Sütun |Açıklama | 
|-------|------------| 
|Abonelik |Bir Azure aboneliği seçin. Yalnızca seçili Abonelikteki uyarılar görünümünde dahil edilir. | 
|Kaynak Grubu |Tek bir kaynak grubu seçin. Yalnızca seçilen kaynak grubunun hedefleri olan uyarılar görünümünde dahil edilir. | 
|Kaynak türü |Bir veya daha fazla kaynak türlerini seçin. Varsayılan olarak, yalnızca hedef uyarıları **sanal makineler** seçilir ve bu görünüme dahil. Bu sütun, yalnızca bir kaynak grubu belirttikten sonra kullanılabilir. | 
|Kaynak |Bir kaynak seçin. Yalnızca bu kaynak bir hedef olarak uyarılarla Görünümü'nde dahil edilir. Bu sütun, yalnızca bir kaynak türünü belirttikten sonra kullanılabilir. | 
|Severity |Uyarı önem seçmeniz ya da seçin *tüm* her türlü önem derecesi, uyarı eklenecek. | 
|İzleme Koşulu |Uyarıları filtrelemek için bir izleme koşulu verilmiş olması durumunda seçin *Fired* sistem tarafından veya *çözümlenmiş* koşul artık etkin olduğunda sistem tarafından. Veya *tüm* tüm koşulların uyarıları eklemek için. | 
|Uyarı durumu |Bir uyarı durumu seçin *yeni*, *kabul*, *kapalı*, ya da seçin *tüm* tüm durumların uyarıları eklemek için. | 
|İzleme hizmet |Bir hizmet veya seçin, *tüm* tüm hizmetleri dahil etmek için. Yalnızca gelen uyarılar *VM Insights* bu özellik için desteklenir.| 
|Zaman aralığı| Yalnızca seçili zaman penceresi içinde tetiklenen uyarılar görünümünde dahil edilir. Desteklenen değerler şunlardır: son bir saat, son 24 saat, son 7 günde ve son 30 gün. | 

**Uyarı ayrıntısı** sayfası, bir uyarı durumuna değiştirmenize olanak tanır ve uyarının ayrıntılarını sağlayan seçtiğinizde görüntülenir. Uyarıları yönetme hakkında daha fazla bilgi için bkz: [oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak Uyarıları yönetme](../../azure-monitor/platform/alerts-metric.md).  

>[!NOTE]
>Bu süre, sistem durumu ölçütleri temel alarak yeni uyarılar oluşturma veya var olan sistem durumu değiştirme Portalı'ndan Azure İzleyicisi'nde uyarı kuralları desteklenmez.  
>

![Seçilen bir uyarının uyarı ayrıntıları bölmesine](./media/vminsights-health/alert-details-pane-01.png)

Uyarı durumu da değiştirilebilir bir veya birden çok uyarı için bunları seçip ardından seçerek **durumunu değiştir** gelen **tüm uyarıları** sayfasında sol üst köşesinde. Üzerinde **uyarı durumunu değiştir** durumlardan biriyle seçtiğiniz bölmesi ekleme değişiklik açıklamasını **açıklama** alan ve ardından **Tamam** değişikliklerinizi işleyebilirsiniz. Bilgiler doğrulanır ve değişiklikler uygulanır, ancak altında ilerleme durumunu izleyebilirsiniz **bildirimleri** menüsünde.  

## <a name="next-steps"></a>Sonraki adımlar
Performans sorunlarını ve Vm'leri performansınızı ile genel kullanımı belirlemek için bkz: [Azure VM performansını görüntüleme](vminsights-performance.md), ya da bulunan Uygulama bağımlılıklarını görüntülemek için bkz: [Vm'leri harita görünümü Azure İzleyici](vminsights-maps.md). 
