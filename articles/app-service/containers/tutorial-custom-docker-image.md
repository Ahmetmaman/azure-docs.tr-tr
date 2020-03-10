---
title: 'Öğretici: özel görüntü oluşturma ve çalıştırma'
description: Azure App Service üzerinde çalışabilecek özel bir Linux görüntüsü oluşturmayı, Azure Container Registry 'ye dağıtmayı ve App Service içinde çalıştırmayı öğrenin.
keywords: azure app service, web uygulaması, linux, docker, kapsayıcı
author: msangapu-msft
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: 965897afc8e23c123575de0c497d4071ff4ca85a
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78356964"
---
# <a name="tutorial-build-a-custom-image-and-run-in-app-service-from-a-private-registry"></a>Öğretici: özel bir görüntü oluşturma ve özel bir kayıt defterinden App Service çalıştırma

[App Service](app-service-linux-intro.md) , LINUX 'ta PHP 7,3 ve Node. js 10,14 gibi belirli sürümleri destekleyen yerleşik Docker görüntüleri sağlar. App Service, yerleşik görüntüleri ve özel görüntüleri bir hizmet olarak platform olarak barındırmak için Docker kapsayıcı teknolojisini kullanır. Bu öğreticide, özel bir görüntü oluşturmayı ve App Service içinde çalıştırmayı öğreneceksiniz. Bu desen, yerleşik görüntülerin sizin tercih ettiğiniz dili içermediği veya uygulamanızın yerleşik görüntülerde sağlanmayan belirli bir yapılandırma gerektirdiği durumlarda yararlı olur.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Özel bir kapsayıcı kayıt defterine özel bir görüntü dağıtma
> * Özel görüntüyü App Service Çalıştır
> * Ortam değişkenlerini yapılandırma
> * Görüntüyü güncelleştirme ve yeniden dağıtma
> * Tanılama günlüklerine erişim
> * SSH kullanarak kapsayıcıya bağlanma

[!INCLUDE [Free trial note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Git](https://git-scm.com/downloads)
* [Docker](https://docs.docker.com/get-started/#setup)

## <a name="download-the-sample"></a>Örneği indirme

Terminal penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak ve örnek kodu içeren dizine gitmek için aşağıdaki komutları çalıştırın.

```bash
git clone https://github.com/Azure-Samples/docker-django-webapp-linux.git --config core.autocrlf=input
cd docker-django-webapp-linux
```

## <a name="build-the-image-from-the-docker-file"></a>Docker dosyasından görüntüyü oluşturma

Git deposunda _Dockerfile_ dosyasına göz atın. Bu dosya, uygulamanızı çalıştırmak için gereken Python ortamını açıklar. Görüntü ayrıca, kapsayıcı ile konak arasında güvenli iletişim için bir [SSH](https://www.ssh.com/ssh/protocol/) sunucusu ayarlar.

```Dockerfile
FROM python:3.4

RUN mkdir /code
WORKDIR /code
ADD requirements.txt /code/
RUN pip install -r requirements.txt
ADD . /code/

# ssh
ENV SSH_PASSWD "root:Docker!"
RUN apt-get update \
        && apt-get install -y --no-install-recommends dialog \
        && apt-get update \
    && apt-get install -y --no-install-recommends openssh-server \
    && echo "$SSH_PASSWD" | chpasswd 

COPY sshd_config /etc/ssh/
COPY init.sh /usr/local/bin/
    
RUN chmod u+x /usr/local/bin/init.sh
EXPOSE 8000 2222
#CMD ["python", "/code/manage.py", "runserver", "0.0.0.0:8000"]
ENTRYPOINT ["init.sh"]
```

`docker build` komutuyla Docker görüntüsünü oluşturun.

```bash
docker build --tag mydockerimage .
```

Docker kapsayıcısını çalıştırarak derlemenin çalışmasını test edin. [`docker run`](https://docs.docker.com/engine/reference/commandline/run/) komutunu gönderin ve bu komuta görüntünün adını ve etiketini geçirin. `-p` bağımsız değişkenini kullanarak bağlantı noktasını belirttiğinizden emin olun.

```bash
docker run -p 8000:8000 mydockerimage
```

`http://localhost:8000` konumuna göz atarak web uygulamasının ve kapsayıcının düzgün çalıştığını doğrulayın.

![Web uygulamasını yerel olarak test etme](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-local.png)

[!INCLUDE [Try Cloud Shell](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-app-to-azure"></a>Uygulamayı Azure’da dağıtma

Yeni oluşturduğunuz görüntüyü kullanan bir uygulama oluşturmak için, bir kaynak grubu oluşturan, görüntüyü gönderen ve sonra App Service planı Web uygulaması oluşturan Azure CLı komutlarını çalıştırın.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)] 

### <a name="create-an-azure-container-registry"></a>Azure Container Registry oluşturma

Cloud Shell'de, [`az acr create`](/cli/azure/acr?view=azure-cli-latest#az-acr-create) komutunu kullanarak bir Azure Container Registry oluşturun.

```azurecli-interactive
az acr create --name <azure-container-registry-name> --resource-group myResourceGroup --sku Basic --admin-enabled true
```

### <a name="sign-in-to-azure-container-registry"></a>Azure Container Registry oturum açın

Bir görüntüyü kayıt defterine göndermek için, özel kayıt defteri ile kimlik doğrulaması yapmanız gerekir. Cloud Shell, oluşturduğunuz kayıt defterinden kimlik bilgilerini almak için [`az acr show`](/cli/azure/acr?view=azure-cli-latest#az-acr-show) komutunu kullanın.

```azurecli-interactive
az acr credential show --name <azure-container-registry-name>
```

Çıktı, Kullanıcı adıyla birlikte iki parola ortaya koyar.

```json
{
  "passwords": [
    {
      "name": "password",
      "value": "{password}"
    },
    {
      "name": "password2",
      "value": "{password}"
    }
  ],
  "username": "<registry-username>"
}
```

Aşağıdaki örnekte gösterildiği gibi, yerel Terminal pencerenize `docker login` komutunu kullanarak Azure Container Registry oturum açın. *\<Azure-Container-Registry-name >* ve *\<registry-username >* kayıt defteriniz için değerlerle değiştirin. İstendiğinde, önceki adımda bulunan parolalardan birini yazın.

```bash
docker login <azure-container-registry-name>.azurecr.io --username <registry-username>
```

Oturum açmanın başarılı olduğunu doğrulayın.

### <a name="push-image-to-azure-container-registry"></a>Azure Container Registry’ye görüntü gönderme

Azure Container Registry için yerel görüntünüzü etiketleyin. Örneğin:
```bash
docker tag mydockerimage <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0
```

`docker push` komutunu kullanarak görüntüyü gönderin. Görüntüyü kayıt defterinin adıyla etiketleyin ve arkasına görüntü adınızı ve etiketinizi ekleyin.

```bash
docker push <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0
```

Cloud Shell geri dönüp gönderim işleminin başarılı olduğunu doğrulayın.

```azurecli-interactive
az acr repository list -n <azure-container-registry-name>
```

Aşağıdaki çıktıyı almalısınız.

```json
[
  "mydockerimage"
]
```

### <a name="create-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-web-app"></a>Web uygulaması oluşturma

Cloud Shell’de, [](app-service-linux-intro.md)`myAppServicePlan` komutuyla [ App Service planında bir `az webapp create`web uygulaması](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) oluşturun. _\<app-name >_ değerini benzersiz bir uygulama adıyla değiştirin ve _Azure-Container-registry-name >_ kayıt defteri adınızla\<.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --deployment-container-image-name <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0
```

Web uygulaması oluşturulduğunda Azure CLI aşağıda yer alan çıktıdaki gibi bilgiler gösterir:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app-name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="configure-registry-credentials-in-web-app"></a>Web uygulamasında kayıt defteri kimlik bilgilerini yapılandırma

Özel görüntüyü çekmek App Service için kayıt defteriniz ve görüntünüz hakkında bilgi gerekir. Cloud Shell, [`az webapp config container set`](/cli/azure/webapp/config/container?view=azure-cli-latest#az-webapp-config-container-set) komutuyla bunları sağlayın. *\<app-name >* , *\<Azure kapsayıcısı-kayıt defteri-adı >* , _\<registry-username >_ ve _\<Password >_ değiştirin.

```azurecli-interactive
az webapp config container set --name <app-name> --resource-group myResourceGroup --docker-custom-image-name <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0 --docker-registry-server-url https://<azure-container-registry-name>.azurecr.io --docker-registry-server-user <registry-username> --docker-registry-server-password <password>
```

> [!NOTE]
> Docker Hub 'dan farklı bir kayıt defteri kullanırken, `--docker-registry-server-url` `https://` olarak biçimlendirilmesi ve kayıt defterinin tam etki alanı adı gelmelidir.
>

### <a name="configure-environment-variables"></a>Ortam değişkenlerini yapılandırma

Çoğu Docker görüntüsü, 80 dışında bir bağlantı noktası gibi özel ortam değişkenleri kullanır. App Service, `WEBSITES_PORT` uygulama ayarını kullanarak görüntünüzün kullandığı bağlantı noktası hakkında bilgi sahibi olursunuz. [Bu öğreticideki Python örneği](https://github.com/Azure-Samples/docker-django-webapp-linux) için GitHub sayfası, `WEBSITES_PORT` olarak _8000_ ayarlamanız gerektiğini gösterir.

Uygulama ayarlarını belirlemek için Cloud Shell'de [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) komutunu kullanın. Uygulama ayarları büyük/küçük harfe duyarlıdır ve boşlukla ayrılmıştır.

```azurecli-interactive
az webapp config appsettings set --resource-group myResourceGroup --name <app-name> --settings WEBSITES_PORT=8000
```

### <a name="test-the-web-app"></a>Web uygulamasını test etme

Web uygulamasına göz atarak uygulamanın çalıştığını doğrulayın (`http://<app-name>.azurewebsites.net`).

> [!NOTE]
> Uygulamaya ilk kez eriştiğinizde, App Service görüntünün tamamını çekmesini gerektirdiğinden bu işlem biraz zaman alabilir. Tarayıcı zaman aşımına uğrarsa sayfayı yenilemeniz yeterlidir.

![Web uygulaması bağlantı noktası yapılandırmasını test etme](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-azure.png)

## <a name="change-web-app-and-redeploy"></a>Web uygulamasını değiştirme ve yeniden dağıtma

Git deponuzda app/templates/app/index.html dosyasını açın. İlk HTML öğesini bulun ve şöyle değiştirin.

```python
<nav class="navbar navbar-inverse navbar-fixed-top">
    <div class="container">
      <div class="navbar-header">
        <a class="navbar-brand" href="#">Azure App Service - Updated Here!</a>
      </div>
    </div>
  </nav>
```

Python dosyasını değiştirdikten ve kaydettikten sonra, yeni Docker görüntüsünü yeniden oluşturup göndermeniz gerekir. Ardından değişikliklerin geçerlilik kazanması için web uygulamasını yeniden başlatın. Bu öğreticide daha önce kullandığınız komutları kullanın. [Docker dosyasından görüntü oluşturma](#build-the-image-from-the-docker-file) ve [Azure Container Registry için görüntü gönderme](#push-image-to-azure-container-registry)bölümüne başvurabilirsiniz. [Web uygulamasını test etme](#test-the-web-app) başlığı altındaki yönergeleri izleyerek web uygulamasını test edin.

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="enable-ssh-connections"></a>SSH bağlantılarını etkinleştir

SSH, kapsayıcı ile istemci arasında güvenli iletişime olanak tanır. Kapsayıcınıza SSH bağlantısı sağlamak için özel görüntünüzün yapılandırılması gerekir. Gerekli yapılandırmaya zaten sahip olan örnek depoya göz atalım.

* [Dockerfile](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/Dockerfile)'da AŞAĞıDAKI kod SSH sunucusunu yükledikten sonra oturum açma kimlik bilgilerini de ayarlar.

    ```Dockerfile
    ENV SSH_PASSWD "root:Docker!"
    RUN apt-get update \
            && apt-get install -y --no-install-recommends dialog \
            && apt-get update \
      && apt-get install -y --no-install-recommends openssh-server \
      && echo "$SSH_PASSWD" | chpasswd 
    ```

    > [!NOTE]
    > Bu yapılandırma kapsayıcıya dış bağlantılar kurulmasına izin vermez. SSH yalnızca Kudu/SCM Sitesi aracılığıyla kullanılabilir. Kudu/SCM sitesinin kimliği, Azure hesabınızla doğrulanır.

* [Dockerfile](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/Dockerfile#L18) , depodaki [sshd_config](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/sshd_config) dosyasını */etc/ssh/* dizinine kopyalar.

    ```Dockerfile
    COPY sshd_config /etc/ssh/
    ```

* [Dockerfile](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/Dockerfile#L22) , kapsayıcıda 2222 numaralı bağlantı noktasını kullanıma sunar. Bu, yalnızca özel sanal ağın köprü ağı içinde yer alan kapsayıcıların erişebildiği dahili bir bağlantı noktasıdır. 

    ```Dockerfile
    EXPOSE 8000 2222
    ```

* [Giriş BETIĞI](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/init.sh#L5) SSH sunucusunu başlatır.

    ```bash
    #!/bin/bash
    service ssh start
    ```

### <a name="open-ssh-connection-to-container"></a>Kapsayıcıya SSH bağlantısı açma

SSH bağlantısı yalnızca `https://<app-name>.scm.azurewebsites.net`adresinden erişilebilen kudu sitesi aracılığıyla kullanılabilir.

Bağlanmak için, `https://<app-name>.scm.azurewebsites.net/webssh/host` adresine göz atın ve Azure hesabınızla oturum açın.

Ardından etkileşimli konsolu görüntüleyen bir sayfaya yeniden yönlendirilirsiniz.

Bazı uygulamaların kapsayıcı içinde çalıştırıldığını doğrulamak isteyebilirsiniz. Kapsayıcıyı incelemek ve çalıştırma işlemlerini doğrulamak için, komut isteminde `top` komutunu verin.

```bash
top
```

`top` komutu kapsayıcıda çalıştırılan tüm işlemleri ortaya çıkarır.

```
PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
 1 root      20   0  945616  35372  15348 S  0.0  2.1   0:04.63 node
20 root      20   0   55180   2776   2516 S  0.0  0.2   0:00.00 sshd
42 root      20   0  944596  33340  15352 S  0.0  1.9   0:05.80 node /opt/s+
56 root      20   0   59812   5244   4512 S  0.0  0.3   0:00.93 sshd
58 root      20   0   20228   3128   2664 S  0.0  0.2   0:00.00 bash
62 root      20   0   21916   2272   1944 S  0.0  0.1   0:03.15 top
63 root      20   0   59812   5344   4612 S  0.0  0.3   0:00.03 sshd
65 root      20   0   20228   3140   2672 S  0.0  0.2   0:00.00 bash
71 root      20   0   59812   5380   4648 S  0.0  0.3   0:00.02 sshd
73 root      20   0   20228   3160   2696 S  0.0  0.2   0:00.00 bash
77 root      20   0   21920   2304   1972 R  0.0  0.1   0:00.00 top
```

Tebrikler! App Service bir özel Linux kapsayıcısı yapılandırdınız.

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Öğrendikleriniz:

> [!div class="checklist"]
> * Özel bir kapsayıcı kayıt defterine özel bir görüntü dağıtma
> * Özel görüntüyü App Service Çalıştır
> * Ortam değişkenlerini yapılandırma
> * Görüntüyü güncelleştirme ve yeniden dağıtma
> * Tanılama günlüklerine erişim
> * SSH kullanarak kapsayıcıya bağlanma

Özel bir DNS adını uygulamanıza nasıl eşleyeceğinizi öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Öğretici: özel DNS adını uygulamanıza eşleyin](../app-service-web-tutorial-custom-domain.md)

Ya da diğer kaynaklara göz atın:

> [!div class="nextstepaction"]
> [Özel kapsayıcıyı yapılandırma](configure-custom-container.md)

> [!div class="nextstepaction"]
> [Öğretici: çok Kapsayıcılı WordPress uygulaması](tutorial-multi-container-app.md)
