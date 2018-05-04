---
title: Service Fabric için .NET uygulaması oluşturma | Microsoft Docs
description: Bu öğreticide, ASP.NET Core ön ucuyla ve durum bilgisi olan bir güvenilir hizmet arka ucuyla uygulama oluşturmayı ve uygulamayı kümeye dağıtmayı öğrenirsiniz.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/29/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 7840dc86ec7753980bb2c35f932f132c50d65f9e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="tutorial-create-and-deploy-an-application-with-an-aspnet-core-web-api-front-end-service-and-a-stateful-back-end-service"></a>Öğretici: ASP.NET Core Web API'si ön uç hizmeti ve durum bilgisi olan bir arka uç hizmetiyle uygulama oluşturma ve dağıtma
Bu öğretici, bir dizinin birinci bölümüdür.  Verileri depolamak için ASP.NET Core Web API'si ön ucu ve durum bilgisi olan bir arka uç hizmetiyle Azure Service Fabric uygulaması oluşturmayı öğreneceksiniz. Bitirdiğinizde, oylama sonuçlarını kümedeki durum bilgisi içeren arka uç hizmetine kaydeden bir ASP.NET Core web ön ucuna sahip oylama uygulaması sağlanır. Oylama uygulamasını el ile oluşturmak istemiyorsanız, tamamlanmış uygulamanın [kaynak kodunu indirebilir](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) ve [Oylama örnek uygulamasında izlenecek yol](#walkthrough_anchor) bölümüne atlayabilirsiniz.  İsterseniz, bu öğreticiye ait bir [video kılavuzu](https://channel9.msdn.com/Events/Connect/2017/E100) da izleyebilirsiniz.

![Uygulama Diyagramı](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

Serinin birinci bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * ASP.NET Core Web API'si hizmetini durum bilgisi olan güvenilir bir hizmet olarak oluşturma
> * ASP.NET Core Web Uygulaması hizmetini durum bilgisi olmayan bir web hizmeti olarak oluşturma
> * Durum bilgisi olan hizmetle iletişim kurmak için ters proxy kullanma

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * .NET Service Fabric uygulaması oluşturma
> * [Uygulamayı uzak kümeye dağıtma](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [Bir ASP.NET Core ön uç hizmetine HTTPS uç noktası ekleme](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
> * [Visual Studio Team Services'i kullanarak CI/CD'yi yapılandırma](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * [Uygulama için izleme ve tanılamayı ayarlama](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce:
- Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun
- [Azure geliştirme](https://www.visualstudio.com/) ve **ASP.NET ve Web geliştirme** iş yükleriyle **Visual Studio 2017** sürüm 15.5 veya üstünü yükleyin.
- [Service Fabric SDK'yı yükleyin](service-fabric-get-started.md)

## <a name="create-an-aspnet-web-api-service-as-a-reliable-service"></a>ASP.NET Web API'si hizmetini güvenilir bir hizmet olarak oluşturma
Başlangıçta, ASP.NET Core kullanarak oylama uygulamasının web ön ucunu oluşturun. ASP.NET Core, modern web kullanıcı arabirimleri ve web API'leri oluşturmak için kullanabileceğiniz basit, platformlar arası bir web geliştirme çerçevesidir. ASP.NET Core'un Service Fabric ile nasıl tümleştirildiğini her yönüyle anlamanız için, [Service Fabric Reliable Services'te ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md) makalesini okumanızı kesinlikle öneririz. Şimdilik, hızlı bir başlangıç yapmak için öğreticiyi izleyebilirsiniz. ASP.NET Core hakkında daha fazla bilgi edinmek için [ASP.NET Core Belgelerine](https://docs.microsoft.com/aspnet/core/) bakın.

1. Visual Studio'yu **yönetici** olarak başlatın.

2. **Dosya**->**Yeni**->**Proje**'yi kullanarak bir proje oluşturun

3. **Yeni Proje** iletişim kutusunda **Bulut > Service Fabric Uygulaması**'nı seçin.

4. Uygulamaya **Voting** adını verin ve **Tamam**'a basın.

   ![Visual Studio'da yeni proje iletişim kutusu](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog.png)

5. **Yeni Service Fabric Hizmeti** sayfasında **Durum Bilgisi Olmayan ASP.NET Core** öğesini seçin ve hizmetinize **VotingWeb** adını verin.
   
   ![Yeni hizmet iletişim kutusunda ASP.NET web hizmetini seçme](./media/service-fabric-tutorial-create-dotnet-app/new-project-dialog-2.png) 

6. Sonraki sayfada, bir dizi ASP.NET Core proje şablonu sağlanır. Bu öğretici için, **Web Uygulaması (Model-View-Controller)** seçeneğini kullanın. 
   
   ![ASP.NET proje türünü seçme](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog.png)

   Visual Studio bir uygulama ve bir hizmet projesi oluşturup bunları Çözüm Gezgini'nde görüntüler.

   ![ASP.NET Core Web API'si hizmeti içeren uygulama oluşturulduktan sonra Çözüm Gezgini]( ./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-service.png)

### <a name="add-angularjs-to-the-votingweb-service"></a>VotingWeb hizmetine AngularJS ekleme
[Bower desteğini](/aspnet/core/client-side/bower) kullanarak hizmetinize [AngularJS](http://angularjs.org/) ekleyin. İlk olarak, projeye bir Bower yapılandırma dosyası ekleyin.  Çözüm Gezgini'nde, **VotingWeb** öğesine sağ tıklayın ve **Ekle->Yeni Öğe**'yi seçin. **Web**'i ve ardından **Bower Yapılandırma Dosyası**'nı seçin.  *Bower.json* dosyası oluşturulur.

*Bower.json* dosyasını açın, angular ile angular-bootstrap için girişler ekleyin ve sonra da değişikliklerinizi kaydedin.

```json
{
  "name": "asp.net",
  "private": true,
  "dependencies": {
    "bootstrap": "3.3.7",
    "jquery": "3.2.1",
    "jquery-validation": "1.16.0",
    "jquery-validation-unobtrusive": "3.2.6",
    "angular": "v1.6.8",
    "angular-bootstrap": "v1.1.0"
  }
}
```
*Bower.json* dosyasının kaydedilmesi üzerine, projenizin *wwwroot/lib* klasörüne Angular yüklenir. Buna ek olarak, *Bağımlılıklar/Bower* klasörünüzde de listelenir.

### <a name="update-the-sitejs-file"></a>Site.js dosyasını güncelleştirme
*wwwroot/js/site.js* dosyasını açın.  Dosyanın içeriğini Giriş görünümlerinde kullanılan JavaScript ile değiştirin:

```javascript
var app = angular.module('VotingApp', ['ui.bootstrap']);
app.run(function () { });

app.controller('VotingAppController', ['$rootScope', '$scope', '$http', '$timeout', function ($rootScope, $scope, $http, $timeout) {

    $scope.refresh = function () {
        $http.get('api/Votes?c=' + new Date().getTime())
            .then(function (data, status) {
                $scope.votes = data;
            }, function (data, status) {
                $scope.votes = undefined;
            });
    };

    $scope.remove = function (item) {
        $http.delete('api/Votes/' + item)
            .then(function (data, status) {
                $scope.refresh();
            })
    };

    $scope.add = function (item) {
        var fd = new FormData();
        fd.append('item', item);
        $http.put('api/Votes/' + item, fd, {
            transformRequest: angular.identity,
            headers: { 'Content-Type': undefined }
        })
            .then(function (data, status) {
                $scope.refresh();
                $scope.item = undefined;
            })
    };
}]);
```

### <a name="update-the-indexcshtml-file"></a>Index.cshtml dosyasını güncelleştirme
Giriş denetleyicisine özgü görünüm olan *Views/Home/Index.cshtml* dosyasını açın.  Dosyanın içeriğini aşağıdakilerle değiştirin ve sonra da değişikliklerinizi kaydedin.

```html
@{
    ViewData["Title"] = "Service Fabric Voting Sample";
}

<div ng-controller="VotingAppController" ng-init="refresh()">
    <div class="container-fluid">
        <div class="row">
            <div class="col-xs-8 col-xs-offset-2 text-center">
                <h2>Service Fabric Voting Sample</h2>
            </div>
        </div>

        <div class="row">
            <div class="col-xs-8 col-xs-offset-2">
                <form class="col-xs-12 center-block">
                    <div class="col-xs-6 form-group">
                        <input id="txtAdd" type="text" class="form-control" placeholder="Add voting option" ng-model="item"/>
                    </div>
                    <button id="btnAdd" class="btn btn-default" ng-click="add(item)">
                        <span class="glyphicon glyphicon-plus" aria-hidden="true"></span>
                        Add
                    </button>
                </form>
            </div>
        </div>

        <hr/>

        <div class="row">
            <div class="col-xs-8 col-xs-offset-2">
                <div class="row">
                    <div class="col-xs-4">
                        Click to vote
                    </div>
                </div>
                <div class="row top-buffer" ng-repeat="vote in votes.data">
                    <div class="col-xs-8">
                        <button class="btn btn-success text-left btn-block" ng-click="add(vote.key)">
                            <span class="pull-left">
                                {{vote.key}}
                            </span>
                            <span class="badge pull-right">
                                {{vote.value}} Votes
                            </span>
                        </button>
                    </div>
                    <div class="col-xs-4">
                        <button class="btn btn-danger pull-right btn-block" ng-click="remove(vote.key)">
                            <span class="glyphicon glyphicon-remove" aria-hidden="true"></span>
                            Remove
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
```

### <a name="update-the-layoutcshtml-file"></a>_Layout.cshtml dosyasını güncelleştirme
ASP.NET uygulaması için varsayılan düzen olan *Views/Shared/_Layout.cshtml* dosyasını açın.  Dosyanın içeriğini aşağıdakilerle değiştirin ve sonra da değişikliklerinizi kaydedin.

```html
<!DOCTYPE html>
<html ng-app="VotingApp" xmlns:ng="http://angularjs.org">
<head>
    <meta charset="utf-8"/>
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>@ViewData["Title"]</title>

    <link href="~/lib/bootstrap/dist/css/bootstrap.min.css" rel="stylesheet"/>
    <link href="~/css/site.css" rel="stylesheet"/>

</head>
<body>
<div class="container body-content">
    @RenderBody()
</div>

<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
<script src="~/lib/angular/angular.js"></script>
<script src="~/lib/angular-bootstrap/ui-bootstrap-tpls.js"></script>
<script src="~/js/site.js"></script>

@RenderSection("Scripts", required: false)
</body>
</html>
```

### <a name="update-the-votingwebcs-file"></a>VotingWeb.cs dosyasını güncelleştirme
WebListener web sunucusunu kullanarak durum bilgisi olmayan hizmetin içinde ASP.NET Core WebHost'u oluşturan *VotingWeb.cs* dosyasını açın.  

Dosyanın en üstüne `using System.Net.Http;` yönergesini ekleyin.  

`CreateServiceInstanceListeners()` işlevini aşağıdakilerle değiştirin, sonra değişikliklerinizi kaydedin.

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(
            serviceContext =>
                new KestrelCommunicationListener(
                    serviceContext,
                    "ServiceEndpoint",
                    (url, listener) =>
                    {
                        ServiceEventSource.Current.ServiceMessage(serviceContext, $"Starting Kestrel on {url}");

                        return new WebHostBuilder()
                            .UseKestrel()
                            .ConfigureServices(
                                services => services
                                    .AddSingleton<HttpClient>(new HttpClient())
                                    .AddSingleton<FabricClient>(new FabricClient())
                                    .AddSingleton<StatelessServiceContext>(serviceContext))
                            .UseContentRoot(Directory.GetCurrentDirectory())
                            .UseStartup<Startup>()
                            .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                            .UseUrls(url)
                            .Build();
                    }))
    };
}
```

Ayrıca, yoklama yapıldığında hizmet adını döndüren `GetVotingDataServiceName` yöntemini de ekleyin:

```csharp
internal static Uri GetVotingDataServiceName(ServiceContext context)
{
    return new Uri($"{context.CodePackageActivationContext.ApplicationName}/VotingData");
}
```

### <a name="add-the-votescontrollercs-file"></a>VotesController.cs dosyasını ekleme
Oylama eylemlerini tanımlayan bir denetleyici ekleyin. **Denetleyiciler** klasörüne sağ tıklayın ve **Ekle->Yeni öğe->Sınıf**'ı seçin.  Dosyaya "VotesController.cs" adını verin ve **Ekle**'ye tıklayın.  

Dosyanın içeriğini aşağıdakilerle değiştirin, sonra değişikliklerinizi kaydedin.  Daha sonra, [VotesController.cs dosyasını güncelleştirme](#updatevotecontroller_anchor) bölümünde bu dosya arka uç hizmetinde oylama verilerini okuyacak ve yazacak şekilde değiştirilir.  Şimdilik, denetleyici görünüme statik dize verileri döndürür.

```csharp
namespace VotingWeb.Controllers
{
    using System;
    using System.Collections.Generic;
    using System.Fabric;
    using System.Fabric.Query;
    using System.Linq;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;
    using Newtonsoft.Json;

    [Produces("application/json")]
    [Route("api/Votes")]
    public class VotesController : Controller
    {
        private readonly HttpClient httpClient;

        public VotesController(HttpClient httpClient)
        {
            this.httpClient = httpClient;
        }

        // GET: api/Votes
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            List<KeyValuePair<string, int>> votes= new List<KeyValuePair<string, int>>();
            votes.Add(new KeyValuePair<string, int>("Pizza", 3));
            votes.Add(new KeyValuePair<string, int>("Ice cream", 4));

            return Json(votes);
        }
     }
}
```

### <a name="configure-the-listening-port"></a>Dinleme bağlantı noktasını yapılandırma
VotingWeb ön uç hizmeti oluşturulduğunda, Visual Studio hizmetin dinlemesi için rastgele bir bağlantı noktası seçer.  VotingWeb hizmeti bu uygulama için ön uç işlevi görür ve dış trafiği kabul eder; bu nedenle şimdi bu hizmeti sabit ve iyi tanınan bir bağlantı noktasına bağlayalım.  [Hizmet bildirimi](service-fabric-application-and-service-manifests.md) hizmet uç noktalarını bildirir. Çözüm Gezgini'nde *VotingWeb/PackageRoot/ServiceManifest.xml* dosyasını açın.  **Kaynaklar** bölümünde **Uç Nokta** kaynağını bulun ve **Bağlantı Noktası** değerini 80 numaralı veya başka bir bağlantı noktasıyla değiştirin. Uygulamayı yerel olarak dağıtmak ve çalıştırmak için, uygulama dinleme bağlantı noktasının bilgisayarınızda açık ve kullanılabilir olması gerekir.

```xml
<Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="80" />
    </Endpoints>
  </Resources>
```

Ayrıca Voting projesindeki Uygulama URL'si özellik değerini de güncelleştirin ve 'F5' kullanarak hata ayıklaması yaptığınızda web tarayıcısının doğru bağlantı noktasına açılmasını sağlayın.  Çözüm Gezgini'nde, **Voting** projesini seçin ve **Uygulama URL'si** özelliğini güncelleştirin.

![Uygulama URL'si](./media/service-fabric-tutorial-deploy-app-to-party-cluster/application-url.png)

### <a name="deploy-and-run-the-application-locally"></a>Uygulamayı yerel olarak dağıtma ve çalıştırma
Artık devam edip uygulamayı çalıştırabilirsiniz. Hata ayıklama için uygulamayı dağıtmak üzere Visual Studio'da `F5` tuşuna basın. Daha önce Visual Studio'yu **yönetici** olarak açmadıysanız, `F5` başarısız olur.

> [!NOTE]
> Uygulamayı yerel olarak ilk kez çalıştırdığınızda ve dağıttığınızda, Visual Studio hata ayıklama için yerel bir küme oluşturur.  Küme oluşturma işlemi biraz zaman alabilir. Küme oluşturma durumu, Visual Studio çıkış penceresinde görüntülenir.

Bu noktada, web uygulamanız şöyle görünmelidir:

![ASP.NET Core ön ucu](./media/service-fabric-tutorial-create-dotnet-app/debug-front-end.png)

Uygulamada hata ayıklamasını durdurmak için, Visual Studio'ya dönün ve **Shift+F5** tuşlarına basın.

## <a name="add-a-stateful-back-end-service-to-your-application"></a>Uygulamanıza durum bilgisi olan bir arka uç hizmeti ekleme
Artık uygulamada bir ASP.NET Web API'si hizmeti çalıştırıldığına göre, devam edin ve uygulamadaki bazı verileri depolamak için durum bilgisi olan güvenilir bir hizmet ekleyin.

Service Fabric, güvenilir koleksiyonlar kullanarak verileri doğrudan hizmetinizin içinden tutarlı ve güvenilir bir şekilde depolamanıza olanak tanır. Güvenilir koleksiyonlar, C# koleksiyonlarını kullanmış olan herkesin iyi tanıdığı bir dizi yüksek kullanılabilirliğe sahip ve güvenilir koleksiyon sınıfıdır.

Bu öğreticide, güvenilir koleksiyonda sayaç değerini depolayan bir hizmet oluşturursunuz.

1. Çözüm Gezgini'nde, uygulama projesinin içindeki **Hizmetler**'e sağ tıklayın ve **Ekle > Yeni Service Fabric Hizmeti**'ni seçin.
    
2. **Yeni Service Fabric Hizmeti** iletişim kutusunda **Durum Bilgisi Olan ASP.NET Core**'u seçin, hizmete **VotingData** adını verin ve **Tamam**'a basın.

    ![Visual Studio'da yeni hizmet iletişim kutusu](./media/service-fabric-tutorial-create-dotnet-app/add-stateful-service.png)

    Hizmet projeniz oluşturulduktan sonra, uygulamanızda iki hizmet olacaktır. Uygulamanızı oluşturmaya devam ederken, aynı yöntemle başka hizmetler de ekleyebilirsiniz. Bunlardan her birinin bağımsız olarak sürümü oluşturulabilir ve yükseltilebilir.

3. Sonraki sayfada, bir dizi ASP.NET Core proje şablonu sağlanır. Bu öğreticide, **Web API'si** seçeneğini kullanın.

    ![ASP.NET proje türünü seçme](./media/service-fabric-tutorial-create-dotnet-app/vs-new-aspnet-project-dialog2.png)

    Visual Studio bir hizmet projesi oluşturur ve bunları Çözüm Gezgini'nde görüntüler.

    ![Çözüm Gezgini](./media/service-fabric-tutorial-create-dotnet-app/solution-explorer-aspnetcore-webapi-service.png)

### <a name="add-the-votedatacontrollercs-file"></a>VoteDataController.cs dosyasını ekleme

**VotingData** projesinde **Denetleyiciler**'e sağ tıklayın ve ardından **Ekle->Yeni öğe->Sınıf**'ı seçin. Dosyaya "VoteDataController.cs" adını verin ve **Ekle**'ye tıklayın. Dosyanın içeriğini aşağıdakilerle değiştirin, sonra değişikliklerinizi kaydedin.

```csharp
namespace VotingData.Controllers
{
    using System.Collections.Generic;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.ServiceFabric.Data;
    using Microsoft.ServiceFabric.Data.Collections;

    [Route("api/[controller]")]
    public class VoteDataController : Controller
    {
        private readonly IReliableStateManager stateManager;

        public VoteDataController(IReliableStateManager stateManager)
        {
            this.stateManager = stateManager;
        }

        // GET api/VoteData
        [HttpGet]
        public async Task<IActionResult> Get()
        {
            CancellationToken ct = new CancellationToken();

            IReliableDictionary<string, int> votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                Microsoft.ServiceFabric.Data.IAsyncEnumerable<KeyValuePair<string, int>> list = await votesDictionary.CreateEnumerableAsync(tx);

                Microsoft.ServiceFabric.Data.IAsyncEnumerator<KeyValuePair<string, int>> enumerator = list.GetAsyncEnumerator();

                List<KeyValuePair<string, int>> result = new List<KeyValuePair<string, int>>();

                while (await enumerator.MoveNextAsync(ct))
                {
                    result.Add(enumerator.Current);
                }

                return this.Json(result);
            }
        }

        // PUT api/VoteData/name
        [HttpPut("{name}")]
        public async Task<IActionResult> Put(string name)
        {
            IReliableDictionary<string, int> votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                await votesDictionary.AddOrUpdateAsync(tx, name, 1, (key, oldvalue) => oldvalue + 1);
                await tx.CommitAsync();
            }

            return new OkResult();
        }

        // DELETE api/VoteData/name
        [HttpDelete("{name}")]
        public async Task<IActionResult> Delete(string name)
        {
            IReliableDictionary<string, int> votesDictionary = await this.stateManager.GetOrAddAsync<IReliableDictionary<string, int>>("counts");

            using (ITransaction tx = this.stateManager.CreateTransaction())
            {
                if (await votesDictionary.ContainsKeyAsync(tx, name))
                {
                    await votesDictionary.TryRemoveAsync(tx, name);
                    await tx.CommitAsync();
                    return new OkResult();
                }
                else
                {
                    return new NotFoundResult();
                }
            }
        }
    }
}
```


## <a name="connect-the-services"></a>Hizmetleri bağlama
Bu sonraki adımda, iki hizmeti bağlayın ve ön uç Web uygulamasının arka uç hizmetinden oylama bilgilerini almasını ve ayarlamasını sağlayın.

Service Fabric, güvenilir hizmetlerle iletişiminize eksiksiz bir esneklik getirir. Tek bir uygulamanın içinde, TCP üzerinden erişilebilen hizmetleriniz olabilir. Diğer hizmetlere HTTP REST API üzerinden erişilebilir ve yine web yuvaları üzerinden erişilebilen başka hizmetler olabilir. Sağlanan seçeneklerin arka planı ve mevcut alışverişler için, bkz. [Hizmetlerle iletişim kurma](service-fabric-connect-and-communicate-with-services.md).

Bu öğreticide, [ASP.NET Core Web API'si](service-fabric-reliable-services-communication-aspnetcore.md) kullanın.

<a id="updatevotecontroller" name="updatevotecontroller_anchor"></a>

### <a name="update-the-votescontrollercs-file"></a>VotesController.cs dosyasını güncelleştirme
**VotingWeb** projesinde *Controllers/VotesController.cs* dosyasını açın.  `VotesController` sınıf tanımı içeriğini aşağıdakilerle değiştirin, sonra değişikliklerinizi kaydedin.

```csharp
public class VotesController : Controller
{
    private readonly HttpClient httpClient;
    private readonly FabricClient fabricClient;
    private readonly StatelessServiceContext serviceContext;

    public VotesController(HttpClient httpClient, StatelessServiceContext context, FabricClient fabricClient)
    {
        this.fabricClient = fabricClient;
        this.httpClient = httpClient;
        this.serviceContext = context;
    }

    // GET: api/Votes
    [HttpGet("")]
    public async Task<IActionResult> Get()
    {
        Uri serviceName = VotingWeb.GetVotingDataServiceName(this.serviceContext);
        Uri proxyAddress = this.GetProxyAddress(serviceName);

        ServicePartitionList partitions = await this.fabricClient.QueryManager.GetPartitionListAsync(serviceName);

        List<KeyValuePair<string, int>> result = new List<KeyValuePair<string, int>>();

        foreach (Partition partition in partitions)
        {
            string proxyUrl =
                $"{proxyAddress}/api/VoteData?PartitionKey={((Int64RangePartitionInformation) partition.PartitionInformation).LowKey}&PartitionKind=Int64Range";

            using (HttpResponseMessage response = await this.httpClient.GetAsync(proxyUrl))
            {
                if (response.StatusCode != System.Net.HttpStatusCode.OK)
                {
                    continue;
                }

                result.AddRange(JsonConvert.DeserializeObject<List<KeyValuePair<string, int>>>(await response.Content.ReadAsStringAsync()));
            }
        }

        return this.Json(result);
    }

    // PUT: api/Votes/name
    [HttpPut("{name}")]
    public async Task<IActionResult> Put(string name)
    {
        Uri serviceName = VotingWeb.GetVotingDataServiceName(this.serviceContext);
        Uri proxyAddress = this.GetProxyAddress(serviceName);
        long partitionKey = this.GetPartitionKey(name);
        string proxyUrl = $"{proxyAddress}/api/VoteData/{name}?PartitionKey={partitionKey}&PartitionKind=Int64Range";

        StringContent putContent = new StringContent($"{{ 'name' : '{name}' }}", Encoding.UTF8, "application/json");
        putContent.Headers.ContentType = new MediaTypeHeaderValue("application/json");

        using (HttpResponseMessage response = await this.httpClient.PutAsync(proxyUrl, putContent))
        {
            return new ContentResult()
            {
                StatusCode = (int) response.StatusCode,
                Content = await response.Content.ReadAsStringAsync()
            };
        }
    }

    // DELETE: api/Votes/name
    [HttpDelete("{name}")]
    public async Task<IActionResult> Delete(string name)
    {
        Uri serviceName = VotingWeb.GetVotingDataServiceName(this.serviceContext);
        Uri proxyAddress = this.GetProxyAddress(serviceName);
        long partitionKey = this.GetPartitionKey(name);
        string proxyUrl = $"{proxyAddress}/api/VoteData/{name}?PartitionKey={partitionKey}&PartitionKind=Int64Range";

        using (HttpResponseMessage response = await this.httpClient.DeleteAsync(proxyUrl))
        {
            if (response.StatusCode != System.Net.HttpStatusCode.OK)
            {
                return this.StatusCode((int) response.StatusCode);
            }
        }

        return new OkResult();
    }


    /// <summary>
    /// Constructs a reverse proxy URL for a given service.
    /// Example: http://localhost:19081/VotingApplication/VotingData/
    /// </summary>
    /// <param name="serviceName"></param>
    /// <returns></returns>
    private Uri GetProxyAddress(Uri serviceName)
    {
        return new Uri($"http://localhost:19081{serviceName.AbsolutePath}");
    }

    /// <summary>
    /// Creates a partition key from the given name.
    /// Uses the zero-based numeric position in the alphabet of the first letter of the name (0-25).
    /// </summary>
    /// <param name="name"></param>
    /// <returns></returns>
    private long GetPartitionKey(string name)
    {
        return Char.ToUpper(name.First()) - 'A';
    }
}
```
<a id="walkthrough" name="walkthrough_anchor"></a>

## <a name="walk-through-the-voting-sample-application"></a>Oylama örnek uygulamasında izlenecek yol
Oylama uygulaması iki hizmetten oluşur:
- Web ön uç hizmeti (VotingWeb)- Web sayfasına hizmet veren ve arka uç hizmetiyle iletişim için web API'lerini kullanıma sunan bir ASP.NET Core web ön uç hizmeti.
- Arka uç hizmeti (VotingData)- Oy sonuçlarını diskte kalıcı olan güvenilir bir sözlükte depolamak için API'yi kullanıma sunan bir ASP.NET Core web hizmeti.

![Uygulama Diyagramı](./media/service-fabric-tutorial-create-dotnet-app/application-diagram.png)

Uygulamada oy kullandığınızda aşağıdaki olaylar gerçekleşir:
1. Oy isteğini bir JavaScript HTTP PUT isteği olarak web ön uç hizmetindeki web API'sine gönderir.

2. Web ön uç hizmeti bir ara sunucu kullanarak HTTP PUT isteğini bulur ve arka uç hizmetine iletir.

3. Arka uç hizmeti gelen isteği alır ve güncelleştirilmiş sonucu güvenilir bir sözlükte depolar. Bu sözlük küme içinde birden çok düğüme çoğaltılır ve diskte kalıcı olur. Uygulamanın tüm verileri kümede depolandığından, veritabanına gerek yoktur.

## <a name="debug-in-visual-studio"></a>Visual Studio'da hata ayıklama
Visual Studio'da uygulamada hata ayıklaması yaparken yerel bir Service Fabric geliştirme kümesi kullanırsınız. Hata ayıklama deneyiminizi senaryonuza göre ayarlama seçeneğiniz vardır. Bu uygulamada, güvenilir bir sözlük kullanarak verileri arka uç hizmetinde depolayın. Hata ayıklayıcıyı durdurduğunuzda Visual Studio varsayılan olarak uygulamayı kaldırır. Uygulamanın kaldırılması arka uç hizmetindeki verilerin de kaldırılmasına neden olur. Hata ayıklama oturumları arasında verilerin kalıcı olmasını sağlamak için, Visual Studio'da **Oylama** projesindeki bir özellik olarak **Uygulama Hata Ayıklama Modu**'nu değiştirebilirsiniz.

Kodda neler olduğuna bakmak için aşağıdaki adımları tamamlayın:
1. **VotesController.cs** dosyasını açın ve web API'sinin **Put** yönteminde (63. satır) bir kesme noktası ayarlayın. Dosyayı Visual Studio'daki Çözüm Gezgini'nde arayıp bulabilirsiniz.

2. **VoteDataController.cs** dosyasını açın ve bu web API'sinin **Put** yönteminde (53. satır) bir kesme noktası ayarlayın.

3. Tarayıcıya dönün ve bir oylama seçeneğine tıklayın veya yeni oylama seçeneği ekleyin. Web ön ucunun api denetleyicisinde ilk kesme noktasına ulaşırsınız.
    
    1. Burası, tarayıcıda JavaScript'in ön uç hizmetindeki API denetleyicisine istek gönderdiği yerdir.
    
    ![Oy Ön Uç Hizmeti Ekleme](./media/service-fabric-tutorial-create-dotnet-app/addvote-frontend.png)

    2. İlk olarak, arka uç hizmeti için ReverseProxy'nin URL'sini oluşturun **(1)**.
    3. Ardından, HTTP PUT İsteğini ReverseProxy'ye gönderin **(2)**.
    4. Son olarak, yanıtı arka uç hizmetinden istemciye döndürün **(3)**.

4. Devam etmek için **F5** tuşuna basın
    1. Şimdi arka uç hizmetindeki kesme noktasındasınız.
    
    ![Oy Arka Uç Hizmeti Ekleme](./media/service-fabric-tutorial-create-dotnet-app/addvote-backend.png)

    2. Yöntemin ilk satırında **(1)** `counts` adlı güvenilir sözlüğü almak veya eklemek için `StateManager` kullanın.
    3. Güvenilir bir sözcükteki değerlerle tüm etkileşimler bir işlem gerektirir; bu using deyimi **(2)** o işlemi oluşturur.
    4. İşlemde, oylama seçeneği için uygun anahtarın değerini güncelleştirin ve işlemi yürütün **(3)**. Commit yöntemi döndüğünde, sözlükteki veriler güncelleştirilir ve kümedeki diğer düğümlere çoğaltılır. Artık veriler güvenli bir şekilde kümede depolanır ve arka uç hizmeti verilerin kullanılabilir olduğu diğer düğümlere yük devretebilir.
5. Devam etmek için **F5** tuşuna basın

Hata ayıklama oturumunu durdurmak için **Shift+F5** tuşlarına basın.


## <a name="next-steps"></a>Sonraki adımlar
Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * ASP.NET Core Web API'si hizmetini durum bilgisi olan güvenilir bir hizmet olarak oluşturma
> * ASP.NET Core Web Uygulaması hizmetini durum bilgisi olmayan bir web hizmeti olarak oluşturma
> * Durum bilgisi olan hizmetle iletişim kurmak için ters proxy kullanma

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Uygulamayı Azure’a dağıtma](service-fabric-tutorial-deploy-app-to-party-cluster.md)
