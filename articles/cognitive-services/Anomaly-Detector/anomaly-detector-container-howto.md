---
title: Anomali algılayıcı API 'SI için Docker Kapsayıcıları yükleyip çalıştırın
titleSuffix: Azure Cognitive Services
description: Azure 'da bir Docker kapsayıcısı kullanarak verilerinize ilişkin anormallikleri bulmak için anomali algılayıcı API 'sinin algoritmalarını kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: conceptual
ms.date: 09/28/2020
ms.author: aahi
ms.custom: cog-serv-seo-aug-2020
keywords: Şirket içi, Docker, kapsayıcı, akış, algoritmalar
ms.openlocfilehash: ff4d15b33cb261e71ea883c0245afe5781005e38
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91460009"
---
# <a name="install-and-run-docker-containers-for-the-anomaly-detector-api"></a>Anomali algılayıcı API 'SI için Docker Kapsayıcıları yükleyip çalıştırın 

[!INCLUDE [container image location note](../containers/includes/image-location-note.md)]

Kapsayıcılar, kendi ortamınızı anomali algılayıcı API 'sini kullanmanıza olanak sağlar. Kapsayıcılar, belirli güvenlik ve veri idare gereksinimleri için çok kullanışlıdır. Bu makalede anomali algılayıcı kapsayıcısını indirme, yükleme ve çalıştırma hakkında bilgi edineceksiniz.

Anomali algılayıcısı, şirket içi API 'yi kullanmak için tek bir Docker kapsayıcısı sunar. Kapsayıcıyı şu şekilde kullanın:
* Verileriniz üzerinde anomali algılayıcısının algoritmalarını kullanın
* Akış verilerini izleyin ve gerçek zamanlı olarak gerçekleştikleri gibi bozukluklar tespit edin.
* Veri kümesinin tamamında bir toplu iş olarak, anormallikleri tespit edin. 
* Veri kümesindeki bir toplu iş olarak eğilim değişiklik noktalarını tespit edin.
* Anomali algılama algoritmasının duyarlılığını verilerinize daha iyi uyacak şekilde ayarlayın.

API hakkında ayrıntılı bilgi için lütfen bkz:
* [Anomali algılayıcı API hizmeti hakkında daha fazla bilgi edinin](https://go.microsoft.com/fwlink/?linkid=2080698&clcid=0x409)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/cognitive-services/) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Anomali algılayıcı kapsayıcılarını kullanmadan önce aşağıdaki önkoşulları karşılamanız gerekir:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Bir [ana bilgisayarda](#the-host-computer)Docker altyapısının yüklü olması gerekir. Docker, [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms) üzerinde Docker ortamını yapılandıran paketler sağlar. Docker ve kapsayıcı temel bilgileri ile ilgili giriş yapmak için [Docker’a genel bakış](https://docs.docker.com/engine/docker-overview/) bölümüne bakın.<br><br> Kapsayıcıların Azure 'a bağlanıp faturalandırma verilerini göndermesini sağlamak için Docker yapılandırılmalıdır. <br><br> **Windows 'da**Docker 'ın de Linux kapsayıcılarını destekleyecek şekilde yapılandırılması gerekir.<br><br>|
|Docker ile benzerlik | Kayıt defterleri, depolar, kapsayıcılar ve kapsayıcı görüntüleri gibi Docker kavramlarının yanı sıra temel komutlar hakkında bilgi sahibi olmanız gerekir `docker` .|
|Anomali algılayıcı kaynağı |Bu kapsayıcıları kullanabilmeniz için, şunları yapmanız gerekir:<br><br>İlişkili API anahtarını ve uç nokta URI 'sini almak için bir Azure _anomali algılayıcısı_ kaynağı. Her iki değer de Azure portal **anomali algılayıcısının** genel bakış ve anahtarlar sayfalarında bulunur ve kapsayıcıyı başlatmak için gereklidir.<br><br>**{API_KEY}**: **anahtarlar** sayfasında kullanılabilir iki kaynak anahtardan biri<br><br>**{ENDPOINT_URI}**: **genel bakış** sayfasında belirtilen bitiş noktası|

[!INCLUDE [Gathering required container parameters](../containers/includes/container-gathering-required-parameters.md)]

## <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

<!--* [Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/). For instructions of deploying Anomaly Detector module in IoT Edge, see [How to deploy Anomaly Detector module in IoT Edge](how-to-deploy-anomaly-detector-module-in-iot-edge.md).-->

### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

Aşağıdaki tabloda, anomali algılayıcı kapsayıcısı için ayrılacak minimum ve önerilen CPU çekirdekleri ve bellek açıklanmaktadır.

| QPS (saniye başına sorgu) | Minimum | Önerilen |
|-----------|---------|-------------|
| 10 QPS | 4 çekirdek, 1 GB bellek | 8 çekirdekli 2 GB bellek |
| 20 QPS | 8 çekirdek, 2 GB bellek | 16 çekirdek 4 GB bellek |

Her çekirdek en az 2,6 gigahertz (GHz) veya daha hızlı olmalıdır.

Çekirdek ve bellek, `--cpus` `--memory` komutunun bir parçası olarak kullanılan ve ayarlarına karşılık gelir `docker run` .

## <a name="get-the-container-image-with-docker-pull"></a>Kapsayıcı görüntüsünü al `docker pull`

[`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/)Bir kapsayıcı görüntüsünü indirmek için komutunu kullanın.

| Kapsayıcı | Depo |
|-----------|------------|
| bilişsel hizmetler-anomali-algılayıcı | `mcr.microsoft.com/azure-cognitive-services/decision/anomaly-detector:latest` |

<!--
For a full description of available tags, such as `latest` used in the preceding command, see [anomaly-detector](https://go.microsoft.com/fwlink/?linkid=2083827&clcid=0x409) on Docker Hub.
-->
[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-anomaly-detector-container"></a>Anomali algılayıcı kapsayıcısı için Docker Pull

```Docker
docker pull mcr.microsoft.com/azure-cognitive-services/anomaly-detector:latest
```

## <a name="how-to-use-the-container"></a>Kapsayıcıyı kullanma

Kapsayıcı [ana bilgisayardan](#the-host-computer)olduktan sonra, kapsayıcında çalışmak için aşağıdaki işlemi kullanın.

1. [Kapsayıcıyı](#run-the-container-with-docker-run)gerekli faturalandırma ayarlarıyla çalıştırın. Komuta [examples](anomaly-detector-container-configuration.md#example-docker-run-commands) daha fazla örnek `docker run` kullanılabilir.
1. [Kapsayıcının tahmin uç noktasını sorgulayın](#query-the-containers-prediction-endpoint).

## <a name="run-the-container-with-docker-run"></a>Kapsayıcıyı ile çalıştırma `docker run`

Kapsayıcıyı çalıştırmak için [Docker Run](https://docs.docker.com/engine/reference/commandline/run/) komutunu kullanın. Ve değerlerini alma hakkında ayrıntılar için [gerekli parametreleri toplama](#gathering-required-parameters) bölümüne bakın `{ENDPOINT_URI}` `{API_KEY}` .

Komut [örnekleri](anomaly-detector-container-configuration.md#example-docker-run-commands) `docker run` mevcuttur.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/decision/anomaly-detector:latest \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Şu komut:

* Kapsayıcı görüntüsünden bir anomali algılayıcı kapsayıcısı çalıştırır
* Bir CPU çekirdeği ve 4 gigabayt (GB) bellek ayırır
* TCP bağlantı noktası 5000 ' i kullanıma sunar ve kapsayıcı için bir sözde TTY ayırır
* Kapsayıcıyı çıktıktan sonra otomatik olarak kaldırır. Kapsayıcı görüntüsü hala ana bilgisayarda kullanılabilir.

> [!IMPORTANT]
> `Eula` `Billing` `ApiKey` Kapsayıcıyı çalıştırmak için, ve seçenekleri belirtilmelidir; Aksi takdirde kapsayıcı başlatılmaz.  Daha fazla bilgi için bkz. [faturalandırma](#billing).

### <a name="running-multiple-containers-on-the-same-host"></a>Aynı konakta birden çok kapsayıcı çalıştırma

Birden çok kapsayıcıyı açığa çıkarılan bağlantı noktalarıyla çalıştırmayı düşünüyorsanız, her kapsayıcıyı farklı bir bağlantı noktasıyla çalıştırdığınızdan emin olun. Örneğin, bağlantı noktası 5000 ' deki ilk kapsayıcıyı ve bağlantı noktası 5001 üzerindeki ikinci kapsayıcıyı çalıştırın.

`<container-registry>`Ve `<container-name>` değerlerini kullandığınız kapsayıcıların değerleriyle değiştirin. Bunların aynı kapsayıcı olması gerekmez. Anomali algılayıcı kapsayıcısının ve LUSıS kapsayıcısının KONAKTA birlikte çalışmasını sağlayabilir veya çalışan birden fazla anomali algılayıcı kapsayıcınıza sahip olabilirsiniz.

İlk kapsayıcıyı 5000 numaralı bağlantı noktasında çalıştırın.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

İkinci kapsayıcıyı 5001 numaralı bağlantı noktasında çalıştırın.


```bash
docker run --rm -it -p 5000:5001 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Sonraki her kapsayıcı farklı bir bağlantı noktasında olmalıdır.

## <a name="query-the-containers-prediction-endpoint"></a>Kapsayıcının tahmin uç noktasını sorgulama

Kapsayıcı REST tabanlı sorgu tahmin uç noktası API’lerini sağlar.

Kapsayıcı API’leri için http://localhost:5000 konağını kullanın.

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Kapsayıcıyı durdurma

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcıyı bir çıkış [bağlaması](anomaly-detector-container-configuration.md#mount-settings) ve günlüğü etkin olarak çalıştırırsanız kapsayıcı, kapsayıcıyı başlatırken veya çalıştırırken oluşan sorunları gidermek için yararlı olan günlük dosyaları oluşturur.

[!INCLUDE [Cognitive Services FAQ note](../containers/includes/cognitive-services-faq-note.md)]

## <a name="billing"></a>Faturalandırma

Anomali algılayıcı kapsayıcıları, Azure hesabınızda bir _anomali algılayıcı_ kaynağı kullanarak faturalandırma bilgilerini Azure 'a gönderir.

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](anomaly-detector-container-configuration.md).

<!--blogs/samples/video coures -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Özet

Bu makalede, anomali algılayıcı kapsayıcılarını indirmek, yüklemek ve çalıştırmak için kavramlar ve iş akışı öğrendiniz. Özet:

* Anomali algılayıcısı, Docker için bir Linux kapsayıcısı, toplu iş vs akışı, beklenen Aralık çıkarımı ve duyarlık ayarlama ile Kapsülleyici algılama sağlar.
* Kapsayıcı görüntüleri, kapsayıcılar için ayrılmış özel bir Azure Container Registry indirilir.
* Kapsayıcı görüntüleri Docker 'da çalışır.
* Kapsayıcının ana bilgisayar URI 'sini belirterek anomali algılayıcı kapsayıcılarındaki işlemleri çağırmak için REST API veya SDK kullanabilirsiniz.
* Bir kapsayıcıyı örnekledikten sonra faturalandırma bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel hizmetler kapsayıcıları, ölçüm için Azure 'a bağlı kalmadan çalıştırılmak üzere lisanslanmaz. Müşterilerin, ödeme bilgilerini her zaman ölçüm hizmetiyle iletişimine olanak tanımak için kapsayıcıların etkinleştirilmesi gerekir. Bilişsel hizmetler kapsayıcıları müşteri verilerini (ör. çözümlenmekte olan zaman serisi verileri) Microsoft 'a göndermez.

## <a name="next-steps"></a>Sonraki adımlar

* Yapılandırma ayarları için [kapsayıcıları](anomaly-detector-container-configuration.md) yapılandırmayı gözden geçir
* [Azure Container Instances için bir anomali algılayıcı kapsayıcısı dağıtın](how-to/deploy-anomaly-detection-on-container-instances.md)
* [Anomali algılayıcı API hizmeti hakkında daha fazla bilgi edinin](https://go.microsoft.com/fwlink/?linkid=2080698&clcid=0x409)
