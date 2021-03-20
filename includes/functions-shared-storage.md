---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/20/2018
ms.author: glenga
ms.openlocfilehash: 82d122ed236dc72ced7ebafe2301ef5f1143897f
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "76963673"
---
Performansı en üst düzeye çıkarmak için, her bir işlev uygulaması için ayrı bir depolama hesabı kullanın. Bu, her ikisi de yüksek hacimli depolama işlemleri üreten Dayanıklı İşlevler veya Olay Hub 'ı tetiklenen işlevleriniz olduğunda özellikle önemlidir. Uygulama mantığınızın, doğrudan (depolama SDK 'sını kullanarak) veya depolama bağlamalarından biri aracılığıyla Azure depolama ile etkileşime geçtiğinde, ayrılmış bir depolama hesabı kullanmanız gerekir. Örneğin, blob depolamaya bazı veriler yazan bir olay hub 'ı tetiklemeli işleviniz varsa, işlev &mdash; uygulaması için bir tane olmak üzere iki depolama hesabı ve işlev tarafından saklanan Bloblar için bir diğeri kullanın.