---
title: Key Vault sertifikası kullanılarak özel etki alanı eklenemiyor
titleSuffix: Azure API Management
description: Anahtar Kasası sertifikası kullanarak Azure API Management 'de özel etki alanı ekleyemiyoruz sorunu nasıl giderebileceğinizi öğrenin.
services: api-management
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/19/2019
ms.author: tehnoonr
ms.openlocfilehash: a09c15466a4a9f62b2696b087cb7ab23cc767379
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "75430588"
---
# <a name="failed-to-update-api-management-service-hostnames"></a>API Management hizmeti ana bilgisayar adları güncelleştirilemedi

Bu makalede, Azure API Management hizmeti için özel bir etki alanı eklediğinizde karşılaşabileceğiniz "API Management hizmet ana bilgisayar adları güncelleştirilemedi" hatası açıklanır. Bu makalede, sorunu çözmenize yardımcı olacak sorun giderme adımları sunulmaktadır.

## <a name="symptoms"></a>Belirtiler

Azure Key Vault bir sertifika kullanarak API Management hizmetiniz için özel bir etki alanı eklemeye çalıştığınızda aşağıdaki hata iletisini alırsınız:

- API Management hizmeti ana bilgisayar adları güncelleştirilemedi. ' ' Kaynağına yapılan istek https://vaultname.vault.azure.net/secrets/secretname/?api-version=7.0 , hata kodu ile başarısız oldu: RequestId:. Özel durum iletisi: Işlem geçersiz bir ' yasak ' durum kodu döndürdü.

## <a name="cause"></a>Nedeni

API Management hizmetin, özel etki alanı için kullanmaya çalıştığınız anahtar kasasına erişim izni yok.

## <a name="solution"></a>Çözüm

Bu sorunu çözmek için şu adımları izleyin:

1. [Azure Portal](Https://portal.azure.com)gidin, API Management örneğinizi seçin ve ardından **Yönetilen kimlikler**' i seçin. **Azure Active Directory Ile kaydet** seçeneğinin **Evet** olarak ayarlandığından emin olun. 
    ![Azure Active Director ile kaydetme](./media/api-management-troubleshoot-cannot-add-custom-domain/register-with-aad.png)
1. Azure portal, **anahtar** Kasası hizmetini açın ve özel etki alanı için kullanmaya çalıştığınız anahtar kasasını seçin.
1. **Erişim ilkeleri**' ni seçin ve API Management hizmet örneğinin adıyla eşleşen bir hizmet sorumlusu olup olmadığını kontrol edin. Varsa, hizmet sorumlusu ' nı seçin ve **gizli izinler** altında, izin **Al** ' ın altında bulunan izinleri doğrulayın.  
    ![Hizmet sorumlusu için erişim ilkesi ekleme](./media/api-management-troubleshoot-cannot-add-custom-domain/access-policy.png)
1. API Management hizmet listede yoksa, **erişim Ilkesi Ekle**' yi seçin ve ardından aşağıdaki erişim ilkesini oluşturun:
    - **Şablondan Yapılandır**: yok
    - **Sorumlu seçin**: API Management hizmetin adını arayın ve listeden seçin
    - **Anahtar izinleri**: yok
    - **Gizli dizi izinleri**: Get
    - **Sertifika izinleri**: yok
1. Erişim ilkesini oluşturmak için **Tamam ' ı** seçin.
1. Değişiklikleri kaydetmek için **Kaydet** ' i seçin.

Sorunun çözümlenip çözümlenmediğini denetleyin. Bunu yapmak için, Key Vault sertifikasını kullanarak API Management hizmetinde özel etki alanını oluşturmayı deneyin.

## <a name="next-steps"></a>Sonraki adımlar
API Management hizmeti hakkında daha fazla bilgi edinin:

- API Management hakkında daha fazla [videoya](https://azure.microsoft.com/documentation/videos/index/?services=api-management) göz atın.
* Arka uç hizmetinizi güvenli hale getirmeye yönelik diğer yollar için bkz. [Karşılıklı sertifika kimlik doğrulaması](api-management-howto-mutual-certificates.md).

* [API Management hizmet örneği oluşturun](get-started-create-service-instance.md).
* [Ilk API 'Nizi yönetin](import-and-publish.md).