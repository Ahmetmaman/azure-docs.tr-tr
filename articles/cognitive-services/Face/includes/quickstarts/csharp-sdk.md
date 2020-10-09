---
title: Yüz .NET istemci kitaplığı hızlı başlangıç
description: Yüzleri saptamak için .NET için yüz istemci kitaplığı 'nı kullanın, benzer (görüntüye göre arama), yüzleri (yüz tanıma arama) belirleyip yüz verilerinizi geçirin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: include
ms.date: 09/17/2020
ms.author: pafarley
ms.openlocfilehash: 6ef0791eeec169bb925b8f667523203beaacdd2c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2020
ms.locfileid: "91859776"
---
.NET için yüz istemci kitaplığını kullanarak yüz tanıma ile çalışmaya başlayın. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin. Yüz tanıma hizmeti, görüntülerdeki insan yüzlerini algılayıp tanımayı sağlayan gelişmiş algoritmalara erişmenizi sağlar.

.NET için yüz istemci kitaplığı 'nı kullanarak şunları yapın:

* [Bir görüntüdeki yüzleri algılama](#detect-faces-in-an-image)
* [Benzer yüzeyleri bulun](#find-similar-faces)
* [Kişi grubu oluşturma ve eğitme](#create-and-train-a-person-group)
* [Yüz tanıma](#identify-a-face)

[Başvuru belgeleri](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/faceapi?view=azure-dotnet)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Vision.Face)  |  [Paket (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.6.0-preview.1)  |  [Örnekler](https://docs.microsoft.com/samples/browse/?products=azure&term=face)

## <a name="prerequisites"></a>Ön koşullar

* [.NET Core](https://dotnet.microsoft.com/download/dotnet-core)'un geçerli sürümü.
* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/)
* Azure aboneliğiniz olduktan sonra, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesFace"  title=" bir yüz kaynağı oluşturun "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> Azure Portal anahtar ve uç noktanıza ulaşmak için bir yüz kaynağı oluşturun. Dağıtıldıktan sonra **Kaynağa Git ' e**tıklayın.
    * Uygulamanızı Yüz Tanıma API'si bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.
* Anahtar ve uç nokta aldıktan sonra, ve sırasıyla adlı anahtar ve uç nokta URL 'SI için [ortam değişkenleri oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) `FACE_SUBSCRIPTION_KEY` `FACE_ENDPOINT` .

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-c-application"></a>Yeni bir C# uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE 'de yeni bir .NET Core uygulaması oluşturun. 

Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` adıyla yeni bir konsol uygulaması oluşturmak için komutunu kullanın `face-quickstart` . Bu komut, tek bir kaynak dosyası olan basit bir "Merhaba Dünya" C# projesi oluşturur: *program.cs*. 

```dotnetcli
dotnet new console -n face-quickstart
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

Proje dizininden, *program.cs* dosyasını tercih ettiğiniz DÜZENLEYICIDE veya IDE 'de açın. Aşağıdaki yönergeleri ekleyin `using` :

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_using)]

Uygulamanın `Main` yönteminde, kaynağınızın Azure uç noktası ve anahtarı için değişkenler oluşturun.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_mainvars)]

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

Uygulama dizini içinde, aşağıdaki komutla .NET için yüz istemci kitaplığı 'nı yüklersiniz:

```dotnetcli
dotnet add package Microsoft.Azure.CognitiveServices.Vision.Face --version 2.6.0-preview.1
```

Visual Studio IDE kullanıyorsanız, istemci kitaplığı indirilebilir bir NuGet paketi olarak kullanılabilir.

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler, yüz .NET istemci kitaplığının bazı önemli özelliklerini işler:

|Ad|Açıklama|
|---|---|
|[FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet) | Bu sınıf, yüz hizmetini kullanma yetkinizi temsil eder ve tüm yüz işlevleri için buna ihtiyacınız vardır. Bunu Abonelik bilgileriniz ile birlikte başlatır ve diğer sınıfların örneklerini oluşturmak için kullanırsınız. |
|[Çok yönlü Işlemler](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperations?view=azure-dotnet)|Bu sınıf, insan yüzeyleri ile gerçekleştirebileceğiniz temel algılama ve tanıma görevlerini işler. |
|[DetectedFace](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.detectedface?view=azure-dotnet)|Bu sınıf, görüntüde tek bir yüz tarafından algılanan tüm verileri temsil eder. Yüz hakkında ayrıntılı bilgi almak için bu uygulamayı kullanabilirsiniz.|
|[FaceListOperations](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.facelistoperations?view=azure-dotnet)|Bu sınıf, bir assıralanan yüz kümesini depolayan, bulutta depolanan çok **yönlü liste** yapılarını yönetir. |
|[Persongrouppersonextensıons](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongrouppersonextensions?view=azure-dotnet)| Bu sınıf, tek bir kişiye ait olan bir yüzey kümesini depolayan, bulutta depolanan **kişi** yapılarını yönetir.|
|[PersonGroupOperations](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.persongroupoperations?view=azure-dotnet)| Bu sınıf, bir dizi yönetilen **kişi** nesnesini depolayan, bulut ile depolanmış olan **persongroup** yapılarını yönetir. |
|[ShapshotOperations](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.snapshotoperations?view=azure-dotnet)|Bu sınıf, anlık görüntü işlevselliğini yönetir. Tüm bulut tabanlı yüz verilerinizi geçici olarak kaydetmek ve bu verileri yeni bir Azure aboneliğine geçirmek için kullanabilirsiniz. |

## <a name="code-examples"></a>Kod örnekleri

Aşağıdaki kod parçacıkları, .NET için yüz istemci kitaplığı ile aşağıdaki görevlerin nasıl yapılacağını göstermektedir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Bir görüntüdeki yüzleri algılama](#detect-faces-in-an-image)
* [Benzer yüzeyleri bulun](#find-similar-faces)
* [Kişi grubu oluşturma ve eğitme](#create-and-train-a-person-group)
* [Yüz tanıma](#identify-a-face)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

> [!NOTE]
> Bu hızlı başlangıçta, ve adında yüz anahtarınız ve uç noktanız için [ortam değişkenleri oluşturmuş](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) olduğunuz `FACE_SUBSCRIPTION_KEY` varsayılır `FACE_ENDPOINT` .

Yeni bir yöntemde, uç nokta ve anahtarınızla bir istemci örneği oluşturun. Anahtarınızla bir **[ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.apikeyserviceclientcredentials?view=azure-dotnet)** nesnesi oluşturun ve bir **[faceclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet)** nesnesi oluşturmak için bunu uç noktanızla birlikte kullanın.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_auth)]

Muhtemelen bu yöntemi yönteminde çağırmak isteyeceksiniz `Main` .

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_client)]

### <a name="declare-helper-fields"></a>Yardımcı alanları bildirme

Daha sonra ekleyeceğiniz birçok yüz işlemi için aşağıdaki alanlar gereklidir. Sınıfınızın kökünde aşağıdaki URL dizesini tanımlayın. Bu URL, örnek görüntülerin bir klasörünü işaret eder.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_image_url)]

Farklı tanıma modeli türlerine işaret etmek için dizeleri tanımlayın. Daha sonra, yüz algılama için kullanmak istediğiniz tanıma modelini belirleyebileceksiniz. Bu seçenekler hakkında bilgi için bkz. [bir tanıma modeli belirtme](../../Face-API-How-to-Topics/specify-recognition-model.md) .

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_detect_models)]

## <a name="detect-faces-in-an-image"></a>Bir görüntüdeki yüzleri algılama

Aşağıdaki yöntem çağrısını **Main** yöntemine ekleyin. Sonraki yöntemi tanımlayacaksınız. Son algılama işlemi bir **[Faceclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet)** nesnesi, bir görüntü URL 'si ve bir tanıma modeli alır.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_detect_call)]

### <a name="get-detected-face-objects"></a>Algılanan yüz nesneleri Al

Sonraki kod bloğunda, `DetectFaceExtract` yöntemi VERILEN URL 'nin üç görüntüde bulunan yüzeyleri algılar ve program belleğindeki **[algılayıcısı](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.detectedface?view=azure-dotnet)** olan nesnelerin bir listesini oluşturur. **[Faceattributetype](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.faceattributetype?view=azure-dotnet)** değerlerinin listesi hangi özelliklerin ayıklanacağını belirtir. 

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_detect)]

### <a name="display-detected-face-data"></a>Algılanan yüz verileri görüntüle

Yöntemin geri kalanı `DetectFaceExtract` algılanan her bir yüzey için öznitelik verilerini ayrıştırır ve yazdırır. Her öznitelik, özgün yüz algılama API çağrısında ( **[Faceattributetype](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.faceattributetype?view=azure-dotnet)** listesinde) ayrı olarak belirtilmelidir. Aşağıdaki kod her özniteliği işler, ancak büyük olasılıkla yalnızca bir veya birkaç tane kullanmanız gerekir.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_detect_parse)]

## <a name="find-similar-faces"></a>Benzer yüzleri bulma

Aşağıdaki kod, algılanan tek bir yüzeyi (kaynak) alır ve eşleşmeleri (yüz görüntüye göre arama) bulmak için bir dizi diğer yüzeyi (hedef) arar. Bir eşleşme bulduğunda, eşleşen yüzün KIMLIĞINI konsola yazdırır.

### <a name="detect-faces-for-comparison"></a>Karşılaştırılacak Yüzleri Algıla

İlk olarak, ikinci bir yüz algılama yöntemi tanımlayın. Bunları karşılaştırabilmeniz için görüntülerdeki yüzeyleri saptamanız gerekir ve bu algılama yöntemi karşılaştırma işlemleri için iyileştirilmiştir. Yukarıdaki bölümde olduğu gibi ayrıntılı yüz özniteliklerini ayıklamaz ve farklı bir tanıma modeli kullanır.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_face_detect_recognize)]

### <a name="find-matches"></a>Eşleşmeleri bul

Aşağıdaki yöntem, bir hedef görüntüler kümesindeki yüzeyleri ve tek bir kaynak görüntüyü algılar. Ardından, bunları karşılaştırır ve kaynak görüntüye benzer tüm hedef görüntüleri bulur.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_find_similar)]

### <a name="print-matches"></a>Yazdırma eşleşmeleri

Aşağıdaki kod, eşleşme ayrıntılarını konsola yazdırır:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_find_similar_print)]

## <a name="identify-a-face"></a>Yüz tanıma

Tanımlama işlemi, bir kişinin (veya birden çok kişinin) bir görüntüsünü alır ve görüntüdeki her bir yüzün kimliğini bulmak için (yüz tanıma arama) arar. Algılanan her yüzü, yüz özellikleri bilinen farklı **kişi** nesnelerinin bir veritabanı olan bir **persongroup**ile karşılaştırır. Bu işlemi tanımlamak için önce bir **Persongroup** oluşturmanız ve eğitmeniz gerekir

### <a name="create-and-train-a-person-group"></a>Kişi grubu oluşturma ve eğitme

Aşağıdaki kod, altı farklı **kişi** nesnesi Ile bir **persongroup** oluşturur. Her **kişiyi** bir örnek görüntü kümesiyle ilişkilendirir ve sonra her kişiyi yüz özellikleriyle tanıyacak şekilde algılar. **Person** ve **Persongroup** nesneleri, Verify, tanımla ve Gruplandır işlemlerinde kullanılır.

#### <a name="create-persongroup"></a>Kişilik grubu oluştur

Oluşturduğunuz **Persongroup** 'un kimliğini temsil etmek için sınıfınızın kökünde bir dize değişkeni bildirin.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_persongroup_declare)]

Yeni bir yöntemde aşağıdaki kodu ekleyin. Bu yöntem, tanımla işlemini çalıştırır. İlk kod bloğu, kişilerin adlarını örnek görüntülerle ilişkilendirir.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_persongroup_files)]

Sonra, Sözlükteki her kişi için bir **kişi** nesnesi oluşturmak ve uygun görüntülerden yüz verilerini eklemek için aşağıdaki kodu ekleyin. Her **kişi** nesnesi, benzersiz kimlik dizesi aracılığıyla aynı **persongroup** ile ilişkilendirilir. , `client` `url` Ve değişkenlerini `RECOGNITION_MODEL1` Bu yönteme geçirmeye unutmayın.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_persongroup_create)]

#### <a name="train-persongroup"></a>Kişilik grubu eğitme

Görüntülerinizden yüz verilerini ayıkladıktan ve farklı **kişi** nesnelerine sıraladıktan sonra, **kişi** nesnelerinden her biriyle ilişkili görsel özellikleri belirlemek için **persongroup** 'u eğitmeniz gerekir. Aşağıdaki kod, zaman uyumsuz **eğitme** yöntemini çağırır ve sonuçları, durumu konsola yazdırarak tarar.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_persongroup_train)]

Bu **kişi** grubu ve ilişkili **kişi** nesneleri artık doğrulama, tanımla veya grupla işlemlerinde kullanılmak üzere hazırdır.

### <a name="get-a-test-image"></a>Test görüntüsü al

[Bir kişi grubu oluşturma ve eğitme](#create-and-train-a-person-group) için kodun bir değişken tanımlıyor olduğuna dikkat edin `sourceImageFileName` . Bu değişken, &mdash; tanımlamak üzere kişileri içeren görüntünün kaynak görüntüsüne karşılık gelir.

### <a name="identify-faces"></a>Yüzleri belirleme

Aşağıdaki kod, kaynak görüntüyü alır ve görüntüde algılanan tüm yüzlerin bir listesini oluşturur. Bunlar, **Persongroup**ile ilgili olarak tanımlanabilecek yüzlerdir.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_identify_sources)]

Sonraki kod parçacığı, **ıdentıyasync** işlemini çağırır ve sonuçları konsola yazdırır. Burada hizmet, kaynak görüntüdeki her yüzü, belirtilen Person **grubundaki**bir **kişiye** eşleştirmeye çalışır. Bu, tanımlana yöntemini kapatır.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_identify)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Yüz tanıma uygulamanızı, komutuyla birlikte uygulama dizininden çalıştırın `dotnet run` .

```dotnetcli
dotnet run
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

Bu hızlı başlangıçta bir **kişilik grubu** oluşturduysanız ve silmek istiyorsanız, programınızda aşağıdaki kodu çalıştırın:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_persongroup_delete)]

Aşağıdaki kodla birlikte silme yöntemini tanımlayın:

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/Face/FaceQuickstart.cs?name=snippet_deletepersongroup)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, temel bir yüz tanıma görevi için .NET için yüz istemci kitaplığı 'nı nasıl kullanacağınızı öğrendiniz. Daha sonra, kitaplık hakkında daha fazla bilgi edinmek için başvuru belgelerini inceleyin.

> [!div class="nextstepaction"]
> [Yüz Tanıma API'si Başvurusu (.NET)](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/faceapi?view=azure-dotnet)

* [Yüz Tanıma hizmeti nedir?](../../overview.md)
* Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/Face/FaceQuickstart.cs)' da bulunabilir.
