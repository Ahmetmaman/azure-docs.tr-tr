---
title: 'Azure CLı: Azure için açık kaynak FHıR sunucusunu dağıtma-Azure için Azure API'
description: Bu hızlı başlangıçta, Azure için açık kaynaklı Microsoft FHıR sunucusunun nasıl dağıtılacağı açıklanmaktadır.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: matjazl
ms.custom: devx-track-azurecli
ms.openlocfilehash: 10af71afd8843e75d5df3be57c909c56a7abca01
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "87843598"
---
# <a name="quickstart-deploy-open-source-fhir-server-using-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak açık kaynaklı FHıR sunucusunu dağıtma

Bu hızlı başlangıçta, Azure &reg; CLI kullanarak Azure 'Da açık kaynaklı bır FHıR sunucusu dağıtmayı öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu için sağlanan kaynakları içerecek bir ad seçin ve oluşturun:

```azurecli-interactive
servicename="myfhirservice"
az group create --name $servicename --location westus2
```

## <a name="deploy-template"></a>Şablon dağıtma

Azure [GitHub deposu](https://github.com/Microsoft/fhir-server) için Microsoft fhır sunucusu, tüm gerekli kaynakları dağıtan bir şablon içerir. İle dağıtın:

```azurecli-interactive
az group deployment create -g $servicename --template-uri https://raw.githubusercontent.com/Microsoft/fhir-server/master/samples/templates/default-azuredeploy.json --parameters serviceName=$servicename
```

## <a name="verify-fhir-server-is-running"></a>FHıR sunucusunun çalıştığını doğrulama

FHıR sunucusundan ile bir yetenek beyanı edinin:

```azurecli-interactive
metadataurl="https://${servicename}.azurewebsites.net/metadata"
curl --url $metadataurl
```

Sunucunun ilk kez yanıt vermesi birkaç dakika sürer.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekecekseniz, kaynak grubunu aşağıdaki adımlarla silin:

```azurecli-interactive
az group delete --name $servicename
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure için Microsoft açık kaynak FHıR sunucusunu aboneliğinize dağıttınız. Postman kullanarak FHıR API 'sine nasıl erişebileceğinizi öğrenmek için Postman öğreticisine geçin.
 
>[!div class="nextstepaction"]
>[Postman kullanarak FHıR API 'sine erişme](access-fhir-postman-tutorial.md)
