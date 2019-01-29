---
title: 'Azure Active Directory etki alanı Hizmetleri: Parola İlkesi | Microsoft Docs'
description: Yönetilen etki alanlarında parola ilkelerini anlama
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 1a14637e-b3d0-4fd9-ba7a-576b8df62ff2
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2018
ms.author: ergreenl
ms.openlocfilehash: 5fc50aeeb2764f90a111c2466ff5a9deefe1e65c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55167997"
---
# <a name="password-and-account-lockout-policies-on-managed-domains"></a>Yönetilen etki alanlarında parola ve hesap kilitleme ilkeleri
Bu makalede, yönetilen etki alanında varsayılan parola ilkeleri açıklanır. Ayrıca, bu ilkelerin nasıl yapılandırabileceğinizi kapsar.

## <a name="fine-grained-password-policies-fgpp"></a>İnce ayrıntılı parola ilkeleri (FGPP)
Tek bir etki alanında çok sayıda parola ilkelerini belirtmek için hassas parola ilkelerini kullanın. FGPP, farklı kullanıcı kümeleri için bir etki alanında farklı parola ve hesap kilitleme ilkeleri kısıtlamaları uygulamak sağlar. Örneğin, ayrıcalıklı hesaplar için katı parola ayarları uygulayabilirsiniz.

FGPP kullanarak aşağıdaki parola ayarlarını yapılandırabilirsiniz:
* Minimum parola uzunluğu
* Parola geçmişi
* Parolalar karmaşıklık gereksinimlerine uymalıdır.
* En az parola geçerlilik süresi
* En fazla parola geçerlilik süresi
* Hesap kilitleme ilkesi
    * Hesap kilitleme süresi
    * İzin verilen hatalı oturum açma denemesi sayısı
    * Sayacını sıfırlama başarısız oturum açma denemesi


## <a name="default-fine-grained-password-policy-settings-on-a-managed-domain"></a>Varsayılan yönetilen etki alanında hassas parola ilkesi ayarları
Aşağıdaki ekran görüntüsünde, bir Azure AD Domain Services yönetilen etki alanında yapılandırılmış varsayılan ince ayrıntılı parola ilkesi gösterilmektedir.

![Varsayılan hassas parola ilkesi](./media/how-to/default-fgpp.png)

> [!NOTE]
> Değiştiremez veya varsayılan yerleşik hassas parola ilkesini silin. 'AAD DC Administrators' grubunun üyeleri özel FGPP oluşturabilir ve bunu geçersiz kılmak için yapılandırma (öncelik kazanır) varsayılan yerleşik FGPP.
>
>

## <a name="password-policy-settings"></a>Parola İlkesi ayarları
Aşağıdaki parola ilkeleri, yönetilen bir etki alanı üzerinde varsayılan olarak yapılandırılır:
* Minimum password length (characters): 7
* En fazla parola geçerlilik süresi (yaşam süresi): 90 gün
* Parolalar karmaşıklık gereksinimlerine uymalıdır.

### <a name="account-lockout-settings"></a>Hesap kilitleme ayarları
Aşağıdaki hesap kilitleme ilkeleri, yönetilen bir etki alanı üzerinde varsayılan olarak yapılandırılır:
* Hesap kilitleme süresi: 30
* İzin verilen hatalı oturum açma denemesi sayısı: 5
* Sayacını sıfırlama başarısız oturum açma girişimi: 30 dakika

2 dakika içinde beş geçersiz parolaların kullanılıp kullanılmadığını etkili bir şekilde, kullanıcı hesaplarını 30 dakika boyunca kilitlidir. 30 dakika sonra kilidi otomatik olarak hesaplarıdır.


## <a name="create-a-custom-fine-grained-password-policy-fgpp-on-a-managed-domain"></a>Özel hassas parola ilkesi (FGPP) yönetilen bir etki alanında oluşturma
Özel bir FGPP oluşturmak ve yönetilen etki alanınızda belirli gruplara uygulayın. Bu yapılandırma, FGPP yönetilen etki alanı için yapılandırılmış varsayılan etkili bir şekilde geçersiz kılar.

> [!TIP]
> Yalnızca üyelerinin **'AAD DC Administrators'** grubu özel hassas parola ilkelerini oluşturmak için izinlere sahip.
>
>

Ayrıca, aynı zamanda özel ince ayrıntılı parola ilkeleri oluşturabilir ve bunları yönetilen etki alanında oluşturduğunuz herhangi bir özel OU'lara uygulayın.

Aşağıdaki nedenlerle bir özel FGPP yapılandırabilirsiniz:
* Farklı bir hesap kilitleme ilkesi ayarlamak için.
* Yönetilen etki alanı için bir varsayılan Parola ömrü ayarı yapılandırmak için.

Yönetilen etki alanınızda özel bir FGPP oluşturmak için:
1. Yönetilen etki alanınızı yönetmek için kullandığınız Windows VM'de oturum açın. Yoksa, yönergeleri [yönetilen bir etki alanını yönetme](active-directory-ds-admin-guide-administer-domain.md)
2. Başlatma **Active Directory Yönetim Merkezi'ni** VM üzerinde.
3. Etki alanı adı (örneğin, ' contoso100.com')'ye tıklayın.
4. Çift **sistem** sistem kapsayıcısında açın.
5. Çift **parola ayarları kapsayıcısı**.
6. Varsayılan gördüğünüz yerleşik FGPP için yönetilen etki alanı adında **AADDSSTFPSO**. Bu yerleşik FGPP değiştiremezsiniz. Bununla birlikte, varsayılan FGPP yeni bir özel FGPP geçersiz kılma oluşturun.
7. Üzerinde **görevleri** sağ tıklayarak panelinde **yeni** tıklatıp **parola ayarları**.
8. İçinde **parola ayarları oluştur** iletişim kutusunda, özel FGPP bir parçası olarak uygulamak için özel parola ayarlarını belirtin. Öncelik, FGPP Varsayılanı geçersiz kılmak için uygun şekilde ayarlamayı unutmayın.

  ![Özel bir FGPP oluşturmak](./media/how-to/custom-fgpp.png)

  > [!TIP]
  > **Yanlışlıkla silme seçeneği Koru işaretini unutmayın.** Bu seçenek belirlenirse, FGPP kaydedilemiyor.
  >
  >

9. İçinde **doğrudan aşağıdakilere**, tıklayın **Ekle** düğmesi. İçinde **kullanıcıları veya Grupları Seç** iletişim kutusunda, tıklayın **konumları** düğmesi.

  ![Kullanıcı ve grupları seçin](./media/how-to/fgpp-applies-to.png)

10. İçinde **konumları** iletişim kutusunda, etki alanı adı'nı genişletin ve **AADDC Users**. Artık, FGPP uygulanacağı kuruluş Birimini yerleşik kullanıcıların bir grup da seçebilirsiniz.

  ![Select OU'yu o grubun ait olduğu](./media/how-to/fgpp-container.png)

11. Grubun adını yazın ve tıklayın **Adları Denetle** grubu doğrulamak için düğmeyi bulunmaktadır.

  ![FGPP uygulamak için grup seçin](./media/how-to/fgpp-apply-group.png)

12. Grubun adı görüntülenen **doğrudan aşağıdakilere** bölümü. Tıklayın **Tamam** düğmesini kullanarak bu değişiklikleri kaydedin.

  ![Uygulanan FGPP](./media/how-to/fgpp-applied.png)

> [!TIP]
> **Özel bir kuruluş içindeki kullanıcı hesapları özel parola ilkeleri uygulamak için:** Yalnızca gruplar için ince ayrıntılı parola ilkeleri uygulanabilir. Kullanıcıların özel bir kuruluş için yalnızca bir özel parola ilkesini yapılandırmak için OU kullanıcılarını içeren bir grup oluşturun.
>
>

## <a name="next-steps"></a>Sonraki adımlar
* [Active Directory ince ayrıntılı parola ilkeleri hakkında bilgi edinin](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770394(v=ws.10))
* [AD yönetim merkezini kullanarak hassas parola ilkelerini yapılandırma](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-#fine_grained_pswd_policy_mgmt)
