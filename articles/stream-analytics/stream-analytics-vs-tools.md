---
title: Visual Studio 'da Azure Stream Analytics işleri görüntüleme
description: Visual Studio kullanarak bağlantıları görüntüleme, başlatma ve durdurma, test etme, sonuçları denetleme ve Azure Stream Analytics işlerinizi dışarı aktarma hakkında bilgi edinin.
author: su-jie
ms.author: sujie
ms.service: stream-analytics
ms.topic: how-to
ms.date: 07/10/2018
ms.openlocfilehash: 4b568fa8d9e2fb8bd00acb27b0dc3f7c45c09c57
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98019134"
---
# <a name="use-visual-studio-to-view-azure-stream-analytics-jobs"></a>Azure Stream Analytics işleri görüntülemek için Visual Studio 'Yu kullanma

Visual Studio için Azure Stream Analytics araçları, geliştiricilerin Stream Analytics işlerini doğrudan IDE 'den yönetmesini kolaylaştırır. Azure Stream Analytics araçlarıyla şunları yapabilirsiniz:
- [Yeni işler oluştur](stream-analytics-quick-create-vs.md)
- İşleri başlatma, durdurma ve [izleme](stream-analytics-monitor-jobs-use-vs.md)
- İş sonuçlarını denetle
- Mevcut işleri bir projeye dışarı aktarma
- Giriş ve çıkış bağlantılarını sınama
- [Sorguları yerel olarak çalıştır](stream-analytics-vs-tools-local-run.md)

[Visual Studio için Azure Stream Analytics araçları](stream-analytics-tools-for-visual-studio-install.md)'nı nasıl yükleyeceğinizi öğrenin.

## <a name="explore-the-job-view"></a>İş görünümünü keşfet

Visual Studio 'dan Azure Stream Analytics işlerle etkileşim kurmak için iş görünümünü kullanabilirsiniz.

### <a name="open-the-job-view"></a>İş görünümünü açın

1. **Sunucu Gezgini** **Stream Analytics işler** ' i seçin ve ardından **Yenile**' yi seçin. İşiniz **Stream Analytics işleri** altında görünmelidir.

    ![Sunucu Gezgini listesini Stream Analytics](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-list-jobs-01.png)

2. İş düğümünü genişletin **ve iş görünümü düğümüne çift** tıklayarak iş görünümünü açın.
    
   ![Genişletilen iş düğümü](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-job-view-01.png)

### <a name="start-and-stop-jobs"></a>İşleri başlatma ve durdurma

Azure Stream Analytics işleri Visual Studio 'daki iş görünümünden tamamen yönetilebilir. Bir işi başlatmak, durdurmak veya silmek için denetimleri kullanın.
    
   ![Stream Analytics iş denetimleri](./media/stream-analytics-vs-tools/azure-stream-analytics-job-view-controls.png)

## <a name="check-job-results"></a>İş sonuçlarını denetle

Visual Studio için Stream Analytics araçları şu anda Azure Data Lake Storage ve BLOB depolama için çıkış önizlemeyi desteklemektedir. Sonucu görüntülemek için, **Iş görünümü** ' nde iş diyagramının çıkış düğümüne çift tıkladıktan sonra uygun kimlik bilgilerini girmeniz yeterlidir.

   ![Stream Analytics iş blobu çıkışı](./media/stream-analytics-vs-tools/stream-analytics-blob-preview.png)

## <a name="export-jobs-to-a-project"></a>İşleri projeye dışarı aktarma

Var olan bir işi projeye dışarı aktarmanın iki yolu vardır.

1. **Sunucu Gezgini**, Stream Analytics işleri düğümünün altında, iş düğümüne sağ tıklayın. **Yeni Stream Analytics projeye aktar**' ı seçin.
    
   ![İşi projeye aktar](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-export-job-01.png)
    
    Oluşturulan proje **Çözüm Gezgini** görüntülenir.
    
   ![Çözüm gezgini](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-export-job-02.png)

2. İş görünümünde **proje oluştur**' u seçin.
    
   ![İş görünümünden proje oluştur](./media/stream-analytics-vs-tools/stream-analytics-tools-for-vs-export-job-03.png)

## <a name="test-connections"></a>Sınama bağlantıları

Giriş ve çıkış bağlantıları, **test bağlantısı** açılan menüsünden bir seçenek belirlenerek **iş görünümünden** test edilebilir.

   ![Bağlantıyı test et açılan menüsü](./media/stream-analytics-vs-tools/stream-analytics-test-connection-dropdown.png)

**Test bağlantı** sonuçları **Çıkış** penceresinde görüntülenir.

   ![Test bağlantı sonuçları](./media/stream-analytics-vs-tools/stream-analytics-test-connection-results.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio 'Yu kullanarak Azure Stream Analytics işlerini izleme ve yönetme](stream-analytics-monitor-jobs-use-vs.md)
* [Hızlı başlangıç: Visual Studio 'Yu kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-vs.md)
* [Stream Analytics araçlarıyla sürekli tümleştirme ve geliştirme](stream-analytics-tools-for-visual-studio-cicd.md)
