---
title: Önceki sürümden farklılıklar
titleSuffix: Azure Digital Twins
description: Yeni Azure dijital TWINS sürümünde nelerin değiştiğini anlayın
author: baanders
ms.author: baanders
ms.date: 3/12/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 6d1786766fc61ed0edfdc53295a50ba212818fc0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89181434"
---
# <a name="what-is-the-new-azure-digital-twins-how-is-it-different-from-the-previous-version-2018"></a>Yeni Azure dijital TWINS nedir? Önceki sürümden (2018) farklı mi?

Azure Digital TWINS 'in ilk genel önizlemesi Ekim 2018 ' de yayımlanmıştır. Bu önceki sürümden temel kavramlar, artık genel önizlemede bulunan yeni hizmete devredilirken, arabirimlerin ve uygulama ayrıntılarının birçoğu hizmeti daha esnek ve erişilebilir hale getirmek için değiştirilmiştir. Bu değişiklikler müşteri geri bildirimleri tarafından görüşmiş.

> [!IMPORTANT]
> Yeni hizmetin genişletilmiş özelliklerine yönelik olarak, önceki Azure dijital TWINS hizmeti 2020 yılının sonuna kadar kullanımdan kaldırılacaktır.

Önceki genel önizleme sırasında Azure Digital TWINS 'in ilk sürümünü kullandıysanız, yeni hizmetle nasıl çalışacağınızı öğrenmek ve özelliklerinden yararlanmak için bu makaledeki bilgileri ve en iyi uygulamaları kullanın.

## <a name="differences-by-topic"></a>Konuya göre farklılıklar

Aşağıdaki grafik, hizmetin önceki sürümü ve yeni (geçerli) hizmeti arasında değişen kavramların yan yana görünümünü sağlar.

| Konu | Önceki sürümde | Yeni sürümde |
| --- | --- | --- | --- |
| **Modelleme**<br>*Daha esnek* | Önceki sürüm akıllı boşluklar etrafında tasarlanmıştı, bu nedenle bina için yerleşik bir sözlük ile birlikte geldi. | Yeni Azure dijital TWINS etki alanı belirsiz. Çözümünüz için kendi özel sözlüğünüzü ve özel modellerinizi tanımlayabilir ve daha esnek yollarla daha fazla ortam temsil edebilirsiniz.<br><br>Kavramlar hakkında daha fazla bilgi edinin [*: özel modeller*](concepts-models.md). |
| **Topoloji**<br>*Daha esnek*| Önceki sürüm, akıllı alanlara uyarlanmış bir ağaç veri yapısını destekliyordu. Dijital TWINS Hiyerarşik ilişkilerle bağlandı. | Yeni sürümle birlikte, dijital ikgörüistemler, sizin istediğiniz şekilde düzenlenmiş, rastgele grafik Topolojilerine bağlanabilir. Bu, gerçek dünyanın karmaşık ilişkilerini ifade etmek için size daha fazla esneklik sağlar.<br><br>Kavramlar hakkında daha fazla bilgi edinin [*: dijital TWINS ve ikizi grafiği*](concepts-twins-graph.md). |
| **İşlem**<br>*Daha zengin, daha esnek* | Önceki sürümde, olay ve telemetri işleme mantığı JavaScript Kullanıcı tanımlı işlevlerde (UDF 'ler) tanımlanmıştır. UDF 'ler ile hata ayıklama sınırlandı. | Yeni yayında açık bir işlem modeli vardır: [Azure işlevleri](../azure-functions/functions-overview.md)gibi dış işlem kaynaklarını ekleyerek özel mantık sağlarsınız. Bu, tercih ettiğiniz bir programlama dilini kullanmanıza, kısıtlama olmadan özel kod kitaplıklarına erişebilmenize ve dış hizmetin sahip olabileceği geliştirme ve hata ayıklama kaynaklarından faydalanabilir.<br><br>[*Nasıl yapılır: verileri işlemek için bir Azure Işlevi ayarlama hakkında*](how-to-create-azure-function.md)daha fazla bilgi edinin. |
| **IoT Hub ile cihaz yönetimi**<br>*Daha erişilebilir* | Önceki sürüme dağıtım, Azure dijital TWINS hizmeti için dahili bir [IoT Hub](../iot-hub/about-iot-hub.md) örneğiyle birlikte yönetilir. Bu tümleşik hub, geliştiriciler için tam olarak erişilebilir değildi. | Yeni sürümde, bağımsız olarak oluşturulan bir IoT Hub örneği (daha önce yönettiği tüm cihazlarla birlikte) ekleyerek kendi IoT Hub 'ınızı "kendi" getirin. Bu, IoT Hub özelliklerine tam erişim sağlar ve sizi cihaz yönetiminin denetimine geçirir.<br><br>[*IoT Hub 'Dan nasıl yapılır: alma telemetrisi*](how-to-ingest-iot-hub-data.md)hakkında daha fazla bilgi edinin. |
| **Güvenlik**<br>*Daha fazla standart* | Önceki sürümde, örneğinizin erişimini yönetmek için kullanabileceğiniz önceden tanımlanmış roller vardı. | Yeni sürüm, diğer Azure hizmetlerinin kullandığı aynı [Azure rol tabanlı erişim denetimi (Azure RBAC)](../role-based-access-control/overview.md) arka uç hizmeti ile tümleşir. Bu, çözümünüzde IoT Hub, Azure Işlevleri, Event Grid gibi diğer Azure hizmetleri arasında kimlik doğrulaması yapmayı daha kolay hale getirir.<br>RBAC ile önceden tanımlanmış rolleri kullanmaya devam edebilir veya özel roller oluşturabilir ve yapılandırabilirsiniz.<br><br>Kavramlar hakkında daha fazla bilgi edinin [*: Azure dijital TWINS çözümleri Için güvenlik*](concepts-security.md). |
| **Ölçeklenebilirlik**<br>*İlerisi* | Önceki sürümde cihazlar, iletiler, grafikler ve ölçek birimleri için ölçek sınırlamaları vardı. Abonelik başına yalnızca bir Azure dijital TWINS örneği desteklenir.  | Yeni sürüm, geliştirilmiş ölçeklenebilirlik ile yeni bir mimariye dayanır ve daha fazla işlem gücü içerir. Ayrıca, her abonelik için bölge başına 10 örnek destekler.<br><br>Şu anda genel önizlemede bulunan limitlerin ayrıntıları için bkz. [*Başvuru: genel önizleme hizmeti sınırları*](reference-service-limits.md) . |

## <a name="service-limits-in-public-preview"></a>Hizmet sınırları genel önizlemede

Bu genel önizleme sırasında Azure dijital TWINS sınırları listesi için bkz. [*Başvuru: genel önizleme hizmeti sınırları*](reference-service-limits.md).

## <a name="next-steps"></a>Sonraki adımlar

Ardından, ilk öğreticiyle Azure dijital ikgörüler ile çalışmaya hemen başlayabilirsiniz:

[*Öğretici: istemci uygulamasını kodlayın*](tutorial-code.md)