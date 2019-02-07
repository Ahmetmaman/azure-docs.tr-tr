---
title: Özel Docker kapsayıcısı kayıt defterleri Azure - genel bakış
description: Bulut tabanlı, yönetilen, özel Docker kayıt defterleri sağlayan Azure Container Kayıt Defteri hizmetine giriş.
services: container-registry
author: stevelas
ms.service: container-registry
ms.topic: overview
ms.date: 09/25/2018
ms.author: stevelas
ms.custom: seodec18, mvc
ms.openlocfilehash: 0ffad78afd6acdfbcea8df970358c639706f14b8
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55814257"
---
# <a name="introduction-to-private-docker-container-registries-in-azure"></a>Azure'da özel Docker kapsayıcısı kayıt defterlerine giriş

Azure Container Kayıt Defteri, açık kaynak Docker Registry 2.0’ı temel alan bir yönetilen [Docker kayıt defteri](https://docs.docker.com/registry/) hizmetidir. Özel [Docker kapsayıcısı](https://www.docker.com/what-docker) görüntülerinizi depolamak ve yönetmek için Azure kapsayıcısı kayıt defterleri oluşturun ve bunların bakımını yapın.

Azure’da mevcut kapsayıcı geliştirme ve dağıtım işlem hatlarınızla kapsayıcı kayıt defterleri kullanın. Azure Container Registry Oluşturması (ACR Build) kullanarak Azure’da kapsayıcı görüntüleri oluşturun. İsteğe bağlı olarak oluşturun veya kaynak kodu yürütme ve temel görüntü güncelleştirmesi oluşturma tetikleyicileri ile yapıları tamamen otomatik hale getirin.

Docker ve kapsayıcılarla ilgili arka plan bilgileri için bkz. [Docker’a genel bakış](https://docs.docker.com/engine/docker-overview/).

## <a name="use-cases"></a>Uygulama alanları

Azure kapsayıcısı kayıt defterinden çeşitli dağıtım hedeflerine görüntü çekme:

* [Kubernetes](http://kubernetes.io/docs/), [DC/OS](https://docs.mesosphere.com/) ve [Docker Swarm](https://docs.docker.com/swarm/) dahil olmak üzere konak kümeleri arasında kapsayıcı haline getirilmiş uygulamaları yöneten **ölçeklenebilir düzenleme sistemleri**.
* [Azure Kubernetes Service (AKS)](../aks/index.yml), [App Service](../app-service/index.yml), [Batch](../batch/index.yml), [Service Fabric](/azure/service-fabric/) ve diğerleri gibi uygun ölçekte uygulama oluşturulmasını ve çalıştırılmasını destekleyen **Azure hizmetleri**.

Geliştiriciler bir kapsayıcı geliştirme iş akışı kapsamında bir kapsayıcı kayıt defterine de öğe itebilir. Örneğin, [Azure DevOps Services](https://docs.microsoft.com/azure/devops/) veya [Jenkins](https://jenkins.io/) gibi bir sürekli tümleştirme ve dağıtım aracından bir kapsayıcı kayıt defteri hedeflenebilir.

ACR kendi temel görüntüleri güncelleştirildiğinde uygulama görüntüleri otomatik olarak yeniden derlemek için görevleri yapılandırın. Ekibiniz bir Git deposunda kod yürüttüğünde görüntü oluşturmalarını otomatikleştirmek için ACR Görevlerini kullanın.

## <a name="key-concepts"></a>Önemli kavramlar

* **Kayıt Defteri** - Azure aboneliğinizde bir veya daha fazla kapsayıcı kayıt defteri oluşturun. Kayıt defterleri üç Sku'da kullanılabilir: [Temel, standart ve Premium](container-registry-skus.md), her Web kancası tümleştirmesi, Azure Active Directory ile kayıt defteri kimlik doğrulamasını desteklemek ve silme işlevi. Kapsayıcı görüntülerinizin yerel, kapalı bir ağda depolanmasının avantajlarından yararlanmak için dağıtımlarınızla aynı Azure konumunda bir kayıt defteri oluşturun. Gelişmiş çoğaltma ve kapsayıcı görüntüsü dağıtma senaryoları için Premium kayıt defterlerinin [coğrafi çoğaltma](container-registry-geo-replication.md) özelliğini kullanın. Tam kayıt defteri adı `myregistry.azurecr.io` biçimindedir.

  [Erişimini](container-registry-authentication.md) kullanarak bir Azure kimlik Azure Active Directory destekli bir kapsayıcı kayıt defterine [hizmet sorumlusu](../active-directory/develop/app-objects-and-service-principals.md), ya sağlanan yönetici hesabı. Azure komut satırı arabirimi ya da standart kullanarak kayıt defterini oturum `docker login` komutu.

* **Depo** -kapsayıcı görüntüleri grubu depolayan bir veya daha fazla depo bir kayıt defteri içerir. Azure Container Kayıt Defteri, çok düzeyli depo ad alanlarını destekler. Çok düzeyli ad alanlarıyla, belirli bir uygulamayla veya uygulama koleksiyonuyla ilişkili görüntü koleksiyonlarını belirli geliştirme veya işlem ekipleri halinde gruplandırabilirsiniz. Örneğin:

  * `myregistry.azurecr.io/aspnetcore:1.0.1`, kurum çapında bir görüntüyü temsil eder
  * `myregistry.azurecr.io/warrantydept/dotnet-build`, garanti departmanı genelinde paylaşılan .NET uygulamaları oluşturmak için kullanılan bir görüntüyü temsil eder
  * `myregistry.azurecr.io/warrantydept/customersubmissions/web`, garanti departmanına ait olan müşteri gönderileri uygulamasında gruplandırılan bir web görüntüsünü temsil eder

* **Görüntü** -bir depoda depolanan görüntülerin her uyumlu Docker kapsayıcısının salt okunur anlık görüntüsüdür. Azure kapsayıcısı kayıt defterleri hem Windows hem de Linux görüntüleri içerebilir. Tüm kapsayıcı dağıtımlarınız için görüntü adlarını siz denetlersiniz. Bir depoya görüntü itmek ya da bir depodan görüntü çekmek için standart [Docker komutlarını](https://docs.docker.com/engine/reference/commandline/) kullanın. Kapsayıcı görüntülerinin yanı sıra, Azure Container Registry depolar [ilgili içerik biçimlerini](container-registry-image-formats.md) gibi [Helm grafikleri](container-registry-helm-repos.md)Kubernetes uygulamaları dağıtmak için kullanılır.

* **Kapsayıcı** - Kapsayıcı, bir yazılım uygulamasını ve uygulamanın bağımlılıklarını kod, çalışma zamanı, sistem araçları ve kitaplıkları içeren eksiksiz bir dosya sistemi şeklinde sarmalanmış bir halde tanımlar. Docker kapsayıcılarını bir kapsayıcı kayıt defterinden çektiğiniz Windows veya Linux görüntülerine bağlı olarak çalıştırın. Tek bir makinede çalışan kapsayıcılar işletim sistemi çekirdeğini paylaşır. Docker kapsayıcıları tüm büyük Linux dağıtımlarına, macOS ve Windows'a tümüyle taşınabilir.

## <a name="azure-container-registry-tasks"></a>Azure Container Registry Görevleri

[Azure Container Registry Görevleri](container-registry-tasks-overview.md) (ACR Görevleri), Azure’da kolaylaştırılmış ve verimli Docker kapsayıcı görüntüsü derlemeleri sağlayan bir Azure Container Registry özellik paketidir. `docker build` işlemlerini Azure’a aktararak geliştirme iç döngünüzü buluta genişletmek için ACR Görevleri kullanın. Kapsayıcı işletim sisteminizi ve çerçeve düzeltme eki uygulama işlem hattınızı otomatikleştirmek ve ekibiniz kaynak denetiminde kod yürüttüğünde otomatik olarak görüntü oluşturmak için oluşturma görevleri yapılandırın.

ACR Görevlerinin bir önizleme özelliği olan [çok adımlı görevler](container-registry-tasks-overview.md#multi-step-tasks-preview), bulutta kapsayıcı görüntüleri oluşturmak, test etmek ve düzeltme eki uygulamak için adım tabanlı görev tanımı ve yürütmesi sağlar. Görev adımları, tek tek kapsayıcı derleme ve gönderme işlemlerini tanımlar. Ayrıca, her adımın kapsayıcıyı kendi yürütme ortamı olarak kullanmasıyla bir veya daha fazla kapsayıcının yürütülmesini de tanımlayabilirler.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure portalını kullanarak kapsayıcı kayıt defteri oluşturma](container-registry-get-started-portal.md)
* [Azure CLI’yı kullanarak kapsayıcı kayıt defteri oluşturma](container-registry-get-started-azure-cli.md)
* [ACR Görevleri ile işletim sistemi ve çerçeve düzeltme eki uygulamayı otomatikleştirme](container-registry-tasks-overview.md)
