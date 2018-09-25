---
title: Azure ön kapısı hizmeti - arka uç sistem durumu izleme | Microsoft Docs
description: Bu makale, nasıl Azure ön kapısı hizmet uçlarınıza izler anlamanıza yardımcı olur.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 256d530590fadc9e2aeb1ea1efb7a52608014978
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46988572"
---
# <a name="health-probes"></a>Sistem durumu araştırmaları

Her bir arka uç sistem durumu belirlemek için her bir ön kapısı ortamı düzenli aralıklarla yapay bir HTTP/HTTPS isteği her yapılandırılmış uçlarınıza gönderir. Ön kapı, bu araştırmaların alınan yanıtları sonra "iyi" arka uçları için gerçek istemci isteklerini yönlendirmek belirlemek için kullanır.


## <a name="supported-protocols"></a>Desteklenen protokoller

Ön kapısı gönderen bir araştırmalarla HTTP veya HTTPS protokollerini destekler. Bu araştırmaların yönlendirme istemci istekleri için yapılandırılan aynı TCP bağlantı noktaları üzerinden gönderilir, geçersiz kılınamaz.

## <a name="health-probe-responses"></a>Sistem durumu araştırma yanıtları

| Yanıtlar  | Açıklama | 
| ------------- | ------------- |
| Sistem durumu belirleme  |  200 Tamam durum kodu, arka uç kötü durumda gösterir. Diğer her şey bir hata olarak kabul edilir. Herhangi bir nedenle (ağ hatası dahil) için bir araştırma geçerli bir HTTP yanıt alınmazsa, araştırma hata olarak kabul edilir.|
| Gecikme süresini ölçme  | Gecikme süresi, araştırma isteğinin yanıtın son baytını aldığımız zaman şu hemen göndereceğiz önce şu ölçülen duvar saati zamanı geldi. Bu ölçüm arka uçları sıcak var olan bağlantıları doğru ağırlıklı olmayan şekilde her istek için yeni TCP bağlantısı kullanırız.  |

## <a name="how-front-door-determines-backend-health"></a>Ön kapısı arka uç sistem durumu nasıl belirler

Azure ön kapısı hizmeti aynı üç adımlı işlem aşağıda sistem tüm algoritmaları kullanır.

1. Dışlama arka uçları devre dışı.

2. Sistem durumu araştırmaları hataları olan arka uçları hariç tut:
    * Bu seçim, son bakarak yapılır _n_ sistem durumu araştırma yanıtları. Varsa, en az _x_ hatasız arka uç sağlıklı olarak değerlendirilir.

    * _n_ yük dengeleme ayarlarını samplesıze'ı özelliğini değiştirerek yapılandırılır.

    * _x_ yük dengeleme ayarlarını SuccessfulSamplesRequired özelliğini değiştirerek yapılandırılır.

3. Arka uç havuzunda iyi durumda arka uçları kümesi dışında ön kapısı ayrıca ölçer ve her arka uç gecikme süresi (gidiş-dönüş süresi) korur.


## <a name="complete-health-probe-failure"></a>Tüm sistem durumu araştırma hatası

Bir arka uç havuzundaki her arka uç için sistem durumu araştırmaları başarısız olursa ön kapısı tüm arka uçlar sağlıklı olarak değerlendirir ve hepsini bir kez deneme dağıtımlarında tümünün trafiği yönlendirir.

Herhangi bir arka uçtan bir sağlık durumuna geri döndüğünde, ön kapısı normal Yük Dengeleme algoritmasını devam edecek.

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [ön kapı oluşturmak](quickstart-create-front-door.md).
- Bilgi [ön kapısı işleyişi](front-door-routing-architecture.md).
