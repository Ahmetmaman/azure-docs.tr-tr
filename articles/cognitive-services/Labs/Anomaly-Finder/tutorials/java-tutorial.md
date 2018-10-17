---
title: 'Öğretici: Anomali Algılama, Java'
titlesuffix: Azure Cognitive Services
description: Anomali Algılama API'sini kullanan bir Java uygulamasını keşfedin. Özgün veri noktalarını API'ye gönderin ve beklenen değerle anomali noktalarını alın.
services: cognitive-services
author: wenya
manager: bix
ms.service: cognitive-services
ms.component: anomaly-detection
ms.topic: tutorial
ms.date: 05/01/2018
ms.author: wenya
ms.openlocfilehash: 4b544e2e59a40cebf75042c4040b84bceebcecf7
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2018
ms.locfileid: "48887709"
---
# <a name="tutorial-anomaly-detection-with-java-application"></a>Öğretici: Java uygulaması ile Anomali Algılama

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Bu makalede Anomali Algılama API'sini çağırmak için örnek Java uygulamasının kullanılması gösterilmektedir.  
Örnek, abonelik anahtarınızı kullanarak Anomali Algılama API'sine zaman serisi verileri gönderir ve API'den her bir veri noktasıyla ilgili anomali noktalarını ve beklenen değerleri alır.

## <a name="prerequisites"></a>Ön koşullar

### <a name="platform-requirements"></a>Platform gereksinimleri

Bu öğretici, [IntelliJ IDEA](https://www.jetbrains.com/idea) kullanılarak geliştirilmiştir. Ayrıca [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/index.html) sürüm 1.8+ ve güncel [Apache's Maven](http://maven.apache.org/) derleme aracını da yüklemeniz gerekir.

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Anomali Algılama için abone olun ve abonelik anahtarını alın 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]
 

## <a name="download-the-tutorial-project"></a>Öğretici projesini indirme

1. MicrosoftAnomalyDetection [Java deposuna](https://github.com/MicrosoftAnomalyDetection/java-sample) gidin.
2. Kopyala veya indir düğmesine tıklayın.
3. Öğretici projesini .zip dosyası olarak indirmek için ZIP İndir'e tıklayın.

<a name="Step1"></a>
### <a name="open-the-tutorial-project"></a>Öğretici projesini açma

1. Öğretici projesinin .zip dosyasını ayıklayın.
2. IntelliJ IDEA'da **File > Open** (Dosya > Aç) yolunu izleyin. Open File or Project (Dosya veya Proje Aç) iletişim kutusu açılır.
3. Ayıklanan projenin kök yolunu seçip Tamam'a tıklayın.
4. Projeler panelinde **src > main > java** yolunu genişletin.
5. com.microsoft.cognitiveservice.anomalydetection.Main.java dosyasına çift tıklayarak düzenleyicide yükleyin.

<a name="Step2"></a>
### <a name="replace-subscriptionkey-and-uri-region"></a>subscriptionKey ve URI bölgesini değiştirme

```
// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
public static final String subscriptionKey = "<Subscription Key>";

public static final String uriBase = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

```

<a name="Step3"></a>
### <a name="build-and-run-the-tutorial-project"></a>Öğretici projesini derleme ve çalıştırma

1. com.microsoft.cognitiveservice.anomalydetection.Main.java kaynak kodu sekmesinin herhangi bir yerinde sağ tıklayarak menüyü açın. 
2. Run (Çalıştır) 'Main.main()' öğesini seçin.
3. Örnek isteğin sonucu döndürülür ve terminalde gösterilir.

### <a name="result-of-the-tutorial-project"></a>Öğretici projesinin sonucu

[!INCLUDE [diagrams](../includes/diagrams.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
