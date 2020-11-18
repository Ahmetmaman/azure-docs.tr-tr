---
title: Blok Blobu depolama hesabı oluşturma-Azure depolama | Microsoft Docs
description: Premium performans özellikleriyle Azure BlockBlobStorage hesabının nasıl oluşturulacağını gösterir.
author: tamram
services: storage
ms.service: storage
ms.topic: how-to
ms.date: 10/30/2020
ms.author: tamram
ms.subservice: blobs
ms.custom: devx-track-azurepowershell, devx-track-azurecli
ms.openlocfilehash: 65a60425f09e9cd7a3e06ca1624621ed8b9fdcbd
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2020
ms.locfileid: "94842082"
---
# <a name="create-a-blockblobstorage-account"></a>BlockBlobStorage hesabı oluşturma

BlockBlobStorage hesap türü, Premium performans özellikleriyle blok Blobları oluşturmanızı sağlar. Bu depolama hesabı türü, yüksek işlem ücretleri olan veya çok hızlı erişim süreleri gerektiren iş yükleri için iyileştirilmiştir. Bu makalede Azure portal, Azure CLı veya Azure PowerShell kullanarak bir blok Blobstorage hesabının nasıl oluşturulacağı gösterilmektedir.

BlockBlobStorage hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](https://docs.microsoft.com/azure/storage/common/storage-account-overview).

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

# <a name="portal"></a>[Portal](#tab/azure-portal)

Yok.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Bu nasıl yapılır makalesi Azure PowerShell modülünü az Version 1.2.0 veya üstünü gerektirir. Geçerli sürümünüzü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-Az-ps).

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure 'da oturum açabilir ve Azure CLı komutlarını iki şekilde çalıştırabilirsiniz:

- CLı komutlarını Azure Cloud Shell Azure portal içinden çalıştırabilirsiniz.
- CLı 'yı yükleyebilir ve CLı komutlarını yerel olarak çalıştırabilirsiniz.

### <a name="use-azure-cloud-shell"></a>Azure Cloud Shell kullanma

Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLı, önceden yüklenmiş ve hesabınızla kullanılmak üzere yapılandırılmış. Azure portal sağ üst kısmındaki menüdeki **Cloud Shell** düğmesine tıklayın:

[![Cloud Shell](../common/media/storage-quickstart-create-account/cloud-shell-menu.png)](https://portal.azure.com)

Düğme, bu nasıl yapılır makalesinde özetlenen adımları çalıştırmak için kullanabileceğiniz etkileşimli bir kabuk başlatır:

[![Portalda Cloud Shell penceresini gösteren ekran görüntüsü](../common/media/storage-quickstart-create-account/cloud-shell.png)](https://portal.azure.com)

### <a name="install-the-cli-locally"></a>CLI’yi yerel olarak yükleme

Ayrıca, Azure CLI’yi yerel olarak yükleyip kullanabilirsiniz. Bu nasıl yapılır makalesi, Azure CLı sürüm 2.0.46 veya üstünü çalıştırıyor olmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

---

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

# <a name="portal"></a>[Portal](#tab/azure-portal)

[Azure portalında](https://portal.azure.com) oturum açın.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Komutuyla Azure aboneliğinizde oturum açın `Connect-AzAccount` ve kimlik doğrulaması yapmak için ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure Cloud Shell başlatmak için [Azure Portal](https://portal.azure.com)oturum açın.

CLı 'nın yerel yüklemesinde oturum açmak için [az Login](/cli/azure/reference-index#az-login) komutunu çalıştırın:

```azurecli
az login
```

---

## <a name="create-a-blockblobstorage-account"></a>BlockBlobStorage hesabı oluşturma

## <a name="portal"></a>[Portal](#tab/azure-portal)
Azure portal bir blok Blobstorage hesabı oluşturmak için aşağıdaki adımları izleyin:

1. Azure portal **depolama** kategorisi > depolama **hesapları**> **tüm hizmetler** ' i seçin.

2. **Depolama hesapları** altında **Ekle**' yi seçin.

3. **Abonelik** alanında, depolama hesabının oluşturulacağı aboneliği seçin.

4. **Kaynak grubu** alanında, var olan bir kaynak grubunu seçin veya **Yeni oluştur**' u seçin ve yeni kaynak grubu için bir ad girin.

5. **Depolama hesabı adı** alanına hesap için bir ad girin. Aşağıdaki yönergeleri dikkate alın:

   - AD Azure genelinde benzersiz olmalıdır.
   - Ad, üç ve 24 karakter uzunluğunda olmalıdır.
   - Ad yalnızca rakamlar ve küçük harfler içerebilir.

6. **Konum** alanında, depolama hesabı için bir konum seçin veya varsayılan konumu kullanın.

7. Ayarların geri kalanı için aşağıdakileri yapılandırın:

   |Alan     |Değer  |
   |---------|---------|
   |**Performans**    |  **Premium**' u seçin.   |
   |**Hesap türü**    | **Blockblobstorage**' ı seçin.      |
   |**Çoğaltma**    |  **Yerel olarak yedekli depolama (LRS)** varsayılan ayarını bırakın.      |

   ![Bir Blok Blobu depolama hesabı oluşturmak için Portal Kullanıcı arabirimini gösterir](media/storage-blob-create-account-block-blob/create-block-blob-storage-account.png)

8. **Gelişmiş** sekmesini seçin.

9. Veri analizi için depolama hesabınızı iyileştirmek isterseniz, **hiyerarşik ad alanını** **etkin** olarak ayarlayın. Aksi takdirde, bu seçeneği varsayılan değerine ayarlayın. BlockBlobStorage hesabınızla bu ayarı etkinleştirmek, [Data Lake Storage için Premium katman](premium-tier-for-data-lake-storage.md)sağlar.  Data Lake Storage hakkında daha fazla bilgi edinmek için bkz. [Azure Data Lake Storage 2. giriş](data-lake-storage-introduction.md).

8. Depolama hesabı ayarlarını gözden geçirmek için **gözden geçir + oluştur** ' u seçin.

9. **Oluştur**’u seçin.

## <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

1. Yükseltilmiş bir Windows PowerShell oturumu açın (yönetici olarak çalıştır).

2. PowerShell modülünün en son sürümünün yüklü olduğundan emin olmak için aşağıdaki komutu çalıştırın `Az` .

   ```powershell
   Install-Module -Name Az -AllowClobber
   ```

3. Yeni bir PowerShell konsolu açın ve Azure hesabınızla oturum açın.

   ```powershell
   Connect-AzAccount -SubscriptionId <SubscriptionID>
   ```

4. Gerekirse, yeni bir kaynak grubu oluşturun. Tekliflerdeki değerleri değiştirin ve aşağıdaki komutu çalıştırın.

   ```powershell
   $resourcegroup = "new_resource_group_name"
   $location = "region_name"
   New-AzResourceGroup -Name $resourceGroup -Location $location
   ```

5. BlockBlobStorage hesabını oluşturun. Tekliflerdeki değerleri değiştirin ve aşağıdaki komutu çalıştırın.

   ```powershell
   $resourcegroup = "resource_group_name"
   $storageaccount = "new_storage_account_name"
   $location = "region_name"

   New-AzStorageAccount -ResourceGroupName $resourcegroup -Name $storageaccount -Location $location -Kind "BlockBlobStorage" -SkuName "Premium_LRS"
   ```
   Veri analizi için depolama hesabınızı iyileştirmek istiyorsanız `-EnableHierarchicalNamespace $True` komuta ekleyin. BlockBlobStorage hesabınızla bu ayarı etkinleştirmek, [Data Lake Storage için Premium katman](premium-tier-for-data-lake-storage.md)sağlar.  Data Lake Storage hakkında daha fazla bilgi edinmek için bkz. [Azure Data Lake Storage 2. giriş](data-lake-storage-introduction.md).

## <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı kullanarak bir Blok Blobu hesabı oluşturmak için önce Azure CLı v 'yi yüklemeniz gerekir. 2.0.46 veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

1. Azure aboneliğinizde oturum açın.

   ```azurecli
   az login
   ```

2. Gerekirse, yeni bir kaynak grubu oluşturun. Köşeli ayraçlar içindeki değerleri (köşeli ayraç dahil) değiştirin ve aşağıdaki komutu çalıştırın.

   ```azurecli
   az group create \
    --name "<new_resource_group_name>" \
    --location "<location>"
   ```

3. BlockBlobStorage hesabını oluşturun. Köşeli ayraçlar içindeki değerleri (köşeli ayraç dahil) değiştirin ve aşağıdaki komutu çalıştırın.

   ```azurecli
   az storage account create \
    --location "<location>" \
    --name "<new_storage_account_name>" \
    --resource-group "<resource_group_name>" \
    --kind "BlockBlobStorage" \
    --sku "Premium_LRS"
   ```

   Veri analizi için depolama hesabınızı iyileştirmek istiyorsanız `--hierarchical-namespace true` komuta ekleyin. BlockBlobStorage hesabınızla bu ayarı etkinleştirmek, [Data Lake Storage için Premium katman](premium-tier-for-data-lake-storage.md)sağlar.  Data Lake Storage hakkında daha fazla bilgi edinmek için bkz. [Azure Data Lake Storage 2. giriş](data-lake-storage-introduction.md).

---

## <a name="next-steps"></a>Sonraki adımlar

- Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](https://docs.microsoft.com/azure/storage/common/storage-account-overview).

- Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
