---
title: Container Instances erişim
description: Azure Active Directory hizmet sorumlusu kullanarak Azure Container Instances özel kapsayıcı kayıt defterinizde görüntülere nasıl erişim sağlayacağınızı öğrenin.
ms.topic: article
ms.date: 04/23/2018
ms.openlocfilehash: b1bc8119c495dea99c6bdc4923db198d041a1e9e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "74456511"
---
# <a name="authenticate-with-azure-container-registry-from-azure-container-instances"></a>Azure Container Instances Azure Container Registry ile kimlik doğrulama

Azure Container Registry içindeki özel kapsayıcı kayıt defterlerine erişim sağlamak için bir Azure Active Directory (Azure AD) hizmet sorumlusu kullanabilirsiniz.

Bu makalede, Kayıt defterinize yönelik *çekme* Izinleriyle BIR Azure AD hizmet sorumlusu oluşturup yapılandırmayı öğreneceksiniz. Daha sonra, kimlik doğrulaması için hizmet sorumlusu kullanarak görüntüsünü özel Kayıt defterinizden alan Azure Container Instances (acı) içinde bir kapsayıcı başlatabilirsiniz.

## <a name="when-to-use-a-service-principal"></a>Hizmet sorumlusu ne zaman kullanılır?

Bir hizmet sorumlusu kullanarak, bir otomatik veya başka şekilde katılımsız bir şekilde kapsayıcı örnekleri oluşturan uygulama veya hizmetlerde gibi, **gözetimsiz senaryolarda**kimlik doğrulaması için bir hizmet sorumlusu kullanmanız gerekir.

Örneğin, gecelik çalıştıran bir otomatik betiğe sahipseniz ve bazı verileri işlemek için [görev tabanlı bir kapsayıcı örneği](../container-instances/container-instances-restart-policy.md) oluşturursa, kayıt defterinde kimlik doğrulamak için yalnızca çekme izinleriyle bir hizmet sorumlusu kullanabilir. Daha sonra hizmet sorumlusunun kimlik bilgilerini döndürebilir veya diğer hizmetleri ve uygulamaları etkilemeden erişimi tamamen iptal edebilirsiniz.

Hizmet sorumluları, kayıt defteri [Yönetici kullanıcısı](container-registry-authentication.md#admin-account) devre dışı olduğunda da kullanılmalıdır.

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

## <a name="authenticate-using-the-service-principal"></a>Hizmet sorumlusunu kullanarak kimlik doğrulama

Hizmet sorumlusu kullanarak Azure Container Instances bir kapsayıcı başlatmak için KIMLIĞINI `--registry-username` ve parolasını belirtin `--registry-password` .

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mycontainerregistry.azurecr.io/myimage:v1 \
    --registry-login-server mycontainerregistry.azurecr.io \
    --registry-username <service-principal-ID> \
    --registry-password <service-principal-password>
```

## <a name="sample-scripts"></a>Örnek betikler

Azure CLı için yukarıdaki örnek betikleri GitHub 'da ve Azure PowerShell için de bulabilirsiniz:

* [Azure CLI][acr-scripts-cli]
* [Azure PowerShell][acr-scripts-psh]

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde hizmet sorumluları ve ACR ile çalışma hakkında ek ayrıntılar yer alır:

* [Hizmet sorumluları ile kimlik doğrulamasını Azure Container Registry](container-registry-auth-service-principal.md)
* [Azure Kubernetes Service (AKS) Azure Container Registry ile kimlik doğrulama](../aks/cluster-container-registry-integration.md)

<!-- IMAGES -->

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
