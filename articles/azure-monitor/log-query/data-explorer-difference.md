---
title: Azure İzleyici günlük sorgu dili farklılıkları | Microsoft Docs
description: Azure İzleyici tarafından kullanılan veri Gezgini sorgu dili için başvuru bilgileri. Azure İzleyici belirli ek öğeler ve Azure izleyici günlüğü sorgularda desteklenmez öğeleri içerir.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/31/2018
ms.author: bwren
ms.openlocfilehash: 9c58796fa19ffb6d38582c809f7bb6ca948bd92c
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2019
ms.locfileid: "56003645"
---
# <a name="azure-monitor-log-query-language-differences"></a>Azure İzleyici günlük sorgu dili farklılıkları

Sırada [Azure İzleyici'de oturum](log-query-overview.md) üzerine kurulmuştur [Azure Veri Gezgini](/azure/data-explorer) ve kullandığı [aynı sorgu dilini](/azure/kusto/query), dil sürümü bazı farklar vardır. Bu makalede, Veri Gezgini'ni ve Azure İzleyici günlük sorguları için kullanılan sürümü için kullanılan dil sürümü arasındaki farkları öğeleri tanımlar.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="data-explorer-elements-not-supported-in-azure-monitor"></a>Azure İzleyici'de desteklenmeyen veri Gezgini öğeleri
Aşağıdaki bölümlerde, Azure İzleyici tarafından desteklenmeyen veri Gezgini sorgu dilinin öğelerini açıklar.

### <a name="statements-not-supported-in-azure-monitor"></a>Azure İzleyici'de desteklenmeyen ifadeler

* [Diğer ad](/azure/kusto/query/aliasstatement)
* [Sorgu parametreleri](/azure/kusto/query/queryparametersstatement)

### <a name="functions-not-supported-in-azure-monitor"></a>Azure İzleyici'de desteklenmeyen işlevleri

* [cluster()](/azure/kusto/query/clusterfunction)
* [cursor_after()](/azure/kusto/query/cursorafterfunction)
* [cursor_before_or_at()](/azure/kusto/query/cursorbeforeoratfunction)
* [cursor_current(), current_cursor()](/azure/kusto/query/cursorcurrent)
* [database()](/azure/kusto/query/databasefunction)
* [current_principal()](/azure/kusto/query/current-principalfunction)
* [extent_id()](/azure/kusto/query/extentidfunction)
* [extent_tags()](/azure/kusto/query/extenttagsfunction)

### <a name="operators-not-supported-in-azure-monitor"></a>Azure İzleyici'de desteklenmeyen işleçleri

* [Çapraz-küme birleştirme](/azure/kusto/query/joincrosscluster)
* [externaldata işleci](/azure/kusto/query/externaldata-operator)

### <a name="plugins-not-supported-in-azure-monitor"></a>Azure İzleyici'de desteklenmeyen eklentileri

* [sql_request plugin](/azure/kusto/query/sqlrequestplugin)


## <a name="additional-operators-in-azure-monitor"></a>Azure İzleyicisi'nde ek işleçleri
Aşağıdaki işleçleri belirli Azure İzleyici özellikleri destekler ve Azure İzleyici dışında kullanılamaz.

* [app()](app-expression.md)
* [Workspace()](workspace-expression.md)

## <a name="next-steps"></a>Sonraki adımlar

- Başvurular farklı alma [kaynakları Azure İzleyici yazmak için oturum sorguları](query-language.md).
- Tam erişim [başvuru belgeleri için Veri Gezgini'ni sorgu dili](/azure/kusto/query/).
