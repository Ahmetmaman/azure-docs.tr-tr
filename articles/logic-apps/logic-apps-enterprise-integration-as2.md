---
title: B2B için AS2 iletileri gönderme ve alma
description: Enterprise Integration Pack ile Azure Logic Apps kullanarak B2B kurumsal tümleştirme senaryoları için Exchange AS2 iletileri
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 10/08/2020
ms.openlocfilehash: 1845de8332287f85e96b4e29c40caa66cb8704d9
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "91856679"
---
# <a name="exchange-as2-messages-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>Enterprise Integration Pack ile Azure Logic Apps'te AS2 kurumsal tümleştirme için AS2 iletilerini gönderme ve alma

> [!IMPORTANT]
> Özgün AS2 Bağlayıcısı kullanım dışı olduğundan, izleme özelliklerine ihtiyacınız yoksa bunun yerine **AS2 (v2)** bağlayıcısını kullanın. İzleme haricinde, v2 Bağlayıcısı özgün sürümle aynı özellikleri sağlar, Logic Apps çalışma zamanına yereldir ve ileti boyutu, aktarım hızı ve gecikme süresi içinde önemli performans iyileştirmeleri vardır. Ayrıca, v2 Bağlayıcısı tümleştirme hesabınıza bir bağlantı oluşturmanızı gerektirmez. Bunun yerine, Önkoşullar bölümünde açıklandığı gibi, tümleştirme hesabınızı bağlayıcıyı kullanmayı planladığınız Logic App 'e bağladığınızdan emin olun.

Azure Logic Apps 'de AS2 iletilerle çalışmak için, AS2 (sürüm 1,2) iletişimini destekleyen ve yöneten Tetikleyiciler ve eylemler sağlayan AS2 bağlayıcısını kullanabilirsiniz. Örneğin, iletileri aktarırken güvenlik ve güvenilirlik sağlamak için şu işlemleri kullanabilirsiniz:

* [ **AS2 kodlama**](#encode) , dijital Imzalama ve ileti değerlendirme bildirimleri (MDN) aracılığıyla, geçiş dışı desteği sağlamaya yardımcı olan bir kodlama eylemi. Örneğin, bu eylem AS2/HTTP üstbilgilerini uygular ve yapılandırıldığında bu görevleri gerçekleştirir:

  * Giden iletileri imzalar.
  * Giden iletileri şifreler.
  * İletiyi sıkıştırır.
  * Dosya adını MIME üstbilgisinde iletir.

* Ileti değerlendirmesi bildirimleri (MDN) aracılığıyla şifre çözme, dijital imzalama ve bildirim sağlamak için [ **AS2 kod çözme** eylemi](#decode) . Örneğin, bu eylem şu görevleri gerçekleştirir:

  * AS2/HTTP üst bilgilerini işler.
  * Uzlaştırdığı, özgün giden iletilerle MDNs aldı.
  * Red olmayan veritabanındaki kayıtları güncelleştirir ve bunları karşılıklı olarak ilişkilendirir.
  * AS2 durum raporlaması için kayıtlar yazar.
  * Yük içeriğini Base64 kodlamalı olarak verir.
  * MDNs gerekip gerekmediğini belirler. AS2 sözleşmesine bağlı olarak, MDNs 'nin zaman uyumlu veya zaman uyumsuz olup olmayacağını belirler.
  * AS2 sözleşmesine göre zaman uyumlu veya zaman uyumsuz MDNs oluşturur.
  * MDNs üzerindeki bağıntı belirteçlerini ve özellikleri ayarlar.

  Bu eylem, yapılandırıldığında da bu görevleri gerçekleştirir:

  * İmzayı doğrular.
  * İletilerin şifresini çözer.
  * İletiyi açar.
  * İleti KIMLIĞI yinelemelerini denetleyin ve izin vermeyin.

Bu makalede, AS2 Encoding ve kod çözme eylemlerinin mevcut bir mantıksal uygulamaya nasıl ekleneceği gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Henüz bir Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* AS2 bağlayıcısını kullanmak istediğiniz mantıksal uygulama ve mantıksal uygulamanızın iş akışını başlatan bir tetikleyici. AS2 Bağlayıcısı, Tetikleyiciler değil yalnızca eylemler sağlar. Logic Apps 'e yeni başladıysanız, [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: Ilk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)' yı gözden geçirin.

* Azure aboneliğinizle ilişkili ve AS2 bağlayıcısını kullanmayı planladığınız mantıksal uygulamaya bağlı olan bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) . Hem mantıksal uygulamanız hem de tümleştirme hesabınız aynı konumda veya Azure bölgesinde bulunmalıdır.

* AS2 Identity niteleyicisi kullanarak, tümleştirme hesabınızda zaten tanımlamış olduğunuz en az iki [ticari iş ortağı](../logic-apps/logic-apps-enterprise-integration-partners.md) .

* AS2 bağlayıcısını kullanabilmeniz için, ticari iş ortaklarınız arasında bir AS2 [sözleşmesi](../logic-apps/logic-apps-enterprise-integration-agreements.md) oluşturmanız ve bu sözleşmeyi tümleştirme hesabınızda depolamanız gerekir.

* Sertifika yönetimi için [Azure Key Vault](../key-vault/general/overview.md) kullanıyorsanız, kasa anahtarlarınızın **şifreleme** ve **şifre çözme** işlemlerine izin verdiğinden emin olun. Aksi takdirde, kodlama ve kod çözme eylemleri başarısız olur.

  Azure portal, anahtar kasasındaki anahtara gidin, anahtarın **Izin verilen işlemlerini** gözden geçirin ve **şifreleme** ve **şifre çözme** işlemlerinin seçili olduğunu onaylayın, örneğin:

  ![Kasa anahtarı işlemlerini denetleme](media/logic-apps-enterprise-integration-as2/key-vault-permitted-operations.png)

<a name="encode"></a>

## <a name="encode-as2-messages"></a>AS2 iletilerini kodlama

1. Henüz yapmadıysanız, [Azure Portal](https://portal.azure.com)mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

1. Tasarımcıda mantıksal uygulamanıza yeni bir eylem ekleyin.

1. **Eylem Seç** ' in altında, arama kutusu ' nun altında **Tümü**' nü seçin. Arama kutusuna "AS2 encode" yazın ve AS2 (v2) eylemini seçtiğinizden emin olun: **As2 kodlama**

   !["AS2 encode" öğesini seçin](./media/logic-apps-enterprise-integration-as2/select-as2-encode.png)

1. Şimdi şu özellikler için bilgi sağlayın:

   | Özellik | Açıklama |
   |----------|-------------|
   | **Kodlanacak ileti** | İleti yükü |
   | **AS2** | AS2 anlaşmanız tarafından belirtilen şekilde ileti göndericisinin tanımlayıcısı |
   | **AS2** | İleti alıcısının AS2 anlaşmanız tarafından belirtilen tanımlayıcısı |
   |||

   Örnek:

   ![İleti kodlama özellikleri](./media/logic-apps-enterprise-integration-as2/as2-message-encoding-details.png)

> [!TIP]
> İmzalı veya şifrelenmiş iletiler gönderirken sorunlarla karşılaşırsanız, farklı SHA256 algoritma biçimlerini denemeyi deneyin. AS2 belirtimi SHA256 biçimleri hakkında herhangi bir bilgi sağlamaz, bu nedenle her sağlayıcı kendi uygulamasını veya biçimini kullanır.

<a name="decode"></a>

## <a name="decode-as2-messages"></a>AS2 iletilerinin kodunu çözme

1. Henüz yapmadıysanız, [Azure Portal](https://portal.azure.com)mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

1. Tasarımcıda mantıksal uygulamanıza yeni bir eylem ekleyin.

1. **Eylem Seç** ' in altında, arama kutusu ' nun altında **Tümü**' nü seçin. Arama kutusuna "AS2 kodunu çöz" yazın ve AS2 (v2) eylemini seçtiğinizden emin olun: **As2 kod çözme**

   !["AS2 kodunu çöz" seçeneğini belirleyin](media/logic-apps-enterprise-integration-as2/select-as2-decode.png)

1. **Kodlanacak ileti** ve **ileti üstbilgileri** özellikleri için önceki tetikleyiciden veya eylem çıktılarından bu değerleri seçin.

   Örneğin, mantıksal uygulamanızın iletileri bir Istek tetikleyicisi aracılığıyla aldığını varsayalım. Bu tetikleyiciden gelen çıktıları seçebilirsiniz.

   ![Istek çıktılarından gövde ve üst bilgi seçin](media/logic-apps-enterprise-integration-as2/as2-message-decoding-details.png)

## <a name="sample"></a>Örnek

Tam işletimsel bir mantıksal uygulamayı ve örnek AS2 senaryosunu dağıtmaya çalışmak için [AS2 Logic App şablonu ve senaryosuna](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/)bakın.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bu bağlayıcı hakkında, bağlayıcının Swagger dosyasında açıklanan eylemler ve sınırlamalar gibi daha teknik ayrıntılar için [bağlayıcının başvuru sayfasına](/connectors/as2/)bakın. 

> [!NOTE]
> Bir [tümleştirme hizmeti ortamındaki (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)Logic Apps için, bu bağlayıcının Ise etiketli sürümü [Ise için B2B ileti sınırlarını](../logic-apps/logic-apps-limits-and-config.md#b2b-protocol-limits)kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
