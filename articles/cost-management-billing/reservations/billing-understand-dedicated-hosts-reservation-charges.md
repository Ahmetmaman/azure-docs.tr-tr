---
title: Azure Ayrılmış Konakları Ayrılmış Örnekleri indirimini anlama
description: Azure Ayrılmış VM Örneği indiriminin Azure Ayrılmış Konaklarına nasıl uygulandığını öğrenin.
author: yashesvi
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 02/28/2020
ms.author: banders
ms.openlocfilehash: 8d273aae3588a006f7b0a55d2798b7a43c040c9b
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92148374"
---
# <a name="how-the-azure-reservation-discount-is-applied-to-azure-dedicated-hosts"></a>Azure rezervasyon indiriminin Azure Ayrılmış Konaklarına nasıl uygulandığını öğrenin

Azure Ayrılmış Konak Örneği satın almanızın ardından, rezervasyonun öznitelikleriyle ve miktarıyla eşleşen ayrılmış konaklara otomatik olarak rezervasyon indirimi uygulanır. Rezervasyon, ayrılmış konakların işlem maliyetlerini kapsar.

## <a name="how-reservation-discount-is-applied"></a>Rezervasyon indiriminin uygulanması

Rezervasyon indirimi “*kullanılmadığı takdirde hakkı kaybedilen*” bir özelliktir. Bu nedenle, herhangi bir saat için eşleşen kaynaklarınız yoksa, o saate ait rezervasyon miktarını kaybedersiniz. Kullanılmayan ayrılmış saatleri devredemezsiniz.

Bir ayrılmış konağı sildiğinizde rezervasyon indirimi, belirtilen kapsamdaki başka bir eşleşen kaynağa otomatik olarak uygulanır. Belirtilen kapsamda eşleşen kaynak bulunamazsa, ayrılan saatler *kaybedilir*.

## <a name="reservation-discount-for-dedicated-hosts"></a>Ayrılmış Konaklar için rezervasyon indirimi

Azure Ayrılmış Konaklar Örneği, ayrılmış konaklarınızla kullanılan işlem altyapısının maliyetinde size indirim sağlar. Ayrılmış konakların sanal makineler tarafından kullanılıp kullanılmamasına bakılmaksızın bunlara indirim uygulanır. Ayrılmış konağa dağıtılan sanal makinenin lisanslama, ağ kullanımı veya depolama tüketimi gibi ek maliyetler, rezervasyon kapsamına girmez.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız  [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure rezervasyonları nedir?](./save-compute-costs-reservations.md)

- [Azure Ayrılmış Konaklarını kullanma](../../virtual-machines/dedicated-hosts.md)

- [Ayrılmış Konaklar Fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/dedicated-host/)

- [Azure rezervasyonlarını yönetme](./manage-reserved-vm-instance.md)

- [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlama](./understand-reserved-instance-usage.md)

- [Kurumsal kaydınız için rezervasyon kullanımını anlama](./understand-reserved-instance-usage-ea.md)

- [CSP abonelikleri için rezervasyon kullanımını anlama](/partner-center/azure-reservations)