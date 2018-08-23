---
title: Azure Cloud Shell özellikleri | Microsoft Docs
description: Azure Cloud Shell'deki Bash hizmetinde özelliklerine genel bakış
services: Azure
documentationcenter: ''
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/13/2018
ms.author: juluk
ms.openlocfilehash: 1321645d97e7f6ff2faed1e61ddb608afcb7b413
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2018
ms.locfileid: "42059648"
---
# <a name="features--tools-for-azure-cloud-shell"></a>Özellikler ve Azure Cloud Shell için Araçlar

[!INCLUDE [features-introblock](../../includes/cloud-shell-features-introblock.md)]

Azure Cloud Shell çalıştığı `Ubuntu 16.04 LTS`.

## <a name="features"></a>Özellikler

### <a name="secure-automatic-authentication"></a>Güvenli otomatik kimlik doğrulaması

Cloud Shell'i güvenli bir şekilde ve otomatik olarak Azure PowerShell ve Azure CLI 2.0 için hesap erişimi doğrular.

### <a name="home-persistence-across-sessions"></a>Oturumlar arasında $Home kalıcılığı

Dosyaları oturumlarda kalıcı hale getirilmesi için Cloud Shell ilk kez başlattığınızda bir Azure dosya paylaşımı ekleme aracılığıyla size.
Tamamlandığında, Cloud Shell depolama alanınızı otomatik olarak eklenecek (takılamadı `$Home\clouddrive`) gelecekteki tüm oturumları.
Ayrıca, `$Home` dizin olarak Azure dosya paylaşımınızdaki bir .img kalıcıdır.
Dışında dosyaları `$Home` ve makine durumu oturumlar arasında sürdürülmez. SSH anahtarları gibi gizli dizilerin depolanmasında en iyi uygulamaları kullanın. Gibi hizmetleri [Azure anahtar kasası kurulumu için öğreticiler sahip](https://docs.microsoft.com/azure/key-vault/key-vault-manage-with-cli2#prerequisites).

[Cloud shell'de kalıcı dosyaları hakkında daha fazla bilgi edinin.](persisting-shell-storage.md)

### <a name="azure-drive-azure"></a>Azure sürücüsü (Azure:)

(Önizleme) Cloud shell'de PowerShell Azure sürücüsü başlar (`Azure:`).
Azure sürücüsü kolay keşfe ve işlem, ağ, depolama vb. için dosya sistemi gezintisi benzer gibi Azure kaynaklarının gezinme sağlar.
Tanıdık kullanmaya devam edebilirsiniz [Azure PowerShell cmdlet'lerini](https://docs.microsoft.com/powershell/azure) bulunduğunuz sürücü bağımsız olarak bu kaynakları.
Azure kaynakları, doğrudan Azure portalında veya Azure PowerShell cmdlet'leri aracılığıyla yapılan ya da yapılan tüm değişiklikler Azure sürücüde yansıtılır.  Çalıştırabileceğiniz `dir -Force` kaynaklarınızı yenilenemedi.

![](media/features-powershell/azure-drive.png)

### <a name="deep-integration-with-open-source-tooling"></a>Açık kaynak araçları ile kapsamlı tümleştirme

Cloud Shell'de önceden yapılandırılmış kimlik doğrulama için Terraform, Ansible ve Chef InSpec gibi açık kaynaklı araçları içerir. Örnek izlenecek deneyin.

## <a name="tools"></a>Araçlar

|Kategori   |Ad   |
|---|---|
|Linux araçları            |Bash<br> zsh<br> Göster<br> tmux<br> dıg<br>               |
|Azure Araçları            |[Azure CLI 2.0](https://github.com/Azure/azure-cli) ve [1.0](https://github.com/Azure/azure-xplat-cli)<br> [AzCopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy)<br> [Service Fabric CLI](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli) |
|Metin düzenleyiciler           |vim<br> nano<br> emacs       |
|Kaynak denetimi         |git                    |
|Derleme araçları            |Olun<br> Maven<br> npm<br> pip         |
|Kapsayıcılar             |[Docker CLI](https://github.com/docker/cli)/[Docker makinesi](https://github.com/docker/machine)<br> [Kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/)<br> [Helm](https://github.com/kubernetes/helm)<br> [DC/OS CLI'Sİ](https://github.com/dcos/dcos-cli)         |
|Veritabanları              |MySQL istemci<br> PostgreSql istemcisi<br> [SQLCMD yardımcı programı](https://docs.microsoft.com/sql/tools/sqlcmd-utility)<br> [mssql-scripter](https://github.com/Microsoft/sql-xplat-cli) |
|Diğer                  |Ipython istemci<br> [Cloud Foundry CLI](https://github.com/cloudfoundry/cli)<br> [Terraform](https://www.terraform.io/docs/providers/azurerm/)<br> [Ansible](https://www.ansible.com/microsoft-azure)<br> [Chef InSpec](https://www.chef.io/inspec/)| 

## <a name="language-support"></a>Dil desteği

|Dil   |Sürüm   |
|---|---|
|.NET Core  |2.0.0       |
|Başlayın         |1.9        |
|Java       |1.8        |
|Node.js    |8.9.4      |
|PowerShell |[6.1.0-Preview.4](https://github.com/PowerShell/powershell/releases)       |
|Python     |2.7 ve 3.5 (varsayılan)|

## <a name="next-steps"></a>Sonraki adımlar
[Bash cloud Shell hızlı başlangıçta](quickstart.md) <br>
[PowerShell Cloud Shell (Önizleme) hızlı başlangıç](quickstart-powershell.md) <br>
[Azure CLI 2.0 hakkında bilgi edinin](https://docs.microsoft.com/cli/azure/) <br>
[Azure PowerShell hakkında bilgi edinin](https://docs.microsoft.com/powershell/azure/) <br>
