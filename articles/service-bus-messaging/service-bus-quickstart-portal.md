---
title: Service Bus kuyruğu oluşturmak için Azure portal kullanma
description: Bu hızlı başlangıçta, Azure portal kullanarak ad alanı ve bir sırayı Service Bus nasıl oluşturacağınızı öğreneceksiniz.
author: spelluru
ms.topic: quickstart
ms.date: 08/12/2020
ms.author: spelluru
ms.openlocfilehash: 0e5a8698da03ee4d9c7d8b0d4d59eda1be3b1883
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "88185511"
---
# <a name="use-azure-portal-to-create-a-service-bus-namespace-and-a-queue"></a>Bir Service Bus ad alanı ve sıra oluşturmak için Azure portal kullanma
Bu hızlı başlangıçta, [Azure Portal][Azure portal]kullanarak bir Service Bus ad alanı ve kuyruğun nasıl oluşturulacağı gösterilmektedir. Ayrıca, bir istemci uygulamanın sıraya/kuyruğa ileti göndermek/almak için kullanabileceği yetkilendirme kimlik bilgilerinin nasıl alınacağını gösterir. 

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlayabilmeniz için bir Azure aboneliğiniz olduğundan emin olun. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][] oluşturabilirsiniz.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, ad alanında bir Service Bus ad alanı ve bir sıra oluşturdunuz. Sıraya/kuyruğa ileti gönderme/alma hakkında bilgi edinmek için **Ileti gönderme ve alma** bölümünde aşağıdaki hızlı başlangıçlardan birine bakın. 

- [.NET](service-bus-dotnet-get-started-with-queues.md)
- [Java](service-bus-java-how-to-use-queues.md)
- [JavaScript](service-bus-nodejs-how-to-use-queues-new-package.md)
- [Python](service-bus-python-how-to-use-queues.md)
- [PHP](service-bus-php-how-to-use-queues.md)
- [Ruby](service-bus-ruby-how-to-use-queues.md)

[Ücretsiz hesap]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[Azure portal]: https://portal.azure.com/

[service-bus-flow]: ./media/service-bus-quickstart-portal/service-bus-flow.png
