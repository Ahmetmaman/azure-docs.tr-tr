---
title: Azure AD rollerimi PıM-Azure Active Directory 'de etkinleştir | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) içinde Azure AD rollerini nasıl etkinleştireceğinizi öğrenin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.subservice: pim
ms.date: 07/06/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 38992c15c23216aa81cda566a333d8e45f90b17e
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96004696"
---
# <a name="activate-my-azure-ad-roles-in-pim"></a>PIM'de Azure AD rollerimi etkinleştirme

Azure Active Directory (Azure AD) Privileged Identity Management (PıM), kuruluşların Azure AD 'deki kaynaklara yönelik ayrıcalıklı erişimi ve Microsoft 365 veya Microsoft Intune gibi diğer Microsoft çevrimiçi hizmetler nasıl yöneteceğini basitleştirir.  

Bir yönetim rolü için uygun yaptıysanız, ayrıcalıklı eylemler gerçekleştirmeniz gerektiğinde rol atamasını etkinleştirmeniz gerekir. Örneğin, bazen Microsoft 365 özellikleri yönetiyorsanız, bu rol diğer hizmetleri de etkilediği için kuruluşunuzun ayrıcalıklı rol yöneticileri kalıcı bir genel yönetici sunmayabilir. Bunun yerine, Exchange Online Yöneticisi gibi Azure AD rollerine uygun hale getirir. Ayrıcalıklarına ihtiyacınız olduğunda bu rolü etkinleştirmek isteyebilirsiniz ve daha sonra önceden belirlenmiş bir süre için yönetici denetimine sahip olursunuz.

Bu makale, Privileged Identity Management ' de Azure AD rolünü etkinleştirmesi gereken yöneticilere yöneliktir.

## <a name="determine-your-version-of-pim"></a>PıM sürümünüzü belirleme

2019 Kasım 'Dan başlayarak Privileged Identity Management Azure AD rolleri bölümü, Azure Kaynak rolleri deneyimleriyle eşleşen yeni bir sürüme güncelleştiriliyor. Bu, ek özellikleri [ve var olan API üzerinde yapılan değişiklikleri](azure-ad-roles-features.md#api-changes)de oluşturur. Yeni sürüm kullanıma sunulurken, bu makalede izlediğiniz yordamlar Şu anda sahip olduğunuz Privileged Identity Management sürümüne bağlıdır. Hangi Privileged Identity Management sürümünü istediğinizi öğrenmek için bu bölümdeki adımları izleyin. Privileged Identity Management Sürümünüzü öğrendikten sonra bu makaledeki sürümle eşleşen yordamları seçebilirsiniz.

1. [Ayrıcalıklı rol yöneticisi](../roles/permissions-reference.md#privileged-role-administrator) rolüyle [Azure Portal](https://portal.azure.com/) oturum açın.
1. **Azure AD Privileged Identity Management** açın. Genel Bakış sayfasının üst kısmında yer alan bir başlık varsa, bu makalenin **Yeni sürüm** sekmesinde yer alan yönergeleri izleyin. Aksi takdirde, **önceki sürüm** sekmesindeki yönergeleri izleyin.

    [![Azure AD > Privileged Identity Management seçin.](media/pim-how-to-add-role-to-user/pim-new-version.png)](media/pim-how-to-add-role-to-user/pim-new-version.png#lightbox)

# <a name="new-version"></a>[Yeni sürüm](#tab/new)

## <a name="activate-a-role-for-new-version"></a>Yeni sürüm için bir rolü etkinleştir

Bir Azure AD rolünü varsaymak istediğinizde, **rollerimi** Privileged Identity Management açarak etkinleştirme isteyebilirsiniz.

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. **Azure AD Privileged Identity Management** açın. Privileged Identity Management kutucuğunu panonuza ekleme hakkında daha fazla bilgi için bkz. [Privileged Identity Management kullanmaya başlama](pim-getting-started.md).

1. **Rollerim**' i seçin ve ardından **Azure AD rolleri** ' nı seçerek uygun Azure AD rollerinizin bir listesini görüntüleyin.

    ![Etkinleştirebilecekleri rolleri gösteren roller sayfası](./media/pim-how-to-activate-role/my-roles.png)

1. **Azure AD rolleri** listesinde, etkinleştirmek istediğiniz rolü bulun.

    ![Azure AD rolleri-uygun rollerim listesi](./media/pim-how-to-activate-role/activate-link.png)

1. Etkinleştir sayfasını açmak için **Etkinleştir** ' i seçin.

    ![Azure AD rolleri-etkinleştirme sayfası süre ve kapsam içerir](./media/pim-how-to-activate-role/activate-page.png)

1. Rolünüz çok faktörlü kimlik doğrulaması gerektiriyorsa, **devam etmeden önce kimliğinizi doğrula**' yı seçin. Her oturum için yalnızca bir kez kimlik doğrulaması yapmanız gerekir.

    ![Rol etkinleştirmeden önce MFA ile kimliğimi doğrula](./media/pim-resource-roles-activate-your-roles/resources-my-roles-mfa.png)

1. **Kimliğimi doğrula** ' yı seçin ve ek güvenlik doğrulaması sağlamak için yönergeleri izleyin.

    ![PIN kodu gibi güvenlik doğrulaması sağlayan ekran](./media/pim-resource-roles-activate-your-roles/resources-mfa-enter-code.png)

1. Daha düşük bir kapsam belirtmek istiyorsanız **kapsam** ' ı seçerek filtre bölmesini açın. Filtre bölmesinde, erişmeniz gereken Azure AD kaynaklarını belirtebilirsiniz. Yalnızca ihtiyacınız olan kaynaklara erişim istemek en iyi uygulamadır.

1. Gerekirse, özel bir etkinleştirme başlangıç saati belirtin. Azure AD rolü seçili zamandan sonra etkinleştirilecektir.

1. **Neden** kutusunda, etkinleştirme isteğinin nedenini girin.

1. **Etkinleştir**' i seçin.

    Rol etkinleştirme için [onay gerektiriyorsa](pim-resource-roles-approval-workflow.md) , tarayıcınızın sağ üst köşesinde, isteğin onay beklendiğini bildiren bir bildirim görüntülenir.

    ![Etkinleştirme isteği onay bildirimi bekliyor](./media/pim-resource-roles-activate-your-roles/resources-my-roles-activate-notification.png)

## <a name="view-the-status-of-your-requests-for-new-version"></a>Yeni sürüm için isteklerinizin durumunu görüntüleyin

Etkinleştirme için bekleyen isteklerinizin durumunu görüntüleyebilirsiniz.

1. Azure AD Privileged Identity Management açın.

1. Azure AD rolünüzün ve Azure Kaynak rolü isteklerinizin listesini görmek için **isteklerim** ' i seçin.

    ![İsteklerim-bekleyen isteklerinizi gösteren Azure AD sayfası](./media/pim-how-to-activate-role/my-requests-page.png)

1. **Istek durumu** sütununu görüntülemek için sağa kaydırın.

## <a name="cancel-a-pending-request-for-new-version"></a>Yeni sürüm için bekleyen isteği iptal et

Onay gerektiren bir rolün etkinleştirilmesini gerektirmiyorsa, bekleyen bir isteği dilediğiniz zaman iptal edebilirsiniz.

1. Azure AD Privileged Identity Management açın.

1. **Isteklerim**' i seçin.

1. İptal etmek istediğiniz rol için **iptal** bağlantısını seçin.

    Iptal ' i seçtiğinizde istek iptal edilir. Rolü yeniden etkinleştirmek için, etkinleştirme için yeni bir istek göndermeniz gerekir.

   ![Iptal eylemi vurgulanmış istek listem](./media/pim-resource-roles-activate-your-roles/resources-my-requests-cancel.png)

## <a name="troubleshoot-for-new-version"></a>Yeni sürüm için sorun giderme

### <a name="permissions-are-not-granted-after-activating-a-role"></a>Rol etkinleştirildikten sonra izinler verilmiyor

Privileged Identity Management bir rolü etkinleştirdiğinizde, etkinleştirme ayrıcalıklı rol gerektiren tüm portallara anında yaymayabilir. Bazı durumlarda değişiklik yayılsa bile portalda web önbelleği değişikliğin anında geçerlilik kazanmamasına yol açabilir. Etkinleştirme gecikirse, yapmanız gerekenler aşağıda verilmiştir.

1. Azure portalında oturumunuzu kapatın ve sonra yeniden oturum açın.

1. Privileged Identity Management, rolün üyesi olarak listelendiğinizi doğrulayın.

# <a name="previous-version"></a>[Önceki sürüm](#tab/previous)

## <a name="activate-a-role-previous-version"></a>Rol etkinleştirme (önceki sürüm)

Bir Azure AD rolünü gerçekleştirmeniz gerektiğinde, Privileged Identity Management ' de **rollerim** gezinti seçeneğini kullanarak etkinleştirme isteğinde bulunabilir.

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. **Azure AD Privileged Identity Management** açın. Privileged Identity Management kutucuğunu panonuza ekleme hakkında daha fazla bilgi için bkz. [Privileged Identity Management kullanmaya başlama](pim-getting-started.md).

1. **Azure AD rolleri**' ne tıklayın.

1. Uygun Azure AD rollerinizin listesini görmek için **rollerim** ' e tıklayın.

    ![Azure AD rolleri-uygun veya etkin roller listesini gösteren rollerim](./media/pim-how-to-activate-role/directory-roles-my-roles.png)

1. Etkinleştirmek istediğiniz bir rol bulun.

    ![Azure AD rolleri-Etkinleştir bağlantısını gösteren uygun rollerim listesi](./media/pim-how-to-activate-role/directory-roles-my-roles-activate.png)

1. Rol etkinleştirme ayrıntıları bölmesini açmak için **Etkinleştir** ' e tıklayın.

1. Rolünüz Multi-Factor Authentication (MFA) gerektiriyorsa, **devam etmeden önce kimliğinizi doğrula**' ya tıklayın. Her oturum için yalnızca bir kez kimlik doğrulaması yapmanız gerekir.

    ![Rol etkinleştirmeden önce MFA ile kimlik bölmi doğrula](./media/pim-how-to-activate-role/directory-roles-my-roles-mfa.png)

1. **Kimliğimi doğrula** ' ya tıklayın ve ek güvenlik doğrulaması sağlamak için yönergeleri izleyin.

    ![Sizinle nasıl iletişim kurasoran ek güvenlik doğrulama sayfası](./media/pim-how-to-activate-role/additional-security-verification.png)

1. Etkinleştirme bölmesini açmak için **Etkinleştir** ' e tıklayın.

    ![Başlangıç zamanı, süre, Bilet ve neden belirtmek için etkinleştirme bölmesi](./media/pim-how-to-activate-role/directory-roles-activate.png)

1. Gerekirse, özel bir etkinleştirme başlangıç saati belirtin.

1. Etkinleştirme süresini belirtin.

1. **Etkinleştirme nedeni** kutusunda, etkinleştirme isteğinin nedenini girin. Bazı roller bir sorun bileti numarası vermenizi gerektirir.

    ![Etkinleştirme bölmesi özel bir başlangıç zamanı, süre, Bilet ve neden ile tamamlandı](./media/pim-how-to-activate-role/directory-roles-activation-pane.png)

1. **Etkinleştir**' e tıklayın.

    Rol onay gerektirmiyorsa, etkinleştirmenin durumunu gösteren bir **etkinleştirme durumu** bölmesi görüntülenir.

    ![Etkinleştirmenin üç aşamasını gösteren etkinleştirme durumu sayfası](./media/pim-how-to-activate-role/activation-status.png)

    Tüm aşamalar tamamlandıktan sonra, Azure portal oturumu kapatmak için **oturumu** kapat bağlantısına tıklayın. Portalda yeniden oturum açtığınızda, artık rolünü kullanabilirsiniz.

    Rolün etkinleştirilmesi için [onay](./azure-ad-pim-approval-workflow.md) gerekiyorsa, tarayıcınızın sağ üst köşesinde, isteğin onay beklendiğini bildiren bir Azure bildirimi görüntülenir.

## <a name="view-the-status-of-your-requests-previous-version"></a>İsteklerinizin durumunu (önceki sürüm) görüntüleyin

Etkinleştirme için bekleyen isteklerinizin durumunu görüntüleyebilirsiniz.

1. Azure AD Privileged Identity Management açın.

1. **Azure AD rolleri**' ne tıklayın.

1. İsteklerinizin listesini görmek için **Isteklerim** ' e tıklayın.

    ![Azure AD rolleri-isteklerim listesi](./media/pim-how-to-activate-role/directory-roles-my-requests.png)

## <a name="deactivate-a-role-previous-version"></a>Rolü devre dışı bırak (önceki sürüm)

Bir rol etkinleştirildikten sonra, zaman sınırına (uygun süre) ulaşıldığında otomatik olarak devre dışı bırakır.

Yönetici görevlerinizi erken tamamladıktan sonra Azure AD Privileged Identity Management bir rolü el ile de devre dışı bırakabilirsiniz.

1. Azure AD Privileged Identity Management açın.

1. **Azure AD rolleri**' ne tıklayın.

1. **Rollerim**' e tıklayın.

1. Etkin roller listenizi görmek için **etkin roller** ' e tıklayın.

1. Kullanarak yaptığınız rolü bulun ve **devre dışı bırak**' a tıklayın.

## <a name="cancel-a-pending-request-previous-version"></a>Bekleyen bir isteği iptal etme (önceki sürüm)

Onay gerektiren bir rolün etkinleştirilmesini gerektirmiyorsa, bekleyen bir isteği dilediğiniz zaman iptal edebilirsiniz.

1. Azure AD Privileged Identity Management açın.

1. **Azure AD rolleri**' ne tıklayın.

1. **Isteklerim**' e tıklayın.

1. İptal etmek istediğiniz rol için **iptal** düğmesine tıklayın.

    Iptal ' e tıkladığınızda istek iptal edilir. Rolü yeniden etkinleştirmek için, etkinleştirme için yeni bir istek göndermeniz gerekir.

   ![Iptal düğmesi vurgulanmış isteklerim listesi](./media/pim-how-to-activate-role/directory-role-cancel.png)

## <a name="troubleshoot-previous-version"></a>Sorun giderme (önceki sürüm)

### <a name="permissions-are-not-granted-after-activating-a-role"></a>Rol etkinleştirildikten sonra izinler verilmiyor

Privileged Identity Management bir rolü etkinleştirdiğinizde, etkinleştirme ayrıcalıklı rol gerektiren tüm portallara anında yaymayabilir. Bazı durumlarda değişiklik yayılsa bile portalda web önbelleği değişikliğin anında geçerlilik kazanmamasına yol açabilir. Etkinleştirme gecikirse, yapmanız gerekenler aşağıda verilmiştir.

1. Azure portalında oturumunuzu kapatın ve sonra yeniden oturum açın.

    Bir Azure AD rolünü etkinleştirdiğinizde, etkinleştirmesinin aşamalarını görürsünüz. Tüm aşamalar tamamlandıktan sonra bir **Oturumu kapat** bağlantısı görürsünüz. Oturumu kapatmak için bu bağlantıyı kullanabilirsiniz. Bu, etkinleştirme gecikmesi için çoğu durumu çöztirecek.

1. Privileged Identity Management, rolün üyesi olarak listelendiğinizi doğrulayın.

 ---

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management 'de Azure AD rollerimi etkinleştirin](pim-how-to-activate-role.md)
