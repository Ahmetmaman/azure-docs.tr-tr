---
title: Geliştiriciler için API 'Ler ve araçlar
description: Azure Batch hizmeti ile çözüm geliştirmek için kullanılabilen API’ler ve araçlar hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: e345e91b2f7d66f014427770614efe42b5fb7a44
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82232692"
---
# <a name="overview-of-batch-apis-and-tools"></a>Batch API'lerine ve araçlarına genel bakış

Azure Batch ile paralel iş yükleri genellikle Batch API'lerinden biri kullanılarak programlı bir şekilde işlenir. İstemci uygulamanız veya hizmetiniz, Batch hizmetiyle iletişim kurmak için Batch API'lerini kullanabilir. Batch API'leriyle, işlem düğümü havuzları (sanal makineler veya bulut hizmetleri) oluşturabilir veya yönetebilirsiniz. Ardından bu düğümlerde çalıştırılacak işleri ve görevleri zamanlayabilirsiniz. 

Kuruluşunuz için büyük ölçekli iş yüklerini verimli bir şekilde işleyebilir ya da müşterilerinizin tek, yüzlerce ve hatta binlerce düğümde istek üzerine veya planlı olarak işleri ve görevleri çalıştırabileceği şekilde onlara bir hizmet ön ucu sağlayabilirsiniz. [Azure Data Factory](../data-factory/transform-data-using-dotnet-custom-activity.md?toc=%2fazure%2fbatch%2ftoc.json) gibi araçlarla yönetilen Azure Batch'i daha büyük bir iş akışının parçası olarak da kullanabilirsiniz.

> [!TIP]
> Sağladığı özellikleri daha derinlemesine anlamak amacıyla Batch API'sinin ayrıntılarına gitmeye hazır olduğunuzda, [Geliştiriciler için Batch özelliklerine genel bakış](batch-api-basics.md) konusunu inceleyin.
> 
> 

## <a name="azure-accounts-for-batch-development"></a>Batch geliştirme için Azure hesapları
Batch çözümleri geliştirdiğinizde, Azure aboneliğinizdeki aşağıdaki hesapları kullanacaksınız:

* **Batch hesabı** - Havuzlar, işlem düğümleri, işler ve görevler gibi bir Azure [Batch hesabıyla](batch-api-basics.md#account) ilişkilendirilmiş Azure Batch kaynakları. Uygulamanız, Batch hizmetinden bir istekte bulunduğunda istek Azure Batch hesabı adı, hesabın URL'si ve erişim anahtarı veya Azure Active Directory belirteci kullanılarak kimlik doğrulamasından geçirilir. Azure portalından veya kod kullanarak [Batch hesabı oluşturabilirsiniz](batch-account-create-portal.md).
* **Depolama hesabı** - Batch’te [Azure Depolama][azure_storage] dosyalarıyla çalışmak için yerleşik destek bulunur. Neredeyse tüm Batch senaryolarında, görevlerinizin çalıştırdığı programların ve işlediği verilerin hazırlanmasının yanı sıra oluşturduğu çıktı verilerinin depolanması için Azure Blob depolama kullanılır. Batch’teki depolama hesabı seçenekleri için bkz. [Batch özelliğine genel bakış](batch-api-basics.md#azure-storage-account).

## <a name="batch-service-apis"></a>Batch hizmeti API’leri

Uygulamalarınız ve hizmetleriniz doğrudan REST API çağrıları kullanabilir veya Azure Batch iş yüklerinizi çalıştırmak ya da yönetmek için aşağıdaki istemci kitaplıklardan birini veya daha fazlasını kullanabilir.

| API | API başvurusu | İndirme | Eğitmen | Kod örnekleri | Daha Fazla Bilgi |
| --- | --- | --- | --- | --- | --- |
| **Batch REST** |[docs.microsoft.com][batch_rest] |Yok |- |- | [Desteklenen sürümler](/rest/api/batchservice/batch-service-rest-api-versioning) |
| **Batch .NET** |[docs.microsoft.com][api_net] |[NuGet][api_net_nuget] |[Eğitmen](tutorial-parallel-dotnet.md) |[GitHub][api_sample_net] | [Sürüm notları](https://aka.ms/batch-net-dataplane-changelog) |
| **Batch Python** |[docs.microsoft.com][api_python] |[PyPI][api_python_pypi] |[Eğitmen](tutorial-parallel-python.md)|[GitHub][api_sample_python] | [Benioku](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/batch/azure-batch/README.md) |
| **Batch Node.js** |[docs.microsoft.com][api_nodejs] |[NPM][api_nodejs_npm] |[Eğitmen](batch-nodejs-get-started.md) |- | [Benioku](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/batch) |
| **Batch Java** |[docs.microsoft.com][api_java] |[Maven][api_java_jar] |- |[Benioku][api_sample_java] | [Benioku](https://github.com/Azure/azure-batch-sdk-for-java)|

## <a name="batch-management-apis"></a>Batch Yönetimi API’leri

Batch için Azure Resource Manager API'leri, Batch hesaplarına programlı erişim sağlar. Bu API’leri kullanarak Batch hesaplarını, kotaları, uygulama paketlerini ve diğer kaynakları Microsoft.Batch sağlayıcısı üzerinden programlı bir şekilde yönetebilirsiniz.  

| API | API başvurusu | İndirme | Eğitmen | Kod örnekleri |
| --- | --- | --- | --- | --- |
| **Batch Yönetimi REST** |[docs.microsoft.com][api_rest_mgmt] |Yok |- |[GitHub](https://github.com/Azure-Samples/batch-dotnet-manage-batch-accounts) |
| **Batch Yönetimi .NET** |[docs.microsoft.com][api_net_mgmt] |[NuGet][api_net_mgmt_nuget] | [Eğitmen](batch-management-dotnet.md) |[GitHub][api_sample_net] |
| **Batch Yönetimi Python** |[docs.microsoft.com][api_python_mgmt] |[PyPI][api_python_mgmt_pypi] |- |- |
| **Batch Yönetimi Node.js** |[docs.microsoft.com][api_nodejs_mgmt] |[NPM][api_nodejs_mgmt_npm] |- |- | 
| **Batch Yönetimi Java** |- |[Maven][api_java_mgmt_jar] |- |- |
## <a name="batch-command-line-tools"></a>Batch komut satırı araçları

Bu komut satırı araçları, Batch hizmeti ve Batch Yönetimi API'leri ile aynı işlevi sağlar: 

* [Batch PowerShell cmdlet'leri][batch_ps]: [Azure PowerShell](/powershell/azure/overview) modülündeki Azure Batch cmdlet'leri PowerShell ile Batch kaynaklarını yönetmenizi sağlar.
* [Azure CLI](/cli/azure): Azure CLI, Batch hizmeti ve Batch Yönetimi hizmeti de dahil olmak üzere çok sayıda Azure hizmetiyle etkileşim için kabuk komutları sağlayan, platformlar arası bir araç takımıdır. Azure CLI’yı Batch ile birlikte kullanma hakkında daha fazla bilgi için bkz. [Azure CLI ile Batch kaynaklarını yönetme](batch-cli-get-started.md).

## <a name="other-tools-for-application-development"></a>Uygulama geliştirme için diğer araçlar

Batch uygulamalarınızı ve hizmetlerinizi oluşturmak ve bunlarda hata ayıklamak için yararlı olabilecek bazı ek araçlar şunlardır:

* [Azure portalı][portal]: Azure portalında Batch havuzlarını, işlerini ve görevlerini oluşturabilir, izleyebilir ve silebilirsiniz. Bu ve diğer kaynakların durum bilgilerini, işlerinizi çalıştırırken görüntüleyebilir, hatta havuzlarınızdaki işlem düğümlerinden dosya indirebilirsiniz. Örneğin, sorun giderme sırasında başarısız bir görevin `stderr.txt` öğesini indirebilirsiniz. İşlem düğümlerinde oturum açmak için kullanabileceğiniz Uzak Masaüstü (RDP) dosyalarını da indirebilirsiniz.
* [Azure Batch Explorer][batch_labs]: Batch Explorer (eski adı BatchLabs), Azure Batch uygulamalarıyla ilgili oluşturma, hata ayıklama ve izleme işlemlerini gerçekleştirmenize yardımcı olan ücretsiz, gelişmiş özelliklere sahip ve tek başına kullanılan bir istemci aracıdır. Mac, Linux veya Windows için [yükleme paketi](https://azure.github.io/BatchExplorer/) indirebilirsiniz.
* [Shipbahçe Azure Batch](https://github.com/Azure/batch-shipyard): Batch shipbahçe, Azure Batch üzerinde kapsayıcı tabanlı toplu Işleme ve HPC iş yüklerini sağlamaya, yürütmeye ve izlemeye yardımcı olan bir araçtır.
* [Azure Depolama Gezgini][storage_explorer]: kesinlikle Azure Batch bir araç olmadığı sürece Depolama Gezgini, Batch çözümlerinizi geliştirirken ve hata ayıklamanıza çalışırken sahip olacak başka bir değerli araçtır.

## <a name="additional-resources"></a>Ek kaynaklar

- Batch uygulamanızdaki olayları günlüğe kaydetme hakkında bilgi edinmek için bkz. [Batch çözümlerine tanılama değerlendirmesi yapmak ve Batch çözümlerini izlemek üzere olayları günlüğe kaydetme](batch-diagnostics.md). Batch hizmeti tarafından oluşturulan olaylarla ilgili bir başvuru için bkz. [Batch Analizi](batch-analytics.md).
- İşlem düğümlerine yönelik ortam değişkenleri hakkında bilgi için bkz. [Azure Batch işlem düğümü ortam değişkenleri](batch-compute-node-environment-variables.md).

## <a name="next-steps"></a>Sonraki adımlar

* Batch kullanmaya hazırlanan herkes için gerekli bilgileri içeren [Geliştiriciler için Batch özelliğine genel bakış](batch-api-basics.md) konusunu okuyun. Bu makalede havuzlar, düğümler, işler ve görevler gibi Batch hizmet kaynakları ve Batch uygulamanızı oluştururken kullanabileceğiniz birçok API özelliği hakkında daha ayrıntılı bilgi verilmektedir.
* Genel bir Batch iş akışını kullanarak basit bir iş yükü yürütmek üzere C# ve Batch .NET kitaplığını kullanma hakkında bilgi için bkz. [.NET için Azure Batch kitaplığını kullanmaya başlama](tutorial-parallel-dotnet.md). Bu öğreticinin [Python sürümü](tutorial-parallel-python.md) ve [Node.js öğreticisi](batch-nodejs-get-started.md) de vardır.
* Hem C# hem de Python'un örnek iş yüklerini zamanlamak ve işlemek üzere Batch ile arabirim oluşturmasını görmek için [GitHub'daki kod örneklerini][github_samples] indirin.

[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: /java/api/overview/azure/batch
[api_java_mgmt]: /java/api/overview/azure/batch/managementapi
[api_java_jar]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_java_mgmt_jar]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: /dotnet/api/overview/azure/batch/
[api_net_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Batch/
[api_rest_mgmt]: /rest/api/batchmanagement/
[api_net_mgmt]: /dotnet/api/overview/azure/batch/management
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: /javascript/api/overview/azure/batch/client
[api_nodejs_mgmt]: /javascript/api/overview/azure/batch/management
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_nodejs_mgmt_npm]: https://www.npmjs.com/package/azure-arm-batch
[api_python]: /python/api/overview/azure/batch/client
[api_python_mgmt]: /python/api/overview/azure/batch/management
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_python_mgmt_pypi]: https://pypi.python.org/pypi/azure-mgmt-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: /powershell/module/az.batch/
[batch_rest]: /rest/api/batchservice/
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_labs]: https://azure.github.io/BatchExplorer/
[storage_explorer]: https://storageexplorer.com/
[portal]: https://portal.azure.com
