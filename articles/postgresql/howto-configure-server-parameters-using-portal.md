---
title: PostgreSQL için Azure portalı üzerinden gönderilmiş olan Azure veritabanı'nda sunucu parametrelerini yapılandırma
description: Bu makalede, PostgreSQL için Azure portalı üzerinden gönderilmiş olan Azure veritabanı'nda sunucu parametrelerini yapılandırma açıklar.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 0d0626c48ecebdead604aab93ab0602c698d0d77
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53540546"
---
# <a name="configure-server-parameters-in-azure-portal"></a>Azure portalında sunucu parametrelerini yapılandırma
Liste, Göster ve Azure Portalı aracılığıyla PostgreSQL sunucusu için Azure veritabanı için yapılandırma parametreleri güncelleştirin.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır Kılavuzu, adım gerekir:
- [PostgreSQL sunucusu için Azure veritabanı](quickstart-create-server-database-portal.md)

## <a name="viewing-and-editing-parameters"></a>Görüntüleme ve düzenleme parametreleri
1. [Azure portalı](https://portal.azure.com) açın.

2. PostgreSQL için Azure Veritabanı sunucunuzu seçin.

3. Altında **ayarları** bölümünden **sunucu parametreleri**. Sayfada parametreleri, değerleri ve açıklamaları listesi gösterilir.
![Parametreler için genel bakış sayfası](./media/howto-configure-server-parameters-in-portal/3-overview-of-parameters.png)

4. Seçin **açılan** düğmeyi client_min_messages gibi numaralandırılmış tür parametreleri için olası değerler.
![Aşağı açılan listeleme](./media/howto-configure-server-parameters-in-portal/4-enum-drop-down.png)

5. Seçin veya üzerine **miyim** cpu_index_tuple_cost gibi sayısal parametre için olası değerler aralığı görmek için düğmeyi (bilgileri).
![bilgi düğmesi](./media/howto-configure-server-parameters-in-portal/4-information-button.png)

6. Gerekirse, kullanın **arama kutusuna** için belirli bir parametrenin daraltmak için. Arama, ad ve açıklama parametrelerinin olur.
![Arama sonuçları](./media/howto-configure-server-parameters-in-portal/5-search.png)

7. Ayarlamak istediğiniz parametre değerlerini değiştirin. Bir oturumda yaptığınız tüm değişiklikler mor renkle vurgulanır. Değerler değiştirildiğinde sonra seçebileceğiniz **Kaydet**. Veya **at** yaptığınız değişiklikleri.
![Kaydet veya değişiklikleri at](./media/howto-configure-server-parameters-in-portal/6-save-and-discard-buttons.png)

8. Parametreler için yeni değerler kaydettiyseniz, her zaman varsayılan değerleri dön her şeyi seçerek geri dönebilirsiniz **tümünü Varsayılana Sıfırla**.
![Tümünü Varsayılana sıfırla](./media/howto-configure-server-parameters-in-portal/7-reset-to-default-button.png)

## <a name="next-steps"></a>Sonraki adımlar
Hakkında bilgi edinin:
- [PostgreSQL için Azure veritabanı'nda sunucu parametrelerini genel bakış](concepts-servers.md)
- [Azure CLI kullanarak parametrelerini yapılandırma](howto-configure-server-parameters-using-cli.md)
