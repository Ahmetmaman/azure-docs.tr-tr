---
title: Ağ Topolojileri için Azure veritabanı geçiş hizmetini kullanarak Azure SQL veritabanı yönetilen örneği geçişleri | Microsoft Docs
description: Veritabanı geçiş hizmeti için kaynak ve hedef yapılandırmalar hakkında bilgi edinin.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 11/8/2018
ms.openlocfilehash: 9b036b74141ce2091d2e68b68d10c44a56a8696d
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2018
ms.locfileid: "51300701"
---
# <a name="network-topologies-for-azure-sql-db-managed-instance-migrations-using-the-azure-database-migration-service"></a>İçin Azure veritabanı geçiş hizmetini kullanarak Azure SQL DB yönetilen örneği geçişlerinin ağ topolojileri
Bu makalede, kapsamlı bir geçiş deneyimi için Azure SQL veritabanı yönetilen örneği şirket içi SQL Server'lardaki sağlamak için Azure veritabanı geçiş hizmeti çalışabilirsiniz çeşitli ağ topolojileri açıklanır.

## <a name="azure-sql-database-managed-instance-configured-for-hybrid-workloads"></a>Azure SQL veritabanı yönetilen hibrit iş yükleri için yapılandırılmış örneği 
Azure SQL veritabanı yönetilen örneği, şirket içi ağınıza bağlıysa bu topolojiyi kullanın. Bu yaklaşım, en basit ağ yönlendirme sağlar ve geçiş sırasında en yüksek veri performansı verir.

![Hibrit iş yükleri için ağ topolojisi](media\resource-network-topologies\hybrid-workloads.png)

**Gereksinimleri**
- Bu senaryoda, Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti örneği aynı Azure sanal ağınızda oluşturulur, ancak bunlar farklı alt ağları kullanın.  
- Bu senaryoda kullanılan VNET kullanarak şirket içi ağa ayrıca bağlı [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).

## <a name="azure-sql-database-managed-instance-isolated-from-the-on-premises-network"></a>Azure SQL veritabanı yönetilen şirket içi ağdan yalıtılmış bir örneği
Bir veya daha fazla aşağıdaki senaryolarda ortamınızın gerektiriyorsa, bu ağ topolojisi kullanın:
- Azure SQL veritabanı yönetilen örneği, şirket içi bağlantısı kesilmişse, ancak Azure veritabanı geçiş hizmeti örneğiniz şirket içi ağa bağlıdır.
- Rol tabanlı erişim denetimi (RBAC) ilkelerinin yerinde olduğundan ve Azure SQL veritabanı yönetilen örneği barındıran aynı aboneliğe erişmek için kullanıcıları sınırlamak gerekiyorsa.
- Azure SQL veritabanı yönetilen örneği için sanal ağlar ve Azure veritabanı geçiş hizmeti, farklı Aboneliklerde kullanıldığını.

![Yönetilen şirket içi ağdan yalıtılmış bir örneği için ağ topolojisi](media\resource-network-topologies\mi-isolated-workload.png)

**Gereksinimleri**
- Bu senaryo için Azure veritabanı geçiş hizmeti kullanan sanal ağın da şirket içi ağa kullanarak bağlanmalıdır (https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Ayarlanan [VNET ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti için kullanılan sanal ağ arasında.


## <a name="cloud-to-cloud-migrations-shared-vnet"></a>Bulut buluta geçiş: sanal ağ paylaşılan

Kaynak SQL Server bir Azure sanal Makinesinde barındırılan ve Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti ile aynı sanal ağ paylaşımları varsa bu topolojiyi kullanın.

![Paylaşılan bir sanal ağ ile bulut buluta geçiş ağ topolojisi](media\resource-network-topologies\cloud-to-cloud.png)

**Gereksinimleri**
- Ek gereksinimi yoktur.

## <a name="cloud-to-cloud-migrations-isolated-vnet"></a>Buluta geçiş için bulut: sanal ağ yalıtılmış

Bir veya daha fazla aşağıdaki senaryolarda ortamınızın gerektiriyorsa, bu ağ topolojisi kullanın:
- Azure SQL veritabanı yönetilen örneği, yalıtılmış bir VNET'te sağlanır.
- Rol tabanlı erişim denetimi (RBAC) ilkelerinin yerinde olduğundan ve Azure SQL veritabanı yönetilen örneği barındıran aynı aboneliğe erişmek için kullanıcıları sınırlamak gerekiyorsa.
- Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti için kullanılan Vnet'ler farklı Aboneliklerde bulunuyor.

![Ağ topolojisi yalıtılmış bir sanal ağ ile bulut buluta geçiş](media\resource-network-topologies\cloud-to-cloud-isolated.png)

**Gereksinimleri**
- Ayarlanan [VNET ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti için kullanılan sanal ağ arasında.

## <a name="inbound-security-rules"></a>Gelen güvenlik kuralları

| **ADI**   | **BAĞLANTI NOKTASI** | **PROTOKOLÜ** | **KAYNAK** | **HEDEF** | **EYLEM** |
|------------|----------|--------------|------------|-----------------|------------|
| DMS_subnet | Herhangi biri      | Herhangi biri          | DMS ALT AĞ | Herhangi biri             | İzin Ver      |

## <a name="outbound-security-rules"></a>Giden güvenlik kuralları

| **ADI**                  | **BAĞLANTI NOKTASI**                                              | **PROTOKOLÜ** | **KAYNAK** | **HEDEF**           | **EYLEM** | **Kural nedeni**                                                                                                                                                                              |
|---------------------------|-------------------------------------------------------|--------------|------------|---------------------------|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| yönetim                | 443,9354                                              | TCP          | Herhangi biri        | Herhangi biri                       | İzin Ver      | Service bus ve Azure blob depolama ile yönetim düzlemi iletişim. <br/>(Microsoft eşdüzey hizmet sağlama etkinse, bu kural gerekmeyebilir.)                                                             |
| Tanılama               | 12000                                                 | TCP          | Herhangi biri        | Herhangi biri                       | İzin Ver      | DMS, sorun giderme amacıyla tanılama bilgilerini toplamak için bu kural kullanır.                                                                                                                      |
| SQL kaynak sunucusu         | 1433 (veya SQL Server dinlediği TCP/IP bağlantı noktası) | TCP          | Herhangi biri        | Şirket içi adres alanı | İzin Ver      | DMS SQL Server Kaynak bağlantısı <br/>(Siteden siteye bağlantınız varsa, bu kural gerekmeyebilir.)                                                                                       |
| SQL Server'ın adlandırılmış örneği | 1434                                                  | UDP          | Herhangi biri        | Şirket içi adres alanı | İzin Ver      | SQL Server adlandırılmış örneği DMS kaynak bağlantısı <br/>(Siteden siteye bağlantınız varsa, bu kural gerekmeyebilir.)                                                                        |
| SMB paylaşımı                 | 445                                                   | TCP          | Herhangi biri        | Şirket içi adres alanı | İzin Ver      | SMB ağ paylaşımı için Azure sanal makinesinde Azure SQL veritabanı ve SQL Server'lar geçişler için veritabanı yedekleme dosyaları depolamak DMS <br/>(Siteden siteye bağlantı varsa, bu kural gerekebilir değil). |
| DMS_subnet                | Herhangi biri                                                   | Herhangi biri          | Herhangi biri        | DMS_Subnet                | İzin Ver      |                                                                                                                                                                                                  |

## <a name="see-also"></a>Ayrıca Bkz.
- [SQL Server'ı Azure SQL veritabanı yönetilen örneğine geçirme](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Azure Veritabanı Geçiş Hizmeti'ni kullanmak için önkoşullara genel bakış](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Azure portalını kullanarak bir sanal ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>Sonraki adımlar
Bölgesel kullanılabilirlik genel Önizleme süresince ve Azure veritabanı geçiş Hizmeti'nin genel bakış için bkz [Azure veritabanı geçiş hizmeti önizlemesi nedir](dms-overview.md). 