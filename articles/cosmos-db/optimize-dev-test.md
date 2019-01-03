---
title: Azure Cosmos DB'de test ve geliştirme için en iyi duruma getirme
description: Bu makale, Azure Cosmos DB geliştirme ve hizmet ücretsiz test için birden çok seçenek nasıl sağladığını açıklar.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/20/2018
ms.author: rimman
ms.openlocfilehash: d3ed5c2c13e5a35bcc6cd0e03a6b553691785cdb
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53548672"
---
# <a name="optimizing-for-development-and-testing-in-azure-cosmos-db"></a>Azure Cosmos DB'de test ve geliştirme için en iyi duruma getirme

Bu makalede, Azure Cosmos DB geliştirme ve ücretsiz maliyet test için kullanılacak farklı seçenekler açıklanır.

## <a name="azure-cosmos-db-emulator-locally-downloadable-version"></a>Azure Cosmos DB öykünücüsü'nü (yerel olarak indirilebilir sürümü)

[Azure Cosmos DB öykünücüsü'nü](local-emulator.md) Azure Cosmos DB bulut hizmeti taklit eden yerel bir indirilebilen sürümüdür. Yazma ve bile hiçbir ağ bağlantısı olması ve masraf olmadan Azure Cosmos DB API'leri kullanan kodu test edebilirsiniz. Azure Cosmos DB öykünücüsü'nü bulut hizmeti yüksek uygunluğa sahip geliştirme amacıyla yerel bir ortam sağlar. Geliştirin ve bir Azure aboneliği oluşturmadan uygulamanızı yerel olarak test edin. Uygulamanızı buluta dağıtmak, bulutta Azure Cosmos DB uç noktasına bağlanmak için bağlantı dizesini güncelleştirme hazır olduğunuzda, herhangi bir değişiklik gereklidir. Ayrıca [bir CI/CD işlem hattı ile Azure Cosmos DB öykünücüsü'nü ayarlama](tutorial-setup-ci-cd.md) Azure DevOps, testleri çalıştırmak için görev oluşturun. Ziyaret ederek oluşturabileceğinize dair [Azure Cosmos DB öykünücüsü'nü](local-emulator.md) makalesi.

## <a name="try-azure-cosmos-db-for-free"></a>Azure Cosmos DB’yi ücretsiz deneyin

[Azure Cosmos DB'yi ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/) ücretsiz bir veritabanı ve koleksiyon oluşturma ve Azure Cosmos DB bulutta denemenizi olanak tanıyan ücretsiz olarak deneyim. Kayıt için Azure için veya yoksa herhangi bir maliyet ödersiniz. Try Azure Cosmos DB hesapları, şu anda 30 gün gibi sınırlı bir süre için kullanılabilir. Bunları istediğiniz zaman yenileyebilirsiniz. Azure Cosmos DB'yi deneyin hesapları Azure Cosmos DB değerlendirmek, derleme ve hızlı başlangıçlarını veya öğreticilerini kullanarak bir uygulamayı test yapmayı kolaylaştırır. Tanıtım oluşturma veya birim masraf olmadan testi gerçekleştirin. Kullanarak Azure Cosmos DB'yi ücretsiz hesapları için deneyin, anahtar teslim küresel dağıtım, SLA'ları ve tutarlılık modelleri de dahil olmak üzere ücretsiz, Azure Cosmos DB'nin premium özelliklerini değerlendirebilir. En fazla 25 Azure Cosmos kapsayıcılar ve 10.000 RU/sn aktarım hızı ile bir veritabanı oluşturabilirsiniz. Bir Azure hesabına abone olma ya da kredi kartınızı kullanarak olmadan, örnek uygulamayı çalıştırabilirsiniz. Azure Cosmos DB ile deneyin ücretsiz olarak, çok bölgeli bir Azure Cosmos hesabı oluşturabilir ve üzerinde yalnızca birkaç dakika içinde bir uygulamayı çalıştırın. Başlamak için bkz: [Azure Cosmos DB'yi ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/) sayfası.

## <a name="azure-free-account"></a>Azure Ücretsiz Hesabı

Azure Cosmos DB dahil [ücretsiz Azure hesabı](https://azure.microsoft.com/free), sunan Azure KREDİLERİ ve kaynaklarına ücretsiz belirli bir zaman aralığı için. Azure Cosmos DB için özellikle, bu ücretsiz hesaba yılın tamamı için 5 GB'lık depolama ve sağlanan aktarım hızı 400 Ru'ya sunar. Bu deneyim, kolayca Azure Cosmos DB özelliklerini test etmek veya sıfır maliyetle diğer Azure hizmetleriyle tümleştirme herhangi bir geliştirici sağlar. Azure ücretsiz hesabı ile ilk 30 gün içinde harcayabileceğiniz 200 ABD Doları değerinde kredi alın. Yükseltene kadar hizmetleri kullanmaya olsa bile, ücret ödemezsiniz. Başlamak için ziyaret [ücretsiz Azure hesabı](https://azure.microsoft.com/free) sayfası.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleleri ile öykünücü veya ücretsiz Azure Cosmos DB hesapları kullanmaya başlayabilirsiniz:

* Daha fazla bilgi edinin [en iyi duruma getirme için geliştirme ve test etme](optimize-dev-test.md)
* Daha fazla bilgi edinin [Azure Cosmos DB faturanızı anlama](understand-your-bill.md)
* Daha fazla bilgi edinin [aktarım hızı maliyeti en iyi duruma getirme](optimize-cost-throughput.md)
* Daha fazla bilgi edinin [depolama maliyetini en iyi duruma getirme](optimize-cost-storage.md)
* Daha fazla bilgi edinin [okuma ve yazma işlemleri maliyetini en iyi duruma getirme](optimize-cost-reads-writes.md)
* Daha fazla bilgi edinin [sorguları maliyetini en iyi duruma getirme](optimize-cost-queries.md)
* Daha fazla bilgi edinin [çok bölgeli Azure Cosmos hesapları maliyetini en iyi duruma getirme](optimize-cost-regions.md)

