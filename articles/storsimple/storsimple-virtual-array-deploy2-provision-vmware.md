---
title: VMware 'de StorSimple Sanal dizisi sağlama
description: StorSimple Sanal dizi dağıtım serisinde bu ikinci öğreticide, VMware 'de bir sanal cihazın sağlanması gerekir.
author: alkohli
ms.assetid: 0425b2a9-d36f-433d-8131-ee0cacef95f8
ms.service: storsimple
ms.topic: conceptual
ms.date: 07/25/2019
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9810a34021aa039354aad24f84aff373229c0190
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "87021486"
---
# <a name="deploy-storsimple-virtual-array---provision-in-vmware"></a>StorSimple Sanal dizisi dağıtma-VMware 'de sağlama
![Bir sanal diziyi dağıtmak için gereken adımları gösteren diyagram. İkinci adımın ikinci bölümünde, VMware üzerinde sağlama etiketlidir ve vurgulanacaktır.](./media/storsimple-virtual-array-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a>Genel Bakış

[!INCLUDE [storsimple-virtual-array-eol-banner](../../includes/storsimple-virtual-array-eol-banner.md)]

Bu öğreticide, VMware ESXi 5,0, 5,5, 6,0 veya 6,5 çalıştıran bir konak sisteminde StorSimple Sanal dizisinin nasıl sağlanacağı ve bağlanacağı açıklanmaktadır. Bu makale, Azure portal ve Microsoft Azure Kamu bulutta StorSimple Sanal dizilerinin dağıtımına yöneliktir.

Sanal cihaz sağlamak ve bağlantı kurmak için yönetici ayrıcalıklarına sahip olmanız gerekir. Sağlama ve ilk kurulum adımlarını tamamlamak yaklaşık 10 dakika sürecektir.

## <a name="provisioning-prerequisites"></a>Sağlama önkoşulları
VMware ESXi 5,0, 5,5, 6,0 veya 6,5 çalıştıran bir konak sisteminde sanal cihaz sağlama önkoşulları aşağıdaki gibidir.

### <a name="for-the-storsimple-device-manager-service"></a>StorSimple Cihaz Yöneticisi hizmeti için
Başlamadan önce aşağıdakilerden emin olun:

* [Portal 'ı StorSimple Sanal dizisi Için hazırlama](storsimple-virtual-array-deploy1-portal-prep.md)bölümündeki tüm adımları tamamladınız.
* VMware için sanal cihaz görüntüsünü Azure portal indirdiniz. Daha fazla bilgi için bkz. 3. Adım: [Portal 'ı StorSimple Sanal dizisi Için hazırlama adlı](storsimple-virtual-array-deploy1-portal-prep.md) **sanal cihaz görüntüsünü indirme** .

### <a name="for-the-storsimple-virtual-device"></a>StorSimple Sanal cihazı için
Sanal cihazı dağıtmadan önce şunlardan emin olun:

* Bir cihaz sağlamak için kullanılabilecek Hyper-V (2008 R2 veya üzeri) çalıştıran bir konak sistemine erişiminiz var.
* Ana bilgisayar sistemi sanal cihazınızı sağlamak için aşağıdaki kaynakları ayırabiliyor:

  * En az 4 çekirdek.
  * En az 8 GB RAM. Sanal diziyi dosya sunucusu olarak yapılandırmayı planlıyorsanız, 8 GB 'dan daha az 2.000.000 dosya desteklenir. 2-4 milyon dosyayı desteklemek için 16 GB RAM gerekir.
  * Bir ağ arabirimi.
  * Sistem verileri için 500 GB sanal disk.

### <a name="for-the-network-in-datacenter"></a>Veri merkezindeki ağ için
Başlamadan önce aşağıdakilerden emin olun:

* Bir StorSimple Sanal cihazı dağıtmak ve veri merkezi ağını gereksinimlere göre yapılandırmak için ağ gereksinimlerini gözden geçirdiniz. 

## <a name="step-by-step-provisioning"></a>Adım adım sağlama
Bir sanal cihaza sağlamak ve bağlanmak için aşağıdaki adımları gerçekleştirmeniz gerekir:

1. Konak sisteminin en düşük sanal cihaz gereksinimlerini karşılamak için yeterli kaynaklara sahip olduğundan emin olun.
2. Hiper yöneticide bir sanal cihaz sağlayın.
3. Sanal cihazı başlatın ve IP adresini alın.

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a>1. Adım: konak sisteminin en düşük sanal cihaz gereksinimlerini karşıladığından emin olun
Sanal bir cihaz oluşturmak için şunlar gerekir:

* VMware ESXi Server 5,0, 5,5, 6,0 veya 6,5 çalıştıran bir konak sistemine erişim.
* ESXi ana bilgisayarını yönetmek için sisteminizde VMware vSphere istemcisi yüklü.

  * En az 4 çekirdek.
  * En az 8 GB RAM. Sanal diziyi dosya sunucusu olarak yapılandırmayı planlıyorsanız, 8 GB 'dan daha az 2.000.000 dosya desteklenir. 2-4 milyon dosyayı desteklemek için 16 GB RAM gerekir.
  * İnternet trafiği için ağa bağlı bir ağ arabirimi. En düşük Internet bant genişliği, cihazın en iyi şekilde çalışmasını sağlamak için 5 MB/sn olmalıdır.
  * Veriler için 500 GB sanal disk.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>2. Adım: Hiper yöneticide sanal cihaz sağlama
Hiper yöneticinizde sanal cihaz sağlamak için aşağıdaki adımları gerçekleştirin.

1. Sanal cihaz görüntüsünü sisteminize kopyalayın. Bu sanal görüntüyü Azure portal aracılığıyla indirdiniz.

   1. En son görüntü dosyasını indirdiğinizden emin olun. Görüntüyü daha önce indirdiyseniz, en son görüntüye sahip olduğunuzdan emin olmak için yeniden indirin. En son görüntüde iki dosya vardır (bir tane yerine).
   2. Bu görüntüyü yordamın ilerleyen bölümlerinde kullanacağınız için kopyaladığınız konumu not edin.

2. vSphere istemcisini kullanarak ESXi sunucusunda oturum açın. Sanal makine oluşturmak için yönetici ayrıcalıklarınızın olması gerekir.

   ![VSphere istemci oturum açma sayfasının ekran görüntüsü. IP adresi, Kullanıcı adı ve parola kutuları değerler içerir ve oturum açma düğmesi vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-vmware/image1.png)
3. VSphere istemcisinde sol bölmedeki envanter bölümünde ESXi sunucusu ' nu seçin.

   ![VSphere istemci ana sayfasının ekran görüntüsü. Envanter bölümünde ESXi sunucusu vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-vmware/image2.png)
4. VMDK dosyasını ESXi sunucusuna yükleyin. Sağ bölmedeki **yapılandırma** sekmesine gidin. **Donanım** altında **depolama**' yı seçin.

   ![VSphere istemcisinin yapılandırma sekmesini gösteren ekran görüntüsü. Donanım bölümünde, depolama vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-vmware/image3.png)
5. Sağ taraftaki bölmede **Datastores** (Veri depoları) öğesini seçerek VMDK dosyasını yüklemek istediğiniz yeri belirleyin. Veri deposu, işletim sistemi ve veri diskleri için yeterli boş alana sahip olmalıdır.

   ![VSphere istemcisinin depolama sayfasını gösteren ekran görüntüsü. Veri depoları sekmesi açıktır ve veri depolarının bir listesini içerir. Bir veri deposu seçilidir.](./media/storsimple-virtual-array-deploy2-provision-vmware/image4.png)
6. Sağ tıklayıp **Browse Datastore** (Veri Deposuna Göz At) öğesini seçin.

   ![Seçili veri deposunun kısayol menüsünü gösteren ekran görüntüsü. Veri deposu öğesine Gözatadır öğesi seçilidir.](./media/storsimple-virtual-array-deploy2-provision-vmware/image5.png)
7. **Datastore Browser** (Veri Deposu Tarayıcısı) penceresi açılır.

   ![Veri deposu tarayıcısının ekran görüntüsü. Veri deposundaki klasörler görünür.](./media/storsimple-virtual-array-deploy2-provision-vmware/image6.png)
8. Araç çubuğunda simge ' ye tıklayarak :::image type="icon" source="./media/storsimple-virtual-array-deploy2-provision-vmware/image7.png"::: Yeni bir klasör oluşturun. Klasör adını belirtin ve not edin. Sanal makine oluştururken bu klasör adını kullanacaksınız (önerilen yöntemdir). **Tamam**'a tıklayın.

   ![Yeni klasör simgesiyle vurgulanmış bir veri deposu tarayıcısının ekran görüntüsü. Bir iletişim kutusunda doldurulmuş bir klasör adı ve Tamam düğmesi vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-vmware/image8.png)
9. Yeni klasör **Datastore Browser** (Veri Deposu Tarayıcısı) penceresinin sol tarafında görünür.

   ![Klasör hiyerarşisinde görünür yeni klasörü içeren bir veri deposu tarayıcısının ekran görüntüsü.](./media/storsimple-virtual-array-deploy2-provision-vmware/image9.png)
10. Karşıya yükle simgesine tıklayın :::image type="icon" source="./media/storsimple-virtual-array-deploy2-provision-vmware/image10.png"::: ve **dosyayı karşıya yükle**' yi seçin.

    ![Karşıya yükleme simgesinin kısayol menüsünü gösteren ekran görüntüsü. Dosya yükleme öğesi seçilidir.](./media/storsimple-virtual-array-deploy2-provision-vmware/image11.png)
11. İndirdiğiniz VMDK dosyalarını bulun. İki dosya vardır. Karşıya yüklemek için dosyalardan birini seçin.

    ![Klasörleri ve iki V M D K dosyasını gösteren bir iletişim kutusunun ekran görüntüsü. Dosyalardan biri vurgulanmıştır.](./media/storsimple-virtual-array-deploy2-provision-vmware/image12m.png)
12. **Aç**’a tıklayın. VMDK dosyası belirtilen veri deposuna yüklenmeye başlar. Dosyanın karşıya yüklenmesi birkaç dakika sürebilir.
13. Karşıya yükleme işlemi tamamlandıktan sonra dosyayı oluşturduğunuz veri deposunda görebilirsiniz.

    ![Veri deposu tarayıcısının ekran görüntüsü. Yeni klasör, klasör hiyerarşisinde vurgulanır ve karşıya yüklenen dosya bu klasörde görünür.](./media/storsimple-virtual-array-deploy2-provision-vmware/image14.png)

    Şimdi ikinci VMDK dosyasını da aynı ver deposuna yükleyin.
14. vSphere istemcisi penceresine dönün. ESXi sunucusu seçiliyken, sağ tıklayıp **Yeni sanal makine**' yi seçin.

    ![ESXi sunucusunun kısayol menüsünün ekran görüntüsü. Yeni sanal makine öğesi seçilidir.](./media/storsimple-virtual-array-deploy2-provision-vmware/image15.png)
15. **Yeni bir sanal makine oluştur** penceresi görüntülenir. **Yapılandırma** sayfasında **özel** seçeneğini belirleyin. **İleri**’ye tıklayın.
    ![Yeni sanal makine oluştur penceresinin yapılandırma sayfasının ekran görüntüsü. Özel seçenek seçilidir ve Ileri düğmesi vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-vmware/image16.png)
16. **Ad ve konum** sayfasında, sanal makinenizin adını belirtin. Bu ad, adım 8 ' de daha önce belirttiğiniz klasör adıyla (önerilen en iyi yöntem) eşleşmelidir.

    ![Yeni sanal makine oluştur penceresinin ad ve konum sayfasının ekran görüntüsü. Ad kutusu doldurulur ve sonraki düğme vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-vmware/image17.png)
17. **Depolama** SAYFASıNDA, VM 'nizi sağlamak için kullanmak istediğiniz bir veri deposunu seçin.

    ![Yeni sanal makine oluştur penceresinin depolama sayfasının ekran görüntüsü. Bir veri deposu seçilir ve bir sonraki düğme vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-vmware/image18.png)
18. **Sanal makine sürümü** sayfasında, **sanal makine sürümü: 8**' i seçin.

    ![Sanal makine sürümü sayfasının ekran görüntüsü. Sanal makine sürüm 8 seçeneği seçilidir ve Ileri düğmesi vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-vmware/image19.png)
19. **Konuk Işletim sistemi** sayfasında, **Windows** olarak **Konuk işletim sistemini** seçin. **Sürüm** için, açılan listeden **Microsoft Windows Server 2012 (64-bit)** öğesini seçin.

    ![Windows seçiliyken Konuk Işletim sistemi sayfasının ekran görüntüsü, Microsoft Windows Server 2012 (64-bit) olarak ayarlanan sürüm ve sonraki vurgulanmış.](./media/storsimple-virtual-array-deploy2-provision-vmware/image20.png)
20. **CPU 'lar** sayfasında, **Toplam çekirdek sayısı** 4 (veya daha fazla) olacak şekilde sanal yuva başına **sanal yuva sayısını** ve **çekirdek sayısını** ayarlayın. **İleri**’ye tıklayın.

    ![Bir sanal yuvayı, sanal yuva başına dört çekirdek ve toplam dört çekirdek gösteren CPU 'Lar sayfasının ekran görüntüsü. Ileri düğmesi vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-vmware/image21.png)
21. **Bellek** sayfasında, 8 GB (veya daha fazla) RAM belirtin. **İleri**’ye tıklayın.

    ![Bellek sayfasının ekran görüntüsü. Bellek boyutu için 8 GB bir değer doldurulur.](./media/storsimple-virtual-array-deploy2-provision-vmware/image22.png)
22. **Ağ** sayfasında, ağ arabirimlerinin sayısını belirtin. En düşük gereksinim bir ağ arabirimidir.

    ![Ağ sayfasının ekran görüntüsü. Ağ arabirimlerinin sayısı bir olarak ayarlanır ve bir sonraki düğme vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-vmware/image23.png)
23. **SCSI denetleyicisi** sayfasında, varsayılan **LSI Logic SAS denetleyicisini** kabul edin.

    ![SCSI denetleyicisi sayfasının ekran görüntüsü. L S I Logic s seçeneği seçilidir ve sonraki düğme vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-vmware/image24.png)
24. **Disk seçin** sayfasında, **var olan bir sanal diski kullan**' ı seçin. **İleri**’ye tıklayın.

    ![Bir disk seçin sayfasının, var olan sanal diski kullan seçeneğinin seçili ve Ileri düğmesinin vurgulandığı ekran görüntüsü.](./media/storsimple-virtual-array-deploy2-provision-vmware/image25.png)
25. **Mevcut diski seçin** sayfasında, **disk dosyası yolu**' nun altında, **Araştır**' a tıklayın. Bu, bir **veri depoları araştır** iletişim kutusu açar. VMDK 'yi karşıya yüklediğiniz konuma gidin. Başlangıçta karşıya yüklediğiniz iki dosya birleştirildiğinden artık veri deposunda yalnızca bir dosya görürsünüz. Dosyayı seçip **Tamam**' a tıklayın. **İleri**’ye tıklayın.

    ![Mevcut disk Seç sayfasının ekran görüntüsü. Araştır düğmesi vurgulanır ve bir iletişim kutusu bir dosya ve vurgulanmış bir Tamam düğmesi içerir.](./media/storsimple-virtual-array-deploy2-provision-vmware/image26.png)
26. **Gelişmiş Seçenekler** sayfasında, Varsayılanı kabul edin ve **İleri**' ye tıklayın.

    ![Gelişmiş Seçenekler sayfasının ekran görüntüsü. Ileri düğmesi vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-vmware/image27.png)
27. **Ready to Complete** (Tamamlanmak İçin Hazır) sayfasında yeni sanal makineyle ilgili tüm ayarları gözden geçirin. **Tamamlanmadan önce sanal makine ayarlarını düzenleyin**' i işaretleyin. **Devam**’a tıklayın.

    ![Vurgulanmış devam düğmesi olan tamamlamaya hazırlanma sayfasının ekran görüntüsü. Tamamlanmadan önce sanal makine ayarlarını Düzenle seçeneği işaretlendi.](./media/storsimple-virtual-array-deploy2-provision-vmware/image28.png)
28. **Sanal makineler Özellikler** sayfasında, **donanım** sekmesinde, cihaz donanımını bulun. **Yeni sabit disk**' i seçin. **Ekle**'ye tıklayın.

    ![Sanal makineler özellikler sayfasının donanım sekmesinin ekran görüntüsü. Donanım listesinde yeni sabit disk seçilidir. Ekle düğmesi vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-vmware/image29.png)
29. **Donanım Ekle** penceresi görürsünüz. **Cihaz türü** sayfasında, **eklemek Istediğiniz cihaz türünü seçin** altında **sabit disk**' i seçin ve **İleri**' ye tıklayın.

    ![Donanım Ekle penceresinin cihaz türü sayfasının ekran görüntüsü. Sabit disk cihazı seçilir ve Ileri düğmesi vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-vmware/image30.png)
30. **Disk seçin** sayfasında **Yeni bir sanal disk oluştur**' u seçin. **İleri**’ye tıklayın.

    ![Disk seçin sayfasının ekran görüntüsü. Yeni sanal disk oluştur seçeneği seçilidir ve Ileri düğmesi vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-vmware/image31.png)
31. **Disk oluştur** sayfasında, **DISK boyutunu** 500 GB (veya daha fazla) olarak değiştirin. 500 GB en düşük gereksinimdir, her zaman daha büyük bir disk sağlayabilirsiniz. Sağlandıktan sonra diski genişletemez veya daraltılamayacağını unutmayın. Sağlanacak disk boyutu hakkında daha fazla bilgi için [en iyi uygulamalar belgesindeki](storsimple-ova-best-practices.md)boyutlandırma bölümünü gözden geçirin. **Disk sağlama** bölümünde **ölçülü kaynak** sağlama ' yı seçin. **İleri**’ye tıklayın.

    ![Disk Oluştur sayfasının ekran görüntüsü. Disk boyutu 500 GB olarak ayarlanır, Ince sağlama seçeneği seçilidir ve sonraki düğme vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-vmware/image32.png)
32. **Gelişmiş Seçenekler** sayfasında, Varsayılanı kabul edin.

    ![Gelişmiş Seçenekler sayfasının ekran görüntüsü. Sanal cihaz düğümü SCSI (0:0) olarak ayarlanır ve sonraki düğme vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-vmware/image33.png)
33. **Tamamlamaya hazırlanıyor** sayfasında, disk seçeneklerini gözden geçirin. **Finish (Son)** düğmesine tıklayın.

    ![Tamamlamaya hazırlanma sayfasının ekran görüntüsü. Disk seçeneklerinin bir özeti görünür ve son düğmesi vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-vmware/image34.png)
34. Sanal makine özellikleri sayfasına dönün. Sanal makinenize yeni bir sabit disk eklenir. **Finish (Son)** düğmesine tıklayın.

    ![Sanal makine özellikleri sayfasının ekran görüntüsü. Donanım listesi yeni sabit diski içerir ve son düğmesi vurgulanır.](./media/storsimple-virtual-array-deploy2-provision-vmware/image35.png)
35. Sağ bölmede sanal makineniz seçiliyken **Özet** sekmesine gidin. Sanal makinenizin ayarlarını gözden geçirin.

    ![VSphere istemci Özeti sekmesinin ekran görüntüsü. Yeni sanal makine vurgulanır ve kaynakları ve genel özellikleri görünür olur.](./media/storsimple-virtual-array-deploy2-provision-vmware/image36.png)

Sanal makineniz sağlanır. Bir sonraki adım bu makineyi açmak ve IP adresini almaktır.

> [!NOTE]
> VMware araçlarını sanal diziniz üzerinde (yukarıda sağlanmak üzere) yüklememeyi öneririz. VMware araçlarının yüklenmesi desteklenmeyen bir yapılandırmaya neden olabilir.

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>3. Adım: sanal cihazı başlatın ve IP 'yi alın
Sanal cihazınızı başlatmak ve bağlantı kurmak için aşağıdaki adımları izleyin.

#### <a name="to-start-the-virtual-device"></a>Sanal cihazı başlatmak için
1. Sanal cihazı başlatın. VSphere Configuration Manager, sol bölmedeki cihazınızı seçin ve bağlam menüsünü açmak için sağ tıklayın. **Power** (Güç) ve ardından **Power on** (Aç) seçimini yapın. Bu işlemin ardından makinenizin açılması gerekir. Durumu vSphere istemcisinin en **son görevler** bölmesinde görüntüleyebilirsiniz.

   ![Cihazın kısayol menüsünün ekran görüntüsü. Güç öğesi seçilidir. Açık öğe seçiliyken, bitişik bir menü görünür.](./media/storsimple-virtual-array-deploy2-provision-vmware/image37.png)
2. Kurulum görevlerinin tamamlanması birkaç dakika sürer. Cihaz çalışmaya başladıktan sonra **konsol** sekmesine gidin. cihazda oturum açmak için Ctrl + Alt + Delete tuşlarını gönderin. Alternatif olarak, imleci konsol penceresinde işaret edebilir ve CTRL + ALT + INSERT tuşlarına basabilirsiniz. Varsayılan Kullanıcı *Storsimpleadmin* ' dir ve varsayılan parola *Parola1*' dir.

   ![VSphere istemci konsolu sekmesinin ekran görüntüsü. Parola kutusu boştur.](./media/storsimple-virtual-array-deploy2-provision-vmware/image38.png)
3. Güvenlik nedeniyle cihazın yönetici parolasının ilk oturum açma işleminin ardından değiştirilmesi gerekir. Parolayı değiştirmeniz istenir.

   ![VSphere istemci konsolu sekmesinin ekran görüntüsü. Sayfadaki metin parolanın değiştirilmesi gerektiğini belirtir.](./media/storsimple-virtual-array-deploy2-provision-vmware/image39.png)
4. En az 8 karakterden oluşan bir parola girin. Parola şu gereksinimlerin 3 ' den 4 ' ü içermelidir: büyük harf, küçük harf, sayısal ve özel karakterler. Onaylamak için parolayı yeniden girin. Parolanın değiştiği bildirilir.

   ![VSphere istemci konsolu sekmesinin ekran görüntüsü. Sayfadaki metin parolanın değiştirildiğini belirtir.](./media/storsimple-virtual-array-deploy2-provision-vmware/image40.png)
5. Parola başarıyla değiştirildikten sonra, sanal cihaz yeniden başlayabilir. Yeniden başlatmanın tamamlanmasını bekleyin. Cihazın Windows PowerShell konsolu bir ilerleme çubuğu ile birlikte görüntülenebilir.

   ![İlerleme çubuğu içeren bir konsol penceresini gösteren ekran görüntüsü. Penceredeki metin ilk kurulumun devam ettiğinden emin olur ve kullanıcıdan beklemesini ister.](./media/storsimple-virtual-array-deploy2-provision-vmware/image41.png)
6. Adım 6-8 yalnızca DHCP bulunmayan bir ortamdaki önyükleme süreci için geçerlidir. DHCP ortamındaysanız bu adımları atlayıp 9. adımla devam edebilirsiniz. Cihazınızı DHCP dışı ortamda önyüklerseniz, aşağıdaki ekranı görürsünüz.

   ![Cihazı tanımlayan metin içeren bir konsol penceresini gösteren ekran görüntüsü. Komut istemi "denetleyiciyi" okur ve giriş için hazırlanın.](./media/storsimple-virtual-array-deploy2-provision-vmware/image42m.png)

   Sonra, ağı yapılandırın.
7. `Get-HcsIpAddress`Sanal cihazınızda etkin olan ağ arabirimlerini listelemek için komutunu kullanın. Cihazınızda tek bir ağ arabirimi varsa `Ethernet` varsayılan adı atanır.

   ![Get-HcsIpAddress komutunun çıkışıyla bir konsol penceresi gösteren ekran görüntüsü. "Ethernet", cihazın adı olarak listelenir.](./media/storsimple-virtual-array-deploy2-provision-vmware/image43m.png)
8. Ağı yapılandırmak için `Set-HcsIpAddress` cmdlet'ini kullanın. Aşağıda bir örnek gösterilmiştir:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![Get-Help Set-HcsIpAddress komutunun çıkışıyla ve Set-HcsIpAddress komutunun doğru kullanımının bir konsol penceresini gösteren ekran görüntüsü.](./media/storsimple-virtual-array-deploy2-provision-vmware/image44.png)
9. İlk kurulum işlemleri tamamlandıktan ve cihaz önyüklendikten sonra cihaz başlık metnini görürsünüz. Cihazı yönetmek için başlık metninde görüntülenen IP adresini ve URL'yi not edin. Bu IP adresini, sanal cihazınızın Web Kullanıcı arabirimine bağlanmak ve yerel kurulumu ve kaydı gerçekleştirmek için kullanacaksınız.

   ![Cihaz başlık metniyle bir konsol penceresi gösteren ekran görüntüsü. Bu metin, cihaz IP adresini ve URL 'YI içerir.](./media/storsimple-virtual-array-deploy2-provision-vmware/image45.png)
10. Seçim Bu adımı yalnızca cihazınızı kamu bulutuna dağıtıyorsanız gerçekleştirin. Artık cihazınızda Federal bilgi Işleme standardı (FIPS) modunu Birleşik Devletler etkinleştirirsiniz. FIPS 140 standardı, önemli verilerin korunması için ABD Federal Kamu bilgisayar sistemleri tarafından kullanılmak üzere onaylanan şifreleme algoritmalarını tanımlar.

    1. FIPS modunu etkinleştirmek için aşağıdaki cmdlet 'i çalıştırın:

        `Enable-HcsFIPSMode`
    2. FIPS modunu etkinleştirdikten sonra şifreleme doğrulamaları geçerli olacak şekilde cihazınızı yeniden başlatın.

       > [!NOTE]
       > Cihazınızda FIPS modunu etkinleştirebilir veya devre dışı bırakabilirsiniz. Cihaz FIPS ve FIPS olmayan mod arasında alternatif olarak desteklenmez.
       >
       >

Cihazınız minimum yapılandırma gereksinimlerini karşılamıyorsa başlık metninde hata iletisi görüntülenir (aşağıda gösterilmiştir). Cihaz yapılandırmasını minimum gereksinimleri karşılayacak şekilde değiştirmeniz gerekir. Ardından cihazı yeniden başlatıp bağlantı kurabilirsiniz. Minimum yapılandırma gereksinimleri için bkz. [1. Adım: Ana bilgisayarın minimum cihaz gereksinimlerini karşıladığından emin olma](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).

![Cihaz başlık metniyle bir konsol penceresi gösteren ekran görüntüsü. Bu metin, sorunu gidermeye yönelik bir URL sağlayan bir hata iletisi içerir.](./media/storsimple-virtual-array-deploy2-provision-vmware/image46.png)

Yerel Web Kullanıcı arabirimini kullanarak ilk yapılandırma sırasında başka bir hata varsa, aşağıdaki iş akışlarına başvurun:

* [Web UI kurulumu sorunlarını gidermek](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors)için tanılama testlerini çalıştırın.
* [Günlük paketi oluşturun ve günlük dosyalarını görüntüleyin](storsimple-ova-web-ui-admin.md#generate-a-log-package).

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple Sanal dizinizi dosya sunucusu olarak ayarlama](storsimple-virtual-array-deploy3-fs-setup.md)
* [StorSimple Sanal dizinizi Iscsı sunucusu olarak ayarlama](storsimple-virtual-array-deploy3-iscsi-setup.md)
