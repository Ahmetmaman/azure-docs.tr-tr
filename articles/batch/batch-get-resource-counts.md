---
title: Görevler ve düğümler için durum sayısı
description: Toplu Iş çözümlerini yönetmeye ve izlemeye yardımcı olmak için Azure Batch görevlerinin ve işlem düğümlerinin durumunu say.
ms.date: 06/18/2020
ms.topic: how-to
ms.custom: seodec18
ms.openlocfilehash: 90f741b9ec5e17da4fd0cc95ef921e116b0c27dc
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "85960597"
---
# <a name="monitor-batch-solutions-by-counting-tasks-and-nodes-by-state"></a>Görevleri ve düğümleri duruma göre sayarak Batch çözümlerini izleme

Büyük ölçekli Azure Batch çözümlerini izlemek ve yönetmek için çeşitli durumlarda kaynak sayılarını belirlemeniz gerekebilir. Azure Batch, Batch görevleri ve işlem düğümleri için sayıları almak üzere etkili işlemler sağlar. Bu işlemleri, büyük görev veya düğüm koleksiyonları hakkında ayrıntılı bilgi döndüren zaman alıcı liste sorguları yerine kullanabilirsiniz.

- [Get görev sayıları](/rest/api/batchservice/job/gettaskcounts) bir işte etkin, çalışan ve tamamlanmış görevlerin toplam sayısını ve başarılı veya başarısız olan görevleri alır. 

  Her durumda görevleri sayarak, bir kullanıcıya iş ilerlemesini daha kolay bir şekilde görüntüleyebilir veya işi etkileyebilecek beklenmedik gecikmeleri veya sorunları tespit edebilirsiniz. Toplu Iş Hizmetleri API sürümü 2017 -06-01.5.1 ve ilgili SDK 'lar ve araçlar olarak görev sayısını Al kullanılabilir.

- [Liste havuzu düğüm sayıları](/rest/api/batchservice/account/listpoolnodecounts) , çeşitli durumlarda bulunan her havuzda adanmış ve düşük öncelikli işlem düğümleri sayısını alır: oluşturma, boşta, çevrimdışı, ön işleme, yeniden başlatma, yeniden görüntüleme, başlangıç ve diğer durumlar.

  Her durumda düğümleri sayarak, işlerinizi çalıştırmak için yeterli işlem kaynağına sahip olduğunuz ve havuzlarınızla ilgili olası sorunları tespit edebilirsiniz. Liste havuzu düğüm sayıları Batch hizmeti API sürümü 2018 -03-01.6.1 ve ilgili SDK 'lar ve araçlar olarak kullanılabilir.

Zaman, bu işlemlerin döndürdüğü sayıların güncel olamayacağını unutmayın. Bir sayının doğru olduğundan emin olmanız gerekiyorsa, bu kaynakları saymak için bir liste sorgusu kullanın. Liste sorguları Ayrıca, uygulamalar gibi diğer Batch kaynakları hakkında bilgi almanızı sağlar. Liste sorgularına filtre uygulama hakkında daha fazla bilgi için bkz. [Batch kaynaklarını etkili bir şekilde listelemek için sorgular oluşturma](batch-efficient-list-queries.md).

## <a name="task-state-counts"></a>Görev durumu sayıları

Görev sayılarını al işlemi görevleri aşağıdaki durumlara göre sayar:

- **Etkin** -sıraya alınmış ve çalıştırılabilen, ancak şu anda bir işlem düğümüne atanmamış bir görevdir. Bir görev, `active` henüz tamamlanmamış [bir üst göreve bağımlıysa](batch-task-dependencies.md) de bir görevdir. 
- **Çalışıyor** -bir işlem düğümüne atanan ancak henüz tamamlanmamış bir görev. Bir görev, `running` `preparing` `running` [bir görev hakkında bilgi al](/rest/api/batchservice/task/get) işlemiyle gösterildiği gibi, durumu veya olduğu zaman sayılır.
- **Tamamlandı** -başarıyla bittiği veya başarısız olduğu ve ayrıca yeniden deneme sınırını tükettiğinden, artık çalıştırılmasına uygun olmayan bir görev. 
- **Başarılı** -görev yürütme sonucu olan bir görev `success` . Batch `TaskExecutionResult` , [ExecutionInfo](/rest/api/batchservice/task/get) özelliğinin özelliğini denetleyerek bir görevin başarılı veya başarısız olup olmadığını belirler.
- **Başarısız oldu** Görev yürütme sonucu olan bir görev `failure` .

Aşağıdaki .NET kod örneği, görev sayılarının duruma göre nasıl alınacağını gösterir:

```csharp
var taskCounts = await batchClient.JobOperations.GetJobTaskCountsAsync("job-1");

Console.WriteLine("Task count in active state: {0}", taskCounts.Active);
Console.WriteLine("Task count in preparing or running state: {0}", taskCounts.Running);
Console.WriteLine("Task count in completed state: {0}", taskCounts.Completed);
Console.WriteLine("Succeeded task count: {0}", taskCounts.Succeeded);
Console.WriteLine("Failed task count: {0}", taskCounts.Failed);
```

Bir iş için görev sayısını almak üzere REST ve diğer desteklenen diller için de benzer bir model kullanabilirsiniz. 

> [!NOTE]
> Batch hizmeti API sürümleri 2018 -08-01.7.0 Ayrıca `validationStatus` görev sayısını Al yanıtı içinde bir özellik döndürüyor. Bu özellik, toplu Işlemin durumunun liste görevleri API 'sinde raporlanan durumlarıyla tutarlı olup olmadığını kontrol edilip edilmeyeceğini belirtir. Değeri `validated` , yalnızca iş için en az bir kez tutarlılık denetimi yapıldığını gösterir. Özelliğin değeri, `validationStatus` görev saylarının döndürdüğü sayımların Şu anda güncel olup olmadığını göstermez.

## <a name="node-state-counts"></a>Düğüm durumu sayıları

Liste havuzu düğüm sayısı işlemi, her havuzda aşağıdaki durumlara göre işlem düğümlerini sayar. Ayrı toplama sayıları, her havuzdaki adanmış düğümler ve düşük öncelikli düğümler için sağlanır.

- **Oluşturma** -bir havuza katılmayı henüz başlatılmamış, Azure ile ayrılmış bir VM.
- **Boşta** -Şu anda bir görevi çalıştırmayan kullanılabilir bir işlem düğümü.
- **Leavingpool** -Kullanıcı açıkça kaldırıldığı veya havuz yeniden boyutlandırılırken ya da otomatik olarak ölçeklendirildiği için havuzdan çıkarılan bir düğüm.
- Çevrimdışı-Batch 'in yeni görevleri zamanlamak için **kullandıı** düğüm.
- **Ön,** Azure VM 'yi geri karşılayan için havuzdan kaldırılan düşük öncelikli bir düğüm. `preempted`Düşük öncelıklı VM kapasitesini değiştirme kullanılabilir olduğunda bir düğüm yeniden başlatılır.
- Yeniden **başlatılıyor** -yeniden başlatılan bir düğüm.
- **Yeniden görüntüleme** -işletim sisteminin yeniden yüklendiği düğüm.
- **Çalışıyor** -bir veya daha fazla görev (başlangıç görevi dışında) çalıştıran bir düğüm.
- **Başlangıç** -Batch hizmetinin başladığı bir düğüm. 
- **Starttaskfailed** - [Başlangıç görevinin](/rest/api/batchservice/pool/add#starttask) başarısız olduğu ve tüm yeniden denemeler tükendi ve `waitForSuccess` Başlangıç görevinde ayarlanan bir düğüm. Düğüm, görevleri çalıştırmak için kullanılamaz.
- **Bilinmiyor** -Batch hizmetiyle iletişim kurmayan ve durumu bilinmeyen bir düğüm.
- **Kullanılamaz** -hatalar nedeniyle görev yürütmesi için kullanılamayan düğüm.
- **Waitingforstarttask** -başlangıç görevinin çalışmaya başladığı, ancak `waitForSuccess` ayarlandığı ve başlangıç görevinin tamamlanmadığında oluşan bir düğümdür.

Aşağıdaki C# kod parçacığında, geçerli hesaptaki tüm havuzlar için düğüm sayımlarını listeleme gösterilmektedir:

```csharp
foreach (var nodeCounts in batchClient.PoolOperations.ListPoolNodeCounts())
{
    Console.WriteLine("Pool Id: {0}", nodeCounts.PoolId);

    Console.WriteLine("Total dedicated node count: {0}", nodeCounts.Dedicated.Total);

    // Get dedicated node counts in Idle and Offline states; you can get additional states.
    Console.WriteLine("Dedicated node count in Idle state: {0}", nodeCounts.Dedicated.Idle);
    Console.WriteLine("Dedicated node count in Offline state: {0}", nodeCounts.Dedicated.Offline);

    Console.WriteLine("Total low priority node count: {0}", nodeCounts.LowPriority.Total);

    // Get low-priority node counts in Running and Preempted states; you can get additional states.
    Console.WriteLine("Low-priority node count in Running state: {0}", nodeCounts.LowPriority.Running);
    Console.WriteLine("Low-priority node count in Preempted state: {0}", nodeCounts.LowPriority.Preempted);
}
```

Aşağıdaki C# kod parçacığında, geçerli hesaptaki belirli bir havuzun düğüm sayımlarını listeleme gösterilmektedir.

```csharp
foreach (var nodeCounts in batchClient.PoolOperations.ListPoolNodeCounts(new ODATADetailLevel(filterClause: "poolId eq 'testpool'")))
{
    Console.WriteLine("Pool Id: {0}", nodeCounts.PoolId);

    Console.WriteLine("Total dedicated node count: {0}", nodeCounts.Dedicated.Total);

    // Get dedicated node counts in Idle and Offline states; you can get additional states.
    Console.WriteLine("Dedicated node count in Idle state: {0}", nodeCounts.Dedicated.Idle);
    Console.WriteLine("Dedicated node count in Offline state: {0}", nodeCounts.Dedicated.Offline);

    Console.WriteLine("Total low priority node count: {0}", nodeCounts.LowPriority.Total);

    // Get low-priority node counts in Running and Preempted states; you can get additional states.
    Console.WriteLine("Low-priority node count in Running state: {0}", nodeCounts.LowPriority.Running);
    Console.WriteLine("Low-priority node count in Preempted state: {0}", nodeCounts.LowPriority.Preempted);
}
```

Havuzlar için düğüm sayılarını almak üzere REST ve diğer desteklenen diller için de benzer bir model kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Batch hizmeti iş akışı ve](batch-service-workflow-features.md) havuzlar, düğümler, işler ve görevler gibi birincil kaynaklar hakkında bilgi edinin.
- Batch kaynaklarını listeleyerek sorgulara filtre uygulama hakkında bilgi edinin. [Batch kaynaklarını verimli bir şekilde listelemek için sorgular oluşturma](batch-efficient-list-queries.md)konusuna bakın.
