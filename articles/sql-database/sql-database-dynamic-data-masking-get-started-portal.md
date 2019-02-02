---
title: 'Azure portalı: SQL veritabanı dinamik veri maskeleme | Microsoft Docs'
description: Nasıl Azure portalında SQL veritabanı dinamik veri maskelemeyi kullanmaya başlama
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: ronitr
ms.author: ronitr
ms.reviewer: vanto
manager: craigg
ms.date: 09/25/2018
ms.openlocfilehash: 9ecbb65b320f64a89c8092afa8caed31c25c619d
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2019
ms.locfileid: "55561901"
---
# <a name="get-started-with-sql-database-dynamic-data-masking-with-the-azure-portal"></a>SQL veritabanı dinamik veri maskeleme Azure portalı ile kullanmaya başlayın

Bu makalede nasıl uygulayacağınızı gösteren [dinamik veri maskeleme](sql-database-dynamic-data-masking-get-started.md) Azure portalı ile. Dinamik veri maskelemeyi kullanarak uygulayabileceğiniz [Azure SQL veritabanı cmdlet'leri](https://docs.microsoft.com/powershell/module/azurerm.sql/) veya [REST API](https://msdn.microsoft.com/library/dn505719.aspx).


## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-portal"></a>Dinamik veri maskeleme Azure portalını kullanarak veritabanınız için ayarlama
1. Adresinden Azure portalında başlatma [ https://portal.azure.com ](https://portal.azure.com).
2. Hassas verileri maskelemek için istediğiniz içeren veritabanı ayarları sayfasına gidin.
3. Tıklayın **dinamik veri maskeleme** başlatan kutucuğa **dinamik veri maskeleme** yapılandırma sayfası.
   
   * Alternatif olarak, aşağı kaydırabilir **işlemleri** tıklayın ve bölüm **dinamik veri maskeleme**.
     
     ![Gezinti bölmesi](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>
4. İçinde **dinamik veri maskeleme** yapılandırma sayfası hizmetinin öneriler motoru maskelemeyi işaretlenmiş bazı veritabanı sütunları ile karşılaşabilirsiniz. Önerileri kabul et için tıklamanız **maske Ekle** bir veya daha fazla sütun ve maskesi için bu sütun için varsayılan türü temel alınarak oluşturulur. Maskeleme işlevi maskeleme kuralı tıklayarak ve maskeleme düzenleyerek değiştirebilirsiniz alan biçim için tercih ettiğiniz farklı bir biçim. Tıkladığınızdan emin olun **Kaydet** ayarlarınızı kaydetmek için.
   
    ![Gezinti bölmesi](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>
5. Herhangi bir sütun için bir maske en üstündeki veritabanınızı eklemek için **dinamik veri maskeleme** yapılandırma sayfası tıklayın **maske Ekle** açmak için **maskeleme Kuralı Ekle** yapılandırma sayfası .
   
    ![Gezinti bölmesi](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>
6. Seçin **şema**, **tablo** ve **sütun** maskeleme için belirtilen alan tanımlamak için.
7. Seçin bir **maskeleme alanı biçimi** hassas verileri maskeleme kategorileri listesinden.
   
    ![Gezinti bölmesi](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>        
8. Tıklayın **Kaydet** veri maskeleme kuralları dinamik veri maskeleme ilke maskeleme kümesi güncelleştirmek için kural sayfası.
9. SQL kullanıcı veya maskeleme işlemi bırakılmalıdır ve maskelenmemiş hassas verilere erişimi AAD kimliklerini yazın. Bu, kullanıcıların noktalı virgülle ayrılmış bir listesi olmalıdır. Yönetici ayrıcalıklarına sahip kullanıcılar her zaman özgün maskelenmemiş verilere erişebilir.
   
    ![Gezinti bölmesi](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)
   
   > [!TIP]
   > Uygulama katmanı uygulama ayrıcalıklı kullanıcılar için hassas verileri görüntülemek üzere hale getirmek için AAD kimlik uygulama veritabanını sorgulamak için kullanır ve SQL kullanıcı ekleyin. Bu liste ayrıcalıklı kullanıcıların, hassas verilerin en aza indirmek için en az sayıda içermesi önerilir.
   > 
   > 
10. Tıklayın **Kaydet** ilke yapılandırma sayfasında, yeni veya güncelleştirilmiş kaydetmek için maskeleme verileri maskeleme.


## <a name="next-steps"></a>Sonraki adımlar

* Dinamik veri maskeleme genel bakış için bkz. [dinamik veri maskeleme](sql-database-dynamic-data-masking-get-started.md).
* Dinamik veri maskelemeyi kullanarak uygulayabileceğiniz [Azure SQL veritabanı cmdlet'leri](https://docs.microsoft.com/powershell/module/azurerm.sql/) veya [REST API](https://msdn.microsoft.com/library/dn505719.aspx).
