---
title: Redsıs için Azure önbelleğini Azure Spring Cloud uygulamanıza bağlama
description: Reda için Azure önbelleğini Azure Spring Cloud uygulamanıza bağlamayı öğrenin
author: bmitchell287
ms.service: spring-cloud
ms.topic: how-to
ms.date: 10/31/2019
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: ef77ba6e34f2a699c8c4f06fde8cb602ae98c728
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "90885673"
---
# <a name="bind-azure-cache-for-redis-to-your-azure-spring-cloud-application"></a>Redsıs için Azure önbelleğini Azure Spring Cloud uygulamanıza bağlama 

**Bu makale şu şekilde geçerlidir:** ✔️ Java

Spring Boot uygulamalarınızı el ile yapılandırmak yerine Azure Spring Cloud kullanarak Azure hizmetlerini otomatik olarak uygulamalarınıza bağlayabilirsiniz. Bu makalede, Redsıs için uygulamanızı Azure önbelleğine bağlama işlemi gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

* Dağıtılan bir Azure yay bulutu örneği
* Redsıs hizmet örneği için bir Azure önbelleği
* Azure CLı için Azure yay bulutu uzantısı

Dağıtılmış bir Azure yay bulut örneğiniz yoksa, [Azure yay bulutu uygulamasını dağıtmaya yönelik hızlı başlangıçta](spring-cloud-quickstart.md)bulunan adımları izleyin.

## <a name="bind-azure-cache-for-redis"></a>Redsıs için Azure önbelleğini bağlama

1. Aşağıdaki bağımlılığı projenizin pom.xml dosyasına ekleyin:

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis-reactive</artifactId>
    </dependency>
    ```
1. Dosyadaki tüm `spring.redis.*` Özellikleri Kaldır `application.properties`

1. Kullanarak geçerli dağıtımı güncelleştirin `az spring-cloud app update` veya kullanarak yeni bir dağıtım oluşturun `az spring-cloud app deployment create` .

1. Azure portal Azure Spring Cloud Service sayfanıza gidin. **Uygulama panosu** ' na gidin ve Redsıs Için Azure önbelleğine bağlanacak uygulamayı seçin. Bu uygulama, önceki adımda güncelleştirdiğiniz veya dağıttığınız aynı bir uygulamadır.

1. **Hizmet bağlamayı** seçin ve **hizmet bağlamayı oluştur**' u seçin. Formu doldurun, **redsıs Için Azure önbelleği** **,** Redsıs sunucusu için Azure önbelleğiniz ve **birincil** anahtar seçeneğini seçtiğinizden emin olun.

1. Uygulamayı yeniden başlatın. Bağlama artık çalışmalıdır.

1. Hizmet bağlamasının doğru olduğundan emin olmak için bağlama adını seçin ve ayrıntılarını doğrulayın. `property`Alan şöyle görünmelidir:
    ```
    spring.redis.host=some-redis.redis.cache.windows.net
    spring.redis.port=6380
    spring.redis.password=abc******
    spring.redis.ssl=true
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Spring Cloud uygulamanızı Redsıs için Azure önbelleğine bağlamayı öğrendiniz. Uygulamanıza hizmet bağlama hakkında daha fazla bilgi edinmek için bkz. [MySQL Için Azure veritabanı örneğine bağlama](spring-cloud-tutorial-bind-mysql.md).
