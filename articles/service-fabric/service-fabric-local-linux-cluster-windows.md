---
title: Windows 'da Azure Service Fabric Linux kümesi ayarlama
description: Bu makalede, Windows geliştirme makinelerinde çalışan Linux kümelerinin Service Fabric nasıl ayarlanacağı ele alınmaktadır. Bu, özellikle platformlar arası geliştirme için yararlıdır.
ms.topic: conceptual
ms.date: 11/20/2017
ms.openlocfilehash: 83d494d777a4a1e1586707c8848056ca8fe9780a
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91537081"
---
# <a name="set-up-a-linux-service-fabric-cluster-on-your-windows-developer-machine"></a>Windows Geliştirici makinenizde Linux Service Fabric kümesi ayarlama

Bu belgede, Windows geliştirme makinelerinde yerel bir Linux Service Fabric ayarlama ele alınmaktadır. Yerel bir Linux kümesi ayarlamak, Linux kümelerine hedeflenmiş ancak bir Windows makinesinde geliştirilen uygulamaları hızlı bir şekilde test etmek için kullanışlıdır.

## <a name="prerequisites"></a>Önkoşullar
Linux tabanlı Service Fabric kümeleri Windows üzerinde yerel olarak çalışmaz. Yerel bir Service Fabric kümesini çalıştırmak için önceden yapılandırılmış bir Docker kapsayıcı görüntüsü sağlanır. Başlamadan önce şunlar gereklidir:

* En az 4 GB RAM
* [Docker](https://store.docker.com/editions/community/docker-ce-desktop-windows)'ın en son sürümü
* Docker, Linux modunda çalışıyor olmalıdır

>[!TIP]
> * Windows 'a Docker yüklemek için resmi Docker [belgelerinde](https://store.docker.com/editions/community/docker-ce-desktop-windows/plans/docker-ce-desktop-windows-tier?tab=instructions) bahsedilen adımları izleyebilirsiniz. 
> * Yüklemeyi tamamladıktan sonra, [burada](https://docs.docker.com/docker-for-windows/#check-versions-of-docker-engine-compose-and-machine) anlatılan adımları izleyerek yüklemeyi doğru yaptığınızı onaylayın


## <a name="create-a-local-container-and-setup-service-fabric"></a>Yerel bir kapsayıcı oluşturma ve Service Fabric’i ayarlama
Yerel bir Docker kapsayıcısı ayarlamak ve üzerinde bir Service Fabric kümesi çalıştırmak için PowerShell 'de aşağıdaki adımları gerçekleştirin:


1. Ana bilgisayarınızda Docker daemon yapılandırmasını aşağıdakiyle güncelleştirin ve Docker daemon programını yeniden başlatın: 

    ```json
    {
      "ipv6": true,
      "fixed-cidr-v6": "2001:db8:1::/64"
    }
    ```
    Güncelleştirme için tavsiye edilen yöntem, Docker simgesine > ayarlar > Daemon > Gelişmiş ' e gidin ve orada güncelleştirin. Sonra, değişikliklerin etkili olması için Docker Daemon programını yeniden başlatın. 

2. Service Fabric Görüntünüzü derlemek için yeni bir dizinde `Dockerfile` adlı bir dosya oluşturun:

    ```Dockerfile
    FROM mcr.microsoft.com/service-fabric/onebox:latest
    WORKDIR /home/ClusterDeployer
    RUN ./setup.sh
    #Generate the local
    RUN locale-gen en_US.UTF-8
    #Set environment variables
    ENV LANG=en_US.UTF-8
    ENV LANGUAGE=en_US:en
    ENV LC_ALL=en_US.UTF-8
    EXPOSE 19080 19000 80 443
    #Start SSH before running the cluster
    CMD /etc/init.d/ssh start && ./run.sh
    ```

    >[!NOTE]
    >Kapsayıcınıza ek programlar veya bağımlılıklar eklemek için bu dosyayı uyarlayabilirsiniz.
    >Örneğin, `RUN apt-get install nodejs -y` komutu eklendiğinde, konuk yürütülebilir dosyaları olarak `nodejs` uygulamaları için destek sağlanır.
    
    >[!TIP]
    > Varsayılan olarak bu, görüntüyü Service Fabric’in en son sürümüyle çeker. Belirli düzeltmeler için lütfen [Docker Hub](https://hub.docker.com/r/microsoft/service-fabric-onebox/) sayfasını ziyaret edin

3. `Dockerfile` içinden yeniden kullanılabilir görüntünüzü derlemek için bir terminal açın ve doğrudan `Dockerfile` öğesini tutarak `cd` uygulayıp sonra şunu çalıştırın:

    ```powershell 
    docker build -t mysfcluster .
    ```
    
    >[!NOTE]
    >Bu işlem biraz zaman alır, ancak yalnızca bir kez gereklidir.

4. Şimdi aşağıdakini çalıştırarak her ihtiyaç duyduğunuzda hızla yerel bir Service Fabric kopyası başlatabilirsiniz:

    ```powershell 
    docker run --name sftestcluster -d -v //var/run/docker.sock:/var/run/docker.sock -p 19080:19080 -p 19000:19000 -p 25100-25200:25100-25200 mysfcluster
    ```

    >[!TIP]
    >Kapsayıcı örneğiniz için bir ad belirterek, örneğinizin daha okunaklı bir biçimde işlenebilmesini sağlayın. 
    >
    >Uygulamanız belirli bağlantı noktalarını dinliyorsa, bağlantı noktaları ek `-p` etiketleri kullanılarak belirtilmelidir. Örneğin, uygulamanız 8080 bağlantı noktasını dinliyorsa, şuradaki `-p` etiketini ekleyin:
    >
    >`docker run -itd -p 19080:19080 -p 8080:8080 --name sfonebox mcr.microsoft.com/service-fabric/onebox:latest`
    >

5. Kümenin başlatılması kısa bir süre sürer, aşağıdaki komutu kullanarak günlükleri görüntüleyebilir veya `http://localhost:19080` küme durumunu görüntülemek için panoya atlayabilirsiniz:

    ```powershell 
    docker logs sftestcluster
    ```

6. 5. adım başarıyla tamamlandıktan sonra, Windows 'tan bölümüne giderek ``http://localhost:19080`` Service Fabric Gezginini görebilirsiniz. Bu noktada, Windows Geliştirici makinenizden herhangi bir aracı kullanarak bu kümeye bağlanabilir ve Linux Service Fabric kümelerine hedeflenmiş uygulamayı dağıtabilirsiniz. 

    > [!NOTE]
    > Eclipse eklentisi Windows üzerinde şu anda desteklenmemektedir. 

7. İşiniz bittiğinde kapsayıcıyı şu komutla durdurun ve temizleyin:

    ```powershell 
    docker rm -f sftestcluster
    ```

### <a name="known-limitations"></a>Bilinen Sınırlamalar 
 
 Mac’e yönelik kapsayıcıdaki yerel küme çalıştırmaya ilişkin bilinen sınırlandırmalar aşağıda verilmiştir: 
 
 * DNS hizmeti çalışmıyor ve desteklenmiyor [Sorun No. 132](https://github.com/Microsoft/service-fabric/issues/132)

## <a name="next-steps"></a>Sonraki adımlar
* [Tutulma](./service-fabric-get-started-eclipse.md) ile çalışmaya başlama
* Diğer [Java örneklerine](https://github.com/Azure-Samples/service-fabric-java-getting-started) göz atın


<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
