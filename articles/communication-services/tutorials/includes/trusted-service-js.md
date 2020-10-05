---
title: Güvenilen hizmet JavaScript 'ı
description: Bu, Iletişim Hizmetleri için güvenilen bir hizmet oluşturmanın JavaScript sürümüdür.
author: dademath
manager: nimag
services: azure-communication-services
ms.author: dademath
ms.date: 07/28/2020
ms.topic: include
ms.service: azure-communication-services
ms.openlocfilehash: d4ef8baa123f805d380b14fa24abff65903cb41d
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "90947895"
---
## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. Ayrıntılar için bkz. [ücretsiz hesap oluşturma](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Desteklenen platformlardan](https://code.visualstudio.com/docs/supporting/requirements#_platforms)birine [Visual Studio Code](https://code.visualstudio.com/) .
- [Node.js](https://nodejs.org/), ETKIN LTS ve bakım LTS sürümleri (10.14.1 önerilir). `node --version`Sürümünüzü denetlemek için komutunu kullanın. 
- Visual Studio Code için [Azure işlevleri uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) . 
- Etkin bir Iletişim Hizmetleri kaynağı ve bağlantı dizesi. [Iletişim Hizmetleri kaynağı oluşturun](../../quickstarts/create-communication-resource.md).

## <a name="overview"></a>Genel Bakış

:::image type="content" source="../media/trusted-service-architecture.png" alt-text="Güvenilen hizmet mimarisi diyagramı":::

Bu öğreticide, güvenilen belirteç sağlama hizmeti olarak kullanılacak bir Azure Işlevi oluşturacağız. Bu öğreticiyi kullanarak kendi belirteç sağlama hizmetinizi önyükleyebilirsiniz.

Bu hizmet, kullanıcıların Azure Iletişim hizmetlerine yönelik kimlik doğrulamasından sorumludur. Iletişim Hizmetleri uygulamalarınızın kullanıcıları, `Access Token` sohbet iş parçacıklarına ve VoIP çağrılarına katılabilmek için bir gerektirir. Azure Işlevi, Kullanıcı ve Iletişim Hizmetleri arasında güvenilir bir Middleman olarak çalışacaktır. Bu, kaynak Bağlantı dizenizi kullanıcılarınıza göstermeden erişim belirteçleri sağlamanıza olanak tanır.

Daha fazla bilgi için bkz. [istemci-sunucu mimarisi](../../concepts/client-and-server-architecture.md) ve [kimlik doğrulaması ve yetkilendirme](../../concepts/authentication.md) kavramsal belgeleri.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="azure-functions-set-up"></a>Azure Işlevleri ayarlandı

İlk olarak Azure işlevimiz için temel yapıyı ayarlayalım. Kurulum üzerinde adım adım yönergeler şurada bulunabilir: [Visual Studio Code kullanarak Işlev oluşturma](https://docs.microsoft.com/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-javascript)

Azure Işlevimiz aşağıdaki yapılandırmayı gerektirir:

- Dil: JavaScript
- Şablon: HTTP tetikleyicisi
- Yetkilendirme düzeyi: anonim (farklı bir yetkilendirme modelini tercih ediyorsanız bu daha sonra değiştirilebilir)
- İşlev adı: Kullanıcı tanımlı

Yukarıdaki yapılandırmayla [Azure işlevleri yönergelerinden](https://docs.microsoft.com/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-javascript) sonra, işlevin kendisini içeren bir dosya Ile Azure işlevi için Visual Studio Code bir projeniz olmalıdır `index.js` . Bu dosyanın içindeki kod şu şekilde olmalıdır:

```javascript

module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    const name = (req.query.name || (req.body && req.body.name));
    const responseMessage = name
        ? "Hello, " + name + ". This HTTP triggered function executed successfully."
        : "This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response.";

    context.res = {
        // status: 200, /* Defaults to 200 */
        body: responseMessage
    };
}

```

Şimdi Azure Communication Services kitaplıklarını yüklemeye devam edeceğiz.

### <a name="install-communication-services-libraries"></a>İletişim Hizmetleri kitaplıklarını yükler

`Administration`Oluşturmak için kitaplığı kullanacağız `User Access Tokens` .

`npm install`JavaScript Için Azure Communication Services yönetim istemcisi kitaplığını yüklemek için komutunu kullanın.

```console

npm install @azure/communication-administration --save

```

`--save`Seçeneği, kitaplığı dosyadaki **package.js** bir bağımlılık olarak listeler.

Dosyasının en üstünde, `index.js``CommunicationIdentityClient`

```javascript
const { CommunicationIdentityClient } = require('@azure/communication-administration');
```

## <a name="access-token-generation"></a>Erişim belirteci oluşturma

Azure Işlevimizin oluşturmasına izin vermek için `User Access Tokens` ilk olarak Iletişim Hizmetleri kaynağınızın bağlantı dizesini kullanmanız gerekir.

Bağlantı dizenizi alma hakkında daha fazla bilgi için [kaynak sağlama hızlı](../../quickstarts/create-communication-resource.md) başlangıcı ' nı ziyaret edin.

``` javascript
const connectionString = 'INSERT YOUR RESOURCE CONNECTION STRING'
```

Sonra, oluşturmak için özgün işlevimizi değiştireceksiniz `User Access Tokens` . 

`User Access Tokens` , yönteminden bir Kullanıcı oluşturularak oluşturulur `createUser` . Kullanıcı oluşturulduktan sonra, `issueToken` Azure işlevinin döndürdüğü bu kullanıcı için bir belirteç oluşturmak üzere yöntemini kullanabiliriz.

Bu örnekte, belirteç kapsamını olarak yapılandıracağız `voip` . Uygulamanız için diğer kapsamlar gerekli olabilir. [Kapsamlar](../../quickstarts/access-tokens.md) hakkında daha fazla bilgi edinin

```javascript
module.exports = async function (context, req) {
    let tokenClient = new CommunicationIdentityClient(connectionString);

    const user = await tokenClient.createUser();

    const userToken = await tokenClient.issueToken(user, ["voip"]);

    const response = {
        "User" : userToken.user,
        "Token": userToken.token,
        "ExpiresOn": userToken.expiresOn
    }

    context.res = {
        body: response
    };
}
```

Mevcut Iletişim hizmetlerinde `CommunicationUser` , oluşturma adımını atlayabilir ve yalnızca bir erişim belirteci oluşturabilirsiniz. [Kullanıcı erişim belirteçleri oluşturma hızlı başlangıç](../../quickstarts/access-tokens.md)bölümünde daha fazla ayrıntı bulundu.

## <a name="test-the-azure-function"></a>Azure Işlevini test etme

Azure Işlevini kullanarak yerel olarak çalıştırın `F5` . Bu işlem, Azure Işlevini yerel olarak başlatır ve aracılığıyla erişilebilir hale getirir: `http://localhost:7071/api/FUNCTION_NAME` . [Yerel olarak çalışmaya](https://docs.microsoft.com/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-javascript#run-the-function-locally) yönelik ek belgelere göz atın

Tarayıcınızda URL 'YI açın ve belirteç için Iletişim Kullanıcı kimliği, belirteç ve süre sonu ile bir yanıt gövdesi görmeniz gerekir.

:::image type="content" source="../media/trusted-service-sample-response.png" alt-text="Güvenilen hizmet mimarisi diyagramı":::

## <a name="deploy-the-function-to-azure"></a>Işlevi Azure 'a dağıtın

Azure Işlevinizi dağıtmak için [adım adım yönergeleri](https://docs.microsoft.com/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-javascript#sign-in-to-azure) izleyebilirsiniz.

Genellikle şunları yapmanız gerekir:
1. Visual Studio 'dan Azure 'da oturum açın
2. Projenizi Azure hesabınızda yayımlayın. Burada, mevcut bir aboneliği seçmeniz gerekir.
3. Visual Studio Sihirbazı 'nı kullanarak yeni bir Azure Işlevi kaynağı oluşturun veya var olan bir kaynağı kullanın. Yeni bir kaynak için, bunu istediğiniz bölge, çalışma zamanı ve benzersiz tanımlayıcı olarak yapılandırmanız gerekecektir.
4. Dağıtımın sonlanmasını bekle
5. İşlevi çalıştırın 🎉

## <a name="run-azure-function"></a>Azure Işlevini Çalıştır

URL 'yi kullanarak Azure işlevini çalıştırma `http://<function-appn-ame>.azurewebsites.net/api/<function-name>`

Visual Studio Code üzerinde işleve sağ tıklayıp Işlev URL 'sini kopyalayarak URL 'YI bulabilirsiniz.

[Azure işlevinizi çalıştırma](https://docs.microsoft.com/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-javascript#run-the-function-in-azure) hakkında daha fazla bilgi için
