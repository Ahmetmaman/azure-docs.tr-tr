---
title: Azure Media Services kullanarak karşıya yükleme, kodlama ve akışla aktarma | Microsoft Docs
description: REST kullanarak Azure Media Services ile bir dosyayı karşıya yüklemek, videoyu kodlamak ve içeriğinizi akışla aktarmak için bu öğreticinin adımlarını izleyin.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: tutorial
ms.custom: mvc
ms.date: 07/16/2018
ms.author: juliako
ms.openlocfilehash: 5cc109467f9affa9cf5f43342203e8d4298269e0
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39115215"
---
# <a name="tutorial-upload-encode-and-stream-videos-with-rest"></a>Öğretici: REST ile karşıya video yükleme, bunları kodlama ve akışla aktarma

Bu öğreticide Azure Media Services ile video dosyalarını karşıya yükleme, kodlama ve akışla aktarma işlemleri gösterilmektedir.

Media Services, medya dosyalarınızı pek çok tarayıcı ve cihazda oynatılabilecek biçimlerde kodlamanızı sağlar. Örneğin, içeriğinizi Apple'ın HLS veya MPEG DASH biçimlerinde akışla göndermek isteyebilirsiniz. Akışla göndermeden önce yüksek kaliteli dijital medya dosyanızı kodlamanız gerekir. Kodlama yönergeleri için bkz. [Kodlama kavramı](encoding-concept.md).

![Videoyu yürütme](./media/stream-files-tutorial-with-api/final-video.png)

Bu öğretici şunların nasıl yapıldığını gösterir:    

> [!div class="checklist"]
> * Media Services hesabı oluşturma
> * Media Services API’sine erişim
> * Postman dosyalarını indirme
> * Postman'i yapılandırma
> * Postman kullanarak istek gönderme
> * Akış URL’sini test etme
> * Kaynakları temizleme

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

- AMS REST öğreticilerinden bazılarında gösterilen REST API'lerini yürütmek için [Postman](https://www.getpostman.com/) REST istemcisini yükleyin. 

    Biz **Postman**'ı kullanıyoruz, ancak herhangi bir REST aracı da olabilir. Diğer seçenekler şunlardır: REST eklentili **Visual Studio Code** veya **Telerik Fiddler**. 

## <a name="download-postman-files"></a>Postman dosyalarını indirme

Postman koleksiyonunu ve ortam dosyalarını içeren bir GitHub deposunu kopyalayın.

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-rest-postman.git
 ```

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="configure-postman"></a>Postman'i yapılandırma

Bu bölümde Postman yapılandırılmaktadır.

### <a name="configure-the-environment"></a>Ortamı yapılandırma 

1. **Postman**'ı açın.
2. Ekranın sağ tarafında **Ortamı yönet** seçeneğini belirleyin.

    ![Ortamı yönetme](./media/develop-with-postman/postman-import-env.png)
4. **Ortamı yönet** iletişim kutusunda **İçe aktar**'ı tıklatın.
2. `https://github.com/Azure-Samples/media-services-v3-rest-postman.git` kopyasını oluşturduğunuzda indirilen `Azure Media Service v3 Environment.postman_environment.json` dosyasına gidin.
6. **Azure Media Service v3 Environment** ortamı eklenir.

    > [!Note]
    > Erişim değişkenlerini yukarıdaki **Media Services API'sine erişme** bölümünden aldığınız değerlerle güncelleştirin.

7. Seçili dosyaya çift tıklayın ve [API'ye erişim](#access-the-media-services-api) adımlarını izleyerek aldığınız değerleri girin.
8. İletişim kutusunu kapatın.
9. Aşağı açılan listeden **Azure Media Service v3 Environment** ortamını seçin.

    ![Ortamı seçme](./media/develop-with-postman/choose-env.png)
   
### <a name="configure-the-collection"></a>Koleksiyonu yapılandırma

1. Koleksiyon dosyasını içe aktarmak için **İçe Aktar**'ı tıklatın.
1. `https://github.com/Azure-Samples/media-services-v3-rest-postman.git` kopyasını oluşturduğunuzda indirilen `Media Services v3.postman_collection.json` dosyasına gidin
3. **Media Services v3.postman_collection.json** dosyasını seçin.

    ![Dosya içe aktarma](./media/develop-with-postman/postman-import-collection.png)

## <a name="send-requests-using-postman"></a>Postman kullanarak istek gönderme

Bu bölümde, dosyanızı akışla aktarabilmeniz için kodlama ve URL oluşturma ile ilgili istekler göndereceğiz. Özellikle de aşağıdaki istekleri göndereceğiz:

1. Hizmet Sorumlusu Kimlik Doğrulaması için Azure AD Belirteci alma
2. Çıktı varlığı oluşturma
3. Dönüşüm oluşturma
4. Bir iş oluşturma 
5. Akış bulucusu oluşturma
6. Akış bulucusu yollarını listeleme

> [!Note]
>  Bu öğretici, tüm kaynakları benzersiz adlarla oluşturmakta olduğunuzu varsayar.  

### <a name="get-azure-ad-token"></a>Azure AD Belirteci alma 

1. Postman'ın sol penceresinde, "Adım 1: AAD Kimlik doğrulama belirteci alma"'yı seçin.
2. Sonra, "Hizmet Sorumlusu Kimlik Doğrulaması için Azure AD Belirteci alma"'yı seçin.
3. **Gönder**’e basın.

    Aşağıdaki **POST** işlemi gönderilir.

    ```
    https://login.microsoftonline.com/:tenantId/oauth2/token
    ```

4. Yanıt belirteç ile gelir ve "AccessToken" ortam değişkenini belirteç değerine ayarlar. "AccessToken"'ı ayarlayan kodu görmek için **Testler** sekmesine tıklayın. 

    ![AAD belirteci alma](./media/develop-with-postman/postman-get-aad-auth-token.png)

### <a name="create-an-output-asset"></a>Çıktı varlığı oluşturma

Çıktı [Varlığı](https://docs.microsoft.com/rest/api/media/assets), kodlama işinizin sonucunu depolar. 

1. Postman'ın sol penceresinde "Varlıklar"'ı seçin.
2. Ardından "Varlık oluşturma veya güncelleştirme"'yi seçin.
3. **Gönder**’e basın.

    * Aşağıdaki **PUT** işlemi gönderilir:

        ```
        https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/assets/:assetName?api-version={{api-version}}
        ```
    * İşlemin gövdesi aşağıdaki gibidir:

        ```json
        {
        "properties": {
            "description": "My Asset",
            "alternateId" : "some GUID"
         }
        }
        ```

### <a name="create-a-transform"></a>Dönüşüm oluşturma

Media Services’te içerik kodlarken veya işlerken, kodlama ayarlarını bir tarif olarak ayarlamak yaygın bir modeldir. Daha sonra bu tarifi bir videoya uygulamak üzere bir **İş** gönderirsiniz. Her yeni video için yeni İşler göndererek, söz konusu tarifi kitaplığınızdaki tüm videolara uygulamış olursunuz. Media Services içinde tarif, **Dönüşüm** olarak adlandırılır. Daha fazla bilgi için [Dönüşümler ve işler](transform-concept.md) konusuna bakın. Bu öğreticide açıklanan örnek, videoyu çeşitli iOS ve Android cihazlarına akışla aktarmak için kodlayan bir tarifi tanımlar. 

Yeni bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) örneği oluştururken çıktı olarak neyi üretmesi istediğinizi belirtmeniz gerekir. Gerekli parametre bir **TransformOutput** nesnesidir. Her **TransformOutput** bir **Ön ayar** içerir. **Ön ayar**, video ve/veya ses işleme işlemlerinin istenen **TransformOutput** nesnesini oluşturmak üzere kullanılacak adım adım yönergelerini açıklar. Bu makalede açıklanan örnek, **AdaptiveStreaming** adlı yerleşik bir Ön Ayar kullanır. Ön Ayar, giriş çözünürlüğü ve bit hızını temel alarak, giriş videosunu otomatik olarak oluşturulan bir bit hızı basamağına (bit hızı-çözünürlük çiftleri) kodlar ve her bir bit hızı-çözünürlük çiftine karşılık gelen H.264 video ve AAC sesi ile ISO MP4 dosyaları üretir. Bu Ön Ayar hakkında bilgi için bkz. [otomatik oluşturulan bit hızı basamağı](autogen-bitrate-ladder.md).

Yerleşik bir EncoderNamedPreset ön ayarını veya özel ön ayarları kullanabilirsiniz. 

> [!Note]
> Bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) oluştururken, önce **Get** yöntemini kullanarak zaten bir tane olup olmadığını denetlemelisiniz. Bu öğretici, dönüştürmeyi benzersiz bir adla oluşturmakta olduğunuzu varsayar.

1. Postman'in sol penceresinde "Kodlama ve Analiz"'i seçin.
2. Ardından, "Dönüşüm Oluşturma"'yı seçin.
3. **Gönder**’e basın.

    * Aşağıdaki **PUT** işlemi gönderilir.

        ```
        https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/transforms/:transformName?api-version={{api-version}}
        ```
    * İşlemin gövdesi aşağıdaki gibidir:

        ```json
        {
            "properties": {
                "description": "Basic Transform using an Adaptive Streaming encoding preset from the libray of built-in Standard Encoder presets",
                "outputs": [
                    {
                    "onError": "StopProcessingJob",
                "relativePriority": "Normal",
                    "preset": {
                        "@odata.type": "#Microsoft.Media.BuiltInStandardEncoderPreset",
                        "presetName": "AdaptiveStreaming"
                    }
                    }
                ]
            }
        }
        ```

### <a name="create-a-job"></a>Bir iş oluşturma

Burada [İş](https://docs.microsoft.com/rest/api/media/jobs), oluşturulan **Dönüşümü** belirli bir video girdisine veya ses içeriğine uygulamak için Media Services'e gönderilen istektir. **İş** giriş videosunun konumu ve çıktının konumu gibi bilgileri belirtir.

Bu örnekte, işin girdisi bir HTTPS URL'sini ("https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/") temel alır.

1. Postman'in sol penceresinde "Kodlama ve Analiz"'i seçin.
2. Ardından "İş Oluşturma veya Güncelleştirme"'yi seçin.
3. **Gönder**’e basın.

    * Aşağıdaki **PUT** işlemi gönderilir.

        ```
        https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/transforms/:transformName/jobs/:jobName?api-version={{api-version}}
        ```
    * İşlemin gövdesi aşağıdaki gibidir:

        ```json
        {
        "properties": {
            "input": {
            "@odata.type": "#Microsoft.Media.JobInputHttp",
            "baseUri": "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/",
            "files": [
                    "Ignite-short.mp4"
                ]
            },
            "outputs": [
            {
                "@odata.type": "#Microsoft.Media.JobOutputAsset",
                "assetName": "testAsset1"
            }
            ]
        }
        }
        ```

İşin tamamlanması biraz sürüyor ve tamamlandığında bildirim almak istiyorsunuz. İşin ilerleme durumunu görmek için Event Grid'in kullanılmasını öneririz. Event Grid yüksek kullanılabilirlik, tutarlı performans ve dinamik ölçek için tasarlanmıştır. Event Grid ile uygulamalarınız neredeyse tüm Azure hizmetleri ve özel kaynaklardan gelen olayları takip edip bu olaylara yanıt verebilir. Basit, HTTP tabanlı reaktif olay işleme özelliği, olayların akıllı filtrelenmesi ve yönlendirilmesi sayesinde etkili çözümler oluşturmanıza yardımcı olur.  Bkz. [Olayları özel bir web uç noktasına yönlendirme](job-state-events-cli-how-to.md).

**İş** genellik şu aşamalardan geçer: **Zamanlandı**, **Kuyruğa Alındı**, **İşleniyor**, **Tamamlandı** (son aşama). İş bir hatayla karşılaştıysa **Hata** durumunu alırsınız. İş iptal edilme sürecindeyse **İptal Ediliyor** ve **İptal Edildi** durumunu alırsınız.

### <a name="create-a-streaming-locator"></a>Akış bulucusu oluşturma

Kodlama işi tamamlandıktan sonra sıradaki adım, çıktı Varlığındaki videoyu yürütme için istemcilerin kullanımına sunmaktır. Bunu iki adımda gerçekleştirebilirsiniz: ilk olarak, bir [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) oluşturun ve ikinci olarak, istemcilerin kullanabildiği akış URL’lerini derleyin. 

**StreamingLocator** oluşturma işlemine yayımlama denir. Varsayılan olarak **StreamingLocator**, API çağrılarını yapmanızdan hemen sonra geçerli olur ve isteğe bağlı başlangıç ve bitiş süreleri yapılandırmadıkça silinene kadar devam eder. 

Bir [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) oluştururken istenen **StreamingPolicyName** değerini belirtmeniz gerekir. Bu örnekte, temiz (şifrelenmemiş) içeriğin akışını yapacağınız için önceden tanımlı temiz akış ilkesi **PredefinedStreamingPolicy.ClearStreamingOnly** kullanılmaktadır.

> [!IMPORTANT]
> Özel [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies)’yi kullanırken Media Service hesabınız için bu tür ilkelerin sınırlı bir kümesini tasarlamanız ve aynı şifreleme seçenekleri ve protokoller gerekli olduğunda StreamingLocators için bunları kullanmanız gerekir. 

Media Service hesabınızda StreamingPolicy girişlerinin sayısı için bir kota bulunur. Her StreamingLocator için yeni bir StreamingPolicy oluşturmamanız gerekir.

1. Postman'in sol penceresinde "Akış İlkeleri"'ni seçin.
2. Ardından, "Akış Bulucusu Oluşturma"'yı seçin.
3. **Gönder**’e basın.

    * Aşağıdaki **PUT** işlemi gönderilir.

        ```
        https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/streamingPolicies/:streamingPolicyName?api-version={{api-version}}
        ```
    * İşlemin gövdesi aşağıdaki gibidir:

        ```json
        {
            "properties":{
            "assetName": "{{assetName}}",
            "streamingPolicyName": "{{streamingPolicyName}}"
            }
        }
        ```

### <a name="list-paths-and-build-streaming-urls"></a>Yolları listeleme ve akış URL'lerini derleme

#### <a name="list-paths"></a>Yolları listeleme

Artık [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) oluşturulduğuna göre, akış URL'lerini alabilirsiniz

1. Postman'in sol penceresinde "Akış İlkeleri"'ni seçin.
2. Ardından, "Yolları Listele"'yi seçin.
3. **Gönder**’e basın.

    * Aşağıdaki **POST** işlemi gönderilir.

        ```
        https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/streamingLocators/:streamingLocatorName/listPaths?api-version={{api-version}}
        ```
        
    * İşlemin gövdesi yoktur:
        
4. Akış için kullanmak istediğiniz yollardan birini not edin; sonraki bölümde kullanacaksınız. Burada aşağıdaki yollar döndürülür:
    
    ```
    "streamingPaths": [
        {
            "streamingProtocol": "Hls",
            "encryptionScheme": "NoEncryption",
            "paths": [
                "/cdb80234-1d94-42a9-b056-0eefa78e5c63/Ignite-short.ism/manifest(format=m3u8-aapl)"
            ]
        },
        {
            "streamingProtocol": "Dash",
            "encryptionScheme": "NoEncryption",
            "paths": [
                "/cdb80234-1d94-42a9-b056-0eefa78e5c63/Ignite-short.ism/manifest(format=mpd-time-csf)"
            ]
        },
        {
            "streamingProtocol": "SmoothStreaming",
            "encryptionScheme": "NoEncryption",
            "paths": [
                "/cdb80234-1d94-42a9-b056-0eefa78e5c63/Ignite-short.ism/manifest"
            ]
        }
    ]
    ```

#### <a name="build-the-streaming-urls"></a>Akış URL'lerini derleme

Bu bölümde HLS akış URL'sini derleyeceğiz. URL'ler aşağıdaki değerlerden oluşur:

1. Veri gönderme protokolü. Burada "https".

    > [!NOTE]
    > Oynatıcı bir https sitesinde barındırılıyorsa, "https" URL’sini güncelleştirdiğinizden emin olun.

2. StreamingEndpoint'in konak adı. Burada "amsaccount-usw22.streaming.media.azure.net".

    Konak adını almak için aşağıdaki GET işlemini kullanabilirsiniz:
    
    ```
    https://management.azure.com/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/streamingEndpoints/default?api-version={{api-version}}
    ```
    
3. Önceki bölümde (Yolları listeleme) aldığınız yol.  

Sonuç olarak, aşağıdaki HLS URL'si derlenir

```
https://amsaccount-usw22.streaming.media.azure.net/cdb80234-1d94-42a9-b056-0eefa78e5c63/Ignite-short.ism/manifest(format=m3u8-aapl)
```

## <a name="test-the-streaming-url"></a>Akış URL’sini test etme


> [!NOTE]
> Akış yapmak istediğiniz akış uç noktasının çalıştığından emin olun.

Bu makalede, akışı test etmek için Azure Media Player kullanılmaktadır. 

1. Bir web tarayıcısı açın ve [https://aka.ms/azuremediaplayer/](https://aka.ms/azuremediaplayer/) sayfasına gidin.
2. **URL:** kutusuna derlediğiniz URL'yi yapıştırın. 
3. **Oynatıcıyı Güncelleştir** düğmesine basın.

Azure Media Player, test için kullanılabilir, ancak üretim ortamında kullanılmamalıdır. 

## <a name="clean-up-resources-in-your-media-services-account"></a>Media Services hesabınızdaki kaynakları temizleme

Genellikle, yeniden kullanmayı planladığınız nesneler dışında her şeyi temizlemeniz gerekir (genellikle Dönüşümleri yeniden kullanırsınız ve StreamingLocators vb. nesneleri tutarsınız). Deneme sonrasında hesabınızın temiz olmasını istiyorsanız, yeniden kullanmayı planlamadığınız kaynakları silmeniz gerekir.  

Bir kaynağı silmek için, silmek istediğiniz kaynağın altından "Sil ..." işlemini seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz Media Services ve depolama hesapları dahil olmak üzere, kaynak grubunuzdaki kaynaklardan herhangi birine artık ihtiyacınız yoksa kaynak grubunu silebilirsiniz. **CloudShell** aracını kullanabilirsiniz.

**CloudShell**’de aşağıdaki komutu yürütün:

```azurecli-interactive
az group delete --name amsResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Videonuzu karşıya yükleme, kodlama ve akışla aktarma hakkında bilgi edindiğinize göre aşağıdaki makaleye bakabilirsiniz: 

> [!div class="nextstepaction"]
> [Videoları analiz etme](analyze-videos-tutorial-with-api.md)
