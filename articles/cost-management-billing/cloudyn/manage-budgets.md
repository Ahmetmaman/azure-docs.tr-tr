---
title: Azure 'da Cloudyn bütçelerini yönetme
description: Bu makalede, hızlı bir şekilde bütçeleri oluşturun ve bunları Cloudyn'de yönetmeye başlama yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/20/2019
ms.topic: conceptual
ms.service: cost-management-billing
manager: vitavor
ms.custom: seodec18
ms.openlocfilehash: 3572296da8e81602e905a3d9f515e7d49067cfa4
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75990599"
---
# <a name="manage-azure-budgets-with-cloudyn"></a>Cloudyn ile Azure bütçelerini yönetin

Ayar bütçelerini'kurmak ve bütçe tabanlı bulut idare ve Sorumluluk geliştirmek için Yardım uyarır. Bu makalede, hızlı bir şekilde bütçeleri oluşturun ve bunları Cloudyn'de yönetmeye başlama yardımcı olur.

Bir kuruluş ya da MSP hesabınız varsa, farklı iş birimleri, Departmanlar veya herhangi bir maliyet varlık aylık bütçe kotaları atamak için hiyerarşik maliyet varlık yapınızı kullanabilirsiniz. Premium hesabı varsa, daha sonra tüm bulut harcamalarını için uygulanan bütçe yönetimi işlevi kullanabilirsiniz. Tüm bütçelerini el ile atanır.

Atanan bütçeleri bağlı olarak, eşik uyarılarının tüketilen bütçenizi yüzdesine göre ayarlayabilir ve her eşiği önem derecesini tanımlayın.

Bütçe raporlar atanan bütçe gösterir. Kullanıcılar, kendi harcama üzerinden, altında veya par tüketimi için zamana sahip olduğunda görüntüleyebilir. Seçtiğinizde, **alanları Göster/Gizle** en üstünde bir bütçe raporun maliyet, bütçe, birikmiş maliyeti veya toplam bütçe görüntüleyebilirsiniz.

Azure Maliyet Yönetimi, Cloudyn'e benzer işlevler sunar. Azure Maliyet Yönetimi, yerel Azure maliyet yönetimi çözümüdür. Maliyet analizi yapmanıza, bütçe oluşturup yönetmenize, verileri dışarı aktarmanıza ve tasarruf önerilerini gözden geçirip gerekli eylemleri gerçekleştirmenize yardımcı olur. Maliyet Yönetimi'nde bütçe hakkında daha fazla bilgi için bkz. [oluştur ve bütçelerini yönetmek](../costs/tutorial-acm-create-budgets.md).

## <a name="create-budgets"></a>Bütçeleri oluşturun

Bütçe oluşturduğunuzda, mali yıl için ayarlayın ve belirli bir varlığa uygulanır.

Bütçe oluşturun ve bunu bir varlığa atamak için:

1. Gidin **maliyetleri** &gt; **maliyet Yönetimi** &gt; **bütçe**.
2. Bütçe Yönetimi sayfasında altında **varlıkları**, bütçe oluşturmak istediğiniz varlığı seçin.
3. Bütçe yılda bütçe oluşturmak istediğiniz yıl seçin.
4. Her ay için bir bütçe değere ayarlayın. İşiniz bittiğinde tıklayın **Kaydet**.
Bu örnekte, Haziran 2018 tarihinden itibaren aylık bütçe 135.000 için ayarlanır. Yıl için toplam bütçe $1,615,000.00 olacaktır.
![Her ay için bir bütçe ayarlandığı bir bütçe sayfası oluşturma](./media/manage-budgets/set-budget.png)


Yıllık bütçenin bir dosyayı içe aktarmak için:

1. Altında **eylemleri**seçin **dışarı** bütçe, temel olarak kullanmak için boş bir CSV şablonu indirilemedi.
2. Bütçe girişlerinizi içeren CSV dosyası doldurun ve yerel olarak kaydedin.
3. Altında **eylemleri**seçin **alma**.
4. Kaydettiğiniz dosyayı seçin ve ardından **Tamam**.

Tamamlanan bütçenizi altında bir CSV dosyası olarak dışarı aktarmak için **eylemleri**seçin **dışarı** dosyasını indirmek için.

## <a name="view-budget-in-reports"></a>Rapor görünümü bütçede

Bu tamamlandığında, bütçeniz **maliyetler** &gt; **Maliyet Analizi** kapsamında ve zaman içinde maliyet ile bütçeyi raporlamak için maliyet raporlarının çoğunda yer aldığı maliyet raporlarında gösterilir. Raporları kullanarak bütçe eşiklere dayanarak de zamanlayabilirsiniz **eylemleri**.

Maliyet analizi raporu örneği aşağıda verilmiştir. Bu yılın başlangıcından bu yana toplam bütçe ve iş yükü ve kullanım türleri maliyeti gösterir.

![Bütçe ile örnek maliyet analizi raporu](./media/manage-budgets/cost-analysis-budget-example.png)

Bu örnekte, geçerli tarihi 22 Temmuz olduğunu varsayalım. Haziran 2018 için $ $135.000 aylık bütçeye kıyasla 71,611.28 maliyetidir. Sekiz gün ve ay sonuna kadar önce harcadığınız hala olduğundan maliyet aylık bütçe daha çok düşüktür.

Raporu görüntülemek için başka bir yolu, bütçenizi birikmiş maliyeti veya aramaktır. Altında birikmiş maliyetleri görmek için **alanları Göster/Gizle**seçin **birikmiş maliyetini** ve **toplam bütçe**. Yılın başlangıcından bu yana birikmiş maliyetini gösteren bir örnek aşağıda verilmiştir.

![Örnek, birikmiş maliyet ve zaman Içinde Bütçe karşılaştırması raporunda gösterilen toplam bütçe](./media/manage-budgets/accumulated-budget.png)

Süre gelecekte birikmiş maliyetini bütçenizi aştığında. Grafik görünümü değiştirirseniz daha kolay görebilirsiniz _satırı_ türü.

![Ay bazında maliyeti çizgi grafikte gösterilen bütçe](./media/manage-budgets/budget-line.png)

## <a name="create-budget-alerts-for-a-filter"></a>Bütçe uyarılar için bir filtre oluşturun

Önceki örnekte birikmiş maliyeti bütçesi yaklaşıldığında görebilirsiniz. Böylece yaklaşım harcama olduğunda bildirim alırsınız veya bütçenizi aştığında otomatik bütçe uyarılar oluşturabilirsiniz. Aslında, uyarı, zamanlanmış bir raporu bir eşik ile belirtir. Bütçe uyarısı eşiği ölçümler şunları içerir:

- Kalan Maliyet ve bütçe, bir para birimi değeri eşiği belirtmek için – karşılaştırması
- -Bir yüzde değeri eşiği belirtmek için bütçe – maliyet yüzdesi

Bir örneğe göz atalım.

Maliyet ve zaman Içinde bütçe raporunda, **Eylemler** ' i ve ardından **rapor zamanla**' yı seçin. Eşik sekmesinde, bir eşiği ölçümünü seçin. Örneğin, **maliyet yüzdesi vs bütçe**. Bir uyarı türünü seçin ve bütçe yüzdesi değeri girin. Yalnızca bir kez bildirim almak isteyip istemediğinizi seçin **ardışık olarak verilecek uyarı sayısını** yazın _1_. **Save (Kaydet)** düğmesine tıklayın.

![Bütçe uyarısı kaydetme ya da bu raporu kutusunda zamanlama oluşturma](./media/manage-budgets/budget-alert.png)

## <a name="next-steps"></a>Sonraki adımlar

- Cloudyn ilk öğreticide zaten tamamlamadıysanız, hem okuma [kullanımı ve maliyetleri gözden geçirme](tutorial-review-usage.md).
- Daha fazla bilgi edinin [Cloudyn içinde kullanılabilir raporlar](use-reports.md).
