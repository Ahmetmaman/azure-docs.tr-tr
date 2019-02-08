---
title: Kapsayıcıları yükleme ve çalıştırma
titleSuffix: Text Analytics -  Azure Cognitive Services
description: İndirme, yükleme ve bu izlenecek yol öğreticide metin analizi için kapsayıcıları çalıştırmak nasıl.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 01/02/2019
ms.author: diberry
ms.openlocfilehash: 105b4e34d307ac08b8efbb5e263825f2df28e28c
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55862335"
---
# <a name="install-and-run-text-analytics-containers"></a>Yükleme ve metin analizi kapsayıcıları çalıştırma

Metin analizi kapsayıcılar sağlar ham metin üzerinde Gelişmiş doğal dil işleme ve üç ana işlev içerir: yaklaşım analizi, anahtar ifade ayıklama ve dil algılama. Varlık bağlama, bir kapsayıcıda şu anda desteklenmiyor. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Herhangi bir metin analizi kapsayıcıları çalıştırmak için aşağıdakilere sahip olmanız gerekir:

## <a name="preparation"></a>Hazırlık

Metin analizi kapsayıcıları kullanmadan önce aşağıdaki gereksinimleri karşılaması gerekir:

|Gerekli|Amaç|
|--|--|
|Docker altyapısı| Docker Altyapısı'nın kurulu ihtiyacınız bir [ana bilgisayar](#the-host-computer). Docker üzerinde Docker ortamını yapılandıran paketler sağlar [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), ve [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Docker ve kapsayıcı temelleri hakkında bilgi için bkz: [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/).<br><br> Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır. <br><br> **Windows üzerinde**, Docker de Linux kapsayıcıları destekler şekilde yapılandırılmalıdır.<br><br>|
|Docker ile aşinalık | Bir temel kavramlarını Docker kayıt defterleri, havuzları, kapsayıcılar ve kapsayıcı görüntülerinin yanı sıra temel bilgi gibi olmalıdır `docker` komutları.| 
|Metin analizi kaynak |Kapsayıcı kullanabilmeniz için şunlara sahip olmalısınız:<br><br>A [ _metin analizi_ ](text-analytics-how-to-access-key.md) fatura uç noktası URI'si ve ilişkili faturalandırma anahtarı almak için Azure kaynak. Her iki değeri de Azure portalının metin Analizi'ne genel bakış ve anahtarları sayfalarında kullanılabilir ve kapsayıcı başlatma için gereklidir.<br><br>**{BILLING_KEY}** : kaynak anahtarı<br><br>**{BILLING_ENDPOINT_URI}** : uç nokta URI'si örnektir: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0`|

### <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Request access to private preview](../../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Kapsayıcı gereksinimleri ve önerileri

Aşağıdaki tabloda açıklanmıştır en düşük ve önerilen CPU çekirdekleri, en az 2.6 gigahertz (GHz) veya daha hızlı ve her bir metin analizi kapsayıcısı için ayrılacak gigabayt (GB) bellek.

| Kapsayıcı | Minimum | Önerilen |
|-----------|---------|-------------|
|Anahtar İfade Ayıklama | 1 çekirdek, 2 GB bellek | 1 çekirdek, 4 GB bellek |
|Dil Algılama | 1 çekirdek, 2 GB bellek | 1 çekirdek, 4 GB bellek |
|Duygu Analizi | 1 çekirdek, 2 GB bellek | 1 çekirdek, 4 GB bellek |

Çekirdek ve bellek karşılık `--cpus` ve `--memory` parçası olarak kullanılan ayarları `docker run` komutu.

## <a name="get-the-container-image-with-docker-pull"></a>İle kapsayıcı görüntüsünü Al `docker pull`

Microsoft kapsayıcı kayıt defterinden kapsayıcı görüntülerini metin analizi için kullanılabilir. 

| Kapsayıcı | Havuz |
|-----------|------------|
|Anahtar İfade Ayıklama | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
|Dil Algılama | `mcr.microsoft.com/azure-cognitive-services/language` |
|Duygu Analizi | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

Kullanım [ `docker pull` ](https://docs.docker.com/engine/reference/commandline/pull/) Microsoft kapsayıcı kayıt defterinden bir kapsayıcı görüntüsü indirilemedi komut...

Metin analizi kapsayıcılar için kullanılabilir etiketler tam bir açıklaması için aşağıdaki kapsayıcıların Docker Hub bakın:

* [Anahtar ifade ayıklama](https://go.microsoft.com/fwlink/?linkid=2018757)
* [Dil algılama](https://go.microsoft.com/fwlink/?linkid=2018759)
* [Yaklaşım analizi](https://go.microsoft.com/fwlink/?linkid=2018654)

Kullanım [ `docker pull` ](https://docs.docker.com/engine/reference/commandline/pull/) komutu, kapsayıcı görüntüsü indirilemedi.


### <a name="docker-pull-for-the-key-phrase-extraction-container"></a>Docker isteği için anahtar tümcecik ayıklama kapsayıcısı

```Docker
docker pull mcr.microsoft.com/azure-cognitive-services/keyphrase:latest
```

### <a name="docker-pull-for-the-language-detection-container"></a>Dil algılama kapsayıcısı için docker isteği

```Docker
docker pull mcr.microsoft.com/azure-cognitive-services/language:latest
```

### <a name="docker-pull-for-the-sentiment-container"></a>Docker isteği yaklaşım kapsayıcısı için

```Docker
docker pull mcr.microsoft.com/azure-cognitive-services/sentiment:latest
```

[!INCLUDE [Tip for using docker list](../../../../includes/cognitive-services-containers-docker-list-tip.md)]


## <a name="how-to-use-the-container"></a>Kapsayıcı kullanma

Kapsayıcı açıldığında [ana bilgisayar](#the-host-computer), kapsayıcı ile çalışmak için aşağıdaki işlemi kullanın.

1. [Kapsayıcıyı çalıştırmak](#run-the-container-with-docker-run), gerekli faturalama ayarları. Daha fazla [örnekler](../text-analytics-resource-container-config.md#example-docker-run-commands) , `docker run` komutu kullanılabilir. 
1. [Kapsayıcının tahmini uç nokta sorgu](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Kapsayıcı ile çalıştırma `docker run`

Kullanım [docker run](https://docs.docker.com/engine/reference/commandline/run/) üç kapsayıcı birini çalıştırmak için komutu. Komutu şu parametreleri kullanır:

| Yer tutucu | Değer |
|-------------|-------|
|{BILLING_KEY} | Bu anahtar kapsayıcısı başlatmak için kullanılır ve Azure portalının metin analizi anahtarlar sayfasında bulabilirsiniz.  |
|{BILLING_ENDPOINT_URI} | Fatura uç noktası URI değerini Azure portalının metin Analizi'ne genel bakış sayfasında kullanılabilir.|

Bu parametreleri aşağıdaki örnekte kendi değerlerinizle değiştirin `docker run` komutu.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/keyphrase \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Bu komut:

* Bir anahtar tümcecik kapsayıcı kapsayıcı görüntüsünü çalıştırır.
* Bir CPU çekirdek ve 4 gigabayt (GB) bellek ayırır.
* 5000 numaralı TCP bağlantı noktasını kullanıma sunar ve sahte TTY için kapsayıcı ayırır.
* Bunu çıktıktan sonra kapsayıcı otomatik olarak kaldırır. Kapsayıcı görüntüsü ana bilgisayarda kullanılabilir durumda kalır. 

Daha fazla [örnekler](../text-analytics-resource-container-config.md#example-docker-run-commands) , `docker run` komutu kullanılabilir. 

> [!IMPORTANT]
> `Eula`, `Billing`, Ve `ApiKey` kapsayıcıyı çalıştırmak için seçenekler belirtilmelidir; Aksi takdirde, kapsayıcı başlatılamıyor.  Daha fazla bilgi için [faturalama](#billing).

## <a name="query-the-containers-prediction-endpoint"></a>Sorgu kapsayıcının tahmini uç noktası

Kapsayıcı, REST tabanlı sorgu tahmin uç nokta API'leri sağlar. 

Ana bilgisayarını kullanmak https://localhost:5000, kapsayıcı API'leri için.

## <a name="stop-the-container"></a>Kapsayıcı Durdur

[!INCLUDE [How to stop the container](../../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Sorun giderme

Kapsayıcı içeren bir çıktı çalıştırırsanız [bağlama](../text-analytics-resource-container-config.md#mount-settings) ve günlüğe kaydetme etkin, kapsayıcı başlatma veya kapsayıcı çalıştırma sırasında gerçekleşen sorunları gidermek yararlı olan günlük dosyalarını oluşturur. 

## <a name="containers-api-documentation"></a>Kapsayıcının API belgeleri

[!INCLUDE [Container's API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="billing"></a>Faturalandırma

Azure için fatura, kullanarak metin analizi kapsayıcıları Gönder bir _metin analizi_ Azure hesabınız kaynaktaki. 

Bilişsel hizmetler kapsayıcıları, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Bilişsel hizmetler kapsayıcılar, Microsoft müşteri verilerini göndermeyin. 

`docker run` Komutu, faturalama amacıyla aşağıdaki değişkenleri kullanır:

| Seçenek | Açıklama |
|--------|-------------|
| `ApiKey` | API anahtarı _metin analizi_ kaynak faturalandırma bilgileri izlemek için kullanılır. |
| `Billing` | Uç noktası _metin analizi_ kaynak faturalandırma bilgileri izlemek için kullanılır.|
| `Eula` | Kapsayıcı lisansını kabul ettiğinizi gösterir.<br/>Bu seçenek değeri ayarlanmalıdır `accept`. |

> [!IMPORTANT]
> Seçeneklerin üçünü geçerli değerlerle belirtilmiş olmalı veya kapsayıcı başlatılamıyor.

Bu seçenekler hakkında daha fazla bilgi için bkz. [kapsayıcıları yapılandırma](../text-analytics-resource-container-config.md).

## <a name="summary"></a>Özet

Bu makalede, kavramlar ve indirme, yükleme ve metin analizi kapsayıcıları çalıştırmak için iş akışı öğrendiniz. Özet:

* Metin analizi, anahtar ifade ayıklama, dil algılama ve yaklaşım analizi Kapsüllenen üç Linux kapsayıcıları için Docker, sağlar.
* Kapsayıcı görüntülerini azure'da Microsoft kapsayıcı kayıt defteri (MCR) alanından indirilir.
* Docker kapsayıcı görüntüleri çalıştırın.
* Metin analizi-kapsayıcılarında işlemleri ana kapsayıcısının URI belirterek çağırmak için REST API veya SDK'sını kullanabilirsiniz.
* Bir kapsayıcı örneği oluşturulurken, fatura bilgilerini belirtmeniz gerekir.

> [!IMPORTANT]
> Bilişsel hizmetler kapsayıcıları, kullanım ölçümü için Azure'a bağlanmadan çalıştırmak için lisanslanmaz. Müşteriler, her zaman faturalandırma bilgileri ölçüm hizmeti ile iletişim kurmak kapsayıcıları etkinleştirmeniz gerekiyor. Bilişsel hizmetler kapsayıcılar, Microsoft müşteri verilerini (örneğin, görüntü veya metin analiz edilen) göndermeyin.

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [kapsayıcıları yapılandırma](../text-analytics-resource-container-config.md) yapılandırma ayarları
* Başvurmak [sık sorulan sorular (SSS)](../text-analytics-resource-faq.md) işlevselliği ile ilgili sorunları gidermek için.

