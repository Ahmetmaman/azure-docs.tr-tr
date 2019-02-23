---
title: Azure haritalar harita denetimini kullanma | Microsoft Docs
description: Azure haritalar harita denetimi istemci tarafı Javascript kitaplığı kullanmayı öğrenin.
author: dsk-2015
ms.author: dkshir
ms.date: 10/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 57850f67b56113036cb6cc37e9f1f2694ba9eb8f
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2019
ms.locfileid: "56672738"
---
# <a name="use-the-azure-maps-map-control"></a>Azure haritalar harita denetimini kullanma

Harita denetimi istemci tarafı Javascript kitaplığı, haritalar ve katıştırılmış Azure haritalar işlevselliği web veya mobil uygulama oluşturmak sağlar.

## <a name="create-a-new-map-in-a-web-page"></a>Bir web sayfasında yeni bir eşleme oluşturma

Harita denetimi istemci tarafı Javascript kitaplığını kullanarak bir web sayfasında bir harita ekleyebilir.

1. Yeni bir HTML dosyası oluşturun.

2. Azure haritalar Web SDK'sını yükleyin. Bu yapılabilir; iki seçenekten birini kullanma
    
    a. Stil sayfası ve komut dosyası başvuruları URL uç noktaları ekleyerek Azure haritalar Web SDK'sı genel barındırılan CDN sürümünü kullanmanız `<head>` öğesi:

    ```html
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=2" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=2"></script>
    ```

    b. Alternatif olarak, Azure haritalar Web SDK kaynak kodu kullanarak yerel olarak yükleme [azure haritalar denetimi](https://www.npmjs.com/package/azure-maps-control) NPM paketini ve uygulamanızı barındırın. Bu paket ayrıca TypeScript tanımları içerir.

    > npm yükleme azure haritalar-denetim

    Azure haritalar stil sayfası ve komut dosyası kaynak başvuruları başvuruları eklersiniz `<head>` öğesi:

    ```html
    <link rel="stylesheet" href="node_modules/azure-maps-control/dist/css/atlas.min.css" type="text/css" />
    <script src="node_modules/azure-maps-control/dist/js/atlas.min.js"></script>
    ```

3. Sayfanın tam gövdesi doldurması harita işlemek için aşağıdakileri ekleyin `<style>` öğesine `<head>` öğesi.

    ```html
    <style>
        html, body {
            margin: 0;
        }
    
        #myMap {
            height: 100vh;
            width: 100vw;
        }
    </style>
    ```

4. Sayfanın gövdesi ekleme bir `<div>` öğesi ve ona bir `id` , **myMap**. 

    ```html
    <body>
        <div id="myMap"></div>
    </body>
    ```

5. Harita denetimi başlatmak için html gövdesinde yeni bir bölüm tanımlayın ve bir betik oluşturabilir. Eşlemesini kullanarak kimlik doğrulaması yapmak için kendi Azure haritalar hesabı anahtarı veya Azure Active Directory (AAD) kimlik bilgilerini kullanan [kimlik doğrulama seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.authenticationoptions). Bir hesap oluşturun veya, anahtar, bkz: bulmak gerekiyorsa [Azure haritalar hesabı ve anahtarları yönetme](how-to-manage-account-keys.md). **Dil** seçeneği harita etiketlerini ve denetimler için kullanılacak dili belirtir. Desteklenen diller hakkında daha fazla bilgi için bkz. [desteklenen diller](supported-languages.md). Bir abonelik anahtarı kimlik doğrulaması için kullanılıyorsa.

    ```html
    <script type='text/javascript'>
        var map = new atlas.Map('myMap', {
            center: [-122.33, 47.6],
            zoom: 12,
            language: 'en-US',
            authOptions: {
                authType: 'subscriptionKey',
                subscriptionKey: '<Your Azure Maps Key>'
            }
        });
    </script>
    ```

    Azure Active Directory (AAD) kimlik doğrulaması için kullanıyorsanız:

    ```html
    <script type='text/javascript'>
        var map = new atlas.Map('myMap', {
            center: [-122.33, 47.6],
            zoom: 12,
            language: 'en-US',
            authOptions: {
                authType: 'aad',
                clientId: '<Your AAD Client Id>',
                aadAppId: '<Your AAD App Id>',
                aadTenant: 'msft.ccsctp.net'
            }
        });
    </script>
    ```

    Bkz: [Azure Haritalar ile kimlik doğrulaması](azure-maps-authentication.md) daha fazla ayrıntı için.
6. İsteğe bağlı olarak, şu meta etiketini öğeleri yararlı sayfanıza gidin ekleme bulabilirsiniz:

    ```html
    <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    
    <!-- Ensures the web page looks good on all screen sizes. -->
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />
    ```

7. Hepsini HTML dosyanız aşağıdaki gibi görünmelidir:

    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <title></title>
    
        <meta charset="utf-8" />
        
        <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
        <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    
        <!-- Ensures the web page looks good on all screen sizes. -->
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />
    
        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=2" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=2"></script>
    
        <style>
            html, body {
                margin: 0;
            }
        
            #myMap {
                height: 100vh;
                width: 100vw;
            }
        </style>
    </head>
    <body>
        <div id="myMap"></div>
        
        <script type='text/javascript'>
            //Create an instance of the map control and set some options.
            var map = new atlas.Map('myMap', {
                center: [-122.33, 47.6],
                zoom: 12,
                language: 'en-US',
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });
        </script>
    </body>
    </html>
    ```

8. Web tarayıcınızda dosyasını açın ve işlenmiş harita görüntüleyin. Aşağıdaki gibi görünmelidir:

    <iframe height="700" style="width: 100%;" scrolling="no" title="Harita Denetimi'ni kullanma" src="//codepen.io/azuremaps/embed/yZpEYL/?height=557&theme-id=0&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Kalem bkz <a href='https://codepen.io/azuremaps/pen/yZpEYL/'>harita denetimini kullanma</a> Azure haritalar tarafından (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) üzerinde <a href='https://codepen.io'>CodePen</a>.
    </iframe>

## <a name="next-steps"></a>Sonraki adımlar

Oluşturma ve bir haritayla etkileşim kurabilir öğrenin:

> [!div class="nextstepaction"]
> [Bir eşleme oluşturma](map-create.md)

Bir eşlem stili hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [Harita stil seçin](choose-map-style.md)
