---
title: Azure NetApp Files için bir birimin hizmet düzeyini dinamik olarak değiştirme | Microsoft Docs
description: Bir birimin hizmet düzeyinin dinamik olarak nasıl değiştirileceğini açıklar.
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
ms.topic: how-to
ms.date: 11/06/2020
ms.author: b-juche
ms.openlocfilehash: fe4b2925a34ae7c06bb0b597f0bcdcc3f4d80896
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2020
ms.locfileid: "94363230"
---
# <a name="dynamically-change-the-service-level-of-a-volume"></a>Birimin hizmet düzeyini dinamik olarak değiştirme

> [!IMPORTANT] 
> Bu özellik için genel önizleme kaydı, daha fazla bildirimde bulununcaya kadar tutuluyor. 

Birimi, birimi için istediğiniz [hizmet düzeyini](azure-netapp-files-service-levels.md) kullanan başka bir kapasite havuzuna taşıyarak mevcut bir birimin hizmet düzeyini değiştirebilirsiniz. Birime yönelik bu yerinde hizmet düzeyi değişikliği, veri geçişini gerektirmez. Ayrıca, birime erişimi etkilemez.  

Bu işlevsellik, istek üzerine iş yükü ihtiyaçlarını karşılamanıza olanak sağlar.  Daha iyi performans için, mevcut bir birimi daha yüksek bir hizmet düzeyi kullanacak şekilde değiştirebilir veya maliyet iyileştirmesi için daha düşük bir hizmet düzeyi kullanabilirsiniz. Örneğin, birim Şu anda *Standart* hizmet düzeyini kullanan bir kapasite havuzunadeyse ve birimin *Premium* hizmet düzeyini kullanmasını istiyorsanız, birimi dinamik olarak *Premium* hizmet düzeyini kullanan bir kapasite havuzuna taşıyabilirsiniz.  

Birimi taşımak istediğiniz kapasite havuzu zaten var olmalıdır. Kapasite havuzu diğer birimleri içerebilir.  Birimi yeni bir kapasite havuzuna taşımak istiyorsanız, birimi Taşımadan önce [Kapasite havuzunu oluşturmanız](azure-netapp-files-set-up-capacity-pool.md) gerekir.  

## <a name="considerations"></a>Dikkat edilmesi gerekenler

* Birim başka bir kapasite havuzuna taşındıktan sonra, önceki toplu etkinlik günlüklerine ve birim ölçümlerine artık erişemeyecektir. Birim yeni kapasite havuzu altında yeni etkinlik günlükleri ve ölçümleri ile başlatılır.

* Bir birimi daha yüksek bir hizmet düzeyinin kapasite havuzuna taşırsanız (örneğin, *Standart* düzeyinden *Premium* veya *Ultra* hizmet düzeyine geçme), *Bu birimi daha* düşük bir hizmet düzeyindeki bir kapasite havuzuna taşıyabilmeniz için en az yedi gün beklemeniz gerekir (örneğin, *Ultra* *Premium* veya *Standart* arasında geçiş).  
<!-- 
## Register the feature

The feature to move a volume to another capacity pool is currently in preview. If you are using this feature for the first time, you need to register the feature first.

1. Register the feature: 

    ```azurepowershell-interactive
    Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFTierChange
    ```

2. Check the status of the feature registration: 

    > [!NOTE]
    > The **RegistrationState** may be in the `Registering` state for up to 60 minutes before changing to`Registered`. Wait until the status is **Registered** before continuing.

    ```azurepowershell-interactive
    Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFTierChange
    ```
You can also use [Azure CLI commands](/cli/azure/feature?preserve-view=true&view=azure-cli-latest) `az feature register` and `az feature show` to register the feature and display the registration status. 
--> 
## <a name="move-a-volume-to-another-capacity-pool"></a>Bir birimi başka bir kapasite havuzuna taşıma

1.  Birimler sayfasında, hizmet düzeyini değiştirmek istediğiniz birime sağ tıklayın. **Havuzu Değiştir** ' i seçin.

    ![Birim ' e sağ tıklayın](../media/azure-netapp-files/right-click-volume.png)

2. Havuzu Değiştir penceresinde, birimi taşımak istediğiniz kapasite havuzunu seçin. 

    ![Havuzu Değiştir](../media/azure-netapp-files/change-pool.png)

3.  **Tamam** ’a tıklayın.


## <a name="next-steps"></a>Sonraki adımlar  

* [Azure NetApp Files için hizmet düzeyleri](azure-netapp-files-service-levels.md)
* [Kapasite havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)
