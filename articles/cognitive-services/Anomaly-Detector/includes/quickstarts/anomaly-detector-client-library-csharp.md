---
title: Anomali algılayıcı .NET istemci kitaplığı hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/22/2020
ms.author: mbullwin
ms.openlocfilehash: ee4dc926269d3ac66243a3953d7e41eb3a30198c
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98948429"
---
.NET için anomali algılayıcı istemci kitaplığını kullanmaya başlayın. Hizmet tarafından sunulan algoritmaları kullanarak paketi başlatmak için bu adımları izleyin. Anomali algılayıcı hizmeti, sektör, senaryo veya veri hacminin ne olursa olsun, üzerinde en iyi şekilde sığdırma modellerini kullanarak zaman serisi verilerinizde yer alan anormallikleri bulmanıza olanak sağlar.

.NET için anomali algılayıcı istemci kitaplığını kullanarak şunları yapın:

* Bir toplu istek olarak zaman serisi veri kümesi genelinde anomali algılama
* Zaman serinizdeki en son veri noktasının anomali durumunu Algıla
* Veri kümesindeki eğilim değişiklik noktalarını tespit edin.

[Kitaplık başvuru belgeleri](https://aka.ms/anomaly-detector-dotnet-ref)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/AnomalyDetector)  |  [Paket (NuGet)](https://www.nuget.org/packages/Azure.AI.AnomalyDetector/3.0.0-preview.2)  |  [GitHub 'da kodu bulma](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/sdk/csharp-sdk-sample.cs)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* Geçerli [.NET Core](https://dotnet.microsoft.com/download/dotnet-core) sürümü
* Azure aboneliğiniz olduktan sonra <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> anahtar ve uç noktanıza ulaşmak için Azure Portal bir anomali algılayıcı kaynağı oluşturun. Dağıtım için bekleyin ve **Kaynağa Git** düğmesine tıklayın.
    * Uygulamanızı anomali algılayıcı API 'sine bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.

## <a name="setting-up"></a>Ayarlanıyor

[!INCLUDE [anomaly-detector-environment-variables](../environment-variables.md)]

### <a name="create-a-new-net-core-application"></a>Yeni bir .NET Core uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` adıyla yeni bir konsol uygulaması oluşturmak için komutunu kullanın `anomaly-detector-quickstart` . Bu komut, tek bir C# kaynak dosyası olan basit bir "Merhaba Dünya" projesi oluşturur: *program.cs*.

```dotnetcli
dotnet new console -n anomaly-detector-quickstart
```

Dizininizi yeni oluşturulan uygulama klasörüyle değiştirin. Uygulamayı ile oluşturabilirsiniz:

```dotnetcli
dotnet build
```

Derleme çıktısı hiçbir uyarı veya hata içermemelidir.

```output
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

Uygulama dizini içinde, aşağıdaki komutla .NET için anomali algılayıcı istemci Kitaplığı ' nı bir daha yükleyeceksiniz:

```dotnetcli
dotnet add package Microsoft.Azure.CognitiveServices.AnomalyDetector
```

Proje dizininden *program.cs* dosyasını açın ve aşağıdakileri kullanarak aşağıdakileri ekleyin `directives` :

[!code-csharp[using statements](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=usingStatements)]

Uygulamanın `main()` yönteminde, kaynağınızın Azure konumu ve anahtarınızın bir ortam değişkeni olarak değişkenlerini oluşturun. Uygulama başlatıldıktan sonra ortam değişkenini oluşturduysanız, bu dosyayı çalıştıran düzenleyicinin, IDE 'nin veya kabuğun kapatılıp yeniden yüklenmesi gerekir.

[!code-csharp[Main method](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=mainMethod)]

## <a name="object-model"></a>Nesne modeli

Anomali algılayıcı istemcisi, anahtarınızı içeren [ApiKeyServiceClientCredentials](/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.apikeyserviceclientcredentials)kullanarak Azure 'da kimlik doğrulaması yapan bir [anorivtorclient](/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclient) nesnesidir. İstemci, [Entiredetectasync ()](/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.entiredetectasync)kullanarak bir veri kümesinin tamamında veya [lastdetectasync ()](/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.lastdetectasync)kullanan en son veri noktasında anomali algılama yapabilir. [Changepointdetectasync](https://aka.ms/anomaly-detector-dotnet-ref) yöntemi, bir eğilim içindeki değişiklikleri işaretleyen noktaları algılar.

Zaman serisi verileri, [istek](/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request) nesnesinde bir dizi [işaret](/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request.series#Microsoft_Azure_CognitiveServices_AnomalyDetector_Models_Request_Series) olarak gönderilir. `Request`Nesnesi, verileri (örneğin,[ayrıntı düzeyi](/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request.granularity) ) ve anomali algılama parametrelerini tanımlayacak özellikler içerir.

Anomali algılayıcısı yanıtı, kullanılan yönteme bağlı olarak bir [Entiredetectresponse](/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.entiredetectresponse), [lastdetectresponse](/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.lastdetectresponse)veya [changepointdetectresponse](https://aka.ms/anomaly-detector-dotnet-ref) nesnesidir.

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, .NET için anomali algılayıcı istemci kitaplığı ile aşağıdakilerin nasıl yapılacağını göstermektedir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Bir dosyadan zaman serisi veri kümesi yükleme](#load-time-series-data-from-a-file)
* [Tüm veri kümesindeki anormallikleri Algıla](#detect-anomalies-in-the-entire-data-set)
* [En son veri noktasının anomali durumunu Algıla](#detect-the-anomaly-status-of-the-latest-data-point)
* [Veri kümesindeki değişiklik noktalarını Algıla](#detect-change-points-in-the-data-set)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Yeni bir yöntemde, uç nokta ve anahtarınızla bir istemci örneği oluşturun. Anahtarınızla bir [ApiKeyServiceClientCredentials](/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.apikeyserviceclientcredentials) nesnesi oluşturun ve bir [Anorivtorclient](/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclient) nesnesi oluşturmak için bunu uç noktanızla birlikte kullanın.

[!code-csharp[Client authentication function](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=createClient)]

## <a name="load-time-series-data-from-a-file"></a>Bir dosyadan zaman serisi verilerini yükle

Bu hızlı başlangıçta [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/example-data/request-data.csv)'dan örnek verileri indirin:
1. Tarayıcınızda, **RAW**' a sağ tıklayın.
2. **Bağlantıyı farklı kaydet**' e tıklayın.
3. Dosyayı uygulama dizininize bir. csv dosyası olarak kaydedin.

Bu zaman serisi verileri. csv dosyası olarak biçimlendirilir ve anomali algılayıcısı API 'sine gönderilir.

Zaman serisi verilerinde okumak için yeni bir yöntem oluşturun ve bunu bir [istek](/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request) nesnesine ekleyin. `File.ReadAllLines()`Dosya yolu ile çağrı yapın ve [nokta](/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.point) nesnelerinin bir listesini oluşturun ve yeni satır karakterlerini kaldırın. Değerleri ayıklayın ve dateStamp değerini sayısal değerinden ayırın ve bunları yeni bir `Point` nesneye ekleyin.

Bir `Request` nesneyi işaret dizisine ve `Granularity.Daily` veri noktalarının [ayrıntı düzeyi](/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.granularity) (veya dönemliği) için yapın.

[!code-csharp[load the time series data file](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=GetSeriesFromFile)]

## <a name="detect-anomalies-in-the-entire-data-set"></a>Tüm veri kümesindeki anormallikleri Algıla

İstemcinin [Entiredetectasync ()](/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.entiredetectasync#Microsoft_Azure_CognitiveServices_AnomalyDetector_AnomalyDetectorClientExtensions_EntireDetectAsync_Microsoft_Azure_CognitiveServices_AnomalyDetector_IAnomalyDetectorClient_Microsoft_Azure_CognitiveServices_AnomalyDetector_Models_Request_System_Threading_CancellationToken_) yöntemini `Request` nesnesiyle çağırmak ve yanıtı bir [entiredetectresponse](/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.entiredetectresponse) nesnesi olarak beklemek için bir yöntem oluşturun. Zaman serisi herhangi bir anomali içeriyorsa, yanıtın [IsAnomaly](/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.entiredetectresponse.isanomaly) değerlerini yineleyin ve bunları yazdırın `true` . Bu değerler, varsa anormal veri noktalarının dizinine karşılık gelir.

[!code-csharp[EntireDetectSampleAsync() function](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=entireDatasetExample)]

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>En son veri noktasının anomali durumunu Algıla

İstemcinin [Lastdetectasync ()](/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.lastdetectasync#Microsoft_Azure_CognitiveServices_AnomalyDetector_AnomalyDetectorClientExtensions_LastDetectAsync_Microsoft_Azure_CognitiveServices_AnomalyDetector_IAnomalyDetectorClient_Microsoft_Azure_CognitiveServices_AnomalyDetector_Models_Request_System_Threading_CancellationToken_) yöntemini `Request` nesnesiyle çağırmak ve yanıtı bir [lastdetectresponse](/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.lastdetectresponse) nesnesi olarak beklemek için bir yöntem oluşturun. Gönderilen en son veri noktasının bir anomali olup olmadığını öğrenmek için yanıtın [IsAnomaly](/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.lastdetectresponse.isanomaly) özniteliğini denetleyin.

[!code-csharp[LastDetectSampleAsync() function](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=latestPointExample)]

## <a name="detect-change-points-in-the-data-set"></a>Veri kümesindeki değişiklik noktalarını Algıla

İstemcinin [Detectchangepointasync](https://aka.ms/anomaly-detector-dotnet-ref) metodunu `Request` nesnesiyle çağırmak ve yanıtı [changepointdetectresponse](https://aka.ms/anomaly-detector-dotnet-ref) nesnesi olarak beklemek için bir yöntem oluşturun. Yanıtın ıschangepoint değerlerini denetleyin ve bunları yazdırın `true` . Bu değerler, varsa eğilim değişiklik noktalarına karşılık gelir.

[!code-csharp[DetectChangePoint() function](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=changePointExample)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı `dotnet run` uygulama dizininizdeki komutla çalıştırın.

```dotnetcli
dotnet run
```

[!INCLUDE [anomaly-detector-next-steps](../quickstart-cleanup-next-steps.md)]
