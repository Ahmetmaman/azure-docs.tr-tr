---
title: Self Servis kaydolma akışlarına özel öznitelikler ekleme-Azure AD
description: Self Servis kaydolma Kullanıcı akışlarınızın özniteliklerini özelleştirme hakkında bilgi edinin.
services: active-directory
author: msmimart
manager: celestedg
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 06/16/2020
ms.author: mimart
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: e64ab70fed13d4ca907b2bfb3aa448acdedc39e9
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92441462"
---
# <a name="define-custom-attributes-for-user-flows-preview"></a>Kullanıcı akışları için özel öznitelikler tanımlayın (Önizleme)

> [!NOTE]
> Özel Kullanıcı öznitelikleri özelliği, Azure Active Directory genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Her uygulama için, kayıt sırasında toplamak istediğiniz bilgiler için farklı gereksinimlerinize sahip olabilirsiniz. Azure AD, belirli ad, soyadı, şehir ve posta kodu gibi özniteliklerde depolanan yerleşik bir bilgi kümesiyle gelir. Azure AD ile, dış Kullanıcı Kullanıcı akışından kaydolduğunda Konuk hesapta depolanan özniteliklerin kümesini genişletebilirsiniz.

Azure portal özel öznitelikler oluşturabilir ve bunları self servis kaydolma Kullanıcı akışlarınızda kullanabilirsiniz. Ayrıca, [MICROSOFT Graph API](../../active-directory-b2c/manage-user-accounts-graph-api.md)'sini kullanarak bu öznitelikleri okuyabilir ve yazabilirsiniz. Microsoft Graph API, uzantı öznitelikleri olan bir kullanıcının oluşturulmasını ve güncelleştirilmesini destekler. Graph API uzantı öznitelikleri, kuralı kullanılarak adlandırılır `extension_<extensions-app-id>_attributename` . Örneğin:

```JSON
"extension_831374b3bd5041bfaa54263ec9e050fc_loyaltyNumber": "212342"
```

, `<extensions-app-id>` Kiracınıza özeldir. Bu tanımlayıcıyı bulmak için, tüm uygulamaları > Uygulama kayıtları Azure Active Directory > gidin. "AAD-Extensions-App" ile başlayan uygulamayı arayın ve seçin. Uygulamanın genel bakış sayfasında, uygulama (istemci) KIMLIĞI ' ne göz atın.

## <a name="create-a-custom-attribute"></a>Özel öznitelik oluştur

1. [Azure portalda](https://portal.azure.com) Azure AD yöneticisi olarak oturum açın.
2. **Azure hizmetleri**altında **Azure Active Directory**' yi seçin.
3. Sol taraftaki menüden **dış kimlikler**' i seçin.
4. **Özel Kullanıcı öznitelikleri (Önizleme)** seçeneğini belirleyin. Kullanılabilir Kullanıcı öznitelikleri listelenir.

   ![Kaydolma için Kullanıcı özniteliklerini seçin](media/user-flow-add-custom-attributes/user-attributes.png)

5. Bir öznitelik eklemek için **Ekle**' yi seçin.
6. **Öznitelik Ekle** bölmesinde, aşağıdaki değerleri girin:

   - **Ad** -özel öznitelik için bir ad sağlayın (örneğin, "shoesize").
   - **Veri türü** -bir veri türü seçin (**dize**, **Boole**veya **Int**).
   - **Açıklama** -isteğe bağlı olarak, iç kullanım için özel özniteliğin bir açıklamasını girin. Bu açıklama kullanıcıya görünür değil.

   ![Öznitelik ekleme](media/user-flow-add-custom-attributes/add-an-attribute.png)

7. **Oluştur**’u seçin.

Özel öznitelik artık Kullanıcı öznitelikleri listesinde ve Kullanıcı akışlarınızda kullanılmak üzere kullanılabilir. Özel bir öznitelik, Kullanıcı öznitelikleri listesine eklediğinizde değil, yalnızca herhangi bir Kullanıcı akışında kullanıldığı zaman oluşturulur.

Yeni oluşturulan özel özniteliği kullanan bir Kullanıcı akışı kullanarak yeni bir kullanıcı oluşturduktan sonra, nesne [Microsoft Graph Explorer](https://developer.microsoft.com/graph/graph-explorer)'da sorgulanabilir. Artık kullanıcı nesnesine kaydolma yolculuğunda toplanan öznitelikler listesinde **Showesize** ' i görmeniz gerekir. Kullanıcı nesnesine eklendikten sonra bu öznitelikten verileri almak için uygulamanızdan Graph API çağırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Bir uygulamaya self servis kaydolma Kullanıcı akışı ekleme](self-service-sign-up-user-flow.md)