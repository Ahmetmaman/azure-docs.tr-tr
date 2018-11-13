---
title: Azure Backup sunucusu V2 sessiz yüklemesi
description: Azure Backup sunucusu V2 sessizce yüklemek için bir PowerShell betiğini kullanın. Bu tür bir yükleme, katılımsız bir yükleme olarak da adlandırılır.
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: markgal
ms.openlocfilehash: 3e106d7f669cf14014114ed0fe63651a1a2fe0eb
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51578128"
---
# <a name="run-an-unattended-installation-of-azure-backup-server"></a>Azure Backup sunucusu katılımsız yükleme çalıştırma

Azure Backup sunucusu için katılımsız bir yükleme çalıştırmayı öğrenin.

Azure Backup sunucusu V1 yüklüyorsanız bu adımları geçerli değildir.

## <a name="install-backup-server"></a>Backup Sunucusu'nu Yükle

1. Sunucuda barındıran Azure Backup sunucusu V2 veya daha sonra bir metin dosyası oluşturun. (Dosyayı Not Defteri'nde veya başka bir metin düzenleyicisinde oluşturabilirsiniz.) Dosyayı MABSSetup.ini kaydedin.

2. Aşağıdaki kod MABSSetup.ini dosyaya yapıştırın. Köşeli ayraçlar içindeki metni değiştirin (\< \>) ortamınızdaki değerlerle. Aşağıda bir örnek gösterilmiştir:

  ```
  [OPTIONS]
  UserName=administrator
  CompanyName=<Microsoft Corporation>
  SQLMachineName=localhost
  SQLInstanceName=<SQL instance name>
  SQLMachineUserName=administrator
  SQLMachinePassword=<admin password>
  SQLMachineDomainName=<machine domain>
  ReportingMachineName=localhost
  ReportingInstanceName=<reporting instance name>
  SqlAccountPassword=<admin password>
  ReportingMachineUserName=<username>
  ReportingMachinePassword=<reporting admin password>
  ReportingMachineDomainName=<domain>
  VaultCredentialFilePath=<vault credential full path and complete name>
  SecurityPassphrase=<passphrase>
  PassphraseSaveLocation=<passphrase save location>
  UseExistingSQL=<1/0 use or do not use existing SQL>
  ```

3. Dosyayı kaydedin. Daha sonra yükleme sunucusundaki yükseltilmiş bir komut istemi, şu komutu girin:

  ```
  start /wait <cdlayout path>/Setup.exe /i  /f <.ini file path>/setup.ini /L <log path>/setup.log
  ```

Bu bayraklar yükleme için kullanabilirsiniz:</br>
**/f**: .ini dosyasının yolu</br>
**/l**: günlük dosyası yolu</br>
**/i**: yükleme yolu</br>
**/x**: yol kaldırma</br>

## <a name="next-steps"></a>Sonraki adımlar
Backup sunucusu yükledikten sonra sunucunuzu hazırlama veya bir iş yükü korumaya başlamak öğrenin.

- [Backup sunucusu iş yükleri hazırlama](backup-azure-microsoft-azure-backup.md)
- [Bir VMware sunucusunu yedeklemek için Backup sunucusu kullanma](backup-azure-backup-server-vmware.md)
- [SQL sunucusunu yedeklemek için Backup sunucusu kullanma](backup-azure-sql-mabs.md)
- [Modern yedekleme depolama alanı, yedekleme sunucusuna Ekle](backup-mabs-add-storage.md)
