---
title: Kullanıcıları grup tabanlı lisanslamaya - geçirme Azure Active Directory | Microsoft Docs
description: Grup tabanlı Azure Active Directory'yi kullanarak lisanslama için tek tek kullanıcı lisanslarını geçiş yapma
services: active-directory
keywords: Azure AD lisanslama
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: users-groups-roles
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: sumitp
ms.custom: seohack1;it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: b4067a54326d0a4a8ab9029dd4afceea384cf6aa
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56188639"
---
# <a name="how-to-add-licensed-users-to-a-group-for-licensing-in-azure-active-directory"></a>Azure Active Directory'de lisanslama için lisanslı kullanıcılar grubuna ekleme

"Doğrudan atama"; aracılığıyla kuruluşlardaki kullanıcılara dağıtılan var olan lisansları olabilir diğer bir deyişle, tek tek kullanıcı lisansı atamak için PowerShell betikleri veya diğer araçları kullanarak. Kuruluşunuzdaki lisansları yönetmek için Grup tabanlı lisanslama'ı kullanmaya başlamak istiyorsanız, Grup tabanlı lisanslama ile mevcut çözümleri sorunsuz bir şekilde değiştirmek için bir geçiş planı gerekir.

Burada, Grup tabanlı lisanslamaya geçirme kullanıcılara geçici olarak atanmış lisanslarını kaybetme neden olacak bir durum kaçınmalısınız göz önünde bulundurmanız gereken en önemli şey var. Lisansları kaldırılmasına neden herhangi bir işlem, kullanıcıların hizmetler ve verilerine erişimi kaybetme riskini kaldırmak için kaçınılmalıdır.

## <a name="recommended-migration-process"></a>Önerilen geçiş işlemi

1. Var olan Otomasyon lisans ataması ve kullanıcılar için temizleme yönetme (örneğin, PowerShell) var. Olduğu gibi çalışmasını bırakın.

2. Yeni bir lisans grubu oluşturun (veya gruplar kullanmak için hangi varolan karar) ve gerekli tüm kullanıcıların üye olarak ekleneceği emin olun.

3. Gerekli lisansları bu gruplara; Amacınız, var olan Otomasyon (örneğin, PowerShell), bu kullanıcılara uygulama aynı lisanslama durumunu yansıtacak şekilde olmalıdır.

4. Lisansları grupların tüm kullanıcılara uygulandığından emin olun. Bu uygulama, her grup işleme durumunu denetleyerek ve denetim günlüklerini denetleyerek yapılabilir.

  - Lisans ayrıntılarının bakarak Noktasal bireysel kullanıcılar kullanabilirsiniz. Bunlar aynı lisansları atanmış "doğrudan" ve "devralınan" gruplarından olduğunu görürsünüz.

  - Bir PowerShell komut dosyasını çalıştırarak [kullanıcılara lisansların nasıl atandığını doğrulayın](licensing-group-advanced.md#use-powershell-to-see-who-has-inherited-and-direct-licenses).

  - Aynı ürün lisansının kullanıcıya hem doğrudan ve Grup atandığında, kullanıcı tarafından yalnızca bir lisans kullanılır. Bu nedenle geçiş işlemini gerçekleştirmek için ek bir lisans gerekir.

5. Her grup için hata durumundaki kullanıcıların denetleyerek lisans ataması başarısız olduğunu doğrulayın. Daha fazla bilgi için [tanımlama ve bir grup için lisans sorunlarını çözme](licensing-groups-resolve-problems.md).

6. Özgün doğrudan atamaları kaldırmayı düşünün; "sonucu kullanıcıların bir alt kümesi üzerinde ilk izlemek için Dalgalar içinde", yavaş yavaş yapmak isteyebilirsiniz.

  Kullanıcıların özgün doğrudan atamaları bırakabilir, ancak kullanıcılar kendi lisanslı gruplar ayrıldığında, büyük olasılıkla istemezsiniz olan özgün lisans yine de korur.

## <a name="an-example"></a>Bir örnek

Bir kuruluşun 1.000 kullanıcı vardır. Tüm kullanıcılar, Enterprise Mobility + Security (EMS) lisansı gerektirir. 200 kullanıcılar Finans departmanında ve Office 365 Kurumsal E3 lisansı gerektirir. Şu anda kuruluşun ekleme ve gelir ve Git kullanıcılardan lisansları kaldırma şirket içinde çalışan bir PowerShell komut dosyası yok. Ancak, kuruluş lisansları otomatik olarak Azure AD tarafından yönetilebilmesi grup tabanlı lisanslama ile betik değiştirin ister.

İşte geçiş işlemi aşağıdaki gibi görünebilir:

1. Azure portalını kullanarak EMS lisansı atayın **tüm kullanıcılar** Azure AD'de grup. E3 lisansı atamak **Finans departmanı** gerekli olan tüm kullanıcıları içeren grup.

2. Her bir grup için tüm kullanıcılar için lisans atama tamamlandığını doğrulayın. Her grubu seçin dikey gidin **lisansları**ve en üstündeki işleme durumunu denetlemek **lisansları** dikey.

  - "En son lisans değişiklikleri tüm kullanıcılara uygulanmadığını" arayın işleme tamamlandığını onaylamak için.

  - Üstteki kendisi için lisans değil başarıyla atandı herhangi bir kullanıcı hakkında bir bildirim arayın. Bazı kullanıcılar için lisans dışında karşılaştınız mı? Bazı kullanıcılar çakışan lisans SKU'ları Grup lisansları devralmasını engellemek var mı?

3. Nokta uygulanan hem doğrudan hem de Grup lisansları yüklü olduğunu doğrulamak için bazı kullanıcılar denetleyin. Bir kullanıcı seçin dikey penceresine gidin **lisansları**ve lisans durumunu inceleyin.

  - Geçiş sırasında beklenen kullanıcı durumunu budur:

      ![Beklenen kullanıcı durumu](./media/licensing-groups-migrate-users/expected-user-state.png)

  Bu, kullanıcının hem doğrudan hem de devralınmış lisansları sahip olduğunu doğrular. Olduğunu görebiliriz hem **EMS** ve **E3** atanır.

  - Etkin hizmetler hakkındaki ayrıntıları göstermek için her bir lisans seçin. Bu doğrudan ve Grup lisansları tam olarak aynı hizmet planları kullanıcı için etkinleştirirseniz denetlemek için kullanılabilir.

      ![hizmet planları denetleyin](./media/licensing-groups-migrate-users/check-service-plans.png)

4. Hem doğrudan hem de Grup lisansları eşdeğer olduğunu onayladıktan sonra kullanıcıları doğrudan lisanslarını kaldırma başlayabilirsiniz. Bu Portalı'nda bireysel kullanıcılar için kaldırarak test edin ve ardından toplu olarak kaldırılması için Otomasyon betikleri çalıştırın. Kaldırılan portalınızda doğrudan lisans ile aynı kullanıcı örneği aşağıda verilmiştir. Lisans durumu değişmeden kalır, ancak artık doğrudan atamaları görüyoruz dikkat edin.

  ![doğrudan lisans kaldırıldı](./media/licensing-groups-migrate-users/direct-licenses-removed.png)


## <a name="next-steps"></a>Sonraki adımlar

Grupları aracılığıyla lisans yönetimi için diğer senaryolar hakkında daha fazla bilgi edinmek için

* [Grup tabanlı Azure Active Directory lisansı nedir?](../fundamentals/active-directory-licensing-whatis-azure-portal.md)
* [Azure Active Directory'de gruba lisans atama](licensing-groups-assign.md)
* [Azure Active Directory'de grubun lisans sorunlarını tanımlama ve çözme](licensing-groups-resolve-problems.md)
* [Kullanıcılar Azure Active Directory'de Grup tabanlı lisanslama kullanarak ürün lisansları arasında geçirme](licensing-groups-change-licenses.md)
* [Azure Active Directory grup tabanlı lisanslamayla ilgili ek senaryolar](licensing-group-advanced.md)
* [Azure Active Directory'de Grup tabanlı lisanslama için PowerShell örnekleri](licensing-ps-examples.md)
