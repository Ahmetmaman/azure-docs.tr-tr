---
title: Azure Active Directory B2C yaş ' i etkinleştirme | Microsoft Docs
description: Uygulamanızı kullanarak sömürmeyi amaçlama 'yi nasıl tanımlayacağınızı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 11/13/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 6bd93f9062f8446ce20436a7a04e2054aaf5be71
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "85386140"
---
# <a name="enable-age-gating-in-azure-active-directory-b2c"></a>Azure Active Directory B2C yaş ve ömrü etkinleştir

>[!IMPORTANT]
>Bu özellik genel önizleme aşamasındadır. Üretim uygulamaları için özelliği kullanmayın.
>

Azure Active Directory B2C (Azure AD B2C) yaş aşımları uygulamanızı kullanmak isteyen sömürmeyi amaçlama tanımlamanızı sağlar. Küçük bir uygulamada oturum açmasını engellemeyi tercih edebilirsiniz. Kullanıcılar ayrıca uygulamaya dönerek yaş gruplarını ve bunların ana öğe onay durumunu tanımlayabilir. Azure AD B2C, ebeveyn izni olmadan sömürmeyi amaçlama 'ı engelleyebilir. Azure AD B2C Ayrıca uygulamanın minors ile ne yapacağına karar vermesini sağlamak için de ayarlanabilir.

[Kullanıcı](user-flow-overview.md)akışınızda yaş ile kullanım süresini etkinleştirdikten sonra, kullanıcılar doğduklarında ve hangi ülke/bölge üzerinde yaşlandıklarında sorulur. Bir Kullanıcı daha önce bilgileri girmeyen bir oturum açarsa, bir sonraki oturum açışlarında bu uygulamayı girmesi gerekir. Kurallar, bir kullanıcının her oturum açışında uygulanır.

Azure AD B2C, kullanıcının küçük olup olmadığını belirlemek için girdiği bilgileri kullanır. Daha sonra, **Agegroup** alanı hesabında güncellenir. Değer,,, `null` `Undefined` ve olabilir `Minor` `Adult` `NotAdult` .  **Agegroup** ve **consentProvidedForMinor** alanları, daha sonra **ligalagegroupclassification**değerini hesaplamak için kullanılır.

Yaş aşımları iki yaş değeri içerir: birinin artık küçük olarak değerlendirilmediği yaş ve küçük bir ana izin olması gereken yaş. Aşağıdaki tabloda, düşük ve küçük bir onay gerektiren bir izin tanımlamak için kullanılan yaş kuralları listelenmektedir.

| Ülke/Bölge | Ülke/bölge adı | Küçük onay yaşı | Küçük yaş |
| -------------- | ------------------- | ----------------- | --------- |
| Varsayılan | Yok | Yok | 18 |
| AE | Birleşik Arap Emirlikleri | Yok | 21 |
| AT | Avusturya | 14 | 18 |
| BE | Belçika | 14 | 18 |
| BG | Bulgaristan | 16 | 18 |
| BH | Bahreyn | Yok | 21 |
| CM | Kamerun | Yok | 21 |
| CY | Kıbrıs | 16 | 18 |
| CZ | Çek Cumhuriyeti | 16 | 18 |
| DE | Almanya | 16 | 18 |
| DK | Danimarka | 16 | 18 |
| EE | Estonya | 16 | 18 |
| EG | Mısır | Yok | 21 |
| ES | İspanya | 13 | 18 |
| GS | Fransa | 16 | 18 |
| GB | Birleşik Krallık | 13 | 18 |
| GR | Yunanistan | 16 | 18 |
| HR | Hırvatistan | 16 | 18 |
| HU | Macaristan | 16 | 18 |
| IE | İrlanda | 13 | 18 |
| BT | İtalya | 16 | 18 |
| KR | Kore Cumhuriyeti | 14 | 18 |
| LT | Litvanya | 16 | 18 |
| LU | Lüksemburg | 16 | 18 |
| LV | Letonya | 16 | 18 |
| MT | Malta | 16 | 18 |
| NA | Namibya | Yok | 21 |
| NL | Hollanda | 16 | 18 |
| PL | Polonya | 13 | 18 |
| PT | Portekiz | 16 | 18 |
| RO | Romanya | 16 | 18 |
| SE | İsveç | 13 | 18 |
| SG | Singapur | Yok | 21 |
| SI | Slovenya | 16 | 18 |
| SK | Slovakya | 16 | 18 |
| TD | Çad | Yok | 21 |
| TH | Tayland | Yok | 20 |
| TW | Tayvan | Yok | 20 |
| ABD | Birleşik Devletler | 13 | 18 |

## <a name="age-gating-options"></a>Yaş aşımları seçenekleri

### <a name="allowing-minors-without-parental-consent"></a>Ebeveyn izni olmadan minörlerin kullanılmasına izin verme

Kaydolma, oturum açma ya da her ikisine de izin veren Kullanıcı akışları için, uygulamanıza onay olmadan en küçük parçalara izin vermeyi tercih edebilirsiniz. Ebeveyn izni olmayan küçük noktalar, oturum açma veya normal şekilde kaydolma için izin verilir ve Azure AD B2C, bir KIMLIK belirtecini, **Ligalagegroupclassification** talebine sahip olarak verir. Bu talep, kullanıcıların sahip olduğu deneyimi tanımlar, örneğin, ebeveyn onayı toplama ve **consentProvidedForMinor** alanını güncelleştirme.

### <a name="blocking-minors-without-parental-consent"></a>Ebeveyn izni olmadan sömürmeyi amaçlama 'yi engelleme

Kaydolma, oturum açma ya da her ikisine de izin veren Kullanıcı akışları için, uygulamayı izin vermeden engellemeyi tercih edebilirsiniz. Azure AD B2C engellenen kullanıcıları işlemek için aşağıdaki seçenekler kullanılabilir:

- Bir JSON 'ı uygulamaya geri gönder-bu seçenek, küçük bir blok uygulamanın uygulamaya geri yanıt gönderir.
- Bir hata sayfası göster-kullanıcıya, uygulamaya erişeemiyorum bildiren bir sayfa gösterilir.

## <a name="set-up-your-tenant-for-age-gating"></a>Kiracınızı yaş için ayarlama

Bir Kullanıcı akışında yaş geçişi kullanmak için kiracınızı ek özelliklere sahip olacak şekilde yapılandırmanız gerekir.

1. Üst menüdeki **Dizin + abonelik** filtresini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun. Kiracınızı içeren dizini seçin.
2. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin, arama yapın ve **Azure AD B2C**seçin.
3. Sol taraftaki menüden kiracınız için **Özellikler** ' i seçin.
2. **Yaş** aşımları bölümünde, **Yapılandır**' a tıklayın.
3. İşlemin tamamlanmasını bekleyin ve kiracınız yaş için ayarlanır.

## <a name="enable-age-gating-in-your-user-flow"></a>Kullanıcı akışınızda yaş kullanım süresini etkinleştirme

Kiracınız yaş kullanımını kullanacak şekilde ayarlandıktan sonra bu özelliği, etkin olduğu [Kullanıcı akışlarında](user-flow-versions.md) kullanabilirsiniz. Ömrü aşağıdaki adımlarla etkinleştirilir:

1. Yaş geçişi etkin olan bir Kullanıcı akışı oluşturun.
2. Kullanıcı akışını oluşturduktan sonra menüdeki **Özellikler** ' i seçin.
3. **Yaş** aşımları bölümünde, **etkin**' i seçin.
4. Daha sonra, minors olarak tanımlayan kullanıcıları nasıl yönetmek istediğinize karar verirsiniz. **Kaydolma veya oturum açma**için `Allow minors to access your application` veya seçin `Block minors from accessing your application` . Sömürmeyi amaçlama 'yi engellemek seçilmişse veya seçeneğini belirleyin `Send a JSON back to the application` `Show an error message` .




