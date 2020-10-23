---
title: Azure Remote Rendering hesabı oluşturma
description: Azure uzaktan Işleme için bir hesap oluşturma adımlarını açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/11/2020
ms.topic: how-to
ms.openlocfilehash: 8169b277dfae918e86ac493259325ff84d0b6a4e
ms.sourcegitcommit: 6906980890a8321dec78dd174e6a7eb5f5fcc029
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92427471"
---
# <a name="create-an-azure-remote-rendering-account"></a>Azure Remote Rendering hesabı oluşturma

Bu bölüm, **Azure uzaktan işleme** hizmeti için bir hesap oluşturma adımlarında size rehberlik eder. Hızlı başlangıç veya öğreticilerden herhangi birini tamamlamak için geçerli bir hesap zorunludur.

## <a name="create-an-account"></a>Hesap oluşturma

Azure uzaktan Işleme hizmeti için bir hesap oluşturmak için aşağıdaki adımlar gereklidir:

1. [Karma gerçeklik önizleme sayfasına](https://aka.ms/MixedRealityPrivatePreview) gidin
1. ' Kaynak oluştur ' düğmesine tıklayın
1. Arama alanında ("markette ara"), "uzaktan Işleme" yazın ve ' Enter ' tuşuna basın.
1. Sonuç listesinde, "uzaktan Işleme" kutucuğuna tıklayın
1. Sonraki ekranda, "Oluştur" düğmesine tıklayın. Yeni bir uzaktan Işleme hesabı oluşturmak için bir form açılır:
    1. ' Kaynak adı ' nı hesabın adına ayarlayın
    1. Gerekirse ' abonelik ' güncelleştirmesini Güncelleştir
    1. ' Kaynak grubu ' nu tercih ettiğiniz bir kaynak grubuna ayarlayın
    1. ' Konum ' açılan listesinden bu kaynağın oluşturulacağı bölgeyi seçin. Aşağıdaki [Hesap bölgelerinde](create-an-account.md#account-regions) yer alan açıklamalara bakın.
1. Hesap oluşturulduktan sonra, bu hesaba gidin ve:
    1. *Genel bakış* sekmesinde, ' hesap kimliği ' ' ne göz atın
    1. *Ayarlar > erişim anahtarları* sekmesinde, ' birincil anahtar ' ' a, hesabın gizli hesap anahtarı ' na göz atalım

### <a name="account-regions"></a>Hesap bölgeleri
Hesabın hesap oluşturma zamanı sırasında belirtilen konum, hesap kaynağının hangi bölgeye atandığını belirler. Bu, oluşturulduktan sonra değiştirilemez. Ancak hesap, hesabın konumundan bağımsız olarak desteklenen herhangi bir [bölgedeki](./../reference/regions.md)uzaktan işleme oturumuna bağlanmak için kullanılabilir.

### <a name="retrieve-the-account-information"></a>Hesap bilgilerini alma

Örnekler ve öğreticiler, hesap KIMLIĞI ve bir anahtar sağlamanızı gerektirir. Örneğin, PowerShell örnek betikleri için kullanılan dosyadaki **arrconfig.js** :

```json
"accountSettings": {
    "arrAccountId": "<fill in the account ID from the Azure portal>",
    "arrAccountKey": "<fill in the account key from the Azure portal>",
    "region": "<select from available regions>"
},
```

*Bölge* seçeneğini doldurmak için [kullanılabilir bölgelerin listesine](../reference/regions.md) bakın.

**`arrAccountId`** Ve değerlerini **`arrAccountKey`** Aşağıdaki adımlarda açıklandığı gibi portalda bulabilirsiniz:

* [Azure portal](https://www.portal.azure.com)'a gidin
* **"Uzaktan Işleme hesabınızı"** bulun; **"en son kaynaklar"** listesinde olması gerekir. Ayrıca, üstteki arama çubuğunda da arama yapabilirsiniz. Bu durumda, kullanmak istediğiniz aboneliğin varsayılan abonelik filtresi (arama çubuğu ' nun yanında bulunan filtre simgesi) seçildiğinden emin olun:

![Abonelik filtresi](./media/azure-subscription-filter.png)

Hesabınıza tıkladığınızda bu ekranda  **hesap kimliği** hemen gösterilir:

![Azure Hesap KIMLIĞI](./media/azure-account-id.png)

Anahtar için sol taraftaki panelde **erişim tuşları** ' nı seçin. Bir sonraki sayfada, birincil ve ikincil anahtar gösterilmektedir:

![Azure erişim tuşları](./media/azure-account-primary-key.png)

Değeri, **`arrAccountKey`** birincil veya ikincil anahtar olabilir.

## <a name="link-storage-accounts"></a>Depolama hesaplarını bağlama

Bu paragraf, depolama hesaplarını uzaktan Işleme hesabınıza bağlamayı açıklar. Bir depolama hesabı bağlandığında, hesabınızdaki verilerle etkileşim kurmak istediğiniz her seferinde bir SAS URI 'SI oluşturmak gerekli değildir. Örneğin, bir model yüklenirken. Bunun yerine, [bir model yükleme bölümünde](../concepts/models.md#loading-models)açıklandığı gibi, depolama hesabı adlarını doğrudan kullanabilirsiniz.

Bu paragraftaki adımların, bu alternatif erişim yöntemini kullanması gereken her depolama hesabı için gerçekleştirilmesi gerekir. Henüz depolama hesapları oluşturmadıysanız, [oluşturma hızlı başlangıcı için modeli dönüştürme](../quickstarts/convert-model.md#storage-account-creation)bölümünde ilgili adımı izleyebilirsiniz.

Artık bir depolama hesabınız olduğunu varsaymıştır. Portalda depolama hesabına gidin ve bu depolama hesabı için **Access Control (IAM)** sekmesine gidin:

![Depolama hesabı ıAM](./media/azure-storage-account.png)

 Rol atamaları ekleyebilmeniz için bu depolama hesabı üzerinde sahip izinlerinizin olduğundan emin olun. Erişiminiz yoksa, **Rol Ekleme atama** seçeneği devre dışı bırakılır.

 Sonraki adımlarda açıklandığı gibi üç farklı rol eklemeniz gerekir. Üç erişim düzeyini sağlamazsanız, depolama hesabına erişmeye çalışırken izin sorunları olur.

 İlk rolü eklemek için "rol ataması Ekle" kutucuğunda **Ekle** düğmesine tıklayın:

![Depolama hesabı ıAM rol ataması Ekle](./media/azure-add-role-assignment.png)

* Atanacak ilk rol, yukarıdaki ekran görüntüsünde gösterildiği gibi **sahibidir** .
* **Uzaktan Işleme hesabı** ' na **erişimi ata** açılan listesinden seçin.
* Son açılan kutudan aboneliğinizi ve uzaktan Işleme hesabınızı seçin.

> [!WARNING]
> Uzaktan Işleme hesabınız listede yoksa, bu [sorun giderme bölümüne](../resources/troubleshoot.md#cant-link-storage-account-to-arr-account)bakın.

**Rol** açılan listesinden ilgili seçimler için yeni roller eklemeyi iki kez daha yineleyin:

* **Depolama Hesabı Katılımcısı**
* **Depolama Blob Verileri Katkıda Bulunanı**

Diğer açılan listeleri ilk adımda olarak seçilidir.

Üç rolü de eklediyseniz, Azure uzaktan Işleme hesabınız, sistem tarafından atanan yönetilen hizmet kimliklerini kullanarak depolama hesabınıza erişebilir.
> [!IMPORTANT]
> Azure rol atamaları Azure depolama tarafından önbelleğe alınır, bu nedenle uzaktan işleme hesabınıza erişim verme ve depolama hesabınıza erişmek için kullanılabileceği zaman arasında 30 dakikalık bir gecikme olabilir. Ayrıntılar için bkz. [Azure rol tabanlı erişim denetimi (Azure RBAC) belgeleri](../../role-based-access-control/troubleshooting.md#role-assignment-changes-are-not-being-detected) .

## <a name="next-steps"></a>Sonraki adımlar

* [Kimlik Doğrulaması](authentication.md)
* [Kimlik doğrulaması için Azure ön uç API 'Lerini kullanma](frontend-apis.md)
* [Örnek PowerShell betikleri](../samples/powershell-example-scripts.md)