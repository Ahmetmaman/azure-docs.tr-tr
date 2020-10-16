---
title: Sistem durumunu bir Windows sunucusuna geri yükleme
description: Windows Server sistem durumunu Azure 'daki bir yedekten geri yüklemek için adım adım açıklama.
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: 824ed5e71934af370211bfa8b4c266fe4323b4ad
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89377446"
---
# <a name="restore-system-state-to-windows-server"></a>Sistem durumunu Windows Server 'a geri yükleme

Bu makalede, Windows Server sistem durumu yedeklemelerinin bir Azure kurtarma hizmetleri kasasından nasıl geri yükleneceği açıklanmaktadır. Sistem durumunu geri yüklemek için, sistem durumu yedeğine sahip olmanız gerekir ( [sistem](backup-azure-system-state.md#back-up-windows-server-system-state)durumu yedekleme bölümündeki yönergeleri kullanarak oluşturulur ve [Microsoft Azure kurtarma hizmetleri (mars) aracısının en son sürümünü](https://aka.ms/azurebackup_agent)yüklediğinizden emin olun. Windows Server sistem durumu verilerini bir Azure kurtarma hizmetleri kasasından kurtarmak iki adımlı bir işlemdir:

1. Sistem durumunu Azure Backup dosya olarak geri yükleyin. Sistem durumunu Azure Backup dosya olarak geri yüklerken şunlardan birini yapabilirsiniz:
   * Sistem durumunu yedeklerin alındığı sunucuya geri yükleyin veya
   * Sistem durumu dosyasını alternatif bir sunucuya geri yükleyin.

2. Geri yüklenen sistem durumu dosyalarını, Windows Server Yedekleme yardımcı programını kullanarak bir Windows Server 'a uygulayın.

## <a name="recover-system-state-files-to-the-same-server"></a>Sistem Durumu dosyalarını aynı sunucuda kurtarma

Aşağıdaki adımlarda, Windows Server yapılandırmanızın önceki bir duruma nasıl geri alınacağı açıklanmaktadır. Sunucu yapılandırmanızın bilinen, kararlı bir duruma geri alınması son derece değerlidir. Aşağıdaki adımlar, bir kurtarma hizmetleri kasasından sunucunun sistem durumunu geri yükler.

1. **Microsoft Azure Backup** ek bileşenini açın. Ek bileşenin nerede yüklü olduğunu bilmiyorsanız, bilgisayarı veya sunucuyu **Microsoft Azure Backup**arayın.

    Masaüstü uygulaması, arama sonuçlarında görünmelidir.

2. Sihirbazı başlatmak için **verileri kurtar** ' ı seçin.

    ![Verileri kurtar](./media/backup-azure-restore-windows-server/recover.png)

3. **Başlarken** bölmesinde, verileri aynı sunucuya veya bilgisayara geri yüklemek Için, **Bu sunucuyu ( `<server name>` )** seçin ve **İleri**' yi seçin.

    ![Verileri aynı makineye geri yüklemek için bu sunucu seçeneğini belirleyin](./media/backup-azure-restore-system-state/samemachine.png)

4. **Kurtarma modunu seçin** bölmesinde **sistem durumu** ' nu seçin ve ardından **İleri**' yi seçin.

    ![Dosyalara gözatamıyorum](./media/backup-azure-restore-system-state/recover-type-selection.png)

5. **Birim ve tarih seçin** bölmesinde takvimde, bir kurtarma noktası seçin.

    Herhangi bir kurtarma noktasından dilediğiniz zaman geri yükleyebilirsiniz. **Kalın yazı** tarihi, en az bir kurtarma noktasının kullanılabilirliğini gösterir. Bir tarih seçtiğinizde, birden fazla kurtarma noktası varsa, **zaman** açılan menüsünde belirli bir kurtarma noktasını seçin.

    ![Birim ve Tarih](./media/backup-azure-restore-system-state/select-date.png)

6. Geri yüklenecek kurtarma noktasını seçtikten sonra **İleri**' yi seçin.

    Azure Backup, yerel kurtarma noktasını takar ve bunu bir kurtarma birimi olarak kullanır.

7. Sonraki bölmede, kurtarılan sistem durumu dosyalarının hedefini belirtin. Ardından, Windows Gezgini 'ni açmak ve istediğiniz dosya ve klasörleri bulmak için **Araştır** ' ı seçin. **Her iki sürüme de sahip olmak için kopya oluştur**seçeneği, tüm sistem durumu arşivi kopyasını oluşturmak yerine, mevcut bir sistem durumu dosya arşivinde tek tek dosyaların kopyalarını oluşturur.

    ![Kurtarma seçenekleri](./media/backup-azure-restore-system-state/recover-as-files.png)

8. **Onay** bölmesinde Kurtarma ayrıntılarını doğrulayın ve **kurtar**' ı seçin.

   ![Kurtarma eylemini onaylamak için kurtar ' ı seçin](./media/backup-azure-restore-system-state/confirm-recovery.png)

9. Kurtarma hedefindeki *WindowsImageBackup* dizinini sunucunun kritik olmayan bir birimine kopyalayın. Genellikle, Windows işletim sistemi birimi kritik birimdir.

10. Kurtarma başarılı olduktan sonra, sistem durumu kurtarma işlemini gerçekleştirmek için [Windows Server 'da geri yüklenen sistem durumunu Uygula](#apply-restored-system-state-on-a-windows-server)bölümündeki adımları izleyin.

## <a name="recover-system-state-files-to-an-alternate-server"></a>Sistem durumu dosyalarını alternatif bir sunucuya kurtarma

Windows Server bozuksa veya erişilemezse ve Windows Server sistem durumunu kurtararak onu kararlı bir duruma geri yüklemek istiyorsanız, bozuk sunucunun sistem durumunu başka bir sunucudan geri yükleyebilirsiniz. Sistem durumunu ayrı bir sunucuda geri yüklemek için aşağıdaki adımları kullanın.  

Bu adımlarda kullanılan terminoloji şunları içerir:

* *Kaynak makine* : yedeklemenin alındığı ve şu anda kullanılamayan özgün makine.
* *Hedef makine* : verilerin kurtarıldığı makine.
* *Örnek kasa* : *kaynak makinenin* ve *hedef makinenin* kaydedildiği kurtarma hizmetleri Kasası.

> [!NOTE]
> Bir makineden alınan yedeklemeler, işletim sisteminin önceki bir sürümünü çalıştıran bir makineye geri yüklenemez. Örneğin, bir Windows Server 2016 makinesinden alınan yedeklemeler Windows Server 2012 R2 'ye geri yüklenemez. Ancak ters bir değer mümkündür. Windows Server 2016 ' ü geri yüklemek için Windows Server 2012 R2 'deki yedeklemeleri kullanabilirsiniz.
>

1. *Hedef makinede* **Microsoft Azure Backup** ek bileşenini açın.
2. *Hedef makinenin* ve *kaynak makinenin* aynı kurtarma hizmetleri kasasında kayıtlı olduğundan emin olun.
3. İş akışını başlatmak için **verileri kurtar** ' ı seçin.
4. **Başka bir sunucu** seçin

    ![Başka bir sunucu](./media/backup-azure-restore-system-state/anotherserver.png)

5. *Örnek kasaya*karşılık gelen kasa kimlik bilgileri dosyasını sağlayın. Kasa kimlik bilgileri dosyası geçersiz (veya dolmuşsa), Azure portal *örnek kasasından* yeni bir kasa kimlik bilgileri dosyası indirin. Kasa kimlik bilgileri dosyası sağlandığında, kasa kimlik bilgileri dosyasıyla ilişkili kurtarma hizmetleri Kasası görüntülenir.

6. Yedekleme sunucusunu seçin bölmesinde, görünen makineler listesinden *kaynak makineyi* seçin.
7. Kurtarma modunu seçin bölmesinde **sistem durumu** ' nu seçin ve **İleri**' yi seçin.

    ![Search](./media/backup-azure-restore-system-state/recover-type-selection.png)

8. **Birim ve tarih seçin** bölmesinde takvimde, bir kurtarma noktası seçin. Herhangi bir kurtarma noktasından dilediğiniz zaman geri yükleyebilirsiniz. **Kalın yazı** tarihi, en az bir kurtarma noktasının kullanılabilirliğini gösterir. Bir tarih seçtiğinizde, birden fazla kurtarma noktası varsa, **zaman** açılan menüsünde belirli bir kurtarma noktasını seçin.

    ![Öğe ara](./media/backup-azure-restore-system-state/select-date.png)

9. Geri yüklenecek kurtarma noktasını seçtikten sonra **İleri**' yi seçin.

10. **Sistem durumu kurtarma modunu seçin** bölmesinde sistem durumu dosyalarının kurtarılmasını istediğiniz hedefi belirtin, ardından **İleri**' yi seçin.

    ![Şifreleme](./media/backup-azure-restore-system-state/recover-as-files.png)

    **Her iki sürüme de sahip olmak için kopya oluştur**seçeneği, tüm sistem durumu arşivi kopyasını oluşturmak yerine, mevcut bir sistem durumu dosya arşivinde tek tek dosyaların kopyalarını oluşturur.

11. Onay bölmesinde Kurtarma ayrıntılarını doğrulayın ve **kurtar**' ı seçin.

    ![Kurtarma işlemini onaylamak için kurtar düğmesini seçin](./media/backup-azure-restore-system-state/confirm-recovery.png)

12. *WindowsImageBackup* dizinini sunucunun kritik olmayan bir birimine kopyalayın (örneğin, D: \) . Genellikle, Windows işletim sistemi birimi kritik birimdir.

13. Kurtarma işlemini gerçekleştirmek için, [geri yüklenen sistem durumu dosyalarını bir Windows Server 'a uygulamak](#apply-restored-system-state-on-a-windows-server)için aşağıdaki bölümü kullanın.

## <a name="apply-restored-system-state-on-a-windows-server"></a>Windows Server’da geri yüklenen Sistem Durumunu uygulama

Azure kurtarma hizmetleri Aracısı 'nı kullanarak sistem durumunu dosyalar olarak kurtardıktan sonra, kurtarılan sistem durumunu Windows Server 'a uygulamak için Windows Server Yedekleme yardımcı programını kullanın. Windows Server Yedekleme yardımcı programı sunucuda zaten var. Aşağıdaki adımlarda kurtarılan sistem durumunun nasıl uygulanacağı açıklanmaktadır.

1. Windows Server Yedekleme ek bileşenini açın. Ek bileşenin nerede yüklü olduğunu bilmiyorsanız, bilgisayarı veya sunucuyu **Windows Server yedekleme**arayın.

    Masaüstü uygulaması arama sonuçlarında görünür. Bu görünmezse veya uygulamayı açtığınızda hata yaşarsanız, **Windows Server yedekleme özellikleri**ve bu bileşenin altındaki bağımlı bileşenleri, **Sunucu Yöneticisi** **Özellik Ekleme Sihirbazı** ' nda bulunan ' yi yüklemelisiniz.

1. Ek bileşende **yerel yedekleme**' yi seçin.

    ![geri yüklemek için yerel yedekleme 'yi seçin](./media/backup-azure-restore-system-state/win-server-backup-local-backup.png)

1. Yerel yedekleme konsolunda, **Eylemler bölmesinde**, Kurtarma Sihirbazı 'nı açmak için **kurtar** ' ı seçin.

1. **Başka bir konumda depolanan bir yedek**seçeneğini belirleyin ve **İleri**' yi seçin.

   ![farklı bir sunucuya kurtarmayı seçin](./media/backup-azure-restore-system-state/backup-stored-in-diff-location.png)

1. Konum türünü belirtirken, sistem durumu yedeğiniz başka bir sunucuya kurtarılıyorsa, **uzak paylaşılan klasör** ' i seçin. Sistem durumlarınızın yerel olarak kurtarılması durumunda **yerel sürücüler**' i seçin.

    ![Yerel sunucu veya başka bir sunucudan kurtarma yapılıp yapılmayacağını seçin](./media/backup-azure-restore-system-state/ss-recovery-remote-shared-folder.png)

1. *WindowsImageBackup* dizininin yolunu girin veya bu dizini içeren yerel sürücüyü (örneğin, d:\windowsımagebackup) seçin, Azure kurtarma hizmetleri Aracısı kullanılarak sistem durumu dosyalarının kurtarmasının bir parçası olarak kurtarıldı ve **İleri**' yi seçin.

    ![paylaşılan dosyanın yolu](./media/backup-azure-restore-system-state/ss-recovery-remote-folder.png)

1. Geri yüklemek istediğiniz sistem durumu sürümünü seçin ve **İleri**' yi seçin.

1. Kurtarma türünü seçin bölmesinde **sistem durumu** ' nu seçin ve **İleri**' yi seçin.

1. Sistem durumu kurtarma konumu için **özgün konum**' u seçin ve **İleri**' yi seçin.

    Bir etki alanı denetleyicisini geri yüklüyorsanız, aşağıdaki ek seçeneği görürsünüz:

    ![Sistem durumu kurtarma konumu](./media/backup-azure-restore-system-state/location-for-system-state-recovery.png)

    >[!NOTE]
    >Yalnızca tüm Active Directory verileri Yetkilendirmeli geri yükleme Işlemini açık bir şekilde düşünüyorsanız, yalnızca "Active Directory dosyalarının yetkili geri yüklemesini gerçekleştir" seçeneğini belirleyin.

1. Onay ayrıntılarını gözden geçirin, yeniden başlatma ayarlarını doğrulayın, geri yüklenen sistem durumu dosyalarını uygulamak için **kurtar** ' ı seçin.

    ![geri yükleme sistem durumu dosyalarını Başlat](./media/backup-azure-restore-system-state/launch-ss-recovery.png)

    >[!NOTE]
    >Geri yüklemeyi DSRM modunda gerçekleştirmezseniz **sunucu seçeneğini otomatik olarak yeniden Başlat** ' ı seçmeyin.

1. Bir geri yüklemeyi başarıyla tamamladıktan sonra, sunucuyu normal modda yeniden başlatmanız gerekir. Bir komut istemi açın ve şunu yazın: `bcdedit /deletevalue safeboot`
1. Sunucuyu yeniden başlatın.

## <a name="special-considerations-for-system-state-recovery-on-a-domain-controller"></a>Bir etki alanı denetleyicisinde sistem durumu kurtarmaya yönelik özel konular

Sistem durumu yedeklemesi Active Directory verileri içerir. Active Directory Etki Alanı hizmetini (AD DS) geçerli durumundan önceki bir duruma geri yüklemek için aşağıdaki adımları kullanın. Bu tür geri yükleme iki senaryoda yapılabilir:

* Ormanda çalışan bir etki alanı denetleyicisi olmadığında tüm Active Directory verilerini geri yükleme
* Bu nesneler silindiğinde veya bozulduğunda Active Directory verilerinin bir bölümünü geri yükleme

Bu makale yalnızca, AD DS yetkisiz geri yükleme ve SYSVOL klasörünün yetkili geri yükleme için çağıran ilk senaryoyu ele alacak.  İkinci senaryoyu (etki alanı denetleyicilerinin hala çalışır durumda olduğunu ancak belirli AD nesnelerini geri yüklemeniz gerekiyorsa) gerçekleştirmeniz gerekiyorsa, [Bu yönergelere](https://support.microsoft.com/help/840001/how-to-restore-deleted-user-accounts-and-their-group-memberships-in-ac)bakın.

1. [Sistem durumu dosyalarını alternatif bir sunucuya kurtarmak](#recover-system-state-files-to-an-alternate-server)için buradaki adımları izleyin.
1. Sunucunuzu *Dizin Hizmetleri onarım modunda*yeniden başlatmak için aşağıdaki komutları kullanın. Yükseltilmiş bir komut isteminde:

    ```cmd
    Bcdedit /set safeboot dsrepair
    Shutdown /r /t 0
    ```

1. Sistem durumu geri yükleme 'nin bir parçası olarak Active Directory kurtarmak için, iki yöntemden birini seçebilirsiniz:

    * Windows Server Yedekleme yardımcı programıyla [Windows Server 'da geri yüklenen sistem durumunu uygulamak](#apply-restored-system-state-on-a-windows-server) için yukarıdaki yönergeleri izleyin.

        >[!NOTE]
        >Tüm Active Directory verilerini geri yüklüyorsanız (ve ormanda kalan çalışan etki alanı denetleyicileri yoksa), yukarıdaki 9. adımda **Active Directory dosyalarının yetkili geri yükleme Işlemini gerçekleştirmeyi**seçtiğinizden emin olun.

    * Komut satırından geri yüklemeyi gerçekleştirmek için [Wbadmin](/windows-server/administration/windows-commands/wbadmin-start-systemstaterecovery) yardımcı programını kullanın.

        Kullanmak istediğiniz yedeğin sürüm tanımlayıcısına ihtiyacınız vardır. Şu komutu çalıştırarak sürüm tanımlayıcılarının listesini alabilirsiniz:

        ```cmd
        wbadmin get versions -backuptarget <servername\sharename>
        ```

        Daha sonra bu sürüm tanımlayıcısını geri yüklemeyi çalıştırmak için kullanacaksınız.

        Örneğin, AD DS yetkisiz geri yükleme ve için paylaşılan kaynakta depolanan 9:00 ' de 04/30/2020 ' dan yedeklemeyi kullanarak [SYSVOL klasörünün yetkilendirmeli geri yüklemesini](/windows-server/identity/ad-ds/manage/ad-forest-recovery-nonauthoritative-restore) gerçekleştirmek için `\\servername\share` `server01` şunu yazın:

        ```cmd
        wbadmin start systemstaterecovery -version:04/30/2020-09:00 -backupTarget:\\servername\share -machine:server01 -authsysvol
        ```

1. Bir geri yüklemeyi başarıyla tamamladıktan sonra, sunucuyu normal modda yeniden başlatmanız gerekir. Bir komut istemi açın ve şunu yazın: `bcdedit /deletevalue safeboot`
1. Sunucuyu yeniden başlatın.

## <a name="troubleshoot-failed-system-state-restore"></a>Başarısız Sistem Durumu geri yükleme sorunlarını giderme

Sistem durumunu uygulamaya yönelik önceki işlem başarıyla tamamlanmazsa Windows Server 'nizi kurtarmak için Windows kurtarma ortamı 'nı (Win RE) kullanın. Aşağıdaki adımlarda Win RE kullanılarak nasıl kurtarılacağı açıklanmaktadır. Bu seçeneği yalnızca Windows Server, sistem durumu geri yüklemesinden sonra normal olarak önyüklenemediğinde kullanın. Aşağıdaki işlem sistem dışı verileri siler, dikkatli olun.

1. Windows Server 'ı Windows kurtarma ortamı 'nda (Win RE) önyükleyin.

2. Kullanılabilir üç seçenekten sorun giderme ' yi seçin.

    ![Sorun giderme seçin](./media/backup-azure-restore-system-state/winre-1.png)

3. **Gelişmiş Seçenekler** ekranından **komut istemi** ' ni seçin ve Sunucu Yöneticisi Kullanıcı adını ve parolasını belirtin.

   ![Komut Istemi seçin](./media/backup-azure-restore-system-state/winre-2.png)

4. Sunucu Yöneticisi Kullanıcı adını ve parolasını belirtin.

    ![Parolayı girin](./media/backup-azure-restore-system-state/winre-3.png)

5. Komut istemi 'ni yönetici modunda açtığınızda, sistem durumu yedekleme sürümlerini almak için aşağıdaki komutu çalıştırın.

    ```cmd
    Wbadmin get versions -backuptarget:<Volume where WindowsImageBackup folder is copied>:
    ```

    ![Sistem durumu yedekleme sürümlerini al](./media/backup-azure-restore-system-state/winre-4.png)

6. Yedeklemede kullanılabilir tüm birimleri almak için aşağıdaki komutu çalıştırın.

    ```cmd
    Wbadmin get items -version:<copy version from above step> -backuptarget:<Backup volume>
    ```

    ![Kullanılabilir tüm birimleri al](./media/backup-azure-restore-system-state/winre-5.png)

7. Aşağıdaki komut, sistem durumu yedeklemesinin parçası olan tüm birimleri kurtarır. Bu adımın yalnızca sistem durumunun parçası olan kritik birimleri kurtardığını unutmayın. Tüm sistem dışı veriler silinir.

    ```cmd
    Wbadmin start recovery -items:C: -itemtype:Volume -version:<Backupversion> -backuptarget:<backup target volume>
    ```

     ![Tüm birimleri kurtar](./media/backup-azure-restore-system-state/winre-6.png)

## <a name="next-steps"></a>Sonraki adımlar

* Artık dosyalarınızı ve klasörlerinizi kurtardığınıza göre, [yedeklemelerinizi yönetebilirsiniz](backup-azure-manage-windows-server.md).
