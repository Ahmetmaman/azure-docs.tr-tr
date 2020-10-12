---
title: Azure Işlevleri çalışma zamanı sürümlerini hedefleme
description: Azure Işlevleri, çalışma zamanının birden çok sürümünü destekler. Azure 'da barındırılan bir işlev uygulamasının çalışma zamanı sürümünü belirtmeyi öğrenin.
ms.topic: conceptual
ms.date: 07/22/2020
ms.openlocfilehash: a7d86ef26d50d60389ae09bf3245ed97fea2c3e3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88926584"
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Azure Işlevleri çalışma zamanı sürümlerini hedefleme

Bir işlev uygulaması, Azure Işlevleri çalışma zamanının belirli bir sürümünde çalışır. Üç ana sürüm vardır: [1. x, 2. x ve 3. x](functions-versions.md). Varsayılan olarak, işlev uygulamaları çalışma zamanının sürüm 3. x ' de oluşturulur. Bu makalede, Azure 'da bir işlev uygulamasının seçtiğiniz sürümde çalışacak şekilde nasıl yapılandırılacağı açıklanmaktadır. Belirli bir sürüm için yerel bir geliştirme ortamının nasıl yapılandırılacağı hakkında bilgi için bkz. [Code ve Azure işlevlerini yerel olarak test](functions-run-local.md)etme.

## <a name="automatic-and-manual-version-updates"></a>Otomatik ve el ile sürüm güncelleştirmeleri

Azure Işlevleri, bir işlev uygulamasındaki uygulama ayarını kullanarak çalışma zamanının belirli bir sürümünü hedeflemenizi sağlar `FUNCTIONS_EXTENSION_VERSION` . İşlev uygulaması, açıkça yeni bir sürüme taşımayı seçinceye kadar belirtilen ana sürümde tutulur. Yalnızca ana sürümü belirtirseniz, işlev uygulaması kullanılabilir hale geldiğinde çalışma zamanının yeni ikincil sürümlerine otomatik olarak güncelleştirilir. Yeni ikincil sürümler, son değişiklikleri sunmamalıdır. 

Küçük bir sürüm belirtirseniz (örneğin, "2.0.12345"), işlev uygulaması, açıkça değiştirene kadar bu belirli sürüme sabitlenmiştir. Eski küçük sürümler düzenli olarak üretim ortamından kaldırılır. Bu durum oluştuktan sonra, işlev uygulamanız içinde ayarlanan sürüm yerine en son sürümde çalışır `FUNCTIONS_EXTENSION_VERSION` . Bu nedenle, önemli sürümü hedeflemesini sağlamak için, belirli bir alt sürüm gerektiren işlev uygulamanızla ilgili tüm sorunları hızlıca çözmeniz gerekir. Küçük sürüm kaldırma işlemleri [App Service bildirilerinde](https://github.com/Azure/app-service-announcements/issues)duyurulur.

> [!NOTE]
> Azure Işlevlerinin belirli bir ana sürümüne sabitleyebilir ve sonra Visual Studio 'Yu kullanarak Azure 'a yayımlamayı denerseniz, en son sürüme güncelleştirmenizi isteyip istemediğinizi soran bir iletişim kutusu penceresi açılır veya yayımlamayı iptal edersiniz. Bunu önlemek için, `<DisableFunctionExtensionVersionUpdate>true</DisableFunctionExtensionVersionUpdate>` dosyanıza özelliği ekleyin `.csproj` .

Yeni bir sürüm herkese açık olduğunda, portalda bir istem bu sürüme kadar ilerme şansı sağlar. Yeni bir sürüme taşıdıktan sonra, `FUNCTIONS_EXTENSION_VERSION` uygulama ayarını her zaman önceki bir sürüme geri gitmek için kullanabilirsiniz.

Aşağıdaki tabloda, `FUNCTIONS_EXTENSION_VERSION` otomatik güncelleştirmeleri etkinleştirmek için her ana sürüm için değerler gösterilmektedir:

| Ana sürüm | `FUNCTIONS_EXTENSION_VERSION` deeri |
| ------------- | ----------------------------------- |
| 3.x  | `~3` |
| 2.x  | `~2` |
| 'in  | `~1` |

Çalışma zamanı sürümünde yapılan bir değişiklik, bir işlev uygulamasının yeniden başlatılmasına neden olur.

## <a name="view-and-update-the-current-runtime-version"></a>Geçerli çalışma zamanı sürümünü görüntüle ve Güncelleştir

İşlev uygulamanız tarafından kullanılan çalışma zamanı sürümünü değiştirebilirsiniz. Son değişiklikler nedeniyle, işlev uygulamanızda herhangi bir işlev oluşturmadan önce çalışma zamanı sürümünü değiştirebilirsiniz. 

> [!IMPORTANT]
> Çalışma zamanı sürümü ayarı tarafından belirlendiği halde `FUNCTIONS_EXTENSION_VERSION` , ayarı doğrudan değiştirip Azure Portal bu değişikliği yapmanız gerekir. Bunun nedeni, portalın yaptığınız değişiklikleri doğrulaması ve ilgili diğer değişiklikleri gerekli hale getirir.

# <a name="portal"></a>[Portal](#tab/portal)

[!INCLUDE [Set the runtime version in the portal](../../includes/functions-view-update-version-portal.md)]

> [!NOTE]
> Azure portal kullanarak, zaten işlevleri bulunan bir işlev uygulamasının çalışma zamanı sürümünü değiştiremezsiniz.

# <a name="azure-cli"></a>[Azure CLI](#tab/azurecli)

Ayrıca, Azure CLı 'dan ' i görüntüleyebilir ve ayarlayabilirsiniz `FUNCTIONS_EXTENSION_VERSION` .  

Azure CLı 'yı kullanarak, [az functionapp config appSettings set](/cli/azure/functionapp/config/appsettings) komutuyla geçerli çalışma zamanı sürümünü görüntüleyin.

```azurecli-interactive
az functionapp config appsettings list --name <function_app> \
--resource-group <my_resource_group>
```

Bu kodda, öğesini `<function_app>` işlev uygulamanızın adıyla değiştirin. Ayrıca `<my_resource_group>` , işlev uygulamanız için kaynak grubunun adıyla değiştirin. 

`FUNCTIONS_EXTENSION_VERSION`Netme için kesilmiş olan aşağıdaki çıktıda görürsünüz:

```output
[
  {
    "name": "FUNCTIONS_EXTENSION_VERSION",
    "slotSetting": false,
    "value": "~2"
  },
  {
    "name": "FUNCTIONS_WORKER_RUNTIME",
    "slotSetting": false,
    "value": "dotnet"
  },
  
  ...
  
  {
    "name": "WEBSITE_NODE_DEFAULT_VERSION",
    "slotSetting": false,
    "value": "8.11.1"
  }
]
```

`FUNCTIONS_EXTENSION_VERSION`İşlev uygulamasındaki ayarı [az functionapp config appSettings set](/cli/azure/functionapp/config/appsettings) komutuyla güncelleştirebilirsiniz.

```azurecli-interactive
az functionapp config appsettings set --name <FUNCTION_APP> \
--resource-group <RESOURCE_GROUP> \
--settings FUNCTIONS_EXTENSION_VERSION=<VERSION>
```

`<FUNCTION_APP>`İşlev uygulamanızın adıyla değiştirin. Ayrıca `<RESOURCE_GROUP>` , işlev uygulamanız için kaynak grubunun adıyla değiştirin. Ayrıca, öğesini `<VERSION>` belirli bir sürümle veya, ya da ile değiştirin `~3` `~2` `~1` .

Yukarıdaki kod örneğinde **deneyin** ' i seçerek bu komutu [Azure Cloud Shell](../cloud-shell/overview.md) çalıştırabilirsiniz. Ayrıca, oturum açmak için [az Login](/cli/azure/reference-index#az-login) komutunu çalıştırdıktan sonra bu komutu yürütmek IÇIN [Azure CLI 'yı yerel olarak](/cli/azure/install-azure-cli) da kullanabilirsiniz.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Azure Işlevleri çalışma zamanını denetlemek için aşağıdaki cmdlet 'i kullanın: 

```powershell
Get-AzFunctionAppSetting -Name "<FUNCTION_APP>" -ResourceGroupName "<RESOURCE_GROUP>"
```

`<FUNCTION_APP>`İşlev uygulamanızın adıyla değiştirin `<RESOURCE_GROUP>` . Ayarın geçerli değeri, `FUNCTIONS_EXTENSION_VERSION` karma tablosunda döndürülür.

Işlevler çalışma zamanını değiştirmek için aşağıdaki betiği kullanın:

```powershell
Update-AzFunctionAppSetting -Name "<FUNCTION_APP>" -ResourceGroupName "<RESOURCE_GROUP>" -AppSetting @{"FUNCTIONS_EXTENSION_VERSION" = "<VERSION>"} -Force
```

Daha önce olduğu gibi, `<FUNCTION_APP>` işlev uygulamanızın adıyla ve `<RESOURCE_GROUP>` kaynak grubunun adıyla değiştirin. Ayrıca, ya da gibi `<VERSION>` belirli bir sürüm veya ana sürümle değiştirin `~2` `~3` . `FUNCTIONS_EXTENSION_VERSION`Döndürülen karma tablosunda ayarının güncelleştirilmiş değerini doğrulayabilirsiniz. 

---

İşlev uygulaması, uygulama ayarında değişiklik yapıldıktan sonra yeniden başlatılır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yerel geliştirme ortamınızda 2,0 çalışma zamanını hedefleyin](functions-run-local.md)

> [!div class="nextstepaction"]
> [Bkz. çalışma zamanı sürümleri için sürüm notları](https://github.com/Azure/azure-webjobs-sdk-script/releases)
