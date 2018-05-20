---
title: Log Analytics'te Wire Data çözümü | Microsoft Docs
description: Sinyal verileri, Operations Manager ve Windows bağlantılı aracılar da dahil olmak üzere OMS aracılarıyla bilgisayarlardan ağ ve performans verilerini bir araya getirir. Verilerin bağıntısını sağlamanıza yardımcı olmak için ağ verileri günlük verilerinizle birleştirilir.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: fc3d7127-0baa-4772-858a-5ba995d1519b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: magoedte
ms.openlocfilehash: c86d1274ed46ff725c9db3093a8852fbae7f67ff
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="wire-data-20-preview-solution-in-log-analytics"></a>Log Analytics'te Wire Data 2.0 (Önizleme) çözümü

![Wire Data sembolü](./media/log-analytics-wire-data/wire-data2-symbol.png)

Sinyal verileri OMS aracısı tarafından, ortamınızda Operations Manager tarafından izlenenler de dahil olmak üzere Windows bağlantılı ve Linux bağlantılı bilgisayarlardan toplanan ağ ve performans verilerini bir araya getirir. Verilerin bağıntısını sağlamanıza yardımcı olmak için ağ verileri diğer günlük verilerinizle birleştirilir.

Wire Data çözümü, OMS aracısına ek olarak BT altyapınızdaki bilgisayarlara yüklediğiniz Microsoft Uyumluluk Aracılarını da kullanır. Bağımlılık Aracıları [OSI modelinde](https://en.wikipedia.org/wiki/OSI_model) 2-3 ağ düzeyleri için bilgisayarlarınıza ve bilgisayarlarınızdan gönderilen ağ verilerini, ayrıca kullanılan çeşitli protokollerle bağlantı noktalarını izler. Ardından aracılar kullanılarak veriler Log Analytics'e gönderilir.  

> [!NOTE]
> Wire Data çözümünün önceki sürümünü yeni çalışma alanlarına ekleyemezsiniz. Özgün Wire Data çözümünü daha önce etkinleştirdiyseniz, bunu kullanmaya devam edebilirsiniz. Öte yandan, Wire Data 2.0'ı kullanmak için önce özgün sürümü kaldırmanız gerekir.

Varsayılan olarak, Log Analytics Windows ve Linux'ın yerleşik sayaçlarıyla belirttiğiniz diğer performans sayaçlarından gelen CPU, bellek, disk ve ağ performansına ilişkin verileri günlüğe kaydeder. Her aracı için, alt ağlar ve bilgisayar tarafından kullanılmakta olan uygulama düzeyi protokoller de dahil olmak üzere, ağ ve diğer verileri toplama işlemi gerçek zamanlı olarak yapılır.  Wire Data, alttaki TCP aktarım katmanında değil uygulama düzeyindeki ağ verilerine bakar.  Çözüm tek tek ACK'lere ve SYN'lere bakmaz.  Karşılıklı anlaşma tamamlandıktan sonra, bu canlı bir bağlantı olarak kabul edilir ve Bağlandı olarak işaretlenir. Söz konusu bağlantı, her iki taraf da yuvanın açık olduğunu ve verilerin ileri ve geri geçiş yapabildiğini kabul ettiği sürece canlı kalır.  İki taraftan biri bağlantıyı kapatınca Bağlantı Kesildi olarak işaretlenir.  Bu nedenle, yalnızca başarıyla tamamlanan paketlerin bant genişliğini sayar; yeniden göndermeleri veya başarısız paketleri raporlamaz.

[Cisco'nun NetFlow protokolüyle](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html) [sFlow](http://www.sflow.org/)'u veya başka bir yazılımı kullandıysanız, sinyal verilerinden gördüğünüz istatistikler ve veriler tanıdık gelecektir.

Yerleşik Günlük arama sorgusu türlerinden bazıları şunlardır:

- Sinyal verileri sağlayan aracılar
- Sinyal verilerini sağlayan aracıların IP adresleri
- IP adresinden giden iletişimler
- Uygulama protokolleri tarafından gönderilen bayt sayısı
- Uygulama hizmeti tarafından gönderilen bayt sayısı
- Farklı protokoller tarafından alınan bayt sayısı
- IP sürümü tarafından gönderilen ve alınan toplam bayt sayısı
- Güvenilir şekilde ölçülmüş bağlantılarda ortalama gecikme süresi
- Ağ trafiği başlatan veya alan bilgisayar işlemleri
- İşlem için ağ trafiği miktarı

Sinyal verilerini kullanarak arama yaptığınızda, en önemli aracılar ve en önemli protokoller hakkındaki bilgileri görüntülemek için verileri filtreleyebilir ve gruplandırabilirsiniz. Öte yandan bazı bilgisayarların (IP adresleri/MAC adresleri) birbiriyle ne zaman, ne kadar süreyle iletişim kurduğunu ve ne kadar veri gönderildiği de görüntüleyebilirsiniz. Temelde, ağ trafiği hakkında arama tabanlı olan meta verileri görüntülersiniz.

Bununla birlikte, meta verileri görüntülediğiniz için bunların kapsamlı bir sorun giderme işleminde kullanışlı olması şart değildir. Log Analytics'teki sinyal verileri, ağ verilerinin tümüyle yakalanması anlamına gelmez.  Derin paket düzeyi sorun giderme işlemlerine yönelik değildir. Aracının kullanılmasının diğer toplama yöntemlerine göre avantajı, gereçler takmanızın, ağ anahtarlarınızı yeniden yapılandırmanızın veya karmaşık yapılandırmalar gerçekleştirmenizin gerekmemesidir. Sinyal verileri yalnızca aracı tabanlıdır; bilgisayara aracıyı yüklersiniz ve o da kendi ağ trafiğini izler. Bir diğer avantajı da, kullanıcının yapı katmanına sahip olmadığı bulut sağlayıcılarında ya da barındıran hizmet sağlayıcısında veya Microsoft Azure'da çalıştırılan iş yüklerini izlemek istediğinizde ortaya çıkar.

## <a name="connected-sources"></a>Bağlı kaynaklar

Wire Data verilerini Microsoft Bağımlılık Aracısı'ndan alır. Bağımlılık Aracısı, Log Analytics ile arasındaki bağlantılar için OMS Aracısına bağımlıdır. Başka bir deyişle, Bağımlılık Aracısı'nı yüklemeniz için sunucuda önce OMS Aracısı yüklü ve yapılandırılmış olmalıdır. Aşağıdaki tabloda Wire Data çözümü tarafından desteklenen bağlı kaynaklar açıklanır:

| **Bağlı kaynak** | **Destekleniyor** | **Açıklama** |
| --- | --- | --- |
| Windows aracıları | Yes | Wire Data, Windows aracı bilgisayarlarından gelen verileri analiz eder ve toplar. <br><br> [OMS Aracısı](log-analytics-windows-agent.md)'na ek olarak, Windows aracılarına Microsoft Bağımlılık Aracısı da gerekir. İşletim sistemi sürümlerinin tam listesi için bkz. [Desteklenen işletim sistemleri](../monitoring/monitoring-service-map-configure.md#supported-operating-systems). |
| Linux aracıları | Yes | Wire Data, Linux aracı bilgisayarlarından gelen verileri analiz eder ve toplar.<br><br> [OMS Aracısı](log-analytics-quick-collect-linux-computer.md)'na ek olarak, Linux aracılarına Microsoft Bağımlılık Aracısı da gerekir. İşletim sistemi sürümlerinin tam listesi için bkz. [Desteklenen işletim sistemleri](../monitoring/monitoring-service-map-configure.md#supported-operating-systems). |
| System Center Operations Manager yönetim grubu | Yes | Wire Data, bağlantılı bir [System Center Operations Manager yönetim grubunda](log-analytics-om-agents.md) Windows ve Linux aracılarından gelen verileri analiz eder ve toplar. <br><br> System Center Operations Manager aracısının doğrudan Log Analytics’e bağlanması gerekir. Veriler yönetim grubundan Log Analytics'e iletilir. |
| Azure depolama hesabı | Hayır | Wire Data verileri aracı bilgisayarlardan topladığından, Azure Depolama'dan toplayacağı veri yoktur. |

Windows'da, hem System Center Operations Manager hem de Log Analytics verileri toplamak ve göndermek için Microsoft Monitoring Agent (MMA) kullanır. Bağlama göre, bu aracı System Center Operations Manager Aracısı, OMS Aracısı, Log Analytics Aracısı, MMA veya Doğrudan Aracı olarak adlandırılır. System Center Operations Manager ve Log Analytics, MMA'nın biraz farklı sürümlerini sağlar. Bu sürümlerin her biri System Center Operations Manager'a, Log Analytics'e veya her ikisine birden raporlayabilir.

Linux'ta, verileri Linux için OMS Aracısı toplar ve Log Analytics'e gönderir. Wire Data'yı OMS Doğrudan Aracılarının bulunduğu sunucularda veya System Center Operations Manager yönetim grupları üzerinden Log Analytics'e bağlanan sunucularda kullanabilirsiniz.

Bu makalede, hem Linux hem de Windows'da ister System Center Operations Manager yönetim grubuna ister doğrudan Log Analytics'e bağlı olsun tüm aracılar için _OMS aracısı_ terimi kullanılır. Aracının dağıtım adını ancak bağlam için gerekli olduğunda kullanacağız.

Bağımlılık Aracısı'nın kendisi hiçbir veri iletmez ve güvenlik duvarları veya bağlantı noktalarında hiçbir değişiklik yapılmasını gerektirmez. Wire Data'daki veriler Log Analytics'e doğrudan veya OMS Ağ Geçidi kullanılarak her zaman OMS aracısı tarafından iletilir.

![aracı diyagramı](./media/log-analytics-wire-data/agents.png)

Log Analytics'e bağlı bir yönetim grubuyla System Center Operations Manager kullanıcısıysanız:

- System Center Operations Manager aracıları Log Analytics'e bağlanmak üzere İnternet'e erişirken ek yapılandırma gerekmez.
- System Center Operations Manager aracılarınız İnternet üzerinden Log Analytics'e erişemediğinde OMS Ağ Geçidi'ni System Center Operations Manager ile çalışacak şekilde yapılandırmanız gerekir.

Doğrudan Aracı kullanıyorsanız, Log Analytics'e veya OMS Ağ Geçidinize bağlanması için OMS aracısının kendisini yapılandırmanız gerekir. OMS Ağ Geçidi'ni [Microsoft İndirme Merkezi](https://www.microsoft.com/download/details.aspx?id=52666)'nden indirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- [İçgörü ve Analiz](https://www.microsoft.com/cloud-platform/operations-management-suite-pricing) çözüm teklifi gereklidir.
- Wire Data çözümünü önceki sürümünü kullanıyorsanız, önce o sürümü kaldırmalısınız. Ancak özgün Wire Data çözümüyle yakalanmış olan tüm veriler Wire Data 2.0'da ve günlük aramasında yine kullanılabilir.
- Bağımlılık Aracısı'nı yükleme veya kaldırmak için yönetici ayrıcalıkları gereklidir.
- Bağımlılık Aracısı 64 bit işletim sistemine sahip bir bilgisayara yüklenmelidir.

### <a name="operating-systems"></a>İşletim sistemleri

Aşağıdaki bölümlerde Bağımlılık Aracısı için desteklenen işletim sistemleri listelenir. Wire Data hiçbir işletim sisteminin 32 bit mimarilerini desteklemez.

#### <a name="windows-server"></a>Windows Server

- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

#### <a name="windows-desktop"></a>Windows masaüstü

- Windows 10
- Windows 8.1
- Windows 8
- Windows 7

#### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Red Hat Enterprise Linux, CentOS Linux ve Oracle Linux (RHEL Çekirdeği ile)

- Yalnızca varsayılan ve SMP Linux çekirdek sürümleri desteklenir.
- PAE ve Xen gibi standart dışı çekirdek sürümleri hiçbir Linux dağıtımında desteklenmez. Örneğin, sürüm dizesi _2.6.16.21-0.8-xen_ olan bir sistem desteklenmez.
- Standart çekirdeklerin yeniden derlemeleri de dahil olmak üzere özel çekirdekler desteklenmez.
- CentOSPlus çekirdeği desteklenmez.
- Oracle Unbreakable Enterprise Kernel (UEK), bu makalenin sonraki bir bölümünde ele alınmıştır.

#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| **İşletim sistemi sürümü** | **Çekirdek sürümü** |
| --- | --- |
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| **İşletim sistemi sürümü** | **Çekirdek sürümü** |
| --- | --- |
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4 | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6.7 | 2.6.32-573 |
| 6.8 | 2.6.32-642 |

#### <a name="red-hat-linux-5"></a>Red Hat Linux 5

| **İşletim sistemi sürümü** | **Çekirdek sürümü** |
| --- | --- |
| 5.8 | 2.6.18-308 |
| 5.9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398 <br> 2.6.18-400 <br>2.6.18-402 <br>2.6.18-404 <br>2.6.18-406 <br> 2.6.18-407 <br> 2.6.18-408 <br> 2.6.18-409 <br> 2.6.18-410 <br> 2.6.18-411 <br> 2.6.18-412 <br> 2.6.18-416 <br> 2.6.18-417 <br> 2.6.18-419 |

#### <a name="oracle-enterprise-linux-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux ve Unbreakable Enterprise Kernel

#### <a name="oracle-linux-6"></a>Oracle Linux 6

| **İşletim sistemi sürümü** | **Çekirdek sürümü** |
| --- | --- |
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5

| **İşletim sistemi sürümü** | **Çekirdek sürümü** |
| --- | --- |
| 5.8 | Oracle 2.6.32-300 (UEK R1) |
| 5.9 | Oracle 2.6.39-300 (UEK R2) |
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11

| **İşletim sistemi sürümü** | **Çekirdek sürümü** |
| --- | --- |
| 11 | 2.6.27 |
| 11 SP1 | 2.6.32 |
| 11 SP2 | 3.0.13 |
| 11 SP3 | 3.0.76 |
| 11 SP4 | 3.0.101 |

#### <a name="suse-linux-10"></a>SUSE Linux 10

| **İşletim sistemi sürümü** | **Çekirdek sürümü** |
| --- | --- |
| 10 SP4 | 2.6.16.60 |

#### <a name="dependency-agent-downloads"></a>Bağımlılık Aracısı indirmeleri

| **Dosya** | **OS** | **Sürüm** | **SHA-256** |
| --- | --- | --- | --- |
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.0.5 | 73B3F6A2A76A08D58F72A550947FF839B588591C48E6EDDD6DDF73AA3FD82B43 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.0.5 | A1BAD0B36EBF79F2B69113A07FCF48C68D90BD169C722689F9C83C69FC032371 |



## <a name="configuration"></a>Yapılandırma

Çalışma alanlarınızda Wire Data çözümünü yapılandırmak için aşağıdaki adımları uygulayın.

1. Etkinlik Günlüğü Analizi çözümünü [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WireData2OMS?tab=Overview)'ten veya [Çözüm Galerisi'nden Log Analytics çözümleri ekleme](log-analytics-add-solutions.md) başlığı altında açıklanan işlemi kullanarak etkinleştirin.
2. Veri almak istediğiniz her bilgisayara Bağımlılık Aracısı'nı yükleyin. Bağımlılık Aracısı en yakındaki komşularla bağlantıları izleyebildiğinden her bilgisayarda bir aracıya ihtiyacınız olmayabilir.

### <a name="install-the-dependency-agent-on-windows"></a>Windows'da Bağımlılık Aracısı'nı yükleme

Aracıyı yüklemek veya kaldırmak için yönetici ayrıcalıkları gereklidir.

Bağımlılık Aracısı, Windows çalıştıran bilgisayarlara InstallDependencyAgent-Windows.exe ile yüklenir. Bu yürütülebilir dosyayı hiçbir seçenek olmadan çalıştırırsanız, etkileşimli yükleme yapmak için izleyebileceğiniz bir sihirbaz başlatır.

Windows çalıştıran her bilgisayara Bağımlılık Aracısı'nı yüklemek için aşağıdaki adımları kullanın:

1. [Ortamınızda barındırılan Windows bilgisayarlarından veri toplama](log-analytics-windows-agent.md) başlığı altında verilen adımları izleyerek OMS Aracısı'nı yükleyin.
2. Önceki bölümde verilen bağlantıyı kullanarak Windows Bağımlılık Aracısı'nı indirin ve ardından şu komutu kullanarak aracıyı çalıştırın: `InstallDependencyAgent-Windows.exe`
3. Sihirbazı izleyerek aracıyı yükleyin.
4. Bağımlılık Aracısı başlatılamazsa, ayrıntılı hata bilgileri için günlükleri denetleyin. Windows aracıları için günlük dizini: %Programfiles%\Microsoft Dependency Agent\logs.

#### <a name="windows-command-line"></a>Windows komut satırı

Komut satırından yüklemek için aşağıdaki tabloda yer alan seçenekleri kullanın. Yükleme bayraklarının listesini görmek için yükleyiciyi çalıştırırken aşağıda gösterildiği gibi /? bayrağını kullanın.

InstallDependencyAgent-Windows.exe /?

| **Bayrak** | **Açıklama** |
| --- | --- |
| <code>/?</code> | Komut satırı seçeneklerinin listesini alır. |
| <code>/S</code> | Kullanıcıdan bilgi istenmeden sessiz yükleme gerçekleştirir. |

Windows Bağımlılık Aracısı'nın dosyaları varsayılan olarak C:\Program Files\Microsoft Dependency Agent konumuna yerleştirilir.

### <a name="install-the-dependency-agent-on-linux"></a>Linux'ta Bağımlılık Aracısı'nı yükleme

Aracıyı yüklemek veya yapılandırmak için kök erişimi gerekir.

Bağımlılık Aracısı Linux bilgisayarlarına kendi kendine ayıklama ikili dosyası içeren InstallDependencyAgent-Linux64.bin kabuk betiğiyle yüklenir. Dosyayı çalıştırmak için _sh_ kullanın veya dosyanın kendisine yürütme izinleri ekleyin.

Her Linux bilgisayarına Bağımlılık Aracısı'nı yüklemek için aşağıdaki adımları kullanın:

1. [Ortamınızda barındırılan Linux bilgisayarlarından veri toplama](log-analytics-quick-collect-linux-computer.md#obtain-workspace-id-and-key) başlığı altında verilen adımları izleyerek OMS Aracısı'nı yükleyin.
2. Önceki bölümde verilen bağlantıyı kullanarak Linux Bağımlılık Aracısı'nı indirin ve ardından bunu kök olarak yüklemek için şu komutu kullanın: sh InstallDependencyAgent-Linux64.bin
3. Bağımlılık Aracısı başlatılamazsa, ayrıntılı hata bilgileri için günlükleri denetleyin. Linux aracıları için günlük dizini: /var/opt/microsoft/dependency-agent/log.

Yükleme bayraklarının listesini görmek için, yükleme programını aşağıda gösterildiği gibi `-help` bayrağıyla çalıştırın.

```
InstallDependencyAgent-Linux64.bin -help
```

| **Bayrak** | **Açıklama** |
| --- | --- |
| <code>-help</code> | Komut satırı seçeneklerinin listesini alır. |
| <code>-s</code> | Kullanıcıdan bilgi istenmeden sessiz yükleme gerçekleştirir. |
| <code>--check</code> | İzinleri ve işletim sistemini denetleyin ama aracıyı yüklemeyin. |

Bağımlılık Aracısı'nın dosyaları aşağıdaki dizinlere yerleştirilir:

| **Dosyalar** | **Konum** |
| --- | --- |
| Çekirdek dosyaları | /opt/microsoft/dependency-agent |
| Günlük dosyaları | /var/opt/microsoft/dependency-agent/log |
| Yapılandırma dosyaları | /etc/opt/microsoft/dependency-agent/config |
| Hizmet yürütülebilir dosyaları | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br><br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| İkili depolama dosyaları | /var/opt/microsoft/dependency-agent/storage |

### <a name="installation-script-examples"></a>Yükleme betiği örnekleri

Bağımlılık Aracısı'nı bir kerede birçok sunucuya kolayca dağıtmak için, betik kullanmak yararlı olabilir. Windows veya Linux'a Bağımlılık Aracısı'nı indirip yüklemek için aşağıdaki betik örneklerini kullanabilirsiniz.

#### <a name="powershell-script-for-windows"></a>Windows için PowerShell betiği

```PowerShell

Invoke-WebRequest &quot;https://aka.ms/dependencyagentwindows&quot; -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S

```

#### <a name="shell-script-for-linux"></a>Linux için kabuk betiği

```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
```

```
sh InstallDependencyAgent-Linux64.bin -s
```

### <a name="desired-state-configuration"></a>İstenen Durum Yapılandırması

Bağımlılık Aracısı'nı Desired State Configuration yoluyla dağıtmak için, xPSDesiredStateConfiguration modülünü ve aşağıdaki gibi küçük bir kod kullanabilirsiniz:

```
Import-DscResource -ModuleName xPSDesiredStateConfiguration

$DAPackageLocalPath = &quot;C:\InstallDependencyAgent-Windows.exe&quot;



Node $NodeName

{

    # Download and install the Dependency Agent

    xRemoteFile DAPackage

    {

        Uri = &quot;https://aka.ms/dependencyagentwindows&quot;

        DestinationPath = $DAPackageLocalPath

        DependsOn = &quot;[Package]OI&quot;

    }

    xPackage DA

    {

        Ensure=&quot;Present&quot;

        Name = &quot;Dependency Agent&quot;

        Path = $DAPackageLocalPath

        Arguments = '/S'

        ProductId = &quot;&quot;

        InstalledCheckRegKey = &quot;HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent&quot;

        InstalledCheckRegValueName = &quot;DisplayName&quot;

        InstalledCheckRegValueData = &quot;Dependency Agent&quot;

    }

}

```
### <a name="uninstall-the-dependency-agent"></a>Bağımlılık Aracısı'nı kaldırma

Aşağıdaki bölümler Bağımlılık Aracısı'nı kaldırmanıza yardımcı olabilir.

#### <a name="uninstall-the-dependency-agent-on-windows"></a>Windows'da Bağımlılık Aracısı'nı kaldırma

Yöneticiler, Denetim Masası aracılığıyla Windows için Bağımlılık Aracısı'nı kaldırabilir.

Bir yönetici Bağımlılık Aracısı'nı kaldırmak için %Programfiles%\Microsoft Dependency Agent\Uninstall.exe'yi çalıştırabilir.

#### <a name="uninstall-the-dependency-agent-on-linux"></a>Linux'ta Bağımlılık Aracısı'nı kaldırma

Bağımlılık Aracısı'nı Linux'tan tümüyle kaldırmak için, hem aracının kendisini hem de aracıyla birlikte otomatik olarak yüklenen bağlayıcıyı kaldırmalısınız. Aşağıdaki tek komutu kullanarak ikisini de kaldırabilirsiniz:

```
rpm -e dependency-agent dependency-agent-connector
```

## <a name="management-packs"></a>Yönetim paketleri

Log Analytics çalışma alanında Wire Data etkinleştirildiğinde, söz konusu çalışma alanındaki tüm Windows sunucularına 300 KB'lık bir yönetim paketi gönderilir. System Center Operations Manager aracılarını bir [bağlı yönetim grubunda](log-analytics-om-agents.md) kullanıyorsanız, System Center Operations Manager'dan Bağımlılık İzleyicisi yönetim paketi dağıtılır. Aracılar doğrudan bağlıysa yönetim paketini Log Analytics verir.

Yönetim paketinin adı Microsoft.IntelligencePacks.ApplicationDependencyMonitor'dır. Şu konuma yazılır: %Programfiles%\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs. Yönetim paketi şu veri kaynağını kullanır: %Program files%\Microsoft Monitoring Agent\Agent\Health Service State\Resources&lt;AutoGeneratedID&gt;\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.

## <a name="using-the-solution"></a>Çözümü kullanma

**Çözümü yükleme ve yapılandırma**

Çözümü yüklemek ve yapılandırmak için aşağıdaki bilgileri kullanın.

- Wire Data çözümü Windows Server 2012 R2, Windows 8.1 ve daha sonraki işletim sistemlerini çalıştıran bilgisayarlardan veri alır.
- Sinyal verilerini almak istediğiniz bilgisayarlarda Microsoft .NET Framework 4.0 veya üstü bulunmalıdır.
- [Çözüm Galerisi'nden Log Analytics çözümleri ekleme](log-analytics-add-solutions.md) başlığı altında açıklanan işlemi kullanarak Wire Data çözümünü Log Analytics çalışma alanınıza ekleyin. Başka bir yapılandırma işlemi gerekmez.
- Belirli bir çözümle ilişkili sinyal verilerini görüntülemek istiyorsanız, çözümün çalışma alanınıza önceden eklenmiş olması gerekir.

Aracılarınız yüklendikten ve siz çözümü yükledikten sonra, çalışma alanınızda Wire Data 2.0 kutucuğu gösterilir.

![Wire Data kutucuğu](./media/log-analytics-wire-data/wire-data-tile.png)

## <a name="using-the-wire-data-20-solution"></a>Wire Data 2.0 çözümünü kullanma

Azure portalında Log Analytics çalışma alanınızın **Genel bakış** sayfasında **Wire Data 2.0** kutucuğuna tıklayarak Wire Data panosunu açın. Pano aşağıdaki tabloda gösterilen dikey pencereleri içerir. Her dikey pencerede, dikey pencerenin belirtilen kapsam ve zaman aralığına yönelik ölçütleriyle eşleşen en fazla 10 öğe listelenir. Dikey pencerenin altındaki **Tümünü göster**’e tıklayarak veya dikey pencere başlığına tıklayarak tüm kayıtları döndüren bir günlük araması yapabilirsiniz.

| **Dikey pencere** | **Açıklama** |
| --- | --- |
| Ağ trafiğini yakalayan aracılar | Ağ trafiğini yakalayan aracıların sayısını gösterir ve trafiği yakalayan ilk 10 bilgisayarı listeler. <code>Type:WireData &#124; measure Sum(TotalBytes) by Computer &#124; top 500000</code> günlük araması çalıştırmak için sayıya tıklayın. Yakalanan toplam bayt sayısını döndüren bir günlük araması çalıştırmak için listedeki bir bilgisayara tıklayın. |
| Yerel Alt Ağlar | Aracıların keşfettiği yerel alt ağların sayısını gösterir.  Tüm alt ağları ve her birinden gönderilen bayt sayısını listeleyen bir <code>Type:WireData &#124; Measure Sum(TotalBytes) by LocalSubnet</code> günlük araması çalıştırmak için sayıya tıklayın. Alt ağ üzerinden gönderilen toplam bayt sayısını döndüren bir günlük araması çalıştırmak için listedeki bir alt ağa tıklayın. |
| Uygulama Düzeyi Protokolleri | Aracılar tarafından keşfedilen, kullanımdaki uygulama düzeyi protokollerinin sayısını gösterir. <code>Type:WireData &#124; Measure Sum(TotalBytes) by ApplicationProtocol</code> günlük araması çalıştırmak için sayıya tıklayın. Protokol kullanılarak gönderilen toplam bayt sayısını döndüren bir günlük araması çalıştırmak için listedeki bir protokole tıklayın. |

[!INCLUDE[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Wire Data panosu](./media/log-analytics-wire-data/wire-data-dash.png)

**Ağ trafiğini yakalayan aracılar** dikey penceresini kullanarak bilgisayarlar tarafından kullanılmakta olan ağ genişliğini miktarını belirleyebilirsiniz. Bu dikey pencere ortamınızdaki en _geveze_ bilgisayarı kolayca bulmanıza yardımcı olabilir. Bu tür bilgisayarlar aşırı yüklenmiş, anormal çalışıyor veya normalin üzerinde ağ kaynağı kullanıyor olabilir.

![günlük araması örneği](./media/log-analytics-wire-data/log-search-example01.png)

Benzer biçimde, **Yerel Alt Ağlar** dikey penceresini kullanarak alt ağlarınız üzerinden ne kadar ağ trafiği taşındığını belirleyebilirsiniz. Kullanıcılar alt ağları çoğunlukla uygulamalarının kritik alanları çevresinde tanımlar. Bu dikey pencere söz konusu alanların görülmesini sağlar.

![günlük araması örneği](./media/log-analytics-wire-data/log-search-example02.png)

**Uygulama Düzeyi Protokolleri** hangi protokollerin kullanımda olduğunu öğrenmenize yardımcı olduğundan, yararlı bir dikey penceredir. Örneğin, ağ ortamınızda SSH'nin kullanımda olmamasını bekliyor olabilirsiniz. Dikey pencerede sağlanan bilgileri görüntüleyerek bu beklentinizin doğru olup olmadığını hızla anlayabilirsiniz.

![günlük araması örneği](./media/log-analytics-wire-data/log-search-example03.png)

Bu örnekte, hangi bilgisayarların SSH kullandığını ve daha birçok iletişim ayrıntısını görmek için SSH bilgilerinde detaya gidebilirsiniz.

![ssh arama sonuçları](./media/log-analytics-wire-data/ssh-details.png)

Ayrıca protokol trafiğinin zaman içinde arttığını mı yoksa azaldığını mı bilmek de yararlı olur. Örneğin, bir uygulama tarafından iletilen verilerin miktarı artıyorsa, bu farkında olmanız gereken bir durum veya dikkate değer bulduğunuz bir bilgi olabilir.

## <a name="input-data"></a>Giriş verileri

Sinyal verileri, etkinleştirilmiş olan aracıları kullanarak ağ trafiği hakkındaki meta verileri toplar. Her aracı yaklaşık her 15 saniyede bir veri gönderir.

## <a name="output-data"></a>Çıktı verileri

Her giriş verileri türü için _WireData_ türünde bir kayıt oluşturulur. Aşağıdaki tabloda WireData kayıtlarının özellikleri gösterilmiştir:

| Özellik | Açıklama |
|---|---|
| Bilgisayar | Verilerin toplandığı bilgisayarın adı |
| TimeGenerated | Kaydın zamanı |
| LocalIP | Yerel bilgisayarın IP adresi |
| SessionState | Bağlantılı veya bağlantısı kesilmiş |
| ReceivedBytes | Alınan bayt miktarı |
| ProtocolName | Kullanılan ağ protokolünün adı |
| IPVersion | IP sürümü |
| Yön | Gelen veya giden |
| MaliciousIP | Bilinen kötü amaçlı kaynağın IP adresi |
| Severity | Kötü amaçlı olduğundan şüphe edilen yazılımın önem derecesi |
| RemoteIPCountry | Uzak IP adresinin ülkesi |
| ManagementGroupName | Operations Manager yönetim grubunun adı |
| SourceSystem | Verilerin toplandığı kaynak |
| SessionStartTime | Oturumun başlangıç saati |
| SessionEndTime | Oturumun bitiş saati |
| LocalSubnet | Verilerin toplandığı alt ağ |
| LocalPortNumber | Yerel bağlantı noktası numarası |
| RemoteIP | Uzak bilgisayar tarafından kullanılan uzak IP adresi |
| RemotePortNumber | Uzak IP adresi tarafından kullanılan bağlantı noktası numarası |
| SessionID | İki IP adresi arasındaki iletişim oturumunu tanımlayan benzersiz bir değer |
| SentBytes | Gönderilen bayt sayısı |
| TotalBytes | Oturum sırasında gönderilen toplam bayt sayısı |
| ApplicationProtocol | Kullanılan ağ protokolünün türü   |
| ProcessID | Windows işlem kimliği |
| ProcessName | İşlemin yıl ve dosya adı |
| RemoteIPLongitude | IP boylam değeri |
| RemoteIPLatitude | IP enlem değeri |


## <a name="next-steps"></a>Sonraki adımlar

- Ayrıntılı sinyal verileri arama kayıtlarını görüntülemek için [günlüklerde arama yapın](log-analytics-log-searches.md).
