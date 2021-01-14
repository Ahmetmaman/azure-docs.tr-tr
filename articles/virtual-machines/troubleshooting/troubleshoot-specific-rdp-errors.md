---
title: Azure VM 'Leri için belirli RDP hata iletileri | Microsoft Docs
description: Azure 'da bir Windows sanal makinesine Uzak Masaüstü bağlantısı kullanmaya çalışırken alabileceğiniz belirli hata iletilerini anlayın
keywords: Uzak Masaüstü hatası, Uzak Masaüstü bağlantı hatası, VM 'ye bağlanılamıyor, Uzak Masaüstü sorunlarını giderme
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: top-support-issue,azure-service-management,azure-resource-manager
ms.assetid: 5feb1d64-ee6f-4907-949a-a7cffcbc6153
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 808443cb805b2dee2426198f9cd4f7ba502d8999
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98197515"
---
# <a name="troubleshooting-specific-rdp-error-messages-to-a-windows-vm-in-azure"></a>Azure’da Windows VM’si için belirli RDP hata iletileri sorunları giderme
Azure 'da bir Windows sanal makinesine (VM) Uzak Masaüstü bağlantısı kullanırken, belirli bir hata iletisi alabilirsiniz. Bu makalede, karşılaşılan daha yaygın hata iletilerinin bazıları ve bunları çözmek için sorun giderme adımlarıyla birlikte ayrıntılar verilmektedir. VM 'nize RDP kullanarak bağlanma sorunları yaşıyorsanız ancak belirli bir hata iletisiyle karşılaşmayın, [Uzak Masaüstü için sorun giderme kılavuzuna](troubleshoot-rdp-connection.md)bakın.

Belirli hata iletileri hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Bir lisans sağlamak için kullanılabilir Uzak Masaüstü lisans sunucusu olmadığından uzak oturumun bağlantısı kesildi](#rdplicense).
* [Uzak Masaüstü "adı" bilgisayarını bulamıyor](#rdpname).
* [Bir kimlik doğrulama hatası oluştu. Yerel güvenlik yetkilisine](#rdpauth)ulaşılamıyor.
* [Windows güvenlik hatası: kimlik bilgileriniz çalışmadı](#wincred).
* [Bu bilgisayar uzak bilgisayara bağlanamıyor](#rdpconnect).

<a id="rdplicense"></a>

## <a name="the-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-to-provide-a-license"></a>Bir lisans sağlamak için kullanılabilir Uzak Masaüstü lisans sunucusu olmadığından uzak oturumun bağlantısı kesildi.
Neden: Uzak Masaüstü sunucu rolü için 120 günlük lisanslama yetkisiz kullanım süresi doldu ve lisans yüklemeniz gerekiyor.

Geçici bir çözüm olarak, portaldan RDP dosyasının yerel bir kopyasını kaydedin ve bağlanmak için bir PowerShell komut isteminde bu komutu çalıştırın. Bu adım yalnızca bu bağlantı için lisanslamayı devre dışı bırakır:

```powershell
mstsc <File name>.RDP /admin
```

VM 'ye en fazla iki eşzamanlı uzak masaüstü bağlantısı gerekmiyorsa, uzak masaüstü sunucu rolünü kaldırmak için Sunucu Yöneticisi kullanabilirsiniz.

Daha fazla bilgi için bkz. [Azure VM 'nin "kullanılabilir Uzak Masaüstü lisans sunucusu yok" ile başarısız olması](/archive/blogs/mast/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available).

<a id="rdpname"></a>

## <a name="remote-desktop-cant-find-the-computer-name"></a>Uzak Masaüstü "adı" bilgisayarını bulamıyor.
Neden: bilgisayarınızdaki uzak masaüstü istemcisi, RDP dosyasının ayarlarındaki bilgisayarın adını çözümleyemiyor.

Olası çözümler:

* Bir kuruluşun intraneti kullanıyorsanız, bilgisayarınızın proxy sunucusuna erişimi olduğundan ve buna HTTPS trafiği gönderediğinden emin olun.
* Yerel olarak depolanan bir RDP dosyası kullanıyorsanız, portal tarafından oluşturulan birini kullanmayı deneyin. Bu adım, sanal makine için doğru DNS adına veya bulut hizmetine ve VM 'nin uç nokta bağlantı noktasına sahip olmanızı sağlar. Portal tarafından oluşturulan örnek bir RDP dosyası aşağıda verilmiştir:

    ```output
    full address:s:tailspin-azdatatier.cloudapp.net:55919
    prompt for credentials:i:1
    ```

Bu RDP dosyasının adres kısmı şunlardır:

* VM 'yi içeren bulut hizmetinin tam etki alanı adı (Bu örnekte "tailspin-azdatatier.cloudapp.net").
* Uzak Masaüstü trafiği için uç noktanın dış TCP bağlantı noktası (55919).

<a id="rdpauth"></a>

## <a name="an-authentication-error-has-occurred-the-local-security-authority-cannot-be-contacted"></a>Bir kimlik doğrulama hatası oluştu. Yerel güvenlik yetkilisine ulaşılamıyor.
Neden: hedef VM, kimlik bilgilerinizin Kullanıcı adı bölümünde güvenlik yetkilisini bulamıyor.

Kullanıcı adınız *securityauthority* \\ *Kullanıcı* adı (örnek: CORP\User1) biçiminde olduğunda, *securityauthority* bölümü sanal makinenin bilgisayar adı (yerel güvenlik yetkilisi için) veya Active Directory etki alanı adıdır.

Olası çözümler:

* Hesap sanal makineye yerelse, VM adının doğru yazıldığından emin olun.
* Hesap bir Active Directory etki alanında ise, etki alanı adının yazımını denetleyin.
* Active Directory bir etki alanı hesabı ve etki alanı adı doğru yazılmışsa, bu etki alanında bir etki alanı denetleyicisinin kullanılabilir olduğunu doğrulayın. Bu, bir etki alanı denetleyicisinin başlatılmamış olduğu için kullanılamayan etki alanı denetleyicileri içeren Azure sanal ağlarında yaygın bir sorundur. Geçici bir çözüm olarak, bir etki alanı hesabı yerine yerel yönetici hesabı kullanabilirsiniz.

<a id="wincred"></a>

## <a name="windows-security-error-your-credentials-did-not-work"></a>Windows güvenlik hatası: kimlik bilgileriniz çalışmadı.
Neden: hedef VM, hesap adınızı ve parolanızı doğrulayamıyor.

Windows tabanlı bir bilgisayar, yerel bir hesabın veya bir etki alanı hesabının kimlik bilgilerini doğrulayabilir.

* Yerel hesaplar için *ComputerName* \\ *Kullanıcı adı* sözdizimini kullanın (örnek: SQL1\Admin4798).
* Etki alanı hesapları için *DomainName* \\ *Kullanıcı adı* sözdizimini kullanın (örnek: CONTOSO\peterodman).

VM 'nizi yeni bir Active Directory ormanında bir etki alanı denetleyicisine yükseltdüyseniz, ile oturum açtığınız yerel yönetici hesabı, yeni orman ve etki alanında aynı parolaya sahip eşdeğer bir hesaba dönüştürülür. Yerel hesap daha sonra silinir.

Örneğin, DC1\DCAdmin yerel hesabı ile oturum açtıysanız ve sonra sanal makineyi corp.contoso.com etki alanı için yeni bir ormanda etki alanı denetleyicisi olarak yükseltin, DC1\DCAdmin yerel hesabı silinir ve aynı parolayla yeni bir etki alanı hesabı (CORP\DCAdmin) oluşturulur.

Hesap adının, sanal makinenin geçerli bir hesap olarak doğrulayabildiğini ve parolanın doğru olduğundan emin olun.

Yerel yönetici hesabının parolasını değiştirmeniz gerekiyorsa, bkz. [Windows sanal makineleri için bir parolayı veya Uzak Masaüstü hizmetini sıfırlama](reset-rdp.md).

<a id="rdpconnect"></a>

## <a name="this-computer-cant-connect-to-the-remote-computer"></a>Bu bilgisayar uzak bilgisayara bağlanamıyor.
Neden: bağlanmak için kullanılan hesabın uzak masaüstü oturum açma hakları yok.

Her Windows bilgisayarında, uzaktan oturum açabilirler hesapları ve grupları içeren uzak masaüstü kullanıcıları yerel grubu bulunur. Yerel Yöneticiler grubunun üyeleri, bu hesaplar uzak masaüstü kullanıcıları yerel grubunda listelenmese de, erişimi de vardır. Etki alanına katılmış makineler için yerel Yöneticiler grubu, etki alanının etki alanı yöneticilerini de içerir.

Bağlanırken kullandığınız hesabın uzak masaüstü oturum açma haklarına sahip olduğundan emin olun. Geçici bir çözüm olarak, Uzak Masaüstü üzerinden bağlanmak için bir etki alanı veya yerel yönetici hesabı kullanın. İstenen hesabı uzak masaüstü kullanıcıları yerel grubuna eklemek için, Microsoft Yönetim Konsolu ek bileşenini (**sistem araçları > yerel kullanıcılar ve gruplar > gruplar > uzak masaüstü kullanıcıları**) kullanın.

## <a name="next-steps"></a>Sonraki adımlar
Bu hatalardan hiçbiri oluştuysa ve RDP kullanarak bağlanma ile ilgili bilinmeyen bir sorununuz varsa, [Uzak Masaüstü için sorun giderme kılavuzuna](troubleshoot-rdp-connection.md)bakın.

* Bir VM üzerinde çalışan uygulamalara erişme sorunlarını giderme adımları için bkz. [Azure VM 'de çalışan bir uygulamaya erişim sorunlarını giderme](./troubleshoot-app-connection.md?toc=/azure/virtual-machines/linux/toc.json).
* Azure 'da bir Linux VM 'sine bağlanmak üzere Secure Shell (SSH) kullanarak sorun yaşıyorsanız, bkz. [Azure 'Da LINUX VM Ile SSH bağlantılarında sorun giderme](./troubleshoot-ssh-connection.md?toc=/azure/virtual-machines/linux/toc.json).
