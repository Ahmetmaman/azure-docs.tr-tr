---
title: Azure SignalR hizmeti için erişim anahtarı döndürme
description: Müşteri neden düzenli olarak erişim anahtarlarını döndürme gerekiyor ve Azure portalıyla GUI ve Azure CLI'yı nasıl genel bir bakış.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: zhshang
ms.openlocfilehash: 15b0ff0bbb96e5fa96d81cfa265e83abf749cf85
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57555272"
---
# <a name="how-to-rotate-access-key-for-azure-signalr-service"></a>Azure SignalR hizmeti için erişim anahtarı döndürme

Her Azure SignalR hizmeti örneği, bir çift birincil ve ikincil anahtarları adlı erişim anahtarına sahiptir. Bunlar hizmete istekler yapıldığında SignalR istemcilerin kimliğini doğrulamak için kullanılır. Anahtarları örnek uç nokta URL'si ile ilişkili değildir. Anahtarlarınızı güvenli tutmalarını ve düzenli olarak döndür. İşiniz sağlanan iki erişim tuşu ile bu nedenle, bağlantıları yeniden oluştururken diğeri bir anahtar kullanarak koruyabilirsiniz.

## <a name="why-rotate-access-keys"></a>Neden döndürme erişim anahtarları?

Güvenlik nedeniyle ve uyumluluk gereksinimleri için erişim anahtarlarınızı düzenli olarak döndür.

## <a name="regenerate-access-keys"></a>Erişim anahtarlarını yeniden oluştur

1. Git [Azure portalında](https://portal.azure.com/)ve kimlik bilgilerinizle oturum açın.

1. Bulma **anahtarları** bölümünde Azure SignalR hizmeti örneği anahtarlarla yeniden oluşturmak istiyor.

1. Seçin **anahtarları** Gezinti menüsünde.

1. Seçin **birincil anahtarı yeniden** veya **ikincil anahtar yeniden**.

   Yeni bir anahtar ve karşılık gelen bağlantı dizesi oluşturulur ve görüntülenir.

   ![Anahtarları Yeniden Oluştur](media/signalr-howto-key-rotation/regenerate-keys.png)

Anahtarları kullanarak da yeniden oluşturabilirsiniz [Azure CLI](/cli/azure/ext/signalr/signalr/key?view=azure-cli-latest#ext-signalr-az-signalr-key-renew).

## <a name="update-configurations-with-new-connection-strings"></a>Yeni bağlantı dizeleri ile yapılandırmaları güncelleştirme

1. Yeni oluşturulan bağlantı dizesini kopyalayın.

1. Yeni bağlantı dizesini kullanmak için tüm yapılandırmaları güncelleştirin.

1. Uygulama, gerektiği gibi yeniden başlatın.

## <a name="forced-access-key-regeneration"></a>Zorlanmış erişim anahtarını yeniden üretme

Azure SignalR hizmeti belirli durumlarda bir zorunlu erişim anahtarını yeniden üretme uygulanmasını şart koşabilir. Hizmet müşterileri e-posta ve portal bildirimi aracılığıyla size bildirir. Bu iletişim almak veya hizmet hatası nedeniyle bir erişim anahtarı karşılaşırsanız, bu kılavuzdaki yönergeleri takip ederek anahtarlarını döndürün.

## <a name="next-steps"></a>Sonraki adımlar

Erişim anahtarlarınızı düzenli olarak iyi güvenlik uygulaması olarak döndür.

Bu kılavuzda, erişim anahtarlarını yeniden oluştur öğrendiniz. Azure işlevleri veya OAuth ile kimlik doğrulaması hakkında sonraki öğreticiler için devam edin.

> [!div class="nextstepaction"]
> [ASP.NET core kimliği ile tümleştirin](./signalr-concept-authenticate-oauth.md)

> [!div class="nextstepaction"]
> [Kimlik doğrulaması ile gerçek zamanlı sunucusuz uygulama oluşturma](./signalr-tutorial-authenticate-azure-functions.md)