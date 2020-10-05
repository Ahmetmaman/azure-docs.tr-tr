---
author: areddish
ms.custom: devx-track-java
ms.author: areddish
ms.service: cognitive-services
ms.date: 09/15/2020
ms.openlocfilehash: 107cc24cc03c7f8716f4ee0577fc2372668adcd9
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "90604866"
---
Bu kılavuz, bir nesne algılama modeli oluşturmak üzere Java için Özel Görüntü İşleme istemci kitaplığı 'nı kullanmaya başlamanıza yardımcı olacak yönergeler ve örnek kod sağlar. Bir proje oluşturacak, Etiketler ekleyecek, projeyi eğtireceksiniz ve projenin tahmin uç nokta URL 'sini programlı bir şekilde test etmek üzere kullanacaksınız. Bu örneği kendi görüntü tanıma uygulamanızı oluşturmak için bir şablon olarak kullanın.

> [!NOTE]
> Bir nesne _algılama modelini kod yazmadan derlemek_ ve eğitebilmek istiyorsanız, bunun yerine [tarayıcı tabanlı kılavuza](../../get-started-build-detector.md) bakın.

## <a name="prerequisites"></a>Önkoşullar

- Kendi seçtiğiniz bir Java IDE
- [JDK 7 veya 8](https://aka.ms/azure-jdks) yüklendi.
- [Maven](https://maven.apache.org/) yüklendi
- [!INCLUDE [create-resources](../../includes/create-resources.md)]

## <a name="get-the-custom-vision-client-library"></a>Özel Görüntü İşleme istemci kitaplığı 'nı al

Java için Özel Görüntü İşleme bir görüntü analizi uygulaması yazmak için Özel Görüntü İşleme Maven paketlerine ihtiyacınız olacaktır. Bu paketler, indirileceği örnek projeye dahildir, ancak bunlara ayrı ayrı erişebilirsiniz.

Özel Görüntü İşleme istemci kitaplığını Maven merkezi deposunda bulabilirsiniz:
- [Eğitim SDK’sı](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
- [Tahmin SDK’sı](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

[Bilişsel Hizmetler Java SDK'sı Örnekleri](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master) projesini kopyalayın veya indirin. **Vision/CustomVision/** klasörüne gidin.

Bu Java projesi, [Özel Görüntü İşleme web sitesi](https://customvision.ai/) üzerinden erişilebilen __Sample Java OD Project__ adlı yeni bir Özel Görüntü İşleme nesne algılama projesi oluşturur. Daha sonra bir sınıflandırıcıyı eğitip test etmek için görüntüleri karşıya yükler. Bu projede, sınıflandırıcının bir nesne **çatalla** mı yoksa **makas**mi olduğunu belirlemesi amaçlanmıştır.

[!INCLUDE [get-keys](../../includes/get-keys.md)]

Program, anahtar verilerinize ortam değişkenleri olarak başvuracak şekilde yapılandırılmıştır. **Vision/customvision** klasörüne gidin ve ortam değişkenlerini ayarlamak Için aşağıdaki PowerShell komutlarını girin. 

> [!NOTE]
> Windows dışı bir işletim sistemi kullanıyorsanız, yönergeler için bkz. [ortam değişkenlerini yapılandırma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account?tabs=multiservice%2Cwindows#configure-an-environment-variable-for-authentication) .

```powershell
$env:AZURE_CUSTOMVISION_TRAINING_API_KEY ="<your training api key>"
$env:AZURE_CUSTOMVISION_PREDICTION_API_KEY ="<your prediction api key>"
```

## <a name="examine-the-code"></a>Kodu inceleme

Java IDE'nize `Vision/CustomVision` projesini yükleyin ve _CustomVisionSamples.java_ dosyasını açın. **RunSample** yöntemini bulun ve **ImageClassification_Sample** yöntemi çağırın &mdash; . Bu yöntem, bu kılavuzda kapsanmayan görüntü sınıflandırma senaryosunu yürütür. **ObjectDetection_Sample** yöntemi bu hızlı başlangıcın birincil işlevini gerçekleştirir; yöntemin tanımına gidin ve kodu inceleyin. 

## <a name="create-a-new-custom-vision-project"></a>Yeni bir Özel Görüntü İşleme projesi oluşturma

Eğitim istemcisi ve nesne algılama projesini oluşturan kod bloğuna gidin. Oluşturulan proje, daha önce ziyaret ettiğiniz [Özel Görüntü İşleme web sitesinde](https://customvision.ai/) gösterilir. Projenizi oluştururken diğer seçenekleri belirtmek için bkz. [CreateProject](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.customvision.training.trainings.createproject?view=azure-java-stable#com_microsoft_azure_cognitiveservices_vision_customvision_training_Trainings_createProject_String_CreateProjectOptionalParameter_&preserve-view=true) yöntemi aşırı yüklemeleri ( [bir algılayıcı derleme](../../get-started-build-detector.md) Web portalı kılavuzunda açıklanmıştır).

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_create_od)]

## <a name="add-tags-to-your-project"></a>Projenize etiketler ekleme

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_tags_od)]

## <a name="upload-and-tag-images"></a>Görüntüleri karşıya yükleme ve etiketleme

Nesne algılama projelerinde resimleri etiketlediğinizde, her etiketlenmiş nesnenin bölgesini normalleştirilmiş koordinatları kullanarak belirtmeniz gerekir. `regionMap` Haritasının tanımına gidin. Bu kod, örnek görüntülerin her birini etiketlendikleri bölgeyle ilişkilendirir.

> [!NOTE]
> Bölgelerin koordinatlarını işaretlemek için bir tıklama ve sürükleme yardımcı programına sahip değilseniz, [Customvision.ai](https://www.customvision.ai/)adresindeki Web Kullanıcı arabirimini kullanabilirsiniz. Bu örnekte, koordinatlar zaten sağlanmış.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_od_mapping)]

Ardından projeye görüntüleri ekleyen kod bloğuna atlayın. Görüntüler projenin **src/main/resources** klasöründen okunur ve uygun etiketleri ve bölge koordinatlarıyla hizmete yüklenir.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_upload_od)]

Önceki kod parçacığı, görüntüleri kaynak akışları olarak alan ve hizmete yükleyen iki yardımcı işlevden yararlanmasına neden olur (tek bir toplu işte en fazla 64 görüntü yükleyebilirsiniz).

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_helpers)]

## <a name="train-the-project-and-publish"></a>Projeyi eğitme ve yayımlama

Bu kod, tahmin modelinin ilk yinelemesini oluşturur ve ardından bu yinelemeyi tahmin uç noktasına yayınlar. Yayımlanan yinelemeye verilen ad, tahmin istekleri göndermek için kullanılabilir. Bir yineleme, yayımlanana kadar tahmin uç noktasında kullanılamaz.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_train_od)]

## <a name="use-the-prediction-endpoint"></a>Tahmin uç noktasını kullanma

Burada `predictor` nesnesiyle gösterilen tahmin uç noktası, bir görüntüyü geçerli modele göndermek ve sınıflandırma tahmini almak için kullandığınız başvurudur. Bu örnekte `predictor`, tahmin anahtarı ortam değişkeni kullanılarak başka bir yerde tanımlanır.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_prediction_od)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Maven kullanarak çözümü derlemek ve çalıştırmak için, bir komut isteminde proje dizinine (**Vision/customvision**) gidin ve Çalıştır komutunu yürütün:

```bash
mvn compile exec:java
```

Günlüğe kaydetme ve tahmin sonuçları için konsol çıkışını görüntüleyin. Ardından test görüntüsünün etiketlendiğini ve algılama bölgesinin doğru olduğunu onaylayabilirsiniz.

[!INCLUDE [clean-od-project](../../includes/clean-od-project.md)]

## <a name="next-steps"></a>Sonraki adımlar

Artık koddaki nesne algılama işleminin her adımını tamamladınız. Bu örnek tek bir eğitim yinelemesi yürütür, ancak genellikle modelinizi daha doğru hale getirmek için birden çok kez eğitmeniz ve test etmeniz gerekir. Sonraki kılavuzda görüntü sınıflandırma konusu üstünde durulur ancak temel ilkeleri nesne algılamaya benzer.

> [!div class="nextstepaction"]
> [Modeli test etme ve yeniden eğitme](../../test-your-model.md)

* [Özel Görüntü İşleme nedir?](../../overview.md)
* [SDK başvuru belgeleri](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/customvision?view=azure-java-stable)