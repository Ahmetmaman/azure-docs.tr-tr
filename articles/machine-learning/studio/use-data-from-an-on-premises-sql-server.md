---
title: Şirket içi SQL Server
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio (klasik) ile gelişmiş analizler gerçekleştirmek için bir şirket içi SQL Server veritabanından veri kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 03/13/2017
ms.openlocfilehash: 97ab0bd275178a080af3491ba8219d4217e233aa
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75432221"
---
# <a name="perform-analytics-with-azure-machine-learning-studio-classic-using-an-on-premises-sql-server-database"></a>Şirket içi SQL Server veritabanı kullanarak Azure Machine Learning Studio (klasik) analiz gerçekleştirme

Genellikle şirket içi verilerle çalışacak kuruluşların avantajı ölçek ve makine öğrenimi iş yükleri için bulutun çevikliğinden yapmak istiyorsunuz. Ancak, geçerli iş süreçleri ve iş akışları, şirket içi verileri buluta taşıyarak kesintiye istemiyorsanız. Azure Machine Learning Studio (klasik) artık verilerinizi şirket içi SQL Server veritabanından okumayı ve ardından bu verilerle bir modeli eğitmek ve Puanlama destekler. Artık bu el ile kopyalayın ve Bulut ve şirket içi sunucunuz arasında veri eşitlemeyi gerekmez. Bunun yerine, Azure Machine Learning Studio (klasik) **veri alma** modülü artık eğitim ve Puanlama işleriniz için şirket içi SQL Server veritabanından doğrudan okuyabilir.

Bu makalede, şirket içi SQL Server verilerinin Azure Machine Learning Studio (klasik) içine nasıl alınacağını gösteren bir genel bakış sunulmaktadır. Çalışma alanları, modüller, veri kümeleri, denemeleri *vb.* gibi Studio (klasik) kavramlarıyla ilgili bilgi sahibi olduğunuzu varsayar.

> [!NOTE]
> Bu özellik, ücretsiz çalışma alanları için kullanılamaz. Machine Learning fiyatlandırma ve katmanlar hakkında daha fazla bilgi için bkz: [Azure Machine Learning fiyatlandırması](https://azure.microsoft.com/pricing/details/machine-learning/).
>
>

<!-- -->



## <a name="install-the-data-factory-self-hosted-integration-runtime"></a>Data Factory şirket içinde barındırılan Integration Runtime'ı yükleme
Azure Machine Learning Studio (klasik) ' de şirket içi SQL Server veritabanına erişmek için, daha önce Veri Yönetimi ağ geçidi olarak bilinen Data Factory kendi kendine barındırılan Integration Runtime indirip yüklemeniz gerekir. Bağlantıyı Machine Learning Studio (klasik) ' de yapılandırdığınızda, aşağıda açıklanan **veri ağ geçidini indir ve Kaydet** iletişim kutusunu kullanarak INTEGRATION RUNTIME (IR) indirme ve yükleme şansınız vardır.


İndirip MSI Kurulumu paketinden çalıştıran önceden IR yükleyebilirsiniz [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717). MSI, var olan bir IR korunur tüm ayarlarla en son sürüme yükseltmek için de kullanılabilir.

Data Factory şirket içinde barındırılan Integration Runtime, aşağıdaki önkoşulları vardır:

* Data Factory şirket içinde barındırılan Integration bir 64-bit işletim sistemi .NET Framework 4.6.1 veya üstü gerektirir.
* Desteklenen Windows işletim sistemi sürümleri olan Windows 10, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016. 
* IR makine için önerilen yapılandırma, en az 2 GHz, 4 çekirdekli CPU, 8 GB RAM ve 80 GB disk ' dir.
* Konak makine hazırda bekleme, IR veri isteklerine yanıt vermiyor. Bu nedenle, IR yüklemeden önce uygun bir güç planı bilgisayarda yapılandırma Makine hazırda bekleme için yapılandırılmışsa, IR yükleme bir ileti görüntüler.
* Kopyalama etkinliği, belirli bir sıklıkta oluştuğundan, makine üzerinde kaynak kullanımı (CPU, bellek), ayrıca yoğun ve boş kalma sürelerinde ile aynı deseni izler. Kaynak Kullanımı Yoğun taşınan veri miktarı da bağlıdır. Birden çok kopyası işleri devam ederken, kaynak kullanımı yoğun zamanlarda Yukarı Git gözlemleyin. Yukarıda listelenen en düşük yapılandırmayı teknik olarak yeterli olsa da, en düşük yapılandırmayı veri taşıma için belirli yük bağlı olarak daha fazla kaynak ile bir yapılandırmaya sahip isteyebilirsiniz.

Ayarlama ve Data Factory şirket içinde barındırılan Integration Runtime'ı kullanırken aşağıdakileri göz önünde bulundurun:

* IR yalnızca bir örneği, tek bir bilgisayara yükleyebilirsiniz.
* Birden çok şirket içi veri kaynakları için tek bir IR kullanabilirsiniz.
* Farklı bilgisayarlara birden fazla IRS aynı şirket içi veri kaynağına bağlanabilirsiniz.
* Bir IRS tek seferde yalnızca bir çalışma alanı için yapılandırırsınız. Şu anda IRS çalışma alanları arasında paylaşılamaz.
* Tek bir çalışma alanı için birden çok IRS yapılandırabilirsiniz. Örneğin, geliştirme sırasında test veri kaynaklarınıza bağlanmış bir IR ve kullanıma hazır olduğunuzda bir üretim IR kullanmak isteyebilirsiniz.
* IR veri kaynağı ile aynı makinede olması gerekmez. Ancak, veri kaynağına daha yakından kaldığını ağ geçidinin veri kaynağına bağlanmak süreyi azaltır. Kaynaklar için ağ geçidi ve veri kaynağı yoksa rekabet. böylece şirket içi veri kaynağı barındıran olandan farklı bir makineye IR yüklemenizi öneririz.
* Bilgisayarınızda Power BI veya Azure Data Factory senaryolarına hizmet eden bir IR zaten varsa, başka bir bilgisayara Azure Machine Learning Studio (klasik) için ayrı bir IR yüklersiniz.

  > [!NOTE]
  > Data Factory şirket içinde barındırılan Integration Runtime ve Power BI Gateway aynı bilgisayarda çalıştıramazsınız.
  >
  >
* Diğer veriler için Azure ExpressRoute kullanıyor olsanız bile Azure Machine Learning Studio (klasik) için Data Factory kendinden konak Integration Runtime kullanmanız gerekir. Veri kaynağı (bir güvenlik duvarının arkasında olan) bir şirket içi veri kaynağı olarak kabul etmelidir da ExpressRoute kullanırken. Data Factory şirket içinde barındırılan Integration Runtime, makine öğrenimi ve veri kaynağı arasında bağlantı kurmak için kullanın.

Yükleme önkoşulları yükleme adımlarını ve sorun giderme ipuçları hakkında ayrıntılı bilgi makalesinde bulabilirsiniz [Data factory'de tümleştirme çalışma zamanı](../../data-factory/concepts-integration-runtime.md).

## <a name="span-idusing-the-data-gateway-step-by-step-walk-classanchorspan-id_toc450838866-classanchorspanspaningress-data-from-your-on-premises-sql-server-database-into-azure-machine-learning"></a><span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>Şirket içi SQL Server veritabanınızı Azure Machine Learning giriş verileri
Bu kılavuzda, bir Azure Machine Learning çalışma alanında Azure Data Factory Integration Runtime ayarlayacaksınız, yapılandırıp sonra şirket içi SQL Server veritabanından verileri okuyacaksınız.

> [!TIP]
> Başlamadan önce tarayıcınızın açılır pencere engelleyicisi için devre dışı `studio.azureml.net`. Google Chrome tarayıcı kullanıyorsanız, karşıdan yüklenip birkaç eklentileri Google Chrome WebStore kullanılabilir birini [kez tıklayın uygulaması uzantısı](https://chrome.google.com/webstore/search/clickonce?_category=extensions).
>
> [!NOTE]
> Azure Data Factory şirket içinde barındırılan Integration Runtime, daha önce veri yönetimi ağ geçidi biliniyordu. Adım adım öğretici için bir ağ geçidi olarak başvurmaya devam edecek.  

### <a name="step-1-create-a-gateway"></a>1\. adım: bir ağ geçidi oluşturma
İlk adım, oluşturmak ve şirket içi SQL veritabanınıza erişmek için ağ geçidi ayarlama sağlamaktır.

1. [Azure Machine Learning Studio (klasik)](https://studio.azureml.net/Home/) oturumunu açın ve içinde çalışmak istediğiniz çalışma alanını seçin.
2. Tıklayın **ayarları** dikey penceresinde soldaki ve ardından **veri ağ GEÇİTLERİ** en üstteki sekmedeki.
3. Tıklayın **yeni DATA GATEWAY'e** ekranın alt kısmındaki.

    ![Yeni veri ağ geçidi](./media/use-data-from-an-on-premises-sql-server/new-data-gateway-button.png)
4. İçinde **yeni veri ağ geçidi** iletişim kutusunda, girin **ağ geçidi adı** ve isteğe bağlı olarak bir **açıklama**. Alt sağ köşesinde yapılandırmasının sonraki adıma geçmek için oka tıklayın.

    ![Ağ geçidi ad ve açıklama girin](./media/use-data-from-an-on-premises-sql-server/new-data-gateway-dialog-enter-name.png)
5. Yükleme ve kaydetme veri ağ geçidi iletişim kutusunda, ağ geçidi kayıt ANAHTARINI panoya kopyalayın.

    ![İndirme ve veri ağ geçidini kaydetme](./media/use-data-from-an-on-premises-sql-server/download-and-register-data-gateway.png)
6. <span id="note-1" class="anchor"></span>Henüz indirilir ve Microsoft Veri Yönetimi ağ geçidi yüklenir, ardından **indirme veri yönetimi ağ geçidi**. Bu Microsoft Download, indirin, ihtiyacınız olan ağ geçidi sürümü seçin ve yükleme Center götürür. Makale başına bölümlerini yükleme önkoşulları yükleme adımlarını ve sorun giderme ipuçları hakkında ayrıntılı bilgi bulabilirsiniz [şirket içi kaynakları ve veri yönetimi ağ geçidi ile bulut arasında veri taşıma](../../data-factory/tutorial-hybrid-copy-portal.md) .
7. Ağ geçidi yüklendikten sonra veri yönetimi ağ geçidi Yapılandırma Yöneticisi açılır ve **kayıt ağ geçidi** iletişim kutusu görüntülenir. Yapıştırma **ağ geçidi kayıt anahtarı** tıklatın ve Pano'ya kopyaladığınız **kaydetme**.
8. Yüklü bir ağ geçidi zaten varsa, veri yönetimi ağ geçidi Yapılandırma Yöneticisi'ni çalıştırın. Tıklayın **değişiklik anahtarı**, Yapıştır **ağ geçidi kayıt anahtarı** önceki adımda panoya kopyalanır ve tıklayın **Tamam**.
9. Yükleme tamamlandığında, **kayıt ağ geçidi** için Microsoft Veri Yönetimi ağ geçidi Yapılandırma Yöneticisi iletişim kutusu görüntülenir. Panoya bir önceki adımda kopyaladığınız bir ağ geçidi kayıt ANAHTARINI yapıştırın ve tıklayın **kaydetme**.

    ![Ağ geçidini kaydetme](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-register-gateway.png)
10. Aşağıdaki değerleri üzerinde ayarlandığında ağ geçidi Yapılandırma tamamlandıktan **giriş** sekmesini Microsoft Veri Yönetimi ağ geçidi Yapılandırma Yöneticisi'nde:

    * **Ağ geçidi adı** ve **örnek adı** ağ geçidi adına ayarlayın.
    * **Kayıt** ayarlanır **kayıtlı**.
    * **Durum** ayarlanır **başlatıldı**.
    * Durum çubuğu altındaki görüntüler **veri yönetimi ağ geçidi bulut hizmetine bağlanıldı** yeşil onay işaretiyle birlikte.

      ![Veri Yönetimi Ağ Geçidi Yöneticisi](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-registered.png)

      Azure Machine Learning Studio (klasik), kayıt başarılı olduğunda da güncellenir.

    ![Başarılı ağ geçidi kaydı](./media/use-data-from-an-on-premises-sql-server/gateway-registered.png)
11. İçinde **indirin ve veri ağ geçidini kaydetme** iletişim kutusunda Kurulumu tamamlamak için onay işaretine tıklayın. **Ayarları** sayfasında "Çevrimiçi" olarak ağ geçidi durumunu gösterir. Sağ bölmede, durumunu ve diğer yararlı bilgiler bulabilirsiniz.

    ![Ağ Geçidi ayarları](./media/use-data-from-an-on-premises-sql-server/gateway-status.png)
12. Microsoft Veri Yönetimi ağ geçidi Configuration Manager **sertifika** sekmesine geçin. Bu sekmede belirtilen sertifika, portalda belirttiğiniz şirket içi veri deposunun kimlik bilgilerini şifrelemek/şifrelerini çözmek için kullanılır. Bu sertifika, varsayılan sertifikadır. Microsoft, bu sertifika yönetimi sisteminizde yedeklemeniz kendi sertifikanızı değiştirerek önerir. Tıklayın **değişiklik** kendi sertifikanızı yerine kullanılacak.

    ![Ağ geçidi sertifikayı Değiştir](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-certificate.png)
13. (isteğe bağlı) Microsoft Veri Yönetimi ağ geçidi Yapılandırma Yöneticisi'nde ağ geçidi ile ilgili sorunları gidermek için ayrıntılı günlük kaydını etkinleştirmek istiyorsanız geçin **tanılama** denetleyin ve sekme **ayrıntılı etkinleştir sorun giderme amacıyla günlük** seçeneği. Günlük bilgilerini Windows Olay Görüntüleyicisi'ni altında bulunabilir **uygulama ve hizmet günlükleri**  - &gt; **veri yönetimi ağ geçidi** düğümü. Ayrıca **tanılama** ağ geçidini kullanarak şirket içi veri kaynağına olan bağlantıyı sınamak için sekmesinde.

    ![Ayrıntılı günlüğe yazmayı etkinleştir](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-verbose-logging.png)

Bu işlem, Azure Machine Learning Studio (klasik) ağ geçidi kurulum işlemini tamamlar.
Artık, şirket içi verileri kullanmaya hazırsınız.

Her çalışma alanı için Studio 'da (klasik) birden çok ağ geçidi oluşturabilir ve ayarlayabilirsiniz. Örneğin, geliştirme sırasında test veri kaynaklarınıza bağlanmak istediğiniz bir ağ geçidi farklı bir ağ geçidi olarak da, üretim veri kaynakları için sahip olabilir. Azure Machine Learning Studio (klasik), kurumsal ortamınıza bağlı olarak birden çok ağ geçidi ayarlama esnekliği sağlar. Şu anda çalışma alanları arasında bir ağ geçidi paylaşamaz ve yalnızca bir ağ geçidi tek bir bilgisayara yüklenebilir. Daha fazla bilgi için [şirket içi kaynakları ve veri yönetimi ağ geçidi ile bulut arasında veri taşıma](../../data-factory/tutorial-hybrid-copy-portal.md).

### <a name="step-2-use-the-gateway-to-read-data-from-an-on-premises-data-source"></a>2\. adım: ağ geçidini bir şirket içi veri kaynağından verileri okumak için kullanın.
Ağ geçidini ayarlamadan ayarladıktan sonra ekleyebileceğiniz bir **verileri içeri aktarma** şirket içi SQL Server veritabanındaki verileri girdi bir denemeyi modülü.

1. Machine Learning Studio (klasik) içinde, **denemeleri** sekmesini seçin, sol alt köşedeki **+ Yeni** ' ye tıklayın ve **boş denemeler** ' i seçin (veya kullanılabilir örnek denemeleri birini seçin).
2. Bulun ve sürükleyin **verileri içeri aktarma** modülünü deneme tuvaline.
3. Tıklayın **Kaydet** tuval aşağıda. Deneme adı için "Azure Machine Learning Studio (klasik) Şirket Içi SQL Server öğreticisini girin, çalışma alanını seçin ve **Tamam** onay işaretine tıklayın.

   ![Deneme yeni bir adla kaydet](./media/use-data-from-an-on-premises-sql-server/experiment-save-as.png)
4. Tıklayın **verileri içeri aktarma** , ardından seçmek için modül **özellikleri** tuvalinin sağ bölmeye Seç "Şirket içi SQL veritabanı" **veri kaynağı** açılır liste.
5. Seçin **veri ağ geçidi** yüklü ve kayıtlı. Başka bir ağ geçidi "(yeni bir veri ağ geçidi Ekle …)"'i seçerek ayarlayabilirsiniz.

   ![Verileri içeri aktarma modülü için veri ağ geçidi seçin](./media/use-data-from-an-on-premises-sql-server/import-data-select-on-premises-data-source.png)
6. SQL girin **veritabanı sunucu adı** ve **veritabanı adı**, SQL birlikte **veritabanı sorgusu** yürütmek istediğiniz.
7. Tıklayın **değerleri girin** altında **kullanıcı adı ve parola** ve veritabanı kimlik bilgilerinizi girin. Windows tümleşik kimlik doğrulaması veya SQL Server şirket içi SQL Server'ınızı nasıl yapılandırıldığına bağlı olarak kimlik doğrulaması kullanabilirsiniz.

   ![Veritabanı kimlik bilgileri girin](./media/use-data-from-an-on-premises-sql-server/database-credentials.png)

   Yeşil onay işaretiyle birlikte "değerleri kümesi" ileti "gerekli değerleri" değişiklikler. Veritabanı bilgileri veya parola değiştirilmediği sürece kimlik bilgilerinin bir kere girmeniz yeterlidir. Azure Machine Learning Studio (klasik), Bulutta kimlik bilgilerini şifrelemek için ağ geçidini yüklediğinizde verdiğiniz sertifikayı kullanır. Azure, hiçbir zaman şifreleme olmadan şirket içi kimlik bilgilerini depolar.

   ![Veri modülü özelliklerini alma](./media/use-data-from-an-on-premises-sql-server/import-data-properties-entered.png)
8. Tıklayın **ÇALIŞTIRMA** denemeyi çalıştırmak için.

Denemeyi çalıştırma tamamlandığında, çıkış bağlantı noktasına tıklayarak veritabanından alınan verileri görselleştirebilir miyim **verileri içeri aktarma** modülü ve seçerek **Görselleştir**.

Geliştirme deneyiminizi tamamladıktan sonra dağıtabilir ve modelinizi kullanıma hazır hale getirin. Toplu yürütme hizmeti, şirket içi SQL Server'dan veri kullanarak veritabanı yapılandırılan **verileri içeri aktarma** modülü okuyun ve puanlama için kullanılır. Microsoft Puanlama şirket içi verileri için istek yanıt hizmeti kullanabilirsiniz, ancak kullanılmasını önerir [Excel eklentisi](excel-add-in-for-web-services.md) yerine. Şu anda bir şirket içi SQL Server veritabanı yazma **verileri dışarı aktarma** denemelerinizi içinde ya da desteklenen veya yayımlanmış web hizmetleri değildir.
