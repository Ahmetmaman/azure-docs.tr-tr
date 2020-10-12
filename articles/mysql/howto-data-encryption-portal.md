---
title: Veri şifreleme-Azure portal-MySQL için Azure veritabanı
description: Azure portal kullanarak MySQL için Azure veritabanı için veri şifrelemeyi ayarlamayı ve yönetmeyi öğrenin.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: how-to
ms.date: 01/13/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 201459f4a7d2d23b384435493d6272e569698933
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90887168"
---
# <a name="data-encryption-for-azure-database-for-mysql-by-using-the-azure-portal"></a>Azure portal kullanarak MySQL için Azure veritabanı için veri şifreleme

MySQL için Azure veritabanınız için veri şifrelemeyi ayarlamak ve yönetmek üzere Azure portal nasıl kullanacağınızı öğrenin.

## <a name="prerequisites-for-azure-cli"></a>Azure CLı önkoşulları

* Bu abonelikte bir Azure aboneliğiniz olması ve bir yönetici olmanız gerekir.
* Azure Key Vault ' de, bir anahtar kasası ve müşterinin yönettiği anahtar için kullanılacak bir anahtar oluşturun.
* Anahtar Kasası, müşteri tarafından yönetilen anahtar olarak kullanmak için aşağıdaki özelliklere sahip olmalıdır:
  * [Geçici silme](../key-vault/general/soft-delete-overview.md)

    ```azurecli-interactive
    az resource update --id $(az keyvault show --name \ <key_vault_name> -o tsv | awk '{print $1}') --set \ properties.enableSoftDelete=true
    ```

  * [Korumalı Temizleme](../key-vault/general/soft-delete-overview.md#purge-protection)

    ```azurecli-interactive
    az keyvault update --name <key_vault_name> --resource-group <resource_group_name>  --enable-purge-protection true
    ```

* Anahtar, müşteri tarafından yönetilen anahtar olarak kullanmak için aşağıdaki özniteliklere sahip olmalıdır:
  * Sona erme tarihi yok
  * Devre dışı değil
  * Al, sarmalama tuşu, anahtar sarmalama işlemini geri alabilir

## <a name="set-the-right-permissions-for-key-operations"></a>Anahtar işlemleri için doğru izinleri ayarla

1. Key Vault ' de erişim **ilkeleri**  >  **erişim ilkesi Ekle**' yi seçin.

   :::image type="content" source="media/concepts-data-access-and-security-data-encryption/show-access-policy-overview.png" alt-text="Erişim ilkeleriyle Key Vault ekran görüntüsü ve erişim Ilkesi vurgulandı":::

2. **Anahtar izinleri**' ni seçin ve MySQL sunucusunun adı olan Al, **sarmalama**, **geri** **Al**ve **asıl**' ı seçin. Sunucu sorumlunuz mevcut sorumlular listesinde bulunamazsa, kaydolmanız gerekir. Veri şifrelemeyi ilk kez ayarlamaya çalıştığınızda sunucu sorumlunuzu kaydetmeniz istenir ve başarısız olur.

   :::image type="content" source="media/concepts-data-access-and-security-data-encryption/access-policy-wrap-unwrap.png" alt-text="Erişim ilkeleriyle Key Vault ekran görüntüsü ve erişim Ilkesi vurgulandı":::

3. **Kaydet**’i seçin.

## <a name="set-data-encryption-for-azure-database-for-mysql"></a>MySQL için Azure veritabanı için veri şifrelemeyi ayarlama

1. MySQL için Azure veritabanı 'nda, müşteri tarafından yönetilen anahtarı ayarlamak için **veri şifreleme** ' yi seçin.

   :::image type="content" source="media/concepts-data-access-and-security-data-encryption/data-encryption-overview.png" alt-text="Erişim ilkeleriyle Key Vault ekran görüntüsü ve erişim Ilkesi vurgulandı":::

2. Bir anahtar kasası ve anahtar çifti seçebilir ya da bir anahtar tanımlayıcı girebilirsiniz.

   :::image type="content" source="media/concepts-data-access-and-security-data-encryption/setting-data-encryption.png" alt-text="Erişim ilkeleriyle Key Vault ekran görüntüsü ve erişim Ilkesi vurgulandı":::

3. **Kaydet**’i seçin.

4. Tüm dosyaların (geçici dosyalar dahil) tamamen şifrelendiğinden emin olmak için sunucuyu yeniden başlatın.

## <a name="using-data-encryption-for-restore-or-replica-servers"></a>Geri yükleme veya çoğaltma sunucuları için veri şifrelemeyi kullanma

MySQL için Azure veritabanı, Key Vault ' de depolanan bir müşterinin yönetilen anahtarıyla şifrelendikten sonra, sunucunun yeni oluşturulan kopyası da şifrelenir. Bu yeni kopyayı yerel veya coğrafi geri yükleme işlemi aracılığıyla ya da bir çoğaltma (yerel/bölge) işlemi aracılığıyla yapabilirsiniz. Bu nedenle, şifrelenmiş bir MySQL sunucusu için, şifrelenmiş bir geri yüklenmiş sunucu oluşturmak için aşağıdaki adımları kullanabilirsiniz.

1. Sunucunuzda **genel bakış**  >  **geri yükleme**' yi seçin.

   :::image type="content" source="media/concepts-data-access-and-security-data-encryption/show-restore.png" alt-text="Erişim ilkeleriyle Key Vault ekran görüntüsü ve erişim Ilkesi vurgulandı":::

   Ya da çoğaltma özellikli bir sunucu için, **Ayarlar** başlığı altında **çoğaltma**' yı seçin.

   :::image type="content" source="media/concepts-data-access-and-security-data-encryption/mysql-replica.png" alt-text="Erişim ilkeleriyle Key Vault ekran görüntüsü ve erişim Ilkesi vurgulandı":::

2. Geri yükleme işlemi tamamlandıktan sonra oluşturulan yeni sunucu birincil sunucunun anahtarıyla şifrelenir. Ancak, sunucudaki Özellikler ve seçenekler devre dışı bırakılır ve sunucuya erişilemez. Bu, tüm veri düzenlemesini engeller, çünkü yeni sunucu kimliği henüz anahtar kasasına erişmek için izin verilmemiş.

   :::image type="content" source="media/concepts-data-access-and-security-data-encryption/show-restore-data-encryption.png" alt-text="Erişim ilkeleriyle Key Vault ekran görüntüsü ve erişim Ilkesi vurgulandı":::

3. Sunucuyu erişilebilir hale getirmek için geri yüklenen sunucuda anahtarı yeniden doğrulayın. **Veri şifrelemeyi**  >  **yeniden doğrula anahtarını**seçin.

   > [!NOTE]
   > Yeni sunucunun hizmet sorumlusunun anahtar kasasına erişim izni verilmesi gerektiğinden, ilk yeniden doğrulama denemesi başarısız olur. Hizmet sorumlusunu oluşturmak için **anahtarı yeniden doğrula**' yı seçin, bu, bir hatayı gösterir, ancak hizmet sorumlusu oluşturur. Bundan sonra bu makalede daha önce bahsedilen [adımlara](#set-the-right-permissions-for-key-operations) bakın.

   :::image type="content" source="media/concepts-data-access-and-security-data-encryption/show-revalidate-data-encryption.png" alt-text="Erişim ilkeleriyle Key Vault ekran görüntüsü ve erişim Ilkesi vurgulandı":::

   Yeni sunucuya Anahtar Kasası erişimi sağlamanız gerekir.

4. Hizmet sorumlusu kaydedildikten sonra anahtarı yeniden doğruladıktan sonra sunucu normal işlevselliğini sürdürür.

   :::image type="content" source="media/concepts-data-access-and-security-data-encryption/restore-successful.png" alt-text="Erişim ilkeleriyle Key Vault ekran görüntüsü ve erişim Ilkesi vurgulandı":::

## <a name="next-steps"></a>Sonraki adımlar

 Veri şifreleme hakkında daha fazla bilgi edinmek için bkz. [müşteri tarafından yönetilen anahtarla MySQL Için Azure veritabanı veri şifrelemesi](concepts-data-encryption-mysql.md).
