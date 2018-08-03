---
title: Azure portalı kullanarak içerik koruma ilkelerini yapılandırma | Microsoft Docs
description: Bu makalede, içerik koruma ilkelerini yapılandırmak için Azure portalını kullanmayı gösterir. Makaleyi de dinamik şifreleme varlıklarınız için etkinleştirme gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: 270b3272-7411-40a9-ad42-5acdbba31154
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: juliako
ms.openlocfilehash: c46faf2298ebaac4f40fb1d18cbfca83076e0d4f
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39423571"
---
# <a name="configure-content-protection-policies-by-using-the-azure-portal"></a>Azure portalı kullanarak içerik koruma ilkelerini yapılandırma
 Azure Media Services ile depolama, işleme ve teslim üzerinden bilgisayarınıza çıkışında medyanızdaki güvenliğini sağlayabilirsiniz. Media Services, dinamik olarak Gelişmiş Şifreleme Standardı (AES ile) 128 bit şifreleme anahtarları kullanılarak şifrelenir, içeriğinizi teslim etmek için kullanabilirsiniz. Ayrıca, ortak şifreleme (CENC) ile PlayReady ve/veya Widevine dijital hak yönetimi (DRM) ve Apple FairPlay kullanarak kullanabilirsiniz. 

Media Services DRM lisanslarını teslim etmek üzere bir hizmet sunar ve AES anahtarları yetkili istemcilere temizleyin. Şifrelemeler her tür için bir anahtar/lisans yetkilendirme ilkesi oluşturmak için Azure portalını kullanabilirsiniz.

Bu makalede, portalı kullanarak içerik koruma ilkesi yapılandırma gösterilmektedir. Makalede de dinamik şifreleme için varlıklarınızı uygulamak gösterilmektedir.

## <a name="start-to-configure-content-protection"></a>İçerik korumayı yapılandırma için Başlat
Media Services hesabınızı kullanarak global içerik korumayı yapılandırma için portalı kullanmak için aşağıdaki adımları uygulayın:

1. İçinde [portalı](https://portal.azure.com/), Media Services hesabınızı seçin.

1. Seçin **ayarları** > **içerik koruma**.

    ![İçerik koruma](./media/media-services-portal-content-protection/media-services-content-protection001.png)

## <a name="keylicense-authorization-policy"></a>Anahtar/lisans yetkilendirme ilkesi
Media Services anahtarını veya lisans isteğinde bulunan kullanıcıların kimliklerini doğrulama birden çok yöntemini destekler. İçerik anahtarı yetkilendirme ilkesini yapılandırmanız gerekir. Anahtar/lisans teslim edilebilir önce istemcinin ilkeyi karşılaması gerekir. İçerik anahtarı yetkilendirme ilkesinin açık veya belirteç kısıtlaması şeklinde bir veya daha fazla yetkilendirme kısıtlaması olabilir.

Şifrelemeler her tür için bir anahtar/lisans yetkilendirme ilkesi oluşturmak için portalı kullanabilirsiniz.

### <a name="open-authorization"></a>Açık yetkilendirme
Açık kısıtlama anlamına gelir Sistem anahtarı bir anahtar istekte herkese olmasını sağlar. Bu kısıtlama, test amaçları için yararlı olabilir. 

### <a name="token-authorization"></a>Yetkilendirme belirteci
Belirteç kısıtlamalı ilkenin beraberinde bir güvenlik belirteci hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Media Services, basit web belirteci (SWT) ve JSON Web Token (JWT) biçimlerindeki belirteçleri destekler. Media Services, STS sağlamaz. Özel STS oluşturma ya da Azure Access Control Service sorunu belirteçleri kullanın. STS belirteci kısıtlama yapılandırmasında belirtilen belirtilen anahtarı ve sorunu talepleri ile imzalanmış bir belirteç oluşturmak için yapılandırılmalıdır. Belirteç geçerliyse ve belirteçteki talepler eşleşen anahtar (veya lisans) için yapılandırılmış, Media Services anahtar dağıtımı hizmetiyle İstenen anahtar (veya lisansı) istemciye döndürür.

Belirteç kısıtlamalı ilkenin yapılandırdığınızda, birincil doğrulama anahtarı, veren ve İzleyici parametrelerini belirtmeniz gerekir. Birincil doğrulama anahtarı belirteç birlikte imzalandığı anahtarını içerir. Verici belirteci veren güvenli belirteç hizmetidir. Belirtecin amacı (kapsam olarak da adlandırılır) İzleyici açıklayan veya kaynak belirteci erişimini yetkilendirir. Media Services anahtar dağıtımı hizmetiyle belirtecindeki bu değerleri şablon değerleri eşleştiğini doğrular.

![Anahtar/lisans yetkilendirme ilkesi](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-license-template"></a>PlayReady lisans şablonu
PlayReady lisans şablonu PlayReady Lisansınızın etkin işlevselliği ayarlar. PlayReady lisans şablonu hakkında daha fazla bilgi için bkz. [Media Services PlayReady lisans şablonuna genel bakış](media-services-playready-license-template-overview.md).

### <a name="nonpersistent"></a>Kalıcı olmayan
Bir lisans kalıcı olarak yapılandırırsanız, oyuncunun lisans kullanırken bellekte tutulur.  

![Kalıcı olmayan içerik koruma](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Kalıcı
Bir lisans kalıcı olarak yapılandırırsanız, istemcide kalıcı depolama alanında kaydedilir.

![Kalıcı içerik koruma](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-license-template"></a>Widevine lisans şablonu
Widevine lisans şablonu etkin işlevselliği, Widevine lisansları ayarlar.

### <a name="basic"></a>Temel
Seçtiğinizde, **temel**, şablonu varsayılan değerlerle oluşturulur.

### <a name="advanced"></a>Gelişmiş
Widevine haklar şablonu hakkında daha fazla bilgi için bkz: [Widevine lisans şablonuna genel bakış](media-services-widevine-license-template-overview.md).

![Gelişmiş içerik koruma](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>FairPlay yapılandırması
FairPlay şifrelemesini etkinleştirmek için işaretleyin **FairPlay Yapılandırması**. Ardından **App sertifikası** girin **uygulama gizli anahtarı**. FairPlay yapılandırması ve gereksinimleri hakkında daha fazla bilgi için bkz. [HLS Apple FairPlay veya Microsoft PlayReady ile içerik koruma](media-services-protect-hls-with-FairPlay.md).

![FairPlay yapılandırması](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>Dinamik şifreleme varlığınız için geçerlidir.
Dinamik şifrelemeden yararlanmak için kaynak dosyanızı Uyarlamalı bit hızı MP4 dosyaları kümesine kodlayın.

### <a name="select-an-asset-that-you-want-to-encrypt"></a>Şifrelemek istediğiniz bir varlık seçin
Varlıklarınızın tümünü görmek için seçin **ayarları** > **varlıklar**.

![Varlıklar seçeneği](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>AES veya DRM ile şifreleme
Seçtiğinizde, **şifrele** bir varlık için iki seçenek görürsünüz: **AES** veya **DRM**. 

#### <a name="aes"></a>AES
AES anahtar şifrelemesi tüm akış protokollerinde etkin Temizle: kesintisiz akış, HLS ve MPEG-DASH.

![Şifreleme yapılandırması](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM
1. Seçtikten sonra **DRM**, (Bu noktaya kadar yapılandırılmalıdır) farklı içerik koruması ilkeleri ve akış protokollerine bir dizi görürsünüz:

    a. **MPEG-DASH ile PlayReady ve Widevine** , MPEG-DASH akışında PlayReady ve Widevine benzeri DRM ile dinamik olarak şifreler.

    b. **PlayReady ve MPEG-DASH ile Widevine + HLS ile FairPlay** , MPEG-DASH akışında PlayReady ve Widevine benzeri DRM ile dinamik olarak şifreleyin. Bu seçenek ayrıca akışlarınız HLS ile FairPlay şifreler.

    c. **Yalnızca kesintisiz akış, HLS ve MPEG-DASH ile PlayReady** kesintisiz akış, HLS ve MPEG-DASH akışları PlayReady DRM ile dinamik olarak şifreler.

    d. **MPEG-DASH ile yalnızca Widevine** MPEG-DASH Widevine DRM ile dinamik olarak şifreler.
    
    e. **HLS ile yalnızca FairPlay** akışınız HLS ile FairPlay dinamik olarak şifreler.

1. FairPlay şifrelemesini etkinleştirmek için **Content Protection genel ayarları** dikey penceresinde **FairPlay Yapılandırması**. Ardından **App sertifikası**girin **uygulama gizli anahtarı**.

    ![Şifreleme türü](./media/media-services-portal-content-protection/media-services-content-protection009.png)

1. Şifreleme seçiminizi yaptıktan sonra seçin **Uygula**.

>[!NOTE] 
>Safari'de AES şifreli HLS yürütme planlıyorsanız, blog gönderisine bakın [şifrelenmiş HLS Safari'de](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

