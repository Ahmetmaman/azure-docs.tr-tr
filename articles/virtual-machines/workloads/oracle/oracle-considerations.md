---
title: Microsoft Azure üzerinde Oracle çözümleri | Microsoft Docs
description: Desteklenen yapılandırmalar ve sınırlamalar, Microsoft Azure üzerinde Oracle çözümleri hakkında bilgi edinin.
services: virtual-machines-linux
documentationcenter: ''
author: romitgirdhar
manager: jeconnoc
tags: azure-resource-management
ms.assetid: 5d71886b-463a-43ae-b61f-35c6fc9bae25
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/02/2018
ms.author: rogirdh
ms.openlocfilehash: 6435c866f6cdf5abea3862a718579f3a6e4d7378
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39493073"
---
# <a name="oracle-solutions-and-their-deployment-on-microsoft-azure"></a>Oracle çözümleri ve dağıtım üzerinde Microsoft Azure
Bu makalede, Microsoft Azure üzerinde çeşitli Oracle çözümlerini başarıyla dağıtmak için gerekli bilgiler ele alınmaktadır. Bu çözümler Azure marketi, Oracle tarafından yayımlanan sanal makine görüntüleri dayanır. Şu anda kullanılabilir görüntülerin listesini almak için aşağıdaki komutu çalıştırın:
```azurecli-interactive
az vm image list --publisher oracle -o table --all
```
6-16-2017 itibarıyla görüntüleri listesi aşağıda verilmiştir:
```bash
Offer                   Publisher    Sku                     Urn                                                          Version
----------------------  -----------  ----------------------  -----------------------------------------------------------  -------------
Oracle-Database-Ee      Oracle       12.1.0.2                Oracle:Oracle-Database-Ee:12.1.0.2:12.1.20170202             12.1.20170202
Oracle-Database-Se      Oracle       12.1.0.2                Oracle:Oracle-Database-Se:12.1.0.2:12.1.20170202             12.1.20170202
Oracle-Linux            Oracle       6.4                     Oracle:Oracle-Linux:6.4:6.4.20141206                         6.4.20141206
Oracle-Linux            Oracle       6.7                     Oracle:Oracle-Linux:6.7:6.7.20161007                         6.7.20161007
Oracle-Linux            Oracle       6.8                     Oracle:Oracle-Linux:6.8:6.8.20161020                         6.8.20161020
Oracle-Linux            Oracle       6.9                     Oracle:Oracle-Linux:6.9:6.9.20170406                         6.9.20170406
Oracle-Linux            Oracle       7.0                     Oracle:Oracle-Linux:7.0:7.0.20141217                         7.0.20141217
Oracle-Linux            Oracle       7.2                     Oracle:Oracle-Linux:7.2:7.2.20161020                         7.2.20161020
Oracle-Linux            Oracle       7.3                     Oracle:Oracle-Linux:7.3:7.3.20170320                         7.3.20170320
Oracle-WebLogic-Server  Oracle       Oracle-WebLogic-Server  Oracle:Oracle-WebLogic-Server:Oracle-WebLogic-Server:12.1.2  12.1.2
```

Bu görüntüler, "Kendi lisansını getir" olarak kabul edilir ve bu nedenle, yalnızca işlem, depolama ve ağ maliyetleri VM çalıştırmaktan kaynaklanan için ücretlendirilirsiniz.  Oracle yazılımı ve Oracle ile yerinde geçerli bir destek sözleşmesi olması kullanmak için düzgün şekilde lisanslandığından varsayılır. Oracle lisans taşınabilirliği şirket içinden Azure'a garantisi. Yayımlanan bkz [Oracle ve Microsoft](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html) Not lisans taşınabilirliği hakkında ayrıntılı bilgi için. 

Kişiler de azure'da sıfırdan oluşturabilir veya kendi şirket içi ortamınızı özel bir görüntüyü karşıya yükleme özel bir görüntü çözümleri temel seçebilirsiniz.

## <a name="support-for-jd-edwards"></a>Azure'da JD Edwards desteği
Oracle desteği Not göre [belge kimliği 2178595.1](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=573435677515785&id=2178595.1&_afrWindowMode=0&_adf.ctrl-state=o852dw7d_4) , azure'da JD Edwards EnterpriseOne sürümleri 9.2 ve üzeri desteklenir **tüm genel bulut teklifi** kendi özel karşılayan `Minimum Technical Requirements` (MTR).  İşletim sistemi ve yazılım uygulama uyumluluğu için kendi MTR belirtimlere özel görüntüleri oluşturmak için ihtiyacınız. 

## <a name="oracle-database-virtual-machine-images"></a>Oracle veritabanı sanal makine görüntüleri
Oracle, Oracle Linux tabanlı sanal makine görüntülerinde, Azure'da çalıştırılan Oracle VT 12,1 Standard ve Enterprise sürümleri destekler.  Azure'da Oracle DB, üretim iş yükleri için en iyi performans için Premium Depolama tarafından yedeklenen yönetilen diskler düzgün bir şekilde sanal makine görüntüsünü boyut ve emin olun. VM görüntüsü, Oracle DB çalışmaya Oracle kullanarak Azure'da hızlıca konusunda yönergeler yayımlanan [Oracle DB Hızlı Başlangıç Kılavuzu deneyin](oracle-database-quick-create.md).

### <a name="attached-disk-configuration-options"></a>Bağlı disk yapılandırma seçenekleri

Kullanıma açılan diskler Azure Blob Depolama hizmetinin avantajlarından yararlanın. Her standart disk, yaklaşık 500 giriş/çıkış işlemi (IOPS) saniyede teorik üst sınırı yeteneğine sahiptir. Premium disk teklifimize yüksek performanslı veritabanı iş yükleri için tercih edilir ve disk başına en fazla 5000 IOPS değerlerine ulaşabilir. Performans gereksinimlerinizi karşılayan tek bir diske kullanabilirsiniz - birden çok kullanıma açılan diskler kullanıyorsanız maliyetli IOPS performansı iyileştirebilir ancak veritabanı verileri bunların arasında yaymaktır ve Oracle otomatik Depolama Yönetimi (ASM) kullanın. Bkz: [Oracle otomatik depolama genel bakış](http://www.oracle.com/technetwork/database/index-100339.html) daha fazla Oracle ASM belirli bilgi. Yükleme ve bir Linux Azure sanal makinesinde - Oracle ASM yapılandırma örneği için deneyebileceğiniz [yükleme ve Oracle otomatik Depolama Yönetimi yapılandırma](configure-oracle-asm.md) öğretici.

## <a name="oracle-real-application-cluster-oracle-rac"></a>Oracle gerçek uygulama kümesi (Oracle RAC)
Oracle RAC, bir şirket içi çok düğümlü küme yapılandırmasında tek bir düğümün hatalarını azaltmak için tasarlanmıştır. Hiper ölçekli genel bulut ortamları için yerel olmayan iki şirket içi teknolojisi kullanır: ağ çok noktaya yayın ve paylaşılan disk. Veritabanı çözümünüze azure'da Oracle RAC gerektiriyorsa, bu teknolojiler etkinleştirmek için 3 taraf yazılım gerekir.  A **Microsoft Azure sertifikası** adlı teklifi [Oracle RAC FlashGrid düğümünü](https://azuremarketplace.microsoft.com/marketplace/apps/flashgrid-inc.flashgrid-racnode?tab=Overview) FlashGrid Inc. tarafından yayımlanan Azure Marketi'nde kullanımınıza sunuluyor Bu çözümün ve Azure'da nasıl çalıştığı hakkında daha fazla bilgi için lütfen bkz [FlashGrid çözüm sayfasında](https://www.flashgrid.io/oracle-rac-in-azure/).

## <a name="high-availability-and-disaster-recovery-considerations"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma değerlendirmeleri
Oracle veritabanlarının Azure'da kullanırken herhangi kapalı kalma süresini önlemek için bir yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümü uygulamak için sorumlu olursunuz. 

(Bağlı olan üzerinde Oracle RAC) olmadan yüksek kullanılabilirlik ve olağanüstü durum kurtarma için Oracle Database Enterprise Edition sürümünü kullanarak Azure üzerinde gerçekleştirilebilir [veri koruma, etkin Data Guard](http://www.oracle.com/technetwork/articles/oem/dataguardoverview-083155.html), veya [Oracle Golden kapısı](http://www.oracle.com/technetwork/middleware/goldengate), iki ayrı sanal makinelere iki veritabanı ile. Her iki sanal makine aynı olmalıdır [sanal ağ](https://azure.microsoft.com/documentation/services/virtual-network/) bunlar erişebildiğinden birbirine özel kalıcı IP adresi emin olmak için.  Ayrıca, sanal makineler aynı kullanılabilirlik kümesinde bunları ayrı hata etki alanlarına yerleştirin ve yükseltme etki alanlarında izin vermeniz yerleştirme öneririz.  Coğrafi yedeklilik - sahip olmasını isterseniz, bu iki veritabanı iki farklı bölgeler arasında çoğaltmak ve iki örnek, bir VPN ağ geçidi ile bağlanma olabilir.

Bir öğretici sahibiz "[uygulama Oracle dataguard'ı azure'da](configure-oracle-dataguard.md)", yol gösterir temel kurulum yordamı deneme sürümü için bu Azure üzerinde.  

Oracle Data Guard ile bir sanal makine, başka bir sanal makine, ikincil (Bekleyen) veritabanında birincil veritabanı ile yüksek kullanılabilirlik sağlanabilir ve tek yönlü çoğaltma aralarında ayarlayın. Veritabanı kopyasını okuma erişimi sonucudur. Oracle Goldengate'i ile iki veritabanı arasında iki yönlü çoğaltmayı yapılandırabilirsiniz. Bu araçları kullanarak veritabanlarınız için bir yüksek kullanılabilirlik çözümü hakkında bilgi edinmek için bkz: [etkin Data Guard](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) ve [Goldengate'i](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) Oracle Web sitesinden belgeleri. Access veritabanı kopyasına salt okunur gereksinim duyarsanız, kullanabileceğiniz [etkin Oracle Data Guard](http://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html).

Bir öğretici sahibiz "[uygulama azure'da Oracle Goldengate'i](configure-oracle-golden-gate.md)", yol gösterir temel kurulum yordamı deneme sürümü için bu Azure üzerinde.

Azure'da desteklemesi için bir yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümü sahip olmasına rağmen veritabanınızı geri yüklemek için yerinde bir yedekleme stratejisi sahip sağlamak istiyorsunuz.  Bir öğretici sahibiz [yedekleme ve Kurtarma bir Oracle veritabanına](oracle-backup-recovery.md) , yol gösterir; böylece tutarlı yedek oluşturmak için temel yordamı.

## <a name="oracle-weblogic-server-virtual-machine-images"></a>Oracle WebLogic Server sanal makine görüntüleri
* **Kümeleme Enterprise Edition'da yalnızca desteklenir.** WebLogic Server Enterprise Edition kullanırken WebLogic kümeleme kullanmak üzere lisanslanır. WebLogic Server Standard Edition ile kümeleme kullanmayın.
* **UDP çok noktaya yayın desteklenmiyor.** Azure, UDP aktarmaya, ancak çok noktaya yayın veya yayın destekler. WebLogic Server, Azure UDP tek noktaya yayın özelliklerine dayanan kuramıyor. En iyi UDP tek noktaya yayın üzerinde bağlı olan sonuçlar, WebLogic küme boyutu statik tutulması önerilir için veya kümeye dahil en fazla 10 yönetilen sunucularla tutulmalıdır.
* **WebLogic Server (örneğin, Kurumsal JavaBeans kullanırken) T3 erişim için aynı olması için genel ve özel bağlantı noktaları bekliyor.** Çok katmanlı oluşturduğunuzu WebLogic Server kümesinde iki veya daha fazla VM, bir vNet oluşan bir hizmet Katmanı (EJB) uygulaması çalıştığı **SLWLS**. İstemci katmanı, aynı sanal ağda çalışan basit bir Java program hizmet katmanındaki EJB çağrılmaya çalışılıyor, farklı bir alt ağdaki içindir. Yük Dengeleme hizmet katmanı için gerekli olduğundan, genel bir yük dengeli uç nokta WebLogic Server kümedeki sanal makinelerin oluşturulması gerekir. Belirttiğiniz özel bağlantı noktası (örneğin, 7006:7008) ortak bağlantı noktasından farklı ise, aşağıdaki gibi bir hata oluşur:

       [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

       Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

   Yük Dengeleyici bağlantı noktası ve yönetilen WebLogic server bağlantı noktası ile aynı WebLogic Server hiçbir uzaktan T3 erişim için bekliyor olmasıdır. Yukarıdaki durumda, istemci bağlantı noktası (yük dengeleyici bağlantı noktası) 7006 erişiyor ve 7008 (özel bağlantı noktası) üzerinde yönetilen sunucu dinliyor. Bu kısıtlama yalnızca T3 erişim için HTTP değil geçerlidir.

   Bu sorunu önlemek için aşağıdaki geçici çözümlerden birini kullanın:

  * T3 erişim için atanmış bir yük dengeli uç noktaları için aynı özel ve genel bağlantı noktası numaralarını kullanın.
  * Aşağıdaki JVM parametresini WebLogic Server başlatırken şunlardır:

         -Dweblogic.rjvm.enableprotocolswitch=true

İlgili bilgi için bkz. KB makalesi **860340.1** adresindeki <http://support.oracle.com>.

* **Dinamik Kümeleme ve sınırlamalar Yük Dengeleme.** Dinamik bir kümeye WebLogic Server kullanın ve bunu bir tek, genel yük dengeli uç noktası aracılığıyla azure'da kullanıma istediğinizi varsayalım. (Bir aralıktan dinamik olarak atanır) yönetilen sunucuların her birinde bir sabit bağlantı noktası numarası kullanmak ve yönetilen sunucuların fazla yönetici izleme makine sayısından başlatmayın sürece bu yapılabilir (diğer bir deyişle, en fazla bir sunucu başına sanal m yönetilen achine &). Başlatılmakta olan sanal makinelerin sayısından daha fazla WebLogic sunucusu yapılandırmanızı neden oluyorsa (diğer bir deyişle, WebLogic Server birden fazla aynı sanal makineye paylaşacağı), birden fazla WebLogic sunucuları örneklerini için mümkün değildir verilen bağlantı noktası numarası için – bağlamak için bu sanal makinedeki diğer başarısız.

   Benzersiz bağlantı noktası numaraları, yönetilen sunuculara otomatik olarak atamak için yönetim sunucusu yapılandırırsanız, bunun için gerekli olduğu gibi Azure birden çok özel bağlantı noktası, tek bir genel bağlantı noktası eşleme desteklemediği için ardından Yük Dengeleme mümkün değildir yapılandırma.
* **Weblogic Server bir sanal makinede birden çok örneğini.** Sanal makine yeterince büyük olduğunda, dağıtımınızın gereksinimlerine bağlı olarak, WebLogic Server'ın birden çok örneği aynı sanal makinede çalışan seçeneği düşünebilirsiniz. Örneğin, bir orta büyüklükte iki çekirdek içeren tıkladığımda, WebLogic Server'ın iki örneği çalıştırmak seçebilirsiniz. Ancak yine de, birden çok örneğini WebLogic Server çalıştıran yalnızca bir sanal makineyi kullandıysanız, durum olacaktır, mimarisine tek hata noktalarından giriş kaçınmanızı öneririz olduğunu unutmayın. En az iki sanal makine kullanarak daha iyi bir yaklaşım olabilir ve bu sanal makinelerin her biri, ardından WebLogic Server'ın birden çok örneğini çalıştırabilirsiniz. WebLogic Server'ın bu örneklerin birinde hala aynı kümenin parçası olabilir. Not, ancak, şu anda Azure yük dengeleyici, Yük Dengeleme sunucuları arasında benzersiz olarak dağıtılmasını gerektirdiğinden aynı sanal makine içinde tür WebLogic Server dağıtımlar tarafından kullanıma sunulan Yük Dengeleme uç noktalarına Azure kullanmak mümkün değildir sanal makineler.

## <a name="oracle-jdk-virtual-machine-images"></a>Oracle JDK sanal makine görüntüleri
* **JDK 6 ve 7 en son güncelleştirmeleri.** Java (şu anda Java 8) en son genel, desteklenen sürümünü kullanmanızı öneririz, ancak Azure de JDK 6 ve 7 görüntüleri kullanılabilir hale getirir. Bu, henüz JDK 8'e yükseltme için hazır olmayan eski uygulamalar için tasarlanmıştır. Önceki JDK görüntülerine artık Oracle, Microsoft ortaklığı verilen genel kullanım için kullanılabilir güncelleştirmeler sırasında JDK 6 ve 7 görüntüleri Azure tarafından sağlanan normalde Oracle tarafından sunulan daha yeni bir genel olmayan güncelleştirmeyi içeren yöneliktir Oracle yalnızca select Grup müşterilerin desteklenen. Yeni sürümler JDK görüntülerin güncelleştirilmiş sürümleri JDK 6 ve 7 ile zaman içinde kullanılabilir hale getirilir.

   Bu JDK 6 ve 7 görüntüleri ve sanal makineleri ve bunları, türetilmiş görüntüleri bulunan JDK, yalnızca Azure içinde kullanılabilir.
* **64 bit JDK.** Oracle WebLogic Server sanal makine görüntüleri ve Azure tarafından sağlanan Oracle JDK sanal makine görüntüleri hem Windows Server hem de JDK 64-bit sürümleri içerir.

## <a name="next-steps"></a>Sonraki adımlar
Artık Microsoft Azure üzerinde geçerli Oracle çözümlerine genel bir bakış sahipsiniz. Azure'da ilk Oracle veritabanı, sonraki adım dağıtmaktır.
- Deneyin [Azure'da bir Oracle veritabanına oluşturma](oracle-database-quick-create.md) kullanmaya başlamak için öğretici.