---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: d35f0ef783a2c48f8211657bc8829635c19495aa
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "67188398"
---
#### <a name="to-enter-maintenance-mode"></a>Bakım moduna girmek için
1. Seri konsol menüsünde, seçenek 1 ' i seçin, **tam erişim Ile oturum açın**.
2. Parolayı yazın. Varsayılan parola **Parola1**' dir.
3. Komut istemine şunu yazın:
   
     `Enter-HcsMaintenanceMode`
4. Bakım modunun tüm g/ç isteklerini kesintiye uğratabileceğini ve klasik Azure portalı bağlantısını kesintiye uğratabileceğini bildiren bir uyarı iletisi görürsünüz ve sizden onay istenir. Bakım moduna girmek için **Y** yazın.
   
    Her iki denetleyici de yeniden başlatılır. Yeniden başlatma tamamlandığında, cihazın bakım modunda olduğunu gösteren başka bir ileti görüntülenir.

