---
title: VM'ler (Önizleme) için Azure İzleyici ile Uygulama bağımlılıklarını görüntüleme | Microsoft Docs
description: Harita, Azure İzleyici sanal makineler için otomatik olarak Windows ve Linux sistemleri üzerindeki uygulama bileşenlerini bulur ve hizmetler arasındaki iletişimi eşleyen bir özelliğidir. Bu makalede, çeşitli senaryoları içinde kullanma hakkında ayrıntılar sağlar.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/25/2018
ms.author: magoedte
ms.openlocfilehash: 0226e5dc03c3f10b951a4e4cc1d97a999c952f3b
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52678891"
---
# <a name="using-azure-monitor-for-vms-preview-map-to-understand-application-components"></a>Uygulama bileşenleri anlamak için Vm'leri (Önizleme) Map için Azure İzleyicisi'ni kullanma
Windows ve Linux ortamınızı Azure İzleyici ile iki şekilde VM'ler için doğrudan bir sanal makineden veya Azure İzleyici'den VM grupları arasında gösterilebilir azure'da çalışan sanal makineler üzerinde bulunan uygulama bileşenlerini görüntüleme. 

Bu makalede, iki perspektiften arasındaki eşleme özelliğini kullanma deneyimi anlamanıza yardımcı olur. VM'ler için Azure İzleyici yapılandırma hakkında daha fazla bilgi için bkz: [VM'ler için Azure İzleyici'ı etkinleştirme](vminsights-onboard.md).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="introduction-to-map-experience"></a>Harita deneyimi giriş
Tek sanal makine veya VM için harita görüntüleme içine girmeden önce kısa bir giriş özelliği sunuyoruz bilgileri nasıl görüntülenir ve görselleştirmeler ne temsil anlamak için önemlidir.  

Doğrudan bir VM veya Azure İzleyici eşleme özelliğini seçmek ister, tutarlı bir deneyim sunar.  Tek fark, bir çok katmanlı uygulaması veya bir eşlem içindeki kümesinin tüm üyeleri görebileceğiniz Azure İzleyici'den yerdir.

Haritalar sunucularını, gelen ve giden bağlantı gecikme süresi ve bağlantı noktaları arasında etkin ağ bağlantılarıyla çalışan işlemleri arasında tüm TCP bağlantılı mimarisi belirtilen zaman aralığı üzerinde bularak Vm'leri bağımlılıkları görselleştirir.  Bir VM genişletme işleminin ayrıntılarını gösterir ve VM ile iletişim kuran işlemleri gösterilir. Sanal makineye bağlanan ön uç istemcilerin sayısı, istemci grubu ile belirtilir. Arka uç sunucularının sayısı VM gösterilir sunucu bağlantı noktası grupları bağlanır. Bu bağlantı noktası üzerinden bağlı sunucuları ayrıntılı listesini görmek için bir sunucu bağlantı noktası grubu genişletin.  

Sanal makinede tıkladığınızda **özellikleri** sistem bilgileri Azure VM ve bir halka özellikleri, işletim sistemi tarafından bildirilen gibi seçili öğenin özelliklerini göstermek için sağ taraftaki bölmesi genişletildi bulunan bağlantılar özetleme. 

![Bilgisayarın Sistem Özellikleri](./media/vminsights-maps/properties-pane-01.png)

Sağ tarafında bölmesinin, tıklayarak **günlüğü olaylarını** odak noktası, sanal makineden toplanan veriler tabloların bir listesini göstermek için bölmenin geçiş yapmak için simge Log Analytics'e gönderilen ve sorgulama için kullanılabilir.  Listelenen kayıt türlerinden birini tıklayarak açılır **günlükleri** sayfası ile önceden yapılandırılmış bir sorgu türü için sonuçları görüntülemek için belirli bir sanal makineye karşı filtrelenmiş.  

![Özellikler bölmesinde günlük arama listesi](./media/vminsights-maps/properties-pane-logs-01.png)

Kapat *günlükleri** ve geri dönüp **özellikleri** bölmesi ve select **uyarılar** durumu ölçütlerini VM'den için oluşturulan uyarılar görünümü uyarılar için. Harita, seçili zaman aralığında sonra seçtiğiniz sunucu için tetiklenen uyarılar gösterilecek Azure uyarıları ile entegre olur. Sunucu, geçerli bir uyarı ve makine uyarılar bölmesinde uyarılar listeler bir simge görüntüler. 

![Özellikler bölmesinde makine uyarıları](./media/vminsights-maps/properties-pane-alerts-01.png)

İlgili uyarıları görüntülemek eşleme özelliğini etkinleştirmek için belirli bir bilgisayar için tetiklenen uyarı kuralı oluşturun. Uygun uyarılar oluşturmak için:
- Bilgisayar tarafından bir yan tümce grubuna ekleyin (örneğin, **bilgisayar aralığı 1 dakika**).
- Uyarı için ölçüm ölçüsü üzerinde temel'ı seçin.

Azure uyarıları ve uyarı oluşturma kuralları hakkında daha fazla bilgi için bkz. [birleşik uyarılar Azure İzleyici'de](../../monitoring-and-diagnostics/monitoring-overview-alerts.md)

**Gösterge** sağ üst köşede seçeneğinde, bir haritada semboller ve rolleri açıklar.  Haritanızı daha yakından göz atmak için yakınlaştırın ve alt kısmında BT etrafında yakınlaştırma denetimleri taşımak için sayfanın sağ tarafındaki yakınlaştırma seviyesini ayarlar ve geçerli sayfa boyutunu Sayfaya Sığdır.  

## <a name="connection-metrics"></a>Bağlantı ölçümü
**Bağlantıları** bölmesi, TCP bağlantı noktası üzerinden VM'den seçilen bağlantı için standart bağlantı ölçümleri görüntüler. Ölçümler, yanıt süresi, istek başına dakika, trafiği hacmi ve bağlantılar içerir.  

![Ağ bağlantısı grafikler bölmesinde örneği](./media/vminsights-maps/map-group-network-conn-pane-01.png)  

### <a name="failed-connections"></a>Başarısız bağlantılar
Başarısız bağlantılar, haritadaki işlemleri ve bilgisayarlar, bir işlem veya bağlantı noktası ulaşmak bir istemci sistem başarısız olduğunu gösteren bir kesikli kırmızı çizgi ile gösterilir. Sistemin başarısız bağlantı denemesi ise başarısız bağlantılar bağımlılık aracısını ile herhangi bir sistemden raporlanır. Harita, bu işlem bir bağlantı kurmak için başarısız TCP yuvaları gözlemleyerek ölçer. Bu hata, bir güvenlik duvarı, istemci veya sunucu veya kullanılamayan bir uzak hizmeti hatalı yapılandırılması kadar neden olabilir.

![Harita üzerinde başarısız bağlantı örneği](./media/vminsights-maps/map-group-failed-connection-01.png)

Başarısız bağlantı sorunlarını giderme, geçişi doğrulama, güvenlik Analizi ile yardımcı anlama ve hizmetin genel mimarisini anlama. Başarısız bağlantılar bazen zararsız, ancak bunlar genellikle doğrudan aniden ulaşılamaz hale bir yük devretme ortam veya Bulut geçişten sonra birbirleri ile iletişim kurmada başarısız olan iki uygulama katmanları gibi bir sorun üzerine gelin.

### <a name="client-groups"></a>İstemci Grupları
Harita üzerinde istemci grupları eşlenen makinede bağlantınız istemci makineleri temsil eder. Tek bir istemci grubundaki istemcileri için bir tek bir işlem veya makine temsil eder.

![Harita üzerinde istemci grupları örneği](./media/vminsights-maps/map-group-client-groups-01.png)

İzlenen istemciler ve bir istemci grubundaki sistemleri IP adreslerini görmek için grubu seçin. Grup içeriğini grubun altında listelenir.  

![Harita üzerinde istemci grubu IP adresi listesi örneği](./media/vminsights-maps/map-group-client-group-iplist-01.png)

İzlenen ve izlenmiyor istemcileri grubu içeriyorsa, uygun grubundaki istemcilerin filtrelemek için halka grafik bölümünü seçebilirsiniz.

### <a name="server-port-groups"></a>Sunucu bağlantı noktası grupları
Sunucu bağlantı noktası gruplarını temsil sunucular sunucu bağlantı noktalarına gelen bağlantılara eşlenen makine. Grup sunucusu bağlantı noktası ve bağlantıları Bu bağlantı noktasına sunucularıyla sayısını içerir. Tek tek sunucuların ve listelenen bağlantıları görmek için bir grup seçin. 

![Harita üzerinde sunucu bağlantı noktası grubu örneği](./media/vminsights-maps/map-group-server-port-groups-01.png)  

İzlenen ve izlenmiyor sunucuları grubu içeriyorsa, uygun grubundaki sunucuların filtrelemek için halka grafik bölümünü seçebilirsiniz.

## <a name="view-map-directly-from-a-virtual-machine"></a>Bir sanal makineden doğrudan haritayı görüntüleme 

Azure İzleyici VM'ler için bir sanal makineden doğrudan erişmek için aşağıdakileri gerçekleştirin.

1. Azure portalında **sanal makineler**. 
2. Listeden VM'yi seçin ve **izleme** bölümü seçin **Insights (Önizleme)**.  
3. Seçin **harita** sekmesi.

Harita işlem gruplarının ve işlemler belirtilen zaman aralığı üzerinde etkin ağ bağlantılarıyla çalıştıran VM'ler bağımlılıkları görselleştirir.  Varsayılan olarak, son 30 dakika eşlemeyi gösterir.  Kullanarak **TimeRange** sol üst köşesinde seçicide (örneğin, bir olay sırasında veya bir değişikliği oluşmadan önce) bağımlılıkları geçmişte nasıl baktığı göstermek için bir saat için geçmiş zaman aralıklarını sorgulayabilir.  

![Doğrudan VM eşlemesi genel bakış](./media/vminsights-maps/map-direct-vm-01.png)

## <a name="view-map-from-azure-monitor"></a>Azure İzleyicisi'nden haritayı görüntüleme
Azure İzleyici'den eşleme özelliğini sanal makinelerinizi ve bunların bağımlılıklarını genel bir görünümünü sağlar.  Azure İzleyici'den harita özelliğe erişmek için aşağıdakileri gerçekleştirin. 

1. Azure portalında **İzleyici**. 
2. Seçin **sanal makineler (Önizleme)** içinde **Insights** bölümü.
3. Seçin **harita** sekmesi.

![Azure İzleyici çok VM'li eşlemesi genel bakış](./media/vminsights-maps/map-multivm-azure-monitor-01.png)

Gelen **çalışma** sayfanın üst kısmındaki seçiciyi birden fazla Log Analytics çalışma alanı varsa Çözümle etkin olduğundan ve kendisine rapor veren sanal makinelerin bulunduğu çalışma alanını seçin. **Grubu** Seçici, abonelikler, kaynak grupları döndürecektir [bilgisayar grupları](../../azure-monitor/platform/computer-groups.md)ve VM ölçek kümeleri seçilen çalışma alanına ilgili bilgisayar. Seçiminiz yalnızca eşleme özelliğini uygular ve performans ya da harita taşımaz.

Varsayılan olarak, son 30 dakika eşlemeyi gösterir. Kullanarak **TimeRange** Seçici, geçmiş zaman aralıkları (örneğin, bir olay sırasında veya bir değişikliği oluşmadan önce) bağımlılıkları geçmişte nasıl baktığı göstermek için bir saat için sorgulayabilir.   

## <a name="next-steps"></a>Sonraki adımlar
Sistem durumu özelliği kullanmayı öğrenmek için bkz: [Azure VM durumunu görüntüle](vminsights-health.md), veya performans sorunlarını ve genel kullanımı ile Vm'leri performansınızı tanımlamak için bkz. [VM'lerin performans görünümü Azure İzleyici](vminsights-performance.md). 
