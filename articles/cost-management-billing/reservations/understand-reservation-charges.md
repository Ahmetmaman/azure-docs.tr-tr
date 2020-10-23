---
title: Azure SQL Veritabanı için rezervasyon indirimini anlama | Microsoft Docs
description: Çalışmakta olan Azure SQL veritabanlarına bir rezervasyon indiriminin nasıl uygulanacağını öğrenin. İndirim, bu veritabanlarına saat temelinde uygulanır.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 10/13/2020
ms.author: banders
ms.openlocfilehash: 054641d8136d121e611182c8d8b104aefcbc6481
ms.sourcegitcommit: 1b47921ae4298e7992c856b82cb8263470e9e6f9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92057884"
---
# <a name="how-a-reservation-discount-is-applied-to-azure-sql-database"></a>Azure SQL Veritabanı’na rezervasyon indiriminin uygulanması

Bir Azure SQL Veritabanı ayrılmış kapasitesi satın almanızın ardından, rezervasyonun öznitelikleriyle ve miktarıyla eşleşen SQL veritabanlarına otomatik olarak rezervasyon indirimi uygulanır. Birincil çoğaltma ve faturalandırılabilir tüm ikinci çoğaltmalar dahil olmak üzere SQL Veritabanınızın işlem maliyetlerine rezervasyon uygulanır. Yazılım, depolama ve ağ iletişimi için normal fiyatlarla ücretlendirilirsiniz. [Azure Hibrit Avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/) ile SQL Veritabanı için lisanslama maliyetlerini kapsayabilirsiniz.

Azure SQL Veritabanı sunucusuza rezervasyon indirimleri uygulanmadığını unutmayın.

Ayrılmış Sanal Makine Örnekleri için bkz. [Azure Ayrılmış Sanal Makine Örnekleri indirimini anlama](../manage/understand-vm-reservation-charges.md).

## <a name="how-reservation-discount-is-applied"></a>Rezervasyon indiriminin uygulanması

Rezervasyon indirimi “*kullanılmadığı takdirde hakkı kaybedilen*” bir özelliktir. Bu nedenle, herhangi bir saat için eşleşen kaynaklarınız yoksa, o saate ait rezervasyon miktarını kaybedersiniz. Kullanılmayan ayrılmış saatleri devredemezsiniz.

Bir kaynağı kapattığınızda rezervasyon indirimi, belirtilen kapsamdaki başka bir eşleşen kaynağa otomatik olarak uygulanır. Belirtilen kapsamda eşleşen kaynak bulunamazsa, ayrılan saatler *kaybedilir*.

## <a name="discount-applied-to-running-sql-databases"></a>Çalışan SQL veritabanlarına uygulanan indirim

SQL Veritabanı ayrılmış kapasite indirimi, çalışmakta olan SQL veritabanlarına saat bazında uygulanır. Satın aldığınız rezervasyon, çalışmakta olan SQL veritabanları tarafından gösterilen işlem kullanımı ile eşleştirilir. Saatin tamamı boyunca çalışmayan SQL veritabanları olursa rezervasyon, rezervasyon öznitelikleriyle eşleşen diğer SQL veritabanlarına otomatik olarak uygulanır. Eş zamanlı olarak çalışan SQL veritabanları için de indirim geçerli olabilir. Rezervasyon öznitelikleriyle eşleşen ve saatin tamamı boyunca çalışan bir SQL veritabanınız yoksa, ilgili saat için rezervasyon indiriminden tam olarak yararlanmazsınız.

Aşağıdaki örneklerde, satın aldığınız çekirdek sayısına ve çalıştırılma zamanına bağlı olarak SQL Veritabanı ayrılmış kapasite indiriminin nasıl uygulanacağı gösterilmektedir.

- Senaryo 1: 8 çekirdekli bir SQL Veritabanı için SQL Veritabanı ayrılmış kapasitesi satın aldınız. Rezervasyon özniteliklerinin geri kalanıyla eşleşen 16 çekirdekli bir SQL Veritabanı çalıştırıyorsunuz. 8 çekirdekli SQL Veritabanı işlem kullanımı için kullandıkça öde fiyatıyla ücretlendirilirsiniz. Bir saatlik 8 çekirdekli SQL Veritabanı işlem kullanımı için rezervasyon indirimi elde edersiniz.

Bu örneklerin geri kalanında, satın aldığınız SQL Veritabanı ayrılmış kapasitesinin 16 çekirdekli SQL Veritabanı için olduğunu ve rezervasyon özniteliklerinin geri kalanının, çalışmakta olan SQL veritabanları ile eşleştiğini varsayın.

- Senaryo 2: Her bir saat için 8’er çekirdekle iki SQL veritabanı çalıştırıyorsunuz. 16 çekirdekli rezervasyon indirimi her iki 8 çekirdekli SQL veritabanı için işlem kullanımına uygulanır.
- Senaryo 3: 13:00’dan 13:30’a kadar tek bir 16 çekirdekli SQL Veritabanı çalıştırıyorsunuz. 13:30’dan 14:00’a kadar başka bir 16 çekirdekli SQL Veritabanı çalıştırıyorsunuz. Her ikisi de rezervasyon indirimi kapsamındadır.
- Senaryo 4: 13:00’dan 13:45’e kadar tek bir 16 çekirdekli SQL Veritabanı çalıştırıyorsunuz. 13:30’dan 14:00’a kadar başka bir 16 çekirdekli SQL Veritabanı çalıştırıyorsunuz. 15 dakikalık çakışma için kullandıkça öde fiyatıyla ücretlendirilirsiniz. Rezervasyon indirimi, geri kalan süre boyunca işlem kullanımına uygulanır.
- 5\. Senaryo: Her birinde 4 çekirdek bulunan üç adet ikincil çoğaltma içeren bir adet 4 çekirdekli SQL Hiper Ölçek veritabanı çalıştırıyorsunuz. Rezervasyon, birincil ve tüm ikincil çoğaltmalardaki işlem kullanımına uygulanır.

Faturalama kullanım raporlarında Azure rezervasyonlarınızın uygulamasını anlamak ve görüntülemek için bkz. [Azure rezervasyon kullanımınızı anlama](understand-reserved-instance-usage-ea.md).

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Ayrılmış Sanal Makine Örnekleri nedir?](save-compute-costs-reservations.md)
- [Azure Ayrılmış VM Örnekleri ile Sanal Makinelere ön ödeme yapma](../../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure SQL Veritabanı ayrılmış kapasitesi ile SQL Veritabanı işlem kaynakları için ön ödeme yapma](../../azure-sql/database/reserved-capacity-overview.md)
- [Azure Ayırmalarını yönetme](manage-reserved-vm-instance.md)
- [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlama](understand-reserved-instance-usage.md)
- [Kurumsal kaydınız için rezervasyon kullanımını anlama](understand-reserved-instance-usage-ea.md)
- [CSP abonelikleri için rezervasyon kullanımını anlama](/partner-center/azure-reservations)
