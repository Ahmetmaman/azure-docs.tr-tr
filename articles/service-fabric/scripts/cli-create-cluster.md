---
title: Azure CLI Betiği Dağıtma Örneği
description: Azure CLI kullanarak Azure’da güvenli bir Service Fabric Linux kümesi oluşturun.
services: service-fabric
documentationcenter: ''
author: rwike77
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 01/18/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 79125beac6ab34b88c951a2fb9c601b93ae36e20
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34642215"
---
# <a name="create-a-secure-service-fabric-linux-cluster-in-azure"></a>Azure’da güvenli bir Service Fabric Linux kümesi oluşturma

Bu komut, otomatik olarak imzalanan sertifika oluşturur, bu sertifikayı bir anahtar kasasına ekler ve sertifikayı yerel ortamda indirir.  Yeni sertifika, dağıtıldığında kümenin güvenliğini sağlamak için kullanılır.  Yeni bir sertifika oluşturmak yerine mevcut sertifikayı da kullanabilirsiniz.  Her iki şekilde de, sertifikanın konu adı, Service Fabric kümesine erişmek için kullandığınız etki alanıyla eşleşmelidir. Kümenin HTTPS yönetim uç noktalarına ve Service Fabric Explorer’a yönelik SSL sağlanması için bu eşleşme gereklidir. `.cloudapp.azure.com` etki alanı için bir sertifika yetkilisinden SSL sertifikası edinemezsiniz. Kümeniz için özel bir etki alanı adı edinmeniz gerekir. CA’dan sertifika istediğinizde sertifikanın konu adı, kümeniz için kullandığınız özel etki alanı adıyla eşleşmelidir.

Gerekirse, [Azure CLI 2.0](/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)’ı yükleyin.

## <a name="sample-script"></a>Örnek betik

[!code-sh[main](../../../cli_scripts/service-fabric/create-cluster/create-cluster.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu, kümeyi ve ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```azurecli
ResourceGroupName = "aztestclustergroup"
az group delete --name $ResourceGroupName
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az sf cluster create](https://docs.microsoft.com/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) | Yeni bir Service Fabric kümesi oluşturur.  |

## <a name="next-steps"></a>Sonraki adımlar

Azure Service Fabric’e yönelik ek Service Fabric CLI örnekleri [Service Fabric CLI örnekleri](../samples-cli.md)’nde bulunabilir.
