---
title: SQL Server Edition yerinde değişiklik
description: Azure 'daki SQL Server sanal makinenizin sürümünü, daha fazla özelliği etkinleştirmek üzere maliyeti düşürmek veya yükseltmek üzere düşürme olarak değiştirme hakkında bilgi edinin.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.topic: how-to
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/14/2020
ms.author: mathoma
ms.reviewer: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 801a9a112615bd6220b5f273b51ed39248ebcd45
ms.sourcegitcommit: dc342bef86e822358efe2d363958f6075bcfc22a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94556480"
---
# <a name="in-place-change-of-sql-server-edition-on-azure-vm"></a>Azure VM'deki SQL Server sürümünü yerinde değiştirme
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Bu makalede, Azure 'da bir Windows sanal makinesinde SQL Server sürümünün nasıl değiştirileceği açıklanır. 

SQL Server sürümü ürün anahtarı tarafından belirlenir ve yükleme medyası kullanılarak yükleme işlemi sırasında belirtilir. Sürüm, SQL Server üründe hangi [özelliklerin](/sql/sql-server/editions-and-components-of-sql-server-2017) kullanılabildiğini belirler. SQL Server sürümünü yükleme medyası ile değiştirebilir ve daha fazla özelliği etkinleştirmek üzere maliyeti düşürmek veya yükseltmek için düşürme yapın.

SQL Server sürümü dahili olarak SQL Server VM değiştirildikten sonra, Faturalama amacıyla Azure portal SQL Server sürüm özelliğini güncelleştirmeniz gerekir. 

## <a name="prerequisites"></a>Ön koşullar

SQL Server sürümünde yerinde bir değişiklik yapmak için şunlar gerekir: 

- Bir [Azure aboneliği](https://azure.microsoft.com/free/).
- [SQL IaaS Aracısı uzantısına](sql-agent-extension-manually-register-single-vm.md)kayıtlı [Windows üzerinde SQL Server VM](./create-sql-vm-portal.md) .
- SQL Server **istenen sürümü** ile medyayı kurun. [Yazılım Güvencesi](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default)'ne sahip olan müşteriler, yükleme medyasını [Toplu Lisanslama Merkezi](https://www.microsoft.com/Licensing/servicecenter/default.aspx)'nden alabilir. Yazılım Güvencesi sahibi olmayan müşteriler, istenen sürümü olan (genellikle içinde bulunan) bir Azure Marketi SQL Server VM görüntüsünden kurulum medyasını kullanabilir `C:\SQLServerFull` . 


## <a name="upgrade-an-edition"></a>Sürüm yükseltme

> [!WARNING]
> SQL Server sürümünü yükseltmek, Analysis Services ve R hizmetleri gibi ilişkili hizmetlerle birlikte SQL Server hizmetini yeniden başlatır. 

SQL Server sürümünü yükseltmek için, istenen SQL Server sürümü için SQL Server Kurulum medyasını edinin ve ardından aşağıdakileri yapın:

1. SQL Server yükleme medyasından Setup.exe açın. 
1. **Bakım** ' a gidin ve **sürüm yükseltme** seçeneğini belirleyin. 

   ![SQL Server sürümünü yükseltmek için seçim](./media/change-sql-server-edition/edition-upgrade.png)

1. **Yükseltmeye hazırlanma** sayfasına ulaşana kadar **İleri** ' yi seçin ve ardından **Yükselt** ' i seçin. Değişiklik etkinleşirken kurulum penceresi birkaç dakika yanıt vermeyi durdurabilir. **Tamamlanmış** bir sayfa, sürüm yükseltmeniz işleminin bittiğini doğrulayacaktır. 

SQL Server sürümü yükseltildikten sonra, Azure portal SQL Server sanal makinesinin sürüm özelliğini değiştirin. Bu işlem, bu VM ile ilişkili meta verileri ve faturaları güncelleştirir.

## <a name="downgrade-an-edition"></a>Sürümü düşürme

SQL Server sürümünün indirgenmesini sağlamak için, SQL Server tamamen kaldırmanız ve istenen sürüm kurulum medyası ile yeniden yüklemeniz gerekir. 

> [!WARNING]
> SQL Server kaldırıldığında ek kesinti oluşabilir. 

Aşağıdaki adımları izleyerek SQL Server sürümünü indirgeyebilmeniz gerekir:

1. Sistem veritabanları da dahil olmak üzere tüm veritabanlarını yedekleyin. 
1. Sistem veritabanlarını (ana, model ve msdb) yeni bir konuma taşıyın. 
1. SQL Server ve tüm ilişkili hizmetleri tamamen kaldırın. 
1. Sanal makineyi yeniden başlatın. 
1. SQL Server istenen sürümü olan medyayı kullanarak SQL Server yükleyin.
1. En son hizmet paketlerini ve toplu güncelleştirmeleri yükler.  
1. Yükleme sırasında oluşturulan yeni sistem veritabanlarını, daha önce farklı bir konuma taşıdığınız sistem veritabanları ile değiştirin. 

SQL Server sürümü indirgendikten sonra, Azure portal SQL Server sanal makinesinin sürüm özelliğini değiştirin. Bu işlem, bu VM ile ilişkili meta verileri ve faturaları güncelleştirir.

## <a name="change-edition-in-portal"></a>Portalda sürümü Değiştir 

Yükleme medyasını kullanarak SQL Server sürümünü değiştirmişseniz ve SQL Server VM [SQL IaaS Aracısı uzantısı](sql-agent-extension-manually-register-single-vm.md)ile kaydettirdiyseniz, faturalandırma amacıyla SQL Server VM sürüm özelliğini değiştirmek için Azure Portal kullanabilirsiniz. Bunu yapmak için aşağıdaki adımları izleyin: 

1. [Azure portalında](https://portal.azure.com) oturum açın. 
1. SQL Server sanal makine kaynağına gidin. 
1. **Ayarlar** altında **Yapılandır** ' ı seçin. Ardından, **Sürüm** ' ün altındaki açılan listeden istediğiniz SQL Server sürümünüzü seçin. 

   ![Sürüm meta verilerini değiştirme](./media/change-sql-server-edition/edition-change-in-portal.png)

1. Önce SQL Server sürümünü değiştirmeniz gerektiğini ve sürüm özelliğinin SQL Server sürümüyle eşleşmesi gerektiğini belirten uyarıyı gözden geçirin. 
1. Sürüm meta verileri değişikliklerinizi uygulamak için **Uygula** ' yı seçin. 


## <a name="remarks"></a>Açıklamalar

- SQL Server VM Edition özelliği, hem Kullandıkça Öde hem de kendi lisans türlerini getir dahil olmak üzere tüm SQL Server sanal makineler için yüklenmiş SQL Server örneğinin sürümüyle eşleşmelidir.
- SQL Server VM kaynağınızı düşürülebiliyorsanız, görüntünün sabit kodlanmış sürüm ayarına geri dönebilirsiniz.
- Sürümü değiştirme özelliği, SQL IaaS Aracısı uzantısının bir özelliğidir. Azure Marketi görüntüsünü Azure portal aracılığıyla dağıtmak, otomatik olarak SQL IaaS Aracısı Uzantısı ile bir SQL Server VM kaydeder. Ancak, kendi kendini yükleyen SQL Server müşterilerin SQL Server VM el ile [kaydetmesi](sql-agent-extension-manually-register-single-vm.md)gerekir.
- Bir kullanılabilirlik kümesine SQL Server VM eklendiğinde VM 'nin yeniden oluşturulması gerekir. Bir kullanılabilirlik kümesine eklenen tüm VM 'Ler varsayılan sürüme geri döner ve sürüm yeniden değiştirilmeyecektir.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın: 

* [Windows VM 'de SQL Server genel bakış](sql-server-on-azure-vm-iaas-what-is-overview.md)
* [Windows VM 'de SQL Server hakkında SSS](frequently-asked-questions-faq.md)
* [Windows VM üzerinde SQL Server için fiyatlandırma Kılavuzu](pricing-guidance.md)
* [Windows VM 'de SQL Server için sürüm notları](doc-changes-updates-release-notes.md)