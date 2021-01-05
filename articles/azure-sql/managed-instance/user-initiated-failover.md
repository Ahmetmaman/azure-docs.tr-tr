---
title: SQL yönetilen örneği üzerinde el ile yük devretme başlatma
description: Azure SQL yönetilen örneği 'nde birincil ve ikincil çoğaltmaların el ile yük devretmesini öğrenin.
services: sql-database
ms.service: sql-managed-instance
ms.custom: seo-lt-2019, sqldbrb=1
ms.devlang: ''
ms.topic: how-to
author: danimir
ms.author: danil
ms.reviewer: douglas, sstein
ms.date: 12/16/2020
ms.openlocfilehash: 4b1c98d8621267b300a82b697bce66a6b94e82f3
ms.sourcegitcommit: e7179fa4708c3af01f9246b5c99ab87a6f0df11c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/30/2020
ms.locfileid: "97825931"
---
# <a name="user-initiated-manual-failover-on-sql-managed-instance"></a>SQL Yönetilen Örneği'nde kullanıcı tarafından başlatılan el ile yük devretme

Bu makalede, SQL yönetilen örnek Genel Amaçlı (GP) ve İş Açısından Kritik (BC) hizmeti katmanlarında bir birincil düğümün nasıl el ile yük devretmekte ve yalnızca BC hizmet katmanında ikincil bir salt okuma Çoğaltma düğümünün el ile nasıl yük devretmekte olduğu açıklanır.

## <a name="when-to-use-manual-failover"></a>El ile yük devretme ne zaman kullanılır?

[Yüksek kullanılabilirlik](../database/high-availability-sla.md) , SQL yönetilen örnek platformunun, veritabanı uygulamalarınız için saydam olarak çalışabilen temel bir parçasıdır. Düğüm performansında azalma veya hata algılama durumunda birincil ile ikincil düğümlere yük devretme veya normal aylık yazılım güncelleştirmeleri sırasında, Azure 'da SQL yönetilen örneği kullanan tüm uygulamalar için beklenen bir durumdur.

Aşağıdaki nedenlerden bazıları için SQL yönetilen örneği üzerinde [el ile yük devretme](../database/high-availability-sla.md#testing-application-fault-resiliency) yürütmeyi düşünebilirsiniz:
- Üretime dağıtılmadan önce uygulamayı yük devretme dayanıklılığı için test edin
- Otomatik yük devretmeler için hata dayanıklılığı için uçtan uca sistemleri test edin
- Yük devretmenin var olan veritabanı oturumlarını nasıl etkilediğini test etme
- Ağ gecikmede yapılan değişiklikler nedeniyle bir yük devretmenin uçtan uca performansını değiştirdiğini doğrulayın
- Sorgu performansı azalmaları bazı durumlarda, el ile yük devretme, performans sorununu azaltmaya yardımcı olabilir.

> [!NOTE]
> Üretim ortamına dağıtmadan önce uygulamalarınızın yük devri açısından dayanıklı olmasını sağlamak, üretimde uygulama hatalarının riskini azaltmaya yardımcı olur ve müşterileriniz için uygulama kullanılabilirliğine katkıda bulunur.

## <a name="initiate-manual-failover-on-sql-managed-instance"></a>SQL yönetilen örneği üzerinde el ile yük devretme başlatma

### <a name="azure-rbac-permissions-required"></a>Azure RBAC izinleri gerekli

Yük devretmeyi başlatan kullanıcının aşağıdaki Azure rollerinden birine sahip olması gerekir:

- Abonelik sahibi rolü veya
- Yönetilen örnek katılımcısı rolü veya
- Aşağıdaki izne sahip özel rol:
  - `Microsoft.Sql/managedInstances/failover/action`

### <a name="using-powershell"></a>PowerShell’i kullanma

En az az. SQL sürümünün [v 2.9.0](https://www.powershellgallery.com/packages/Az.Sql/2.9.0)olması gerekir. Her zaman en son PowerShell sürümü bulunan Azure portal [Azure Cloud Shell](../../cloud-shell/overview.md) kullanmayı göz önünde bulundurun. 

Bir ön gereksinim olarak, gerekli Azure modüllerini yüklemek için aşağıdaki PowerShell betiğini kullanın. Ayrıca, yük devretmek istediğiniz yönetilen örneğin bulunduğu aboneliği seçin.

```powershell
$subscription = 'enter your subscription ID here'
Install-Module -Name Az
Import-Module Az.Accounts
Import-Module Az.Sql

Connect-AzAccount
Select-AzSubscription -SubscriptionId $subscription
```

Birincil düğümün yük devretmesini başlatmak için aşağıdaki örnekle birlikte [Invoke-Azsqlınstancefailover](/powershell/module/az.sql/invoke-azsqlinstancefailover) PowerShell komutunu kullanın.

```powershell
$ResourceGroup = 'enter resource group of your MI'
$ManagedInstanceName = 'enter MI name'
Invoke-AzSqlInstanceFailover -ResourceGroupName $ResourceGroup -Name $ManagedInstanceName
```

Yalnızca BC hizmet katmanı için geçerli olan okuma ikincil düğümünü yük devretmek için aşağıdaki PS komutunu kullanın.

```powershell
$ResourceGroup = 'enter resource group of your MI'
$ManagedInstanceName = 'enter MI name'
Invoke-AzSqlInstanceFailover -ResourceGroupName $ResourceGroup -Name $ManagedInstanceName -ReadableSecondary
```

### <a name="using-cli"></a>CLI kullanma

En son CLı betiklerinin yüklü olduğundan emin olun.

Birincil düğümün yük devretmesini başlatmak için aşağıdaki örnekle az SQL mı yük devretme CLı komutunu kullanın.

```cli
az sql mi failover -g myresourcegroup -n myinstancename
```

Yalnızca BC hizmet katmanı için geçerli olan okuma ikincil düğümünü yük devretmek için aşağıdaki CLı komutunu kullanın.

```cli
az sql mi failover -g myresourcegroup -n myinstancename --replica-type ReadableSecondary
```

### <a name="using-rest-api"></a>Rest API'yi kullanma

Sürekli test işlem hattı veya otomatik performans mitigators uygulama amacıyla SQL tarafından yönetilen örneklerinin yük devretmesini otomatik hale getirmesi gerekebilecek ileri düzey kullanıcılar için, bu işlev bir API çağrısıyla yük devretme başlatılmasına göre gerçekleştirilebilir. Ayrıntılar için bkz. [yönetilen örnekler-yük devretme REST API](/rest/api/sql/managed%20instances%20-%20failover/failover) .

REST API çağrısı kullanarak yük devretmeyi başlatmak için önce tercih ettiğiniz API istemcisini kullanarak kimlik doğrulama belirtecini oluşturun. Oluşturulan kimlik doğrulama belirteci, API isteği üstbilgisinde Authorization özelliği olarak kullanılır ve zorunludur.

Aşağıdaki kod, çağrılacak API URI 'sinin bir örneğidir:

```HTTP
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Sql/managedInstances/{managedInstanceName}/failover?api-version=2019-06-01-preview
```

Aşağıdaki özelliklerin API çağrısında geçirilmesi gerekir:

| **API özelliği** | **Parametre** |
| --- | --- |
| subscriptionId | Yönetilen örneğin dağıtıldığı abonelik KIMLIĞI |
| resourceGroupName | Yönetilen örnek içeren kaynak grubu |
| Managedınstancename | Yönetilen örnek adı |
| replicaType | Seçim (Birincil veya ReadableSecondary). Bu parametreler yük devredilecek çoğaltmanın türünü temsil eder: birincil veya okunabilir ikincil. Belirtilmemişse, varsayılan olarak birincil çoğaltmada yük devretme başlatılır. |
| api-sürümü | Statik değer ve şu anda "2019-06-01-Preview" olması gerekir |

API yanıtı, aşağıdakilerden biri olacaktır:

- 202 kabul edildi
- 400 istek hatalarından biri.

İşlem durumu, yanıt üst bilgilerinde API yanıtlarını gözden geçirerek izlenebilir. Daha fazla bilgi için bkz. [zaman uyumsuz Azure Işlemlerinin durumu](../../azure-resource-manager/management/async-operations.md).

## <a name="monitor-the-failover"></a>Yük devretmeyi izleme

Kullanıcı tarafından başlatılan el ile yük devretmenin ilerlemesini izlemek için, SQL yönetilen örneği üzerinde en sevdiğiniz istemcinizde (SSMS) aşağıdaki T-SQL sorgusunu yürütün. Bu işlem, örnekte bulunan sistem görünümü sys.dm_hadr_fabric_replica_states ve rapor çoğaltmalarını okur. El ile yük devretmeyi başlattıktan sonra aynı sorguyu yenileyin.

```T-SQL
SELECT DISTINCT replication_endpoint_url, fabric_replica_role_desc FROM sys.dm_hadr_fabric_replica_states
```

Yük devretmeyi başlatmadan önce, çıktı, BC hizmet katmanındaki geçerli birincil çoğaltmayı AlwaysOn kullanılabilirlik grubunda bir birincil ve üç ikincil alan içerecek şekilde gösterecektir. Yük devretme işlemi tamamlandıktan sonra, bu sorguyu yeniden çalıştırmak, birincil düğümün bir değişikliğini belirtmesinin gerekli olması gerekir.

Daha önce BC için gösterilen GP hizmet katmanıyla aynı çıktıyı göremezsiniz. Bunun nedeni, GP hizmet katmanının yalnızca tek bir düğümü temel alır. GP hizmet katmanı için T-SQL sorgu çıktısı, yük devretmeden önce ve sonra tek bir düğüm gösterir. Yük devretme sırasında istemcinizden bağlantı kaybı, genellikle bir dakika içinde, yük devretme yürütmesinin göstergesi olacaktır.

> [!NOTE]
> Yük devretme işleminin tamamlanması (gerçek kısa kullanım dışı kalması), **yüksek yoğunlukta** iş yükleri olması durumunda birkaç dakika sürebilir. Bunun nedeni, örnek altyapısının, yük devretmeden önce birincil ve ikincil düğümde yakalandığı tüm geçerli işlemleri ele alırken.

> [!IMPORTANT]
> Kullanıcı tarafından başlatılan el ile yük devretmenin işlevsel sınırlamaları şunlardır:
> - Her **15 dakikada** bir yönetilen örnek üzerinde bir (1) yük devretme başlatılmış olabilir.
> - BC örnekleri için, yük devretme isteğinin kabul edilmesi için mevcut çoğaltmaların çekirdeği olması gerekir.
> - BC örnekleri için, hangi okunabilir ikincil çoğaltmanın üzerinde yük devretmeyi başlatacağını belirtmek mümkün değildir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL yönetilen örneği için](../database/high-availability-sla.md)yüksek oranda yönetilen örnek kullanılabilirliği hakkında daha fazla bilgi edinin.
- Genel bakış için bkz. [Azure SQL yönetilen örneği nedir?](sql-managed-instance-paas-overview.md).
