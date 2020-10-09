---
title: Azure CLI Betik Örneği - Hesabınızın kimlik bilgilerini sıfırlama | Microsoft Docs
description: Azure CLI betiğini kullanarak hesabınızın kimlik bilgilerini sıfırlayın ve app.config ayarlarını geri alın.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: troubleshooting
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: devx-track-azurecli
ms.openlocfilehash: da997ec71655231876749d9f3aa65ba06fd3a1f8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89295844"
---
# <a name="azure-cli-example-reset-the-account-credentials"></a>Azure CLı örneği: hesap kimlik bilgilerini sıfırlayın

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Bu makaledeki Azure CLI betiği, hesabınızın kimlik bilgilerini sıfırlamayı ve app.config ayarlarını geri almayı gösterir.

## <a name="prerequisites"></a>Önkoşullar

[Media Services hesabı oluşturun](./create-account-howto.md).

[!INCLUDE [media-services-cli-instructions.md](../../../includes/media-services-cli-instructions.md)]

## <a name="example-script"></a>Örnek betik

```azurecli-interactive
# Update the following variables for your own settings:
resourceGroup=amsResourceGroup
amsAccountName=amsmediaaccountname

az ams account sp reset-credentials \
  --account-name $amsAccountName \
  --resource-group $resourceGroup
 ```

## <a name="next-steps"></a>Sonraki adımlar

* [az AMS](/cli/azure/ams)
* [Kimlik bilgilerini sıfırlama](/cli/azure/ams/account/sp#az-ams-account-sp-reset-credentials)
