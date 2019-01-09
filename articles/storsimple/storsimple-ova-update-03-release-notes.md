---
title: StorSimple sanal dizisi güncelleştirmeleri sürüm notları | Microsoft Docs
description: StorSimple sanal Update 0.3 çalıştıran dizisi için kritik açık sorunlar ve çözümleri açıklanmaktadır.
services: storsimple
documentationcenter: ''
author: alkohli
manager: carmonm
editor: ''
ms.assetid: b197651a-3c40-4185-b23d-4c8f22cfa8f4
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/15/2016
ms.author: alkohli
ms.openlocfilehash: 635b5f4edf5d403c569b4957540fc105997b3e8e
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2019
ms.locfileid: "54117682"
---
# <a name="storsimple-virtual-array-update-03-release-notes"></a>StorSimple sanal dizisi güncelleştirme 0.3 sürüm notları
## <a name="overview"></a>Genel Bakış
Aşağıdaki sürüm notları, kritik açık sorunlar ve çözümlenen sorunlar için Microsoft Azure StorSimple Virtual Array güncelleştirme belirleyin.

Sürüm Notları sürekli olarak güncelleştirilir ve geçici bir çözüm gerektiren kritik sorunlar bulundukça eklenir. StorSimple Virtual Array'iniz dağıtmadan önce bu sürüm notlarında yer alan bilgileri dikkatle gözden geçirin.

Güncelleştirme 0.3 yazılım sürümüne karşılık gelen **10.0.10288.0**.

> [!NOTE]
> Güncelleştirmeler kesintiye uğratan bir durumdur ve Cihazınızı yeniden başlatın. Cihaz g/ç ediyor, kapalı kalma süresi artmasına neden olur.
> 
> 

## <a name="whats-new-in-the-update-03"></a>Güncelleştirme 0.3 yenilikler nelerdir?
Güncelleştirme 0.3 öncelikle bir hata düzeltmesi yapıdır. Bu sürümde çeşitli hatalar önceki sürümde yedekleme hataları výsledek çözüldü.

## <a name="issues-fixed-in-the-update-03"></a>Güncelleştirme 0.3 giderilen sorunlar
Aşağıdaki tabloda, bu sürümde giderilen sorunlar özetini sağlar.

| Hayır. | Özellik | Sorun |
| --- | --- | --- |
| 1 |Yedeklemeler |Bir sorun, yedeklemeleri tamamlamak için bir dosya paylaşımı için burada başarısız olacağı önceki sürümde görüldü. Bu sorun oluştuysa, yedekleme işi başarısız olur ve kullanıcıya bildirmek için StorSimple Yöneticisi hizmeti bir kritik uyarı tetiklendi. Bu sorunu veri paylaşımları veya verilere erişimin etkilemeyen. Kök nedeni tanımlanır ve bu sürümde giderilen. <br></br> Düzeltme zaten bu sorunu görüyorsanız paylaşımlarına geriye dönük olarak uygulanmaz. Müşteriler, bu sorunu görüyorsanız ilk güncelleştirme 0.3 uygulayın ve ardından Microsoft sorunu düzeltmek için bir tam sistem yedeklemesi gerçekleştirmeyi Support başvurun. Microsoft Support başvurarak yerine müşteriler ayrıca yeni bir paylaşım etkilenen paylaşımları için iyi bir yedekten geri yükleyebilirsiniz. |
| 2 |iSCSI |Bir sorun, birimler, StorSimple sanal dizisi üzerinde bir birime veri kopyalama işlemi sırasında nerede kaybolur önceki sürümde görüldü. Bu sürümde bu sorunu düzeltildi. <br></br> Düzeltmeleri yalnızca birimler yeni oluşturulan etkili. Düzeltmeler, zaten bu sorunu görüyorsanız birimlerin geriye dönük olarak uygulanmaz. Müşterilerin, etkilenen birimleri çevrimiçi Klasik Azure Portalı aracılığıyla, bu birimler için yedekleme ve yeni birimleri için bu birimler'geri yükleme için önerilir. |

## <a name="known-issues-in-the-update-03"></a>Güncelleştirme 0.3'de bilinen sorunlar
Aşağıdaki tabloda StorSimple sanal dizisi için bilinen sorunların bir Özet sağlar ve önceki sürümlerden yayın belirtildiği sorunları içerir. 

| Hayır. | Özellik | Sorun | Geçici çözüm/açıklamaları |
| --- | --- | --- | --- |
| **1.** |Güncelleştirmeler |Önizleme sürümünde oluşturulan sanal cihazlar için desteklenen genel kullanılabilirlik sürümü güncelleştirilemiyor. |Bu sanal cihazlar için genel kullanım sürümünde bir olağanüstü durum kurtarma (DR) iş akışı kullanarak devredilen gerekir. |
| **2.** |Sağlanan veri diski |Belirli bir belirtilen boyutta bir veri diski sağladığınız ve karşılık gelen StorSimple sanal cihazı oluşturdunuz, gerekir değil genişletin veya veri diski küçültmeye sonra. Cihaz yerel katmanlarda tüm verilerin kaybıyla sonuçları yapmak çalışıyor. | |
| **3.** |Grup ilkesi |Bir cihaz etki alanına katılmış olduğunda, bir Grup İlkesi uygulama cihaz işlemi olumsuz yönde etkileyebilir. |Sanal diziniz kendi kuruluş birimi (OU) için Active Directory olduğundan ve hiçbir Grup İlkesi nesneleri (GPO) uygulanmış emin olun. |
| **4.** |Yerel web kullanıcı Arabirimi |Internet Explorer (IE ESC) Artırılmış güvenlik özellikleri etkinleştirilirse, bazı sorun giderme veya bakım gibi yerel web kullanıcı Arabirimi sayfalarını düzgün çalışmayabilir. Bu sayfa düğmelerini de çalışmayabilir. |Internet Explorer Gelişmiş güvenlik özelliklerini devre dışı bırakın. |
| **5.** |Yerel web kullanıcı Arabirimi |Bir Hyper-V sanal makine, GB/sn ağ arabirimlerinin de kullanıcı Arabirimi olarak 10 görüntülenen web arabirimleri. |Bir yansıma Hyper-V, davranıştır. Hyper-V, sanal ağ bağdaştırıcıları için 10 GB/sn her zaman gösterilir. |
| **6.** |Katmanlı birimler veya paylaşımlar |Katmanlı birimlerin desteklenmiyor StorSimple ile çalışan uygulamalar için kilitleme bayt aralığı. Bayt aralığı kilitleme etkinse, StorSimple katmanlama çalışmaz. |Önerilen ölçüleri içerir: <br></br>Bayt aralığı uygulama mantığınızın kilitleme devre dışı bırakın.<br></br>Bu uygulama için verileri yerel olarak sabitlenmiş birim katmanlı birimlerin yerine koymak seçin.<br></br>*Uyarı*: Geri yükleme tamamlamadan önce kullanarak yerel olarak sabitlenmiş birimler ve bayt aralığı kilitleme etkin olduğunda, yerel olarak sabitlenmiş birimin çevrimiçi olabilir. Bir geri yükleme devam ediyor, bu gibi durumlarda, daha sonra tamamlamak geri yüklemek için beklemeniz gerekir. |
| **7.** |Katmanlı paylaşımları |Büyük dosyaları ile çalışma, yavaş bir katmanın ölçeğini sonuçlanabilir. |Büyük dosyalarla çalışırken, en büyük dosya paylaşım boyutunun %3 küçükse öneririz. |
| **8.** |Kapasite paylaşımlar için kullanılan |Görebileceğiniz paylaşımında veri olduğunda tüketim paylaşın. Kullanılan kapasite paylaşımları için meta veriler içeren olmasıdır. | |
| **9.** |Olağanüstü durum kurtarma |Yalnızca dosya sunucusu aynı etki, kaynak cihaz için olağanüstü durum kurtarma gerçekleştirebilirsiniz. Olağanüstü durum kurtarma için başka bir etki alanındaki bir hedef cihaz, bu sürümde desteklenmiyor. |Bu, bir sonraki sürümde uygulanır. |
| **10.** |Azure PowerShell |StorSimple sanal cihazları, bu sürüm Azure PowerShell aracılığıyla yönetilemez. |Tüm sanal cihazların yönetimini, Klasik Azure portalı ve yerel web UI aracılığıyla yapılmalıdır. |
| **11.** |Parola değiştirme |Sanal dizi cihaz konsolu yalnızca en-US klavye biçimde giriş kabul eder. | |
| **12.** |CHAP |CHAP kimlik oluşturulduktan sonra kaldırılamaz. Ayrıca, CHAP kimlik bilgilerini değiştirirseniz, birimlerin çevrimdışına alın ve değişikliğin etkili olması çevrimiçi bunları getirmek gerekir. |Bu sorun, bir sonraki sürümde değinilmiştir. |
| **13.** |iSCSI sunucusu |'Depolama görüntülenen bir iSCSI birimi için kullanılan' StorSimple Yöneticisi hizmeti ve iSCSI konağının farklı olabilir. |İSCSI ana bilgisayar dosya sistemi görünüme sahiptir.<br></br>Cihaz, birim maksimum boyutta olduğunda ayrılan blokları görür. |
| **14.** |Dosya sunucusu |Bir klasördeki bir dosyaya bir alternatif veri Stream (ilişkili REKLAM) varsa, REKLAM değil yedeklenen veya olağanüstü durum kurtarma, kopyalama ve öğe düzeyinde Kurtarma ile geri. | |

## <a name="next-step"></a>Sonraki adım
[Güncelleştirme 0.3 yükleme](storsimple-ova-install-update-01.md) StorSimple Virtual Array'iniz üzerinde.

## <a name="references"></a>Başvurular
Eski bir sürüm notu için mi arıyorsunuz? Şuraya gidin: 

* [StorSimple sanal dizisi güncelleştirme 0.1 ve 0.2 sürüm notları](storsimple-ova-update-01-release-notes.md)
* [StorSimple sanal dizisi genel kullanılabilirlik sürüm notları](storsimple-ova-pp-release-notes.md)

