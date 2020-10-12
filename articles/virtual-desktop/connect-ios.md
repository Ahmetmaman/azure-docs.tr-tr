---
title: İOS 'tan Windows sanal masaüstüne bağlanma-Azure
description: İOS istemcisini kullanarak Windows sanal masaüstüne bağlanma.
author: Heidilohr
ms.topic: how-to
ms.date: 02/08/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 87bb2cc53ce056552e8f44aac4ade96e603a8787
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89230050"
---
# <a name="connect-to-windows-virtual-desktop-with-the-ios-client"></a>İOS istemcisiyle Windows sanal masaüstüne bağlanma

> Uygulama hedefi: iOS 13,0 veya üzeri. İPhone, iPad ve iPod Touch ile uyumludur.

>[!IMPORTANT]
>Bu içerik Azure Resource Manager Windows sanal masaüstü nesneleri ile Windows sanal masaüstü için geçerlidir. Azure Resource Manager nesneleri olmadan Windows sanal masaüstü (klasik) kullanıyorsanız, [Bu makaleye](./virtual-desktop-fall-2019/connect-ios-2019.md)bakın.

Windows sanal masaüstü kaynaklarına, indirilebilir istemcimiz ile iOS cihazınızdan erişebilirsiniz. Bu kılavuz, iOS istemcisini nasıl ayarlayacağınızı bildirir.

## <a name="install-the-ios-client"></a>İOS istemcisini yükler

Başlamak için, istemcisini iOS cihazınıza [indirin](https://aka.ms/rdios) ve yükleyin.

## <a name="subscribe-to-a-feed"></a>Bir akışa abone olma

İOS cihazınıza erişebileceğiniz yönetilen kaynakların listesini almak için yöneticiniz tarafından sunulan akışa abone olun.

Bir akışa abone olmak için:

1. Bağlantı merkezi 'nde **+** , ve ardından **çalışma alanı Ekle**' ye dokunun.
2. Akış **URL 'si alanına AKıŞ** URL 'sini girin. Akış URL 'SI ya bir URL ya da bir e-posta adresi olabilir.
   - Bir URL kullanıyorsanız, yöneticinizin size verdiği bir URL 'yi kullanın. Normalde, URL olur <https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery> .
   - E-posta kullanmak için e-posta adresinizi girin. Bu, yönetici sunucuyu bu şekilde yapılandırdıysa, istemciye e-posta adresinizle ilişkili bir URL aramasını söyler.
   - US Gov portalı üzerinden bağlanmak için kullanın <https://rdweb.wvd.azure.us/api/arm/feeddiscovery> .
3. **İleri**' ye dokunun.
4. İstendiğinde kimlik bilgilerinizi sağlayın.
   - **Kullanıcı adı**için, Kullanıcı adına kaynaklara erişim izni verin.
   - **Parola**için Kullanıcı adıyla ilişkili parolayı verin.
   - Ayrıca, yöneticinizin kimlik doğrulamasını bu şekilde yapılandırdıysa ek etmenler sağlamanız istenebilir.
5. **Kaydet**'e dokunun.

Bundan sonra bağlantı merkezi, uzak kaynakları görüntülemelidir.

Akışa abone olduktan sonra akışın içeriği düzenli aralıklarla otomatik olarak güncelleştirilecek. Kaynaklar, yöneticiniz tarafından yapılan değişikliklere göre eklenebilir, değiştirilebilir veya kaldırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

İOS istemcisini kullanma hakkında daha fazla bilgi edinmek için [iOS istemcisi ile çalışmaya başlama](/windows-server/remote/remote-desktop-services/clients/remote-desktop-ios/) belgelerini inceleyin.
