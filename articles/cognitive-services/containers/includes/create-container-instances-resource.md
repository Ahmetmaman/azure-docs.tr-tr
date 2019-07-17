---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
description: Bir Azure container örneği kaynak oluşturmayı öğrenin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/3/2019
ms.author: dapine
ms.openlocfilehash: 05284d434e6bd22fd50957f7cc5ec966f88a4fd4
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "68229182"
---
## <a name="create-an-azure-container-instance-resource"></a>Bir Azure Container Instance kaynağı oluşturun

1. Git [Oluştur](https://ms.portal.azure.com/#create/Microsoft.ContainerInstances) Container Instances için sayfa.

2. Üzerinde **Temelleri** sekmesinde, aşağıdaki bilgileri girin:

    |Ayar|Value|
    |--|--|
    |Subscription|Aboneliğinizi seçin.|
    |Resource group|Kullanılabilir kaynak grubu seçin veya yeni bir tane oluşturmanız `cognitive-services`.|
    |Kapsayıcı adı|Gibi bir ad girin `cognitive-container-instance`. Ad, daha düşük bir büyük harf olarak olmalıdır.|
    |Location|Dağıtım için bir bölge seçin.|
    |Görüntü türü|`Public`|
    |Görüntü adı|Bilişsel hizmetler kapsayıcı konumunu girin. Konum kullanılan aynı olabilir `docker pull` komutu, başvurmak [kapsayıcı depoları ve görüntüleri](../../cognitive-services-container-support.md#container-repositories-and-images) için kullanılabilir görüntü adlarını ve bunların karşılık gelen bir depo.|
    |İşletim sistemi türü|`Linux`|
    |Size|Belirli bir Bilişsel hizmet kapsayıcınız için önerilen önerileri boyutunu değiştir:<br>2 CPU çekirdekleri<br>4 GB

3. Üzerinde **ağ** sekmesinde, aşağıdaki bilgileri girin:

    |Ayar|Değer|
    |--|--|
    |Bağlantı Noktaları|TCP bağlantı noktası olarak `5000`. Kapsayıcı 5000 numaralı bağlantı noktasında kullanıma sunar.|

4. Üzerinde **Gelişmiş** sekmesinde, gerekli girin **ortam değişkenlerini** için kapsayıcı faturalama ACI kaynak ayarları:

    | Anahtar | Değer |
    |--|--|
    |`apikey`|Öğesinden kopyalanan **anahtarları** kaynak sayfası. Boşluk veya tire ile 32 bir alfasayısal karakter dizesi olduğu `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
    |`billing`|Öğesinden kopyalanan **genel bakış** kaynak sayfası.|
    |`eula`|`accept`|

1. Tıklayın **gözden geçir ve Oluştur**
1. Doğrulama denetimini geçtikten tıklayın **Oluştur** oluşturma işlemini tamamlamak için
1. Kaynak başarıyla dağıtıldıktan sonra hazır