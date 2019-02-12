---
title: Hızlı Başlangıç - Azure Active Directory koşullu erişim ile bir oturumu risk algılandığında erişimi engelleme | Microsoft Docs
description: Bu hızlı başlangıçta, oturum açma oturumu riskler için temel engellemek için bir Azure Active Directory (Azure AD) koşullu erişim ilkesini nasıl yapılandırabileceğinizi öğrenin.
services: active-directory
keywords: uygulamalara koşullu erişim, Azure AD ile koşullu erişim, şirket kaynaklarına güvenli erişim, koşullu erişim ilkeleri
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/14/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: b6738496a25c49a74d3156b5dfba1ba2a91f102c
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2019
ms.locfileid: "56002056"
---
# <a name="quickstart-block-access-when-a-session-risk-is-detected-with-azure-active-directory-conditional-access"></a>Hızlı Başlangıç: Azure Active Directory koşullu erişim ile bir oturumu risk algılandığında erişimi engelleme  

Ortamınızın korumasını sürdürün için şüpheli kullanıcılardan oturum açma engellemek isteyebilirsiniz. [Azure Active Directory (Azure AD) kimlik koruması](../active-directory-identityprotection.md) her oturum açma analiz eder ve bir kullanıcı hesabının meşru sahibi tarafından bir oturum açma denemesi olasılığını gerçekleştirilmediği hesaplar. Olasılığını (düşük, Orta, yüksek) adında bir hesaplanan değer biçiminde belirtilir [oturum açma risk düzeyleri](conditions.md#sign-in-risk). Oturum açma riski koşuluna ayarlayarak, belirli oturum açma risk düzeyleri için yanıt vermek için koşullu erişim ilkesi yapılandırabilirsiniz. 

Bu hızlı başlangıçta nasıl yapılandırılacağını gösteren bir [koşullu erişim ilkesi](../active-directory-conditional-access-azure-portal.md) , engelleyen bir oturum açma yapılandırılmış oturum açma risk düzeyini tespit edildiğinde. 

![İlke oluşturma](./media/app-sign-in-risk/1000.png)


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.



## <a name="prerequisites"></a>Önkoşullar 

Bu öğreticide senaryoyu tamamlamak için şunlar gereklidir:

- **Bir Azure AD Premium P2 sürümünü erişimi** -koşullu erişim bir Azure AD Premium P1 özelliği olsa da, bu hızlı başlangıçta bir senaryoda, kimlik koruması gerektirdiğinden P2 sürümü gerekir. 

- **Kimlik koruması** -Bu hızlı başlangıçta senaryoda kimlik korumasının etkinleştirilmesini gerektirir. Kimlik Koruması'nı etkinleştirme bilmiyorsanız, bkz. [etkinleştirme Azure Active Directory kimlik koruması](../identity-protection/enable.md).

- **Tor tarayıcı** - [Tor tarayıcı](https://www.torproject.org/projects/torbrowser.html.en) çevrimiçi gizliliğinizi korumak amacıyla tasarlanmıştır. Kimlik Koruması'nın algıladığı bir oturum açma bir Tor tarayıcıdan **anonim IP adreslerinden oturum açma**, bir orta düzeyde risk düzeyine sahip. Daha fazla bilgi için bkz. [Azure Active Directory risk olayları](../reports-monitoring/concept-risk-events.md).  

- **Alain Charon adlı bir test hesabı** - bir test hesabı oluşturmak için bkz bilmiyorsanız [bulut tabanlı kullanıcılar eklemek](../fundamentals/add-users-azure-active-directory.md#add-a-new-user).


## <a name="test-your-sign-in"></a>Oturum açma testi 

Bu adımın amacı, test hesabınızı Tor tarayıcı kullanarak kiracınızda erişebildiğinizden emin olmaktır.

**Oturum açma işleminizi test etmek için:**

1. Oturum açın, [Azure portalında](https://portal.azure.com) olarak **Alain Charon**.

2. Oturumunuzu kapatın. 


## <a name="create-your-conditional-access-policy"></a>Koşullu erişim ilkenizi oluşturun 

Algılanan bir oluşturmak için bir oturum açma Tor tarayıcısından Bu hızlı başlangıçta bir senaryo kullanır **anonim IP adreslerinden oturum açma** risk olayı. Bu risk olayı risk düzeyini Orta'dır. Bu risk olayına yanıt olarak oturum açma riski koşuluna Orta ayarlayın. Bir üretim ortamında, oturum açma riski koşuluna yüksek ya da orta ve yüksek ayarlamanız gerekir.     

Bu bölümde, gerekli koşullu erişim ilkesi oluşturma işlemi gösterilmektedir. İlkenizde, ayarlayın:

|Ayar |Değer|
|---     | --- |
| Kullanıcılar ve gruplar | Alain Charon  |
| Bulut uygulamaları | Tüm bulut uygulamaları |
| Oturum açma riski | Orta |
| Erişim İzni Verme | Erişimi engelle |
 

![İlke oluşturma](./media/app-sign-in-risk/130.png)

 


**Koşullu erişim ilkenizi yapılandırmak için:**

1. [Azure portalınızda](https://portal.azure.com) genel yönetici, güvenlik yöneticisi veya koşullu erişim yöneticisi olarak oturum açın.

2. Azure portalında sol gezinti çubuğunda tıklatın **Azure Active Directory**. 

    ![Azure Active Directory](./media/app-sign-in-risk/02.png)

3. Üzerinde **Azure Active Directory** sayfasında **güvenlik** bölümünde **koşullu erişim**.

    ![Koşullu erişim](./media/app-sign-in-risk/03.png)
 
4. Üzerinde **koşullu erişim** sayfasında, üstteki araç çubuğunda tıklatın **Ekle**.

    ![Ad](./media/app-sign-in-risk/108.png)

5. Üzerinde **yeni** sayfasında **adı** metin kutusuna **orta düzeyde risk düzeyi için erişimi engelle**.

    ![Ad](./media/app-sign-in-risk/104.png)

6. İçinde **atama** bölümünde **kullanıcılar ve gruplar**.

    ![Kullanıcılar ve gruplar](./media/app-sign-in-risk/06.png)

7. Üzerinde **kullanıcılar ve gruplar** sayfası:

    ![Koşullu erişim](./media/app-sign-in-risk/107.png)

    a. Tıklayın **kullanıcıları ve grupları seçin**ve ardından **kullanıcılar ve gruplar**.

    b. **Seç**'e tıklayın.

    c. Üzerinde **seçin** sayfasında **Alain Charon**ve ardından **seçin**.

    d. Üzerinde **kullanıcılar ve gruplar** sayfasında **Bitti**.

8. Tıklayın **bulut uygulamaları**.

    ![Bulut uygulamaları](./media/app-sign-in-risk/08.png)

9. Üzerinde **bulut uygulamaları** sayfası:

    ![Koşullu erişim](./media/app-sign-in-risk/109.png)

    a. Tıklayın **tüm bulut uygulamaları**.

    b. **Bitti**’ye tıklayın.

10. Tıklayın **koşullar**. 

    ![Erişim denetimleri](./media/app-sign-in-risk/19.png)

11. Üzerinde **koşullar** sayfası:

    ![Oturum açma risk düzeyi](./media/app-sign-in-risk/21.png)

    a. Tıklayın **oturum açma riski**.
 
    b. Olarak **yapılandırma**, tıklayın **Evet**.

    c. Oturum açma risk düzeyini seçin. **orta**.

    d. **Seç**'e tıklayın.

    e. Üzerinde **koşullar** sayfasında **Bitti**.



10. İçinde **erişim denetimleri** bölümünde **Grant**.

    ![Erişim denetimleri](./media/app-sign-in-risk/10.png)

11. Üzerinde **Grant** sayfası:

    ![Koşullu erişim](./media/app-sign-in-risk/105.png)

    a. Seçin **erişimi engelle**.

    b. **Seç**'e tıklayın.

12. İçinde **ilkesini etkinleştir** bölümünde **üzerinde**.

    ![İlkeyi etkinleştir](./media/app-sign-in-risk/18.png)

13. **Oluştur**’a tıklayın.


## <a name="evaluate-a-simulated-sign-in"></a>Bir sanal oturum değerlendir

Koşullu erişim ilkenizi yapılandırdığınıza göre şimdi beklenen şekilde çalışıp çalışmadığını kontrol etmek isteyebilirsiniz. İlk adım, koşullu erişim kullanmak **ne İlkesi aracı** bir oturum açma, test kullanıcının benzetimini yapmak için. Benzetim, bu oturum açma işleminin ilkeleriniz üzerindeki etkisini tahmin eder ve bir benzetim raporu oluşturur.  

Çalıştırdığınızda **ne İlkesi aracı** bu senaryo için **orta düzeyde risk düzeyi için erişimi engelle** altında listelenmelidir **uygulanacak ilkeler**. 

![Kullanıcı](./media/app-sign-in-risk/117.png)


**Koşullu erişim ilkenizi değerlendirmek için:**

1. Üzerinde [koşullu erişim - ilkeleri](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) sayfasında, üstteki menüde **ne**.  
 
    ![What If](./media/app-sign-in-risk/14.png)

2. Tıklayın **kullanıcı**seçin **Alan Charon** üzerinde **kullanıcılar** sayfasında ve ardından **seçin**.

    ![Kullanıcı](./media/app-sign-in-risk/116.png)

3. Olarak **oturum açma riski**seçin **orta**.

    ![Kullanıcı](./media/app-sign-in-risk/119.png)


3. Tıklayın **ne**.


## <a name="test-your-conditional-access-policy"></a>Koşullu erişim ilkenizi test etme

Önceki bölümde, bir sanal oturum değerlendirilecek öğrendiniz. Bir simülasyon ek olarak, koşullu erişim ilkenizi, beklendiği gibi çalıştığından emin olmak için sınamalısınız. 

İlkenizi test etmek için oturum açın deneyin, [Azure portalında](https://portal.azure.com) olarak **Alan Charon** Tor tarayıcıyı kullanarak. Oturum açma denemesi, koşullu erişim ilkesi tarafından engellenmesi gerekir.

![Multi-factor authentication](./media/app-sign-in-risk/118.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, test kullanıcısı, Tor tarayıcı ve koşullu erişim ilkesini Sil:

- Bir Azure AD kullanıcı silme işlemini bilmiyorsanız, bkz. [Azure AD'den kullanıcı silme](../fundamentals/add-users-azure-active-directory.md#delete-a-user).

- İlkeniz silinecek ilkenizi seçin ve ardından **Sil** hızlı erişim araç çubuğu.

    ![Multi-factor authentication](./media/app-sign-in-risk/33.png)

- Tor tarayıcı kaldırma yönergeleri için bkz: [kaldırma](https://tb-manual.torproject.org/uninstalling/).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kabul edilmesi için kullanım koşullarını gerektiren](require-tou.md)
> [belirli uygulamalar için MFA gerekir](app-based-mfa.md)

