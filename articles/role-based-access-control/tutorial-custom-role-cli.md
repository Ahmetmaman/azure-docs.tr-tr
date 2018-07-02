---
title: "Öğretici: Azure CLI'yı kullanarak özel bir rol oluşturma | Microsoft Docs"
description: Azure CLI'yı kullanarak özel rol oluşturmaya başlama.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 06/12/2018
ms.author: rolyon
ms.openlocfilehash: 6302ae3bbb97f8f40733074b9d9dc708d10641ca
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36322602"
---
# <a name="tutorial-create-a-custom-role-using-azure-cli"></a>Öğretici: Azure CLI'yı kullanarak özel bir rol oluşturma

[Yerleşik roller](built-in-roles.md) kuruluşunuzun ihtiyaçlarını karşılamıyorsa kendi özel rollerinizi oluşturabilirsiniz. Bu öğretici için Azure CLI'yı kullanarak Reader Support Tickets adlı özel bir rol oluşturacaksınız. Bu özel rol, kullanıcının abonelikteki her şeyi görüntülemesini ve destek bileti açmasını sağlar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Özel rol oluşturma
> * Özel rolleri listeleme
> * Özel rolü güncelleştirme
> * Özel rolü silme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

- [Sahip](built-in-roles.md#owner) veya [Kullanıcı Erişimi Yöneticisi](built-in-roles.md#user-access-administrator) gibi özel rol oluşturma izni
- Yerel ortama yüklenmiş [Azure CLI](/cli/azure/install-azure-cli)

## <a name="sign-in-to-azure-cli"></a>Azure CLI'da oturum açma

[Azure CLI](/cli/azure/authenticate-azure-cli)'da oturum açın.

## <a name="create-a-custom-role"></a>Özel rol oluşturma

Özel rol oluşturmanın en kolay yolu bir JSON şablonuyla başlayıp değişikliklerinizi ekledikten sonra yeni bir rol oluşturmaktır.

1. [Microsoft.Support kaynak sağlayıcısının](resource-provider-operations.md#microsoftsupport) işlem listesini gözden geçirin. İzinlerinizi oluşturmak için kullanabileceğiniz işlemleri bilmeniz yararlıdır.

    | İşlem | Açıklama |
    | --- | --- |
    | Microsoft.Support/register/action | Destek Kaynağı Sağlayıcısı'na kayıt yapar |
    | Microsoft.Support/supportTickets/read | Durum, önem derecesi, kişi ayrıntıları ve iletişimler gibi Destek Biletleri ayrıntılarını alır veya aboneliklerdeki Destek Biletleri listesini alır. |
    | Microsoft.Support/supportTickets/write | Destek Bileti oluşturur veya güncelleştirir. Teknik, Faturalama, Kotalar veya Abonelik Yönetimi konusunda bir Destek Bileti oluşturabilirsiniz. Var olan destek biletlerinin önem derecesini, iletişim bilgilerini ve iletişimlerini güncelleştirebilirsiniz. |
    
1. **ReaderSupportRole.json** adlı yeni bir dosya oluşturun.

1. ReaderSupportRole.json dosyasını bir düzenleyicide açıp aşağıdaki JSON kodunu ekleyin.

    Farklı özellikler hakkında bilgi edinmek için bkz. [Özel roller](custom-roles.md).

    ```json
    {
        "Name":  "",
        "IsCustom":  true,
        "Description":  "",
        "Actions":  [
    
                    ],
        "NotActions":  [
    
                       ],
        "DataActions":  [
    
                        ],
        "NotDataActions":  [
    
                           ],
        "AssignableScopes":  [
                                 "/subscriptions/{subscriptionId1}"
                             ]
    }
    ```
    
1. Aşağıdaki işlemleri `Actions` özelliğine ekleyin. Bu eylemler, kullanıcının abonelikteki her şeyi görüntülemesini ve destek bileti oluşturmasını sağlar.

    ```
    "*/read",
    "Microsoft.Support/*"
    ```

1. [az account list](/cli/azure/account#az-account-list) komutunu kullanarak aboneliğinizin kimliğini alın.

    ```azurecli
    az account list --output table
    ```

1. `AssignableScopes` içinde `{subscriptionId1}` yerine abonelik kimliğinizi yazın.

    Açık abonelik kimliklerini girmeniz gerekir, aksi halde rolü aboneliğinize aktaramazsınız.

1. `Name` ve `Description` özelliklerini "Okuyucu Destek Biletleri" ve "Abonelikteki her şeyi görüntüleme ve destek bileti açma." olarak değiştirin.

    JSON dosyanız aşağıdaki gibi görünmelidir:

    ```json
    {
        "Name":  "Reader Support Tickets",
        "IsCustom":  true,
        "Description":  "View everything in the subscription and also open support tickets.",
        "Actions":  [
                        "*/read",
                        "Microsoft.Support/*"
                    ],
        "NotActions":  [
    
                       ],
        "DataActions":  [
    
                        ],
        "NotDataActions":  [
    
                           ],
        "AssignableScopes":  [
                                 "/subscriptions/00000000-0000-0000-0000-000000000000"
                             ]
    }
    ```
    
1. Yeni özel rolü oluşturmak için [az role definition create](/cli/azure/role/definition#az-role-definition-create) komutunu kullanın ve JSON rol tanımı dosyasını belirtin.

    ```azurecli
    az role definition create --role-definition "~/CustomRoles/ReaderSupportRole.json"
    ```

    ```Output
    {
      "additionalProperties": {},
      "assignableScopes": [
        "/subscriptions/00000000-0000-0000-0000-000000000000"
      ],
      "description": "View everything in the subscription and also open support tickets.",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleDefinitions/22222222-2222-2222-2222-222222222222",
      "name": "22222222-2222-2222-2222-222222222222",
      "permissions": [
        {
          "actions": [
            "*/read",
            "Microsoft.Support/*"
          ],
          "additionalProperties": {},
          "dataActions": [],
          "notActions": [],
          "notDataActions": []
        }
      ],
      "roleName": "Reader Support Tickets",
      "roleType": "CustomRole",
      "type": "Microsoft.Authorization/roleDefinitions"
    }
    ```

    Yeni özel rol artık kullanılabilir ve yerleşik roller gibi kullanıcılara, gruplara veya hizmet sorumlularına atanabilir.

## <a name="list-custom-roles"></a>Özel rolleri listeleme

- Özel rollerinizin tümünü listelemek için [az role definition list](/cli/azure/role/definition#az-role-definition-list) komutunu `--custom-role-only` parametresiyle birlikte kullanın.

    ```azurecli
    az role definition list --custom-role-only true
    ```
    
    ```Output
    [
      {
        "additionalProperties": {},
        "assignableScopes": [
          "/subscriptions/00000000-0000-0000-0000-000000000000"
        ],
        "description": "View everything in the subscription and also open support tickets.",
        "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleDefinitions/22222222-2222-2222-2222-222222222222",
        "name": "22222222-2222-2222-2222-222222222222",
        "permissions": [
          {
            "actions": [
              "*/read",
              "Microsoft.Support/*",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Insights/diagnosticSettings/*/read"
            ],
            "additionalProperties": {},
            "dataActions": [],
            "notActions": [],
            "notDataActions": []
          }
        ],
        "roleName": "Reader Support Tickets",
        "roleType": "CustomRole",
        "type": "Microsoft.Authorization/roleDefinitions"
      }
    ]
    ```
    
    Özel rolü Azure portalında da görebilirsiniz.

    ![Azure portalına aktarılmış olan özel rolün ekran görüntüsü](./media/tutorial-custom-role-cli/custom-role-reader-support-tickets.png)

## <a name="update-a-custom-role"></a>Özel rolü güncelleştirme

Özel rolü güncelleştirmek için JSON dosyasını ve ardından özel rolü güncelleştirin.

1. ReaderSupportRole.json dosyasını açın.

1. `Actions` içine kaynak grubu dağıtımlarını oluşturma ve yönetme işlemini ekleyin: `"Microsoft.Resources/deployments/*"`. Önceki işlemden sonra virgül eklemeyi unutmayın.

    Güncelleştirilmiş JSON dosyanız aşağıdaki gibi görünmelidir:

    ```json
    {
        "Name":  "Reader Support Tickets",
        "IsCustom":  true,
        "Description":  "View everything in the subscription and also open support tickets.",
        "Actions":  [
                        "*/read",
                        "Microsoft.Support/*",
                        "Microsoft.Resources/deployments/*"
                    ],
        "NotActions":  [
    
                       ],
        "DataActions":  [
    
                        ],
        "NotDataActions":  [
    
                           ],
        "AssignableScopes":  [
                                 "/subscriptions/00000000-0000-0000-0000-000000000000"
                             ]
    }
    ```
        
1. Özel rolü güncelleştirmek için [az role definition update](/cli/azure/role/definition#az-role-definition-update) komutunu kullanarak güncelleştirilmiş JSON dosyasını belirtin.

    ```azurecli
    az role definition update --role-definition "~/CustomRoles/ReaderSupportRole.json"
    ```

    ```Output
    {
      "additionalProperties": {},
      "assignableScopes": [
        "/subscriptions/00000000-0000-0000-0000-000000000000"
      ],
      "description": "View everything in the subscription and also open support tickets.",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleDefinitions/22222222-2222-2222-2222-222222222222",
      "name": "22222222-2222-2222-2222-222222222222",
      "permissions": [
        {
          "actions": [
            "*/read",
            "Microsoft.Support/*",
            "Microsoft.Resources/deployments/*"
          ],
          "additionalProperties": {},
          "dataActions": [],
          "notActions": [],
          "notDataActions": []
        }
      ],
      "roleName": "Reader Support Tickets",
      "roleType": "CustomRole",
      "type": "Microsoft.Authorization/roleDefinitions"
    }
    ```
    
## <a name="delete-a-custom-role"></a>Özel rolü silme

- Özel rolü silmek için [az role definition delete](/cli/azure/role/definition#az-role-definition-delete) komutunu kullanın ve rol adını veya rol kimliğini belirtin.

    ```azurecli
    az role definition delete --name "Reader Support Tickets"
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure CLI'yı kullanarak özel roller oluşturma](custom-roles-cli.md)