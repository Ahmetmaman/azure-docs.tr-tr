---
title: Azure Güvenlik Duvarı verilerini Azure Sentinel 'e bağlama
description: Azure Güvenlik Duvarı verilerini Azure Sentinel 'e bağlamayı öğrenin.
author: yelevin
manager: rkarlin
ms.assetid: bfa2eca4-abdc-49ce-b11a-0ee229770cdd
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: how-to
ms.date: 08/30/2020
ms.author: yelevin
ms.openlocfilehash: 4dffaac329f1581d9082fd8ab2c314f52b1730ab
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94656029"
---
# <a name="connect-data-from-azure-firewall"></a>Azure güvenlik duvarından veri bağlama

> [!IMPORTANT]
> Azure Sentinel 'deki Azure Güvenlik Duvarı veri Bağlayıcısı Şu anda genel önizlemededir.
> Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir. Yerleşik yüksek kullanılabilirliğe sahip ve Kısıtlanmamış bulut ölçeklenebilirliğine sahip, hizmet olarak tam durum bilgisi olan bir güvenlik duvarıdır. 

Azure Güvenlik Duvarı günlüklerini Azure Sentinel 'e bağlanarak, çalışma kitaplarında günlük verilerini görüntülemenize, özel uyarılar oluşturmak için bu uygulamayı kullanmanıza ve araştırmanızı iyileştirebilecek şekilde katabilirsiniz.

[Azure Güvenlik Duvarı günlüklerini izleme](../firewall/firewall-diagnostics.md)hakkında daha fazla bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

- Azure Sentinel çalışma alanında okuma ve yazma izinlerine sahip olmanız gerekir.

## <a name="connect-to-azure-firewall"></a>Azure Güvenlik Duvarı 'na bağlanma
    
1. Azure Sentinel gezinti menüsünde **veri bağlayıcıları**' nı seçin.

1. Veri bağlayıcıları galerisinden **Azure Güvenlik Duvarı** ' nı seçin ve ardından Önizleme bölmesinde **bağlayıcı sayfasını aç**  ' ı seçin.

1. Günlüklerini bağlamak istediğiniz tüm güvenlik duvarları üzerinde **tanılama günlüklerini** etkinleştirin:

    1. **>Azure Güvenlik Duvarı kaynağını Aç** bağlantısını seçin.

    1. **Güvenlik duvarları** gezinti menüsünde **Tanılama ayarları**' nı seçin.

    1. Listenin en altında **+ Tanılama ayarı Ekle** ' yi seçin.

    1. **Tanılama ayarları** ekranında, **Tanılama ayarları adı** alanına bir ad girin.
    
    1. **Log Analytics gönder** onay kutusunu işaretleyin. Burada iki yeni alan görüntülenir. İlgili **aboneliği** ve **Log Analytics çalışma alanını** (Azure Sentinel 'in bulunduğu yer) seçin.

    1. Günlüklerini almak istediğiniz olan kural türlerinin onay kutularını işaretleyin. **AzureFirewallApplicationRule** ve **AzureFirewallNetworkRule** önerilir.

    1. Ekranın üst kısmındaki **Kaydet** ' i seçin.

1. Azure Güvenlik Duvarı uyarıları için Log Analytics ilgili şemayı kullanmak için, **AzureDiagnostics** aratın.

> [!NOTE]
>
> Bu veri bağlayıcısıyla bağlantı durumu göstergeleri (veri bağlayıcıları galerisindeki bir renk şeridi ve veri türü adlarının yanında bulunan bağlantı simgeleri), yalnızca geçen iki haftada bir noktada verilerin alınmış olması halinde *bağlı* (yeşil) olarak görünür. Veri alımı olmadan iki hafta geçtikten sonra bağlayıcının bağlantısı kesilmekte olarak gösterilir. Daha fazla veri geldiğinde *bağlantılı* durum döndürülür.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede Azure Güvenlik Duvarı günlüklerini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.