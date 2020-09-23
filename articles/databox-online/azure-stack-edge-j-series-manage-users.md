---
title: Azure Stack Edge Pro GPU kullanıcıları yönetme | Microsoft Docs
description: Azure Stack Edge Pro GPU 'unuzda kullanıcıları yönetmek için Azure portal nasıl kullanılacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 08/28/2020
ms.author: alkohli
ms.openlocfilehash: 5f7e8c09a09084b28278667cdeaf94f7e38908cd
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90904500"
---
# <a name="use-the-azure-portal-to-manage-users-on-your-azure-stack-edge-pro"></a>Azure Stack Edge Pro 'unuzdaki kullanıcıları yönetmek için Azure portal kullanma

<!--[!INCLUDE [applies-to-skus](../../includes/azure-stack-edge-applies-to-all-sku.md)]-->

Bu makalede Azure Stack Edge Pro 'daki kullanıcıların nasıl yönetileceği açıklanır. Azure Stack Edge Pro 'Yu Azure portal veya yerel Web Kullanıcı arabirimi aracılığıyla yönetebilirsiniz. Kullanıcı ekleme, değiştirme ve silme işlemleri için Azure portalı kullanın.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Kullanıcı ekleme
> * Kullanıcıyı değiştirme
> * Kullanıcı silme

## <a name="about-users"></a>Kullanıcılar hakkında

Kullanıcılara salt okunur erişim veya tam ayrıcalık verilebilir. Adından anlaşılacağı üzere salt okunur kullanıcılar paylaşım verilerini yalnızca görüntüleyebilir. Tam ayrıcalık sahibi kullanıcılar paylaşım verilerini okuyabilir, bu paylaşımlara veri yazabilir, paylaşım verilerini değiştirebilir veya silebilir.

 - **Tam ayrıcalıklı kullanıcı**: Tam erişime sahip yerel kullanıcıdır.
 - **Salt okunur kullanıcı**: Salt okunur erişime sahip yerel kullanıcıdır. Bu kullanıcılar yalnızca salt okunur işlemlere izin veren paylaşımlarla ilişkilendirilir.

Kullanıcı izinleri, paylaşım oluşturma sırasında kullanıcı oluşturulurken tanımlanır. Bir kullanıcıya atanan izinler Dosya Gezgini kullanılarak değiştirilebilir.


## <a name="add-a-user"></a>Kullanıcı ekleme

Kullanıcı eklemek için Azure portalda aşağıdaki adımları gerçekleştirin.

1. Azure portal, Azure Stack Edge kaynağınız ' ne gidin ve ardından **genel bakış > kullanıcılar**' a gidin. Komut çubuğunda **+ Kullanıcı Ekle** ' yi seçin.

    ![Kullanıcı Ekle ' yi seçin](media/azure-stack-edge-j-series-manage-users/add-user-1.png)

2. Eklemek istediğiniz kullanıcının kullanıcı adını ve parolasını belirtin. Parolayı onaylayın ve **Ekle**' yi seçin.

    ![Kullanıcı adı ve parola belirtin](media/azure-stack-edge-j-series-manage-users/add-user-2.png)

    > [!IMPORTANT] 
    > Bu kullanıcılar sistem tarafından ayrılmıştır ve kullanılmamalıdır: Administrator, EdgeUser, EdgeSupport, HcsSetupUser, WDAGUtilityAccount, CLIUSR, DefaultAccount, Guest.  

3. Kullanıcı oluşturma başladığında ve tamamlandığında bir bildirim gösterilir. Kullanıcı oluşturulduktan sonra, komut çubuğundan, güncelleştirilmiş Kullanıcı listesini görüntülemek için **Yenile** ' yi seçin.


## <a name="modify-user"></a>Kullanıcıyı değiştirme

Kullanıcı oluşturulduktan sonra parolasını değiştirebilirsiniz. Kullanıcı listesinden seçin. Yeni parolayı girin ve onaylayın. Değişiklikleri kaydedin.
 
![Kullanıcıyı değiştirme](media/azure-stack-edge-j-series-manage-users/modify-user-1.png)


## <a name="delete-a-user"></a>Kullanıcı silme

Kullanıcı silmek için Azure portalda aşağıdaki adımları gerçekleştirin.


1. Azure portal, Azure Stack Edge kaynağınız ' ne gidin ve ardından **genel bakış > kullanıcılar**' a gidin.

    ![Silinecek kullanıcıyı seçin](media/azure-stack-edge-j-series-manage-users/delete-user-1.png)

2. Kullanıcılar listesinden bir kullanıcı seçin ve **Sil**' i seçin. Sorulduğunda silme işlemini onaylayın.

    ![Silinecek kullanıcıyı seçin](media/azure-stack-edge-j-series-manage-users/delete-user-2.png)

Kullanıcı listesi silinen kullanıcıya göre güncelleştirilir.

![Güncelleştirilmiş kullanıcı listesi](media/azure-stack-edge-j-series-manage-users/delete-user-4.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Bant genişliğini yönetmeyi](azure-stack-edge-j-series-manage-bandwidth-schedules.md) öğrenin.
