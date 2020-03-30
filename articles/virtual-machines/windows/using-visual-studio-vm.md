---
title: Visual Studio'yı Azure sanal makinesinde kullanma
description: Visual Studio'yi Azure sanal makinede kullanma.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cathysull
manager: cathys
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.date: 12/04/2019
ms.author: cathys
keywords: visualstudio
ms.openlocfilehash: 2977fb6f14468429eb651e4f8a6034c060cdee2f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75895938"
---
# <a name="visual-studio-images-on-azure"></a>Azure'da Visual Studio görüntüleri
Visual Studio'nun önceden yapılandırılmış bir Azure sanal makinesinde (VM) kullanılması, hiçbir şeyden güncel bir geliştirme ortamına geçmenin hızlı ve kolay bir yoludur. [Azure](https://azuremarketplace.microsoft.com/marketplace/apps/category/compute?filters=virtual-machine-images%3Bmicrosoft%3Bwindows&page=1&subcategories=application-infrastructure)Marketi'nde farklı Visual Studio yapılandırmalarına sahip sistem görüntüleri mevcuttur.

Azure’da yeni misiniz? [Ücretsiz bir Azure hesabı oluşturun](https://azure.microsoft.com/free).

> [!NOTE]
> Tüm abonelikler Windows 10 resimlerini dağıtmaya uygun değildir. Daha fazla bilgi için [geliştirme/test senaryoları için Azure'da Windows istemcisi kullan](https://docs.microsoft.com/azure/virtual-machines/windows/client-images)

## <a name="what-configurations-and-versions-are-available"></a>Hangi yapılandırmalar ve sürümler kullanılabilir?
En son ana sürümler olan Visual Studio 2019, Visual Studio 2017 ve Visual Studio 2015'e ait görseller Azure Marketi'nde bulunabilir.  Yayımlanan her ana sürüm için, başlangıçta "web'e yayımlanan" (RTW) sürümünü ve en son güncelleştirilmiş sürümleri görürsünüz.  Bu sürümlerin her biri Visual Studio Enterprise ve Visual Studio Community sürümlerini sunar.  Bu görüntüler en son Visual Studio ve Windows güncelleştirmelerini içerecek şekilde en az her ay güncellenir.  Görüntülerin adları aynı kalırken, her görüntünün açıklaması yüklü ürün sürümünü ve görüntünün "itibariyle" tarihini içerir.

| Sürüm sürümü                                                                                                                                                | Sürümler              | Ürün sürümü   |
|:--------------------------------------------------------------------------------------------------------------------------------------------------------------:|:---------------------:|:-----------------:|
| [Visual Studio 2019: En Son (Sürüm 16.4)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftvisualstudio.visualstudio2019latest?tab=Overview) | Kurumsal, Topluluk | Sürüm 16.4.0    |
| [Visual Studio 2019: RTW](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftvisualstudio.visualstudio2019?tab=Overview)                         | Kurumsal, Topluluk | Sürüm 16.0.9    |
| [Visual Studio 2017: En Son (Sürüm 15.9)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftvisualstudio.visualstudio?tab=Overview)           | Kurumsal, Topluluk | Sürüm 15.9.17   |
| [Visual Studio 2017: RTW](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftvisualstudio.visualstudio?tab=Overview)                             | Kurumsal, Topluluk | Sürüm 15.0.27  |
| [Visual Studio 2015: En Son (Güncelleme 3)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftvisualstudio.visualstudio?tab=Overview)               | Kurumsal, Topluluk | Sürüm 14.0.25431.01 |

> [!NOTE]
> Microsoft hizmet ilkesine uygun olarak, Visual Studio 2015'in ilk yayımlanan (RTW) sürümünün hizmet süresi doldu. Visual Studio 2015 Update 3, Visual Studio 2015 ürün serisi için sunulan tek kalan sürümdür.

Daha fazla bilgi için [Visual Studio Servis Politikası'na](https://www.visualstudio.com/productinfo/vs-servicing-vs)bakın.

## <a name="what-features-are-installed"></a>Hangi özellikler yüklenir?
Her görüntü, Visual Studio sürümü için önerilen özellik kümesini içerir. Genellikle, yükleme içerir:

* Her iş yükünün önerilen isteğe bağlı bileşenleri de dahil olmak üzere tüm kullanılabilir iş yükleri
* .NET 4.6.2 ve .NET 4.7 SDK'lar, Hedefleme Paketleri ve Geliştirici Araçları
* Visual F#
* Visual Studio için GitHub Uzantısı
* LINQ 'dan SQL Araçlarına

Görüntüleri yaparken Visual Studio'yu yüklemek için kullanılan komut satırı aşağıdaki gibidir:

```
    vs_enterprise.exe --allWorkloads --includeRecommended --passive ^
       add Microsoft.Net.Component.4.7.SDK ^
       add Microsoft.Net.Component.4.7.TargetingPack ^ 
       add Microsoft.Net.Component.4.6.2.SDK ^
       add Microsoft.Net.Component.4.6.2.TargetingPack ^
       add Microsoft.Net.ComponentGroup.4.7.DeveloperTools ^
       add Microsoft.VisualStudio.Component.FSharp ^
       add Component.GitHub.VisualStudio ^
       add Microsoft.VisualStudio.Component.LinqToSql
```

Görüntüler de gereksinim duyduğunuz bir Visual Studio özelliği içermiyorsa, sayfanın sağ üst köşesindeki geri bildirim aracı aracılığıyla geri bildirim sağlayın.

## <a name="what-size-vm-should-i-choose"></a>VM boyutunu seçmeliyim?
Azure, çok çeşitli sanal makine boyutları sunar. Visual Studio güçlü, çok iş parçacığı uygulaması olduğundan, en az iki işlemci ve 7 GB bellek içeren bir VM boyutu istiyorsunuz. Visual Studio görüntüleri için aşağıdaki VM boyutlarını öneririz:

   * Standard_D2_v3
   * Standard_D2s_v3
   * Standard_D4_v3
   * Standard_D4s_v3
   * Standard_D2_v2
   * Standard_D2S_v2
   * Standard_D3_v2
    
En son makine boyutları hakkında daha fazla bilgi için [Azure'daki Windows sanal makineleri için Boyutlar'a](/azure/virtual-machines/windows/sizes)bakın.

Azure ile, VM'yi yeniden boyutlandırma yaparak ilk seçiminizi yeniden dengeleyebilirsiniz. Yeni bir VM'yi daha uygun bir boyuta sağlayabilir veya varolan VM'nizi farklı temel donanıma yeniden boyutlandırabilirsiniz. Daha fazla bilgi için windows [vm yeniden boyutlandırma'ya](/azure/virtual-machines/windows/resize-vm)bakın.

## <a name="after-the-vm-is-running-whats-next"></a>VM çalışmaya devam ettikten sonra sırada ne var?
Visual Studio, Azure'da "kendi lisansınızı getirin" modelini izler. Tescilli donanımyüklemede olduğu gibi, ilk adımlardan biri Visual Studio yüklemenizi lisanslamanızdır. Visual Studio'nun kilidini açmak için:
- Visual Studio aboneliğiyle ilişkili bir Microsoft hesabıyla oturum açın 
- İlk satın alma işleminizle birlikte gelen ürün anahtarıyla Visual Studio'nun kilidini açın

Daha fazla bilgi için Visual [Studio'da Oturum Aç](/visualstudio/ide/signing-in-to-visual-studio) ve [Visual Studio'nun kilidini nasıl açacağına](/visualstudio/ide/how-to-unlock-visual-studio)bakın.

## <a name="how-do-i-save-the-development-vm-for-future-or-team-use"></a>Geliştirme VM'sini gelecekteki veya takım kullanımı için nasıl kaydedebilirim?

Geliştirme ortamlarının spektrumu çok büyüktür ve daha karmaşık ortamların oluşturulmasıyla ilgili gerçek bir maliyet vardır. Ortamınızın yapılandırması ne olursa olsun, yapılandırılmış VM'inizi gelecekteki kullanımiçin bir "temel görüntü" olarak veya ekibinizin diğer üyeleri için kaydedebilir veya yakalayabilirsiniz. Ardından, yeni bir VM önyükleme yaparken, azure marketi görüntüsü yerine temel görüntüden sağlarsınız.

Hızlı bir özet: Sistem Hazırlama aracını (Sysprep) kullanın ve çalışan VM'yi kapatın ve ardından *(Şekil 1)* VM'yi Azure portalında UI üzerinden görüntü olarak yakalayın. Azure, seçtiğiniz `.vhd` depolama hesabında görüntüyü içeren dosyayı kaydeder. Yeni resim daha sonra aboneliğinizin kaynak listesinde bir Resim kaynağı olarak gösterilmektedir.

<img src="media/using-visual-studio-vm/capture-vm.png" alt="Capture an image through the Azure portal UI" style="border:3px solid Silver; display: block; margin: auto;"><center>*(Şekil 1) Azure portal ui üzerinden bir görüntü yakalayın.*</center>

Daha fazla bilgi için bkz. [Azure'da genelleştirilmiş bir VM'nin yönetilen görüntüsünü oluştur.](/azure/virtual-machines/windows/capture-image-resource)

> [!IMPORTANT]
> VM hazırlamak için Sysprep kullanmayı unutmayın. Bu adımı kaçırırsanız, Azure görüntüden bir VM sağlayamaz.

> [!NOTE]
> Görüntülerin depolanması için hala bazı maliyetlere maruz kaldığınız için, bu artımlı maliyet, ihtiyacı olan her takım üyesi için VM'yi sıfırdan yeniden oluşturmak için genel gider maliyetleriyle karşılaştırıldığında önemsiz olabilir. Örneğin, 127 GB'lık bir görüntüyü tüm ekibiniz tarafından yeniden kullanılabilir bir ay boyunca oluşturmak ve depolamak birkaç dolara mal olur. Ancak, bu maliyetler, her çalışanın kendi kullanımı için düzgün yapılandırılmış bir dev kutusu oluşturmak ve doğrulamak için yatırım saatleriyle karşılaştırıldığında önemsizdir.

Ayrıca, geliştirme görevlerinizin veya teknolojilerinizin geliştirme yapılandırmaları çeşitleri ve birden çok makine yapılandırması gibi daha fazla ölçek gerekebilir. Azure DevTest Labs'ı kullanarak "altın görüntünüzün" oluşturulmasını otomatikleştiren _tarifler_ oluşturabilirsiniz. Ekibinizin çalıştıran VM'leri için ilkeleri yönetmek için DevTest Labs'ı da kullanabilirsiniz. [Geliştiriciler için Azure DevTest Labs'ı kullanmak,](/azure/devtest-lab/devtest-lab-developer-lab) DevTest Labs hakkında daha fazla bilgi için en iyi kaynaktır.

## <a name="next-steps"></a>Sonraki adımlar
Artık önceden yapılandırılmış Visual Studio görüntülerini bildiğinize göre, bir sonraki adım yeni bir VM oluşturmaktır:

* [Azure portalı üzerinden Bir VM oluşturma](quick-create-portal.md)
* [Windows Sanal Makinelere genel bakış](overview.md)
