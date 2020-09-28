---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 09/25/2020
ms.author: alkohli
ms.openlocfilehash: 6dc201af2271909de15af9bac1a2e2bb68faed1a
ms.sourcegitcommit: 4313e0d13714559d67d51770b2b9b92e4b0cc629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2020
ms.locfileid: "91401105"
---
Aşağıdaki uyarılar, Azure 'a taşırken veriler için geçerlidir.

- Birden fazla cihazın aynı kapsayıcıya yazmamalıdır.
- Bulutta, Kopyalanmakta olan nesneyle aynı ada sahip mevcut bir Azure nesneniz (blob veya bir dosya gibi) varsa, cihaz buluttaki dosyanın üzerine yazar.
- Klasörleri paylaşma altında oluşturulan boş bir dizin hiyerarşisi (herhangi bir dosya olmadan) blob kapsayıcılarına yüklenemez.
- Dosya Gezgini ile veya komut satırı aracılığıyla sürükle ve bırak kullanarak verileri kopyalayabilirsiniz. Kopyalanan dosyaların toplam boyutu 10 GB 'den büyükse Robocopy veya rsync gibi bir toplu kopyalama programı kullanmanızı öneririz. Toplu kopyalama araçları, zaman aralıklı hatalar için kopyalama işlemini yeniden dener ve ek dayanıklılık sağlar.
- Azure depolama kapsayıcısı ile ilişkilendirilen paylaşımın, oluşturma sırasında, paylaşma için tanımlanan Blobların türüyle eşleşmeyen Blobları karşıya yüklediğinde, bu tür Bloblar güncellenmez. Örneğin, cihazda bir Blok Blobu paylaşma oluşturursunuz. Paylaşma, sayfa Blobları olan mevcut bir bulut kapsayıcısı ile ilişkilendirin. Dosyaları indirmek için bu paylaşımın yenilenmesini yapın. Bulutta sayfa Blobları olarak zaten depolanmış olan yenilenmiş dosyalardan bazılarını değiştirin. Karşıya yükleme başarısızlıklarını görürsünüz.
- Paylaşımlardaki bir dosya oluşturulduktan sonra dosyanın yeniden adlandırılması desteklenmez.
- Paylaşımdan dosya silindiğinde, depolama hesabındaki girdi silinmez.
- Verileri kopyalamak için rsync kullanılıyorsa, `rsync -a` seçeneği desteklenmez.

