---
title: SQL Server VM 'Leri için depolama yapılandırması | Microsoft Docs
description: Bu konu, Azure 'un sağlama sırasında (Azure Resource Manager dağıtım modeli) SQL Server VM 'Ler için depolamayı nasıl yapılandırdığını açıklamaktadır. Ayrıca, mevcut SQL Server sanal makinelerinize yönelik depolamayı nasıl yapılandırabileceğinizi açıklar.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
tags: azure-resource-manager
ms.assetid: 169fc765-3269-48fa-83f1-9fe3e4e40947
ms.service: virtual-machines-sql
ms.subservice: management
ms.topic: how-to
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 12/26/2019
ms.author: mathoma
ms.openlocfilehash: d713faf7062f82110be5fa8378faca368b9bb7a2
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "97356740"
---
# <a name="storage-configuration-for-sql-server-vms"></a>SQL Server VM’leri için depolama yapılandırması
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Azure 'da bir SQL Server sanal makine (VM) görüntüsü yapılandırdığınızda, Azure portal depolama yapılandırmanızın otomatikleştirilmesine yardımcı olur. Bu, depolama alanının sanal makineye iliştirilmesi, bu depolamanın SQL Server için erişilebilir hale getirilmesi ve belirli performans gereksinimlerinizi iyileştirmek üzere yapılandırılmasını içerir.

Bu konu, Azure 'un sağlama ve mevcut VM 'Ler için SQL Server sanal makinelerinize yönelik depolamayı nasıl yapılandırdığını açıklamaktadır. Bu yapılandırma, SQL Server çalıştıran Azure sanal makineleri için [en iyi performans uygulamalarına](performance-guidelines-best-practices.md) dayalıdır.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Önkoşullar

Otomatik depolama yapılandırma ayarlarını kullanmak için, sanal makineniz aşağıdaki özellikleri gerektirir:

* [SQL Server Galeri görüntüsü](sql-server-on-azure-vm-iaas-what-is-overview.md#payasyougo)ile sağlandı.
* [Kaynak Yöneticisi dağıtım modelini](../../../azure-resource-manager/management/deployment-models.md)kullanır.
* [Premium SSD 'ler](../../../virtual-machines/disks-types.md)kullanır.

## <a name="new-vms"></a>Yeni VM 'Ler

Aşağıdaki bölümlerde, yeni SQL Server sanal makineler için depolamanın nasıl yapılandırılacağı açıklanır.

### <a name="azure-portal"></a>Azure portalı

SQL Server Galeri görüntüsü kullanarak bir Azure VM sağlarken, **SQL Server ayarları** sekmesinde **yapılandırmayı Değiştir** ' i seçerek performans için iyileştirilmiş depolama yapılandırması sayfasını açın. Değerleri varsayılan olarak bırakabilir ya da iş yükünüze göre gereksinimlerinize en uygun disk yapılandırma türünü değiştirebilirsiniz. 

![SQL Server ayarları sekmesini ve yapılandırma Değiştir seçeneğini vurgulayan ekran görüntüsü.](./media/storage-configuration/sql-vm-storage-configuration-provisioning.png)

**Depolama iyileştirmesi** altında SQL Server dağıttığınız iş yükünün türünü seçin. **Genel** iyileştirme seçeneğiyle, varsayılan olarak en fazla 5000 IOPS içeren bir veri diskine sahip olursunuz ve bu sürücüyü verileriniz, işlem günlüğü ve tempdb depolaması için kullanacaksınız. **İşlemsel işleme** (OLTP) veya **veri depolama** alanı seçildiğinde veriler için ayrı bir disk, işlem günlüğü için ayrı bir disk oluşturulur ve tempdb için yerel SSD kullanılır. **İşlemsel işleme** ve **veri depolama** arasında bir depolama farkı yoktur, ancak [Stripe yapılandırmanızı ve izleme bayraklarını](#workload-optimization-settings)değiştirir. Premium Depolama ' yı seçtiğinizde, veri sürücüsü için önbelleğe alma özelliği *ReadOnly* olarak ayarlanır ve günlük sürücü için [SQL Server VM performans En Iyi uygulamalarına](performance-guidelines-best-practices.md)göre *yoktur* . 

![Sağlama sırasında depolama yapılandırması SQL Server VM](./media/storage-configuration/sql-vm-storage-configuration.png)

Disk yapılandırması tamamen özelleştirilebilir olduğundan, SQL Server VM iş yükünüz için gereken depolama topolojisini, disk türünü ve IOPS 'yi yapılandırabilirsiniz. Ayrıca, SQL Server VM desteklenen bölgelerden birinde (Doğu ABD 2, Güneydoğu Asya ve Kuzey Avrupa) ve [aboneliğiniz için Ultra diskler](../../../virtual-machines/disks-enable-ultra-ssd.md)etkinleştirdiyseniz, **disk türü** Için bir seçenek olarak UltraSSD (Önizleme) özelliğini kullanabilirsiniz.  

Ayrıca, diskler için önbelleğe alma özelliğini ayarlayabilirsiniz. Azure VM 'Leri, [Premium disklerle](../../../virtual-machines/disks-types.md#premium-ssd)kullanıldığında [blob önbelleği](../../../virtual-machines/premium-storage-performance.md#disk-caching) adlı çok katmanlı bir önbelleğe alma teknolojisine sahiptir. Blob önbelleği, önbelleğe alma için sanal makine RAM ve yerel SSD 'nin bir birleşimini kullanır. 

Premium SSD için disk önbelleğe alma *ReadOnly*, *ReadWrite* veya *none* olabilir. 

- *ReadOnly* önbelleğe alma, Premium depolamada depolanan SQL Server veri dosyaları için oldukça faydalıdır. *Salt* okunur önbellek, düşük okuma gecikmesi, yüksek okuma IOPS ve aktarım hızı gibi IŞLEMLERI, VM belleği ve yerel SSD içindeki önbellekten gerçekleştirilen okuma işlemleri, önbellekten gerçekleştirilir. Bu okumalar, Azure Blob depolamadan olan veri diskinden okumalarından çok daha hızlıdır. Premium Depolama, önbellekten sunulan okuma sayısını disk ıOPS ve aktarım hızı ile saymaz. Bu nedenle, uygun olan toplam ıOPS ve aktarım hızı elde edebilirsiniz. 
- Günlük dosyası sıralı olarak yazıldığı ve *salt okunur* önbelleğe alma özelliğinden yararlanmadığı için, SQL Server günlük dosyası barındıran diskler için *hiçbiri* önbellek yapılandırması kullanılmamalıdır. 
- SQL Server, *ReadWrite* önbelleği ile veri tutarlılığını desteklemediğinden SQL Server dosyaları barındırmak için *ReadWrite* önbelleği kullanılmamalıdır. *ReadOnly* blob önbelleğinin çöp kapasitesini yazar ve yazmalar *salt okunur* blob önbelleği katmanlarına gitmez biraz artar. 


   > [!TIP]
   > Depolama yapılandırmanızın seçili VM boyutu tarafından uygulanan kısıtlamalarla eşleştiğinden emin olun. VM boyutunun performans üst sınırını aşan depolama parametrelerinin seçilmesi uyarı olarak sonuçlanır: `The desired performance might not be reached due to the maximum virtual machine disk performance cap` . Disk türünü değiştirerek IOPS 'yi azaltın veya VM boyutunu artırarak performans üst sınırı değerini artırın. Bu, sağlamayı durdurmaz. 


Azure, seçimlerinize bağlı olarak VM 'yi oluşturduktan sonra aşağıdaki depolama yapılandırma görevlerini gerçekleştirir:

* Premium SSD 'leri oluşturur ve sanal makineye ekler.
* SQL Server için erişilebilir olacak veri disklerini yapılandırır.
* Veri disklerini, belirtilen boyut ve performans (ıOPS ve aktarım hızı) gereksinimlerine bağlı olarak bir depolama havuzunda yapılandırır.
* Depolama havuzunu sanal makinede yeni bir sürücüyle ilişkilendirir.
* Bu yeni sürücüyü, belirtilen iş yükü türüne (veri ambarı, Işlemsel işleme veya genel) göre iyileştirir.

Azure 'un depolama ayarlarını yapılandırma hakkında daha fazla bilgi için [depolama yapılandırması bölümüne](#storage-configuration)bakın. Azure portal SQL Server VM oluşturma hakkında tam bir anlatım için [sağlama öğreticisine](../../../azure-sql/virtual-machines/windows/create-sql-vm-portal.md)bakın.

### <a name="resource-manager-templates"></a>Resource Manager şablonları

Aşağıdaki Kaynak Yöneticisi şablonlarını kullanıyorsanız, varsayılan olarak, depolama havuzu yapılandırması olmadan iki Premium veri diski eklenir. Ancak, bu şablonları, sanal makineye bağlı olan Premium veri disklerinin sayısını değiştirecek şekilde özelleştirebilirsiniz.

* [Otomatik yedekleme ile VM oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autobackup)
* [Otomatik düzeltme eki uygulama ile VM oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autopatching)
* [AKV tümleştirmesi ile VM oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-keyvault)

### <a name="quickstart-template"></a>Hızlı Başlangıç şablonu

Depolama iyileştirmesi kullanarak bir SQL Server VM dağıtmak için aşağıdaki hızlı başlangıç şablonunu kullanabilirsiniz. 

* [Depolama iyileştirmesi ile VM oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-new-storage/)
* [UltraSSD kullanarak VM oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-new-storage-ultrassd)

## <a name="existing-vms"></a>Mevcut VM 'Ler

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Mevcut SQL Server VM 'Ler için Azure portal bazı depolama ayarlarını değiştirebilirsiniz. [SQL sanal makineleri kaynağını](manage-sql-vm-portal.md#access-the-sql-virtual-machines-resource)açın ve **genel bakış**' ı seçin. SQL Server genel bakış sayfası, sanal makinenizin geçerli depolama kullanımını gösterir. SANAL makinenizde bulunan tüm sürücüler bu grafikte görüntülenir. Her sürücü için, depolama alanı dört bölümde görüntülenir:

* SQL verileri
* SQL günlüğü
* Diğer (SQL depolama olmayan)
* Kullanılabilir

Depolama ayarlarını değiştirmek için **Ayarlar** altında **Yapılandır** ' ı seçin. 

![Yapılandırma seçeneğini ve depolama kullanımı bölümünü vurgulayan ekran görüntüsü.](./media/storage-configuration/sql-vm-storage-configuration-existing.png)

SQL Server VM oluşturma işlemi sırasında yapılandırılmış sürücüler için disk ayarlarını değiştirebilirsiniz. **Sürücüyü Genişlet** ' i seçtiğinizde sürücü değiştirme sayfası açılır ve bu da disk türünü değiştirmenize ve ek diskler eklemenize olanak tanır. 

![Mevcut SQL Server VM için depolamayı yapılandırma](./media/storage-configuration/sql-vm-storage-extend-drive.png)


## <a name="storage-configuration"></a>Depolama yapılandırması

Bu bölümde, Azure 'un Azure portal SQL Server VM sağlama veya yapılandırma sırasında otomatik olarak gerçekleştirdiği depolama yapılandırması değişikliklerine yönelik bir başvuru sağlanmaktadır.

* Azure, VM 'nizden seçilen depolama alanından bir depolama havuzu yapılandırır. Bu konunun sonraki bölümünde, depolama havuzu yapılandırması hakkında ayrıntılı bilgi verilmektedir.
* Otomatik depolama yapılandırması her zaman [Premium SSDs](../../../virtual-machines/disks-types.md) P30 veri disklerini kullanır. Sonuç olarak, seçtiğiniz terabaytlık ve sanal makinenize bağlı veri disklerinin sayısı arasında bir 1:1 eşlemesi vardır.

Fiyatlandırma bilgileri için **disk depolama** sekmesindeki [Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage) sayfasına bakın.

### <a name="creation-of-the-storage-pool"></a>Depolama havuzu oluşturma

Azure, SQL Server VM 'lerde depolama havuzu oluşturmak için aşağıdaki ayarları kullanır.

| Ayar | Değer |
| --- | --- |
| Şerit boyutu |256 KB (veri ambarı); 64 KB (Işlem) |
| Disk boyutları |1 TB her |
| Önbellek |Okuma |
| Ayırma boyutu |64 KB NTFS ayırma birimi boyutu |
| Kurtarma | Basit kurtarma (dayanıklılık yok) |
| Sütun sayısı |8<sup>1</sup> adede kadar veri diski sayısı |


<sup>1</sup> depolama havuzu oluşturulduktan sonra, depolama havuzundaki sütun sayısını değiştiremezsiniz.


## <a name="workload-optimization-settings"></a>İş yükü iyileştirme ayarları

Aşağıdaki tabloda, kullanılabilir üç iş yükü türü seçeneği ve bunların karşılık gelen iyileştirmeleri açıklanmaktadır:

| İş yükü türü | Description | İyileştirmeler |
| --- | --- | --- |
| **Genel** |Çoğu iş yüklerini destekleyen varsayılan ayar |Yok |
| **İşlemsel işleme** |Geleneksel veritabanı OLTP iş yükleri için depolamayı iyileştirir |İzleme bayrağı 1117<br/>İzleme bayrağı 1118 |
| **Veri depolama** |Analitik ve raporlama iş yükleri için depolamayı iyileştirir |İzleme bayrağı 610<br/>İzleme bayrağı 1117 |

> [!NOTE]
> Yalnızca depolama yapılandırması adımında bir SQL Server sanal makine sağladığınızda iş yükü türünü belirtebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure VM 'lerinde SQL Server çalıştırmaya ilişkin diğer konular için bkz. [Azure sanal makinelerinde SQL Server](sql-server-on-azure-vm-iaas-what-is-overview.md).