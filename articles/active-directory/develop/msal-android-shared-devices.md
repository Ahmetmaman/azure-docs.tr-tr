---
title: Android cihazlar için paylaşılan cihaz modu
titleSuffix: Microsoft identity platform | Azure
description: Firstline çalışanlarının bir Android cihazını paylaşmasına izin vermek için paylaşılan cihaz modunu etkinleştirmeyi öğrenin
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 03/31/2020
ms.author: marsma
ms.reviewer: hahamil
ms.custom: aaddev, identitypla | Azuretformtop40
ms.openlocfilehash: fc32b4f583aea2fa9a34ab8b235f3f99fe4def9d
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94562177"
---
# <a name="shared-device-mode-for-android-devices"></a>Android cihazlar için paylaşılan cihaz modu

>[!IMPORTANT]
> Bu özellik [!INCLUDE [PREVIEW BOILERPLATE](../../../includes/active-directory-develop-preview.md)]

Retail Associates, uçuş ekibi üyeleri ve alan hizmeti çalışanları gibi Firstline çalışanları genellikle işlerini yapmak için paylaşılan bir mobil cihaz kullanır. Bu, paylaşılan cihazdaki müşteri ve iş verilerine erişmek üzere parolaları paylaşmaya veya PIN numaralarını açtıklarında sorunlu hale gelir.

Paylaşılan cihaz modu, bir Android cihazını birden çok çalışan tarafından kolayca paylaşılabilecek şekilde yapılandırmanıza olanak tanır. Çalışanlar oturum açabilir ve müşteri bilgilerine hızlıca erişebilir. Bunlar, vardiyası veya göreviyle bitdiklerinde cihazda oturum açabilir ve bir sonraki çalışanın kullanması için hemen hazır olacaktır.

Paylaşılan cihaz modu, cihazın Microsoft tarafından desteklenen yönetimini de sağlar.

Bir paylaşılan cihaz modu uygulaması oluşturmak için, geliştiriciler ve bulut cihaz yöneticileri birlikte çalışır:

- Geliştiriciler tek hesap uygulaması yazar (çoklu hesap uygulamaları paylaşılan cihaz modunda desteklenmez), `"shared_device_mode_supported": true` uygulamanın yapılandırmasına ekler ve paylaşılan cihaz oturumu kapatma gibi şeyleri işlemek için kod yazar.
- Cihaz yöneticileri, kimlik doğrulayıcı uygulamasını yükleyerek cihazı paylaşıma hazırlayın ve kimlik doğrulayıcı uygulamasını kullanarak cihazı paylaşılan moda ayarlar. Yalnızca [bulut aygıtı yönetici](../roles/permissions-reference.md#cloud-device-administrator-permissions) rolünde olan kullanıcılar, [kimlik doğrulayıcı uygulamasını](../user-help/user-help-auth-app-overview.md)kullanarak bir cihazı paylaşılan moda yerleştirebilir. Azure Portal kurumsal rollerinizin üyeliğini: **Azure Active Directory**  >  **Roller ve yöneticiler**  >  **bulut Cihaz Yöneticisi** aracılığıyla yapılandırabilirsiniz.

 Bu makalede öncelikle geliştiricilerin ne düşündüklerini ele alınmaktadır.

## <a name="single-vs-multiple-account-applications"></a>Tek vs çoklu hesap uygulamaları

Microsoft kimlik doğrulama kitaplığı SDK 'Sı (MSAL) kullanılarak yazılan uygulamalar tek bir hesabı veya birden çok hesabı yönetebilir. Ayrıntılar için bkz. [tek hesap modu veya birden çok hesap modu](single-multi-account.md). Uygulamanıza sunulan Microsoft Identity platform özellikleri, uygulamanın tek hesap modunda veya birden çok hesap modunda çalışıyor olmasına bağlı olarak farklılık gösterir.

**Paylaşılan cihaz modu uygulamaları yalnızca tek hesap modunda çalışır**.

> [!IMPORTANT]
> Yalnızca birden çok hesap modunu destekleyen uygulamalar paylaşılan bir cihazda çalıştırılamaz. Bir çalışan, tek hesap modunu desteklemeyen bir uygulama yüklerse, bu, paylaşılan cihazda çalışmaz.
>
> MSAL SDK kullanılmadan önce yazılan uygulamalar birden çok hesap modunda çalıştırın ve paylaşılan mod cihazında çalıştırılmadan önce tek hesap modunu destekleyecek şekilde güncelleştirilmeleri gerekir.

**Hem tek hesap hem de birden çok hesabı destekleme**

Uygulamanız, hem kişisel cihazlarda hem de Paylaşılan cihazlarda çalışmayı destekleyecek şekilde oluşturulabilir. Uygulamanız Şu anda birden çok hesabı destekliyorsa ve paylaşılan cihaz modunu desteklemek istiyorsanız, tek hesap modu için destek ekleyin.

Uygulamanızın, üzerinde çalıştığı cihaz türüne bağlı olarak davranışını değiştirmesini de isteyebilirsiniz. `ISingleAccountPublicClientApplication.isSharedDevice()`Tek hesap modunda ne zaman çalıştırılacağını öğrenmek için kullanın.

Uygulamanızın bulunduğu cihaz türünü temsil eden iki farklı arabirim vardır. MSAL 'ın uygulama fabrikasından bir uygulama örneği istediğinizde, doğru uygulama nesnesi otomatik olarak sağlanır.

Aşağıdaki nesne modeli, alabileceğiniz nesnenin türünü ve paylaşılan bir cihaz bağlamında ne anlama geldiğini gösterir:

![ortak istemci uygulaması devralma modeli](media/v2-shared-device-mode/ipublic-client-app-inheritance.png)

Nesne aldığınızda bir tür denetimi yapmanız ve uygun arabirime dönüştürmeniz gerekir `PublicClientApplication` . Aşağıdaki kod, birden çok hesap modunu veya tek hesap modunu denetler ve uygulama nesnesini uygun şekilde yayınlar:

```java
private IPublicClientApplication mApplication;

        // Running in personal-device mode?
        if (mApplication instanceOf IMultipleAccountPublicClientApplication) {
          IMultipleAccountPublicClientApplication multipleAccountApplication = (IMultipleAccountPublicClientApplication) mApplication;
          ...
        // Running in shared-device mode?
        } else if (mApplication instanceOf ISingleAccountPublicClientApplication) {
           ISingleAccountPublicClientApplication singleAccountApplication = (ISingleAccountPublicClientApplication) mApplication;
            ...
        }
```

Aşağıdaki farklar, uygulamanızın paylaşılan veya kişisel bir cihazda çalışıyor olmasına bağlı olarak geçerlidir:

|  | Paylaşılan mod cihazı  | Kişisel cihaz |
|---------|---------|---------|
| **Hesaplar**     | Tek hesap | Birden çok hesap |
| **Oturum Açma** | Genel | Genel |
| **Oturumu kapatma** | Genel | Her uygulama, oturum açma 'nın uygulamada veya uygulamalar ailesinde yerel olup olmadığını denetleyebilir. |
| **Desteklenen hesap türleri** | Yalnızca iş hesapları | Desteklenen kişisel ve iş hesapları  |

## <a name="why-you-may-want-to-only-support-single-account-mode"></a>Neden yalnızca tek hesap modunu desteklemek isteyebilirsiniz

Yalnızca bir paylaşılan cihaz kullanan ilk satır çalışanları için kullanılacak bir uygulama yazıyorsanız, uygulamanızı yalnızca tek hesap modunu destekleyecek şekilde yazmanızı öneririz. Bu, tıp kayıtları uygulamaları, fatura uygulamaları ve çoğu iş kolu uygulaması gibi görev odaklı birçok uygulamayı içerir. Çoklu hesap uygulamalarının parçası olan ek özellikleri uygulamanız gerekmediğinden, yalnızca tek hesap modunu desteklemek geliştirmeyi basitleştirir.

## <a name="what-happens-when-the-device-mode-changes"></a>Cihaz modu değiştiğinde ne olur?

Uygulamanız birden çok hesap modunda çalışıyorsa ve bir yönetici cihazı paylaşılan cihaz moduna geçirir, cihazdaki tüm hesaplar uygulamadan temizlenir ve uygulamanın tek hesap moduna geçişi yapılır.

## <a name="shared-device-sign-out-and-the-overall-app-lifecycle"></a>Paylaşılan cihaz oturumu kapatma ve genel uygulama yaşam döngüsü

Kullanıcı oturumu kapattığında, kullanıcının gizliliğini ve verilerini korumak için işlem yapmanız gerekir. Örneğin, bir tıp kayıtları uygulaması oluşturuyorsanız, Kullanıcı daha önce görüntülenen hasta kayıtlarının temizlenme durumunda olduğundan emin olmak isteyeceksiniz. Uygulamanız bu için hazırlanmalıdır ve ön plana her girdiğinde kontrol denetlenir.

Uygulamanız, paylaşılan modda olan cihazda çalışan bir uygulamada kullanıcının oturumunu kapatmak için MSAL kullandığında, oturum açma hesabı ve önbelleğe alınmış belirteçler hem uygulama hem de cihazdan kaldırılır.

Aşağıdaki diyagramda, uygulamanız çalışırken ortaya çıkabilecek genel uygulama yaşam döngüsü ve genel olaylar gösterilmektedir. Diyagram bir etkinliğin üzerinde başlatma, oturum açma ve oturumu kapatma ve etkinliği duraklatma, sürdürme ve durdurma gibi olayların nasıl uyduğunu anlatmaktadır.

![Paylaşılan cihaz uygulaması yaşam döngüsü](media/v2-shared-device-mode/lifecycle.png)

## <a name="next-steps"></a>Sonraki adımlar

Bir Firstline çalışan uygulamasının paylaşılan mod Android cihazında nasıl çalıştırılacağını gösteren [Android uygulama öğreticinizdeki paylaşılan cihaz modunu kullanın '](tutorial-v2-shared-device-mode.md) i deneyin.
