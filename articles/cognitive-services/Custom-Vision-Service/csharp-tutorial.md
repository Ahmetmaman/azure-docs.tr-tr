---
title: "Hızlı Başlangıç: Bir görüntü sınıflandırma proje için özel görüntü işleme SDK'sı ile oluşturmaC#"
titlesuffix: Azure Cognitive Services
description: Bir proje oluşturun, etiketler ekleyin, görüntüleri karşıya yükleyin, projenizi eğitin ve C# ile .NET SDK'yı kullanarak bir tahminde bulunun.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: quickstart
ms.date: 10/31/2018
ms.author: anroth
ms.openlocfilehash: aaffd5e47c8465d44c2dbcdde3f40cf6e6995e55
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54079094"
---
# <a name="quickstart-create-an-image-classification-project-with-the-custom-vision-net-sdk"></a>Hızlı Başlangıç: Özel görüntü işleme .NET SDK ile birlikte bir görüntü sınıflandırma projesi oluşturma

Bu makalede, Özel Görüntü İşleme SDK'sını C# ile kullanarak görüntü sınıflandırma modeli oluşturmaya başlarken size yardımcı olacak bilgiler ve örnek kod sağlanır. Oluşturulduktan sonra etiketler ekleyebilir, görüntüleri karşıya yükleyebilir, projeyi eğitebilir, projenin varsayılan tahmin uç nokta URL’sini alabilir ve bir görüntüyü programlama yoluyla test etmek için uç noktayı kullanabilirsiniz. Kendi .NET uygulamanızı oluştururken bu örneği şablon olarak kullanın. Kod _içermeyen_ bir sınıflandırma modeli oluşturma ve kullama işlemi yapmak istiyorsanız, [tarayıcı tabanlı kılavuz](getting-started-build-a-classifier.md) konusuna bakın.

## <a name="prerequisites"></a>Önkoşullar
- [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/)'nin herhangi bir sürümü


## <a name="get-the-custom-vision-sdk-and-sample-code"></a>Özel Görüntü İşleme SDK’sını ve örnek kodu alma
Özel Görüntü İşleme kullanan bir .NET uygulaması yazmak için Özel Görüntü İşleme NuGet paketlerine ihtiyacınız olacaktır. Bunlar indireceğiniz örnek projeye dahil edilmiştir, ama burada bunlara tek tek erişebilirsiniz.

* [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training/)
* [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction/)

[Bilişsel Hizmetler .NET Örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples) projesini kopyalayın veya indirin. Visual Studio'da **CustomVision/ImageClassification** klasörüne gidin ve _ImageClassification.csproj_ dosyasını açın.

Bu Visual Studio projesi, [Özel Görüntü İşleme web sitesi](https://customvision.ai/) üzerinden erişilebilen __My New Project__ adlı yeni bir proje oluşturur. Daha sonra bir sınıflandırıcıyı eğitip test etmek için görüntüleri karşıya yükler. Bu projede sınıflandırıcının, bir ağacın __Köknar__ mı yoksa __Japon Kirazı__ mı olduğunu belirlemesi hedeflenmiştir.

[!INCLUDE [get-keys](includes/get-keys.md)]

## <a name="understand-the-code"></a>Kodu anlama

_Program.cs_ dosyasını açın ve kodu inceleyin. Abonelik anahtarlarınızı **Main** yöntemindeki uygun tanımlara ekleyin.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?range=21-30)]

Uç nokta parametresi, özel görüntü işleme kaynakları içeren Azure kaynak grubu içinde oluşturulduğu bölgeye işaret etmelidir. Bu örnek için Orta Güney ABD bölgesinde varsayar ve kullanın:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?range=14-14)]

Aşağıdaki kod satırları projenin birincil işlevini yürütür.

### <a name="create-a-new-custom-vision-service-project"></a>Yeni bir Özel Görüntü İşleme hizmeti projesi oluşturma

Oluşturulan proje, daha önce ziyaret ettiğiniz [Özel Görüntü İşleme web sitesinde](https://customvision.ai/) gösterilir. 

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?range=32-34)]

### <a name="create-tags-in-the-project"></a>Projede etiketler oluşturma

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?range=36-38)]

### <a name="upload-and-tag-images"></a>Görüntüleri karşıya yükleme ve etiketleme

Bu projenin görüntüleri dahil edilmiştir. Bunlara, _Program.cs_'de **LoadImagesFromDisk** yönteminde başvurulur.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?range=40-55)]

### <a name="train-the-classifier"></a>Sınıflandırıcıyı eğitme

Bu kod, projedeki ilk yinelemeyi oluşturur ve bunu varsayılan yineleme olarak işaretler. Varsayılan yineleme, tahmin isteklerine yanıt verecek modelin sürümünü yansıtır. Bu modeli her yeniden eğitişinizde bunu güncelleştirmeniz gerekir.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?range=57-73)]

### <a name="set-the-prediction-endpoint"></a>Tahmin uç noktasını ayarlama

Tahmin uç noktası, bir görüntüyü geçerli modele göndermek ve sınıflandırma tahmini almak için kullanabileceğiniz başvurudur.
 
[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?range=77-82)]
 
### <a name="submit-an-image-to-the-default-prediction-endpoint"></a>Varsayılan tahmin uç noktasına bir görüntü gönderme

Bu betikte, test görüntüsü **LoadImagesFromDisk** yönteminde yüklenir ve modelin tahmin çıkışı konsolda görüntülenir.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?range=84-92)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama çalışırken bir konsol penceresi açmalı ve aşağıdaki çıkışı yazmalıdır:

```
Creating new project:
        Uploading images
        Training
Done!

Making a prediction:
        Hemlock: 95.0%
        Japanese Cherry: 0.0%
```

Ardından test görüntüsünün (**Images/Test/** yolunda bulunur) düzgün etiketlendiğini doğrulayabilirsiniz. Uygulamadan çıkmak için herhangi bir tuşa basın. Ayrıca [Özel Görüntü İşleme web sitesine](https://customvision.ai) geri dönebilir ve yeni oluşturulan projenizin geçerli durumunu görebilirsiniz.

[!INCLUDE [clean-ic-project](includes/clean-ic-project.md)]

## <a name="next-steps"></a>Sonraki adımlar

Artık kodda görüntü sınıflandırma işleminin her adımının nasıl uygulanabileceğini gördünüz. Bu örnek tek bir eğitim yinelemesi yürütür ama modelinizin daha doğru olmasını sağlamak için çoğunlukla birden çok kez eğitmeniz ve test etmeniz gerekecektir.

> [!div class="nextstepaction"]
> [Modeli test etme ve yeniden eğitme](test-your-model.md)