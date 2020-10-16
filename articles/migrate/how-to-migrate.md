---
title: Azure geçişi 'ne geçiş araçları ekleme
description: Azure geçişi 'ne geçiş araçları ekleme hakkında bilgi edinin.
ms.topic: article
ms.date: 04/26/2020
ms.openlocfilehash: b6ac8ed64d3f12783cc16f0428874a19d027adf9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "86109816"
---
# <a name="add-migration-tools"></a>Geçiş araçları ekleme

Bu makalede, [Azure](./migrate-services-overview.md)geçişi 'nde geçiş araçlarının nasıl ekleneceği açıklanmaktadır.

- Bir geçiş aracı eklemek ve henüz bir Azure geçişi projesi ayarlamadıysanız, bu [makaleye](how-to-add-tool-first-time.md)uyun.
- Geçiş için bir ISV aracı eklediyseniz, araçla çalışmaya hazırlanmak için [adımları izleyin](prepare-isv-movere.md).

## <a name="select-a-migration-scenario"></a>Bir geçiş senaryosu seçin

1. Azure geçişi projesinde **Genel Bakış ' a**tıklayın.
2. Kullanmak istediğiniz geçiş senaryosunu seçin:

    - Makineleri ve iş yüklerini Azure 'a geçirmek için, **sunucuları değerlendir ve geçir**' i seçin.
    - Şirket içi SQL Server veritabanlarını geçirmek için **veritabanlarını değerlendir ve geçir**' i seçin.
    - Şirket içi Web uygulamalarını geçirmek için **Web uygulamalarını değerlendir ve geçir**' i seçin.
    - Çevrimiçi modda büyük miktarlarda şirket içi verileri Azure 'a geçirmek için **Data Box Sırala**' yı seçin.

    ![Değerlendirme senaryosu](./media/how-to-migrate/assess-scenario.png)

## <a name="select-a-server-migration-tool"></a>Sunucu geçiş aracı seçin

1. **Sunucuları değerlendir ve geçir**' e tıklayın.
2. **Azure geçişi-sunucularında**, henüz geçiş araçları eklemediyseniz geçiş **araçları**' nın altında, geçiş **Aracı eklemek için buraya tıklayın ' ı**seçin. Daha önce geçiş araçları eklediyseniz, **daha fazla geçiş Ekle**' de **Değiştir**' i seçin.

    > [!NOTE]
    > Farklı bir projeye gitmeniz gerekiyorsa, **Azure geçişi-sunucular**' da, **farklı bir geçiş projesinin ayrıntılarını gör**' ün yanındaki **buraya tıklayın**' e tıklayın.

3. **Azure geçişi**' nde, kullanmak istediğiniz geçiş aracını seçin.
    - Azure geçişi sunucu geçişini kullanıyorsanız, doğrudan Azure geçişi projesindeki geçişleri ayarlayabilir ve çalıştırabilirsiniz.
    - Üçüncü taraf bir değerlendirme aracı kullanıyorsanız, ISV için sunulan bağlantıya gidin ve bu geçişi sağladığı yönergelere uygun olarak çalıştırın.

## <a name="select-a-database-migration-tool"></a>Veritabanı geçiş aracı seçin

1. **Veritabanlarını değerlendir ve geçir** ' e tıklayın
2. **Veritabanları**' nda **araç ekle**' ye tıklayın.
3. Araç Ekle > **geçiş aracını seçin**bölümünde, veritabanınızı geçirmek için kullanmak istediğiniz aracı seçin.

## <a name="select-a-web-app-migration-tool"></a>Web uygulaması geçiş aracı seçin

1. **Web uygulamalarını değerlendir ve geçir**' e tıklayın.
2. Azure App Service geçiş aracının bağlantısını izleyin. Geçiş aracını kullanarak şunları yapın:

    - **Uygulamaları çevrimiçi değerlendirin**: Azure App Service Geçiş Yardımcısı kullanarak, uygulamaları çevrimiçi olarak ortak bir URL ile değerlendirebilir ve geçirebilirsiniz.
    - **.Net/php**: iç .net ve PHP uygulamaları için Geçiş Yardımcısı indirebilir ve çalıştırabilirsiniz.

## <a name="order-an-azure-data-box"></a>Azure Data Box sıralama

Büyük miktarlardaki verileri Azure 'a geçirmek için çevrimdışı veri aktarımı için bir Azure DAta Box siparişi verebilirsiniz.

1. **Data Box Sırala**' ya tıklayın.
2. **Azure Data Box seçin**bölümünde aboneliğinizi belirtin. 
3. Aktarım, Azure 'a bir içeri aktarma olacaktır. Veri kaynağını ve verilerin Azure bölgesi hedefini belirtin.

## <a name="next-steps"></a>Sonraki adımlar

[Hyper-V](tutorial-migrate-hyper-v.md) veya [VMware](tutorial-migrate-vmware.md) VM 'Leri Için Azure geçişi sunucu geçişini kullanarak geçiş yapmayı deneyin.
