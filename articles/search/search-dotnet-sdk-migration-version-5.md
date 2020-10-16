---
title: Azure Search .NET SDK sürüm 5 ' e yükseltme
titleSuffix: Azure Cognitive Search
description: Kodu eski sürümlerden Azure Search .NET SDK sürüm 5 ' e geçirin. Nelerin yeni olduğunu ve hangi kod değişikliklerinin gerekli olduğunu öğrenin.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 7f0e994cd41fc55824cf347a51ccfed25589a310
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91539546"
---
# <a name="upgrade-to-azure-search-net-sdk-version-5"></a>Azure Search .NET SDK sürüm 5 ' e yükseltme

[.NET SDK 'sının](/dotnet/api/overview/azure/search)4,0-Preview veya daha eski bir sürümünü kullanıyorsanız, bu makale uygulamanızı sürüm 5 ' i kullanacak şekilde yükseltmenize yardımcı olur.

Örnek içeren SDK hakkında daha genel bir anlatım için bkz. [.NET uygulamasından Azure Search kullanma](search-howto-dotnet-sdk.md).

Azure Search .NET SDK 'sının 5. sürümü önceki sürümlerden bazı değişiklikler içerir. Bunlar çoğunlukla düşüktür, bu nedenle kodunuzun değiştirilmesi yalnızca en az çaba gerektirir. Kodunuzu yeni SDK sürümünü kullanacak şekilde değiştirme hakkında yönergeler için bkz. [yükseltme adımları](#UpgradeSteps) .

> [!NOTE]
> Sürüm 2,0-Önizleme veya daha eski bir sürümü kullanıyorsanız, önce sürüm 3 ' e yükseltmeniz ve ardından 5. sürüme yükseltmeniz gerekir. Yönergeler için bkz. [.NET SDK sürüm 3 Azure Search yükseltme](search-dotnet-sdk-migration.md) .
>
> Azure Search hizmet örneğiniz, en son sürüm dahil olmak üzere birkaç REST API sürümü destekler. Artık en son bir sürüm olmadığında bir sürümü kullanmaya devam edebilirsiniz, ancak kodunuzu en yeni sürümü kullanmak için geçirmeniz önerilir. REST API kullanırken, API sürümü parametresi aracılığıyla her istekte API sürümünü belirtmeniz gerekir. .NET SDK kullanıldığında, kullanmakta olduğunuz SDK sürümü REST API ilgili sürümünü belirler. Daha eski bir SDK kullanıyorsanız, hizmet daha yeni bir API sürümünü destekleyecek şekilde yükseltilse bile, bu kodu hiçbir değişiklik yapmadan çalıştırmaya devam edebilirsiniz.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-5"></a>Sürüm 5 ' teki yenilikler
Azure Search .NET SDK 'nın 5. sürümü, Azure Search REST API, özellikle 2017-11-11 olan en son genel kullanıma sunulan sürümü hedefler. Bu, aşağıdakiler de dahil olmak üzere bir .NET uygulamasından Azure Search yeni özelliklerini kullanmayı mümkün kılar:

* [Eş anlamlılar](search-synonyms.md).
* Artık, Dizin Oluşturucu yürütme geçmişinde uyarılara programlı olarak erişebilirsiniz ( `Warning` `IndexerExecutionResult` daha fazla ayrıntı için bkz. [.net Reference](/dotnet/api/microsoft.azure.search.models.indexerexecutionresult) içindeki özelliği).
* .NET Core 2 desteği.
* Yeni paket yapısı yalnızca ihtiyacınız olan SDK bölümlerinin kullanılmasını destekler (Ayrıntılar için [sürüm 5 ' teki son değişikliklere](#ListOfChanges) bakın).

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Yükseltme adımları
İlk olarak, NuGet ' i `Microsoft.Azure.Search` Paket Yöneticisi konsolu 'nu kullanarak veya proje başvurularınızı sağ tıklatıp "NuGet Paketlerini Yönet..." seçeneğini belirleyerek NuGet başvurunuz ' ı güncelleştirin. Visual Studio 'da.

NuGet yeni paketleri ve bağımlılıklarını indirdikten sonra projenizi yeniden derleyin. Kodunuzun nasıl yapılandırıldığına bağlı olarak, başarıyla yeniden oluşturulabilir. Öyleyse başlamaya hazırsınız demektir!

Derlemeniz başarısız olursa, aşağıdaki gibi bir yapı hatası görmeniz gerekir:

```output
The name 'SuggesterSearchMode' does not exist in the current context
```

Sonraki adım bu derleme hatasını düzeltemedi. Hataya neden olan ve nasıl düzeltileceğini gösteren Ayrıntılar için [sürüm 5 ' teki son değişikliklere](#ListOfChanges) bakın.

Azure Search .NET SDK 'sının paketlenmesi nedeniyle, sürüm 5 ' i kullanabilmeniz için uygulamanızı yeniden oluşturmanız gerektiğini lütfen unutmayın. Bu değişiklikler [sürüm 5 ' teki önemli değişikliklere](#ListOfChanges)göre ayrıntılıdır.

Eski yöntemler veya özelliklerle ilgili ek derleme uyarıları görebilirsiniz. Uyarılar, kullanım dışı özellik yerine, nelerin kullanılacağı hakkında yönergeler içerir. Örneğin, uygulamanız `IndexingParametersExtensions.DoNotFailOnUnsupportedContentType` yöntemini kullanıyorsa, "Bu davranış artık varsayılan olarak etkindir, bu nedenle bu yöntemin çağrılması artık gerekli değildir." ifadesini bildiren bir uyarı almalısınız.

Herhangi bir derleme hatasını veya uyarıyı düzelttikten sonra, isterseniz yeni işlevlerden yararlanmak için uygulamanızda değişiklikler yapabilirsiniz. SDK 'daki yeni özellikler [sürüm 5 ' teki](#WhatsNew)Yenilikler bölümünde ayrıntılı olarak açıklanmıştır.

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-5"></a>Sürüm 5 ' teki son değişiklikler

### <a name="new-package-structure"></a>Yeni paket yapısı

Sürüm 5 ' teki en önemli önemli değişiklik, `Microsoft.Azure.Search` derlemenin ve içeriğinin artık dört ayrı NuGet paketi olarak dağıtılan dört ayrı derlemeye bölündüğü gibidir:

 - `Microsoft.Azure.Search`: Bu, tüm diğer Azure Search paketlerini bağımlılık olarak içeren bir meta pakettir. SDK 'nın önceki bir sürümünden yükseltiyorsanız, bu paketin yükseltilmesi ve yeniden oluşturulması yeni sürümü kullanmaya başlamak için yeterli olmalıdır.
 - `Microsoft.Azure.Search.Data`: Azure Search kullanarak bir .NET uygulaması geliştiriyorsanız bu paketi kullanın ve yalnızca dizininizdeki belgeleri sorgulayıp güncelleştirmeniz yeterlidir. Ayrıca dizinleri, eş anlamlı haritaları veya diğer hizmet düzeyi kaynaklarını oluşturmanız ya da güncelleştirmeniz gerekiyorsa, `Microsoft.Azure.Search` bunun yerine paketini kullanın.
 - `Microsoft.Azure.Search.Service`: Azure Search dizinleri, eş anlamlı haritaları, Dizin oluşturucuyu, veri kaynaklarını veya diğer hizmet düzeyi kaynakları yönetmek için .NET 'te Otomasyon geliştiriyorsanız bu paketi kullanın. Dizininizdeki belgeleri sorgulamak veya güncelleştirmek istiyorsanız, `Microsoft.Azure.Search.Data` bunun yerine paketini kullanın. Tüm Azure Search işlevselliğine ihtiyacınız varsa, `Microsoft.Azure.Search` bunun yerine paketini kullanın.
 - `Microsoft.Azure.Search.Common`: Azure Search .NET kitaplıkları için gereken ortak türler. Bu paketi uygulamanızda doğrudan kullanmanız gerekmez; Yalnızca bir bağımlılık olarak kullanılmak üzere tasarlanmıştır.
 
Bu değişiklik, derlemeler arasında çok sayıda tür taşındığı için teknik olarak kesiliyor. Bu, SDK sürüm 5 ' e yükseltmek için uygulamanızı yeniden derleme işlemi için gereklidir.

Sürüm 5 ' te, uygulamanızı yeniden oluşturmak için ek olarak kod değişikliği gerektirebilecek daha az sayıda diğer Son değişiklik vardır.

### <a name="change-to-suggesters"></a>Öneri araçları olarak değiştir 

`Suggester`Oluşturucunun `enum` için parametresi artık yoktur `SuggesterSearchMode` . Bu Enum yalnızca bir değer içeriyordu ve bu nedenle yedekli. Bunun sonucunda derleme hataları görürseniz, yalnızca parametreye başvuruları kaldırın `SuggesterSearchMode` .

### <a name="removed-obsolete-members"></a>Artık kullanılmayan Üyeler kaldırıldı

Önceki sürümlerde eski olarak işaretlenen ve daha sonra sürüm 5 ' te kaldırılan yöntemlerle veya özelliklerle ilgili derleme hataları görebilirsiniz. Bu tür hatalarla karşılaşırsanız, bu sorunları çözmek için aşağıdaki adımları uygulayın:

- `IndexingParametersExtensions.IndexStorageMetadataOnly`Yöntemini kullanıyorsanız `SetBlobExtractionMode(BlobExtractionMode.StorageMetadata)` bunun yerine kullanın.
- `IndexingParametersExtensions.SkipContent`Yöntemini kullanıyorsanız `SetBlobExtractionMode(BlobExtractionMode.AllMetadata)` bunun yerine kullanın.

### <a name="removed-preview-features"></a>Önizleme özellikleri kaldırıldı

Sürüm 4,0 ' den sürüm 5 ' e yükseltiyorsanız, bu özellikler hala önizlemede olduğundan blob Dizin oluşturucular için JSON dizisinin ve CSV ayrıştırma desteğinin kaldırıldığını unutmayın. Özellikle, sınıfının aşağıdaki yöntemleri `IndexingParametersExtensions` kaldırılmıştır:

- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

Uygulamanızın bu özelliklere sabit bir bağımlılığı varsa, Azure Search .NET SDK 'sının 5 sürümüne yükseltemezsiniz. 4,0-Preview sürümünü kullanmaya devam edebilirsiniz. Ancak, lütfen **Üretim uygulamalarında önizleme SDK 'larını kullanmanızı önermiyoruz**. Önizleme özellikleri yalnızca değerlendirme amaçlıdır ve değişebilir.

## <a name="conclusion"></a>Sonuç
Azure Search .NET SDK 'yı kullanma hakkında daha fazla ayrıntıya ihtiyacınız varsa bkz. [.NET nasıl yapılır](search-howto-dotnet-sdk.md).

SDK 'daki geri bildirimlerinize hoş geldiniz. Sorunlarla karşılaşırsanız [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-search)hakkında yardım almak için bize danışabilirsiniz. Bir hata bulursanız, [Azure .NET SDK GitHub deposunda](https://github.com/Azure/azure-sdk-for-net/issues)bir sorun oluşturabilirsiniz. Sorun başlığınız "[Azure Search]" ile öneklediğinizden emin olun.

Azure Search kullandığınız için teşekkürler!