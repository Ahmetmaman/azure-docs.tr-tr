---
title: 'Öğretici: Azure Cosmos DB erişmek için yönetilen bir kimlik kullanma-Windows-Azure AD'
description: Windows VM üzerinde bir sistem tarafından atanan yönetilen hizmet kimliği kullanarak Azure Cosmos DB'ye erişme işleminde size yol gösteren bir öğretici.
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2020
ms.author: barclayn
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6b571b2b8e0d334a02631e3f443ec54398117ee9
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91532678"
---
# <a name="tutorial-use-a-windows-vm-system-assigned-managed-identity-to-access-azure-cosmos-db"></a>Öğretici: Azure Cosmos DB hizmetine erişmek için Windows VM sistem tarafından atanan yönetilen kimliği kullanma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bu öğreticide, Cosmos DB'ye erişmek amacıyla, Windows sanal makinesi (VM) için sistem tarafından atanan bir yönetilen kimliği nasıl kullanacağınız gösterilmektedir. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Cosmos DB hesabı oluşturma
> * Windows VM sistem tarafından atanan yönetilen kimliğine Cosmos DB hesabı erişim anahtarları için erişim verme
> * Windows VM’nin sistem tarafından atanan yönetilen kimliğini kullanarak erişim belirteci alma ve Azure Resource Manager çağrısı yapma
> * Cosmos DB çağrıları yapmak için Azure Resource Manager'dan erişim anahtarları alma

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

- [Azure PowerShell](/powershell/azure/install-az-ps) en son sürümünü yükler


## <a name="enable"></a>Etkinleştir

[!INCLUDE [msi-tut-enable](../../../includes/active-directory-msi-tut-enable.md)]



## <a name="grant-access"></a>Erişim verme


### <a name="create-a-cosmos-db-account"></a>Cosmos DB hesabı oluşturma 

Henüz Cosmos DB hesabınız yoksa, bir hesap oluşturun. Bu adımı atlayabilir ve varolan bir Cosmos DB hesabını kullanabilirsiniz. 

1. Azure portalının sol üst köşesinde bulunan **+/Yeni hizmet oluştur** düğmesine tıklayın.
2. **Veritabanları**'na ve sonra da **Azure Cosmos DB**'ye tıklayın; yeni bir "Yeni hesap" paneli görüntülenir.
3. Cosmos DB hesabı için daha sonra kullanacağınız bir **Kimlik** girin.  
4. **API** olarak "SQL" ayarlanmalıdır. Bu öğreticide açıklanan yaklaşım varolan diğer API türleriyle kullanılabilir, ama bu öğreticideki adımlar SQL API'ye yöneliktir.
5. **Abonelik** ve **Kaynak Grubu** değerlerinin, önceki adımda VM'nizi oluştururken belirttiklerinizle eşleştiğinden emin olun.  Cosmos DB'nin kullanılabileceği **Konum**'u seçin.
6. **Oluştur**’a tıklayın.

### <a name="create-a-collection"></a>Koleksiyon oluşturma 

Ardından, Cosmos DB hesabına sonraki adımlarda sorgulayabileceğiniz bir veri koleksiyonu ekleyin.

1. Yeni oluşturduğunuz Cosmos DB hesabınıza gidin.
2. **Genel Bakış** sekmesinde **+/Koleksiyon Ekle** düğmesine tıklayın; "Koleksiyon Ekle" paneli belirir.
3. Koleksiyona bir veritabanı kimliği ve koleksiyon kimliği girin, depolama kapasitesini seçin, bölüm anahtarı girin, aktarım hızı değeri girin ve sonra da **Tamam**'a tıklayın.  Bu öğretici için, veritabanı kimliği ve koleksiyon kimliği olarak "Test" kullanmak, sabit bir depolama kapasitesi ve en düşük aktarım hızı (400 RU/s) seçmek yeterlidir.  


### <a name="grant-access-to-the-cosmos-db-account-access-keys"></a>Cosmos DB hesabı erişim anahtarlarına erişim izni verme

Bu bölümde, Cosmos DB hesabı erişim anahtarlarına Windows VM sistem tarafından atanan yönetilen kimlik erişiminin nasıl atanacağı gösterilmektedir. Cosmos DB Azure AD kimlik doğrulamayı yerel olarak desteklemez. Bununla birlikte, Kaynak Yöneticisi'nden Cosmos DB erişim anahtarını almak için sistem tarafından atanan bir yönetilen kimliği kullanabilir ve ardından anahtarı kullanarak Cosmos DB'ye erişebilirsiniz. Bu adımda, Windows VM sistem tarafından atanan yönetilen kimliğinize Cosmos DB hesabının anahtarları için erişim verirsiniz.

PowerShell kullanarak Azure Resource Manager'da Cosmos DB hesabına Windows VM sistem tarafından atanan yönetilen kimliği erişimi vermek için, `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` ve `<COSMOS DB ACCOUNT NAME>` değerlerini ortamınıza uygun olarak güncelleştirin. Cosmos DB, erişim anahtarları kullanılırken iki ayrıntı düzeyini destekler: hesaba okuma/yazma erişimi ve hesaba salt okuma erişimi.  Hesap için okuma/yazma anahtarları almak istiyorsanız `DocumentDB Account Contributor` rolünü veya hesap için salt okuma anahtarları almak istiyorsanız `Cosmos DB Account Reader Role` rolünü atayın.  Bu öğretici için `Cosmos DB Account Reader Role` rolünü atayın:

```azurepowershell
$spID = (Get-AzVM -ResourceGroupName myRG -Name myVM).identity.principalid
New-AzRoleAssignment -ObjectId $spID -RoleDefinitionName "Cosmos DB Account Reader Role" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.DocumentDb/databaseAccounts/<COSMOS DB ACCOUNT NAME>"
```

>[!NOTE]
> Bir işlemi gerçekleştiremeyebilirsiniz doğru izinlere sahip olmayabilirsiniz. Anahtarlara yazma erişimi istiyorsanız DocumentDB hesabı katılımcısı gibi bir RBAC rolü kullanmanız veya özel bir rol oluşturmanız gerekir. Daha fazla bilgi için [Azure Cosmos DB Içindeki rol tabanlı erişim denetimini](../../cosmos-db/role-based-access-control.md) gözden geçirin

## <a name="access-data"></a>Verilere erişme

Bu bölümde, Windows VM sistem tarafından atanan yönetilen kimlik için bir erişim belirteci kullanarak Azure Resource Manager nasıl çağrılacağını gösterir. Bu öğreticinin kalan bölümünde, daha önce oluşturmuş olduğunuz VM'den çalışacağız. 

Windows sanal makinenize en son [Azure CLI](/cli/azure/install-azure-cli) sürümünü yüklemeniz gerekir.



### <a name="get-an-access-token"></a>Bir erişim belirteci alma

1. Azure portalında **Sanal Makineler**'e gidin, Windows sanal makinenize gidin ve ardından **Genel Bakış** sayfasında üst kısımdaki **Bağlan**'a tıklayın. 
2. Windows VM 'yi oluştururken eklediğiniz **Kullanıcı adınızı** ve **parolanızı** girin. 
3. Artık sanal makineyle **Uzak Masaüstü Bağlantısı**'nı oluşturduğunuza göre, uzak oturumda PowerShell'i açın.
4. PowerShell’in Invoke-WebRequest komutunu kullanarak, Azure kaynakları uç noktası için yerel yönetilen kimliklere Azure Resource Manager için erişim belirteci alma isteğinde bulunun.

   ```powershell
   $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Method GET -Headers @{Metadata="true"}
   ```

   > [!NOTE]
   > "Resource" parametre değeri Azure AD'nin beklediği değerle tam olarak eşleşmelidir. Azure Resource Manager kaynak kimliği kullanıldığında, URI'nin sonundaki eğik çizgiyi de eklemelisiniz.
    
   Ardından, $response nesnesinde JavaScript Nesne Gösterimi (JSON) biçimlendirilmiş dizesi olarak depolanan “Content” öğesini ayıklayın. 
    
   ```powershell
   $content = $response.Content | ConvertFrom-Json
   ```
   Ardından, yanıttan erişim belirtecini ayıklayın.
    
   ```powershell
   $ArmToken = $content.access_token
   ```

### <a name="get-access-keys"></a>Erişim anahtarları al 

Bu bölüm, Cosmos DB çağrısı yapmak için Azure Resource Manager erişim tuşlarının nasıl alınacağını gösterir. Şimdi Cosmos DB hesabı erişim anahtarını almak için önceki bölümde alınan erişim belirtecini kullanarak Resource Manager'ı çağırmak için PowerShell kullanın. Erişim anahtarını aldıktan sonra Cosmos DB'yi sorgulayabiliriz. `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` ve `<COSMOS DB ACCOUNT NAME>` parametre değerlerini kendi değerlerinizden değiştirmeyi unutmayın. `<ACCESS TOKEN>` değerini daha önce aldığınız erişim belirteciyle değiştirin.  Okuma/yazma anahtarlarını almak istiyorsanız, `listKeys` anahtar işlem türünü kullanın.  Salt okuma anahtarlarını almak istiyorsanız, `readonlykeys` anahtar işlem türünü kullanın:

```powershell
Invoke-WebRequest -Uri 'https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.DocumentDb/databaseAccounts/<COSMOS DB ACCOUNT NAME>/listKeys/?api-version=2016-03-31' -Method POST -Headers @{Authorization="Bearer $ARMToken"}
```
Yanıt Anahtarların listesini verir.  Örneğin, salt okuma anahtarlarını alıyorsanız:

```powershell
{"primaryReadonlyMasterKey":"bWpDxS...dzQ==",
"secondaryReadonlyMasterKey":"38v5ns...7bA=="}
```
Artık Cosmos DB hesabı için erişim anahtarınız olduğundan, bunu Cosmos DB SDK'sına geçirebilir ve hesaba erişmek için çağrılar yapabilirsiniz.  Hızlı bir örnek olarak, erişim anahtarını Azure CLI'ye geçirebilirsiniz.  Azure portalındaki Cosmos DB hesabı dikey penceresinin **Genel Bakış** sekmesinden `<COSMOS DB CONNECTION URL>` değerini alabilirsiniz.  `<ACCESS KEY>` değerini yukarıda elde ettiğiniz değerle değiştirin:

```azurecli
az cosmosdb collection show -c <COLLECTION ID> -d <DATABASE ID> --url-connection "<COSMOS DB CONNECTION URL>" --key <ACCESS KEY>
```

Bu CLI komutu koleksiyon hakkındaki ayrıntıları döndürür:

```output
{
  "collection": {
    "_conflicts": "conflicts/",
    "_docs": "docs/",
    "_etag": "\"00006700-0000-0000-0000-5a8271e90000\"",
    "_rid": "Es5SAM2FDwA=",
    "_self": "dbs/Es5SAA==/colls/Es5SAM2FDwA=/",
    "_sprocs": "sprocs/",
    "_triggers": "triggers/",
    "_ts": 1518498281,
    "_udfs": "udfs/",
    "id": "Test",
    "indexingPolicy": {
      "automatic": true,
      "excludedPaths": [],
      "includedPaths": [
        {
          "indexes": [
            {
              "dataType": "Number",
              "kind": "Range",
              "precision": -1
            },
            {
              "dataType": "String",
              "kind": "Range",
              "precision": -1
            },
            {
              "dataType": "Point",
              "kind": "Spatial"
            }
          ],
          "path": "/*"
        }
      ],
      "indexingMode": "consistent"
    }
  },
  "offer": {
    "_etag": "\"00006800-0000-0000-0000-5a8271ea0000\"",
    "_rid": "f4V+",
    "_self": "offers/f4V+/",
    "_ts": 1518498282,
    "content": {
      "offerIsRUPerMinuteThroughputEnabled": false,
      "offerThroughput": 400
    },
    "id": "f4V+",
    "offerResourceId": "Es5SAM2FDwA=",
    "offerType": "Invalid",
    "offerVersion": "V2",
    "resource": "dbs/Es5SAA==/colls/Es5SAM2FDwA=/"
  }
}
```


## <a name="disable"></a>Devre Dışı Bırak

[!INCLUDE [msi-tut-disable](../../../includes/active-directory-msi-tut-disable.md)]



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Cosmos DB'ye erişmek için Windows VM sistem tarafından atanan kimliğini kullanmayı öğrendiniz.  Cosmos DB hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
>[Azure Cosmos DB genel bakış](../../cosmos-db/introduction.md)