---
title: Linux destekli dağıtımlar | Microsoft Docs
description: Ubuntu, CentOS, Oracle ve SUSE yönergeleri de dahil olmak üzere, Azure destekli dağıtımlarda Linux hakkında bilgi edinin.
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: jeconnoc
editor: tysonn
tags: azure-service-management,azure-resource-manager
ms.assetid: 2777a526-c260-4cb9-a31a-bdfe1a55fffc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/21/2017
ms.author: szark
ms.openlocfilehash: 80a165c4533f555dce2d86f044f3caa328761123
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49645756"
---
# <a name="linux-on-distributions-endorsed-by-azure"></a>Tarafından Azure destekli dağıtımlarda Linux
İş ortakları, Azure Market'teki Linux görüntüleri sağlar. Desteklenen dağıtım listesine daha da fazla çeşitleme katmak çeşitli Linux topluluklarıyla çalışıyoruz. Bu sırada, Market'ten mevcut olmayan dağıtımlar her zaman kendi Linux yönergeleri izleyerek getirebilir [oluşturma ve karşıya yükleme Linux işletim sistemini içeren bir sanal sabit disk](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic).

## <a name="supported-distributions-and-versions"></a>Desteklenen dağıtımlar ve sürümler
Aşağıdaki tabloda Linux dağıtımları ve Azure üzerinde desteklenen sürümleri listelenmiştir. Başvurmak [Microsoft azure'da Linux görüntüleri için destek](https://support.microsoft.com/help/2941892/support-for-linux-and-open-source-technology-in-azure) Linux ve açık kaynak teknolojisi azure'da desteği hakkında daha ayrıntılı bilgi için.

Hyper-V ve Azure Linux Integration Services (LIS) sürücülerini Microsoft katkıda bulunan doğrudan Yukarı Akış Linux çekirdeğinin çekirdek modüllerdir.  Bazı LIS sürücüleri dağıtım'ın çekirdeğe varsayılan olarak oluşturulur. Red Hat Enterprise (RHEL) tabanlı eski dağıtımları veya CentOS ayrı bir indirme olarak kullanılabilir [Hyper-V ve Azure için Linux Tümleştirme hizmetleri sürüm 4.2](https://www.microsoft.com/en-us/download/details.aspx?id=55106). Bkz: [Linux çekirdek gereksinimleri](create-upload-generic.md#linux-kernel-requirements) LIS sürücüleri hakkında daha fazla bilgi için.

Azure Linux Aracısı, Azure Market görüntüleri üzerinde önceden yüklü olan ve dağıtım ait bir paket deposundaki genellikle kullanılabilir. Kaynak kodu bulunabilir [GitHub](https://github.com/azure/walinuxagent).

  
| Dağıtım | Sürüm | Sürücüler | Aracı |
| --- | --- | --- | --- |
| CentOS |CentOS 6.3 + 7.0 + |CentOS 6.3: [LIS indirin](https://www.microsoft.com/en-us/download/details.aspx?id=55106)<p>CentOS 6.4 +: çekirdek |Paket: İçinde [depo](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) "WALinuxAgent" altında <br/>Kaynak kodu: [GitHub](https://github.com/Azure/WALinuxAgent) |
| [CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/) |494.4.0+ |Çekirdek |Kaynak kodu: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent) |
| Debian |Debian 7,9 +, 8.2 + |Çekirdek |Paketi: "waagent" altında depoya <br/>Kaynak kodu: [GitHub](https://github.com/Azure/WALinuxAgent) |
| Oracle Linux |6.4+, 7.0+ |Çekirdek |Paketi: "WALinuxAgent" altında depoya <br/>Kaynak kodu: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998) |
| Red Hat Enterprise Linux |RHEL 6.7 + 7.1 + |Çekirdek |Paketi: "WALinuxAgent" altında depoya <br/>Kaynak kodu: [GitHub](https://github.com/Azure/WALinuxAgent) |
| SUSE Linux Enterprise |SAP için SLES/SLES<br>11 SP4<br>12 SP1 +<br>15|Çekirdek |Paket:<p> 11'de için [bulut: Araçları](https://build.opensuse.org/project/show/Cloud:Tools) depo<br>için "Genel bulut" modülünü "azure-python-agent" altında bulunan 12<br/>Kaynak kodu: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998) |
| openSUSE |openSUSE 42.2 + artık |Çekirdek |Paket: İçinde [bulut: Araçları](https://build.opensuse.org/project/show/Cloud:Tools) altındaki "azure-python-agent" depo <br/>Kaynak kodu: [GitHub](https://github.com/Azure/WALinuxAgent) |
| Ubuntu |Ubuntu 12.04 +  **<sup>1</sup>** |Çekirdek |Paketi: "walinuxagent" altında depoya <br/>Kaynak kodu: [GitHub](https://github.com/Azure/WALinuxAgent) |

  - **<sup>1</sup>**  azure'da Ubuntu 12.04 için destek başvurmak için lütfen [EOL'ye bildirimi](https://azure.microsoft.com/blog/ubuntu-12-04-precise-pangolin-nearing-end-of-life/).


## <a name="partners"></a>İş Ortakları

### <a name="coreos"></a>CoreOS
[https://coreos.com/docs/running-coreos/cloud-providers/azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

CoreOS Web sitesinden:

*CoreOS, güvenlik, tutarlılık ve güvenilirlik için tasarlanmıştır. Yum aracılığıyla veya apt paket yüklemek yerine, CoreOS Linux kapsayıcıları daha yüksek düzeyde soyutlama hizmetlerinizi yönetmek üzere kullanır. Tek bir hizmetin kodu ve tüm bağımlılıkları, bir veya daha çok CoreOS makinelerde çalıştırılabilir bir kapsayıcı içinde paketlenir.*

### <a name="credativ"></a>credativ
[http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure](http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ bağımsız danışmanlık ve ücretsiz yazılım kullanarak geliştirme ve uygulama profesyonel çözümlerinin uzmanlaşmış Hizmetleri şirket ' dir. Önde gelen açık kaynak uzmanlarına Credativ destek kullanan birçok BT departmanı ile uluslararası tanıma sahip. Microsoft ile birlikte, Credativ şu anda karşılık gelen bir Debian görüntüleri Debian 8 (Jessie) ve Debian 7 (Wheezy) önce hazırlanıyor. Her iki görüntüleri, Azure üzerinde çalışacak şekilde özel olarak tasarlanmıştır ve platformu kolayca yönetilebilir. Credativ uzun süreli Bakım ve kendi açık kaynak desteği merkezleri aracılığıyla Azure için Debian görüntülerini güncelleştirme de destekler.

### <a name="oracle"></a>Oracle
[http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Oracle'nın geniş bir Portföy ortak ve özel bulut çözümleri sunmak için stratejisidir. Stratejisi müşteriler, seçim ve Oracle bulutlarında Oracle yazılımları ve diğer bulutlarda nasıl dağıttıkları esneklik sağlar. Oracle ile Microsoft arasındaki iş ortaklığı sayesinde müşteriler, Oracle tarafından sağlanan sertifika ve desteğin verdiği güvenle Microsoft’un genel ve özel bulutlarında Oracle yazılımlarını dağıtabilir.  Oracle'nın taahhüt ve Oracle ortak ve özel bulut çözümleri yatırım değişmez.

### <a name="red-hat"></a>Red Hat
[http://www.redhat.com/en/partners/strategic-alliance/microsoft](http://www.redhat.com/en/partners/strategic-alliance/microsoft)

Açık kaynak çözümleri, dünyanın önde gelen sağlayıcısı, birden fazla iş sorunlarını çözmeye, kendi BT hizalama % 90 Fortune 500 şirketlerinin Red Hat yardımcı olur ve iş stratejilerine ve teknolojisinin geleceği için hazırlayın. Red Hat, bunu bir açık iş modeli ve uygun maliyetli ve öngörülebilir abonelik modeli aracılığıyla güvenli çözümler sağlayarak yapar.

### <a name="suse"></a>SUSE
[http://www.suse.com/suse-linux-enterprise-server-on-azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux Enterprise Server azure'da daha üstün güvenilirlik ve güvenlik için bulut bilgi işlem sağlayan bir kendini kanıtlamış platformudur. SUSE'ın çok yönlü Linux platformuna bir kolayca yönetilebilen bir bulut ortamında sunmak için Azure bulut Hizmetleri ile sorunsuz şekilde entegre olur. SUSE Linux Enterprise Server için 1800'den fazla bağımsız yazılım satıcıları birden fazla 9,200 sertifikalı uygulamaları ile desteklenen veri merkezinde çalışan iş yüklerini Azure'da güvenle dağıtılabilir SUSE sağlar.

### <a name="canonical"></a>Canonical
[http://www.ubuntu.com/cloud/azure](http://www.ubuntu.com/cloud/azure)

Kurallı mühendislik ve açık bir topluluk idare sürücü Ubuntu'nın başarı, istemci, sunucu ve bulut bilgi işlem Tüketiciler için kişisel bulut hizmetlerini içerir. Canonical Ubuntu, birleşik, ücretsiz platform buluta, Phone sunulmasıyla, telefon, tablet, TV ve Masaüstü için tutarlı arabirimleri ailesi sağlar. Bu işleme, tüketici elektroniği üreticileri ve sık kullanılan arasında bireysel ekiplerindeki genel bulut sağlayıcılarından farklı kurumlar için gereken ilk seçim Ubuntu hale getirir.

Geliştiriciler ve dünyanın dört bir yanındaki mühendislik merkezleri, Canonical donanım üreticileri, içerik sağlayıcıları ve yazılım geliştiricileri bilgisayarları, sunucuları ve taşınabilir cihazları pazar için Ubuntu çözümleri ile ortaklık kurmak için benzersiz olarak konumlandırıldı.
