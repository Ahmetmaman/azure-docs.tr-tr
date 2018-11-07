---
title: Azure konuk işletim sistemi için desteklenebilirlik ve kullanımdan kaldırma İlkesi Kılavuzu | Microsoft Docs
description: Hangi Microsoft olarak yönlendirilmesinin Azure konuk işletim sistemi bulut Hizmetleri tarafından kullanılan destekleyecek hakkında bilgi sağlar.
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
editor: ''
ms.assetid: 919dd781-4dc6-4e50-bda8-9632966c5458
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 9/20/2017
ms.author: raiye
ms.openlocfilehash: 6068f054a2ce695a889351b1f959319c64eb73fd
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51235607"
---
# <a name="azure-guest-os-supportability-and-retirement-policy"></a>Azure konuk işletim sistemi desteklenebilirliği ve kullanımdan kaldırma İlkesi
Azure konuk işletim sistemi için bu sayfadaki bilgileri ilgili ([konuk işletim sistemi](cloud-services-guestos-update-matrix.md)) bulut Hizmetleri worker ve web rolleri (PaaS). Sanal makineler (Iaas) için geçerli değildir.

Microsoft olan bir yayımlanan [konuk işletim sistemi için destek ilkesi](https://support.microsoft.com/gp/azure-cloud-lifecycle-faq). Okuduğunuz sayfası artık ilke nasıl uygulandığını açıklar.

İlke

1. Microsoft destekleyeceği **en az iki en son konuk işletim sistemi aileleri**. Bir aile devre dışı bırakıldığında, müşterilerin daha yeni bir desteklenen konuk işletim sistemi ailesi güncelleştirmek için resmi o tarihten itibaren 12 ay süreyle var.
2. Microsoft destekleyeceği **en az iki en son sürümü desteklenen konuk işletim sistemi aileleri**.
3. Microsoft destekleyeceği **en az iki sürümü en son Azure SDK'sı**. SDK sürümü devre dışı bırakıldığında, müşterilerinin daha yeni bir sürüme güncelleştirmek için resmi o tarihten itibaren 12 ay süreyle olacaktır.

Bazen, iki ailesi veya sürümler birden fazla desteklenebilir. Resmi konuk işletim sistemi destek bilgileri görüntülenecek [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md).

## <a name="when-a-guest-os-version-is-retired"></a>Bir konuk işletim sistemi sürümü ne zaman devre dışı bırakıldı
Yeni konuk işletim sistemi **sürümleri** ayda hakkında en son MSRC güncelleştirmeler sunulmuştur. Normal aylık güncelleştirmeler nedeniyle bir konuk işletim sistemi sürümü genellikle yaklaşık 60 gün sonra sürüm devre dışı. Bu etkinlik, kullanılabilir her ürün ailesi için en az iki konuk işletim sistemi sürümleri tutar.

### <a name="process-during-a-guest-os-family-retirement"></a>İşlem sırasında bir konuk işletim sistemi ailesi kullanımdan kaldırma
Müşterilerimizi duyurulan sonra müşterilerin eski ailesi resmi olarak hizmetinden kaldırılmadan önce "geçiş" 12 aylık dönem var. Bu geçiş süresi Microsoft kararımıza uzatılabilir. Güncelleştirmeleri denetleyecekse [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md).

Aşamalı olarak kullanımdan kaldırma işlemi, geçiş dönemi altı (6) ay başlar. Bu süre boyunca:

1. Microsoft, müşterilerinin kullanımdan kaldırma bilgilendirir.
2. Azure SDK'sının daha yeni sürümü devre dışı bırakılan konuk işletim sistemi ailesi desteklemeyeceği.
3. Yeni dağıtımlar ve bulut hizmetlerinin yeniden dağıtım gerekmeksizin devre dışı bırakılan ailesinde izin verilmiyor

Microsoft, yeni konuk işletim sistemi sürümü en son MSRC güncelleştirmeleri "sona erme tarihi" bilinen geçiş dönemin son günü kadar ekleme tanıtmak devam eder. Sona erme tarihini Cloud Services çalışan yine de Azure SLA altında desteklenmeyen. Microsoft, yükseltme, silmek veya bu tarihten sonra bu hizmetlerini durdurmak için takdirine bağlı olarak sahiptir.

### <a name="process-during-a-guest-os-version-retirement"></a>İşlem sırasında bir konuk işletim sistemi sürümü kullanımdan kaldırma
Müşteriler otomatik olarak güncelleştirmek için konuk işletim sistemi ayarlarsanız, hiçbir zaman konuk işletim sistemi sürümleri ile ne yapılacağı hakkında endişelenmeye gerek sahiptirler. Bunlar her zaman en son konuk işletim sistemi sürümü kullanıyor olacaksınız.

Konuk işletim sistemi sürümleri, her ay kullanıma sunulur. Normal yayın oranı nedeniyle, her sürüm sabit bir kullanım ömrü vardır.

Kullanım ömrü içinde 60 günde bir sürümüdür "*devre dışı*". "Devre dışı" sürüm Portalı'ndan kaldırılır anlamına gelir. Sürüm artık CSCFG yapılandırma dosyasında ayarlanabilir. Var olan dağıtımları, çalışan bırakılır. Ancak, yeni dağıtımlar ve varolan dağıtımları için kod ve yapılandırma güncelleştirmeleri değil izin verilir.

Bir süre sonra "devre dışı olma", konuk işletim sistemi sürümü "*süresi*" ve yine de bu sürümü çalıştıran tüm yüklemeleri yükseltme ve konuk işletim sistemi gelecekte otomatik olarak güncelleştirmek için force. Sona erme disablement süre sonu dönemi değişebilir şekilde toplu olarak yapılır.

Bu nokta müşteri geçişi kolaylaştırmak için Microsoft'un kararımıza uzun yapılabilir. Üzerinde herhangi bir değişiklik duyurulacaktır [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md).

### <a name="notifications-during-retirement"></a>Kullanımdan kaldırma sırasında bildirim
* **Aile devre dışı bırakma** <br>Microsoft blog gönderileri ve portal bildirimi kullanır. Yine de devre dışı bırakılan bir konuk işletim sistemi ailesi kullanan müşteriler, atanan hizmet yöneticileri için doğrudan iletişim (e-posta, portal iletileri, telefon görüşmesi) aracılığıyla bildirilir. Tüm değişiklikler için yayımlanacak [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md).
* **Sürüm devre dışı bırakma** <br>Tüm değişikliklerin ve ortaya tarihler için yayımlanacak [Azure konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md)sürüm, devre dışı bırakıldı ve süre sonu. Bunlar bir devre dışı konuk işletim sistemi sürümü veya ailesinde çalışan dağıtımlarınız varsa Hizmetleri Yöneticiler e-posta alırsınız. Bu e-postaları zamanlamasını farklılık gösterebilir. Genellikle bu zamanlama resmi SLA olmasa da en az bir ay önce disablement, benzerdir.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
**Geçiş etkilerini azaltmak ne?**

Cloud Services tasarlamaya yönelik en son konuk işletim sistemi ailesi kullanmanızı öneririz.

1. Erken geçişinizi daha yeni bir ailesine planlamaya başlayın.
2. Yeni ailesinde çalışan, bulut hizmeti test etmek için geçici test dağıtımları ayarlayın.
3. Konuk işletim sistemi sürümünüzü ayarlamak **otomatik** (osVersion = * içinde [.cscfg](cloud-services-model-and-package.md#cscfg) dosyası) otomatik olarak yeni konuk işletim sistemi sürümleri için geçiş oluşmaz.

**Ne web Uygulamam işletim sistemi ile daha derin tümleştirme gerektiriyor?**

Web uygulama mimarinizin işletim sisteminin temel alınan özelliklere bağımlı, desteklenen platform özellikleri gibi kullanın [başlangıç görevleri](cloud-services-startup-tasks.md) veya diğer genişletilebilirlik mekanizması. Alternatif olarak, ayrıca kullanabileceğiniz [Azure sanal makineler](https://azure.microsoft.com/documentation/scenarios/virtual-machines/) (Iaas – hizmet olarak altyapı), temel alınan işletim sistemin bakımından sorumlu olduğu.

## <a name="next-steps"></a>Sonraki adımlar
En son gözden [konuk işletim sistemi sürümlerinden](cloud-services-guestos-update-matrix.md).
