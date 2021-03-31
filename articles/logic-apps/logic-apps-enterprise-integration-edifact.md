---
title: B2B tümleştirmesi için EDIOLGU iletileri
description: Enterprise Integration Pack ile Azure Logic Apps 'de B2B kurumsal tümleştirme için EDI biçimindeki Exchange EDIOLGU iletileri
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 04/22/2020
ms.openlocfilehash: b0df55e59bd519a816c4022f2434edfcd4460780
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "96006532"
---
# <a name="exchange-edifact-messages-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>Enterprise Integration Pack ile Azure Logic Apps'te B2B kurumsal tümleştirme için EDIFACT iletilerini gönderme ve alma

Azure Logic Apps için EDIOLGU iletilerini değiş tokuş etmeden önce, bir EDIOLGU sözleşmesi oluşturmanız ve bu sözleşmeyi tümleştirme hesabınızda depolamanız gerekir. Bir EDIOLGU anlaşması oluşturma adımları aşağıda verilmiştir.

> [!NOTE]
> Bu sayfa Azure Logic Apps için EDIOLGU özelliklerini içerir. Daha fazla bilgi için bkz. [x12](logic-apps-enterprise-integration-x12.md).

## <a name="before-you-start"></a>Başlamadan önce

İhtiyacınız olan öğeler aşağıda verilmiştir:

* Zaten tanımlanmış ve Azure aboneliğinizle ilişkilendirilen bir [tümleştirme hesabı](logic-apps-enterprise-integration-create-integration-account.md)  
* Tümleştirme hesabınızda zaten tanımlanmış olan en az iki [iş ortağı](logic-apps-enterprise-integration-partners.md)

> [!NOTE]
> Bir anlaşma oluşturduğunuzda, iş ortağınızdan gelen veya bu bilgisayardan gönderilen iletilerde bulunan içeriğin anlaşma türüyle eşleşmesi gerekir.

[Bir tümleştirme hesabı oluşturup](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) [iş ortakları](logic-apps-enterprise-integration-partners.md)ekledikten sonra, bu ADıMLARı izleyerek bir ediolgu anlaşması oluşturabilirsiniz.

## <a name="create-an-edifact-agreement"></a>EDIOLGU sözleşmesi oluşturma 

1. [Azure portalında](https://portal.azure.com "Azure portalı") oturum açın. 

2. Ana Azure menüsünde **tüm hizmetler**' i seçin. Arama kutusuna "tümleştirme" yazın ve ardından **tümleştirme hesapları**' nı seçin.

   ![Tümleştirme hesabınızı bulun](./media/logic-apps-enterprise-integration-edifact/edifact-0.png)

   > [!TIP]
   > **Tüm hizmetler** görünmezse, önce menüyü genişletmeniz gerekebilir. Daraltılan menünün en üstünde **metin etiketlerini göster**' i seçin.

3. **Tümleştirme hesapları** altında, sözleşmeyi oluşturmak istediğiniz tümleştirme hesabını seçin.

   ![Anlaşmanın oluşturulacağı tümleştirme hesabını seçin](./media/logic-apps-enterprise-integration-edifact/edifact-1-4.png)

4. **Anlaşmaları** seçin. Anlaşma kutucuğuna sahipseniz, önce kutucuğu ekleyin.   

   !["Anlaşmalar" kutucuğunu seçin](./media/logic-apps-enterprise-integration-edifact/edifact-1-5.png)

5. Anlaşmalar sayfasında **Ekle**' yi seçin.

   !["Ekle" yi seçin](./media/logic-apps-enterprise-integration-edifact/edifact-agreement-2.png)

6. **Ekle**' nin altına sözleşmeniz Için bir **ad** girin. **Anlaşma türü** Için **ediolgu**' ı seçin. Sözleşmeniz için **konak ortağını**, **ana bilgisayar kimliğini**, **Konuk iş ortağını** ve **Konuk kimliğini** seçin.

   ![Anlaşma ayrıntılarını sağlama](./media/logic-apps-enterprise-integration-edifact/edifact-1.png)

   | Özellik | Açıklama |
   | --- | --- |
   | Ad |Sözleşmenin adı |
   | Anlaşma türü | EDIOLGU olmalıdır |
   | Ana bilgisayar ortağı |Bir anlaşmanın hem ana bilgisayar hem de Konuk iş ortağı olması gerekir. Ana bilgisayar ortağı, sözleşmeyi yapılandıran kuruluşu temsil eder. |
   | Ana bilgisayar kimliği |Ana bilgisayar ortağı için bir tanımlayıcı |
   | Konuk Iş ortağı |Bir anlaşmanın hem ana bilgisayar hem de Konuk iş ortağı olması gerekir. Konuk iş ortağı, ana bilgisayar ortağıyla iş yapan kuruluşu temsil eder. |
   | Konuk kimliği |Konuk iş ortağı için bir tanımlayıcı |
   | Ayarları al |Bu özellikler, bir sözleşme tarafından alınan tüm iletiler için geçerlidir. |
   | Ayarları gönder |Bu özellikler, bir anlaşma tarafından gönderilen tüm iletiler için geçerlidir. |
   ||| 

## <a name="configure-how-your-agreement-handles-received-messages"></a>Anlaşmanızın alınan iletileri nasıl işleyeceğini yapılandırın

Anlaşma özelliklerini ayarlamış olduğunuza göre, bu sözleşmenin bu sözleşme aracılığıyla iş ortağınızdan alınan gelen iletileri nasıl tanımladığını ve işleyeceğini yapılandırabilirsiniz.

> [!IMPORTANT]
> EDIOLGU Bağlayıcısı yalnızca UTF-8 karakterlerini destekler.
> Çıktılarınız beklenmeyen karakterler içeriyorsa, EDIOLGU iletilerinizin UTF-8 karakter kümesini kullanıp kullandığından emin olun.

1. **Ekle**' nin altında, **alma ayarları**' nı seçin.
Sizinle ileti alışverişi yapan iş ortağıyla anlaşmanıza bağlı olarak bu özellikleri yapılandırın. Özellik açıklamaları için bu bölümdeki tablolara bakın.

   **Alma ayarları** şu bölümler halinde düzenlenmiştir: tanımlayıcılar, bildirim, şemalar, denetim numaraları, doğrulama ve iç ayarlar.

   !["Ayarları al" ayarını yapılandırın](./media/logic-apps-enterprise-integration-edifact/edifact-2.png)  

2. İşiniz bittiğinde, **Tamam**' ı seçerek ayarlarınızı kaydettiğinizden emin olun.

Artık sözleşmeniz, seçili ayarlarınıza uygun gelen iletileri işlemeye hazır.

### <a name="identifiers"></a>Tanımlayıcılar

| Özellik | Açıklama |
| --- | --- |
| UNB 6.1 (alıcı başvuru parolası) |1 ila 14 karakter arasında bir alfasayısal değer girin. |
| UNB 6.2 (alıcı başvuru niteleyicisi) |En az bir karakter ve en fazla iki karakter içeren alfasayısal bir değer girin. |

### <a name="acknowledgments"></a>Teşekkürler

| Özellik | Açıklama |
| --- | --- |
| Ileti alındısı (CONM) |Değiş tokuş göndericisine teknik (CONU) bildirimi döndürmek için bu onay kutusunu seçin. Bildirim, sözleşme için gönderme ayarlarına bağlı olarak değişim gönderene gönderilir. |
| Onay (conı) |Bu onay kutusunu seçerek, değişim göndericisine işlevsel (CONCE) alındı bildirimi döndürmek için bildirim, sözleşmenin gönderme ayarlarına bağlı olarak değişim göndericisine gönderilir. |

### <a name="schemas"></a>Şemalar

| Özellik | Açıklama |
| --- | --- |
| UNH 2.1 (TÜR) |Bir işlem kümesi türü seçin. |
| UNH 2.2 (SÜRÜM) |İleti sürümü numarasını girin. (Minimum, bir karakter; maksimum, üç karakter). |
| UNH 2.3 (YAYıN) |İleti yayın numarasını girin. (Minimum, bir karakter; maksimum, üç karakter). |
| UNH 2.5 (ILIŞKILI ATANAN KOD) |Atanan kodu girin. (En fazla, altı karakter. Alfasayısal olmalıdır). |
| UNG 2.1 (UYGULAMA GÖNDERICI KIMLIĞI) |En az bir karakter ve en fazla 35 karakter içeren alfasayısal bir değer girin. |
| UNG 2.2 (UYGULAMA GÖNDERICI KODU NITELEYICISI) |En fazla dört karakter içeren alfasayısal bir değer girin. |
| MANıZı |İlişkili tümleştirme hesabınızdan kullanmak istediğiniz daha önce karşıya yüklenmiş şemayı seçin. |

### <a name="control-numbers"></a>Denetim numaraları

| Özellik | Açıklama |
| --- | --- |
| Değişim denetim numarası çoğaltmasına izin verme |Yinelenen değişiklikleri engellemek için bu özelliği seçin. Seçilirse, EDIOLGU kod çözme eylemi, alınan değişim için değişim denetim numarasının (UNB5) daha önce işlenmiş bir değişim denetim numarasıyla eşleşip eşleşmediğini denetler. Bir eşleşme algılanırsa, değişim işlenmez. |
| Yinelenen UNB5 denetle (gün) |Yinelenen değişim denetimi numaralarına izin vermemeyi seçerseniz, bu ayar için uygun değeri vererek, denetimin gerçekleştirileceği gün sayısını belirtebilirsiniz. |
| Grup denetim numarası yinelenmesine izin verme |Yinelenen grup denetim numaralarıyla (UNG5) yapılan değişiklikleri engellemek için bu özelliği seçin. |
| Işlem kümesi denetim numarası yinelenmesine izin verme |Yinelenen işlem kümesi denetim numaralarıyla (UNH1) yapılan değişiklikleri engellemek için bu özelliği seçin. |
| EDIOLGU onaylama denetim numarası |Bir bildirim içinde kullanılmak üzere işlem kümesi başvuru numaralarını belirlemek için, önek için bir değer, bir başvuru numarası aralığı ve bir sonek girin. |

### <a name="validation"></a>Doğrulama

Her doğrulama satırını tamamladığınızda, başka bir otomatik olarak eklenir. Herhangi bir kural belirtmezseniz, doğrulama "varsayılan" satırını kullanır.

| Özellik | Açıklama |
| --- | --- |
| İleti türü |EDI ileti türünü seçin. |
| EDI doğrulaması |Şema EDI özellikleri, uzunluk kısıtlamaları, boş veri öğeleri ve sondaki ayırıcılar tarafından tanımlanan veri türlerinde EDI doğrulaması gerçekleştirin. |
| Genişletilmiş Doğrulama |Veri türü EDI değilse, doğrulama veri öğesi gereksinimidir ve izin verilen yineleme, numaralandırmalar ve veri öğesi uzunluğu doğrulaması (min/max). |
| Baştaki/sondaki sıfırları izin ver |Tüm ek öndeki veya sondaki sıfır ve boşluk karakterlerini koruyun. Bu karakterleri kaldırmayın. |
| Baştaki/sondaki sıfırları Kırp |Baştaki veya sondaki sıfır ve boşluk karakterlerini kaldırın. |
| Sondaki ayırıcı Ilkesi |Sondaki ayırıcılar oluştur. <p>Alınan değişim içindeki sonda sınırlayıcılar ve ayırıcılara **Izin verilmeyeceğine Izin verilmiyor** ' ı seçin. Değişim, sonda sınırlayıcılar ve ayırıcılar içeriyorsa, değişim geçerli değil olarak bildirilmiştir. <p>Sondaki sınırlayıcılarla veya Ayırıcısız değişiklikler kabul etmek için **Isteğe bağlı** ' i seçin. <p>Alınan değişim için sonda sınırlayıcılar ve ayırıcılar olması gerektiğinde **zorunlu** ' ı seçin. |

### <a name="internal-settings"></a>İç ayarlar

| Özellik | Açıklama |
| --- | --- |
| Sondaki ayırıcılara izin veriliyorsa boş XML etiketleri oluştur |Bu onay kutusunu, değişim göndericisinin sondaki ayırıcılar için boş XML etiketleri içermesini sağlamak için seçin. |
| Değişimi işlem kümeleri olarak Böl-hata durumunda işlem kümelerini askıya al|Bir değişim içindeki her işlem kümesini, işlem kümesine uygun zarfı uygulayarak ayrı bir XML belgesi olarak ayrıştırır. Yalnızca doğrulamanın başarısız olduğu işlem kümelerini askıya alın. |
| Değişimi işlem kümeleri olarak Böl-hata durumunda değişimi askıya al|Bir değiş tokuş içindeki her işlem kümesini, uygun zarfı uygulayarak ayrı bir XML belgesi olarak ayrıştırır. Değişim başarısız doğrulamasında bir veya daha fazla işlem kümesi olduğunda tüm değişimi askıya alın. | 
| Değişimi koru-hata durumunda işlem kümelerini askıya al |Değişimi dokunulmadan bırakır, tüm toplu değişim için bir XML belgesi oluşturur. Yalnızca doğrulamanın başarısız olduğu işlem kümelerini askıya alın, diğer tüm işlem kümelerini işlemeye devam edin. |
| Değişimi koru-hata durumunda değişimi askıya al |Değişimi dokunulmadan bırakır, tüm toplu değişim için bir XML belgesi oluşturur. Değişim başarısız doğrulamasında bir veya daha fazla işlem kümesi olduğunda tüm değişimi askıya alın. |

## <a name="configure-how-your-agreement-sends-messages"></a>Sözleşmenizin iletileri nasıl göndereceğini yapılandırın

Bu sözleşmenin, bu sözleşme aracılığıyla iş ortaklarınıza göndereceğiniz giden iletileri nasıl tanımladığını ve işleyeceğini yapılandırabilirsiniz.

1.  **Ekle**' nin altında, **ayarları gönder**' i seçin.
Sizinle ileti alışverişi yapan iş ortağınız ile anlaşmanıza bağlı olarak bu özellikleri yapılandırın. Özellik açıklamaları için bu bölümdeki tablolara bakın.

    **Gönderme ayarları** şu bölümler halinde düzenlenmiştir: tanımlayıcılar, bildirim, şemalar, zarflar, karakter kümeleri ve ayırıcılar, denetim numaraları ve doğrulamalar.

    !["Ayarları gönder" i yapılandırın](./media/logic-apps-enterprise-integration-edifact/edifact-3.png)    

2. İşiniz bittiğinde, **Tamam**' ı seçerek ayarlarınızı kaydettiğinizden emin olun.

Artık sözleşmeniz, seçili ayarlarınıza uygun olan giden iletileri işlemeye hazır.

### <a name="identifiers"></a>Tanımlayıcılar

| Özellik | Açıklama |
| --- | --- |
| UNB 1.2 (söz dizimi sürümü) |**1** ile **4** arasında bir değer seçin. |
| UNB 2.3 (gönderici ters yönlendirme adresi) |En az bir karakter ve en fazla 14 karakter içeren alfasayısal bir değer girin. |
| UNB 3.3 (alıcı ters yönlendirme adresi) |En az bir karakter ve en fazla 14 karakter içeren alfasayısal bir değer girin. |
| UNB 6.1 (alıcı başvuru parolası) |En az bir ve en fazla 14 karakter uzunluğunda alfasayısal bir değer girin. |
| UNB 6.2 (alıcı başvuru niteleyicisi) |En az bir karakter ve en fazla iki karakter içeren alfasayısal bir değer girin. |
| UNB7 (uygulama başvuru KIMLIĞI) |En az bir karakter ve en fazla 14 karakter içeren alfasayısal bir değer girin |

### <a name="acknowledgment"></a>Olumlu

| Özellik | Açıklama |
| --- | --- |
| Ileti alındısı (CONM) |Barındırılan iş ortağı bir teknik (CONU) bildirimi almayı bekliyorsa bu onay kutusunu işaretleyin. Bu ayar, iletiyi gönderen barındırılan iş ortağının Konuk iş ortağından alındı bildirimi istemesi gerektiğini belirtir. |
| Onay (conı) |Barındırılan iş ortağı bir işlevsel (CONı) bildirimi almayı bekliyorsa bu onay kutusunu işaretleyin. Bu ayar, iletiyi gönderen barındırılan iş ortağının Konuk iş ortağından alındı bildirimi istemesi gerektiğini belirtir. |
| Kabul edilen işlem kümeleri için SG1/SG4 döngüsü oluştur |İşlevsel onay istemeyi seçtiyseniz, kabul edilen işlem kümelerine yönelik işlevsel uyumluluk bildirimleri içinde SG1/SG4 döngülerini oluşturmaya zorlamak için bu onay kutusunu seçin. |

### <a name="schemas"></a>Şemalar

| Özellik | Açıklama |
| --- | --- |
| UNH 2.1 (TÜR) |Bir işlem kümesi türü seçin. |
| UNH 2.2 (SÜRÜM) |İleti sürümü numarasını girin. |
| UNH 2.3 (YAYıN) |İleti yayın numarasını girin. |
| MANıZı |Kullanılacak şemayı seçin. Şemalar tümleştirme hesabınızda bulunur. Şemalerinize erişmek için öncelikle tümleştirme hesabınızı mantıksal uygulamanıza bağlayın. |

### <a name="envelopes"></a>Larla

| Özellik | Açıklama |
| --- | --- |
| UNB8 (Işleme öncelik kodu) |Bir karakterden daha uzun olmayan alfabetik bir değer girin. |
| UNB10 (Iletişim Sözleşmesi) |En az bir karakter ve en fazla 40 karakter içeren alfasayısal bir değer girin. |
| UNB11 (test göstergesi) |Oluşturulan değişim verilerinin test verileri olduğunu göstermek için bu onay kutusunu seçin |
| UNA segmenti Uygula (hizmet dizesi önerisi) |Değişim gönderilmesi için bir UNA segmenti oluşturmak üzere bu onay kutusunu seçin. |
| UNG segmentlerini Uygula (Işlev grubu üst bilgisi) |Konuk iş ortağına gönderilen iletilerde işlevsel Grup üstbilgisinde gruplandırma kesimleri oluşturmak için bu onay kutusunu seçin. Aşağıdaki değerler, UNG segmentlerini oluşturmak için kullanılır: <p>**UNG1** için en az bir karakter ve en fazla altı karakter içeren bir alfasayısal değer girin. <p>**Ung 2.1** için, en az bir karakter ve en fazla 35 karakter içeren alfasayısal bir değer girin. <p>**Ung 2.2** için en fazla dört karakter içeren bir alfasayısal değer girin. <p>**Ung 3.1** için, en az bir karakter ve en fazla 35 karakter içeren alfasayısal bir değer girin. <p>**Ung 3.2** için en fazla dört karakter uzunluğunda bir alfasayısal değer girin. <p>**UNG6** için en az bir alfasayısal değer ve en fazla üç karakter girin. <p>**Ung 7.1** için, en az bir karakter ve en fazla üç karakter içeren alfasayısal bir değer girin. <p>**Ung 7.2** için, en az bir karakter ve en fazla üç karakter içeren alfasayısal bir değer girin. <p>**Ung 7.3** için, en az 1 karakter ve en fazla 6 karakter uzunluğunda bir alfasayısal değer girin. <p>**UNG8** için, en az bir karakter ve en fazla 14 karakter içeren alfasayısal bir değer girin. |

### <a name="character-sets-and-separators"></a>Karakter kümeleri ve ayırıcılar

Karakter kümesi dışında, her ileti türü için kullanılacak farklı bir sınırlayıcı kümesi girebilirsiniz. Belirli bir ileti şeması için bir karakter kümesi belirtilmemişse, varsayılan karakter kümesi kullanılır.

| Özellik | Açıklama |
| --- | --- |
| UNB 1.1 (sistem tanımlayıcısı) |Giden değişim üzerinde uygulanacak EDIOLGU karakter kümesini seçin. |
| Şema |Açılan listeden bir şema seçin. Her satırı tamamladıktan sonra, yeni bir satır otomatik olarak eklenir. Seçili şema için, aşağıdaki ayırıcı açıklamalara göre kullanmak istediğiniz ayırıcılar kümesini seçin. |
| Giriş Türü |Açılan listeden bir giriş türü seçin. |
| Bileşen ayırıcısı |Bileşik veri öğelerini ayırmak için tek bir karakter girin. |
| Veri öğesi ayırıcısı |Bileşik veri öğeleri içindeki basit veri öğelerini ayırmak için tek bir karakter girin. |
| Segment Sonlandırıcı |EDI segmentinin sonunu belirtmek için tek bir karakter girin. |
| Önekini |Kesim tanımlayıcısıyla birlikte kullanılan karakteri seçin. Bir sonek belirlerseniz, segment Sonlandırıcı veri öğesi boş olabilir. Segment Sonlandırıcı boş bırakılırsa, bir sonek atamanız gerekir. |

### <a name="control-numbers"></a>Denetim numaraları

| Özellik | Açıklama |
| --- | --- |
| UNB5 (değişim denetim numarası) |Bir önek, değişim denetim numarası için bir değer aralığı ve bir sonek girin. Bu değerler bir giden değişim oluşturmak için kullanılır. Ön ek ve sonek isteğe bağlıdır, ancak denetim numarası gereklidir. Her yeni ileti için denetim numarası artırılır; ön ek ve sonek aynı kalır. |
| UNG5 (Grup denetim numarası) |Bir önek, değişim denetim numarası için bir değer aralığı ve bir sonek girin. Bu değerler, Grup denetim numarasını oluşturmak için kullanılır. Ön ek ve sonek isteğe bağlıdır, ancak denetim numarası gereklidir. En yüksek değere ulaşılana kadar, her yeni ileti için denetim numarası artırılır; ön ek ve sonek aynı kalır. |
| UNH1 (Ileti üst bilgisi başvuru numarası) |Bir önek, değişim denetim numarası için bir değer aralığı ve bir sonek girin. Bu değerler, ileti üst bilgisi başvuru numarasını oluşturmak için kullanılır. Önek ve sonek isteğe bağlıdır, ancak başvuru numarası gereklidir. Her yeni ileti için başvuru numarası artırılır; ön ek ve sonek aynı kalır. |

### <a name="validation"></a>Doğrulama

Her doğrulama satırını tamamladığınızda, başka bir otomatik olarak eklenir. Herhangi bir kural belirtmezseniz, doğrulama "varsayılan" satırını kullanır.

| Özellik | Açıklama |
| --- | --- |
| İleti türü |EDI ileti türünü seçin. |
| EDI doğrulaması |Şema, uzunluk kısıtlamaları, boş veri öğeleri ve sondaki ayırıcıların EDI özellikleri tarafından tanımlanan veri türlerinde EDI doğrulaması gerçekleştirin. |
| Genişletilmiş Doğrulama |Veri türü EDI değilse, doğrulama veri öğesi gereksinimidir ve izin verilen yineleme, numaralandırmalar ve veri öğesi uzunluğu doğrulaması (min/max). |
| Baştaki/sondaki sıfırları izin ver |Tüm ek öndeki veya sondaki sıfır ve boşluk karakterlerini koruyun. Bu karakterleri kaldırmayın. |
| Baştaki/sondaki sıfırları Kırp |Baştaki veya sondaki sıfır karakterleri kaldırın. |
| Sondaki ayırıcı Ilkesi |Sondaki ayırıcılar oluştur. <p>Gönderilen değişim içindeki sonda ayırıcıları ve ayırıcıları yasakla ' ya **Izin verilmiyor** ' ı seçin. Değişim, sonda sınırlayıcılar ve ayırıcılar içeriyorsa, değişim geçerli değil olarak bildirilmiştir. <p>Sondaki sınırlayıcılar ve ayırıcılar olmadan veya bunlarla karşılıklı değişiklikler göndermek için **Isteğe bağlı** ' yı seçin. <p>Gönderilen değişim için sonda sınırlayıcılar ve ayırıcılar olması gerekiyorsa **zorunlu** ' ı seçin. |

## <a name="find-your-created-agreement"></a>Oluşturulan sözleşmeyi bulun

1.  Tüm anlaşma özelliklerinizi ayarlamayı bitirdikten sonra, **Ekle** sayfasında, sözleşmenizi oluşturmayı tamamladıktan sonra tümleştirme hesabınıza geri dönmek için **Tamam** ' ı seçin.

    Yeni eklenen sözleşmeniz artık **anlaşmalar** listenizde görünür.

2.  Ayrıca, tümleştirme hesabınıza genel bakış konusunda anlaşmalar da görüntüleyebilirsiniz. Tümleştirme hesabı menüsünde **genel bakış**' ı seçin, sonra **anlaşmalar** kutucuğunu seçin. 

    ![Anlaşmalar kutucuğunu gösteren ekran görüntüsü.](./media/logic-apps-enterprise-integration-edifact/edifact-4.png)   

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bu bağlayıcı hakkında, bağlayıcının Swagger dosyasında açıklanan eylemler ve sınırlamalar gibi daha teknik ayrıntılar için [bağlayıcının başvuru sayfasına](/connectors/edifact/)bakın.

> [!NOTE]
> Bir [tümleştirme hizmeti ortamındaki (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)Logic Apps için, bu bağlayıcının Ise etiketli sürümü [Ise için B2B ileti sınırlarını](../logic-apps/logic-apps-limits-and-config.md#b2b-protocol-limits)kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
