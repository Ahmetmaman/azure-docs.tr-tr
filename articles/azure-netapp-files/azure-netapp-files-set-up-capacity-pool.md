---
title: Azure NetApp Files için kapasite havuzunu ayarlama | Microsoft Docs
description: İçinde birimleri oluşturabileceğiniz bir kapasite havuzunu ayarlama işlemi açıklanır.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/15/2019
ms.author: b-juche
ms.openlocfilehash: 8f50b2ad34c705c8d3831d8243f136c41d750dc0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60691113"
---
# <a name="set-up-a-capacity-pool"></a>Kapasite havuzunu ayarlama

Kapasite havuzu ayarlamak, içinde birim oluşturmanıza olanak tanır.  

## <a name="before-you-begin"></a>Başlamadan önce 

Önceden bir NetApp hesabınız olması gerekir.   

[NetApp hesabı oluşturma](azure-netapp-files-create-netapp-account.md)

## <a name="steps"></a>Adımlar 

1. NetApp hesabınız için yönetim bıçağına gidin ve ardından gezinti bölmesinden **Kapasite havuzlarını**tıklatın.  
    
    ![Kapasite havuzuna gidin](../media/azure-netapp-files/azure-netapp-files-navigate-to-capacity-pool.png)

2. Yeni kapasite havuzu oluşturmak için **+ Havuz ekle**’ye tıklayın.   
    Yeni Kapasite Havuzu penceresi görüntülenir.

3. Yeni kapasite havuzuyla ilgili aşağıdaki bilgileri sağlayın:  
   * **Adı**  
     Kapasite havuzunun adını belirtin.  
     Kapasite havuzu adı her NetApp hesabı için benzersiz olmalıdır.

   * **Hizmet düzeyi**   
     Bu alan, kapasite havuzunun hedef performansını gösterir.  
     Kapasite havuzu için hizmet düzeyini belirtin: [**Premium**](azure-netapp-files-service-levels.md#Premium) veya [**Standart.**](azure-netapp-files-service-levels.md#Standard)

   * **Boyutu**     
     Satın aldığınız kapasite havuzunun boyutunu belirtin.        
     Kapasite havuzunun boyutu en az 4 TiB’dir. Havuzunuzu 4 TiB’nin katları olan büyüklüklerde oluşturabilirsiniz.   
      
     ![Yeni kapasite havuzu](../media/azure-netapp-files/azure-netapp-files-new-capacity-pool.png)

4. **Tamam**'a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar 

- [Azure NetApp Files için hizmet düzeyleri](azure-netapp-files-service-levels.md)
- Farklı hizmet düzeylerinin fiyatı için [Azure NetApp Dosyaları fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/storage/netapp/) bakın
- [Azure NetApp Files için bir alt ağı temsilci olarak belirleme](azure-netapp-files-delegate-subnet.md)
