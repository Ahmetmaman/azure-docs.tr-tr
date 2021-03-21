---
title: Azure Dev Spaces nedir?
services: azure-dev-spaces
ms.date: 05/07/2019
ms.topic: overview
description: Azure Dev Spaces Azure Kubernetes hizmet kümelerinde takımlar için hızlı ve yinelemeli bir Kubernetes geliştirme deneyimi sağlar
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, kubectl, k8s
manager: gwallace
ms.openlocfilehash: fd6f52c71c81f253f3f40f05408e45b6a6c0dbce
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102199484"
---
# <a name="what-is-azure-dev-spaces"></a>Azure Dev Spaces nedir?

[!INCLUDE [Azure Dev Spaces deprecation](../../includes/dev-spaces-deprecation.md)]

Azure Dev Spaces, Azure Kubernetes Service (AKS) kümelerinde takımlar için hızlı ve yinelemeli bir Kubernetes geliştirme deneyimi sağlar. Azure Dev Spaces Ayrıca, bağımlılıklarda uygulamanızın tüm bileşenlerinin hatalarını ayıklamanıza ve test etmenize gerek kalmadan, en az geliştirme makinesi kurulumuyla test etmenizi sağlar.

![Diyagramda, uygulamanın bağımsız olarak geliştirilen iki sürümü gösterilmektedir. Daha sonra bir Azure Dev Spaces geliştirme ortamında birleştirilir.](media/azure-dev-spaces/collaborate-graphic.gif)

## <a name="how-azure-dev-spaces-simplifies-kubernetes-development"></a>Azure Dev Spaces’in Kubernetes geliştirmeyi kolaylaştırması

Azure Dev Spaces, takımların mikro hizmet uygulamalarının geliştirilmesine ve hızlı yinelemesine odaklanarak ekiplerin, AKS 'de çalışan tüm mikro hizmetler mimarisi veya uygulamaları ile doğrudan çalışmasına yardımcı olur. Azure Dev Spaces, mikro hizmetler mimarinizin bölümlerini, AKS kümesinin veya diğer geliştiricilerin geri kalanını etkilemeden yalıtımsız olarak güncelleştirmek için bir yol da sağlar. Azure Dev Spaces, düşük düzey geliştirme ve test ortamlarında geliştirme ve test amaçlıdır ve üretim AKS kümelerinde çalıştırılmak üzere tasarlanmamıştır.

Takımlar tüm uygulamayla çalışarak ve doğrudan AKS 'de işbirliği yapabileceğinizden Azure Dev Spaces:

* Yerel makine kurulumunu en aza indirir
* Ekipte yeni geliştiriciler için kurulum süresini düşürür
* Ekibin hızını daha hızlı yineleme ile artırır
* Ekip üyelerinin bir kümeyi paylaştığından bu yana gereksiz geliştirme ve tümleştirme ortamları sayısını azaltır
* Bağımlılıkları çoğaltma veya artırma gereksinimini ortadan kaldırır
* , Geliştirme ekiplerinin yanı sıra çalıştıkları ekipler ve DevOps takımları gibi işbirliğini geliştirir

Azure Dev Spaces, projelerinize yönelik Docker ve Kubernetes varlıkları oluşturmak için araç sağlar. Bu araç, hem geliştirme alanı hem de diğer AKS kümelerine kolayca yeni ve mevcut uygulamaları eklemenize olanak tanır.

Azure Dev Spaces nasıl çalıştığı hakkında daha fazla bilgi için, bkz. [Azure dev Spaces nasıl çalıştığını ve nasıl yapılandırıldığını][how-dev-spaces-works]öğrenin.

## <a name="supported-regions-and-configurations"></a>Desteklenen bölgeler ve Konfigürasyonlar

Azure Dev Spaces yalnızca [bazı bölgelerde][supported-regions]aks kümeleri tarafından desteklenir. Azure Dev Spaces, uygulamalarınızı bir AKS üzerinde derlemek ve çalıştırmak için Linux, macOS veya Windows 8 veya sonraki bir sürümü yüklü [Azure dev Spaces uzantılı](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) [Azure CLI](/cli/azure/install-azure-cli) veya [Visual Studio Code](https://code.visualstudio.com/download) kullanımını destekler. Ayrıca, Azure geliştirme iş yükü ile Windows 'da yüklü [Visual Studio 2019](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) kullanmayı da destekler.

## <a name="next-steps"></a>Sonraki adımlar

Azure Dev Spaces nasıl çalıştığı hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure Dev Spaces nasıl çalışır?](how-dev-spaces-works.md)

[how-dev-spaces-works]: how-dev-spaces-works.md
[supported-regions]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service
