---
title: Azure Stack VM 'lerde dosyaları yedekleme
description: Azure Stack dosyalarını ve uygulamaları Azure Stack ortamınıza yedeklemek ve kurtarmak için Azure Backup kullanın.
ms.topic: conceptual
ms.date: 06/05/2018
ms.openlocfilehash: cae95c10c510969cc0553a54a506789d6be427d7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "89180992"
---
# <a name="back-up-files-and-applications-on-azure-stack"></a>Azure Stack dosya ve uygulamaları yedekleme

Azure Stack üzerinde dosya ve uygulamaları korumak (veya yedeklemek) için Azure Backup kullanabilirsiniz. Dosya ve uygulamaları yedeklemek için, Azure Stack üzerinde çalışan bir sanal makine olarak Microsoft Azure Backup sunucusu ' nu kurun. Aynı sanal ağdaki herhangi bir Azure Stack sunucusundaki dosyaları koruyabilirsiniz. Azure Backup Sunucusu yükledikten sonra, kısa vadeli yedekleme verileri için kullanılabilir yerel depolamayı artırmak üzere Azure diskleri ekleyin. Azure Backup Sunucusu uzun süreli saklama için Azure depolama kullanır.

> [!NOTE]
> Azure Backup Sunucusu ve System Center Data Protection Manager (DPM) benzerdir, ancak DPM Azure Stack ile kullanım için desteklenmez.
>

Bu makale, Azure Stack ortamında Azure Backup Sunucusu yüklemeyi kapsamaz. Azure Stack Azure Backup Sunucusu yüklemek için [Azure Backup sunucusu yükleme](backup-mabs-install-azure-stack.md)makalesine bakın.

## <a name="back-up-files-and-folders-in-azure-stack-vms-to-azure"></a>Azure Stack VM 'lerdeki dosya ve klasörleri Azure 'a yedekleme

Azure Stack sanal makinelerdeki dosyaları korumak üzere Azure Backup Sunucusu yapılandırmak için Azure Backup Sunucusu konsolunu açın. Koruma gruplarını yapılandırmak ve sanal makinelerinizdeki verileri korumak için konsolunu kullanırsınız.

1. Azure Backup Sunucusu konsolunda **koruma** ' yı seçin ve araç çubuğunda **Yeni** ' yi seçerek **yeni koruma grubu oluşturma** Sihirbazı ' nı açın.

   ![Azure Backup Sunucusu konsolunda korumayı yapılandırma](./media/backup-mabs-files-applications-azure-stack/1-mabs-menu-create-protection-group.png)

    Sihirbazın açılması birkaç saniye sürebilir. Sihirbaz açıldığında, **İleri** ' yi seçerek **koruma grubu türü seçin** ekranına ilerleyin.

   ![Yeni Koruma Grubu Sihirbazı açılır](./media/backup-mabs-files-applications-azure-stack/2-create-new-protection-group-wiz.png)

2. **Koruma grubu türünü seçin** ekranında **sunucular** ' ı seçin ve ileri ' **yi** seçin.

    ![Koruma grubu türünü seçin](./media/backup-mabs-files-applications-azure-stack/3-select-protection-group-type.png)

    **Grup üyelerini seçin** ekranı açılır.

    ![Grup üyelerini seçin](./media/backup-mabs-files-applications-azure-stack/4-opening-screen-choose-servers.png)

3. **Grup üyelerini seçin** ekranında, alt **+** öğeleri listesini genişletmeyi seçin. Korumak istediğiniz tüm öğeler için onay kutusunu işaretleyin. Tüm öğeler seçildikten sonra **İleri**' yi seçin.

    ![Korumak istediğiniz her öğeyi seçin](./media/backup-mabs-files-applications-azure-stack/5-select-group-members.png)

    Microsoft, bir koruma ilkesini paylaşacak tüm verilerin tek bir koruma grubuna yerleştirilmesini önerir. Koruma gruplarını planlama ve dağıtma hakkında ayrıntılı bilgi için bkz. System Center DPM makalesi, [koruma gruplarını dağıtma](/system-center/dpm/create-dpm-protection-groups).

4. **Veri koruma yöntemini seçin** ekranında, koruma grubu için bir ad yazın. **Kullanarak kısa vadeli koruma** istiyorum onay kutusunu seçin ve **çevrimiçi koruma** istiyorum ' u seçin. **İleri**’yi seçin.

    ![Veri koruma yöntemini seçin](./media/backup-mabs-files-applications-azure-stack/6-select-data-protection-method.png)

    **Çevrimiçi koruma** istiyorum ' u seçmek için, önce: disk **kullanarak kısa vadeli koruma istiyorum** ' u seçmeniz gerekir. Azure Backup Sunucusu banda korumadığı için disk, kısa vadeli koruma için tek seçenektir.

5. **Short-Term hedeflerini belirtin** ekranında, diske kaydedilen kurtarma noktalarının ne kadar süreyle saklanacağını ve artımlı yedeklemelerin ne zaman kaydedileceğini seçin. **İleri**’yi seçin.

    > [!IMPORTANT]
    > Beş günden uzun bir süre boyunca Azure Backup Sunucusu bağlı disklerde işlemsel kurtarma (yedekleme) **verilerini saklamamalıdır** .
    >

    ![Short-Term hedeflerini belirtin](./media/backup-mabs-files-applications-azure-stack/7-select-short-term-goals.png)

    Artımlı yedeklemeler için bir Aralık seçmek yerine, zamanlanan her kurtarma noktasının hemen öncesinde hızlı tam yedekleme çalıştırmak için, **bir kurtarma noktasındaki hemen önce** seçeneğini belirleyin. Uygulama iş yüklerini koruyorsanız, Azure Backup Sunucusu eşitleme sıklığı zamanlaması başına kurtarma noktaları oluşturur (uygulama artımlı yedeklemeleri desteklediğinde). Uygulama artımlı yedeklemeleri desteklemiyorsa Azure Backup Sunucusu hızlı tam yedekleme çalıştırır.

    **Dosya kurtarma noktaları** için, kurtarma noktalarının ne zaman oluşturulacağını belirtin. Kurtarma noktalarının oluşturulduğu haftanın saatlerini ve günlerini ayarlamak için **Değiştir** ' i seçin.

6. **Disk ayırmayı İncele** ekranında, koruma grubu için ayrılmış depolama havuzu disk alanını gözden geçirin.

    **Toplam veri boyutu** , yedeklemek istediğiniz verilerin boyutudur ve Azure Backup sunucusu üzerinde **sağlanacak disk alanı** , koruma grubu için önerilen alandır. Azure Backup Sunucusu, ayarlara bağlı olarak ideal yedekleme birimini seçer. Ancak, yedekleme birimi seçeneklerini Disk ayırma ayrıntıları'nda düzenleyebilirsiniz. İş yükleri için açılan menüden tercih edilen depolamayı seçin. Düzenlemeleriniz, Kullanılabilir Disk Depolaması bölmesindeki Toplam Depolama ve Boş Depolama değerlerini değiştirir. Yetersiz sağlanan alan, daha sonra yedeklemeye sorunsuz bir şekilde devam etmek için, birime eklemenizi öneren Azure Backup Sunucusu depolama miktarıdır.

7. **Çoğaltma oluşturma yöntemini seçin** kısmında ilk tam veri çoğaltmasını nasıl işlemek istediğinizi seçin. Ağ üzerinden çoğaltmaya karar verirseniz, Azure yoğun olmayan bir zaman seçmenizi önerir. Büyük miktarlarda veri veya en iyi ağ koşullarına göre, verileri çıkarılabilir medya kullanarak çoğaltmayı göz önünde bulundurun.

8. **Tutarlılık Denetimi Seçenekleri** kısmında, tutarlılık denetimlerinin nasıl otomatikleştirilmesini istediğinizi seçin. Tutarlılık denetimlerini yalnızca veri çoğaltma tutarsız hale geldiğinde veya bir zamanlamaya göre çalışacak şekilde etkinleştirin. Otomatik tutarlılık denetimini yapılandırmak istemiyorsanız, istediğiniz zaman el ile denetim çalıştırın:
    * Azure Backup Sunucusu konsolunun **koruma** alanında, koruma grubuna sağ tıklayın ve **tutarlılık denetimi gerçekleştir**' i seçin.

9. Azure 'a yedeklemeyi seçerseniz, **çevrimiçi koruma verilerini belirtin** sayfasında Azure 'a yedeklemek istediğiniz iş yüklerinin seçili olduğundan emin olun.

10. **Çevrimiçi yedekleme zamanlamasını belirtin** kısmında, Azure 'a artımlı yedeklemelerin ne zaman gerçekleşeceğini belirtin.

    Yedeklemeleri her gün/hafta/ay/yıl yapılacak şekilde ve çalışacakları saat/tarihi belirterek zamanlayabilirsiniz. Yedeklemeler günde iki kez gerçekleşebilir. Bir yedekleme işi her çalıştığında, Azure 'da Azure Backup Sunucusu diskte depolanan yedeklenen verilerin kopyasından bir veri kurtarma noktası oluşturulur.

11. **Çevrimiçi saklama Ilkesini belirtin** kısmında günlük/haftalık/aylık/yıllık yedeklerden oluşturulan kurtarma noktalarının Azure 'da nasıl korunacağını belirtin.

12. **Çevrimiçi çoğaltma Seç**' de, verilerin ilk tam çoğaltmasının nasıl gerçekleşeceğini belirtin.

13. **Özet** sayfasında, ayarlarınızı gözden geçirin. **Grup Oluştur**' u seçtiğinizde, ilk veri çoğaltma işlemi gerçekleşir. Veri çoğaltma tamamlandığında, **durum** sayfasında, koruma grubu durumu **Tamam** olarak gösterilir. İlk yedekleme işi, koruma grubu ayarları ile birlikte gerçekleşir.

## <a name="recover-file-data"></a>Dosya verilerini kurtar

Verileri sanal makinenize kurtarmak için Azure Backup Sunucusu konsolunu kullanın.

1. Azure Backup Sunucusu konsolunda, gezinti çubuğunda **Kurtarma** ' yı seçin ve kurtarmak istediğiniz verilere gidin. Sonuçlar bölmesinde verileri seçin.

2. Kurtarma noktaları bölümündeki takvimde, kalın olan tarihler kurtarma noktalarının kullanılabilir olduğunu gösterir. Kurtarılacak tarihi seçin.

3. **Kurtarılabilir öğe** bölmesinde, kurtarmak istediğiniz öğeyi seçin.

4. **Eylemler** bölmesinde, Kurtarma Sihirbazı 'nı açmak için **kurtar** ' ı seçin.

5. Verileri aşağıdaki şekilde kurtarabilirsiniz:

    * **Özgün konuma kurtar** -ISTEMCI bilgisayar VPN üzerinden bağlandıysa, bu seçenek çalışmaz. Bunun yerine, alternatif bir konum kullanın ve ardından bu konumdan verileri kopyalayın.
    * **Alternatif bir konuma kurtar**

6. Kurtarma seçeneklerini belirtin:

    * **Var olan sürüm kurtarma davranışı** Için **kopyalama oluştur**, **Atla** veya **üzerine yaz**' ı seçin. Üzerine yazma yalnızca özgün konuma kurtarma sırasında kullanılabilir.
    * **Güvenlik geri yükleme** için **hedef bilgisayarın ayarlarını uygula** ' yı veya **Kurtarma noktası sürümünün güvenlik ayarlarını uygula**' yı seçin.
    * **Ağ bant genişliği kullanımını azaltma** için, ağ bant genişliği kullanımını azaltmayı etkinleştirmek için **Değiştir** ' i seçin.
    * **Bildirim** **Kurtarma tamamlandığında e-posta gönder**' i seçin ve bildirimi alacak alıcıları belirtin. E-posta adreslerini virgülle ayırın.
    * Seçimleri yaptıktan sonra **İleri** ' yi seçin.

7. Kurtarma ayarlarınızı gözden geçirin ve **kurtar**' ı seçin.

    >[!Note]
    >Kurtarma işi devam ederken, seçilen kurtarma öğeleri için tüm eşitleme işleri iptal edilir.

Modern Yedekleme Alanı (MB) kullanıyorsanız, dosya sunucusu son kullanıcı kurtarma (EUR) desteklenmez. Dosya sunucusu EUR 'nin Birim Gölge Kopyası Hizmeti (VSS) bağımlılığı vardır ve bu Modern Yedekleme Alanı kullanmaz. EUR etkinse, verileri kurtarmak için aşağıdaki adımları kullanın:

1. Korunan dosyalar ' a gidin ve dosya adına sağ tıklayıp **Özellikler**' i seçin.

2. **Özellikler** menüsünde, **önceki sürümler** ' i seçin ve kurtarmak istediğiniz sürümü seçin.

## <a name="view-azure-backup-server-with-a-vault"></a>Kasala Azure Backup Sunucusu görüntüleme

Azure portal Azure Backup Sunucusu varlıkları görüntülemek için aşağıdaki adımları izleyebilirsiniz:

1. Kurtarma Hizmetleri kasasını açın.
2. Yedekleme altyapısını seçin.
3. Yedekleme yönetim sunucularını görüntüleyin.

## <a name="next-steps"></a>Sonraki adımlar

Diğer iş yüklerini korumak için Azure Backup Sunucusu kullanma hakkında daha fazla bilgi için aşağıdaki makalelerden birine bakın:

* [SharePoint grubunu yedekleme](./backup-mabs-sharepoint-azure-stack.md)
* [SQL sunucusunu yedekleme](./backup-mabs-sql-azure-stack.md)
