---
title: Hadoop bileşenleri ve sürümleri - Azure HDInsight | Microsoft Docs
description: Hadoop bileşenleri ve sürümleri HDInsight ve bu Hortonworks Data Platform bulut dağıtımlarında kullanılabilir hizmet düzeyleri öğrenin.
keywords: hadoop sürümlerini, hadoop ekosistemi bileşenleri, hadoop bileşenleri, hadoop sürüm denetimi yapma
services: hdinsight
editor: cgronlun
manager: asadk
author: kkampf
tags: azure-portal
documentationcenter: ''
ms.assetid: 367b3f4a-f7d3-4e59-abd0-5dc59576f1ff
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: kakampf
ms.openlocfilehash: fd2539830ab20fe4c63ddf3bb97cccdb13e535ea
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37857013"
---
# <a name="what-are-the-hadoop-components-and-versions-available-with-hdinsight"></a>Hadoop bileşenleri ve sürümleri HDInsight ile kullanılabilen nelerdir?

Microsoft Azure HDInsight yanı sıra Kurumsal güvenlik paketi sürümleri ve Apache Hadoop ekosistemi bileşenleri hakkında bilgi edinin. Ayrıca, HDInsight Hadoop bileşeni sürümlerinde denetleme konusunda bilgi edinin. 

Her bir HDInsight sürüm, Hortonworks Data Platform (HDP) sürümünün bir bulut dağıtımıdır.

## <a name="hadoop-components-available-with-different-hdinsight-versions"></a>Farklı HDInsight sürümlerle kullanılabilir Hadoop bileşenleri
Azure HDInsight, herhangi bir zamanda dağıtılabilir birden çok Hadoop küme sürümleri destekler. Her sürüm seçimi HDP dağıtım belirli bir sürümünü ve dağıtımı içinde bulunan bileşenler kümesi oluşturur. 4 Nisan 2017'den itibaren Azure HDInsight tarafından kullanılan varsayılan küme sürümü 3.6 olduğu ve HDP 2.6 üzerinde temel alır.

HDInsight küme sürümleri ile ilişkili bileşen sürümü aşağıdaki tabloda listelenmiştir: 

> [!NOTE]
> HDInsight hizmeti için varsayılan sürüm verilmeksizin. .NET SDK'sı ile Azure PowerShell ve Azure CLI ile kümeleri oluşturduğunuzda, bir sürüm bağımlılığı varsa, HDInsight sürüm belirtin.

| Bileşen | HDInsight 3.6 (varsayılan) | HDInsight 3.5 | HDInsight 3.4 | HDInsight 3.3 | HDInsight 3.2 | HDInsight 3.1 | HDInsight 3.0 |
| --- | --- | --- | --- | --- | --- | --- |--- |
| Hortonworks Veri Platformu |2.6 |2.5 |2.4 |2.3 |2.2 |2.1.7 |2.0 |
| Apache Hadoop ve YARN |2.7.3 |2.7.3 |2.7.1 |2.7.1 |2.6.0 |2.4.0 |2.2.0 |
| Apache Tez |0.7.0 |0.7.0 |0.7.0 |0.7.0 |0.5.2 |0.4.0 |-|
| Apache Pig |0.16.0 |0.16.0 |0.15.0 |0.15.0 |0.14.0 |0.12.1 |0.12.0 |
| Apache Hive ve HCatalog |1.2.1 |1.2.1 |1.2.1 |1.2.1 |0.14.0 |0.13.1 |0.12.0 |
| Apache Hive2 | 2.1.0 |-|-|-|-|-|-|
| Apache Tez Hive2 | 0.8.4 |-|-|-|-|-|-|
| Apache Ranger | 0.7.0 |0.6.0 |-|-|-|-|-|
| Apache HBase |1.1.2 |1.1.2 |1.1.2 |1.1.1 |0.98.4 |0.98.0 |-|
| Apache Sqoop |1.4.6 |1.4.6 |1.4.6 |1.4.6 |1.4.5 |1.4.4 |1.4.4 |
| Apache Oozie |4.2.0 |4.2.0 |4.2.0 |4.2.0 |4.1.0 |4.0.0 |4.0.0 |
| Apache Zookeeper |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.5 |3.4.5 |
| Apache Storm |1.1.0 |1.0.1 |0.10.0 |0.10.0 |0.9.3 |0.9.1 |-|
| Apache Mahout |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0 |0.9.0 |-|
| Apache Phoenix |4.7.0 |4.7.0 |4.4.0 |4.4.0 |4.2.0 |4.0.0.2.1.7.0-2162 |-|
| Apache Spark |2.3.0, 2.2.0, 2.1.0 |1.6.2, 2.0 |1.6.0 |1.5.2 |1.3.1 (yalnızca Windows) |-|-|
| Apache Livy |0.4 |0.3 |0.3 |0.2 |-|-|-|
| Apache Kafka | 1.0, 0.10.1 | 0.10.0 | 0.9.0 |-|-|-|-|
| Apache Ambari | 2.6.0 | 2.4.0 | 2.2.1 | 2.1.0 |-|-|-|
| Apache Zeppelin | 0.7.0 |-|-|-|-|-|-|
| Mono |4.2.1 |4.2.1 |3.2.8 |-|-|-|
| Apache kaydırıcı | 0.92.0 |-|-|-|-|-|-|

## <a name="check-for-current-hadoop-component-version-information"></a>Geçerli Hadoop bileşeni sürüm bilgileri için onay

HDInsight küme sürümleri ile ilişkili Hadoop ekosistemi bileşen sürümü, HDInsight için güncelleştirmeleri ile değiştirebilirsiniz. Hadoop bileşenleri denetimi ve bir küme için hangi sürümlerinin kullanıldığını doğrulamak için Ambari REST API'yi kullanın. **GetComponentInformation** komutu, hizmet bileşenleri hakkında bilgi alır. Ayrıntılar için bkz [Ambari belgeleri][ambari-docs].

Windows kümeleri için bileşen sürümü denetlemek için başka bir Uzak Masaüstü kullanarak bir küme için oturum açın ve C:\apps\dist\ dizininin içeriğini incelemek için yoludur.

> [!IMPORTANT]
> Linux üzerinde HDInsight sürüm 3.4 veya üzeri kullanılan tek işletim sistemidir. Daha fazla bilgi için [HDInsight üzerinde Windows emeklilik](#hdinsight-windows-retirement).

### <a name="release-notes"></a>Sürüm notları

Bkz: [HDInsight sürüm notları](hdinsight-release-notes.md) HDInsight'ın en son sürümlerinde ek sürüm notları.

## <a name="supported-hdinsight-versions"></a>Desteklenen HDInsight sürümleri
HDInsight sürümleri aşağıdaki tablolarda listelenmiştir. HDInsight her sürüme karşılık HDP sürümleri ürün yayın tarihleri ile birlikte listelenir. Bunlar bilinen olduğunda destek sona erme ve sona erme tarihleri de sağlanır.

### <a name="available-versions"></a>Kullanılabilir sürümler

Aşağıdaki tabloda, PowerShell ve .NET SDK'sı gibi diğer dağıtım yöntemleri yanı sıra Azure portalı içinde kullanılabilir olan HDInsight sürümleri listelenmiştir.

| HDInsight sürümü | HDP sürümü | VM İŞLETİM SİSTEMİ | Sürüm tarihi | Destek sona erme tarihi | Sona erme tarihi | Yüksek kullanılabilirlik |  Azure Portal kullanılabilirliği | 
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 3.6 |HDP 2.6 |16.0.4 ubuntu LTS |4 Nisan 2017 | | |Evet |Evet |
| HDInsight 3.3 |HDP 2.3 |Windows Server 2012 R2 |2 aralık 2015 |27 Haziran 2016 |31 Temmuz 2018 |Evet |Hayır |

> [!NOTE]
> Bir sürümünün süresi doldu için destek sonra onu Microsoft Azure Portalı aracılığıyla kullanılabilir olmayabilir. Ancak, küme sürümleri kullanılabilir kullanarak devam `Version` Windows PowerShell parametresi [New-Azurermhdınsightcluster](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/new-azurermhdinsightcluster) komut ve .NET SDK'sı sürüm devre dışı bırakılacağı tarihten kadar.
>

### <a name="retired-versions"></a>Devre dışı bırakılan sürümleri

Aşağıdaki tabloda, HDInsight sürümleri listelenmiştir **değil** Azure Portalı'nda kullanılabilir.

| HDInsight sürümü | HDP sürümü | VM İŞLETİM SİSTEMİ | Sürüm tarihi | Destek sona erme tarihi | Sona erme tarihi | Yüksek kullanılabilirlik |  Azure Portal kullanılabilirliği | 
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 3.5 |HDP 2.5 |16.0.4 ubuntu LTS |30 Eylül 2016 |5 Eylül 2017 |28 Haziran 2018'e |Evet |Hayır |
| HDInsight 3.4 |HDP 2.4 |14.0.4 ubuntu LTS |29 Mart 2016 |29 Aralık 2016 |9 Ocak 2018 |Evet |Hayır |
| HDInsight 3.3 |HDP 2.3 |14.0.4 ubuntu LTS |2 aralık 2015 |27 Haziran 2016 |31 Temmuz 2017 |Evet |Hayır |
| HDInsight 3.2 |HDP 2.2 |Ubuntu 12.04 LTS veya Windows Server 2012 R2 |18 Şubat 2015 |1 Mart 2016 |1 Nisan 2017 |Evet |Hayır |
| HDInsight 3.1 |HDP 2.1 |Windows Server 2012 R2 |24 Haziran 2014'e |18 Mayıs 2015 |30 Haziran 2016 |Evet |Hayır |
| HDInsight 3.0 |HDP 2.0 |Windows Server 2012 R2 |11 Şubat 2014 |17 Eylül 2014'ten |30 Haziran 2015 |Evet |Hayır |
| HDInsight 2.1 |HDP 1.3 |Windows Server 2012 R2 |28 Ekim 2013 |12 Mayıs 2014 |31 Mayıs 2015 |Evet |Hayır |
| HDInsight 1.6 |HDP 1.1 | |28 Ekim 2013 |26 Nisan 2014 |31 Mayıs 2015 |Hayır |Hayır |

> [!NOTE]
> İki baş düğümü ile yüksek oranda kullanılabilir kümeleri, HDInsight sürüm 2.1 ve üzeri için varsayılan olarak dağıtılır. HDInsight sürüm 1.6 kümelerinde kullanılamaz.

## <a name="enterprise-security-package-for-hdinsight"></a>HDInsight için Kurumsal güvenlik paketi

Kurumsal güvenlik HDInsight kümenizi oluşturma küme iş akışının parçası olarak ekleyebileceğiniz isteğe bağlı bir pakettir. Kurumsal güvenlik paketi destekler:

- Kimlik doğrulaması için Active Directory ile tümleştirme.

    Geçmişte, yerel bir yönetici ve yerel bir SSH kullanıcısı ile HDInsight kümeleri yalnızca oluşturabilirsiniz. Yerel yönetici kullanıcı, tüm dosyaları, klasörleri, tablolar ve sütunlar erişebilirsiniz.  Kurumsal güvenlik paketi ile HDInsight kümeleri kendi Active Directory ile tümleştirerek rol tabanlı erişim denetimi etkinleştirebilirsiniz içeren Active Directory, Azure Active Directory etki alanı Hizmetleri veya ıaas Active Directory şirket sanal makine. Kullanıcılar kendi Kurumsal (etki alanı) kullanıcı adı ve parola kümeye erişmek için kullanılacak etki alanı yöneticisi küme üzerinde verebilirsiniz. 

    Daha fazla bilgi için bkz.

    - [Bir etki alanına katılmış HDInsight kümeleriyle Hadoop güvenliğine giriş](./domain-joined/apache-domain-joined-introduction.md)
    - [HDInsight, Azure etki alanına katılmış Hadoop kümeleri planlama](./domain-joined/apache-domain-joined-architecture.md)
    - [Etki alanına katılmış korumalı alan ortamını yapılandırma](./domain-joined/apache-domain-joined-configure.md)
    - [Azure Active Directory Domain Services'ı kullanarak etki alanına katılmış HDInsight kümelerini yapılandırma](./domain-joined/apache-domain-joined-configure-using-azure-adds.md)

- Veriler için yetkilendirme

    - Hive, Spark SQL ve Yarn kuyruklar için yetkilendirme için Apache Ranger tümleştirme.
    - Dosyalar ve klasörler üzerindeki erişim denetimi ayarlayabilirsiniz.

    Daha fazla bilgi için bkz.

    - [İçinde etki alanına katılmış HDInsight Hive ilkelerini yapılandırma](./domain-joined/apache-domain-joined-run-hive.md)

- İzleyici erişir ve yapılandırılmış ilkeler için denetim günlüklerini görüntüleyin. 

### <a name="supported-cluster-types"></a>Desteklenen küme türleri

Şu anda yalnızca şu küme türlerini Kurumsal güvenlik paketi destekler:

- Hadoop (yalnızca HDInsight 3.6)
- Spark
- Interactive Query

### <a name="support-for-azure-data-lake-store"></a>Azure Data Lake Store desteği

Birincil depolama alanı ve ek depolama Azure Data Lake Store kullanarak kurumsal güvenlik paketi destekler.

### <a name="pricing-and-sla"></a>Fiyatlandırma ve SLA
Kurumsal güvenlik paketi için fiyatlandırma ve SLA hakkında ek bilgi için bkz: [HDInsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="hdinsight-windows-retirement"></a>HDInsight Windows'un devre dışı bırakılması
Microsoft Azure HDInsight sürüm 3.3 Windows üzerinde HDInsight son sürümünü oluştu. 31 Temmuz 2018'den Windows üzerinde HDInsight için kullanımdan kaldırma tarihtir. Tüm HDInsight kümeleri Windows 3.3 veya önceki sürümleri varsa, (HDInsight sürüm 3.5 veya sonraki sürümler) Linux'ta HDInsight için 31 Temmuz 2018'den önce geçirmeniz gerekir. Linux işletim sistemine geçiş oluşturun ya da HDInsight kümeleri yeniden boyutlandırma özelliği korumak sağlar. HDInsight sürümü 3.3 Windows üzerinde desteği, 27 Haziran 2016 tarihinde süresi doldu.

HDInsight sürüm 3.4 ile başlayarak, Microsoft HDInsight yalnızca Linux işletim sisteminde kullanıma sundu. Sonuç olarak, bazı bileşenlerin HDInsight içinde yalnızca Linux için kullanılabilir. Apache Ranger, Kafka, Interactive Query, Spark, HDInsight uygulamaları, bunlar ve birincil dosya sistemi olarak Azure Data Lake Store. HDInsight'ın gelecek sürümlerini, yalnızca Linux işletim sisteminde kullanılabilir. Hiçbir Windows üzerinde HDInsight'ın gelecek sürümlerini olacaktır. 

## <a name="faqs"></a>SSS

### <a name="what-is-the-timeline-for-retiring-hdinsight-on-windows"></a>HDInsight üzerinde Windows devre dışı bırakma zaman çizelgesi nedir?
31 Temmuz 2018'den, Windows üzerinde HDInsight emeklilik tarihtir. Planlanan o tarihten bölgeniz için farklı ise, ayrı olarak bildirilir. 

### <a name="what-is-the-impact-of-retiring-hdinsight-on-windows-for-existing-customers"></a>HDInsight üzerinde Windows mevcut müşteriler için devre dışı bırakma etkisi nedir?
Windows üzerinde HDInsight kullanımdan kaldırıldıktan sonra yeni bir HDInsight Windows kümesi oluşturma veya var olan HDInsight Windows kümesi yeniden boyutlandırma olamaz. HDInsight sürümü 3.3 desteği, 27 Haziran 2016 tarihinde süresi doldu. Bu nedenle, HDInsight 3.3 veya önceki sürümleri için hata düzeltmeleri veya desteği yoktur. HDInsight'ın gelecek sürümlerini, yalnızca Linux işletim sisteminde kullanılabilir. Hiçbir Windows üzerinde HDInsight'ın gelecek sürümlerini olacaktır.
 
### <a name="which-versions-of-hdinsight-on-windows-are-affected"></a>Windows üzerinde HDInsight'ın hangi sürümleri etkilenir?
Azure HDInsight sürüm 3.3 HDInsight için Windows son sürümüdür. Windows üzerinde HDInsight kullanımdan önce tüm HDInsight Windows kümeleri sürüm 3.3 veya önceki sürüm 3.5 veya sonraki sürümler Linux'ta HDInsight geçirilmelidir. Linux üzerinde HDInsight kümelerinizi geçirme, yeni kümeler oluşturun veya var olan kümeleri yeniden boyutlandırma yeteneği korumak sağlar. 

### <a name="what-do-i-need-to-do"></a>Yapmak ne gerekiyor?
HDInsight Windows kümeleri, 31 Temmuz 2018'den önce desteklenen bir HDInsight Linux kümeye geçirme. Daha fazla bilgi [HDInsight dokumentu migrace](hdinsight-migrate-from-windows-to-linux.md). Azure HDInsight sürümleri hakkında daha fazla ayrıntı için bkz: listesini [desteklenen sürümleri](hdinsight-component-versioning.md#supported-hdinsight-versions). 

### <a name="where-do-i-find-the-cluster-os-type"></a>Küme işletim sistemi türü nerede bulabilirim?
Azure portalında, HDInsight kümesine Genel Bakış sayfasına gidin ve bulmak **küme türü** altında **Essentials**. Küme işletim sistemi türleri, ilgili sayfada listelenir. 

### <a name="i-cant-migrate-to-an-hdinsight-linux-cluster-by-july-31-2018-what-is-the-impact-to-my-hdinsight-windows-cluster"></a>Bir HDInsight Linux kümesi için 31 Temmuz 2018 tarihine kadar geçişi yapamazsınız. Windows HDInsight kümem için etkisi nedir?
HDInsight Windows kümesi olarak çalıştırır-olduğunu, ancak yeni bir HDInsight Windows kümesi oluşturma, veya var olan HDInsight Windows kümesi yeniden boyutlandırın. 

### <a name="my-cluster-has-a-net-dependency-how-do-i-resolve-this-dependency-on-linux"></a>Kümem bir .NET bağımlılığı vardır. Bu bağımlılık Linux üzerinde nasıl giderebilirim?
Kullanarak, Linux küme bağımlılık çözümleyebilir [Mono projesi](http://www.mono-project.com/). Bu açık kaynak uygulaması .NET HDInsight Linux kümeleri için kullanılabilir. Daha fazla bilgi [HDInsight dokumentu migrace](hdinsight-migrate-from-windows-to-linux.md). 

### <a name="im-a-new-customer-for-hdinsight-on-windows-how-can-i-create-an-hdinsight-windows-cluster"></a>Windows üzerinde HDInsight için yeni bir müşteri ortağıyım. Bir Windows HDInsight kümesine nasıl oluşturabilirim?
3 Temmuz 2017'den itibaren yalnızca var olan HDInsight Windows müşterilerin yeni HDInsight Windows kümeleri oluşturabilirsiniz. Yeni müşteriler PowerShell veya SDK'sını kullanarak Azure portalında bir Windows HDInsight kümesi oluşturulamıyor. Yeni müşteriler Linux HDInsight kümesi oluşturmanızı öneririz. Mevcut müşterileri yeni HDInsight Windows HDInsight kadar Windows üzerinde devre dışı bırakılacağı tarihten kümeler oluşturabilirsiniz. 

### <a name="is-there-a-pricing-impact-associated-with-moving-from-hdinsight-on-windows-to-hdinsight-on-linux"></a>HDInsight Linux üzerinde HDInsight Windows üzerinde taşıma ile ilişkili bir fiyatlandırma etkisi var mı?
Hayır, fiyatlandırma ya da OS HDInsight için aynıdır. 

### <a name="what-are-the-customer-advantages-associated-with-the-move-to-only-using-hdinsight-on-linux"></a>Taşıma işlemi yalnızca Linux üzerinde HDInsight kullanma ile ilgili müşteri üstün olan yönleri nelerdir?
* Saati-açık kaynaklı büyük veri teknolojileri HDInsight hizmeti aracılığıyla market
* Büyük topluluk ve destek için bir ekosistem
* Açık kaynak topluluğu tarafından etkin olarak geliştirme Hadoop ve diğer büyük veri teknolojileri için çalışma olanağı

### <a name="does-hdinsight-on-linux-provide-additional-functionality-beyond-what-is-available-in-hdinsight-on-windows"></a>Linux üzerinde HDInsight sistemlerdekinden Windows üzerinde HDInsight içinde ek işlevsellik sağlar mı?
HDInsight sürüm 3.4 ile başlayarak, Microsoft HDInsight yalnızca Linux işletim sisteminde kullanıma sundu. Sonuç olarak, bazı bileşenlerin HDInsight içinde yalnızca Linux için kullanılabilir. Apache Ranger, Kafka, Interactive Query, Spark, HDInsight uygulamaları, bunlar ve birincil dosya sistemi olarak Azure Data Lake Store. 

## <a name="service-level-agreement-for-hdinsight-cluster-versions"></a>HDInsight küme sürümleri için hizmet düzeyi sözleşmesi
Hizmet düzeyi sözleşmesi (SLA) de tanımlanan bir _destek penceresi_. Destek, Microsoft Müşteri Hizmetleri ve desteği tarafından desteklenen bir HDInsight kümesi sürüm süre penceredir. Sürüm varsa, bir _destek sona erme tarihi_ geçirilen, HDInsight kümesi desteği penceresi dışında. Desteklenen sürümler hakkında daha fazla bilgi için bkz: listesini [desteklenen HDInsight küme sürümleri](hdinsight-migrate-from-windows-to-linux.md). Belirtilen bir HDInsight sürüm (yeni bir X + 1 sürümü kullanıma sunulduktan sonra) X desteği sona erme tarihini sonraki hesaplanır biri:  

* Formül 1: 180 gün boyunca HDInsight kümesi sürüm X serbest bırakıldığında tarihe ekler.
* Formül 2: 90 gün olduğunda HDInsight kümesi sürüm X + 1 Azure portalında kullanılabilir hale getirileceğini tarih ekleyin.

_Devre dışı bırakılacağı tarihten_ sonra küme sürümünü oluşturulamıyor HDInsight üzerinde tarihtir. 31 Temmuz 2017'den itibaren o tarihten sonra bir HDInsight kümesi yeniden boyutlandıramazsınız. 

> [!NOTE]
> Azure konuk işletim sistemi ailesi sürüm 4, Windows Server 2012 R2 64 bit sürümünü kullanan Windows HDInsight kümeleri (2.1, 3.0, 3.1, 3.2 ve 3.3 dahil olmak üzere sürümler) çalıştırın. Azure konuk işletim sistemi ailesi 4 sürümü, .NET Framework sürüm 4.0, 4.5, 4.5.1 ve 4.5.2'yi destekler.

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>Hortonworks sürüm notları HDInsight sürümleri ile ilişkili

Bölüm sürüm notları için HDInsight ile kullanılan Apache bileşenleri ve Hortonworks Data Platform dağıtımları için bağlantılar sağlar.
* HDInsight küme sürümü 3.6 temel alan bir Hadoop dağıtımı kullanır [Hortonworks Data Platform 2.6](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html).
* HDInsight kümesi sürüm 3.5 temel alan bir Hadoop dağıtımı kullanır [Hortonworks Data Platform 2.5](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.5.0/bk_release-notes/content/ch_relnotes_v250.html). HDInsight kümesi sürüm 3.5 _varsayılan_ Azure portalında oluşturduğunuz Hadoop kümesi.
* HDInsight küme sürümü 3.4 temel alan bir Hadoop dağıtımı kullanır [Hortonworks veri platformu 2.4](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html).
* HDInsight kümesi sürüm 3.3 temel alan bir Hadoop dağıtımı kullanır [Hortonworks veri platformu 2.3](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html).

  * [Apache Storm sürüm notları](https://storm.apache.org/2015/11/05/storm0100-released.html) Apache Web sitesinde mevcuttur.
  * [Apache Hive sürüm notları](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843) Apache Web sitesinde mevcuttur.
* HDInsight kümesi sürüm 3.2 temel alan bir Hadoop dağıtımı kullanır [Hortonworks Data Platform 2.2][hdp-2-2].

  * Kullanılabilir gibi olan belirli Apache bileşenler için sürüm notları: [0.14 Hive](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450), [Pig 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954), [HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810), [Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581), [M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180), [HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181), [YARN 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197), [ortak](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179), [Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742), [Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486), [Storm 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112), ve [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620).
* HDInsight kümesi sürüm 3.1 temel alan bir Hadoop dağıtımı kullanır [Hortonworks Data Platform 2.1.7][hdp-2-1-7]. Kasım, 7, 2014'ten önce oluşturulmuş 3.1 HDInsight kümeleri dayanır [Hortonworks Data Platform 2.1.1][hdp-2-1-1].
* HDInsight kümesi sürüm 3.0 temel alan bir Hadoop dağıtımı kullanır [Hortonworks Data Platform 2.0][hdp-2-0-8].
* HDInsight kümesi sürüm 2.1 temel alan bir Hadoop dağıtımı kullanır [Hortonworks veri platformu 1.3][hdp-1-3-0].
* HDInsight kümesi sürüm 1.6 temel alan bir Hadoop dağıtımı kullanır [Hortonworks veri platformu 1.1][hdp-1-1-0].






## <a name="default-node-configuration-and-virtual-machine-sizes-for-clusters"></a>Kümeler için varsayılan düğüm yapılandırması ve sanal makine boyutları
Aşağıdaki tablolar, HDInsight kümeleri için varsayılan sanal makine (VM) boyutları listeler.

> [!IMPORTANT]
> Bir kümedeki 32'den fazla alt düğüme ihtiyacınız varsa, bir baş düğüm boyutu en az 8 çekirdek ve 14 GB RAM ile seçmeniz gerekir.
> 
> 

* Brezilya Güney ve Japonya Batı dışındaki tüm desteklenen bölgeler:

  | Küme türü | Hadoop | HBase | Interactive Query | Storm | Spark | ML Server |
  | --- | --- | --- | --- | --- | --- | --- |
  | HEAD: varsayılan VM boyutu |D3 v2 |D3 v2 | D13, D14 |A3 |D12 v2 |D12 v2 |
  | HEAD: Önerilen VM boyutları |D3 v2, D4 v2, D12 v2 |D3 v2, D4 v2, D12 v2  | D13, D14 |A3, A4, A5 |D12 v2, D13 v2, D14 v2 |D12 v2, D13 v2, D14 v2 |
  | Çalışan: varsayılan VM boyutu |D3 v2 |D3 v2  | D13, D14 |D3 v2 |Windows: D12 v2; Linux: D4 v2 |Windows: D12 v2; Linux: D4 v2 |
  | Çalışan: Önerilen VM boyutları |D3 v2, D4 v2, D12 v2 |D3 v2, D4 v2, D12 v2  | D13, D14 |D3 v2, D4 v2, D12 v2 |Windows: D12 v2, D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |Windows: D12 v2, D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |
  | ZooKeeper: varsayılan VM boyutu | |A3 | |A2 | | |
  | ZooKeeper: Önerilen VM boyutları | |A3, A4, A5 | | A2, A3, A4 | | |
  | Edge: varsayılan VM boyutu | | | | | |Windows: D12 v2; Linux: D4 v2 |
  | Sınırı: Önerilen VM boyut | | | | | |Windows: D12 v2, D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |
* Brezilya Güney ve yalnızca Japonya Batı (v2 boyutları):

  | Küme türü | Hadoop | HBase | Interactive Query |Storm | Spark | ML Server |
  | --- | --- | --- | --- | --- | --- | --- |
  | HEAD: varsayılan VM boyutu |D3 |D3  | D13, D14 |A3 |D12 |D12 |
  | HEAD: Önerilen VM boyutları |D3, D4, D12 |D3, D4, D12  | D13, D14 |A3, A4, A5 |D12, D13, D14 |D12, D13, D14 |
  | Çalışan: varsayılan VM boyutu |D3 |D3  | D13, D14 |D3 |Windows: D12; Linux: D4 |Windows: D12; Linux: D4 |
  | Çalışan: Önerilen VM boyutları |D3, D4, D12 |D3, D4, D12  | D13, D14 |D3, D4, D12 |Windows: D12, D13, D14; Linux: D4, D12, D13, D14 |Windows: D12, D13, D14; Linux: D4, D12, D13, D14 |
  | ZooKeeper: varsayılan VM boyutu | |A2 | | A2 | | |
  | ZooKeeper: Önerilen VM boyutları | |A2, A3, A4 | |A2, A3, A4 | | |
  | Edge: varsayılan VM boyutları | | | | | |Windows: D12; Linux: D4 |
  | Edge: Önerilen VM boyutları | | | | | |Windows: D12, D13, D14; Linux: D4, D12, D13, D14 |

> [!NOTE]
> - HEAD olarak bilinen *Nimbus* Storm için küme türü.
> - Çalışan olarak bilinen *gözetmen* Storm için küme türü.
> - Çalışan olarak bilinen *bölge* için HBase küme türü.

## <a name="next-steps"></a>Sonraki adımlar
- [Hadoop, Spark ve HDInsight hakkında daha fazla bilgi için Kurulum küme](hdinsight-hadoop-provision-linux-clusters.md)
- [Bir Windows PC ile gelen HDInsight üzerinde Hadoop çalışma](hdinsight-hadoop-windows-tools.md)

[Supported HDInsight versions]:(#supported-hdinsight-versions)

[image-hdi-versioning-versionscreen]: ./media/hdinsight-component-versioning/hdi-versioning-version-screen.png

[wa-forums]: http://azure.microsoft.com/support/forums/

[connect-excel-with-hive-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md

[hdp-2-2]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.9/bk_HDP_RelNotes/content/ch_relnotes_v229.html

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: https://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.1.1.16_1.html

[ambari-docs]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[zookeeper]: http://zookeeper.apache.org/
