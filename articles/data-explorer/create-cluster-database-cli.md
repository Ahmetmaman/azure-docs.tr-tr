---
title: 'Hızlı Başlangıç: CLI kullanarak bir Azure Veri Gezgini kümesi ile veritabanı oluşturma'
description: Azure CLI kullanarak bir Azure Veri Gezgini küme ve veritabanı oluşturmayı öğrenin
services: data-explorer
author: radennis
ms.author: radennis
ms.reviewer: orspod
ms.service: data-explorer
ms.topic: quickstart
ms.date: 2/4/2019
ms.openlocfilehash: 357f0efcf7300545d10113c92702d9fed4aad049
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56958031"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-the-cli"></a>CLI kullanarak bir Azure Veri Gezgini kümesi ile veritabanı oluşturma

Bu hızlı başlangıçta, Azure CLI kullanarak bir Azure Veri Gezgini kümesi ile veritabanı oluşturma işlemini açıklar.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak bir Azure aboneliğinizin olması gerekir. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLI'yı yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıçta Azure CLI 2.0.4 sürüm gerektirir veya üzeri. Sürümünüzü kontrol etmek için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="configure-the-cli-parameters"></a>CLI parametreleri Yapılandır

Komutları Azure Cloud Shell'de çalıştırıyorsanız, aşağıdaki adımları gerekli değildir. CLI'yi yerel olarak çalıştırıyorsanız, Azure'da oturum açın ve geçerli aboneliğinizi ayarlamak için aşağıdaki adımları izleyin:

1. Azure'da oturum açmak için aşağıdaki komutu çalıştırın:

    ```azurecli-interactive
    az login
    ```

2. Abonelik oluşturulacak kümenizi istediğiniz ayarlayın. Değiştirin `MyAzureSub` kullanmak istediğiniz Azure aboneliği adı:

    ```azurecli-interactive
    az account set --subscription MyAzureSub
    ```

## <a name="create-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesi oluşturma

1. Aşağıdaki komutu kullanarak kümenizi oluşturun:

    ```azurecli-interactive
    az kusto cluster create --name azureclitest --sku D11_v2 --resource-group testrg
    ```

   |**Ayar** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | ad | *azureclitest* | İstenen kümenizin adıdır.|
   | sku | *D13_v2* | Kümeniz için kullanılan SKU. |
   | resource-group | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |

    Küme kapasitesi gibi kullanabileceğiniz ek isteğe bağlı parametre yok.

2. Kümenizi başarıyla oluşturulup oluşturulmadığını kontrol etmek için aşağıdaki komutu çalıştırın:

    ```azurecli-interactive
    az kusto cluster show --name azureclitest --resource-group testrg
    ```

Sonuç içeriyorsa `provisioningState` ile `Succeeded` değer sonra küme başarıyla oluşturuldu.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümede veritabanı oluşturma

1. Aşağıdaki komutu kullanarak veritabanınızı oluşturun:

    ```azurecli-interactive
    az kusto database create --cluster-name azureclitest --name clidatabase --resource-group testrg --soft-delete-period 3650:00:00:00 --hot-cache-period 3650:00:00:00
    ```

   |**Ayar** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | Küme adı | *azureclitest* | Veritabanının oluşturulacağı, kümenizin adıdır.|
   | ad | *clidatabase* | Veritabanınızın adı.|
   | resource-group | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |
   | Geçici silme süresi | *3650:00:00:00* | Verileri sorgulamak kullanılabilen tutulacak süre miktarı. |
   | Sık erişimli-cache-süresi | *3650:00:00:00* | Veriler önbellekte tutulacak süre miktarı. |

2. Oluşturduğunuz veritabanını görmek için aşağıdaki komutu çalıştırın:

    ```azurecli-interactive
    az kusto database show --name clidatabase --resource-group testrg --cluster-name azureclitest
    ```

Artık bir küme ve bir veritabanı vardır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

* Diğer hızlı başlangıçlarımızı ve öğreticilerimizi izlemeyi planlıyorsanız, oluşturduğunuz kaynakları tutun.
* Kaynakları temizlemek için kümeyi silin. Bir küme sildiğinizde, tüm veritabanları da siler. Kümenizi silmek için aşağıdaki komutu kullanın:

    ```azurecli-interactive
    az kusto cluster delete --name azureclitest --resource-group testrg
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Azure Veri Gezgini Python kitaplığı kullanarak veri alma](python-ingest-data.md)
