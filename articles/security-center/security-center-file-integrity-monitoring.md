---
title: Dosya bütünlüğünü izleme Azure Güvenlik Merkezi'nde | Microsoft Docs
description: " Dosya bütünlüğünü izleme Azure Güvenlik Merkezi'nde etkinleştirmeyi öğrenin. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/06/2018
ms.author: terrylan
ms.openlocfilehash: 187253bc2ef4a61b7a241b8f5c121bf3eec73eae
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162011"
---
# <a name="file-integrity-monitoring-in-azure-security-center"></a>Dosya bütünlüğünü Azure Güvenlik Merkezi'nde izleme
Dosya bütünlüğünü izleme (FIM), Azure Güvenlik Merkezi'nde bu kılavuzu kullanarak yapılandırmayı öğrenin.

## <a name="what-is-fim-in-security-center"></a>FIM Güvenlik Merkezi'nde nedir?
Dosya bütünlüğünü izleme (değişiklik izleme olarak da bilinen FIM), dosya ve kayıt defterleri işletim sistemi, uygulama yazılımı ve diğer bir saldırı gösterebilir değişiklikleri inceler. Dosya geçerli durumunu dosyanın son tarama dışında farklı olup olmadığını belirlemek için karşılaştırma metodu kullanılır. Dosyalarınızda geçerli veya şüpheli değişiklikler yapılıp yapılmadığını belirlemek için bu karşılaştırmayı yararlanabilirsiniz.

Güvenlik Merkezi'nin dosya bütünlüğünü izleme için Linux dosyaları Windows dosyaları ve Windows kayıt defteri bütünlüğünü doğrular. FIM etkinleştirerek izlenen istediğiniz dosyaları seçin. Güvenlik Merkezi ile FIM etkinliği için gibi etkin dosyaları izler:

- Dosya ve kayıt defteri oluşturma ve kaldırma
- Dosya değişiklikleri (dosya boyutu, erişim denetim listeleri ve içeriğin karmasını değişiklikler)
- Kayıt defteri değişiklikleri (boyut, erişim conrol listeler, türü ve içerik değişiklikler)

Güvenlik Merkezi üzerinde FIM kolayca etkinleştirebilirsiniz izlemek için varlıklar önerir. Ayrıca, kendi FIM ilkeleri veya varlıkları izlemek için de tanımlayabilirsiniz. Bu izlenecek yol size nasıl gösterir.

> [!NOTE]
> Dosya bütünlüğünü izleme (FIM) özelliği, Windows ve Linux bilgisayarlar ve Vm'leri için çalışır ve Güvenlik Merkezi'nin standart katmanında kullanılabilir. Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md).
FIM, verilerini Log Analytics çalışma alanına yükler. Karşıya yüklediğiniz veri miktarına göre veri ücretleri uygulanır. Bkz: [Log Analytics fiyatlandırma](https://azure.microsoft.com/pricing/details/log-analytics/) daha fazla bilgi için.
>
>

> [!NOTE]
> FIM Azure değişiklik izleme çözümü, ortamınızdaki değişiklikleri tanımlamak ve izlemek için kullanır. Dosya bütünlüğünü izleme etkin olduğunda, sahip olduğunuz bir **değişiklik izleme** çözüm türü. Kaldırırsanız **değişiklik izleme** kaynak, devre dışı dosya bütünlüğünü izleme Güvenlik Merkezi'nde özelliğini.
>
>

## <a name="which-files-should-i-monitor"></a>Hangi dosyaların izleyebilirim?
Hangi dosyaların izleyin seçerken sistemi ve uygulamalar için kritik olan dosyaları hakkında almalısınız. Plan olmadan değiştirme duymayacağınızı dosyaları seçmeyi düşünün. Uygulama veya işletim sistemi (örneğin, günlük dosyaları ve metin dosyaları) sık değiştirilenlerin seçme dosyaları çok fazla olduğu belirlenmesine zorlaştıran gürültü oluşturun.

Güvenlik Merkezi önerir, bir dosyaları, dosya ve kayıt defteri değişiklikleri içeren bilinen saldırı desenlerine göre varsayılan olarak izlemeniz gerekir.

## <a name="using-file-integrity-monitoring"></a>Kullanarak dosya bütünlüğünü izleme
1. **Güvenlik Merkezi** panosunu açın.
2. Sol bölmede altında **Gelişmiş bulut savunması**seçin **dosya bütünlüğünü izleme**.
![Güvenlik Merkezi Panosu][1]

**Dosya bütünlüğünü izleme** açılır.
  ![Güvenlik Merkezi Panosu][2]

Her bir çalışma alanı için aşağıdaki bilgiler sağlanır:

- Son bir hafta içinde gerçekleşen değişikliklerin toplam sayısı (bir çizgi görebilirsiniz "-" FIM çalışma alanı etkin değilse)
- Bilgisayarlar ve çalışma alanınıza raporlayan VM'lerin toplam sayısı
- Çalışma alanının coğrafi konum
- Çalışma alanı altında olan bir azure aboneliği

Aşağıdaki düğmeler, bir çalışma alanı için de gösterilebilir:

- ![Simge etkinleştir][3] FIM çalışma alanı için etkin olmadığını gösterir. Çalışma alanını seçerek, FIM çalışma alanı altındaki tüm makinelerde etkinleştir olanak tanır.
- ![Yükseltme planı simgesi][4] çalışma alanı veya abonelik altında Güvenlik Merkezi'nin standart katmanında çalışmıyor belirtir. FIM bu özelliği kullanmak için aboneliğinizi standart çalıştırılması gerekir.  Çalışma alanını seçmeyi standart yükseltmenize olanak tanır. Standart katman ve yükseltme hakkında daha fazla bilgi için bkz: [Güvenlik Merkezi'nin standart katmanında Gelişmiş güvenlik yükseltme](security-center-pricing.md).
- Boş (Hayır düğmesi yoktur) FIM, çalışma alanınızda zaten etkin anlamına gelir.

Altında **dosya bütünlüğünü izleme**, bu çalışma alanı için FIM etkinleştirmek için bu çalışma alanı için Dosya bütünlüğünü izleme panosunu görüntülemek istediğiniz çalışma alanını seçebilirsiniz veya [yükseltme](security-center-pricing.md) standart çalışma alanına.

## <a name="enable-fim"></a>FIM etkinleştir
Bir çalışma alanında FIM etkinleştirmek için:

1. Altında **dosya bütünlüğünü izleme**, bir çalışma alanı ile **etkinleştirmek** düğmesi.
2. **Dosya bütünlüğünü İzleme'yi etkinleştirmek** çalışma alanı altında Windows ve Linux makinelerin sayısını görüntüleyen açılır.

   ![Dosya bütünlüğünü İzleme'yi etkinleştir][5]

   Windows ve Linux için önerilen ayarları da listelenir.  Genişletin **Windows dosyaları**, **kayıt defteri**, ve **Linux dosyaları** önerilen öğeler tam listesini görmek için.

3. Önerilen FIM'e uygulamak istemediğiniz herhangi bir varlık işaretini kaldırın.
4. Seçin **dosya bütünlüğünü izleme uygulamak** FIM etkinleştirmek için.

> [!NOTE]
> Ayarları istediğiniz zaman değiştirebilirsiniz. Bkz [düzenleme izlenen varlık](security-center-file-integrity-monitoring.md#edit-monitored-items) aşağıda daha fazla bilgi edinin.
>
>

## <a name="view-the-fim-dashboard"></a>FIM panosunu görüntüleyin
**Dosya bütünlüğünü izleme** Pano çalışma alanları için FIM etkinleştirdiğiniz görüntüler. Bir çalışma alanında veya bir çalışma alanı seçtiğinizde FIM etkinleştirdikten sonra FIM Pano açılır **dosya bütünlüğünü izleme** FIM etkin olan pencere.

![Dosya bütünlüğünü izleme Panosu][6]

Bir çalışma alanı için FIM Pano şunları görüntüler:

- Toplam makine sayısı çalışma alanına bağlı
- Seçili dönem boyunca gerçekleşen değişikliklerin toplam sayısı
- Değişiklik türü (dosyaları, kayıt defteri) dökümünü
- Değişiklik kategorisi dökümünü (değiştirilmiş, eklenen, kaldırıldı)

Panonun üst kısmındaki filtre seçmek, değişiklikler için görmek istediğiniz süreyi uygulamanıza olanak tanır.

![Zaman dönemi filtresi][7]

**Bilgisayarlar** (yukarıda gösterilmiştir) sekmesinde, bu çalışma alanınıza raporlayan tüm makinelerde listelenir. Her makine için Pano listeler:

- Seçilen süre boyunca gerçekleşen toplam değişikliklerin
- Bir dosya değişikliklerini veya kayıt defteri değişiklikleri toplam değiştikçe dökümü

**Günlük arama** aramaya makine adını girin, açılır alan veya bilgisayarlar sekmesinde listelenen bir makine seçin. Günlük araması makine için seçilen süre boyunca yapılan tüm değişiklikleri görüntüler. Daha fazla bilgi için bir değişiklik genişletebilirsiniz.

![Günlük araması][8]

**Değişiklikleri** sekmesinde (aşağıda gösterilmiştir), seçilen süre içinde çalışma alanı için tüm değişiklikleri listeler. Pano listesi değiştirildi her varlık için:

- Üzerinde değişikliğin oluşma bilgisayar
- Tür değişiklik (kayıt defteri veya dosya)
- Değişiklik kategorisi (değiştirilmiş, eklenen, kaldırıldı)
- Tarih ve saat değişikliği

![Çalışma alanı için değişiklikler][9]

**Değişiklik ayrıntıları** altında listelenmiş bir varlık seçin veya alanı bir değişiklik aramaya girdiğinizde açılır **değişiklikleri** sekmesi.

![Değişiklik ayrıntıları][10]

## <a name="edit-monitored-entities"></a>İzlenen düzenleme varlıklar

1. Geri dönüp **dosya bütünlüğünü izleme panosunu** seçip **ayarları**.

  ![Ayarlar][11]

  **Çalışma alanı yapılandırması** üç sekme görüntüleyen açılır: **Windows kayıt defteri**, **Windows dosyaları**, ve **Linux dosyaları**. Her sekme, bu kategorideki düzenleyebileceğiniz varlıkları listeler. FIM ise, listelenen her varlık için güvenlik merkezini tanımlar. (true) etkin veya etkin değil (false).  Varlığı düzenlemeyi etkinleştirmek veya devre dışı FIM sağlar.

  ![Çalışma alanı yapılandırması][12]

2. Bir identityprotection seçin. Bu örnekte, Windows kayıt defteri altında bir öğe seçtik. **Değişiklik izleme için düzenleme** açılır.

  ![Düzenlemek veya değişiklik izleme][13]

Altında **değişiklik izleme için düzenleme** şunları yapabilirsiniz:

- (True) etkinleştirin veya devre dışı (False) dosya bütünlüğünü izleme
- Sağlamak veya varlık adını değiştirme
- Sağlamak veya değer veya yolunu değiştirme
- Varlığı silmek, değişikliği atmak veya yapılan değişiklik kaydedilemiyor

## <a name="add-a-new-entity-to-monitor"></a>İzlemek için yeni bir varlık ekleme
1. Geri dönüp **izleme Panosu dosya olmadığına** seçip **ayarları** en üstünde. **Çalışma alanı yapılandırması** açılır.
2. Altında **çalışma alanı yapılandırması**, eklemek istediğiniz varlık türü için sekmesinde seçin: Windows kayıt defteri, Windows dosyalarını veya Linux dosyaları. Biz bu örnekte, seçili **Linux dosyaları**.

  ![İzlemek için yeni bir öğe ekleyin][14]

3. **Add (Ekle)** seçeneğini belirleyin. **Değişiklik izleme için ekleme** açılır.

  ![İstenen bilgileri girin][15]

4. Üzerinde **Ekle** sayfasında istenen bilgileri girin ve seçin **Kaydet**.

## <a name="disable-monitored-entities"></a>İzlenen devre dışı varlıklar
1. Geri dönüp **dosya bütünlüğünü izleme** Pano.
2. FIM şu anda etkin olduğu bir çalışma alanı seçin. Etkinleştirme veya planı Yükselt düğmesini eksikse bir çalışma alanı için FIM etkinleştirilir.

  ![FIM etkinleştirdiğiniz bir çalışma alanı seçin][16]

3. Dosya bütünlüğünü izleme altında seçin **ayarları**.

  ![ayarları seçin][17]

4. Altında **çalışma alanı yapılandırması**, bir grup seçin burada **etkin** ayarlanır true.

  ![Çalışma alanı yapılandırması][18]

5. Altında **değişiklik izleme için düzenleme** penceresi kümesi **etkin** false.

  ![False kümesi etkin][19]

6. **Kaydet**’i seçin.

## <a name="disable-fim"></a>FIM devre dışı bırak
FIM devre dışı bırakabilirsiniz. FIM Azure değişiklik izleme çözümü, ortamınızdaki değişiklikleri tanımlamak ve izlemek için kullanır. FIM devre dışı bırakarak, değişiklik izleme çözümü seçili çalışma alanından kaldırın.

1. FIM devre dışı bırakmak için iade **dosya bütünlüğünü izleme** Pano.
2. Bir çalışma alanı seçin.
3. Altında **dosya bütünlüğünü izleme**seçin **devre dışı**.

  ![FIM devre dışı bırak][20]

4. Seçin **Kaldır** devre dışı bırakmak için.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede Güvenlik Merkezi'ndeki Dosya bütünlüğünü izleme (FIM) kullanmayı öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Güvenlik ilkelerini ayarlama](security-center-policies.md) --Azure Abonelikleriniz ve kaynak grupları için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Güvenlik önerilerini yönetme](security-center-recommendations.md) --önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Güvenlik durumunu izleme](security-center-monitoring.md)--Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Yönetme ve güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md)--yönetme ve güvenlik uyarılarını yanıtlama hakkında bilgi edinin.
* [İş ortağı çözümlerini izleme](security-center-partner-solutions.md) --iş ortağı çözümlerinizin sistem durumunu izleme hakkında bilgi edinin.
* [Güvenlik Merkezi SSS](security-center-faq.md)--hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - En son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-file-integrity-monitoring/security-center-dashboard.png
[2]: ./media/security-center-file-integrity-monitoring/file-integrity-monitoring.png
[3]: ./media/security-center-file-integrity-monitoring/enable.png
[4]: ./media/security-center-file-integrity-monitoring/upgrade-plan.png
[5]: ./media/security-center-file-integrity-monitoring/enable-fim.png
[6]: ./media/security-center-file-integrity-monitoring/fim-dashboard.png
[7]: ./media/security-center-file-integrity-monitoring/filter.png
[8]: ./media/security-center-file-integrity-monitoring/log-search.png
[9]: ./media/security-center-file-integrity-monitoring/changes-tab.png
[10]: ./media/security-center-file-integrity-monitoring/change-details.png
[11]: ./media/security-center-file-integrity-monitoring/fim-dashboard-settings.png
[12]: ./media/security-center-file-integrity-monitoring/workspace-config.png
[13]: ./media/security-center-file-integrity-monitoring/edit.png
[14]: ./media/security-center-file-integrity-monitoring/add.png
[15]: ./media/security-center-file-integrity-monitoring/add-item.png
[16]: ./media/security-center-file-integrity-monitoring/fim-dashboard-disable.png
[17]: ./media/security-center-file-integrity-monitoring/fim-dashboard-settings-disabled.png
[18]: ./media/security-center-file-integrity-monitoring/workspace-config-disable.png
[19]: ./media/security-center-file-integrity-monitoring/edit-disable.png
[20]: ./media/security-center-file-integrity-monitoring/disable-fim.png
