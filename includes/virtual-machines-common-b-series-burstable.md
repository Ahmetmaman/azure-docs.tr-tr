---
title: include dosyası
description: include dosyası
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 14feb7ad09a24904034f9ae90cf4a54cf786a44c
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44369468"
---
B serisi VM ailesi, hangi sanal makine boyutu %100 Intel® Broadwell E5-2673 v4'üne kadar CPU performans düzeyine çıkış yapması özelliği sayesinde, iş yükü için gereken temel düzeyde performans sağlar seçmenizi sağlar 2.3 GHz veya bir Intel® Haswell 2.4 GHz E5-2673 v3 işlemci vCPU.

B serisi sanal makineler, web sunucuları gibi kavramları, küçük veritabanları ve geliştirme yapı ortamları sağlama CPU'nun tam performansta sürekli olarak ihtiyacınız olan iş yükleri için idealdir. Bu iş yükleri genellikle seri aktarıma uygun performans gereksinimleri vardır. B serisi, bir VM boyutu temel performans satın almanıza olanak sağlar ve taban sayısından az kullanılırken, sanal makine örneği kredi oluşturur. VM VM kredi toplandığında, daha yüksek bir CPU performans uygulamanızın gerektirdiği kadar vCPU %100 kullandığınızda temel veri bloğu.

B serisi, aşağıdaki altı VM boyutlarında gelir:

| Boyut          | vCPU'ın | Bellek: GiB | Geçici depolama (SSD) GiB | Sanal makinenin temel bir CPU performans | En fazla CPU performans VM | Bankaya nakledilen KREDİLERİ / saat | En fazla KREDİLERİ Bankaya nakledilen |
|---------------|--------|-------------|----------------|--------------------------------|---------------------------|-----------------------|--------------------|
| Standard_B1s  | 1      | 1           | 4              | %10                            | 100%                      | 6                     | 144                |
| Standard_B1ms | 1      | 2           | 4              | %20                            | 100%                      | 12                    | 288                |
| Standard_B2s  | 2      | 4           | 8              | 40%                            | 200%                      | 24                    | 576                |
| Standard_B2ms | 2      | 8           | 16             | 60%                            | 200%                      | 36                    | 864                |
| Standard_B4ms | 4      | 16          | 32             | 90%                            | 400%                      | 54                    | 1296               |
| Standard_B8ms | 8      | 32          | 64             | 135%                           | 800%                      | 81                    | 1944               |




## <a name="q--a"></a>Soru-Cevap 

### <a name="q-how-do-you-get-135-baseline-performance-from-a-vm"></a>S: nasıl bir VM'den %135 temel performans elde ederim?
**A**: %135 yaptığınız VM boyutu 8 vCPU's arasında paylaşılır. Örneğin, 4, 8 çekirdek toplu işlem üzerinde çalışan uygulama kullanıyorsa ve % 30 kullanımı sırasında çalışan her biri bu 4 vCPU's VM CPU performansı toplam miktarı %120 eşit.  Sanal makinenizin temel performansınızı % 15 deltasında dayalı kredi zaman oluşturursunuz anlamına gelir.  Ancak, aynı VM tüm 8 vCPU, %100 kullanabileceğiniz krediler olduğunda bu VM bir en fazla CPU performans %800 vererek, ayrıca anlamına gelir.


### <a name="q-how-can-i-monitor-my-credit-balance-and-consumption"></a>S: my kredi bakiye ve tüketimi nasıl izleyebilirim
**A**: şu 2 yeni ölçümler önümüzdeki haftalarda sunmuyoruz **kredi** ölçüm, kaç tane VM'niz Bankaya nakledilen KREDİLERİ görüntülemenize olanak sağlayacaktır ve **ConsumedCredit** ölçüm, kaç CPU gösterilir KREDİLERİ sanal makinenize banka tüketti.    Portalında veya Azure İzleyici API'ler aracılığıyla programlı olarak ölçümleri bölmesinden bu ölçümleri görüntüleme olanağınız olacaktır.

Ölçüm verilerini Azure için erişim hakkında daha fazla bilgi için bkz. [Microsoft azure'da ölçümlere genel bakış](../articles/monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="q-how-are-credits-accumulated"></a>S: nasıl KREDİLERİ toplanır?
**A**: VM birikmesi ve Tüketim oranları, tamamen kendi temel performans düzeyinde çalışan bir VM ağ birikmesi veya KREDİLERİ Patlaması, tüketim ne sağlayacak şekilde ayarlanır.  Bir VM, temel performans düzeyinin altında çalışır durumda olduğunda krediler net bir artış olacaktır ve VM temel performans düzeyiyle birden çok CPU kullanan her KREDİLERİ net bir düşüş olur.

**Örnek**: küçük zaman ve katılım veritabanı Uygulamam için B1ms boyutunu kullanarak VM dağıtabilir. Bu boyut en fazla %20 bir vCPU, 0.2 kredi veya banka kullanabilirim dakika başına my taban çizgisi olarak kullanılacak Uygulamam sağlar. 

Uygulamamın 7:00-9:00 ÖÖ ve 4:00-18:00:00 arasında my çalışanların iş günü sonunda ve başındaki meşgul. Diğer 20 saatleri günün Uygulamam genellikle olduğu anda, yalnızca % 10 ' vCPU kullanarak boş. Yoğun olmayan saatlere miyim dakika başına 0.2 KREDİLERİ kazanın, ancak sanal Makinem 0,1 x 60 = 6 banka şekilde yalnızca dakika başına 0.l aktarımla birlikte bunların KREDİLERİ / saat.  20, ı yoğun olmayan saatler için ı 120 KREDİLERİ banka.  

Yoğun saatlerde Uygulamam % 60 vCPU kullanımı ortalama, ı hala dakika başına 0.2 KREDİLERİ kazanın ancak ben dakika başına 0,6 aktarımla birlikte bunların, 0,4 KREDİLERİ net maliyeti için bir dakika ya da 0.4 x 60 = 24 saatlik kredisi. En yüksek kullanımı, günde 4 saat sahibim 4 x 24 = 96 maliyetleri için en yüksek kullanımı için kredi.

120 miyim kazanılan krediler yoğun alın ve benim için tepe zamanları kullandım 96 KREDİLERİ çıkarma, ı kullanabileceğim diğer etkinlik patlamalarına günde bir ek 24 KREDİLERİ banka.


### <a name="q-does-the-b-series-support-premium-storage-data-disks"></a>S: B serisi, Premium depolama diskleri destekleyen mu?
**A**: Evet, Premium depolama diskleri tüm B serisi boyutları destekler.   
    
### <a name="q-why-is-my-remaining-credit-set-to-0-after-a-redeploy-or-a-stopstart"></a>S: neden kalan kredilerimi sonra bir yeniden dağıtın veya durdurmak/başlatmak 0 olarak ayarlanır?
**A** : olduğunda bir VM olan "REDPLOYED" ve sanal Makineyi başka bir düğüme taşır birikmiş kredi kaybolur. VM durduruldu ve başlatıldı ancak aynı düğümde kalır, VM birikmiş kredi korur. Sanal makine bir düğüm üzerinde yeni başlatıldığında bir başlangıç kredisi alır, Standard_B8ms için 240 dakika.

    

    
