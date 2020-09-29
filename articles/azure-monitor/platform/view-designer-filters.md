---
title: Azure Izleyici görünümlerindeki filtreler | Microsoft Docs
description: Azure Izleyici görünümündeki bir filtre, kullanıcıların görünümün kendisini değiştirmeden belirli bir özelliğin değerine göre görünümdeki verileri filtrelemesine olanak tanır.  Bu makalede bir filtrenin nasıl kullanılacağı ve bir özel görünüme nasıl ekleneceği açıklanmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/22/2018
ms.openlocfilehash: d428382493e15d2e0571f4cb4b6f090cf9056fe4
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91449315"
---
# <a name="filters-in-azure-monitor-views"></a>Azure Izleyici görünümlerindeki filtreler
[Azure izleyici görünümündeki](view-designer.md) bir **filtre** , kullanıcıların görünümün kendisini değiştirmeden belirli bir özelliğin değerine göre görünümdeki verileri filtrelemesine olanak tanır.  Örneğin, görünüminizdeki kullanıcıların görünümü yalnızca belirli bir bilgisayar veya bilgisayar kümesi tarafından filtrelemesine izin verebilirsiniz.  Kullanıcıların birden çok özelliğe göre filtrelemesine olanak tanımak için, tek bir görünümde birden çok filtre oluşturabilirsiniz.  Bu makalede bir filtrenin nasıl kullanılacağı ve bir özel görünüme nasıl ekleneceği açıklanmaktadır.

## <a name="using-a-filter"></a>Filtre kullanma
Görünümü açmak için bir görünümün en üstündeki tarih saat aralığına tıklayın ve sonra görünümün tarih saat aralığını değiştirebileceğiniz açılan açılan listesini açın.

![Azure Izleyici 'de bir görünüm için zaman aralığı açılan menüsünün, son 7 gün için radyo düğmesini gösteren ekran görüntüsü.](media/view-designer-filters/filters-example-time.png)

**+** Görünüm için tanımlanan özel filtreleri kullanarak filtre eklemek için öğesine tıklayın. Açılan listeden filtre için bir değer seçin ya da bir değer yazın. Öğesine tıklayarak filtre eklemeye devam edin **+** . 


![Azure Izleyici 'de özel filtre ekleme iletişim kutusunun ekran görüntüsü. Bilgisayarlar özelliği, özellik seç açılan menüsünde seçilir.](media/view-designer-filters/filters-example-custom.png)

Bir filtrenin tüm değerlerini kaldırırsanız, bu filtre artık uygulanmaz.


## <a name="creating-a-filter"></a>Filtre oluşturma

[Bir görünüm düzenlenirken](view-designer.md) **Filtreler** sekmesinden bir filtre oluşturun.  Filtre görünüm için geneldir ve görünümdeki tüm parçalar için geçerlidir.  

![Filtre ayarları](media/view-designer-filters/filters-settings.png)

Aşağıdaki tabloda bir filtrenin ayarları açıklanmaktadır.

| Ayar | Açıklama |
|:---|:---|
| Alan Adı | Filtreleme için kullanılan alanın adı.  Bu alan, **değerler Için sorgudaki**özetleme alanıyla eşleşmelidir. |
| Değerler için sorgu | Kullanıcının filtre açılan listesini doldurmak için çalıştırılacak sorgu.  Bu sorgu, belirli bir alan için benzersiz değerler sağlamak üzere [özetleme](/azure/kusto/query/summarizeoperator) veya [ayrı](/azure/kusto/query/distinctoperator) bir değer kullanmalıdır ve **alan adıyla**eşleşmesi gerekir.  [Sıralamayı](/azure/kusto/query/sortoperator) kullanarak kullanıcıya görüntülenen değerleri sıralayabilirsiniz. |
| Etiket | Filtreyi destekleyen sorgularda kullanılan alanın adı ve ayrıca kullanıcıya görüntülenir. |

### <a name="examples"></a>Örnekler

Aşağıdaki tabloda, yaygın filtrelerin birkaç örneği yer almaktadır.  

| Alan Adı | Değerler için sorgu | Etiket |
|:--|:--|:--|
| Bilgisayar   | Sinyal &#124; farklı bilgisayar &#124; bilgisayar için Sırala ASC | Bilgisayarlar |
| EventLevelName | Event &#124; farklı EventLevelName | Önem Derecesi |
| Severıtylevel | Syslog &#124; farklı SeverityLevel | Önem Derecesi |
| SvcChangeType | ConfigurationChange &#124; DISTINCT svcChangeType | ChangeType |


## <a name="modify-view-queries"></a>Görünüm sorgularını Değiştir

Bir filtrenin herhangi bir etkiye sahip olması için, görünümdeki tüm sorguları seçili değerlere göre filtreleyecek şekilde değiştirmeniz gerekir.  Görünümdeki herhangi bir sorguyu değiştirmezseniz, kullanıcının seçtiği tüm değerlerin hiçbir etkisi olmayacaktır.

Sorguda filtre değeri kullanmanın sözdizimi şöyledir: 

`where ${filter name}`  

Örneğin, görünümünüzde olayları döndüren ve _bilgisayarlar_adlı bir filtre kullanan bir sorgu varsa, aşağıdaki sorguyu kullanabilirsiniz.

```kusto
Event | where ${Computers} | summarize count() by EventLevelName
```

Önem derecesi adlı başka bir filtre eklediyseniz, her iki filtreyi da kullanmak için aşağıdaki sorguyu kullanabilirsiniz.

```kusto
Event | where ${Computers} | where ${Severity} | summarize count() by EventLevelName
```

## <a name="next-steps"></a>Sonraki adımlar
* Özel görünüminizdeki ekleyebileceğiniz [görselleştirme bölümleri](view-designer-parts.md) hakkında daha fazla bilgi edinin.
