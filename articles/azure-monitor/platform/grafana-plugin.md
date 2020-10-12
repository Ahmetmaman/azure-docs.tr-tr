---
title: Grafana kullanarak Azure hizmetlerini ve uygulamalarını izleme
description: Azure Izleyici 'yi yönlendirin ve verileri Grafana içinde görüntüleyebilmeniz için Application Insights.
ms.subservice: ''
ms.topic: conceptual
ms.date: 11/06/2017
ms.openlocfilehash: 23bba091628eee767fbf292a8a8d772ffab674cb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "87073465"
---
# <a name="monitor-your-azure-services-in-grafana"></a>Grafana 'de Azure hizmetlerinizi izleyin
Artık Azure [izleyici veri kaynağı eklentisini](https://grafana.com/plugins/grafana-azure-monitor-datasource)kullanarak [Grafana](https://grafana.com/) adresinden Azure hizmetlerini ve uygulamalarını izleyebilirsiniz. Eklenti, çeşitli Günlükler ve ölçümler de dahil olmak üzere Azure Izleyici tarafından toplanan uygulama performans verilerini toplar. Daha sonra bu verileri Grafana panonuzda görüntüleyebilirsiniz.

Bir Grafana sunucusunu ayarlamak ve Azure Izleyici 'de ölçümler ve Günlükler için panolar oluşturmak için aşağıdaki adımları kullanın.

## <a name="set-up-a-grafana-server"></a>Grafana sunucusu kurma

### <a name="set-up-grafana-locally"></a>Grafana 'ı yerel olarak ayarlama
Yerel bir Grafana sunucusu kurmak için [yerel ortamınıza Grafana indirip yükleyin](https://grafana.com/grafana/download). Eklentinin Azure Izleyici tümleştirmesini kullanmak için Grafana sürüm 5,3 veya üstünü yüklemelisiniz.

### <a name="set-up-grafana-on-azure-through-the-azure-marketplace"></a>Azure 'da Azure Market aracılığıyla Grafana ayarlama
1. Azure Marketi ' ne gidin ve Grafana by Grafana Labs ' i seçin.

2. Adları ve ayrıntıları girin. Yeni bir kaynak grubu oluşturma. VM Kullanıcı adı, VM parolası ve Grafana Server yönetici parolası için seçtiğiniz değerleri izleyin.  

3. VM boyutu ve depolama hesabı seçin.

4. Ağ yapılandırma ayarlarını yapılandırın.

5. Özeti görüntüleyin ve kullanım koşullarını kabul ettikten sonra **Oluştur** ' u seçin.

6. Dağıtım tamamlandıktan sonra **kaynak grubuna git**' i seçin. Yeni oluşturulan kaynakların listesini görürsünüz.

    ![Grafana kaynak grubu nesneleri](media/grafana-plugin/grafana1.png)

    Ağ güvenlik grubunu (Bu durumda*grafana-NSG* ) seçerseniz, 3000 bağlantı noktasının grafana sunucusuna erişmek için kullanıldığını görebilirsiniz.

7. Grafana sunucunuzun genel IP adresini alın-kaynak listesine geri dönün ve **genel IP adresi**' ni seçin.

## <a name="sign-in-to-grafana"></a>Grafana 'de oturum açın

1. Sunucunuzun IP adresini kullanarak, *http:// \<IP address\> : 3000* veya tarayıcınızda * \<DNSName> \: 3000* konumundaki oturum açma sayfasını açın. Varsayılan bağlantı noktası 3000 iken, kurulum sırasında farklı bir bağlantı noktası seçmiş olabilirsiniz. Oluşturduğunuz Grafana sunucusu için bir oturum açma sayfası görmeniz gerekir.

    ![Grafana oturum açma ekranı](./media/grafana-plugin/grafana-login-screen.png)

2. Daha önce oluşturduğunuz Kullanıcı adı *Yöneticisi* ve Grafana Server yönetici parolasıyla oturum açın. Yerel bir kurulum kullanıyorsanız, varsayılan parola *yönetici*olur ve ilk oturum açışınızda değiştirmeniz istenir.

## <a name="configure-data-source-plugin"></a>Veri kaynağı eklentisini yapılandırma

Başarıyla oturum açtıktan sonra, Azure Izleyici veri kaynağı eklentisinin zaten eklenmiş olduğunu görmeniz gerekir.

![Grafana Azure Izleyici eklentisini içerir](./media/grafana-plugin/grafana-includes-azure-monitor-plugin-dark.png)

1. Azure Izleyici veri kaynağını eklemek ve yapılandırmak için **veri kaynağı Ekle** ' yi seçin.

2. Veri kaynağı için bir ad seçin ve açılan menüden tür olarak **Azure izleyici** 'yi seçin.

3. Hizmet sorumlusu oluşturma-Grafana Azure Izleyici API 'Lerine bağlanmak ve veri toplamak için Azure Active Directory hizmet sorumlusu kullanır. Azure kaynaklarınıza erişimi yönetmek için mevcut bir hizmet sorumlusu oluşturmanız veya kullanmanız gerekir.
    * Hizmet sorumlusu oluşturmak için [Bu yönergelere](../../active-directory/develop/howto-create-service-principal-portal.md) bakın. Kiracı KIMLIĞINIZI (dizin KIMLIĞI), istemci KIMLIĞINI (uygulama KIMLIĞI) ve istemci gizli anahtarını (uygulama anahtarı değeri) kopyalayın ve kaydedin.
    * İzlemek istediğiniz abonelik, kaynak grubu veya kaynaktaki Azure Active Directory uygulamasına okuyucu rolü atamak için bkz. [role uygulama atama](../../active-directory/develop/howto-create-service-principal-portal.md) . 
    Log Analytics API 'SI, okuyucu rolü izinlerini içeren ve buna ekleyen [Log Analytics okuyucu rolünü](../../role-based-access-control/built-in-roles.md#log-analytics-reader)gerektirir.

4. Kullanmak istediğiniz API 'lere bağlantı ayrıntılarını sağlayın. Tümüne veya bazılarına bağlanabilirsiniz. 
    * Azure Izleyici 'de hem ölçümlere hem de günlüklere bağlanıyorsanız, **Azure İzleyici API 'Siyle aynı ayrıntıları**seçerek aynı kimlik bilgilerini yeniden kullanabilirsiniz.
    * Eklentiyi yapılandırırken, eklentinin hangi Azure bulutunun izlenmesini istediğinizi (genel, Azure ABD Kamu, Azure Almanya veya Azure Çin) belirtebilirsiniz.
    * Application Insights kullanıyorsanız, Application Insights tabanlı ölçümleri toplamak için Application Insights API 'nizi ve uygulama KIMLIĞINIZI da dahil edebilirsiniz. Daha fazla bilgi için bkz. [API anahtarınızı ve uygulama kimliğinizi alma](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID).

        > [!NOTE]
        > Bazı veri kaynağı alanları, bağıntılı Azure ayarlarından farklı şekilde adlandırılır:
        > * Kiracı KIMLIĞI, Azure Dizin KIMLIĞIDIR
        > * İstemci KIMLIĞI Azure Active Directory Uygulama KIMLIĞIDIR
        > * İstemci parolası Azure Active Directory Uygulama anahtarı değeridir

5. Application Insights kullanıyorsanız, Application Insights tabanlı ölçümleri toplamak için Application Insights API 'nizi ve uygulama KIMLIĞINIZI da dahil edebilirsiniz. Daha fazla bilgi için bkz. [API anahtarınızı ve uygulama kimliğinizi alma](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID).

6. **Kaydet**' i seçin ve GRAFANA her API için kimlik bilgilerini test eder. Aşağıdakine benzer bir ileti görmeniz gerekir.  
    ![Grafana veri kaynağı yapılandırması onaylandı](./media/grafana-plugin/grafana-data-source-config-approved-dark.png)

## <a name="build-a-grafana-dashboard"></a>Grafana panosu oluşturma

1. Grafana ana sayfasına gidin ve **Yeni Pano**' yı seçin.

2. Yeni panoda, **grafiği**seçin. Diğer grafik seçeneklerini deneyebilirsiniz, ancak bu makale örnek olarak *Graf* kullanır.

3. Panonuzda boş bir grafik görüntülenir. Panel başlığına tıklayın ve bu grafik grafiğinde çizmek istediğiniz verilerin ayrıntılarını girmek için **Düzenle** ' yi seçin.
    ![Grafana yeni grafik](./media/grafana-plugin/grafana-new-graph-dark.png)

4. Yapılandırdığınız Azure Izleyici veri kaynağını seçin.
   * Azure Izleyici ölçümlerini toplama-hizmet açılan menüsünde **Azure İzleyicisi** ' ni seçin. Bu grafikte izlenecek kaynakları ve ölçümü seçebileceğiniz seçicilerin bir listesi görüntülenir. Bir VM 'den ölçümleri toplamak için, **Microsoft. COMPUTE/VirtualMachines**ad alanını kullanın. VM 'Leri ve ölçümleri seçtikten sonra, verilerini panoda görüntülemeye başlayabilirsiniz.
     ![Azure Izleyici için Grafana Graph config](./media/grafana-plugin/grafana-graph-config-for-azure-monitor-dark.png)
   * Azure Izleyici günlük verilerini toplama-hizmet açılan menüsünde **azure Log Analytics** seçin. Sorgulamak istediğiniz çalışma alanını seçin ve sorgu metnini ayarlayın. Zaten sahip olduğunuz herhangi bir günlük sorgusunu kopyalayabilir veya yeni bir tane oluşturabilirsiniz. Sorgunuzu yazarken IntelliSense, otomatik tamamlama seçeneklerini gösterip önerecek. Görselleştirme türünü, **zaman serisi** **tablosunu**seçin ve sorguyu çalıştırın.
    
     > [!NOTE]
     >
     > Eklenti ile birlikte sunulan varsayılan sorgu iki makro kullanır: "$ __timeFilter () ve $ __interval. 
     > Bu makrolar, bir grafiğin bir bölümünü yakınlaştırdığınızda, Grafana zaman aralığını ve zaman aralığını dinamik olarak hesaplamasını sağlar. Bu makroları kaldırabilir ve *TimeGenerated > önce (1h)* gibi standart bir zaman filtresi kullanabilirsiniz ancak bu, grafiğin yakınlaştırma özelliğini desteklemediği anlamına gelir.
    
     ![Azure Log Analytics için Grafana Graph config](./media/grafana-plugin/grafana-graph-config-for-azure-log-analytics-dark.png)

5. İki grafik içeren basit bir pano aşağıda verilmiştir. Sol tarafta iki VM 'nin CPU yüzdesi gösterilmektedir. Sağdaki grafik, Işlem API 'SI türüne göre ayrılmış bir Azure depolama hesabındaki işlemleri gösterir.
    ![Grafana Iki grafik örneği](media/grafana-plugin/grafana6.png)


## <a name="optional-monitor-your-custom-metrics-in-the-same-grafana-server"></a>İsteğe bağlı: özel ölçümlerinizi aynı Grafana sunucusunda Izleyin

Ayrıca, hem özel hem de aracı tabanlı ölçümleri aynı Grafana örneği toplamak ve çizmek için telegraf ve etkileyen ve etkileyen bir veritabanı da yükleyebilirsiniz. Bu ölçümleri bir panoda bir araya getirmek için kullanabileceğiniz birçok veri kaynağı eklentisi vardır.

Bu ayarı, Prometheus sunucunuzdaki ölçümleri dahil etmek için de kullanabilirsiniz. Grafana 'ın eklenti galerisinde Prometheus veri kaynağı eklentisini kullanın.

Telegraf, etkileyen, Prometheus ve Docker 'ın nasıl kullanılacağına ilişkin en iyi başvuru makaleleri şunlardır.
 - [Ubuntu 16,04 üzerinde değer yığını ile sistem ölçümlerini Izleme](https://www.digitalocean.com/community/tutorials/how-to-monitor-system-metrics-with-the-tick-stack-on-ubuntu-16-04)

 - [Docker konakları, kapsayıcılar ve Kapsayıcılı hizmetler için bir izleme çözümü](https://stefanprodan.com/2016/a-monitoring-solution-for-docker-hosts-containers-and-containerized-services/)

Azure Izleyici ve Application Insights ölçümlerinden oluşan eksiksiz bir Grafana panosu görüntüsü aşağıda verilmiştir.
![Grafana örnek ölçümleri](media/grafana-plugin/grafana8.png)

## <a name="advanced-grafana-features"></a>Gelişmiş Grafana özellikleri

### <a name="variables"></a>Değişkenler
Bazı sorgu değerleri UI açılan listeleri aracılığıyla seçilebilir ve sorguda güncelleştirilir. Örnek olarak aşağıdaki sorguyu göz önünde bulundurun:
```
Usage 
| where $__timeFilter(TimeGenerated) 
| summarize total_KBytes=sum(Quantity)*1024 by bin(TimeGenerated, $__interval) 
| sort by TimeGenerated
```

Tüm kullanılabilir **çözüm** değerlerini listelemek için bir değişken yapılandırabilir ve ardından sorgunuzu kullanmak üzere güncelleştirebilirsiniz.
Yeni bir değişken oluşturmak için sağ üstteki alanın Pano ayarları düğmesine tıklayın, **değişkenler**' i ve sonra **Yeni**' yi seçin.
Değişken sayfasında, değerlerin listesini almak için çalıştırılacak veri kaynağını ve sorguyu tanımlayın.
![Grafana yapılandırma değişkeni](./media/grafana-plugin/grafana-configure-variable-dark.png)

Oluşturulduktan sonra, seçili değerleri kullanmak için sorguyu ayarlayın ve grafiklerinizin buna uygun şekilde yanıt vermesi gerekir:
```
Usage 
| where $__timeFilter(TimeGenerated) and Solution in ($Solutions)
| summarize total_KBytes=sum(Quantity)*1024 by bin(TimeGenerated, $__interval) 
| sort by TimeGenerated
```
    
![Grafana kullanma değişkenleri](./media/grafana-plugin/grafana-use-variables-dark.png)

### <a name="create-dashboard-playlists"></a>Pano çalma listeleri oluşturma

Grafana 'in birçok yararlı özellikten biri Pano çalma listesidir. Birden çok Pano oluşturabilir ve bunları gösterilecek her Pano için bir Aralık yapılandırarak bir çalma listesine ekleyebilirsiniz. Panoların geçiş yaparken görmek için **Yürüt** ' ü seçin. Grubunuza yönelik bir durum panosu sağlamak için bunları büyük bir duvar izleyicisinde görüntülemek isteyebilirsiniz.

![Grafana Playlist örneği](./media/grafana-plugin/grafana7.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure 'da bir Grafana ortamı oluşturduysanız, bunları kullanıp kullanmayacağınızı, sanal makineler çalışırken ücretlendirilirsiniz. Ek ücretler vermekten kaçınmak için, bu makalede oluşturulan kaynak grubunu temizleyin.

1. Azure portal sol taraftaki menüden **kaynak grupları** ' na ve ardından **Grafana**' e tıklayın.
2. Kaynak grubu sayfanızda, **Sil**' e tıklayın, metin kutusuna **Grafana** yazın ve ardından **Sil**' e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Izleyici ölçümlerine genel bakış](data-platform.md)
