---
title: Tehdit algılama, Azure Güvenlik Merkezi'nde | Microsoft Docs
description: " Tehditleri algılamak ve Microsoft Cloud App Security, Azure Güvenlik Merkezi ile tümleştirmek tarafından kötü amaçlı. "
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: c42d02e4-201d-4a95-8527-253af903a5c6
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2018
ms.author: rkarlin
ms.openlocfilehash: d6777e187c04ef9a2f03e4ae813476f5a1093156
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44724220"
---
# <a name="ueba-for-azure-resources-and-users"></a>Azure kaynaklarını ve kullanıcılar için UEBA 

Kullanıcı ve varlık davranış çözümlemesinden (UEBA) Azure kaynakları ve kullanıcıları (Azure etkinlik) dayalı uyarılar getirmek için Microsoft Cloud App Security ile Azure Güvenlik Merkezi'nde iş ortağı. Bu uyarılar anomalileri içinde kullanıcı davranışlarını algılayın ve kullanıcı ve varlık davranış analizi ve makine Gelişmiş tehdit algılama, aboneliklerinizi etkinliklerinde hemen çalıştırabilirsiniz (ML) öğrenimi dayanır. Otomatik olarak etkin olduğundan, yeni anomali algılama sonuçlarını hemen hemen algılamalar, aboneliğinizle ilişkili kaynakları ve kullanıcıları arasında çok sayıda davranış anormallikleri hedefleyen sağlayarak sağlar. Ayrıca, bu uyarıları araştırma sürecini hızlandırmak ve devam eden tehditleri içeren yardımcı olmak için Microsoft Cloud App Security algılama altyapısında, zaten var olan verilerden yararlanacak. 

> [!NOTE]
> Güvenlikle ilgili Müşteri verilerinin bir kopyasını depolayabilir, Azure Güvenlik Merkezi, toplanan ya da (örneğin, sanal makine veya Azure Active Directory Kiracı) müşteri kaynakla ilişkili: (a) bu kaynak, aynı coğrafi bölgede içinde olması dışında bu bölgelerde burada Microsoft Azure Güvenlik Merkezi, bu tür verilerin bir kopyasını durumunun Amerika Birleşik Devletleri'nde depolanacağı dağıtmak henüz var; ve (b) Azure Güvenlik Merkezi, bu tür verileri işlemek için başka bir Microsoft çevrimiçi hizmeti kullanır. Burada, bu tür veriler, bir çevrimiçi hizmet coğrafi konum kurallarına uygun olarak depolayabilir.
>

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

## <a name="prerequisites"></a>Önkoşullar

- Geçerli, etkinleştirilmiş [Microsoft Cloud App Security lisans](https://docs.microsoft.com/cloud-app-security/getting-started-with-cloud-app-security)
- [Güvenlik Merkezi standart katmanı](https://azure.microsoft.com/pricing/details/security-center/)
 
## <a name="threat-detection-alerts"></a>Tehdit algılama uyarıları

Güvenlik Merkezi, Cloud App Security anomali algılama uyarıları gibi destekler:

**Mümkün olmayan seyahat**
-  Bu algılama yöntemi tanımlayan iki kullanıcı etkinlikleri (tek veya birden çok değer) bu süreden daha kısa bir süre içinde coğrafi olarak uzak konumlardan gerçekleştirilen sunucucu kullanıcının ilk konumdan gösteren ikinci, seyahat farklı bir kullanıcı kimlik bilgilerini kullandığını. Bu algılama yöntemi, bir makine öğrenme "VPN'ler ve kuruluştaki diğer kullanıcılar tarafından düzenli olarak kullanılan konumlar gibi mümkün olmayan seyahat koşulu'ına katkıda bulunan belirgin hatalı pozitif sonuçlar" yoksayar algoritmasına yararlanır. Bu sırada yeni bir kullanıcının etkinlik deseninin öğrenir yedi günlük bir öğrenme dönemi algılama sahiptir.

**Sık erişilmeyen ülkeden etkinlik**
- Bu algılama yöntemi, yeni ve sık erişilmeyen konumlarını belirlemek için etkinlik konumları dikkate alır. Anomali algılama altyapısında, kuruluşunuzdaki kullanıcılar tarafından kullanılan önceki konumları hakkında bilgi depolar. Yakın zamanda ya da hiç kuruluştaki herhangi bir kullanıcı tarafından ziyaret edilmedi bir konumdan bir etkinlik gerçekleştiğinde bir uyarı tetiklenir. 

**Anonim IP adreslerinden etkinliği**
- Bu algılama yöntemi, kullanıcıların bir IP adresinden anonim proxy IP adresi belirlenmiştir etkin tanımlar. Bu proxy'ler cihazlarının IP adresini gizlemek istediğiniz ve için kötü amaçlı kullanılan kişiler tarafından kullanılır. Bu algılama yöntemi, bir makine öğrenme algoritmasına "hatalı pozitif sonuçlar", kuruluşunuzdaki kullanıcılar tarafından yaygın olarak kullanılan yanlış etiketli IP adresleri gibi azaltan yararlanır.
 
  ![Tehdit algılama Uyarısı](./media/security-center-ueba-mcas/security-center-mcas-alert.png)

## <a name="disabling-threat-detection-alerts"></a>Tehdit algılama uyarıları devre dışı bırakma

Bu uyarılar varsayılan olarak etkindir, ancak bunları devre dışı bırakabilirsiniz:

1. Güvenlik Merkezi dikey penceresinden seçin **tehdit algılama**.
2. Altında **tehdit algılama - tümleştirmeler etkinleştirin**, işaretini kaldırın **izin Microsoft Cloud App verilerimi erişmeye Security**, tıklatıp **Kaydet**.

   ![Tehdit algılama Uyarısı](./media/security-center-ueba-mcas/security-center-mcas-optout.png)

> [!NOTE]
> Bir öğrenme dönemi sırasında hangi tüm anomali algılama uyarıları üretilir yedi gün yoktur. Bundan sonra her oturum karşılaştırılır kullanıcı etkinliği, IP adresleri, cihazlar, vb. geçtiğimiz ay ve bu etkinliklerin risk puanlarıyla üzerinden algılandı. Bu algılamaların amacı, makine ortamı ve tetikleyiciler uyarılar, kuruluşunuzun faaliyet öğrenilen temel göre bu profilleri anomali algılama altyapısı öğrenimi bir parçasıdır. Bu algılamaların amacı, ayrıca kullanıcılar ve oturum açma düzeni hatalı pozitif sonuçları azaltmak için profil için tasarlanan machine learning algoritmaları yararlanın.
>
  
## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Güvenlik Merkezi'ndeki tehdit algılama ile çalışmak için gösterdi. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.



<!--Image references-->
[1]: ./media/security-center-confidence-score/confidence-score.png
[2]: ./media/security-center-confidence-score/suspicious-confidence-score.png
