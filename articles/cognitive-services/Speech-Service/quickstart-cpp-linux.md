---
title: 'Hızlı Başlangıç: C++ (Linux) - konuşma Hizmetleri konuşma tanıma'
titleSuffix: Azure Cognitive Services
description: Konuşma Tanıma Hizmeti SDK'sını kullanarak Linux üzerinde C++ dilinde konuşma tanımayı öğrenin
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/18/2018
ms.author: wolfma
ms.openlocfilehash: 914f86e8705916af4927aa192a41fc7636b80f3a
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228704"
---
# <a name="quickstart-recognize-speech-in-c-on-linux-by-using-the-speech-sdk"></a>Hızlı Başlangıç: Speech SDK'sı kullanarak Linux üzerinde c++ konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, Ubuntu Linux 16.04 veya 18.04 için C++ konsol uygulaması oluşturun. Bilgisayarınızın mikrofonundan gerçek zamanda konuşmayı metne dönüştürmek için Bilişsel Hizmetler [Konuşma SDK'sı](speech-sdk.md)'nı kullanırsınız. Uygulama [Linux için Konuşma SDK'sı](https://aka.ms/csspeech/linuxbinary) ve Linux dağıtımınızın C++ derleyicisi (örneğin `g++`) ile oluşturulur.

## <a name="prerequisites"></a>Önkoşullar

Bu Hızlı Başlangıcı tamamlamak için bir Konuşma hizmeti abonelik anahtarınız olması gerekir. Anahtarı ücretsiz alabilirsiniz. Ayrıntılar için bkz: [Konuşma hizmetini ücretsiz olarak deneme](get-started.md).

## <a name="install-speech-sdk"></a>Konuşma SDK'sını yükleme

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Bilişsel Hizmetler Konuşma SDK'sının geçerli sürümü: `1.2.0`.

Linux için Konuşma SDK'sı, gerek 64 bit gerekse 32 bit uygulamalar derlemek için kullanılabilir. Gerekli kitaplıklar ve üst bilgi dosyaları, tar dosyasından olarak indirilebilir https://aka.ms/csspeech/linuxbinary.

SDK'yı aşağıda gösterildiği gibi indirin ve yükleyin:

1. SDK'nın bağımlılık dosyalarının yüklü olduğundan emin olun.

   ```sh
   sudo apt-get update
   sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
   ```

1. Konuşma SDK'sı dosyalarının ayıklanacağı bir dizin seçin ve `SPEECHSDK_ROOT` ortam değişkenini bu dizine işaret edecek şekilde ayarlayın. Bu değişken, ileride komutlarda bu dizine başvurmayı kolaylaştırır. Örneğin, giriş dizininizdeki `speechsdk` dizinine başvurmak istiyorsanız, şunun gibi bir komut kullanın:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Henüz yoksa dizini oluşturun.

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Konuşma SDK'sı ikili dosyalarını içeren `.tar.gz` arşivini indirin ve açın:

   ```sh
   wget -O SpeechSDK-Linux.tar.gz https://aka.ms/csspeech/linuxbinary
   tar --strip 1 -xzf SpeechSDK-Linux.tar.gz -C "$SPEECHSDK_ROOT"
   ```

1. Açılan paketin en üst düzey dizininin içeriğini doğrulayın:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   Dizinde üçüncü taraf bildirim ve lisans dosyaları, ayrıca üstbilgi (`.h`) dosyalarını içeren bir `include` dizini ve kitaplıkları içeren bir `lib` dizini olmalıdır.

   [!INCLUDE [Linux Binary Archive Content](../../../includes/cognitive-services-speech-service-linuxbinary-content.md)]

## <a name="add-sample-code"></a>Örnek kodu ekleyin

1. `helloworld.cpp` adlı bir C++ kaynak dosyası oluşturun ve aşağıdaki kodu dosyaya yapıştırın.

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-linux/helloworld.cpp#code)]

1. Bu yeni dosyada `YourSubscriptionKey` dizesini Konuşma hizmeti abonelik anahtarınız ile değiştirin.

1. `YourServiceRegion` dizesini, aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin, ücretsiz deneme aboneliği için `westus`) değiştirin.

## <a name="build-the-app"></a>Uygulama oluşturma

> [!NOTE]
> Aşağıdaki komutları _tek bir komut satırı_ olarak girdiğinizden emin olun. Bunu yapmanın en kolay yolu, komutu her komutun yanındaki **Kopyala** düğmesini kullanarak kopyalayıp kabuk isteminize yapıştırmaktır.

* Uygulamayı derlemek için **x64**  (64 bit) bir sistemde aşağıdaki komutu çalıştırın.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

* Uygulamayı derlemek için **x86** (32 bit) bir sistemde aşağıdaki komutu çalıştırın.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libssl.so.1.0.0 -l:libcurl.so.4 -l:libasound.so.2
  ```

## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Yükleyicinin kitaplık yolunu Konuşma SDK'sı kitaplığına işaret edecek şekilde yapılandırın.

   * **x64** (64 bit) bir sistemde, aşağıdaki komutu girin.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x64"
     ```

   * **x86** (32 bit) bir sistemde, aşağıdaki komutu girin.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x86"
     ```

1. Uygulamayı çalıştırın.

   ```sh
   ./helloworld
   ```

1.  Konsol penceresinde bir istem görünerek bir şey söylemenizi ister. İngilizce bir deyim ya da cümle söyleyin. Söyledikleriniz Konuşma hizmetine aktarılır ve metne dönüştürülür; metin aynı pencerede görünür.

   ```text
   Say something...
   We recognized: What's the weather like?
   ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub üzerinde C++ örneklerini keşfedin](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
