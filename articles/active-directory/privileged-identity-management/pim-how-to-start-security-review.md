---
title: Erişim gözden geçirmesi başlatma | Microsoft Docs
description: Azure Privileged Identity Management uygulaması ile ayrıcalıklı kimlikleri için erişim gözden geçirmesi oluşturmayı öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 06/21/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 3ead2fe01c932c76a83e989d8908de4c9bfa541b
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38590355"
---
# <a name="how-to-start-an-access-review-in-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management'ta erişim gözden geçirmesi başlatma
Kullanıcılar artık ihtiyacınız yoksa erişim ayrıcalıklı, rol atamaları "eski" olur. Bu eski rol atamaları, ayrıcalıklı rol ile ilişkili riski azaltmak için yöneticileri veya genel yöneticileri düzenli olarak kullanıcılara verilen rolleri gözden geçirmek için yöneticileri istemek için erişim gözden geçirmeleri oluşturmanız gerekir. Bu belge, Azure AD Privileged Identity Management (PIM) erişim gözden geçirmesi başlatma adımlarını kapsar.

## <a name="start-an-access-review"></a>Erişim incelemesi başlat
> [!NOTE]
> Azure portalında panonuza PIM uygulama eklemediyseniz adımlara bakın [Azure Privileged Identity Management ile çalışmaya başlama](pim-getting-started.md)
> 
> 

PIM uygulama ana sayfasından erişim gözden geçirmesi başlamanın üç yolu vardır:

* **Erişim gözden geçirmeleriyle** > **Ekle**
* **Rolleri** > **gözden geçirme** düğmesi
* Rolleri listeden geçirilecek özel rolü seçin > **gözden geçirme** düğmesi

Tıkladığınızda **gözden** düğmesi **erişim değerlendirmesi başlatma** dikey penceresi görünür. Bu dikey pencerede, yedekleyeceksiniz gözden geçirme adı ve süre sınırı ile yapılandırmak için gözden geçirin ve kimin gözden geçirmeyi gerçekleştirip karar vermek için bir rol seçin.

![Erişim gözden geçirmesi - ekran görüntüsü Başlat](./media/pim-how-to-start-security-review/PIM_start_review.png)

### <a name="configure-the-review"></a>Gözden geçirmeyi yapılandırma
Erişim gözden geçirmesi oluşturma için adlandırın ve bir başlangıç ve bitiş tarihi ayarlamak gerekir.

![Gözden geçirme - ekran görüntüsü yapılandırma](./media/pim-how-to-start-security-review/PIM_review_configure.png)

Gözden geçirmeyi tamamlamak, kullanıcılar için yeterince uzun uzunluğunu olun. Bitiş tarihinden önce son, erken gözden her zaman durdurabilirsiniz.

### <a name="choose-a-role-to-review"></a>Gözden geçirmek için bir rol seçin
Her gözden geçirme, yalnızca bir rol üzerinde odaklanır. Erişim gözden geçirmesi özel rol dikey penceresinden çalışmaya sürece, bir rol şimdi seçmek gerekir.

1. Gidin **rol üyeliğini gözden geçirme**
   
    ![Gözden geçirme rol üyeliğini - ekran görüntüsü](./media/pim-how-to-start-security-review/PIM_review_role.png)
2. Listeden bir rol seçin.

### <a name="decide-who-will-perform-the-review"></a>Kimin gözden geçirmeyi gerçekleştirip karar verin
Bir gözden geçirme gerçekleştirmeye yönelik üç seçenek vardır. Gözden geçirmeyi tamamlamak için başka bir kişiye atayabilirsiniz, kendinize yapabileceğiniz veya her kullanıcının kendi erişimini gözden geçirmek olabilir.

1. Gidin **Gözden Geçiren seçin**
   
    ![Gözden Geçiren - ekran görüntüsü seçin](./media/pim-how-to-start-security-review/PIM_review_reviewers.png)
2. Seçeneklerden birini seçin:
   
   * **Select Gözden Geçiren**: erişim gerek duyan bilmiyorsanız bu seçeneği kullanın. Bu seçenek belirtilmişse, gözden geçirmeyi tamamlamak için bir kaynak sahibi veya grup yöneticisi atayabilirsiniz.
   * **Bana**: nasıl iş erişim gözden geçirmeleri Önizleme veya olamaz kişilerin adına gözden geçirmek istediğiniz istiyorsanız yararlıdır.
   * **Üyeleri gözden kendilerini**: kullanıcılar kendi rol atamalarını gözden geçirmek için bu seçeneği kullanın.

### <a name="start-the-review"></a>Değerlendirmesi başlatma
Son olarak, kullanıcılar kendi erişim'i onaylarsanız bir neden sağlamasını istemek için seçeneğiniz vardır. İsterseniz, gözden geçirme açıklaması ekleyin ve seçin **Başlat**.

Kullanıcılarınız kendileri için bekleyen bir erişim gözden geçirmesi olduğunu biliyor ve bunları Göster olanak sağlayın [erişim gözden geçirmesi gerçekleştirme](pim-how-to-perform-security-review.md).

## <a name="manage-the-access-review"></a>Erişim gözden geçirmesi yönetme
Gözden geçirenler, Azure AD PIM Panoda erişim gözden geçirmeleri bölümünde incelemelerde tamamladıkça, ilerleme durumunu izleyebilirsiniz. Erişim hakları dizine kadar değiştirilecek [gözden geçirme tamamlandıktan](pim-how-to-complete-review.md).

Değerlendirme süresi bitene kadar gözden geçirmelerini tamamlamak için kullanıcılara hatırlatın veya erken erişim gözden geçirmeleri bölümünden gözden geçirmesini durdur.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="pim-table-of-contents"></a>PIM İçindekiler
[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]
