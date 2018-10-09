---
title: Bir Azure Container Instances gitRepo birimi
description: Kapsayıcı örneklerinizin bir Git deposunu kopyalamak için gitRepo birim bağlama hakkında bilgi edinin
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 06/15/2018
ms.author: danlep
ms.openlocfilehash: 9f693299d2466b560e0957a77e898e1b10433fb0
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48857419"
---
# <a name="mount-a-gitrepo-volume-in-azure-container-instances"></a>Azure Container ınstances'da bir gitRepo birimi

Bağlama öğrenin bir *gitRepo* birim kapsayıcı örneklerinizin bir Git deposunu kopyalamak için.

> [!NOTE]
> Bağlama bir *gitRepo* birim şu anda Linux kapsayıcıları için kısıtlanmış. Tüm özellikleri Windows kapsayıcılarına getirmek için çalışmamız esnasında, geçerli platform farklılıklarını [Azure Kapsayıcı Örnekleri için kotalar ve bölge kullanılabilirliği](container-instances-quotas.md) bölümünde bulabilirsiniz.

## <a name="gitrepo-volume"></a>gitRepo birim

*GitRepo* toplu bir dizin bağlar ve belirtilen Git deposu kapsayıcı başlangıçta içine kopyalar. Kullanarak bir *gitRepo* kapsayıcı örneklerinizin birimde önlemek uygulamalarınızda bunu yapmak için kod ekleme.

Bağlama ne zaman bir *gitRepo* birimi, birim yapılandırmak için üç özellikleri ayarlayabilirsiniz:

| Özellik | Gerekli | Açıklama |
| -------- | -------- | ----------- |
| `repository` | Evet | Tam URL de dahil olmak üzere `http://` veya `https://`, Git deposunun kopyalanacak.|
| `directory` | Hayır | Dizin deposu kopyalanma. Yolun gerekir değil içerebilir veya ile başlayan "`..`".  Belirtirseniz "`.`", depo birimin dizinine kopyalanmış olan. Aksi takdirde, Git deposu birim dizin içinde belirtilen ad, bir alt kopyalandı. |
| `revision` | Hayır | Düzeltme kopyalamak için işleme karması. Belirtilmemişse, `HEAD` düzeltme kopyalanabilir. |

## <a name="mount-gitrepo-volume-azure-cli"></a>Birim bağlama gitRepo: Azure CLI

Container Instances ile dağıttığınızda bir gitRepo birimi bağlamak için [Azure CLI](/cli/azure), sağlar `--gitrepo-url` ve `--gitrepo-mount-path` parametreleri [az kapsayıcı oluşturma] [ az-container-create] komutu. İsteğe bağlı olarak içine kopyalamak için birimin içindeki dizin belirtebilirsiniz (`--gitrepo-dir`) ve düzeltme kopyalamak için işleme Karması (`--gitrepo-revision`).

Bu örnek komut klonlar [aci-helloworld] [ aci-helloworld] örnek uygulamaya `/mnt/aci-helloworld` kapsayıcı örneğinde:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name hellogitrepo \
    --image microsoft/aci-helloworld \
    --dns-name-label aci-demo \
    --ports 80 \
    --gitrepo-url https://github.com/Azure-Samples/aci-helloworld \
    --gitrepo-mount-path /mnt/aci-helloworld
```

GitRepo birimin takılı doğrulamak için kapsayıcı ile shell'de başlatma [az container exec] [ az-container-exec] ve dizin listesinde:

```console
$ az container exec --resource-group myResourceGroup --name hellogitrepo --exec-command /bin/sh
/usr/src/app # ls -l /mnt/aci-helloworld/
total 16
-rw-r--r--    1 root     root           144 Apr 16 16:35 Dockerfile
-rw-r--r--    1 root     root          1162 Apr 16 16:35 LICENSE
-rw-r--r--    1 root     root          1237 Apr 16 16:35 README.md
drwxr-xr-x    2 root     root          4096 Apr 16 16:35 app
```

## <a name="mount-gitrepo-volume-resource-manager"></a>Birim bağlama gitRepo: Resource Manager

Container Instances ile dağıttığınızda bir gitRepo birimi bağlamak için bir [Azure Resource Manager şablonu](/azure/templates/microsoft.containerinstance/containergroups), ilk doldurmak `volumes` kapsayıcı grubu dizisinde `properties` şablon bölümü. Ardından, istediğiniz bağlamak kapsayıcı grubundaki her kapsayıcı için *gitRepo* birim doldurmak `volumeMounts` içindeki dizi `properties` kapsayıcı tanımının bölümü.

Örneğin, aşağıdaki Resource Manager şablonu tek bir kapsayıcının oluşan bir kapsayıcı grubu oluşturur. Kapsayıcı tarafından belirtilen iki GitHub depoları klonlar *gitRepo* birim bloklarında. İkinci birimin ek özellikler belirtmek için kopyalamak için bir dizin ve belirli bir düzeltmesini kopyalamak için işleme karması içerir.

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-gitrepo.json --> [!code-json[volume-gitrepo](~/azure-docs-json-samples/container-instances/aci-deploy-volume-gitrepo.json)]

Sonuçta elde edilen önceki şablonunda tanımlanan iki kopyalanan depolarda dizin yapısı şöyledir:

```
/mnt/repo1/aci-helloworld
/mnt/repo2/my-custom-clone-directory
```

Kapsayıcı örneği dağıtımıyla bir Azure Resource Manager şablonu ile bir örneğini görmek için bkz: [Azure Container ınstances'da çok kapsayıcılı grupları dağıtma](container-instances-multi-container-group.md).

## <a name="private-git-repo-authentication"></a>Özel Git deposu kimlik doğrulaması

Özel bir Git deposu için bir gitRepo birimi bağlamak için depo URL'de kimlik bilgilerini belirtin. Genellikle, kimlik bilgileri bir kullanıcı adı biçiminde ve depoya erişim veren bir kişisel erişim belirteci (PAT) kapsamı.

Örneğin, Azure CLI `--gitrepo-url` parametresi için bir özel GitHub deposu (burada "gituser" GitHub kullanıcı adı ve "abcdef1234fdsa4321abcdef" kullanıcının kişisel erişim belirteci) aşağıdaki gibi görüneceği:

```azurecli
--gitrepo-url https://gituser:abcdef1234fdsa4321abcdef@github.com/GitUser/some-private-repository
```

Azure DevOps Git deposu için geçerli bir PAT birlikte ("azuredevopsuser" aşağıdaki örnekte olduğu gibi kullanabilirsiniz) herhangi bir kullanıcı adı belirtin:

```azurecli
--gitrepo-url https://azuredevopsuser:abcdef1234fdsa4321abcdef@azuredevopsorganizationname.visualstudio.com/_git/some-private-repository
```

GitHub ve Azure DevOps için kişisel erişim belirteçleri hakkında daha fazla bilgi için aşağıdakilere bakın:

GitHub: [komut satırı için bir kişisel erişim belirteci oluşturma][pat-github]

Azure DevOps: [kimlik doğrulaması yapmak için kişisel erişim belirteçleri oluşturun][pat-vsts]

## <a name="next-steps"></a>Sonraki adımlar

Azure Container ınstances'da diğer birim türleri bağlama işlemleri gerçekleştirmeyi öğreneceksiniz:

* [Azure kapsayıcı durumlarda bir Azure dosya paylaşımını bağlama](container-instances-volume-azure-files.md)
* [Azure kapsayıcı durumlarda emptyDir birim](container-instances-volume-emptydir.md)
* [Azure Container ınstances'da bir gizli birimi](container-instances-volume-secret.md)

<!-- LINKS - External -->
[aci-helloworld]: https://github.com/Azure-Samples/aci-helloworld
[pat-github]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/
[pat-vsts]: https://docs.microsoft.com/vsts/organizations/accounts/use-personal-access-tokens-to-authenticate

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec
