---
title: Güvenlik duvarı kurallarını yönetme-hiper ölçek (Citus)-PostgreSQL için Azure veritabanı
description: Azure portal kullanarak PostgreSQL için Azure veritabanı-hiper ölçek (Citus) için güvenlik duvarı kuralları oluşturun ve yönetin
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 9/11/2020
ms.openlocfilehash: dadd04497eae0e91bdf5ea3caad38beda35f7fa3
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91275430"
---
# <a name="manage-firewall-rules-for-azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL için Azure veritabanı 'nın güvenlik duvarı kurallarını yönetme-hiper ölçek (Citus)
Sunucu düzeyinde güvenlik duvarı kuralları, belirtilen IP adreslerinden veya IP adresi aralığından bir Hyperscale (Citus) düzenleyici düğümüne erişimi yönetmek için kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda ilerlemek için şunlar gerekir:
- Bir sunucu grubu [PostgreSQL Için Azure veritabanı – Hyperscale (Citus) sunucu grubu oluşturun](quickstart-create-hyperscale-portal.md).

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Azure portalında sunucu düzeyinde bir güvenlik duvarı kuralı oluşturma

> [!NOTE]
> Bu ayarlar, PostgreSQL için Azure veritabanı-hiper ölçek (Citus) sunucu grubu oluşturma sırasında da erişilebilir. **Ağ** sekmesinde, **genel uç nokta**' ı tıklatın.

> :::image type="content" source="./media/howto-hyperscale-manage-firewall-using-portal/0-create-public-access.png" alt-text="Azure portal-ağ sekmesi":::

1. PostgreSQL sunucu grubu sayfasında, güvenlik başlığı altında, güvenlik duvarı kurallarını açmak için **ağ** ' ı tıklatın.

   :::image type="content" source="./media/howto-hyperscale-manage-firewall-using-portal/1-connection-security.png" alt-text="Azure portal-ağ ' a tıklayın":::

2. Azure sistemi tarafından algılanan şekilde, bilgisayarınızın genel IP adresiyle bir güvenlik duvarı kuralı oluşturmak için **geçerli ISTEMCI IP adresi ekle** ' ye tıklayın.

   :::image type="content" source="./media/howto-hyperscale-manage-firewall-using-portal/2-add-my-ip.png" alt-text="Azure portal-istemci IP 'si Ekle ' ye tıklayın":::

Alternatif olarak, **+ 0.0.0.0-255.255.255.255** (B seçeneğinin sağında) öğesine TıKLADıĞıNıZDA yalnızca IP 'niz olamaz, ancak düzenleyici düğümünün 5432 numaralı bağlantı noktasına erişmek için internet 'in tamamına izin verir. Bu durumda, istemcilerin kümeyi kullanmak için yine de doğru Kullanıcı adı ve parolayla oturum açması gerekir. Bununla birlikte, yalnızca kısa süreler için ve yalnızca üretim dışı veritabanları için dünya genelindeki erişime izin vermeyi öneririz.

3. Yapılandırmayı kaydetmeden önce IP adresinizi doğrulayın. Bazı durumlarda Azure portal tarafından gözlenen IP adresi, internet ve Azure sunucularına erişirken kullanılan IP adresinden farklıdır. Bu nedenle, kural işlevini beklendiği gibi yapmak için başlangıç IP 'sini ve bitiş IP 'sini değiştirmeniz gerekebilir.
   Kendi IP adresinizi denetlemek için bir arama altyapısı veya başka bir çevrimiçi araç kullanın. Örneğin, "IP nedir?" ifadesini aratın.

   :::image type="content" source="./media/howto-hyperscale-manage-firewall-using-portal/3-what-is-my-ip.png" alt-text="IP 'im nedir için Bing arama":::

4. Ek adres aralıkları ekleyin. Güvenlik duvarı kurallarında, tek bir IP adresi veya bir adres aralığı belirtebilirsiniz. Kuralı tek bir IP adresiyle sınırlandırmak istiyorsanız, başlangıç IP 'si ve bitiş IP 'si için alana aynı adresi yazın. Güvenlik duvarının açılması, yöneticilerin, kullanıcıların ve uygulamaların 5432 numaralı bağlantı noktasındaki düzenleyici düğümüne erişmesine olanak sağlar.

5. Bu sunucu düzeyinde güvenlik duvarı kuralını kaydetmek için araç çubuğunda **Kaydet** ' e tıklayın. Güvenlik duvarı kuralları güncelleştirmesi başarılı oldu onay için bekleyin.

## <a name="connecting-from-azure"></a>Azure'dan bağlanma

Azure 'da barındırılan uygulamalara (örneğin, Azure Web Apps uygulaması veya bir Azure VM 'de çalışan) hiper ölçek (Citus) veritabanı erişimi sağlamanın kolay bir yolu vardır. **Ağ** bölmesine ve **Kaydet**'e, portalda **Azure hizmetleri ve kaynaklarına bu sunucu grubuna erişim izni ver** seçeneğini **Evet** olarak ayarlayın.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Azure portalı aracılığıyla mevcut sunucu düzeyinde güvenlik duvarı kurallarını yönetme
Güvenlik duvarı kurallarını yönetmek için adımları yineleyin.
* Geçerli bilgisayarı eklemek için düğmeye tıklayarak + **geçerli ISTEMCI IP adresini ekleyin**. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Daha fazla IP adresi eklemek için Kural Adı, Başlangıç IP Adresi ve Bitiş IP Adresi’ni yazın. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Mevcut bir kuralı değiştirmek için kuraldaki alanlardan dilediğinize tıklayıp değiştirin. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.
* Mevcut bir kuralı silmek için, üç nokta (...] simgesine tıklayın ve **Sil** ' e tıklayarak kuralı kaldırın. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
- Bağlantı sorunlarının nasıl giderileceği dahil olmak üzere [güvenlik duvarı kuralları kavramı](concepts-hyperscale-firewall-rules.md)hakkında daha fazla bilgi edinin.
