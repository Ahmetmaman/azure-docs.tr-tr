---
title: Azure için vCPU kotaları
description: Azure sanal makineleri için vCPU kotaları hakkında bilgi edinin.
author: cynthn
ms.service: virtual-machines
ms.subservice: quota
ms.workload: infrastructure-services
ms.topic: conceptual
ms.date: 05/31/2018
ms.author: cynthn
ms.openlocfilehash: ca7d95a9916aafdab2550eee48ea05ddfa5874c1
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2021
ms.locfileid: "102560727"
---
# <a name="check-vcpu-quotas-using-azure-powershell"></a>Azure PowerShell kullanarak vCPU kotalarını denetleme

Sanal makineler ve sanal makine ölçek kümeleri için vCPU kotaları her bir bölgede her bir abonelik için iki katmanda düzenlenir. İlk katman toplam bölgesel vCPU sayısı ve ikinci katman D serisi vCPU 'Lar gibi çeşitli VM boyutu ailesi çekirdekleri. Yeni bir VM 'nin dağıtıldığı her zaman, sanal makine için vCPU 'Lar VM boyut ailesi için vCPU kotasını veya toplam bölgesel vCPU kotasını aşmamalıdır. Bu kotalardan biri aşılırsa, VM dağıtımına izin verilmez. Ayrıca, bölgedeki toplam sanal makine sayısı için bir kota de vardır. Bu kotaların her biri hakkındaki ayrıntılar, [Azure Portal](https://portal.azure.com) **abonelik** sayfasının **kullanım + kotalar** bölümünde görünebilir veya PowerShell kullanarak değerleri sorgulayabilirsiniz.

> [!NOTE]
> Kota, birlikte kullanılan ve serbest bırakılmış toplam çekirdek sayısına göre hesaplanır. Ek çekirdeklere ihtiyacınız varsa, daha fazla ihtiyaç duyulmayan [bir kota artışı](../../azure-portal/supportability/resource-manager-core-quotas-request.md) veya silme VM 'leri isteyin. 
 
## <a name="check-usage"></a>Kullanımı denetleme

Kota kullanımınızı denetlemek için [Get-AzVMUsage](/powershell/module/az.compute/get-azvmusage) cmdlet 'ini kullanabilirsiniz.

```azurepowershell-interactive
Get-AzVMUsage -Location "East US"
```

Çıktı şuna benzer olacaktır:

```
Name                             Current Value Limit  Unit
----                             ------------- -----  ----
Availability Sets                            0  2000 Count
Total Regional vCPUs                         4   260 Count
Virtual Machines                             4 10000 Count
Virtual Machine Scale Sets                   1  2000 Count
Standard B Family vCPUs                      1    10 Count
Standard DSv2 Family vCPUs                   1   100 Count
Standard Dv2 Family vCPUs                    2   100 Count
Basic A Family vCPUs                         0   100 Count
Standard A0-A7 Family vCPUs                  0   250 Count
Standard A8-A11 Family vCPUs                 0   100 Count
Standard D Family vCPUs                      0   100 Count
Standard G Family vCPUs                      0   100 Count
Standard DS Family vCPUs                     0   100 Count
Standard GS Family vCPUs                     0   100 Count
Standard F Family vCPUs                      0   100 Count
Standard FS Family vCPUs                     0   100 Count
Standard NV Family vCPUs                     0    24 Count
Standard NC Family vCPUs                     0    48 Count
Standard H Family vCPUs                      0     8 Count
Standard Av2 Family vCPUs                    0   100 Count
Standard LS Family vCPUs                     0   100 Count
Standard Dv2 Promo Family vCPUs              0   100 Count
Standard DSv2 Promo Family vCPUs             0   100 Count
Standard MS Family vCPUs                     0     0 Count
Standard Dv3 Family vCPUs                    0   100 Count
Standard DSv3 Family vCPUs                   0   100 Count
Standard Ev3 Family vCPUs                    0   100 Count
Standard ESv3 Family vCPUs                   0   100 Count
Standard FSv2 Family vCPUs                   0   100 Count
Standard ND Family vCPUs                     0     0 Count
Standard NCv2 Family vCPUs                   0     0 Count
Standard NCv3 Family vCPUs                   0     0 Count
Standard LSv2 Family vCPUs                   0     0 Count
Standard Storage Managed Disks               2 10000 Count
Premium Storage Managed Disks                1 10000 Count
```


## <a name="reserved-vm-instances"></a>Ayrılmış VM Örnekleri
VM boyutu esnekliği olmayan tek bir aboneliğin kapsamına alınmış ayrılmış VM örnekleri, vCPU kotaları için yeni bir boyut ekler. Bu değerler, belirtilen boyutun abonelikte dağıtılabilir olması gereken örneklerinin sayısını anlatır. Kota, ayrılmış VM örneklerinin abonelikte dağıtılabilir olmasını sağlamak için kotanın ayrıldığından emin olmak üzere kota sisteminde bir yer tutucu olarak çalışır. Örneğin, belirli bir abonelikte 10 Standard_D1 ayrılmış VM örneği varsa, Standard_D1 ayrılmış VM örnekleri için kullanım sınırı 10 olur. Bu, Azure 'un, Standard_D1 örnekleri için kullanılmak üzere toplam bölgesel vCPU kotasında her zaman en az 10 vCPU olduğundan ve Standard_D1 örnekleri için kullanılacak standart D ailesi vCPU kotasında en az 10 vCPU olduğundan emin olmasına neden olur.

Tek bir aboneliği satın almak için kota artışı gerekiyorsa, aboneliğinizde [bir kota artışı isteyebilirsiniz](../../azure-portal/supportability/resource-manager-core-quotas-request.md) .

## <a name="next-steps"></a>Sonraki adımlar

Faturalandırma ve Kotalar hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../../azure-resource-manager/management/azure-subscription-service-limits.md?toc=/azure/billing/TOC.json).
