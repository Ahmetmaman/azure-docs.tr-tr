---
title: Azure Key Vault'u Resource Manager şablonu dağıtımıyla tümleştirme | Microsoft Docs
description: Resource Manager şablonu dağıtımı sırasında parametre değerlerini güvenli bir şekilde geçirme amacıyla Azure Key Vault'u kullanmayı öğrenin
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 01/25/2019
ms.topic: tutorial
ms.author: jgao
ms.custom: seodec18
ms.openlocfilehash: 7371808db8d40948f501b051692172fd6a84e2ac
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56270224"
---
# <a name="tutorial-integrate-azure-key-vault-in-resource-manager-template-deployment"></a>Öğretici: Resource Manager şablon dağıtımı Azure anahtar kasası tümleştirme

Azure Key Vault'tan gizli dizileri almak ve gizli dizileri, Resource Manager dağıtım sırasında parametre olarak geçirmeniz hakkında bilgi edinin. Anahtar kasası kimliğini yalnızca başvuru değeri hiçbir zaman sunulur Daha fazla bilgi için bkz. [Dağıtım sırasında gizli parametre değeri geçirmek için Azure Key Vault'u kullanma](./resource-manager-keyvault-parameter.md)

[Kaynak dağıtım sırasını ayarlama](./resource-manager-tutorial-create-templates-with-dependent-resources.md) öğreticisinde bir sanal makine ve bir sanal ağ dahil olmak üzere ek birkaç bağımlı kaynak oluşturmuştunuz. Bu öğreticide, sanal makine yöneticisi parolası anahtar kasasından almak için bu şablonu özelleştirin.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Bir anahtar Kasası'nı hazırlama
> * Hızlı başlangıç şablonunu açma
> * Parametre dosyasını düzenleme
> * Şablonu dağıtma
> * Dağıtımı doğrulama
> * Kaynakları temizleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlamak için gerekenler:

* [Resource Manager Araçları uzantısı](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites) içeren [Visual Studio Code](https://code.visualstudio.com/).
* Güvenliği artırmak istiyorsanız sanal makine yönetici hesabı için oluşturulmuş bir parola kullanın. Parola oluşturma örneği aşağıda verilmiştir:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Oluşturulan parolanın sanal makine parola gereksinimlerine uygun olduğundan emin olun. Her Azure hizmetinin parola gereksinimleri farklıdır. VM parola gereksinimleri için bkz. [VM oluşturma sırasında parola gereksinimleri nedir?](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

## <a name="prepare-a-key-vault"></a>Bir anahtar Kasası'nı hazırlama

Bu bölümde, bir anahtar kasası ve gizli dizi oluşturmak için Resource Manager şablonu kullanın. Bu şablonu şu işlemleri gerçekleştirir:

* Key vault ile oluşturma `enabledForTemplateDeployment` özelliği sağlar. Bu anahtar Kasası'nda tanımlanan gizli dizileri şablon dağıtım işlemi erişebilmeniz için önce bu özelliği true olmalıdır.
* Gizli anahtar Kasası'na ekleyin.  Gizli dizi, sanal makine yönetici parolasını depolar.

(Gibi sanal makine şablonu dağıtmak için kullanıcının) sahibi veya katkıda bulunan anahtar kasasının emin değilseniz, sahibi veya katkıda bulunan anahtar kasasının anahtar kasası için erişim Microsoft.KeyVault/vaults/deploy/action izin vermeniz gerekir. Daha fazla bilgi için bkz. [Dağıtım sırasında gizli parametre değeri geçirmek için Azure Key Vault'u kullanma](./resource-manager-keyvault-parameter.md)

Şablonda izinlerin yapılandırılması için Azure AD kullanıcı nesnesi kimliğiniz gerekir. Aşağıdaki yordam ' % s'nesne kimliği (GUID) alır.

1. Aşağıdaki Azure PowerShell veya Azure CLI komutunu çalıştırın.  

    ```azurecli-interactive
    echo "Enter your email address that is associated with your Azure subscription):" &&
    read upn &&
    az ad user show --upn-or-object-id $upn --query "objectId" &&
    ```
    ```azurepowershell-interactive
    $upn = Read-Host -Prompt "Input your user principal name (email address) used to sign in to Azure"
    (Get-AzADUser -UserPrincipalName $upn).Id
    ```
2. Nesne Kimliği yazma Bu öğreticinin ilerleyen bölümlerinde gerekir.

Bir anahtar kasası oluşturmak için:

1. Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın. Şablon, bir anahtar kasası ve gizli dizi oluşturur.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Farmtutorials.blob.core.windows.net%2Fcreatekeyvault%2FCreateKeyVault.json"><img src="./media/resource-manager-tutorial-use-key-vault/deploy-to-azure.png" alt="deploy to azure"/></a>

2. Aşağıdaki değerleri seçin veya girin.  Değerleri girdikten sonra **Satın al**'ı seçmeyin.

    ![Resource Manager şablonu Key Vault tümleştirmesi portal dağıtımı](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-key-vault-portal.png)

    * **Abonelik**: Bir Azure aboneliği seçin.
    * **Kaynak grubu**: Benzersiz bir ad girin. Bu kaynak grubunu bir sonraki oturumda sanal makineyi dağıtmak için de kullanacağınızdan bu adı not edin. Aynı kaynak grubunda anahtar kasası hem de sanal makine yerleştirme öğreticinin sonunda kaynağı temizlemek kolaylaştırır.
    * **Konum**: Bir konum seçin.  Varsayılan konum: **Orta ABD**.
    * **Anahtar Kasası Adı**: Benzersiz bir ad girin. 
    * **Kiracı Kimliği**: Şablon işlevi kiracı kimliğinizi otomatik olarak alır.  Varsayılan değeri değiştirmeyin
    * **AD Kullanıcı Kimliği**: Son yordamdan aldığınız Azure AD kullanıcı nesnesi kimliğinizi girin.
    * **Gizli dizi adı**: Varsayılan ad **vmAdminPassword**. Buradaki gizli adı değiştirirseniz sanal makine dağıtma adımında da gizli adı güncelleştirmeniz gerekir.
    * **Gizli değer**: Gizli anahtarı girin.  Gizli dizi, sanal makinede oturum açmak için kullanılan paroladır. Son yordamda oluşturduğunuz parolayı kullanmanız önerilir.
    * **Yukarıdaki hüküm ve koşulları durumu için kabul ediyorum**: Seçin.
3. Şablona göz atmak için yukarıdan **Parametreleri düzenle**'yi seçin.
4. JSON dosyasının 28. satırına göz atın. Anahtar kasası kaynak tanımı budur.
5. 35. satıra göz atın:

    ```json
    "enabledForTemplateDeployment": true,
    ```
    `enabledForTemplateDeployment`, anahtar kasası özelliğidir. Dağıtım sırasında bu anahtar kasasından gizli dizileri almadan önce bu özelliği true olmalıdır.
6. 89. satıra göz atın. Bu, anahtar kasası gizli dizi tanımıdır.
7. Sayfanın en altındaki **Kapat**'ı seçin. Dosyada herhangi bir değişiklik yapmadınız.
8. Yukarıdaki ekran görüntüsünde görünen tüm değerleri sağladığınızdan emin olun ve sayfanın en altında **Satın al**'a tıklayın.
9. Sayfanın üst tarafındaki zil simgesini (bildirim) seçerek **Bildirimler** bölmesini açın. Kaynak başarıyla dağıtılana kadar bekleyin.
10. **Bildirimler** bölmesinden **Kaynak grubuna git**'i seçin. 
11. Açmak için anahtar kasası adı seçin.
12. Seçin **gizli dizileri** sol bölmeden. **vmAdminPassword** listelenip.
13. Sol bölmede **Erişim ilkeleri**'ni seçin. Listede adınız (Active Directory) bulunmalıdır. Aksi takdirde anahtar kasasına erişemezsiniz.
14. **Gelişmiş erişim ilkelerini görüntülemek için tıklayın**'ı seçin. **Şablon dağıtımı için Azure Resource Manager'a erişimi etkinleştir** ayarının seçili olduğunu görebilirsiniz. Bu ayar çalışmak için anahtar kasası tümleştirme yapmak için başka bir durumdur.

    ![Resource Manager şablonu Key Vault tümleştirmesi erişim ilkeleri](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-key-vault-access-policies.png)
15. Soldaki bölmeden **Özellikler**'i seçin.
16. **Kaynak Kimliği** değerini kopyalayın. Sanal makineyi dağıtırken bu kimliğe ihtiyacınız olacak.  Kaynak Kimliği biçimi şu şekildedir:

    ```json
    /subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>
    ```

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Azure Hızlı Başlangıç Şablonları, Resource Manager şablonları için bir depolama alanıdır. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu öğreticide kullanılan şablonun adı: [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Basit bir Windows sanal makinesi dağıtma).

1. Visual Studio Code’dan **Dosya**>**Dosya Aç**’ı seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Dosyayı açmak için **Aç**’ı seçin. [Öğretici: Bağımlı kaynaklarla Azure Resource Manager şablonları oluşturma](./resource-manager-tutorial-create-templates-with-dependent-resources.md) bölümünde kullanılan senaryoyla aynıdır.
4. Şablonun tanımladığı beş kaynak vardır:

    * `Microsoft.Storage/storageAccounts`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
    * `Microsoft.Network/publicIPAddresses`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
    * `Microsoft.Network/virtualNetworks`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
    * `Microsoft.Network/networkInterfaces`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
    * `Microsoft.Compute/virtualMachines`. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

    Şablonu özelleştirmeden önce temel noktaları kavramak faydalı olacaktır.
5. **Dosya**>**Farklı Kaydet**'i seçerek dosyanın bir kopyasını yerel bilgisayarınıza **azuredeploy.json** adıyla kaydedin.
6. 1-4 arası adımları tekrarlayarak aşağıdaki URL'yi açın ve dosyayı **azuredeploy.parameters.json** olarak kaydedin.

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.parameters.json
    ```

## <a name="edit-the-parameters-file"></a>Parametre dosyasını düzenleme

Şablon dosyasında değişiklik yapmanıza gerek yoktur.

1. Açık değilse, Visual Studio Code’da **azuredeploy.parameters.json** dosyasını açın.
2. **adminPassword** parametresini şu şekilde güncelleştirin:

    ```json
    "adminPassword": {
        "reference": {
            "keyVault": {
            "id": "/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>"
            },
            "secretName": "vmAdminPassword"
        }
    },
    ```

    Değiştirin **kimliği** anahtar kasanız son yordamda oluşturduğunuz kaynak kimliği.  

    ![Key Vault ve Resource Manager şablonu sanal makine dağıtımını tümleştirme parametre dosyası](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-vm-parameters-file.png)
3. Şu değerleri verin:

    * **adminUsername**: Sanal makine yönetici hesabının adı.
    * **dnsLabelPrefix**: dnsLablePrefix’i adlandırın.
4. Değişiklikleri kaydedin.

## <a name="deploy-the-template"></a>Şablonu dağıtma

[Şablonu dağıtma](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) bölümündeki yönergeleri izleyerek şablonu dağıtın. Hem **azuredeploy.json** hem de **azuredeploy.parameters.json** dosyasını Cloud Shell'e yükledikten sonra aşağıdaki PowerShell betiğini kullanarak şablonu dağıtmanız gerekir:

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile azuredeploy.json `
    -TemplateParameterFile azuredeploy.parameters.json
```

> [!NOTE]
> Dosya g/ç Cloud shell'de Azure PowerShell kullanarak sorun yoktur.  Hata iletisi *cmdlet'i için dinamik parametreler alınamıyor. Bunu mevcut olmadığından 'Azure:/azuredeploy.json' yolu bulunamıyor.*  Dahil edilmemesi için geçici bir çözüm olan **- TemplateFile** ve **TemplateParameterFile** geçer `New-AzResourceGroupDeploy` komutu. Komut dosya adını girmenizi ister.

Şablonu dağıtırken, anahtar kasası olarak aynı kaynak grubunu kullanın. Bu işlem kaynakları temizlemeyi kolaylaştırır. İki yerine tek bir kaynak grubu silmeniz gerekir.

## <a name="valid-the-deployment"></a>Dağıtımı doğrulama

Başarılı bir şekilde sanal makineyi dağıttıktan sonra anahtar kasasında depolanan parola kullanarak oturum açmayı test edin.

1. [Azure portalı](https://portal.azure.com) açın.
2. **Kaynak grupları**/**KaynakGrubunuzunAdı>**/**simpleWinVM** yolunu izleyin.
3. En üstte **Bağlan**'ı seçin.
4. Seçin **RDP dosyasını indir** ve ardından anahtar Kasası'nda depolanan parola kullanarak sanal makinede oturum açmak için yönergeleri izleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure Key Vault'tan bir gizli dizi aldınız ve bunu şablon dağıtımında kullandınız.  Bağlı şablon oluşturmayı öğrenmek için bkz.:

> [!div class="nextstepaction"]
> [Bağlı şablonlar oluşturma](./resource-manager-tutorial-create-linked-templates.md)
