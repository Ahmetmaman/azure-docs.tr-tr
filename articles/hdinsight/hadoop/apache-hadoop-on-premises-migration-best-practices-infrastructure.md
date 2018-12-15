---
title: Azure HDInsight - altyapı en iyi uygulamaları şirket içi Apache Hadoop kümelerini geçirme
description: Azure HDInsight için geçirme şirket içi Hadoop kümeleri için en iyi altyapı öğrenin.
services: hdinsight
author: hrasheed-msft
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: hrasheed
ms.openlocfilehash: 6b0b047e74496fb9e58df05dc6118c5f376cb99d
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2018
ms.locfileid: "53437529"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---infrastructure-best-practices"></a>Azure HDInsight - altyapı en iyi uygulamaları şirket içi Apache Hadoop kümelerini geçirme

Bu makalede, Azure HDInsight kümelerinin altyapının yönetilmesi için öneriler sunar. Geçirme şirket içi Apache Hadoop sistemler ile Azure HDInsight için yardımcı olması için en iyi yöntemler sağlayan bir dizi gereksinimlerimizim bir parçasıdır.

## <a name="plan-for-hdinsight-cluster-capacity"></a>HDInsight küme kapasitesi planlama

HDInsight küme kapasitesi planlama yapmak için temel seçenekler şunlardır:

- **Bir bölge seçin** -Azure bölgesi belirler burada küme fiziksel olarak sağlanır. Küme okuma ve yazma gecikme süresini en aza indirmek için verilerin aynı bölgede olmalıdır.
- **Depolama konumu ve boyutu seçmek** -varsayılan depolama alanı kümeyle aynı bölgede olması gerekir. 48 düğümlü bir küme için 4-8 depolama hesabınızın olması önerilir. Yeterli toplam depolama alanı zaten olabilir ancak her depolama hesabı, işlem düğümleri için ek ağ bant genişliği sağlar. Birden çok depolama hesabında olduğunda, bir önek olmayan her bir depolama hesabı için rastgele bir ad kullanın. Rastgele adlandırma amacı dışında tüm hesaplarda (azaltma) depolama performans sorunlarını veya ortak modu hataları olasılığını azaltır. Daha iyi performans için depolama hesabı başına yalnızca bir kapsayıcı kullanın.
- **Türü (G serisi şimdi destekler) ve VM boyutu seçin** - düğüm türleri kümesi her küme türü vardır ve her düğüm türü, VM boyutunu ve türünü belirli seçenekler içerir. VM boyutunu ve türünü CPU işleme güç, RAM boyutu ve ağ gecikmesi tarafından belirlenir. Sanal bir iş yükü, en iyi VM boyutu ve her düğüm türü için türü belirlemek için kullanılabilir.
- **Çalışan düğümlerinin sayısını seçin** -sanal iş yükleri kullanarak ilk çalışan düğümü sayısı belirlenebilir. Küme, yoğun yük taleplerini karşılamak üzere daha fazla alt düğüm ekleyerek daha sonra ölçeklendirilebilir. Küme, daha sonra geri ek çalışan düğümleri gerekli olmadığında ölçeklendirilebilir.

Daha fazla bilgi için bkz [HDInsight kümeleri için kapasite planlaması](../hdinsight-capacity-planning.md).

## <a name="use-recommended-virtual-machine-type-for-cluster"></a>Küme için önerilen sanal makine türünü kullanın

Bkz: [varsayılan küme düğümü yapılandırması ve sanal makine boyutları](../hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters) için önerilen sanal makine türleri için HDInsight kümesi her türü.

## <a name="check-hadoop-components-availability-in-hdinsight"></a>HDInsight Hadoop bileşenlerinin kullanılabilirliğini denetleyin

Her bir HDInsight sürüm sürümünün Hortonworks Data Platform (HDP) bir bulut dağıtımıdır ve Hadoop ekonomik sistemi bileşenlerinin kümesinden oluşur. Bkz: [HDInsight bileşen sürümü oluşturma](../hdinsight-component-versioning.md) tüm HDInsight bileşenleri ve bunların geçerli sürümleri hakkında ayrıntılı bilgi için.

Hadoop bileşenleri ve HDInsight sürümleri denetlemek için Apache Ambari UI veya Ambari REST API de kullanabilirsiniz.

Uygulamaları veya bileşenleri, şirket içi kümeleri kullanılabilir ancak HDInsight kümelerinin parçası olmayan kenar düğümünde veya bir VM'de aynı sanal ağda HDInsight kümesi olarak eklenebilir. Azure HDInsight üzerinde kullanılabilir değil bir üçüncü taraf Hadoop uygulamasını, HDInsight kümesinde "Uygulamalar" seçeneğini kullanarak yüklenebilir. Özel Hadoop uygulamaları "betik eylemleri" kullanarak HDInsight kümesine yüklenebilir. Aşağıdaki tabloda bazı ortak uygulamalar ve kendi HDInsight tümleştirme seçenekleri yer almaktadır:

|**Uygulama**|**Tümleştirme**
|---|---|
|Hava akışı|Iaas veya HDInsight kenar düğümüne
|Alluxio|IaaS  
|Arcadia|IaaS 
|Atlas|Hiçbiri (yalnızca HDP)
|Datameer|HDInsight kenar düğümüne
|Datastax (Cassandra)|Iaas (CosmosDB alternatif azure'da)
|DataTorrent|IaaS 
|Drill|IaaS 
|Ignite|IaaS
|Jethro|IaaS 
|Mapador|IaaS 
|Mongo|Iaas (CosmosDB alternatif azure'da)
|NiFi|IaaS 
|Presto|Iaas veya HDInsight kenar düğümüne
|Python 2|PaaS 
|Python 3|PaaS 
|R|PaaS 
|SAS|IaaS 
|Vertica|Iaas (SQLDW alternatif azure'da)
|Tableau|IaaS 
|Waterline|HDInsight kenar düğümüne
|StreamSets|HDInsight Edge 
|Palantir|IaaS 
|Sailpoint|Iaas 

Daha fazla bilgi için bkz [farklı HDInsight sürümlerle kullanılabilir olan Apache Hadoop bileşenleri](../hdinsight-component-versioning.md#apache-hadoop-components-available-with-different-hdinsight-versions)

## <a name="customize-hdinsight-clusters-using-script-actions"></a>Komut dosyası Eylemleri'ni kullanarak HDInsight kümelerini özelleştirin

HDInsight, küme yapılandırmasının adlı bir yöntem sağlar bir **betik eylemi**. Betik eylemi, bir HDInsight kümesindeki düğümler üzerinde çalışan ve ek bileşenler yükleme ve yapılandırma ayarlarını değiştirmek için kullanılan Bash komut dosyasıdır.

Betik eylemleri, HDInsight kümesinden erişilebilir bir URI üzerinde depolanmalıdır. Bunlar, küme oluşturma sırasında veya sonrasında kullanılabilir ve yalnızca belirli düğüm türleri üzerinde çalıştırmak için kısıtlı olabilir.

Betik kalıcı veya bir kez yürütülür. Kalıcı duruma getirilmiş betiklerin küme işlemleri ölçeklendirme aracılığıyla eklenen yeni çalışan düğümlerindeki özelleştirmek için kullanılır. Ölçeklendirme işlemlerini gerçekleştiğinde kalıcı bir betik gibi bir baş düğüm, başka bir düğüm türü için değişiklikleri de uygulanabilir.

HDInsight, HDInsight kümelerinde aşağıdaki bileşenleri yüklemek için önceden yazılmış betik sağlar:

- Azure Depolama hesabı ekleme
- Hue yükleme
- Presto yükleme
- Solr Yükleme
- Giraph Yükleme
- Hive kitaplıklarını önceden yükleme
- Mono’yu yükleme veya güncelleştirme

> [!Note]  
> HDInsight, özel hadoop bileşenleri veya betik eylemleri kullanılarak yüklenen bileşenler için doğrudan destek sağlamaz.

Betik eylemleri, bir HDInsight uygulaması olarak Azure Marketi'nde ayrıca yayımlanabilir.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [HDInsight üzerinde üçüncü taraf Apache Hadoop uygulamaları yükleme](../hdinsight-apps-install-applications.md)
- [Komut dosyası Eylemleri'ni kullanarak HDInsight kümelerini özelleştirin](../hdinsight-hadoop-customize-cluster-linux.md)
- [Bir HDInsight uygulaması Azure Market'te yayımlama](../hdinsight-apps-publish-applications.md)

## <a name="customize-hdinsight-configs-using-bootstrap"></a>Bootstrap ile HDInsight yapılandırmaları özelleştirme

Yapılandırma dosyalarında yapılandırmaları gibi değişikliklerini `core-site.xml`, `hive-site.xml` ve `oozie-env.xml` Bootstrap ile yapılabilir. Aşağıdaki betik, Powershell kullanarak bir örnek verilmiştir:

```powershell
# hive-site.xml configuration
$hiveConfigValues = @{"hive.metastore.client.socket.timeout"="90"}

$config = New—AzureRmHDInsightClusterConfig '
    | Set—AzureRmHDInsightDefaultStorage
    —StorageAccountName "$defaultStorageAccountName.blob. core.windows.net" `
    —StorageAccountKey "defaultStorageAccountKey " `
    | Add—AzureRmHDInsightConfigValues `
        —HiveSite $hiveConfigValues

New—AzureRmHDInsightCluster `
    —ResourceGroupName $existingResourceGroupName `
    —Cluster-Name $clusterName `
    —Location $location `
    —ClusterSizeInNodes $clusterSizeInNodes `
    —ClusterType Hadoop `
    —OSType Linux `
    —Version "3.6" `
    —HttpCredential $httpCredential `
    —Config $config
```

Daha fazla bilgi için bkz [özelleştirme HDInsight kümeleri Bootstrap ile](../hdinsight-hadoop-customize-cluster-bootstrap.md).

## <a name="access-client-tools-from-hdinsight-hadoop-cluster-edge-nodes"></a>Erişim istemci araçlarından HDInsight Hadoop küme kenar düğümleri

Bir Linux sanal makinesi ile aynı istemci araçları yüklenir ve baş düğümleri olarak yapılandırılır, ancak hiçbir Hadoop Hizmetleri çalıştıran bir boş kenar düğümüdür. Kenar düğümüne aşağıdaki amaçlarla kullanılabilir:

- Küme erişme
- istemci uygulamalarını test etme
- istemci uygulamaları barındırma

Kenar düğümleri oluşturulabilir ve Azure portalı üzerinden silinmesi sırasında kullanılabilir ve sonra oluşturma küme. Kenar düğümüne oluşturulduktan sonra kenar düğümüne SSH kullanarak bağlanma ve HDInsight Hadoop kümesinde erişmek için İstemci Araçları'nı çalıştırın. Kenar düğümünün ssh uç noktası olan `<EdgeNodeName>.<ClusterName>-ssh.azurehdinsight.net:22`.


Daha fazla bilgi için bkz [HDInsight, Apache Hadoop kümelerinde boş kenar düğümlerini kullanma](../hdinsight-apps-use-edge-node.md).


## <a name="use-scale-up-and-scale-down-feature-of-clusters"></a>Küme ölçek büyütme ve ölçek azaltma özelliğini kullanın

HDInsight, ölçeğini ve, kümede çalışan düğümleri sayısını ölçeğini seçeneği vererek esneklik sağlar. Bu özellik, bir küme saat sonra veya hafta sonu Daralt ve en yüksek iş gereksinimlerini sırasında genişletmek verir.

Küme ölçeklendirme, aşağıdaki yöntemleri kullanarak otomatikleştirilebilir:

### <a name="powershell-cmdlet"></a>PowerShell cmdlet'i

```powershell
Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
```

### <a name="azure-cli"></a>Azure CLI

```powershell
azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>
```

### <a name="azure-portal"></a>Azure portal

Tüm bekleyen veya çalışan işler çalışan HDInsight kümenize düğümleri eklediğinizde, etkilenmez. Ölçeklendirme işlemi devam ederken yeni işleri güvenli bir şekilde gönderilebilir. Ölçeklendirme işlemleri için herhangi bir nedenle başarısız olursa, hata düzgün bir şekilde, kümenin işlevsel bir durumda bırakarak ele alınır.

Ancak, düğümleri kaldırarak kümenizi ölçeklendirme, ölçeklendirme işlemi tamamlandığında Beklemede veya çalışan tüm işleri başarısız olur. Bu hata yeniden başlatma işlemi sırasında hizmetlerinden bazılarını neden olur. Bu sorunu gidermek için kümenizi ölçeklendirmeden önce tamamlamak, el ile işleri sonlandırmak veya ölçeklendirme işlemini sonlandırdığını sonra sunmaları işlerinin tamamlanmasını bekleyebilirsiniz.

Kümenizin en az bir çalışan düğümü aşağı küçültme, çalışan düğümleri için düzeltme eki uygulama veya ölçeklendirme işleminin hemen sonra yeniden başlatılır, HDFS güvenli modda takılırsa haline. Güvenli mod dışında HDFS getirmek için aşağıdaki komutu çalıştırabilirsiniz:

```bash
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode leave
```

Güvenli mod bırakarak sonra el ile geçici dosyaları kaldırabilir, veya Hive sonunda otomatik olarak temizlenmesi için bekleyin.

Daha fazla bilgi için bkz [ölçek HDInsight kümeleri](../hdinsight-scaling-best-practices.md).

## <a name="use-hdinsight-with-azure-virtual-network"></a>HDInsight ile Azure sanal ağı kullanma

Azure kaynakları gibi Azure birbiriyle, İnternet'e güvenli şekilde iletişim kurması için sanal makineler, Azure sanal ağları etkinleştirebilir ve şirket içi ağlarda, filtrelemesini ve yönlendirmesini ağ trafiği.

HDInsight ile Azure sanal ağ kullanarak aşağıdaki senaryolar sağlar:

- HDInsight için bir şirket içi ağdan doğrudan bağlanma.
- HDInsight verilerine bağlantı kurma, bir Azure sanal ağında depolar.
- Doğrudan internet üzerinden genel olarak kullanılamayan Hadoop hizmetlerine erişme. Örneğin, Kafka API'lerin veya HBase Java API'si.

HDInsight, ya da yeni veya var olan Azure sanal ağa eklenebilir. HDInsight, mevcut bir sanal ağa ekleniyor, var olan ağ güvenlik grupları ve kullanıcı tanımlı yollar Kısıtlanmamış erişime izin vermek için güncelleştirilmesi gereken [birden fazla IP adresi](../hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip) Azure veri merkezi. Ayrıca, trafiği engellemediğinizden emin [bağlantı noktaları](../hdinsight-extend-hadoop-virtual-network.md#hdinsight-ports) HDInsight Hizmetleri tarafından kullanılan.

> [!Note]  
> HDInsight, zorlamalı tünel şu anda desteklemiyor. Zorlamalı tünel, giden Internet trafiği İnceleme için bir cihaz için zorlayan bir alt ağ ayarı ve günlük kaydı değildir. Zorlamalı tünel bir alt ağa HDInsight'ı yüklemeden önce kaldırın ya da HDInsight için yeni bir alt ağ oluşturun. HDInsight, giden ağ bağlantısını kısıtlama da desteklemez.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure sanal-ağ-genel bakış](../../virtual-network/virtual-networks-overview.md)
- [Bir Azure sanal ağı kullanarak Azure Hdınsight genişletme](../hdinsight-extend-hadoop-virtual-network.md)

## <a name="securely-connect-to-azure-services-with-azure-virtual-network-service-endpoints"></a>Azure Hizmetleri Azure sanal ağ hizmet uç noktaları ile güvenli bir şekilde bağlanma

HDInsight'ı destekleyen [sanal ağ hizmet uç noktaları](../../virtual-network/virtual-network-service-endpoints-overview.md) güvenli bir şekilde Azure Blob Depolama, Azure Data Lake depolama Gen2, Cosmos DB ve SQL veritabanlarına bağlanmak sağlar. Trafik, Azure HDInsight için bir hizmet uç noktası etkinleştirildiğinde, Azure veri merkezinde bir güvenli rotadaki aracılığıyla akar. Bu gelişmiş düzeyde güvenlik ağ katmanında ile büyük veri depolama hesapları, belirtilen sanal ağlar (Vnet'ler) kilitleme ve yine de HDInsight kümeleri sorunsuz bir şekilde erişmek ve bu verileri işlemek için kullanın.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Sanal ağ hizmeti uç noktaları](../../virtual-network/virtual-network-service-endpoints-overview.md)
- [Hizmet uç noktaları ile HDInsight güvenliğini artırın](https://azure.microsoft.com/blog/enhance-hdinsight-security-with-service-endpoints/.md)

## <a name="connect-hdinsight-to-the-on-premises-network"></a>HDInsight şirket içi ağa bağlanma

HDInsight, Azure sanal ağları ve VPN ağ geçidi kullanarak şirket içi ağa bağlanabilir. Aşağıdaki adımlar, bağlantı kurmak için kullanılabilir:

- Azure sanal ağında şirket içi ağ bağlantısı olan HDInsight'ı kullanın.
- DNS ad çözümlemesi, şirket içi ağ ve sanal ağ arasında yapılandırın.
- Ağ güvenlik grupları veya kullanıcı tanımlı yollar (UDR) ağ trafiğinizi denetlemek için yapılandırın.

Daha fazla bilgi için bkz [HDInsight'ı şirket içi ağınıza bağlama](../connect-on-premises-network.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu serideki sonraki makaleyi okuyun:

- [Şirket içi-Azure HDInsight Hadoop geçiş için en iyi depolama](apache-hadoop-on-premises-migration-best-practices-storage.md)