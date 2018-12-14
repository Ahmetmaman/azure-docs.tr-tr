---
title: El ile olmayan HTTP ile tetiklenen bir Azure işlevleri'ni çalıştırma
description: Azure işlevleri HTTP olmayan bir HTTP isteği kullanım tetiklendi
services: functions
keywords: ''
author: craigshoemaker
manager: jeconnoc
ms.service: azure-functions
ms.topic: tutorial
ms.date: 12/12/2018
ms.author: cshoe
ms.openlocfilehash: eba0901561b7c1455b7b06849e1773629d9d357f
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53388036"
---
# <a name="manually-run-a-non-http-triggered-function"></a>El ile olmayan HTTP ile tetiklenen bir işlev çalıştırın

Bu makalede, olmayan HTTP ile tetiklenen bir işlev özel olarak biçimlendirilmiş bir HTTP isteği aracılığıyla el ile çalıştırabileceğiniz gösterilmiştir.

Bazı bağlamlarda "isteğe bağlı" çalıştırmanız gerekebilir dolaylı olarak tetikleyen bir Azure işlevi.  Dolaylı Tetikleyicileri örnekler [işlevleri bir zamanlamaya göre](./functions-create-scheduled-function.md) veya sonucu olarak çalıştırılan işlevleri [başka bir kaynağın eylem](./functions-create-storage-blob-triggered-function.md). 

[Postman](https://www.getpostman.com/) aşağıdaki örnekte kullanılır ancak kullanabilirsiniz [cURL](https://curl.haxx.se/), [Fiddler](https://www.telerik.com/fiddler) veya HTTP istekleri göndermek için aracı gibi diğer.

## <a name="define-the-request-location"></a>İstek konumunu tanımlayın

Olmayan HTTP ile tetiklenen bir işlev çalıştırmak için Azure için bir istek göndermek için bir yol işlevi çalıştırmak için gerekir. Bu istek için kullanılan URL'nin, belirli bir form alır.

![İstek konumunu tanımlayın: ana bilgisayar adı, klasör yolu + işlev adı](./media/functions-manually-run-non-http/azure-functions-admin-url-anatomy.png)

- **Ana bilgisayar adı:** Adı işlevi uygulamanın artı oluşur ortak bir konum işlevi uygulamanın *azurewebsites.net* veya özel etki alanınız.
- **Klasör yolu:** Klasörler üzerinden istek göndermek zorunda olmayan HTTP ile tetiklenen işlev bir HTTP isteği aracılığıyla erişmek için *yönetici/işlevleri*.
- **İşlev adı:** Çalıştırmak istediğiniz işlev adı.

Postman isteği bu konumda Azure isteğinde işlevin ana anahtarı ile birlikte, işlev çalıştırmak için kullanın.

## <a name="get-the-functions-master-key"></a>İşlevin ana anahtarı alma

Azure Portal'da işlevinize gidin ve tıklayarak **Yönet** ve bulma **ana bilgisayar anahtarları** bölümü. Tıklayarak **kopyalama** düğmesine *ana* ana anahtarı panonuza kopyalamak için satır.

![İşlev yönetim ekranından ana anahtarı Kopyala](./media/functions-manually-run-non-http/azure-portal-functions-master-key.png)

Ana anahtarı kopyaladıktan sonra kod dosyası penceresine dönmek için işlev adına tıklayın. Ardından, tıklayarak **günlükleri** sekmesi. Postman ' el ile işlevi çalıştırdığınızda, burada oturum işlevden mesajlar görürsünüz.

## <a name="call-the-function"></a>İşlev çağrısı

Postman'ı açın ve aşağıdaki adımları izleyin:

1. Girin **URL'si metin kutusuna bir konum ister**. 
2. **Tıklayın** üzerinde **üstbilgileri** sekmesi.
3. Girin **x-işlevler-key** ilk olarak **anahtarı** ve ana anahtarı (panodan) yapıştırın **değer** kutusu.
4. Girin **Content-Type** ikinci olarak **anahtarı** girin **application/json** olarak **değer**.

    ![Postman üstbilgi ayarları](./media/functions-manually-run-non-http/functions-manually-run-non-http-headers.png)

5. **Tıklayın** üzerinde **gövdesi** sekmesi.
6. Girin **{"Giriş": "test"}** istek gövdesi olarak.

    ![Postman gövdesi ayarları](./media/functions-manually-run-non-http/functions-manually-run-non-http-body.png)

7. Tıklayın **Gönder**.

    ![Postman isteği gönderme](./media/functions-manually-run-non-http/functions-manually-run-non-http-send.png)

Postman, daha sonra durumunu raporlar **202 kabul edildi**.

Ardından, Azure Portal'da işlevinize geri dönün. Bulun *günlükleri* penceresindeki el ile işlev çağrısından gelen iletileri göreceksiniz.

![El ile çağrı günlüğü sonuçlardan işlevi](./media/functions-manually-run-non-http/azure-portal-function-log.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Kodunuzu Azure işlevleri'nde test stratejileri](./functions-test-a-function.md)
- [Azure işlevi olay Kılavuzu tetikleyicisi yerel hata ayıklama](./functions-debug-event-grid-trigger-local.md)