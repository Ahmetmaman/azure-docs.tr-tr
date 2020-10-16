---
title: Azure NetApp Files çapraz bölge çoğaltmasını kullanarak olağanüstü durum kurtarmayı yönetme | Microsoft Docs
description: Azure NetApp Files çapraz bölge çoğaltma kullanılarak olağanüstü durum kurtarmanın nasıl yönetileceğini açıklar.
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
ms.date: 09/16/2020
ms.author: b-juche
ms.openlocfilehash: ad006279a656758ba856cd3f39c17b0410e525e6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90709038"
---
# <a name="manage-disaster-recovery-using-cross-region-replication"></a>Çapraz bölge çoğaltmasını kullanarak olağanüstü durum kurtarmayı yönetme 

Kaynak ve hedef birimler arasında devam eden bir çoğaltma (bkz. [çoğaltma eşlemesi oluşturma](cross-region-replication-create-peering.md)) sizi bir olağanüstü durum kurtarma etkinliği hazırlar. 

Böyle bir olay gerçekleştiğinde, [hedef birime yük devretmek](#break-replication-peering-to-activate-the-destination-volume)için istemcinin hedef birimde okuma ve yazma işlemini sağlayabilirsiniz. 

Olağanüstü durum kurtarma işleminden sonra kaynak birime, hedef birim verileriyle kaynak birim verilerinin üzerine yazan yeniden [eşitleme işlemi](#resync-replication-to-reactivate-the-source-volume) ile geri dönebilirsiniz.  Ardından [kaynaktan hedefe çoğaltmayı](#reestablish-source-to-destination-replication) yeniden yükledikten sonra istemci için kaynak birimi uzaktan yeniden bağlayın. 

Ayrıntılar aşağıda açıklanmıştır. 

## <a name="break-replication-peering-to-activate-the-destination-volume"></a>Hedef birimi etkinleştirmek için çoğaltma eşlemesini kes

Hedef birimi etkinleştirmeniz gerektiğinde (örneğin, hedef bölgeye yük devretmek istediğinizde), çoğaltma eşlemesini kesmeniz ve ardından hedef birimi bağlamanız gerekir.  

1. Çoğaltma eşlemesini bölmek için hedef birimi seçin. Depolama hizmeti altında **çoğaltma** ' ya tıklayın.  

2.  Devam etmeden önce aşağıdaki alanları kontrol edin:  
    * Yansıtma durumunun ***yansıtmalı***olduğundan emin olun.   
        Yansıtma durumu *başlatılmamış*olarak görünüyorsa çoğaltma eşlemesini kesmeyi denemeyin.
    * Ilişki durumunun ***Boşta***gösterildiğinden emin olun.   
        Ilişki durumu *aktarmayı*gösteriyorsa çoğaltma eşlemesini kesmeyi denemeyin.   

    Bkz. [çoğaltma ilişkisinin sistem durumunu görüntüleme](cross-region-replication-display-health-status.md). 

3.  **Eşlemeyi kes**' e tıklayın.  

4.  İstendiğinde **Evet** yazın ve **Kes** düğmesine tıklayın. 

    ![Çoğaltma eşlemesini kes](../media/azure-netapp-files/cross-region-replication-break-replication-peering.png)

5.  [Windows veya Linux sanal makineleri için bir birimi bağlama veya çıkarma](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)bölümündeki adımları izleyerek hedef birimi bağlayın.   
    Bu adım, bir istemcinin hedef birime erişmesine olanak sağlar.

## <a name="resync-replication-to-reactivate-the-source-volume"></a>Kaynak birimi yeniden etkinleştirmek için çoğaltmayı yeniden eşitleme   

Olağanüstü durum kurtarma sonrasında, yeniden eşitleme işlemi gerçekleştirerek kaynak birimi yeniden etkinleştirebilirsiniz.  Yeniden eşitleme işlemi, çoğaltma işlemini tersine çevirir ve hedef birimdeki verileri kaynak birime eşitler.  

> [!IMPORTANT] 
> Yeniden eşitleme işlemi, kaynak birim verilerinin üzerine hedef birim verilerini yazar.  Kullanıcı arabirimi, veri kaybı potansiyeli hakkında sizi uyarır. İşlem başlamadan önce yeniden eşitleme eylemini onaylamanız istenir.

1. Çoğaltmayı yeniden eşitleme için *kaynak* birimi seçin. Depolama hizmeti altında **çoğaltma** ' ya tıklayın. Yeniden **Eşitle**' ye tıklayın.  

2. İstendiğinde **Evet** yazın ve yeniden **Eşitle** düğmesine tıklayın. 
 
    ![Çoğaltmayı yeniden eşitle](../media/azure-netapp-files/cross-region-replication-resync-replication.png)

3. [Çoğaltma ilişkisinin sistem durumunu görüntüleme](cross-region-replication-display-health-status.md)bölümündeki adımları izleyerek kaynak birim sistem durumunu izleyin.   
    Kaynak birim sistem durumu aşağıdaki değerleri gösterdiğinde, yeniden eşitleme işlemi tamamlanır ve hedef birimde yapılan değişiklikler artık kaynak birimde yakalanır:   

    * Yansıtılmış durum: *yansıtılmış*  
    * Aktarım durumu: *Boşta*  

## <a name="reestablish-source-to-destination-replication"></a>Kaynaktan hedefe çoğaltmayı yeniden kurma

Hedefin kaynağa yeniden eşitleme işlemi tamamlandıktan sonra, kaynaktan hedefe çoğaltmayı yeniden oluşturmak için çoğaltma eşlemesini tekrar kesmeniz gerekir. Ayrıca, istemcinin erişebilmesi için kaynak birimini de yeniden bağlayın.  

1. Çoğaltma eşlemesini kes:  
    a. *Hedef* birimi seçin. Depolama hizmeti altında **çoğaltma** ' ya tıklayın.  
    b. Devam etmeden önce aşağıdaki alanları kontrol edin:   
    * Yansıtma durumunun ***yansıtmalı***olduğundan emin olun.   
    Yansıtma durumu *başlatılmamış*olarak görünüyorsa çoğaltma eşlemesini kesmeyi denemeyin.  
    * Ilişki durumunun ***Boşta***gösterildiğinden emin olun.   
    Ilişki durumu *aktarmayı*gösteriyorsa çoğaltma eşlemesini kesmeyi denemeyin.    

        Bkz. [çoğaltma ilişkisinin sistem durumunu görüntüleme](cross-region-replication-display-health-status.md). 

    c. **Eşlemeyi kes**' e tıklayın.   
    d. İstendiğinde **Evet** yazın ve **Kes** düğmesine tıklayın.  

2. Kaynak birimi hedef birimle yeniden eşitleyin:  
    a. *Hedef* birimi seçin. Depolama hizmeti altında **çoğaltma** ' ya tıklayın. Yeniden **Eşitle**' ye tıklayın.   
    b. İstendiğinde **Evet** yazın ve yeniden **Eşitle** düğmesine tıklayın.

3. [Windows veya Linux sanal makineleri için bir birimi bağlama veya çıkarma](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)bölümündeki adımları izleyerek kaynak birimi uzaktan yeniden bağlayın.  
    Bu adım, bir istemcinin kaynak birime erişmesine olanak sağlar.

## <a name="next-steps"></a>Sonraki adımlar  

* [Bölgeler arası çoğaltma](cross-region-replication-introduction.md)
* [Bölgeler arası çoğaltmayı kullanma gereksinimleri ve konuları](cross-region-replication-requirements-considerations.md)
* [Çoğaltma ilişkisinin uygunluk durumunu görüntüleme](cross-region-replication-display-health-status.md)
* [Birim çoğaltma ölçümleri](azure-netapp-files-metrics.md#replication)
* [Bölgeler arası çoğaltma sorunlarını giderme](troubleshoot-cross-region-replication.md)

