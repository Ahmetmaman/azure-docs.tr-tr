---
title: include dosyası
description: include dosyası
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 07/08/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 754562487f0fe9f825107445a3059cc15a1faa26
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39146263"
---
Azure dosya eşitleme hizmeti sunucudan erişilebilir olduğunda bu hata oluşabilir. Aşağıdaki adımları aracılığıyla bu hatayı gidermek:

1. Windows hizmeti doğrulamak `FileSyncSvc.exe` güvenlik duvarı tarafından engellenip engellenmediğini.
2. Bağlantı noktası 443 giden bağlantı Azure dosya eşitleme hizmeti için açık olduğunu doğrulayın. İle bunu yapabilirsiniz `Test-NetConnection` cmdlet'i. URL `<azure-file-sync-endpoint>` yer tutucu aşağıda bulunan [Azure dosya Eşitleme proxy'si ve güvenlik duvarı ayarlarını](../articles/storage/files/storage-sync-files-firewall-and-proxy.md#firewall) belge. 

    ```PowerShell
    Test-NetConnection -ComputerName <azure-file-sync-endpoint> -Port 443
    ```

3. Proxy yapılandırması beklenen şekilde ayarlandığından emin olun. Bu ile yapılabilir `Get-StorageSyncProxyConfiguration` cmdlet'i. Azure dosya eşitleme bulunabilir için proxy yapılandırması'nı yapılandırma hakkında daha fazla bilgi [Azure dosya Eşitleme proxy'si ve güvenlik duvarı ayarlarını](../articles/storage/files/storage-sync-files-firewall-and-proxy.md#firewall).

    ```PowerShell
    $agentPath = "C:\Program Files\Azure\StorageSyncAgent"
    Import-Module "$agentPath\StorageSync.Management.ServerCmdlets.dll"
    Get-StorageSyncProxyConfiguration
    ```
    
4. Ağ bağlantısı sorun giderme Ek Yardım almak için ağ yöneticinize başvurun.