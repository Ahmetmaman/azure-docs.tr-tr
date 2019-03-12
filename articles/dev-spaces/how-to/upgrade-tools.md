---
title: Azure geliştirme alanları araçları yükseltme
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 07/03/2018
ms.topic: conceptual
ms.technology: azds-kubernetes
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, kapsayıcılar
ms.openlocfilehash: 5bf85fe36b5c59574a8e12f76aaa44ebe2d85711
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57781621"
---
# <a name="how-to-upgrade-azure-dev-spaces-tools"></a>Azure geliştirme alanları araçları yükseltme

Varsa yeni bir yayın ve Azure Dev alanları zaten kullanıyorsanız, Azure geliştirme alanları istemci araçları yükseltmeniz gerekebilir.

## <a name="update-the-azure-cli"></a>Azure CLI'yı güncelleştirme

En son Azure CLI'yı güncelleştirdiğinizde, ayrıca geliştirme alanları CLI uzantısını en son sürümünü alın.

Önceki sürümü kaldırmalısınız, yalnızca uygun yükleme bulun gerekmeyen [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).


## <a name="update-the-dev-spaces-cli-extension-and-command-line-tools"></a>Komut satırı araçları ve geliştirme alanları CLI uzantısını güncelleştirin

Şu komutu çalıştırın:

```cmd
az aks use-dev-spaces -n <your-aks-cluster> -g <your-aks-cluster-resource-group> --update
```

## <a name="update-the-vs-code-extension"></a>VS Code uzantısını güncelleştirin

Yüklendikten sonra uzantı otomatik olarak güncelleştirir. Yeni özellikleri kullanmak için uzantı yeniden yüklemeniz gerekebilir. VS Code'da açın **uzantıları** bölmesinde seçin **Azure geliştirme alanları** uzantıları ve **yeniden**.

## <a name="update-the-visual-studio-extension"></a>Visual Studio uzantısını güncelleştirme

Azure geliştirme alanları içeren bir Kubernetes için Visual Studio Araçları için kullanılabilir bir güncelleştirme olduğunda gibi diğer uzantılar ve Güncelleştirmeler ile Visual Studio size bildirecektir. Üstte bir bayrak simgesine aramak için ekranın sağ.

Visual Studio Araçları'nı güncelleştirmek için seçin **Araçlar > Uzantılar ve güncelleştirmeler** menü öğesi ve sol tarafta, seçim **güncelleştirmeleri**. Bulma **Kubernetes için Visual Studio Araçları** ve **güncelleştirme** düğmesi.

## <a name="next-steps"></a>Sonraki adımlar

Yeni bir küme oluşturarak yeni araçlarını gözden test edin. Hızlı başlangıçlar ve öğreticilerle en deneyin [Azure geliştirme alanları](/azure/dev-spaces).

> [!WARNING]
> Tüm en son sürümü kullandığınızdan emin olmak için Azure dağıtımlarınızı oluşturmak için yeni bir küme araçları yükseltme yaptıktan sonra var olan kümelerinde azure geliştirme alanları hemen, yama değil.
