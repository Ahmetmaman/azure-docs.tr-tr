---
title: Yerleşik güvenlik duvarında bağlantı noktası güvenliğini doğrulama
description: Azure SQL yönetilen örneği 'nde yerleşik güvenlik duvarı korumasını doğrulamayı öğrenin.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein
ms.date: 12/04/2018
ms.openlocfilehash: da9f3e2b6b8936c74e20a226b606082fb3e0d3ea
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91263173"
---
# <a name="verify-the-azure-sql-managed-instance-built-in-firewall"></a>Azure SQL Yönetilen Örneği yerleşik güvenlik duvarını doğrulama
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL yönetilen örnek [zorunlu gelen güvenlik kuralları](connectivity-architecture-overview.md#mandatory-inbound-security-rules) , yönetim bağlantı noktalarını 9000, 9003, 1438, 1440 ve 1452 ' yi, ağ güvenlik grubundaki (NSG) SQL yönetilen örneğini koruyan **herhangi bir kaynaktan** açık olmasını gerektirir. Bu bağlantı noktaları NSG düzeyinde açık olsa da, yerleşik güvenlik duvarı tarafından ağ düzeyinde korunur.

## <a name="verify-firewall"></a>Güvenlik duvarını doğrulama

Bu bağlantı noktalarını doğrulamak için, bu bağlantı noktalarını sınamak üzere herhangi bir güvenlik tarayıcısı aracını kullanın. Aşağıdaki ekran görüntüsünde, bu araçlardan birinin nasıl kullanılacağı gösterilmektedir.

![Yerleşik güvenlik duvarı doğrulanıyor](./media/management-endpoint-verify-built-in-firewall/03_verify_firewall.png)

## <a name="next-steps"></a>Sonraki adımlar

SQL yönetilen örneği ve bağlantısı hakkında daha fazla bilgi için bkz. [Azure SQL yönetilen örnek bağlantısı mimarisi](connectivity-architecture-overview.md).
