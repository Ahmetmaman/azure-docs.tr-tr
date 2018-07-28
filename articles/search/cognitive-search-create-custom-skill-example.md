---
title: 'Örnek: bilişsel arama (Azure Search) işlem hattı, özel bir yetenek oluştur | Microsoft Docs'
description: Bilişsel arama dizinleme işlem hattı, Azure Search eşlenen özel nitelik içinde metin çevirme API'sini kullanmayı gösterir.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 06/29/2018
ms.author: luisca
ms.openlocfilehash: b428e6e7738c8a9052c3fcfe2ad5284bfd5293d6
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39308002"
---
# <a name="example-create-a-custom-skill-using-the-text-translate-api"></a>Örnek: metin çevirme API'sini kullanarak özel bir yetenek oluşturma

Bu örnekte, web, herhangi bir dilde kabul eden ve İngilizceye çevirir API özel beceri oluşturma konusunda bilgi edinin. Örnekte bir [Azure işlevi](https://azure.microsoft.com/services/functions/) sarmalamak için [Çevir metin API'si](https://azure.microsoft.com/services/cognitive-services/translator-text-api/) böylece özel bir yetenek arabirimini uygular.

## <a name="prerequisites"></a>Önkoşullar

+ Hakkında bilgi edinin [özel bir yetenek arabirimi](cognitive-search-custom-skill-interface.md) özel bir yetenek uygulamalıdır giriş/çıkış arabirimi ile aşina değilseniz makalesi.

+ [Translator metin çevirisi API'si için kaydolun](../cognitive-services/translator/translator-text-how-to-signup.md)ve onu kullanmak üzere bir API anahtarınızı alın.

+ Yükleme [Visual Studio 2017 sürüm 15.5](https://www.visualstudio.com/vs/) veya daha sonra Azure geliştirme iş yükü dahil.

## <a name="create-an-azure-function"></a>Azure İşlevi oluşturma

Bu örnek bir web API barındırmak için bir Azure işlevi kullansa da, gerekli değildir.  Karşıladıkları sürece [arabirim bilişsel bir beceri gereksinimleri](cognitive-search-custom-skill-interface.md), hangi yaklaşımın kurucularýn. Azure işlevleri, ancak özel bir yetenek oluşturmak kolaylaştırır.

### <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

1. Visual Studio'da **yeni** > **proje** Dosya menüsünden.

1. Yeni Proje iletişim kutusunda, seçmek **yüklü**, genişletme **Visual C#** > **bulut**seçin **Azure işlevleri**, yazın Projeniz için ad ve seçin **Tamam**. İşlev uygulamasının adı, bir C# ad alanı olarak geçerli olmalıdır; bu nedenle alt çizgi, kısa çizgi veya alfasayısal olmayan herhangi bir karakter kullanmayın.

1. Olmasını seçin **HTTP tetikleyicisi**

1. Depolama hesabı için seçtiğiniz **hiçbiri**gibi bu işlev için herhangi bir depolama alanı gerekmez.

1. Seçin **Tamam** işlevi oluşturmak için proje ve HTTP ile tetiklenen işlev.

### <a name="modify-the-code-to-call-the-translate-cognitive-service"></a>Çevirme Bilişsel hizmet çağırmak için kodu değiştirin

Visual Studio bir proje oluşturur ve bu projenin içinde seçili işlev türü için ortak kod içeren bir sınıf bulunur. Metottaki *FunctionName* özniteliği işlevin adını ayarlar. *HttpTrigger* özniteliği, işlevin bir HTTP isteği tarafından tetiklenip tetiklenmediğini belirtir.

Şimdi, tüm dosya içeriğini değiştirmek *Function1.cs* aşağıdaki kod ile:

```csharp
using System;
using System.Net.Http;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.IO;
using System.Text;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Azure.WebJobs.Host;
using Newtonsoft.Json;

namespace TranslateFunction
{
    // This function will simply translate messages sent to it.
    public static class Function1
    {
        static string path = "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0";

        // NOTE: Replace this example key with a valid subscription key.
        static string key = "<enter your api key here>";

        #region classes used to serialize the response
        private class WebApiResponseError
        {
            public string message { get; set; }
        }

        private class WebApiResponseWarning
        {
            public string message { get; set; }
        }

        private class WebApiResponseRecord
        {
            public string recordId { get; set; }
            public Dictionary<string, object> data { get; set; }
            public List<WebApiResponseError> errors { get; set; }
            public List<WebApiResponseWarning> warnings { get; set; }
        }

        private class WebApiEnricherResponse
        {
            public List<WebApiResponseRecord> values { get; set; }
        }
        #endregion

        [FunctionName("Translate")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Function, "post", Route = null)]HttpRequest req,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            string recordId = null;
            string originalText = null;
            string toLanguage = null;
            string translatedText = null;

            string requestBody = new StreamReader(req.Body).ReadToEnd();
            dynamic data = JsonConvert.DeserializeObject(requestBody);

            // Validation
            if (data?.values == null)
            {
                return new BadRequestObjectResult(" Could not find values array");
            }
            if (data?.values.HasValues == false || data?.values.First.HasValues == false)
            {
                // It could not find a record, then return empty values array.
                return new BadRequestObjectResult(" Could not find valid records in values array");
            }

            recordId = data?.values?.First?.recordId?.Value as string;
            originalText = data?.values?.First?.data?.text?.Value as string;
            toLanguage = data?.values?.First?.data?.language?.Value as string;

            if (recordId == null)
            {
                return new BadRequestObjectResult("recordId cannot be null");
            }

            translatedText = TranslateText(originalText, toLanguage).Result;
        
            // Put together response.
            WebApiResponseRecord responseRecord = new WebApiResponseRecord();
            responseRecord.data = new Dictionary<string, object>();
            responseRecord.recordId = recordId;
            responseRecord.data.Add("text", translatedText);

            WebApiEnricherResponse response = new WebApiEnricherResponse();
            response.values = new List<WebApiResponseRecord>();
            response.values.Add(responseRecord);

            return (ActionResult)new OkObjectResult(response);
        }


        /// <summary>
        /// Use Cognitive Service to translate text from one language to antoher.
        /// </summary>
        /// <param name="originalText">The text to translate.</param>
        /// <param name="toLanguage">The language you want to translate to.</param>
        /// <returns>Asynchronous task that returns the translated text. </returns>
        async static Task<string> TranslateText(string originalText, string toLanguage)
        {
            System.Object[] body = new System.Object[] { new { Text = originalText } };
            var requestBody = JsonConvert.SerializeObject(body);

            var uri = $"{path}&to={toLanguage}";

            string result = "";

            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Post;
                request.RequestUri = new Uri(uri);
                request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                var response = await client.SendAsync(request);
                var responseBody = await response.Content.ReadAsStringAsync();

                dynamic data = JsonConvert.DeserializeObject(responseBody);
                result = data?.First?.translations?.First?.text?.Value as string;

            }
            return result;
        }
    }
}
```

Girmek kendi emin *anahtarı* değerini *TranslateText* Çevir metin API'si için kaydolurken aldığınız anahtarı temelinde yöntemi.

Bu, yalnızca tek bir kayıt üzerinde aynı anda çalışan basit bir enricher örneğidir. Bunu daha sonra beceri kümesi için toplu iş boyutu ayarlarken önemli hale gelir.

## <a name="test-the-function-from-visual-studio"></a>Visual Studio'dan işlevi test etme

Tuşuna **F5** program ve test işlevi davranışları çalıştırılacak. Bu durumda İngilizce, İspanyolca bir metni çevirmek için aşağıdaki işlevi kullanacağız. Postman veya fiddler'ı aşağıda gösterilene benzer bir çağrı vermek için kullanın:

```http
POST https://localhost:7071/api/Translate
```
### <a name="request-body"></a>İstek gövdesi
```json
{
   "values": [
        {
            "recordId": "a1",
            "data":
            {
               "text":  "Este es un contrato en Inglés",
               "language": "en"
            }
        }
   ]
}
```
### <a name="response"></a>Yanıt
Aşağıdaki örneğe benzer bir yanıt görmeniz gerekir:

```json
{
    "values": [
        {
            "recordId": "a1",
            "data": {
                "text": "This is a contract in English"
            },
            "errors": null,
            "warnings": null
        }
    ]
}
```

## <a name="publish-the-function-to-azure"></a>İşlevi Azure'a yayımlama

İşlev davranışını ile memnun kaldığınızda, bunu yayımlayabilirsiniz.

1. **Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin. Seçin **Yeni Oluştur** > **yayımlama**.

1. Visual Studio henüz Azure hesabınıza bağlamadıysanız, seçin **Hesap Ekle...**

1. İzleyin ekrandaki ister. Azure hesabı, kaynak grubunu, barındırma planı ve kullanmak istediğiniz depolama hesabı belirtmeniz istenir. Bunlar zaten yoksa, yeni bir kaynak grubu, yeni bir barındırma planı ve bir depolama hesabı oluşturabilirsiniz. İşiniz bittiğinde seçin **oluştur**

1. Dağıtım tamamlandıktan sonra Site URL'sini not alın. İşlev uygulamanızda Azure'nın adresidir. 

1. İçinde [Azure portalında](https://portal.azure.com), kaynak grubuna gidin ve çevirme yayımladığınız işlevi bakın. Altında **Yönet** bölümünde, ana bilgisayar anahtarları görmeniz gerekir. Seçin **kopyalama** simgesi *varsayılan* ana bilgisayar anahtarı.  

## <a name="update-ssl-settings"></a>SSL ayarlarını güncelleştirme

Azure işlevleri ile 30 Haziran 2018'den sonra oluşturulan tüm özel becerilere sahip şu anda uyumlu olmayan TLS 1.0 devre dışı bırakıldı.

1. İçinde [Azure portalında](https://portal.azure.com), kaynak grubuna gidin ve çevirme yayımladığınız işlevi bakın. Altında **Platform özellikleri** bölümünde, SSL görmeniz gerekir.

1. SSL seçtikten sonra değiştirmelisiniz **en düşük TLS sürümünü** 1.0 için. TLS 1.2 işlevleri özel becerileri henüz desteklenmemektedir.

## <a name="test-the-function-in-azure"></a>Azure'da işlevi test etme

Varsayılan ana bilgisayar anahtarı olduğuna göre işlevinizi şu şekilde test edin:

```http
POST https://translatecogsrch.azurewebsites.net/api/Translate?code=[enter default host key here]
```
### <a name="request-body"></a>İstek Gövdesi
```json
{
   "values": [
        {
            "recordId": "a1",
            "data":
            {
               "text":  "Este es un contrato en Inglés",
               "language": "en"
            }
        }
   ]
}
```

Bu işlev yerel ortamda çalıştırırken daha önce gördüğünüz bir benzer bir sonuç üretir.

## <a name="connect-to-your-pipeline"></a>Ardışık düzeninize bağlanma
Yeni özel bir yetenek olduğuna göre bunu standartlarındaki şu ekleyebilirsiniz. Aşağıdaki örnekte, yetenek çağırmak nasıl gösterir. Yetenek toplu işlemiyor olduğundan, en yüksek toplu iş boyutu yalnızca olması için bir yönerge ekleyin ```1``` göndermek için tek tek belgeler.

```json
{
    "skills": [
      ...,  
      {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "Our new translator custom skill",
        "uri": "http://translatecogsrch.azurewebsites.net/api/Translate?code=[enter default host key here]",
        "batchSize":1,
        "context": "/document",
        "inputs": [
          {
            "name": "text",
            "source": "/document/content"
          },
          {
            "name": "language",
            "source": "/document/destinationLanguage"
          }
        ],
        "outputs": [
          {
            "name": "text",
            "targetName": "translatedText"
          }
        ]
      }
  ]
}
```

## <a name="next-steps"></a>Sonraki Adımlar
Tebrikler! İlk, özel enricher oluşturdunuz. Şimdi, kendi özel işlevsellik eklemek için aynı deseni takip edebilirsiniz. 

+ [Bilişsel arama işlem hattı için özel bir yetenek Ekle](cognitive-search-custom-skill-interface.md)
+ [Bir beceri kümesi tanımlama](cognitive-search-defining-skillset.md)
+ [Beceri kümesi (REST) oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Zenginleştirilmiş alanlarını eşleme](cognitive-search-output-field-mapping.md)
