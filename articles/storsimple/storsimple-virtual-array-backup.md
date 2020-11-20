---
title: Sanal dizi yedekleme öğreticisini Microsoft Azure StorSimple | Microsoft Docs
description: Microsoft Azure StorSimple Sanal dizi için zamanlanmış ve el ile yedeklemeler oluşturun ve silinen bir dosyayı sanal dizinizdeki geri yüklemek için öğe düzeyinde kurtarma gerçekleştirin.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: e3cdcd9e-33b1-424d-82aa-b369d934067e
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 99c7f4ab92e4cce3937a933fec97008c281a0eae
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94954129"
---
# <a name="back-up-shares-or-volumes-on-your-storsimple-virtual-array"></a>StorSimple Sanal dizinizdeki paylaşımları veya birimleri yedekleyin

## <a name="overview"></a>Genel Bakış

StorSimple Sanal dizisi, bir dosya sunucusu veya bir Iscsı sunucusu olarak yapılandırılabilen bir karma bulut depolaması şirket içi sanal cihazdır. Sanal dizi, kullanıcının cihazdaki tüm paylaşımların veya birimlerin zamanlanan ve el ile yedeklerini oluşturmasına izin verir. Dosya sunucusu olarak yapılandırıldığında, öğe düzeyinde kurtarmaya de izin verir. Bu öğreticide, sanal dizinizdeki silinmiş bir dosyayı geri yüklemek için zamanlanmış ve el ile yedeklemeler oluşturma ve öğe düzeyinde kurtarma gerçekleştirme işlemleri açıklanmaktadır.

Bu öğretici yalnızca StorSimple Sanal dizileri için geçerlidir. 8000 serisi hakkında bilgi için, [8000 serisi cihaz için yedekleme oluşturma](./storsimple-8000-manage-backup-policies-u2.md) sayfasına gidin

## <a name="back-up-shares-and-volumes"></a>Paylaşımları ve birimleri yedekleme

Yedeklemeler, zaman içinde koruma sağlar, kurtarılabiliri geliştirir ve paylaşımlar ve birimler için geri yükleme sürelerini en aza indirir. StorSimple cihazınızda bir paylaşımın veya birimin bir kopyasını iki şekilde yedekleyebilirsiniz: **Zamanlanmış** veya **el ile**. Yöntemlerin her biri aşağıdaki bölümlerde ele alınmıştır.

## <a name="change-the-backup-start-time"></a>Yedekleme başlangıç saatini değiştirme

> [!NOTE]
> Bu sürümde zamanlanan yedeklemeler, belirtilen saatte günlük olarak çalışan bir varsayılan ilke tarafından oluşturulur ve cihazdaki tüm paylaşımları veya birimleri yedekler. Şu anda zamanlanmış yedeklemeler için özel ilkeler oluşturulması mümkün değildir.


StorSimple Sanal diziniz, belirtilen gün (22:30) ile başlayan bir varsayılan yedekleme ilkesine sahiptir ve her gün bir kez cihazdaki tüm paylaşımları veya birimleri yedekler. Yedeklemenin başladığı saati değiştirebilirsiniz, ancak sıklık ve bekletme (tutulacak yedeklemelerin sayısını belirtir) değiştirilemez. Bu yedeklemeler sırasında tüm sanal cihaz yedeklenir. Bu, cihazın performansını etkileyebilir ve cihaza dağıtılan iş yüklerini etkileyebilir. Bu nedenle, bu yedeklemeleri yoğun olmayan saatlerde zamanlamanız önerilir.

 Varsayılan yedekleme başlangıç saatini değiştirmek için [Azure Portal](https://portal.azure.com/)aşağıdaki adımları gerçekleştirin.

#### <a name="to-change-the-start-time-for-the-default-backup-policy"></a>Varsayılan yedekleme ilkesinin başlangıç saatini değiştirmek için

1. **Cihazlara** gidin. StorSimple Aygıt Yöneticisi hizmetinize kayıtlı cihazların listesi görüntülenir. 
   
    ![cihazlara git](./media/storsimple-virtual-array-backup/changebuschedule1.png)

2. Cihazınızı seçin ve tıklatın. **Ayarlar** dikey penceresi görüntülenir. **> yedekleme Ilkelerini Yönet**' e gidin.
   
    ![Cihazınızı seçin](./media/storsimple-virtual-array-backup/changebuschedule2.png)

3. **Yedekleme ilkeleri** dikey penceresinde varsayılan başlangıç saati 22:30 ' dir. Cihaz saat diliminde günlük zamanlama için yeni başlangıç saatini belirtebilirsiniz.
   
    ![Yedekleme ilkelerine git](./media/storsimple-virtual-array-backup/changebuschedule5.png)

4. **Kaydet**’e tıklayın.

### <a name="take-a-manual-backup"></a>El ile yedekleme yapın

Zamanlanan yedeklemelere ek olarak, cihaz verilerinin el ile (isteğe bağlı) yedeklemesini istediğiniz zaman alabilirsiniz.

#### <a name="to-create-a-manual-backup"></a>El ile yedekleme oluşturmak için

1. **Cihazlara** gidin. Cihazınızı seçin ve seçili satırda en sağdaki **.** .. öğesine sağ tıklayın. Bağlam menüsünden **yedek al**' ı seçin.
   
    ![Yedekleme al 'a git](./media/storsimple-virtual-array-backup/takebackup1m.png)

2. **Yedeği al** dikey penceresinde, **yedeklemeyi al**' ı tıklatın. Bu işlem, dosya sunucusundaki tüm paylaşımları veya Iscsı sunucunuzdaki tüm birimleri yedekler. 
   
    ![Yedekleme başlatılıyor](./media/storsimple-virtual-array-backup/takebackup2m.png)
   
    İsteğe bağlı yedekleme başlar ve bir yedekleme işinin başlatıldığını görürsünüz.
   
    ![Yedekleme başlatılıyor 2](./media/storsimple-virtual-array-backup/takebackup3m.png) 
   
    İş başarıyla tamamlandıktan sonra yeniden bildirilir. Sonra yedekleme işlemi başlar.
   
    ![yedekleme işi oluşturuldu](./media/storsimple-virtual-array-backup/takebackup4m.png)

3. Yedeklemelerin ilerlemesini izlemek ve iş ayrıntılarına bakmak için bildirime tıklayın. Böylece  **iş ayrıntılarına** gidersiniz.
   
     ![yedekleme işi ayrıntıları](./media/storsimple-virtual-array-backup/takebackup5m.png)

4. Yedekleme tamamlandıktan sonra **yönetim > yedekleme kataloğu**' na gidin. Cihazınızdaki tüm paylaşımların (veya birimlerin) bulut anlık görüntüsünü görürsünüz.
   
    ![Yedekleme tamamlandı](./media/storsimple-virtual-array-backup/takebackup19m.png) 

## <a name="view-existing-backups"></a>Mevcut yedeklemeleri görüntüle
Mevcut yedekleri görüntülemek için Azure portal aşağıdaki adımları gerçekleştirin.

#### <a name="to-view-existing-backups"></a>Mevcut yedekleri görüntülemek için

1. **Cihazlar** dikey penceresine gidin. Cihazınızı seçin ve tıklatın. **Ayarlar** dikey penceresinde **Yönetim > yedekleme kataloğu**' na gidin.
   
    ![Yedekleme kataloğuna git](./media/storsimple-virtual-array-backup/viewbackups1.png)
2. Filtreleme için kullanılacak aşağıdaki ölçütleri belirtin:
   
   - **Zaman aralığı** – **1 saat**, son **24 saat**, son **7 gün**, son **30 gün**, **geçmiş yıl** ve **özel tarih** olabilir.
    
   - **Cihazlar** : storsimple Aygıt Yöneticisi hizmetinize kayıtlı dosya sunucuları veya iSCSI sunucuları listesinden seçim yapın.
   
   - **Başlatıldı** – otomatik olarak **zamanlanabilir** (bir yedekleme Ilkesiyle) veya **el ile** başlatılabilir (sizin tarafınızdan).
   
     ![Yedeklemeleri filtrele](./media/storsimple-virtual-array-backup/viewbackups2.png)

3. **Uygula**’ya tıklayın. Filtrelenen yedeklemeler listesi, **Yedekleme kataloğu** dikey penceresinde görüntülenir. Yalnızca belirli bir zamanda 100 yedekleme öğelerinin görüntülenebileceğini göz önünde bulundurarak.
   
    ![Yedekleme kataloğu güncelleştirildi](./media/storsimple-virtual-array-backup/viewbackups3.png)

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple Sanal dizinizi yönetme](storsimple-ova-web-ui-admin.md)hakkında daha fazla bilgi edinin.