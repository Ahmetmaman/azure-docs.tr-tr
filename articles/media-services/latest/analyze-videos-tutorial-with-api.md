---
title: Azure Media Services ile videoları analiz etme | Microsoft Docs
description: Azure Media Services kullanarak videoları analiz etmek için bu öğreticideki adımları izleyin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: tutorial
ms.custom: mvc
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: 96ed5c17501aeb195ee584ce6d41a84fc2118bac
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49376661"
---
# <a name="tutorial-analyze-videos-with-azure-media-services"></a>Öğretici: Azure Media Services ile videoları analiz etme 

Bu öğreticide Azure Media Services ile videoları analiz etme işlemi gösterilir. Kayıtlı videolar veya ses içerikleri hakkında derin içgörüler kazanmak isteyebileceğiniz çok sayıda senaryo mevcuttur. Örneğin, daha yüksek müşteri memnuniyeti elde etmek isteyen kuruluşlar, müşteri destek kayıtlarını dizinler ve panolarla aranabilir bir katalog haline getirmek için konuşmayı metne dönüştürme işlemini çalıştırabilir. Daha sonra, şirketleri hakkında yaygın şikayetlerin listesi, bu şikayetlerin kaynakları gibi içgörüler elde edebilir.

Bu öğretici şunların nasıl yapıldığını gösterir:    

> [!div class="checklist"]
> * Media Services hesabı oluşturma
> * Media Services API’sine erişim
> * Örnek uygulamayı yapılandırma
> * Belirtilen videoyu analiz eden kodu inceleme
> * Uygulamayı çalıştırma
> * Çıktıyı inceleme
> * Kaynakları temizleme

> [!Note]
> Media Services hesabınızı 10 S3 Medya Ayrılmış Birimleri olarak ayarlamak için [Medya işlemeyi ölçeklendirme](../previous/media-services-scale-media-processing-overview.md) bölümünde anlatılan şekilde Azure portalı kullanın.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Visual Studio yüklü değilse, [Visual Studio Community 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15)’yi edinebilirsiniz.

## <a name="download-the-sample"></a>Örneği indirme

Aşağıdaki komutu kullanarak, .NET örneğini içeren bir GitHub havuzunu makinenize kopyalayın:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```

Örnek, [AnalyzeVideos](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/AnalyzeVideos) klasöründe yer alır.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="examine-the-code-that-analyzes-the-specified-video"></a>Belirtilen videoyu analiz eden kodu inceleme

Bu bölümde, *AnalyzeVideos* projesinin [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/AnalyzeVideos/Program.cs) dosyasında tanımlı işlevler incelenmektedir.

Örnek aşağıdaki eylemleri gerçekleştirir:

1. Bir dönüşüm ve videonuzu analiz eden bir iş oluşturur.
2. Bir giriş varlığı oluşturur ve videoyu varlığın içine yükler. Varlık, işin girişi olarak kullanılır.
3. İşin çıkışını depolayan bir çıkış varlığı oluşturur. 
4. İşi gönderir.
5. İşin durumunu denetler.
6. İşin çalıştırılması sonucu oluşan dosyaları indirir. 

### <a name="start-using-media-services-apis-with-net-sdk"></a>.NET SDK ile Media Services API’sini kullanmaya başlama

.NET ile Media Services API’lerini kullanmaya başlamak için bir **AzureMediaServicesClient** nesnesi oluşturmanız gerekir. Nesneyi oluşturmak için, Azure AD kullanarak Azure’a bağlanmak üzere istemcinin ihtiyaç duyduğu kimlik bilgilerini sağlamanız gerekir. Makalenin başlangıcında kopyaladığınız koddaki **GetCredentialsAsync** işlevi, yerel yapılandırma dosyasında sağlanan kimlik bilgilerini temel alan ServiceClientCredentials nesnesini oluşturur. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CreateMediaServicesClient)]

### <a name="create-an-input-asset-and-upload-a-local-file-into-it"></a>Bir giriş varlığı oluşturma ve içine yerel dosya yükleme 

**CreateInputAsset** işlevi yeni bir giriş [Varlığı](https://docs.microsoft.com/rest/api/media/assets) oluşturur ve içine belirtilen yerel video dosyasını yükler. Bu Varlık, kodlama işinizde giriş olarak kullanılır. Media Services v3’te bir İşe yapılan giriş, Varlık olabilir veya HTTPS URL’leri üzerinden Media Services hesabınızın kullanımına açtığınız bir içerik olabilir. Bir HTTPS URL’sinden kodlama yapmayı öğrenmek için [bu](job-input-from-http-how-to.md) makaleye bakın.  

Media Services v3’te dosyaları karşıya yüklemek için Azure Depolama API’lerini kullanırsınız. Aşağıdaki .NET kod parçacığı bunun nasıl yapıldığını gösterir.

Aşağıdaki işlev şu eylemleri gerçekleştirir:

* Varlık oluşturur 
* Varlığın [depolamadaki kapsayıcısına](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet?tabs=windows#upload-blobs-to-the-container) yazılabilir bir [SAS URL](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)’si alır
* SAS URL’sini kullanarak dosyayı depolamadaki kapsayıcıya yükler

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CreateInputAsset)]

### <a name="create-an-output-asset-to-store-the-result-of-the-job"></a>İşin sonucunu depolamak için çıkış varlığı oluşturma 

[Çıktı](https://docs.microsoft.com/rest/api/media/assets) varlığı, işinizin sonucunu depolar. Proje, bu çıktı varlığının sonuçlarını "output" klasörüne indiren **DownloadResults** işlevini tanımlar, böylece elinizde neyin olduğunu görebilirsiniz.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CreateOutputAsset)]

### <a name="create-a-transform-and-a-job-that-analyzes-videos"></a>Bir dönüşüm ve videoları analiz eden bir iş oluşturma

Media Services’te içerik kodlarken veya işlerken, kodlama ayarlarını bir tarif olarak ayarlamak yaygın bir modeldir. Daha sonra bu tarifi bir videoya uygulamak üzere bir **İş** gönderirsiniz. Her yeni video için yeni İşler göndererek, söz konusu tarifi kitaplığınızdaki tüm videolara uygulamış olursunuz. Media Services içinde tarif, **Dönüşüm** olarak adlandırılır. Daha fazla bilgi için [Dönüşümler ve işler](transform-concept.md) konusuna bakın. Bu öğreticide açıklanan örnek, belirtilen videoyu analiz eden bir tarifi tanımlar. 

#### <a name="transform"></a>Dönüşüm

Yeni bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) örneği oluştururken çıktı olarak neyi üretmesi istediğinizi belirtmeniz gerekir. Gerekli parametre, yukarıdaki kodda gösterildiği gibi bir **TransformOutput** nesnesidir. Her **TransformOutput** bir **Ön ayar** içerir. **Ön ayar**, video ve/veya ses işleme işlemlerinin istenen **TransformOutput** nesnesini oluşturmak üzere kullanılacak adım adım yönergelerini açıklar. Bu örnekte **VideoAnalyzerPreset** ön ayarı kullanılır ve dil ("en-US"), oluşturucusuna geçirilir. Bu ön ayar, bir videodan birden fazla ses ve video içgörüsü elde etmenizi sağlar. Bir videodan birden fazla ses içgörüsü elde etmeniz gerekiyorsa **AudioAnalyzerPreset** ön ayarını kullanın. 

Bir **Dönüşüm** oluştururken ilk olarak aşağıdaki kodda gösterildiği gibi **Get** yöntemi ile bir dönüşümün zaten var olup olmadığını denetlemeniz gerekir.  Media Services v3’te varlıklar üzerindeki **Get** yöntemleri, varlığın mevcut olmaması durumunda **null** değerini döndürür (büyük/küçük harfe duyarlı ad denetimi).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#EnsureTransformExists)]

#### <a name="job"></a>İş

Yukarıda bahsedildiği gibi [Transform](https://docs.microsoft.com/rest/api/media/transforms) nesnesi tarif, [Job](https://docs.microsoft.com/rest/api/media/jobs) ise bu **Transform** nesnesini belirli bir giriş videosu veya ses içeriğine uygulamak için Media Services’e gönderilen gerçek istektir. **İş** giriş videosunun konumu ve çıktının konumu gibi bilgileri belirtir. Videonuzun konumunu, Media Services hesabınızda bulunan HTTPS URL’lerini, SAS URL’lerini veya Varlıkları kullanarak belirtebilirsiniz. 

Bu örnekte, iş girdisi yerel bir videodur.  

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#SubmitJob)]

### <a name="wait-for-the-job-to-complete"></a>İşin tamamlanmasını bekleyin

İşin tamamlanması biraz sürüyor ve tamamlandığında bildirim almak istiyorsunuz. [İşin](https://docs.microsoft.com/rest/api/media/jobs) tamamlanması hakkında bildirim almaya ilişkin farklı seçenekler mevcuttur. En basit seçenek (burada gösterilir), yoklama kullanmaktır. 

Yoklama, olası gecikme süresi nedeniyle üretim uygulamaları için önerilen en iyi uygulamalardan biri değildir. Yoklama, bir hesap üzerinde gereğinden fazla kullanılırsa kısıtlanabilir. Geliştiricilerin onun yerine Event Grid kullanmalıdır.

Event Grid yüksek kullanılabilirlik, tutarlı performans ve dinamik ölçek için tasarlanmıştır. Event Grid ile uygulamalarınız neredeyse tüm Azure hizmetleri ve özel kaynaklardan gelen olayları takip edip bu olaylara yanıt verebilir. Basit, HTTP tabanlı reaktif olay işleme özelliği, olayların akıllı filtrelenmesi ve yönlendirilmesi sayesinde etkili çözümler oluşturmanıza yardımcı olur. Bkz. [Olayları özel bir web uç noktasına yönlendirme](job-state-events-cli-how-to.md).

**İş** genellik şu aşamalardan geçer: **Zamanlandı**, **Kuyruğa Alındı**, **İşleniyor**, **Tamamlandı** (son aşama). İş bir hatayla karşılaştıysa **Hata** durumunu alırsınız. İş iptal edilme sürecindeyse **İptal Ediliyor** ve **İptal Edildi** durumunu alırsınız.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#WaitForJobToFinish)]

### <a name="download-the-result-of-the-job"></a>İşin sonucunu indirme

Aşağıdaki işlev, çıktı [Varlığındaki](https://docs.microsoft.com/rest/api/media/assets) sonuçları "output" klasörüne indirir, böylece işin sonuçlarını inceleyebilirsiniz. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#DownloadResults)]

### <a name="clean-up-resource-in-your-media-services-account"></a>Media Services hesabınızdaki kaynakları temizleme

Genellikle, yeniden kullanmayı planladığınız nesneler dışında her şeyi temizlemeniz gerekir (genellikle Dönüşümleri yeniden kullanırsınız ve StreamingLocators’ı tutarsınız). Deneme sonrasında hesabınızın temiz olmasını istiyorsanız, yeniden kullanmayı planlamadığınız kaynakları silmeniz gerekir. Örneğin, aşağıdaki kod İşleri siler.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CleanUp)]

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

*AnalyzeVideos* uygulamasını çalıştırmak için Ctrl+F5’e basın.

Programı çalıştırdığınızda iş, videoda bulduğu her yüz için küçük resimler üretir. Ayrıca, insights.json dosyasını oluşturur.

## <a name="examine-the-output"></a>Çıktıyı inceleme

Analiz edilen videoların çıktı dosyası insights.json olarak adlandırılır. Bu dosya, videonuz hakkında içgörüler içerir. Json dosyasındaki öğelerin açıklamalarını [Medya zekası](intelligence-concept.md) makalesinde bulabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz Media Services ve depolama hesapları dahil olmak üzere, kaynak grubunuzdaki kaynaklardan herhangi birine artık ihtiyacınız yoksa kaynak grubunu silebilirsiniz. **CloudShell** aracını kullanabilirsiniz.

**CloudShell**’de aşağıdaki komutu yürütün:

```azurecli-interactive
az group delete --name amsResourceGroup
```

## <a name="multithreading"></a>Çoklu iş parçacığı kullanımı

Azure Media Services v3 SDK’ları, iş parçacığı güvenli değildir. Çok iş parçacıklı uygulama ile çalışırken, iş parçacığı başına yeni bir AzureMediaServicesClient nesnesi oluşturmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: dosyaları karşıya yükleme, kodlama ve akışa alma](stream-files-tutorial-with-api.md)
