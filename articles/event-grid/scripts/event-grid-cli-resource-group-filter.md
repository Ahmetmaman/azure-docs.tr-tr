---
title: Azure CLı-kaynağa göre filtrelemek & kaynak grubuna abone olma
description: Bu makalede bir kaynak için Event Grid olaylarına nasıl abone olunacağı ve bir kaynağın filtreleneceği bir örnek Azure CLı betiği sunulmaktadır.
ms.devlang: azurecli
ms.topic: sample
ms.date: 07/08/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 1a4343f4fb7791459cc3dc6e7db34433d8b7b60f
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2021
ms.locfileid: "107766650"
---
# <a name="subscribe-to-events-for-a-resource-group-and-filter-for-a-resource-with-azure-cli"></a>Azure CLI ile bir kaynak grubu için olaylara abone olma ve kaynağa göre filtreleme

Bu betik, bir kaynak grubu için olaylara bir Event Grid aboneliği oluşturur. Yalnızca kaynak grubundaki belirtilen bir kaynakla ilgili olayları almak için bir filtre kullanır.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Önizleme örnek betiği için Event Grid uzantısı gerekir. Yüklemek için `az extension add --name eventgrid` komutunu çalıştırın.

## <a name="sample-script---stable"></a>Örnek betik - kararlı

[!code-azurecli[main](../../../cli_scripts/event-grid/filter-events/filter-events.sh "Subscribe to Azure subscription")]

## <a name="sample-script---preview-extension"></a>Örnek betik - önizleme uzantısı

[!code-azurecli[main](../../../cli_scripts/event-grid/filter-events-preview/filter-events-preview.sh "Subscribe to Azure subscription")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, olay aboneliğini oluşturmak için aşağıdaki komutu kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az eventgrid event-subscription create](/cli/azure/eventgrid/event-subscription#az_eventgrid_event_subscription_create) | Event Grid aboneliği oluşturun. |
| [az eventgrid event-subscription create](/cli/azure/ext/eventgrid/eventgrid/event-subscription#ext-eventgrid-az-eventgrid-event-subscription-create) - uzantı sürümü | Event Grid aboneliği oluşturun. |

## <a name="next-steps"></a>Sonraki adımlar

* Abonelikleri sorgulama hakkında bilgi edinmek için bkz. [Event Grid aboneliklerini sorgulama](../query-event-subscriptions.md).
* Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).
