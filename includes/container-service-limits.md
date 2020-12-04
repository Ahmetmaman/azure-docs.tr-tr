---
title: dosya dahil etme
description: dosya dahil etme
services: container-service
author: mlearned
ms.service: container-service
ms.topic: include
ms.date: 11/22/2019
ms.author: mlearned
ms.custom: include file
ms.openlocfilehash: a42bba1b6524825aa571e4c18319b61b97829792
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96584568"
---
| Kaynak | Sınır |
| --- | :--- |
| Abonelik başına en fazla küme | 1000 |
| Sanal makine kullanılabilirlik kümeleri ve temel Load Balancer SKU 'SU ile küme başına en fazla düğüm  | 100 |
| Sanal Makine Ölçek Kümeleri ve [Standart Load Balancer SKU 'su][standard-load-balancer] olan küme başına en fazla düğüm | 1000 ( [düğüm havuzu][node-pool]başına 100 düğüm) |
| Düğüm başına maksimum Pod: kubenet ile [temel ağ][basic-networking] | 110 |
| Düğüm başına en fazla düğüm sayısı: Azure Container ağ arabirimi ile [Gelişmiş ağ][advanced-networking] | Azure CLI dağıtımı: 30<sup>1</sup><br />Azure Resource Manager şablonu: 30<sup>1</sup><br />Portal dağıtımı: 30 |

<sup>1</sup> Azure CLI veya Kaynak Yöneticisi şablonuyla bir Azure Kubernetes hizmeti (aks) kümesi dağıttığınızda, bu değer düğüm başına 250 Pod 'ye yapılandırılabilir. Zaten bir aks kümesi dağıttıktan sonra veya Azure Portal kullanarak bir küme dağıtırsanız, düğüm başına maksimum Pod yapılandıramazsınız.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#kubenet-basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#azure-cni-advanced-networking
[standard-load-balancer]: ../articles/load-balancer/load-balancer-overview.md
[node-pool]: ../articles/aks/use-multiple-node-pools.md

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest