---
title: (KULLANIM DIŞI) Sysdig ile bir Azure Container Service kümesini izleme
description: Sysdig ile bir Azure Container Service kümesini izleyin.
services: container-service
author: sauryadas
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 08/08/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 3b6781a6b87f9f11b341cf3cf2bcf6668b640c2b
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52992190"
---
# <a name="deprecated-monitor-an-azure-container-service-cluster-with-sysdig"></a>(KULLANIM DIŞI) Sysdig ile bir Azure Container Service kümesini izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Bu makalede, Azure Container Service kümenizdeki tüm aracı düğümlere Sysdig aracıları dağıtılır. Bu yapılandırma için bir Sysdig hesabınızın olması gerekir. 

## <a name="prerequisites"></a>Önkoşullar
Azure Container Service tarafından yapılandırılmış bir kümeyi [dağıtın](container-service-deployment.md) ve [bağlayın](../container-service-connect.md). [Marathon Kullanıcı Arabirimi](container-service-mesos-marathon-ui.md)’ni keşfedin. Git [ http://app.sysdigcloud.com ](http://app.sysdigcloud.com) bir Sysdig bulut hesabı ayarlamak için. 

## <a name="sysdig"></a>Sysdig
Sysdig, kümenizdeki kapsayıcıları izlemenize olanak tanıyan bir izleme hizmetidir. Sysdig, sorun gidermeye yardımcı olmasıyla bilinir, ayrıca CPU, Ağ, Bellek ve G/Ç izlemede kullandığınız temel ölçümleri de içerir. Sysdig, üzerinde en fazla çalıştığınız veya en fazla bellek ve CPU kullanan kapsayıcıları görmenizi kolaylaştırır. Bu görünüm, şu anda beta sürümünde olan “Genel Bakış” bölümünde yer alır. 

![Sysdig Kullanıcı Arabirimi](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a>Marathon ile bir Sysdig dağıtımı yapılandırma
Bu adımlarda Sysdig uygulamalarını Marathon ile yapılandırma ve kümenize dağıtma konuları açıklanmaktadır. 

DC/OS kullanıcı arabiriminize erişin [ http://localhost:80/ ](http://localhost:80/) bir kez ", sol altta bulunan Evreni", DC/OS Arabiriminde gidin ve ardından "Sysdig" için arama yapın

![DC/OS Evreninde Sysdig](./media/container-service-monitoring-sysdig/sysdig1.png)

Yapılandırmayı tamamlayabilmeniz için bir Sysdig bulut hesabınız veya ücretsiz deneme hesabınız olmalıdır. Sysdig bulut web sitesinde oturum açtıktan sonra kullanıcı adınıza tıklayın, sayfada “Erişim Anahtarınızı” göreceksiniz. 

![Sysdig API anahtarı](./media/container-service-monitoring-sysdig/sysdig2.png) 

Ardından DC/OS Evreninde Sysdig yapılandırmanıza Erişim Anahtarınızı girin. 

![DC/OS Evreninde Sysdig yapılandırması](./media/container-service-monitoring-sysdig/sysdig3.png)

Kümeye yeni bir düğüm eklendiğinde Sysdig’in bu yeni düğüme otomatik olarak bir aracı dağıtması için örnekleri 10000000 olarak ayarlayın. Bu, Sysdig’in küme içindeki tüm aracılara dağıtım yapmasını sağlayacak geçici bir çözümdür. 

![DC/OS Evreninde Sysdig yapılandırmasına örnekler](./media/container-service-monitoring-sysdig/sysdig4.png)

Paketi yükledikten sonra Sysdig kullanıcı arabirimine geri dönün. Buradan kümeniz içindeki kapsayıcılar için farklı ölçümlerin nasıl kullanıldığını keşfedebilirsiniz. 

Ayrıca [yeni pano sihirbazıyla](https://app.sysdigcloud.com/#/dashboards/new) Mesos ve Marathon'a özel panoları da yükleyebilirsiniz.
