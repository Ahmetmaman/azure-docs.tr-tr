---
title: Öğretici-Service Fabric bir kafes uygulaması dağıtma
description: Visual Studio kullanarak arka uç web hizmetiyle iletişim kuran ASP.NET Core web sitesi içeren bir Azure Service Fabric Mesh uygulamasını dağıtmayı öğrenin.
author: georgewallace
ms.topic: tutorial
ms.date: 09/18/2018
ms.author: gwallace
ms.custom: mvc, devcenter
ms.openlocfilehash: be1e477bf3f11d487b8a6705535c09ff6e2b9c3e
ms.sourcegitcommit: 59cfed657839f41c36ccdf7dc2bee4535c920dd4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2021
ms.locfileid: "99626742"
---
# <a name="tutorial-deploy-a-service-fabric-mesh-application"></a>Öğretici: Service Fabric Mesh uygulaması dağıtma

> [!IMPORTANT]
> Azure Service Fabric ağı önizlemesi devre dışı bırakıldı. Yeni dağıtımlar Service Fabric kafes API 'SI aracılığıyla artık izin verilmeyecektir. Mevcut dağıtımlar için destek 28 Nisan 2021 ' den devam edecektir.
> 
> Ayrıntılar için bkz. [Azure Service Fabric kafes önizleme kullanımdan](https://azure.microsoft.com/updates/azure-service-fabric-mesh-preview-retirement/)kaldırma.

Bir serinin üçüncü kısmı olan bu öğreticide doğrudan Visual Studio'dan bir Azure Service Fabric Mesh web uygulaması yayımlamayı öğreneceksiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Visual Studio kullanarak uygulamayı yayımlayın.
> * Uygulama dağıtım durumunu denetleme
> * Aboneliğinize dağıtılmış olan tüm uygulamaları görme

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [Visual Studio’da Service Fabric Mesh uygulaması oluşturma](service-fabric-mesh-tutorial-create-dotnetcore.md)
> * [Yerel geliştirme kümenizde çalışan bir Service Fabric Mesh uygulamasının hatalarını ayıklama](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
> * Service Fabric Mesh uygulaması dağıtma
> * [Service Fabric Mesh uygulamasını yükseltme](service-fabric-mesh-tutorial-upgrade.md)
> * [Service Fabric Mesh kaynaklarını temizleme](service-fabric-mesh-tutorial-cleanup-resources.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturabilirsiniz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) .

* [Geliştirme ortamınızı ayarladığınızdan](service-fabric-mesh-howto-setup-developer-environment-sdk.md) ve Service Fabric çalışma zamanı, SDK, Docker ve Visual Studio 2017'yi yüklediğinizden emin olun.

## <a name="download-the-to-do-sample-application"></a>Yapılacaklar örnek uygulamasını indirme

[Bu öğretici serisinin ikinci kısmında](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md) yapılacak işler örnek uygulamasını oluşturmadıysanız, indirebilirsiniz. Komut penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın.

```
git clone https://github.com/azure-samples/service-fabric-mesh
```

Uygulama `src\todolistapp` dizininin altındadır.

## <a name="publish-to-azure"></a>Azure’da Yayımlama

Service Fabric Mesh projenizi Azure'da yayımlamak için Visual Studio'da **todolistapp** girişine sağ tıklayıp **Yayımla...** öğesini seçin.

Ardından **Service Fabric Uygulamasını Yayımla** iletişim kutusunu göreceksiniz.

![Visual Studio Service Fabric Mesh yayımla iletişim kutusu](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-dialog.png)

Azure hesabınızı ve aboneliğinizi seçin. Bir **konum** seçin. Bu makalede **Doğu ABD** kullanılmıştır.

**Kaynak grubu** altında öğesini seçin **\<Create New Resource Group...>** . Yeni bir kaynak grubu oluşturabileceğiniz bir iletişim kutusu açılır. Bu makalede **Doğu ABD** konumu kullanılmış ve grup **sfmeshTutorial1RG** olarak adlandırılmıştır (kuruluşunuzda aynı aboneliği kullanan birden fazla kullanıcı varsa benzersiz bir grup adı seçin).  **Oluştur**'a bastığınızda kaynak grubu oluşturulur ve yayımla iletişim kutusu açılır.

![Visual Studio Service Fabric Mesh yeni kaynak grubu iletişim kutusu](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-new-resource-group-dialog.png)

**Service Fabric uygulamayı Yayımla** iletişim kutusuna geri dönün **Azure Container Registry** altında öğesini seçin **\<Create New Container Registry...>** . **Container Registry Oluştur** iletişim kutusunda **Container registry adı** için benzersiz bir ad girin. Bir **Konum** belirtin (bu öğreticide **Doğu ABD** kullanılmıştır). Açılan menüden önceki adımda oluşturduğunuz **Kaynak grubu** adını seçin, örneğin **sfmeshTutorial1RG**. **SKU** değerini **Temel**’e ayarladıktan sonra **Oluştur**’a basarak özel Azure kapsayıcı kayıt defterini kapatıp yayımlama iletişim kutusuna geri dönün.

![Visual Studio Service Fabric Mesh yeni kapsayıcı kayıt defteri iletişim kutusu](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-new-container-registry-dialog.png)

Aboneliğiniz için bir kaynak sağlayıcısı kaydedilmediğini belirten bir hata alırsanız gerekli kaydı yapabilirsiniz. Öncelikle kaynak sağlayıcısının aboneliğiniz için kullanılabilir durumda olup olmadığına bakın:

```Powershell
Get-AzureRmResourceProvider -ListAvailable
```

Container Registry sağlayıcısı (`Microsoft.ContainerRegistry`) kullanılabilir durumdaysa Powershell'den kaydedin:

```Powershell
Connect-AzureRmAccount
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ContainerRegistry
```

Yayımla iletişim kutusunda **Yayımla** düğmesine basarak Service Fabric uygulamanızı Azure'a dağıtın.

Azure'da ilk kez uygulama yayımladığınızda, Docker görüntüsü Azure Container Registry (ACR) konumuna iletilir ve bu da görüntünün boyutuna göre zaman alabilir. Aynı projenin sonraki yayımlama işlemleri daha hızlı olacaktır. Visual Studio **Çıktı** penceresinde **Service Fabric Araçları** bölmesini seçerek dağıtımın ilerleme durumunu izleyebilirsiniz. Dağıtım tamamlandıktan sonra **Service Fabric Araçları** çıktısı, uygulamanızın IP adresini ve bağlantı noktasını URL biçiminde gösterir.

```
Packaging Application...
Building Images...
Web1 -> C:\Code\ServiceFabricMeshApp\ToDoService\bin\Any CPU\Release\netcoreapp2.0\ToDoService.dll
Uploading the images to Azure Container Registry...
Deploying application to remote endpoint...
The application was deployed successfully and it can be accessed at http://10.000.38.000:20000.
```

Bir web tarayıcısı açıp URL'ye giderek Azure'da çalışan web sitenizi görebilirsiniz.

## <a name="set-up-service-fabric-mesh-cli"></a>Service Fabric Mesh CLI’yi ayarlama

Kalan adımlar için Azure Cloud Shell veya yerel bir Azure CLI yüklemesi kullanabilirsiniz. Şu [yönergeleri](service-fabric-mesh-howto-setup-cli.md) izleyerek Azure Service Fabric Mesh CLI uzantısı modülünü yükleyin.

## <a name="check-application-deployment-status"></a>Uygulama dağıtım durumunu denetleme

Bu noktada uygulamanız artık dağıtılmış durumdadır. `app show` komutunu kullanarak durumunu kontrol edebilirsiniz. 

Öğretici uygulamasının adı `todolistapp` olacaktır. Aşağıdaki komutla uygulama hakkında ayrıntılı bilgilere ulaşabilirsiniz:

```azurecli-interactive
az mesh app show --resource-group $rg --name todolistapp
```

## <a name="get-the-ip-address-of-your-deployment"></a>Dağıtımınızın IP adresini alın

Uygulamanızın IP adresini almak istiyorsanız aşağıdaki komutu kullanın:
  
```azurecli-interactive
az mesh gateway show --resource-group myResourceGroup --name todolistappGateway
```

## <a name="see-all-applications-currently-deployed-to-your-subscription"></a>Aboneliğinize dağıtılmış olan tüm uygulamaları görme

Aboneliğinize dağıttığınız uygulamaların listesini almak için "app list" komutunu kullanabilirsiniz.

```azurecli-interactive
az mesh app list --output table
```

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]
> * Uygulamayı Azure’da yayımlama.
> * Uygulama dağıtım durumunu denetleme
> * Aboneliğinize dağıtılmış olan tüm uygulamaları görme

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Service Fabric Mesh uygulamasını yükseltme](service-fabric-mesh-tutorial-upgrade.md)

[azure-cli-install]: /cli/azure/install-azure-cli?view=azure-cli-latest
