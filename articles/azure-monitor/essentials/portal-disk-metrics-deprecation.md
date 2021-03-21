---
title: Azure portal disk ölçümleri kullanımdan kaldırıldı | Microsoft Docs
description: Hangi disk ölçümlerinin kullanım dışı olduğunu ve ölçüm uyarılarınızı yeni ölçümleri kullanacak şekilde güncelleştirmeyi öğrenin.
services: azure-monitor
ms.topic: conceptual
author: albecker1
ms.author: albecker
ms.date: 03/12/2020
ms.openlocfilehash: 607ccacbfff86decaecaad0a869c8c0fe7500680
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102033410"
---
# <a name="disk-metrics-deprecation-in-the-azure-portal"></a>Azure portal disk ölçümleri kullanımdan kaldırıldı

Kullanımdan kaldırılan diskle ilgili ölçümler yakında Azure portal kaldırılacak. Kullanım dışı bırakılan her ölçümün yeni bir sürümü kullanılabilir. Bu makalede, hangi ölçümlerin yeni olduğu ve ölçüm uyarılarınızın bunları kullanmak için nasıl güncelleştirilmesi gösterilmektedir.

## <a name="list-of-new-metrics"></a>Yeni ölçümlerin listesi

Bu tablo, kullanım dışı bırakılan her ölçüyü buna karşılık gelen yeni ölçüyle eşler. 

|Kullanım dışı ölçüm|Yeni (değiştirme) ölçüm|
|----|----|
|Veri diski QD (kullanım dışı)|Veri diski sıra derinliği (Önizleme)|
|Veri diski okuma bayt/sn (kullanım dışı)|Veri diski okuma bayt/sn (Önizleme)|
|Veri diski okuma Işlemi/sn (kullanım dışı)|Veri diski okuma Işlemi/sn (Önizleme)|
|Veri diski yazma bayt/sn (kullanım dışı)|Veri diski yazma bayt/sn (Önizleme)|
|Veri diski yazma Işlemi/sn (kullanım dışı)|Veri diski yazma Işlemi/sn (Önizleme)|
|OS QD (kullanım dışı)|İşletim sistemi sıra derinliği (Önizleme)|
|İşletim sistemi okuma bayt/sn (kullanım dışı)|İşletim sistemi okuma bayt/sn (Önizleme)|
|İşletim sistemi okuma Işlemi/sn (kullanım dışı)|İşletim sistemi okuma Işlemi/sn (Önizleme)|
|İşletim sistemi yazma bayt/sn (kullanım dışı)|İşletim sistemi yazma bayt/sn (Önizleme)|
|İşletim sistemi yazma Işlemi/sn (kullanım dışı)|İşletim sistemi yazma Işlemi/sn (Önizleme)|

<a id="update-metrics" />

## <a name="migrate-metrics-in-your-metric-alerts"></a>Ölçüm uyarılarınıza ölçümleri geçirin

Ölçüm uyarılarınızı yeni ölçümleri kullanacak şekilde güncelleştirin.

1. Azure portal, **Uyarılar**' ı arayın. Ardından, **Hizmetler** bölümünde **Uyarılar**' ı seçin.

   > [!div class="mx-imgBorder"]
   > ![Uyarı hizmeti](./media/portal-disk-metrics-deprecation/alert-service-azure-portal.png)

2. **Uyarılar** sayfasında, **Uyarı kurallarını yönet** düğmesini seçin. 

   > [!div class="mx-imgBorder"]
   > ![Uyarı kurallarını yönetin](./media/portal-disk-metrics-deprecation/manage-alert-rules-button.png)

3. **Kaynak grubu** açılan listesinde, **sanal makineler** onay kutusunu seçin ve **sinyal türü** aşağı açılan listesinde **ölçümler** onay kutusunu seçin. 

   > [!div class="mx-imgBorder"]
   > ![Uyarıları filtrele](./media/portal-disk-metrics-deprecation/filter-alerts.png)

4. Ölçümler listesinde disklerle ilgili koşulları tanımla. Kuralın adına tıklayın. 

   Ad, tablonun **ad** sütununda köprü olarak görünür.

   > [!div class="mx-imgBorder"]
   > ![Disk koşullarını bul](./media/portal-disk-metrics-deprecation/find-disk-conditions.png)

5. **Kural yönetimi** sayfasının **koşullar** bölümünde, uyarının koşuluna tıklayın. 

   Koşul bir köprü olarak görünür.  

   > [!div class="mx-imgBorder"]
   > ![Koşulları ayarla](./media/portal-disk-metrics-deprecation/adjust-condition.png)

   **Sinyal mantığını Yapılandır** sayfası görünür ve bu sayfanın **Uyarı mantığı** bölümünde koşulun ayarları görüntülenir.

6. Kullanım dışı olan ölçüyü kaldırdığınızda bu ayarların kaydını temizler.

   > [!div class="mx-imgBorder"]
   > ![Koşul kuralları](./media/portal-disk-metrics-deprecation/condition-rules.png)

   > [!TIP] 
   > Bu ayarları bir ekran görüntüsünde veya metin dosyasında yakalamayı göz önünde bulundurun. 

7. **Sinyal seçimine dön** bağlantısına tıklayın.

   > [!div class="mx-imgBorder"]
   > ![Sinyal seçimine geri dön](./media/portal-disk-metrics-deprecation/back-to-signal-selection.png)

8. **Sinyal mantığını Yapılandır** sayfasında, uygun değiştirme ölçümünü (yeni ölçüm) seçin. Yeni ölçümün adını kimlik olarak vermek için bu makalenin önceki kısımlarında yer alan [tabloyu](#update-metrics) kullanın.

   > [!TIP] 
   > Ölçüm adlarının listesini daraltmak için arama çubuğuna yazmaya başlayın. 

   > [!div class="mx-imgBorder"]
   > ![Yeni ölçüm seçin](./media/portal-disk-metrics-deprecation/choose-new-metric.png)

9. **Bitti** düğmesini seçin. 

   > [!div class="mx-imgBorder"]
   > ![Yeni ölçüm ayarla](./media/portal-disk-metrics-deprecation/set-new-metric.png)

10. **Kaydet** düğmesini seçerek yaptığınız değişiklikleri işleyin. 

    > [!div class="mx-imgBorder"]
    > ![Yeni ölçümü Kaydet](./media/portal-disk-metrics-deprecation/save-new-metric.png)






