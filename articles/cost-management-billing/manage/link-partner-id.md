---
title: Azure hesabını iş ortağı kimliğine bağlama
description: İş ortağı kimliğini, müşterinin kaynaklarını yönetmek için kullandığınız kullanıcı hesabına bağlayarak Azure müşterileriyle etkileşimleri izleyin.
author: dhirajgandhi
ms.reviewer: dhgandhi
ms.author: banders
ms.date: 10/05/2020
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.openlocfilehash: a214e91307308e191ce92b6461c1454d2cc7dd2b
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100370487"
---
# <a name="link-a-partner-id-to-your-azure-accounts"></a>İş ortağı kimliğini Azure hesaplarınıza bağlama

Microsoft iş ortakları, müşterilerin Microsoft ürünlerini kullanarak iş ve görev hedeflerini başarmasına yardımcı olan hizmetler sağlar. Azure hizmetlerini yöneten, yapılandıran ve destekleyen müşteri adına hareket ederken iş ortağı kullanıcılarının müşterinin ortamına erişmesi gerekir. İş ortakları, İş Ortağı Yönetici Bağlantısını (PAL) kullanarak iş ortağı ağı kimliğini, hizmet teslimi için kullanılan kimlik bilgileriyle ilişkilendirebilir.

PAL, Microsoft’un Azure müşteri başarısını artıran iş ortaklarını belirlemesine ve tanımasına olanak tanır. Microsoft, hesabın izinleri (Azure rolü) ve kapsamı (abonelik, kaynak grubu, kaynak) temelinde etkiyi ve Azure’ın tüketim gelirini kuruluşunuzla ilişkilendirebilir.

## <a name="get-access-from-your-customer"></a>Müşterinizden erişim elde etme

Siz iş ortağı kimliğinizi bağlamadan önce müşteriniz aşağıdaki seçeneklerden birini kullanarak Azure kaynaklarına erişmenizi sağlamalıdır:

- **Konuk kullanıcı**: Müşteriniz, sizi konuk kullanıcı olarak ekleyip Azure rolleri atayabilir. Daha fazla bilgi için bkz. [Başka bir dizinden konuk kullanıcılar ekleme](../../active-directory/external-identities/what-is-b2b.md).

- **Dizin hesabı**: Müşteriniz, kendi dizininde sizin için bir kullanıcı hesabı oluşturabilir ve Azure rolü atayabilir.

- **Hizmet sorumlusu**: Müşteriniz, kendi dizininde kuruluşunuzdan bir uygulama veya betik ekleyebilir ve Azure rolü atayabilir. Uygulamanın veya betiğin kimliği, hizmet sorumlusu olarak bilinir.

- **Azure Lighthouse**: Müşteriniz bir aboneliği (veya kaynak grubunu) temsilci olarak atayarak kullanıcılarınızın kiracınızda bunun üzerinde çalışmasını sağlayabilir. Daha fazla bilgi için bkz. [Azure’da atanan kaynak yönetimi](../../lighthouse/concepts/azure-delegated-resource-management.md).

## <a name="link-to-a-partner-id"></a>İş ortağı kimliğine bağlantı

Müşterinin kaynaklarına erişiminiz olduğunda, Microsoft İş Ortağı Ağı Kimliğinizi (MPN Kimliği) kullanıcı kimliğinize veya hizmet sorumlunuza bağlamak için Azure portalı, PowerShell ya da Azure CLI’yı kullanın. Her müşteri kiracısında iş ortağı kimliğini bağlayın.

### <a name="use-the-azure-portal-to-link-to-a-new-partner-id"></a>Yeni bir iş ortağı kimliğine bağlantı için Azure portalını kullanma

1. Azure portalında [İş ortağı kimliğine bağlantı](https://portal.azure.com/#blade/Microsoft_Azure_Billing/managementpartnerblade) bölümüne gidin.

2. Azure Portal’da oturum açın.

3. Microsoft iş ortağı kimliğini girin. İş ortağı kimliği, kuruluşunuzun [Microsoft İş Ortağı Ağı](https://partner.microsoft.com/) kimliğidir. İş ortağı profilinizde gösterilen **İlişkili MPN kimliğini** kullandığınızdan emin olun.

   ![İş ortağı kimliğine bağlantıyı gösteren ekran görüntüsü](./media/link-partner-id/link-partner-id01.png)

4. Başka bir müşterinin iş ortağı kimliğine bağlantı için dizini değiştirin. **Dizini değiştir** bölümünde dizininizi seçin.

   ![Dizini değiştir seçeneğini gösteren ekran görüntüsü](./media/link-partner-id/directory-switcher.png)

### <a name="use-powershell-to-link-to-a-new-partner-id"></a>Yeni bir iş ortağı kimliğine bağlanmak için PowerShell’i kullanma

1. [Az.ManagementPartner](https://www.powershellgallery.com/packages/Az.ManagementPartner/) PowerShell modülünü yükleyin.

2. Kullanıcı hesabı veya hizmet sorumlusu ile müşterinin kiracısında oturum açın. Daha fazla bilgi için bkz [PowerShell ile oturum açma](/powershell/azure/authenticate-azureps).

   ```azurepowershell-interactive
    C:\> Connect-AzAccount -TenantId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
   ```

3. Yeni iş ortağı kimliğine bağlantı oluşturun. İş ortağı kimliği, kuruluşunuzun [Microsoft İş Ortağı Ağı](https://partner.microsoft.com/) kimliğidir. İş ortağı profilinizde gösterilen **İlişkili MPN kimliğini** kullandığınızdan emin olun.


    ```azurepowershell-interactive
    C:\> new-AzManagementPartner -PartnerId 12345
    ```

#### <a name="get-the-linked-partner-id"></a>Bağlı iş ortağı kimliğini alma
```azurepowershell-interactive
C:\> get-AzManagementPartner
```

#### <a name="update-the-linked-partner-id"></a>Bağlı iş ortağı kimliğini güncelleştirme
```azurepowershell-interactive
C:\> Update-AzManagementPartner -PartnerId 12345
```
#### <a name="delete-the-linked-partner-id"></a>Bağlı iş ortağı kimliğini silme
```azurepowershell-interactive
C:\> remove-AzManagementPartner -PartnerId 12345
```

### <a name="use-the-azure-cli-to-link-to-a-new-partner-id"></a>Yeni bir iş ortağı kimliğine bağlantı için Azure CLI’yı kullanma
1. Azure CLI uzantısını yükleyin.

    ```azurecli-interactive
    C:\ az extension add --name managementpartner
    ```

2. Kullanıcı hesabı veya hizmet sorumlusu ile müşterinin kiracısında oturum açın. Daha fazla bilgi için bkz. [Azure CLI ile oturum açma](/cli/azure/authenticate-azure-cli).

    ```azurecli-interactive
    C:\ az login --tenant <tenant>
    ```

3. Yeni iş ortağı kimliğine bağlantı oluşturun. İş ortağı kimliği, kuruluşunuzun [Microsoft İş Ortağı Ağı](https://partner.microsoft.com/) kimliğidir.

     ```azurecli-interactive
     C:\ az managementpartner create --partner-id 12345
      ```  

#### <a name="get-the-linked-partner-id"></a>Bağlı iş ortağı kimliğini alma
```azurecli-interactive
C:\ az managementpartner show
```

#### <a name="update-the-linked-partner-id"></a>Bağlı iş ortağı kimliğini güncelleştirme
```azurecli-interactive
C:\ az managementpartner update --partner-id 12345
```

#### <a name="delete-the-linked-partner-id"></a>Bağlı iş ortağı kimliğini silme
```azurecli-interactive
C:\ az managementpartner delete --partner-id 12345
```

## <a name="next-steps"></a>Sonraki adımlar

Güncelleştirmeleri almak veya geri bildirim göndermek için [Microsoft İş Ortağı Topluluğu](https://aka.ms/PALdiscussion)’ndaki tartışmaya katılın.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**İş ortağı kimliğini kim bağlayabilir?**

Müşterinin Azure kaynaklarını yöneten iş ortağı kuruluşundaki tüm kullanıcılar, iş ortağı kimliğini hesaba bağlayabilir.

**Bir iş ortağı kimliği bağlandıktan sonra değiştirilebilir mi?**

Evet. Bağlı bir iş ortağı kimliği değiştirilebilir, eklenebilir veya kaldırılabilir.

**Bir kullanıcının birden fazla müşteri kiracısında hesabı varsa ne olur?**

Her müşteri kiracısı için iş ortağı kimliği ve hesap arasında bağlantı oluşturulur. Her müşteri kiracısında iş ortağı kimliğini bağlayın.

Ancak, Azure Lighthouse üzerinden müşteri kaynaklarını yönetiyorsanız müşteri kaynaklarına erişimi olan bir hesabı kullanarak hizmet sağlayıcı kiracınızda bağlantı oluşturmanız gerekir. Daha fazla bilgi için bkz. [İş ortağı kimliğinizi, temsilci atanan kaynaklardaki etkinizi izleyecek şekilde bağlama](../../lighthouse/how-to/partner-earned-credit.md).

**Diğer iş ortakları veya müşteriler, iş ortağı kimliğine bağlantıyı düzenleyebilir ya da kaldırabilir mi?**

Bağlantı, kullanıcı hesabı düzeyinde ilişkilendirilir. Yalnızca siz iş ortağı kimliğine bağlantıyı düzenleyebilir veya kaldırabilirsiniz. Müşteri ve diğer iş ortakları, iş ortağı kimliğine bağlantıyı değiştiremez.

**Şirketimin birden çok MPN kimliği varsa hangisini kullanmalıyım?**

İş ortağı profilinizde gösterilen **İlişkili MPN kimliğini** kullandığınızdan emin olun.

**Bağlı iş ortağı kimliği için etkilenen gelir raporlamasını nereden bulabilirim?**

Bulut Ürün Performansı raporlaması, İş Ortağı Merkezinde [İçgörülerim panosunda](https://partner.microsoft.com/membership/reports/myinsights) iş ortaklarının kullanımına sunulur. İş ortağı ilişkilendirme türü olarak İş Ortağı Yönetici Bağlantısını seçmeniz gerekir.

**Neden raporlarda müşterimi göremiyorum?**

Aşağıdaki nedenlerle raporlarda müşteriyi göremezsiniz

1. Bağlantılı kullanıcı hesabının herhangi bir müşterinin Azure aboneliğinde veya kaynağında [Azure rol tabanlı erişim denetimi (Azure RBAC)](../../role-based-access-control/overview.md) yoktur.

2. Kullanıcının [Azure rol tabanlı erişim denetimi (Azure RBAC)](../../role-based-access-control/overview.md) erişimine sahip olduğu Azure aboneliğinde herhangi bir kullanım yoktur.

**İş ortağı kimliği bağlantısı, Azure Stack ile çalışır mı?**

Evet, Azure Stack için iş ortağı kimliğinizi bağlayabilirsiniz.

**Şirketim müşteri kaynaklarına erişmek için [Azure Lighthouse](../../lighthouse/overview.md) kullanıyorsa iş ortağı kimliğimi nasıl ilişkilendirebilirim?**

Azure Sthouse etkinliklerinin tanınması için, MPN KIMLIĞINIZI eklendi aboneliklerinizin her birine erişimi olan en az bir kullanıcı hesabıyla ilişkilendirmeniz gerekir. Bunu her bir müşteri kiracısında değil, hizmet sağlayıcısı kiracınızda yapmanız gerektiğini unutmayın. Kolaylık olması için kiracınızda bir hizmet sorumlusu hesabı oluşturmanızı, bunu MPN kimliğinizle ilişkilendirmenizi ve sonra [iş ortağı tarafından kazanılan krediye uygun bir Azure yerleşik rolüyle](/partner-center/azure-roles-perms-pec) eklediğiniz her müşteriye erişim vermenizi öneririz. Daha fazla bilgi için bkz. [İş ortağı kimliğinizi, temsilci atanan kaynaklardaki etkinizi izleyecek şekilde bağlama](../../lighthouse/how-to/partner-earned-credit.md).

**İş Ortağı Yönetici Bağlantısını (PAL) Müşterime nasıl açıklayabilirim?**

İş Ortağı Yönetici Bağlantısı (PAL) Microsoft'un iş hedeflerine ulaşma ve bulutta değer elde etme yönünde müşterilere yardımcı olan iş ortaklarını belirlemesini ve tanımasını sağlar. Müşterilerin önce Azure kaynağına iş ortağı erişimi sağlaması gerekir. Erişim verildikten sonra iş ortağının Microsoft İş Ortağı Ağı (MPN ID) ilişkilendirilir. Bu ilişkilendirme Microsoft'un BT hizmet sağlayıcıları ekosistemini anlamasına ve ortak müşterilerimizi en iyi şekilde desteklemek için gereken araçlarla programları iyileştirmesine yardımcı olur.

**PAL hangi verileri toplar?**

Mevcut kimlik bilgilerine PAL ilişkilendirmesi Microsoft'a yeni müşteri verileri sağlamaz. Yalnızca Microsoft'a müşterinin Azure ortamına etkin bir şekilde dahil olan iş ortağının telemetrisini sağlar. Microsoft müşteri tarafından iş ortağına sağlanan hesabın izinleri (Azure rolü) ve kapsamı (Yönetim Grubu, Abonelik, Kaynak Grubu, Kaynak) temelinde müşteri ortamından gelen etkiyi ve Azure tüketim gelirini iş ortağı kuruluşuyla ilişkilendirebilir. 

**Bu bir müşterinin Azure Ortamının güvenliğini etkiler mi?**

PAL ilişkilendirmesi yalnızca iş ortağının MPN ID değerini önceden sağlanmış kimlik bilgilerine ekler ve hiçbir izni (Azure rolü) değiştirmediği gibi iş ortağına veya Microsoft'a ek Azure hizmet verileri de sağlamaz.