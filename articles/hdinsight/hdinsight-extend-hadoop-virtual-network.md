---
title: Sanal ağ - Azure HDInsight'ı genişletin
description: HDInsight, diğer bulut kaynaklarını veya, veri merkezinizdeki kaynaklarına bağlanmak için Azure sanal ağ kullanmayı öğrenin
services: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/08/2018
ms.openlocfilehash: 724d3d7fe8ff037b82bbce797e391c21060aa53d
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48870970"
---
# <a name="extend-azure-hdinsight-using-an-azure-virtual-network"></a>Azure HDInsight'ın bir Azure sanal ağı kullanarak genişletme

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

HDInsight ile kullanmayı öğrenin bir [Azure sanal ağı](../virtual-network/virtual-networks-overview.md). Bir Azure sanal ağı kullanarak aşağıdaki senaryolar sağlar:

* HDInsight için bir şirket içi ağdan doğrudan bağlanma.

* HDInsight verilerine bağlantı kurma, bir Azure sanal ağında depolar.

* Doğrudan genel internet üzerinden kullanılabilir olmayan Hadoop hizmetlerine erişme. Örneğin, Kafka API'lerin veya HBase Java API'si.

> [!WARNING]
> Bu belgedeki bilgiler, TCP/IP ağ'ın bilinmesini gerektirir. TCP/IP ağ bağlantısı ile ilgili bilgi sahibi değilseniz, üretim ağları için değişiklik yapmadan önce olan bir kullanıcıyla ortak.

> [!IMPORTANT]
> HDInsight için şirket içi bağlanma konusunda adım adım yönergeler arıyorsanız, Azure sanal ağı kullanarak ağ, bkz [HDInsight'ı şirket içi ağınıza bağlama](connect-on-premises-network.md) belge.

## <a name="planning"></a>Planlama

Bir sanal ağda HDInsight'ı yüklemek planlama yaparken yanıt sorular şunlardır:

* Mevcut bir sanal ağa HDInsight'ı yüklemek ihtiyacınız var? Veya yeni bir ağ oluşturuyorsunuz?

    Bir sanal ağınız kullanıyorsanız, HDInsight'ı yüklemeden önce ağ yapılandırmasını değiştirmeniz gerekebilir. Daha fazla bilgi için [HDInsight mevcut bir sanal ağa ekleme](#existingvnet) bölümü.

* Başka bir sanal ağ veya şirket içi ağınıza HDInsight içeren sanal ağa bağlanmak istiyor musunuz?

    Kolayca ağlarda kaynak ile çalışmak için özel bir DNS oluşturun ve DNS iletme'yi yapılandırma gerekebilir. Daha fazla bilgi için [birden çok ağları bağlama](#multinet) bölümü.

* HDInsight için gelen ve giden trafiği kısıtlama/yeniden yönlendirme istiyor musunuz?

    HDInsight Azure veri merkezindeki belirli IP adresleri ile iletişim sınırsız gerekir. İstemci iletişimi için güvenlik duvarlarından izin verilmelidir çeşitli bağlantı noktaları vardır. Daha fazla bilgi için [ağ trafiğini denetleme](#networktraffic) bölümü.

## <a id="existingvnet"></a>HDInsight, mevcut bir sanal ağa ekleme

Nasıl yeni bir HDInsight mevcut bir Azure sanal ağına eklemek keşfetmek için bu bölümdeki adımları kullanın.

> [!NOTE]
> Bir sanal ağa var olan bir HDInsight kümesine eklenemiyor.

1. Sanal ağ için bir Klasik veya Resource Manager dağıtım modelini kullanıyor musunuz?

    HDInsight 3.4 ve büyük bir Resource Manager sanal ağı gerekir. HDInsight'ın önceki sürümlerinde, bir Klasik sanal ağ gereklidir.

    Ardından mevcut ağınıza klasik bir sanal ağ, bir Resource Manager sanal ağı oluşturmak ve sonra iki bağlanma gerekir. [Klasik sanal ağları yeni sanal ağlara bağlama](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

    Birleştirilmiş sonra Resource Manager ağında yüklü HDInsight Klasik ağ içindeki kaynaklarla etkileşim kurabilir.

2. Zorlamalı tünel kullanıyor musunuz? Zorlamalı tünel, giden Internet trafiği İnceleme için bir cihaz için zorlayan bir alt ağ ayarı ve günlük kaydı değildir. HDInsight, zorlamalı tünel desteklemez. Zorlamalı tünel bir alt ağa HDInsight'ı yüklemeden önce kaldırın veya HDInsight için yeni bir alt ağ oluşturun.

3. İçine veya dışına sanal ağ trafiği kısıtlamak için ağ güvenlik grupları, kullanıcı tanımlı yollar ve ağ sanal Gereçleri kullanıyorsunuz?

    Yönetilen bir hizmet olarak HDInsight, Azure veri merkezinde birden fazla IP adresi için sınırsız erişim gerektirir. Bu IP adresleri ile iletişime izin vermek için herhangi bir mevcut ağ güvenlik grupları veya kullanıcı tanımlı yollar güncelleştirin.

    HDInsight, çeşitli bağlantı noktaları kullanan birden çok hizmetleri barındırır. Bu bağlantı noktaları için trafiği engellemediğinizden. Sanal gereç güvenlik duvarlarından izin vermek için bağlantı noktalarının listesi için bkz. [güvenlik](#security) bölümü.

    Mevcut güvenlik yapılandırmanızı bulmak için aşağıdaki Azure PowerShell veya Azure Klasik CLI komutları kullanın:

    * Ağ güvenlik grupları

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
        get-azurermnetworksecuritygroup -resourcegroupname $resourceGroupName
        ```

        ```azurecli-interactive
        read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
        az network nsg list --resource-group $RESOURCEGROUP
        ```

        Daha fazla bilgi için [ağ güvenlik gruplarında sorun giderme](../virtual-network/diagnose-network-traffic-filter-problem.md) belge.

        > [!IMPORTANT]
        > Ağ güvenlik grubu kuralları kural önceliği temelinde sırayla uygulanır. Trafik deseni ile eşleşen ilk kural uygulanır ve hiçbir diğerleri için o trafiğe uygulanır. Sipariş en esnek en katı kuralları. Daha fazla bilgi için [ağ güvenlik grupları ile ağ trafiğini filtreleme](../virtual-network/security-overview.md) belge.

    * Kullanıcı tanımlı yollar

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
        get-azurermroutetable -resourcegroupname $resourceGroupName
        ```

        ```azurecli-interactive
        read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
        az network route-table list --resource-group $RESOURCEGROUP
        ```

        Daha fazla bilgi için [yollarla ilgili sorunları giderme](../virtual-network/diagnose-network-routing-problem.md) belge.

4. Bir HDInsight kümesi oluşturma ve yapılandırma sırasında Azure sanal ağı seçin. Adımları, küme oluşturma işlemi anlamak için aşağıdaki belgeleri kullanın:

    * [Azure portalını kullanarak HDInsight oluşturma](hdinsight-hadoop-create-linux-clusters-portal.md)
    * [Azure PowerShell kullanarak HDInsight oluşturma](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
    * [Klasik Azure CLI kullanarak HDInsight oluşturma](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
    * [Bir Azure Resource Manager şablonu kullanarak HDInsight oluşturma](hdinsight-hadoop-create-linux-clusters-arm-templates.md)

  > [!IMPORTANT]
  > HDInsight'ı bir sanal ağa ekleyerek bir isteğe bağlı yapılandırma adımdır. Küme yapılandırma sırasında sanal ağ'ı seçtiğinizden emin olun.

## <a id="multinet"></a>Birden çok ağları bağlama

En büyük güçlük birden çok ağ yapılandırmasına sahip ağlar arasında ad çözümlemesine ' dir.

Azure sanal ağında yüklü olan Azure Hizmetleri için ad çözümleme sağlar. HDInsight, tam etki alanı adını (FQDN) kullanarak aşağıdaki kaynaklara bağlanmak bu yerleşik ad çözümlemesi sağlar:

* İnternet üzerinde kullanılabilir olan herhangi bir kaynaktır. Örneğin, microsoft.com, google.com.

* Aynı Azure sanal ağında, kullanarak herhangi bir kaynağa __iç DNS ad__ kaynak. Örneğin, varsayılan ad çözümlemesi kullanırken, örnek HDInsight çalışan düğümlerine atanan iç DNS adları şunlardır:

    * wn0-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net
    * wn2-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net

    Hem bu düğümler doğrudan birbirine ve HDInsight, diğer düğümleri iç DNS adları kullanarak iletişim kurabilir.

Varsayılan ad çözümlemesini yapar __değil__ sanal ağa katılan ağlardaki kaynakların adlarını çözümlemek HDInsight izin. Örneğin, sanal ağa şirket içi ağınıza katılmak için yaygındır. Yalnızca varsayılan ad çözümlemesi ile HDInsight adına göre şirket ağındaki kaynaklara erişemez. Şirket içi ağınızdaki kaynaklara adına göre sanal ağ içindeki kaynaklarla erişilemiyor, bunun tersi de geçerlidir.

> [!WARNING]
> Özel DNS sunucusu oluşturup HDInsight küme oluşturmadan önce kullanmak için sanal ağ yapılandırmanız gerekir.

Sanal ağ ve birleştirilmiş ağlardaki kaynaklar arasında ad çözümlemesine etkinleştirmek için aşağıdaki eylemleri gerçekleştirmeniz gerekir:

1. Azure sanal HDInsight yüklemeyi planladığınız ağ üzerinde özel bir DNS sunucusu oluşturun.

2. Sanal ağ özel DNS sunucusunu kullanacak şekilde yapılandırın.

3. Azure sanal ağınız için DNS son eki atanan bulun. Bu değer benzer `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net`. DNS son eki bulma hakkında daha fazla bilgi için bkz. [örnek: özel DNS](#example-dns) bölümü.

4. DNS sunucuları arasında iletim yapılandırın. Yapılandırma, uzak ağ türüne bağlıdır.

    * Uzak ağ ile şirket içi ağ ise, DNS gibi yapılandırın:
        
        * __Özel DNS__ (sanal ağdaki):

            * Azure yinelemeli çözümleyici (168.63.129.16) sanal ağa ait DNS sonekini istekleri iletmek. Azure sanal ağdaki kaynaklar için istekleri işler

            * Şirket içi DNS sunucusuna tüm istekleri iletin. Şirket içi DNS diğer ad çözümleme isteklerinin Microsoft.com gibi Internet kaynaklarına yönelik bile istekleri işler.

        * __Şirket içi DNS__: özel bir DNS sunucusu için sanal ağ DNS soneki için istekleri iletmek. Özel DNS sunucusu için Azure özyinelemeli çözümleyici ardından iletir.

        Bu yapılandırma yollar istekleri için sanal ağ özel DNS sunucusuna DNS sonekinde etki alanı adları tam. Diğer tüm istekler (hatta için genel internet adresleri), şirket içi DNS sunucusu tarafından işlenir.

    * Uzak ağ başka bir Azure sanal ağ değilse, DNS gibi yapılandırın:

        * __Özel DNS__ (içinde her sanal ağ için):

            * DNS soneki sanal ağlar için istekleri, özel DNS sunucularını iletilir. Her bir sanal ağ DNS, alt ağ içindeki kaynakları çözümleme için sorumludur.

            * Diğer tüm istekler için Azure özyinelemeli çözümleyici iletin. Özyinelemeli çözümleyici yerel çözme ve internet kaynakları sorumludur.

        DNS sunucusu DNS son ekini temel alınarak her ağ diğer istekleri iletir. Azure yinelemeli çözümleyici kullanarak diğer istekleri çözümlenir.

    Her yapılandırma örneği için bkz: [örnek: özel DNS](#example-dns) bölümü.

Daha fazla bilgi için [VM'ler ve rol örnekleri için ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) belge.

## <a name="directly-connect-to-hadoop-services"></a>Hadoop Hizmetleri doğrudan bağlanın

HDInsight üzerinde çoğu belgeleri, internet üzerinden kümeye erişim sahibi olduğunuzu varsayar. Örneğin, https://CLUSTERNAME.azurehdinsight.net konumundaki kümeye bağlanabildiğiniz kabul edilir. Bu adres, internet'ten erişimi kısıtlamak için Nsg veya Udr'ler kullandıysanız, kullanılabilir olmayan ortak ağ geçidi kullanır.

Ambari ve sanal ağ üzerinden diğer web sayfalarına bağlanmak için aşağıdaki adımları kullanın:

1. HDInsight küme düğümleri dahili tam etki alanı adlarını (FQDN) bulmak için aşağıdaki yöntemlerden birini kullanın:

    ```powershell
    $resourceGroupName = "The resource group that contains the virtual network used with HDInsight"

    $clusterNICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

    $nodes = @()
    foreach($nic in $clusterNICs) {
        $node = new-object System.Object
        $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
        $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
        $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
        $nodes += $node
    }
    $nodes | sort-object Type
    ```

    ```azurecli
    az network nic list --resource-group <resourcegroupname> --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

    Döndürülen düğümleri listesinde, FQDN için baş düğümlerine bulup, Ambari ve diğer web hizmetlerine bağlanmak için FQDN'leri kullanın. Örneğin, `http://<headnode-fqdn>:8080` Ambari erişmek için.

    > [!IMPORTANT]
    > Baş düğümler üzerinde barındırılan bazı hizmetler aynı anda yalnızca bir düğümde etkin olan. Bir baş düğüm üzerinde bir hizmete erişim deneyin ve bir 404 hatası döndürürse diğer baş düğümüne geçin.

2. Düğüm ve kullanılabilir bir hizmet bağlantı noktasını belirlemek için bkz: [HDInsight üzerindeki Hadoop Hizmetleri tarafından kullanılan bağlantı noktaları](./hdinsight-hadoop-port-settings-for-services.md) belge.

## <a id="networktraffic"></a> Ağ trafiğini denetleme

Aşağıdaki yöntemleri kullanarak bir Azure sanal ağlarda ağ trafiğini denetlenebilir:

* **Ağ güvenlik grupları** (NSG) ağa gelen ve giden trafiği filtrelemenize olanak tanır. Daha fazla bilgi için [ağ güvenlik grupları ile ağ trafiğini filtreleme](../virtual-network/security-overview.md) belge.

    > [!WARNING]
    > HDInsight, giden trafiği kısıtlama desteklemez.

* **Kullanıcı tanımlı yollar** (UDR) ağdaki kaynakları arasındaki trafiğin nasıl akacağını tanımlayın. Daha fazla bilgi için [kullanıcı tanımlı yollar ve IP iletme](../virtual-network/virtual-networks-udr-overview.md) belge.

* **Ağ sanal Gereçleri** cihazların güvenlik duvarları ve yönlendiriciler gibi çoğaltın. Daha fazla bilgi için [ağ Gereçleri](https://azure.microsoft.com/solutions/network-appliances) belge.

Yönetilen bir hizmet olarak HDInsight, Azure sistem durumu ve Yönetim Hizmetleri Azure bulutunda sınırsız erişim gerektirir. Nsg'leri ve Udr kullanırken HDInsight hizmetlerin hala HDInsight ile iletişim kurabildiğinden emin olmanız gerekir.

HDInsight, çeşitli bağlantı noktaları üzerinde hizmetleri sunar. Güvenlik Duvarı sanal Gereci kullanırken, bu hizmetler için kullanılan bağlantı noktalarında trafiğe izin vermeniz gerekir. Daha fazla bilgi için [gerekli bağlantı noktaları] bölümüne bakın.

### <a id="hdinsight-ip"></a> HDInsight ile ağ güvenlik gruplarını ve kullanıcı tanımlı yollar

Kullanmayı planlıyorsanız **ağ güvenlik grupları** veya **kullanıcı tanımlı yollar** ağ trafiğinizi denetlemek için HDInsight'ı yüklemeden önce aşağıdaki eylemleri gerçekleştirin:

1. HDInsight için kullanmayı düşündüğünüz Azure bölgesinin belirleyin.

2. HDInsight tarafından gerekli IP adresleri belirleyin. Daha fazla bilgi için [HDInsight tarafından gerekli IP adresleri](#hdinsight-ip) bölümü.

3. Oluşturun veya ağ güvenlik grupları veya kullanıcı tanımlı yollar ile HDInsight'ı yüklemeyi planladığınız alt ağ için değiştirin.

    * __Ağ güvenlik grupları__: izin __gelen__ bağlantı noktası üzerinde trafiğe __443__ IP adresleri.
    * __Kullanıcı tanımlı yollar__: her IP adresi için bir yol oluşturmak ve ayarlamak __sonraki atlama türü__ için __Internet__.

Ağ güvenlik grupları veya kullanıcı tanımlı yollar hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Ağ güvenlik grubu](../virtual-network/security-overview.md)

* [Kullanıcı tanımlı yollar](../virtual-network/virtual-networks-udr-overview.md)

#### <a name="forced-tunneling"></a>Zorlamalı tünel oluşturma

Zorlamalı tünel bir kullanıcı tanımlı yönlendirme burada tüm trafiğin bir alt ağdan belirli ağ veya şirket içi ağınız gibi konuma zorlanır yapılandırmadır. HDInsight mu __değil__ destek zorlamalı tünel oluşturma.

## <a id="hdinsight-ip"></a> Gerekli IP adresleri

> [!IMPORTANT]
> Azure sistem durumu ve Yönetim Hizmetleri, HDInsight ile iletişim kurabildiğini olmalıdır. Ağ güvenlik grupları veya kullanıcı tanımlı yollar kullanıyorsanız, IP adresleri HDInsight ulaşmak bu hizmetler için gelen trafiğe izin vermeniz.
>
> Bu bölümde, trafiği denetlemek için ağ güvenlik grupları veya kullanıcı tanımlı yollar kullanmazsanız, yoksayabilirsiniz.

Ağ güvenlik grupları veya kullanıcı tanımlı yollar kullanıyorsanız, HDInsight ulaşmak için Azure sistem durumu ve Yönetim hizmetlerinden gelen trafiğe izin vermeniz gerekir. Alt ağ içindeki VM'ler arasında trafiği de izin vermeniz gerekir. İzin verilmiş olmalıdır IP adreslerini bulmak için aşağıdaki adımları kullanın:

1. Her zaman aşağıdaki IP adreslerinden gelen trafiğe izin vermeniz gerekir:

    | IP adresi | İzin verilen bağlantı noktası | Yön |
    | ---- | ----- | ----- |
    | 168.61.49.99 | 443 | Gelen |
    | 23.99.5.239 | 443 | Gelen |
    | 168.61.48.131 | 443 | Gelen |
    | 138.91.141.162 | 443 | Gelen |

2. Ardından HDInsight kümenizi aşağıdaki bölgelerden birinde ise bölge için listelenen IP adreslerinden gelen trafiğe izin vermeniz gerekir:

    > [!IMPORTANT]
    > Kullanmakta olduğunuz Azure bölgesi listede yoksa, yalnızca adım 1'deki dört IP adreslerini kullanır.

    | Ülke | Bölge | İzin verilen IP adresleri | İzin verilen bağlantı noktası | Yön |
    | ---- | ---- | ---- | ---- | ----- |
    | Asya | Doğu Asya | 23.102.235.122</br>52.175.38.134 | 443 | Gelen |
    | &nbsp; | Güneydoğu Asya | 13.76.245.160</br>13.76.136.249 | 443 | Gelen |
    | Avustralya | Avustralya Doğu | 104.210.84.115</br>13.75.152.195 | 443 | Gelen |
    | &nbsp; | Avustralya Güneydoğu | 13.77.2.56</br>13.77.2.94 | 443 | Gelen |
    | Brezilya | Güney Brezilya | 191.235.84.104</br>191.235.87.113 | 443 | Gelen |
    | Kanada | Doğu Kanada | 52.229.127.96</br>52.229.123.172 | 443 | Gelen |
    | &nbsp; | Orta Kanada | 52.228.37.66</br>52.228.45.222 | 443 | Gelen |
    | Çin | Çin Kuzey | 42.159.96.170</br>139.217.2.219 | 443 | Gelen |
    | &nbsp; | Çin Doğu | 42.159.198.178</br>42.159.234.157 | 443 | Gelen |
    | &nbsp; | Çin Kuzey 2 | 40.73.37.141</br>40.73.38.172 | 443 | Gelen |
    | Avrupa | Kuzey Avrupa | 52.164.210.96</br>13.74.153.132 | 443 | Gelen |
    | &nbsp; | Batı Avrupa| 52.166.243.90</br>52.174.36.244 | 443 | Gelen |
    | Almanya | Almanya Orta | 51.4.146.68</br>51.4.146.80 | 443 | Gelen |
    | &nbsp; | Almanya Kuzeydoğu | 51.5.150.132</br>51.5.144.101 | 443 | Gelen |
    | Hindistan | Orta Hindistan | 52.172.153.209</br>52.172.152.49 | 443 | Gelen |
    | &nbsp; | Güney Hindistan | 104.211.223.67<br/>104.211.216.210 | 443 | Gelen |
    | Japonya | Japonya Doğu | 13.78.125.90</br>13.78.89.60 | 443 | Gelen |
    | &nbsp; | Japonya Batı | 40.74.125.69</br>138.91.29.150 | 443 | Gelen |
    | Kore | Kore Orta | 52.231.39.142</br>52.231.36.209 | 433 | Gelen |
    | &nbsp; | Kore Güney | 52.231.203.16</br>52.231.205.214 | 443 | Gelen
    | Birleşik Krallık | Birleşik Krallık Batı | 51.141.13.110</br>51.141.7.20 | 443 | Gelen |
    | &nbsp; | Birleşik Krallık Güney | 51.140.47.39</br>51.140.52.16 | 443 | Gelen |
    | Amerika Birleşik Devletleri | Orta ABD | 13.67.223.215</br>40.86.83.253 | 443 | Gelen |
    | &nbsp; | Doğu ABD | 13.82.225.233</br>40.71.175.99 | 443 | Gelen |
    | &nbsp; | Orta Kuzey ABD | 157.56.8.38</br>157.55.213.99 | 443 | Gelen |
    | &nbsp; | Batı Orta ABD | 52.161.23.15</br>52.161.10.167 | 443 | Gelen |
    | &nbsp; | Batı ABD | 13.64.254.98</br>23.101.196.19 | 443 | Gelen |
    | &nbsp; | Batı ABD 2 | 52.175.211.210</br>52.175.222.222 | 443 | Gelen |

    Azure kamu için kullanılacak IP adresleri hakkında daha fazla bilgi için bkz: [Azure kamu INTELLIGENCE + Analytıcs](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics) belge.

3. Erişim de izin vermeniz gerekir __168.63.129.16__. Azure'nın yinelemeli çözümleyici adresidir. Daha fazla bilgi için [VM'ler ve rol için ad çözümlemesi örnekleri](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) belge.

Daha fazla bilgi için [ağ trafiğini denetleme](#networktraffic) bölümü.

## <a id="hdinsight-ports"></a> Gerekli bağlantı noktaları

Bir ağ kullanmayı planlıyorsanız **sanal gereç Güvenlik Duvarı** sanal ağı güvenli hale getirmek için aşağıdaki bağlantı noktaları üzerinde giden trafiğe izin vermeniz gerekir:

* 53
* 443
* 1433
* 11000-11999
* 14000-14999

Belirli hizmetlere yönelik bağlantı noktalarının listesi için bkz. [HDInsight üzerindeki Hadoop Hizmetleri tarafından kullanılan bağlantı noktaları](hdinsight-hadoop-port-settings-for-services.md) belge.

Sanal gereçler için güvenlik duvarı kuralları hakkında daha fazla bilgi için bkz. [sanal gereç senaryo](../virtual-network/virtual-network-scenario-udr-gw-nva.md) belge.

## <a id="hdinsight-nsg"></a>Örnek: ağ güvenlik grupları ile HDInsight

Bu bölümdeki örneklerde, ağ güvenliği HDInsight, Azure Yönetim Hizmetleri ile iletişim kurmasına izin ver Grup kuralları oluşturmak nasıl ekleyebileceğiniz gösterilmektedir. Örnekleri kullanmadan önce kullanmakta olduğunuz Azure bölgesi için olanlarla eşleşmesi için IP adreslerini ayarlayın. Bu bilgiler bulabilirsiniz [HDInsight ile ağ güvenlik gruplarını ve kullanıcı tanımlı yollar](#hdinsight-ip) bölümü.

### <a name="azure-resource-management-template"></a>Azure kaynak yönetimi şablonu

Aşağıdaki kaynak yönetimi şablonu, gelen trafiği kısıtlar ancak HDInsight tarafından gerekli IP adreslerinden gelen trafiğe izin veren bir sanal ağ oluşturur. Bu şablon, ayrıca sanal ağ içinde bir HDInsight kümesi oluşturur.

* [Güvenli bir Azure sanal ağ ve bir HDInsight Hadoop kümesi dağıtma](https://azure.microsoft.com/resources/templates/101-hdinsight-secure-vnet/)

> [!IMPORTANT]
> Bu örnekte, kullanmakta olduğunuz Azure bölgesi eşleştirmek için kullanılan IP adreslerini değiştirin. Bu bilgiler bulabilirsiniz [HDInsight ile ağ güvenlik gruplarını ve kullanıcı tanımlı yollar](#hdinsight-ip) bölümü.

### <a name="azure-powershell"></a>Azure PowerShell

Gelen trafiği sınırlayan ve Kuzey Avrupa bölgesinde için IP adreslerinden gelen trafiğe izin veren bir sanal ağ oluşturmak için aşağıdaki PowerShell betiğini kullanın.

> [!IMPORTANT]
> Bu örnekte, kullanmakta olduğunuz Azure bölgesi eşleştirmek için kullanılan IP adreslerini değiştirin. Bu bilgiler bulabilirsiniz [HDInsight ile ağ güvenlik gruplarını ve kullanıcı tanımlı yollar](#hdinsight-ip) bölümü.

```powershell
$vnetName = "Replace with your virtual network name"
$resourceGroupName = "Replace with the resource group the virtual network is in"
$subnetName = "Replace with the name of the subnet that you plan to use for HDInsight"
# Get the Virtual Network object
$vnet = Get-AzureRmVirtualNetwork `
    -Name $vnetName `
    -ResourceGroupName $resourceGroupName
# Get the region the Virtual network is in.
$location = $vnet.Location
# Get the subnet object
$subnet = $vnet.Subnets | Where-Object Name -eq $subnetName
# Create a Network Security Group.
# And add exemptions for the HDInsight health and management services.
$nsg = New-AzureRmNetworkSecurityGroup `
    -Name "hdisecure" `
    -ResourceGroupName $resourceGroupName `
    -Location $location `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -name "hdirule1" `
        -Description "HDI health and management address 52.164.210.96" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "52.164.210.96" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 300 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 13.74.153.132" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "13.74.153.132" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 301 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule3" `
        -Description "HDI health and management 168.61.49.99" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.49.99" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 302 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule4" `
        -Description "HDI health and management 23.99.5.239" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "23.99.5.239" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 303 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule5" `
        -Description "HDI health and management 168.61.48.131" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.48.131" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 304 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule6" `
        -Description "HDI health and management 138.91.141.162" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "138.91.141.162" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 305 `
        -Direction Inbound `
# Set the changes to the security group
Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
# Apply the NSG to the subnet
Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name $subnetName `
    -AddressPrefix $subnet.AddressPrefix `
    -NetworkSecurityGroup $nsg
$vnet | Set-AzureRmVirtualNetwork
```

> [!IMPORTANT]
> Bu örnek, gerekli IP adreslerini gelen trafiğe izin vermek için kural eklemek üzere nasıl gösterir. Diğer kaynaklardan gelen erişimi kısıtlamak için bir kuralı içermiyor.
>
> Aşağıdaki örnek, Internet'ten SSH erişimini etkinleştirmek gösterilmektedir:
>
> ```powershell
> Add-AzureRmNetworkSecurityRuleConfig -Name "SSH" -Description "SSH" -Protocol "*" -SourcePortRange "*" -DestinationPortRange "22" -SourceAddressPrefix "*" -DestinationAddressPrefix "VirtualNetwork" -Access Allow -Priority 306 -Direction Inbound
> ```

### <a name="azure-classic-cli"></a>Klasik Azure CLI

Gelen trafiği kısıtlar ancak HDInsight tarafından gerekli IP adreslerinden gelen trafiğe izin veren bir sanal ağ oluşturmak için aşağıdaki adımları kullanın.

1. Adlı yeni bir ağ güvenlik grubu oluşturmak için aşağıdaki komutu kullanın `hdisecure`. Değiştirin **RESOURCEGROUPNAME** kaynak grubuyla Azure sanal ağı içerir. Değiştirin **konumu** grubunun oluşturulduğu konum (bölge).

    ```azurecli
    az network nsg create -g RESOURCEGROUPNAME -n hdisecure -l LOCATION
    ```

    Grup oluşturulduktan sonra yeni grubu hakkında bilgi alırsınız.

2. Azure HDInsight sistem durumu ve yönetim hizmetinden 443 numaralı bağlantı noktasında gelen iletişime izin verecek yeni bir ağ güvenlik grubu kuralları eklemek için aşağıdakileri kullanın. Değiştirin **RESOURCEGROUPNAME** ile Azure sanal ağı içeren kaynak grubunun adı.

    > [!IMPORTANT]
    > Bu örnekte, kullanmakta olduğunuz Azure bölgesi eşleştirmek için kullanılan IP adreslerini değiştirin. Bu bilgiler bulabilirsiniz [HDInsight ile ağ güvenlik gruplarını ve kullanıcı tanımlı yollar](#hdinsight-ip) bölümü.

    ```azurecli
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule1 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "52.164.210.96" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 300 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "13.74.153.132" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 301 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.49.99" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 302 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "23.99.5.239" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 303 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.48.131" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 304 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "138.91.141.162" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 305 --direction "Inbound"
    ```

3. Bu ağ güvenlik grubu için benzersiz tanımlayıcı almak için aşağıdaki komutu kullanın:

    ```azurecli
    az network nsg show -g RESOURCEGROUPNAME -n hdisecure --query 'id'
    ```

    Bu komut, aşağıdaki metne benzer bir değer döndürür:

        "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"

    Beklediğiniz sonuçları alamazsanız kimliği etrafında çift tırnak içinde komutunu kullanın.

4. Bir alt ağ için ağ güvenlik grubunu uygulamak için aşağıdaki komutu kullanın. Değiştirin __GUID__ ve __RESOURCEGROUPNAME__ fiyatlarla değerleri önceki adımdaki döndürdü. Değiştirin __VNETNAME__ ve __SUBNETNAME__ oluşturmak istediğiniz alt ağ adı ve sanal ağ adı.

    ```azurecli
    az network vnet subnet update -g RESOURCEGROUPNAME --vnet-name VNETNAME --name SUBNETNAME --set networkSecurityGroup.id="/subscriptions/GUID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"
    ```

    Bu komut tamamlandığında, sanal ağa HDInsight yükleyebilirsiniz.

> [!IMPORTANT]
> Bu adımlar, yalnızca Azure bulut üzerindeki HDInsight sistem durumu ve yönetim hizmetine erişim açın. HDInsight küme sanal ağ dışındaki diğer tüm erişim engellenir. Sanal Ağ dışından erişim etkinleştirmek için ek ağ güvenlik grubu kuralları eklemeniz gerekir.
>
> Aşağıdaki örnek, Internet'ten SSH erişimini etkinleştirmek gösterilmektedir:
>
> ```azurecli
> az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule5 --protocol "*" --source-port-range "*" --destination-port-range "22" --source-address-prefix "*" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 306 --direction "Inbound"
> ```

## <a id="example-dns"></a> Örnek: DNS yapılandırması

### <a name="name-resolution-between-a-virtual-network-and-a-connected-on-premises-network"></a>Bir sanal ağ ile bağlı şirket içi ağ arasında ad çözümleme

Bu örnek aşağıdaki varsayımların yapar:

* Bir Azure sanal bir VPN ağ geçidi kullanarak şirket ağına bağlı ağ var.

* Sanal ağda özel DNS sunucusu, Linux veya UNIX işletim sistemi olarak çalışıyor.

* [Bağlama](https://www.isc.org/downloads/bind/) özel DNS sunucusuna yüklenir.

Özel DNS sunucusunda sanal ağ:

1. Sanal ağın DNS soneki bulmak için Azure PowerShell veya Azure Klasik CLI kullanın:

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli-interactive
    read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. Sanal ağ için özel DNS sunucusunda aşağıdaki metni içeriğini kullanın. `/etc/bind/named.conf.local` dosyası:

    ```
    // Forward requests for the virtual network suffix to Azure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {168.63.129.16;}; # Azure recursive resolver
    };
    ```

    Değiştirin `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` sanal ağınızın DNS soneki ile değeri.

    Bu yapılandırma sanal ağın DNS soneki için tüm DNS istekleri için Azure özyinelemeli çözümleyici yönlendirir.

2. Sanal ağ için özel DNS sunucusunda aşağıdaki metni içeriğini kullanın. `/etc/bind/named.conf.options` dosyası:

    ```
    // Clients to accept requests from
    // TODO: Add the IP range of the joined network to this list
    acl goodclients {
        10.0.0.0/16; # IP address range of the virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            # All other requests are sent to the following
            forwarders {
                192.168.0.1; # Replace with the IP address of your on-premises DNS server
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform to RFC1035
            listen-on { any; };
    };
    ```
    
    * Değiştirin `10.0.0.0/16` değeri ile sanal ağ IP adresi aralığı. Bu giriş, bu aralıktaki ad çözümleme istekleri adreslerini sağlar.

    * Şirket içi ağ için IP adres aralığı Ekle `acl goodclients { ... }` bölümü.  Giriş şirket içi ağdaki kaynakları adı çözümlemesi isteklerinden sağlar.
    
    * Değeri Değiştir `192.168.0.1` ile şirket içi DNS sunucunuzun IP adresidir. Bu giriş, diğer tüm DNS istekleri şirket içi DNS sunucusuna yönlendirir.

3. Yapılandırmayı kullanmak için bağlama yeniden başlatın. Örneğin, `sudo service bind9 restart`.

4. Koşullu ileticisi şirket içi DNS sunucusuna ekleyin. Adım 1'den özel DNS sunucusuna DNS soneki için istekleri göndermeye koşullu ileticisi yapılandırın.

    > [!NOTE]
    > Koşullu ileticisi ekleme hakkında daha fazla ayrıntı için DNS yazılımınızın belgelerine bakın.

Bu adımları tamamladıktan sonra tam etki alanı adlarını (FQDN) kullanarak ya da ağ içindeki kaynaklarla bağlanabilirsiniz. HDInsight, artık sanal ağa yükleyebilirsiniz.

### <a name="name-resolution-between-two-connected-virtual-networks"></a>İki bağlı sanal ağlar arasında ad çözümleme

Bu örnek aşağıdaki varsayımların yapar:

* İki Azure sanal bir VPN ağ geçidi kullanarak veya eşleme bağlı ağ var.

* Her iki ağ özel DNS sunucusu, Linux veya UNIX işletim sistemi olarak çalışıyor.

* [Bağlama](https://www.isc.org/downloads/bind/) özel DNS sunucularında yüklü.

1. Her iki sanal ağ DNS sonekini bulmak için Azure PowerShell veya Azure Klasik CLI kullanın:

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter the resource group that contains the virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli-interactive
    read -p "Enter the name of the resource group that contains the virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. Aşağıdaki metni içeriğini kullanın `/etc/bind/named.config.local` özel DNS sunucusundaki dosya. Her iki sanal ağ özel DNS sunucusunda bu değişiklik yapın.

    ```
    // Forward requests for the virtual network suffix to Azure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # The IP address of the DNS server in the other virtual network
    };
    ```

    Değiştirin `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` DNS soneki değeriyle __diğer__ sanal ağ. Bu giriş, ağda özel DNS istekleri uzak ağ DNS soneki için yönlendirir.

3. Her iki sanal ağ içindeki özel DNS sunucularında aşağıdaki metni içeriğini kullanın. `/etc/bind/named.conf.options` dosyası:

    ```
    // Clients to accept requests from
    acl goodclients {
        10.1.0.0/16; # The IP address range of one virtual network
        10.0.0.0/16; # The IP address range of the other virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            forwarders {
            168.63.129.16;   # Azure recursive resolver         
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform to RFC1035
            listen-on { any; };
    };
    ```
    
    * Değiştirin `10.0.0.0/16` ve `10.1.0.0/16` değerleri ile IP adresi aralıkları, sanal ağların. Bu giriş, DNS sunucularının isteğinde bulunmak için her bir ağ kaynakları sağlar.

    Tüm istekler için DNS son eklerini sanal ağlar (örneğin, microsoft.com) olmayan Azure özyinelemeli çözümleyici tarafından gerçekleştirilir.

4. Yapılandırmayı kullanmak için bağlama yeniden başlatın. Örneğin, `sudo service bind9 restart` hem DNS sunucularında.

Bu adımları tamamladıktan sonra tam etki alanı adlarını (FQDN) bir sanal ağdaki kaynaklara bağlanabilir. HDInsight, artık sanal ağa yükleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Bir şirket içi ağa bağlanmak için HDInsight yapılandırma uçtan uca örneği için bkz: [bir şirket içi ağa bağlanma HDInsight](./connect-on-premises-network.md).
* Hbase kümeleri Azure sanal ağları yapılandırmak için bkz: [HBase kümeleri oluşturma Azure sanal ağdaki HDInsight üzerinde](hbase/apache-hbase-provision-vnet.md).
* HBase coğrafi çoğaltmayı yapılandırmak için bkz: [Azure sanal ağları içinde HBase kümesi çoğaltma ayarlama](hbase/apache-hbase-replication.md).
* Azure sanal ağları hakkında daha fazla bilgi için bkz. [Azure sanal ağına genel bakış](../virtual-network/virtual-networks-overview.md).

* Ağ güvenlik grupları hakkında daha fazla bilgi için bkz. [ağ güvenlik grupları](../virtual-network/security-overview.md).

* Kullanıcı tanımlı yollar hakkında daha fazla bilgi için bkz. [kullanıcı tanımlı yollar ve IP iletme](../virtual-network/virtual-networks-udr-overview.md).
