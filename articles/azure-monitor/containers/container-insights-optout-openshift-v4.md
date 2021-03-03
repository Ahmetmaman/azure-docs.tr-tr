---
title: Azure ve Red Hat OpenShift v4 kümenizi izlemeyi durdurma | Microsoft Docs
description: Bu makalede, kapsayıcı öngörülerine sahip Azure Red Hat OpenShift ve Red Hat OpenShift sürüm 4 kümenizin izlenmesini nasıl durdurulabileceğinizi açıklanmaktadır.
ms.topic: conceptual
ms.date: 04/24/2020
ms.openlocfilehash: 09ca05a25ce9bb02b8a3d515acf060e2e9e7e8c2
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101731808"
---
# <a name="how-to-stop-monitoring-your-azure-and-red-hat-openshift-v4-cluster"></a>Azure ve Red Hat OpenShift v4 kümenizi izlemeyi durdurma

Azure Red Hat OpenShift ve Red Hat OpenShift sürüm 4. x kümesini izlemeyi etkinleştirdikten sonra, artık bunu izlemek istediğinize karar verirseniz, kümeyi kapsayıcı öngörüleri ile izlemeyi durdurabilirsiniz. Bu makalede bunun nasıl yapılacağı gösterilmektedir.  

## <a name="how-to-stop-monitoring-using-helm"></a>Held kullanarak izlemeyi durdurma

1. Kümenizde yüklü olan Container Insights Held grafik sürümünü ilk olarak tanımlamak için aşağıdaki HELI komutunu çalıştırın.

    ```
    helm list
    ```

    Çıktı aşağıdakine benzeyecektir:

    ```
    NAME                            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                           APP VERSION
    azmon-containers-release-1      default         3               2020-04-21 15:27:24.1201959 -0700 PDT   deployed        azuremonitor-containers-2.7.0   7.0.0-1
    ```

    *Azmon-kapsayıcılar-Release-1* kapsayıcı öngörüleri için Helu grafik sürümünü temsil eder.

2. Grafik sürümünü silmek için aşağıdaki Held komutunu çalıştırın.

    `helm delete <releaseName>`

    Örnek:

    `helm delete azmon-containers-release-1`

    Bu işlem, yayını kümeden kaldırır. Komutunu çalıştırarak doğrulayabilirsiniz `helm list` :

    ```
    NAME                            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                           APP VERSION
    ```

Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Held, sildikten sonra bile yayınlarınızı izliyorsa, bir kümenin geçmişini denetleyebilir ve hatta ile bir yayını geri alabilirsiniz `helm rollback` .

## <a name="next-steps"></a>Sonraki adımlar

Log Analytics çalışma alanı yalnızca kümeyi izlemeyi desteklemek için oluşturulduysa ve artık gerekmiyorsa, el ile silmeniz gerekir. Bir çalışma alanının nasıl silineceği konusunda bilgi sahibi değilseniz bkz. [Azure Log Analytics çalışma alanını silme](../logs/delete-workspace.md).