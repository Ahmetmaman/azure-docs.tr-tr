---
title: Gözcü Azure önizlemesinde F5 veri toplama | Microsoft Docs
description: Azure Gözcü içinde F5 verilerini nasıl toplayacağınızı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 0001cad6-699c-4ca9-b66c-80c194e439a5
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 552d78869a9b4b7eb65497bd0c5e6efe5f7aa8c9
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57434717"
---
# <a name="connect-your-f5-appliance"></a>F5 gerecinize bağlanma

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Gözcü herhangi bir F5 gereç Syslog CEF günlük dosyalarını kaydederek bağlanabilirsiniz. Gözcü Azure ile tümleştirme kolayca analiz ve sorguları F5 arasında günlük dosyası verilerini çalıştırmanıza olanak sağlar. CEF verileri Azure Gözcü nasıl alan daha fazla bilgi için bkz: [bağlanma CEF Gereçleri](connect-common-event-format.md).

> [!NOTE]
> - Veriler Azure Gözcü çalıştırıyorsanız çalışma alanının coğrafi konumda depolanır.

## <a name="step-1-connect-your-f5-appliance-using-an-agent"></a>1. Adım: F5 gerecinize bir aracı kullanarak bağlanma

Azure Gözcü için F5 cihazınıza bağlanmak için adanmış bir makinede bir aracı dağıtmak gerekir (VM veya şirket içi) Gereci ve Azure Gözcü arasındaki iletişimi desteklemek için. Deploly aracıyı otomatik olarak veya elle yapabilirsiniz. Otomatik dağıtım, yalnızca ayrılmış makineniz Azure'da oluşturduğunuz yeni bir VM ise kullanılabilir. 

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
            
            2. İndirme ve yükleme [security_events yapılandırma dosyası](https://aka.ms/asi-syslog-config-file-linux) Syslog aracı 25226'daki bağlantı noktasında dinleyecek şekilde yapılandırır. `wget -P /etc/opt/microsoft/omsagent/{workspace GUID}/conf/omsagent.d/ -O security_events.conf "https://aka.ms/syslog-config-file-linux"`
            
            1. Syslog daemon'u başlatmak `sudo service rsyslog restart`
             
          - Syslog-ng seçtiyseniz:

              1. Tesis local_4 üzerinde dinleme ve bağlantı noktası 25226'daki kullanarak Azure Gözcü Aracısı Syslog iletileri göndermek için Syslog daemon'u söyleyin. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
              2. İndirme ve yükleme [security_events yapılandırma dosyası](https://aka.ms/asi-syslog-config-file-linux) Syslog aracı 25226'daki bağlantı noktasında dinleyecek şekilde yapılandırır. `wget -P /etc/opt/microsoft/omsagent/{workspace GUID}/conf/omsagent.d/ -O security_events.conf "https://aka.ms/syslog-config-file-linux"`

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
            
          2. İndirme ve yükleme [security_events yapılandırma dosyası](https://aka.ms/asi-syslog-config-file-linux) Syslog aracı 25226'daki bağlantı noktasında dinleyecek şekilde yapılandırır. `wget -P /etc/opt/microsoft/omsagent/{workspace GUID}/conf/omsagent.d/ -O security_events.conf "https://aka.ms/syslog-config-file-linux"`
          3. Syslog daemon'u başlatmak `sudo service rsyslog restart`
       - Syslog-ng seçtiyseniz:
            1. Tesis local_4 üzerinde dinleme ve bağlantı noktası 25226'daki kullanarak Azure Gözcü Aracısı Syslog iletileri göndermek için Syslog daemon'u söyleyin. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
            2. İndirme ve yükleme [security_events yapılandırma dosyası](https://aka.ms/asi-syslog-config-file-linux) Syslog aracı 25226'daki bağlantı noktasında dinleyecek şekilde yapılandırır. `wget -P /etc/opt/microsoft/omsagent/{workspace GUID}/conf/omsagent.d/ -O security_events.conf "https://aka.ms/syslog-config-file-linux"`
            3. Syslog daemon'u başlatmak `sudo service syslog-ng restart`
    5. Bu komutu kullanarak Syslog aracıyı yeniden başlatın: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
    6. Hiçbir hata aracı günlüğünde şu komutu çalıştırarak onaylayın: `tail /var/opt/microsoft/omsagent/log/omsagent.log`
 
## <a name="step-2-forward-f5-logs-to-the-syslog-agent"></a>2. Adım: F5 günlükleri Syslog aracıya ilet

F5'e Azure çalışma alanınıza Syslog aracı üzerinden CEF biçiminde Syslog iletilerini iletecek şekilde yapılandırın:

F5'e gidin [uygulama güvenlik olay günlüğü yapılandırma](https://aka.ms/asi-syslog-f5-forwarding), aşağıdaki yönergeleri kullanarak uzak günlük yedekleme ayarlamak için yönergeleri izleyin:
  - Ayarlama **uzak depolama türü** için **CEF**.
  - Ayarlama **Protokolü** için **UDP**.
  - Ayarlama **IP adresi** Syslog sunucusu IP adresi.
  - Ayarlama **bağlantı noktası numarası** için **514**, veya bağlantı noktası kullanılacak aracınızı ayarlayın.
  - Ayarlama **tesis** Syslog aracıyı ayarladığınız bir (varsayılan olarak, aracı bu ayarlar **local4**).
  - Ayarlayabileceğiniz **maksimum sorgu dizesi boyutu** aracınızı içinde ayarladığınız boyuta.

## <a name="step-3-validate-connectivity"></a>3. Adım: Bağlantıyı doğrula

Çalınıyor Log Analytics'te görünmesini günlüklerinizi başlatana kadar 20 dakika sürebilir. 

1. Günlüklerinizi Syslog aracıyı doğru bağlantı noktasına aldığınızdan emin olun. Aracı makinede Syslog şu komutu çalıştırın: `tcpdump -A -ni any  port 514 -vv` Bu komut Syslog makineye CİHAZDAN akışı günlükleri gösterir. Günlükleri doğru bağlantı noktası ve doğru tesis kaynak gerecinde gelen alındığından emin olun.
2. Syslog cinini ve aracı arasındaki iletişim olup olmadığını denetleyin. Aracı makinede Syslog şu komutu çalıştırın: `tcpdump -A -ni any  port 25226 -vv` Bu komut Syslog makineye CİHAZDAN akışı günlükleri gösterir. Günlükleri de aracıda alındığından emin olun.
3. Bu komutların her ikisi de başarılı sonuçları sağlanan günlüklerinizi gelme görmek için Log Analytics kontrol edin. Bu gereçlerini akışa tüm olayları ham biçimde Log Analytics kapsamında görünen `CommonSecurityLog ` türü.
1. Hatalar varsa veya günlükleri gelen olmayan denetlemek için konum `tail /var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`
4. Syslog iletisi varsayılan boyutunuz (2 KB) 2048 bayt ile sınırlı olduğundan emin olun. Günlükleri çok uzun olması durumunda, bu komutu kullanarak security_events.conf güncelleştirin: `message_length_limit 4096`



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için F5 cihazları bağlayın öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).

