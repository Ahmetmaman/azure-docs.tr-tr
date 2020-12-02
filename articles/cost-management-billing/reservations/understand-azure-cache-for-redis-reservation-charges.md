---
title: Redis için Azure Cache’e rezervasyon indiriminin nasıl uygulandığını öğrenme | Microsoft Docs
description: Rezervasyon indiriminin Redis için Azure Cache örneklerine nasıl uygulandığını öğrenin.
author: yegu-ms
manager: maiye
ms.service: cache
ms.topic: conceptual
ms.date: 01/22/2020
ms.author: yegu
ms.openlocfilehash: 5f9e0a18db0920acd35ebd7b133ed3fe5d0eaee9
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96352958"
---
# <a name="how-the-reservation-discount-is-applied-to-azure-cache-for-redis"></a>Redis için Azure Cache’e rezervasyon indiriminin nasıl uygulandığını öğrenme

Bir Redis için Azure Cache ayrılmış kapasitesi satın aldığınızda, rezervasyonun öznitelikleriyle ve miktarıyla eşleşen SQL Veritabanlarına otomatik olarak rezervasyon indirimi uygulanır. Rezervasyon yalnızca Redis için Azure Cache işlem maliyetlerini kapsar. Depolama ve ağ iletişimi için normal fiyatlarla ücretlendirilirsiniz. Ayrılmış kapasite yalnızca [premium katmanı](../../azure-cache-for-redis/quickstart-create-redis.md) önbellekler için sunulur.

## <a name="how-reservation-discount-is-applied"></a>Rezervasyon indiriminin uygulanması

Rezervasyon indirimi ***kullanılmadığı takdirde hakkı kaybedilen*** bir özelliktir. Bu nedenle, herhangi bir saat için eşleşen kaynaklarınız yoksa, o saate ait rezervasyon miktarını kaybedersiniz. Kullanılmayan ayrılmış saatleri devredemezsiniz.

Bir kaynağı kapattığınızda rezervasyon indirimi, belirtilen kapsamdaki başka bir eşleşen kaynağa otomatik olarak uygulanır. Belirtilen kapsamda eşleşen kaynak bulunamazsa, ayrılan saatler kaybedilir.

## <a name="discount-applied-to-azure-cache-for-redis"></a>Redis için Azure Cache’e uygulanan indirim

Redis için Azure Cache ayrılmış kapasite indirimi, önbelleklerinize saatlik olarak uygulanır. Satın aldığınız rezervasyon, çalışan önbelleklerin gösterdiği işlem kullanımı ile eşleştirilir. Bu önbellekler saatin tamamı boyunca çalışmadığında, rezervasyon öznitelikleriyle eşleşen diğer SQL veritabanlarına otomatik olarak rezervasyon uygulanır. İndirim, eş zamanlı olarak çalışan önbellekler için de geçerli olabilir. Rezervasyon öznitelikleriyle eşleşen ve saatin tamamı boyunca çalışan önbellekleriniz yoksa ilgili saat için rezervasyon indiriminden tam olarak yararlanamazsınız.

Aşağıdaki örneklerde, satın aldığınız önbellek sayısına ve çalıştırma zamanına bağlı olarak Redis için Azure Cache ayrılmış kapasite indiriminin nasıl uygulandığı gösterilmektedir.

_ **Örnek 1**: 6 GB’lık önbellek için Redis için Azure Cache ayrılmış kapasitesi satın aldığınızı varsayalım. Rezervasyonun diğer öznitelikleriyle eşleşen 13 GB’lık önbellek çalıştırıyorsanız Redis için Azure Cache işlem kullanımınızın 7 GB’ı için kullandıkça öde fiyatı üzerinden ücretlendirilir ve 6 GB önbellek işlem kullanımı için bir saatlik rezervasyon indirimi elde edersiniz.

Bu örneklerin geri kalanında, satın aldığınız Redis için Azure Cache ayrılmış kapasitesinin 26 GB önbellek için olduğunu ve rezervasyon özniteliklerinin çalışan önbellek ile eşleştiğini varsayalım.

* **Örnek 2**: 13 GB’lık iki önbelleği bir saat boyunca çalıştırdığınızı varsayalım. 26 GB’lık rezervasyon indirimi her iki önbelleğin işlem kullanımına uygulanır.

* **Örnek 3**: 13.00 - 13.30 arasında bir 26 GB’lık önbellek çalıştırdığınızı varsayalım. 13.30 - 14.00 arasında farklı bir 26 GB’lık önbellek çalıştırdığınızı da varsayalım. Her ikisi de rezervasyon indirimi kapsamındadır.

* **Örnek 4**: 13.00 - 13.45 arasında bir 26 GB’lık önbellek çalıştırdığınızı varsayalım. 13.30 - 14.00 arasında farklı bir 26 GB’lık önbellek çalıştırdığınızı da varsayalım. 15 dakikalık çakışma için kullandıkça öde fiyatıyla ücretlendirilirsiniz. Rezervasyon indirimi, geri kalan süre boyunca işlem kullanımına uygulanır.

Faturalama kullanım raporlarında Azure rezervasyonlarınızın uygulamasını anlamak ve görüntülemek için bkz. [Azure rezervasyon kullanımınızı anlama](./understand-reserved-instance-usage-ea.md).

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun
Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).