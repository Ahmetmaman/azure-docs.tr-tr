---
title: Azure portalı - Azure Batch bir hesabı oluşturma | Microsoft Docs
description: Büyük ölçekli paralel iş yükleri bulutta çalıştırmak için Azure portalda bir Azure Batch hesabı oluşturmayı öğrenin
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: 3fbae545-245f-4c66-aee2-e25d7d5d36db
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/25/2019
ms.author: lahugh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 04631431c03f6fdd378bfa99edb9b67f8d6a0cad
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56193926"
---
# <a name="create-a-batch-account-with-the-azure-portal"></a>Azure portalıyla Batch hesabı oluşturma

[Azure portalında][azure_portal] Azure Batch hesabı oluşturma hakkında bilgi alın ve işlem senaryonuza uygun hesap özelliklerini seçin. Erişim anahtarları ve hesap URL’leri gibi önemli hesap özelliklerini nerede bulabileceğinizi öğrenin.

Batch hesapları ve senaryoları hakkında arka plan bilgileri için bkz. [özelliğe genel bakış](batch-api-basics.md).

## <a name="create-a-batch-account"></a>Batch hesabı oluşturma

[!INCLUDE [batch-account-mode-include](../../includes/batch-account-mode-include.md)]

1. [Azure portalında][azure_portal] oturum açın.

1. **Kaynak oluştur** > **İşlem** > **Batch Hizmeti**'ni seçin.

    ![Market’te Batch][marketplace_portal]

1. **Yeni Batch hesabı** ayarlarını girin. Aşağıdaki ayrıntılara bakın.

    ![Batch hesabı oluşturma][account_portal]

    a. **Hesap adı**: Seçtiğiniz ad hesabın oluşturulduğu Azure bölgesi içinde benzersiz olması gerekir (bkz **konumu** aşağıda). Hesap adı yalnızca küçük harfler, sayılar içerebilir ve 3-24 karakter uzunluğunda olmalıdır.

    b. **Abonelik**: Batch hesabının oluşturulacağı bir abonelik. Yalnızca bir aboneliğiniz varsa, varsayılan olarak seçilidir.

    c. **Kaynak grubu**: Yeni Batch hesabınız için mevcut bir kaynak grubunu seçin ya da isteğe bağlı olarak yeni bir tane oluşturun.

    d. **Konum**: Batch hesabının oluşturulacağı Azure bölgesi. Yalnızca aboneliğiniz ve kaynak grubunuz tarafından desteklenen bölgeler seçenek olarak görüntülenir.

    e. **Depolama hesabı** (isteğe bağlı): Batch hesabınızla ilişkilendireceğiniz bir Azure depolama hesabı. Çoğu Batch hesabı için önerilen seçenek budur. Batch’teki depolama hesabı seçenekleri için bkz. [Batch özelliğine genel bakış](batch-api-basics.md#azure-storage-account). Portalda mevcut bir depolama hesabını seçin veya isteğe bağlı olarak yeni bir depolama hesabı oluşturun.

      ![Depolama hesabı oluşturma][storage_account]

    f. **Havuz ayırma modu**: Çoğu senaryo için varsayılan değerleri kabul **Batch hizmeti**.

1. Hesabı oluşturmak için **Oluştur**'u seçin.

## <a name="view-batch-account-properties"></a>Batch hesabı özelliklerini görüntüleme

Hesap oluşturulduktan sonra seçerek ayarlarına ve özelliklerine erişebilirsiniz. Sol menüyü kullanarak tüm hesap ayarlarına ve özelliklerine erişebilirsiniz.

![Azure portalında Batch hesabı sayfası][account_blade]

* **Batch hesabı adı, URL ve anahtarları**: İle bir uygulama geliştirirken [Batch API'leri](batch-apis-tools.md#azure-accounts-for-batch-development), Batch kaynaklarınıza erişebilmeniz için bir hesap URL'si ve anahtar gerekir. (Batch, Azure Active Directory kimlik doğrulamasını da destekler.)

    Batch hesabı erişim bilgilerini görüntülemek için **Anahtarlar**'ı seçin.

    ![Azure portalında Batch hesabı anahtarları][account_keys]

* Batch hesabınızla ilişkili depolama hesabının adını ve anahtarlarını görüntülemek için **Depolama hesabı**'nı seçin.

* Batch hesabı için geçerli olan kaynak kotalarını görüntülemek için **Kotalar**'ı seçin. Ayrıntılar için bkz. [Batch hizmeti kotaları ve limitleri](batch-quota-limit.md).

## <a name="additional-configuration-for-user-subscription-mode"></a>Kullanıcı aboneliği modu için ek yapılandırma

Kullanıcı aboneliği modunda bir Batch hesabı oluşturmayı seçerseniz, hesabı oluşturmadan önce aşağıdaki ek adımları gerçekleştirin.

### <a name="allow-azure-batch-to-access-the-subscription-one-time-operation"></a>Azure Batch hizmetinin aboneliğe erişmesine izin verme (tek seferlik işlem)

Kullanıcı aboneliği modunda ilk Batch hesabınızı oluştururken, aboneliğinizi Batch hizmetine kaydetmeniz gerekir. (Bunu daha önce yaptıysanız sonraki bölüme atlayın.)

1. [Azure portalında][azure_portal] oturum açın.

1. **Tüm hizmetler** > **Abonelikler**'i ve ardından Batch hesabı için kullanmak istediğiniz aboneliği seçin.

1. **Abonelik** sayfasında **Kaynak sağlayıcıları**'nı seçip **Microsoft.Batch**'i arayın. **Microsoft.Batch** kaynak sağlayıcısının aboneliğe kayıtlı olup olmadığını kontrol edin. Kayıtlı değilse **Kaydet** bağlantısını seçin.

    ![Microsoft.Batch sağlayıcısını kaydetme][register_provider]

1. İçinde **abonelik** sayfasında **erişim denetimi (IAM)** > **rol atamaları** > **RolatamasıEkle**.

    ![Abonelik erişim denetimi][subscription_access]

1. Üzerinde **rol ataması Ekle** sayfasında **katkıda bulunan** rolü, Batch API'sini arayın. API'yi bulana kadar aşağıdaki dizelerden her birini arayın:
    1. **MicrosoftAzureBatch**.
    1. **Microsoft Azure Batch**. Daha yeni Azure AD kiracıları bu adı kullanıyor olabilir.
    1. Batch API'sinin kimliği: **ddbf3205-c6bd-46ae-8127-60eb93363864**.

1. Batch API'sini bulduktan sonra seçin ve **Kaydet**'i seçin.

    ![Batch izinleri ekleme][add_permission]

### <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Kullanıcı aboneliği modunda, oluşturulacak Batch hesabı ile aynı kaynak grubuna ait olan bir Azure key vault gereklidir. Kaynak grubunun, Batch hizmetinin [mevcut](https://azure.microsoft.com/regions/services/) olduğu ve aboneliğinizin desteklediği bir bölgede olduğundan emin olun.

1. [Azure portalında][azure_portal] **Yeni** > **Güvenlik** > **Anahtar Kasası**'nı seçin.

1. **Key Vault Oluştur** sayfasında anahtar kasası için bir ad girin ve Batch hesabınız için istediğiniz bölgede bir kaynak grubu oluşturun. Kalan ayarları varsayılan değerlerinde bırakın ve ardından **Oluştur**'u seçin.

Batch hesabını kullanıcı aboneliği modunda oluştururken, anahtar kasasının kaynak grubunu kullanın, havuz ayırma modu olarak **Kullanıcı aboneliği**’ni belirtin ve anahtar kasasını seçin.

### <a name="configure-subscription-quotas"></a>Abonelik kotaları yapılandırın

Çekirdek kotaları kullanıcı abonelik Batch hesaplarında varsayılan olarak ayarlı değil. Hesaplara kullanıcı aboneliği modunda Batch çekirdek kotaları tipik uygulanmadıkları çekirdek kotaları el ile ayarlamanız gerekir.

1. İçinde [Azure portalında][azure_portal], kendi kullanıcı aboneliği modunda Batch hesabı ayarlarını ve özelliklerini görüntülemek için seçin.

1. Sol menüden **kotalar** görüntülemek ve Batch hesabınızla ilişkili çekirdek kotaları yapılandırmak için.

Başvurmak [Batch hizmet kotaları ve limitleri](batch-quota-limit.md) kullanıcı aboneliği modu çekirdek kotaları hakkında daha fazla bilgi için.

## <a name="other-batch-account-management-options"></a>Diğer Batch hesabı yönetim seçenekleri

Azure portalını kullanmaya ek olarak Batch hesaplarını aşağıdaki gibi araçlarla oluşturup yönetebilirsiniz:

* [Batch PowerShell cmdlet’leri](batch-powershell-cmdlets-get-started.md)
* [Azure CLI](batch-cli-get-started.md)
* [Batch Yönetimi .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Sonraki adımlar

* Batch hizmeti kavramları ve özellikler hakkında daha fazla bilgi edinmek için bkz. [Batch özelliklerine genel bakışı](batch-api-basics.md). Makale havuzlar, işlem düğümleri, işler ve görevler gibi birincil Batch kaynaklarını ele alır ve büyük ölçekli işlem iş yüklerine olanak tanıyan hizmetin özelliklerine genel bir bakış sağlar.
* [Batch .NET istemci kitaplığı](quick-run-dotnet.md) veya [Python](quick-run-python.md) kullanarak Batch özellikli bir uygulama geliştirmenin temellerini öğrenin. Bu hızlı başlangıçlar, bir iş yükünü birden fazla işlem düğümünde yürütmek üzere Batch hizmetini kullanan örnek uygulamalar konusunda size rehberlik sağlamanın yanı sıra, iş yükü dosyası hazırlama ve alma işlemleri için Azure Depolama kullanma ile ilgili bilgiler de içerir.

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[marketplace_portal]: ./media/batch-account-create-portal/marketplace-batch.png
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch-account-portal.png
[account_keys]: ./media/batch-account-create-portal/batch-account-keys.png
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[subscription_access]: ./media/batch-account-create-portal/subscription_iam.png
[add_permission]: ./media/batch-account-create-portal/add_permission.png
[register_provider]: ./media/batch-account-create-portal/register_provider.png