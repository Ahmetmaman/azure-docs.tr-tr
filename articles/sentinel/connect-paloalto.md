---
title: Gözcü Azure önizlemesinde Palo Alto veri toplama | Microsoft Docs
description: Azure Gözcü içinde Palo Alto verilerini nasıl toplayacağınızı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: a4b21d67-1a72-40ec-bfce-d79a8707b4e1
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/6/2019
ms.author: rkarlin
ms.openlocfilehash: 46ae6e6400834b11964c5f743782cc469011ac6d
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57534217"
---
# <a name="connect-your-palo-alto-networks-appliance"></a>Palo Alto Networks gerecinize bağlanma

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Gözcü herhangi bir Palo Alto Networks gereç Syslog CEF günlük dosyalarını kaydederek bağlanabilirsiniz. Gözcü Azure ile tümleştirme kolayca analiz ve sorguları Palo Alto arasında günlük dosyası verilerini çalıştırmanıza olanak sağlar. CEF verileri Azure Gözcü nasıl alan daha fazla bilgi için bkz: [bağlanma CEF Gereçleri](connect-common-event-format.md).

> [!NOTE]
> - Veriler Azure Gözcü çalıştırıyorsanız çalışma alanının coğrafi konumda depolanır.

## <a name="step-1-connect-your-palo-alto-appliance-using-an-agent"></a>1. Adım: Palo Alto gerecinize bir aracı kullanarak bağlanma

Palo Alto gerecinize Azure Gözcü için bağlanmak için adanmış bir makinede bir aracı dağıtmak gerekir (VM veya şirket içi) Gereci ve Azure Gözcü arasındaki iletişimi desteklemek için. Deploly aracıyı otomatik olarak veya elle yapabilirsiniz. Otomatik dağıtım, yalnızca ayrılmış makineniz Azure'da oluşturduğunuz yeni bir VM ise kullanılabilir. 

Alternatif olarak, aracı vm'sinde başka bir bulut, mevcut bir Azure sanal makinesinde el ile veya bir şirket içi makinede dağıtabilirsiniz.

İki seçenek de ağ diyagramı için bkz [veri kaynağına bağlanın](connect-data-sources.md#agent-options).


### <a name="deploy-the-agent-in-azure"></a>Aracıyı azure'da dağıtın

1. Gözcü Azure portalında **veri toplama** ve gereç türünüzü seçin. 

1. Altında **Linux Syslog aracı Yapılandırması**:
   - Seçin **otomatik dağıtım** yukarıda açıklandığı gibi Azure Gözcü aracıyla birlikte önceden yüklenir ve tüm yapılandırma gerekli içeren yeni bir makine oluşturmak istiyorsanız. Seçin **otomatik dağıtım** tıklatıp **otomatik aracı dağıtımı**. Bu, satın alma sayfasına, otomatik olarak çalışma alanınıza bağlı olduğu adanmış bir VM için götürür. VM bir **standart D2s v3 (2 vcpu, 8 GB bellek)** ve genel bir IP adresi vardır.
      1. İçinde **özel dağıtım** sayfasında ayrıntılarınızı sağlamak ve bir kullanıcı adı ve parola seçin ve hüküm ve koşulları kabul ediyorsanız, VM satın alın.
      1. Bağlantı sayfada listelenen ayarları kullanarak günlükleri göndermek için gerecinizin yapılandırın. Genel Common Event Format bağlayıcısının bu ayarları kullanın:
         - Protokol UDP =
         - Bağlantı noktası 514 =
         - Özelliği yerel 4 =
         - Biçim CEF =
   - Seçin **el ile dağıtım** ileride Azure Gözcü aracısının yüklenmesi gerekir özel Linux makine varolan bir VM'yi kullanmak istiyorsanız. 
      1. Altında **Syslog aracısını indirme ve yükleme**seçin **Azure Linux sanal makinesi**. 
      1. İçinde **sanal makineler** açılır, ekran seçin'e tıklayın, istediğiniz makine **Connect**.
      1. Bağlayıcı ekranda altında **yapılandırma ve iletme Syslog**ayarlayın, Syslog daemon olup **rsyslog.d** veya **syslog-ng**. 
      1. Bu komutlar kopyalayın ve bunları gerecinizde çalıştırın:
          - Rsyslog.d seçtiyseniz:
              
            1. Tesis local_4 üzerinde dinleme ve bağlantı noktası 25226'daki kullanarak Azure Gözcü Aracısı Syslog iletileri göndermek için Syslog daemon'u söyleyin. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
            2. İndirme ve yükleme [security_events yapılandırma dosyası](https://aka.ms/asi-syslog-config-file-linux) Syslog aracı 25226'daki bağlantı noktasında dinleyecek şekilde yapılandırır. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Burada {0} çalışma GUID ile değiştirilmelidir.
            
            1. Syslog daemon'u başlatmak `sudo service rsyslog restart`
             
          - Syslog-ng seçtiyseniz:

              1. Tesis local_4 üzerinde dinleme ve bağlantı noktası 25226'daki kullanarak Azure Gözcü Aracısı Syslog iletileri göndermek için Syslog daemon'u söyleyin. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
              2. İndirme ve yükleme [security_events yapılandırma dosyası](https://aka.ms/asi-syslog-config-file-linux) Syslog aracı 25226'daki bağlantı noktasında dinleyecek şekilde yapılandırır. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Burada {0} çalışma GUID ile değiştirilmelidir.

              3. Syslog daemon'u başlatmak `sudo service syslog-ng restart`
      2. Bu komutu kullanarak Syslog aracıyı yeniden başlatın: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Hiçbir hata aracı günlüğünde şu komutu çalıştırarak onaylayın: `tail /var/opt/microsoft/omsagent/log/omsagent.log`

### <a name="deploy-the-agent-on-an-on-premises-linux-server"></a>Aracı üzerinde bir şirket içi Linux sunucusunda dağıtma

Azure kullanmıyorsanız, adanmış bir Linux sunucusu üzerinde çalıştırmak için Azure Gözcü aracıyı el ile dağıtın.


1. Gözcü Azure portalında **veri toplama** ve gereç türünüzü seçin.
1. Altında adanmış bir Linux VM oluşturmak için **Linux Syslog aracı Yapılandırması** seçin **el ile dağıtım**.
   1. Altında **Syslog aracısını indirme ve yükleme**seçin **Azure olmayan Linux makine**. 
   1. İçinde **doğrudan aracı** seçtiğiniz açılır, ekran **Linux için aracıyı** aracıyı indirin veya Linux makinenizde indirmek için şu komutu çalıştırın:   `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w {workspace GUID} -s gehIk/GvZHJmqlgewMsIcth8H6VqXLM9YXEpu0BymnZEJb6mEjZzCHhZgCx5jrMB1pVjRCMhn+XTQgDTU3DVtQ== -d opinsights.azure.com`
    3. Bağlayıcı ekranda altında **yapılandırma ve iletme Syslog**ayarlayın, Syslog daemon olup **rsyslog.d** veya **syslog-ng**. 
    4. Bu komutlar kopyalayın ve bunları gerecinizde çalıştırın:
       - Rsyslog seçtiyseniz:
          1. Tesis local_4 üzerinde dinleme ve bağlantı noktası 25226'daki kullanarak Azure Gözcü Aracısı Syslog iletileri göndermek için Syslog daemon'u söyleyin. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
          2. İndirme ve yükleme [security_events yapılandırma dosyası](https://aka.ms/asi-syslog-config-file-linux) Syslog aracı 25226'daki bağlantı noktasında dinleyecek şekilde yapılandırır. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Burada {0} çalışma GUID ile değiştirilmelidir.
          3. Syslog daemon'u başlatmak `sudo service rsyslog restart`
       - Syslog-ng seçtiyseniz:
            1. Tesis local_4 üzerinde dinleme ve bağlantı noktası 25226'daki kullanarak Azure Gözcü Aracısı Syslog iletileri göndermek için Syslog daemon'u söyleyin. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
            2. İndirme ve yükleme [security_events yapılandırma dosyası](https://aka.ms/asi-syslog-config-file-linux) Syslog aracı 25226'daki bağlantı noktasında dinleyecek şekilde yapılandırır. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Burada {0} çalışma GUID ile değiştirilmelidir.
            3. Syslog daemon'u başlatmak `sudo service syslog-ng restart`
    5. Bu komutu kullanarak Syslog aracıyı yeniden başlatın: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
    6. Hiçbir hata aracı günlüğünde şu komutu çalıştırarak onaylayın: `tail /var/opt/microsoft/omsagent/log/omsagent.log`
 
## <a name="step-2-forward-palo-alto-logs-to-the-syslog-agent"></a>2. Adım: Palo Alto günlükleri Syslog aracıya ilet

Palo Alto Networks, Syslog aracı üzerinden Azure çalışma alanınıza CEF biçiminde Syslog iletilerini iletecek şekilde yapılandırın:
1.  Git [Common Event Format (CEF) yapılandırma kılavuzları](https://docs.paloaltonetworks.com/resources/cef) ve gereç türünüz için PDF dosyasını indirin. CEF olaylarını toplamak için Palo Alto gerecini ayarlamak için tüm Kılavuzu'ndaki yönergeleri izleyin. 

1.  Git [yapılandırma Syslog izleme](https://aka.ms/asi-syslog-paloalto-forwarding) ve 2. ve 3 CEF Olay iletme Palo Alto gerecinize Azure Gözcü için yapılandırma adımlarını izleyin.

    1. Ayarladığınızdan emin olun **Syslog sunucusu biçimi** için **BSD**.
    1. Ayarladığınızdan emin olun **tesis numarası** aynı değere Syslog aracıyı ayarlayın.

> [!NOTE]
> Kopyalama/yapıştırma işlemleri PDF'den metni değiştirme ve rastgele bir karakter yerleştirin. Bunu önlemek için bir düzenleyici metni kopyalayın ve bu örnekte görebileceğiniz gibi günlük biçimi, yapıştırmadan önce bozulabilir tüm karakterleri kaldırın.
 
  ![CEF metin Kopyala sorun](./media/connect-cef/paloalto-text-prob1.png)

## <a name="step-3-validate-connectivity"></a>3. Adım: Bağlantıyı doğrula

Çalınıyor Log Analytics'te görünmesini günlüklerinizi başlatana kadar 20 dakika sürebilir. 

1. Günlüklerinizi Syslog aracıyı doğru bağlantı noktasına aldığınızdan emin olun. Aracı makinede Syslog şu komutu çalıştırın: `tcpdump -A -ni any  port 514 -vv` Bu komut Syslog makineye CİHAZDAN akışı günlükleri gösterir. Günlükleri doğru bağlantı noktası ve doğru tesis kaynak gerecinde gelen alındığından emin olun.
2. Syslog cinini ve aracı arasındaki iletişim olup olmadığını denetleyin. Aracı makinede Syslog şu komutu çalıştırın: `tcpdump -A -ni any  port 25226 -vv` Bu komut Syslog makineye CİHAZDAN akışı günlükleri gösterir. Günlükleri de aracıda alındığından emin olun.
3. Bu komutların her ikisi de başarılı sonuçları sağlanan günlüklerinizi gelme görmek için Log Analytics kontrol edin. Bu gereçlerini akışa tüm olayları ham biçimde Log Analytics kapsamında görünen `CommonSecurityLog ` türü.
1. Hatalar varsa veya günlükleri gelen olmayan denetlemek için konum `tail /var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`
4. Syslog iletisi varsayılan boyutunuz (2 KB) 2048 bayt ile sınırlı olduğundan emin olun. Günlükleri çok uzun olması durumunda, bu komutu kullanarak security_events.conf güncelleştirin: `message_length_limit 4096`







## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için Palo Alto cihazları bağlayın öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).

