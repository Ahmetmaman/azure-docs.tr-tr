---
title: '& aktarma HSM korumalı anahtarlar oluşturma – Azure Key Vault'
description: Azure Key Vault ile kullanmak için kendi HSM korumalı anahtarlarınızı nasıl planlayacağınızı, oluşturacağınızı ve aktaracağınızı öğrenin. BYOK olarak da bilinir veya kendi anahtarınızı getir.
services: key-vault
author: amitbapat
manager: devtiw
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: tutorial
ms.date: 05/29/2020
ms.author: ambapat
ms.openlocfilehash: f75fbdd2d9860598f72014159fa5287360e106c3
ms.sourcegitcommit: 65d518d1ccdbb7b7e1b1de1c387c382edf037850
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2020
ms.locfileid: "94372821"
---
# <a name="import-hsm-protected-keys-to-key-vault"></a>HSM korumalı anahtarları Key Vault’a içeri aktarma

Ek güvence için, Azure Key Vault kullandığınızda, donanım güvenlik modüllerinde (HSM 'ler), hiçbir zaman HSM sınırını bırakmamış anahtarlar içeri aktarabilir veya oluşturabilirsiniz. Bu senaryo genellikle *kendi anahtarını getir* veya KAG olarak adlandırılır. Azure Key Vault anahtarlarınızı korumak için HSM 'lerin nCipher nShield ailesini (FIPS 140-2 düzey 2 doğrulanan) kullanır.

Bu işlev, Azure Çin 21Vianet için kullanılamaz.

> [!NOTE]
> Azure Key Vault hakkında daha fazla bilgi için bkz. [Azure Key Vault nedir?](../general/overview.md)  
> HSM korumalı anahtarlar için Anahtar Kasası oluşturmayı içeren bir başlangıç öğreticisi için bkz. [Azure Key Vault nedir?](../general/overview.md).

## <a name="supported-hsms"></a>Desteklenen HSM 'ler

HSM korumalı anahtarların Key Vault aktarmak, kullandığınız HSMs 'ye bağlı olarak iki farklı yöntem aracılığıyla desteklenir. Aşağıdaki tabloyu kullanarak, HSMs 'nizin oluşturması için hangi yöntemin kullanılacağını ve ardından Azure Key Vault ile kullanmak üzere kendi HSM korumalı anahtarlarınızı aktarmanızı öğrenin. 

|Satıcı adı|Satıcı türü|Desteklenen HSM modelleri|Desteklenen HSM-anahtar aktarım yöntemi|
|---|---|---|---|
|[nCipher](https://www.ncipher.com/products/key-management/cloud-microsoft-azure)|Üreticisini<br/>Hizmet olarak HSM|<ul><li>HSM 'lerin nShield ailesi</li><li>hizmet olarak nShield</ul>|**Yöntem 1:** [nCipher bYok](hsm-protected-keys-ncipher.md) (anahtar içeri aktarma ve HSM doğrulaması için güçlü kanıtlama ile)<br/>**Yöntem 2:** [Yeni bYok yöntemini kullanma](hsm-protected-keys-byok.md) |
|Thales|Üretici|<ul><li>Bellenim sürüm 7,3 veya daha yeni bir sürümü içeren Luna HSM 7 ailesi</li></ul>| [Yeni BYOK yöntemi kullan](hsm-protected-keys-byok.md)|
|Fortanx|Üreticisini<br/>Hizmet olarak HSM|<ul><li>Self-Defending anahtar yönetim hizmeti (SDKMS)</li><li>Equinix SmartKey</li></ul>|[Yeni BYOK yöntemi kullan](hsm-protected-keys-byok.md)|
|Marvell|Üretici|Tüm LiquidSecurity HSM 'leri<ul><li>Bellenim sürümü 2.0.4 veya üzeri</li><li>Bellenim sürüm 3,2 veya daha yenisi</li></ul>|[Yeni BYOK yöntemi kullan](hsm-protected-keys-byok.md)|
|Cryptomathic|ISV (Kurumsal anahtar yönetim sistemi)|Birden çok HSM markalarını ve modellerini kapsayan<ul><li>nCipher</li><li>Thales</li><li>Utıco</li></ul>[Ayrıntılar için bkz. Cryptomathic sitesi](https://www.cryptomathic.com/azurebyok)|[Yeni BYOK yöntemi kullan](hsm-protected-keys-byok.md)|
|Securosys SA|Üretici, hizmet olarak HSM|Primus HSM ailesi, Securosys bulutları HSM|[Yeni BYOK yöntemi kullan](hsm-protected-keys-byok.md)|
|||||

## <a name="next-steps"></a>Sonraki adımlar

* Anahtarlarınız için güvenlik, dayanıklılık ve izleme sağlamak üzere [en iyi Key Vault uygulamaları](../general/best-practices.md) izleyin.
* Yeni BYOK yönteminin tamamen açıklaması için [bYok belirtimine](./byok-specification.md) başvurun