---
title: Azure NetApp Files (SMB) ile Windows üzerinde SAP NW için Azure VM 'Leri HA | Microsoft Docs
description: SAP uygulamaları için Azure NetApp Files (SMB) ile Windows üzerinde Azure VM 'lerinde SAP NetWeaver için yüksek kullanılabilirlik
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-sap
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/18/2021
ms.author: radeltch
ms.openlocfilehash: a4c4631a0a1263e5a5398c44a8570f92571102e8
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102045845"
---
# <a name="high-availability-for-sap-netweaver-on-azure-vms-on-windows-with-azure-netapp-filessmb-for-sap-applications"></a>SAP uygulamaları için Azure NetApp Files (SMB) ile Windows üzerinde Azure VM 'lerinde SAP NetWeaver için yüksek kullanılabilirlik

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[anf-azure-doc]:https://docs.microsoft.com/azure/azure-netapp-files/
[anf-avail-matrix]:https://azure.microsoft.com/global-infrastructure/services/?products=storage&regions=all
[anf-register]:https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-register
[anf-sap-applications-azure]:https://www.netapp.com/us/media/tr-4746.pdf

[2205917]:https://launchpad.support.sap.com/#/notes/2205917
[1944799]:https://launchpad.support.sap.com/#/notes/1944799
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[1410736]:https://launchpad.support.sap.com/#/notes/1410736

[sap-swcenter]:https://support.sap.com/en/my-support/software-downloads.html

[suse-ha-guide]:https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/
[suse-drbd-guide]:https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha_techguides/book_sleha_techguides.html
[suse-ha-12sp3-relnotes]:https://www.suse.com/releasenotes/x86_64/SLE-HA/12-SP3/

[template-multisid-xscs]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-xscs-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged-md%2Fazuredeploy.json
[template-file-server]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-file-server-md%2Fazuredeploy.json

[sap-hana-ha]:sap-hana-high-availability.md
[nfs-ha]:high-availability-guide-suse-nfs.md

Bu makalede, [Azure NetApp Files](../../../azure-netapp-files/azure-netapp-files-introduction.md) [SMB](/windows/win32/fileio/microsoft-smb-protocol-and-cifs-protocol-overview) kullanarak, sanal makinelerin nasıl dağıtılacağı, yapılandırıldığı, küme çerçevesinin nasıl yükleneceği ve Windows VM 'lerine yüksek oranda kullanılabilir bir SAP NetWeaver 7,50 sisteminin nasıl yükleneceği açıklanır.  

Veritabanı katmanı Bu makalede ayrıntılı olarak ele alınmıyor. Azure [sanal ağının](../../../virtual-network/virtual-networks-overview.md) zaten oluşturulduğunu varsayalım.  

Önce aşağıdaki SAP notlarını ve kağıtları okuyun:

* [Azure NetApp Files belgeleri][anf-azure-doc] 
* SAP Note [1928533][1928533], şunları içerir:  
  * SAP yazılımının dağıtımı için desteklenen Azure VM boyutlarının listesi
  * Azure VM boyutları için önemli kapasite bilgileri
  * Desteklenen SAP yazılımı ve işletim sistemi (OS) ve veritabanı birleşimleri
  * Microsoft Azure Windows için gereken SAP Kernel sürümü
* SAP Note [2015553][2015553] , Azure 'da SAP tarafından desteklenen SAP yazılım dağıtımları için önkoşulları listeler.
* SAP Note [2178632][2178632] , Azure 'da SAP için raporlanan tüm izleme ölçümleriyle ilgili ayrıntılı bilgiler içerir.
* SAP Note [1999351][1999351] , SAP Için Azure Gelişmiş izleme uzantısı için ek sorun giderme bilgilerine sahiptir.
* SAP Note [2287140](https://launchpad.support.sap.com/#/notes/2287140) , SMB 3. x protokolünün SAP tarafından desteklenen CA özelliği için önkoşulları listeler.
* SAP Note [2802770](https://launchpad.support.sap.com/#/notes/2802770) , Windows 2012 ve 2016 üzerinde yavaş çalışan SAP Transaction AL11 için sorun giderme bilgileri içerir.
* SAP Note [1911507](https://launchpad.support.sap.com/#/notes/1911507) , Windows Server 'da SMB 3,0 protokolüyle bir dosya paylaşımının saydam yük devretme özelliği hakkında bilgi içerir.
* SAP Note [662452](https://launchpad.support.sap.com/#/notes/662452) , veri erişimi sırasında zayıf dosya sistemi performansına/hatalarına yönelik öneriye sahiptir (8,3 ad oluşturmayı devre dışı bırakma).
* [SAP NetWeaver yüksek kullanılabilirliği 'ni bir Windows Yük devretme kümesine ve Azure 'daki SAP Ass/SCS örnekleri için dosya paylaşımında yükler](./sap-high-availability-installation-wsfc-file-share.md) 
* [SAP NetWeaver için Azure sanal makineler yüksek kullanılabilirliğe sahip mimari ve senaryolar](./sap-high-availability-architecture-scenarios.md)
* [ASCS küme yapılandırmasında araştırma bağlantı noktası ekle](sap-high-availability-installation-wsfc-file-share.md)
* [Bir yük devretme kümesine (A) SCS örneği yükleme](https://www.sap.com/documents/2017/07/f453332f-c97c-0010-82c7-eda71af511fa.html)
* [Azure NetApp Files için SMB birimi oluşturma](../../../azure-netapp-files/create-active-directory-connections.md#requirements-for-active-directory-connections)
* [Microsoft Azure Azure NetApp Files kullanarak NetApp SAP uygulamaları][anf-sap-applications-azure]

> [!IMPORTANT]
> DIKKAT: [Azure NetApp Files][anf-azure-doc] SMB BIRIMINDE barındırılan SMB paylaşımında swpm ile SAP sistemi yüklemesinin, "warningizin tanımlanmadı" gibi yetersiz izinler için yükleme hatasıyla başarısız olabileceğini unutmayın. Hatayı önlemek için, SWPM 'nin çalıştırıldığı kullanıcının, SAP sisteminin yüklenmesi sırasında yükseltilmiş ayrıcalığa "etki alanı yöneticisi" olması gerekir.  

## <a name="overview"></a>Genel Bakış

SAP, bir Windows Yük devretme kümesindeki SAP ASCS/SCS örneğini Kümelendirmek için yeni bir yaklaşım ve küme paylaşılan disklerine bir alternatifi geliştirmiştir. Küme paylaşılan disklerini kullanmak yerine, bir tane, SAP Küresel Ana bilgisayar dosyalarını dağıtmak için bir SMB dosya paylaşımının kullanılmasına olanak sağlar. Azure NetApp Files, Active Directory kullanılarak NTFS ACL 'SI ile SMBv3 (NFS ile birlikte) destekler. Azure NetApp Files, otomatik olarak yüksek oranda kullanılabilir (PaaS hizmeti gibi). Bu özellikler, SAP Global için SMB dosya paylaşımının barındırılmasına yönelik Azure NetApp Files harika bir seçenek sunar.  
[Azure Active Directory (ad) etki alanı Hizmetleri](../../../active-directory-domain-services/overview.md) ve [Active Directory Domain Services (AD DS)](/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview) desteklenir. Mevcut Active Directory etki alanı denetleyicilerini Azure NetApp Files ile birlikte kullanabilirsiniz. Etki alanı denetleyicileri, Azure 'da sanal makineler veya ExpressRoute ya da S2S VPN aracılığıyla şirket içi olabilir. Bu makalede, bir Azure VM 'de etki alanı denetleyicisi kullanacağız.  
SAP NetWeaver Merkezi Hizmetleri için yüksek kullanılabilirlik (HA), paylaşılan depolama gerektirir. Windows 'da bunu başarmak için, en çok SOFS kümesi oluşturmak veya SIOS gibi küme paylaşılan disk s/w kullanması gerekiyordu. Artık Azure NetApp Files dağıtılan paylaşılan depolamayı kullanarak SAP NetWeaver HA elde etmek mümkündür. Paylaşılan depolama için Azure NetApp Files kullanmak SOFS veya SIOS gereksinimini ortadan kaldırır.  

> [!NOTE]
> SAP ASCS/SCS örneklerinin bir dosya paylaşımının kullanılarak kümelenmesi, SAP çekirdek 7,49 (ve üzeri) ile SAP NetWeaver 7,40 (ve üzeri) için desteklenir.  

![SMB paylaşımıyla SAP yoks/SCS HA mimarisi](./media/virtual-machines-shared-sap-high-availability-guide/high-availability-windows-azure-netapp-files-smb.png)

SMB dosya paylaşımının önkoşulları şunlardır:
* SMB 3,0 (veya üzeri) protokolü.
* Active Directory Kullanıcı grupları ve bilgisayar $ bilgisayar nesnesi için Active Directory erişim denetim listeleri (ACL 'Ler) ayarlama yeteneği.
* Dosya paylaşımının HA etkin olması gerekir.

Bu başvuru mimarisinde SAP Merkezi Hizmetleri için olan paylaşımın Azure NetApp Files tarafından sunulmaktadır:

![SMB ayrıntısı ile SAP yoks/SCS HA mimarisi](./media/virtual-machines-shared-sap-high-availability-guide/high-availability-windows-azure-netapp-files-smb-detail.png)

## <a name="create-and-mount-smb-volume-for-azure-netapp-files"></a>Azure NetApp Files için SMB birimi oluşturma ve bağlama

Azure NetApp Files kullanmanın hazırlanması için aşağıdaki adımları gerçekleştirin.  

1. [Azure NetApp Files kaydolmak](../../../azure-netapp-files/azure-netapp-files-register.md) için adımları izleyin  
2. [NetApp hesabı oluşturma](../../../azure-netapp-files/azure-netapp-files-create-netapp-account.md) bölümünde açıklanan adımları Izleyerek Azure NetApp hesabı oluşturun  
3. [Kapasite havuzunu ayarlama bölümündeki yönergeleri](../../../azure-netapp-files/azure-netapp-files-set-up-capacity-pool.md) izleyerek kapasite havuzunu ayarlama
4. Azure NetApp Files kaynaklar, temsilci alt ağında bulunmalıdır. Temsilci alt ağ oluşturmak için [Azure NetApp Files için bir alt ağ Için temsilci seçme](../../../azure-netapp-files/azure-netapp-files-delegate-subnet.md) bölümündeki yönergeleri izleyin.  

   > [!IMPORTANT]
   > SMB birimi oluşturmadan önce Active Directory bağlantı oluşturmanız gerekir. [Active Directory bağlantıları için gereksinimleri](../../../azure-netapp-files/create-active-directory-connections.md#requirements-for-active-directory-connections)gözden geçirin.  

5. [Active Directory bağlantısı oluşturma](../../../azure-netapp-files/create-active-directory-connections.md#create-an-active-directory-connection) bölümünde açıklandığı gibi Active Directory bağlantı oluştur  
6. SMB [birimi ekleme](../../../azure-netapp-files/azure-netapp-files-create-volumes-smb.md#add-an-smb-volume) bölümündeki yönergeleri izleyerek SMB Azure NetApp Files SMB birimi oluşturun  
7. SMB birimini Windows sanal makinenize bağlayın.

> [!TIP]
> Azure NetApp Files birimini bağlama yönergelerini bulabilirsiniz. [Azure portalında](https://portal.azure.com/#home) Azure NetApp Files nesnesine gittiğinizde, **birimler** dikey penceresine tıklayın ve sonra **yönergeleri bağlayın**.  

## <a name="prepare-the-infrastructure-for-sap-ha-by-using-a-windows-failover-cluster"></a>Windows Yük devretme kümesi kullanarak SAP HA altyapısını hazırlama 

1. [Azure iç yük dengeleyici IÇIN ASCS/SCS Yük Dengeleme kurallarını ayarlayın](./sap-high-availability-infrastructure-wsfc-shared-disk.md#fe0bd8b5-2b43-45e3-8295-80bee5415716).
2. [Windows sanal makinelerini etki alanına ekleyin](./sap-high-availability-infrastructure-wsfc-shared-disk.md#e69e9a34-4601-47a3-a41c-d2e11c626c0c).
3. [SAP ASCS/SCS örneğinin küme düğümlerine kayıt defteri girişleri ekleme](./sap-high-availability-infrastructure-wsfc-shared-disk.md#661035b2-4d0f-4d31-86f8-dc0a50d78158)
4. [SAP ASCS/SCS örneği için Windows Server yük devretme kümesi ayarlama](./sap-high-availability-infrastructure-wsfc-shared-disk.md#0d67f090-7928-43e0-8772-5ccbf8f59aab)
5. Windows Server 2016 kullanıyorsanız, [Azure bulut tanığını](/windows-server/failover-clustering/deploy-cloud-witness)yapılandırmanızı öneririz.


## <a name="install-sap-ascs-instance-on-both-nodes"></a>SAP ASCS örneğini her iki düğüme de yükler

SAP 'den aşağıdaki yazılımlara ihtiyacınız vardır:
   * SAP yazılım sağlama Yöneticisi (SWPM) Yükleme Aracı sürüm SPS25 veya üzeri.
   * SAP Kernel 7,49 veya üzeri
   * Kümelenmiş SAP ascs/SCS örneği için [sanal ana bilgisayar adı oluşturma](./sap-high-availability-installation-wsfc-shared-disk.md#a97ad604-9094-44fe-a364-f89cb39bf097)bölümünde açıklandığı gibi, kümelenmiş SAP ascs/SCS örneği için bir sanal ana bilgisayar adı (küme ağ adı) oluşturun.

> [!NOTE]
> SAP ASCS/SCS örneklerinin bir dosya paylaşımının kullanılarak kümelenmesi, SAP çekirdek 7,49 (ve üzeri) ile SAP NetWeaver 7,40 (ve üzeri) için desteklenir.  

### <a name="install-an-ascsscs-instance-on-the-first-ascsscs-cluster-node"></a>İlk yoks/SCS kümesi düğümüne bir ASCS/SCS örneği yükler

1. İlk küme düğümüne bir SAP ASCS/SCS örneği yükler. SAP swpm yükleme aracını başlatın ve şuraya gidin: **ürün**  >  **DBMS** > yükleme > uygulama sunucusu ABAP (veya Java) > High-Availability sistem > ascs/SCS örneği > ilk küme düğümü.  

2. SWPM 'de küme paylaşma yapılandırması olarak **dosya paylaşma kümesi** ' ni seçin.  
3. **SAP sistem kümesi parametrelerine** adım sorulduğunda, **dosya paylaşma ana bilgisayar adı** olarak zaten oluşturduğunuz Azure NetApp Files SMB paylaşımının ana bilgisayar adını girin.  Bu örnekte, SMB paylaşımının ana bilgisayar adı **anfsmb-9562**' dir. 

   > [!IMPORTANT]
   > Önkoşul denetleyicisi, SWPM ile sonuçlanıyorsa sürekli kullanılabilirlik özelliği koşulunun karşılanmadığını gösteriyorsa, [Windows 'ta artık mevcut olmayan bir paylaşılan klasöre erişmeye çalıştığınızda geciken hata iletisindeki](https://support.microsoft.com/help/2820470/delayed-error-message-when-you-try-to-access-a-shared-folder-that-no-l)yönergeleri izleyerek bu durum çözülebilir.  

   > [!TIP]
   > Önkoşul denetleyicisi, SWPM ile sonuçlanıyorsa, takas boyutu koşulunun karşılanmadığını gösteriyorsa, değişim boyutunu Bilgisayarım>sistem özellikleri>performans ayarları> gelişmiş> sanal bellek> değiştirme ' ye giderek ayarlayabilirsiniz.  

4. PowerShell kullanarak bir SAP kümesi kaynağını, `SAP-SID-IP` araştırma bağlantı noktasını yapılandırın. Bu yapılandırmayı, [araştırma bağlantı noktasını yapılandırma](./sap-high-availability-installation-wsfc-shared-disk.md#10822f4f-32e7-4871-b63a-9b86c76ce761)bölümünde AÇıKLANDıĞı gıbı SAP Ass/SCS küme düğümlerinden birinde yürütün.

### <a name="install-an-ascsscs-instance-on-the-second-ascsscs-cluster-node"></a>İkinci yoks/SCS küme düğümüne bir ASCS/SCS örneği yükler

1. İkinci küme düğümüne bir SAP ASCS/SCS örneği yükler. SAP swpm yükleme aracını başlatın ve ardından > **ürün**  >  **DBMS** 'ye gidin > uygulama sunucusu ABAP (veya Java) > High-Availability sistem > ascs/SCS örneği > ek küme düğümü.  

### <a name="install-a-dbms-instance-and-sap-application-servers"></a>Bir DBMS örneği ve SAP uygulama sunucuları yükler

Aşağıdakileri yükleyerek SAP yüklemenizi doldurun:

   * Bir DBMS örneği  
   * Birincil SAP uygulama sunucusu  
   * Ek bir SAP uygulama sunucusu  

## <a name="test-the-sap-ascsscs-instance-failover"></a>SAP ASCS/SCS örneği yük devretmesini test etme 

### <a name="fail-over-from-cluster-node-a-to-cluster-node-b-and-back"></a>A kümesi düğümünden B ve geri küme düğümüne yük devretme
Bu test senaryosunda, A düğümü olarak küme düğümüne sapascs1 ve düğüm sapascs2 olarak düğüm B olarak kümelendirilecektir.

1. Küme kaynaklarının A düğümünde çalıştığını doğrulayın. ![ Şekil 1: yük devretme testi öncesinde düğüm üzerinde çalışan Windows Server yük devretme kümesi kaynakları](./media/virtual-machines-shared-sap-high-availability-guide/high-availability-windows-azure-netapp-files-smb-figure-1.png)  

2. Küme düğümü A 'yi yeniden başlatın. SAP kümesi kaynakları B küme düğümüne taşınır. ![ Şekil 2: yük devretme testi sonrasında düğüm B 'de çalışan Windows Server yük devretme kümesi kaynakları](./media/virtual-machines-shared-sap-high-availability-guide/high-availability-windows-azure-netapp-files-smb-figure-2.png)  


## <a name="lock-entry-test"></a>Giriş testini kilitle

1. SAP sıraya alma çoğaltma sunucusunun (ERS) etkin olduğunu doğrulayın  
2. SAP sisteminde oturum açın, işlem SU01 yürütün ve değişiklik modunda bir kullanıcı KIMLIĞI açın. Bu, SAP Lock girişi oluşturacak.  
3. SAP sisteminde oturum açtığınızda, işlem ST12 ' a giderek kilit girişini görüntüleyin.  
4. Küme düğümü A 'dan düğüm B 'ye kadar yoks kaynağı yük devreder.  
5. SAP Ass/SCS küme kaynakları yük devretmesi gerçekleştirilmeden önce oluşturulan kilit girişinin bekletileceği doğrulayın.  

![Şekil 3: kilit girdisi yük devretme testi sonrasında tutulur](./media/virtual-machines-shared-sap-high-availability-guide/high-availability-windows-azure-netapp-files-smb-figure-3.png)  

Daha fazla bilgi için bkz. [CIS Ile yük devretme sırasında kuyruğa alma sorunlarını giderme](https://wiki.scn.sap.com/wiki/display/SI/Troubleshooting+for+Enqueue+Failover+in+ASCS+with+ERS)
## <a name="next-steps"></a>Sonraki adımlar

* [SAP için Azure sanal makineleri planlama ve uygulama][planning-guide]
* [SAP için Azure sanal makineleri dağıtımı][deployment-guide]
* [SAP için Azure sanal makineleri DBMS dağıtımı][dbms-guide]
* SAP 'nin olağanüstü durum kurtarma için yüksek kullanılabilirlik ve plan yapmayı öğrenin 
* Azure 'da HANA (büyük örnekler) için bkz. [Azure 'da SAP HANA (büyük örnekler) yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md).
* Azure VM 'lerinde SAP HANA olağanüstü durum kurtarma için yüksek kullanılabilirlik ve plan planı oluşturma hakkında bilgi edinmek için bkz. [Azure sanal makinelerinde (VM) SAP HANA yüksek kullanılabilirliği][sap-hana-ha]
