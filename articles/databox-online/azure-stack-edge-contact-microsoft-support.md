---
title: Azure Stack Edge Pro için günlük destek bileti, Azure Data Box Gateway | Microsoft Docs
description: Azure Stack Edge Pro veya Data Box Gateway emirleriniz ile ilgili sorunlar için destek isteğini günlüğe kaydetme hakkında bilgi edinin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 03/05/2021
ms.author: alkohli
ms.openlocfilehash: f76652600f42d7e82914836537935ac9a74decb4
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102436812"
---
# <a name="open-a-support-ticket-for-azure-stack-edge-pro-and-azure-data-box-gateway"></a>Azure Stack Edge Pro ve Azure Data Box Gateway için bir destek bileti açın

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-databox-gateway-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-databox-gateway-sku.md)]

Bu makale, Azure Stack Edge Pro/Azure Data Box Gateway hizmeti tarafından yönetilen Azure Stack Edge Pro ve Azure Data Box Gateway için geçerlidir. Hizmetinize ilişkin herhangi bir sorunla karşılaşırsanız, teknik destek için bir hizmet isteği oluşturabilirsiniz. Bu makalede izlenecek yol gösterilmektedir:

* Destek isteği oluşturma.
* Portalın içinden destek isteği yaşam döngüsünü yönetme.

## <a name="create-a-support-request"></a>Destek isteği oluşturma

Bir destek isteği oluşturmak için aşağıdaki adımları uygulayın:

1. Azure Stack Edge Pro veya Data Box Gateway sıraya gidin. **Destek + sorun giderme** Bölümü ' ne gidin ve **Yeni destek isteği**' ni seçin.

2. **Yeni destek isteğinde**, **temel bilgiler** sekmesinde aşağıdaki adımları uygulayın:

    1. **Sorun türü** açılan listesinden **Teknik**' i seçin.
    2. **Aboneliğinizi** seçin.
    3. **Hizmet** altında **hizmetlerimi** denetleyin. Açılan listeden **Azure Stack Edge Pro ve Data Box Gateway** öğesini seçin.
    4. Kaynağınızı seçin **.** Bu, siparişinizin adına karşılık gelir.
    5. Yaşadığınız sorunun kısa bir **özetini** sunun. 
    6. **Sorun türünü** seçin.
    7. Seçtiğiniz sorun türüne göre ilgili bir **sorun alt türünü** seçin.
    8. **İleri ' yi seçin: çözümler >>**.

        ![Temel Bilgiler](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-support-request-1.png)

3. **Ayrıntılar** sekmesinde aşağıdaki adımları uygulayın:

    1. Sorunun başlangıç tarihini ve saatini belirtin.
    2. Sorununuz için bir **Açıklama** sağlayın.
    3. Karşıya yüklemek istediğiniz diğer dosyalara gözatabilmeniz için **karşıya dosya yükleme** bölümünde klasör simgesini seçin.
    4. **Paylaşma tanılama bilgilerini** denetleyin.
    5. Aboneliğiniz temelinde, bir **destek planı** otomatik olarak doldurulur.
    6. Açılan listeden **önem derecesi**' ni seçin.
    7. **Tercih edilen bir iletişim yöntemi** belirtin.
    8. **Yanıt saatleri** , abonelik planınıza göre otomatik olarak seçilir.
    9. Destek için tercih ettiğiniz dili sağlayın.
    10. **İletişim bilgilerinde** adınızı, e-postanızı, telefonunuzu, isteğe bağlı iletişim, ülke/bölge ' yi belirtin. Microsoft Desteği, bu bilgileri daha fazla bilgi, tanılama ve çözümleme için size ulaşmak üzere kullanır. 
    11. **İleri ' yi seçin: gözden geçir + oluştur >>**.

        ![Sorun](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-support-request-2.png)

4. **Gözden geçir + oluştur** sekmesinde destek bileti ile ilgili bilgileri gözden geçirin. **Oluştur**’u seçin. 

    ![Sorun 2](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-support-request-3.png)

    Destek bileti oluşturduktan sonra bir destek mühendisi, isteğinize devam etmek için en kısa sürede sizinle iletişim kuracaktır.

## <a name="get-hardware-support"></a>Donanım desteğini al

Bu bilgiler yalnızca Azure Stack cihaz için geçerlidir. Donanım sorunlarını bildirme işlemi aşağıdaki gibidir:

1. Bir donanım sorunu için Azure portal bir destek bileti açın. **Sorun türü** altında **donanım Azure Stack**' yi seçin. **Sorun alt türünü** **donanım hatası** olarak seçin.

    ![Donanım sorunu](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-hardware-issue-1.png)

    Destek bileti oluşturduktan sonra bir destek mühendisi, isteğinize devam etmek için en kısa sürede sizinle iletişim kuracaktır.

2. Microsoft Desteği bunun bir donanım sorunu olduğunu belirlerse, aşağıdaki eylemlerden biri oluşur:

    * Başarısız donanım bölümü için bir alan değiştirme birimi (FRU) gönderilir. Şu anda, güç kaynağı birimleri ve katı hal sürücüleri desteklenen tek FRU 'lardır.
    * Yalnızca FRU 'lar bir sonraki iş günü içinde değiştirilmiştir, başka her şey için bir tam sistem değişikliği (FSR) gerekir.

3. Bir FRU değişikliği için 1 PM yerel saati (Pazartesi-Cuma) gerektiğini tespit ediyorsanız, bir FRU değişikliği yapmak için işyerindeki bir sonraki iş günü konumunuza bir sonraki iş günü gönderilir. Tüm sistem değişikliği genellikle çok daha uzun sürer, çünkü parçalar fabrikamızda gönderilir ve nakliye ve gümrük gecikmelerine tabi olabilir.

## <a name="manage-a-support-request"></a>Destek isteğini yönetme

Bir destek bileti oluşturduktan sonra portal üzerinden bu biletin yaşam döngüsünü yönetebilirsiniz.

### <a name="to-manage-your-support-requests"></a>Destek isteklerinizi yönetmek için

1. Yardım ve destek sayfasına ulaşmak için, **> yardım ve destek**' e gidin.

    ![Destek isteklerini yönetme](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-manage-support-request-1.png)

2. **Son destek isteklerinin** tablolu bir listesi **Yardım + Destek** bölümünde görüntülenir.

    <!--[Manage support requests](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-support-request-1.png)--> 

3. Bir destek isteği seçin ve tıklayın. Bu istek için durumu ve ayrıntıları görüntüleyebilirsiniz. Bu istekte izlemek istiyorsanız **+ Yeni ileti** ' ya tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack Edge Pro ile ilgili sorunları nasıl giderebileceğinizi](azure-stack-edge-troubleshoot.md)öğrenin.
[Data Box Gateway ilgili sorunları nasıl giderebileceğinizi](../databox-gateway/data-box-gateway-troubleshoot.md)öğrenin.