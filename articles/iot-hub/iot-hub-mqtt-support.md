---
title: Azure IOT hub'ı MQTT desteği anlama | Microsoft Docs
description: Geliştirici Kılavuzu - ve MQTT protokolünü kullanarak bir IOT Hub cihaz'e yönelik uç noktasına bağlanan cihazlar için destek. Yerleşik MQTT desteği Azure IOT cihaz SDK'ları hakkında bilgi içerir.
author: rezasherafat
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/12/2018
ms.author: rezas
ms.openlocfilehash: d1214df922e8e656ba2ff566571d878b0031fea9
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2019
ms.locfileid: "54000266"
---
# <a name="communicate-with-your-iot-hub-using-the-mqtt-protocol"></a>Ve MQTT protokolünü kullanarak IOT hub ile iletişim

IOT hub'ı kullanarak IOT Hub cihaz uç noktaları ile iletişim kurmak cihazları sağlar:

* [MQTT v3.1.1] [ lnk-mqtt-org] 8883 numaralı bağlantı noktasında
* Bağlantı noktası 443 üzerinden WebSocket üzerinden MQTT v3.1.1.

IOT hub'ı, tam özellikli bir MQTT aracı değildir ve MQTT v3.1.1 standardında belirtilen tüm davranışları desteklemez. Bu makalede, cihazlar IOT Hub ile iletişim kurmak için MQTT davranışları desteklenen nasıl kullanabileceğinizi açıklar.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

IOT Hub ile tüm cihaz iletişimi TLS/SSL kullanarak güvenli hale getirilmelidir. Bu nedenle, IOT hub'ı bağlantı noktası üzerinden 1883 güvenli olmayan bağlantılara desteklemiyor.

## <a name="connecting-to-iot-hub"></a>IOT hub'ına bağlama

Bir cihaz ve MQTT protokolünü kullanarak bir IOT hub'ı bağlanmak için kullanabilirsiniz:

* Ya da kitaplıkları [Azure IOT SDK'ları][lnk-device-sdks].
* Veya doğrudan ve MQTT protokolünü.

## <a name="using-the-device-sdks"></a>Cihaz SDK'ları kullanma

[Cihaz SDK'ları] [ lnk-device-sdks] destekleyen ve MQTT protokolünü Java, Node.js, C, C# ve Python için kullanılabilir. Cihaz SDK'ları, bir IOT hub'ına bağlantı kurmak için standart IOT Hub bağlantı dizesini kullanın. MQTT protokolünü kullanmak için istemci protocol parametresi ayarlanmalıdır **MQTT**. Varsayılan olarak, bir IOT Hub SDK'ları cihazı bağlamak **CleanSession** bayrağı ayarlanmış **0** ve **QoS 1** IOT hub ile ileti değişimi için.

Cihaz SDK'ları bir cihaz IOT hub'a bağlandığında, cihazın IOT hub ile ileti alışverişi sağlayan yöntemler sağlar.

Aşağıdaki tabloda, desteklenen her dil için kod örnekleri bağlantılarını içerir ve MQTT protokolünü kullanarak IOT hub'a bağlantı kurmak için kullanılacak parametre belirtir.

| Dil | Protocol parametresi |
| --- | --- |
| [Node.js][lnk-sample-node] |azure-iot-device-mqtt |
| [Java][lnk-sample-java] |IotHubClientProtocol.MQTT |
| [C][lnk-sample-c] |MQTT_Protocol |
| [C#][lnk-sample-csharp] |TransportType.Mqtt |
| [Python][lnk-sample-python] |IoTHubTransportProvider.MQTT |

### <a name="migrating-a-device-app-from-amqp-to-mqtt"></a>Bir cihaz uygulaması için MQTT AMQP geçiş

Kullanıyorsanız [cihaz SDK'ları][lnk-device-sdks], için MQTT, AMQP kullanarak geçiş daha önce belirtildiği gibi istemci Başlatma Protokolü parametreyi değiştirmeyi gerektirir.

Bunun yapılması, aşağıdaki öğeleri denetlemek emin olun:

* AMQP MQTT bağlantıyı sonlandırır birçok koşulları için hataları döndürür. Sonuç olarak, özel durum işleme mantığı, bazı değişiklikler gerektirebilir.
* MQTT desteği içermez *Reddet* alındığında operations [bulut-cihaz iletilerini][lnk-messaging]. Arka uç uygulamanızın cihaz uygulamasından bir yanıt almanız gerekiyorsa, kullanmayı [doğrudan yöntemler][lnk-methods].

## <a name="using-the-mqtt-protocol-directly"></a>Ve MQTT protokolünü kullanarak doğrudan

Bir cihaz, cihaz SDK'ları kullanamıyorsanız, bağlantı noktası 8883 ve MQTT protokolünü kullanarak genel cihaz uç noktalarına yine de bağlanabilir. İçinde **CONNECT** paket cihaz, aşağıdaki değerleri kullanmalıdır:

* İçin **ClientID** alan, kullanın **DeviceID**.

* İçin **kullanıcıadı** alan, kullanın `{iothubhostname}/{device_id}/api-version=2018-06-30`burada `{iothubhostname}` IOT hub'ın tam CNAME.

    Örneğin, IOT hub'ınızın adını ise **contoso.azure-devices.net** ve cihazınızın adına **MyDevice01**, tam **kullanıcı adı** alanı içermelidir:

    `contoso.azure-devices.net/MyDevice01/api-version=2018-06-30`

* İçin **parola** alan, bir SAS belirteci kullanabilir. SAS belirteci biçimi HTTPS ve AMQP protokolleri aynıdır:

  `SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`

  > [!NOTE]
  > X.509 sertifika kimlik doğrulaması kullanıyorsanız, SAS belirteci parola gerekli değildir. Daha fazla bilgi için [Azure IOT hub'ınızdaki X.509 güvenliği][lnk-x509]

  Cihaz bölümünü SAS belirteçleri oluşturmak nasıl hakkında daha fazla bilgi için bkz. [kullanarak IOT Hub güvenlik belirteçleri][lnk-sas-tokens].

  Test ederken, platformlar arası de kullanabilirsiniz [Visual Studio Code için Azure IOT hub'ı Toolkit uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (eski adıyla Azure IOT Toolkit uzantısını) veya [Device Explorer] [ lnk-device-explorer]aracını hızla kendi kodu kopyalayıp bir SAS belirteci oluşturmak için:

Azure IOT hub'ı Araç Seti için:

  1. Genişletin **AZURE IOT HUB CİHAZLARI** Visual Studio Code, sol alt köşedeki sekmesi.
  2. Cihazınızı sağ tıklayıp **oluşturmak SAS belirteci için cihaz**.
  3. Ayarlama **süre sonu** 'Enter' tuşuna basın.
  4. SAS belirteci oluşturulur ve panoya kopyalandı.

Device Explorer için:

  1. Git **Yönetim** sekmesinde **Device Explorer**.
  2. Tıklayın **SAS belirteci** (üst, sağ).
  3. Üzerinde **SASTokenForm**, Cihazınızı seçin **DeviceID** açılır. Ayarlama, **TTL**.
  4. Tıklayın **Oluştur** belirtecinizi oluşturmak için.

     Oluşturulan SAS belirteci aşağıdaki yapıya sahiptir:

     `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`

     Bir parçası olarak kullanmak için bu belirteci **parola** MQTT kullanarak bağlanmak için gereken alan:

     `SharedAccessSignature sr={your hub name}.azure-devices.net%2Fdevices%2FMyDevice01%2Fapi-version%3D2016-11-14&sig=vSgHBMUG.....Ntg%3d&se=1456481802`

MQTT için bağlanın ve paketleri kesmek, IOT Hub üzerinde bir olayın sorunlarını **izleme işlemleri** kanal. Bu olay, bağlantı sorunlarını gidermenize yardımcı olabilecek ek bilgiler vardır.

Cihaz uygulamasını belirtebilirsiniz bir **olacak** içinde ileti **CONNECT** paket. Cihaz uygulamasını kullanmanız gerekir `devices/{device_id}/messages/events/` veya `devices/{device_id}/messages/events/{property_bag}` olarak **olacak** tanımlamak için konu adı **olacak** iletileri bir telemetri iletisi olarak iletilir. Bu durumda, ağ bağlantısı kapalı ise, ancak bir **Bağlantıyı Kes** paket CİHAZDAN daha önce alınmadı, sonra IOT hub'a gönderir **olacak** ileti sağlanan **BAĞLAN** paket telemetri kanalı. Telemetri kanal ya da varsayılan olabilir **olayları** uç nokta veya yönlendirme IOT Hub tarafından tanımlanan özel bir uç nokta. İletinin **iothub-MessageType** özellik değeriyle **olacak** atanmış.

### <a name="tlsssl-configuration"></a>TLS/SSL yapılandırması

Kullanılacak MQTT protokolünü doğrudan istemcinizi *gerekir* TLS/SSL üzerinden. Bu adımı atlayın girişimleri bağlantı hataları ile başarısız olur.

TLS bağlantı kurulabilmesi için indirme ve DigiCert Baltimore kök sertifikasının başvuru gerekebilir. Bu bağlantının güvenliğini sağlamak için Azure kullanan bir sertifikadır. Bu sertifikanın bulabilirsiniz [Azure-iot-sdk-c] [ lnk-sdk-c-certs] depo. Bu sertifikalar hakkında daha fazla bilgi bulunabilir [Digicert'in Web sitesi][lnk-digicert-root-certs].

Bunu Python sürümünü kullanarak uygulamaya ilişkin bir örnek [Paho MQTT Kitaplığı] [ lnk-paho] Eclipse Foundation tarafından aşağıdaki gibi görünebilir.

İlk olarak, komut satırı ortamınızdan Paho kitaplığını yükleyin:

```cmd/sh
pip install paho-mqtt
```

Ardından, istemci bir Python betiği uygulayın. Yer tutucuları şu şekilde değiştirin:

* `<local path to digicert.cer>` DigiCert Baltimore kök sertifikasını içeren bir yerel dosya yoludur. Sertifika bilgileri kopyalayarak bu dosyayı oluşturabilirsiniz [certs.c](https://github.com/Azure/azure-iot-sdk-c/blob/master/certs/certs.c) satırları c için Azure IOT SDK'sı dahil `-----BEGIN CERTIFICATE-----` ve `-----END CERTIFICATE-----`, kaldırma `"` başlangıcına ve her bir satır sonu işareti ve kaldırma `\r\n` her satırın sonunda bir karakter.
* `<device id from device registry>` bir cihaz IOT hub'ınıza eklediğiniz kimliğidir.
* `<generated SAS token>` Bu makalede daha önce açıklandığı gibi oluşturduğunuz cihaz için bir SAS belirteci ' dir.
* `<iot hub name>` IOT hub'ınızın adıdır.

```python
from paho.mqtt import client as mqtt
import ssl

path_to_root_cert = "<local path to digicert.cer>"
device_id = "<device id from device registry>"
sas_token = "<generated SAS token>"
iot_hub_name = "<iot hub name>"

def on_connect(client, userdata, flags, rc):
  print ("Device connected with result code: " + str(rc))
def on_disconnect(client, userdata, rc):
  print ("Device disconnected with result code: " + str(rc))
def on_publish(client, userdata, mid):
  print ("Device sent message")

client = mqtt.Client(client_id=device_id, protocol=mqtt.MQTTv311)

client.on_connect = on_connect
client.on_disconnect = on_disconnect
client.on_publish = on_publish

client.username_pw_set(username=iot_hub_name+".azure-devices.net/" + device_id, password=sas_token)

client.tls_set(ca_certs=path_to_root_cert, certfile=None, keyfile=None, cert_reqs=ssl.CERT_REQUIRED, tls_version=ssl.PROTOCOL_TLSv1, ciphers=None)
client.tls_insecure_set(False)

client.connect(iot_hub_name+".azure-devices.net", port=8883)

client.publish("devices/" + device_id + "/messages/events/", "{id=123}", qos=1)
client.loop_forever()
```

### <a name="sending-device-to-cloud-messages"></a>CİHAZDAN buluta ileti gönderme

Başarılı bir bağlantı yaptıktan sonra bir cihaz IOT Hub'ına kullanarak iletileri gönderebilirsiniz `devices/{device_id}/messages/events/` veya `devices/{device_id}/messages/events/{property_bag}` olarak bir **konu adı**. `{property_bag}` Öğesi bir url olarak kodlanmış biçimde ek özelliklerle iletileri göndermek cihaz sağlar. Örneğin:

```text
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [!NOTE]
> Bu `{property_bag}` öğe aynı kodlama için sorgu dizelerine HTTPS protokolünü kullanır.

IOT hub'ı uygulamaya özel davranışların bir listesi verilmiştir:

* QoS 2 iletileri IOT hub'ı desteklemez. Bir cihaz uygulaması içeren bir ileti yayımlar, **QoS 2**, IOT Hub ağ bağlantıyı kapatır.
* IOT hub'ı tut iletileri devam etmez. Bir CİHAZDAN bir ileti ile gönderilen **TUT** bayrağı, IOT hub'ı ekler 1 olarak ayarlayın **x-opt-korumak** iletisi uygulama özelliği. Bu durumda, tut ileti kalıcı yerine IOT hub'ı arka uç uygulamasına geçirir.
* IOT Hub, cihaz başına bir etkin MQTT bağlantısı yalnızca destekler. Herhangi bir yeni MQTT bağlantısı cihaz kimliği aynı adına, IOT Hub'ın var olan bağlantıyı kesmek neden olur.

Daha fazla bilgi için [Geliştirici Kılavuzu Mesajlaşma][lnk-messaging].

### <a name="receiving-cloud-to-device-messages"></a>Bulut-cihaz iletilerini alma

IOT Hub'ından iletiler almak için bir cihaz kullanarak abone olmalıdır `devices/{device_id}/messages/devicebound/#` olarak bir **konu filtresi**. Çok düzeyli joker `#` konu filtresinde yalnızca ek özellikler konu adı almak cihazın izin vermek için kullanılır. IOT hub'ı kullanımına izin verme `#` veya `?` alt konularını filtreleme için joker karakter. IOT Hub genel amaçlı pub-sub Mesajlaşma Aracısı olmadığından, yalnızca belgelenmiş konu adları ve konu başlığı filtreleri destekler.

Cihaz herhangi bir ileti almazsa, cihaza özel uç noktasına başarıyla abone olduğu kadar IOT Hub'ından tarafından temsil edilen `devices/{device_id}/messages/devicebound/#` konu Filtresi. Bir abonelik oluşturulduktan sonra cihazın kendisine aboneliğin sonra gönderilen bulut-cihaz iletilerini alır. Cihaz ile bağlanıyorsa **CleanSession** bayrağı ayarlanmış **0**, abonelik farklı oturumlarda kalıcı hale getirilir. Cihaz bu durumda, sonraki açışınızda bağlanır ile **CleanSession 0** bağlı değilken gönderilen tüm bekleyen iletileri alır. Cihaz kullanıyorsa **CleanSession** bayrağı ayarlanmış **1** için cihaz uç noktası, abone kadar bu iletileri IOT Hub'ından almaz.

IOT Hub ile ileti sunar **konu adı** `devices/{device_id}/messages/devicebound/`, veya `devices/{device_id}/messages/devicebound/{property_bag}` ileti özelliklerini olduğunda. `{property_bag}` İleti özelliklerini URL olarak kodlanmış bir anahtar/değer çiftleri içerir. Yalnızca uygulama özellikleri ve kullanıcı ayarlanabilir Sistem Özellikleri (gibi **MessageID** veya **Correlationıd**) özellik paketindeki dahil edilir. Sistem özellik adlarına sahip önek **$**, uygulama özellikleri özgün özellik adı ile önek kullanın.

Bir konu ile bir cihaz uygulaması zaman aboneliği **QoS 2**, IOT hub'ı en fazla QoS düzey 1 içinde verir **SUBACK** paket. Bundan sonra IOT Hub iletilerini cihazda QoS 1 kullanarak sunar.

### <a name="retrieving-a-device-twins-properties"></a>Bir cihaz ikizinin özellikleri alınıyor

Bir cihaz ilk olarak, abone `$iothub/twin/res/#`, işlemin yanıt almak için. Ardından, konuya boş bir ileti gönderir `$iothub/twin/GET/?$rid={request id}`, için doldurulmuş bir değerle **istek kimliği**. Hizmet ardından konuyla ilgili cihaz ikizi verileri içeren bir yanıt iletisi gönderir `$iothub/twin/res/{status}/?$rid={request id}`, aynı kullanarak **istek kimliği** istek olarak.

İstek Kimliği olarak başına herhangi bir ileti özellik değeri için geçerli bir değer olabilir [IOT Hub Geliştirici Kılavuzu Mesajlaşma][lnk-messaging], ve durum bir tamsayı olarak doğrulandı.

Yanıt gövdesi cihaz çiftinin özellikler bölümü içerir. Aşağıdaki kod parçacığı gövdesi sınırlı kimlik kayıt defteri girişi, örneğin "Özellikler" üyesine gösterir:

```json
{
    "properties": {
        "desired": {
            "telemetrySendFrequency": "5m",
            "$version": 12
        },
        "reported": {
            "telemetrySendFrequency": "5m",
            "batteryLevel": 55,
            "$version": 123
        }
    }
}
```

Olası durum kodları şunlardır:

|Durum | Açıklama |
| ----- | ----------- |
| 200 | Başarılı |
| 429 | Olarak başına (daraltılmış) çok fazla istek [IOT Hub'ın azaltma][lnk-quotas] |
| 5** | Sunucu hataları |

Daha fazla bilgi için [cihaz ikizlerini Geliştirici Kılavuzu][lnk-devguide-twin].

### <a name="update-device-twins-reported-properties"></a>Cihaz ikizinin bildirilen özellikleri güncelleştirmek

Bildirilen özellikleri güncelleştirmek için cihaz isteği IOT Hub'ına yayını belirlenen bir MQTT konu üzerinde yayınlar. İstek işlendikten sonra IOT hub'ı güncelleştirme işlemi aracılığıyla başka bir konuya bir yayın başarı veya başarısızlık durumunu yanıt verir. Bu konu hakkında kendi ikiz güncelleştirmesi isteğinin sonucunu bildirmek için cihaz tarafından abone olabilir. İmplment için MQTT, istek/yanıt etkileşiminde bu tür biz kavramı yararlanarak istek kimliği (`$rid`) başlangıçta güncelleştirme isteğinde cihaz tarafından sağlanan. Bu istek kimliği, IOT Hub'ı, belirli bir önceki isteğin yanıtını ilişkilendirmek cihazın izin vermek için gelen yanıt da bulunmaktadır.

Bir cihaz tarafından bildirilen özellikleri IOT hub'daki cihaz ikizinde güncelleştirmeleri nasıl aşağıdaki sırayı açıklar:

1. Bir cihaz ilk abone olmalısınız `$iothub/twin/res/#` IOT Hub'ından işlemin yanıtlar almak için konu.

1. Bir cihaz için cihaz ikizi güncelleştirme içeren bir ileti gönderir `$iothub/twin/PATCH/properties/reported/?$rid={request id}` konu. Bu iletiyi içeren bir **istek kimliği** değeri.

1. Hizmet ardından konu bildirilen özellikler koleksiyonunda yeni ETag değeri içeren bir yanıt iletisi gönderir `$iothub/twin/res/{status}/?$rid={request id}`. Bu yanıt iletisiyle ndedir **istek kimliği** istek olarak.

İstek iletisi gövdesi, bildirilen özellikleri için yeni değerler içeren bir JSON belgesini içerir. JSON belgesini içindeki her üyenin güncelleştirir veya cihaz ikizinin belgede karşılık gelen bir üye ekleyin. Ayarlanmış bir üye `null`, üyeyi içeren nesneyi siler. Örneğin:

```json
{
    "telemetrySendFrequency": "35m",
    "batteryLevel": 60
}
```

Olası durum kodları şunlardır:

|Durum | Açıklama |
| ----- | ----------- |
| 200 | Başarılı |
| 400 | Hatalı istek. Hatalı biçimlendirilmiş JSON |
| 429 | Olarak başına (daraltılmış) çok fazla istek [IOT Hub'ın azaltma][lnk-quotas] |
| 5** | Sunucu hataları |

Python kodu aşağıdaki kod parçacığında, gösterir ikiz özellikler güncelleştirme işlemi MQTT (Paho MQTT istemci kullanarak) bildirilen:
```python
from paho.mqtt import client as mqtt

# authenticate the client with IoT Hub (not shown here)

client.subscribe("$iothub/twin/res/#")
rid = "1"
twin_reported_property_patch = "{\"firmware_version\": \"v1.1\"}"
client.publish("$iothub/twin/PATCH/properties/reported/?$rid=" + rid, twin_reported_property_patch, qos=0)
```

İkizinin başarılı olduktan sonra bildirilen özellikler güncelleştirme işlemi yukarıdaki, IOT Hub'ından yayın ileti konusuna sahip olur: `$iothub/twin/res/204/?$rid=1&$version=6`burada `204` başarının, durum kodu `$rid=1` istek Kimliğine karşılık gelen kodda, cihaz tarafından sağlanan ve `$version` güncelleştirmesinden sonra cihaz ikizlerini bildirilen özellikler bölümünü sürümüne karşılık gelir.

Daha fazla bilgi için [cihaz ikizlerini Geliştirici Kılavuzu][lnk-devguide-twin].

### <a name="receiving-desired-properties-update-notifications"></a>İstenen özellikleri güncelleştirme bildirimlerini alma

Bir cihaz bağlandığında, IOT hub'ı bildirim konuya gönderir. `$iothub/twin/PATCH/properties/desired/?$version={new version}`, çözüm arka ucu tarafından gerçekleştirilen güncelleştirme içeriğini içerir. Örneğin:

```json
{
    "telemetrySendFrequency": "5m",
    "route": null,
    "$version": 8
}
```

Özellik güncelleştirmeleri, olduğu gibi `null` değerleri, JSON nesne üyesi silindiğini anlamına gelir. Ayrıca, `$version` ikizinin istenen özellikleri bölümünde yeni sürümünü gösterir.

> [!IMPORTANT]
> Yalnızca cihazları bağlı IOT hub'ı değişiklik bildirimleri oluşturur. Uygulamak emin [cihaz yeniden akış] [ lnk-devguide-twin-reconnection] istenen özellikleri IOT Hub cihaz uygulaması arasında eşitlenmiş tutmak için.

Daha fazla bilgi için [cihaz ikizlerini Geliştirici Kılavuzu][lnk-devguide-twin].

### <a name="respond-to-a-direct-method"></a>Doğrudan bir yönteme yanıt

İlk olarak, abone olmak bir cihaz sahip `$iothub/methods/POST/#`. IOT hub'ı konuya yöntemi istekleri gönderen `$iothub/methods/POST/{method name}/?$rid={request id}`, geçerli bir JSON ya da boş bir gövdeye sahip.

Cihaz yanıt vermek için konuya geçerli JSON veya boş gövdesi olan bir ileti gönderir `$iothub/methods/res/{status}/?$rid={request id}`. Bu iletideki **istek kimliği** bir istek iletisi eşleşmelidir ve **durumu** bir tamsayı olmalıdır.

Daha fazla bilgi için [yöntemi Geliştirici Kılavuzu doğrudan][lnk-methods].

### <a name="additional-considerations"></a>Diğer konular

Son yaparken, bulut tarafındaki MQTT protokolünü davranışını özelleştirmek gerekiyorsa gözden geçirmeniz gereken [Azure IOT protokolü ağ geçidini][lnk-azure-protocol-gateway]. Bu yazılım, yüksek performanslı özel protokolü ağ geçidini dağıtmak için IOT hub'ı ile doğrudan arabirim sağlar. Azure IOT protokolü ağ geçidini brownfield MQTT dağıtımları uyum sağlamak için cihaz protokolü veya diğer özel protokoller özelleştirmenize olanak sağlar. Bu yaklaşım, ancak çalıştırın ve bir özel protokol ağ geçidi çalışması gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

Ve MQTT protokolünü hakkında daha fazla bilgi için bkz: [MQTT belgeleri][lnk-mqtt-docs].

IOT Hub dağıtımınızı planlama hakkında daha fazla bilgi için bkz:

* [Azure IOT için sertifikalı cihaz Kataloğu][lnk-devices]
* [Ek protokol desteği][lnk-protocols]
* [Event Hubs ile karşılaştırma][lnk-compare]
* [Ölçeklendirme, HA ve DR][lnk-scaling]

Daha fazla IOT Hub'ın özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu][lnk-devguide]
* [Azure IOT Edge ile sınır cihazlarına Al dağıtma][lnk-iotedge]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdk-node/blob/master/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/sdk/iot/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt_dm
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/iothub/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdk-python/tree/master/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer
[lnk-sas-tokens]: iot-hub-devguide-security.md#use-sas-tokens-in-a-device-app
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsuite.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-x509]: iot-hub-security-x509-get-started.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md

[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-twin-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow
[lnk-devguide-twin]: iot-hub-devguide-device-twins.md
[lnk-sdk-c-certs]: https://github.com/Azure/azure-iot-sdk-c/blob/master/certs/certs.c
[lnk-digicert-root-certs]: https://www.digicert.com/digicert-root-certificates.htm
[lnk-paho]: https://pypi.python.org/pypi/paho-mqtt
