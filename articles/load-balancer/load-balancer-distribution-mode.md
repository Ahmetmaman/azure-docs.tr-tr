---
title: Azure yük dengeleyici dağıtım modunu yapılandırma | Microsoft Docs
description: Kaynak IP benzeşimi desteklemek Azure Load Balancer için dağıtım modunu yapılandırma
services: load-balancer
documentationcenter: na
author: KumudD
manager: jpconnock
ms.assetid: 7df27a4d-67a8-47d6-b73e-32c0c6206e6e
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/01/2018
ms.author: kumud
ms.openlocfilehash: 776621f9ef95867c6e3c25dd11c656d451b6730e
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48018137"
---
# <a name="configure-the-distribution-mode-for-azure-load-balancer"></a>Azure Load Balancer için dağıtım modunu yapılandırma

## <a name="hash-based-distribution-mode"></a>Karma tabanlı bir dağıtım modu

Azure Load Balancer için varsayılan dağıtım modu bir 5 bölütlü karmadır. Kayıt düzeni, kaynak IP, kaynak bağlantı noktası, hedef IP, hedef bağlantı noktası ve protokol türü oluşur. Karma şekilde kullanılabilir sunuculara trafik eşlemek için kullanılır ve algoritma yalnızca bir aktarım oturumu içinde süreklilik sağlar. Aynı oturumda paketlerin yük dengeli uç nokta arkasındaki aynı veri merkezi IP (DIP) örneğine yönlendirilir. İstemci aynı kaynak IP, yeni bir oturum başlatıldığında, kaynak bağlantı noktası değiştirir ve farklı bir DIP uç noktasına gidin, trafik neden olur.

![5 bölütlü karma tabanlı bir dağıtım modu](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

## <a name="source-ip-affinity-mode"></a>Kaynak IP benzeşimi modu

Yük Dengeleyici kaynak IP benzeşimi dağıtım modunu kullanarak de yapılandırılabilir. Bu dağıtım olarak da bilinen oturum benzeşimi veya istemci IP benzeşimi modudur. 3'lü demet (kaynak IP, hedef IP ve protokol türü) karma şekilde kullanılabilir sunuculara trafik eşlemek için ya da 2-tanımlama grubu (kaynak IP ve hedef IP) modunu kullanır. Kaynak IP benzeşimi kullanarak, aynı istemci bilgisayardan başlatılır bağlantıları aynı DIP uç noktasına gidin.

Aşağıdaki şekilde, 2 bölütlü yapılandırma gösterilmektedir. 2 bölütlü yük dengeleyiciden sanal makineye 1 (VM1) nasıl çalıştığını dikkat edin. VM1, ardından VM2 ve VM3 tarafından yedeklenir.

![2 bölütlü oturum benzeşimi dağıtım modu](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

Kaynak IP benzeşimi modu, Azure Load Balancer ve Uzak Masaüstü Ağ Geçidi (RD Ağ Geçidi) arasında bir uyumsuzluk çözer. Bu mod kullanarak, bir tek bulut hizmetinde bir RD Ağ Geçidi grubu oluşturabilirsiniz.

Başka bir kullanım örneği senaryosu medya karşıya yükleme ' dir. UDP karşıya veri yükleme olur, ancak denetim düzlemi TCP elde edilir:

* Bir istemci, yük dengeli genel adresine TCP oturumu başlatır ve belirli bir DÜŞÜŞ yönlendirilir. Kanal, bağlantı durumunu izlemek için etkin kalır.
* Aynı yük dengeli genel uç noktası için yeni bir UDP oturumu aynı istemci bilgisayardan başlatılır. Bağlantı, önceki TCP bağlantı olarak aynı DIP uç noktasına yönlendirilir. Medya karşıya yükleme, bir denetim kanalı üzerinden TCP korurken yüksek aktarım hızını çalıştırılabilir.

> [!NOTE]
> Yük dengeli bir kümedeki bir sanal makineye ekleyerek veya çıkararak değiştiğinde, istemci istekleri dağıtımını değeri yeniden hesaplanır. Aynı sunucuda sonuna var olan istemcilerden gelen yeni bağlantıları bağlı olamaz. Ayrıca, kaynak IP benzeşimi dağıtım modunu trafik eşit olmayan bir dağıtım neden olabilir. Proxy çalıştıran istemciler bir benzersiz istemci uygulaması olarak görülebilir.

## <a name="configure-source-ip-affinity-settings"></a>Kaynak IP benzeşimi ayarlarını yapılandırma

Resource Manager ile dağıtılan sanal makineler için yük dengeleyicinin Yük Dengeleme kuralı üzerinde yük dengeleyici dağıtım ayarlarını değiştirmek için PowerShell kullanın.  Bu, mevcut bir yük dengeleyici kuralı dağıtım modunu güncelleştirir:

```powershell 
$lb = Get-AzureRmLoadBalancer -Name MyLb -ResourceGroupName MyLbRg 
$lb.LoadBalancingRules[0].LoadDistribution = 'sourceIp' 
Set-AzureRmLoadBalancer -LoadBalancer $lb 
``` 

Klasik sanal makineler için dağıtım ayarlarını değiştirmek için Azure PowerShell kullanırsınız. Bir sanal makineye Azure uç nokta ekleyin ve yük dengeleyici dağıtım modunu yapılandırın:

```powershell
Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM
```

Değerini `LoadBalancerDistribution` istenen miktarda Yük Dengeleme için öğesi. 2-demet (kaynak IP ve hedef IP) Yük Dengeleme için Sourceıp belirtin. 3'lü demet (kaynak IP, hedef IP ve protokol türü) Yük Dengeleme sourceIPProtocol belirtin. Hiçbiri için 5 tanımlama grubu Yük Dengeleme varsayılan davranışını belirtin.

Bir uç nokta yük dengeleyici dağıtım modu yapılandırma, bu ayarları kullanarak alın:

    PS C:\> Get-AzureVM –ServiceName MyService –Name MyVM | Get-AzureEndpoint

    VERBOSE: 6:43:50 PM - Completed Operation: Get Deployment
    LBSetName : MyLoadBalancedSet
    LocalPort : 80
    Name : HTTP
    Port : 80
    Protocol : tcp
    Vip : 65.52.xxx.xxx
    ProbePath :
    ProbePort : 80
    ProbeProtocol : tcp
    ProbeIntervalInSeconds : 15
    ProbeTimeoutInSeconds : 31
    EnableDirectServerReturn : False
    Acl : {}
    InternalLoadBalancerName :
    IdleTimeoutInMinutes : 15
    LoadBalancerDistribution : sourceIP

Zaman `LoadBalancerDistribution` öğesi yoksa, Azure Load Balancer varsayılan 5-tuple algoritması kullanır.

### <a name="configure-distribution-mode-on-load-balanced-endpoint-set"></a>Yük dengeli uç nokta kümesine dağıtım modunu yapılandırma

Uç noktaları yük dengeli uç nokta kümesinin bir parçası olduğunda dağıtım modunu yük dengeli uç nokta kümesinde yapılandırılması gerekir:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP
```

### <a name="configure-distribution-mode-for-cloud-services-endpoints"></a>Bulut Hizmetleri uç noktaları için dağıtım modunu yapılandırma

Bulut hizmetinizi güncelleştirme için .NET 2.5 için Azure SDK'sını kullanın. Bulut Hizmetleri için uç nokta ayarları .csdef dosyasında yapılır. Bulut Hizmetleri dağıtımı için yük dengeleyici dağıtım modu güncelleştirmek için bir dağıtım yükseltmesi gerekiyor.

.Csdef değişikliklerinin uç noktası için ayarların bir örnek aşağıda verilmiştir:

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancerDistribution="sourceIP" />
    </Endpoints>
</WorkerRole>
<NetworkConfiguration>
    <VirtualNetworkSite name="VNet"/>
    <AddressAssignments>
<InstanceAddress roleName="VMRolePersisted">
    <PublicIPs>
    <PublicIP name="public-ip-name" idleTimeoutInMinutes="timeout-in-minutes"/>
    </PublicIPs>
</InstanceAddress>
    </AddressAssignments>
</NetworkConfiguration>
```

## <a name="api-example"></a>API örneği

Aşağıdaki örnek, bir dağıtımda, belirtilen bir yük dengeli küme için yük dengeleyici dağıtım modunu yapılandırmak gösterilmektedir. 

### <a name="change-distribution-mode-for-deployed-load-balanced-set"></a>Dağıtılmış yük dengeli küme için dağıtım modunu değiştirme

Azure Klasik dağıtım modeli, var olan bir dağıtım yapılandırmasını değiştirmek için kullanın. Ekleme `x-ms-version` başlığı ve sürümü 2014-09-01 değeri ayarlayın veya üzeri.

#### <a name="request"></a>İstek

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>?comp=UpdateLbSet   x-ms-version: 2014-09-01
    Content-Type: application/xml

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName> endpoint-set-name </LoadBalancedEndpointSetName>
        <LocalPort> local-port-number </LocalPort>
        <Port> external-port-number </Port>
        <LoadBalancerProbe>
          <Port> port-assigned-to-probe </Port>
          <Protocol> probe-protocol </Protocol>
          <IntervalInSeconds> interval-of-probe </IntervalInSeconds>
          <TimeoutInSeconds> timeout-for-probe </TimeoutInSeconds>
        </LoadBalancerProbe>
        <Protocol> endpoint-protocol </Protocol>
        <EnableDirectServerReturn> enable-direct-server-return </EnableDirectServerReturn>
        <IdleTimeoutInMinutes>idle-time-out</IdleTimeoutInMinutes>
        <LoadBalancerDistribution>sourceIP</LoadBalancerDistribution>
      </InputEndpoint>
    </LoadBalancedEndpointList>

Daha önce açıklandığı gibi belirlenen `LoadBalancerDistribution` Sourceıp 2 bölütlü benzeşimi, 3'lü demet benzeşimi sourceIPProtocol ya da hiçbir benzeşim (5-tuple benzeşim) için yok için öğesi.

#### <a name="response"></a>Yanıt

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a>Sonraki adımlar

* [Azure iç yük dengeleyiciye genel bakış](load-balancer-internal-overview.md)
* [İnternet'e yönelik Yük Dengeleyici yapılandırmaya başlayın](load-balancer-get-started-internet-arm-ps.md)
* [Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)
