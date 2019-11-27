---
title: Azure maliyet yönetimi verilerine erişim atama
description: Bu makalede, Azure maliyet Yönetimi verilerine erişim kapsamları çeşitli iznini atama rağmen gösterilmektedir.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 10/14/2019
ms.topic: conceptual
ms.service: cost-management-billing
manager: vitavor
ms.custom: secdec18
ms.openlocfilehash: e3140ee990127db6815828314103a09dff7cf26e
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74219841"
---
# <a name="assign-access-to-cost-management-data"></a>Maliyet Yönetimi verilerine erişim atama

Azure Kurumsal sözleşmeleri olan kullanıcılar için, Azure portal ve Enterprise (EA) portalında verilen izinlerin bir birleşimi kullanıcının Azure maliyet yönetimi verilerine erişim düzeyini tanımlar. Diğer Azure hesabı türlerine sahip kullanıcılar için, bir kullanıcının maliyet yönetimi verilerine erişim düzeyini tanımlamak, Azure rol tabanlı erişim denetimi kullanılarak daha basittir. Bu makalede, maliyet Yönetimi verilerine erişim atama aracılığıyla gösterilmektedir. Maliyet Yönetimi'nde kullanıcı görünümleri verileri için ve Azure Portalı'nda seçtiğiniz kapsamda erişime sahip oldukları kapsamına göre bu izinleri birleşimi atandıktan sonra.

Bir kullanıcının seçtiği kapsam maliyet yönetimi, veri birleştirme sağlamak ve maliyet bilgilerini erişimi denetlemek için kullanılır. Kapsamları kullanırken, kullanıcılar çoklu seçim yoksa bunları. Bunun yerine, alt kapsamlar kadar geri alma ve sonra bunlar filtre görüntülemek istediklerini için aşağı daha büyük bir kapsam seçin. Veri birleştirme, bazı kişiler, alt kapsamlar aktarma hedefi bir üst kapsama erişimi olmaması nedeniyle anlamak önemlidir.

Azure rol tabanlı erişim denetimi ile maliyetleri ve ücretleri görüntüleme erişimi atama hakkında bilgi edinmek için [Azure maliyet yönetimi videosu Ile nasıl erişim atayacağınızı](https://www.youtube.com/watch?v=J997ckmwTa8) izleyin.

>[!VIDEO https://www.youtube.com/embed/J997ckmwTa8]

## <a name="cost-management-scopes"></a>Maliyet Yönetimi kapsamları

Maliyet yönetimi çeşitli Azure hesap türlerini destekler. Desteklenen hesap türlerinin tam listesini görüntülemek için bkz. [maliyet yönetimi verilerini anlama](understand-cost-mgt-data.md). Hesap türü kullanılabilir kapsamları belirler.

### <a name="azure-ea-subscription-scopes"></a>Azure EA abonelik kapsamları

Azure EA abonelikleriyle ilgili maliyet verilerini görüntülemek için, bir kullanıcının aşağıdaki kapsamlardan bir veya daha fazlasına en azından okuma erişimi olması gerekir.

| **Kapsam** | **Tanımlanma yeri** | **Verileri görüntülemek için gerekli erişim** | **Önkoşul EA ayarı** | **Verileri birleştirir** |
| --- | --- | --- | --- | --- |
| Faturalama hesabı<sup>1</sup> | [https://ea.azure.com](https://ea.azure.com/) | Kuruluş Yöneticisi | None | Kurumsal sözleşmedeki tüm abonelikler |
| Bölüm | [https://ea.azure.com](https://ea.azure.com/) | Bölüm Yöneticisi | **Da görüntüleme ücretleri** etkin | Bölüme bağlı olan kayıt hesabına ait olan tüm abonelikler |
| Kayıt hesabı<sup>2</sup> | [https://ea.azure.com](https://ea.azure.com/) | Hesap Sahibi | **Ao görüntüleme ücretleri** etkin | Kayıt hesabındaki tüm abonelikler |
| Yönetim grubu | [https://portal.azure.com](https://portal.azure.com/) | Maliyet Yönetimi Okuyucusu (veya Okuyucu) | **Ao görüntüleme ücretleri** etkin | Yönetim grubu altındaki tüm abonelikler |
| Abonelik | [https://portal.azure.com](https://portal.azure.com/) | Maliyet Yönetimi Okuyucusu (veya Okuyucu) | **Ao görüntüleme ücretleri** etkin | Abonelikteki tüm kaynaklar/kaynak grupları |
| Kaynak grubu | [https://portal.azure.com](https://portal.azure.com/) | Maliyet Yönetimi Okuyucusu (veya Okuyucu) | **Ao görüntüleme ücretleri** etkin | Kaynak grubundaki tüm kaynaklar |

<sup>1</sup> faturalandırma hesabına kurumsal anlaşma veya kayıt olarak da başvurulur.

<sup>2</sup> kayıt hesabı da hesap sahibi olarak adlandırılır.

Aşağıdaki diyagramda, rol ve EA Portal ayarlarıyla maliyet yönetimi kapsamları arasındaki ilişki gösterilmektedir.

![Roller ve EA Portal ayarlarıyla maliyet yönetimi kapsamları arasındaki ilişkiyi gösteren diyagram](./media/assign-access-acm-data/scope-access-relationship-diagram.png)

EA portalında **da görüntüleme ücretleri** devre dışı bırakıldığında, departmanlar ve hesapların maliyetlerini görüntülemeye çalıştığınızda *Kuruluşunuz için maliyetleri devre dışı* bildiren bir ileti görürsünüz.

Benzer şekilde, EA portalında, **Ao görüntüleme ücretleri** devre dışı bırakıldığında, kayıt hesapları, yönetim grupları, abonelikler ve kaynak grupları için maliyetleri görüntülemeye çalıştığınızda *Kuruluşunuz için maliyetleri devre dışı* olarak belirten bir ileti görürsünüz.

## <a name="other-azure-account-scopes"></a>Diğer Azure hesap kapsamları

Diğer Azure abonelikleriyle ilgili maliyet verilerini görüntülemek için, bir kullanıcının aşağıdaki kapsamlardan bir veya daha fazlasına en azından okuma erişimi olması gerekir:

- Azure hesabı
- Yönetim grubu
- Kaynak grubu

Çeşitli kapsamlar, iş ortaklarının müşterileri bir Microsoft Müşteri sözleşmesine ekledikten sonra kullanılabilir. CSP müşterileri, CSP iş ortakları tarafından etkinleştirildiğinde maliyet yönetimi özelliklerini kullanabilir. Daha fazla bilgi için bkz. [iş ortakları Için Azure maliyet yönetimi ile çalışmaya başlama](get-started-partners.md).

## <a name="enable-access-to-costs-in-the-ea-portal"></a>EA portal maliyetlerini erişimi etkinleştirme

Departman kapsamı, EA portalında da **etkin** olan **da görüntüleme ücretleri** seçeneğini gerektirir. Diğer tüm kapsamlar, EA portalında **etkin** bir şekilde **görüntüleme ücretleri** seçeneği gerektirir.

Seçeneği etkinleştirmek için:

1. Kurumsal Yönetici hesabıyla [https://ea.azure.com](https://ea.azure.com) ' de EA portalında oturum açın.
2. Sol bölmedeki **Yönet** ' i seçin.
3. Erişim sağlamak istediğiniz maliyet yönetimi kapsamları için, ücretlendirmeyi ve/veya **Ao görünüm ücretlerini** **görüntüleme ücreti** seçeneğini etkinleştirin.  
    DA ve AO görüntüleme ücretleri seçeneklerini gösteren kayıt sekmesi ![](./media/assign-access-acm-data/ea-portal-enrollment-tab.png)

Görünüm ücret seçeneklerini etkinleştirildikten sonra çoğu kapsamları, ayrıca Azure portalında rol tabanlı erişim denetimi (RBAC) iznini yapılandırma gerektirir.

## <a name="enterprise-administrator-role"></a>Kuruluş yöneticisi rolü

Varsayılan olarak, bir kuruluş yöneticisi Faturalama hesabı (Kurumsal Anlaşma/kaydı) ve alt kapsamlar olan tüm diğer kapsamlar için erişim vardır. Kuruluş Yöneticisi, diğer kullanıcılar için kapsamlar için erişim atar. İş sürekliliği için en iyi uygulama olarak her zaman iki kullanıcı Kurumsal yönetici erişimine sahip olmalıdır. Aşağıdaki bölümlerde kapsamlar diğer kullanıcılar için Kurumsal Yönetici atama erişim gözden geçirme örnekleri var.

## <a name="assign-billing-account-scope-access"></a>Fatura hesabı kapsam erişim atama

Fatura hesabı kapsamı erişim EA portalında Kurumsal yönetici izni gerektirir. Kuruluş Yöneticisi tüm EA kaydı veya birden çok kayıtlar arasında maliyetlerini görüntüleme erişimine sahiptir. Azure portalında Faturalama hesabı kapsam için herhangi bir eylemi gereklidir.

1. Kurumsal Yönetici hesabıyla [https://ea.azure.com](https://ea.azure.com) ' de EA portalında oturum açın.
2. Sol bölmedeki **Yönet** ' i seçin.
3. **Kayıt** sekmesinde, yönetmek istediğiniz kaydı seçin.  
    ![EA portalında kaydınız seçin](./media/assign-access-acm-data/ea-portal.png)
4. **+ Yönetici Ekle**' ye tıklayın.
5. Yönetici Ekle iletişim kutusunda, kimlik doğrulama türünü seçin ve kullanıcının e-posta adresini yazın.
6. Kullanıcının maliyet ve kullanım verilerine salt okuma erişimi olması gerekiyorsa, **salt okunurdur**' ın altında **Evet**' i seçin.  Aksi takdirde **Hayır**' ı seçin.
7. Hesabı oluşturmak için **Ekle** ' ye tıklayın.  
    Yönetici Ekle kutusunda gösterilen örnek bilgileri ![](./media/assign-access-acm-data/add-admin.png)

Bu yeni kullanıcı maliyet Yönetimi'nde verilere erişmeden önce tamamlanması 30 dakika sürebilir.

### <a name="assign-department-scope-access"></a>Departman kapsam erişim atama

Departman kapsamı için EA portalında departman Yöneticisi (DA ücretleri görüntüle) erişim gerekir. Bölüm Yöneticisi maliyetleri ve bir departman veya birden çok bölümlerine ilişkili kullanım verilerini görüntüleme erişimine sahiptir. Veri bölümü için bölüm için bağlı bir kayıt hesabına ait tüm abonelikler içerir. Azure portalında herhangi bir eylemi gereklidir.

1. Kurumsal Yönetici hesabıyla [https://ea.azure.com](https://ea.azure.com) ' de EA portalında oturum açın.
2. Sol bölmedeki **Yönet** ' i seçin.
3. **Kayıt** sekmesinde, yönetmek istediğiniz kaydı seçin.
4. **Departman** sekmesine tıklayın ve ardından **yönetici Ekle**' ye tıklayın.
5. Departman Yöneticisi Ekle iletişim kutusunda, kimlik doğrulaması türünü seçin ve ardından kullanıcının e-posta adresini yazın.
6. Kullanıcının maliyet ve kullanım verilerine salt okuma erişimi olması gerekiyorsa, **salt okunurdur**' ın altında **Evet**' i seçin.  Aksi takdirde **Hayır**' ı seçin.
7. Departman yönetici izni vermek istediğiniz bölümleri seçin.
8. Hesabı oluşturmak için **Ekle** ' ye tıklayın.  
    ![Departman Yöneticisi Ekle kutusuna gerekli bilgileri girin](./media/assign-access-acm-data/add-depart-admin.png)

## <a name="assign-enrollment-account-scope-access"></a>Kayıt hesabı kapsam erişim atama

Kayıt hesabı kapsam erişim EA portalında hesap sahibi (saniye başına AO görünümü ücretleri) erişimi gerektirir. Hesap sahibinin kim maliyetlerini ve kullanım verileri kayıt hesaptan oluşturulan abonelikleriyle ilişkili görüntüleyebilirsiniz. Azure portalında herhangi bir eylemi gereklidir.

1. Kurumsal Yönetici hesabıyla [https://ea.azure.com](https://ea.azure.com) ' de EA portalında oturum açın.
2. Sol bölmedeki **Yönet** ' i seçin.
3. **Kayıt** sekmesinde, yönetmek istediğiniz kaydı seçin.
4. **Hesap** sekmesine tıklayın ve ardından **Hesap Ekle**' ye tıklayın.
5. Hesap Ekle kutusunda, hesabı ilişkilendirilecek **departmanı** seçin veya atanmamış olarak bırakın.
6. Kimlik doğrulaması türünü seçin ve hesap adını yazın.
7. Kullanıcının e-posta adresini yazın ve isteğe bağlı olarak maliyet merkezini girin.
8. Hesabı oluşturmak için **Ekle** ' ye tıklayın.  
    ![bir kayıt hesabının hesap ekle kutusuna gerekli bilgileri girin](./media/assign-access-acm-data/add-account.png)

Yukarıdaki adımları tamamladıktan sonra kullanıcı hesabının Enterprise portal kayıt hesabı haline gelir ve abonelikleri oluşturabilirsiniz. Kullanıcı, oluşturdukları abonelikler için maliyet ve kullanım verileri erişebilir.

## <a name="assign-management-group-scope-access"></a>Yönetim grubu kapsamı erişim atama

Yönetim grubu kapsamını görüntüleme erişimi için en azından maliyet yönetimi okuyucusu (veya okuyucu) izni gerekir. Azure portalında bir yönetim grubu için izinleri yapılandırabilirsiniz. En az, diğerleri için erişimi etkinleştirmek yönetim grubu için kullanıcı erişimi Yöneticisi'ni (veya sahibi) iznine de sahip olmalıdır. Ayrıca, Azure EA hesaplarında de EA portalındaki **Ao görünüm ücretleri** ayarını etkinleştirmiş olmanız gerekir.

1. [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.
2. Yan çubukta **tüm hizmetler** ' i seçin, _Yönetim grupları_' nı arayın ve **Yönetim grupları**' nı seçin.
3. Yönetim grubu ve hiyerarşide seçebilirsiniz.
4. Yönetim grubunuzun adının yanındaki **Ayrıntılar**' a tıklayın.
5. Sol bölmeden **Access Control (IAM)** seçeneğini belirleyin.
6. **Ekle**'ye tıklayın.
7. **Rol**altında **maliyet yönetimi okuyucusu**' nu seçin.
8. **Erişim ata**' nın altında **Azure AD Kullanıcı, Grup veya uygulama**' yı seçin.
9. Erişim atamak için arama yapın ve ardından kullanıcıyı seçin.
10. **Save (Kaydet)** düğmesine tıklayın.  
    bir yönetim grubu için izin Ekle kutusunda örnek bilgi ![](./media/assign-access-acm-data/add-permissions.png)

## <a name="assign-subscription-scope-access"></a>Abonelik kapsamı erişim atama

Bir aboneliğe erişim, en az maliyet Yönetimi Okuyucu (veya Okuyucu) izni gerektirir. Azure portalında bir aboneliği üzerindeki izinler yapılandırabilirsiniz. En az, diğerleri için erişimi etkinleştirmek abonelik için kullanıcı erişimi Yöneticisi'ni (veya sahibi) iznine de sahip olmalıdır. Ayrıca, Azure EA hesaplarında de EA portalındaki **Ao görünüm ücretleri** ayarını etkinleştirmiş olmanız gerekir.

1. [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.
2. Yan çubukta **tüm hizmetler** ' i seçin, _abonelikler_' i arayın ve ardından **abonelikler**' i seçin.
3. Aboneliğinizi seçin.
4. Sol bölmeden **Access Control (IAM)** seçeneğini belirleyin.
5. **Ekle**'ye tıklayın.
6. **Rol**altında **maliyet yönetimi okuyucusu**' nu seçin.
7. **Erişim ata**' nın altında **Azure AD Kullanıcı, Grup veya uygulama**' yı seçin.
8. Erişim atamak için arama yapın ve ardından kullanıcıyı seçin.
9. **Save (Kaydet)** düğmesine tıklayın.

## <a name="assign-resource-group-scope-access"></a>Kaynak grubu kapsamı erişim atama

Bir kaynak grubuna erişim, en az maliyet Yönetimi Okuyucu (veya Okuyucu) izni gerektirir. Azure portalında bir kaynak grubu için izinleri yapılandırabilirsiniz. En az, diğerleri için erişimi etkinleştirmek kaynak grubu için kullanıcı erişimi Yöneticisi'ni (veya sahibi) iznine de sahip olmalıdır. Ayrıca, Azure EA hesaplarında de EA portalındaki **Ao görünüm ücretleri** ayarını etkinleştirmiş olmanız gerekir.

1. [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.
2. Yan çubukta **tüm hizmetler** ' i seçin, _kaynak grupları_' nı arayın ve **kaynak grupları**' nı seçin.
3. Kaynak grubunuzu seçin.
4. Sol bölmeden **Access Control (IAM)** seçeneğini belirleyin.
5. **Ekle**'ye tıklayın.
6. **Rol**altında **maliyet yönetimi okuyucusu**' nu seçin.
7. **Erişim ata**' nın altında **Azure AD Kullanıcı, Grup veya uygulama**' yı seçin.
8. Erişim atamak için arama yapın ve ardından kullanıcıyı seçin.
9. **Save (Kaydet)** düğmesine tıklayın.

## <a name="cross-tenant-authentication-issues"></a>Çapraz Kiracı kimlik doğrulama sorunları

Şu anda Azure maliyet yönetimi, çapraz Kiracı kimlik doğrulaması için sınırlı desteğe sahiptir. Kiracılar genelinde kimlik doğrulamaya çalıştığınızda bazı durumlarda, maliyet analizinde **erişim reddedildi** hatası alabilirsiniz. Bu sorun, rol tabanlı erişim denetimi 'ni (RBAC) başka bir kiracının aboneliğine yapılandırırsanız ve maliyet verilerini görüntülemeye çalışırsanız meydana gelebilir.

*Sorunu geçici olarak çözmek için*: çapraz kiracı RBAC yapılandırdıktan sonra bir saat bekleyin. Ardından, maliyet analizinde maliyetleri görüntülemeyi deneyin veya her iki Kiracıdaki kullanıcılara maliyet yönetimi erişimi verin.  


## <a name="next-steps"></a>Sonraki adımlar

- Maliyet yönetimi için ilk hızlı başlangıcı Henüz tamamlamadıysanız, [maliyetleri çözümlemeye başlamak](quick-acm-cost-analysis.md)için bunu okuyun.
