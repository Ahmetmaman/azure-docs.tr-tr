---
title: Azure Geçişi'ndeki Grup bağımlılık eşlemesi ile bir değerlendirme grubu geliştirmek | Microsoft Docs
description: Azure geçişi hizmeti Grup bağımlılık eşlemesini kullanarak bir değerlendirme iyileştirmek nasıl açıklar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/05/2018
ms.author: raynew
ms.openlocfilehash: 5c4d16ff85972bc4b608e6ce2006912fb27d49d2
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55895440"
---
# <a name="refine-a-group-using-group-dependency-mapping"></a>Grubun bağımlılık eşlemesini kullanarak bir grubu daraltma

Bu makalede, gruptaki tüm makinelerin bağımlılıklarını görselleştirerek bir grubu daraltma açıklar. Bir değerlendirmeyi çalıştırmadan önce mevcut bir grubu için üyelik arası denetimi Grup bağımlılıklarını tarafından iyileştirmek istediğinizde genellikle bu yöntemi kullanın. Bağımlılık görselleştirmesi kullanarak bir grubu daraltma etkili bir şekilde azure'a geçişinizi planlamanıza yardımcı olabilir. Birlikte geçirmek için gereken tüm bağımlı sistemlere bulabilir. Hiçbir şey geride bıraktığı ve Azure'a geçirirken şaşkınlık kesintiler meydana gelmediğinden emin olmanıza yardımcı olur.


> [!NOTE]
> 10'dan fazla makine bağımlılıklarını görselleştirme istediğiniz grupları içermemelidir. Grupta 10'dan fazla makineleriniz varsa, bağımlılık görselleştirme işlevini yararlanmak için daha küçük gruplar halinde ayırmak için önerilir.


## <a name="prepare-for-dependency-visualization"></a>Bağımlılık görselleştirmesi için hazırlama
Azure geçişi, hizmet eşlemesi çözümünü'makineler için bağımlılık görselleştirme etkinleştirmek için Log analytics'te yararlanır.

> [!NOTE]
> Bağımlılık görselleştirme işlevini Azure Kamu'da kullanılabilir değil.

### <a name="associate-a-log-analytics-workspace"></a>Log Analytics çalışma alanını ilişkilendir
Bağımlılık görselleştirmesi yararlanmak için yeni veya var olan, Log Analytics çalışma alanı yeniden eşlemeniz gerekir ile bir Azure geçişi projesi. Yalnızca oluşturma veya geçiş projesi oluşturulduğu aynı Abonelikteki bir çalışma alanı ekleyin.

- İçinde bir proje için bir Log Analytics çalışma alanı eklemek için **genel bakış**Git **Essentials** projenin bölümünü tıklatın **yapılandırma gerektirir**

    ![Log Analytics çalışma alanını ilişkilendir](./media/concepts-dependency-visualization/associate-workspace.png)

- Bir çalışma alanı ilişkilendirilirken yeni bir çalışma alanı oluşturun veya mevcut bir paylaşımın seçeneği alırsınız:
    - Yeni bir çalışma alanı oluşturduğunuzda, çalışma alanı için bir ad belirtmeniz gerekir. Çalışma alanı aynı bölgede oluşturulduktan sonra [her Azure coğrafyası](https://azure.microsoft.com/global-infrastructure/geographies/) geçiş projesi olarak.
    - Mevcut bir çalışma alanı eklediğinizde, geçiş projesi ile aynı abonelikte kullanılabilir olan tüm çalışma arasından seçim yapabilirsiniz. Yalnızca bu çalışma alanlarının bir bölgede oluşturulan listelendiğine dikkat edin burada [hizmet eşlemesi desteklenir](https://docs.microsoft.com/azure/azure-monitor/insights/service-map-configure#supported-azure-regions). Bir çalışma alanı eklemek için çalışma alanına 'Reader' erişiminiz olduğundan emin olun.

> [!NOTE]
> Bir geçiş projesine ilişkili çalışma alanı değiştiremezsiniz.

### <a name="download-and-install-the-vm-agents"></a>Sanal makine aracılarını indirip yükleme
Grup bağımlılıklarını görüntülemek için indirip grubun parçası olan her bir şirket içi makinede aracıları yüklemeniz gerekir. İnternet bağlantısı olmayan makineleriniz varsa, ayrıca, indirmek ve yüklemek ihtiyacınız [Log Analytics gateway](../azure-monitor/platform/gateway.md) bunlar üzerinde.

1. İçinde **genel bakış**, tıklayın **Yönet** > **grupları**gerekli grubuna gidin.
2. Makineler listesinde, **bağımlılık aracısını** sütun tıklayın **yüklemesi gerektirir** aracılarını indirin ve yükleyin ayarlama ile ilgili yönergeleri görmek için.
3. Üzerinde **bağımlılıkları** sayfasında indirin ve grubun bir parçası olan her sanal makinede Microsoft Monitoring Agent (MMA) ve bağımlılık aracısını yükleyin.
4. Çalışma alanı kimliğini ve anahtarını kopyalayın. Şirket içi makinelerde MMA'yı yüklediğinizde bunlar gerekir.

### <a name="install-the-mma"></a>MMA’yı yükleme

#### <a name="install-the-agent-on-a-windows-machine"></a>Aracı bir Windows makinesine yükleyin.

Bir Windows makinede aracı yüklemek için:

1. İndirilen aracıya çift tıklayın.
2. **Hoş Geldiniz** sayfasında **İleri**'ye tıklayın. **Lisans Koşulları** sayfasında **Kabul Ediyorum**’a tıklayarak lisansı kabul edin.
3. İçinde **hedef klasör**, saklamak veya varsayılan yükleme klasörünü değiştirin > **sonraki**.
4. İçinde **Aracı Kurulum Seçenekleri**seçin **Azure Log Analytics** > **sonraki**.
5. Tıklayın **Ekle** yeni bir Log Analytics çalışma alanı eklemek için. Çalışma alanı kimliği ve portaldan kopyaladığınız anahtarını yapıştırın. **İleri**’ye tıklayın.

Komut satırı veya Microsoft Azure Stack veri merkezinizde dağıttıysanız, Azure Automation DSC, System Center Configuration Manager gibi veya bir Azure Resource Manager şablonu ile otomatikleştirilmiş bir yöntem kullanarak aracıyı yükleyebilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent) MMA aracısını yüklemek için bu yöntemleri kullanma hakkında.

#### <a name="install-the-agent-on-a-linux-machine"></a>Bir Linux makine üzerinde aracı yükleme

Bir Linux makinesinde aracıyı yüklemek için:

1. Uygun olan paketi (x86 veya x64), scp/sftp kullanarak Linux bilgisayarınıza aktarın.
2. Paket kullanarak yükleme yükleme bağımsız değişken.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```

#### <a name="install-the-agent-on-a-machine-monitored-by-system-center-operations-manager"></a>Aracıyı System Center Operations Manager tarafından izlenen bir makineye yükleme

Makineler için Operations Manager 2012 R2 veya üzeri izlenen MMA aracısını yüklemek için gerek yoktur. Hizmet eşlemesi, gerekli bağımlılık verileri toplamak için Operations Manager MMA'yı yararlanan bir Operations Manager ile bir tümleştirmeye sahiptir. Tümleştirme yönergeleri kullanarak etkinleştirebilirsiniz [burada](https://docs.microsoft.com/azure/azure-monitor/insights/service-map-scom#prerequisites). Ancak, bu makinelerde yüklü bağımlılık Aracısı'nı gerektiğini unutmayın.

### <a name="install-the-dependency-agent"></a>Bağımlılık aracısını yükleme
1. Bir Windows makinede bağımlılık Aracısı'nı yüklemek için kurulum dosyasına çift tıklayın ve sihirbazı izleyin.
2. Bağımlılık Aracısı'nı bir Linux makineye yüklemek için aşağıdaki komutu kullanarak kök olarak yükleyin:

    ```sh InstallDependencyAgent-Linux64.bin```

Bağımlılık Aracısı desteği hakkında daha fazla bilgi [Windows](../azure-monitor/insights/service-map-configure.md#supported-windows-operating-systems) ve [Linux](../azure-monitor/insights/service-map-configure.md#supported-linux-operating-systems) işletim sistemleri.

[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples) bağımlılık Aracısı'nı yüklemek için komut dosyalarını nasıl kullanabileceğiniz hakkında.

## <a name="refine-the-group-based-on-dependency-visualization"></a>Bağımlılık görselleştirme üzerinde göre Grubu Daralt
Grubun tüm makinelerde aracılarını yükledikten sonra Grup bağımlılıklarını görselleştirin ve izleyerek İyileştir aşağıdaki adımları.

1. Azure projesi altında geçirme **Yönet**, tıklayın **grupları**, grubunu seçin.
2. Grup sayfasında tıklayın **bağımlılıklarını görüntüleme**grubun bağımlılık Haritası'nı açmak için.
3. Grup için bağımlılık eşlemi aşağıdaki ayrıntıları gösterir:
    - (İstemciler) gelen ve giden (sunucu) TCP bağlantıları gönderip buralardan grubunun parçası olan tüm makineler
        - MMA ve bağımlılık aracısı yüklü olmayan bağımlı makineler bağlantı noktası numaralarını tarafından gruplandırılır.
        - MMA ve bağımlılık aracısının yüklü olduğu bağımlı makineler, ayrı kutular olarak gösterilir
    - İşlemler, makinenin içinde çalışan işlemleri görüntülemek için her makine kutusunu genişletebilirsiniz.
    - Tam etki alanı adı, işletim sistemi, her makinenin MAC adresi vb. gibi özellikler, bu ayrıntıları görüntülemek için her makine kutusuna tıklayın

     ![Grup bağımlılıklarını görüntüleme](./media/how-to-create-group-dependencies/view-group-dependencies.png)

3. Daha ayrıntılı bağımlılıkları görüntülemek için zaman aralığını değiştirmek için tıklayın. Varsayılan olarak, aralığı bir saattir. Zaman aralığı değiştirmek veya başlangıç ve bitiş tarihlerini ve süresini belirtin.

    > [!NOTE]
      Şu anda, bağımlılık görselleştirmesi UI bir saatten uzun bir zaman aralığı seçimini desteklemez. Log Analytics'i [bağımlılık verileri sorgulamak](https://docs.microsoft.com/azure/migrate/how-to-create-a-group) üzerinden uzun bir süre.

4. Bağımlı makineler, her makinenin içinde çalışan işlemi doğrulayın ve eklenebilir veya gruptan makine tanımlayın.
5. Ekleyip bunları gruptan çıkarmak için harita üzerinde makineler seçmek için CTRL tuşuna basıp tıklayarak kullanın.
    - Bulunan makineler yalnızca ekleyebilirsiniz.
    - Ekleme ve makineleri gruptan kaldırma geçmiş değerlendirmeleri için onu geçersiz kılar.
    - Grubun değişiklik yaptığınızda, isteğe bağlı olarak yeni bir değerlendirme oluşturabilirsiniz.
5. Tıklayın **Tamam** grubunu kaydetmek için.

    ![Makine Ekle Kaldır](./media/how-to-create-group-dependencies/add-remove.png)

Grubun bağımlılık haritası görüntülenir, belirli bir makinenin bağımlılıklarını denetlemek istiyorsanız [makine bağımlılık eşlemesi ayarlayın](how-to-create-group-machine-dependencies.md).

## <a name="query-dependency-data-from-log-analytics"></a>Log Analytics sorgu bağımlılık verileri

Hizmet eşlemesi tarafından yakalanan bağımlılık verileri Azure geçişi projenizle ilişkili Log Analytics workspare sorgulama için kullanılabilir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#log-analytics-records) Log Analytics'te sorgu hizmet eşlemesi veri tabloları hakkında. 

Log Analytics sorguları çalıştırmak için:

1. Aracıları yükledikten sonra portal ve tıklayın Git **genel bakış**.
2. İçinde **genel bakış**Git **Essentials** yanındaki sağlanan çalışma alanı adına tıklayın ve proje bölümünü **OMS çalışma alanı**.
3. Log Analytics çalışma alanı sayfasında tıklayın **genel** > **günlükleri**.
4. Log Analytics kullanarak bağımlılık veri toplamak üzere sorgunuzu yazın. Bağımlılık verileri toplamak için örnek sorgular kullanılabilir [burada](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#sample-log-searches).
5. Sorguyu Çalıştır'ı tıklayarak çalıştırın. 

[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal) Log Analytics sorguları yazma hakkında. 


## <a name="next-steps"></a>Sonraki adımlar
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/resources-faq#dependency-visualization) bağımlılık görselleştirme hakkında sık sorulan sorular hakkında.
- Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
