---
title: MacOS-Azure ' dan Windows sanal masaüstüne (klasik) bağlanma
description: MacOS istemcisini kullanarak Windows sanal masaüstüne (klasik) bağlanma.
author: Heidilohr
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: a0d2bd6a07cfc9e5ebe205322550c9e898022846
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "88008449"
---
# <a name="connect-to-windows-virtual-desktop-classic-with-the-macos-client"></a>MacOS istemcisiyle Windows sanal masaüstüne (klasik) bağlanma

> Uygulama hedefi: macOS 10,12 veya üzeri

>[!IMPORTANT]
>Bu içerik, Windows sanal masaüstü nesneleri Azure Resource Manager desteklemeyen Windows sanal masaüstü (klasik) için geçerlidir. Azure Resource Manager Windows sanal masaüstü nesnelerini yönetmeye çalışıyorsanız, [Bu makaleye](../connect-macos.md)bakın.

Windows sanal masaüstü kaynaklarına, macOS cihazlarınızdan indirilebilir istemcimizi kullanarak erişebilirsiniz. Bu kılavuz, istemcisini nasıl ayarlayacağınızı bildirir.

## <a name="install-the-client"></a>İstemciyi yükleme

Başlamak için, istemciyi macOS cihazınıza [indirin](https://apps.apple.com/app/microsoft-remote-desktop/id1295203466?mt=12) ve yükleyin.

## <a name="subscribe-to-a-feed"></a>Bir akışa abone olma

Size, macOS cihazınızda kullanabileceğiniz yönetilen kaynakların listesini almak için yöneticinize verdiği akışa abone olun.

Bir akışa abone olmak için:

1. Hizmete bağlanmak ve kaynaklarınızı almak için ana sayfada **çalışma alanı Ekle** ' yi seçin.
2. Akış URL 'sini girin. Bu bir URL veya e-posta adresi olabilir:
   - Bir URL kullanıyorsanız, yöneticinizin size verdiği bir URL 'yi kullanın. Normalde, URL olur <https://rdweb.wvd.microsoft.com> .
   - E-posta kullanmak için e-posta adresinizi girin. Bu, yönetici sunucuyu bu şekilde yapılandırdıysa, istemciye e-posta adresinizle ilişkili bir URL aramasını söyler.
3. **Add (Ekle)** seçeneğini belirleyin.
4. İstendiğinde kullanıcı hesabınızla oturum açın.

Oturum açtıktan sonra, kullanılabilir kaynakların bir listesini görmeniz gerekir.

Bir akışa abone olduktan sonra, akışın içeriği düzenli aralıklarla otomatik olarak güncelleşmeyecektir. Kaynaklar, yöneticiniz tarafından yapılan değişikliklere göre eklenebilir, değiştirilebilir veya kaldırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

MacOS istemcisi hakkında daha fazla bilgi edinmek için [MacOS istemcisini kullanmaya başlama](/windows-server/remote/remote-desktop-services/clients/remote-desktop-mac/) belgesine göz atın.