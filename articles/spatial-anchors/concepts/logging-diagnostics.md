---
title: Günlüğe kaydetme ve tanılama
description: Azure uzamsal bağlayıcılarda günlük ve tanılamayı oluşturma ve alma hakkında ayrıntılı açıklamalar.
author: ramonarguelles
manager: vriveras
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/22/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.custom: devx-track-csharp
ms.openlocfilehash: 45d345e8d8dbe45ecb4873904f31141111827647
ms.sourcegitcommit: 93329b2fcdb9b4091dbd632ee031801f74beb05b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92097363"
---
# <a name="logging-and-diagnostics-in-azure-spatial-anchors"></a>Azure uzamsal bağlayıcılarda günlüğe kaydetme ve tanılama

Azure uzamsal bağlantıları, uygulama geliştirme için yararlı olan standart bir günlüğe kaydetme mekanizması sağlar. Uzamsal Tutturucuların tanılama günlüğü modu, hata ayıklama için daha fazla bilgiye ihtiyacınız olduğunda faydalıdır. Tanılama günlüğü, ortamın görüntülerini depolar.

## <a name="standard-logging"></a>Standart günlüğe kaydetme
Uzamsal bağlayıcıların API 'sinde, uygulama geliştirme ve hata ayıklama için yararlı Günlükler almak amacıyla günlüğe kaydetme mekanizmasına abone olabilirsiniz. Standart günlüğe kaydetme API 'Leri, ortamın resimlerini cihaz diskine depolamaz. SDK bu günlükleri Olay geri çağırmaları olarak sağlar. Bu günlükleri uygulamanın günlüğe kaydetme mekanizmasıyla tümleştirmeniz en iyisidir.

### <a name="configuration-of-log-messages"></a>Günlük iletilerinin yapılandırması
Kullanıcı için ilgilendiğiniz iki geri çağrı vardır. Aşağıdaki örnek, oturumunun nasıl yapılandırılacağını gösterir.

```csharp
    cloudSpatialAnchorSession = new CloudSpatialAnchorSession();
    . . .
    // set up the log level for the runtime session
    cloudSpatialAnchorSession.LogLevel = SessionLogLevel.Information;

    // configure the callback for the debug log
    cloudSpatialAnchorSession.OnLogDebug += CloudSpatialAnchorSession_OnLogDebug;

    // configure the callback for the error log
    cloudSpatialAnchorSession.Error += CloudSpatialAnchorSession_Error;
```

### <a name="events-and-properties"></a>Olaylar ve Özellikler

Bu olay geri çağırmaları, oturumdaki günlükleri ve hataları işlemek için verilmiştir:

- [LogLevel](/dotnet/api/microsoft.azure.spatialanchors.cloudspatialanchorsession.loglevel): çalışma zamanından alınan olaylar için ayrıntı düzeyini belirtir.
- [Onlogdebug](/dotnet/api/microsoft.azure.spatialanchors.cloudspatialanchorsession.onlogdebug): standart hata ayıklama günlüğü olayları sağlar.
- [Hata](/dotnet/api/microsoft.azure.spatialanchors.cloudspatialanchorsession.error): çalışma zamanının hata olarak kabul eden günlük olaylarını sağlar.

## <a name="diagnostics-logging"></a>Tanılama günlüğü

Günlük kaydı için standart işlem moduna ek olarak, uzamsal bağlayıcıların da bir tanılama modu vardır. Tanılama modu, ortamın görüntülerini yakalar ve bunları diske kaydeder. Bu modu, bir bağlayıcıyı öngörülebilir bir şekilde bulma hatası gibi belirli türlerde sorunları ayıklamak için kullanabilirsiniz. Yalnızca belirli bir sorunu yeniden oluşturmak için tanılama günlüğünü etkinleştirin. Ardından devre dışı bırakın. Uygulamalarınızı normal şekilde çalıştırırken tanılamayı etkinleştirmeyin.

Microsoft ile destek etkileşimi sırasında, bir Microsoft temsilcisi daha fazla araştırma için bir tanılama paketi göndermek isteyip istemediğinizi sorabilir. Bu durumda, tanılama paketini gönderebilmeniz için tanılamayı etkinleştirmeyi ve sorunu yeniden oluşturmaya karar verebilirsiniz.

Bir Microsoft temsilcisinden önceki doğrulamadan önce Microsoft 'a bir tanılama günlüğü gönderirseniz, gönderim yanıt vermez.

Aşağıdaki bölümlerde tanılama modunun nasıl etkinleştirileceği ve ayrıca Microsoft 'a tanılama günlüklerinin nasıl gönderileceği gösterilmektedir.

### <a name="enable-diagnostics-logging"></a>Tanılama günlüğünü etkinleştirme

Tanılama günlüğü için bir oturumu etkinleştirdiğinizde, oturumdaki tüm işlemlerin yerel dosya sisteminde karşılık gelen tanılama günlüğü vardır. Günlüğe kaydetme sırasında, ortamın görüntüleri diske kaydedilir.

```csharp
private void ConfigureSession()
{
    cloudSpatialAnchorSession = new CloudSpatialAnchorSession();
    . . .

    // set up the log level for the runtime session
    cloudSpatialAnchorSession.LogLevel = SessionLogLevel.Information;

    // configure the callbacks for logging and errors
    cloudSpatialAnchorSession.OnLogDebug += CloudSpatialAnchorSession_OnLogDebug;
    cloudSpatialAnchorSession.Error += CloudSpatialAnchorSession_Error;

    // opt in to diagnostics logging of environment images
    // if this is enabled, the diagnostics bundle includes images of the environment captured by the session
    cloudSpatialAnchorSession.Diagnostics.ImagesEnabled = true;

    // set the level of detail to be collected in the diagnostics log by the session
    cloudSpatialAnchorSession.Diagnostics.LogLevel = SessionLogLevel.All;

    // set the max bundle size to capture the bug information
    cloudSpatialAnchorSession.Diagnostics.MaxDiskSizeInMB = 200;
    . . .
}
```

### <a name="submit-the-diagnostics-bundle"></a>Tanılama paketi 'ni gönderme

Aşağıdaki kod parçacığı, Microsoft 'a bir tanılama paketi göndermeyi gösterir. Bu paket, tanılamayı etkinleştirdikten sonra oturum tarafından yakalanan ortamın görüntülerini içerir.

```csharp
// method to handle the diagnostics bundle submission
private async Task CreateAndSubmitBundle()
{
    // create the diagnostics bundle manifest to collect the session information
    string path = await cloudSpatialAnchorSession
                              .Diagnostics
                              .CreateManifestAsync("Description of the issue");

    // submit the manifest and data to send feedback to Microsoft
    await cloudSpatialAnchorSession.Diagnostics.SubmitManifestAsync(path);
}
```

### <a name="parts-of-a-diagnostics-bundle"></a>Bir tanılama paketi 'nin parçaları
Tanılama paketi aşağıdaki bilgileri içerebilir:

- **Ana kare görüntüleri**: Tanılama etkinken, oturum sırasında yakalanan ortamın görüntüleri.
- **Günlükler**: çalışma zamanı tarafından kaydedilen günlük olayları.
- **Oturum meta verileri**: oturumu tanımlayan meta veriler.