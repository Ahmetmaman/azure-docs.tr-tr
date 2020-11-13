---
title: Azure Machine Learning tasarımcısını kullanarak modelleri yeniden eğitme
titleSuffix: Azure Machine Learning
description: Azure Machine Learning tasarımcısında yayımlanmış işlem hatları ile makine öğrenimi modellerini yeniden eğitme hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: keli19
author: likebupt
ms.date: 04/06/2020
ms.topic: conceptual
ms.custom: how-to, designer
ms.openlocfilehash: d8ef4d9f768d6fdcf976c9317d1abec3d4533824
ms.sourcegitcommit: dc342bef86e822358efe2d363958f6075bcfc22a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94554810"
---
# <a name="retrain-models-with-azure-machine-learning-designer"></a>Azure Machine Learning Designer ile modelleri yeniden eğitme


Bu nasıl yapılır makalesinde, bir makine öğrenimi modelini yeniden eğitmek için Azure Machine Learning tasarımcısını kullanmayı öğreneceksiniz. İş akışınızı otomatikleştirmek için yayımlanmış işlem hatlarını kullanacaksınız ve modelinizi yeni verilerde eğtirecek parametreleri ayarlayacaksınız. 

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Makine öğrenimi modelini eğitme.
> * İşlem hattı parametresi oluşturun.
> * Eğitim işlem hattınızı yayımlayın.
> * Modelinize yeni parametrelerle yeniden eğitme.

## <a name="prerequisites"></a>Ön koşullar

* Azure Machine Learning çalışma alanı
* Bu nasıl yapılır serisinin 1. kısmını, [tasarımcıda veri dönüştürme](how-to-designer-transform-data.md)

[!INCLUDE [machine-learning-missing-ui](../../includes/machine-learning-missing-ui.md)]

Bu makalede ayrıca tasarımcı 'da işlem hattı oluşturma hakkında bilgi sahibi olduğunuz varsayılır. Kılavuzlu bir giriş için [öğreticiyi](tutorial-designer-automobile-price-train-score.md)izleyin. 

### <a name="sample-pipeline"></a>Örnek işlem hattı

Bu makalede kullanılan işlem hattı, tasarımcı giriş hattının örnek bir ardışık düzen [geliri tahmini](samples-designer.md#classification) sürümü olarak değiştirilmiştir. İşlem hattı, kendi verilerinizi kullanarak modellerinizi nasıl eğitekullanacağınızı göstermek için örnek veri kümesi yerine [veri alma](algorithm-module-reference/import-data.md) modülünü kullanır.

![Değiştirilen örnek işlem hattını, verileri Içeri aktarma modülünü vurgulama kutusuyla gösteren ekran görüntüsü](./media/how-to-retrain-designer/modified-sample-pipeline.png)

## <a name="create-a-pipeline-parameter"></a>İşlem hattı parametresi oluşturma

Çalışma zamanında değişkenleri dinamik olarak ayarlamak için işlem hattı parametreleri oluşturun. Bu örnekte, modelinizi farklı verilerde yeniden eğitebilmeniz için sabit bir değerden bir parametreye eğitim verileri yolunu değiştirirsiniz.

1. **Veri Içeri aktarma** modülünü seçin.

    > [!NOTE]
    > Bu örnek, kayıtlı bir veri deposundaki verilere erişmek için veri alma modülünü kullanır. Ancak, alternatif veri erişim desenleri kullanıyorsanız benzer adımları izleyebilirsiniz.

1. Sağ taraftaki modül ayrıntısı bölmesinde, veri kaynağınızı seçin.

1. Verilerinizin yolunu girin. Ayrıca dosya ağacınızı taramak için, **yolu araştır** ' ı seçebilirsiniz. 

1. **Yol** alanı üzerinde Mouseve görüntülenen **yol** alanının üzerindeki üç noktayı seçin.

    ![İşlem hattı parametresinin nasıl oluşturulacağını gösteren ekran görüntüsü](media/how-to-retrain-designer/add-pipeline-parameter.png)

1. **Ardışık düzen parametresine Ekle** ' yi seçin.

1. Bir parametre adı ve varsayılan değer sağlayın.

   > [!NOTE]
   > İşlem hattı taslağınız başlığının yanındaki **Ayarlar** dişli simgesini seçerek işlem hattı parametrelerinizi inceleyebilir ve düzenleyebilirsiniz. 

1. **Kaydet** ’i seçin.

1. İşlem hattı çalıştırmasını gönder.

## <a name="find-a-trained-model"></a>Eğitilen bir model bulun

Tasarımcı, eğitilen modeller dahil olmak üzere tüm işlem hattı çıkışlarını varsayılan çalışma alanı depolama hesabına kaydeder. Ayrıca, eğitilen modellere doğrudan tasarımcıda erişebilirsiniz:

1. İşlem hattının çalışmayı bitirmesini bekleyin.
1. **Model eğitme** modülünü seçin.
1. Modülün sağ tarafındaki modül ayrıntıları bölmesinde **çıktılar + Günlükler** ' i seçin.
1. Modelinizi **diğer çıkışlara** , çalışma günlükleri ile birlikte bulabilirsiniz.
1. Alternatif olarak, **çıktıyı görüntüle** simgesini seçin. Buradan, doğrudan veri deposuna gitmek için iletişim kutusundaki yönergeyi izleyebilirsiniz. 

> [!div class="mx-imgBorder"]
> ![Eğitilen modelin nasıl indirileceği gösteren ekran görüntüsü](./media/how-to-retrain-designer/trained-model-view-output.png)

## <a name="publish-a-training-pipeline"></a>Eğitim işlem hattı yayımlama

İşlem hatlarınızı ileride kolayca yeniden kullanmak için bir ardışık düzen uç noktasına bir işlem hattı yayımlayın. Ardışık düzen uç noktası, gelecekteki işlem hattını çağırmak için bir REST uç noktası oluşturur. Bu örnekte, işlem hattı uç noktanız, farklı verilerde bir modeli yeniden eğitebilmeniz için işlem hattınızı yeniden kullanmanıza olanak tanır.

1. Tasarımcı tuvalinin üzerinde **Yayımla** ' yı seçin.
1. Ardışık düzen uç noktası seçin veya oluşturun.

   > [!NOTE]
   > Tek bir uç noktada birden çok işlem hattı yayımlayabilirsiniz. Belirli bir uç noktadaki her işlem hattına, ardışık düzen uç noktasını çağırdığınızda belirtebileceğiniz bir sürüm numarası verilir.

1. **Yayımla** ’yı seçin.

## <a name="retrain-your-model"></a>Modelinizi yeniden eğitme

Artık yayımlanmış bir eğitim işlem hattına sahip olduğunuza göre, bunu kullanarak modelinize yeni verileri yeniden eğitebilirsiniz. Çalışan bir ardışık düzen uç noktasından veya programlama yoluyla çalışma gönderebilirsiniz.

### <a name="submit-runs-by-using-the-studio-portal"></a>Çalışmaları, Studio portalını kullanarak gönderme

Studio portalından parametreli bir işlem hattı çalıştırması göndermek için aşağıdaki adımları kullanın:

1. Studio çalışma alanınızdaki **uç noktalar** sayfasına gidin.
1. **Ardışık düzen uç noktaları** sekmesini seçin. Ardından, işlem hattı uç noktanızı seçin.
1. Yayınlanan işlem **hatları** sekmesini seçin. Sonra, çalıştırmak istediğiniz işlem hattı sürümünü seçin.
1. **Gönder** ’i seçin.
1. Kurulum iletişim kutusunda, çalıştırma için parametre değerlerini belirtebilirsiniz. Bu örnekte, modelinizi ABD olmayan bir veri kümesi kullanarak eğitebilmeniz için veri yolunu güncelleştirin.

![Tasarımcıda parametreli bir işlem hattı çalıştırmasının nasıl ayarlanacağını gösteren ekran görüntüsü](./media/how-to-retrain-designer/published-pipeline-run.png)

### <a name="submit-runs-by-using-code"></a>Kodu kullanarak çalıştırmaları gönder

Yayımlanan bir işlem hattının REST uç noktasını genel bakış panelinde bulabilirsiniz. Uç noktayı çağırarak, yayımlanan işlem hattını yeniden eğitebilirsiniz.

REST çağrısı yapmak için bir OAuth 2,0 taşıyıcı türü kimlik doğrulama üst bilgisi gerekir. Çalışma alanınıza kimlik doğrulama ayarlama ve parametreli REST çağrısı yapma hakkında bilgi için bkz. [Batch Puanlama için Azure Machine Learning işlem hattı oluşturma](tutorial-pipeline-batch-scoring-classification.md#publish-and-run-from-a-rest-endpoint).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Tasarımcıyı kullanarak parametreli bir eğitim işlem hattı uç noktası oluşturmayı öğrendiniz.

Tahmine dayalı hale getirmek üzere bir modeli nasıl dağıtacağınızı gösteren kapsamlı bir kılavuz için bkz. bir regresyon modeli eğitme ve dağıtmaya yönelik [Tasarımcı öğreticisi](tutorial-designer-automobile-price-train-score.md) .
