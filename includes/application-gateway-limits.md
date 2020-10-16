---
author: vhorne
ms.service: application-gateway
ms.topic: include
ms.date: 03/04/2020
ms.author: victorh
ms.openlocfilehash: 92ec7c0a1469c9f02855cd6191faa8514e54c8f0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91829088"
---
| Kaynak | Sınır | Not |
| --- | --- | --- |
| Azure Application Gateway |Abonelik başına 1.000 | |
| Ön uç IP yapılandırması |2 |1 ortak ve 1 özel |
| Ön uç bağlantı noktaları |100<sup>1</sup> | |
| Arka uç adres havuzları |100<sup>1</sup> | |
| Havuz başına arka uç sunucuları |1.200 | |
| HTTP dinleyicileri |200<sup>1</sup> |Yönlendirme trafiği olan 100 etkin dinleyiciyle sınırlıdır. Etkin dinleyiciler = toplam dinleyici sayısı-dinleyici etkin değil.<br>Yönlendirme kuralı içindeki bir varsayılan yapılandırma, trafiği yönlendirmek üzere ayarlandıysa (örneğin, bir dinleyici, arka uç havuzu ve HTTP ayarları), bu da dinleyici olarak sayılır.|
| HTTP Yük Dengeleme kuralları |100<sup>1</sup> | |
| Arka uç HTTP ayarları |100<sup>1</sup> | |
| Ağ geçidi başına örnek |V1 SKU 'SU-32<br>V2 SKU-125 | |
| SSL sertifikaları |100<sup>1</sup> |HTTP dinleyicisi başına 1 |
| En fazla SSL sertifika boyutu |V1 SKU 'SU-10 KB<br>V2 SKU-16 KB| |
| Kimlik doğrulama sertifikaları |100 | |
| Güvenilen kök sertifikalar |100 | |
| İstek zaman aşımı en az |1 saniye | |
| İstek zaman aşımı üst sınırı |24 saat | |
| Site sayısı |100<sup>1</sup> |HTTP dinleyicisi başına 1 |
| Dinleyici başına URL haritaları |1 | |
| URL eşlemesi başına en fazla yol tabanlı kural|100||
| Yeniden yönlendirme yapılandırması |100<sup>1</sup>| |
| Eşzamanlı WebSocket bağlantıları |Orta ağ geçitleri 20.000<br> Büyük ağ geçitleri 50.000| |
| En fazla URL uzunluğu|AŞAMAZ| |
| HTTP/2 için üst bilgi boyutu üst sınırı |4 KB 'DIR| |
| Dosya karşıya yükleme boyutu üst sınırı, standart |2 GB | |
| En fazla dosya yükleme boyutu WAF |V1 orta WAF ağ geçitleri, 100 MB<br>V1 büyük WAF ağ geçitleri, 500 MB<br>V2 WAF, 750 MB| |
| WAF gövde boyutu sınırı, dosyalar olmadan|128 KB||
| En fazla WAF özel kuralları|100||
| Maksimum WAF dışlamaları|100||

<sup>1</sup> WAF özellikli SKU 'lar söz konusu olduğunda, kaynak sayısını 40 ile sınırlamanız gerekir.
