---
title: Azure Notebooks önizlemede oturum açın
description: Azure bir Microsoft hesabı kullanarak not defterleri için kullanıcı hesabınız ya da bir iş/Okul hesabı yapılandırın.
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: 9401a9e483f3f8db5af827f53937cb0f01b27a28
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75646305"
---
# <a name="your-user-account-for-azure-notebooks-preview"></a>Azure Notebooks önizlemesi için Kullanıcı hesabınız

Azure not defterleri ile veya bir kullanıcı hesabıyla oturum açmadan kullanabilirsiniz:

- Oturum açma olmadan oluşturabilirsiniz ve not defterlerini çalıştırmak ancak not defterlerini veya veri dosyaları projelerin bir parçası olarak tutamayacağınızı. Bir Azure not defteri için bağlantıya kullanıcılar oturum açmaya gerek olmadan Not Defteri gibi keyfini çıkarabilirsiniz.
- Oturum açtığınızda Azure not defterleri hesabınız ile tüm projeleriniz korur. Oturum açmış kullanıcıların, projeleri ve Not Defterleri diğer kullanıcılarla paylaşmasına izin veren bir kullanıcı kimliği de var.
- Azure not defterleri için kullanılan hesap ayrıca bir Azure aboneliği ile ilişkili olduğunda, daha güçlü sunucularında çalışan not defterleri gibi ek avantajlar elde özel not defterleri oluşturma ve bireysel kullanıcılara dizüstü bilgisayarlar izinleri veriliyor.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

Azure not defterlerine imzalama Microsoft Account ya da bir "İş veya Okul" hesabı gerektirir. Hesabınız için seçerken istenir **oturum** not defterlerini sayfanın üst sağ taraftaki komutu:

![Komutu Azure not defterleri için oturum açın.](media/accounts/sign-in-command.png)

Azure not defterlerinde yaptığınız tüm iş oturum açmak için kullandığınız hesap ile ilişkilidir. Her hesap ayrıca benzersiz bir kullanıcı kimliği olması gerekir, [kullanıcı profili](azure-notebooks-user-profile.md). Sonuç olarak, projeleri ve ayrı kimlikler ayrı kümesini korumanız gerekiyorsa Azure not defterlerine farklı hesaplarla oturum açabilirsiniz. Örneğin, her bir veri bilimi takım üyesi hem tek tek hesapları yanı sıra, şirket dışındaki kişilere işlerini sunmak için kullandıkları paylaşılan grup hesabı olarak olabilir. Eğitmenler, benzer şekilde, öğretim rollerine dış danışmanlık veya açık kaynak çalışmalarını kullanılan hesaptan farklı bir hesabı tutabilir.

## <a name="microsoft-accounts"></a>Microsoft hesapları

Microsoft hesapları, Microsoft ürünleri ve Hizmetleri, Windows, Azure, outlook.com, OneDrive ve XBox Live gibi herhangi bir sayıda oturum açmak için kullanılır. Bu hizmetlerin birini kullanıyorsanız, zaten Azure not defterleri ile kullanabileceğiniz Microsoft Account olması olasıdır.

Emin değilseniz, seçin **oluşturmak bir** hesabı isteminde komutu. Herhangi bir sağlayıcıdan gelen herhangi bir e-posta adresini kullanarak yeni bir Microsoft hesabı oluşturabilirsiniz.

![Yeni bir Microsoft hesabı oluşturmak için komutu](media/accounts/create-new-microsoft-account.png)

> [!Note]
> Zaten onunla ilişkili bir hesabı olan bir e-posta adresine sahip yeni bir hesap oluşturmaya çalışırsanız, "bir iş veya okul e-posta adresi ile burada kayıt yapamazsınız" iletisini görebilirsiniz. Gmail veya Yahoo! gibi kişisel bir e-posta kullanın veya yeni bir Outlook e-postası alın. " Bu durumda, yeni bir hesap oluşturmadan iş e-posta adresiyle oturum açmayı deneyin.

Çocuk hesapları için varsayılan olarak Azure not defterleri için erişim engellenir. Bir alt hesapla oturum açma, aşağıda gösterilen hata görüntüler:

![Bir alt hesabıyla oturum çalışılırken hata oluştu: bir sorun oluştu, ana erişimi engelledi.](media/accounts/child-account-error.png)

Bir üst erişimi etkinleştirmek için aşağıdakileri yapmanız gerekir:

1. Ziyaret `https://account.live.com/mk` ve üst hesapla oturum açın.
1. Söz konusu alt bölümünde seçin **çocuğunuzun üçüncü taraf uygulamalara erişimi yönetme**.
1. Sonraki sayfada seçin **erişimi etkinleştirme**.
1. Çocuk hesabı, sonraki Azure not defterlerine oturum kullanıldığında seçin **Evet** görünen izinleri istemi.

> [!Warning]
> Azure not defterleri için üçüncü taraf uygulamalarına erişimin etkinleştirilmesine yönelik tüm üçüncü taraf uygulamalar için erişim sağlar. Üst öğeleri önerilir etkinleştirilirken takdirine bağlı olarak kullanılacak erişimi ve kendi alt etkinlik daha yakından izlemek isteyebilir.

## <a name="work-or-school-accounts"></a>İş veya okul hesapları

Bir iş veya Okul hesabı, bir kuruluşun Yöneticisi gibi Office 365 ve Windows etki alanına katılmış bir bilgisayarda oturum açmak için bir hesap olarak Microsoft bulut hizmetlerine erişmek kuruluşunuzun bir üyesi tarafından oluşturulur. Bir iş veya Okul hesabı genellikle bir kuruluş e-posta adresi gibi kullanan any-user@contoso.com.

Azure not defterleri toplar veya kullanır (ancak ifşa etmeyeceğiz olduğundan) ve hesabın e-posta adresi gibi bilgileri kullanıcının tarayıcı bilgileri Azure not defterleri ile bir iş veya Okul hesabıyla oturum yönetici onayı gerektirebilir. (Tarayıcı verilerini özellikleri popüler kullanım göre iyileştirmek için kullanılır.)

Bir kurumsal Hesap Yöneticisi, kullanıcılar tek tek verme konusunda çekince kısıtlı ise kullanıcılar adına izin sağlamanız gerekir. Bu durumda, kullanıcılar, "Bu uygulamaya erişemezsiniz" iletisini görürsünüz:

![Bir iş veya Okul hesabı kullanılırken "Bu uygulamaya erişemezsiniz" iletisi](media/accounts/consent-permissions-denied.png)

Bir yönetici onayı sağlamak için kullanın [yönetici onayı sayfası](https://notebooks.azure.com/account/adminConsent), yol gösterir sürecinde.

## <a name="next-steps"></a>Sonraki adımlar  

> [!div class="nextstepaction"]
> [Profil ve kullanıcı Kimliğinizi Düzenle](azure-notebooks-user-profile.md)
