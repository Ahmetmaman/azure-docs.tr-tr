---
title: Azure portal kullanarak Azure 'da SQL Server sanal makineleri yönetme | Microsoft Docs
description: Azure 'da barındırılan bir SQL Server VM için Azure portal SQL sanal makine kaynağına erişmeyi öğrenin.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.subservice: management
ms.topic: how-to
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/13/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 68c7805136a7361a64a6ff6dfbb9e7d910b2ea9b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "97357951"
---
# <a name="manage-sql-server-vms-in-azure-by-using-the-azure-portal"></a>Azure 'da SQL Server VM 'Leri Azure portal kullanarak yönetin
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

[Azure Portal](https://portal.azure.com), [**SQL sanal makineler**](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.SqlVirtualMachine%2FSqlVirtualMachines) kaynağı, Azure VM 'lerinde SQL Server yönetmek için bağımsız bir yönetim hizmetidir. Bunu, tüm SQL Server sanal makinelerinizi aynı anda görüntülemek ve SQL Server adanmış ayarları değiştirmek için kullanabilirsiniz: 

![SQL sanal makineler kaynağı](./media/manage-sql-vm-portal/sql-vm-manage.png)


## <a name="remarks"></a>Açıklamalar

- Azure 'da SQL Server VM 'lerinizi görüntülemek ve yönetmek için [**SQL sanal makineler**](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.SqlVirtualMachine%2FSqlVirtualMachines) kaynağını kullanmanızı öneririz. Ancak şu anda **SQL sanal makineler** kaynağı, [destek sonu](sql-server-2008-extend-end-of-support.md) SQL Server VM 'lerinin yönetimini desteklemez. Destek sonu SQL Server sanal makinelerinizin ayarlarını yönetmek için, bunun yerine kullanım dışı [SQL Server Yapılandırma sekmesini](#access-the-sql-server-configuration-tab) kullanın. 
- **SQL sanal makineler** kaynağı yalnızca [SQL IaaS aracısı uzantısına kayıtlı](sql-agent-extension-manually-register-single-vm.md)SQL Server VM 'ler için kullanılabilir. 


## <a name="access-the-sql-virtual-machines-resource"></a>SQL sanal makineler kaynağına erişin
**SQL sanal makineler** kaynağına erişmek için aşağıdakileri yapın:

1. [Azure portalını](https://portal.azure.com) açın. 
1. **Tüm hizmetler**' i seçin. 
1. Arama kutusuna **SQL sanal makinelerini** girin.
1. (İsteğe bağlı): Bu seçeneği **Sık Kullanılanlar** menünüzde eklemek için **SQL sanal makineler** ' in yanındaki yıldızı seçin. 
1. **SQL sanal makinelerini** seçin. 

   ![Tüm hizmetlerde SQL Server sanal makineler bulun](./media/manage-sql-vm-portal/sql-vm-search.png)

1. Portal, abonelik içinde kullanılabilir olan tüm SQL Server VM 'Leri listeler. **SQL sanal makineler** kaynağını açmak için yönetmek istediğiniz birini seçin. SQL Server VM görünmediğinde arama kutusunu kullanın. 

   ![Tüm kullanılabilir SQL Server VM 'Leri](./media/manage-sql-vm-portal/all-sql-vms.png)

   SQL Server VM seçtiğinizde **SQL sanal makineler** kaynağı açılır: 


   ![SQL sanal makineler kaynağını görüntüle](./media/manage-sql-vm-portal/sql-vm-resource.png)

> [!TIP]
> **SQL sanal makineler** kaynağı adanmış SQL Server ayarları içindir. **Sanal makine** kutusunda VM 'nin adını seçerek VM 'ye özgü ayarları açın, ancak SQL Server dışlamalı. 

## <a name="access-the-sql-server-configuration-tab"></a>SQL Server yapılandırma sekmesine erişin
**SQL Server yapılandırma** sekmesi kullanım dışı bırakıldı. Şu anda, [destek sonu](sql-server-2008-extend-end-of-support.md) SQL Server VM 'LERI ve [SQL IaaS Aracısı uzantısına kayıtlı](sql-agent-extension-manually-register-single-vm.md)olmayan SQL Server VM 'leri yönetmeye yönelik tek yöntem budur.

Kullanım dışı **SQL Server yapılandırma** sekmesine erişmek için **sanal makineler** kaynağına gidin. Aşağıdaki adımları kullanın:

1. [Azure portalını](https://portal.azure.com) açın. 
1. **Tüm hizmetler**' i seçin. 
1. Arama kutusuna **sanal makineler** girin.
1. (İsteğe bağlı): Bu seçeneği **Sık Kullanılanlar** menünüzde eklemek için **sanal makineler** ' in yanındaki yıldızı seçin. 
1. **Sanal makineler**'i seçin. 

   ![Sanal makineleri ara](./media/manage-sql-vm-portal/vm-search.png)

1. Portal, abonelikteki tüm sanal makineleri listeler. **Sanal makineler** kaynağını açmak için yönetmek istediğiniz birini seçin. SQL Server VM görünmediğinde arama kutusunu kullanın. 
1. SQL Server VM yönetmek için **Ayarlar** bölmesinde **SQL Server yapılandırma** ' yı seçin. 

   ![SQL Server yapılandırması](./media/manage-sql-vm-portal/sql-vm-configuration.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makaleleri inceleyin: 

* [Windows VM 'de SQL Server genel bakış](sql-server-on-azure-vm-iaas-what-is-overview.md)
* [Windows VM 'de SQL Server hakkında SSS](frequently-asked-questions-faq.md)
* [Windows VM üzerinde SQL Server için fiyatlandırma Kılavuzu](pricing-guidance.md)
* [Windows VM 'de SQL Server için sürüm notları](doc-changes-updates-release-notes.md)


