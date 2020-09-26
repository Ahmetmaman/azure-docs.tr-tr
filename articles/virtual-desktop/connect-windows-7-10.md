---
title: Windows sanal masaüstü Windows 10 veya 7-Azure 'a bağlanma
description: Windows masaüstü istemcisini kullanarak Windows sanal masaüstüne bağlanma.
author: Heidilohr
ms.topic: how-to
ms.date: 09/22/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: c9da2acac0957d7fe06d0249775fbed73b5f458d
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91287347"
---
# <a name="connect-with-the-windows-desktop-client"></a>Windows Masaüstü istemcisine bağlanma

> Uygulama hedefi: Windows 10, Windows 10 IoT Enterprise ve Windows 7

>[!IMPORTANT]
>Bu içerik Azure Resource Manager Windows sanal masaüstü nesneleri ile Windows sanal masaüstü için geçerlidir. Azure Resource Manager nesneleri olmadan Windows sanal masaüstü (klasik) kullanıyorsanız, [Bu makaleye](./virtual-desktop-fall-2019/connect-windows-7-10-2019.md)bakın.

Windows Masaüstü istemcisi 'ni kullanarak Windows 10, Windows 10 IoT Enterprise ve Windows 7 ile cihazlarda Windows sanal masaüstü kaynaklarına erişebilirsiniz. İstemci, Windows 8 veya Windows 8.1 desteklemez.

## <a name="install-the-windows-desktop-client"></a>Windows masaüstü istemcisini yükler

Windows sürümünüzle eşleşen istemciyi seçin:

- [Windows 64-bit](https://go.microsoft.com/fwlink/?linkid=2068602)
- [Windows 32-bit](https://go.microsoft.com/fwlink/?linkid=2098960)
- [Windows ARM64](https://go.microsoft.com/fwlink/?linkid=2098961)

Yönetici hakları gerektirmeyen geçerli kullanıcı için istemcisini yükleyebilir veya yöneticiniz, cihazdaki tüm kullanıcıların erişebilmesi için istemciyi yükleyip yapılandırabilir.

Yüklendikten sonra istemci, **Uzak Masaüstü**araması yaparak başlangıç menüsünden başlatılabilir.

> [!IMPORTANT]
> Windows sanal masaüstü, RemoteApp ve Masaüstü bağlantıları (ÇDC) istemcisini veya Uzak Masaüstü Bağlantısı (MSTSC) istemcisini desteklemez.

## <a name="subscribe-to-a-workspace"></a>Çalışma alanına abone olma

Bir çalışma alanına abone olabileceğiniz iki yol vardır. İstemci, iş veya okul hesabınızdan size sunulan kaynakları bulmayı deneyebilir ya da kaynaklarınızın istemci tarafından bulamadığı durumlar için olduğu durumlarda doğrudan URL 'YI belirtebilirsiniz. Bir çalışma alanına abone olduktan sonra, kaynakları aşağıdaki yöntemlerden birini kullanarak başlatabilirsiniz:

- Bağlantı merkezine gidin ve başlatmak için bir kaynağa çift tıklayın.
- Ayrıca Başlat menüsüne gidebilir ve çalışma alanı adına sahip bir klasörü arayabilir ya da arama çubuğuna kaynak adını girebilirsiniz.

### <a name="subscribe-with-a-user-account"></a>Kullanıcı hesabıyla abone olma

1. İstemcinin ana sayfasından **abone ol**' u seçin.
2. İstendiğinde kullanıcı hesabınızla oturum açın.
3. Kaynaklar bağlantı merkezinde görünür ve çalışma alanına göre gruplandırılır.

>[!NOTE]
>Windows istemcisi otomatik olarak Windows sanal masaüstü 'Nü (klasik) varsayılan olarak belirler. Ancak, istemci kullanıcının Azure Resource Manager kaynaklara sahip olduğunu algılarsa, kaynakları otomatik olarak ekler veya kullanıcılara kullanılabilir olduğunu bildirir.

### <a name="subscribe-with-a-url"></a>URL ile abone olma

1. İstemcinin ana sayfasında **URL Ile abone ol**' u seçin.
2. Çalışma alanı URL 'sini veya e-posta adresinizi girin:
   - **Çalışma alanı URL 'sini**kullanıyorsanız, yöneticinizin size verdiği bir tane kullanın. Windows sanal masaüstünden kaynaklara erişim için aşağıdaki URL 'Lerden birini kullanabilirsiniz:
     - Windows sanal masaüstü (klasik): `https://rdweb.wvd.microsoft.com/api/feeddiscovery/webfeeddiscovery.aspx`
     - Windows sanal masaüstü: `https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery`
     - Windows sanal masaüstü (US Gov): `https://rdweb.wvd.azure.us/api/arm/feeddiscovery`
   - Bunun yerine **e-posta** alanını kullanıyorsanız, e-posta adresinizi girin. Bu, yönetici [e-posta bulmayı](/windows-server/remote/remote-desktop-services/rds-email-discovery)ayarlamışsa, istemciye e-posta adresinizle ILIŞKILI bir URL aramasını söyler.
3. **İleri**’yi seçin.
4. İstendiğinde kullanıcı hesabınızla oturum açın.
5. Kaynaklar, bağlantı merkezinde, çalışma alanına göre gruplanmış olarak görünmelidir.

## <a name="next-steps"></a>Sonraki adımlar

Windows Masaüstü istemcisinin nasıl kullanılacağı hakkında daha fazla bilgi edinmek için, [Windows Masaüstü istemcisi ile çalışmaya başlama](/windows-server/remote/remote-desktop-services/clients/windowsdesktop/)konusunu inceleyin.
