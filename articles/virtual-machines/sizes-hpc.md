---
title: Azure VM boyutları-HPC | Microsoft Docs
description: Azure 'da yüksek performanslı bilgi işlem sanal makineleri için kullanılabilen farklı boyutları listeler. Bu serideki boyutlarda vCPU sayısı, veri diskleri ve NIC 'lerin yanı sıra depolama aktarım hızı ve ağ bant genişliği hakkındaki bilgileri listeler.
author: vermagit
ms.service: virtual-machines
ms.subservice: vm-sizes-hpc
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 12/09/2020
ms.author: amverma
ms.reviewer: jushiman
ms.openlocfilehash: bfc49cf7c9f5489248aeba7465a88e97ad16abd8
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102554405"
---
# <a name="high-performance-computing-vm-sizes"></a>Yüksek performanslı bilgi işlem VM boyutları

Azure H serisi sanal makineler (VM 'Ler), çeşitli gerçek dünyada HPC iş yükleri için liderlik sınıfı performans, ölçeklenebilirlik ve maliyet verimliliği sağlamak üzere tasarlanmıştır.

[HBv2 serisi](hbv2-series.md) Sanal makineler, akıcı Dynamics, sınırlı öğe analizi ve rezervoır benzetimi gibi bellek bant genişliğine göre çalışan uygulamalar için iyileştirilmiştir. HBv2 VM 'Ler özelliği 120 AMD EPIC 7742 işlemci çekirdekleri, CPU çekirdeği başına 4 GB RAM ve eşzamanlı çoklu iş parçacığı yok. Her HBv2 VM, en fazla 340 GB/sn bellek bant genişliği ve en fazla 4 işlem FP64 işlem sağlar.

HBv2 VM 'Ler özelliği 200 GB/sn Mellanox HDR InfiniBand, her ikisi de HB ve HC Serisi VM 'Ler özelliği 100 GB/sn Mellanox EDR InfiniBand. Bu sanal makine türlerinin her biri, iyileştirilmiş ve tutarlı RDMA performansı için engelleyici olmayan bir FAT ağacına bağlanır. HBv2 VM 'Leri, uyarlamalı yönlendirmeyi ve dinamik bağlı taşımayı (Standart RC ve UD aktarımlarında ek olarak) destekler. Bu özellikler uygulama performansı, ölçeklenebilirliği ve tutarlılığı geliştirir ve bunların kullanımı kesinlikle önerilir.

[HB Serisi](hb-series.md) Sanal makineler, akıcı Dynamics, açık sınırlı öğe analizi ve hava durumu modelleme gibi bellek bant genişliğine göre çalışan uygulamalar için iyileştirilmiştir. HB VM 'Ler özelliği 60 AMD EPIC 7551 işlemci çekirdekleri, CPU çekirdeği başına 4 GB RAM ve hiper iş parçacığı yok. AMD EPYıC platformu 260 GB/sn 'den fazla bellek bant genişliği sağlar.

[HC Serisi](hc-series.md) VM 'Ler, örtük sınırlı öğe analizi, molesel Dynamics ve hesaplama Chemistry gibi yoğun hesaplama tarafından yönetilen uygulamalar için iyileştirilmiştir. HC VM 'Ler özelliği 44 Intel Xeon Platinum 8168 işlemci çekirdekleri, CPU çekirdeği başına 8 GB RAM ve hiper iş parçacığı yok. Intel Xeon Platinum platformu Intel Math çekirdek kitaplığı gibi yazılım araçlarının zengin ekosistemini destekler.

[H serisi](h-series.md) VM 'Ler, yüksek CPU sıklıklarca veya çekirdek gereksinimlerine göre büyük bellek kullanan uygulamalar için iyileştirilmiştir. H serisi VM 'Ler özelliği 8 veya 16 Intel Xeon E5 2667 v3 işlemci çekirdeği, 7 veya CPU çekirdeği başına 14 GB RAM ve hiper iş parçacığı yok. Uyumlu RDMA performansı için engelleyici olmayan bir FAT ağacı yapılandırmasında 56 GB/sn Mellanox FDR InfiniBand içindeki H Serisi özellikleri. H serisi VM 'Ler Intel MPı 5. x ve MS-MPı 'yi destekler.

> [!NOTE]
> Tüm HBv2, HB ve HC Serisi VM 'Lerin fiziksel sunuculara özel erişimi vardır. Fiziksel sunucu başına yalnızca 1 VM vardır ve bu VM boyutları için diğer VM 'Ler ile paylaşılan çok kiracılı değildir.

> [!NOTE]
> [A8 – A11 VM 'leri](./sizes-previous-gen.md#a-series---compute-intensive-instances) 3/2021 tarihinde kullanımdan kaldırma için planlanmaktadır. Daha fazla bilgi için bkz. [HPC geçiş kılavuzu](https://azure.microsoft.com/resources/hpc-migration-guide/).

## <a name="rdma-capable-instances"></a>RDMA özellikli örnekler

HPC VM boyutlarının çoğu (HBv2, HB, HC, H16r, H16mr, A8 ve A9), uzak doğrudan bellek erişimi (RDMA) bağlantısı için bir ağ arabirimi özelliğidir. Seçili [N serisi](./nc-series.md) Boyutlar ' r ' (ND40rs_v2, ND24rs, NC24rs_v3, NC24rs_v2 ve NC24r) ile ayrılmış olarak da RDMA özellikli. Bu arabirim, diğer VM boyutlarında bulunan standart Azure Ethernet ağ arabirimine ek niteliğindedir.

Bu arabirim, RDMA özellikli örneklerin bir InfiniBand (ıB) ağı üzerinden iletişim kurmasını, HBv2 için HDR ücretine, HB, HC, NDv2, FDR tarifelerinin H16r, H16mr ve diğer RDMA özellikli N serisi sanal makinelere ve A8 ve A9 VM 'Ler için QDR ücretlerden çalışmasına izin verir. Bu RDMA özellikleri, bazı Ileti geçirme arabirimi (MPı) uygulamalarının ölçeklenebilirliğini ve performansını artırabilir.

> [!NOTE]
> Azure HPC 'de, InfiniBand için SR-ıOV etkinleştirilmiş olmasına bağlı olarak iki VM sınıfı vardır. Şu anda, H16r, H16mr, NC24r, A8, A9 dışında, Azure 'da daha yeni nesil, RDMA özellikli veya Infiniband özellikli VM 'Ler SR-ıOV etkindir.
> RDMA yalnızca InfiniBand (ıB) ağı üzerinden etkinleştirilir ve tüm RDMA özellikli sanal makineler için desteklenir.
> IB üzerinden IP yalnızca SR-ıOV etkinleştirilmiş VM 'lerde desteklenir.
> RDMA, Ethernet ağı üzerinden etkinleştirilmemiş.

- **Işletim sistemi** -LINUX, HPC VM 'leri için çok iyi desteklenir; CentOS, RHEL, Ubuntu, SUSE gibi kaldırmalar yaygın olarak kullanılır. Windows desteği ile ilgili Windows Server 2016 ve daha yeni sürümler, tüm HPC serisi VM 'lerinde desteklenir. Windows Server 2012 R2, Windows Server 2012, SR-ıOV olmayan VM 'Ler (H16r, H16mr, A8 ve A9) üzerinde de desteklenir. [Windows Server 2012 R2 'Nin HBv2 'de ve 64 ' den fazla (sanal veya fiziksel) çekirdeğe sahip diğer VM 'lerde desteklenmediğini](/windows-server/virtualization/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows)unutmayın. Market 'te desteklenen VM görüntülerinin listesi için [VM görüntülerini](./workloads/hpc/configure.md) ve bunların nasıl uygun şekilde yapılandırılabileceğini görün.

- **InfiniBand ve sürücüler** -InfiniBand etkinleştirilmiş VM 'LERDE, RDMA 'yi etkinleştirmek için uygun sürücüler gereklidir. Linux 'ta hem SR-ıOV hem de SR-ıOV etkin olmayan VM 'Ler için, marketteki CentOS-HPC sanal makine görüntüleri, uygun sürücülerle önceden yapılandırılmış olarak gelir. Ubuntu VM görüntüleri, [Buradaki yönergeler](https://techcommunity.microsoft.com/t5/azure-compute/configuring-infiniband-for-ubuntu-hpc-and-gpu-vms/ba-p/1221351)kullanılarak doğru sürücülerle yapılandırılabilir. Kullanıma hazırlama VM Linux işletim sistemi görüntüleri hakkında daha fazla bilgi için bkz. [LINUX OS Için VM 'Leri yapılandırma ve iyileştirme](./workloads/hpc/configure.md) .

   Linux 'ta, [ınfinibanddriverlinux sanal makine uzantısı](./extensions/hpc-compute-infiniband-linux.md) , The SR-IOV etkin H ve N serisi VM 'lerde [HPC Iş YÜKLERINDE](./workloads/hpc/enable-infiniband.md)RDMA özellikli VM 'lerde InfiniBand 'yi etkinleştirme hakkında daha fazla bilgi edinin.

   Windows 'da, [ınfinibanddriverwindows VM Uzantısı](./extensions/hpc-compute-infiniband-windows.md) , RDMA bağlantısı Için Windows ağ doğrudan SÜRÜCÜLERINI (SR-IOV olmayan VM 'lerde) veya (SR-IOV VM 'lerinde) A8 ve A9 örneklerinin bazı dağıtımlarında, HpcVmDrivers uzantısı otomatik olarak eklenir. HpcVmDrivers VM uzantısının kullanım dışı olduğunu unutmayın; güncellenmeyecektir.

   VM uzantısını bir VM 'ye eklemek için [Azure PowerShell](/powershell/azure/) cmdlet 'lerini kullanabilirsiniz. Daha fazla bilgi için bkz. [sanal makine uzantıları ve özellikleri](./extensions/overview.md). [Klasik dağıtım modelinde](/previous-versions/azure/virtual-machines/windows/classic/agents-and-extensions-classic)dağıtılan VM 'ler için uzantılara de çalışabilirsiniz.

- **MPI** -Azure 'daki SR-ıOV etkinleştirilmiş VM boyutları, her türlü MPI 'ın Mellanox ile kullanılmasına izin verir. SR-ıOV olmayan VM 'lerde desteklenen MPı uygulamaları, VM 'Ler arasında iletişim kurmak için Microsoft ağ doğrudan (ND) arabirimini kullanır. Bu nedenle, yalnızca Microsoft MPı (MS-MPı) 2012 R2 veya üzeri ve Intel MPı 5. x sürümleri desteklenir. Intel MPı çalışma zamanı kitaplığı 'nın sonraki sürümleri (2017, 2018), Azure RDMA sürücüleriyle uyumlu olmayabilir veya olmayabilir. Bkz. Azure üzerinde MPı üzerinde HPC VM 'Leri ayarlama hakkında daha fazla bilgi için bkz. [HPC Için Setup MPI](./workloads/hpc/setup-mpi.md) .

- **RDMA ağ adresi alanı** -Azure 'daki RDMA ağı, 172.16.0.0/16 adres alanını ayırır. MPı uygulamalarını bir Azure sanal ağında dağıtılan örneklerde çalıştırmak için, sanal ağ adres alanının RDMA ağıyla çakışmadığından emin olun.

## <a name="cluster-configuration-options"></a>Küme yapılandırma seçenekleri

Azure, RDMA ağını kullanarak iletişim kurabilen Windows HPC VM kümeleri oluşturmak için çeşitli seçenekler sunar; örneğin: 

- **Sanal makineler**  -RDMA özellikli HPC VM 'lerini aynı ölçek kümesine veya kullanılabilirlik kümesine dağıtın (Azure Resource Manager dağıtım modelini kullandığınızda). Klasik dağıtım modelini kullanıyorsanız, VM 'Leri aynı bulut hizmetinde dağıtın.

- **Sanal Makine Ölçek Kümeleri** -bir sanal makine ölçek kümesinde dağıtımı, ölçek kümesi içindeki InfiniBand iletişimi için tek bir yerleştirme grubuyla sınırlandırtığınızdan emin olun. Örneğin, bir Kaynak Yöneticisi şablonunda, `singlePlacementGroup` özelliğini olarak ayarlayın `true` . Özelliği ile birlikte kullanılabilecek maksimum ölçek kümesi boyutunun, `singlePlacementGroup` `true` varsayılan olarak 100 sanal makinelerde olduğunu unutmayın. HPC iş ölçeği, tek bir Kiracıdaki 100 sanal makinelerinden daha yüksekse, bir artış isteyebilir, [çevrimiçi müşteri destek talebi](../azure-portal/supportability/how-to-create-azure-support-request.md) ücretsiz olarak açabilirsiniz. Tek bir ölçek kümesindeki sanal makine sayısı sınırı 300 'e artırılabilir. Kullanılabilirlik kümeleri kullanarak VM 'Leri dağıttığınızda, en fazla sınır kullanılabilirlik kümesi başına 200 VM 'dir.

- **Sanal makineler arasında MPI** -sanal makineler (VM 'ler) arasında RDMA (ör. MPI iletişimi kullanma) gerekiyorsa, VM 'lerin aynı sanal makine ölçek kümesi veya kullanılabilirlik kümesi içinde olduğundan emin olun.

- **Azure CycleCloud** -MPI işlerini çalıştırmak Için [Azure CYCLECLOUD](/azure/cyclecloud/) 'te bir HPC kümesi oluşturun.

- **Azure Batch** MPI iş yüklerini çalıştırmak için bir [Azure Batch](../batch/index.yml) havuzu oluşturun. MPı uygulamalarını Azure Batch ile çalıştırırken yoğun işlem yoğunluklu örnekler kullanmak için bkz. [Azure Batch 'de Ileti geçirme arabirimi (MPı) uygulamalarını çalıştırmak için çok örnekli görevleri kullanma](../batch/batch-mpi.md).

- **MICROSOFT HPC Pack**  -  [HPC Pack](/powershell/high-performance-computing/overview) , RDMA özellikli Linux sanal makinelerinde DAĞıTıLDıĞıNDA Azure RDMA AĞıNı kullanan MS-MPI için bir çalışma zamanı ortamı içerir. Örneğin, dağıtımlar için bkz. [MPI uygulamalarını çalıştırmak IÇIN HPC Pack Ile LINUX RDMA kümesi ayarlama](/powershell/high-performance-computing/hpcpack-linux-openfoam).

## <a name="deployment-considerations"></a>Dağıtma konuları

- **Azure aboneliği** : birkaç işlem yoğunluğu yoğun örneği dağıtmak için, Kullandıkça Öde aboneliğine veya diğer satın alma seçeneklerine göz önünde bulundurun. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/) kullanıyorsanız, yalnızca sınırlı sayıda Azure işlem çekirdeği kullanabilirsiniz.

- **Fiyatlandırma ve kullanılabilirlik** -bu VM boyutları yalnızca standart fiyatlandırma katmanında sunulur. Azure bölgelerinde kullanılabilirlik için [bölgeye göre kullanılabilir ürünleri](https://azure.microsoft.com/global-infrastructure/services/) denetleyin.

- **Çekirdek kotası** – Azure aboneliğinizdeki çekirdek kotasını varsayılan değerden artırmanız gerekebilir. Aboneliğiniz Ayrıca, H serisi de dahil olmak üzere belirli VM boyutu ailelerinde dağıtabileceğiniz çekirdek sayısını sınırlayabilir. Kota artışı istemek için, ücretsiz [bir çevrimiçi müşteri destek isteği açın](../azure-portal/supportability/how-to-create-azure-support-request.md) . (Varsayılan sınırlar, abonelik kategorime bağlı olarak değişebilir.)

  > [!NOTE]
  > Büyük ölçekli kapasite gereksinimleriniz varsa Azure desteği 'ne başvurun. Azure kotaları, kapasite garantisi değil kredi limitlerdir. Kotasından bağımsız olarak yalnızca kullandığınız çekirdekler için ücretlendirilirsiniz.
  
- **Sanal ağ** : yoğun işlem yoğunluklu örnekleri kullanmak Için bir Azure [sanal ağı](https://azure.microsoft.com/documentation/services/virtual-network/) gerekli değildir. Ancak, birçok dağıtım için, şirket içi kaynaklara erişmeniz gerekiyorsa, en az bir bulut tabanlı Azure sanal ağı veya siteden siteye bağlantı gerekir. Gerektiğinde, örnekleri dağıtmak için yeni bir sanal ağ oluşturun. Benzeşim grubundaki bir sanal ağa işlem yoğunluklu VM 'Lerin eklenmesi desteklenmez.

- **Yeniden boyutlandırma** – özel donanımlar nedeniyle, yalnızca aynı büyüklükte aile (h serisi veya N serisi) içindeki işlem yoğunluğu örnekleri yeniden boyutlandırabilirsiniz. Örneğin, bir h serisi VM 'yi yalnızca bir H serisi boyutundan diğerine yeniden boyutlandırabilirsiniz. InfiniBand sürücü desteğinin ve NVMe disklerinin yanı sıra belirli VM 'Ler için göz önünde bulundurulması gerekebilir.


## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaçlı](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Sanal makinelerinizi yapılandırma](./workloads/hpc/configure.md)hakkında daha fazla bilgi edinin, [InfiniBand 'Yi etkinleştirir](./workloads/hpc/enable-infiniband.md) [ve Azure](./workloads/hpc/setup-mpi.md) için HPC uygulamalarını [HPC iş yüklerinde](./workloads/hpc/overview.md)optimize edin.
- En son duyurular ve bazı HPC örnekleri hakkında bilgi edinin ve [Azure Işlem teknik topluluk bloglarında](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute)bu sonuçları elde edin.
- Çalıştırılan HPC iş yüklerinin daha yüksek düzey mimari görünümü için bkz. [Azure 'Da yüksek performanslı bilgi işlem (HPC)](/azure/architecture/topics/high-performance-computing/).
