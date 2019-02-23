---
title: Azure uygulamaları yönetilen uygulama teklifi yayımlama Kılavuzu
description: Bu makalede Market'te bir yönetilen uygulama yayımlama için gereksinimleri anlatılmaktadır.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: ellacroi
manager: nunoc
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 12/19/2018
ms.author: ellacroi
ms.openlocfilehash: 432a65771727b68dab23fbaeee1e3a08f7759420
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56733087"
---
# <a name="azure-applications-managed-application-offer-publishing-guide"></a>Azure uygulamaları için: Yönetilen uygulama teklifi yayımlama Kılavuzu

Yönetilen bir uygulamaya bir çözüm Market'te yayımlamak için temel yollarından biridir. Bu teklif gereksinimlerini anlamak için bu kılavuzu kullanın. 

Hangi dağıtılır ve faturalandırılır Market aracılığıyla işlem teklifler şunlardır. Bir kullanıcının gördüğü eylem çağrısı "Şimdi edinin." olan

Azure uygulama kullanma: yönetilen uygulama Teklif türü aşağıdaki koşullar gerekli olduğunda:
- Her bir abonelik tabanlı çözüm müşteriniz için bir VM veya Iaas tabanlı tüm bir çözümü kullanarak dağıtırsınız.
- Siz veya müşteriniz gerektiren çözüm iş ortağı tarafından yönetiliyor.

>[!NOTE]
>Örneğin, bir iş ortağı bir sı veya yönetilen hizmet sağlayıcısı (MSP) olabilir.  

## <a name="managed-application-offer"></a>Yönetilen uygulama teklifi

|Gereksinimler |Ayrıntılar  |
|---------|---------|
|Bir müşterinin Azure aboneliğinize dağıtılır | Yönetilen uygulamaları müşteri aboneliğinde dağıtılmış olması gereken ve 3. taraf tarafından yönetilebilir. | 
|Faturalama ve ölçüm    |  Müşterinin Azure aboneliğinde kaynakları sağlanır. Kullandıkça Öde (DÖNÜŞTÜREBİLMEMİZ) sanal makineler aracılığıyla müşterinin Azure aboneliği (DÖNÜŞTÜREBİLMEMİZ) faturalandırılır. Microsoft, müşteri ile tamamlanmasını 
Getirin-kendi lisansını söz konusu olduğunda, Microsoft Müşteri aboneliğinde altyapı maliyetleri faturalandırır sırasında müşteriye ücretleri doğrudan lisans transact        |
|Azure ile uyumlu sanal sabit disk (VHD)    |   Windows veya Linux Vm'leri oluşturulmalıdır.<ul> <ul> <li>Linux VHD'si oluşturma hakkında daha fazla bilgi için bkz. [Azure'da desteklenen Linux dağıtımı](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros).</li> <li>Bir Windows VHD oluşturma hakkında daha fazla bilgi için bkz. [Azure ile uyumlu bir VHD oluşturma](./cloud-partner-portal/virtual-machine/cpp-create-vhd.md).</li> </ul> |

>[!NOTE]
> Yönetilen uygulamalar Market aracılığıyla dağıtılabilir olmalıdır. Müşteri iletişimi önemliyse, müşteri paylaşımı etkinleştirdikten sonra sonra isteyen müşterilere ulaşmanızı.  


## <a name="next-steps"></a>Sonraki adımlar
Zaten yapmadıysanız, 

- [Kayıt](https://azuremarketplace.microsoft.com/sell) Market'te.

Kayıtlı ve yeni bir teklif oluşturur veya mevcut bir proje üzerinde çalışmaya,

- [Bulut iş ortağı portalında oturum açın](https://cloudpartner.azure.com) oluşturmak veya teklifiniz tamamlayın.
