---
title: Azure Otomasyonu’nu Event Grid ile tümleştirme | Microsoft Docs
description: Yeni bir VM oluşturulduğunda otomatik olarak etiket oluşturmayı ve Microsoft Teams'e bildirim göndermeyi öğrenin.
keywords: otomasyon, runbook, ekipler, event grid, sanal makine, VM
services: automation
documentationcenter: ''
author: eamonoreilly
manager: ''
editor: ''
ms.service: automation
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/06/2017
ms.author: eamono
ms.openlocfilehash: 6270f8bad893798f46d8db91e7b1140b6a125350
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39049873"
---
# <a name="integrate-azure-automation-with-event-grid-and-microsoft-teams"></a>Azure Otomasyonu’nu Event Grid ve Microsoft Teams ile tümleştirme

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Event Grid örnek runbook'unu içeri aktarma.
> * İsteğe bağlı bir Microsoft Teams web kancası oluşturma.
> * Runbook için web kancası oluşturma.
> * Event Grid aboneliği oluşturun.
> * Runbook'u tetikleyen bir VM oluşturma.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için, bir [Azure Otomasyonu hesabının](../automation/automation-offering-get-started.md) Azure Event Grid aboneliğinden tetiklenen runbook'u barındırması gerekir.

* Otomasyon Hesabınıza `AzureRM.Tags` modülü yüklenmelidir. Modülleri Azure Otomasyonu'na aktarmayı öğrenmek için bkz. [Modülleri Azure Otomasyonu'na aktarma](../automation/automation-update-azure-modules.md).

## <a name="import-an-event-grid-sample-runbook"></a>Event Grid örnek runbook'unu içeri aktarma

1. Otomasyon hesabınızı seçin ve sonra da **Runbook'lar** sayfasını seçin.

   ![Runbook'ları seçme](./media/ensure-tags-exists-on-new-virtual-machines/select-runbooks.png)

2. **Galeriye gözat** düğmesini seçin.

3. **Event Grid** için arama yapın ve **Azure Otomasyonu'nu Event Grid ile tümleştirme** öğesini seçin.

    ![Runbook galerisini içeri aktarma](media/ensure-tags-exists-on-new-virtual-machines/gallery-event-grid.png)

4. **İçeri Aktar**'ı seçin ve bunu **Watch-VMWrite** olarak adlandırın.

5. İçeri aktarıldıktan sonra, runbook kaynağını görüntülemek için **Düzenle**'yi seçin. **Yayımla** düğmesini seçin.

> [!NOTE]
> Betiğin 74. satırı `Update-AzureRmVM -ResourceGroupName $VMResourceGroup -VM $VM -Tag $Tag | Write-Verbose` olarak değiştirilmelidir. `-Tags` parametresi artık `-Tag` olmuştur.

## <a name="create-an-optional-microsoft-teams-webhook"></a>İsteğe bağlı bir Microsoft Teams web kancası oluşturma

1. Microsoft Teams'de, kanal adının yanındaki **Diğer Seçenekler**'i ve sonra da **Bağlayıcılar**'ı seçin.

    ![Microsoft Teams bağlantıları](media/ensure-tags-exists-on-new-virtual-machines/teams-webhook.png)

2. Bağlayıcılar listesini **Gelen Web Kancası**'na kadar kaydırın ve **Ekle**'yi seçin.

3. Ad olarak **AzureAutomationIntegration** girin ve **Oluştur**'u seçin.

4. Web kancasını panoya kopyalayın ve kaydedin. Web kancası URL'si, Microsoft Teams'e bilgi göndermek için kullanılır.

5. **Bitti**'yi seçerek web kancasını kaydedin.

## <a name="create-a-webhook-for-the-runbook"></a>Runbook için web kancası oluşturma

1. Watch-VMWrite runbook'unu açın.

2. **Web kancaları**'nı ve ardından **Web Kancası Ekle** düğmesini seçin.

3. Ad olarak **WatchVMEventGrid** girin. URL'yi panoya kopyalayın ve kaydedin.

    ![Web kancası adını yapılandırma](media/ensure-tags-exists-on-new-virtual-machines/copy-url.png)

4. **Parametreleri ve çalıştırma ayarlarını yapılandır**'ı seçin ve **CHANNELURL** olarak Microsoft Teams web kancası URL'sini girin. **WEBHOOKDATA** alanını boş bırakın.

    ![Web kancası parametrelerini yapılandırma](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-parameters.png)

5. Otomasyon runbook web kancasını oluşturmak için **Tamam**'ı seçin.


## <a name="create-an-event-grid-subscription"></a>Event Grid aboneliği oluşturma
1. **Otomasyon Hesabı** genel bakış sayfasında **Event Grid**'i seçin.

    ![Event Grid'i seçme](media/ensure-tags-exists-on-new-virtual-machines/select-event-grid.png)

2. **+ Olay Aboneliği** düğmesini seçin.

3. Aboneliği aşağıdaki bilgilerle yapılandırın:

    *   Ad olarak **AzureAutomation** girin.
    *   **Konu Başlığı Türü** altında **Azure Abonelikleri**'ni seçin.
    *   **Tüm olay türlerine abone ol** onay kutusunu temizleyin.
    *   **Olay Türleri**'nde **Kaynak Yazma Başarısı**'nı seçin.
    *   **Abone Uç Noktası** alanında, Watch-VMWrite runbook'u için web kancası URL'sini girin.
    *   **Önek Filtresi** alanında, oluşturulan yeni VM'leri aramak istediğiniz aboneliği ve kaynak grubunu girin. Şu şekilde görünmelidir: `/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines`

4. Event Grid aboneliğini kaydetmek için **Oluştur**'u seçin.

## <a name="create-a-vm-that-triggers-the-runbook"></a>Runbook'u tetikleyen bir VM oluşturma
1. Event Grid aboneliği önek filtresinde belirttiğiniz kaynak grubunda yeni bir VM oluşturun.

2. Watch-VMWrite runbook çağrılmalı ve VM'ye yeni etiket eklenmelidir.

    ![VM etiketi](media/ensure-tags-exists-on-new-virtual-machines/vm-tag.png)

3. Microsoft Teams kanalına yeni bir ileti gönderilir.

    ![Microsoft Teams bildirimi](media/ensure-tags-exists-on-new-virtual-machines/teams-vm-message.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Event Grid ile Otomasyon arasında tümleştirme ayarladınız. Şunları öğrendiniz:

> [!div class="checklist"]
> * Event Grid örnek runbook'unu içeri aktarma.
> * İsteğe bağlı bir Microsoft Teams web kancası oluşturma.
> * Runbook için web kancası oluşturma.
> * Event Grid aboneliği oluşturma.
> * Runbook'u tetikleyen bir VM oluşturma.

> [!div class="nextstepaction"]
> [Event Grid ile özel olaylar oluşturma ve yönlendirme](../event-grid/custom-event-quickstart.md)
