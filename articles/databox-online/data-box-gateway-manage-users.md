---
title: Azure Data Box Gateway kullanıcılarını yönetme | Microsoft Docs
description: Azure portalı kullanarak Azure Data Box Gateway kullanıcılarını yönetme adımları.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: how-to
ms.date: 03/25/2019
ms.author: alkohli
ms.openlocfilehash: 38b4b701329cf35088d797b095fa3caca46f55b6
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84338983"
---
# <a name="use-the-azure-portal-to-manage-users-on-your-azure-data-box-gateway"></a>Azure portalı kullanarak Azure Data Box Gateway kullanıcılarını yönetme

Bu makalede Azure Data Box Gateway kullanıcılarını yönetme adımları açıklanmaktadır. Azure Data Box Gateway’i Azure portal veya yerel web kullanıcı arabirimiyle yönetebilirsiniz. Kullanıcı ekleme, değiştirme ve silme işlemleri için Azure portalı kullanın. 

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Kullanıcı ekleme
> * Kullanıcıyı değiştirme
> * Kullanıcı silme

## <a name="about-users"></a>Kullanıcılar hakkında

Kullanıcılara salt okunur erişim veya tam ayrıcalık verilebilir. Adından anlaşılacağı üzere salt okunur kullanıcılar paylaşım verilerini yalnızca görüntüleyebilir. Tam ayrıcalık sahibi kullanıcılar paylaşım verilerini okuyabilir, bu paylaşımlara veri yazabilir, paylaşım verilerini değiştirebilir veya silebilir.

 - **Tam ayrıcalıklı kullanıcı**: Tam erişime sahip yerel kullanıcıdır.
 - **Salt okunur kullanıcı**: Salt okunur erişime sahip yerel kullanıcıdır. Bu kullanıcılar yalnızca salt okunur işlemlere izin veren paylaşımlarla ilişkilendirilir.

Kullanıcı izinleri, paylaşım oluşturma sırasında kullanıcı oluşturulurken tanımlanır. Paylaşma düzeyi izinlerinin değiştirilmesi Şu anda desteklenmiyor.

## <a name="add-a-user"></a>Kullanıcı ekleme

Kullanıcı eklemek için Azure portalda aşağıdaki adımları gerçekleştirin.

1. Data Box Gateway kaynağınızın Azure portal sayfasında **Genel bakış** bölümüne gidin. Komut çubuğunda **+ Kullanıcı ekle**'ye tıklayın.

    ![Kullanıcı ekle'ye tıklayın](media/data-box-gateway-manage-users/add-user-1.png)

2. Eklemek istediğiniz kullanıcının kullanıcı adını ve parolasını belirtin. Parolayı onaylayın ve **Ekle**'ye tıklayın.

    ![Kullanıcı ekle'ye tıklayın](media/data-box-gateway-manage-users/add-user-2.png)

    > [!IMPORTANT] 
    > Bu kullanıcılar sistem tarafından ayrılmıştır ve kullanılmamalıdır: Administrator, EdgeUser, EdgeSupport, HcsSetupUser, WDAGUtilityAccount, CLIUSR, DefaultAccount, Guest.  

3. Kullanıcı oluşturma işlemi başladığında ve tamamlandığında bildirim alırsınız. Kullanıcı oluşturulduktan sonra komut çubuğunda **Yenile**'ye tıklayarak güncel kullanıcı listesini görüntüleyebilirsiniz.


## <a name="modify-user"></a>Kullanıcıyı değiştirme

Kullanıcı oluşturulduktan sonra parolasını değiştirebilirsiniz. Kullanıcı listesinden seçin ve tıklayın. Yeni parolayı girin ve onaylayın. Değişiklikleri kaydedin.
 
![Kullanıcıyı değiştirme](media/data-box-gateway-manage-users/modify-user-1.png)


## <a name="delete-a-user"></a>Kullanıcı silme

Kullanıcı silmek için Azure portalda aşağıdaki adımları gerçekleştirin.

1. Kullanıcı listesinden bir kullanıcı seçin ve **Sil**'e tıklayın.  

   ![Kullanıcı silme](media/data-box-gateway-manage-users/delete-user-1.png)

2. Sorulduğunda silme işlemini onaylayın. 

   ![Kullanıcı silme](media/data-box-gateway-manage-users/delete-user-2.png)

Kullanıcı listesi silinen kullanıcıya göre güncelleştirilir.

![Kullanıcı silme](media/data-box-gateway-manage-users/delete-user-3.png)


## <a name="next-steps"></a>Sonraki adımlar

- [Bant genişliğini yönetmeyi](data-box-gateway-manage-bandwidth-schedules.md) öğrenin.
