---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 09/20/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: f2e252a000c5633b2cc1ef34683d7d2c7c133c03
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47048389"
---
## <a name="using-rbac-to-share-images"></a>Görüntüleri paylaşmak için RBAC kullanma

Rol tabanlı erişim denetimi (RBAC) kullanarak abonelikler arasında görüntüleri paylaşabilir. Bir görüntü sürümü bile abonelikler arasında okuma izni herhangi bir kullanıcı görüntü sürümünü kullanarak bir sanal makineyi dağıtmak mümkün olacaktır.

RBAC kullanarak kaynakları paylaşma hakkında daha fazla bilgi için bkz. [RBAC ve Azure CLI kullanarak erişimini yönetme](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli).


## <a name="list-information"></a>Liste bilgileri

Konum, durum ve kullanılabilir resim galerileri kullanma hakkındaki diğer bilgileri alma [az sig listesi](/cli/azure/sig#az-sig-list).

```azurecli-interactive 
az sig list -o table
```

İşletim sistemi türü ve durumu hakkında bilgiler dahil olmak üzere, kullanarak bir galeride görüntü tanımları listesi [az sig görüntü tanım listesi](/cli/azure/sig/image-definition#az-sig-image-definition-list).

```azurecli-interactive 
az sig image-definition list -g myGalleryRG -r myGallery -o table
```

Bir galeride bulunan paylaşılan görüntü sürümleri listesi kullanarak [az sig görüntü sürüm listesi](/cli/azure/sig/image-version#az-sig-image-version-list).

```azurecli-interactive
az sig image-version list -g myGalleryRG -r myGallery -i myGalleryImage -o table
```

Kullanarak bir görüntü sürüm Kimliğini alın [az sig görüntü sürümü göster](/cli/azure/sig/image-version#az-sig-image-version-show).

```
az sig image-version show \
-g myGalleryRG \     
-r myGallery \     
-i myGalleryImage \     
--gallery-image-version-name 1.0.0 \     
--query "id"
```