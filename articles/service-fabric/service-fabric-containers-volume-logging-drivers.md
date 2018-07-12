---
title: Service Fabric Azure dosyaları, birim sürücüsü (Önizleme) | Microsoft Docs
description: Service Fabric, Azure dosyaları için yedekleme birimleri, kapsayıcıdan kullanarak destekler. Bu, şu anda Önizleme aşamasındadır.
services: service-fabric
documentationcenter: other
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: other
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/10/2018
ms.author: subramar
ms.openlocfilehash: a5b75a7069375f503cbe25554eb7c04cba868413
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38969614"
---
# <a name="service-fabric-azure-files-volume-driver-preview"></a>Service Fabric Azure dosyaları, birim sürücüsü (Önizleme)
Azure dosyaları toplu eklentidir bir [Docker birim eklentisi](https://docs.docker.com/engine/extend/plugins_volume/) sağlayan [Azure dosyaları](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) birimleri Docker kapsayıcıları için temel. Bu Docker birim eklentisi, Service Fabric kümelerine dağıtılabilir bir Service Fabric uygulaması olarak paketlenir. Onun amacı, kümeye dağıtılan diğer Service Fabric kapsayıcı uygulamaları için birim tabanlı Azure dosyaları sağlamaktır.

> [!NOTE]
> Azure dosyaları toplu eklentisi 6.255.389.9494 sürümünü bu belgeyle kullanılabilir olan bir önizleme sürümüdür. Bir önizleme sürümü olduğu **değil** üretim ortamında kullanım için desteklenir.
>

## <a name="prerequisites"></a>Önkoşullar
* Azure dosyaları toplu eklentisi Windows sürümünü çalışır [Windows Server 1709 sürümü](https://docs.microsoft.com/windows-server/get-started/whats-new-in-windows-server-1709), [Windows 10 sürüm 1709](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1709) veya üzeri işletim sistemlerini yalnızca. Azure dosyaları toplu eklentisi Linux sürümü, Service Fabric tarafından desteklenen tüm işletim sistemi sürümlerinde çalışır.

* Azure dosyaları toplu eklenti yalnızca sürüm 6.2 ve yeni Service Fabric üzerinde çalışır.

* Bölümündeki yönergeleri [Azure dosyaları belgeleri](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share) birimi olarak kullanmak Service Fabric kapsayıcı uygulaması için bir dosya paylaşımı oluşturmak için.

* İhtiyacınız olacak [Powershell ile Service Fabric Modülü](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started) veya [SFCTL](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) yüklü.

## <a name="deploy-the-service-fabric-azure-files-application"></a>Service Fabric Azure dosyaları uygulamayı dağıtma

Birimleri sağlar, kapsayıcılar için Service Fabric uygulaması aşağıdaki indirilebilir [bağlantı](https://aka.ms/sfvolume). Uygulamayı kümeye dağıtılan [PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications), [CLI](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-lifecycle-sfctl) veya [FabricClient API'leri](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications-fabricclient).

1. Komut satırını kullanarak dizini indirilen uygulama paketi kök dizinine değiştirin.

    ```powershell
    cd .\AzureFilesVolume\
    ```

    ```bash
    cd ~/AzureFilesVolume
    ```

2. Uygulama paketini [Imagestoreconnectionstring] ve [ApplicationPackagePath] için uygun değeri ile aşağıdaki komutu çalıştırın görüntü deposuna kopyalayın:

    ```powershell
    Copy-ServiceFabricApplicationPackage -ApplicationPackagePath [ApplicationPackagePath] -ImageStoreConnectionString [ImageStoreConnectionString] -ApplicationPackagePathInImageStore AzureFilesVolumePlugin
    ```

    ```bash
    sfctl cluster select --endpoint https://testcluster.westus.cloudapp.azure.com:19080 --pem test.pem --no-verify
    sfctl application upload --path [ApplicationPackagePath] --show-progress
    ```

3. Uygulama türünü kaydedin

    ```powershell
    Register-ServiceFabricApplicationType -ApplicationPathInImageStore AzureFilesVolumePlugin
    ```

    ```bash
    sfctl application provision --application-type-build-path [ApplicationPackagePath]
    ```

4. Aşağıdaki uygulama oluşturmak, Not komutta uygulama oluşturma **ListenPort** uygulama parametresi. Bu değer, bu uygulama parametresi için belirtilen Azure dosyaları toplu eklentisi için Docker Daemon programını gelen istekleri dinlediği bağlantı noktasıdır. Uygulamaya verilen bağlantı noktasını kümenin veya uygulamalarınızı kullanın diğer bağlantı noktasıyla çakışmadığından emin olmak önemlidir.

    ```powershell
    New-ServiceFabricApplication -ApplicationName fabric:/AzureFilesVolumePluginApp -ApplicationTypeName AzureFilesVolumePluginType -ApplicationTypeVersion 6.255.389.9494 -ApplicationParameter @{ListenPort='19100'}
    ```

    ```bash
    sfctl application create --app-name fabric:/AzureFilesVolumePluginApp --app-type AzureFilesVolumePluginType --app-version 6.255.389.9494 --parameter '{"ListenPort":"19100"}'
    ```

> [!NOTE]

> Windows Server 2016 Datacenter eşleme SMB takar kapsayıcılarına desteklemez ([yalnızca Windows Server 1709 sürümü desteklenen](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/container-storage)). Bu sınırlama, ağ birimi eşlemenin ve Azure dosyaları birim sürücüsü 1709 ' daha eski sürümlerinde engeller.
>   

### <a name="deploy-the-application-on-a-local-development-cluster"></a>Bir yerel geliştirme kümesinde uygulamayı dağıtma
Azure dosyaları toplu eklentisi uygulama için varsayılan hizmet örnek sayısı, kümedeki her düğümde dağıtılan hizmetinin bir örneği olduğu anlamına gelir -1 ' dir. Ancak, bir yerel geliştirme kümesinde Azure dosyaları toplu eklentisi Uygulama dağıtırken, hizmeti örnek sayısını 1 belirtilmelidir. Bu, aracılığıyla yapılabilir **Instancecount** uygulama parametresi. Bu nedenle, bir yerel geliştirme kümesinde Azure dosyaları toplu eklentisi uygulama dağıtmak için komut şöyledir:

```powershell
New-ServiceFabricApplication -ApplicationName fabric:/AzureFilesVolumePluginApp -ApplicationTypeName AzureFilesVolumePluginType -ApplicationTypeVersion 6.255.389.9494 -ApplicationParameter @{ListenPort='19100';InstanceCount='1'}
```

```bash
sfctl application create --app-name fabric:/AzureFilesVolumePluginApp --app-type AzureFilesVolumePluginType --app-version 6.255.389.9494 --parameter '{"ListenPort": "19100","InstanceCount": "1"}'
```
## <a name="configure-your-applications-to-use-the-volume"></a>Uygulamalarınızı birimi kullanmak için yapılandırma
Aşağıdaki kod parçacığında, bir Azure dosyaları temel birim, uygulamanızın uygulama bildiriminde nasıl belirtilebilir gösterir. İlgilendiğiniz belirli bir öğedir **birim** etiketi:

```xml
?xml version="1.0" encoding="UTF-8"?>
<ApplicationManifest ApplicationTypeName="WinNodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <Description>Calculator Application</Description>
    <Parameters>
      <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
      <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
      <Parameter Name="MyStorageVar" DefaultValue="c:\tmp"></Parameter>
    </Parameters>
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeServicePackage" ServiceManifestVersion="1.0"/>
     <Policies>
       <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv">
            <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            <RepositoryCredentials PasswordEncrypted="false" Password="****" AccountName="test"/>
            <Volume Source="azfiles" Destination="c:\VolumeTest\Data" Driver="sfazurefile">
                <DriverOption Name="shareName" Value="" />
                <DriverOption Name="storageAccountName" Value="" />
                <DriverOption Name="storageAccountKey" Value="" />
            </Volume>
       </ContainerHostPolicies>
   </Policies>
    </ServiceManifestImport>
    <ServiceTemplates>
        <StatelessService ServiceTypeName="StatelessNodeService" InstanceCount="5">
            <SingletonPartition></SingletonPartition>
        </StatelessService>
    </ServiceTemplates>
</ApplicationManifest>
```

Azure dosyaları toplu eklentisi için sürücü adı **sfazurefile**. Bu değer için ayarlanmış **sürücü** özniteliği **birim** uygulama bildiriminde öğesi.

İçinde **birim** Yukarıdaki kod parçacığı, Azure dosyaları toplu eklentisi öğesinde aşağıdaki etiketlerin gerektirir:
- **Kaynak** -birim adıdır. Kullanıcı, birim için herhangi bir ad seçebilirsiniz.
- **Hedef** -bu etiketi içinde çalışmakta olan kapsayıcıyı birim eşlendiği konumdur. Bu nedenle, hedef kapsayıcı içinde zaten bir konuma olamaz

Gösterildiği **DriverOption** öğeleri parçacığında yukarıdaki, Azure dosyaları toplu eklentisi aşağıdaki sürücü seçenekleri destekler:

- **shareName** -birim için kapsayıcı sağlar. Azure dosyaları dosya paylaşımının adı
- **storageAccountName** - Name Azure dosyaları dosyasını içeren Azure depolama hesabını paylaşmak
- **storageAccountKey** -Azure dosyaları dosya paylaşımını içeren Azure depolama hesabı için erişim anahtarı

Gerekli olan tüm yukarıdaki sürücü seçenekleri.

## <a name="using-your-own-volume-or-logging-driver"></a>Kendi birim kullanarak veya sürücü günlüğe kaydetme
Service Fabric Ayrıca kendi özel kullanımı sağlayan [birim](https://docs.docker.com/engine/extend/plugins_volume/) veya [günlüğü](https://docs.docker.com/engine/admin/logging/overview/) sürücüleri. Küme üzerinde Docker birim/günlük sürücü yüklü değilse, bunu el ile RDP/SSH'yi protokolleri kullanarak yükleyebilirsiniz. Yükleme ile bu protokolleri aracılığıyla gerçekleştirebileceğiniz bir [sanal makine ölçek kümesinin başlangıç betiği](https://azure.microsoft.com/resources/templates/201-vmss-custom-script-windows/) veya bir [SetupEntryPoint betik](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-model#describe-a-service).

Yüklemek için komut dosyası örneği [Azure için Docker birim sürücüsü](https://docs.docker.com/docker-for-azure/persistent-data-volumes/) aşağıdaki gibidir:

```bash
docker plugin install --alias azure --grant-all-permissions docker4x/cloudstor:17.09.0-ce-azure1  \
    CLOUD_PLATFORM=AZURE \
    AZURE_STORAGE_ACCOUNT="[MY-STORAGE-ACCOUNT-NAME]" \
    AZURE_STORAGE_ACCOUNT_KEY="[MY-STORAGE-ACCOUNT-KEY]" \
    DEBUG=1
```

Uygulamalarınızda birim veya yüklediğiniz, günlüğe kaydetme sürücü kullanmak, uygun değerleri belirlemek zorunda **birim** ve **LogConfig** altındaki öğeleri  **Healthcheck** uygulama bildiriminizi de.

```xml
<ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv">
    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
    <RepositoryCredentials PasswordEncrypted="false" Password="****" AccountName="test"/>
    <LogConfig Driver="[YOUR_LOG_DRIVER]" >
        <DriverOption Name="test" Value="vale"/>
    </LogConfig>
    <Volume Source="c:\workspace" Destination="c:\testmountlocation1" IsReadOnly="false"></Volume>
    <Volume Source="[MyStorageVar]" Destination="c:\testmountlocation2" IsReadOnly="true"> </Volume>
    <Volume Source="myvolume1" Destination="c:\testmountlocation2" Driver="[YOUR_VOLUME_DRIVER]" IsReadOnly="true">
        <DriverOption Name="[name]" Value="[value]"/>
    </Volume>
</ContainerHostPolicies>
```

Bir birim eklenti belirtirken, Service Fabric belirtilen parametreleri kullanarak birimi otomatik olarak oluşturur. **Kaynak** etiketinde **birim** öğesi toplu adıdır ve **sürücü** eklenti birimin sürücü etiketi belirtir. **Hedef** etikettir konumu, **kaynak** içinde çalışmakta olan kapsayıcıyı eşlenir. Bu nedenle, hedef kapsayıcı içinde zaten bir konuma olamaz. Kullanılarak seçeneği belirtilebilir **DriverOption** aşağıda gösterildiği gibi etiketleyin:

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azure" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```

Önceki bildirim kod parçacığında gösterildiği gibi uygulama parametreler birimlerde desteklenir (Ara `MyStorageVar` bir örnek için kullanın).

Bir Docker günlük sürücü belirtilirse, günlükleri işlemek için agents'ı (veya kapsayıcıları) kümede dağıtmak zorunda. **DriverOption** etiketi, günlük sürücü seçeneklerini belirtmek için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
* Kapsayıcı örnekleri, birim sürücüsü dahil görmek için lütfen [Service Fabric kapsayıcı örnekleri](https://github.com/Azure-Samples/service-fabric-containers)
* Bir Service Fabric kümesine kapsayıcıları dağıtın, makaleyi başvurmak [Service fabric'te kapsayıcı dağıtma](service-fabric-deploy-container.md)
