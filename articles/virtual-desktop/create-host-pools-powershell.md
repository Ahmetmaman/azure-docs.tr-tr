---
title: PowerShell ile (Önizleme) - Azure konak havuz oluşturma
description: Konak havuzu sanal masaüstü Windows PowerShell cmdlet'leri ile oluşturma
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 4b65d7614db94a9cc3fdca3f4b784c2c84ebaef8
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58318547"
---
# <a name="create-a-host-pool-with-powershell-preview"></a>PowerShell (Önizleme) ile bir ana makine havuzu oluşturma

Ana bilgisayar havuzları, Windows sanal masaüstü (Önizleme) Kiracı ortamlar içinde bir veya daha fazla aynı sanal makinelerden oluşan bir koleksiyondur. Her konak havuzu, fiziksel masaüstünde yaptıkları gibi kullanıcı etkileşim kurabilir bir uygulama grubu içerebilir.

## <a name="use-your-powershell-client-to-create-a-host-pool"></a>Bir konak havuzu oluşturmak için PowerShell istemcinizi kullanın

İlk olarak, [indirin ve Windows sanal masaüstü PowerShell modülünü içeri aktarın](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) henüz yapmadıysanız, PowerShell oturumunuzda kullanılacak.

Windows sanal masaüstü ortama oturum açmak için aşağıdaki cmdlet'i çalıştırın

```powershell
Add-RdsAccount -DeploymentUrl https://rdbroker.wvd.microsoft.com
```

Bundan sonra Kiracı grubunuza bağlamını ayarlamak için aşağıdaki cmdlet'i çalıştırın. Kiracı grubunun adı yoksa, bu cmdlet atlayabilirsiniz kiracınızın "Varsayılan Kiracı gruba" kaynaklanıyor olabilir.

```powershell
Set-RdsContext -TenantGroupName <tenantgroupname>
```

Ardından, Windows sanal masaüstü kiracınıza yeni bir ana makine havuzu oluşturmak için bu cmdlet'i çalıştırın:

```powershell
New-RdsHostPool -TenantName <tenantname> -Name <hostpoolname>
```

Konak havuzu katılın ve yerel bilgisayarınızda yeni bir dosyaya kaydetmek için bir oturum ana bilgisayarı yetkilendirmek için bir kayıt belirtecinizi oluşturmak için İleri cmdlet'ini çalıştırın. -ExpirationHours parametresini kullanarak kayıt belirtecinizi ne kadar süreyle geçerli olacağını belirtebilirsiniz.

```powershell
New-RdsRegistrationInfo -TenantName <tenantname> -HostPoolName <hostpoolname> -ExpirationHours <number of hours> | Select-Object -ExpandProperty Token > <PathToRegFile>
```

Bundan sonra varsayılan masaüstü uygulaması Grup ana makine havuzu için Azure Active Directory Kullanıcıları eklemek için bu cmdlet'i çalıştırın.

```powershell
Add-RdsAppGroupUser -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName "Desktop Application Group" -UserPrincipalName <userupn>
```

**Ekle RdsAppGroupUser** cmdlet'i güvenlik grupları eklemeyi desteklemez ve yalnızca bir kullanıcı aynı anda uygulama grubuna ekler. Uygulama grubu için birden çok kullanıcı eklemek istiyorsanız, uygun kullanıcı asıl adlarından cmdlet'ini yeniden çalıştırın.

Daha sonra kullanacağınız bir değişken için kayıt belirteci vermek için aşağıdaki cmdlet'i çalıştırın [Windows sanal masaüstü ana havuzuna sanal makineleri kaydetmek](#register-the-virtual-machines-to-the-windows-virtual-desktop-host-pool).

```powershell
$token = (Export-RdsRegistrationInfo -TenantName <tenantname> -HostPoolName <hostpoolname>).Token
```

## <a name="create-virtual-machines-for-the-host-pool"></a>Ana makine havuzu için sanal makine oluşturma

Artık Windows sanal masaüstü konak havuzunuza katılabilir bir Azure sanal makinesinde oluşturabilirsiniz.

Birden çok yolla bir sanal makine oluşturabilirsiniz:

- [Bir Azure Galerisi görüntüsünü bir sanal makine oluşturun](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#create-virtual-machine)
- [Yönetilen bir görüntüden sanal makine oluşturma](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-generalized-managed)
- [Yönetilmeyen bir görüntüden sanal makine oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

## <a name="prepare-the-virtual-machines-for-windows-virtual-desktop-agent-installations"></a>Windows sanal masaüstü aracı yüklemeleri için sanal makineleri hazırlama

Windows sanal masaüstü aracıları yüklemek ve Windows sanal masaüstü konak havuzunuz için sanal makineleri kaydetmek için önce sanal makinelerinizi hazırlamak için şunları yapmanız gerekir:

- Etki alanına katılım makine gerekir. Bu gelen Windows sanal masaüstü kullanıcılar kendi Azure Active Directory hesabı, Active Directory hesabına eşlenmesi ve başarılı bir şekilde sanal makineye erişim izni sağlar.
- Sanal makine Windows Server işletim sistemi olarak çalışıyorsa Uzak Masaüstü oturumu ana bilgisayarı (RDSH) rolü (Önizleme) yüklemeniz gerekir. RDSH rolü düzgün şekilde yüklemek Windows sanal masaüstü aracıları sağlar.

Başarıyla etki alanına katılma için her sanal makinede aşağıdaki işlemleri yapın:

1. [Sanal makineye bağlanma](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine) sanal makine oluştururken sağladığınız kimlik.
2. Sanal makinede başlatın **Denetim Masası** seçip **sistem**.
3. Seçin **bilgisayar adı**seçin **ayarlarını değiştir**ve ardından **Değiştir...**
4. Seçin **etki alanı** ve ardından sanal ağ üzerinde Active Directory etki alanını girin.
5. Makine etki alanına katılım ayrıcalıkları olan bir etki alanı hesabıyla kimlik doğrulaması.

## <a name="register-the-virtual-machines-to-the-windows-virtual-desktop-host-pool"></a>Windows sanal masaüstü ana havuzuna sanal makineleri kaydetmek

Bir Windows sanal masaüstü ana havuzuna sanal makineleri kaydetme, sanal masaüstü Windows aracılarını yükleme olarak kadar kolaydır.

Sanal Masaüstü Windows aracılarını kaydetmek için her sanal makinede aşağıdakileri yapın:

1. [Sanal makineye bağlanma](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal#connect-to-virtual-machine) sanal makine oluştururken sağladığınız kimlik.
2. İndirin ve Windows sanal masaüstü Aracısı'nı yükleyin.
   - İndirme [Windows sanal masaüstü aracı](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrmXv).
   - İndirilen yükleyiciye sağ tıklayın, **özellikleri**seçin **Engellemeyi Kaldır**, ardından **Tamam**. Bu yükleyici güven için sisteminizi olanak tanır.
   - Yükleyiciyi çalıştırın. Yükleyici için kayıt belirtecinizi sorduğunda, aradığınızı değeri girin **dışarı aktarma RdsRegistrationInfo** cmdlet'i.
3. İndirin ve sanal masaüstü Aracısı Windows Önyükleme Yükleyicisi'ni yükleyin.
   - İndirme [Windows sanal masaüstü aracı Şifresizdir](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrxrH).
   - İndirilen yükleyiciye sağ tıklayın, **özellikleri**seçin **Engellemeyi Kaldır**, ardından **Tamam**. Bu yükleyici güven için sisteminizi olanak tanır.
   - Yükleyiciyi çalıştırın.
4. Yükleme ya da Windows sanal masaüstü yan yana yığın etkinleştirin. Adımlar farklı olacaktır sanal makine bağlı olarak hangi işletim sistemi sürümü kullanır.
   - Sanal makinenin işletim sistemi Windows Server 2016 ise:
     - İndirme [Windows sanal masaüstü yan yana yığın](https://go.microsoft.com/fwlink/?linkid=2084270).
     - İndirilen yükleyiciye sağ tıklayın, **özellikleri**seçin **Engellemeyi Kaldır**, ardından **Tamam**. Bu yükleyici güven için sisteminizi olanak tanır.
     - Yükleyiciyi çalıştırın.
   - Sanal makinenin işletim sistemi Windows ise 10 1809 veya üzeri ya da Windows Server 2019 veya sonraki bir sürümü:
     - İndirme [betik](https://go.microsoft.com/fwlink/?linkid=2084268) yan yana yığın etkinleştirmek için.
     - İndirdiğiniz betiğin sağ tıklayın, **özellikleri**seçin **Engellemeyi Kaldır**, ardından **Tamam**. Bu betik güven için sisteminizi olanak tanır.
     - Gelen **Başlat** menüsünde, Windows PowerShell ISE'yi arayın, sağ tıklayın ve ardından **yönetici olarak çalıştır**.
     - Seçin **dosya**, ardından **Aç...** ve ardından PowerShell betiğini indirilen dosyaları bulmak ve açmak.
     - Betiği çalıştırmak için yeşil Oynat düğmesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bir konak havuzu yaptığınız, RemoteApps (Önizleme) ile doldurmak için zaman var. Windows sanal masaüstü uygulamalarında yönetme hakkında daha fazla bilgi için Yönet uygulama grupları öğretici bakın.

> [!div class="nextstepaction"]
> [Uygulama grupları öğretici yönetme](./manage-app-groups.md)
