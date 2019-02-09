---
title: Konuşma hizmeti SDK'sı - konuşma hizmetleri hakkında
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti Yazılım Geliştirme Seti (SDK), uygulamalarınızın yerel konuşma hizmeti işleve yazılım geliştirmeyi kolaylaştıran erişmenizi sağlar. Bu makalede, SDK'sı için Windows, Linux ve Android hakkında ek ayrıntılar sağlar.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/18/2018
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: 5d98ae7fe4062ce88ee2a3df5f9bb4d992d4023c
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2019
ms.locfileid: "55984690"
---
# <a name="about-the-speech-service-sdk"></a>Konuşma hizmeti SDK'sı hakkında

Konuşma hizmeti Yazılım Geliştirme Seti (SDK), uygulamalarınızın yerel konuşma hizmeti işleve yazılım geliştirmeyi kolaylaştıran erişmenizi sağlar. Şu anda Erişim SDK'sı sağlar **Konuşmayı metne dönüştürme**, **konuşma çevirisi**, ve **amaç tanıma**.

[!INCLUDE [Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

## <a name="get-the-sdk"></a>SDK'yı edinme

### <a name="windows"></a>Windows

Windows için aşağıdaki dilleri destekliyoruz:

* C#C++: (UWP ve .NET) Başvuru ve bizim konuşma SDK'sı NuGet paketi en son sürümünü kullanın. Paket, 32-bit ve 64 bit istemci kitaplıkları ve yönetilen (.NET) kitaplıkları içerir. SDK, Visual Studio'da NuGet kullanarak yüklenebilir. Arama **Microsoft.CognitiveServices.Speech**.

* Java: Başvuru ve yalnızca Windows x64 destekleyen bizim konuşma SDK Maven paketini en son sürümünü kullanın. Maven projenizde ekleyin `https://csspeechstorage.blob.core.windows.net/maven/` bir ek bir depo ve başvuru olarak `com.microsoft.cognitiveservices.speech:client-sdk:1.2.0` bağımlılık olarak.

### <a name="linux"></a>Linux

> [!NOTE]
> Şu anda yalnızca Ubuntu 16.04 ve 18.04 PC'de (x86 veya C++ geliştirme x64 ve .NET Core ve Java için x64) destekliyoruz.

Gerekli derleyici ve kitaplıkları, aşağıdaki Kabuk komutları çalıştırarak yüklü olduğundan emin olun:

```sh
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2
```

* C# İÇİN: Başvuru ve bizim konuşma SDK'sı NuGet paketi en son sürümünü kullanın. SDK'ya başvurmak için şu paket başvuruyu projenize ekleyin:

  ```xml
  <PackageReference Include="Microsoft.CognitiveServices.Speech" Version="1.2.0" />
  ```

* Java: Başvuru ve bizim konuşma SDK Maven paketini en son sürümünü kullanın. Maven projenizde ekleyin `https://csspeechstorage.blob.core.windows.net/maven/` bir ek bir depo ve başvuru olarak `com.microsoft.cognitiveservices.speech:client-sdk:1.2.0` bağımlılık olarak.

* C++: SDK'yı indirin bir [.tar paketinin](https://aka.ms/csspeech/linuxbinary) ve tercih ettiğiniz bir dizindeki dosyaları paket. Aşağıdaki tablo, SDK'sı klasör yapısını gösterir:

  |Yol|Açıklama|
  |-|-|
  |`license.md`|Lisans|
  |`ThirdPartyNotices.md`|Üçüncü taraf bildirimleri|
  |`include`|C ve C++ üstbilgi dosyaları|
  |`lib/x64`|Yerel x64 uygulamanızla bağlama kitaplığı|
  |`lib/x86`|Yerel x86 uygulamanızla bağlama kitaplığı|

  Bir uygulama oluşturmak için kopyalayabilir veya gerekli ikili dosyaların (ve kitaplıkları) geliştirme ortamınıza taşıyabilirsiniz. Yapı işleminiz için gereken şekilde bunları ekleyebilirsiniz.

### <a name="android"></a>Android

Android için Java SDK'sı olarak paketlenmiş bir [AAR (Android kitaplık)](https://developer.android.com/studio/projects/android-library), gerekli kitaplıkları ve Android gerekli izinleri içerir. Maven deponun barındırılan `https://csspeechstorage.blob.core.windows.net/maven/` paketi olarak `com.microsoft.cognitiveservices.speech:client-sdk:1.2.0`.

Android Studio projenizden paketi kullanmak için aşağıdaki değişiklikleri yapın:

* Proje düzeyi build.gradle dosyasına ekleyin `repository` bölümü:

  ```gradle
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

* Modül düzeyi build.gradle dosyasına ekleyin `dependencies` bölümü:

  ```gradle
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:1.2.0'
  ```

Java SDK'sını da olan parçası [konuşma cihaz SDK'sı](speech-devices-sdk.md).

[!INCLUDE [Get the samples](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [C# ' de Konuşma tanıma öğrenin](quickstart-csharp-dotnet-windows.md)
