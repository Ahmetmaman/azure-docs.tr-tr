---
title: Depolama erişim anahtarlarını değiştirdikten sonra Media Services güncelleştir | Microsoft Docs
description: Bu makaleler, depolama erişim anahtarlarını değiştirdikten sonra Media Services güncelleştirme hakkında rehberlik sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: a892ebb0-0ea0-4fc8-b715-60347cc5c95b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2018
ms.author: milanga;cenkdin;juliako
ms.openlocfilehash: 4ab2c58e5a0f9e04d824eeea443a13af7a32617d
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52619665"
---
# <a name="update-media-services-after-rolling-storage-access-keys"></a>Depolama erişim anahtarlarını değiştirdikten sonra Media Services'ı güncelleştirme

Yeni bir Azure Media Services (AMS) hesabı oluşturduğunuzda, medya içeriğinizi depolamak için kullanılan bir Azure depolama hesabı seçmeniz istenir. Media Services hesabınız için birden fazla depolama hesapları ekleyebilirsiniz. Bu makale depolama anahtarlarını döndürme işlemini gösterir. Ayrıca bir medya hesabı için depolama hesapları ekleme işlemini de gösterir. 

Bu makalede açıklanan işlemleri gerçekleştirmek için kullanmanız [Azure Resource Manager API'leri](/rest/api/media/operations/azure-media-services-rest-api-reference) ve [Powershell](https://docs.microsoft.com/powershell/module/azurerm.media).  Daha fazla bilgi için [PowerShell ve Resource Manager ile Azure kaynaklarını yönetmek nasıl](../../azure-resource-manager/powershell-azure-resource-manager.md).

## <a name="overview"></a>Genel Bakış

Yeni bir depolama hesabı oluşturulduğunda Azure depolama hesabınıza erişim kimlik doğrulaması için kullanılan iki adet 512 bit depolama erişim tuşu oluşturur. Depolama bağlantılarınızı daha güvenli tutmak için bunu düzenli aralıklarla yeniden üretin ve depolama erişim anahtarınızı döndürme önerilir. İki erişim tuşu (birincil ve ikincil) depolama hesabı erişim anahtarı yeniden oluşturmak, bir erişim anahtarı kullanılarak bağlantı sürdürmenizi etkinleştirmek için sağlanır. Bu yordamı, "erişim anahtarları" olarak da adlandırılır.

Media Services, sağlanan depolama anahtarı bağlıdır. Özellikle, akış veya varlıklarınızı indirmek için kullanılan bulucular belirtilen depolama erişim anahtarını temel bağlıdır. AMS hesabınız oluşturulduğunda, bir bağımlılık birincil depolama erişim anahtarı varsayılan olarak alır ancak AMS sahip depolama anahtarı bir kullanıcı olarak güncelleştirebilirsiniz. Media Services'ın bu makalede açıklanan adımları izleyerek kullanmak için hangi anahtarı biliyor olanak emin olmanız gerekir.  

>[!NOTE]
> Birden çok depolama hesabınız yoksa, bu yordamı her bir depolama hesabı ile gerçekleştirmelisiniz. Depolama anahtarlarını döndürme sırasını sabit değildir. İkincil anahtar ilk ve birincil anahtar veya bunun tersini de döndürebilirsiniz.
>
> Bir üretim hesapta bu makalede açıklanan adımları yürütülmeden önce bir üretim öncesi hesabında sınanacak emin olun.
>

## <a name="steps-to-rotate-storage-keys"></a>Depolama anahtarlarını döndürmek için adımları 
 
 1. Depolama hesabının birincil anahtarını powershel cmdlet'i değiştirmek veya [Azure](https://portal.azure.com/) portalı.
 2. Depolama hesabı anahtarlarını almak için medya hesabı zorlamak için uygun params ile eşitleme AzureRmMediaServiceStorageKeys cmdlet'i çağırın
 
    Aşağıdaki örnek, depolama hesaplarının anahtarlarını eşitleme gösterilmektedir.
  
         Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId
  
 3. Bir saat kadar bekleyin. Akış senaryoları çalışma doğrulayın.
 4. Depolama hesabı ikincil anahtar powershell cmdlet'ini veya Azure portalı üzerinden değiştirin.
 5. Eşitleme AzureRmMediaServiceStorageKeys powershell ile yeni depolama hesabı anahtarlarını almak için medya hesabı zorlamak için uygun params çağırın. 
 6. Bir saat kadar bekleyin. Akış senaryoları çalışma doğrulayın.
 
### <a name="a-powershell-cmdlet-example"></a>Bir powershell cmdlet'i örneği 

Aşağıdaki örnek, depolama hesabı almak ve AMS hesabı ile eşitlemek gösterilmektedir.

    $regionName = "West US"
    $resourceGroupName = "SkyMedia-USWest-App"
    $mediaAccountName = "sky"
    $storageAccountName = "skystorage"
    $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

    Sync-AzureRmMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId

 
## <a name="steps-to-add-storage-accounts-to-your-ams-account"></a>AMS hesabınızı depolama hesapları ekleme adımları

Aşağıdaki makalede AMS hesabınızı depolama hesapları ekleme işlemi gösterilmektedir: [bir Media Services hesabına birden çok depolama hesabı ekleme](meda-services-managing-multiple-storage-accounts.md).

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a>İlgili kaynaklar
Bu belge oluşturma doğrultusunda katkıda bulunan aşağıdaki kişilerin onaylamak istiyoruz: Cenk Dingiloglu, Milano Gada Seva Titov.
