---
title: Microsoft Threat Modeling Tool özelliğe genel bakış-Azure
description: Analiz görünümü ve raporlar gibi Threat Modeling Tool bulunan tüm özellikler hakkında bilgi edinin.
author: jegeib
ms.author: jegeib
ms.service: security
ms.subservice: security-develop
ms.topic: article
ms.date: 08/17/2017
ms.openlocfilehash: 65a88f309602462e77336181316c0d5bf19a8a1e
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90980670"
---
# <a name="threat-modeling-tool-feature-overview"></a>Threat Modeling Tool özelliğe genel bakış

Threat Modeling Tool, tehdit modelleme gereksinimlerinize yardımcı olabilir. Araca temel bir giriş için, bkz. [Threat Modeling Tool kullanmaya başlama](threat-modeling-tool-getting-started.md).

> [!NOTE]
>Threat Modeling Tool sıklıkla güncelleştirilir, bu nedenle en son özellikleri ve geliştirmelerimizi görmek için bu kılavuzu sık sık kontrol edin.

Boş bir sayfa açmak için **model oluştur**' u seçin.

![Boş sayfa](./media/threat-modeling-tool-feature-overview/tmtstart.png)

Araçta Şu anda kullanılabilir olan özellikleri görmek için, [Başlarken](threat-modeling-tool-getting-started.md) örneğinde takımımız tarafından oluşturulan tehdit modelini kullanın.

![Temel tehdit modeli](./media/threat-modeling-tool-feature-overview/basictmt.png)

## <a name="navigation"></a>Gezinti

Yerleşik özellikleri tartışmadan önce, araçta bulunan ana bileşenleri gözden geçirelim.

### <a name="menu-items"></a>Menü öğeleri

Deneyim, diğer Microsoft ürünleriyle benzerdir. Üst düzey menü öğelerini gözden geçirelim.

![Menü öğeleri](./media/threat-modeling-tool-feature-overview/menuitems.png)

| Etiketle                               | Ayrıntılar      |
| --------------------------------------- | ------------ |
| **Dosya** | <ul><li>Dosyaları açma, kaydetme ve kapatma</li><li>OneDrive hesaplarının oturumunu açın ve oturumunuzu kapatın.</li><li>Bağlantıları paylaşma (görüntüleme ve düzenleme).</li><li>Dosya bilgilerini görüntüleyin.</li><li>Varolan modellere yeni bir şablon uygulayın.</li></ul> |
| **Düzenle** | Eylemleri geri alın ve yeniden yapın, Ayrıca kopyalama, yapıştırma ve silme işlemlerini yapın. |
| **Görünüm** | <ul><li>**Analiz** ve **Tasarım** görünümleri arasında geçiş yapın.</li><li>Kapalı pencereleri açın (örneğin, kalıplar, öğe özellikleri ve iletiler).</li><li>Düzeni varsayılan ayarlarına sıfırlayın.</li></ul> |
| **Diyagram** | Diyagramlar ekleyin ve silin ve diyagramlardaki sekmeler arasında geçiş yapın. |
| **Raporlar** | Başkalarıyla paylaşmak için HTML raporları oluşturun. |
| **Yardım** | Aracını kullanmanıza yardımcı olacak kılavuzlar bulun. |

Simgeler, üst düzey menülere yönelik kısayollardır:

| Sembol                               | Ayrıntılar      |
| --------------------------------------- | ------------ |
| **Aç** | Yeni bir dosya açar. |
| **Kaydet** | Geçerli dosyayı kaydeder. |
| **Tasarım** | Model oluşturabileceğiniz **Tasarım** görünümünü açar. |
| **Analiz** | Oluşturulan tehditleri ve bunların özelliklerini gösterir. |
| **Diyagram Ekle** | Yeni bir diyagram ekler (Excel 'deki yeni sekmelere benzer şekilde). |
| **Diyagramı Sil** | Geçerli diyagramı siler. |
| **Kopyala/Kes/Yapıştır** | Öğeleri kopyalar, keser ve yapıştırır. |
| **Geri Al/Yinele** | Geri alır ve eylemleri yeniden yapar. |
| **Yakınlaştır/Uzaklaştır** | Daha iyi bir görünüm için diyagramı yakınlaştırır ve uzaklaştırır. |
| **Geri Bildirim** | MSDN forumunu açar. |

### <a name="canvas"></a>Tuval

Tuval, öğeleri sürükleyip bıraktığınızda yer alan alandır. Model oluşturmak için en hızlı ve en verimli yol sürükleyip bırakın. Ayrıca, menü ' de sağ tıklayıp, öğelerin genel sürümlerini gösterildiği gibi ekleyebilirsiniz:

#### <a name="drop-the-stencil-on-the-canvas"></a>Kalıbı tuvalde bırak

![Tuval bırakma](./media/threat-modeling-tool-feature-overview/canvasdrop1.png)

#### <a name="select-the-stencil"></a>Kalıbı seçin

![Öğe özellikleri](./media/threat-modeling-tool-feature-overview/canvasdrop2.png)

### <a name="stencils"></a>Kalıplar

Seçtiğiniz şablona bağlı olarak, kullanıma sunulan tüm kalıpları bulabilirsiniz. Doğru öğeleri bulamıyorsanız, başka bir şablon kullanın. İsterseniz, bir şablonu gereksinimlerinize uyacak şekilde değiştirebilirsiniz. Genellikle, aşağıdaki gibi kategorilerin bir birleşimini bulabilirsiniz:

| Şablon adı                               | Ayrıntılar      |
| --------------------------------------- | ------------ |
| **İşleme** | Uygulamalar, tarayıcı eklentileri, iş parçacıkları, sanal makineler |
| **Dış etkileşen** | Kimlik doğrulama sağlayıcıları, tarayıcılar, kullanıcılar, Web uygulamaları |
| **Veri deposu** | Önbellek, depolama, yapılandırma dosyaları, veritabanları, kayıt defteri |
| **Veri akışı** | İkili, ALPC, HTTP, HTTPS/TLS/SSL, ıOCTL, IPSec, adlandırılmış kanal, RPC/DCOM, SMB, UDP |
| **Güven çizgisi/kenarlık sınırı** | Şirket ağları, internet, makine, korumalı alan, Kullanıcı/çekirdek modu |

### <a name="notesmessages"></a>Notlar/mesajlar

| Bileşen                               | Ayrıntılar      |
| --------------------------------------- | ------------ |
| **İletiler** | Öğeler arasında veri akışı yok gibi bir hata olduğunda kullanıcıları uyaran iç araç mantığı. |
| **Notlar** | El ile notlar, tasarım ve gözden geçirme süreci boyunca mühendislik ekiplerine göre dosyaya eklenir. |

### <a name="element-properties"></a>Öğe özellikleri

Öğe özellikleri, seçtiğiniz öğelere göre farklılık gösterir. Güven sınırlarının dışında, diğer tüm öğeler üç genel seçim içerir:

| Element özelliği                               | Ayrıntılar      |
| --------------------------------------- | ------------ |
| **Ad** | İşlemlerinizi, mağazalarınızı, karışık aktörleri ve akışları kolayca tanınabilmeleri için adlandırmak faydalı olur. |
| **Kapsam dışı** | Seçilirse, öğe tehdit oluşturma matrisinden alınır (önerilmez). |
| **Kapsam dışı nedeni** | Kullanıcıların kapsam dışı neden seçili olduğunu bilmesini sağlamak için gerekçe alanı. |

Özellikler her öğe kategorisi altında değiştirilir. Kullanılabilir seçenekleri incelemek için her bir öğeyi seçin. Ya da daha fazla bilgi edinmek için şablonu açabilirsiniz. Özellikleri gözden geçirelim.

## <a name="welcome-screen"></a>Hoş Geldiniz ekranı

Uygulamayı açtığınızda, **hoş geldiniz** ekranını görürsünüz.

### <a name="open-a-model"></a>Model açma

İki seçeneği ortaya çıkarmak için **bir modeli aç** üzerine gelin: **Bu bilgisayardan açın** ve **OneDrive 'dan açın**. İlk seçenek **dosya açma** ekranını açar. İkinci seçenek, OneDrive için oturum açma sürecinde size götürür. Başarılı kimlik doğrulamasından sonra klasörler ve dosyalar ' ı seçebilirsiniz.

![Modeli aç](./media/threat-modeling-tool-feature-overview/openmodel.png)

![Bilgisayardan veya OneDrive 'dan aç](./media/threat-modeling-tool-feature-overview/openmodel2.png)

### <a name="feedback-suggestions-and-issues"></a>Geri bildirim, öneriler ve sorunlar

**Geri bildirim, öneriler ve sorunlar**' ı SEÇTIĞINIZDE, SDL araçları Için MSDN forumuna gidebilirsiniz. Çözümler ve yeni fikirler de dahil olmak üzere diğer kişilerin araç hakkında söylediklerini okuyabilirsiniz.

![Ekran görüntüsünde, metin geri bildirimi, öneriler ve sorunlar içeren bir düğme gösterilir.](./media/threat-modeling-tool-feature-overview/feedback.png)

## <a name="design-view"></a>Tasarım görünümü

Yeni bir model açtığınızda veya oluşturduğunuzda **Tasarım** görünümü açılır.

### <a name="add-elements"></a>Öğe ekleme

Kılavuza iki şekilde öğe ekleyebilirsiniz:

- **Sürükleyip bırakma**: istenen öğeyi kılavuza sürükleyin. Daha sonra ek bilgi sağlamak için öğe özelliklerini kullanın.
- **Sağ tıklayın**: kılavuzda herhangi bir yere sağ tıklayın ve açılan menüden öğeler ' i seçin. Seçtiğiniz öğenin genel bir temsili ekranda görüntülenir.

### <a name="connect-elements"></a>Bağlama öğeleri

Öğeleri iki şekilde bağlayabilirsiniz:

- **Sürükleyip bırakma**: istenen veri akışını kılavuza sürükleyin ve her iki ucu da uygun öğelere bağlayın.
- **+ SHIFT ' e tıklayın**: ilk öğeye (veri gönderme) tıklayın, SHIFT tuşuna basın ve basılı tutun, sonra ikinci öğeyi (veri alma) seçin. Sağ tıklayın ve **Bağlan**' ı seçin. Çift yönlü bir veri akışı kullanıyorsanız, sipariş önemli değildir.

### <a name="properties"></a>Özellikler

 Kalıpta değiştirilebilen özellikleri görmek için kalıbı seçin ve bilgiler uygun şekilde doldurulur. Aşağıdaki örnek, bir **veritabanı** kalıbı diyagram üzerine sürüklendikten önce ve sonra gösterilmektedir:

#### <a name="before"></a>Önce

![Önce](./media/threat-modeling-tool-feature-overview/properties1.png)

#### <a name="after"></a>Sonra

![Sonra](./media/threat-modeling-tool-feature-overview/properties2.png)

### <a name="messages"></a>İletiler

Bir tehdit modeli oluşturur ve veri akışlarını öğelere bağlamayı unutursanız, bir bildirim alırsınız. İletiyi yoksayabilir veya sorunu gidermeye yönelik yönergeleri izleyebilirsiniz. 

![Ekran görüntüsünde, bu sorunun neden olduğu iletiyle birlikte öğelere bağlı olmayan bir tehdit modeli Bağlayıcısı gösterilmektedir.](./media/threat-modeling-tool-feature-overview/messages.png)

### <a name="notes"></a>Notlar

Diyagramınıza Not eklemek için **iletiler** sekmesinden **Notlar** sekmesine geçin.

## <a name="analysis-view"></a>Analiz görünümü

Diyagramınızı oluşturduktan sonra, **analiz** görünümüne geçmek için kısayollar araç çubuğunda **analiz** sembolünü (Büyüteç Camı) seçin.

![Analiz görünümü](./media/threat-modeling-tool-feature-overview/analysisview.png)

### <a name="generated-threat-selection"></a>Oluşturulan tehdit seçimi

Bir tehdit seçtiğinizde üç farklı işlev kullanabilirsiniz:

| Öne çıkan özelliği                               | Bilgi      |
| --------------------------------------- | ------------ |
| **Göstergeyi oku** | <p>Tehdit okundu olarak işaretlenir ve bu, gözden geçirdiğinizi planladığınız öğeleri izlemenize yardımcı olur.</p><p>![Okuma/okunmamış gösterge](./media/threat-modeling-tool-feature-overview/readmode.png)</p> |
| **Etkileşim odağı** | <p>Bir tehdide ait olan diyagramdaki etkileşim vurgulanmıştır.</p><p>![Etkileşim odağı](./media/threat-modeling-tool-feature-overview/interactionfocus.png)</p> |
| **Tehdit özellikleri** | <p>Tehdit hakkında ek bilgiler **tehdit özellikleri** penceresinde görünür.</p><p>![Tehdit özellikleri](./media/threat-modeling-tool-feature-overview/threatproperties.png)</p> |

### <a name="priority-change"></a>Öncelik değişikliği

Oluşturulan her tehdit için öncelik düzeyini değiştirebilirsiniz. Farklı renkler, yüksek, orta ve düşük öncelikli tehditleri belirlemeyi kolaylaştırır.

![Öncelik değişikliği](./media/threat-modeling-tool-feature-overview/prioritychange.png)

### <a name="threat-properties-editable-fields"></a>Tehdit özellikleri düzenlenebilir alanları

Önceki görüntüde görüldüğü gibi araç tarafından oluşturulan bilgileri de değiştirebilirsiniz. Ayrıca, bloklama gibi belirli alanlara bilgi ekleyebilirsiniz. Bu alanlar şablon tarafından oluşturulur. Her tehdit için daha fazla bilgiye ihtiyacınız varsa, değişiklikler yapabilirsiniz.

![Tehdit özellikleri](./media/threat-modeling-tool-feature-overview/threatproperties.png)

## <a name="reports"></a>Raporlar

Önceliklerin değiştirilmesini ve oluşturulan her bir tehdidin durumunu güncellemeyi tamamladıktan sonra, dosyayı kaydedebilir ve/veya bir raporu yazdırabilirsiniz. **Raporla**  >  **tam rapor oluştur**' a gidin. Raporu adlandırın ve aşağıdaki görüntüye benzer bir şey görmeniz gerekir:

![Ekran görüntüsünde Özet, diyagramlar ve diğer bilgiler de dahil olmak üzere örnek bir tehdit modelleme raporu gösterilmektedir.](./media/threat-modeling-tool-feature-overview/report.png)

## <a name="next-steps"></a>Sonraki adımlar

- Sorularınızı, yorumlarınızı ve kaygılarınızı ile gönderin tmtextsupport@microsoft.com . Başlamak için Threat Modeling Tool **[indirin](https://aka.ms/threatmodelingtool)** .
- Topluluğun şablonuna katkıda bulunmak için [GitHub](https://github.com/Microsoft/threat-modeling-templates) sayfamıza gidin.