---
title: Azure Media Services hesabı oluşturma
description: Bu öğretici Azure Media Services hesap oluşturma adımlarında size yol gösterir.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: devx-track-azurecli
ms.openlocfilehash: 93f5e4d659b94bd79345a5e687de14ab6a5e8ba6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89267981"
---
# <a name="create-a-media-services-account"></a>Media Services hesabı oluşturma

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Azure 'da medya içeriğini şifrelemeyi, kodlamayı, çözümlemeyi, yönetmeyi ve akışını başlatmak için bir Media Services hesabı oluşturmanız gerekir. Media Services hesabının bir veya daha fazla depolama hesabıyla ilişkilendirilmesi gerekir. Bu makalede yeni bir Azure Media Services hesabı oluşturmaya yönelik adımlar açıklanmaktadır.

> [!NOTE]
> Media Services hesabı ve onunla ilişkili tüm depolama hesaplarının aynı Azure aboneliğinde olması gerekir. Gecikme süreleri ve veri çıkış maliyetleri ile karşı karşıya kalmamak için mutlaka Media Services ile aynı konumda bulunan depolama hesaplarını kullanmanız önerilir.

 Bir Media Services hesabı oluşturmak için Azure portal ya da CLı kullanabilirsiniz. Kullanmak istediğiniz yöntem için sekmeyi seçin.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="portal"></a>[Portal](#tab/portal/)

## <a name="use-the-azure-portal"></a>Azure portalını kullanma

<!-- Move this section. This section should be moved to conceptual.  It doesn't belong in task based -->
Azure portal, hızlı bir şekilde Azure Media Services hesabı oluşturmak için bir yol sağlar. Azure’da medya içeriği depolamanıza, şifrelemenize, kodlamanıza, yönetmenize ve akış yapmanıza imkan tanıyan Media Services’e erişmek için hesabınızı kullanabilirsiniz.

Şu anda [Azure Portal](https://portal.azure.com/) şunları yapmak için kullanabilirsiniz:

* Media Services v3 [canlı olaylarını](live-events-outputs-concept.md)yönetin, 
* v3 [varlıklarını](assets-concept.md)görüntüleme (yönetme), 
* [API 'lere erişme hakkında bilgi alın](./access-api-howto.md). 

Diğer tüm yönetim görevleri (örneğin, [dönüşümler ve işler](transforms-jobs-concept.md) ve [içerik koruması](content-protection-overview.md)) için [REST API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref)veya desteklenen [SDK 'lardan](media-services-apis-overview.md#sdks)birini kullanın.
<!-- Move this section. This section should be moved to conceptual.  It doesn't belong in task based -->

### <a name="use-the-azure-portal-to-create-a-media-services-account"></a>Media Services hesabı oluşturmak için Azure portal kullanın

1. [Azure portalda](https://portal.azure.com/) oturum açın.
1. **+ Kaynak**  >  **medyası**oluştur  >  **Media Services**' a tıklayın.
1. **Media Services hesabı oluştur** bölümünde gerekli değerleri girin.

    | Ad | Açıklama |
    | ---|---|
    |**Hesap Adı**|Yeni Media Services hesabının adını girin. Media Services hesabı adı, boşluk olmadan, tümü sayı ve küçük harften oluşmalı ve 3-24 karakter uzunluğunda olmalıdır.|
    |**Abonelik**|Birden fazla aboneliğiniz varsa, erişiminiz olan Azure abonelikleri listesinden birini seçin.|
    |**Kaynak Grubu**|Yeni veya mevcut kaynağı seçin. Kaynak grubu; yaşam döngüsünü, izinleri ve ilkeleri paylaşan kaynakların bir koleksiyonudur. [Burada](../../azure-resource-manager/management/overview.md#resource-groups) daha fazla bilgi edinin.|
    |**Konum**|Media Services hesabınız için medya ve meta veri kayıtlarını depolamak üzere kullanılacak coğrafi bölgeyi seçin. Bu bölge medyanızı işlemek ve akışını sağlamak için kullanılır. Yalnızca Media Services kullanılabilen bölgeler açılır listede görüntülenir. |
    |**Depolama Hesabı**|Media Services hesabınızdan medya içeriğinin blob depolamasını sağlamak için bir depolama hesabı seçin. Media Services hesabınızla aynı coğrafi bölgede bulunan mevcut bir depolama hesabını seçebilir ya da yeni bir depolama hesabı oluşturabilirsiniz. Aynı bölgede yeni bir depolama hesabı oluşturulur. Depolama hesabı adları için kurallar Media Services hesapları ile aynıdır.<br/><br/>Bir **birincil** depolama hesabınız olmalıdır ve Media Services hesabınızla Ilişkili birkaç **İkincil** depolama hesabı olabilir. İkincil depolama hesaplarını eklemek için Azure portal kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Media Services hesapları Ile Azure depolama hesapları](storage-account-concept.md).<br/><br/>Media Services hesabı ve onunla ilişkili tüm depolama hesaplarının aynı Azure aboneliğinde olması gerekir. Gecikme süreleri ve veri çıkış maliyetleri ile karşı karşıya kalmamak için mutlaka Media Services ile aynı konumda bulunan depolama hesaplarını kullanmanız önerilir.|

1. Hesap dağıtımını ilerleme durumunu görmek için **Panoya sabitle**’yi seçin.
1. Formun alt kısmındaki **Oluştur**’a tıklayın.

    Media Services hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçeriğinizi akışa almak ve [dinamik paketleme](dynamic-packaging-overview.md) ile [dinamik şifrelemeden](content-protection-overview.md)yararlanmak için içerik akışı yapmak Istediğiniz akış uç noktasının **çalışıyor** durumda olması gerekir. 

## <a name="cli"></a>[CLI](#tab/cli/)

## <a name="use-the-azure-cli"></a>Azure CLI'yi kullanma

<!-- NOTE: The following are in the includes file and are reused in other How To articles. All task based content should be in the includes folder with the task- prefix prepended to the file name. -->

### <a name="set-the-azure-subscription"></a>Azure aboneliğini ayarlama

[!INCLUDE [Set the Azure subscription with CLI](./includes/task-set-azure-subscription-cli.md)]

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [Create a resource group with CLI](./includes/task-create-resource-group-cli.md)]

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

[!INCLUDE [Create a storage account with CLI](./includes/task-create-storage-account-cli.md)]

### <a name="create-a-media-services-account"></a>Media Services hesabı oluşturma

[!INCLUDE [Create a Media Services account with CLI](./includes/task-create-media-services-account-cli.md)]

### <a name="see-also"></a>Ayrıca bkz.

* [Azure CLI](/cli/azure/ams?view=azure-cli-latest)
* [Media Services hesaba ikincil depolama iliştirme](/cli/azure/ams/account/storage?view=azure-cli-latest#az-ams-account-storage-add)

---

## <a name="next-steps"></a>Sonraki adımlar

[Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
