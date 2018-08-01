---
title: İçin özel olarak geliştirilmiş bir uygulamada belirli alanları doldurmak nasıl | Microsoft Docs
description: Azure AD ile özel olarak geliştirilen bir uygulama kaydı sırasında belirli alanları doldurun hakkında yönergeler
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: ecdd76a8a6974a819cd0bedffd095299435dcc88
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365322"
---
# <a name="how-to-fill-out-specific-fields-for-a-custom-developed-application"></a>Nasıl için özel olarak geliştirilmiş bir uygulamada belirli alanları doldurun

Bu makalede, kullanılabilir tüm alanları uygulama kayıt formunda kısa bir açıklamasını sunar [Azure portalında](https://portal.azure.com).

## <a name="register-a-new-application"></a>Yeni bir uygulamayı kaydetme

-   Yeni bir uygulamayı kaydetmek için gidin [Azure portalında](https://portal.azure.com).

-   Sol gezinti bölmesinden tıklayın **Azure Active Directory.**

-   Seçin **uygulama kayıtları** tıklatıp **Ekle**.

-   Bu kayıt formunu açın.

## <a name="fields-in-the-application-registration-form"></a>Kayıt formunu alanları


| Alan            | Açıklama                                                                              |
|------------------|------------------------------------------------------------------------------------------|
| Ad             | Uygulamanın adı. Bu, en az dört karakter olmalıdır.                |
| Uygulama Türü | **Web uygulaması/Web API'si**: bir web uygulaması, bir web API'sini veya her ikisini temsil eden bir uygulama 
| |**Yerel**: bir kullanıcının cihazına veya bilgisayarına yüklenebilen bir uygulama           |
| Oturum Açma URL'si      | Kullanıcıların uygulamanızı kullanmak için oturum içinde URL'si                                  |

Yukarıdaki alanları doldurduktan sonra Azure portalında uygulama kaydedilir ve uygulama sayfasına yönlendirilirsiniz. **Ayarları** uygulama bölmesinde düğmesine uygulamanızı özelleştirmek size daha fazla alan olan ayarları sayfası açılır. Aşağıdaki tabloda Ayarları sayfasında tüm alanlar açıklanır. Not, bir web uygulaması veya yerel bir uygulama oluşturduğunuz bağlı olarak, bu alanların bir alt kümesini yalnızca görür.

| Alan           | Açıklama                                                                                                                                                                                                                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Uygulama Kimliği  | Bir uygulamayı kaydettiğinizde, Azure AD uygulamanızı bir uygulama kimliği atar Azure ad kimlik doğrulama isteklerini uygulamanıza benzersiz olarak tanımlanabilmesi gibi kaynaklara erişmek için kimlik kullanılabilir uygulama Graph API'sini ister.                                                          |
| Uygulama Kimliği URI'si      | Bu genellikle formun benzersiz bir URI olmalıdır **https://&lt;Kiracı\_adı&gt;/&lt;uygulama\_adı&gt;.** Yetkilendirme verme akışı sırasında bu belirteç için yayınlanması gereken kaynak belirtmek için benzersiz bir tanımlayıcı olarak kullanılır. Ayrıca, verilen erişim belirtecinde 'aud' talep olur. |
| Karşıya yeni logo yükle | Bu, uygulamanız için bir logosu yüklemek için kullanabilirsiniz. Logo, .bmp, .jpg veya .png biçiminde olmalıdır ve dosya boyutu 100 KB'tan küçük olmalıdır. Görüntü boyutları ile merkez resim boyutları: 94 x 94 piksel 215 x 215 piksel olmalıdır.                                                       |
| Giriş sayfası URL'si   | Uygulama kaydı sırasında belirtilen oturum açma URL'si budur.                                                                                                                                                                                                                                              |
| Oturum Kapatma URL'si      | Bu çoklu oturum kapatma oturum kapatma URL'si. Azure AD ile kullanıcı oturumuna temizlediğinde azure AD'ye kayıtlı herhangi bir uygulamayı kullanarak bu URL'ye oturum kapatma isteği gönderir.                                                                                                                                       |
| Çok kiracılı  | Bu anahtar, birden fazla Kiracı tarafından uygulamanın kullanılıp kullanılamayacağını belirtir. Bu genellikle, dış kuruluşlar kendilerine ait kiracıda kaydedip kuruluş verilerine erişimi verme uygulamanızın kullanabileceği anlamına gelir.                                                                   |
| Yanıt URL'leri      | Yanıt URL'leri, burada Azure AD, uygulamanız tarafından istenen belirteçleri döndürdüğü uç noktalardır.                                                                                                                                                                                                          |
| Yeniden Yönlendirme URI'leri   | Yerel uygulamalar için kullanıcı başarılı yetkilendirme sonrasında burada gönderilen budur. Yeniden yönlendirme URI'si, uygulamanız OAuth 2.0 isteğine sağlayan azure AD denetimi Portalı'nda kayıtlı değerlerinden eşleşir.                                                            |
| Anahtarlar            | Programlama yoluyla erişim Web güvenliği herhangi bir kullanıcı etkileşimi olmadan Azure AD tarafından sağlanan API anahtarları oluşturabilirsiniz. Gelen \* \*anahtarları\* \* sayfasında, bir anahtar açıklaması ve sona erme tarihini girin ve anahtarı oluşturmak için kaydedin. Daha sonra erişmek mümkün olmayacaktır gibi güvenli bir yere kaydettiğinizden emin olun.             |

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](manage-apps/what-is-application-management.md)
