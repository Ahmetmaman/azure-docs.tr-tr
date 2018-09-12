---
title: include dosyası
description: include dosyası
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 07/06/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: eac6c6d76bcc3b3d9cfeda7d8ca4e52e28ba9d8f
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44369315"
---
Dengeli CPU / bellek oranı genel amaçlı sanal makine boyutları sağlar. Test ve geliştirme, küçük - orta boyutlu veritabanları, düşük - orta yoğunluklu trafiğe sahip web sunucuları için idealdir. Bu makalede, Vcpu, veri diskleri ve NIC yanı sıra bu gruplandırmaki boyutları için depolama aktarım hızı sayısı hakkında bilgi sağlar. 

- Av2 serisi VM'ler, çeşitli donanım türleri ve işlemciler üzerinde dağıtılabilir. A serisi VM'ler, geliştirme ve test gibi giriş düzeyinde iş yüküne en uygun CPU performansına ve bellek yapılandırmasına sahiptir. Dağıtıldığı donanımdan bağımsız olarak, çalışan örneğe tutarlı işlemci performansı sunmak için boyut donanıma göre genişletilir. Bu boyutun dağıtıldığı fiziksel donanımı belirlemek için Sanal Makinenin içinden sanal donanımı sorgulayın.

  Kullanım örnekleri, geliştirme içerir ve sunucuları, düşük trafikli web sunucuları, Orta büyüklükte veri tabanları, kavram kanıtı ve kod depoları küçük test edin.

- Dv2 serisi, orijinal D serisinin devamı daha güçlü bir CPU ve üretim iş yüklerinin çoğu için uygun hale getirme en iyi CPU / bellek yapılandırması sunar. Dv2 Serisi CPU, D Serisi CPU'dan yaklaşık %35 daha hızlıdır. Temel en yeni nesil Intel Xeon® E5-2673 v3 (Haswell) 2,4 GHz veya E5-2673 v4 (Broadwell) 2,3 GHz işlemcileri ve Intel Turbo Boost Technology 2.0 ile 3,1 GHz'e varan hızlara çıkabilir. Dv2 Serisi, D Serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.

- Dv3 serisi özellikleri 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) işlemci veya en son 2,3 GHz Intel XEON® E5-2673 v4 (Broadwell) işlemciyi sağlayarak daha iyi bir değer önerisi en genel amaçlı iş yükleri için hiper iş parçacıklı bir yapılandırmada,.  Disk ve ağ sınırlarını hiper iş parçacıklı Git ile hizalamak için çekirdek başına temelinde ayarlanmış durumdayken (Başlangıç ~3.5 GiB/vCPU için 4 GiB/vCPU) bellek genişletildi.  Dv3 artık yüksek bellek VM boyutları D/Dv2 ailelerinin sahipse, bu yeni Ev3 ailesi için taşınmıştır.

  D serisi kullanım örnekleri, kurumsal düzeyde uygulamalar, ilişkisel veritabanları, bellek içi önbelleğe alma ve analiz içerir. 

## <a name="b-series"></a>B serisi

Premium Depolama: desteklenir

Premium depolama önbelleğe alma: Desteklenmez

B serisi seri aktarıma uygun VM'ler CPU'nun tam performansta küçük veritabanları ve geliştirme gibi web sunucuları gibi sürekli olarak gerekir ve test ortamları iş yükleri için idealdir. Bu iş yükleri genellikle seri aktarıma uygun performans gereksinimleri vardır. B serisi, bu müşteriler VM temel performansını sayısından az kullanarak kredi oluşturulacak VM örneği izin veren bir fiyat bilinçli temel performans ile bir VM boyutu satın almanıza olanak sağlar. VM VM kredi toplandığında, daha yüksek bir CPU performans uygulamanızın gerektirdiği en fazla % 100 CPU kullandığınızda sanal makinenin temel veri bloğu.

Örnek kullanım durumları, geliştirme ve test sunucuları, düşük trafikli web sunucuları, küçük veritabanları, mikro hizmetler, -kavram, derleme sunucuları için sunucuları içerir.


| Boyut             | Sanal işlemci  | Bellek: GiB | Yerel SSD: GiB | Bir çekirdeği temel performans | Bankaya nakledilen KREDİLERİ / saat | En fazla KREDİLERİ Bankaya nakledilen | Maksimum veri diskleri | Maksimum yerel disk perf: IOPS / MB/sn | Maksimum önbelleğe alınmamış disk perf: IOPS / MB/sn | En fazla NIC |          
|---------------|-------------|----------------|----------------------------|-----------------------|--------------------|----------------|----------------------------------------|-------------------------------------------|-------------------------------------------|----------|
| Standard_B1s  | 1           | 1              | 4                          | %10                   | 6                  | 144            | 2                                      | 400 / 10                                  | 320 / 10                                  | 2  |
| Standard_B1ms | 1           | 2              | 4                          | %20                   | 12                 | 288            | 2                                      | 800 / 10                                  | 640 / 10                                  | 2  |
| Standard_B2s  | 2           | 4              | 8                          | 40%                   | 24                 | 576            | 4                                      | 1600 / 15                                 | 1280 / 15                                 | 3  |
| Standard_B2ms | 2           | 8              | 16                         | 60%                   | 36                 | 864            | 4                                      | 2400 / 22.5                               | 1920 / 22.5                               | 3  |
| Standard_B4ms | 4           | 16             | 32                         | 90%                   | 54                 | 1296           | 8                                      | 3600 / 35                                 | 2880 / 35                                 | 4  |
| Standard_B8ms | 8           | 32             | 64                         | 135%                  | 81                 | 1944           | 16                                     | 4320 / 50                                 | 4320 / 50                                 | 4  |


## <a name="dsv3-series-sup1sup"></a>Dsv3 serisi <sup>1</sup>

ACU: 160-190

Premium Depolama: desteklenir

Premium depolama önbelleğe alma: desteklenir

Dsv3 serisi boyutlarındaki dayalı 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) işlemci veya en son 2,3 GHz Intel XEON® E5-2673 v4, Intel Turbo Boost Technology 2.0 ile 3,5 GHz hıza ve premium depolama kullanan (Broadwell) işlemciyi. Dsv3 serisi boyutları, üretim iş yüklerinin çoğu için uygun bir vCPU, bellek ve geçici depolama kombinasyonu sunar.


| Boyut             | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_D2s_v3  | 2      | 8           | 16             | 4              | 4,000 / 32 (50)                                                       | 3200/48                                | 2 / 1,000                                   |
| Standard_D4s_v3  | 4      | 16          | 32             | 8              | 8,000 / 64 (100)                                                      | 6400/96                                | 2 / 2,000                                   |
| Standard_D8s_v3  | 8      | 32          | 64             | 16             | 16,000 / 128 (200)                                                    | 12.800/192                              | 4 / 4,000                                      |
| Standard_D16s_v3 | 16     | 64          | 128            | 32             | 32,000 / 256 (400)                                                    | 25.600/384                              | 8 / 8,000                                      |
| Standard_D32s_v3 | 32     | 128          | 256            | 32             | 64,000 / 512 (800)                                                    | 51.200/768                              | 8 / 16,000                                               |
| Standard_D64s_v3 | 64     | 256          | 512            | 32             | 128,000 / 1024 (1600)                                                    | 80,000 / 1200                              | 8 / 30,000                                               |

<sup>1</sup> Dsv3 serisi sanal makineler Intel® Hyper-Threading teknolojisine özelliği

## <a name="dv3-series-sup1sup"></a>Dv3 serisi <sup>1</sup>

ACU: 160-190

Premium Depolama: Desteklenmez

Premium depolama önbelleğe alma: Desteklenmez

Dv3 serisi boyutları olan bağlı 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) işlemci veya 2,3 GHz Intel XEON® E5-2673 v4 (Broadwell) işlemciyi, Intel Turbo Boost Technology 2.0 ile 3,5 GHz hıza çıkabilir. Dv3 serisi boyutları, üretim iş yüklerinin çoğu için uygun bir vCPU, bellek ve geçici depolama kombinasyonu sunar.

Veri disk depolaması, sanal makinelerden ayrı olarak faturalandırılır. Premium depolama disklerini kullanmak için Dsv3 boyutlarını kullanın. Dsv3 boyutları için fiyatlandırma ve faturalandırma oranları Dv3 serisi ile aynıdır. 


| Boyut            | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | Maksimum NIC/Ağ bant genişliği |
|-----------------|-----------|-------------|----------------|----------------|----------------------------------------------------------|------------------------------|
| Standard_D2_v3  | 2         | 8           | 50             | 4              | 3000/46/23                                               | 2 / 1,000                    |
| Standard_D4_v3  | 4         | 16          | 100            | 8              | 6000/93/46                                               | 2 / 2,000                    |
| Standard_D8_v3  | 8         | 32          | 200            | 16             | 12000/187/93                                             | 4 / 4,000                    |
| Standard_D16_v3 | 16        | 64          | 400            | 32             | 24000/375/187                                            | 8 / 8,000                    |
| Standard_D32_v3 | 32        | 128          | 800            | 32             | 48000/750/375                                            | 8 / 16,000                             |
| Standard_D64_v3 | 64        | 256          | 1600            | 32             | 96000/1000/500                                            | 8 / 30,000                             |

<sup>1</sup> Dv3 serisi sanal makineler Intel® Hyper-Threading teknolojisine özelliği


## <a name="dsv2-series"></a>DSv2 serisi

ACU: 210-250

Premium Depolama: desteklenir

Premium depolama önbelleğe alma: desteklenir

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1_v2 |1 |3,5 |7 |4 |4000/32 (43) |3200/48 |2 / 750 |
| Standard_DS2_v2 |2 |7 |14 |8 |8000/64 (86) |6400/96 |2 / 1500 |
| Standard_DS3_v2 |4 |14 |28 |16 |16.000/128 (172) |12.800/192 |4 / 3000 |
| Standard_DS4_v2 |8 |28 |56 |32 |32.000/256 (344) |25.600/384 |8 / 6000 |
| Standard_DS5_v2 |16 |56 |112 |64 |64.000/512 (688) |51.200/768 |8 / 12000 |



## <a name="dv2-series"></a>Dv2 Serisi

ACU: 210-250

Premium Depolama: Desteklenmez

Premium depolama önbelleğe alma: Desteklenmez

| Boyut           | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | Maksimum veri diskleri | Aktarım hızı: IOPS | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
|----------------|------|-------------|------------------------|------------------------------------------------------------|----------------|------------------|----------------------------------------------|
| Standard_D1_v2 | 1    | 3,5         | 50                     | 3000/46/23                                             | 4              | 4x500            | 2 / 750                                      |
| Standard_D2_v2 | 2    | 7           | 100                    | 6000/93/46                                             | 8              | 8x500            | 2 / 1500                                     |
| Standard_D3_v2 | 4    | 14          | 200                    | 12000/187/93                                           | 16             | 16x500           | 4 / 3000                                       |
| Standard_D4_v2 | 8    | 28          | 400                    | 24000/375/187                                          | 32             | 32x500           | 8 / 6000                                       |
| Standard_D5_v2 | 16   | 56          | 800                    | 48000/750/375                                          | 64             | 64 x 500           | 8 / 12000                                    |


## <a name="av2-series"></a>Av2 Serisi

ACU: 100

Premium Depolama: Desteklenmez

Premium depolama önbelleğe alma: Desteklenmez

| Boyut            | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | Maksimum veri diski/aktarım hızı: IOPS | Maks NIC / beklenen ağ bant genişliği (MB/sn) | 
|-----------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_A1_v2  | 1         | 2           | 10             | 1000/20/10                                           | 2/2x500               | 2 / 250                 |
| Standard_A2_v2  | 2         | 4           | 20             | 2000/40/20                                           | 4/4x500               | 2 / 500                 |
| Standard_A4_v2  | 4         | 8           | 40             | 4000/80/40                                           | 8/8x500               | 4 / 1000                     |
| Standard_A8_v2  | 8         | 16          | 80             | 8000/160/80                                          | 16/16x500             | 8 / 2000                     |
| Standard_A2m_v2 | 2         | 16          | 20             | 2000/40/20                                           | 4/4x500               | 2 / 500                 |
| Standard_A4m_v2 | 4         | 32          | 40             | 4000/80/40                                           | 8/8x500               | 4 / 1000                     |
| Standard_A8m_v2 | 8         | 64          | 80             | 8000/160/80                                          | 16/16x500             | 8 / 2000                     |

<br>





