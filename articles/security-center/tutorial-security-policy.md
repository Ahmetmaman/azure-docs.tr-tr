---
title: Güvenlik ilkeleriyle çalışma | Microsoft Docs
description: Bu makalede, Azure Güvenlik Merkezi 'nde güvenlik ilkeleriyle nasıl çalışılacağı açıklanır.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 2d248817-ae97-4c10-8f5d-5c207a8019ea
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 5aa6906f7f06e109342d81db6171773a68642e0c
ms.sourcegitcommit: f88074c00f13bcb52eaa5416c61adc1259826ce7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92342289"
---
# <a name="working-with-security-policies"></a>Güvenlik ilkeleriyle çalışma

Bu makalede güvenlik ilkelerinin nasıl yapılandırıldığı ve Güvenlik Merkezi 'nde nasıl görüntüleneceği açıklanmaktadır. 

## <a name="introduction-to-security-policies"></a>Güvenlik ilkelerine giriş

Güvenlik ilkesi, iş yüklerinizin istenen yapılandırmasını tanımlar ve şirketinizin veya düzenleyicilerinin güvenlik gereksinimleriyle uyumlu olmanıza yardımcı olur.

Azure Güvenlik Merkezi, seçtiğiniz ilkelere göre güvenlik önerilerini sağlar. Güvenlik Merkezi ilkeleri, Azure Ilkesinde oluşturulan ilke girişimlerini temel alır. İlkelerinizi yönetmek ve ilkeleri yönetim grupları ve birden çok abonelik arasında ayarlamak için [Azure ilkesi](../governance/policy/overview.md) 'ni kullanabilirsiniz.

Güvenlik Merkezi güvenlik ilkeleriyle çalışma için aşağıdaki seçenekleri sunar:

* **Yerleşik varsayılan Ilkeyi görüntüleyin ve düzenleyin** -Güvenlik Merkezi 'ni etkinleştirdiğinizde, tüm güvenlik merkezi kayıtlı aboneliklerine ' ASC default ' adlı yerleşik bir girişim otomatik olarak atanır. Bu girişimi özelleştirmek için, içindeki ilkeleri tek tek etkinleştirebilir veya devre dışı bırakabilirsiniz. Kullanıma hazır seçenekleri anlamak için [yerleşik güvenlik ilkeleri](./policy-reference.md) listesine bakın.

* **Kendi özel Ilkelerinizi ekleyin** -aboneliğinize uygulanan güvenlik girişimlerini özelleştirmek istiyorsanız, bunu Güvenlik Merkezi içinde yapabilirsiniz. Daha sonra, makineleriniz oluşturduğunuz ilkeleri izleyememesi durumunda öneriler alacaksınız. Özel ilkeler oluşturma ve atama hakkında yönergeler için bkz. [özel güvenlik Ilkeleri kullanma](custom-security-policies.md).

* **Mevzuat uyumluluk Ilkeleri ekleme** -Güvenlik Merkezi 'nin mevzuat uyumluluk panosu, ortamınızdaki tüm değerlendirmelerin durumunu belirli bir standart veya Yönetmeliği (Azure CIS, nıst SP 800-53 R4, SWIFT CSP cscf-V2020) bağlamında gösterir. Daha fazla bilgi için bkz. [mevzuata uyumluluğunu geliştirme](security-center-compliance-dashboard.md).


## <a name="manage-your-security-policies"></a>Güvenlik ilkelerinizi yönetme

Güvenlik Merkezi'nde güvenlik ilkelerinizi görüntüleme:

1. **Güvenlik Merkezi** panosunda **güvenlik ilkesi**' ni seçin.

    :::image type="content" source="./media/security-center-policies/security-center-policy-mgt.png" alt-text="İlke yönetimi sayfası&quot;:::

   **İlke yönetimi** ekranında yönetim gruplarının, aboneliklerin ve çalışma alanlarının yanı sıra yönetim grubu yapınızı da görebilirsiniz.

1. İlkelerini görüntülemek istediğiniz aboneliği veya yönetim grubunu seçin.

1. Bu abonelik veya yönetim grubu için güvenlik ilkesi sayfası görüntülenir. Kullanılabilir ve atanan ilkeleri gösterir.

   ![ilke sayfası](./media/tutorial-security-policy/security-policy-page.png)

    > [!NOTE]
    > Varsayılan ilkenize göre &quot;MG devralınmış" etiketi varsa, ilke bir yönetim grubuna atandı ve görüntülemekte olduğunuz aboneliğin Devralındığı anlamına gelir.


1. Bu sayfadaki kullanılabilir seçenekler arasından seçim yapın:

    1. Sektör ilkeleriyle çalışmak için **daha fazla standartlar Ekle**' yi seçin. Daha fazla bilgi için bkz. [dinamik uyumluluk paketlerine güncelleştirme](update-regulatory-compliance-packages.md).

    1. Özel girişimler atamak ve yönetmek için **özel girişimler Ekle**' yi seçin. Daha fazla bilgi için bkz. [özel güvenlik Ilkeleri kullanma](custom-security-policies.md).

    1. Varsayılan ilkeyi görüntülemek ve düzenlemek için, **geçerli Ilkeyi görüntüle** ' yi seçin ve aşağıda açıklandığı gibi devam edin. 

        :::image type="content" source="./media/security-center-policies/policy-screen.png" alt-text="İlke yönetimi sayfası&quot;:::

   **İlke yönetimi** ekranında yönetim gruplarının, aboneliklerin ve çalışma alanlarının yanı sıra yönetim grubu yapınızı da görebilirsiniz.

1. İlkelerini görüntülemek istediğiniz aboneliği veya yönetim grubunu seçin.

1. Bu abonelik veya yönetim grubu için güvenlik ilkesi sayfası görüntülenir. Kullanılabilir ve atanan ilkeleri gösterir.

   ![ilke sayfası](./media/tutorial-security-policy/security-policy-page.png)

    > [!NOTE]
    > Varsayılan ilkenize göre &quot;MG devralınmış":::

       Bu **güvenlik ilkesi** ekranı, seçtiğiniz abonelik veya yönetim grubunda atanan ilkeler tarafından gerçekleştirilen eylemi yansıtır.
       
       * En üstteki bağlantıları kullanarak abonelik veya yönetim grubuna uygulanan bir ilke **atamasını** açın. Bu bağlantılar atamaya erişmenizi ve ilkeyi düzenlemenizi veya devre dışı bırakmanızı sağlar. Örneğin, belirli bir ilke atamasının Endpoint Protection 'ın etkin bir şekilde reddediyor olduğunu görürseniz, ilkeyi düzenlemek veya devre dışı bırakmak için bağlantıyı kullanın.
       
       * İlke listesinde, ilkenin etkin olan uygulamasını aboneliğinizde veya yönetim grubunuzda görebilirsiniz. Kapsama uygulanan her ilkenin ayarları dikkate alınır ve ilke tarafından alınan eylemlerin toplu sonucu gösterilir. Örneğin, bir ilkenin bir atamasında devre dışıysa, ancak başka bir tane de Auditınotexist olarak ayarlandıysa, kümülatif efekt Auditınotexist uygular. Her zaman daha etkin olan etkilerden önceliklidir.
       
       * İlkelerin etkisi: Append, Audit, Auditınotexists, deny, DeployIfNotExists, Disabled. Efektlerin nasıl uygulandığı hakkında daha fazla bilgi için bkz. [ilke efektlerini anlama](../governance/policy/concepts/effects.md).

       > [!NOTE]
       > Atanan ilkeleri görüntülediğinizde, birden çok atamayı görebilir ve her atamanın kendi kendine nasıl yapılandırıldığını görebilirsiniz.


## <a name="who-can-edit-security-policies"></a>Güvenlik ilkelerini kimler düzenleyebilir?

Azure Ilke portalından, REST API aracılığıyla veya Windows PowerShell kullanarak güvenlik ilkelerini düzenleyebilirsiniz.

Güvenlik Merkezi, Azure kullanıcılarına, gruplarına ve hizmetlerine atayabileceğiniz yerleşik roller sağlayan Role-Based Access Control (RBAC) kullanır. Kullanıcılar Güvenlik Merkezi 'ni açtıklarında yalnızca erişebildikleri kaynaklarla ilgili bilgileri görürler. Kullanıcılara, kaynağın aboneliğine *sahip*, *katkıda bulunan*veya *okuyucu* rolünün atandığı anlamına gelir. Ayrıca iki özel güvenlik merkezi rolü de vardır:

- **Güvenlik okuyucusu**: öneriler, uyarılar, ilke ve sistem durumu gibi güvenlik merkezi öğelerini görüntüleme haklarına sahiptir. Değişiklik yapılamıyor.
- **Güvenlik Yöneticisi**: *güvenlik okuyucusu*ile aynı görünüm haklarına sahiptir. Ayrıca güvenlik ilkesini güncelleştirebilir ve uyarıları kapatabilir.


## <a name="disable-security-policies-and-disable-recommendations"></a>Güvenlik ilkelerini devre dışı bırakma ve önerileri devre dışı bırakma

Güvenlik girişimimiz ortamınız için önemli olmayan bir öneri tetiklerse, bu önerinin yeniden görüntülenmesini engelleyebilirsiniz. Bir öneriyi devre dışı bırakmak için, öneriyi üreten ilkeyi devre dışı bırakın.

Devre dışı bırakmak istediğiniz öneri, güvenlik merkezi 'nin mevzuat uyumluluk araçlarıyla uyguladığınız bir yasal düzenleme standardı için gerekliyse görünmeye devam eder. Yerleşik girişimde bir ilkeyi devre dışı bıraksanız bile, mevzuata standart girişim içindeki bir ilke uyumluluk için gerekliyse öneriyi tetikleyecektir. Yasal standart girişimlerden ilkeleri devre dışı bırakamıyoruz.

Öneriler hakkında daha fazla bilgi için bkz. [güvenlik önerilerini yönetme](security-center-recommendations.md).

1. Güvenlik Merkezi 'nde, **ilke & uyumluluk** bölümünde **güvenlik ilkesi**' ni seçin.

   ![ilke yönetimi](./media/tutorial-security-policy/policy-management.png)

2. Öneriyi devre dışı bırakmak istediğiniz aboneliği veya yönetim grubunu seçin.

   > [!NOTE]
   > Bir yönetim grubunun ilkelerini aboneliklerine uygulayacağını unutmayın. Bu nedenle, bir aboneliğin ilkesini devre dışı bırakırsanız ve abonelik hala aynı ilkeyi kullanan bir yönetim grubuna aitse, ilke önerilerini almaya devam edersiniz. İlke hala yönetim düzeyinden uygulanır ve öneriler yine de oluşturulur.

1. **Geçerli Ilkeyi görüntüle**' yi seçin.

   ![ilkeyi görüntüle](./media/tutorial-security-policy/view-effective-policy.png)

1. Atanan ilkeyi seçin.

   ![ilke seçin](./media/tutorial-security-policy/security-policy.png)

1. **Parametreler** bölümünde, devre dışı bırakmak istediğiniz öneriyi çağıran ilkeyi arayın ve açılan listeden **devre dışı** ' yı seçin.

   ![ilkeyi devre dışı bırak](./media/tutorial-security-policy/disable-policy.png)

1. **Kaydet**’i seçin.

   > [!NOTE]
   > İlke devre dışı bırakma değişikliklerinin etkili olması 12 saate kadar sürebilir.



## <a name="next-steps"></a>Sonraki adımlar
Bu makalede güvenlik ilkeleri açıklanmaktadır. İlgili bilgiler için aşağıdaki makalelere bakın:

- [PowerShell kullanarak ilkeleri ayarlamayı öğrenin](../governance/policy/assign-policy-powershell.md) - 
- [Azure Ilkesinde bir güvenlik ilkesini nasıl düzenleyeceğinizi öğrenin](../governance/policy/tutorials/create-and-manage.md) - 
- [Azure ilkesi kullanarak abonelikler arasında veya yönetim gruplarında ilke ayarlamayı öğrenin](../governance/policy/overview.md).
- [Yönetim grubundaki tüm aboneliklerde Güvenlik Merkezi 'ni nasıl etkinleştirebileceğinizi öğrenin](onboard-management-group.md)