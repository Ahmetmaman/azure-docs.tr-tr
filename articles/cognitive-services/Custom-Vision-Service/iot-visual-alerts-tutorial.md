---
title: 'Öğretici: IoT görsel uyarıları örneği'
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, bir kamera video akışından Görsel durumları tanımak ve raporlamak için bir IoT cihazındaki Özel Görüntü İşleme kullanırsınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: tutorial
ms.date: 11/23/2020
ms.author: pafarley
ms.openlocfilehash: c6405e2fcddef9ae3228ede76dfa57f7542164c8
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "96020186"
---
# <a name="tutorial-use-custom-vision-with-an-iot-device-to-report-visual-states"></a>Öğretici: Görsel durumları raporlamak için IoT cihazındaki Özel Görüntü İşleme kullanma

Bu örnek uygulama, Görsel durumları algılamak için bir cihazı kamera ile eğiten Özel Görüntü İşleme nasıl kullanacağınızı göstermektedir. Bu algılama senaryosunu, bir IoT cihazında, dışarıya aktarılmış bir ONNX modeli kullanarak çalıştırabilirsiniz.

Görsel bir durum, bir görüntünün içeriğini açıklar: boş bir oda veya kişi içeren bir oda, boş bir sürücü yolu veya kamyonun bir driveyöntemi vb. Aşağıdaki görüntüde, kameranın önüne bir muz veya Apple yerleştirildiğinde uygulamayı tespit edebilirsiniz.

![Kullanıcı arabirimi etiketlemenin animasyonunu kameranın önünde](./media/iot-visual-alerts-tutorial/scoring.gif)

Bu öğreticide şunları nasıl yapacağınızı gösterilecek:
> [!div class="checklist"]
> * Örnek uygulamayı kendi Özel Görüntü İşleme ve IoT Hub kaynaklarınızı kullanacak şekilde yapılandırın.
> * Özel Görüntü İşleme projenizi eğitebilmeniz için uygulamayı kullanın.
> * Yeni görüntüleri gerçek zamanlı olarak öğrenmek ve sonuçları Azure 'a göndermek için uygulamayı kullanın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/cognitive-services) oluşturun. 

## <a name="prerequisites"></a>Önkoşullar

* [!INCLUDE [create-resources](includes/create-resources.md)]
    > [!IMPORTANT]
    > Bu projenin bir **Compact** Image sınıflandırma projesi olması gerekir, çünkü model onnx 'e daha sonra dışarı aktaracağız.
* Ayrıca Azure 'da [bir IoT Hub kaynağı oluşturmanız](https://ms.portal.azure.com/#create/Microsoft.IotHub) gerekecektir.
* [Visual Studio 2015 veya üzeri](https://www.visualstudio.com/downloads/)
* İsteğe bağlı olarak, Windows 10 IoT Core sürüm 17763 veya üstünü çalıştıran bir IoT aygıtıdır. Uygulamayı doğrudan bilgisayarınızdan de çalıştırabilirsiniz.
   * Raspberry PI 2 ve 3 için, Windows 10 ' u doğrudan IoT panosu uygulamasından ayarlayabilirsiniz. Drangwith gibi diğer cihazlarda, [eMMC metodunu](/windows/iot-core/tutorials/quickstarter/devicesetup#flashing-with-emmc-for-dragonboard-410c-other-qualcomm-devices)kullanarak Flash 'ı kullanmanız gerekir. Yeni bir cihaz ayarlamayla ilgili yardıma ihtiyacınız varsa, bkz. Windows IoT belgelerinde [cihazınızı ayarlama](/windows/iot-core/tutorials/quickstarter/devicesetup) .

## <a name="about-the-visual-alerts-app"></a>Görsel uyarılar uygulaması hakkında

IoT görsel uyarıları uygulaması sürekli bir döngüde çalışarak dört farklı durum arasında uygun şekilde geçiş yapar:

* **Model yok**: işlem dışı durum. Uygulama bir saniyede sürekli uykuya geçecek ve kamerayı denetlecektir.
* **Eğitim görüntülerini yakalama**: Bu durumda, uygulama bir resim yakalar ve hedef özel görüntü işleme projesine bir eğitim görüntüsü olarak yükler. Uygulama daha sonra 500 MS için uyku moduna geçer ve görüntünün hedef sayısını ayarla yakalanana kadar işlemi yineler. Ardından Özel Görüntü İşleme modelin eğitimini tetikler.
* **Eğitilen model bekleniyor**: Bu durumda uygulama, hedef projenin eğitilen bir yineleme içerip içermediğini denetlemek için her sanıye özel görüntü işleme API 'sini çağırır. Bir tane bulduğunda, karşılık gelen ONNX modelini yerel bir dosyaya indirir ve **Puanlama** durumuna geçirir.
* **Puanlama**: Bu durumda, uygulama, kameradan yerel onnx modeline karşı tek bir çerçeveyi değerlendirmek IÇIN Windows ml 'yi kullanır. Ortaya çıkan görüntü sınıflandırması ekranda görüntülenir ve IoT Hub ileti olarak gönderilir. Daha sonra uygulama, yeni bir görüntü Puanlama yapmadan önce bir saniye boyunca uyku moduna geçer.

## <a name="examine-the-code-structure"></a>Kod yapısını İnceleme

Aşağıdaki dosyalar uygulamanın ana işlevlerini işler.

| Dosya | Description |
|-------------|-------------|
| [MainPage. xaml](https://github.com/Azure-Samples/Cognitive-Services-Vision-Solution-Templates/blob/master/IoTVisualAlerts/MainPage.xaml) | Bu dosya XAML Kullanıcı arabirimini tanımlar. Web Kamerası denetimini barındırır ve durum güncelleştirmeleri için kullanılan etiketleri içerir.|
| [MainPage.xaml.cs](https://github.com/Azure-Samples/Cognitive-Services-Vision-Solution-Templates/blob/master/IoTVisualAlerts/MainPage.xaml.cs) | Bu kod, XAML kullanıcı arabirimi 'nin davranışını denetler. Durum makine işleme kodunu içerir.|
| [CustomVision\CustomVisionServiceWrapper.cs](https://github.com/Azure-Samples/Cognitive-Services-Vision-Solution-Templates/blob/master/IoTVisualAlerts/CustomVision/CustomVisionServiceWrapper.cs) | Bu sınıf, Özel Görüntü İşleme Hizmeti tümleştirmeyi işleyen bir sarmalayıcıdır.|
| [CustomVision\CustomVisionONNXModel.cs](https://github.com/Azure-Samples/Cognitive-Services-Vision-Solution-Templates/blob/master/IoTVisualAlerts/CustomVision/CustomVisionONNXModel.cs) | Bu sınıf, üzerinde ONNX modelini ve Puanlama görüntülerini yüklemek için Windows ML ile tümleştirmeyi işleyen bir sarmalayıcıdır.|
| [IoTHub\IotHubWrapper.cs](https://github.com/Azure-Samples/Cognitive-Services-Vision-Solution-Templates/blob/master/IoTVisualAlerts/IoTHub/IoTHubWrapper.cs) | Bu sınıf, Puanlama sonuçlarını Azure 'a yüklemek için IoT Hub ile tümleştirmeyi işleyen bir sarmalayıcıdır.|

## <a name="set-up-the-visual-alerts-app"></a>Görsel uyarılar uygulamasını ayarlama

Bilgisayarınızda veya IoT cihazınızda çalışan IoT görsel uyarıları uygulamasını almak için bu adımları izleyin.

1. GitHub 'da [ıotgörselalalerts örneğini](https://github.com/Azure-Samples/Cognitive-Services-Vision-Solution-Templates/tree/master/IoTVisualAlerts) kopyalayın veya indirin.
1. Visual Studio 'da _ıotgörselalalerts. sln_ çözümünü açın
1. Özel Görüntü İşleme projenizi tümleştirin:
    1. _CustomVision\CustomVisionServiceWrapper.cs_ `ApiKey` betiğiyle, değişkeni eğitim anahtarınızla güncelleştirin.
    1. Sonra değişkeni, `Endpoint` anahtarınızla ilişkili uç nokta URL 'siyle güncelleştirin.
    1. Değişkeni, `targetCVSProjectGuid` kullanmak istediğiniz özel görüntü işleme projenin karşılık gelen kimliğiyle güncelleştirin. 
1. IoT Hub kaynağını ayarlama:
    1. _IoTHub\IotHubWrapper.cs_ komut dosyasında, `s_connectionString` değişkeni cihazınız için uygun bağlantı dizesiyle güncelleştirin. 
    1. Azure portal, IoT Hub örneğinizi yükleyin, **araştırıcılar** altında **IoT cihazları** ' na tıklayın, hedef cihazınızda ' i seçin (veya gerekirse bir tane oluşturun) ve **birincil bağlantı dizesi** altında bağlantı dizesini bulun. Dize, IoT Hub adı, cihaz KIMLIĞI ve paylaşılan erişim anahtarınızı içerir; Şu biçimdedir: `{your iot hub name}.azure-devices.net;DeviceId={your device id};SharedAccessKey={your access key}` .

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Uygulamayı bilgisayarınızda çalıştırıyorsanız, Visual Studio 'da hedef cihaz için **yerel makine** ' yi seçin ve hedef platform için **x64** veya **x86** ' yı seçin. Ardından programı çalıştırmak için F5 tuşuna basın. Uygulamanın, kameradan ve bir durum iletisinden canlı akışı başlatması ve görüntülemesi gerekir.

ARM işlemcisine sahip bir IoT cihazına dağıtım yapıyorsanız hedef cihaz olarak **ARM** 'yi hedef platform ve **uzak makine** olarak seçmeniz gerekir. İstendiğinde cihazınızın IP adresini sağlayın (BILGISAYARıNıZLA aynı ağda olmalıdır). Cihazı önyükledikten ve ağa bağladığınızda, Windows IoT varsayılan uygulamasından IP adresini alabilirsiniz. Programı çalıştırmak için F5 tuşuna basın.

Uygulamayı ilk kez çalıştırdığınızda, hiçbir görsel durum bilgisine sahip olmaz. Kullanılabilir model yok olarak bir durum iletisi görüntüler. 

## <a name="capture-training-images"></a>Eğitim görüntülerini yakala

Bir model ayarlamak için uygulamayı **yakalama eğitim görüntüleri** durumuna yerleştirmeniz gerekir. Aşağıdaki adımlardan birini uygulayın:
* Uygulamayı BILGISAYAR üzerinde çalıştırıyorsanız, Kullanıcı arabiriminin sağ üst köşesindeki düğmeyi kullanın.
* Uygulamayı bir IoT cihazında çalıştırıyorsanız, `EnterLearningMode` IoT Hub aracılığıyla cihazdaki yöntemi çağırın. Bunu, Azure portal IoT Hub menüsündeki cihaz girişi veya [IoT Hub Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp)gibi bir araçla çağırabilirsiniz.
 
Uygulama, **yakalama eğitim görüntülerinin** durumuna girdiğinde, hedef görüntü sayısına ulaşıncaya kadar her saniye iki görüntü yakalar. Varsayılan olarak, hedef 30 görüntülerdir, ancak istenen sayıyı IoT Hub metoduna bir bağımsız değişken olarak geçirerek bu parametreyi ayarlayabilirsiniz `EnterLearningMode` . 

Uygulama görüntüleri yakalarken, kamerayı, algılamak istediğiniz görsel durumlar türlerine (örneğin, boş bir oda, kişilerle ilgili bir oda, boş bir masa, oyuncak bir masa vb.) kullanıma sunmalısınız.

## <a name="train-the-custom-vision-model"></a>Özel Görüntü İşleme modelini eğitme

Uygulama görüntüleri yakalamayı tamamladıktan sonra, bunları karşıya yükler ve ardından **eğitilen model** durumuna geçer. Bu noktada, [özel görüntü işleme Web sitesine](https://www.customvision.ai/) gitmeniz ve yeni eğitim görüntülerini temel alan bir model derlemeniz gerekir. Aşağıdaki animasyon bu işleme bir örnek gösterir.

![Animasyon: muzlar 'in birden çok görüntüsünü etiketleme](./media/iot-visual-alerts-tutorial/labeling.gif)

Bu işlemi kendi senaryoınızla yinelemek için:

1. [Özel görüntü işleme web sitesinde](http://customvision.ai)oturum açın.
1. Artık uygulamanın karşıya yüklediği tüm eğitim görüntülerini içermelidir hedef projenizi bulun.
1. Tanımlamak istediğiniz her görsel durum için uygun görüntüleri seçin ve etiketi el ile uygulayın.
    * Örneğin, hedefiniz bir boş oda ve içindeki kişilerle bir oda arasında ayrım yapmak ise, kişilerle birlikte beş veya daha fazla görüntüyü yeni bir sınıf, **kişi** ve kişiler olmadan beş veya daha fazla görüntü etiketlemesini öneririz. **Negative** Bu, modelin iki durum arasında ayrım yapmanıza yardımcı olur.
    * Başka bir örnek olarak, amacınız bir rafı ne kadar yaklaşmayı düşünüyorsanız, **Emptyrafı**, **Partiallyfullrafı** ve **fullrafı** gibi etiketleri kullanabilirsiniz.
1. İşiniz bittiğinde **eğitme** düğmesini seçin.
1. Eğitim tamamlandıktan sonra uygulama, eğitilen bir yinelemenin kullanılabildiğini algılar. Eğitilen modeli ONNX 'e aktarma ve cihaza indirme işlemi başlatılır.

## <a name="use-the-trained-model"></a>Eğitilen modeli kullanma

Uygulama eğitilen modeli indirdikten sonra, **Puanlama** durumuna geçiş yapar ve bir sürekli döngüde kameradan Puanlama görüntülerini başlatır.

Yakalanan her görüntü için, uygulama ekranda en üstteki etiketi görüntüler. Görsel durumu algılamazsa, **hiçbir eşleşme** göstermez. Uygulama ayrıca bu iletileri IoT Hub gönderir ve algılanmakta olan bir sınıf varsa, bu ileti etiketi, güvenirlik Puanını ve adlı bir özelliği içerir `detectedClassAlert` . Bu, özellikleri temel alarak hızlı ileti yönlendirmesi yapmak isteyen IoT Hub istemciler tarafından kullanılabilir.

Buna ek olarak, örnek bir Sense HAT birimiyle bir Raspberry Pi üzerinde ne zaman çalıştığını algılamak için bir [Sense hat kitaplığı](https://github.com/emmellsoft/RPi.SenseHat) kullanır. bu nedenle, bir sınıf algıladığında tüm görüntüleme ışıkları kırmızı ve herhangi bir şeyi algılamadığında boş olarak ayarlayarak bunu bir çıktı görüntüsü olarak kullanabilir.

## <a name="reuse-the-app"></a>Uygulamayı yeniden kullanma

Uygulamayı özgün durumuna geri döndürmek isterseniz, Kullanıcı arabiriminin sağ üst köşesindeki düğmeye tıklayarak veya IoT Hub aracılığıyla yöntemi çağırarak bunu yapabilirsiniz `DeleteCurrentModel` ...

Herhangi bir noktada, en sağ üst UI düğmesine tıklayarak veya yöntemi yeniden çağırarak eğitim görüntülerini karşıya yükleme adımını yineleyebilirsiniz `EnterLearningMode` .

Uygulamayı bir cihazda çalıştırıyorsanız ve IP adresini yeniden almanız gerekiyorsa (örneğin, [Windows IoT uzak istemcisi](https://www.microsoft.com/p/windows-iot-remote-client/9nblggh5mnxz#activetab=pivot:overviewtab)aracılığıyla uzak bağlantı kurmak için), `GetIpAddress` yöntemi IoT Hub aracılığıyla çağırabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Özel Görüntü İşleme projenizi artık sürdürmek istemiyorsanız silin. [Özel görüntü işleme web sitesinde](https://customvision.ai), **Projeler** ' e gidin ve yeni projenizin altında çöp kutusu ' nu seçin.

![Çöp kutusu simgesiyle yeni projem etiketli bir panelin ekran görüntüsü](./media/csharp-tutorial/delete_project.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir IoT cihazında görsel durum bilgilerini algılayan ve sonuçları IoT Hub gönderen bir uygulamayı ayarlayıp çalıştırdınız. Ardından, kaynak kodu daha fazla araştırın ve önerilen değişikliklerden birini yapın.

> [!div class="nextstepaction"]
> [Iotgörselalalerts örneği (GitHub)](https://github.com/Azure-Samples/Cognitive-Services-Vision-Solution-Templates/tree/master/IoTVisualAlerts)

* Uygulamayı doğrudan **eğitilen model durumuna bekleme** durumuna geçirmek için bir IoT Hub yöntemi ekleyin. Bu şekilde, modeli cihazın kendisi tarafından yakalanmayan görüntülerle eğitebilirsiniz ve ardından yeni modeli, komut üzerindeki cihaza gönderebilirsiniz.
* Örnek tarafından gönderilen IoT Hub uyarılarını görselleştirmek üzere bir Power BI panosu oluşturmak için [gerçek zamanlı algılayıcı verilerini görselleştirin](../../iot-hub/iot-hub-live-data-visualization-in-power-bi.md) öğreticisini izleyin.
* Görsel durumlar algılandığında IoT Hub uyarılara yanıt veren bir mantıksal uygulama oluşturmak için [IoT uzaktan izleme](../../iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps.md) öğreticisini izleyin.