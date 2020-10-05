---
title: Azure Güvenlik Merkezi 'nde iş akışı Otomasyonu | Microsoft Docs
description: Azure Güvenlik Merkezi 'nde iş akışları oluşturmayı ve otomatikleştirmeyi öğrenin
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 09/22/2020
ms.author: memildin
ms.openlocfilehash: b713977d811411ea2ccd7dfa22c7757321ecd7aa
ms.sourcegitcommit: 5b69ba21787c07547edfbfd5254eaf34315cfadd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91712298"
---
# <a name="create-automatic-responses-to-alerts-and-recommendations-with-workflow-automation"></a>İş akışı otomasyonu ile uyarılara ve önerilere otomatik yanıtlar oluşturma

Her güvenlik programı, olay yanıtı için birden çok iş akışı içerir. Bu işlemler ilgili hissedarları bildirmeyi, bir değişiklik yönetimi işlemini başlatmayı ve belirli düzeltme adımlarını uygulamayı içerebilir. Güvenlik uzmanları, yaptığınız gibi bu yordamların pek çok adımını otomatikleştirmenizi önerir. Otomasyon ek yükü azaltır. Ayrıca işlem adımlarının hızlı, tutarlı bir şekilde ve önceden tanımlanmış gereksinimlerinize göre hızla yapılmasını sağlayarak güvenliği geliştirebilir.

Bu makalede, Azure Güvenlik Merkezi 'nin iş akışı Otomasyonu özelliği açıklanır. Bu özellik güvenlik uyarıları ve önerileri üzerinde Logic Apps tetiklenebilir. Örneğin, bir uyarı oluştuğunda Güvenlik Merkezi 'nin belirli bir kullanıcıyı e-posta ile e-posta olarak istemeniz gerekebilir. Ayrıca, [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview)kullanarak Logic Apps oluşturmayı öğreneceksiniz.

> [!NOTE]
> Daha önce kenar çubuğunda playbooks (Önizleme) görünümünü kullandıysanız, yeni iş akışı Otomasyonu sayfasında genişletilmiş işlevlerle aynı özellikleri bulacaksınız.



## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Genel olarak kullanılabilir (GA)|
|Fiyat|Ücretsiz|
|Gerekli roller ve izinler:|Kaynak grubundaki **Güvenlik Yöneticisi rolü** veya **sahibi**<br>Ayrıca hedef kaynak için yazma izinlerine sahip olmalıdır<br><br>Azure Logic Apps iş akışlarıyla çalışmak için aşağıdaki Logic Apps rollere/izinlere de sahip olmanız gerekir:<br> - [Mantıksal uygulama işleci](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#logic-app-operator) izinleri gerekiyor veya mantıksal uygulama okuma/tetikleme erişimi (Bu rol, mantıksal uygulamalar oluşturamaz veya düzenleyemez; yalnızca var olanları *Çalıştır* )<br> - Mantıksal uygulama oluşturma ve değiştirme için [mantıksal uygulama katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#logic-app-contributor) izinleri gereklidir<br>Logic App bağlayıcıları kullanmak istiyorsanız, ilgili hizmetlerinde oturum açmak için ek kimlik bilgilerine ihtiyacınız olabilir (örneğin, Outlook/takımlar/bolluk örnekleri)|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![Yes](./media/icons/yes-icon.png) Ulusal/Sogeign (US Gov, Çin gov, diğer gov)|
|||



## <a name="create-a-logic-app-and-define-when-it-should-automatically-run"></a>Bir mantıksal uygulama oluşturun ve ne zaman otomatik olarak çalışacağını tanımlayın 

1. Güvenlik Merkezi 'nin kenar çubuğundan **Iş akışı Otomasyonu**' nu seçin.

    :::image type="content" source="./media/workflow-automation/list-of-workflow-automations.png" alt-text="İş akışı otomasyonları listesi&quot;:::

    Bu sayfadan yeni otomasyon kuralları oluşturabilir, Ayrıca var olanları etkinleştirebilir, devre dışı bırakabilir veya silebilirsiniz.

1. Yeni bir iş akışı tanımlamak için **iş akışı Otomasyonu Ekle**' ye tıklayın. 

    Yeni otomasyonuna yönelik seçenekleri içeren bir bölme açılır. Buraya şunu girebilirsiniz:
    1. Otomasyon için bir ad ve açıklama.
    1. Bu otomatik iş akışını başlatacak Tetikleyiciler. Örneğin, &quot;SQL" içeren bir güvenlik uyarısı oluşturulduğunda mantıksal uygulamanızın çalışmasını isteyebilirsiniz.
    1. Tetikleyici koşullarınız karşılandığında çalıştırılacak mantıksal uygulama. 

        :::image type="content" source="./media/workflow-automation/add-workflow.png" alt-text="İş akışı otomasyonları listesi&quot;:::

    Bu sayfadan yeni otomasyon kuralları oluşturabilir, Ayrıca var olanları etkinleştirebilir, devre dışı bırakabilir veya silebilirsiniz.

1. Yeni bir iş akışı tanımlamak için **iş akışı Otomasyonu Ekle**' ye tıklayın. 

    Yeni otomasyonuna yönelik seçenekleri içeren bir bölme açılır. Buraya şunu girebilirsiniz:
    1. Otomasyon için bir ad ve açıklama.
    1. Bu otomatik iş akışını başlatacak Tetikleyiciler. Örneğin, &quot;SQL":::

1. Mantıksal uygulama oluşturma işlemini başlatmak için Eylemler bölümünden **Yeni bir tane oluştur ' a** tıklayın.

    Azure Logic Apps için yönlendirilirsiniz.

    [![Yeni bir mantıksal uygulama oluşturma](media/workflow-automation/logic-apps-create-new.png)](media/workflow-automation/logic-apps-create-new.png#lightbox)

1. Bir ad, kaynak grubu ve konum girin ve **Oluştur**' a tıklayın.

1. Yeni mantıksal uygulamanızda, güvenlik kategorisinden yerleşik, önceden tanımlanmış şablonlar arasından seçim yapabilirsiniz. Ya da bu işlem tetiklendiğinde gerçekleşecek özel olay akışını tanımlayabilirsiniz.

    Mantıksal uygulama tasarımcısında, güvenlik merkezi bağlayıcılarından aşağıdaki Tetikleyiciler desteklenir:

    * **Bir Azure Güvenlik Merkezi önerisi oluşturulduğunda veya tetiklendiğinde** -mantıksal uygulamanız kullanım dışı veya değiştirilmiş bir öneriyi kullanıyorsa, Otomasyon çalışmayı durdurur ve tetikleyiciyi güncelleştirmeniz gerekir. Önerilerle ilgili değişiklikleri izlemek için bkz. [Azure Güvenlik Merkezi sürüm notları](release-notes.md).

    * **Bir Azure Güvenlik Merkezi uyarısı oluşturulduğunda veya tetiklendiğinde** , tetikleyiciyi yalnızca ilgilendiğiniz önem düzeyleri ile ilgili uyarılarla ilişkili olacak şekilde özelleştirebilirsiniz.
    
    > [!NOTE]
    > Eski tetikleyiciyi kullanıyorsanız "bir Azure Güvenlik Merkezi uyarısıyla bir yanıt tetiklendiğinde", Logic Apps Iş akışı Otomasyonu özelliği tarafından başlatılmaz. Bunun yerine, yukarıda bahsedilen tetikleyicilerden birini kullanın. 

    [![Örnek mantıksal uygulama](media/workflow-automation/sample-logic-app.png)](media/workflow-automation/sample-logic-app.png#lightbox)

1. Mantıksal uygulamanızı tanımladıktan sonra, iş akışı Otomasyon tanımı bölmesine ("iş akışı Otomasyonu Ekle") dönün. Yeni mantıksal uygulamanızın seçim için kullanılabilir olduğundan emin olmak için **Yenile** ' ye tıklayın.

    ![Yenile](media/workflow-automation/refresh-the-list-of-logic-apps.png)

1. Mantıksal uygulamanızı seçin ve Otomasyonu kaydedin. Mantıksal uygulama açılan listesinin yalnızca yukarıda bahsedilen destekleyici Güvenlik Merkezi bağlayıcıları olan Logic Apps gösterdiğini unutmayın.


## <a name="manually-trigger-a-logic-app"></a>Mantıksal uygulamayı el ile tetikleme

Ayrıca, herhangi bir güvenlik uyarısını veya öneriyi görüntülerken Logic Apps el ile de çalıştırabilirsiniz.

Bir mantıksal uygulamayı el ile çalıştırmak için bir uyarı veya öneri açın ve **mantıksal uygulamayı Tetikle**' ye tıklayın:

[![Mantıksal uygulamayı el ile tetikleme](media/workflow-automation/manually-trigger-logic-app.png)](media/workflow-automation/manually-trigger-logic-app.png#lightbox)

## <a name="data-types-schemas"></a>Veri türleri şemaları

Mantıksal uygulama örneğine geçirilen güvenlik uyarıları veya öneriler olaylarının Ham olay şemalarını görüntülemek için, [Iş akışı Otomasyonu veri türleri şemalarını](https://aka.ms/ASCAutomationSchemas)ziyaret edin. Bu, yukarıda bahsedilen güvenlik merkezi 'nin yerleşik Logic Apps bağlayıcılarını kullanmadığınız, ancak mantıksal uygulamanın genel HTTP bağlayıcısını kullanan durumlarda yararlı olabilir-olay JSON şemasını, uygun gördüğünüz şekilde el ile ayrıştırmanız için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede Logic Apps oluşturma, güvenlik merkezi 'nde yürütmesini otomatikleştirme ve bunları el ile çalıştırma hakkında bilgi edindiniz. 

İlgili malzemeler için bkz.: 

- [Bir güvenlik yanıtını otomatikleştirmek için iş akışı Otomasyonu 'nu kullanma hakkında Microsoft Learn modülü](https://docs.microsoft.com/learn/modules/resolve-threats-with-azure-security-center/)
- [Azure Güvenlik Merkezi'nde güvenlik önerileri](security-center-recommendations.md)
- [Azure Güvenlik Merkezi'nde güvenlik uyarıları](security-center-alerts-overview.md)
- [Azure Logic Apps hakkında](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview)
- [Logic Apps bağlayıcıları](https://docs.microsoft.com/connectors/)
- [İş akışı Otomasyonu veri türleri şemaları](https://aka.ms/ASCAutomationSchemas)
