---
title: Linux üzerinde özel görüntü kullanarak bir işlev oluşturma (önizleme) | Microsoft Docs
description: Özel bir Linux görüntüsü üzerinde çalışan Azure İşlevleri oluşturmayı öğrenin.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 10/19/2018
ms.topic: tutorial
ms.service: azure-functions
ms.custom: mvc
ms.devlang: azure-cli
manager: jeconnoc
ms.openlocfilehash: aa3c72c7ff2aa5e25fbff9fc38c33fd2dda34ecd
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49985089"
---
# <a name="create-a-function-on-linux-using-a-custom-image-preview"></a>Linux üzerinde özel görüntü kullanarak bir işlev oluşturma (önizleme)

Azure İşlevleri, işlevlerinizi Linux’ta kendi özel kapsayıcınızda barındırmanıza olanak sağlar. Ayrıca, [varsayılan bir Azure App Service kapsayıcısı üzerinde barındırabilirsiniz](functions-create-first-azure-function-azure-cli-linux.md). Bu işlevsellik şu anda önizleme aşamasındadır ve [İşlevler 2.0 çalışma zamanını](functions-versions.md) gerektirir.

Bu öğreticide işlevlerinizi Azure'a özel bir Docker görüntüsü olarak dağıtmayı öğreneceksiniz. Yerleşik App Service kapsayıcı görüntüsünü özelleştirmeniz gerektiğinde bu desen yararlıdır. İşlevleriniz belirli bir dil sürümüne gereksinim duyduğunda veya yerleşik görüntüde sağlanmayan belirli bir bağımlılık ya da yapılandırma gerektirdiğinde özel görüntü kullanmak isteyebilirsiniz.

Bu öğretici, özel bir Linux görüntüsünde bir işlev oluşturmak için Azure İşlevleri Çekirdek Araçları'nı kullanma adımlarını göstermektedir. Bu görüntüyü Azure CLI kullanılarak oluşturulan Azure'daki bir işlev uygulamasında yayımlayabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Çekirdek araçları ile bir işlev uygulaması ve Dockerfile oluşturun.
> * Docker kullanarak özel bir görüntü oluşturun.
> * Özel görüntüyü bir kapsayıcı kayıt defterinde yayımlayın.
> * Bir Azure Depolama hesabı oluşturun.
> * Bir Linux App Service planı oluşturun.
> * Docker Hub’dan bir işlev uygulaması dağıtın.
> * Uygulama ayarlarını işlevi uygulamasına ekleyin.

Aşağıdaki adımlar Mac, Windows veya Linux bilgisayarlarda desteklenir.  

## <a name="prerequisites"></a>Ön koşullar

Bu örneği çalıştırmadan önce aşağıdakilere sahip olmanız gerekir:

* [Azure Core Tools sürüm 2.x](functions-run-local.md#v2) yükleme.

* [Azure CLI]( /cli/azure/install-azure-cli)’yi yükleyin. Bu makale, Azure CLI 2.0 veya sonraki bir sürümü gerektirir. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın.  
[Azure Cloud Shell](https://shell.azure.com/bash)’i de kullanabilirsiniz.

* Etkin bir Azure aboneliği.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-local-function-app-project"></a>Yerel işlev uygulaması projesi oluşturma

Geçerli yerel dizinin `MyFunctionProj` klasöründe bir işlev uygulaması projesi oluşturmak için komut satırından aşağıdaki komutu çalıştırın.

```bash
func init MyFunctionProj --docker
```

`--docker` seçeneğini dahil ettiğinizde proje için bir Docker dosyası oluşturulur. Bu dosya, içinde projenin çalıştırılacağı özel bir kapsayıcı oluşturmak için kullanılır. Kullanılan temel görüntü, seçilen alt çalışma zamanı diline bağlıdır.  

İstendiğinde, şu dillerden bir alt çalışma zamanı seçin:

* `dotnet`: yeni bir .NET Core sınıf kitaplığı projesi (.csproj) oluşturur.
* `node`: bir JavaScript projesi oluşturur.

Komut yürütüldüğünde, aşağıdaki çıktıya benzer bir şey görürsünüz:

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Writing Dockerfile
```

Yeni `MyFunctionProj` proje klasörüne gitmek için aşağıdaki komutu kullanın.

```bash
cd MyFunctionProj
```

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-update-function-code](../../includes/functions-update-function-code.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

## <a name="build-the-image-from-the-docker-file"></a>Docker dosyasından görüntüyü oluşturma

Projenin kök klasöründeki _Dockerfile_'a bir göz atın. Bu dosya, işlev uygulamasını Linux üzerinde çalıştırmak için gereken ortamı açıklar. Aşağıda JavaScript (Node.js) alt çalışma zamanında bir işlev uygulaması çalıştıran bir kapsayıcı oluşturan bir Dockerfile örneği verilmiştir: 

```docker
FROM mcr.microsoft.com/azure-functions/node:2.0

ENV AzureWebJobsScriptRoot=/home/site/wwwroot
COPY . /home/site/wwwroot
```

> [!NOTE]
> Bir görüntüyü özel bir kapsayıcı kayıt defterinde barındırırken, Dockerfile dosyasındaki **ENV** değişkenlerini kullanarak işlev uygulamasına bağlantı ayarlarını eklemeniz gerekir. Bu öğretici özel bir kayıt defteri kullanmanızı garanti edemediğinden, en iyi güvenlik yöntemi olarak bağlantı ayarları, [dağıtımdan sonra Azure CLI kullanılarak eklenir](#configure-the-function-app).

### <a name="run-the-build-command"></a>`build` komutunu çalıştırın
Kök klasörde [docker derleme](https://docs.docker.com/engine/reference/commandline/build/) komutunu çalıştırın ve bir ad `mydockerimage` ve bir etiket `v1.0.0` sağlayın. `<docker-id>` değerini Docker Hub hesabınızın kimliğiyle değiştirin. Bu komut, kapsayıcı için Docker görüntüsünü derler.

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

Komut yürütüldüğünde, burada bir JavaScript alt çalışma zamanı için olan aşağıdaki gibi bir çıktı görürsünüz:

```bash
Sending build context to Docker daemon  17.41kB
Step 1/3 : FROM mcr.microsoft.com/azure-functions/node:2.0
2.0: Pulling from azure-functions/node
802b00ed6f79: Pull complete
44580ea7a636: Pull complete
73eebe8d57f9: Pull complete
3d82a67477c2: Pull complete
8bd51cd50290: Pull complete
7bd755353966: Pull complete
Digest: sha256:480e969821e9befe7c61dda353f63298f2c4b109e13032df5518e92540ea1d08
Status: Downloaded newer image for mcr.microsoft.com/azure-functions/node:2.0
 ---> 7c71671b838f
Step 2/3 : ENV AzureWebJobsScriptRoot=/home/site/wwwroot
 ---> Running in ed1e5809f0b7
Removing intermediate container ed1e5809f0b7
 ---> 39d9c341368a
Step 3/3 : COPY . /home/site/wwwroot
 ---> 5e196215935a
Successfully built 5e196215935a
Successfully tagged <docker-id>/mydockerimage:v1.0.0
```

### <a name="test-the-image-locally"></a>Görüntüyü yerel olarak test etme
Docker görüntüsünü yerel bir kapsayıcıda çalıştırarak derlediğiniz görüntün çalıştığını doğrulayın. [docker run](https://docs.docker.com/engine/reference/commandline/run/) komutunu gönderin ve bu komuta görüntünün adını ve etiketini geçirin. `-p` bağımsız değişkenini kullanarak bağlantı noktasını belirttiğinizden emin olun.

```bash
docker run -p 8080:80 -it <docker-ID>/mydockerimage:v1.0.0
```

Özel görüntü ile yerel bir Docker kapsayıcısı içinde çalışırken <http://localhost:8080> sayfasına göz atarak uygulama ve kapsayıcının doğru şekilde çalıştığını onaylayın.

![İşlev uygulamasını yerel olarak test edin.](./media/functions-create-function-linux-custom-image/run-image-local-success.png)

İsterseniz işlevinizi, bu kez aşağıdaki URL'yi kullanan yerel kapsayıcıda yeniden test edebilirsiniz:

`http://localhost:8080/api/myhttptrigger?name=<yourname>`

Kapsayıcıda işlev uygulamasını doğruladıktan sonra, yürütmeyi durdurun. Şimdi, özel görüntüyü Docker Hub hesabınıza gönderebilirsiniz.

## <a name="push-the-custom-image-to-docker-hub"></a>Özel görüntüyü Docker Hub’a gönderme

Kayıt defteri, görüntüleri barındıran ve hizmet görüntüsüyle kapsayıcı hizmetleri sağlayan bir uygulamadır. Görüntünüzü paylaşmak için bir kayıt defterine göndermelisiniz. Docker Hub, Docker görüntüleri için kayıt defteridir ve kendi depolarınızı (ortak veya özel) barındırmanıza olanak tanır.

Görüntüyü gönderebilmek için önce [docker login](https://docs.docker.com/engine/reference/commandline/login/) komutunu kullanarak Docker Hub'da oturum açmalısınız. `<docker-id>` yerine hesabınızı kullanın ve konsoldaki bilgi istemine parolanızı yazın. Diğer Docker Hub parola seçenekleri için [docker oturum açma komutu belgelerine](https://docs.docker.com/engine/reference/commandline/login/) bakın.

```bash
docker login --username <docker-id>
```

"Oturum açma başarılı oldu" iletisi, oturumunuzun açıldığını onaylar. Oturum açtıktan sonra [docker push](https://docs.docker.com/engine/reference/commandline/push/) komutunu kullanarak görüntüyü Docker Hub’a gönderin.

```bash
docker push <docker-id>/mydockerimage:v1.0.0
```

Komutun çıkışını inceleyerek göndermenin başarılı olduğunu doğrulayın.

```bash
The push refers to a repository [docker.io/<docker-id>/mydockerimage:v1.0.0]
24d81eb139bf: Pushed
fd9e998161c9: Mounted from <docker-id>/mydockerimage
e7796c35add2: Mounted from <docker-id>/mydockerimage
ae9a05b85848: Mounted from <docker-id>/mydockerimage
45c86e20670d: Mounted from <docker-id>/mydockerimage
v1.0.0: digest: sha256:be080d80770df71234eb893fbe4d... size: 1796
```

Artık bu görüntüyü, Azure’da yeni bir işlev uygulaması için dağıtım kaynağı olarak kullanabilirsiniz.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Bir Linux App Service planı oluşturma

Linux İşlev barındırma şu anda tüketim planlarında desteklenmemektedir. Bir Linux App Service planında Linux kapsayıcı uygulamaları barındırmalısınız. Barındırma hakkında daha fazla bilgi edinmek için, bkz. [Azure İşlevleri barındırma planları karşılaştırması](functions-scale.md).

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

## <a name="create-and-deploy-the-custom-image"></a>Özel görüntü oluşturup dağıtma

İşlev uygulaması, işlevlerinizin yürütülmesini barındırır. [az functionapp create](/cli/azure/functionapp#az-functionapp-create) komutunu kullanarak bir Docker Hub görüntüsünden işlev uygulaması oluşturun.

Aşağıdaki komutta benzersiz bir işlev uygulamasının adını `<app_name>` yer tutucusunun ve `<storage_name>` depolama hesabı adının yerine ekleyin. `<app_name>`, işlev uygulamasının varsayılan DNS etki alanı olarak kullanılacağı için adın Azure’daki tüm uygulamalarda benzersiz olması gerekir. Daha önce olduğu gibi, `<docker-id>` Docker hesap adınızdır.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myAppServicePlan --deployment-container-image-name <docker-id>/mydockerimage:v1.0.0
```

İşlev uygulaması oluşturulduktan sonra Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.net",
    "quickstart.scm.azurewebsites.net"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

_deployment-container-image-name_ parametresi, işlev uygulamasını oluşturmak üzere kullanmak için Docker Hub üzerinde barındırılan görüntüyü belirtir.

## <a name="configure-the-function-app"></a>İşlev uygulamasını yapılandırma

İşlevin varsayılan depolama hesabına bağlanması için bağlantı dizesi gerekir. Özel görüntünüzü özel bir kapsayıcı hesabında yayımlarken, bu uygulama ayarlarını bunun yerine, [ENV yönergesi](https://docs.docker.com/engine/reference/builder/#env) ya da benzeri ile Dockerfile'da ortam değişkenleri olarak belirlemeniz gerekir.

Bu durumda `<storage_account>`, oluşturduğunuz depolama hesabının adıdır. [az storage account show-connection-string](/cli/azure/storage/account#show-connection-string) komutu ile bağlantı dizesini alın. [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) komutu ile bu uygulama ayarlarını işlev uygulamasına ekleyin.

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings AzureWebJobsDashboard=$storageConnectionString \
AzureWebJobsStorage=$storageConnectionString
```

Artık Azure’da Linux üzerinde çalışan işlevlerinizi test edebilirsiniz.

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Çekirdek araçları ile bir işlev uygulaması ve Dockerfile oluşturun.
> * Docker kullanarak özel bir görüntü oluşturun.
> * Özel görüntüyü bir kapsayıcı kayıt defterinde yayımlayın.
> * Bir Azure Depolama hesabı oluşturun.
> * Bir Linux App Service planı oluşturun.
> * Docker Hub’dan bir işlev uygulaması dağıtın.
> * Uygulama ayarlarını işlevi uygulamasına ekleyin.

Temel App Service platformu içine yerleştirilmiş sürekli tümleştirme işlevselliğini nasıl etkinleştireceğinizi öğrenin. İşlev uygulamanızı, Docker Hub’da görüntünüzü güncelleştirdiğinizde kapsayıcının yeniden dağıtılmasını sağlayacak şekilde yapılandırabilirsiniz.

> [!div class="nextstepaction"] 
> [Kapsayıcılar için Web App ile sürekli dağıtım](../app-service/containers/app-service-linux-ci-cd.md)
