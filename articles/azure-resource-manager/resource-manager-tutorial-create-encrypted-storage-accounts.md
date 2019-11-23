---
title: Şablon başvurusunu kullanma
description: Utilize the Azure Resource Manager template reference to create a template for deploying an encrypted storage account.
author: mumian
ms.date: 03/04/2019
ms.topic: tutorial
ms.author: jgao
ms.custom: seodec18
ms.openlocfilehash: 99ec64529b90c7a80aea62090f80c55cf4e23510
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74326490"
---
# <a name="tutorial-utilize-the-azure-resource-manager-template-reference"></a>Tutorial: Utilize the Azure Resource Manager template reference

Şablon şema bilgilerini bulmayı ve bu bilgileri kullanarak Azure Resource Manager şablonu oluşturmayı öğrenin.

Bu öğreticide Azure Hızlı Başlangıç şablonları arasından temel bir şablon kullanırsınız. Şablon başvuru belgelerini kullanarak şablonu özelleştirecek ve şifrelenmiş bir Depolama hesabı kullanmasını sağlayacaksınız.

![Resource Manager template reference deploy encrypted storage account](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-tutorial-deploy-encrypted-storage-account.png)

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Hızlı başlangıç şablonunu açma
> * Şablon biçimini anlama
> * Şablon başvurusunu bulma
> * Şablonu düzenleme
> * Şablonu dağıtma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlamak için gerekenler:

* Visual Studio Code with Resource Manager Tools extension. See [Use Visual Studio Code to create Azure Resource Manager templates](./resource-manager-tools-vs-code.md).

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

[Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/resources/templates/), Resource Manager şablonları için bir depolama alanıdır. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu hızlı başlangıçta kullanılan şablon [Standart depolama hesabı oluşturma](https://azure.microsoft.com/resources/templates/101-storage-account-create/) olarak adlandırılır. Şablon, Azure Depolama hesabı kaynağını tanımlar.

1. Visual Studio Code’dan **Dosya**>**Dosya Aç**’ı seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Dosyayı açmak için **Aç**’ı seçin.
4. Dosyayı yerel bilgisayarınıza **azuredeploy.json** olarak kaydetmek için **Dosya**>**Farklı Kaydet**’i seçin.

## <a name="understand-the-schema"></a>Şemayı anlama

1. VS Code'da şablonu kök düzeye daraltın. Aşağıdaki öğelere sahip çok basit bir yapı görürsünüz:

    ![Resource Manager şablonu basit yapısı](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-simplest-structure.png)

    * **$schema**: Şablon dilinin sürümünü tanımlayan JSON şema dosyasının konumunu belirtin.
    * **contentVersion**: Şablonunuzdaki önemli değişiklikleri belirtmek için bu öğeye istediğiniz değeri verebilirsiniz.
    * **parameters**: Dağıtım, kaynak dağıtımını özelleştirme amacıyla yürütüldüğünde sağlanan değerleri belirtin.
    * **variables**: Şablon dili ifadelerini basitleştirmek için şablonda JSON parçaları olarak kullanılan değerleri belirtin.
    * **resources**: Kaynak grubunda dağıtılan veya güncelleştirilen kaynak türlerini belirtin.
    * **outputs**: Dağıtım sonrasında döndürülen değerleri belirtin.

2. **resources** bölümünü genişletin. Tanımlı bir `Microsoft.Storage/storageAccounts` kaynağı olduğunu göreceksiniz. Şablon, şifrelenmemiş bir Depolama hesabı oluşturur.

    ![Resource Manager şablonu depolama hesabı tanımı](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-encrypted-storage-resource.png)

## <a name="find-the-template-reference"></a>Şablon başvurusunu bulma

1. Browse to [Azure Template reference](https://docs.microsoft.com/azure/templates/).
2. In the **Filter by title** box, enter **storage accounts**.
3. Select **Reference/Template reference/Storage/&lt;Version>/Storage Accounts** as shown in the following screenshot:

    ![Resource Manager şablon başvurusu depolama hesabı](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-resources-reference-storage-accounts.png)

    If you don't know which version to choose, use the latest version.

4. Şifrelemeyle ilgili tanım bilgilerini bulun.

    ```json
    "encryption": {
      "services": {
        "blob": {
          "enabled": boolean
        },
        "file": {
          "enabled": boolean
        }
      },
      "keySource": "string",
      "keyvaultproperties": {
        "keyname": "string",
        "keyversion": "string",
        "keyvaulturi": "string"
      }
    },
    ```

    Aynı web sayfasında bulunan aşağıdaki açıklama, `encryption` nesnesinin şifrelenmiş depolama hesabı oluşturmak için kullanıldığını doğrulamaktadır.

    ![Resource Manager şablon başvurusu depolama hesabı şifreleme](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-resources-reference-storage-accounts-encryption.png)

    Şifreleme anahtarını yönetmek için kullanabileceğiniz iki yöntem vardır. Depolama Hizmeti Şifrelemesi ile Microsoft tarafından yönetilen şifreleme anahtarlarını veya kendi şifreleme anahtarlarınızı kullanabilirsiniz. Bu öğreticinin basit olmasını sağlamak için `Microsoft.Storage` seçeneğini kullanabilir, Azure Key Vault oluşturmadan devam edebilirsiniz.

    ![Resource Manager şablon başvurusu depolama hesabı şifreleme nesnesi](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-resources-reference-storage-accounts-encryption-object.png)

    Şifreleme nesneniz şu şekilde görünmelidir:

    ```json
    "encryption": {
        "services": {
            "blob": {
                "enabled": true
            },
            "file": {
              "enabled": true
            }
        },
        "keySource": "Microsoft.Storage"
    }
    ```

## <a name="edit-the-template"></a>Şablonu düzenleme

resources öğesinin aşağıdaki şekilde görünmesi için Visual Studio Code'dan şablonu değiştirin:

![Resource Manager şablonu şifrelenmiş depolama hesabı kaynakları](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-encrypted-storage-resources.png)

## <a name="deploy-the-template"></a>Şablonu dağıtma

Dağıtım yordamı için Visual Studio Code hızlı başlangıçta [Şablonu dağıtma](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#deploy-the-template) bölümüne bakın.

Aşağıdaki ekran görüntüsünde, şifrelemenin blob depolama için etkinleştirildiğini belirten, yeni oluşturulan depolama hesabını listelemeye yönelik CLI komutu gösterilmektedir.

![Azure Resource Manager şifrelenmiş depolama hesabı](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-encrypted-storage-account.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şablon başvurusunu kullanarak var olan bir şablonu özelleştirmeyi öğrendiniz. Birden çok depolama hesabı örneği oluşturmayı öğrenmek için, bkz:

> [!div class="nextstepaction"]
> [Birden çok örnek oluşturma](./resource-manager-tutorial-create-multiple-instances.md)
