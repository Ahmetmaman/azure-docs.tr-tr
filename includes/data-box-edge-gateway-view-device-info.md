---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/04/2019
ms.author: alkohli
ms.openlocfilehash: 3312d1ec7c2535e103cf8959599c0d4c3014f520
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "86218101"
---
1. [PowerShell arabirimine bağlanın](#connect-to-the-powershell-interface).
2. `Get-HcsApplianceInfo`Cihazınızın bilgilerini almak için kullanın.

    Aşağıdaki örnek, bu cmdlet 'in kullanımını gösterir:

    ```
    [10.100.10.10]: PS>Get-HcsApplianceInfo
    
    Id                            : b2044bdb-56fd-4561-a90b-407b2a67bdfc
    FriendlyName                  : DBE-NBSVFQR94S6
    Name                          : DBE-NBSVFQR94S6
    SerialNumber                  : HCS-NBSVFQR94S6
    DeviceId                      : 40d7288d-cd28-481d-a1ea-87ba9e71ca6b
    Model                         : Virtual
    FriendlySoftwareVersion       : Data Box Gateway 1902
    HcsVersion                    : 1.4.771.324
    IsClustered                   : False
    IsVirtual                     : True
    LocalCapacityInMb             : 1964992
    SystemState                   : Initialized
    SystemStatus                  : Normal
    Type                          : DataBoxGateway
    CloudReadRateBytesPerSec      : 0
    CloudWriteRateBytesPerSec     : 0
    IsInitialPasswordSet          : True
    FriendlySoftwareVersionNumber : 1902
    UploadPolicy                  : All
    DataDiskResiliencySettingName : Simple
    ApplianceTypeFriendlyName     : Data Box Gateway
    IsRegistered                  : False
    ```

    Bazı önemli cihaz bilgilerini özetleyen bir tablo aşağıda verilmiştir:

    | Parametre | Açıklama |
    |-----------|-------------|
    | FriendlyName                   | Cihaz dağıtımı sırasında yerel Web Kullanıcı arabirimi aracılığıyla yapılandırılan cihazın kolay adı. Varsayılan kolay ad, cihaz seri numarasıdır.  |
    | SerialNumber                   | Cihaz seri numarası, fabrikada atanan benzersiz bir sayıdır.                                                                             |
    | Model                          | Azure Stack Edge veya Data Box Gateway cihazınız için model. Model, Data Box Gateway için Azure Stack Edge ve sanal için fiziksel bir modeldir.                   |
    | Daha önce yazılım sürümü        | Cihaz yazılımı sürümüne karşılık gelen kolay dize. Önizleme çalıştıran bir sistem için, kolay yazılım sürümü Data Box Edge 1902 olur. |
    | HcsVersion                     | Cihazınızda çalışan HCS yazılım sürümü. Örneğin, 1902 Data Box Edge karşılık gelen HCS yazılım sürümü 1.4.771.324 ' dir.            |
    | Localcapacityınmb              | Cihazın megabit 'daki toplam yerel kapasitesi.                                                                                                        |
    | IsRegistered                   | Bu değer, cihazınızın hizmetle etkinleştirilmiş olup olmadığını gösterir.                                                                                         |


