---
title: Öğretici`:` Access Key-Linux-Azure AD aracılığıyla Azure depolama 'ya erişmek için yönetilen kimlik kullanma
description: Linux VM üzerinde bir sistem tarafından atanmış yönetilen kimlik kullanarak Azure Depolama'ya erişme işleminde size yol gösteren bir öğretici.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: daveba
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/04/2020
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 86f875fa80f8bb8dd33a369a23f49833162cd417
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78273813"
---
# <a name="tutorial-use-a-linux-vm-system-assigned-managed-identity-to-access-azure-storage-via-access-key"></a>Öğretici: Erişim anahtarı yoluyla Azure Depolama'ya erişmek için Linux VM sistem tarafından atanan yönetilen kimliği kullanma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bu öğreticide depolama hesabı erişim anahtarlarını almak amacıyla bir Linux sanal makinesi (VM) için sistem tarafından atanmış yönetilen bir kimliği nasıl kullanacağınız gösterilmektedir. Depolama işlemleri yaparken, örneğin Depolama SDK'sını kullanırken depolama erişim anahtarını olağan şekilde kullanabilirsiniz. Bu öğreticide, Azure CLI kullanarak blob'ları karşıya yüklüyor ve indiriyoruz. Şunları öğrenirsiniz:

> [!div class="checklist"]
> * VM’inize Resource Manager’da yer alan depolama hesabı erişim anahtarı için erişim verme 
> * VM'nizin kimliğini kullanarak erişim belirteci alma ve Resource Manager'dan depolama erişim anahtarlarını almak için bu belirteci kullanma  

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma 

Henüz bir depolama hesabınız yoksa, şimdi oluşturacaksınız.  Ayrıca bu adımı atlayabilir ve VM sistem tarafından atanan yönetilen kimliğinize mevcut depolama hesabının anahtarları için erişim verebilirsiniz. 

1. Azure portalının sol üst köşesinde bulunan **+/Yeni hizmet oluştur** düğmesine tıklayın.
2. **Depolama**'ya ve **Depolama Hesabı**'na tıklayın; yeni bir "Depolama hesabı oluştur" paneli görüntülenir.
3. Daha sonra kullanacağınız depolama hesabı için bir **Ad** girin.  
4. **Dağıtım modeli** ve **Hesap türü** sırasıyla "Kaynak yöneticisi" ve "Genel amaçlı" olarak ayarlanmalıdır. 
5. **Abonelik** ve **Kaynak Grubu** değerlerinin, önceki adımda VM'nizi oluştururken belirttiklerinizle eşleştiğinden emin olun.
6. **Oluştur**'a tıklayın.

    ![Yeni depolama hesabı oluşturma](./media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Depolama hesabında bir blob kapsayıcısı oluşturma

Daha sonra yeni depolama hesabına dosya yükleyecek ve indireceğiz. Dosyalar için blob depolaması gerektiğinden, dosyanın depolanacağı bir blob kapsayıcısı oluşturmalıyız.

1. Yeni oluşturulan depolama hesabınıza geri gidin.
2. Sol tarafta, "Blob hizmeti" öğesinin altındaki **Kapsayıcılar** bağlantısına tıklayın.
3. Sayfanın en üstündeki **+ Kapsayıcı**'ya tıklayın; "Yeni kapsayıcı" paneli belirir.
4. Kapsayıcıya bir ad verin, erişim düzeyini seçin ve ardından **Tamam**'a tıklayın. Belirttiğiniz ad bu öğreticide daha sonra kullanılacaktır. 

    ![Depolama kapsayıcısı oluşturma](./media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-system-assigned-managed-identity-access-to-use-storage-account-access-keys"></a>Depolama hesabı erişim anahtarlarını kullanmak için VM'nize sistem tarafından atanan yönetilen kimliği erişimi verme

Bu adımda, VM sistem tarafından atanan yönetilen kimliğinize depolama hesabının anahtarları için erişim verirsiniz.   

1. Yeni oluşturulan depolama hesabınıza geri gidin.
2. Sol bölmedeki **Erişim denetimi (IAM)** bağlantısına tıklayın.  
3. VM 'niz için yeni bir rol ataması eklemek üzere sayfanın üstünde **+ rol ataması Ekle** ' ye tıklayın
4. Sayfanın sağ tarafında, **Rol** olarak  "Depolama Hesabı Anahtarı İşleci Hizmet Rolü" seçeneğini ayarlayın. 
5. Sonraki açılan listede **Erişimin atanacağı hedef** olarak "Sanal Makine" seçeneğini ayarlayın.  
6. Ardından, uygun aboneliğin **Abonelik**’te listelendiğinden emin olun ve sonra **Kaynak Grubu**’nu "Tüm kaynak grupları" olarak ayarlayın.  
7. Son olarak, **Seç**'in altındaki açılan listede Linux Sanal Makinenizi seçin ve **Kaydet**'e tıklayın. 

    ![Alternatif resim metni](./media/msi-tutorial-linux-vm-access-storage/msi-storage-role.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-resource-manager"></a>VM kimliğini kullanarak erişim belirteci alma ve Azure Resource Manager çağrısı yapmak için bunu kullanma

Bu öğreticinin kalan bölümünde, daha önce oluşturmuş olduğumuz VM'den çalışacağız.

Bu adımları tamamlamak bir SSH istemciniz olmalıdır. Windows kullanıyorsanız, [Linux için Windows Alt Sistemi](https://msdn.microsoft.com/commandline/wsl/install_guide)'ndeki SSH istemcisini kullanabilirsiniz. SSSH istemcinizin anahtarlarını yapılandırmak için yardıma ihtiyacınız olursa, bkz. [Azure'da Windows ile SSH anahtarlarını kullanma](../../virtual-machines/linux/ssh-from-windows.md) veya [Azure’da Linux VM’ler için SSH ortak ve özel anahtar çifti oluşturma](../../virtual-machines/linux/mac-create-ssh-keys.md).

1. Azure portalında **Sanal Makineler**'e gidin, Linux sanal makinenize gidin ve ardından **Genel Bakış** sayfasında üst kısımdaki **Bağlan**'a tıklayın. VM'nize bağlanma dizesini kopyalayın. 
2. SSH istemcinizi kullanarak VM'nize bağlanın.  
3. Ardından, **Linux VM'sini** oluştururken eklediğiniz **Parolanızı** girmeniz istenecektir. Bundan sonra başarıyla oturum açabilmeniz gerekir.  
4. Azure Resource Manager'ın erişim anahtarını almak için CURL kullanın.  

    Erişim belirteci için CURL istek ve yanıtı aşağıda yer alır:
    
    ```bash
    curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true
    ```
    
    > [!NOTE]
    > Önceki istekte, "resource" parametresinin değeri Azure AD'nin beklediği değerle tam olarak eşleşmelidir. Azure Resource Manager kaynak kimliği kullanıldığında, URI'nin sonundaki eğik çizgiyi de eklemelisiniz.
    > Aşağıdaki yanıtta, access_token öğesi kısaltılmıştır.
    
    ```bash
    {"access_token":"eyJ0eXAiOiJ...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1504130527",
    "not_before":"1504126627",
    "resource":"https://management.azure.com",
    "token_type":"Bearer"} 
     ```
    
## <a name="get-storage-account-access-keys-from-azure-resource-manager-to-make-storage-calls"></a>Depolama çağrıları yapmak için Azure Resource Manager'dan depolama hesabı erişim anahtarları alma  

Şimdi depolama erişim anahtarını almak için önceki bölümde aldığımız erişim belirtecini kullanarak Resource Manager'ı çağırmak için CURL kullanın. Depolama erişim anahtarımızı aldıktan sonra, depolama karşıya yükleme/indirme işlemlerini çağırabiliriz. `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` ve `<STORAGE ACCOUNT NAME>` parametre değerlerini kendi değerlerinizden değiştirmeyi unutmayın. `<ACCESS TOKEN>` değerini daha önce aldığınız erişim belirteciyle değiştirin:

```bash 
curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>/listKeys?api-version=2016-12-01 --request POST -d "" -H "Authorization: Bearer <ACCESS TOKEN>" 
```

> [!NOTE]
> Önceki URL'nin metni büyük/küçük harfe duyarlıdır; bu nedenle Kaynak Gruplarınız için büyük/küçük harf kullanımının bunu düzgün yansıttığından emin olun. Ayrıca, bir GET isteği değil POST isteği olduğunu bilmeniz ve -d ile NULL olabilecek uzunluk sınırını yakalamak üzere bir değer geçirmeniz de önemlidir.  

CURL yanıtı Anahtarların listesini verir:  

```bash 
{"keys":[{"keyName":"key1","permissions":"Full","value":"iqDPNt..."},{"keyName":"key2","permissions":"Full","value":"U+uI0B..."}]} 
```
Blob depolama kapsayıcısına yüklemek için örnek bir blob dosyası oluşturun. Linux VM’sinde bu işlemi aşağıdaki komutla gerçekleştirebilirsiniz. 

```bash
echo "This is a test file." > test.txt
```

Ardından depolama erişim anahtarını kullanarak CLI `az storage` komutuyla kimlik doğrulaması yapın ve dosyayı blob kapsayıcısına yükleyin. Bu adım için, daha önceden yüklemediyseniz VM’inize [en son Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) sürümünü yüklemeniz gerekir.
 

```azurecli-interactive
az storage blob upload -c <CONTAINER NAME> -n test.txt -f test.txt --account-name <STORAGE ACCOUNT NAME> --account-key <STORAGE ACCOUNT KEY>
```

Yanıt: 

```JSON
Finished[#############################################################]  100.0000%
{
  "etag": "\"0x8D4F9929765C139\"",
  "lastModified": "2017-09-12T03:58:56+00:00"
}
```

Ayrıca dosyayı Azure CLI kullanarak indirebilir ve kimlik doğrulamasını depolama erişim anahtarıyla yapabilirsiniz. 

İstek: 

```azurecli-interactive
az storage blob download -c <CONTAINER NAME> -n test.txt -f test-download.txt --account-name <STORAGE ACCOUNT NAME> --account-key <STORAGE ACCOUNT KEY>
```

Yanıt: 

```JSON
{
  "content": null,
  "metadata": {},
  "name": "test.txt",
  "properties": {
    "appendBlobCommittedBlockCount": null,
    "blobType": "BlockBlob",
    "contentLength": 21,
    "contentRange": "bytes 0-20/21",
    "contentSettings": {
      "cacheControl": null,
      "contentDisposition": null,
      "contentEncoding": null,
      "contentLanguage": null,
      "contentMd5": "LSghAvpnElYyfUdn7CO8aw==",
      "contentType": "text/plain"
    },
    "copy": {
      "completionTime": null,
      "id": null,
      "progress": null,
      "source": null,
      "status": null,
      "statusDescription": null
    },
    "etag": "\"0x8D5067F30D0C283\"",
    "lastModified": "2017-09-28T14:42:49+00:00",
    "lease": {
      "duration": null,
      "state": "available",
      "status": "unlocked"
    },
    "pageBlobSequenceNumber": null,
    "serverEncrypted": false
  },
  "snapshot": null
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, erişim anahtarı kullanarak Azure Depolama'ya erişmek için Linux VM sistem tarafından atanan yönetilen kimlik kullanmayı öğrendiniz.  Azure Depolama erişim anahtarları hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
>[Depolama erişim anahtarlarınızı yönetme](/azure/storage/common/storage-create-storage-account)
