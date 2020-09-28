---
author: baanders
description: Azure dijital TWINS öğreticileri için dosya ekleme-örnek projeyi yapılandırma
ms.service: digital-twins
ms.topic: include
ms.date: 5/25/2020
ms.author: baanders
ms.openlocfilehash: 4ac748c606d8ec3c8ba754c34d9c9e7512344a83
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91292749"
---
## <a name="configure-the-sample-project"></a>Örnek projeyi yapılandırma

Ardından, Azure dijital TWINS örneğinizle etkileşime girebilen bir örnek istemci uygulaması ayarlayın.

Daha önce [*Azure dijital TWINS örneklerinden*](https://docs.microsoft.com/samples/azure-samples/digital-twins-samples/digital-twins-samples) indirdiğiniz bir dosya için makinenizde gezinme yapın (daha önce yapmadıysanız dosyayı ayıklayın.)

Klasörün içindeyken _AdtSampleApp_adresine gidin. Visual Studio 2019 ' de _**AdtE2ESample. sln**_ 'yi açın. 

Visual Studio 'da, *Solution Explorer* _üzerinde sampleclientapp >serviceConfig.jsbir kopyasını oluşturmak için Çözüm Gezgini bölmesini kullanın **. Şablon** _ dosyası (kopyalamak ve yapıştırmak için sağ seçme menülerini kullanabilirsiniz). Kopyalama *serviceConfig.js*yeniden adlandırın. Bu, projeyi çalıştırmak için gerekli yapılandırma değişkenlerine sahip bir önceden ayarlanmış JSON dosyası işlevi görür.

Dosyayı Düzenle penceresinde açmak için *serviceConfig.js* seçin. ' I `tenantId` *Dizin Kimliğinizle*, `clientId` *uygulama Kimliğinizle*ve `instanceUrl` Azure dijital TWINS örnek *ana bilgisayar adı* URL 'si ile değiştirin (aşağıda gösterildiği gibi *https://* önünde).

```json
{
  "tenantId": "<your-directory-ID>",
  "clientId": "<your-application-ID>",
  "instanceUrl": "https://<your-Azure-Digital-Twins-instance-hostName>"
}
```



Dosyayı kaydedin ve kapatın. 

Sonra, *Sampleclientapp*' i oluştururken dosyayı çıkış dizinine kopyalamak üzere *serviceConfig.js* yapılandırın. Bunu yapmak için dosya * üzerindeserviceConfig.js* sağ seçin ve Özellikler ' i seçin *.* *Özellikler* denetçisinde, *Çıkış Dizinine Kopyala* özelliğinin değerini *daha yeniyse kopyala*olarak değiştirin.

:::image type="content" source="../articles/digital-twins/media/includes/copy-config.png" alt-text="Vurgulanan serviceConfig.jsiçeren Çözüm Gezgini bölmesini ve ' çıkış dizinine Kopyala ' özelliği olan Özellikler bölmesini daha yeniyse kopyala olarak gösteren Visual Studio penceresinden alıntı" border="false":::

Öğreticide kullanmaya devam etmek için _**AdtE2ESample**_ projesini Visual Studio 'da açık tutun.

