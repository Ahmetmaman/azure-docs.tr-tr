---
title: Batch hizmeti iş akışı ve kaynakları
description: Batch hizmetinin özellikleri ve bir geliştirme açısından üst düzey iş akışı hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 05/12/2020
ms.openlocfilehash: 76a0b140f3bea4b07a6de632abbcbc3fd26e582d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "85965221"
---
# <a name="batch-service-workflow-and-resources"></a>Batch hizmeti iş akışı ve kaynakları

Azure Batch hizmetinin temel bileşenlerine ilişkin bu genel bakışta, Batch geliştiricilerinin büyük ölçekli paralel işlem çözümleri oluşturmak için kullandığı üst düzey iş akışını ve kullanılan birincil hizmet kaynaklarını tartıştık.

Dağıtılmış bir Işlem uygulaması veya doğrudan [REST API](/rest/api/batchservice/) çağrıları veren bir hizmet geliştirirken ya da [Batch SDK](batch-apis-tools.md#batch-service-apis)'larından başka birini kullanıyorsanız, burada ele alınan kaynak ve özelliklerin birçoğunu kullanırsınız.

> [!TIP]
> Batch hizmetine daha yüksek düzeyde bir giriş için bkz. [Azure Batch temel bilgileri](batch-technical-overview.md). Ayrıca en son [Toplu İşlem hizmet güncelleştirmeleri](https://azure.microsoft.com/updates/?product=batch)’ne bakın.

## <a name="basic-workflow"></a>Temel iş akışı

Aşağıdaki üst düzey iş akışı, paralel iş yüklerini işlemek üzere Batch hizmetini kullanan neredeyse tüm uygulamalar ve hizmetler için tipiktir:

1. İşlemek istediğiniz **veri dosyalarını** bir [Azure Depolama](../storage/index.yml) hesabına yükleyin. Batch, Azure Blob depolama alanına erişim için yerleşik destek içerir ve görevler çalıştırıldığında görevleriniz bu dosyaları [işlem düğümlerine](nodes-and-pools.md#nodes) indirebilir.
2. Görevlerinizin çalıştıracağı **uygulama dosyalarını** karşıya yükleyin. Bu dosyalar ikili dosyalar ya da komut dosyaları ve onların bağımlılıkları olabilir ve işlerinizdeki görevler tarafından yürütülür. Görevleriniz bu dosyaları Storage hesabınızdan indirebilir veya uygulama yönetimi ve dağıtımı için Batch hizmetinin [uygulama paketleri](nodes-and-pools.md#application-packages) özelliğini kullanabilirsiniz.
3. Bir işlem düğümleri [havuzu](nodes-and-pools.md#pools) oluşturun. Bir havuz oluşturduğunuzda havuz için işlem düğümü sayısını, boyutlarını ve işletim sistemini belirtin. İşinizdeki her bir görev çalıştığında havuzunuzdaki düğümlerden birini yürütmek üzere atanır.
4. [İş](jobs-and-tasks.md#jobs)oluşturun. İş bir görev koleksiyonunu yönetir. Her işi, işin görevlerinin çalışacağı belirli bir havuz ile ilişkilendirin.
5. İşe [Görevler](jobs-and-tasks.md#tasks) ekleyin. Her görev, Storage hesabınızdan indirdiği veri dosyalarını işlemek üzere karşıya yüklediğiniz uygulamayı ve komut dosyasını çalıştırır. Her görev tamamlandığında çıktısını Azure Depolama hizmetine yükleyebilir.
6. İşin ilerleme durumunu izleyin ve görev çıktısını Azure Depolama’dan alın.

> [!NOTE]
> Batch hizmetini kullanmak için bir [Batch hesabı](accounts.md) gereklidir. Ayrıca, Batch çözümlerinin çoğunda dosya depolama ve alma işlemleri için ilişkilendirilmiş bir [Azure Depolama](../storage/index.yml) hesabı kullanılır.

## <a name="batch-service-resources"></a>Batch hizmet kaynakları

Aşağıdaki konularda, dağıtılmış hesaplama senaryolarınızı etkinleştiren toplu Iş kaynakları ele alınmaktadır.

- [Batch hesapları ve depolama hesapları](accounts.md)
- [Düğümler ve havuzlar](nodes-and-pools.md)
- [İşler ve görevler](jobs-and-tasks.md)
- [Dosyalar ve dizinler](files-and-directories.md)

## <a name="next-steps"></a>Sonraki adımlar

- Batch çözümleri oluşturmak için kullanılabilen [Batch API’leri ve araçları](batch-apis-tools.md) hakkında bilgi alın.
- [Batch .NET istemci kitaplığı](quick-run-dotnet.md) veya [Python](quick-run-python.md) kullanarak Batch özellikli bir uygulama geliştirmenin temellerini öğrenin. Bu hızlı başlangıçlar, bir iş yükünü birden fazla işlem düğümünde yürütmek üzere Batch hizmetini kullanan örnek uygulamalar konusunda size rehberlik sağlamanın yanı sıra, iş yükü dosyası hazırlama ve alma işlemleri için Azure Depolama kullanma ile ilgili bilgiler de içerir.
- Batch çözümlerinizi geliştirirken kullanmak üzere [Batch Explorer](https://azure.github.io/BatchExplorer/) uygulamasını indirin ve yükleyin. Batch Explorer; Azure uygulamalarıyla ilgili oluşturma, hata ayıklama ve izleme işlemlerinde yardımcı olabilir.
- [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-batch), [Batch community deposunun](https://github.com/Azure/Batch)ve [Azure Batch forumunu](/answers/topics/azure-batch.html)içeren topluluk kaynaklarına bakın.
