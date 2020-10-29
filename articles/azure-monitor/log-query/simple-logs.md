---
title: Azure Izleyici 'de basit Günlükler deneyimi (Önizleme) | Microsoft Docs
description: Basit Günlükler deneyimi, Azure Izleyici 'de doğrudan KQL ile etkileşimde bulunmadan temel sorgular oluşturmanızı sağlar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/12/2019
ms.openlocfilehash: 4926e18aa6b00fe36608843ea5253903ace774e2
ms.sourcegitcommit: dd45ae4fc54f8267cda2ddf4a92ccd123464d411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2020
ms.locfileid: "92929311"
---
# <a name="simple-logs-experience-in-azure-monitor-preview"></a>Azure Izleyici 'de basit Günlükler deneyimi (Önizleme)
Azure Izleyici, KQL dilini kullanarak [günlük sorguları](log-query-overview.md) oluşturmaya yönelik [zengin bir deneyim](get-started-portal.md) sağlar. KQL 'nin tam gücünden birini zorunlu kılabilir ve temel sorgu gereksinimleri için basitleştirilmiş bir deneyim tercih edebilirsiniz. Basit Günlükler deneyimi, doğrudan KQL ile etkileşimde bulunmadan temel sorgular oluşturmanızı sağlar. Daha karmaşık sorgular gerektirdiğinde, KQL için bir öğrenme aracı olarak basit Günlükler de kullanabilirsiniz.

> [!NOTE]
> Basit Günlükler Şu anda yalnızca Cosmos DB ve Anahtar kasaları için bir test olarak uygulanır. Lütfen bu özelliği genişletip serbest bırakmamız için lütfen deneyiminizi Microsoft ile [Kullanıcı sesiyle](https://feedback.azure.com/forums/913690-azure-monitor) paylaşabilirsiniz.


## <a name="scope"></a>Kapsam
Basit Günlükler deneyimi, seçili kaynak için *AzureDiagnostics* , *AzureMetrics* ve *AzureActivity* tablosundan verileri alır. 

## <a name="using-simple-logs"></a>Basit günlükleri kullanma
[Log Analytics çalışma alanında günlükleri toplamak için yapılandırılmış tanılama ayarlarıyla](../platform/resource-logs.md#send-to-azure-storage)Azure aboneliğinizdeki herhangi bir Cosmos DB veya Key Vault gidin. **İzleme** menüsünde **Günlükler** ' e tıklayarak basit Günlükler deneyimini açın.

![Ekran görüntüsü Günlükler seçiliyken Izleme menüsünü gösterir.](media/simple-logs/menu.png)

Bir **alan** ve **operatör** seçin ve karşılaştırma için bir **değer** belirtin. **+** Ek ölçütler eklemek için tıklayın ve belirtin **ve/veya** belirtin.

![Ekran görüntüsü, günlükleri bölmesinde basit Günlükler seçiliyken aramayı gösterir.](media/simple-logs/criteria.png)

Sorgu sonuçlarını görüntülemek için **Çalıştır** ' a tıklayın.

## <a name="view-and-edit-kql"></a>KQL 'i görüntüleme ve düzenleme
Tam Log Analytics deneyiminde basit Günlükler sorgusu tarafından oluşturulan KQL 'yi açmak için **sorgu Düzenleyicisi** ' ni seçin. 

![Sorgu düzenleyicisi](media/simple-logs/query-editor.png)

KQL 'i doğrudan düzenleyebilir ve sonuçlarınızı daha da belirginleştirmek için filtreler gibi Log Analytics diğer özellikleri kullanabilirsiniz.

![KQL 'yi Düzenle](media/simple-logs/edit-kql.png)


## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal Log Analytics kullanma](get-started-portal.md)öğreticisini doldurun.
- [Günlük sorguları yazma](get-started-portal.md)hakkında öğreticiyi doldurun.
