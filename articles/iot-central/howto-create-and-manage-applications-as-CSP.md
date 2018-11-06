---
title: Oluşturma ve CSP olarak Azure IOT Central uygulamaları yönetme | Microsoft Docs
description: Bir CSP olarak müşteri adına bir Azure IOT Central uygulaması oluşturmayı öğrenin.
services: iot-central
ms.service: iot-central
author: tbhagwat3
ms.author: tanmayb
ms.date: 10/29/2018
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: dbc72e040f3d68ca77c036c41612f57616d1e74e
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51016837"
---
# <a name="as-a-csp-create-and-manage-an-azure-iot-central-application-on-behalf-of-your-customer"></a>Bir CSP olarak oluşturun ve bir Azure IOT Central uygulamasına, müşteri adına yönetmek 

Microsoft bulut çözümü sağlayıcısı (CSP) programı bir Microsoft Reseller programdır. Konuşmanın niyetini tüm Microsoft ticari Çevrimiçi Hizmetleri satmak üzere kanal ortaklarımızla birlikte tek bir program sağlamaktır. Daha fazla bilgi edinin [bulut çözümü sağlayıcısı programı](https://partner.microsoft.com/cloud-solution-provider).

Bir CSP olarak oluşturabilir ve Microsoft Azure IOT Central uygulamaları müşterileriniz adına yönetme [Microsoft Partner Center](https://partnercenter.microsoft.com/partner/home). Azure IOT Central uygulamaları müşteriler adına CSP tarafından gibi diğer oluşturulduğunda CSP tarafından yönetilen Azure Hizmetleri, CSP müşterileri için faturalandırma yönetin. Azure IOT Central için ücret toplam faturanız Microsoft Partner Center'daki görünür.

Başlamak için hesabınızı bir Microsoft iş ortağı portalı için oturum açın ve Azure IOT Central bir uygulama oluşturmak istediğiniz bir müşteri seçin. Sol gezinti bölmesinde müşteri için Hizmet Yönetimi gidin

![Microsoft Partner Center, müşteri görünümü](media\howto-create-application-asCSP\image1.png)

Azure IOT Central yönetmek kullanılabilir hizmet olarak listelenir. Sayfasında, yeni uygulamalar oluşturabilir veya bu müşteri için mevcut uygulamaları yönetmek için Azure IOT Central bağlantıya tıklayın.

![Azure IOT Central yönetmek kullanılabilir](media\howto-create-application-asCSP\image2.png)

Azure IOT Central Uygulama Yöneticisi sayfasına gelirsiniz. Azure IOT Central, Microsoft Partner Center'dan gelen geldiğini ve müşterinin yönetmek için geldiğini bağlam tutar. Bu uygulama Yöneticisi sayfanın üst bilgisinde onaylanır görürsünüz. Buradan, ya da oluşturulan mevcut bir uygulamaya yönetin veya müşteri için yeni bir uygulama oluşturmak daha önce bu müşteri için gidebilirsiniz.

![CSP'ler için Yöneticisi oluşturma](media\howto-create-application-asCSP\image3.png)

Azure IOT Central bir uygulama oluşturmak için tıklayın **yeni uygulama** Döşe. Bu uygulama oluşturma sayfası yüklenir. Bu sayfadaki tüm alanları doldurun ve ardından **Oluştur**. Aşağıdaki alanların her biri hakkında daha fazla bilgi bulabilirsiniz.

![CSP'ler için uygulama sayfası oluşturma](media\howto-create-application-asCSP\image4.png)

![CSP'ler için uygulama sayfası oluşturma](media\howto-create-application-asCSP\image4-1.png)

## <a name="payment-plan"></a>Ödeme planı

CSP olarak yalnızca Kullandıkça Öde uygulamalar oluşturabilirsiniz. Azure IOT Central müşterinize göstermek için ayrı bir deneme uygulaması oluşturabilirsiniz. Deneme ve Kullandıkça Öde uygulamalar hakkında daha fazla bilgi [Azure IOT fiyatlandırma sayfası Merkezi](https://azure.microsoft.com/pricing/details/iot-central/).

## <a name="application-name"></a>Uygulama Adı

Uygulamanızın ad **Uygulama Yöneticisi** sayfası ve her bir Azure IOT Central uygulaması içinde. Azure IOT Central uygulamanız için herhangi bir ad seçebilirsiniz. Size ve diğer kullanıcılarla kuruluşunuzda anlamlı bir ad seçin.

## <a name="application-url"></a>Uygulama URL'si

Uygulama URL'si uygulamanıza bağlantıdır. Tarayıcınızda yer işareti kaydetmesi veya başkalarıyla paylaşabilirsiniz.

Uygulamanız için bir ad girin, uygulama URL'sini otomatik olarak oluşturulan olur. Tercih ederseniz, uygulamanız için farklı bir URL seçebilirsiniz. Her Azure IOT Central URL, Azure IOT Central içinde benzersiz olmalıdır. Seçtiğiniz URL önceden alınmış bir hata iletisi görürsünüz.

## <a name="directory"></a>Dizin

Azure IOT Central Microsoft iş ortağı Portalı'nda seçtiğiniz müşteri yönetmek için gelen bağlam olduğundan, yalnızca Azure Active Directory kiracısı dizin alanında o müşteri için bkz. 

Azure Active Directory kiracısı, kullanıcı kimliklerini, kimlik bilgilerini ve diğer kuruluş bilgilerini içerir. Birden çok Azure aboneliği, tek bir Azure Active Directory kiracısı ile ilişkili olabilir.

Daha fazla bilgi için bkz. [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).

## <a name="azure-subscription"></a>Azure aboneliği

Bir Azure aboneliği, Azure Hizmetleri örneklerini oluşturmanıza olanak sağlar. Azure IOT Central otomatik olarak erişim tüm Azure abonelikleri olan müşterinin bulur ve bunları açılır menüde görüntüler **uygulama oluşturma** sayfası. Yeni bir Azure IOT Central uygulaması oluşturmak için bir Azure aboneliği seçin.

Azure aboneliğiniz yoksa, Microsoft Partner Center'daki oluşturabilirsiniz. Azure abonelik oluşturduktan sonra geri gidin **uygulama oluşturma** sayfası. Yeni aboneliğinizi görünür **Azure abonelik** açılır.

Daha fazla bilgi için bkz. [Azure abonelikleri](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing).

## <a name="region"></a>Bölge

Azure IOT Central uygulaması oluşturmak için istediğiniz bölgeyi seçin. Genellikle, en iyi performansı elde etmek için fiziksel olarak cihazlarınıza en yakın bölgeyi seçmeniz gerekir.

Daha fazla bilgi için bkz. [Azure bölgeleri](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#azure-regions).

Azure IOT Central olduğu kullanılabilir bölgeler gördüğünüz [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/) sayfası.

> [!Note]
> Bir bölge seçtikten sonra daha sonra uygulamanızı farklı bir bölgeye taşıyamazsınız.

## <a name="application-template"></a>Uygulama şablonu

Yeni Azure IOT Central uygulamanız için mevcut uygulama şablonlarından birini seçebilirsiniz. Bir uygulama şablonunu gibi cihaz şablonları önceden tanımlanmış öğeleri içerebilir ve yardımcı olması için panoları kullanmaya başlayın.

| Uygulama şablonu | Açıklama |
| -------------------- | ----------- |
| Özel uygulama   | Kendi cihaz şablonlarını ve cihazlarla doldurmak, boş bir uygulama oluşturur. |
| Örnek Contoso       | Basit bağlı bir cihaz için cihaz şablon içeren bir uygulama oluşturur. Azure IOT Central'ı keşfetmeye başlamak için bu şablonu kullanın. |
| Örnek Devkits       | Uygulama cihaz şablonları ile MXChip veya Raspberry Pi bir aygıt bağlanmaya hazır oluşturur. Cihaz geliştiriciyseniz bu cihaz üzerinde kodla denemeler bu şablonu kullanın. |

## <a name="next-steps"></a>Sonraki adımlar

Bir CSP olarak bir Azure IOT Central uygulaması oluşturmayı öğrendiniz, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Uygulamanızı yönetme](howto-administer.md)