---
title: Azure klasik CLı kullanarak Redsıs için Azure önbelleğini yönetme
description: Azure klasik CLı 'yı dilediğiniz platforma yüklemeyi, Azure hesabınıza bağlanmak için nasıl kullanacağınızı ve klasik CLı 'dan Reda için Azure önbelleği oluşturma ve yönetme hakkında bilgi edinin.
author: yegu-ms
ms.service: cache
ms.topic: conceptual
ms.date: 01/23/2017
ms.author: yegu
ms.custom: devx-track-azurecli
ms.openlocfilehash: 7643f882d5ac330046c169e0a3f2fa4920331d4e
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92537703"
---
# <a name="how-to-create-and-manage-azure-cache-for-redis-using-the-azure-classic-cli"></a>Azure klasik CLı kullanılarak Redsıs için Azure önbelleği oluşturma ve yönetme
> [!div class="op_single_selector"]
> * [PowerShell](cache-how-to-manage-redis-cache-powershell.md)
> * [Azure klasik CLı](cache-manage-cli.md)
>

Azure klasik CLı, Azure altyapınızı herhangi bir platformdan yönetmenin harika bir yoludur. Bu makalede, Azure klasik CLı kullanarak Redsıs örnekleri için Azure önbelleğinizi oluşturma ve yönetme işlemi gösterilmektedir.

[!INCLUDE [outdated-cli-content](../../includes/contains-classic-cli-content.md)]
> [!NOTE]
> En son Azure CLı örnek betikleri için bkz. [redsıs örnekleri Için Azure CLI Azure önbelleği](cli-samples.md).

## <a name="prerequisites"></a>Önkoşullar
Azure klasik CLı kullanarak Redsıs örnekleri için Azure önbelleği oluşturup yönetmek için aşağıdaki adımları gerçekleştirmeniz gerekir.

* Azure hesabınız olmalıdır. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir [Hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturabilirsiniz.
* [Azure klasık CLI 'Yı yükler](/cli/azure/install-classic-cli).
* Azure CLı yüklemenizi bir kişisel Azure hesabıyla veya bir iş ya da okul Azure hesabıyla bağlayın ve komutunu kullanarak klasik CLı 'dan oturum açın `azure login` .
* Aşağıdaki komutlardan herhangi birini çalıştırmadan önce, komutunu çalıştırarak klasik CLı 'yı Kaynak Yöneticisi moduna geçirin `azure config mode arm` . Daha fazla bilgi için bkz. Azure [klasık CLI kullanarak Azure kaynaklarını ve kaynak gruplarını yönetme](../azure-resource-manager/management/manage-resources-cli.md).

## <a name="azure-cache-for-redis-properties"></a>Redsıs özellikleri için Azure önbelleği
Aşağıdaki özellikler, Redsıs örnekleri için Azure önbelleği oluşturma ve güncelleştirme sırasında kullanılır.

| Özellik | Anahtar | Açıklama |
| --- | --- | --- |
| name |-n,--ad |Redsıs için Azure önbelleğinin adı. |
| kaynak grubu |-g,--Resource-Group |Kaynak grubunun adı. |
| location |-l,--konum |Önbellek oluşturma konumu. |
| boyut |-z,--boyut |Redsıs için Azure önbelleğinin boyutu. Geçerli değerler: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4] |
| isteyin |-x,--SKU |Redis SKU. Şunlardan biri olmalıdır: [temel, standart, Premium] |
| EnableNonSslPort |-e,--etkinleştir-SSL olmayan-bağlantı noktası |Redin için Azure önbelleğinin EnableNonSslPort özelliği. Önbelleğiniz için TLS olmayan/SSL bağlantı noktasını etkinleştirmek istiyorsanız bu bayrağı ekleyin |
| Redsıs yapılandırması |-c,--redsıs-yapılandırma |Redsıs yapılandırması. Yapılandırma anahtarları ve değerleri için JSON biçimli bir dize girin. Biçim: "{" ":" "," ":" "}" |
| Redsıs yapılandırması |-f,--redsıs-yapılandırma-dosya |Redsıs yapılandırması. Yapılandırma anahtarlarını ve değerlerini içeren bir dosyanın yolunu buraya girin. Dosya girişi için biçim: {"": "", "": ""} |
| Parça sayısı |-r,--parça-sayısı |Kümeleme ile Premium küme önbelleğinde oluşturulacak parça sayısı. |
| Sanal Ağ |-v,--sanal-ağ |Önbelleğinizi VNET 'te barındırırken, redin için Azure önbelleğini dağıtmak üzere sanal ağın tam ARM kaynak KIMLIĞINI belirtir. Örnek biçim:/subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| anahtar türü |-t,--anahtar-tür |Yenilenecek anahtar türü. Geçerli değerler: [birincil, Ikincil] |
| Staticıp |-p,--static-IP \<static-ip\> |Önbelleğinizi VNET 'te barındırırken, önbelleğin alt ağında benzersiz bir IP adresi belirtir. Sağlanmazsa, alt ağdan bir tane seçilir. |
| Alt ağ |t,--alt ağ \<subnet\> |Önbelleğinizi VNET 'te barındırırken, önbelleğin dağıtılacağı alt ağın adını belirtir. |
| VirtualNetwork |-v,--sanal-ağ \<virtual-network\> |Önbelleğinizi VNET 'te barındırırken, redin için Azure önbelleğini dağıtmak üzere sanal ağın tam ARM kaynak KIMLIĞINI belirtir. Örnek biçim:/subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Abonelik |-s,--abonelik |Abonelik tanımlayıcısı. |

## <a name="see-all-azure-cache-for-redis-commands"></a>Redsıs komutları için tüm Azure önbelleğine bakın
Redsıs komutları ve parametreleri için tüm Azure önbelleğini görmek için `azure rediscache -h` komutunu kullanın.

```azurecli
C:\>azure rediscache -h
help:    Commands to manage your Azure Cache for Redis(s)
help:
help:    Create an Azure Cache for Redis
help:      rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
help:
help:    Delete an existing Azure Cache for Redis
help:      rediscache delete [--name <name> --resource-group <resource-group> ]
help:
help:    List all Azure Cache for Redis within your Subscription or Resource Group
help:      rediscache list [options]
help:
help:    Show properties of an existing Azure Cache for Redis
help:      rediscache show [--name <name> --resource-group <resource-group>]
help:
help:    Change settings of an existing Azure Cache for Redis
help:      rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
help:
help:    Renew the authentication key for an existing Azure Cache for Redis
help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
help:
help:    Lists Primary and Secondary key of an existing Azure Cache for Redis
help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
help:
help:    Options:
help:      -h, --help  output usage information
help:
help:    Current Mode: arm (Azure Resource Management)
```

## <a name="create-an-azure-cache-for-redis"></a>Redis için Azure Cache oluşturma
Redsıs için bir Azure önbelleği oluşturmak üzere aşağıdaki komutu kullanın:

```azurecli
    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
```

Bu komut hakkında daha fazla bilgi için komutunu çalıştırın `azure rediscache create -h` .

```azurecli
C:\>azure rediscache create -h
help:    Create an Azure Cache for Redis
help:
help:    Usage: rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
help:
help:    Options:
help:      -h, --help                                               output usage information
help:      -v, --verbose                                            use verbose output
help:      -vv                                                      more verbose with debug output
help:      --json                                                   use json output
help:      -n, --name <name>                                        Name of the Azure Cache for Redis.
help:      -g, --resource-group <resource-group>                    Name of the Resource Group
help:      -l, --location <location>                                Location to create cache.
help:      -z, --size <size>                                        Size of the Azure Cache for Redis. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of the Azure Cache for Redis. Add this flag if you want to enable the non-TLS/SSL Port for your cache
help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here. Format for the file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
help:      -r, --shard-count <shard-count>                          Number of Shards to create on a Premium Cluster Cache
help:      -v, --virtual-network <virtual-network>                  The exact ARM resource ID of the virtual network to deploy the Azure Cache for Redis in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
help:      -t, --subnet <subnet>                                    Required when deploying an Azure Cache for Redis inside an existing Azure Virtual Network
help:      -p, --static-ip <static-ip>                              Required when deploying an Azure Cache for Redis inside an existing Azure Virtual Network
help:      -s, --subscription <id>                                  the subscription identifier
help:
help:    Current Mode: arm (Azure Resource Management)
```

## <a name="delete-an-existing-azure-cache-for-redis"></a>Redsıs için mevcut bir Azure önbelleğini silme
Redsıs için bir Azure önbelleğini silmek üzere aşağıdaki komutu kullanın:

```azurecli
    azure rediscache delete [--name <name> --resource-group <resource-group> ]
```

Bu komut hakkında daha fazla bilgi için komutunu çalıştırın `azure rediscache delete -h` .

```azurecli
C:\>azure rediscache delete -h
help:    Delete an existing Azure Cache for Redis
help:
help:    Usage: rediscache delete [--name <name> --resource-group <resource-group> ]
help:
help:    Options:
help:      -h, --help                             output usage information
help:      -v, --verbose                          use verbose output
help:      -vv                                    more verbose with debug output
help:      --json                                 use json output
help:      -n, --name <name>                      Name of the Azure Cache for Redis.
help:      -g, --resource-group <resource-group>  Name of the Resource Group under which the cache exists
help:      -s, --subscription <subscription>      the subscription identifier
help:
help:    Current Mode: arm (Azure Resource Management)
```

## <a name="list-all-azure-cache-for-redis-within-your-subscription-or-resource-group"></a>Tüm redin Azure önbelleğini aboneliğiniz veya kaynak grubunuz dahilinde listeleyin
Tüm redin Azure önbelleğini aboneliğiniz veya kaynak grubunuz dahilinde listelemek için aşağıdaki komutu kullanın:

```azurecli
    azure rediscache list [options]
```

Bu komut hakkında daha fazla bilgi için komutunu çalıştırın `azure rediscache list -h` .

```azurecli
C:\>azure rediscache list -h
help:    List all Azure Cache for Redis within your Subscription or Resource Group
help:
help:    Usage: rediscache list [options]
help:
help:    Options:
help:      -h, --help                             output usage information
help:      -v, --verbose                          use verbose output
help:      -vv                                    more verbose with debug output
help:      --json                                 use json output
help:      -g, --resource-group <resource-group>  Name of the Resource Group
help:      -s, --subscription <subscription>      the subscription identifier
help:
help:    Current Mode: arm (Azure Resource Management)
```

## <a name="show-properties-of-an-existing-azure-cache-for-redis"></a>Redsıs için mevcut bir Azure önbelleğinin özelliklerini göster
Redsıs için mevcut bir Azure önbelleğinin özelliklerini göstermek üzere aşağıdaki komutu kullanın:

```azurecli
    azure rediscache show [--name <name> --resource-group <resource-group>]
```

Bu komut hakkında daha fazla bilgi için komutunu çalıştırın `azure rediscache show -h` .

```azurecli
C:\>azure rediscache show -h
help:    Show properties of an existing Azure Cache for Redis
help:
help:    Usage: rediscache show [--name <name> --resource-group <resource-group>]
help:
help:    Options:
help:      -h, --help                             output usage information
help:      -v, --verbose                          use verbose output
help:      -vv                                    more verbose with debug output
help:      --json                                 use json output
help:      -n, --name <name>                      Name of the Azure Cache for Redis.
help:      -g, --resource-group <resource-group>  Name of the Resource Group
help:      -s, --subscription <subscription>      the subscription identifier
help:
help:    Current Mode: arm (Azure Resource Management)
```

<a name="scale"></a>

## <a name="change-settings-of-an-existing-azure-cache-for-redis"></a>Redsıs için mevcut bir Azure önbelleğinin ayarlarını değiştirin
Redu için mevcut bir Azure önbelleğinin ayarlarını değiştirmek için aşağıdaki komutu kullanın:

```azurecli
    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
```

Bu komut hakkında daha fazla bilgi için komutunu çalıştırın `azure rediscache set -h` .

```azurecli
C:\>azure rediscache set -h
help:    Change settings of an existing Azure Cache for Redis
help:
help:    Usage: rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
help:
help:    Options:
help:      -h, --help                                               output usage information
help:      -v, --verbose                                            use verbose output
help:      -vv                                                      more verbose with debug output
help:      --json                                                   use json output
help:      -n, --name <name>                                        Name of the Azure Cache for Redis.
help:      -g, --resource-group <resource-group>                    Name of the Resource Group
help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here.
help:      -s, --subscription <subscription>                        the subscription identifier
help:
help:    Current Mode: arm (Azure Resource Management)
```

## <a name="renew-the-authentication-key-for-an-existing-azure-cache-for-redis"></a>Redsıs için mevcut bir Azure önbelleğinin kimlik doğrulama anahtarını yenileyin
Redin için mevcut bir Azure önbelleğinin kimlik doğrulama anahtarını yenilemek için aşağıdaki komutu kullanın:

```azurecli
    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]
```

`Primary`İçin veya `Secondary` belirtin `key-type` .

Bu komut hakkında daha fazla bilgi için komutunu çalıştırın `azure rediscache renew-key -h` .

```azurecli
C:\>azure rediscache renew-key -h
help:    Renew the authentication key for an existing Azure Cache for Redis
help:
help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
help:
help:    Options:
help:      -h, --help                             output usage information
help:      -v, --verbose                          use verbose output
help:      -vv                                    more verbose with debug output
help:      --json                                 use json output
help:      -n, --name <name>                      Name of the Azure Cache for Redis.
help:      -g, --resource-group <resource-group>  Name of the Resource Group under which cache exists
help:      -t, --key-type <key-type>              type of key to renew. Valid values are: 'Primary', 'Secondary'.
help:      -s, --subscription <subscription>      the subscription identifier
help:
help:    Current Mode: arm (Azure Resource Management)
```

## <a name="list-primary-and-secondary-keys-of-an-existing-azure-cache-for-redis"></a>Redsıs için mevcut bir Azure önbelleğinin birincil ve Ikincil anahtarlarını listeleyin
Redu için mevcut bir Azure önbelleğinin birincil ve Ikincil anahtarlarını listelemek için aşağıdaki komutu kullanın:

```azurecli
    azure rediscache list-keys [--name <name> --resource-group <resource-group>]
```

Bu komut hakkında daha fazla bilgi için komutunu çalıştırın `azure rediscache list-keys -h` .

```azurecli
C:\>azure rediscache list-keys -h
help:    Lists Primary and Secondary key of an existing Azure Cache for Redis
help:
help:    Usage: rediscache list-keys [--name <name> --resource-group <resource-group>]
help:
help:    Options:
help:      -h, --help                             output usage information
help:      -v, --verbose                          use verbose output
help:      -vv                                    more verbose with debug output
help:      --json                                 use json output
help:      -n, --name <name>                      Name of the Azure Cache for Redis.
help:      -g, --resource-group <resource-group>  Name of the Resource Group under which Cache exists
help:      -s, --subscription <subscription>      the subscription identifier
help:
help:    Current Mode: arm (Azure Resource Management)
```