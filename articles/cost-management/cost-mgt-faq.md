---
title: Cloudyn Azure için sık sorulan sorular | Microsoft Docs
description: Cloudyn hakkında genel soruların yanıtlarını sağlar.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/18/2018
ms.topic: troubleshooting
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 2126875a18d4a6581629ea1c8362236242a666a8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46961034"
---
# <a name="frequently-asked-questions-for-cloudyn"></a>Cloudyn için sık sorulan sorular

Bu makalede Cloudyn hakkında bazı yaygın sorular ele alınmıştır. Cloudyn hakkında sorularınız varsa, onları sorabilir [Cloudyn için sık sorulan sorular](https://social.msdn.microsoft.com/Forums/231bf072-2c71-4121-8339-ac9d868137b9/faqs-for-azure-cost-management-by-cloudyn?forum=Cloudyn).

## <a name="how-can-i-resolve-common-indirect-enterprise-setup-problems"></a>Sık karşılaşılan dolaylı Kurumsal kurulum sorunlarını nasıl giderebilirim?

Cloudyn portalını ilk kez kullandığınızda, bir Kurumsal Anlaşma ya da bulut çözümü sağlayıcısı (CSP) kullanıcı varsa aşağıdaki iletileri görebilirsiniz:

- "Belirtilen API anahtarı bir üst düzey kayıt anahtarı değil" görüntülenen **Cloudyn ayarlamak** Sihirbazı.
- "– Hayır Kurumsal Anlaşma portalında gösterilen doğrudan kayıt".
- "Son 30 güne ait kullanım verisi bulunamadı. Biçimlendirme Azure hesabınız için etkinleştirildi emin olmak için dağıtımcı başvurun", Cloudyn portalında görüntülenmez.

Önceki ileti, bir kurumsal bayi veya CSP aracılığıyla Azure Kurumsal Anlaşma satın aldığınızı belirtir. Satıcınıza veya CSP etkinleştirmek gereken _biçimlendirme_ Azure hesabı için böylece verilerinizi Cloudyn'de görüntüleyebilirsiniz.

Sorunların çözümü:

1. Kurumsal bayinin hesabınız için _işaretlemeyi_ etkinleştirmesi gerekir. Yönergeler için bkz. [Dolaylı Müşteri Ekleme Kılavuzu](https://ea.azure.com/api/v3Help/v2IndirectCustomerOnboardingGuide).

2. Cloudyn ile kullanmak için Azure Kurumsal Anlaşma anahtarı üret Yönergeler için [ekleme bilgisayarınızı Azure EA](https://support.cloudyn.com/hc/articles/210429585-Adding-Your-AZURE-EA) veya [Bul EA kayıt Kimliğinizi ve API anahtarı](https://youtu.be/u_phLs_udig).

Yalnızca bir Azure Hizmet Yöneticisi, Cloudyn etkinleştirebilirsiniz. Ortak yönetici izinleri yeterli değil.

Cloudyn ayarlamak için Azure Kurumsal Anlaşma API anahtarı oluşturmadan önce adresindeki yönergeleri izleyerek Azure faturalandırma API'si etkinleştirmeniz gerekir:

- [Kurumsal müşteriler için Raporlama API’lerine genel bakış](../billing/billing-enterprise-api.md)
- **API’lere veri erişimini etkinleştirme** bölümünde [Microsoft Azure kurumsal portal Raporlama API’si](https://ea.azure.com/helpdocs/reportingAPI)


Departman yöneticilerine, hesap sahiplerine ve kurumsal yöneticilere Faturalama API’si ile _ücretleri görüntüleme_ izni vermeniz de gerekebilir.

## <a name="why-dont-i-see-optimizer-recommendations"></a>İyileştirici önerileri neden göremiyorum?

Öneri bilgiler, yalnızca etkinleştirilen hesapları için kullanılabilir. Herhangi bir öneri bilgisi görmeyeceğiniz **iyileştirici** rapor hesapları için kategorileri *etkinleştirilmemiş*de dahil olmak üzere:

- En iyi duruma getirme Yöneticisi
- Boyutlandırma optimizasyonu
- Verimsizlikleri

İyileştirici öneri verileri görüntüleyemezsiniz sonra büyük olasılıkla etkinleştirilmemiş olan hesapların varsa. Bir hesabı etkinleştirmek için Azure kimlik bilgilerinizle kaydetmeniz gerekir.

Bir hesabı etkinleştirmek için:

1.  Cloudyn portalında, sağ üst kısımdaki **Ayarlar**’a tıklayın ve **Bulut Hesapları**’nı seçin.
2.  Microsoft Azure hesapları sekmesinde sahip hesaplar için Ara bir **etkinleştirilmemiş** abonelik.
3.  Sağa etkinleştirilmemiş bir hesabın tıklayın **Düzenle** benzer bir kalem simgesi.
4.  Kiracı kimliği ve oran Kimliğinizi otomatik olarak algılanır. **İleri**’ye tıklayın.
5.  Azure portalına yönlendirilirsiniz. Portalda oturum açın ve Azure verilerinize erişmek için Cloudyn Toplayıcı yetkisi verin.
6.  Ardından, Cloudyn hesapları Yönetim sayfasına yönlendirilirsiniz ve aboneliğiniz ile güncelleştirilir **etkin** hesap durumu. Bu, yeşil onay işareti simgesi gösterir.
7.  Bir veya daha fazla abonelik için bir yeşil onay işareti simgesini görmüyorsanız, (CloudynCollector) abonelik için bir okuyucu uygulaması oluşturmak için gerekli izinlere sahip değilsiniz demektir. Abonelik için daha yüksek izinlere sahip bir kullanıcı, 3 ve 4 gerekiyor.  

Yukarıdaki adımları tamamladıktan sonra bir veya iki gün içinde iyileştirici önerileri görüntüleyebilirsiniz. Ancak, uygulamanın tam iyileştirme veri kullanılabilir olmadan önce en fazla beş gün sürebilir.


## <a name="how-do-i-enable-suspended-or-locked-out-users"></a>Askıya alınmış veya kilitlenmiş kullanıcıları nasıl etkinleştirebilirim?

İlk olarak, kullanıcı hesaplarını almak neden en yaygın senaryo bakalım *initiallySuspended*.

> Microsoft bulut çözümü sağlayıcısı veya Kurumsal Anlaşma kullanıcı admin1 olabilir. Kuruluşundaki Cloudyn kullanmaya başlamak hazır olur.  Kendisi, Azure portalı üzerinden kaydeder ve Cloudyn portalında oturum açar. Cloudyn portalında oturum açtığında ve Cloudyn hizmet kaydeder kişi kendisine olur *birincil yönetici*. Tüm kullanıcı hesaplarını admin1 oluşturmaz. Ancak, Cloudyn portalını kullanarak yaptığı Azure hesapları oluşturma ve bir varlık hiyerarşisi ayarlar. Admin1 Admin2, kendisinin Cloudyn'e kaydetmek ve Cloudyn portalında oturum açmak için gereken bir kiracı Yöneticisi bildirir.

> Azure portalı üzerinden Admin2 kaydeder. Ancak o hesabını olduğunu söyleyen bir hata alır kendisi, Cloudyn portalında oturum açmak çalıştığında, **askıya**. Admin1, birincil yönetici hesabı askıya alınması bildirilir. Admin1 Admin2'ın hesabı etkinleştirin ve vermek için gereksinim duyduğu *yönetici varlık erişimi* uygun varlıkların ve kullanıcı yönetim erişimini ve etkin kullanıcı hesabı sağlar.


Bir istek, bir kullanıcı için erişim izni olan bir uyarı alırsanız, kullanıcı hesabı'nı etkinleştirmeniz gerekir.

Kullanıcı hesabını etkinleştirmek için:

1. İçin Cloudyn, Cloudyn ' için kullandığınız Azure yönetici kullanıcı hesabıyla oturum açın. Veya yönetici erişim izni bir kullanıcı hesabıyla oturum açın.
2. Sağ üst köşedeki dişli simgesini seçin ve seçin **kullanıcı yönetimi**.
3. Kullanıcıyı bulun, kalem simgesini seçin ve ardından kullanıcı düzenleyin.
4. Altında **kullanıcı durumu**, durumu değiştirme **askıya alındı** için **etkin**.

Cloudyn kullanıcı hesapları, çoklu oturum açmayı azure'dan kullanarak bağlanın. Bir kullanıcı parolasını mistypes, Azure yine de erişebilmeleri olsa da bunlar Cloudyn dışında saldırdığında.

Azure'da varsayılan adresinden e-posta adresinizi Cloudyn değiştirirseniz, hesabınızın kilitli. "Durumu initiallySuspended" gösterebilir Kullanıcı hesabınız kilitlendi, hesabınızı sıfırlamak için başka bir yönetici ile iletişime geçin.

Hesaplardan birini kilitli durumunda en az iki Cloudyn yöneticisi hesabı oluşturmanızı öneririz.

Cloudyn portalında oturum açamıyorsanız Cloudyn'e oturum açmak için doğru URL'yi kullandığınızdan emin olun. Kullanım [ https://azure.cloudyn.com ](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/CloudynMainBlade).

Cloudyn doğrudan URL kullanmaktan kaçının https://app.cloudyn.com.

## <a name="how-do-i-activate-unactivated-accounts-with-azure-credentials"></a>Azure kimlik etkinleştirilmemiş hesaplarıyla nasıl etkinleştirebilirim?

Hesaplarınız Cloudyn tarafından bulunduktan hemen sonra maliyet verilerini hemen maliyet tabanlı raporlara sağlanır. Bununla birlikte, Cloudyn kullanım ve performans verilerini sağlamak hesapları için Azure kimlik bilgilerinizi kaydetmeniz gerekir. Yönergeler için [Azure Resource Manager'ı ekleme](https://support.cloudyn.com/hc/articles/212784085-Adding-Azure-Resource-Manager).

Azure'ı eklemek için hesap adı, abonelik sağındaki düzenleme simgesi Cloudyn portalında, bir hesabın kimlik bilgilerini seçin.

Azure kimlik bilgilerinizi Cloudyn'e eklenene kadar hesabı olarak görünür _beklemediğiniz etkinleştirilmiş_.

## <a name="how-do-i-add-multiple-accounts-and-entities-to-an-existing-subscription"></a>Mevcut bir aboneliğe birden çok hesap ve varlıkları nasıl ekleyebilirim?

Ek varlıklar Cloudyn aboneliğine ek Kurumsal anlaşmalar eklemek için kullanılır. Aşağıdaki bağlantılar, ek varlıklar ekleme işlemi açıklanmaktadır:

- [Varlık ekleme](https://support.cloudyn.com/hc/articles/212016145-Adding-an-Entity) makale
- [Maliyet varlıkları ile hiyerarşinizi tanımlama](https://support.cloudyn.com/hc/articles/115005142529-Video-Defining-your-hierarchy-with-Cost-Entities) video

CSP'ler için:

Bir varlığa ek CSP hesapları eklemek için seçin **MSP erişim** yerine **Kurumsal** yeni bir varlık oluşturduğunuzda. Hesabınızı bir Kurumsal Anlaşma kaydedilir ve CSP kimlik bilgileri eklemek istediğiniz, Cloudyn destek personeli, hesap ayarlarını değiştirmek gerekebilir. Ücretli bir Azure abonesi iseniz, Azure portalında yeni bir destek isteği oluşturabilirsiniz. Seçin **Yardım + Destek**ve ardından **yeni destek isteği**.

## <a name="currency-symbols-in-cloudyn-reports"></a>Cloudyn raporlarında para birimi sembolleri

Farklı para birimlerinin kullanarak birden çok Azure hesabında olabilir. Bununla birlikte, Cloudyn maliyet raporlarında, rapor başına birden fazla para birimi türü gösterme.

Farklı para birimlerinin kullanarak birden fazla aboneliğiniz varsa, bir üst varlık ve onun alt varlık para ABD Doları görüntülenir **$**. Bizim önerilen en iyi uygulama, aynı varlık hiyerarşideki farklı para birimlerinin kullanarak önlemek içindir. Diğer bir deyişle, bir varlık yapısında düzenlenmiş tüm abonelikler aynı para kullanmanız gerekir.

Cloudyn, otomatik olarak, Kurumsal Anlaşma abonelik para algılar ve düzgün şekilde raporlar sunar.  Bununla birlikte, Cloudyn ABD Doları yalnızca görüntüler **$** CSP ve doğrudan web Azure hesapları için.

## <a name="what-are-cloudyn-data-refresh-timelines"></a>Cloudyn veri nelerdir zaman çizelgeleri yenilensin mi?

Cloudyn, aşağıdaki veri yenileme zaman çizelgeleri sahiptir:

- **İlk**: ayarladıktan sonra Cloudyn maliyet verilerini görüntülemek için 24 saat sürebilir. Ayrıca, Cloudyn boyutlandırma önerileri görüntülemek için yeterli veri toplamak üzere 10 güne kadar da sürebilir.
- **Günlük**: on günün sonuna kadar her ay, Cloudyn verilerinizin önceki günden sonra UTC + 3 hakkında sonraki günün tarihi göstermelidir.
- **Aylık**: her ayın on günü için ilk günden Cloudyn verilerinizi yalnızca önceki ayın sonuna üzerinden gösterebilir.

Cloudyn, önceki günün önceki gün tam veri kullanılabilir olduğunda verileri işler. Önceki günün verileri her gün genellikle UTC + 3 hakkında Cloudyn tarafından kullanılabilir. Etiketler gibi bazı veriler ilaveten 24 işlemek için saat sürebilir.

Geçerli ay için verileri, her ayın başında toplamalarında kullanılamaz. Dönemi boyunca, hizmet sağlayıcıları önceki ay için bunların faturalandırma sonlandırın. Önceki ayın verilerini 10 gün sonra her ay başı Cloudyn 5'te görünür. Bu süre boyunca, yalnızca önceki ayın amorti edilmiş maliyet görebilirsiniz. Günlük faturalandırma ya da kullanım verilerini göremeyebilirsiniz. Cloudyn veri kullanılabilir olduğunda, geriye dönük olarak işler. İşlemden sonra tüm aylık verileri görüntülenir her ayın on günü beşinci arasında.

Cloudyn'e Azure'dan veri gönderen bir gecikme varsa, verileri Azure'da hala kaydedilir. Bağlantı geri geldiğinde, veriler Cloudyn'e aktarılır.

## <a name="cost-fluctuations-in-cloudyn-cost-reports"></a>Cloudyn maliyet raporlarında maliyet dalgalanmaları

Her bulut hizmeti sağlayıcıları güncelleştirilmiş fatura dosyaları gönderdiğinizde maliyet raporlarında maliyet dalgalanmaları gösterebilirsiniz. Yeni dosyaları bir bulut hizmeti sağlayıcısından dışında normal günlük veya aylık zamanlama bildirimi alındığında dalgalı maliyetler oluşur. Maliyet değişiklikler Cloudyn yeniden hesaplanmasına neden yoktur.

Ay boyunca, bulut hizmet sağlayıcınız tarafından gönderilen tüm faturalandırma günlük maliyetlerinizi tahmini dosyalarıdır. Bazen verileri sık sık güncelleştirilir — birden çok kez günde bazen. AWS ile Azure sık güncelleştirmeler. Önceki aya ait fatura hesaplama ve son fatura dosya alındığında, maliyet toplamları kararlı kalmalıdır. Genellikle, 10 ay tarafından.

Değişiklikler, bulut hizmeti sağlayıcısından maliyet ayarlamaları aldığında gerçekleşir. Kredisi alma örnek verilebilir. İlgili ay kapatıldıktan sonra değişiklikleri ay ortaya çıkabilir. Değişiklikler yeniden hesaplama bulut hizmet sağlayıcınız tarafından yapılan her. Cloudyn, tüm ayarlarını hesaplanır emin olmak için geçmiş verilerini güncelleştirir. Maliyetlerini doğru şekilde içinde görüntülendiğini raporları olduğunu doğrular.

## <a name="how-can-a-direct-csp-configure-cloudyn-access-for-indirect-csp-customers-or-partners"></a>Nasıl doğrudan bir CSP dolaylı CSP müşterileri veya iş ortakları için Cloudyn erişimi yapılandırabilirsiniz?

Yönergeler için [Cloudyn'de dolaylı CSP erişimini yapılandırma](quick-register-csp.md#configure-indirect-csp-access-in-cloudyn).

## <a name="what-causes-the-optimizer-menu-item-to-appear"></a>Görüntülenecek iyileştirici menü öğesi nedeni nedir?

Azure Resource Manager erişim ve veri ekledikten sonra görmelisiniz, toplanır **iyileştirici** seçeneği. Azure Resource Manager erişimi etkinleştirmek için bkz: [nasıl etkinleştirilmemiş hesapları Azure kimlik bilgileri ile etkinleştirebilirim?](#how-do-i-activate-unactivated-accounts-with-azure-credentials)

## <a name="is-cloudyn-agent-based"></a>Cloudyn Aracısı bağlıdır?

Hayır. Aracıları kullanılmaz. Microsoft Insights API'den VM'ler için Azure sanal makine ölçüm verileri toplanır. Azure sanal makinelerinden ölçüm verilerini toplamak istiyorsanız, tanılama ayarları etkin olması gerekir.

## <a name="do-cloudyn-reports-show-more-than-one-ad-tenant-per-report"></a>Cloudyn raporlarını, rapor başına birden fazla AD Kiracı göstermemesidir?

Evet. Yapabilecekleriniz [karşılık gelen bir bulut hesap varlığı oluşturma](tutorial-user-access.md#create-and-manage-entities) sahip olduğunuz her bir AD Kiracı için. Ardından, tüm Azure AD Kiracı verilerinizi ve Amazon Web Services ve Google Cloud Platform dahil diğer bulut platform sağlayıcıları görüntüleyebilirsiniz.
