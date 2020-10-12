---
title: SOIL Moisture heatmap oluştur
description: Azure Farmınts 'de SOIL Moisture heatmap oluşturmayı açıklar
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: a2115e9c1601c86cce8857c10baf12b91cc2b997
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "75482575"
---
# <a name="generate-soil-moisture-heatmap"></a>SOIL Moisture heatmap oluştur

SOIL nemi, SOIL parçacıkları arasındaki boşlukların tutulduğu su.SOIL nemi heatmap, nemi verilerini herhangi bir derinlikte ve çiftlerinizin içinde yüksek çözünürlükte anlamanıza yardımcı olur. Doğru ve kullanılabilir bir SOIL nemi heatmap oluşturmak için aynı sağlayıcıdan alınan sensörlerden oluşan tek bir dağıtım gerekir. Farklı sağlayıcılar, SOIL nemi 'in ayarlama farklılıklarıyla birlikte ölçüldüğü fark eder. Heatmap, bu derinlikte dağıtılan sensörler kullanılarak belirli bir derinlik için oluşturulur.

Bu makalede, Azure Farmtts Hızlandırıcısı kullanılarak grubunuz için bir SOIL Moisture heatmap oluşturma işlemi açıklanmaktadır. Bu makalede şunları yapmayı öğreneceksiniz:

- [Gruplar oluşturma](#create-a-farm)
- [Gruplara algılayıcılar ata](#get-soil-moisture-sensor-data-from-partner)
- [SOIL Moisture heatmap oluştur](#generate-soil-moisture-heatmap)

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdakilerden emin olun:  

- Azure aboneliği.
- Azure Farmtts 'nin çalışan bir örneği.
- Grup için en az üç Soil nemi sensöri vardır.

## <a name="create-a-farm"></a>Grup oluşturma

Grup, bir SOIL nemi heatmap oluşturmak istediğiniz coğrafi bir alandır. [Gruplar API](https://aka.ms/FarmBeatsDatahubSwagger) 'Sini veya [Farmssink TS Hızlandırıcısı Kullanıcı arabirimini](manage-farms-in-azure-farmbeats.md#create-farms) kullanarak bir grup oluşturabilirsiniz.

## <a name="deploy-sensors"></a>Sensöri dağıt

Sunucu üzerinde SOIL nemi Sensörlerinizi fiziksel olarak dağıtmanız gerekir. Onaylı iş ortaklarımızdan SOIL nemi sensörlerinden ve [Teralytic](https://teralytic.com/)satın [Davis Instruments](https://www.davisinstruments.com/product/enviromonitor-gateway/) alabilirsiniz. Grubunuzda fiziksel kurulum yapmak için sensör sağlayıcınızla birlikte koordine etmeniz gerekir.

## <a name="get-soil-moisture-sensor-data-from-partner"></a>İş ortağından SOIL nemi algılayıcı verilerini al

Sensörler, verileri iş ortağı verileri panosuna başlattığında verileri Azure Farmfor olarak etkinleştirir. Bu, iş ortağı uygulamasından yapılabilir.

Örneğin, Davis sensörleri satın aldıysanız, hava durumu bağlantı hesabınızda oturum açıp veri akışını Azure Farm'e sağlamak için gereken kimlik bilgilerini sağlamanız gerekir. Gerekli kimlik bilgilerini almak için [sensör verilerini al](get-sensor-data-from-sensor-partner.md#get-sensor-data-from-sensor-partners)' daki yönergeleri izleyin.

Kimlik bilgilerinizi girdikten ve iş ortağı uygulamasına **Gönder** ' i seçtikten sonra, verilerin Azure Farmtts 'de akışını sağlayabilirsiniz.

### <a name="assign-soil-moisture-sensors-to-the-farm"></a>Gruba SOIL nemi sensöri atama

Algılayıcı hesabınızı Azure farmınts 'e bağladıktan sonra, SOIL nemi sensörlerinden ilgi grubuna atamanız gerekir.

1.  Giriş sayfasında, menüden **gruplar** ' ı seçin, **gruplar** listesi sayfası görüntülenir.
2.  **Myfarm**  >  **Cihaz Ekle**' yi seçin.
3.  **Cihaz Ekle** penceresi görüntülenir. Grubunuz için SOIL nemi sensörlerinden bağlantılı herhangi bir cihaz seçin.

    ![Proje grubu ları](./media/get-sensor-data-from-sensor-partner/add-devices-1.png)

4. **Cihaz Ekle**' yi seçin.     

## <a name="generate-soil-moisture-heatmap"></a>SOIL Moisture heatmap oluştur

Bu adım, grubunuz için SOIL Moisture heatmap oluşturacak bir iş veya uzun süre çalışan bir işlem oluşturmaktır.

1.  Giriş sayfasında, gruplar sayfasını görüntülemek için sol gezinti menüsünden **gruplar** ' a gidin.
2.  **Myfarm**' ı seçin.
3.  **Grup ayrıntıları** sayfasında **duyarlık eşlemesi oluştur**' u seçin.
4.  Aşağı açılan menüden **Soil Moisture**' i seçin.
5.  **SOIL Moisture** penceresinde **Bu hafta**' yı seçin.
6.  **SOIL Moisture** **algılayıcısı ölçüsünü**seçin alanında, eşleme için kullanmak istediğiniz ölçüyü girin.
    Algılayıcı ölçüsünü bulmak için **sensör**nemi algılayıcısı ' nı seçin. **Algılayıcı özellikleri**' nde **Ölçü adı** değerini kullanın.

    ![Proje grubu ları](./media/get-sensor-data-from-sensor-partner/soil-moisture-1.png)


7.  **Haritaları oluştur**' u seçin.
    İş ayrıntıları olan bir onay iletisi görüntülenir. Daha fazla bilgi için bkz. Işler içindeki Iş durumu.

    >[!NOTE]
    > İşin tamamlaması üç-dört saat sürer.

### <a name="download-the-soil-moisture-heatmap"></a>SOIL Moisture heatmap 'i indirin

Aşağıdaki adımları kullanın:

1. **İşler** sayfasında, son yordamda oluşturduğunuz Işin **iş durumunu** kontrol edin.
2. İş durumu **başarılı**olarak gösterildiği zaman menüdeki **haritalar** ' ı seçin.
3. <Soil-moisture_MyFarm_YYYY-aa-gg> biçiminde oluşturulduğu güne göre haritada arama yapın.
4. **Ad** sütununda bir harita seçin, seçilen haritanın önizlemesine sahip bir açılır pencere görüntülenir.
5. **İndir**'i seçin. Eşleme indirilir ve bilgisayarınızın yerel klasörüne depolanır.

    ![Proje grubu ları](./media/get-sensor-data-from-sensor-partner/download-soil-moisture-map-1.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık bir SOIL Moisture heatmap 'i başarıyla oluşturduğunuza göre, [algılayıcı yerleşimi oluşturma](generate-maps-in-azure-farmbeats.md#sensor-placement-map) ve [Geçmiş telemetri verilerini](ingest-historical-telemetry-data-in-azure-farmbeats.md)alma hakkında bilgi edinin. 
