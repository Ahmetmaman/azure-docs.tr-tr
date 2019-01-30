---
title: Azure kaynaklarını (Önizleme) - Azure depolama BLOB'ları ve kuyrukları ile Azure Active Directory tarafından yönetilen kimlikleri için erişimi kimlik doğrulaması | Microsoft Docs
description: Azure Blob ve kuyruk depolama, Azure kaynakları için yönetilen kimliklerle Azure Active Directory kimlik doğrulamasını destekler. Azure kaynakları için yönetilen kimlikleri, Azure sanal makineleri, işlev uygulamaları, sanal makine ölçek kümeleri ve diğerleri çalışan uygulamalardan erişim BLOB'lar ve Kuyruklar için kimlik doğrulaması için kullanabilirsiniz. Yönetilen kimlik kullanarak Azure kaynakları ve Azure AD kimlik doğrulaması gücünden yararlanan, bulutta çalışan uygulamalarınızın ile kimlik bilgilerini depolama önleyebilirsiniz.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 10/15/2018
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 15c37be3f3b1b3f72c32865e095091fa10ee9750
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55251699"
---
# <a name="authenticate-access-to-blobs-and-queues-with-managed-identities-for-azure-resources-preview"></a>BLOB'lar ve Kuyruklar yönetilen kimliklerle erişim (Önizleme) Azure kaynakları için kimlik doğrulaması

Azure Blob ve kuyruk depolama ile Azure Active Directory (Azure AD) kimlik doğrulaması desteği [kimliklerini Azure kaynakları için yönetilen](../../active-directory/managed-identities-azure-resources/overview.md). Kimlikleri yönetilen Azure kaynaklarını bloblara erişimi kimlik doğrulaması için ve Azure sanal makineleri (VM'ler), işlev uygulamaları, sanal makine ölçek kümeleri ve diğerleri çalışan uygulamalardan Azure AD kimlik bilgilerini kullanarak sıralar. Yönetilen kimlik kullanarak Azure kaynakları ve Azure AD kimlik doğrulaması gücünden yararlanan, bulutta çalışan uygulamalarınızın ile kimlik bilgilerini depolama önleyebilirsiniz.  

Yönetilen bir kimlik bir blob kapsayıcısı veya kuyruğa izinler vermek için uygun kapsamda bu kaynak için izinleri kapsayan yönetilen kimlik için rol tabanlı erişim denetimi (RBAC) rolüne atayın. Depolamadaki RBAC rolleri hakkında daha fazla bilgi için bkz. [Yönet RBAC (Önizleme) ile depolama verilere erişim hakları](storage-auth-aad-rbac.md). 

Bu makalede, Azure Blob veya kuyruk depolama ile yönetilen bir kimlik için bir Azure VM'den kimlik doğrulaması yapmayı gösterir.  

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="enable-managed-identities-on-a-vm"></a>Bir VM'de yönetilen kimlikleri etkinleştir

BLOB'lar ve Kuyruklar makinenizden kimlik doğrulaması yapmak için Azure kaynakları için yönetilen kimlikleri kullanabilmeniz için önce VM üzerindeki Azure kaynakları için önce yönetilen kimlikleri etkinleştirmeniz gerekir. Azure kaynakları için yönetilen kimlikleri etkinleştirme konusunda bilgi edinmek için şu makalelerden birine bakın:

- [Azure Portal](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure CLI](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager şablonu](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure SDK'ları](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

## <a name="assign-an-rbac-role-to-an-azure-ad-managed-identity"></a>Bir yönetilen Azure AD kimlik için bir RBAC rolü atayın

Azure Storage uygulamanızı bir yönetilen kimlik doğrulamak için önce yönetilen kimliğe için rol tabanlı erişim denetimi (RBAC) ayarlarını yapılandırın. Azure depolama kapsayıcıları ve Kuyruklar için izinleri kapsayacak RBAC rolleri tanımlar. Yönetilen kimliği bir yönetilen kimlik için RBAC rolü atandığında, bu kaynağa erişim izni verilir. Daha fazla bilgi için [RBAC (Önizleme) ile Azure Blob ve kuyruk verilere erişim haklarını yönetme](storage-auth-aad-rbac.md).

## <a name="get-a-managed-identity-access-token"></a>Yönetilen kimlik erişim belirteci alma

İle yönetilen bir kimlik doğrulamak için uygulama veya betik bir yönetilen kimlik erişim belirtecini almalıdır. Erişim belirteci alma hakkında bilgi edinmek için [bir erişim belirteci almak için bir Azure sanal makinesinde Azure kaynakları için yönetilen kimlikleri kullanmak nasıl](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md).

## <a name="net-code-example-create-a-block-blob"></a>.NET kod örneği: Bir blok blobu oluştur

Kod örneği, bir yönetilen kimlik erişim belirteci sahibi olduğunuzu varsayar. Erişim belirteci, bir blok blobu oluşturmak için yönetilen kimlik yetkilendirmek için kullanılır.

### <a name="add-references-and-using-statements"></a>Başvurular ekleyin ve using deyimleri  

Visual Studio'da Azure depolama istemci kitaplığı önizleme sürümünü yükleyin. Gelen **Araçları** menüsünde **Nuget Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. Konsolunda aşağıdaki komutu yazın:

```
Install-Package https://www.nuget.org/packages/WindowsAzure.Storage  
```

Aşağıdaki using deyimlerini kodunuz:

```dotnet
using Microsoft.WindowsAzure.Storage.Auth;
```

### <a name="create-credentials-from-the-managed-identity-access-token"></a>Yönetilen kimlik erişim belirtecinden kimlik bilgileri oluşturma

Blok blobu oluşturmak için kullanın **TokenCredentials** Önizleme paketi tarafından sağlanan sınıfı. Yeni bir örneğini oluşturmak **TokenCredentials**, daha önce edindiğiniz yönetilen kimlik erişim belirtecinde geçen:

```dotnet
// Create storage credentials from your managed identity access token.
TokenCredential tokenCredential = new TokenCredential(accessToken);
StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

// Create a block blob using the credentials.
CloudBlockBlob blob = new CloudBlockBlob(new Uri("https://storagesamples.blob.core.windows.net/sample-container/Blob1.txt"), storageCredentials);
``` 

> [!NOTE]
> Azure depolama ile Azure AD tümleştirmesi, Azure depolama işlemleri için HTTPS kullanmanızı gerektirir.

## <a name="next-steps"></a>Sonraki adımlar

- RBAC rolleri için Azure depolama hakkında daha fazla bilgi için bkz: [Yönet RBAC (Önizleme) ile depolama verilere erişim hakları](storage-auth-aad-rbac.md).
- Kapsayıcılar ve kuyruklardaki depolama uygulamalarınızda erişim yetkisi verme konusunda bilgi almak için bkz: [depolama uygulamaları ile kullanmak üzere Azure AD](storage-auth-aad-app.md).
- Azure CLI ve PowerShell ile bir Azure AD kimlikleriyle oturum öğrenmek için bkz. [CLI veya PowerShell (Önizleme) ile Azure depolamaya erişmek için bir Azure AD kimlik kullanmak](storage-auth-aad-script.md).
- Azure AD tümleştirmesi için Azure BLOB'ları ve kuyrukları hakkında ek bilgi için bkz: gönderin, Azure depolama ekibi blogu [Önizleme, Azure AD kimlik doğrulaması için Azure depolama ile tanışın](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
