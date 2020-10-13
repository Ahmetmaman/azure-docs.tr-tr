---
title: Azure RBAC kullanarak Azure anahtar kasasına erişmek için uygulamalara izin verme | Microsoft Docs
description: Azure rol tabanlı erişim denetimi kullanarak anahtarlar, gizlilikler ve sertifikalara erişim sağlamayı öğrenin.
services: key-vault
author: msmbaldwin
manager: rkarlin
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 8/30/2020
ms.author: mbaldwin
ms.openlocfilehash: b4c48f25f1d4bd3822a93d2c780dd148d9e29827
ms.sourcegitcommit: 541bb46e38ce21829a056da880c1619954678586
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2020
ms.locfileid: "91940540"
---
# <a name="provide-access-to-key-vault-keys-certificates-and-secrets-with-an-azure-role-based-access-control-preview"></a>Azure rol tabanlı erişim denetimi (Önizleme) ile Key Vault anahtarlarına, sertifikalara ve gizli anahtarlara erişim sağlama

> [!NOTE]
> Key Vault kaynak sağlayıcısı iki kaynak türünü destekler: **kasa** ve **yönetilen HSM**'ler. Bu makalede açıklanan erişim denetimi yalnızca kasaların için **geçerlidir**. Yönetilen HSM için erişim denetimi hakkında daha fazla bilgi edinmek için bkz. [YÖNETILEN HSM erişim denetimi](../managed-hsm/access-control.md).

Azure rol tabanlı erişim denetimi (Azure RBAC), Azure kaynakları üzerinde ayrıntılı erişim yönetimi sağlayan [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) yerleşik bir yetkilendirme sistemidir.

Azure RBAC, kullanıcıların anahtar, gizli dizi ve sertifika izinlerini yönetmesine olanak tanır. Tüm Anahtar kasaları genelinde tüm izinleri yönetmek için bir yer sağlar. 

Azure RBAC modeli, farklı kapsam düzeylerinde izin ayarlamanıza olanak sağlar: yönetim grubu, abonelik, kaynak grubu veya ayrı kaynaklar.  Anahtar Kasası için Azure RBAC, tek tek anahtarlar, gizlilikler ve sertifikalar üzerinde ayrı izinler sağlama olanağı da sağlar

Daha fazla bilgi için bkz. [Azure rol tabanlı erişim denetimi (Azure RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview).

## <a name="best-practices-for-individual-keys-secrets-and-certificates"></a>Tek tek anahtarlar, gizlilikler ve sertifikalar için en iyi uygulamalar

Önerimiz, her ortam (geliştirme, ön üretim ve üretim) başına uygulama başına bir kasa kullanmaktır.

Tek tek anahtarlar, gizli diziler ve sertifikalar izinleri yalnızca belirli senaryolar için kullanılmalıdır:

-   Katmanlar arasında erişim denetimini ayırmak için gereken çok katmanlı uygulamalar

-   Uygulamaların ilgili anahtar kasasındaki gizli dizi alt kümelerine erişmesi gerektiğinde ortak gizli dizileri olan paylaşılan anahtar Kasası

Azure Key Vault yönetim yönergeleri hakkında daha fazla bilgi için bkz.:

- [En iyi Azure Key Vault uygulamalar](best-practices.md)
- [Hizmet sınırlarını Azure Key Vault](service-limits.md)

## <a name="azure-built-in-roles-for-key-vault-data-plane-operations-preview"></a>Key Vault veri düzlemi işlemleri için Azure yerleşik rolleri (Önizleme)
> [!NOTE]
> `Key Vault Contributor` rol, anahtar kasalarını yönetmek için yönetim düzlemi işlemlerine yöneliktir. Anahtarlar, gizlilikler ve sertifikalara erişime izin vermez.

| Yerleşik rol | Açıklama | ID |
| --- | --- | --- |
| Key Vault Yöneticisi (Önizleme) | Tüm veri düzlemi işlemlerini bir anahtar kasasında ve içindeki tüm nesneleri (sertifikalar, anahtarlar ve gizli diziler dahil) gerçekleştirin. Anahtar Kasası kaynakları yönetemez veya rol atamaları yönetilemez. Yalnızca ' Azure rol tabanlı erişim denetimi ' izin modelini kullanan anahtar kasaları için geçerlidir. | 00482a5a-887f-4fb3-b363-3b7fe8e74483 |
| Key Vault sertifikaları Müdürü (Önizleme) | İzinleri Yönet dışında bir anahtar kasasının sertifikaları üzerinde herhangi bir işlem gerçekleştirin. Yalnızca ' Azure rol tabanlı erişim denetimi ' izin modelini kullanan anahtar kasaları için geçerlidir. | a4417e6f-fecd-4de8-b567-7b0420556985 |
| Key Vault şifre Müdürü (Önizleme)| Anahtar kasasının anahtarları üzerinde, izinleri yönet dışında herhangi bir işlem gerçekleştirin. Yalnızca ' Azure rol tabanlı erişim denetimi ' izin modelini kullanan anahtar kasaları için geçerlidir. | 14b46e9e-c2b7-41B4-b07b-48a6ebf60603 |
| Key Vault şifreleme hizmeti şifrelemesi (Önizleme) | Anahtarların meta verilerini okuyun ve sarmalama/sarmalama işlemleri gerçekleştirin. Yalnızca ' Azure rol tabanlı erişim denetimi ' izin modelini kullanan anahtar kasaları için geçerlidir. | e147488a-f6f5-4113-8e2d-b22465e65bf6 |
| Key Vault şifreleme kullanıcısı (Önizleme) | Anahtarları kullanarak şifreleme işlemleri gerçekleştirin. Yalnızca ' Azure rol tabanlı erişim denetimi ' izin modelini kullanan anahtar kasaları için geçerlidir. | 12338af0-0e69-4776-kir7-57ae8d297424 |
| Key Vault okuyucu (Önizleme)| Anahtar kasalarının ve sertifika, anahtar ve gizli dizileri için meta verileri okuyun. Gizli içerik veya anahtar malzeme gibi hassas değerler okunamaz. Yalnızca ' Azure rol tabanlı erişim denetimi ' izin modelini kullanan anahtar kasaları için geçerlidir. | 21090545-7CA7-4776-B22C-e363652d74d2 |
| Key Vault gizli bilgileri Müdürü (Önizleme)| Anahtar kasasının gizli dizileri üzerinde, izinleri yönet dışında herhangi bir işlem gerçekleştirin. Yalnızca ' Azure rol tabanlı erişim denetimi ' izin modelini kullanan anahtar kasaları için geçerlidir. | b86a8fe4-44ce-4948-aee5-eccb2c155cd7 |
| Key Vault gizli dizi kullanıcısı (Önizleme)| Gizli dizi içeriğini okuyun. Yalnızca ' Azure rol tabanlı erişim denetimi ' izin modelini kullanan anahtar kasaları için geçerlidir. | 4633458b-17de-408A-b874-0445c86b69e6 |

Azure yerleşik rol tanımları hakkında daha fazla bilgi için bkz. [Azure yerleşik rolleri](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).

## <a name="using-azure-rbac-secret-key-and-certificate-permissions-with-key-vault"></a>Azure RBAC gizli anahtarı, anahtar ve sertifika izinlerini Key Vault ile kullanma

Anahtar Kasası için yeni Azure RBAC izin modeli, kasa erişimi ilkesi izin modeli için alternatif sağlar. 

### <a name="prerequisites"></a>Ön koşullar

Rol atamaları eklemek için şunları yapmanız gerekir:

- Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
- `Microsoft.Authorization/roleAssignments/write`ve `Microsoft.Authorization/roleAssignments/delete` [Kullanıcı erişimi Yöneticisi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#user-access-administrator) veya [sahibi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) gibi izinler

### <a name="enable-azure-rbac-permissions-on-key-vault"></a>Key Vault Azure RBAC izinlerini etkinleştirme

> [!IMPORTANT]
> Azure RBAC izin modelinin ayarlanması tüm erişim ilkeleri izinlerini geçersiz kılar. Eşdeğer Azure rolleri atanmadığı zaman kesintilere neden olabilir.

1.  Yeni Anahtar Kasası 'nda Azure RBAC izinlerini etkinleştirin:

    ![RBAC izinlerini etkinleştir-yeni kasa](../media/rbac/image-1.png)

2.  Mevcut anahtar kasasında Azure RBAC izinlerini etkinleştirin:

    ![RBAC izinlerini etkinleştir-mevcut kasa](../media/rbac/image-2.png)

### <a name="assign-role"></a>Rol atama

> [!Note]
> Betiklerdeki rol adı yerine benzersiz rol KIMLIĞI kullanılması önerilir. Bu nedenle, bir rol yeniden adlandırılırsa, komut dosyalarınız çalışmaya devam eder. Önizleme süresince her rolün "(Önizleme)" soneki olur ve bu, daha sonra kaldırılacaktır. Bu belge rolü adı yalnızca okunabilirlik için kullanılır.

Rol ataması oluşturmak için Azure CLı komutu:

```azurecli
az role assignment create --role <role_name_or_id> --assignee <assignee> --scope <scope>
```

Azure portal, Azure rol atamaları ekranı, erişim denetimi (ıAM) sekmesindeki tüm kaynaklar için kullanılabilir.

![Rol ataması-(ıAM) sekmesi](../media/rbac/image-3.png)

### <a name="resource-group-scope-role-assignment"></a>Kaynak grubu kapsamı rol ataması

1.  Anahtar Kasası kaynak grubu ' na gidin.
    ![Rol ataması-kaynak grubu](../media/rbac/image-4.png)

2.  Erişim denetimi (ıAM) \> rol atama Ekle ' ye \> tıklayın

3.  Geçerli Kullanıcı için "Key Vault okuyucu (Önizleme)" Key Vault okuyucu rolü oluştur

    ![Rol Ekle-kaynak grubu](../media/rbac/image-5.png)

Azure CLı:
```azurecli
az role assignment create --role "Key Vault Reader (preview)" --assignee {i.e user@microsoft.com} --scope /subscriptions/{subscriptionid}/resourcegroups/{resource-group-name}
```

Yukarıdaki rol ataması, Anahtar Kasası 'nda Anahtar Kasası nesnelerini listeleyebilme olanağı sağlar.

### <a name="key-vault-scope-role-assignment"></a>Kapsam rolü atamasını Key Vault

1. Key Vault \> Access Control (IAM) sekmesine git

2. Rol atama Ekle ' ye tıklayın. \>

3. Geçerli Kullanıcı için anahtar gizli bilgileri "Key Vault gizli Müdürü (Önizleme)" oluşturun.

    ![Rol ataması-Anahtar Kasası](../media/rbac/image-6.png)

 Azure CLı:

```azurecli
az role assignment create --role "Key Vault Secrets Officer (preview)" --assignee {i.e jalichwa@microsoft.com} --scope /subscriptions/{subscriptionid}/resourcegroups/{resource-group-name}/providers/Microsoft.KeyVault/vaults/{key-vault-name}
```

Yukarıdaki rol atamasını oluşturduktan sonra, gizli dizileri oluşturabilir/güncelleştirebilir/silebilirsiniz.

4. \>Gizli dizi düzeyi rol atamasını test etmek için yeni gizli dizi (gizlilikler + Generate/Import) oluşturun.

    ![Rol-Anahtar Kasası Ekle](../media/rbac/image-7.png)

### <a name="secret-scope-role-assignment"></a>Gizli dizi kapsamı rol ataması

1. Önceden oluşturulmuş gizli dizilerle birini açın, bildirim genel bakış ve erişim denetimi (ıAM) (Önizleme)

2. Erişim denetimi (ıAM) (Önizleme) sekmesine tıklayın

    ![Rol ataması-gizli](../media/rbac/image-8.png)

3. Geçerli Kullanıcı için "Key Vault gizli Müdürü (Önizleme)" adlı anahtar gizli bilgileri, Key Vault için yukarıda yapıldığından aynı şekilde oluşturun.

Azure CLı:

```azurecli
az role assignment create --role "Key Vault Secrets Officer (preview)" --assignee {i.e user@microsoft.com} --scope /subscriptions/{subscriptionid}/resourcegroups/{resource-group-name}/providers/Microsoft.KeyVault/vaults/{key-vault-name}/secrets/RBACSecret
```

### <a name="test-and-verify"></a>Test ve doğrulama

> [!NOTE]
> Tarayıcılar, rol atamalarını kaldırdıktan sonra önbelleğe alma ve sayfa yenilemeyi kullanır.<br>
> Rol atamalarının yenilenmesi için birkaç dakika bekleyin

1. Anahtar Kasası düzeyinde "Key Vault gizli müdür" rolü olmadan yeni gizli dizi eklemeyi doğrulayın.

Anahtar Kasası erişim denetimi (ıAM) sekmesine gidin ve bu kaynak için "Key Vault gizli Müdürü (Önizleme)" rol atamasını kaldırın.

![Atama anahtarı kasasını kaldır](../media/rbac/image-9.png)

Önceden oluşturulmuş gizli dizi ' a gidin. Tüm gizli özellikleri görebilirsiniz.

![Access ile gizli dizi görünümü](../media/rbac/image-10.png)

Yeni gizli dizi oluştur (gizlilikler \> + Generate/Import) aşağıdaki hatayı göstermelidir:

   ![Yeni gizli dizi oluştur](../media/rbac/image-11.png)

2.  Gizli anahtar üzerinde "Key Vault gizli müdür" rolü olmadan gizli düzenlemenin doğrulanması.

-   Önceden oluşturulan gizli dizi Access Control (ıAM) (Önizleme) sekmesine gidin ve bu kaynak için "Key Vault gizli bilgiler Müdürü (Önizleme)" rol atamasını kaldırın.

-   Önceden oluşturulmuş gizli dizi ' a gidin. Gizli dizi özelliklerini görebilirsiniz.

   ![Erişim olmadan gizli görünüm](../media/rbac/image-12.png)

3. Anahtar Kasası düzeyinde, okuyucu rolü olmadan gizli dizileri okumayı doğrulayın.

-   Anahtar Kasası kaynak grubu erişim denetimi (ıAM) sekmesine gidin ve "Key Vault okuyucu (Önizleme)" rol atamasını kaldırın.

-   Anahtar kasasının gizli anahtarı sekmesine gidilirken şu hata gösterilmelidir:

   ![Gizli sekme-hata](../media/rbac/image-13.png)

### <a name="creating-custom-roles"></a>Özel roller oluşturma 

[az role Definition Create komutu](https://docs.microsoft.com/cli/azure/role/definition#az-role-definition-create)

**(CLı Bash betiği)</br>**
```azurecli
az role definition create --role-definition '{ \
   "Name": "Backup Keys Operator", \
   "Description": "Perform key backup/restore operations", \
    "Actions": [ 
    ], \
    "DataActions": [ \
        "Microsoft.KeyVault/vaults/keys/read ", \
        "Microsoft.KeyVault/vaults/keys/backup/action", \
         "Microsoft.KeyVault/vaults/keys/restore/action" \
    ], \
    "NotDataActions": [ 
   ], \
    "AssignableScopes": ["/subscriptions/{subscriptionId}"] \
}'
```

Özel roller oluşturma hakkında daha fazla bilgi için bkz.:

[Özel Azure rolleri](https://docs.microsoft.com/azure/role-based-access-control/custom-roles)

## <a name="known-limits-and-performance"></a>Bilinen sınırlar ve performans

-   Abonelik başına 2000 Azure rol ataması

-   Rol atamaları gecikmesi: rolün uygulanması için rol atamaları değiştirildikten sonra en fazla 10 dakika (600 saniye) sürer

## <a name="learn-more"></a>Daha fazla bilgi edinin

- [Azure RBAC genel bakış](https://docs.microsoft.com/azure/role-based-access-control/overview)
- [Özel roller öğreticisi](https://docs.microsoft.com/azure/role-based-access-control/tutorial-custom-role-cli)
