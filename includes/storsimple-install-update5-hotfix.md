---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 19d2dedc2ccf7015696504a94f5ef7c43a90d3be
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188568"
---
#### <a name="to-download-hotfixes"></a>Düzeltmeleri indirmek için

Microsoft Update Kataloğu'ndan yazılım güncelleştirmesi indirmek için aşağıdaki adımları uygulayın.

1. Internet Explorer'ı [http://catalog.update.microsoft.com](https://catalog.update.microsoft.com)başlatın ve ''ye gidin.
2. Microsoft Update Kataloğu’nu bu bilgisayarda ilk kez kullanıyorsanız, sorulduğunda **Yükle**’ye tıklayarak Microsoft Update Kataloğu eklentisini yükleyin.

    ![Katalog yükleme](./media/storsimple-install-update2-hotfix/HCS_InstallCatalog-include.png)

3. Microsoft Update Kataloğu'nun arama kutusuna, örneğin **4037264**gibi indirmek istediğiniz düzeltmenin Bilgi Bankası (KB) numarasını girin ve ardından **Arama'yı**tıklatın.
   
    Düzeltme listesi, örneğin **StorSimple 8000 Serisi için Kümülatif Yazılım Paketi Güncelleştirmesi 5.0**olarak görünür.
   
    ![Katalogda arama](./media/storsimple-install-update5-hotfix/update-catalog-search.png)

4. **İndir'i**tıklatın. İndirilen öğelerin görünmesini istediğiniz yerel konumu belirtin veya **Gözat** seçeneğiyle konumu bulun. Belirtilen konuma ve klasöre indirmek için dosyaları tıklatın. Klasör, cihazdan erişilebilen bir ağ paylaşımına da kopyalanabilir.
5. Yukarıdaki tabloda listelenen ek düzeltmeleri arayın **(4037266),** ve ilgili dosyaları önceki tabloda listelenen belirli klasörlere indirin.

> [!NOTE]
> Eş denetleyiciden gelen olası hata iletilerini algılamak için düzeltmelere her iki denetleyiciden de erişilebilmelidir.
>
> Düzeltmeler 3 ayrı klasöre kopyalanmalıdır. Örneğin, aygıt yazılımı/Cis/MDS aracısı güncelleştirmesi _FirstOrderUpdate_ klasöründe kopyalanabilir, diğer kesintisiz güncelleştirmeler _SecondOrderUpdate_ klasöründe kopyalanabilir ve bakım modu güncelleştirmeleri _ThirdOrderUpdate_ klasöründe kopyalanabilir.

#### <a name="to-install-and-verify-regular-mode-hotfixes"></a>Normal mod düzeltmelerini yüklemek ve doğrulamak için

Normal mod düzeltmelerini yüklemek ve doğrulamak için aşağıdaki adımları gerçekleştirin. Azure portalını kullanarak zaten yüklediyseniz, [bakım modu düzeltmelerini yüklemek ve doğrulamak](#to-install-and-verify-maintenance-mode-hotfixes)için ileri ye geçin.

1. Düzeltmeleri yüklemek için StorSimple cihazı seri konsolunuzdaki Windows PowerShell arabirimine erişin. [Seri konsola bağlanmak için PuTTy kullanma](../articles/storsimple/storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console) bölümündeki ayrıntılı yönergeleri izleyin. Komut isteminde **Enter** tuşuna basın.
2. Seçenek 1'i seçin, **tam erişimle giriş yapın.** Düzeltmeyi ilk olarak edilgen denetleyiciye yüklemeniz önerilir.
3. Düzeltmeyi yüklemek için komut istemine şunu yazın:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    Yukarıdaki komuttaki paylaşım yolunda DNS yerine IP kullanın. Kimlik bilgisi parametresi yalnızca kimliği doğrulanmış bir paylaşımdan erişiyorsanız kullanılır.
   
    Paylaşımlara erişmek için kimlik bilgisi parametresini kullanmanız önerilir. “Herkese” açık paylaşımlar bile genellikle kimliği doğrulanmamış kullanıcılara açık değildir.
   
4. İstendiğinde parolayı belirtin. Birinci sipariş güncelleştirmelerini yüklemeye ilişkin örnek çıktı aşağıda gösterilmiştir. İlk sipariş güncelleştirmesi için belirli bir dosyayı işaret etmeniz gerekir.

    >[!NOTE] 
    > Önce _HcsSoftwareUpdate.exe'yi_ yüklemeniz gerekir. Bu yükleme tamamlandıktan sonra _CisMdsAgentUpdate.exe'yi_yükleyin.
   
        ```
        Controller0>Start-HcsHotfix -Path \\10.100.100.100\share
        \FirstOrderUpdate\HcsSoftwareUpdate.exe -Credential contoso\John
   
        Confirm
   
        This operation starts the hotfix installation and could reboot one or
        both of the controllers. If the device is serving I/Os, these will not
        be disrupted. Are you sure you want to continue?
        [Y] Yes [N] No [?] Help (default is "Y"): Y
   
        ```
5. Düzeltme yüklemesini onaylamak için sorulduğunda **Y** yazın.
6. `Get-HcsUpdateStatus` cmdlet'ini kullanarak güncelleştirmeyi izleyin. Güncelleştirme ilk olarak edilgen denetleyicide tamamlanır. Edilgen denetleyici güncelleştirildikten sonra yük devretme gerçekleştirilir ve bundan sonra güncelleştirme diğer denetleyiciye uygulanır. Her iki denetleyici de güncelleştirildiğinde güncelleştirme tamamlanır.
   
    Devam etmekte olan güncelleştirme aşağıdaki örnek çıktıda gösterilir. Güncelleştirme `RunInprogress` `True` nin devam eden durumudur.

    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : True
    LastHotfixTimestamp :
    LastUpdateTimestamp : 07/28/2017 2:04:02 AM
    Controller0Events   :
    Controller1Events   :
    ```
   
     Aşağıdaki örnek çıktıda güncelleştirmenin tamamlandığı gösterilir. `False` Güncelleştirme `RunInProgress` tamamlandığında.
   
    ```
    Controller0>Get-HcsUpdateStatus
    RunInprogress       : False
    LastHotfixTimestamp : 07/28/2017 9:15:55 AM
    LastUpdateTimestamp : 07/28/2017 9:06:07 AM
    Controller0Events   :
    Controller1Events   :
    ```

    > [!NOTE]
    > Bazı durumlarda cmdlet, güncelleştirme hala devam ediyorsa `False` raporu gönderir. Düzeltmenin tamamlandığından emin olmak için birkaç dakika bekleyin, bu komutu yeniden çalıştırın ve `RunInProgress` değerinin `False` olduğunu doğrulayın. Değer değiştiyse düzeltme tamamlanmıştır.

7. Yazılım güncelleştirmesi tamamlandıktan sonra sistem yazılımı sürümlerini doğrulayın. Şunu yazın:
   
    `Get-HcsSystem`
   
    Aşağıdaki sürümleri görmeniz gerekir:
   
   * `FriendlySoftwareVersion: StorSimple 8000 Series Update 5.0`
   * `HcsSoftwareVersion: 6.3.9600.17845`
   
     Güncelleştirme uygulandıktan sonra sürüm numarası değişmezse, düzeltmenin uygulanamadığı anlamına gelir. Bunu görmeniz durumunda daha fazla yardım için lütfen [Microsoft Desteği](../articles/storsimple/storsimple-8000-contact-microsoft-support.md)’ne başvurun.
     
     > [!IMPORTANT]
     > Bir sonraki güncelleştirmeyi uygulamadan `Restart-HcsController` önce cmdlet üzerinden etkin denetleyiciyi yeniden başlatmanız gerekir.
     
8. _FirstOrderUpdate_ klasörünüze indirilen _CisMDSAgentupdate.exe_ aracısını yüklemek için 3-6 adımlarını yineleyin.
8. İkinci sipariş güncelleştirmelerini yüklemek için 3-6 adımlarını yineleyin. 

    > [!NOTE] 
    > İkinci sipariş güncelleştirmeleri için, yalnızca çalıştırArak `Start-HcsHotfix cmdlet` ve ikinci sipariş güncelleştirmelerinin bulunduğu klasöre işaret ederek birden çok güncelleştirme yüklenebilir. Cmdlet, klasörde bulunan tüm güncelleştirmeleri yürütür. Bir güncelleştirme zaten yüklüyse, güncelleştirme mantığı bunu saptar ve ilgili güncelleştirmeyi uygulamaz.

    Tüm düzeltmeler yüklendikten sonra `Get-HcsSystem` cmdlet'ini kullanın. Sürümler şunlar olmalıdır:
    
    * `CisAgentVersion:  1.0.9724.0`
    * `MdsAgentVersion: 35.2.2.0`
    * `Lsisas2Version: 2.0.78.00`


#### <a name="to-install-and-verify-maintenance-mode-hotfixes"></a>Bakım modu düzeltmelerini yüklemek ve doğrulamak için

Disk firmware güncelleştirmelerini yüklemek için KB4037263'ü kullanın. Bunlar kesintiye uğratan güncelleştirmelerdir ve tamamlanması yaklaşık 30 dakika sürer. Bunları cihaz seri konsoluna bağlanarak planlı bakım penceresinde yüklemeyi seçebilirsiniz.

> [!NOTE] 
> Disk firmware'iniz zaten güncelse, bu güncelleştirmeleri yüklemeniz gerekmez. Güncelleştirmelerin mevcut olup olmadığını ve güncelleştirmelerin kesintiye uğratıp (bakım modu) uğratmayacağını (normal mod) denetlemek için cihaz seri konsolundan `Get-HcsUpdateAvailability` cmdlet’ini çalıştırın.

Disk üretici yazılımı güncelleştirmelerini yüklemek için aşağıdaki yönergeleri izleyin.

1. Cihazı bakım moduna alın. 

    > [!NOTE] 
    > Bakım modunda bir aygıta bağlanırken Windows PowerShell remoting'i kullanmayın. Bunun yerine, cihaz seri konsolu üzerinden bağlanırken bu cmdlet’i cihaz denetleyicisinde çalıştırın.

    Denetleyiciyi bakım moduna yerleştirmek için şunları yazın:
   
    `Enter-HcsMaintenanceMode`
   
    Örnek çıktı aşağıda gösterilmiştir.
   
        Controller0>Enter-HcsMaintenanceMode
        Checking device state...
   
        In maintenance mode, your device will not service IOs and will be disconnected from the Microsoft Azure StorSimple Manager service. Entering maintenance mode will end the current session and reboot both controllers, which takes a few minutes to complete. Are you sure you want to enter maintenance mode?
        [Y] Yes [N] No (Default is "Y"): Y
   
        -----------------------MAINTENANCE MODE------------------------
        Microsoft Azure StorSimple Appliance Model 8600
        Name: Update4-8600-mystorsimple
        Copyright (C) 2014 Microsoft Corporation. All rights reserved.
        You are connected to Controller0 - Passive
        ---------------------------------------------------------------
   
        Serial Console Menu
        [1] Log in with full access
        [2] Log into peer controller with full access
        [3] Connect with limited access
        [4] Change language
        Please enter your choice>
   
    Bu durumda her iki denetleyici de bakım modunda yeniden başlatılır.
2. Disk üretici yazılımı güncelleştirmesini yüklemek için şunu yazın:
   
    `Start-HcsHotfix -Path <path to update file> -Credential <credentials in domain\username format>`
   
    Örnek çıktı aşağıda gösterilmiştir.
   
        Controller1>Start-HcsHotfix -Path \\10.100.100.100\share\ThirdOrderUpdates\ -Credential contoso\john
        Enter Password:
        WARNING: In maintenance mode, hotfixes should be installed on each controller sequentially. After the hotfix is installed on this controller, install it on the peer controller.
        Confirm
        This operation starts a hotfix installation and could reboot one or both of the controllers. By installing new updates you agree to, and accept any additional terms associated with, the new functionality listed in the release notes (https://go.microsoft.com/fwLink/?LinkID=613790). Are you sure you want to continue?
        [Y] Yes [N] No (Default is "Y"): Y
        WARNING: Installation is currently in progress. This operation can take several minutes to complete.
3. `Get-HcsUpdateStatus` komutunu kullanarak yükleme ilerleme durumunu izleyin. `RunInProgress` değeri `False` olarak değiştiğinde güncelleştirme tamamlanır.
4. Yükleme tamamlandıktan sonra, bakım modu düzeltmesinin yüklendiği denetleyici yeniden başlatılır. Seçenek 1 olarak oturum açın, **tam erişimle oturum açın**ve disk firmware sürümünü doğrulayın. Şunu yazın:
   
   `Get-HcsFirmwareVersion`
   
   Beklenen disk üretici yazılımı sürümleri şunlardır:
   
   `XMGJ, XGEG, KZ50, F6C2, VR08, N003, 0107`
   
   Örnek çıktı aşağıda gösterilmiştir.
   
       -----------------------MAINTENANCE MODE------------------------
       Microsoft Azure StorSimple Appliance Model 8600
       Name: Update4-8600-mystorsimple
       Software Version: 6.3.9600.17845
       Copyright (C) 2014 Microsoft Corporation. All rights reserved.
       You are connected to Controller1
       ---------------------------------------------------------------
   
       Controller1>Get-HcsFirmwareVersion
   
       Controller0 : TalladegaFirmware
           ActiveBIOS:0.45.0010
              BackupBIOS:0.45.0006
              MainCPLD:17.0.000b
              ActiveBMCRoot:2.0.001F
              BackupBMCRoot:2.0.001F
              BMCBoot:2.0.0002
              LsiFirmware:20.00.04.00
              LsiBios:07.37.00.00
              Battery1Firmware:06.2C
              Battery2Firmware:06.2C
              DomFirmware:X231600
              CanisterFirmware:3.5.0.56
              CanisterBootloader:5.03
              CanisterConfigCRC:0x9134777A
              CanisterVPDStructure:0x06
              CanisterGEMCPLD:0x19
              CanisterVPDCRC:0x142F7DC2
              MidplaneVPDStructure:0x0C
              MidplaneVPDCRC:0xA6BD4F64
              MidplaneCPLD:0x10
              PCM1Firmware:1.00|1.05
              PCM1VPDStructure:0x05
              PCM1VPDCRC:0x41BEF99C
              PCM2Firmware:1.00|1.05
              PCM2VPDStructure:0x05
              PCM2VPDCRC:0x41BEF99C

           EbodFirmware
              CanisterFirmware:3.5.0.56
              CanisterBootloader:5.03
              CanisterConfigCRC:0xB23150F8
              CanisterVPDStructure:0x06
              CanisterGEMCPLD:0x14
              CanisterVPDCRC:0xBAA55828
              MidplaneVPDStructure:0x0C
              MidplaneVPDCRC:0xA6BD4F64
              MidplaneCPLD:0x10
              PCM1Firmware:3.11
              PCM1VPDStructure:0x03
              PCM1VPDCRC:0x6B58AD13
              PCM2Firmware:3.11
              PCM2VPDStructure:0x03
              PCM2VPDCRC:0x6B58AD13

           DisksFirmware
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              SmrtStor:TXA2D20800GA6XYR:KZ50
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
              WD:WD4001FYYG-01SL3:VR08
   
    Yazılım sürümünün güncelleştirildiğinden emin olmak için ikinci denetleyicide `Get-HcsFirmwareVersion` komutunu çalıştırın. Bundan sonra bakım modundan çıkabilirsiniz. Bunu yapmak için, her bir cihaz denetleyicisi için aşağıdaki komutu yazın:
   
   `Exit-HcsMaintenanceMode`

5. Bakım modundan çıktığınızda denetleyiciler yeniden başlatılır. Disk firmware güncelleştirmeleri başarıyla uygulandıktan ve cihaz bakım modundan çıktıktan sonra Azure portalına geri dönün. Yüklediğiniz bakım modu güncelleştirmeleri, 24 saat boyunca portalda gösterilmeyebilir.

