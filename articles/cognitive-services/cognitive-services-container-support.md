---
title: Azure Bilişsel hizmetler kapsayıcı desteği
titleSuffix: Azure Cognitive Services
description: Docker kapsayıcıları yakın Bilişsel hizmetler verilerinize nasıl edinebildiğini öğrenin.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.topic: article
ms.date: 12/04/2018
ms.author: diberry
ms.openlocfilehash: c71d7de2ac036fe47253be08bb0b1e01e9e76701
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52957223"
---
# <a name="container-support-in-azure-cognitive-services"></a>Azure Bilişsel hizmetler kapsayıcı desteği

Azure Bilişsel hizmetler kapsayıcı desteği sayesinde geliştiriciler, Azure'da kullanılabilen zengin API'leri kullanmak için ve nerede dağıtmak ve gelir hizmetlerini barındırmak esneklik sağlar [Docker kapsayıcıları](https://www.docker.com/what-container). Kapsayıcı desteği şu anda bir alt kümesini Azure Bilişsel bölümlerini dahil olmak üzere hizmetler için önizleme modunda [görüntü işleme](Computer-vision/Home.md), [yüz](Face/Overview.md), [metin analizi](text-analytics/overview.md)ve [ Language Understanding'i](LUIS/luis-container-howto.md) (LUIS).

Kapsayıcı içinde bir uygulama veya onun bağımlılıklarını & yapılandırması gibi hizmet birlikte bir kapsayıcı görüntüsüne paketlenmiştir yazılım dağıtımı için bir yaklaşımdır. Çok az kayıpla veya hiç değişiklik yapmadan kapsayıcı görüntüsü, bir kapsayıcı konağı üzerinde dağıtılabilir. Birbirine ve bir sanal makine değerinden daha küçük bir kaplama alanı ile temel işletim sistemi, yalıtılmış kapsayıcılardır. Kapsayıcılar için kısa vadeli görevleri kapsayıcı görüntülerinden oluşturulan ve artık gerekli olmadığında kaldırıldı.

Aşağıdaki videoda, Bilişsel hizmetler kapsayıcı kullanmayı gösterir.

[![Bilişsel hizmetler için kapsayıcı Tanıtımı](./media/index/containers-video-image.png)](https://azure.microsoft.com/resources/videos/containers-support-of-cognitive-services)

[Görüntü işleme](Computer-vision/Home.md), [yüz](Face/Overview.md), [metin analizi](text-analytics/overview.md), ve [Language Understanding (LUIS)](LUIS/what-is-luis.md) hizmetleri üzerinde [Microsoft Azure](https://azure.microsoft.com). Oturum [Azure portalında](https://portal.azure.com/) oluşturma ve bu hizmetler için Azure kaynaklarını keşfedin.

## <a name="features-and-benefits"></a>Özellikler ve avantajlar

- **Veriler üzerinde denetim**: Burada bu Bilişsel hizmetler, veri işlem seçme özgürlüğü tanıyın.  Bu, verileri buluta Gönder olamaz, ancak Bilişsel hizmetler teknoloji erişmesi gereken müşteriler için gereklidir. Tutarlılık, veri, yönetim, kimlik ve güvenlik arasında Karma ortamlarda – destekler.
- **Model güncelleştirmelerini denetime**: müşteriler sürüm oluşturma ve kendi çözümlerinde dağıtılan modelleri güncelleştirme konusunda esneklik sağlar.
- **Taşınabilir mimarisi**: Azure'da, şirket içi ve uç dağıtılabilir bir taşınabilir uygulama mimarisi oluşturulmasını etkinleştir. Kapsayıcıları doğrudan dağıtılabilir [Azure Kubernetes hizmeti](/azure/aks/), [Azure Container Instances](/azure/container-instances/), veya bir [Kubernetes](https://kubernetes.io/) kümesi dağıtıldı için [Azure Yığın](/azure/azure-stack/). Daha fazla bilgi için [Azure Stack dağıtma Kubernetes](/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy).
- **Yüksek performans / düşük gecikme süresi**: müşteriler kendi uygulama mantığı ve verileri yakın fiziksel olarak çalıştırmak, Bilişsel hizmetler sağlayarak yüksek aktarım hızına ve düşük gecikme süresi gereksinimleri için ölçekleme olanağı sunar. Kapsayıcılar, saniye başına işlem (TPS) cap değil ve gerekli donanım kaynakları sağlarsanız, isteğe bağlı işlemek için yukarı ve dışarı ölçeklendirme yapılabilir. 


## <a name="containers-in-azure-cognitive-services"></a>Azure Bilişsel hizmetler, kapsayıcılar

Azure Bilişsel hizmetler kapsayıcılar, Docker kapsayıcıları, aşağıdaki dizi her biri, Azure Bilişsel hizmetler hizmetlerden işlevlerinin bir alt kümesini içeren sağlar:

| Hizmet | Kapsayıcı| Açıklama |
|---------|----------|-------------|
|[Görüntü İşleme](Computer-vision/computer-vision-how-to-install-containers.md) |**Metin tanıma** |Farklı yüzey ve arka planlar, giriş ve posterler kartvizitler gibi çeşitli nesne görüntülerdeki yazdırılan metin ayıklar.<br/><br/>**Önemli:** metni tanı kapsayıcı şu anda yalnızca İngilizce ile çalışır.<br>[Erişim isteği](Computer-vision/computer-vision-how-to-install-containers.md#request-access-to-the-private-container-registry)|
|[Yüz tanıma](Face/face-how-to-install-containers.md) |**Yüz tanıma** |Görüntülerdeki İnsan yüzlerini algılar ve yüz yer işareti (örneğin, noses ve gözler), cinsiyet, geçerlilik süresi ve diğer makine tahmin yüz özellikleri dahil olmak üzere, öznitelikleri tanımlar. Yüz algılama ek olarak, iki yüzün aynı görüntü ya da farklı görüntüleri bir güven puanı kullanarak aynı olduğundan veya bir benzeyen olmadığını görmek için bir veritabanında yüzleri karşılaştırın veya aynı yüz zaten kontrol edebilirsiniz. Bu gibi durumlarda, benzer yüzlerden de paylaşılan visual nitelikler kullanarak gruplar halinde düzenleyebilirsiniz.<br>[Erişim isteği](Face/face-how-to-install-containers.md#request-access-to-the-private-container-registry) |
|[LUIS](LUIS/luis-container-howto.md) |**LUIS** ([görüntü](https://go.microsoft.com/fwlink/?linkid=2043204))|Bir eğitilen veya yayımlanmış dil anlama modeli olarak da bilinen bir LUIS uygulaması bir docker kapsayıcısına yükler ve kapsayıcının API uç noktalardan gelen sorgu tahminler elde etmek için erişim sağlar. Kapsayıcıdan sorgu günlüklerini toplamak ve bu geri yükleme [LUIS portalı](https://www.luis.ai) uygulamanın tahmin doğruluğunu artırmak için.|
|[Metin Analizi](text-analytics/how-tos/text-analytics-how-to-install-containers.md) |**Anahtar ifade ayıklama** ([görüntü](https://go.microsoft.com/fwlink/?linkid=2018757)) |Ana noktaları belirleyin, anahtar ifadeleri ayıklar. Örneğin, "The food was delicious and there were wonderful staff" (Yemekler lezzetliydi ve personel harikaydı) giriş metni olduğunda API, "food" (yemek) ve "wonderful staff" (personel harikaydı) ana konuşma noktalarını döndürür. |
|[Metin Analizi](text-analytics/how-tos/text-analytics-how-to-install-containers.md)|**Dil algılama** ([görüntü](https://go.microsoft.com/fwlink/?linkid=2018759)) |En fazla 120 dil için hangi dil giriş metni yazılır ve rapor istekte gönderilen her belge için bir tek dil kodu algılar. Dil kodu, puanın ağırlığını belirten bir puanla eşleştirilir. |
|[Metin Analizi](text-analytics/how-tos/text-analytics-how-to-install-containers.md)|**Yaklaşım analizi** ([görüntü](https://go.microsoft.com/fwlink/?linkid=2018654)) |Ham metin pozitif veya negatif yaklaşım hakkında ipuçları için analiz eder. API, her belge için 0 ile 1 arasında bir yaklaşım puanı döndürür ve 1 en pozitif değerdir. Analiz modelleri metin ve doğal dil Microsoft teknolojilerinin kapsamlı bir gövdesi kullanarak önceden eğitilir. API, [seçili dillerde](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages.md) sağladığınız ham metni analiz edip puanlayabilir ve sonuçları doğrudan çağrıyı yapan uygulamaya döndürebilir. |

## <a name="container-availability-in-azure-cognitive-services"></a>Azure Bilişsel hizmetler kapsayıcı kullanılabilirlik

Azure aboneliğiniz üzerinden genel kullanıma açık Azure Bilişsel hizmetler kapsayıcıları ve Docker kapsayıcı görüntülerini Microsoft kapsayıcı kayıt defteri veya Docker Hub çekilebilir. Kullanabileceğiniz [docker isteği](https://docs.docker.com/engine/reference/commandline/pull/) uygun kayıt defterinden bir kapsayıcı görüntüsü indirilemedi komutu.

> [!IMPORTANT]
> Şu anda erişmek için bir kaydolma işlemini tamamlamanız gereken [yüz](Face/face-how-to-install-containers.md) ve [metni tanı](Computer-vision/computer-vision-how-to-install-containers.md) kapsayıcılar, doldurun ve sorularınız varsa, şirketinizin ve kullanım örneği hakkında bir soru gönderin, kapsayıcıları uygulamak istediğiniz. Erişim izni ve da sağlanan kimlik bilgileri sonra Azure Container Registry tarafından barındırılan bir özel kapsayıcı kayıt defterinden tipini ve metin tanıma kapsayıcılar için kapsayıcı görüntülerini çeker.

## <a name="prerequisites"></a>Önkoşullar

Azure Bilişsel hizmetler kapsayıcıları kullanmadan önce aşağıdaki önkoşulları karşılamanız gerekir:

**Docker altyapısı**: Docker altyapısının yerel olarak yüklü olması gerekir. Docker üzerinde Docker ortamını yapılandıran paketler sağlar [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms), ve [Windows](https://docs.docker.com/docker-for-windows/). Windows Docker Linux kapsayıcıları destekleyecek şekilde yapılandırılması gerekir. Docker kapsayıcıları da dağıtılabilir doğrudan [Azure Kubernetes hizmeti](/azure/aks/) veya [Azure Container Instances](/azure/container-instances/).

Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır.

**Microsoft Container Registry ve Docker konusunda**: bir temel kavramlarını hem Microsoft Container Registry ve Docker kayıt defterleri, depoları, kapsayıcılar ve kapsayıcı görüntülerinin yanı sıra bilgisi gibi olmalıdır temel `docker` komutları.  

Docker ve kapsayıcı temelleri hakkında bilgi için bkz: [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/).

Kapsayıcılara sunucu ve bellek ayırma gereksinimleri dahil olmak üzere kendi gereksinimleriyle de sahip olabilir.

## <a name="developer-samples"></a>Geliştirici örnekleri

Geliştirici Örnekleri kullanıma sunduğumuz [Github deposu](https://github.com/Azure-Samples/cognitive-services-containers-samples). 

## <a name="next-steps"></a>Sonraki adımlar

Yükleme ve Azure Bilişsel hizmetler, kapsayıcılar tarafından sağlanan işlevselliği keşfedin:

* [Yükleme ve görüntü işleme kapsayıcıları kullanma](Computer-vision/computer-vision-how-to-install-containers.md)
* [Yükleme ve yüz kapsayıcıları kullanma](Face/face-how-to-install-containers.md)
* [Yükleme ve metin analizi kapsayıcıları kullanma](text-analytics/how-tos/text-analytics-how-to-install-containers.md)
* [Yükleme ve Language Understanding (LUIS) kapsayıcıları kullanma](LUIS/luis-container-howto.md)
