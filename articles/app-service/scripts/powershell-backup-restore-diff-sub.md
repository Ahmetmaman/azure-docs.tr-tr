---
title: 'PowerShell: yedeği başka bir aboneliğe geri yükleme'
description: App Service dağıtımı ve yönetimini otomatik hale getirmek için Azure PowerShell nasıl kullanacağınızı öğrenin. Bu örnek, bir yedeklemenin başka bir abonelikte nasıl geri yükleneceğini gösterir.
author: msangapu-msft
tags: azure-service-management
ms.assetid: a2a27d94-d378-4c17-a6a9-ae1e69dc4a72
ms.topic: sample
ms.date: 11/21/2018
ms.author: msangapu
ms.custom: mvc, seodec18, devx-track-azurepowershell
ms.openlocfilehash: a0728d9926cc7f5d8b200a9003353b015dd3a97c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89075928"
---
# <a name="restore-a-web-app-from-a-backup-in-another-subscription-using-powershell"></a>PowerShell kullanarak bir Web uygulamasını başka bir abonelikteki yedekten geri yükleme

Bu örnek betik, daha önce tamamlanmış bir yedeği var olan bir Web uygulamasından alır ve onu başka bir abonelikteki Web uygulamasına geri yükler. 

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/)bulunan yönergeyi kullanarak Azure PowerShell yükleyip `Connect-AzAccount` Azure ile bağlantı oluşturmak için öğesini çalıştırın. 

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/backup-restore-diff-sub/backup-restore-diff-sub.ps1?highlight=1-6 "Restore a web app from a backup in another subscription")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Artık Web uygulamasına ihtiyacınız yoksa, kaynak grubunu, Web uygulamasını ve tüm ilgili kaynakları kaldırmak için aşağıdaki komutu kullanın.

```powershell
Remove-AzResourceGroup -Name $resourceGroupName -Force
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Add-AzAccount](/powershell/module/az.accounts/connect-azaccount) | Azure Resource Manager cmdlet’i istekleri için kullanılacak bir kimliği doğrulanmış hesap ekler.  |
| [Get-AzWebAppBackupList](/powershell/module/az.websites/get-azwebappbackuplist) | Bir web uygulamasının yedekleme listesini alır. |
| [New-AzWebApp](/powershell/module/az.websites/new-azwebapp) | Bir Web uygulaması oluşturur |
| [Restore-AzWebAppBackup](/powershell/module/az.websites/restore-azwebappbackup) | Bir Web uygulamasını daha önce tamamlanmış bir yedekten geri yükler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/).

Azure App Service Web Apps için ek Azure PowerShell örneklerini [Azure PowerShell örnekleri](../samples-powershell.md) bölümünde bulabilirsiniz.
