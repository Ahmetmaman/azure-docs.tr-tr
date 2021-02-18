---
title: Azure kurumsal maliyet görünümleri ile ilgili sorunları giderme
description: Azure portalındaki kurumsal maliyet görünümleriyle ilgili karşılaşabileceğiniz sorunların nasıl çözüleceğini öğrenin.
author: bandersmsft
ms.reviewer: amberb
ms.service: cost-management-billing
ms.subservice: enterprise
ms.topic: troubleshooting
ms.date: 08/20/2019
ms.author: banders
ms.custom: seodec18
ms.openlocfilehash: d78621697d53f429624dc22a8e91ba4367dcfd18
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "101093974"
---
# <a name="troubleshoot-enterprise-cost-views"></a>Kurumsal maliyet görünümleri ile ilgili sorunları giderme

Kurumsal kayıtlar içinde, kayıttaki kullanıcıların maliyetleri görmemesine neden olabilecek birçok ayar vardır.  Bu ayarlar, kayıt yöneticisi tarafından yönetilir. Veya kayıt doğrudan Microsoft üzerinden satın alınmadıysa ayarlar iş ortağı tarafından yönetilir.  Bu makale, ayarların ne olduğunu ve kaydı nasıl etkilediğini anlamanıza yardımcı olur. Bu ayarlar Azure rollerinden bağımsızdır.

## <a name="enable-access-to-costs"></a>Maliyetlere erişimi etkinleştirme

Maliyet bilgilerini ararken Yetkisiz veya *“Kaydınızda maliyet görünümleri devre dışı bırakıldı.”* gibi bir ileti görüyor musunuz?
![Abonelik için Geçerli Maliyet alanında “yetkisiz” ifadesini gösteren ekran görüntüsü.](./media/enterprise-mgmt-grp-troubleshoot-cost-view/unauthorized.png)

Bunun nedeni aşağıdakilerden biri olabilir:

1. Azure’u bir kurumsal iş ortağı aracılığıyla satın aldınız ve iş ortağı henüz fiyatlandırmayı yayınlamadı. [Enterprise portal](https://ea.azure.com) içinde fiyatlandırma ayarını güncelleştirmesi için iş ortağınızla görüşün.
2. EA Direct müşterisiyseniz birkaç olasılık vardır:
    * Hesap Sahibisiniz ve Kayıt Yöneticiniz, **Hesap Sahibi için ücretleri görüntüleme** ayarını devre dışı bıraktı.  
    * Bölüm Yöneticisisiniz ve Kayıt Yöneticiniz, **Bölüm Yöneticisi için ücretleri görüntüleme** ayarını devre dışı bıraktı.
    * Erişim elde etmek için Kayıt Yöneticinizle görüşün. Kayıt Yöneticisi, [Enterprise portal](https://ea.azure.com/manage/enrollment)’daki ayarları güncelleştirebilir.

      ![Ücretleri görüntüleme için Enterprise Portal Ayarlarını gösteren ekran görüntüsü.](./media/enterprise-mgmt-grp-troubleshoot-cost-view/ea-portal-settings.png)

## <a name="asset-is-unavailable"></a>Varlık kullanılamıyor

Bir abonelik veya yönetim grubuna erişmeye çalışırken **Bu varlık kullanılamıyor** ifadesini gösteren bir hata iletisi alırsanız, bu öğeyi görüntülemek için doğru role sahip değilsiniz demektir.  

!["Varlık kullanılamıyor" iletisini gösteren ekran görüntüsü.](./media/enterprise-mgmt-grp-troubleshoot-cost-view/asset-not-found.png)

Azure abonelik veya yönetim grubu yöneticinizden erişim izni vermesini isteyin. Daha fazla bilgi için bkz. [Azure Portal kullanarak Azure rolleri atama](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Sonraki adımlar
- Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).
