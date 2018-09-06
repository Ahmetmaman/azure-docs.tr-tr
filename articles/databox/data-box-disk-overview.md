---
title: Microsoft Azure Data Box Disk'e genel bakış | Verilerde Microsoft Docs
description: Büyük miktarlarda verinin Azure’a aktarılmasını sağlayan bir bulut çözümü olan Azure Data Box Disk’i açıklar
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: overview
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/04/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand what Data Box Disk is and how it works so I can use it to import on-premises data into Azure.
ms.openlocfilehash: b1beb0e9a5a0435bdf298eddbefc230b2f95ed0a
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43698511"
---
# <a name="what-is-azure-data-box-disk-preview"></a>Azure Data Box Disk nedir? (Önizleme)

Microsoft Azure Data Box Disk çözümü, terabaytlarca şirket içi veriyi Azure'a hızlı, uygun maliyetli ve güvenilir bir şekilde göndermenizi sağlar. Güvenli veri aktarımının daha hızlı gerçekleştirilmesi için 1 ila 5 katı hal sürücüsü (SSD) gönderilir. Her biri 8 TB boyutundaki bu şifrelenmiş diskler bölgesel dağıtım şirketi aracılığıyla veri merkezinize gönderilir. 

Diskleri Azure portalda Data Box hizmetiyle hızlıca yapılandırabilir, bağlayabilir ve disklerin kilidini açabilirsiniz. Verilerinizi disklere kopyalayın ve diskleri Azure'a geri gönderin. Azure veri merkezinde verileriniz sürücülerden hızlı, özel bir ağ yükleme bağlantısı kullanılarak buluta yüklenir.


> [!IMPORTANT]
> - Data Box Disk önizleme aşamasındadır. Bu çözümü dağıtmadan önce [Önizleme için Azure hizmet şartlarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin. 
> - Bu hizmet için kaydolmanız gerekir. Kaydolmak için [Önizleme portalına](http://aka.ms/azuredataboxfromdiskdocs) gidin.
> - Önizleme sırasında Data Box Disk, ABD ve Avrupa Birliği ülkelerindeki müşterilere gönderilebilir. Daha fazla bilgi için bkz. [Bölge kullanılabilirliği](#region-availability).

## <a name="use-cases"></a>Uygulama alanları

Data Box Disk'i kullanarak sıfır veya sınırlı ağ bağlantısı olan senaryolarda terabaytlarca veriyi aktarabilirsiniz. Veri taşıma işlemi tek seferlik, düzenli veya başta toplu veri aktarımı ve ardından düzenli aktarımlardan oluşabilir. 

- **Tek seferlik geçiş**: Büyük miktardaki şirket içi verilerinin Azure'a taşınmasıdır. Çevrimdışı bantlardaki verilerin Azure seyrek erişimli depolama katmanında arşivlenmesi örnek olarak verilebilir.
- **Artımlı aktarım**: Data Box Disk (seed) ile toplu veri aktarımı yapıldıktan sonra ağ üzerinden artımlı aktarım işlemlerinin gerçekleştirilmesidir. Örneğin Commvault ve Data Box Disk ile yedek kopyalar Azure'a taşınabilir. Bu geçişin ardından Azure depolamaya ağ üzerinden artımlı veri aktarımı yapılır. 
- **Düzenli yüklemeler**: Düzenli olarak oluşturulan büyük miktarlardaki verilerin Azure'a taşınması gerektiği durumlardır. Enerji keşfinde petrol kuyularında ve rüzgar enerjisini santrallerinde oluşturulan videolar örnek olarak verilebilir.

## <a name="the-workflow"></a>İş akışı

Tipik iş akışı aşağıdaki adımlardan oluşur:

1. **Sipariş**: Azure portaldan bir sipariş oluşturup gönderim bilgilerini ve verileriniz için hedef Azure depolama hesabını belirtin. Diskler mevcutsa Azure şifreler, hazırlar ve gönderip bir gönderi takip numarası iletir.

2. **Alma**: Diskler teslim edildikten sonra paketini açın ve kopyalanacak verilerin bulunduğu bilgisayara bağlayın. Disklerin kilidini açın.
    
3. **Verileri kopyalama**: Verileri sürükleyip disklere kopyalayın.

4. **İade**: Diskleri hazırlayın ve Azure veri merkezine geri gönderin.

5. **Yükleme**: Veriler otomatik olarak disklerden Azure'a kopyalanır. Diskler Ulusal Standart ve Teknoloji Kurumu (NIST) yönergelerine uygun ve güvenli bir şekilde silinir.

Bu işlem boyunca tüm durum değişiklikleri e-posta ile bildirilir. Ayrıntılı akış hakkında daha fazla bilgi için bkz. [Azure portalda Data Box Disk dağıtma](data-box-disk-quickstart-portal.md).


## <a name="benefits"></a>Avantajlar

Data Box Disk, büyük miktarlarda veriyi ağ bağlantısını etkilemeden Azure'a taşımak için tasarlanmıştır. Çözümün şöyle avantajları vardır:

- **Hız**: Data Box Disk, USB 3.0 bağlantısını kullanarak 35 TB boyutunda veriyi bir haftadan daha kısa bir sürede Azure'a aktarır.   

- **Kullanımı kolay**: Data Box, kullanımı kolay bir çözümdür.

    - Diskler USB bağlantısı kullanır ve kuruluma neredeyse hiç ihtiyaç duymaz.
    - Disklerin fiziksel boyutu küçük olduğundan kullanımı ve gönderimi kolaydır.
    - Diskler harici güç kaynağına ihtiyaç duymaz.
    - Diskler veri merkezi sunucusu, masaüstü veya dizüstü bilgisayarlar ile kullanılabilir.
    - Bu çözüm, Azure portal üzerinden uçtan uca takip imkanı sunar.    

- **Güvenli**: Data Box Disk diskler, veriler ve hizmet için yerleşik güvenlik önlemlerine sahiptir. 
    - Diskler kurcalamaya karşı korumalıdır ve güvenli güncelleştirmeyi destekler. 
    - Disklerde bulunan veriler her zaman AES 128 bit ile şifrelenir. 
    - Disklerin kilidi yalnızca Azure portaldan alınan anahtarla açılabilir. 
    - Hizmet, Azure güvenlik özelliklerinin koruması altındadır. 
    - Verileriniz Azure'a yüklendikten sonra diskler NIST 800-88r1 standartlarına göre silinir.  
    
Daha fazla bilgi için bkz. [Azure Data Box Disk güvenliği ve veri koruması](data-box-disk-security.md).


## <a name="features-and-specifications"></a>Özellikler ve belirtimler


| Belirtimler                                          | Açıklama              |
|---------------------------------------------------------|--------------------------|
| Ağırlık                                                  | < 2 lb. kutu başına. Kutuda en fazla 5 disk bulunur                |
| Boyutlar                                              | Disk - 2,5" SSD |            
| Kablolar                                                  | Disk başına 1 USB 3.1 kablosu|
| Sipariş başına depolama kapasitesi                              | 40 TB (kullanılabilir: ~ 35 TB)|
| Disk depolama kapasitesi                                   | 8 TB (kullanılabilir: ~ 7 TB)|
| Veri arabirimi                                          | USB   |
| Güvenlik                                                | BitLocker ile önceden şifreleme ve güvenli güncelleştirme <br> Destek anahtarı korumalı diskler <br> Veriler her zaman şifrelenir  |
| Veri aktarımı hızı                                      | dosya boyutuna bağlı olarak en fazla 430 MB/sn      |
|Yönetim                                               | Azure portal |


## <a name="region-availability"></a>Bölge kullanılabilirliği

Data Box Disk önizleme sırasında aşağıdaki Azure bölgelerine veri aktarabilir:


|Azure bölgesi  |Azure bölgesi  |
|---------|---------|
|Batı Orta ABD     |Orta Kanada       |        
|Batı ABD 2     |Doğu Kanada         |     
|Batı ABD     | Batı Avrupa        |      
|Orta Güney ABD   |Kuzey Avrupa     |         
|Orta ABD     |Avustralya Doğu|
|Orta Kuzey ABD  |Avustralya Güneydoğu   |
|Doğu ABD      |Avustralya Orta |
|Doğu ABD 2     |Avustralya Orta 2|


## <a name="pricing"></a>Fiyatlandırma

Önizleme sırasında Data Box Diskleri ücretsiz olarak kullanılabilir. Data Box Disk genel kullanıma sunulduğunda bu durum değişecektir.

## <a name="next-steps"></a>Sonraki adımlar

- [Data Box Disk gereksinimlerini](data-box-disk-system-requirements.md) gözden geçirin.
- [Data Box Disk sınırlarını](data-box-disk-limits.md) anlayın.
- Azure portalında [Azure Data Box Disk](data-box-disk-quickstart-portal.md)’i hızlı dağıtın.
