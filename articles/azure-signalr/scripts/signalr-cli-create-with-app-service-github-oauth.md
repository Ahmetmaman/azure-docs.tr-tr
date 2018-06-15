---
title: Azure CLI Betik Örneği - SignalR Hizmeti ve GitHub kimlik doğrulamasını kullanan bir web uygulaması oluşturma | Microsoft Docs
description: Azure CLI Betik Örneği - SignalR Hizmeti ve GitHub kimlik doğrulamasını kullanan bir web uygulaması oluşturma
services: signalr
documentationcenter: signalr
author: wesmc7777
manager: cfowler
editor: ''
tags: azure-service-management
ms.service: signalr
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: signalr
ms.date: 04/22/2018
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: ec095572dc8fe20c913d543ffcf926355b5715df
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33766453"
---
# <a name="create-a-web-app-that-uses-signalr-service-and-github-authentication"></a>SignalR Hizmeti ve GitHub kimlik doğrulamasını kullanan bir web uygulaması oluşturma

Bu örnek betik, istemcilere gerçek zamanlı içerik güncelleştirmeleri göndermek için kullanılan yeni bir Azure SignalR Hizmeti kaynağı oluşturur. Bu betik, SignalR Hizmetini kullanan ASP.NET Core Web Uygulamanızı barındırmak için yeni bir Web Uygulaması ve Uygulama Hizmeti planı da ekler. Web uygulaması, yeni SignalR hizmet kaynağına bağlanmak ve [GitHub kimlik doğrulaması](https://developer.github.com/v3/guides/basics-of-authentication/) ile kimlik doğrulaması yapmak için uygulama ayarlarıyla birlikte yapılandırılır. Web uygulaması ayrıca bir yerel git deposu dağıtım kaynağını kullanacak şekilde de yapılandırılır.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik

Bu betik, Azure CLI için *signalr* uzantısını kullanır. Bu örnek betiği kullanmadan önce, aşağıdaki komutu yürüterek Azure CLI için *signalr* uzantısını yükleyin:

```azurecli-interactive
az extension add -n signalr
```

[!code-azurecli-interactive[main](../../../cli_scripts/azure-signalr/create-signalr-with-app-service-github-oauth/create-signalr-with-app-service-github-oauth.sh "Create a new SignalR Service and Web App configured to use SignalR, GitHub OAuth, and local Git repository deployment source.")]

Yeni kaynak grubu için oluşturulan gerçek adı not edin. Çıktıda gösterilecektir. Tüm grup kaynaklarını silmek istediğinizde bu kaynak grubu adını kullanacaksınız.

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Tablodaki her komut, komuta özgü belgelere yönlendirir. Bu betik şu komutları kullanır:

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az signalr create](/cli/azure/signalr#az-signalr-create) | Bir Azure SignalR Hizmeti kaynağı oluşturur. |
| [az signalr key list](/cli/azure/signalr/key#az-signalr-key-list) | SignalR ile gerçek zamanlı içerik güncelleştirmeleri gönderilirken uygulamanız tarafından kullanılacak anahtarları listeler. |
| [az appservice plan create](/cli/azure/appservice/plan#az-appservice-plan-create) | Web uygulamalarını barındırmak için bir Azure App Service Planı oluşturur. |
| [az webapp create](/cli/azure/webapp#az-webapp-create) | App Service barındırma planını kullanarak bir Azure Web uygulaması oluşturur. |
| [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#az-webapp-config-appsettings-set) | Web uygulaması için yeni uygulama ayarları ekler. Bu uygulama ayarları, SignalR bağlantı dizesini ve GitHub OAuth uygulama gizli dizilerini depolamak için kullanılır. |
| [az webapp deployment user set](/cli/azure/webapp/deployment/user#az-webapp-deployment-user-set) | Dağıtım kimlik bilgilerini güncelleştirir. |
| [az webapp deployment source config-local-git](/cli/azure/webapp/deployment/source#az-webapp-deployment-source-config-local-git) | Web uygulaması dağıtımı için kopyalanıp gönderilecek bir git deposu uç noktası için URL alın. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek Azure SignalR Hizmeti CLI betik örnekleri, [Azure SignalR Hizmeti belgelerinde](../signalr-cli-samples.md) bulunabilir.
