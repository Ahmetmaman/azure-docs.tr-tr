---
title: Linux 'ta ilk Azure Service Fabric uygulamanızı kullanarak oluşturmaC#
description: Ve .NET Core 2,0 kullanarak C# bir Service Fabric uygulaması oluşturmayı ve dağıtmayı öğrenin.
ms.topic: conceptual
ms.date: 04/11/2018
ms.openlocfilehash: 202dde933b41a98c1c119f422d47cbdbb0be84ae
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75458131"
---
# <a name="create-your-first-azure-service-fabric-application"></a>İlk Azure Service Fabric uygulamanızı oluşturma
> [!div class="op_single_selector"]
> * [Java - Linux (Önizleme)](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# - Linux (Önizleme)](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

Service Fabric, Linux üzerinde hem .NET Core hem de Java dillerinde hizmet oluşturmaya yönelik SDK’lar sağlar. Bu öğreticide, .NET Core 2.0 üzerinde C# kullanarak Linux için bir uygulama ve hizmet oluşturmayı öğreneceğiz.

## <a name="prerequisites"></a>Ön koşullar
Başlamadan önce [Linux geliştirme ortamınızı ayarladığınızdan](service-fabric-get-started-linux.md) emin olun. Mac OS X kullanıyorsanız, [Vagrant kullanarak bir sanal makinede Linux one-box ortamı ayarlayabilirsiniz](service-fabric-get-started-mac.md).

[Service Fabric CLI](service-fabric-cli.md)’yı de yüklemeniz gerekir.

### <a name="install-and-set-up-the-generators-for-c"></a>C# için oluşturucuları yükleme ve ayarlama
Service Fabric, Yeoman şablon oluşturucuları kullanarak terminalden Service Fabric uygulamaları oluşturmanıza yardımcı olan yapı iskelesi araçları sağlar. C# için Service Fabric Yeoman şablon oluşturucularını ayarlama amacıyla bu adımları izleyin:

1. Makinenize nodejs ve NPM yükleme

   ```bash
   curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.0/install.sh | bash 
   nvm install node 
   ```
2. NPM’den makinenize [Yeoman](https://yeoman.io/) şablon oluşturucuyu yükleme

   ```bash
   npm install -g yo
   ```
3. NPM'den Service Fabric Yeoman C# uygulama oluşturucuyu yükleme

   ```bash
   npm install -g generator-azuresfcsharp
   ```

## <a name="create-the-application"></a>Uygulama oluşturma
Service Fabric uygulaması bir veya birden çok hizmet içerebilir. Bu hizmetlerin her biri uygulamanın işlevselliğini aktarma konusunda belirli bir role sahiptir. Son adımda yüklediğiniz C# için Service Fabric [Yeoman](https://yeoman.io/) oluşturucu, ilk hizmetinizi oluşturmanızı ve daha sonra yeni hizmetler eklemenizi kolaylaştırır. Tek bir hizmetle uygulama oluşturmak için Yeoman’ı kullanalım.

1. Bir terminalde iskele oluşturmaya başlamak için aşağıdaki komutu yazın:`yo azuresfcsharp`
2. Uygulamanızı adlandırın.
3. Birinci hizmetinizin türünü seçin ve adlandırın. Bu öğreticinin amaçları doğrultusunda, Reliable Actor Hizmetini seçiyoruz.

   ![C# için Service Fabric Yeoman oluşturucusu][sf-yeoman]

> [!NOTE]
> Seçenekler hakkında daha fazla bilgi için bkz. [Service Fabric programlama modeline genel bakış](service-fabric-choose-framework.md).
>
>

## <a name="build-the-application"></a>Uygulama oluşturma
Service Fabric Yeoman şablonları, uygulamayı terminalden oluşturmak (uygulama klasörüne gittikten sonra) için kullanabileceğiniz bir yapı betiği içerir.

  ```sh
 cd myapp
 ./build.sh
  ```

## <a name="deploy-the-application"></a>Uygulamayı dağıtma

Uygulama oluşturulduktan sonra uygulamayı yerel kümeye dağıtabilirsiniz.

1. Yerel Service Fabric kümesine bağlanın.

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. Uygulama paketini kümenin görüntü deposuna kopyalamak, uygulama türünü kaydetmek ve uygulamanın bir örneğini oluşturmak için şablonda verilen yükleme betiğini çalıştırın.

    ```bash
    ./install.sh
    ```

Oluşturulan uygulamayı dağıtma işlemi, diğer tüm Service Fabric uygulamalarında olduğu gibidir. Ayrıntılı yönergeler için [Service Fabric uygulamasını Service Fabric CLI ile yönetme](service-fabric-application-lifecycle-sfctl.md) ile ilgili belgelere bakın.

Bu komutların parametreleri, uygulama paketi içinde oluşturulmuş bildirimlerde bulunabilir.

Uygulama dağıtıldığında bir tarayıcı açın ve [http://localhost:19080/Explorer](http://localhost:19080/Explorer) konumundaki [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)'a gidin. Ardından, **Uygulamalar** düğümünü genişletin ve geçerli olarak uygulamanızın türü için bir giriş ve bu türün ilk örneği için başka bir giriş olduğuna dikkat edin.

> [!IMPORTANT]
> Uygulamayı Azure 'da güvenli bir Linux kümesine dağıtmak için, Service Fabric çalışma zamanı ile uygulamanızı doğrulamak üzere bir sertifika yapılandırmanız gerekir. Bunun yapılması, Reliable Services hizmetlerinizin temel alınan Service Fabric çalışma zamanı API 'Leriyle iletişim kurmasını sağlar. Daha fazla bilgi edinmek için bkz. [Reliable Services uygulamasını Linux kümelerinde çalışacak şekilde yapılandırma](./service-fabric-configure-certificates-linux.md#configure-a-reliable-services-app-to-run-on-linux-clusters).  
>

## <a name="start-the-test-client-and-perform-a-failover"></a>Test istemcisini başlatma ve yük devre gerçekleştirme
Actor projeleri kendi başına bir işlem yapamaz. Bunlar başka bir hizmet veya istemcinin kendilerine iletiler göndermesini gerektirir. Actor şablonu, actor hizmetiyle etkileşim kurmak üzere kullanabileceğiniz basit bir test betiği içerir.

1. Actor hizmetinin çıktısını görmek için izleme yardımcı programını kullanarak betiği çalıştırın.

   MAC OS X söz konusu olduğunda, aşağıdaki ek komutları çalıştırarak myactorsvcTestClient klasörünü kapsayıcının içindeki aynı konuma kopyalamanız gerekir.
    
    ```bash
    docker cp  [first-four-digits-of-container-ID]:/home
    docker exec -it [first-four-digits-of-container-ID] /bin/bash
    cd /home
    ```
    
    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```
2. Service Fabric Explorer’da actor hizmetinin birincil çoğaltmasını barındıran düğümü bulun. Aşağıdaki ekran görüntüsünde düğüm 3’tür.

    ![Service Fabric Explorer’da birincil çoğaltmayı bulma][sfx-primary]
3. Önceki adımda bulduğunuz düğüme tıklayın, ardından Eylemler menüsünden **Devre dışı bırak (yeniden başlat)** öğesini seçin. Bu eylem, yerel kümenizdeki bir düğümü yeniden başlatır. Böylece başka bir düğümde çalışan ikincil bir çoğaltmaya yük devretmesi için zorlanır. Bu eylemi gerçekleştirirken, test istemcisinden gelen çıkışa dikkat edin ve sayacın yük devretmeye rağmen artmaya devam ettiğini unutmayın.

## <a name="adding-more-services-to-an-existing-application"></a>Mevcut bir uygulamaya daha fazla hizmet ekleme

`yo` kullanılarak oluşturulmuş bir uygulamaya başka bir hizmet eklemek için aşağıdaki adımları uygulayın:
1. Dizini mevcut uygulamanın kök dizinine değiştirin.  Örneğin Yeoman tarafından oluşturulan uygulama `MyApplication` ise `cd ~/YeomanSamples/MyApplication` olacaktır.
2. `yo azuresfcsharp:AddService` öğesini çalıştırın

## <a name="next-steps"></a>Sonraki adımlar

* [Service Fabric CLI’sını kullanarak Service Fabric kümeleriyle etkileşim kurma](service-fabric-cli.md)
* [Service Fabric destek seçenekleri](service-fabric-support.md) hakkında bilgi edinin
* [Service Fabric CLI ile çalışmaya başlama](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png
