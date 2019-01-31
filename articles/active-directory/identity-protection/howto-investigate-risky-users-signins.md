---
title: Riskli kullanıcılar ve oturum açma (yenilenmiş) Azure Active Directory kimlik koruması, araştırma | Microsoft Docs
description: Riskli kullanıcılar ve oturum açma (yenilenmiş) Azure Active Directory kimlik koruması, araştırma hakkında bilgi edinin.
services: active-directory
keywords: Azure active directory kimlik koruması, bulut uygulaması bulma, yönetme, uygulamaları, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.author: markvi
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.date: 01/25/2019
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.reviewer: raluthra
ms.openlocfilehash: f72819bacd7c65ced7ccff3665789d3b96c5e30f
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55478381"
---
# <a name="how-to-investigate-risky-users-and-sign-ins"></a>Nasıl Yapılır: Riskli kullanıcıları ve oturum açma işlemlerini araştırma 


Riskli oturum açma işlemleri ve riskli kullanıcılar raporları kullanarak araştırın ve ortamınızda risk öngörü. Filtreleme ve kullanıcıların ve riskli oturum açma işlemleri sıralama olanağı, olası yetkisiz erişim kuruluşunuza daha iyi anlayabilirsiniz. 


## <a name="risky-users-report"></a>Riskli kullanıcılar raporu

Riskli kullanıcılar raporuyla sağlanan bilgiler sayesinde aşağıdakiler gibi soruların yanıtlarını bulabilirsiniz:

- Hangi kullanıcıların yüksek riskli misiniz?
- Bir risk durumu kullanıcınız düzeltilen?



Bu rapor için ilk giriş noktanız, **Araştır** Güvenlik sayfasında bölümü.

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/01.png)


Riskli kullanıcılar raporu aşağıdakileri gösteren bir varsayılan görünümü vardır:

- Ad

- Risk durumu

- Risk düzeyi

- Risk ayrıntısı

- Riskin son güncelleştirilmesi

- Type

- Durum
 

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/03.png)


Araç çubuğunda **Sütunlar**’a tıklayarak liste görünümünü özelleştirebilirsiniz.

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/04.png)

Sütunları iletişim kutusu ek alanları görüntüleyebilir ya da zaten görüntülenen alanları kaldırabilirsiniz olanak sağlar.

Liste görünümündeki bir öğeye tıklayarak bu öğe hakkında mevcut olan tüm ayrıntıları yatay bir görünümde alabilirsiniz.

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/05.png)


Detaylarını gösterir:

- Temel bilgiler

- Son riskli oturum açmalar

- Bir oturum açma adına bağlı olmayan riskli olaylar

- Risk geçmişi



Buna ek olarak, şunları yapabilirsiniz:

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/08.png)

- Bu kullanıcı için oturum açma işlemleri raporu görüntülemek için tüm oturum açma kısayol görüntüleyin.

- Tüm riskli oturum açma tüm oturum açma, riskli olarak işaretlenmiş işlemleri bu kullanıcı için görüntülenecek işlemleri görüntüleyin.

- Kullanıcının kimliğini tehlikede olduğunu düşünüyorsanız, bir kullanıcının parolasını sıfırlayın.

- Bir kullanıcının etkin risk olayları hatalı pozitif sonuç olduğunu düşünüyorsanız, kullanıcı riski yok sayın. Daha fazla bilgi için [algılama doğruluğunu artırmak nasıl](howto-improve-detection-accuracy.md).



### <a name="filter-risky-users"></a>Filtre riskli kullanıcılar

Raporlanan verileri kendinize uygun bir seviyeye gelecek şekilde daraltmak için aşağıdaki varsayılan alanları kullanarak riskli kullanıcı verilerini filtreleyebilirsiniz:

- Ad

- Kullanıcı adı

- Risk durumu

- Risk düzeyi

- Type

- Durum

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/06.png)



**Adı** filtresi adını veya, önem verdiğiniz kullanıcının kullanıcı asıl adı (UPN) belirtmenize imkan tanır.


**Risk durumu** filtre seçmenize imkan tanır:

- Risk altında
- Düzeltildi
- Kapatıldı


**Risk düzeyini** filtre seçmenize imkan tanır:

- Yüksek
- Orta
- Düşük


**Türü** filtre seçmenize imkan tanır:

- Konuk
- Üye

**Durumu** filtre seçmenize imkan tanır:

- Silinen
- Etkin


### <a name="download-risky-users-data"></a>Riskli kullanıcılar verileri indirme

Azure portal dışındaki iş onunla istiyorsanız riskli kullanıcılar verilerini indirebilir. Yükle'yi tıklatarak bir CSV dosyası en son 5 K kayıtlar oluşturur. 

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/07.png)


Araç çubuğunda sütunların tıklayarak liste görünümünü özelleştirebilirsiniz.
 
Bu sayede ek alanları görüntüleyebilir ya da zaten görüntülenen alanları kaldırabilirsiniz.
 
Genişletmek için ayrıntıları çekmece riskli kullanıcı hakkında daha fazla bilgi edinmek için tıklayın

 



## <a name="risky-sign-ins-report"></a>Riskli oturum açma işlemleri raporu

Riskli oturum açma işlemlerinin raporuyla sağlanan bilgiler sayesinde aşağıdakiler gibi soruların yanıtlarını bulabilirsiniz:

- Anonim IP adresi risk olayları kaç başarılı oturum açma işlemleri, var olan son bir hafta içinde vardı?

- Hangi kullanıcıların Onaylanmadı geçen ay tehlikeye?

- Hangi kullanıcıların Office 365 portalında riskli oturum açma işlemleri oldu?




Bu rapor için ilk giriş noktanız, **Araştır** Güvenlik sayfasında bölümü.

![Riskli oturum açma işlemleri raporu](./media/howto-investigate-risky-users-signins/02.png)

Riskli oturum açma işlemleri raporu aşağıdakileri gösteren bir varsayılan görünümü vardır:

- Tarih

- Kullanıcı

- Uygulama

- Oturum açma durumu

- Risk durumu

- Risk düzeyi (toplam)

- Risk düzeyi (gerçek zamanlı)

- Koşullu erişim

- MFA gerekli  
 

![Riskli oturum açma işlemleri raporu](./media/howto-investigate-risky-users-signins/09.png)


Araç çubuğunda **Sütunlar**’a tıklayarak liste görünümünü özelleştirebilirsiniz.

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/11.png)

Sütunları iletişim kutusu ek alanları görüntüleyebilir ya da zaten görüntülenen alanları kaldırabilirsiniz olanak sağlar.

Liste görünümündeki bir öğeye tıklayarak bu öğe hakkında mevcut olan tüm ayrıntıları yatay bir görünümde alabilirsiniz.

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/12.png)


Detaylarını gösterir:

- Temel bilgiler

- Cihaz bilgileri

- Risk bilgileri

- MFA bilgileri

- Koşullu erişim





Buna ek olarak, şunları yapabilirsiniz:

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/13.png)

- Gizliliğin ihlal edildiğini onayla 

- Güvenli olduğunu onayla

Daha fazla bilgi için [algılama doğruluğunu artırmak nasıl](howto-improve-detection-accuracy.md).




### <a name="filter-risky-sign-ins"></a>Filtre riskli oturum açma işlemleri

Raporlanan verileri kendinize uygun bir seviyeye gelecek şekilde daraltmak için aşağıdaki varsayılan alanları kullanarak riskli kullanıcı verilerini filtreleyebilirsiniz:

- Kullanıcı
- Uygulama
- Oturum açma durumu
- Risk durumu
- Risk düzeyi (toplam)
- Risk düzeyi (gerçek zamanlı)
- Koşullu erişim
- Tarih
- Risk düzeyi türü

![Riskli oturum açma işlemleri raporu](./media/howto-investigate-risky-users-signins/14.png)



**Adı** filtresi adını veya, önem verdiğiniz kullanıcının kullanıcı asıl adı (UPN) belirtmenize imkan tanır.

**Uygulama** filtre erişmeyi denedi kullanıcının bulut uygulaması belirtmenize imkan tanır.

**Oturum açma durumu** filtresi aşağıdakilerden birini seçmenize imkan tanır:

- Tümü
- Başarılı
- Hata


**Risk durumu** filtre seçmenize imkan tanır:

- Risk altında
- Riskli işlem onaylandı
- Güvenli işlem onaylandı
- Kapatıldı
- Düzeltildi


**Risk düzeyini (toplama)** filtre seçmenize imkan tanır:

- Yüksek
- Orta
- Düşük

**Risk düzeyini (gerçek zamanlı)** filtre seçmenize imkan tanır:

- Yüksek
- Orta
- Düşük


**Koşullu erişim** filtre seçmenize imkan tanır:

- Tümü
- Uygulanmadı
- Başarılı
- Hata


**Tarih** filtresi, döndürülen veriler için bir zaman çerçevesi tanımlamanıza olanak sağlar.
Olası değerler şunlardır:

- Son 1 ay
- Son 7 gün
- Son 24 saat
- Özel zaman aralığı





### <a name="download-risky-sign-ins-data"></a>Riskli oturum açma verilerini indir

Azure portal dışındaki iş onunla istiyorsanız riskli oturum açma verilerini indirebilir. Yükle'yi tıklatarak bir CSV dosyası en son 5 K kayıtlar oluşturur. 

![Riskli kullanıcılar raporu](./media/howto-investigate-risky-users-signins/15.png)



## <a name="next-steps"></a>Sonraki adımlar

Azure AD kimlik koruması genel bakış için bkz: [Azure AD kimlik koruması genel bakış](overview-v2.md).
