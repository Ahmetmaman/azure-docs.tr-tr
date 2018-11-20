---
title: "Hızlı Başlangıç: Power BI için Azure Veri Gezgini Bağlayıcısı'nı kullanarak verileri Görselleştir"
description: "Bu hızlı başlangıçta, Power bı'da verileri görselleştirmek için üç seçenekten birini kullanmayı öğrenin: Azure Veri Gezgini için Power BI Bağlayıcısı."
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 11/14/2018
ms.openlocfilehash: 3cb8f52677991997a0176a9f8d408e2fd6d2d8d9
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2018
ms.locfileid: "51974280"
---
# <a name="quickstart-visualize-data-using-the-azure-data-explorer-connector-for-power-bi"></a>Hızlı Başlangıç: Power BI için Azure Veri Gezgini Bağlayıcısı'nı kullanarak verileri Görselleştir

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve yüksek oranda ölçeklenebilir veri keşfetme hizmetidir. Power BI, verilerinizi görselleştirmenizi ve sonuçları kuruluşunuzda paylaşmanızı sağlayan bir iş analizi çözümüdür.

Azure Veri Gezgini, Power bı'daki verilere bağlanmak için üç seçenek sunar: yerleşik Bağlayıcısı, Azure veri Gezgini'nde bir sorguyu içeri aktarmak veya bir SQL sorgusu kullanın. Bu hızlı başlangıçta, veri almak ve bir Power BI raporuna görselleştirme yerleşik bağlayıcı kullanmayı gösterir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için şunlara ihtiyacınız vardır:

* Bağlanabilir, böylece Azure Active directory üyesi olan bir kuruluş e-posta hesabı [Azure Veri Gezgini Yardım kümesi](https://dataexplorer.azure.com/clusters/help/databases/samples).

* [Power BI Desktop](https://powerbi.microsoft.com/get-started/) (seçin **DOWNLOAD FREE**)

## <a name="get-data-from-azure-data-explorer"></a>Azure veri Gezgini'nde verileri alma

İlk olarak, Azure Veri Gezgini Yardım kümeye bağlanın ve ardından bir veri alt kümelerinin getirin *StormEvents* tablo. [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

1. Power BI Desktop'ta üzerinde **giriş** sekmesinde **Veri Al** ardından **daha fazla**.

    ![Verileri alma](media/power-bi-connector/get-data-more.png)

1. Arama *Azure Veri Gezgini*seçin **Azure Veri Gezgini (Beta)** ardından **Connect**.

    ![Arama ve veri alma](media/power-bi-connector/search-get-data.png)

1. **Bağlayıcıyı önizle** ekranında **Devam**'ı seçin.

1. Sonraki ekranda formunu aşağıdaki bilgilerle doldurun.

    ![Küme, veritabanı, tablo seçenekleri](media/power-bi-connector/cluster-database-table.png)

    **Ayar** | **Değer** | **Alan açıklaması**
    |---|---|---|
    | Küme | *https://help.kusto.windows.net* | Yardım kümesi için URL. Diğer kümeler için URL biçimindedir *https://\<ClusterName\>.\< Bölge\>. kusto.windows.net*. |
    | Database | Boş bırakın | Bağlanmakta olduğunuz kümesi üzerinde barındırılan bir veritabanı. Biz bu daha sonraki bir adımda seçersiniz. |
    | Tablo adı | Boş bırakın | Bir veritabanı ya da bir sorgu tablo ister ' StormEvents | 1000' yararlanın. Biz bu daha sonraki bir adımda seçersiniz. |
    | Gelişmiş seçenekler | Boş bırakın | Sonuç gibi sorgularınız için seçenekleri boyutunu ayarlayın. |
    | Veri bağlantısı modu | *DirectQuery* | Power BI veri aldığında veya doğrudan veri kaynağına bağlanan belirler. Bu bağlayıcıyı kullanarak, iki seçenekten birini kullanabilirsiniz. |
    | | | |

1. Yardım kümeyle bir bağlantı zaten yoksa, oturum açın. Bir kuruluş hesabıyla oturum açın ve ardından **Connect**.

    ![Oturum aç](media/power-bi-connector/sign-in.png)

1. Üzerinde **Gezgin** ekranında, **örnekleri** veritabanı **StormEvents** ardından **Düzenle**.

    ![Tablo seçin](media/power-bi-connector/select-table.png)

    Tablo, Power Query Düzenleyicisi'nde açılır ve burada verileri içeri aktarmadan önce satırları ve sütunları düzenleyebilirsiniz.

1. Güç sorgu Düzenleyicisi'nde yanındaki oku seçerek **DamageCrops** ardından sütun **zalan Sıralama**.

    ![Azalan düzende sıralama DamageCrops](media/power-bi-connector/sort-descending.png)

1. Üzerinde **giriş** sekmesinde **satırları tutmak** ardından **üst satırları tut**. Değeri girin *1000* sıralanmış bir tablo üst 1000 satırlarda getirilecek.

    ![Üst satırları tut](media/power-bi-connector/keep-top-rows.png)

1. Üzerinde **giriş** sekmesinde **Kapat & Uygula**.

    ![Kapat ve uygula](media/power-bi-connector/close-apply.png)

## <a name="visualize-data-in-a-report"></a>Bir rapordaki verileri görselleştirin

[!INCLUDE [data-explorer-power-bi-visualize-basic](../../includes/data-explorer-power-bi-visualize-basic.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu Hızlı Başlangıç için oluşturduğunuz rapor artık ihtiyacınız kalmadığında Power BI Desktop (.pbix) dosyasını silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Power BI'da içeri aktarılan bir sorgu kullanarak verileri Görselleştir](power-bi-imported-query.md)