---
title: PHP ile Azure Notification Hubs kullanma
description: Azure Notification Hubs bir PHP arka ucundan nasıl kullanacağınızı öğrenin.
services: notification-hubs
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: php
ms.devlang: php
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: daebf7c6a5fc9056e16b77a40ee9f90db598749f
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "87076627"
---
# <a name="how-to-use-notification-hubs-from-php"></a>PHP 'den Notification Hubs kullanma

[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Bir Java/PHP/Ruby arka ucundan, MSDN konusunda [NOTIFICATION HUBS REST API 'lerinde](/previous-versions/azure/reference/dn223264(v=azure.100))açıklandığı gibi Bildirim Hub 'ı Rest arabirimini kullanarak tüm Notification Hubs özelliklerine erişebilirsiniz.

Bu konuda nasıl yapılacağını göstereceğiz:

* PHP 'de Notification Hubs özellikleri için bir REST istemcisi oluşturun;
* [Azure Notification Hubs kullanarak iOS uygulamalarına anında iletme bildirimleri gönderin](ios-sdk-get-started.md) ' i tercih ettiğiniz mobil platformunuz IÇIN, php 'de arka uç bölümünü uygulayarak izleyin.

## <a name="client-interface"></a>İstemci arabirimi

Ana istemci arabirimi, bu sitede mevcut olan tüm öğreticileri ve örnekleri doğrudan çevirebilmeniz ve Internet 'teki topluluk tarafından katkıda bulungirmenize olanak sağlayan [.net Notification Hubs SDK 'sında](https://msdn.microsoft.com/library/jj933431.aspx)kullanılabilen yöntemlerin aynısını sağlayabilir.

[Php Rest sarmalayıcı örneğinde]bulunan tüm kodu bulabilirsiniz.

Örneğin, bir istemci oluşturmak için:

```php
$hub = new NotificationHub("connection string", "hubname");
```

İOS yerel bildirimi göndermek için:

```php
$notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
$hub->sendNotification($notification, null);
```

## <a name="implementation"></a>Uygulama

Henüz yapmadıysanız, arka ucunu uygulamak istediğiniz son bölüme kadar [kullanmaya başlama öğreticisi] izleyin.
Ayrıca, [php Rest sarmalayıcı örneğindeki] kodu kullanabilir ve doğrudan [öğreticiyi tamamlayın](#complete-tutorial) bölümüne gidebilirsiniz.

Tam REST sarmalayıcı uygulamak için tüm ayrıntılar [MSDN](/previous-versions/azure/reference/dn530746(v=azure.100))'de bulunabilir. Bu bölümde, Notification Hubs REST uç noktalarına erişmek için gereken ana adımların PHP uygulamasını açıklıyoruz:

1. Bağlantı dizesini ayrıştırma
2. Yetkilendirme belirtecini oluşturma
3. HTTP çağrısını gerçekleştirme

### <a name="parse-the-connection-string"></a>Bağlantı dizesini ayrıştırma

Aşağıda, bağlantı dizesini çözümleyen Oluşturucu olan istemciyi uygulayan ana sınıf verilmiştir:

```php
class NotificationHub {
    const API_VERSION = "?api-version=2013-10";

    private $endpoint;
    private $hubPath;
    private $sasKeyName;
    private $sasKeyValue;

    function __construct($connectionString, $hubPath) {
        $this->hubPath = $hubPath;

        $this->parseConnectionString($connectionString);
    }

    private function parseConnectionString($connectionString) {
        $parts = explode(";", $connectionString);
        if (sizeof($parts) != 3) {
            throw new Exception("Error parsing connection string: " . $connectionString);
        }

        foreach ($parts as $part) {
            if (strpos($part, "Endpoint") === 0) {
                $this->endpoint = "https" . substr($part, 11);
            } else if (strpos($part, "SharedAccessKeyName") === 0) {
                $this->sasKeyName = substr($part, 20);
            } else if (strpos($part, "SharedAccessKey") === 0) {
                $this->sasKeyValue = substr($part, 16);
            }
        }
    }
}
```

### <a name="create-a-security-token"></a>Güvenlik belirteci oluşturma

[SAS güvenlik belirteci oluşturma](/previous-versions/azure/reference/dn495627(v=azure.100)#create-sas-security-token)hakkında bilgi için Azure belgelerine bakın.

`generateSasToken` `NotificationHub` GEÇERLI isteğin URI 'sini ve bağlantı dizesinden ayıklanan kimlik bilgilerini temel alarak belirteci oluşturmak için yöntemini sınıfına ekleyin.

```php
private function generateSasToken($uri) {
    $targetUri = strtolower(rawurlencode(strtolower($uri)));

    $expires = time();
    $expiresInMins = 60;
    $expires = $expires + $expiresInMins * 60;
    $toSign = $targetUri . "\n" . $expires;

    $signature = rawurlencode(base64_encode(hash_hmac('sha256', $toSign, $this->sasKeyValue, TRUE)));

    $token = "SharedAccessSignature sr=" . $targetUri . "&sig="
                . $signature . "&se=" . $expires . "&skn=" . $this->sasKeyName;

    return $token;
}
```

### <a name="send-a-notification"></a>Bildirim gönder

İlk olarak, bir bildirimi temsil eden bir sınıf tanımlamamıza izin verin.

```php
class Notification {
    public $format;
    public $payload;

    # array with keynames for headers
    # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
    # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
    public $headers;

    function __construct($format, $payload) {
        if (!in_array($format, ["template", "apple", "windows", "fcm", "windowsphone"])) {
            throw new Exception('Invalid format: ' . $format);
        }

        $this->format = $format;
        $this->payload = $payload;
    }
}
```

Bu sınıf, yerel bir bildirim gövdesi için bir kapsayıcıdır veya bir şablon bildirimi olması durumunda bir dizi özellik ve biçim (yerel platform veya şablon) ve platforma özgü özellikler (Apple Expiration özelliği ve WNS üstbilgileri gibi) içeren bir üst bilgi kümesi.

Kullanılabilir tüm seçenekler için [NOTIFICATION HUBS REST API belgelerine](/previous-versions/azure/reference/dn495827(v=azure.100)) ve belirli bildirim platformlarına yönelik biçimlere bakın.

Bu sınıfla birlikte, artık sınıfın içine gönder bildirim yöntemlerini yazabiliriz `NotificationHub` :

```php
public function sendNotification($notification, $tagsOrTagExpression="") {
    if (is_array($tagsOrTagExpression)) {
        $tagExpression = implode(" || ", $tagsOrTagExpression);
    } else {
        $tagExpression = $tagsOrTagExpression;
    }

    # build uri
    $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
    $ch = curl_init($uri);

    if (in_array($notification->format, ["template", "apple", "fcm"])) {
        $contentType = "application/json";
    } else {
        $contentType = "application/xml";
    }

    $token = $this->generateSasToken($uri);

    $headers = [
        'Authorization: '.$token,
        'Content-Type: '.$contentType,
        'ServiceBusNotification-Format: '.$notification->format
    ];

    if ("" !== $tagExpression) {
        $headers[] = 'ServiceBusNotification-Tags: '.$tagExpression;
    }

    # add headers for other platforms
    if (is_array($notification->headers)) {
        $headers = array_merge($headers, $notification->headers);
    }

    curl_setopt_array($ch, array(
        CURLOPT_POST => TRUE,
        CURLOPT_RETURNTRANSFER => TRUE,
        CURLOPT_SSL_VERIFYPEER => FALSE,
        CURLOPT_HTTPHEADER => $headers,
        CURLOPT_POSTFIELDS => $notification->payload
    ));

    // Send the request
    $response = curl_exec($ch);

    // Check for errors
    if($response === FALSE){
        throw new Exception(curl_error($ch));
    }

    $info = curl_getinfo($ch);

    if ($info['http_code'] <> 201) {
        throw new Exception('Error sending notification: '. $info['http_code'] . ' msg: ' . $response);
    }
} 
```

Yukarıdaki yöntemler `/messages` , bildirimi göndermek için doğru gövde ve üstbilgilere sahip olan Notification Hub 'ının uç noktasına BIR http post isteği gönderir.

## <a name="complete-the-tutorial"></a><a name="complete-tutorial"></a>Öğreticiyi doldurun

Artık bir PHP arka ucunun bildirimini göndererek başlangıç öğreticisini tamamlayabilirsiniz.

Notification Hubs istemcinizi başlatın (bağlantı dizesini ve hub adını [Başlangıç öğreticisinde belirtildiği gibi değiştirin):

```php
$hub = new NotificationHub("connection string", "hubname");
```

Ardından, hedef mobil platformunuza bağlı olarak gönderme kodunu ekleyin.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows Mağazası ve Windows Phone 8,1 (Silverlight olmayan)

```php
$toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
$notification = new Notification("windows", $toast);
$notification->headers[] = 'X-WNS-Type: wns/toast';
$hub->sendNotification($notification, null);
```

### <a name="ios"></a>iOS

```php
$alert = '{"aps":{"alert":"Hello from PHP!"}}';
$notification = new Notification("apple", $alert);
$hub->sendNotification($notification, null);
```

### <a name="android"></a>Android

```php
$message = '{"data":{"msg":"Hello from PHP!"}}';
$notification = new Notification("fcm", $message);
$hub->sendNotification($notification, null);
```

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8,0 ve 8,1 Silverlight

```php
$toast = '<?xml version="1.0" encoding="utf-8"?>' .
            '<wp:Notification xmlns:wp="WPNotification">' .
               '<wp:Toast>' .
                    '<wp:Text1>Hello from PHP!</wp:Text1>' .
               '</wp:Toast> ' .
            '</wp:Notification>';
$notification = new Notification("windowsphone", $toast);
$notification->headers[] = 'X-WindowsPhone-Target : toast';
$notification->headers[] = 'X-NotificationClass : 2';
$hub->sendNotification($notification, null);
```

### <a name="kindle-fire"></a>Lütfen yangın ateş

```php
$message = '{"data":{"msg":"Hello from PHP!"}}';
$notification = new Notification("adm", $message);
$hub->sendNotification($notification, null);
```

PHP kodunuzun çalıştırılması artık hedef cihazınızda bir bildirim görünür.

## <a name="next-steps"></a>Sonraki Adımlar

Bu konu başlığında, Notification Hubs için basit bir Java REST istemcisi oluşturmayı gösterdik. Burada yapabilecekleriniz:

* Yukarıdaki tüm kodu içeren tam [php Rest sarmalayıcı örneğini]indirin.
* [Son Haberler öğreticisinde Notification Hubs etiketleme özelliğini öğrenmeye devam edin
* [Kullanıcıları bilgilendir öğreticisini bildirme] içindeki bireysel kullanıcılara yönelik bildirimleri iletme hakkında bilgi edinin

Daha fazla bilgi için bkz. [php Geliştirici Merkezi](https://azure.microsoft.com/develop/php/).

[PHP REST sarmalayıcı örneği]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Azure Notification Hubs kullanarak iOS uygulamalarına anında iletme bildirimleri gönderme](ios-sdk-get-started.md))
