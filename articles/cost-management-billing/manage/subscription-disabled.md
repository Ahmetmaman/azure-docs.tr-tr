---
title: Devre dışı bırakılmış bir Azure aboneliğini yeniden etkinleştirme
description: Bir Azure aboneliğinizin ne zaman devre dışı bırakılabileceğini ve nasıl yeniden etkinleştirilebileceğini açıklar.
keywords: azure aboneliği devre dışı bırakıldı
author: bandersmsft
ms.reviewer: amberb
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 11/17/2020
ms.author: banders
ms.openlocfilehash: cad3082981bcfc699bc230badf44e2ffc2e1bed3
ms.sourcegitcommit: c2dd51aeaec24cd18f2e4e77d268de5bcc89e4a7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2020
ms.locfileid: "94744434"
---
# <a name="reactivate-a-disabled-azure-subscription"></a>Devre dışı bırakılmış bir Azure aboneliğini yeniden etkinleştirme

Kredinizin süresi dolduğu, harcama limitinize ulaştığınız, süresi geçmiş bir faturanız olduğu, kredi kartı limitinize ulaştığınız veya abonelik Hesap Yöneticisi tarafından iptal edildiği için Azure aboneliğiniz devre dışı bırakılabilir. Hangi sorunun sizin için geçerli olduğunu görün ve aboneliğinizin yeniden etkinleştirilmesini sağlamak için bu makaledeki adımları izleyin.

## <a name="your-credit-is-expired"></a>Kredinizin süresi doldu

Azure ücretsiz hesabına kaydolduğunuzda size 30 gün boyunca kullanmak üzere 200 ABD doları Azure kredisi ve 12 ay boyunca ücretsiz hizmetler sağlayan bir Ücretsiz Deneme aboneliği alırsınız. 30 günün sonunda Azure aboneliğinizi devre dışı bırakır. Aboneliğiniz, abonelikle sağlanan krediyi ve ücretsiz hizmetleri yanlışlıkla aşan kullanım için ücretlendirilmeye karşı sizi korumak için devre dışı bırakılır. Azure hizmetlerini kullanmaya devam etmek için [aboneliğinizi yükseltmelisiniz](upgrade-azure-subscription.md). Yükseltme sonrasında aboneliğinizin yine 12 ay boyunca ücretsiz hizmetlere erişimi olacaktır. Yalnızca ücretsiz hizmetleri ve miktarları aşan kullanım için ücretlendirilirsiniz.

## <a name="you-reached-your-spending-limit"></a>Harcama limitinize ulaştınız

Ücretsiz Deneme ve Visual Studio Enterprise gibi krediler içeren Azure abonelikleri için harcama limitleri vardır. Başka bir deyişle yalnızca dahil edilen krediye kadar olan hizmetleri kullanabilirsiniz. Kullanımınız harcama limitine ulaştığında Azure o faturalama döneminin kalanı boyunca aboneliğinizi devre dışı bırakır. Aboneliğiniz, abonelikle sağlanan krediyi yanlışlıkla aşan kullanım için ücretlendirilmeye karşı sizi korumak için devre dışı bırakılır. Harcama limitinizi kaldırmak için bkz. [Hesap Merkezinde harcama limitini kaldırma](spending-limit.md#remove).

> [!NOTE]
> Ücretsiz Deneme aboneliğiniz varsa ve harcama limitini kaldırırsanız, aboneliğiniz Ücretsiz Deneme süresinin sonunda kullandıkça öde fiyatlarına tabi bireysel aboneliğe dönüştürülür. Kalan kredinizi, aboneliği oluşturduğunuz tarihten itibaren 30 gün boyunca korursunuz. Ayrıca 12 ay boyunca ücretsiz hizmetlere erişmeye devam edebilirsiniz.

Azure'ın faturalama etkinliğini izlemek ve yönetmek için bkz. [Azure maliyetlerini yönetmeyi planlama](../understand/plan-manage-costs.md).


## <a name="your-bill-is-past-due"></a>Faturanızın ödeme süresi geçmiş

Süresi geçen bakiye sorununu çözmek için bkz.[Azure’dan e-posta aldıktan sonra Azure aboneliğiniz için süresi geçen bakiye sorununu çözme](resolve-past-due-balance.md).

## <a name="the-bill-exceeds-your-credit-card-limit"></a>Fatura, kredi kartı limitinizi aşıyor

Bu sorunu çözmek için [farklı bir kredi kartına geçin](change-credit-card.md). Öte yandan bir işletmeyi temsil ediyorsanız [faturayla ödemeye geçebilirsiniz](pay-by-invoice.md).

## <a name="the-subscription-was-accidentally-canceled"></a>Abonelik yanlışlıkla iptal edildi

Hesap Yöneticisiyseniz ve kullandıkça öde fiyatlarına tabi bir bireysel aboneliği yanlışlıkla iptal ettiyseniz, Hesap Merkezinde aboneliği yeniden etkinleştirebilirsiniz.

1. [Hesap Merkezi](https://account.windowsazure.com/Subscriptions)’nde oturum açın.
1. İptal edilen aboneliği seçin.
1. **Yeniden etkinleştir**’e tıklayın.

    ![Sağ bölmedeki yeniden etkinleştir bağlantılarını gösteren ekran görüntüsü](./media/subscription-disabled/reactivate-sub.png)

Diğer abonelik türleri için [destek ekibiyle iletişime geçerek](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) aboneliğinizin yeniden etkinleştirilmesini sağlayın.

## <a name="after-reactivation"></a>Yeniden etkinleştirme sonrasında

Aboneliğiniz yeniden etkinleştirildikten sonra kaynakların oluşturulmasında ve yönetilmesinde gecikme olabilir. Gecikme 30 dakikayı aşarsa yardım için lütfen [Azure Faturalama Desteği](https://go.microsoft.com/fwlink/?linkid=2083458)’ne başvurun. Azure kaynaklarının çoğu otomatik olarak serbest bırakılır ve hiçbir eylem gerektirmez. Bununla birlikte Azure hizmet kaynaklarınızı denetlemenizi ve otomatik olarak serbest bırakılmamış olanları yeniden başlatmanızı öneririz.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar
- [Azure maliyetlerini yönetmeyi planlama](../understand/plan-manage-costs.md) işleminin nasıl yapıldığını öğrenin.
