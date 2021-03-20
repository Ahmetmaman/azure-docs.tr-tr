---
title: Görüntüleri sınıflandırıcı ile programlama yoluyla test etmek için tahmin uç noktası kullanın Özel Görüntü İşleme
titleSuffix: Azure Cognitive Services
description: Özel Görüntü İşleme sınıflandırıcınızla programlama yoluyla görüntüleri test etmek için API’nin nasıl kullanılacağını öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 04/02/2019
ms.author: pafarley
ms.custom: devx-track-csharp
ms.openlocfilehash: 7f1939536e033d2cf964dd2f4ee562e4ee20061b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "88934761"
---
# <a name="use-your-model-with-the-prediction-api"></a>Tahmin API 'SI ile modelinizi kullanma

Modelinize eğtikten sonra yansımaları, tahmin API uç noktasına göndererek programlama yoluyla test edebilirsiniz.

> [!NOTE]
> Bu belgede, Tahmin API’sine görüntü göndermek için C# kullanımı gösterilmektedir. Daha fazla bilgi ve örnek için bkz. [tahmin API 'si başvurusu](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c15).

## <a name="publish-your-trained-iteration"></a>Eğitilen yinelelerinizi yayımlayın

[Özel Görüntü İşleme web sayfasından](https://customvision.ai) projenizi ve __Performans__ sekmesini seçin.

Tahmin API 'sine görüntü göndermek için öncelikle, __Yayımla__ ' yı seçerek ve yayımlanan yineleme için bir ad belirterek, yinelemeyi tahmin etmek üzere yayımlamanız gerekir. Bu, modelinizi Özel Görüntü İşleme Azure kaynağınızın tahmin API 'SI için erişilebilir hale getirir.

![Performans sekmesi, Yayınla düğmesini çevreleyen kırmızı bir dikdörtgenle gösterilir.](./media/use-prediction-api/unpublished-iteration.png)

Modelinize başarıyla yayımlandıktan sonra, sol taraftaki kenar çubuğundan yineleinizin yanında "yayımlandı" etiketini görürsünüz ve bu adın adı yinelemenin açıklamasında görüntülenir.

![Performans sekmesi, yayımlanan etiketi çevreleyen kırmızı bir dikdörtgen ve yayımlanan yinelemenin adı ile gösterilir.](./media/use-prediction-api/published-iteration.png)

## <a name="get-the-url-and-prediction-key"></a>URL ve tahmin anahtarını alma

Modeliniz yayımlandıktan sonra, __tahmin URL 'sini__ seçerek gerekli bilgileri alabilirsiniz. Bu, tahmin __URL 'si__ ve __tahmin anahtarı__ da dahil olmak üzere, tahmin API 'si kullanımıyla ilgili bilgileri içeren bir iletişim kutusu açar.

![Performans sekmesi, tahmin URL 'SI düğmesini çevreleyen kırmızı bir dikdörtgenle gösterilir.](./media/use-prediction-api/published-iteration-prediction-url.png)

![Performans sekmesi, bir görüntü dosyası ve Prediction-Key değeri kullanmak için tahmin URL 'SI değerini çevreleyen kırmızı bir dikdörtgenle gösterilir.](./media/use-prediction-api/prediction-api-info.png)


Bu kılavuzda, bir yerel görüntü kullanacaksınız, bu nedenle geçici bir konuma **bir görüntü dosyanız** varsa, URL 'yi kopyalayın. İlgili __tahmin anahtarı__ değerini de kopyalayın.

## <a name="create-the-application"></a>Uygulama oluşturma

1. Visual Studio 'da yeni bir C# konsol uygulaması oluşturun.

1. __Program.cs__ dosyasının gövdesi olarak aşağıdaki kodu kullanın.

    ```csharp
    using System;
    using System.IO;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Threading.Tasks;

    namespace CVSPredictionSample
    {
        public static class Program
        {
            public static void Main()
            {
                Console.Write("Enter image file path: ");
                string imageFilePath = Console.ReadLine();

                MakePredictionRequest(imageFilePath).Wait();

                Console.WriteLine("\n\nHit ENTER to exit...");
                Console.ReadLine();
            }

            public static async Task MakePredictionRequest(string imageFilePath)
            {
                var client = new HttpClient();

                // Request headers - replace this example key with your valid Prediction-Key.
                client.DefaultRequestHeaders.Add("Prediction-Key", "<Your prediction key>");

                // Prediction URL - replace this example URL with your valid Prediction URL.
                string url = "<Your prediction URL>";

                HttpResponseMessage response;

                // Request body. Try this sample with a locally stored image.
                byte[] byteData = GetImageAsByteArray(imageFilePath);

                using (var content = new ByteArrayContent(byteData))
                {
                    content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
                    response = await client.PostAsync(url, content);
                    Console.WriteLine(await response.Content.ReadAsStringAsync());
                }
            }

            private static byte[] GetImageAsByteArray(string imageFilePath)
            {
                FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
                BinaryReader binaryReader = new BinaryReader(fileStream);
                return binaryReader.ReadBytes((int)fileStream.Length);
            }
        }
    }
    ```

1. Aşağıdaki bilgileri değiştirin:
   * `namespace`Alanı projenizin adına ayarlayın.
   * Yer tutucusunu, `<Your prediction key>` daha önce aldığınız anahtar değeriyle değiştirin.
   * Yer tutucusunu, `<Your prediction URL>` daha önce ALDıĞıNıZ URL ile değiştirin.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırdığınızda, konsolunda bir görüntü dosyasının yolunu girmeniz istenir. Görüntü daha sonra tahmin API 'sine gönderilir ve tahmin sonuçları JSON biçimli bir dize olarak döndürülür. Aşağıda örnek bir yanıt verilmiştir.

```json
{
    "id":"7796df8e-acbc-45fc-90b4-1b0c81b73639",
    "project":"8622c779-471c-4b6e-842c-67a11deffd7b",
    "iteration":"59ec199d-f3fb-443a-b708-4bca79e1b7f7",
    "created":"2019-03-20T16:47:31.322Z",
    "predictions":[
        {"tagId":"d9cb3fa5-1ff3-4e98-8d47-2ef42d7fb373","tagName":"cat", "probability":1.0},
        {"tagId":"9a8d63fb-b6ed-4462-bcff-77ff72084d99","tagName":"dog", "probability":0.1087869}
    ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, görüntüleri özel görüntü sınıflandırıcıya/algılayıcısının nasıl göndereceğini ve C# SDK ile programlı bir şekilde yanıt almanızı öğrendiniz. Daha sonra, C# ile uçtan uca senaryoları tamamlamayı veya farklı bir dil SDK 'sını kullanmaya başlamanızı öğrenin.

* [Hızlı başlangıç: Özel Görüntü İşleme SDK](quickstarts/image-classification.md)
