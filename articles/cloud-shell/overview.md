---
title: Azure Cloud Shell Genel Bakış | Microsoft Docs
description: Azure Cloud Shell Genel Bakış.
services: ''
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/20/2020
ms.author: damaerte
ms.openlocfilehash: f824bddf833a1e2c01a3b779abc2c5252d8e0547
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "89468666"
---
# <a name="overview-of-azure-cloud-shell"></a>Azure Cloud Shell'e Genel Bakış

Azure Cloud Shell, Azure kaynaklarını yönetmek için kullanabileceğiniz etkileşimli, kimliği doğrulanmış ve tarayıcı ile erişilebilen bir kabuktur. Bu hizmet, Bash veya PowerShell ile çalışma şeklinize en uygun kabuk deneyimini seçme esnekliği getirir.

Cloud Shell üç yolla erişebilirsiniz:

- **Doğrudan bağlantı**: ' a bir tarayıcı açın [https://shell.azure.com](https://shell.azure.com) .

- **Azure Portal**: [Azure Portal](https://portal.azure.com)Cloud Shell simgesini seçin:

    ![Azure portal Cloud Shell başlatılacak simge](media/overview/portal-launch-icon.png)

- **Kod parçacıkları**: [docs.Microsoft.com]() ve [Microsoft Learn](/learn/)üzerinde, Azure CLI ve Azure PowerShell kod parçacıkları ile görüntülenen **TRY It** düğmesini seçin:

    ```azurecli-interactive
    az account show
    ```

    ```azurepowershell-interactive
    Get-AzSubscription
    ```

    **Deneyin** düğmesine Bash (Azure CLI kod parçacıkları için) veya PowerShell (Azure PowerShell parçacıkları için) ile birlikte Cloud Shell doğrudan açılır.

    Komutu çalıştırmak için kod parçacığında **kopyayı** kullanın,  +  + komutu yapıştırmak için CTRL SHIFT **v** (Windows/Linux) veya **cmd** + **SHIFT** + **v** (MacOS) kullanın ve ardından **ENTER** tuşuna basın.

## <a name="features"></a>Özellikler

### <a name="browser-based-shell-experience"></a>Tarayıcı tabanlı kabuk deneyimi

Cloud Shell, Azure yönetim görevleriyle birlikte oluşturulan tarayıcı tabanlı bir komut satırı deneyimine erişim sağlar. Cloud Shell, yerel bir makineden yalnızca bulutun sağlayabilebir şekilde Untethered işe faydalanır.

### <a name="choice-of-preferred-shell-experience"></a>Tercih edilen kabuk deneyimi seçimi

Kullanıcılar bash veya PowerShell arasında seçim yapabilir.

1. **Cloud Shell** seçin.

    ![Cloud Shell simgesi](media/overview/overview-cloudshell-icon.png)

2. **Bash** veya **PowerShell**' i seçin.

    ![Bash veya PowerShell seçin](media/overview/overview-choices.png)

    İlk başlatıldıktan sonra, bash ve PowerShell arasında geçiş yapmak için kabuk türü açılan denetimini kullanabilirsiniz:

    ![Bash veya PowerShell seçmek için açılan denetim](media/overview/select-shell-drop-down.png)

### <a name="authenticated-and-configured-azure-workstation"></a>Kimliği doğrulanmış ve yapılandırılmış Azure iş istasyonu

Cloud Shell, Microsoft tarafından yönetiliyor ve bu nedenle popüler komut satırı araçları ve dil desteğiyle birlikte geldi. Cloud Shell Ayrıca, Azure CLı veya Azure PowerShell cmdlet 'leri aracılığıyla kaynaklarınıza anında erişim için otomatik olarak kimlik doğrulaması gerçekleştirir.

[Cloud Shell yüklü araçların tam listesini görüntüleyin.](features.md#tools)

### <a name="integrated-cloud-shell-editor"></a>Tümleşik Cloud Shell Düzenleyicisi

Cloud Shell, açık kaynak Monako düzenleyicisine bağlı olarak tümleşik bir grafik metin düzenleyicisi sunar. `code .`Azure CLI veya Azure PowerShell ile sorunsuz dağıtım için çalıştırarak yapılandırma dosyalarını oluşturmanız ve düzenlemeniz yeterlidir.

[Cloud Shell Düzenleyicisi hakkında daha fazla bilgi edinin](using-cloud-shell-editor.md).

### <a name="multiple-access-points"></a>Birden çok erişim noktası

Cloud Shell, tarafından kullanılabilen esnek bir araçtır:

* [portal.azure.com](https://portal.azure.com)
* [shell.azure.com](https://shell.azure.com)
* [Azure CLI belgeleri](/cli/azure)
* [Azure PowerShell belgeleri](/powershell/azure/)
* [Azure mobil uygulaması](https://azure.microsoft.com/features/azure-portal/mobile-app/)
* [Visual Studio Code Azure Hesap uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)

### <a name="connect-your-microsoft-azure-files-storage"></a>Microsoft Azure dosyaları depolama alanınızı bağlama

Cloud Shell makineler geçicidir, ancak dosyalarınız iki şekilde kalıcı hale getirilir: bir disk görüntüsü ile ve adlı bağlı dosya paylaşımından `clouddrive` . Cloud Shell ilk kez çalıştırıldığında sizin adınıza bir kaynak grubu, depolama hesabı ve Azure Dosyaları paylaşımı oluşturur. Bu bir kerelik bir adımdır ve tüm oturumlar için otomatik olarak eklenir. Tek bir dosya paylaşma eşleştirilebilir ve Cloud Shell hem Bash hem de PowerShell tarafından kullanılır.

[Yeni veya mevcut bir depolama hesabını](persisting-shell-storage.md) nasıl bağlayacağınızı veya [Cloud Shell ' de kullanılan Kalıcılık mekanizmaları](persisting-shell-storage.md#how-cloud-shell-storage-works)hakkında bilgi edinmek için daha fazla bilgi edinin.

> [!NOTE]
> Azure Storage güvenlik duvarı, Cloud Shell depolama hesapları için desteklenmez.

## <a name="concepts"></a>Kavramlar

* Cloud Shell, oturum başına ve kullanıcı bazında belirtilen geçici bir konakta çalıştırılır
* Etkileşimli etkinlik olmadan 20 dakika sonra zaman aşımı Cloud Shell
* Cloud Shell, bir Azure dosya paylaşımının bağlanmasını gerektirir
* Cloud Shell hem Bash hem de PowerShell için aynı Azure dosya paylaşımının kullanır
* Cloud Shell Kullanıcı hesabı başına bir makine atanır
* Cloud Shell dosya paylaşımınızda bulunan 5 GB bir görüntü kullanarak $HOME devam ediyor
* İzinler Bash 'te normal bir Linux kullanıcısı olarak ayarlanır

Cloud Shell ve PowerShell 'deki [Bash](features.md) özellikleri hakkında daha fazla bilgi edinin [Cloud Shell](./features.md).

## <a name="pricing"></a>Fiyatlandırma

Cloud Shell barındıran makine, bağlı bir Azure dosyaları paylaşımının önkoşul olarak ücretsizdir. Normal depolama ücretleri uygulanır.

## <a name="next-steps"></a>Sonraki adımlar

[Bash Cloud Shell hızlı başlangıç](quickstart.md) <br>
[PowerShell Cloud Shell hızlı başlangıç](quickstart-powershell.md)