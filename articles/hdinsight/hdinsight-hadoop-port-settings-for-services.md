---
title: HDInsight - Azure üzerinde Hadoop Hizmetleri tarafından kullanılan bağlantı noktaları
description: HDInsight üzerinde çalışan Hadoop Hizmetleri tarafından kullanılan bağlantı noktalarının listesi.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: hrasheed
ms.openlocfilehash: 447e09e86eea7d88a6e4f320ea7b32a33f33d102
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51015392"
---
# <a name="ports-used-by-hadoop-services-on-hdinsight"></a>HDInsight üzerindeki Hadoop Hizmetleri tarafından kullanılan bağlantı noktaları

Bu belge, Linux tabanlı HDInsight kümelerinde çalışan Hadoop Hizmetleri tarafından kullanılan bağlantı noktalarının bir listesini sağlar. SSH kullanarak kümeye bağlanmak için kullanılan bağlantı noktaları hakkında bilgi de sağlar.

## <a name="public-ports-vs-non-public-ports"></a>Ortak bağlantı noktası ve genel olmayan bağlantı noktaları

Linux tabanlı HDInsight kümeleri yalnızca üç bağlantı noktalarını internet'te genel olarak kullanıma; 22, 23 ve 443. Bu bağlantı noktaları, güvenli bir şekilde SSH ve güvenli bir HTTPS protokolü üzerinden sunulan Hizmetleri kullanılarak kümeye erişmek için kullanılır.

HDInsight tarafından birden fazla Azure sanal makineler (küme içindeki düğümler) dahili olarak uygulanan bir Azure sanal ağ üzerinde çalışıyor. Sanal ağ içinde bağlantı noktaları internet üzerinden gösterilmeyen erişebilirsiniz. SSH kullanarak baş düğümlerinden biri için bağlarsanız, örneğin, baş düğümünden daha sonra doğrudan küme düğümleri üzerinde çalışan hizmetleri erişebilirsiniz.

> [!IMPORTANT]
> HDInsight için bir yapılandırma seçeneği bir Azure sanal ağı belirtmezseniz bir otomatik olarak oluşturulur. Ancak, diğer makineler (örneğin, diğer Azure sanal makinelerini veya istemci geliştirme makinenizde) bu sanal ağa katılamaz.


Ek makineler sanal ağa katılmak için önce sanal ağ oluşturun ve HDInsight kümenizi oluştururken belirtmeniz gerekir. Daha fazla bilgi için [kullanarak bir Azure sanal ağ genişletme HDInsight özellikleri](hdinsight-extend-hadoop-virtual-network.md)

## <a name="public-ports"></a>Genel bağlantı noktaları

Bir HDInsight kümesindeki tüm düğümler, bir Azure sanal ağında bulunur ve doğrudan internet'ten erişilemez. Genel bir ağ geçidi, tüm HDInsight küme türleri arasında ortak olan aşağıdaki bağlantı noktaları, İnternet'e erişim sağlar.

| Hizmet | Bağlantı noktası | Protokol | Açıklama |
| --- | --- | --- | --- |
| sshd |22 |SSH |İstemcileri birincil baş düğümdeki sshd bağlanır. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md). |
| sshd |22 |SSH |İstemciler kenar düğümündeki sshd bağlanır. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md). |
| sshd |23 |SSH |İstemciler sshd üzerinde ikincil baş düğümüne bağlanır. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md). |
| Ambari |443 |HTTPS |Ambari web kullanıcı Arabirimi. Bkz: [Ambari Web kullanıcı arabirimini kullanarak HDInsight yönetme](hdinsight-hadoop-manage-ambari.md) |
| Ambari |443 |HTTPS |Ambari REST API. Bkz: [Ambari REST API'yi kullanarak HDInsight yönetme](hdinsight-hadoop-manage-ambari-rest-api.md) |
| WebHCat |443 |HTTPS |HCatalog REST API. Bkz: [Curl ile Hive kullanma](hadoop/apache-hadoop-use-pig-curl.md), [Curl ile Pig kullanma](hadoop/apache-hadoop-use-pig-curl.md), [Curl ile MapReduce kullanma](hadoop/apache-hadoop-use-mapreduce-curl.md) |
| HiveServer2 |443 |ODBC |İçin Hive ODBC kullanarak bağlanır. Bkz: [bağlanmak Excel için Microsoft ODBC sürücüsü ile HDInsight](hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md). |
| HiveServer2 |443 |JDBC |İçin Hive JDBC kullanarak bağlanır. Bkz: [Hive JDBC sürücüsü kullanarak HDInsight üzerindeki Hive'a bağlanma](hadoop/apache-hadoop-connect-hive-jdbc-driver.md) |

Belirli küme türlerinin için şunlar kullanılabilir:

| Hizmet | Bağlantı noktası | Protokol | Küme türü | Açıklama |
| --- | --- | --- | --- | --- |
| Stargate |443 |HTTPS |HBase |HBase REST API. Bkz: [HBase kullanmaya başlama](hbase/apache-hbase-tutorial-get-started-linux.md) |
| Livy |443 |HTTPS |Spark |Spark REST API. Bkz: [uzaktan Livy kullanarak Spark gönderme işleri](spark/apache-spark-livy-rest-interface.md) |
| Spark Thrift sunucusu |443 |HTTPS |Spark |Spark Thrift sunucusu Hive sorguları göndermek için kullanılır. Bkz: [HDInsight üzerinde Hive ile Beeline kullanma](hadoop/apache-hadoop-use-hive-beeline.md) |
| Storm |443 |HTTPS |Storm |Storm web kullanıcı Arabirimi. Bkz: [Dağıt HDInsight üzerinde Storm topolojilerini ve yönetme](storm/apache-storm-deploy-monitor-topology-linux.md) |

### <a name="authentication"></a>Kimlik Doğrulaması

İnternet'te genel olarak kullanıma sunulan tüm hizmetleri kimlik doğrulamasından geçmesi gerekir:

| Bağlantı noktası | Kimlik Bilgileri |
| --- | --- |
| 22 veya 23 |Küme oluşturma sırasında SSH kullanıcı kimlik bilgileri belirtildi |
| 443 |Oturum açma adını (varsayılan: Yönetici) ve küme oluşturma sırasında ayarlanan parola |

## <a name="non-public-ports"></a>Genel olmayan bağlantı noktaları

> [!NOTE]
> Bazı hizmetler, yalnızca belirli küme türlerinin üzerinde kullanılabilir. HBase gibi yalnızca üzerinde HBase küme türleri olarak kullanılabilir.

> [!IMPORTANT]
> Bazı hizmetler, aynı anda yalnızca bir baş düğüm üzerinde çalıştırın. Birincil baş düğümdeki hizmetine bağlanın ve bir hata alırsanız çalışırsanız, ikincil baş düğümüne kullanarak yeniden deneyin.

### <a name="ambari"></a>Ambari

| Hizmet | Düğümler | Bağlantı noktası | URL yolu | Protokol | 
| --- | --- | --- | --- | --- |
| Ambari web kullanıcı Arabirimi | Baş düğümler | 8080 | / | HTTP |
| Ambari REST API | Baş düğümler | 8080 | / api/v1 | HTTP |

Örnekler:

* Ambari REST API: `curl -u admin "http://10.0.0.11:8080/api/v1/clusters"`

### <a name="hdfs-ports"></a>HDFS bağlantı noktaları

| Hizmet | Düğümler | Bağlantı noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| NameNode web kullanıcı Arabirimi |Baş düğümler |30070 |HTTPS |Web UI durumunu görüntülemek için |
| NameNode meta veri hizmeti |Baş düğüm |8020 |IPC |Dosya sistemi meta verileri |
| DataNode |Tüm çalışan düğümleri |30075 |HTTPS |Görünüm durumu, günlükleri vb. için Web kullanıcı Arabirimi. |
| DataNode |Tüm çalışan düğümleri |30010 |&nbsp; |Veri aktarımı |
| DataNode |Tüm çalışan düğümleri |30020 |IPC |Meta veri işlemleri |
| İkincil NameNode |Baş düğümler |50090 |HTTP |NameNode meta veriler için denetim noktası |

### <a name="yarn-ports"></a>YARN bağlantı noktaları

| Hizmet | Düğümler | Bağlantı noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| Resource Manager web kullanıcı Arabirimi |Baş düğümler |8088 |HTTP |Web kullanıcı Arabirimi için Resource Manager |
| Resource Manager web kullanıcı Arabirimi |Baş düğümler |8090 |HTTPS |Web kullanıcı Arabirimi için Resource Manager |
| Kaynak Yöneticisi Yönetim Arabirimi |Baş düğüm |8141 |IPC |Uygulamanın gönderileri için (Hive, Pig, Hive sunucusu, vs.) |
| Kaynak Yöneticisi scheduler |Baş düğüm |8030 |HTTP |Yönetim Arabirimi |
| Kaynak Yöneticisi uygulama arabirimi |Baş düğüm |8050 |HTTP |Uygulama Yöneticisi arabiriminin adresini |
| NodeManager |Tüm çalışan düğümleri |30050 |&nbsp; |Kapsayıcı Yöneticisi adresi |
| NodeManager web kullanıcı Arabirimi |Tüm çalışan düğümleri |30060 |HTTP |Kaynak Yöneticisi arabirimi |
| Zaman Çizelgesi adresi |Baş düğümler |10200 |RPC |Zaman Çizelgesi RPC hizmeti. |
| Zaman Çizelgesi web kullanıcı Arabirimi |Baş düğümler |8181 |HTTP |Zaman Çizelgesi hizmeti web kullanıcı Arabirimi |

### <a name="hive-ports"></a>Hive bağlantı noktaları

| Hizmet | Düğümler | Bağlantı noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| HiveServer2 |Baş düğümler |10001 |Thrift |Hive için (Thrift/JDBC) bağlamak için bir hizmet |
| Hive meta veri deposu |Baş düğümler |9083 |Thrift |Hive meta veri (Thrift/JDBC) bağlamak için bir hizmet |

### <a name="webhcat-ports"></a>WebHCat bağlantı noktaları

| Hizmet | Düğümler | Bağlantı noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| WebHCat sunucusu |Baş düğümler |30111 |HTTP |Web API üstünde HCatalog ve diğer Hadoop Hizmetleri |

### <a name="mapreduce-ports"></a>MapReduce bağlantı noktaları

| Hizmet | Düğümler | Bağlantı noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| JobHistory |Baş düğümler |19888 |HTTP |MapReduce JobHistory web kullanıcı Arabirimi |
| JobHistory |Baş düğümler |10020 |&nbsp; |MapReduce JobHistory sunucusu |
| ShuffleHandler |&nbsp; |13562 |&nbsp; |Aktarımları Ara harita genişletin isteyen için çıkarır. |

### <a name="oozie"></a>Oozie

| Hizmet | Düğümler | Bağlantı noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| Oozie sunucusu |Baş düğümler |11000 |HTTP |Oozie hizmeti için URL |
| Oozie sunucusu |Baş düğümler |11001 |HTTP |Oozie Yöneticisi için bağlantı noktası |

### <a name="ambari-metrics"></a>Ambari ölçümleri

| Hizmet | Düğümler | Bağlantı noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| Zaman Çizelgesi (uygulama geçmişi) |Baş düğümler |6188 |HTTP |Zaman Çizelgesi hizmeti web kullanıcı Arabirimi |
| Zaman Çizelgesi (uygulama geçmişi) |Baş düğümler |30200 |RPC |Zaman Çizelgesi hizmeti web kullanıcı Arabirimi |

### <a name="hbase-ports"></a>HBase bağlantı noktaları

| Hizmet | Düğümler | Bağlantı noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| HMaster |Baş düğümler |16000 |&nbsp; |&nbsp; |
| HMaster bilgileri Web kullanıcı Arabirimi |Baş düğümler |16010 |HTTP |HBase Master web kullanıcı Arabirimi için bir bağlantı noktası |
| Bölge sunucusu |Tüm çalışan düğümleri |16020 |&nbsp; |&nbsp; |
| &nbsp; |&nbsp; |2181 |&nbsp; |İstemcilerin ZooKeeper için bağlanmak için kullandığı bağlantı noktası |

### <a name="kafka-ports"></a>Kafka bağlantı noktaları

| Hizmet | Düğümler | Bağlantı noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| Aracısı |Çalışan düğümleri |9092 |[Kafka kablo protokolü](http://kafka.apache.org/protocol.html) |İstemci iletişimi için kullanılan |
| &nbsp; |Zookeeper düğümleri |2181 |&nbsp; |İstemcilerin Zookeeper için bağlanmak için kullandığı bağlantı noktası |

### <a name="spark-ports"></a>Spark bağlantı noktaları

| Hizmet | Düğümler | Bağlantı noktası | Protokol | URL yolu | Açıklama |
| --- | --- | --- | --- | --- | --- |
| Spark Thrift sunucuları |Baş düğümler |10002 |Thrift | &nbsp; | Spark SQL (Thrift/JDBC) bağlamak için bir hizmet |
| Livy sunucusu | Baş düğümler | 8998 | HTTP | &nbsp; | Deyimler, işleri ve uygulamaları çalıştırmak için hizmeti |
| Jupyter notebook | Baş düğümler | 8001 | HTTP | &nbsp; | Jupyter not defteri Web sitesi |

Örnekler:

* Livy: `curl -u admin -G "http://10.0.0.11:8998/"`. Bu örnekte, `10.0.0.11` Livy hizmetini barındıran baş düğümüne IP adresidir.
