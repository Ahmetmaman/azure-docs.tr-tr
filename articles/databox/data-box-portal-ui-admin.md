---
title: Azure Data Box Disk portalı yönetici kılavuzu | Microsoft Docs
description: Azure portal kullanarak Data Box Disk yönetmeyi öğrenin. Siparişleri yönetin, diskleri yönetin ve ilerledikçe bir siparişin durumunu izleyin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: how-to
ms.date: 01/09/2019
ms.author: alkohli
ms.openlocfilehash: 538a650c6063422f89c8ed3d1753981a293693b7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94338178"
---
# <a name="use-azure-portal-to-administer-your-data-box-disk"></a>Data Box Disk’inizi yönetmek için Azure portalını kullanma

Bu makaledeki öğreticiler, Önizleme aşamasındaki Microsoft Azure Data Box Disk için geçerlidir. Bu makalede Data Box Disk ile gerçekleştirilebilen bazı karmaşık iş akışları ve yönetim görevleri anlatılmaktadır. 

Data Box Disk'i Azure portaldan yönetebilirsiniz. Bu makale, Azure portalı kullanarak gerçekleştirebileceğiniz görevlere odaklanmaktadır. Azure portalı kullanarak siparişleri yönetebilir, diskleri yönetebilir ve siparişin durumunu son aşamaya kadar takip edebilirsiniz.

## <a name="cancel-an-order"></a>Siparişi iptal etme

Siparişinizi verdikten sonra çeşitli nedenlerle iptal etmeniz gerekebilir. Siparişi ancak disk hazırlığı başlamadan önce iptal edebilirsiniz. Diskler hazırlandıktan ve sipariş işleme alındıktan sonra iptal işlemi gerçekleştiremezsiniz. 

Bir siparişi iptal etmek için aşağıdaki adımları gerçekleştirin.

1.  **Genel bakış > İptal**'e gidin. 

    ![Bir siparişin Genel Bakış sekmesinde iptal komutu](media/data-box-portal-ui-admin/portal-ui-admin-cancel-command.png)

2.  Sipariş iptal etme nedenini belirtin.  

    ![Siparişi iptal etme nedeni](media/data-box-portal-ui-admin/portal-ui-admin-cancel-order-reason.png)

3.  Sipariş iptal edildikten sonra portaldaki durumu **İptal edildi** olarak görüntülenir.

    ![İptal edilme sırası](media/data-box-portal-ui-admin/portal-ui-admin-canceled-order.png)

Sipariş iptal edildiğinde e-posta bildirimi gönderilmez.

## <a name="clone-an-order"></a>Siparişi kopyalama

Kopyalama belirli durumlarda kullanışlıdır. Örneğin kullanıcı, veri aktarımı için daha önceden Data Box Disk kullanmıştır. Yeni veriler üretildikçe Azure'a aktarmak için daha fazla diske ihtiyaç duyulur. Bu durumda aynı sipariş kopyalanabilir.

Siparişi kopyalamak için aşağıdaki adımları gerçekleştirin.

1.  **Genel bakış > Kopyala**'ya gidin. 

    ![Bir siparişin Genel Bakış sekmesinde kopyalama komutu](media/data-box-portal-ui-admin/portal-ui-admin-clone-command.png)

2.  Siparişin tüm ayrıntıları aynı şekilde korunur. Siparişin adı, özgün siparişin adına *-Kopya* eklenerek oluşturulur. Gizlilik bilgilerini gözden geçirdiğinizi onaylamak için onay kutusunu seçin. **Oluştur**’a tıklayın.    

Kopya sipariş birkaç dakikada oluşturulur ve portal yeni siparişi gösterecek şekilde güncelleştirilir.

[![Klonlanan sıra](media/data-box-portal-ui-admin/portal-ui-admin-cloned-order.png)](media/data-box-portal-ui-admin/portal-ui-admin-cloned-order.png#lightbox) 

## <a name="delete-order"></a>Siparişi silme

Tamamlanan siparişleri silmek isteyebilirsiniz. Siparişte adınız, adresiniz ve iletişim bilgileriniz gibi kişisel bilgileriniz yer alır. Sipariş silindiğinde bu kişisel bilgiler de silinir.

Yalnızca tamamlanan veya iptal edilen siparişleri silebilirsiniz. Siparişi silmek için aşağıdaki adımları gerçekleştirin.

1. **Tüm kaynaklar**'a gidin. Siparişinizi arayın.

    ![Siparişlerde ara](media/data-box-portal-ui-admin/portal-ui-admin-search-data-box-disk-orders.png)

2. Silmek istediğiniz siparişe tıklayın ve **Genel bakış**'a gidin. Komut çubuğundan **Sil**'e tıklayın.

    ![Bir siparişi silme](media/data-box-portal-ui-admin/portal-ui-admin-delete-command.png)

3. Siparişi silme işlemini onaylamanız istendiğinde siparişin adını girin. **Sil**'e tıklayın.

     ![Sipariş Silmeyi Onayla](media/data-box-portal-ui-admin/portal-ui-admin-confirm-deletion.png)


## <a name="download-shipping-label"></a>Sevkiyat etiketini indirme

Disklerinizle birlikte gönderilen iade sevkiyat etiketi kaybolduysa sevkiyat etiketini indirmeniz gerekebilir. 

Sevkiyat etiketini indirmek için aşağıdaki adımları gerçekleştirin.
1.  **Genel bakış > Sevkiyat etiketi indir** bölümüne gidin. Bu seçenek yalnızca disk gönderildikten sonra kullanılabilir. 

    ![Sevkiyat etiketini indirme](media/data-box-portal-ui-admin/portal-ui-admin-download-shipping-label.png)

2.  Aşağıda gösterilen iade sevkiyat etiketi indirilir. Etiketi kaydedin, yazdırın ve iade edilen pakete yapıştırın.

    ![Örnek sevkiyat etiketi](media/data-box-portal-ui-admin/portal-ui-admin-example-shipping-label.png)

## <a name="edit-shipping-address"></a>Teslimat adresini düzenleme

Siparişi verdikten sonra teslimat adresini düzenlemeniz gerekebilir. Bu işlem yalnızca disk yola çıkana kadar kullanılabilir. Disk yola çıktıktan sonra bu seçenek kullanılamaz.

Siparişi düzenlemek için aşağıdaki adımları gerçekleştirin.

1. **Sipariş ayrıntıları > Teslimat adresini düzenle**'ye gidin.

    ![Sipariş ayrıntılarında sevkiyat adresini Düzenle komutu](media/data-box-portal-ui-admin/portal-ui-admin-edit-shipping-address-command.png)

2. Bu sayfada gönderim adresini düzenleyebilir ve yaptığınız değişiklikleri kaydedebilirsiniz.

    ![Sevkiyat adresini Düzenle iletişim kutusu](media/data-box-portal-ui-admin/portal-ui-admin-edit-shipping-address-dbox.png)

## <a name="edit-notification-details"></a>Bildirim ayrıntılarını düzenleme

Sipariş durumu e-postalarının gönderilmesini istediğiniz kullanıcıları değiştirmek isteyebilirsiniz. Örneğin disk teslim edildiğinde veya alındığında bir kullanıcının bilgilendirilmesi gerekebilir. Verilerin kaynaktan silinmeden önce Azure depolama hesabında olduğunu doğrulayabilmeleri için, veri kopyalama işlemi tamamlandığında başka bir kullanıcının bilgilendirilmesi gerekebilir. Bu gibi durumlarda bildirim ayrıntılarını düzenleyebilirsiniz.

Bildirim ayrıntılarını düzenlemek için aşağıdaki adımları gerçekleştirin.

1. **Sipariş ayrıntıları > Bildirim ayrıntılarını düzenle**'ye gidin.

    ![Bildirim ayrıntılarını Düzenle komutu sıralı Ayrıntılar](media/data-box-portal-ui-admin/portal-ui-admin-edit-notification-details-command.png)

2. Bu sayfada bildirim ayrıntılarını düzenleyebilir ve yaptığınız değişiklikleri kaydedebilirsiniz.
 
    ![Bildirim ayrıntılarını Düzenle iletişim kutusu](media/data-box-portal-ui-admin/portal-ui-admin-edit-notification-details-dbox.png)

## <a name="view-order-status"></a>Sipariş durumunu görüntüleme

|Sipariş durumu |Description |
|---------|---------|
|Sipariş edildi     | Sipariş başarıyla oluşturuldu. <br> Diskler kullanılabilir durumda değilse bir bildirim gönderilir. <br>Diskler kullanılabilir durumdaysa Microsoft tarafından gönderilecek disk belirlenir ve disk paketi hazırlanır.        |
|İşlendi     | Siparişin işlenmesi tamamlandı. <br> Sipariş sırasında aşağıdaki eylemler gerçekleştirilir:<li>Diskler AES-128 BitLocker şifrelemesi kullanılarak şifrelenir. </li> <li>Data Box Disk, yetkisiz erişimi önlemek için kilitlenir.</li><li>Bu işlem sırasında disklerin kilidini açan destek anahtarı oluşturulur.</li>        |
|Yola çıktı     | Sipariş sevk edildi. Siparişin 1-2 gün içinde elinize geçmesi gerekir.        |
|Teslim Edildi     | Sipariş, belirtilen adrese teslim edildi.        |
|Teslim alındı     |İade gönderiniz teslim alındı. <br> Azure veri merkezinde sevkiyat alındıktan sonra veriler Azure 'a otomatik olarak yüklenir.         |
|Alındı     | Diskleriniz Azure veri merkezine alındı. Veri kopyalama işlemi yakında başlayacak.        |
|Veriler kopyalandı     |Veri kopyalama işlemi devam ediyor.<br> Veri kopyalama işlemi tamamlanana kadar bekleyin.         |
|Tamamlandı       |Sipariş başarıyla tamamlandı.<br> Şirket içi verilerini sunuculardan silmeden önce verilerinizin Azure’a kopyalandığından emin olun.         |
|Hatalarla tamamlandı| Veri kopyalama işlemi tamamlandı ancak hatalar var. <br> **Genel bakışta** belirtilen yolu kullanarak karşıya yükleme için hata günlüklerini gözden geçirin. Daha fazla bilgi için karşıya yükleme [hata günlüklerini indirme](data-box-disk-troubleshoot-upload.md#download-logs)sayfasına gidin.   |
|İptal edildi            |Sipariş iptal edildi. <br> Siparişi iptal ettiniz veya bir hatayla karşılaşıldı ve sipariş, hizmet tarafından iptal edildi.     |



## <a name="next-steps"></a>Sonraki adımlar

- [Data Box Disk sorunlarını giderme](data-box-disk-troubleshoot.md) hakkında bilgi edinin.
