---
title: Azure aboneliği sahipliğini başka bir hesaba aktarma | Microsoft Docs
description: Başka bir kullanıcı ve süreci hakkında sık sorulan bazı sorular (SSS) için bir Azure aboneliği aktarım açıklar
keywords: Azure aboneliği, azure aktarım aboneliği aktarmayı, azure aboneliği taşımak için başka bir hesabı, azure yeni abonelik sahibi, azure aboneliği başka bir hesaba aktarma
services: ''
documentationcenter: ''
author: genlin
manager: jlian
editor: ''
tags: billing,top-support-issue
ms.assetid: c8ecdc1e-c9c5-468c-a024-94ae41e64702
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f58c156019048a4e6b08267bf28325857ec69b3a
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44302218"
---
# <a name="transfer-ownership-of-an-azure-subscription-to-another-account"></a>Azure aboneliğinin sahipliğini başka bir hesaba aktarma

Aboneliğiniz, hesap Merkezi'nde Hesap Yöneticisi değiştirmek ve abonelik faturalandırma sahipliğini teslim için başka bir kullanıcıya aktarın. Aboneliğinizi farklı bir teklif değiştirmek için bkz [Azure aboneliğinizi başka bir teklife geç](billing-how-to-switch-azure-offer.md).

> [!IMPORTANT]
> 
> Yeni bir Azure AD aboneliği transfer ederseniz Kiracı, tüm rol atamalarında [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md) kaynak kiracıdan kalıcı olarak silinir ve hedef kiracıya geçirilmez.

> [!div class="nextstepaction"]
> [Azure faturalama belgelerinin iyileştirilmesine yardımcı olun](https://go.microsoft.com/fwlink/p/?linkid=2010091)

## <a name="transfer-ownership-of-an-azure-subscription"></a>Bir Azure aboneliğinin sahipliğini devretme

> [!VIDEO https://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/Transfer-an-Azure-subscription/player]
>
>

1. Oturum açmak [Azure hesap Merkezi](https://account.windowsazure.com/Subscriptions) hesap yöneticisi olarak Aboneliğin Hesap yöneticisinin kim olduğunu bulmak için bkz: [sık sorulan sorular](#faq).

1. Aktarım için abonelik seçin.

1. Kontrol ederek aboneliğinizi Self Servis aktarım için uygun olduğundan emin olun **teklif** ve **Teklif kimliği** ile [desteklenen Teklifler listesi](#supported).

   ![Teklif aboneliği hesap Merkezi'nde Kimliğini doğrulayın](./media/billing-subscription-transfer/image0.png)
1. **Aboneliği devret**'e tıklayın.

   ![Azure hesabı abonelikler sekmesine](./media/billing-subscription-transfer/image1.png)
1. Alıcıyı belirtin.

   > [!IMPORTANT]
   > 
   > Yeni bir Azure AD aboneliği transfer ederseniz Kiracı, tüm rol atamalarında [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md) kaynak kiracıdan kalıcı olarak silinir ve hedef kiracıya geçirilmez.

   ![Aktarım aboneliği iletişim kutusu](./media/billing-subscription-transfer/image2.PNG)

1. Alıcı otomatik olarak, kabul bağlantısı içeren bir e-posta alır.

   ![Alıcı için abonelik aktarım e-posta](./media/billing-subscription-transfer/image3.png)
1. Alıcı bağlantıya tıklar ve yönergeleri izler; bu arada ödeme bilgilerini de girer.

   ![İlk abonelik aktarımı web sayfası](./media/billing-subscription-transfer/image4.png)

   ![İkinci abonelik aktarımı web sayfası](./media/billing-subscription-transfer/image5.png)
1. Başarılı! Abonelik artık aktarılır.

<a id="EA"></a>

## <a name="transfer-subscription-ownership-for-enterprise-agreement-ea-customers"></a>Kurumsal Anlaşma (EA) müşterileri için aboneliğin sahipliğini devretme

Kuruluş Yöneticisi bir kayıt aboneliklerini sahipliğini aktarabilir. Başlamak için bkz: [hesap sahipliğini aktarma](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription) EA portalında.

## <a name="next-steps-after-accepting-ownership-of-a-subscription"></a>Aboneliğin sahipliğini kabul sonraki adımlar

1. Artık Hesap Yöneticisi Gözden geçirin ve Hizmet Yöneticisi ve ortak Yöneticiler diğer RBAC rollerini güncelleştirin. Daha fazla bilgi için bkz. [ekleme veya değiştirme Azure aboneliği yöneticileri](billing-add-change-azure-subscription-administrator.md) ve [RBAC ve Azure portalını kullanarak erişimini yönetme](../role-based-access-control/role-assignments-portal.md).
1. Dahil olmak üzere bu aboneliğin hizmetleriyle ilişkili kimlik bilgilerini güncelleştirin:
   1. Yönetici hakları abonelik kaynaklarına kullanıcı yönetim sertifikaları. Daha fazla bilgi için [oluşturup Azure için bir yönetim sertifikası yükleme](../cloud-services/cloud-services-certs-create.md)
   1. Depolama Hizmetleri için erişim anahtarlarını ister. Daha fazla bilgi için [Azure depolama hesapları hakkında](../storage/common/storage-create-storage-account.md)
   1. Azure sanal makineler Hizmetleri için uzaktan erişim kimlik bilgilerini ister. 
1. [Bu abonelik için faturalama uyarılarını güncelleştirin](billing-set-up-alerts.md) adresindeki [Azure hesap Merkezi](https://account.windowsazure.com/Subscriptions). 
1. Bir iş ortağıyla çalışıyorsanız, iş ortağı Kimliğini bu abonelikte güncelleştirmeyi göz önünde bulundurun. İş ortağı kimliği güncelleştirebilirsiniz [Azure portalında](https://portal.azure.com).

<a id="supported"></a>

## <a name="whats-supported"></a>Ne desteklenir:

Self Servis abonelik aktarımı, aşağıdaki tabloda listelenen abonelik türlerini ve teklifler için kullanılabilir. Şu anda ücretsiz deneme aktarılamaz veya [içinde Aç (AIO) Azure](https://azure.microsoft.com/offers/ms-azr-0111p/) abonelikler. Geçici bir çözüm için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/resource-group-move-resources.md). Diğer abonelikler aktarmak istediğiniz [sponsorluk](https://azure.microsoft.com/offers/ms-azr-0036p/) veya destek planları, [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

| Teklif Adı                                                                             | Teklif Numarası |
|----------------------------------------------------------------------------------------|--------------|
| [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)\*|MS-AZR-0017P        |
| [Microsoft iş ortağı ağı](https://azure.microsoft.com/offers/ms-azr-0025p/)          | MS-AZR-0025P        |
| [MSDN platformları](https://azure.microsoft.com/offers/ms-azr-0062p/)                     | MS-AZR-0062P        |
| [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p/)                      | MS-AZR-0003P        |
| [Kullandıkça Öde geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0023p/)             | MS-AZR-0023P        |
| [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/)           | MS-AZR-0063P        |
| [Visual Studio Enterprise: BizSpark](https://azure.microsoft.com/offers/ms-azr-0064p/) | MS-AZR-0064P        |
| [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p/)         | MS-AZR-0059P        |
| [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p/)    | MS-AZR-0060P        |

\* [EA portal](#EA)

<a id="faq"></a>

## <a name="frequently-asked-questions-faq"></a>Sık sorulan sorular (SSS)

### <a name="whoisaa"></a> Aboneliğin Hesap Yöneticisi kimdir?

Hesap Yöneticisi yapıldı veya Azure aboneliği satın kişidir. Erişim yetkiniz [hesap Merkezi](https://account.azure.com/Subscriptions) ve abonelikleri oluşturabilir, aboneliklerinizi iptal etmeniz, bir abonelik için faturalama değiştirmek veya hizmet yöneticisini değiştiremez gibi çeşitli yönetim görevlerini gerçekleştirebilirsiniz. Bir abonelik için hesap yöneticisi olan bilmiyorsanız, öğrenmek için aşağıdaki adımları kullanın.

1. Ziyaret [Azure portalındaki abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Kontrol edin ve ardından altına bakın istediğiniz aboneliği seçin **ayarları**.
1. Seçin **özellikleri**. Aboneliğin Hesap Yöneticisi görüntülenen **Hesap Yöneticisi** kutusu.

### <a name="does-everything-transfer-including-resource-groups-vms-disks-and-other-running-services"></a>Her şeyi aktarım mu? Kaynak grupları, VM'ler, diskler ve çalışan başka hizmetler de dahil olmak üzere?

Yeni sahip Web siteleri aktarımı, disk ve VM'lerin tüm kaynaklarınızı ister. Ancak, tüm [yönetici rolleri](billing-add-change-azure-subscription-administrator.md) ve [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/role-assignments-portal.md) ayarladığınız ilkeleri farklı dizinler arasında aktarım değil. Ayrıca, [uygulama kayıtları](../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md) ve başka bir kiracıya özgü Hizmetleri boyunca aktarılmıyor.

### <a id="no-button"></a> "Abonelik aktarımı" düğmesini neden göremiyorum?

Ne yazık ki, Self Servis abonelik aktarımı teklifine veya ülke için kullanılamaz. Aboneliğinizin aktarılmasına yönelik [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="does-a-subscription-transfer-result-in-any-service-downtime"></a>Abonelik aktarımı, hizmet kapalı kalma süresi içinde sonuç vermez?

Hizmet için herhangi bir etkisi yoktur. Aboneliği aktarmadan, geçerli hesap yöneticisi aboneliğinde iptal eder ve alıcının hesabı altındaki bir abonelikte oluşturur. Temel Azure Hizmetleri için yeni abonelik ilişkilidir. Abonelik kimliği aynı kalır.

### <a name="how-do-i-use-this-process-to-change-the-directory-for-subscription"></a>Abonelik dizini değiştirmek için bu işlemi nasıl kullanırım?

Azure aboneliğinin hesap yöneticisine ait olduğu dizinde oluşturulur. Dizini değiştirmek için hedef dizinde bir kullanıcı hesabı için abonelik aktarım. Kullanıcının, aktarımı kabul etmek için adımları tamamlandığında, abonelik hedef dizine otomatik olarak taşınır.

### <a name="if-i-take-over-billing-ownership-of-a-subscription-from-another-organization-do-they-continue-to-have-access-to-my-resources"></a>Başka bir kuruluştan bir abonelik faturalandırma sahipliğini üzerinden alırsa, Kaynaklarım erişiminiz devam eder?

Abonelik başka bir kiracıya aktarıldı, önceki Kiracı ile ilişkilendirilen kullanıcı abonelik erişimini kaybeder. Bir kullanıcı bir Hizmet Yöneticisi veya ortak yönetici artık olmasa bile, yine de dahil olmak üzere, başka bir güvenlik mekanizması aracılığıyla abonelik erişimleri:

* Yönetici hakları abonelik kaynaklarına kullanıcı yönetim sertifikaları. Daha fazla bilgi için [oluşturup Azure yönetim sertifikası karşıya](../cloud-services/cloud-services-certs-create.md).
* Depolama Hizmetleri için erişim anahtarlarını ister. Daha fazla bilgi için bkz. [Azure depolama hesapları hakkında](../storage/common/storage-create-storage-account.md).
* Azure sanal makineler Hizmetleri için uzaktan erişim kimlik bilgilerini ister.

Alıcı, kaynaklara erişimi kısıtlamak gerekiyorsa, hizmetle ilişkili herhangi bir gizli anahtar güncelleştirme düşünmelisiniz. En fazla kaynak, aşağıdaki adımları kullanarak güncelleştirilebilir:

  1. [Azure Portal](https://portal.azure.com) gidin.
  2. Hub menüsünde **tüm kaynakları**.
  3. Kaynak seçin.
  4. Kaynak dikey penceresinde **ayarları**. Burada görüntüleyebilir ve var olan gizli anahtarları güncelleştirme.

### <a name="if-i-transfer-the-subscription-in-the-middle-of-the-billing-cycle-does-the-recipient-pay-for-the-entire-billing-cycle"></a>Ben faturalandırma döngüsü ortasında aboneliği transfer ederseniz, alıcı ödeme tüm faturalandırma döngüsü mu?

Gönderen, transfer tamamlandıktan noktaya kadar bildirilen herhangi bir kullanım için ödeme sorumludur. Alıcı, aktarımı ve sonraki sürümlerde zamandan bildirilen kullanım sorumludur. Aktarım önce gerçekleşen ancak daha sonra bildirilen bazı kullanım olabilir. Kullanım, alıcı fatura dahil edilir.

### <a name="does-the-recipient-have-access-to-usage-and-billing-history"></a>Alıcı, kullanım ve fatura geçmişinizi erişimi var mı?

  Bilgiler yalnızca alıcıya kullanılabilir son faturanızdan miktarıdır veya ilk fatura öncesinde abonelik devredilir varsa geçerli bakiye oluşturulur. Kullanım ve fatura geçmişinizi geri kalanını abonelikle aktarılmaz.

### <a name="can-the-offer-be-changed-during-a-transfer"></a>Bu teklif, aktarım sırasında değiştirilebilir mi?

Teklif aynı kalmalıdır. Teklifinizi değiştirmek için bkz [Azure aboneliğinizi başka bir teklife geç](billing-how-to-switch-azure-offer.md).

### <a name="can-i-transfer-a-subscription-to-a-user-account-in-another-country"></a>Başka bir ülkede bir kullanıcı hesabı için abonelik aktarabilirim?

Hayır, bir kullanıcı hesabına başka bir ülkede bir aboneliğin aktarılması desteklenmez. Alıcının kullanıcı hesabı, aynı ülkede olmalıdır.

### <a name="can-the-recipient-use-a-different-payment-method"></a>Alıcı, farklı bir ödeme yöntemi kullanabilir miyim?

Evet. Ancak, aboneliğin faturalama geçmişi, iki hesap arasında bölünür.  

### <a name="is-the-payment-method-impacted-after-i-transferred-an-azure-subscription"></a>Ben bir Azure aboneliği transfer sonra ödeme yöntemini etkilenir?

Abonelik aktarımı kabul etmek için bir kredi kartı veya ödeme yöntemini benzer abonelik için ödeme yapmayı sağlanmalıdır. Örneğin, Bob bir abonelik için Jane aktarır ve Jane aktarımı kabul eder, Jane aboneliği ödemesi için bir ödeme yöntemi sağlamanız gerekir. Transfer işlemi tamamlandıktan sonra Jane değil Bob abonelik için faturalandırılır.

### <a name="how-do-i-migrate-data-and-services-for-my-azure-subscription-to-new-subscription"></a>Nasıl yeni abonelik için veri ve hizmetlerinizi Azure Aboneliğimi için geçiş yaparım?

Aboneliğin sahipliğini devretmek olamaz, kaynaklarınızı el ile geçirebilirsiniz. Bkz: [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/resource-group-move-resources.md).

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun.

Hala yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.