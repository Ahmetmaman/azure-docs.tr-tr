---
title: Microsoft Azure Data Box'ı geri göndermeye yönelik öğretici | Microsoft Docs
description: Bu öğreticide göndermeye hazırlanma, Data Box'ı gönderme, verilerin karşıya yüklenmesini doğrulama ve Data Box'tan verileri silme işlemleriyle Azure Data Box'ı nasıl geri göndereceğinizi öğrenin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 11/19/2020
ms.author: alkohli
ms.localizationpriority: high
ms.openlocfilehash: 28637f35294e9c70cbf0e96fb2cdcb1853d2ba39
ms.sourcegitcommit: 9889a3983b88222c30275fd0cfe60807976fd65b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94992735"
---
::: zone target="docs"

# <a name="tutorial-return-azure-data-box-and-verify-data-upload-to-azure"></a>Öğretici: Azure Data Box'ı iade etme ve Azure'a verilerin yüklendiğini doğrulama

::: zone-end

::: zone target="chromeless"

## <a name="return-data-box-and-verify-data-upload-to-azure"></a>Data Box'ı iade etme ve Azure'a verilerin yüklendiğini doğrulama

::: zone-end

::: zone target="docs"

Bu öğretici, Azure Data Box’ın nasıl iade edileceğini ve yüklenen verileri nasıl doğrulayabileceğinizi anlatır.

Bu öğreticide şu gibi konular hakkında bilgi edineceksiniz:

> [!div class="checklist"]
>
> * Ön koşullar
> * Göndermeye hazırlama
> * Data Box'ı Microsoft'a gönderme
> * Azure'a verilerin yüklendiğini doğrulama
> * Data Box'tan verileri silme

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunlardan emin olun:

* [Öğretici: Verileri Azure Data Box'a kopyalama ve doğrulama](data-box-deploy-copy-data.md) bölümünü tamamladınız.
* Kopyalama işleri tamamlanıp **Bağlan ve kopyala** sayfasında hata kalmadıysa. Kopyalama işleri devam ediyorsa veya **Bağlan ve kopyala** sayfasında hatalar varsa göndermeye hazırlama adımı çalışmaz.

## <a name="prepare-to-ship"></a>Göndermeye hazırlama

[!INCLUDE [data-box-prepare-to-ship](../../includes/data-box-prepare-to-ship.md)]

::: zone-end

::: zone target="chromeless"

Veri kopyalama işlemi tamamlandıktan sonra cihazı hazırlayıp gönderin. Cihaz Azure veri merkezine ulaştığında veriler Azure'a otomatik olarak yüklenir.

## <a name="prepare-to-ship"></a>Göndermeye hazırlama

Göndermeye hazırlama işleminden önce kopyalama işlerinin tamamlandığından emin olun.

1. Yerel web kullanıcı arabirimindeki **Göndermeye hazırla** sayfasına gidin ve gönderme hazırlığına başlayın. 
2. Cihazı yerel web kullanıcı arabiriminden kapatın. Kabloları cihazdan çıkarın. 

Sonraki adımlar cihazın iade edildiği bölgeye göre belirlenir.

::: zone-end

::: zone target="docs"

## <a name="ship-data-box-back"></a>Data Box'ı geri gönderme

Cihaza veri kopyalamanın tamamlandığından ve **göndermeye hazırlama** işleminin başarılı olduğundan emin olun. Cihazı gönderdiğiniz bölgeye bağlı olarak yordam farklıdır.

::: zone-end

## <a name="us-canada-europe"></a>[ABD, Kanada, Avrupa](#tab/in-us-canada-europe)

Cihazı ABD, Kanada veya Avrupa’da iade ediyorsanız aşağıdaki adımları uygulayın.

1. Cihazın kapalı olduğundan ve kabloların çıkartılmış olduğundan emin olun. 
2. Cihaz ile beraber sağlanan güç kablosunu sararak emniyetli şekilde cihazın arkasına yerleştirin.
3. E-ink ekranda gönderi etiketinin görüntülendiğinden emin olun ve taşıyıcınızdan bir teslim alma randevusu alın. Etiket hasar görür veya kaybolursa ya da E-ink ekranında gösterilmezse Microsoft Desteği’ne başvurun. Destek öneride bulunursa Azure portalındaki **Genel Bakış > Gönderim etiketini indir** bölümüne gidebilirsiniz. Gönderim etiketini indirip cihaza yapıştırın. 
4. Cihazı iade ediyorsanız UPS ile bir toplama zamanı ayarlayın. Teslim alma planlamak için:

    * Yerel UPS şubesini (ülkeye/bölgeye özel ücretsiz hatlar) arayın.
    * Görüşme esnasında, E-ink ekranında veya basılı etiketinizde gördüğünüz iade izleme numarasını bildirin.
    * İzleme numarası bildirilmezse, teslim alma esnasında UPS’ye ek ücret ödemeniz gerekir.

    Teslim alma zamanlama yerine Data Box’ı en yakın teslim etme yerine bırakabilirsiniz.
4. Data Box nakliyeciniz tarafından toplandıktan ve tarandıktan sonra, portaldaki sipariş durumu **Toplandı** olarak güncelleştirilir. Ayrıca bir takip numarası da görüntülenir.

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Data Box'tan verileri silme

Veriler Azure'a yüklendikten sonra Data Box disklerindeki veriyi [NIST SP 800-88 Revision 1 yönergelerine](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi) uygun şekilde siler.

::: zone-end

## <a name="australia"></a>[Avustralya](#tab/in-australia)

Avustralya’daki Azure veri merkezlerinde ek güvenlik bildirimi uygulanır. Gelen tüm sevkiyatların önceden bildirilmesi gerekir. Avustralya’da gönderme için aşağıdaki adımları uygulayın.

1. İade gönderimi için cihazı gönderirken kullanılan orijinal kutuyu saklayın.
2. Cihaza veri kopyalamanın tamamlandığından ve **göndermeye hazırlama** işleminin başarılı olduğundan emin olun.
3. Cihazı kapatın ve kabloları çıkarın.
4. Cihaz ile beraber sağlanan güç kablosunu sararak emniyetli şekilde cihazın arkasına yerleştirin.
5. [DHL Bağlantısında](https://mydhl.express.dhl/au/en/schedule-pickup.html#/schedule-pickup#label-reference) çevrimiçi olarak bir teslim alma rezervasyonu yapın.

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Data Box'tan verileri silme

Veriler Azure'a yüklendikten sonra Data Box disklerindeki veriyi [NIST SP 800-88 Revision 1 yönergelerine](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi) uygun şekilde siler.

::: zone-end

## <a name="japan"></a>[Japonya](#tab/in-japan)

1. İade gönderimi için cihazı gönderirken kullanılan orijinal kutuyu saklayın.
2. Cihazı kapatın ve kabloları çıkarın.
3. Cihaz ile beraber sağlanan güç kablosunu sararak emniyetli şekilde cihazın arkasına yerleştirin.
4. Sevk mektubuna, gönderici bilgisi olarak şirket adınızı ve adres bilgilerinizi yazın.
5. Aşağıdaki e-posta şablonunu kullanarak Quantium Solutions’a e-posta gönderin.

    * Japan Post Chakubarai sevk mektubu gönderilmediyse veya kaybolduysa, gönderdiğiniz e-postada bunu da bildirin. Quantium Solutions Japan, teslim alma esnasında Japan Post’un sevk mektubunu da getirmesini ister.
    * Birden fazla siparişiniz varsa her birinin ayrı olarak teslim alınması için e-posta gönderin.

    ```
    To: Customerservice.JP@quantiumsolutions.com
    Subject: Pickup request for Azure Data Box｜Job name： 
    Body:
    - Japan Post Yu-Pack tracking number (reference number)：
    - Requested pickup date：mmdd (Select a requested time slot from below).
    a. 08：00-13：00 
    b. 13：00-15：00 
    c. 15：00-17：00 
    d. 17：00-19：00 
    ```

6. Teslim alma rezervasyonu yaptıktan sonra Quantium Solutions’tan e-posta onay iletisi alırsınız. E-posta onay iletisi, Chakubarai sevk mektubuna yönelik bilgileri de içerir.

Gerekirse, aşağıdaki bilgilerden Quantium Solutions Desteği (Japonca) ile iletişime geçebilirsiniz: 

* E-posta: Customerservice.JP@quantiumsolutions.com 
* Telefon：03-5755-0150 

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Data Box'tan verileri silme
 
Veriler Azure'a yüklendikten sonra Data Box disklerindeki veriyi [NIST SP 800-88 Revision 1 yönergelerine](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi) uygun şekilde siler.

::: zone-end

## <a name="singapore"></a>[Singapur](#tab/in-singapore)

1. İade gönderimi için cihazı gönderirken kullanılan orijinal kutuyu saklayın.
2. İzleme numarasını (Data Box yerel web kullanıcı arabiriminin Göndermeye Hazırlama sayfasında başvuru numarası olarak gösterilir) not alın. Bu numara, göndermeye hazırlama adımı başarıyla tamamlandıktan sonra oluşturulur. Bu sayfadaki gönderim etiketini indirin ve kutunun üzerine yapıştırın.
3. Cihazı kapatın ve kabloları çıkarın.
4. Cihaz ile beraber sağlanan güç kablosunu sararak emniyetli şekilde cihazın arkasına yerleştirin. 
5. Aşağıdaki e-posta şablonuna takip numarasını ekleyin ve SingPost Müşteri Hizmetleri'ne gönderin.

    ```
    To: kadcustcare@singpost.com
    Subject: Microsoft Azure Pickup - OrderName 
    Body: 
        1. Requestor name  
        2. Requestor contact number
        3. Requestor collection address
        4. Preferred collection date
    ```

   > [!NOTE]
   > İş gününde alınan rezervasyon istekleri için:
   > * 15:00’ten önce, teslim alma sonraki iş gününde 09:00 ile 13:00 arasında gerçekleştirilir.
   > * 15:00’ten sonra, teslim alma sonraki iş gününde 14:00 ile 18:00 arasında gerçekleştirilir.  

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Data Box'tan verileri silme

Veriler Azure'a yüklendikten sonra Data Box disklerindeki veriyi [NIST SP 800-88 Revision 1 yönergelerine](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi) uygun şekilde siler.

::: zone-end

## <a name="south-africa"></a>[Güney Afrika](#tab/in-sa)

1. İade gönderimi için cihazı orijinal kutusunda paketleyin.
2. Cihaz ile beraber sağlanan güç kablosunu sararak emniyetli şekilde cihazın arkasına yerleştirin.
3. İzleme numarasını (Data Box yerel web kullanıcı arabiriminin **Göndermeye Hazırlama** sayfasında başvuru numarası olarak gösterilir) not alın. Bu numara, “Göndermeye hazırlama” adımı başarıyla tamamlandıktan sonra sağlanır. Bu sayfadaki gönderim etiketini indirin ve kutunun üzerine yapıştırın.
4. Azure Data Box İşlemlerinden dönüş kodunu isteyin. Dönüş kodu, paketi veri merkezine geri teslim etmek için gereklidir. [adbops@microsoft.com](mailto:adbops@microsoft.com) adresine e-posta gönderin. Açıkça görünebilmesi için bu kodu gönderim etiketinde iade adresinin yanına ekleyin.
5. Aşağıdaki yöntemlerden birini kullanarak DHL ile bir teslim alma rezervasyonu yapın:
 
   * [DHL Express Güney Afrika, **Schedule a Pickup**](https://mydhl.express.dhl/za/en/schedule-pickup.html#/schedule-pickup#label-reference) (Teslim Alma Zamanla) öğesine gidip bir çevrimiçi teslim alma rezervasyonu yapın.
   * Aşağıdaki şablonu kullanarak [Priority.Support@dhl.com](mailto:Priority.Support@dhl.com) adresine e-posta gönderin:

     ```output
     To: Priority.Support@dhl.com
     Subject: Pickup request for Microsoft Azure
     Body: Need pick up for the below shipment
       *  DHL tracking number: (reference number/waybill number)
       *  Requested pickup date: yyyy/mm/dd;time:HH MM
       *  Shipper contact: (company name)
       *  Contact person: 
       *  Phone number: 
       *  Full physical address: 
       *  Item to be collected: Azure Dt
     ```

    * İsterseniz paketi en yakın DHL hizmet noktasına da bırakabilirsiniz.

6. Herhangi bir sorunla karşılaşırsanız [Priority.Support@dhl.com](mailto:Priority.Support@dhl.com) adresine karşılaştığınız sorunun ayrıntılarını içeren bir e-posta gönderin ve Konu: satırına irsaliye numarasını ekleyin. +27(0)119213902 numaralı telefonu da arayabilirsiniz.

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Data Box'tan verileri silme

Veriler Azure'a yüklendikten sonra Data Box disklerindeki veriyi [NIST SP 800-88 Revision 1 yönergelerine](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi) uygun şekilde siler.

::: zone-end

## <a name="hong-kong"></a>[Hong Kong](#tab/in-hk)

1. İade gönderimi için cihazı orijinal kutusunda paketleyin.
2. Cihaz ile beraber sağlanan güç kablosunu sararak emniyetli şekilde cihazın arkasına yerleştirin.
3. Çalışma saatleri içinde (Pazartesi - Cuma saat 09:00 ile 18:00 arası) **(852) 2318 1213** numaralı telefondan **Quantium Solutions** bilgi hattını arayın.  
4. Teslim almayı planlamak için iade gönderimi etiketinde Microsoft Azure teslim alma ifadesini, referans numarasını ve takip numarasını (barkodun üzerinde) belirtin.
5. Teslim alma zamanlaması için sözlü teyit alırsınız. Teslim almak için kurye gönderilmezse alternatif düzenlemeler için Quantium Solutions bilgi hattını arayın.
6. Quantium'la teslim alma rezervasyonunu yaptıktan sonra, aşağıdaki şablonu kullanarak onayı [Microsoft Data Box Operations Asia](mailto:adbo@microsoft.com) ile paylaşın:

    ```output
    To: adbo@microsoft.com
    Subject: Microsoft Data Box Job: [order name] has completed copy
    Body:
    We have confirmed the pickup details with Quantium.

       * Requestor name:
       * Requestor contact number:
       * Pickup Date:  
       * Pickup time:
    ```

Sorunlarla karşılaşırsanız konu üst bilgisine iş adını ve karşılaşılan sorunu belirterek Data Box Operations Asia [adbo@microsoft.com](mailto:adbo@microsoft.com) adresine e-posta gönderin.

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Data Box'tan verileri silme
 
Veriler Azure'a yüklendikten sonra Data Box disklerindeki veriyi [NIST SP 800-88 Revision 1 yönergelerine](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi) uygun şekilde siler.

::: zone-end

<!--## [In Korea](#tab/in-korea) 

1. Retain the original box used to ship the device for return shipment.
2. Note down the tracking number (shown as reference number on the Prepare to Ship page of the Data Box local web UI). This is available after the prepare to ship step successfully completes. Download the shipping label from this page and paste on the packing box. 
3. Power off the device and remove the cables.
4. Spool and securely place the power cord that was provided with the device in the back of the device. 

Request pickup  
If consignment note is present:  

1. Call Quantium Solutions International hotline at 070-8231-1418 during office hours (10 AM to 5 PM, Monday to Friday). Quote Microsoft Azure pickup and the service request number to arrange for a collection.
2. If the hotline is busy, email microsoft@rocketparcel.com, with the email subject Microsoft Azure Pickup and the service request number as reference.  
3. If the courier does not arrive for collection, call Quantium Solutions International hotline for alternate arrangements.  
4. You will receive an email confirmation for the pickup schedule.  

Exception process
If the consignment note is not present:
1. Call Quantium Solutions International hotline at 070-8231-1418 during office hours (10 AM to 5 PM, Monday to Friday). Quote Microsoft Azure pickup and the service request number. Specify that you need a new consignment note to arrange for a collection. Provide sender (customer), receiver information (Azure datacenter), and reference number (service request number).
2. If the hotline is busy, email microsoft@rocketparcel.com, with the email subject Microsoft Azure Pickup and the service request number as reference.
3. If the courier does not arrive for collection, call Quantium Solutions International hotline for alternate arrangements.
4. You get a verbal confirmation if request is made via telephone.  
::: zone target="chromeless"

## Verify data upload to Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## Erasure of data from Data Box
 
Once the upload to Azure is complete, the Data Box erases the data on its disks as per the [NIST SP 800-88 Revision 1 guidelines](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).

::: zone-end

::: zone target="docs"

[!INCLUDE [data-box-verify-upload-return](../../includes/data-box-verify-upload-return.md)]

::: zone-end
-->

## <a name="self-managed"></a>[Kullanıcı Yönetimli](#tab/in-selfmanaged)

Data Box’ı US Government, Japonya, Singapur, Güney Kore, Hindistan, Güney Afrika veya Batı Avrupa’da kullanıyorsanız ve sipariş oluşturma sırasında kullanıcı yönetimli gönderim seçeneğini belirlediyseniz bu yönergeleri izleyin. 

1. Bu adım başarıyla tamamlandıktan sonra Data Box yerel web kullanıcı arabiriminin Göndermeye Hazırlama sayfasında gösterilen Yetkilendirme kodunu not alın.
2. Cihazı kapatın ve kabloları çıkarın. Cihaz ile beraber sağlanan güç kablosunu sararak emniyetli şekilde cihazın arkasına yerleştirin.
3. Cihazı iade etmeye hazırsanız aşağıdaki şablonu kullanarak Azure Data Box Operasyon ekibine bir e-posta gönderin.

    ```
    To: adbops@microsoft.com
    Subject: Request for Azure Data Box drop-off for order: 'orderName'
    Body:
        1. Order name  
        2. Authorization code available after Prepare to Ship has completed [Yes/No]  
        3. Contact name of the person dropping off. You will need to display a Government approved ID during the drop off.
    ```

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Data Box'tan verileri silme
 
Veriler Azure'a yüklendikten sonra Data Box disklerindeki veriyi [NIST SP 800-88 Revision 1 yönergelerine](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi) uygun şekilde siler.

::: zone-end

---

::: zone target="docs"

## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

[!INCLUDE [data-box-verify-upload-return](../../includes/data-box-verify-upload-return.md)]

::: zone-end

