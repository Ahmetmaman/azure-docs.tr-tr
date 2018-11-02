---
title: 'Azure AD Connect eşitleme: Azure AD Connect eşitleme hizmeti hesabını değiştirme | Microsoft Docs'
description: Bu konuda belge şifreleme anahtar ve parolalar değiştirildikten sonra iptal etmek nasıl açıklar.
services: active-directory
keywords: Azure AD eşitleme hizmeti hesabı, parola
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 331c536970445dacdb9afc9d3cfa5711b82bfbf0
ms.sourcegitcommit: 6678e16c4b273acd3eaf45af310de77090137fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50747261"
---
# <a name="changing-the-azure-ad-connect-sync-service-account-password"></a>Azure AD Connect eşitleme hizmeti hesabı parolasını değiştirme
Azure AD Connect eşitleme hizmeti hesabı parolasını değiştirirseniz, şifreleme anahtarını terk ve Azure AD Connect eşitleme hizmeti hesabı parolasını yeniden kadar eşitleme hizmeti mümkün başlangıç doğru olmayacak. 

Azure AD Connect Eşitleme hizmetlerini bir parçası olarak AD DS ve Azure AD hizmet hesaplarının parolaları depolamak için bir şifreleme anahtarı kullanır.  Bu hesapları, veritabanı içinde depolanmadan önce şifrelenir. 

Kullanılan şifreleme anahtarı kullanılarak güvenli [Windows Data Protection (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx). DPAPI koruyan şifreleme anahtarı kullanarak **Azure AD Connect eşitleme hizmeti hesabının parolasını**. 

Hizmet hesabı parolasını değiştirmeniz gerekirse yordamları kullanabileceğiniz [Azure AD Connect Sync şifreleme anahtarını bırakıp](#abandoning-the-azure-ad-connect-sync-encryption-key) bunu sağlamak için.  Şifreleme anahtarı herhangi bir nedenle iptal etmeniz gerekiyorsa bu yordamları da kullanılmalıdır.

## <a name="issues-that-arise-from-changing-the-password"></a>Parola değiştirmesini ortaya çıkan sorunları
Hizmet hesabı parolasını değiştirdiğinizde yapılması gereken iki şey vardır.

İlk olarak, altında Windows hizmet denetimi yöneticisi parolasını değiştirmek gerekir.  Bu sorun çözülene kadar aşağıdaki hatalar görürsünüz:


- Windows hizmeti Denetim Yöneticisi'nde Eşitleme Hizmeti'ni başlatmak çalışırsanız hata alırsınız "**Windows yerel bilgisayarda Microsoft Azure AD eşitleme hizmeti başlatılamadı**". **1069. hata: Hizmet, bir oturum açma hatası nedeniyle başlatılamadı.** "
- Altındaki Windows Olay Görüntüleyicisi, sistem olay günlüğüne bir hata ile içeren **olay kimliği 7038** ve ileti "**ADSync hizmeti şu hata nedeniyle şu anda yapılandırılmış parola olarak oturum açamadı: kullanıcı adı veya parola doğru değil.** "

Parola güncelleştirdiyseniz, ikinci olarak, belirli koşullar altında eşitleme hizmeti artık DPAPI ile şifreleme anahtarını geri alabilirsiniz. Şifreleme anahtarı olmadan, eşitleme hizmeti şirket içine/dışına AD ile Azure AD eşitleme için gerekli olan parolaları şifresi çözülemiyor.
Hataları gibi görürsünüz:

- Windows Hizmet Denetimi Yöneticisi altında eşitleme hizmeti başlatmayı deneyin ve şifreleme anahtarı alınamıyor bu hatayla başarısız oluyor "** Windows yerel bilgisayarda Microsoft Azure AD eşitleme başlatılamadı. Daha fazla bilgi için sistem olay günlüğünü inceleyin. Microsoft olmayan bir hizmette varsa hizmet satıcısını ve hizmete özgü hata koduna bakın **-21451857952 ***. "
- Altındaki Windows Olay Görüntüleyicisi, uygulama olay günlüğüne bir hata içeriyor. **olay kimliği 6028** ve hata iletisi *"**sunucu şifreleme anahtarı erişilemez durumda.**"*

Bu hata almazsınız emin olmak için konusundaki yordamları izleyin [Azure AD Connect Sync şifreleme anahtarını bırakıp](#abandoning-the-azure-ad-connect-sync-encryption-key) parola değiştirme.
 
## <a name="abandoning-the-azure-ad-connect-sync-encryption-key"></a>Azure AD Connect Sync şifreleme anahtarını bırakıp
>[!IMPORTANT]
>Aşağıdaki yordamlar, yalnızca Azure AD Connect derleme 1.1.443.0 uygulanır veya daha eski.

Şifreleme anahtarını iptal etmek için aşağıdaki yordamları kullanın.

### <a name="what-to-do-if-you-need-to-abandon-the-encryption-key"></a>Şifreleme anahtarını iptal etmeniz gerekiyorsa sizin yapmanız gerekenler

Şifreleme anahtarını iptal gerekiyorsa, bunu gerçekleştirmek için aşağıdaki yordamları kullanın.

1. [Eşitleme hizmeti durdurulamıyor](#stop-the-synchronization-service)

1. [Var olan şifreleme anahtarını iptal](#abandon-the-existing-encryption-key)

2. [AD DS hesap parolasını girin](#provide-the-password-of-the-ad-ds-account)

3. [Azure AD eşitleme hesabının parolasını yeniden Başlat](#reinitialize-the-password-of-the-azure-ad-sync-account)

4. [Eşitleme Hizmeti Başlat](#start-the-synchronization-service)

#### <a name="stop-the-synchronization-service"></a>Eşitleme hizmeti durdurulamıyor
İlk Windows hizmeti Denetim Yöneticisi'nde hizmetini durdurabilirsiniz.  Hizmet çalışırken durdurmak çalışmadığından emin olun.  İse, tamamlar ve ardından stop kadar bekleyin.


1. Windows Hizmet Denetimi Yöneticisi (Başlangıç → Hizmetleri) gidin.
2. Seçin **Microsoft Azure AD eşitleme** ve Durdur'u tıklatın.

#### <a name="abandon-the-existing-encryption-key"></a>Var olan şifreleme anahtarını iptal
Yeni şifreleme anahtarının oluşturulabilmesi mevcut şifreleme anahtarını iptal:

1. Azure AD Connect sunucunuza yönetici olarak oturum açın.

2. Yeni bir PowerShell oturumu başlatın.

3. Klasörüne gidin: `$env:Program Files\Microsoft Azure AD Sync\bin\`

4. Komutu çalıştırın: `./miiskmu.exe /a`

![Azure AD Connect eşitleme şifreleme anahtarı yardımcı programı](./media/how-to-connect-sync-change-serviceacct-pass/key5.png)

#### <a name="provide-the-password-of-the-ad-ds-account"></a>AD DS hesap parolasını girin
Veritabanı içinde depolanan mevcut parolalar artık şifresi çözülebilir gibi eşitleme hizmeti ile AD DS hesap parolasını sağlamanız gerekir. Eşitleme hizmeti yeni bir şifreleme anahtarı kullanarak parolalarını şifreler:

1. Eşitleme Hizmeti Yöneticisi'ni (Başlangıç → eşitleme hizmeti) başlatın.
</br>![Eşitleme Hizmeti Yöneticisi](./media/how-to-connect-sync-change-serviceacct-pass/startmenu.png)  
2. Git **Bağlayıcılar** sekmesi.
3. Seçin **AD Bağlayıcısı** şirket içi için karşılık gelen AD. Birden fazla AD bağlayıcısı varsa, bunların her biri için aşağıdaki adımları yineleyin.
4. Altında **eylemleri**seçin **özellikleri**.
5. Açılan iletişim kutusunda, seçmek **Active Directory ormanına Bağlan**:
6. AD DS hesap parolasını girin **parola** metin. Parolasını bilmiyorsanız, bu adımı gerçekleştirmeden önce bilinen bir değere ayarlamalısınız.
7. Tıklayın **Tamam** yeni parolayı kaydedin ve açılır iletişim kutusunu kapatın.
![Azure AD Connect eşitleme şifreleme anahtarı yardımcı programı](./media/how-to-connect-sync-change-serviceacct-pass/key6.png)

#### <a name="reinitialize-the-password-of-the-azure-ad-sync-account"></a>Azure AD eşitleme hesabının parolasını yeniden Başlat
Eşitleme hizmeti için Azure AD hizmet hesabı parolasını doğrudan sağlayamaz. Bunun yerine, cmdlet'ini kullanmanız gerekir **Ekle ADSyncAADServiceAccount** Azure AD hizmet hesabını yeniden başlatmak için. Cmdlet, hesap parolasını sıfırlar ve eşitleme hizmeti için kullanılabilir hale getirir:

1. Azure AD Connect sunucusunda yeni bir PowerShell oturumu başlatın.
2. Cmdlet'ini çalıştırma `Add-ADSyncAADServiceAccount`.
3. Açılan iletişim kutusunda, Azure AD kiracınız için Azure AD genel yönetici kimlik bilgilerini sağlayın.
![Azure AD Connect eşitleme şifreleme anahtarı yardımcı programı](./media/how-to-connect-sync-change-serviceacct-pass/key7.png)
4. Başarılı olursa, PowerShell komut istemi görürsünüz.

#### <a name="start-the-synchronization-service"></a>Eşitleme Hizmeti Başlat
Eşitleme hizmeti şifreleme anahtarını ve ihtiyaç duyduğu tüm parolalar erişebilir, Windows hizmeti Denetim Yöneticisi'nde hizmetini yeniden başlatabilirsiniz:


1. Windows Hizmet Denetimi Yöneticisi (Başlangıç → Hizmetleri) gidin.
2. Seçin **Microsoft Azure AD eşitleme** ve yeniden Başlat'a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
**Genel bakış konuları**

* [Azure AD Connect eşitleme: anlamak ve eşitleme özelleştirme](how-to-connect-sync-whatis.md)

* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
