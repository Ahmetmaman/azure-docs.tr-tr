---
title: dosya dahil etme
description: dosya dahil etme
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 02/07/2019
ms.author: robb
ms.custom: include file
ms.openlocfilehash: a25f28b19e0f00830fd0290ff0296c317b9a5ed9
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91371772"
---
**Veri toplama birimi ve bekletme** 

| Katman | Gün başına sınır | Veri saklama | Yorum |
|:---|:---|:---|:---|
| Geçerli GB başına Fiyatlandırma Katmanı<br>(2018 Nisan 'da tanıtılan) | Sınır yok | 30-730 gün | 31 günden daha fazla veri saklama, ek ücretler için kullanılabilir. Azure Izleyici fiyatlandırması hakkında daha fazla bilgi edinin. |
| Eski ücretsiz Katmanlar<br>(2016 Nisan 'da tanıtılan) | 500 MB | 7 gün | Çalışma alanınız günde 500 MB sınırına ulaştığında, veri alımı durduruluyor ve sonraki günün başlangıcında devam eder. Bir gün için UTC temel alınır. Azure Güvenlik Merkezi tarafından toplanan verilerin bu 500 MB/gün sınırına dahil edilmediğini ve bu sınırın üzerine toplanmaya devam edileceğini unutmayın.  |
| GB katmanı başına eski tek başına<br>(2016 Nisan 'da tanıtılan) | Sınır yok | 30 ila 730 gün | 31 günden daha fazla veri saklama, ek ücretler için kullanılabilir. Azure Izleyici fiyatlandırması hakkında daha fazla bilgi edinin. |
| Düğüm başına eski (OMS)<br>(2016 Nisan 'da tanıtılan) | Sınır yok | 30 ila 730 gün | 31 günden daha fazla veri saklama, ek ücretler için kullanılabilir. Azure Izleyici fiyatlandırması hakkında daha fazla bilgi edinin. |
| Eski Standart katman | Sınır yok | 30 gün  | Saklama ayarlanamıyor |
| Eski Premium katmanı | Sınır yok | 365 gün  | Saklama ayarlanamıyor |

**Abonelik başına çalışma alanı sayısı.**

| Fiyatlandırma katmanı    | Çalışma alanı sınırı | Yorumlar
|:---|:---|:---|
| Ücretsiz katmanı  | 10 | Bu sınır arttırılamıyor. |
| Diğer tüm katmanlar | Sınır yok | Bir kaynak grubu içindeki kaynak sayısıyla ve abonelik başına kaynak grubu sayısına göre sınırlandırılırsınız. |

**Azure Portal**

| Kategori | Sınır | Yorumlar |
|:---|:---|:---|
| Günlük sorgusu tarafından döndürülen en fazla kayıt sayısı | 10,000 | Sorgu kapsamını, zaman aralığını ve sorgudaki filtreleri kullanarak sonuçları küçültün. |


**Veri Toplayıcı API 'SI**

| Kategori | Sınır | Yorumlar |
|:---|:---|:---|
| Tek gönderi için en büyük boyut | 30 MB | Daha büyük birimleri birden çok gönderime ayırın. |
| Alan değerleri için en büyük boyut  | 32 KB | 32 KB'den daha uzun alanlar kesilir. |

**Arama API’si**

| Kategori | Sınır | Yorumlar |
|:---|:---|:---|
| Tek bir sorguda döndürülen en fazla kayıt sayısı | 500.000 | |
| Döndürülen en fazla veri boyutu | 64.000.000 bayt (~ 61 MIB)| |
| En fazla sorgu çalışma süresi | 10 dakika | Ayrıntılar için bkz. [zaman aşımları](https://dev.loganalytics.io/documentation/Using-the-API/Timeouts) .  |
| En yüksek istek hızı | Azure AD kullanıcısı veya istemci IP adresi başına 30 saniyelik 200 istek | Ayrıntılar için [Hız sınırlarına](https://dev.loganalytics.io/documentation/Using-the-API/Limits) bakın. |

**Genel çalışma alanı sınırları**

| Kategori | Sınır | Yorumlar |
|:---|:---|:---|
| Tablodaki en fazla sütun         | 500 | |
| Sütun adı için en fazla karakter | 500 | |
| Veri dışarı aktarma | Şu anda kullanılamıyor | Verileri toplamak ve dışarı aktarmak için Azure Işlevini veya mantıksal uygulamayı kullanın. | 

**<a name="data-ingestion-volume-rate">Veri alımı birim oranı</a>**

Azure Izleyici, her ay büyüyen bir hızda çok sayıda müşteriye hizmet veren binlerce müşteriyi sunan yüksek ölçekli bir veri hizmetidir. Birim hızı sınırı, çok kiracılı ortamdaki ani artış artışlarından Azure Izleyici müşterilerinin yalıtılmayı amaçlamaktadır. Çalışma alanlarında 500 MB (sıkıştırılmış) varsayılan Alım birimi hız eşiği tanımlanmıştır, bu, yaklaşık **6 GB/dak** sıkıştırılmamış değere çevrilir. gerçek boyut, günlük uzunluğuna ve sıkıştırma oranına bağlı olarak veri türleri arasında farklılık gösterebilir. Toplu hız sınırı, [Tanılama ayarları](../articles/azure-monitor/platform/diagnostic-settings.md), [Veri Toplayıcı API 'si](../articles/azure-monitor/platform/data-collector-api.md) veya Aracılar kullanılarak Azure kaynaklarından gönderilmeksizin, alınan tüm veriler için geçerlidir.

Çalışma alanınızda yapılandırılan eşiğin %80 ' inden daha yüksek olan bir çalışma alanına veri gönderdiğinizde, eşik aşılmaya devam edilirken her 6 saatte bir bir olay gönderilir *Operation* . Alınan birim oranı eşiğin üstünde olduğunda, bazı veriler bırakılır ve eşik aşılmaya devam edilirken her 6 saatte bir olay, çalışma alanınızda *işlem* tablosuna gönderilir. Alım hacminin oranı eşiği aşmaya devam ediyorsa veya kısa bir süre sonra bu duruma ulaşmayı bekliyorsanız, bir destek isteği açarak onu ' de artırma isteğinde bulunabilir. 

Çalışma alanınızda geçen birim hızı sınırına yaklaşmaya veya ulaşmaya yönelik bildirim almak için, sıfıra kıyasla sonuç sayısı, 5 dakikalık değerlendirme süresi ve 5 dakikalık sıklık üzerinde uyarı mantığı temeli ile aşağıdaki sorguyu kullanarak bir [günlük uyarı kuralı](../articles/azure-monitor/platform/alerts-log.md) oluşturun.

Alım birimi oranı eşiği geçti
```Kusto
Operation
| where Category == "Ingestion"
| where OperationKey == "Ingestion rate limit"
| where Level == "Error"
```

Alma birimi oranı eşiğin %80 ' ü geçti
```Kusto
Operation
| where Category == "Ingestion"
| where OperationKey == "Ingestion rate limit"
| where Level == "Warning"
```

Alma birimi oranı eşiğin %70 ' ü geçti
```Kusto
Operation
| where Category == "Ingestion"
| where OperationKey == "Ingestion rate limit"
| where Level == "Info"
```

>[!NOTE]
>Log Analytics kullanmaya ne kadar süre kullandığınıza bağlı olarak eski fiyatlandırma katmanlarına erişiminiz olabilir. [Eski Log Analytics fiyatlandırma katmanları](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#legacy-pricing-tiers)hakkında daha fazla bilgi edinin. 
