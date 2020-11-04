---
title: 'ML Studio (klasik): klasik Web hizmetini yeniden eğitme-Azure'
description: Bir modeli yeniden eğitme ve klasik Web hizmetini Azure Machine Learning Studio (klasik) ' de yeni eğitilen modeli kullanacak şekilde güncelleştirme hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: how-to
author: peterclu
ms.author: peterlu
ms.custom: seodec18, previous-ms.author=yahajiza, previous-author=YasinMSFT, devx-track-csharp
ms.date: 02/14/2019
ms.openlocfilehash: 158541d34568b7ea02ea82dbfe90f5801824716f
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93325783"
---
# <a name="retrain-and-deploy-a-classic-studio-classic-web-service"></a>Klasik bir Studio (klasik) Web hizmetini yeniden eğitme ve dağıtma

**Uygulama hedefi:** ![ Yeşil onay işareti. ](../../../includes/media/aml-applies-to-skus/yes.png) Machine Learning Studio (klasik) ![ X olmadığını belirten. ](../../../includes/media/aml-applies-to-skus/no.png)[Azure Machine Learning](../overview-what-is-machine-learning-studio.md#ml-studio-classic-vs-azure-machine-learning-studio)  


Makine öğrenimi modellerini yeniden eğitmek, uygun olan en uygun verilere göre ve bunların doğru kalmasını sağlamanın bir yoludur. Bu makalede, klasik bir Studio (klasik) Web hizmetini yeniden eğitme konusu gösterilmektedir. Yeni bir Studio (klasik) Web hizmetini yeniden eğitme hakkında bir kılavuz için, [Bu nasıl yapılır makalesini görüntüleyin.](retrain-machine-learning-model.md)

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, hem yeniden eğitim denemenize hem de tahmine dayalı denemenize sahip olduğunuz varsayılır. Bu adımlar, [makine öğrenimi modelini yeniden eğitme ve dağıtma](./retrain-machine-learning-model.md) bölümünde açıklanmaktadır. Ancak, makine öğrenimi modelinizi yeni bir Web hizmeti olarak dağıtmak yerine, tahmine dayalı denemenizi klasik bir Web hizmeti olarak dağıtacaksınız.
     
## <a name="add-a-new-endpoint"></a>Yeni uç nokta Ekle

Dağıttığınız tahmine dayalı Web hizmeti, özgün eğitim ve Puanlama denemeleri eğitilen modeliyle eşitlenmiş olarak tutulan bir varsayılan Puanlama uç noktası içerir. Web hizmetinizi yeni eğitilen bir modelle güncelleştirmek için yeni bir Puanlama uç noktası oluşturmanız gerekir.

Web hizmetine yeni bir uç nokta eklemenin iki yolu vardır:

* Programlı olarak
* Azure Web Hizmetleri portalını kullanma

> [!NOTE]
> Eğitim Web hizmetine değil, tahmine dayalı Web hizmetine uç noktayı eklediğinizden emin olun. Hem eğitim hem de tahmine dayalı bir Web hizmetini doğru bir şekilde dağıttıysanız, listelenen iki ayrı Web hizmeti görmeniz gerekir. Tahmine dayalı Web hizmeti "[Predictive exp.]" ile bitmelidir.
>

### <a name="programmatically-add-an-endpoint"></a>Program aracılığıyla bir uç nokta ekleme

Bu [GitHub deposunda](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint)sunulan örnek kodu kullanarak Puanlama uç noktaları ekleyebilirsiniz.

### <a name="use-the-azure-web-services-portal-to-add-an-endpoint"></a>Uç nokta eklemek için Azure Web Hizmetleri portalını kullanma

1. Machine Learning Studio (klasik) ' de, sol gezinti sütununda Web Hizmetleri ' ne tıklayın.
1. Web hizmeti panosunun en altında, **uç nokta önizlemeyi Yönet** ' e tıklayın.
1. **Ekle** 'ye tıklayın.
1. Yeni uç nokta için bir ad ve açıklama yazın. Günlüğe kaydetme düzeyini ve örnek verilerin etkinleştirilip etkinleştirilmeyeceğini seçin. Günlüğe kaydetme hakkında daha fazla bilgi için bkz. [Machine Learning Web Hizmetleri için günlüğü etkinleştirme](web-services-logging.md).

## <a name="update-the-added-endpoints-trained-model"></a>Eklenen uç noktanın eğitilen modelini Güncelleştir

### <a name="retrieve-patch-url"></a>YAMA URL 'sini al

Web portalını kullanarak doğru düzeltme eki URL 'sini almak için aşağıdaki adımları izleyin:

1. [Azure Machine Learning Web Hizmetleri](https://services.azureml.net/) portalında oturum açın.
1. En üstteki **Web Hizmetleri** veya **Klasik Web Hizmetleri** ' ne tıklayın.
1. Üzerinde çalıştığınız Puanlama Web hizmeti ' ne tıklayın (Web hizmetinin varsayılan adını değiştirmediyseniz, "[Puanlama exp.]" içinde sona acaktır).
1. **+ Yeni** seçeneğine tıklayın.
1. Uç nokta eklendikten sonra uç nokta adına tıklayın.
1. **Düzeltme Eki** URL 'Si altında **API yardımı** ' na tıklayarak düzeltme eki uygulama yardım sayfasını açın.

> [!NOTE]
> Bitiş noktasını, tahmine dayalı Web hizmeti yerine eğitim Web hizmetine eklediyseniz, **Kaynağı Güncelleştir** bağlantısına tıkladığınızda şu hatayı alırsınız: "Üzgünüm, ancak bu özellik desteklenmiyor veya bu bağlamda kullanılamıyor. Bu Web hizmetinde güncelleştirilebilir kaynak yok. Bu sorundan dolayı özür dileriz ve bu iş akışını iyileştirmek için çalışıyoruz. "
>

YAMA yardım sayfası, kullanmanız gereken düzeltme eki URL 'sini içerir ve bunu çağırmak için kullanabileceğiniz örnek kodu sağlar.

![Düzeltme Eki URL 'SI.](./media/retrain-classic/ml-help-page-patch-url.png)

### <a name="update-the-endpoint"></a>Uç noktayı Güncelleştir

Artık, daha önce oluşturduğunuz Puanlama bitiş noktasını güncelleştirmek için eğitilen modeli kullanabilirsiniz.

Aşağıdaki örnek kod, uç noktayı güncelleştirmek için *Baselocation* , *relativelocation* , *SASBLOBTOKEN* ve Patch URL 'sini nasıl kullanacağınızı gösterir.

```csharp
private async Task OverwriteModel()
{
    var resourceLocations = new
    {
        Resources = new[]
        {
            new
            {
                Name = "Census Model [trained model]",
                Location = new AzureBlobDataReference()
                {
                    BaseLocation = "https://esintussouthsus.blob.core.windows.net/",
                    RelativeLocation = "your endpoint relative location", //from the output, for example: "experimentoutput/8946abfd-79d6-4438-89a9-3e5d109183/8946abfd-79d6-4438-89a9-3e5d109183.ilearner"
                    SasBlobToken = "your endpoint SAS blob token" //from the output, for example: "?sv=2013-08-15&sr=c&sig=37lTTfngRwxCcf94%3D&st=2015-01-30T22%3A53%3A06Z&se=2015-01-31T22%3A58%3A06Z&sp=rl"
                }
            }
        }
    };

    using (var client = new HttpClient())
    {
        client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", apiKey);

        using (var request = new HttpRequestMessage(new HttpMethod("PATCH"), endpointUrl))
        {
            request.Content = new StringContent(JsonConvert.SerializeObject(resourceLocations), System.Text.Encoding.UTF8, "application/json");
            HttpResponseMessage response = await client.SendAsync(request);

            if (!response.IsSuccessStatusCode)
            {
                await WriteFailedResponse(response);
            }

            // Do what you want with a successful response here.
        }
    }
}
```

Çağrı için *Apikey* ve *endpointUrl* , Endpoint panosundan elde edilebilir.

*Kaynaklardaki* *ad* parametresinin değeri, tahmine dayalı deneyde kaydedilen eğitilen modelin kaynak adıyla eşleşmelidir. Kaynak adını almak için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol menüden **Machine Learning** ' ye tıklayın.
1. Ad ' ın altında, çalışma alanınıza ve ardından **Web Hizmetleri** ' ne tıklayın.
1. Ad ' ın altında, **Census modeli [tahmine dayalı exp.]** öğesine tıklayın.
1. Eklediğiniz yeni uç noktaya tıklayın.
1. Uç nokta panosunda **Kaynağı Güncelleştir** ' e tıklayın.
1. Web hizmetinin kaynak API belgelerini Güncelleştir sayfasında, **kaynak adını** **güncelleştirilebilir kaynaklar** altında bulabilirsiniz.

Uç noktayı güncelleştirmeden önce SAS belirtecinizin süresi dolarsa, yeni bir belirteç edinmek için Iş KIMLIĞIYLE birlikte bir alma işlemi gerçekleştirmeniz gerekir.

Kod başarıyla çalıştırıldığında, yeni uç noktanın yaklaşık 30 saniye içinde geri çekme modelini kullanmaya başlaması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Web hizmetlerini yönetme veya birden çok denemeleri çalıştırmasını izleme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Web Hizmetleri portalını keşfet](manage-new-webservice.md)
* [Deneme yinelemelerini yönetme](manage-experiment-iterations.md)