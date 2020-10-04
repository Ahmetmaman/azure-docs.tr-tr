---
title: Azure Kubernetes hizmetine bağlanma-PostgreSQL için Azure veritabanı-tek sunucu
description: Azure Kubernetes hizmeti 'ni (AKS) PostgreSQL için Azure veritabanı-tek sunucu ile bağlama hakkında bilgi edinin
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.date: 07/14/2020
ms.topic: conceptual
ms.openlocfilehash: 9b7da2fcc1310f03f894e048089658f25be3a149
ms.sourcegitcommit: 19dce034650c654b656f44aab44de0c7a8bd7efe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2020
ms.locfileid: "91708858"
---
# <a name="connecting-azure-kubernetes-service-and-azure-database-for-postgresql---single-server"></a>Azure Kubernetes hizmetini ve PostgreSQL için Azure veritabanı 'nı bağlama-tek sunucu

Azure Kubernetes hizmeti (AKS), Azure 'da kullanabileceğiniz yönetilen bir Kubernetes kümesi sağlar. Aşağıda, bir uygulama oluşturmak için AKS ve PostgreSQL için Azure veritabanı kullanılırken dikkate alınması gereken bazı seçenekler verilmiştir.


## <a name="accelerated-networking"></a>Hızlandırılmış ağ iletişimi
AKS kümenizde hızlandırılmış ağ özellikli, temel alınan VM 'Leri kullanın. Bir VM 'de hızlandırılmış ağ etkinleştirildiğinde, VM 'de daha düşük gecikme süresi, azaltılmış değişim ve CPU kullanımı azalır. Hızlandırılmış ağ oluşturma, desteklenen işletim sistemi sürümleri ve [Linux](../virtual-network/create-vm-accelerated-networking-cli.md)IÇIN desteklenen VM örnekleri hakkında daha fazla bilgi edinin.

AKS, Kasım 2018 ' den bu desteklenen sanal makine örneklerinde hızlandırılmış ağı destekler. Hızlandırılmış ağ, bu VM 'Leri kullanan yeni AKS kümelerinde varsayılan olarak etkindir.

AKS kümenizin hızlandırılmış ağa sahip olup olmadığını doğrulayabilirsiniz:
1. Azure portal gidin ve AKS kümenizi seçin.
2. Özellikler sekmesini seçin.
3. **Altyapı kaynak grubunun**adını kopyalayın.
4. Altyapı kaynak grubunu bulmak ve açmak için Portal arama çubuğunu kullanın.
5. Bu kaynak grubunda bir VM seçin.
6. VM 'nin **ağ** sekmesine gidin.
7. **Hızlandırılmış ağın** ' etkin ' olup olmadığını onaylayın.

Ya da aşağıdaki iki komutu kullanarak Azure CLı aracılığıyla:
```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --query "nodeResourceGroup"
```
Çıktı, ağ arabirimini içeren AKS oluşturduğu kaynak grubu olacaktır. "NodeResourceGroup" adını alıp sonraki komutta kullanın. **Enableivme ağı** doğru veya yanlış olacaktır:
```azurecli
az network nic list --resource-group nodeResourceGroup -o table
```


## <a name="connection-pooling"></a>Bağlantı havuzu
Bir bağlantı havuzlayıcı, veritabanına yeni bağlantılar oluşturma ve kapatma ile ilişkili maliyet ve süreyi en aza indirir. Havuz yeniden kullanılabilen bir bağlantı koleksiyonudur. 

PostgreSQL ile kullanabileceğiniz birden çok bağlantı havuzleyiciler vardır. Bunlardan biri [Pgbouncer](https://pgbouncer.github.io/). Microsoft Container Registry, AKS 'ten PostgreSQL için Azure veritabanı arasındaki bağlantıları havuza almak üzere bir sepet içinde kullanılabilen hafif bir Kapsayıcılı PgBouncer sunuyoruz. Bu resme erişme ve bu görüntüyü kullanma hakkında bilgi edinmek için [Docker Hub sayfasını](https://hub.docker.com/r/microsoft/azureossdb-tools-pgbouncer/) ziyaret edin. 


## <a name="next-steps"></a>Sonraki adımlar
-  [Azure Kubernetes Service kümesini oluşturma](../aks/kubernetes-walkthrough.md)
