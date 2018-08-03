---
title: Tanılama ve sorun giderme için Azure IOT hub'ı ile bağlantısını keser
description: Tanılama ve Azure IOT Hub için cihaz bağlantısı ile sık karşılaşılan hataları giderme hakkında bilgi edinin
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/19/2018
ms.author: jlian
ms.openlocfilehash: 91e435c60a342768093b3bc869a78fa61df8782f
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39446573"
---
# <a name="detect-and-troubleshoot-disconnects-with-azure-iot-hub"></a>Algılama ve giderme Azure IOT Hub ile bağlantısını keser

IOT cihazları için bağlantı sorunları olmadığı için birçok olası hata noktaları sorun giderme zor olabilir. Aygıt tarafı uygulama mantığı, fiziksel ağları, protokolleri, donanım ve Azure IOT hub'ı tüm sorunlara neden olabilir. Bu belge, algılamak ve bulut tarafındaki (aksine, aygıt tarafı) cihaz bağlantısı sorunlarını giderme hakkında öneriler sağlar.

## <a name="get-alerts-and-error-logs"></a>Uyarıları ve Hata günlüklerini alma

Azure İzleyici uyarı alın ve cihaz bağlantılarını düşürdüğünüzde, günlükleri yazmak için kullanın.

### <a name="turn-on-diagnostic-logs"></a>Tanılama günlüklerini açın 

Cihaz bağlantı olayları ve hataları günlüğe kaydetmek için IOT hub'ı için tanılamayı açın. 

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. IoT Hub'ınıza gidin.
1. Seçin **tanılama ayarları**.
1. Ardından **tanılamayı Aç**.
1. Etkinleştirdiğinizden emin olun **bağlantıları** toplanacak günlükleri. 
1. Analizi kolaylaştırmak için açmanız **Log Analytics'e gönderme** ([fiyatlandırmaya](https://azure.microsoft.com/pricing/details/log-analytics/)). Daha sonra bu makaledeki örnek Log Analytics kullanır.

   ![Önerilen ayarlar][2]

Daha fazla bilgi için bkz. [Azure IOT Hub durumunu izleyin ve sorunları hızla tanılayın](iot-hub-monitor-resource-health.md).

### <a name="set-up-alerts-for-the-connected-devices-count-metric"></a>Bağlı cihazların sayısı ölçüm için uyarıları ayarlama

Cihazları bağlantısını kestiğinizde uyarıları almak için uyarıları yapılandırın *bağlı cihazları* ölçümü. 

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. IOT Hub'ınıza gidin.
1. Seçin **uyarılar (Klasik)**.
1. Tıklayın **ölçüm uyarısı Ekle (Klasik)**.
1. Seçin ve formu doldurun **Tamam**. 

   ![Önerilen bir ölçüm Uyarısı][3]

Daha fazla bilgi için bkz. [Microsoft azure'da Klasik uyarılar nedir?](../monitoring-and-diagnostics/monitoring-overview-alerts.md).

## <a name="resolve-connectivity-errors"></a>Bağlantı hataları çözün

Tanılama günlükleri ve uyarılar bağlı cihazlar için etkinleştirildiğinde, bir sorun olduğunda uyarı alın. Bu bölümde, bir uyarı aldığınızda, sık karşılaşılan sorunları çözmek açıklar. Aşağıdaki adımları, tanılama günlükleriniz için Log Analytics'i ayarlama ayarlamış olduğunuz varsayılır. 

1. Çalışma alanınızı gitmek **Log Analytics** Azure portalında.
1. Tıklayın **günlük arama**.
1. IOT Hub için bağlantı Hata günlüklerini ayırmak için bu sorguyu girin kutusuna tuşuna **çalıştırma**.

    ```
    search *
    | where ( Type == "AzureDiagnostics" and ResourceType == "IOTHUBS")
    | where ( Category == "Connections" and Level == "Error")
    ```

1. Sonuç yoksa Ara `OperationName`, `ResultType` (hata kodu) ve `ResultDescription` (hata hakkında daha fazla ayrıntı almak için hata iletisi).

   ![Hata günlüğü örneği][4]

1. Ortak hatalarını anlama ve çözme için bu tabloyu kullanın.

    | Hata | Kök neden | Çözüm |
    |---------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | 404104 DeviceConnectionClosedRemotely | Cihaz tarafından bağlantı kapatıldı ancak IOT Hub neden bilmez. Yaygın nedenler MQTT/AMQP zaman aşımı ve internet bağlantısı kaybı. | Cihaz IOT hub'ına bağlanabilir olduğundan emin olun [bağlantı test ediliyor](tutorial-connectivity.md). Bağlantı bir sakınca yoktur ancak cihaz aralıklı olarak kesiliyor Protokolü (MQTT/AMPQ) tercih ettiğiniz için uygun canlı tutma cihaz mantığını emin olun. |
    | 401003 IoTHubUnauthorized | IOT Hub bağlantı doğrulanamıyor. | SAS ya da kullandığınız diğer güvenlik belirteci süresi olmadığından emin olun. [Azure IOT SDK'ları](iot-hub-devguide-sdks.md) otomatik olarak özel bir yapılandırma gerektirmeden belirteçleri oluşturun. |
    | 409002 LinkCreationConflict | Aynı cihazda birden fazla bağlantılarında vardır. Yeni bir bağlantı isteği için bir cihaz söz konusu olduğunda, bu hata önceki bir IOT hub'ı kapatır. | En yaygın durumda, bir cihaz bir bağlantıyı kes algılar ve bağlantıyı yeniden kurmak çalışır, ancak önceki bağlantıyı kapatır ve bu hata günlükleri henüz bağlı IOT hub'ı kabul edilmemiş. Bu hata genellikle bir yan etkisi olarak farklı geçici bir sorun ortaya çıkar kadar başka hatalar da daha fazla sorun giderme için günlüklere bakın. Aksi takdirde, yeni bir bağlantı isteği bağlantı düşerse verdiğinizden emin olun. |
    | 500001 ServerError | IOT hub'ı bir sunucu tarafı sorunla karşılaşıldı. Büyük olasılıkla sorun geçici olmasıdır. IOT hub'ı takımınızın çalıştığı bakımını yapmayı while [SLA](https://azure.microsoft.com/support/legal/sla/iot-hub/), IOT hub'ı düğümler küçük kümelerine bazen geçici hataların karşılaşırsınız. Cihazınızı sorunları bir düğüme bağlanmaya çalışırken bu hatayı alırsınız. | Geçici hata gidermek için bir yeniden deneme CİHAZDAN sorun. İçin [otomatik olarak yeniden denemeleri yönetmek](iot-hub-reliability-features-in-sdks.md#connection-and-retry), en son sürümünü kullandığınızdan emin olun [Azure IOT SDK'ları](iot-hub-devguide-sdks.md).<br><br>Geçici hata işleme ve yeniden denemeler en iyi uygulama için bkz: [geçici hata işleme](/azure/architecture/best-practices/transient-faults.md).  <br><br>Yeniden denemelerden sonra sorun devam ederse, kontrol [kaynak durumu](iot-hub-monitor-resource-health.md#use-azure-resource-health) ve [Azure durumu](https://azure.microsoft.com/status/history/) IOT Hub bir bilinen sorun olup olmadığını belirlemek için. Varsa herhangi bir bilinen sorun ve sorun devam ederse, [desteğe](https://azure.microsoft.com/support/options/) araştırılması için. |
    | 500008 GenericTimeout | IOT Hub bağlantı isteği zaman aşımına uğramadan önce tamamlanamadı. 500001 ServerError gibi bu hata geçici olabilir değil. | Kök neden ve bu hatayı gidermek 500001 ServerError için sorun giderme adımlarını izleyin.|

## <a name="other-steps-to-try"></a>Diğer adımları deneyin

Yukarıdaki adımları yaramazsa, denemek için daha fazla bazı noktalar şunlardır:

* Sorunlu cihazlara fiziksel olarak veya uzaktan (SSH) gibi erişiminiz varsa izleyin [aygıt tarafı sorun giderme kılavuzu](https://github.com/Azure/azure-iot-sdk-node/wiki/Troubleshooting-Guide-Devices) sorun giderme devam etmek için.
* Cihazlarınızı doğrulayın **etkin** Azure portalında > IOT Hub'ınıza > IOT cihazları.
* Yardım almak [Azure IOT hub'ı Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=azureiothub), [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-iot-hub), veya [Azure Destek](https://azure.microsoft.com/support/options/).

Bu kılavuzda işe yaramazsa herkesin belgelerini geliştirmeye yardımcı olmak için aşağıda bir yorum bırakın.

## <a name="next-steps"></a>Sonraki adımlar

* Geçici bir sorun giderme hakkında daha fazla bilgi için bkz. [geçici hata işleme](/azure/architecture/best-practices/transient-faults.md).
* Azure IOT SDK'sı hakkında daha fazla bilgi edinin ve deneme için bkz. [bağlantı ve Azure IOT Hub cihazı SDK'larını kullanarak güvenilir Mesajlaşma yönetme](iot-hub-reliability-features-in-sdks.md#connection-and-retry).

<!-- Images -->
[1]: ../../includes/media/iot-hub-diagnostics-settings/turnondiagnostics.png
[2]: ./media/iot-hub-troubleshoot-connectivity/diagnostic-settings-recommendation.png
[3]: ./media/iot-hub-troubleshoot-connectivity/metric-alert.png
[4]: ./media/iot-hub-troubleshoot-connectivity/diag-logs.png
