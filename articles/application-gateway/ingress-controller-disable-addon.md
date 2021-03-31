---
title: Azure Kubernetes hizmet kümesi için Application Gateway ınress denetleyicisi eklentisini devre dışı bırakın ve yeniden etkinleştirin
description: Bu makalede, AKS kümeniz için AGIC eklentisinin devre dışı bırakılması ve yeniden etkinleştirilmesi hakkında bilgi verilmektedir
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: how-to
ms.date: 06/10/2020
ms.author: caya
ms.openlocfilehash: fe4da0435731c536a723cb2cb43428166456360b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "84807949"
---
# <a name="disable-and-re-enable-agic-add-on-for-your-aks-cluster"></a>AKS kümeniz için AGIC eklentisini devre dışı bırakıp yeniden etkinleştirin
AKS eklentisi olarak dağıtılan Application Gateway giriş denetleyicisi (AGIC), eklentiyi Azure CLı 'de tek bir satırla etkinleştirmenizi ve devre dışı bırakmanızı sağlar. Application Gateway yaşam döngüsü, belirsiz eklentiyi devre dışı bıraktığınızda, Application Gateway AGIC eklentisi tarafından oluşturulup oluşturulamadığını veya AGIC eklentisi 'nden ayrı olarak dağıtılıp dağıtılmadığını fark eder. Yeniden devre dışı bıraktığınızda veya var olan bir AKS kümesini kullanarak AGIC eklentisini etkinleştirmek veya Application Gateway için aynı komutu çalıştırabilirsiniz.

## <a name="disabling-agic-add-on-with-associated-application-gateway"></a>İlişkili Application Gateway AGIC eklentisini devre dışı bırakma 
AGIC eklentisi, her şeyi ilk kez ayarladığınızda Application Gateway otomatik olarak dağıtılırsa, varsayılan olarak AGIC eklentisini devre dışı bırakmak, Application Gateway her bir ölçüte göre siler. AGIC eklentisinin, devre dışı bıraktığınızda ilişkili Application Gateway silmesi gerekip gerekmediğini öğrenmek için aradığı iki ölçüt vardır:
- Application Gateway AGIC eklentisinin MC_ * node kaynak grubunda dağıtılan ile ilişkili olması mı? 
- AGIC eklentisinin ilişkili olduğu Application Gateway "created-by: ınress-appgw" etiketiyle ilişkilendirilmiş mi? Etiketi, Application Gateway eklenti tarafından dağıtılıp dağıtılmadığını öğrenmek için AGIC tarafından kullanılır. 

Her iki ölçüt de karşılanıyorsa, AGIC eklentisi eklenti devre dışı bırakıldığında oluşturduğu Application Gateway siler; Ancak, genel IP veya Application Gateway dağıtıldığı alt ağı silmez. İlk ölçüt karşılanmazsa, Application Gateway "created-by: ınress-appgw" etiketine sahip olup olmadığını değil, eklentinin devre dışı bırakılması Application Gateway silmez. Benzer şekilde, ikinci ölçüt karşılanmazsa, Application Gateway bu etikete sahip olmadığı için, eklentinin devre dışı bırakılması MC_ * düğüm kaynak grubundaki Application Gateway silmez. 

> [!TIP] 
> Eklentiyi devre dışı bırakırken Application Gateway silinmesini istemiyorsanız, ancak her iki ölçütü de karşılıyorsa, eklentinin Application Gateway silmesini engellemek için "created-by: ınress-appgw" etiketini kaldırın. 

AGIC eklentisini devre dışı bırakmak için aşağıdaki komutu çalıştırın: 
```azurecli-interactive
az aks disable-addons -n <AKS-cluster-name> -g <AKS-resource-group-name> -a ingress-appgw 
```

## <a name="enable-agic-add-on-on-existing-application-gateway-and-aks-cluster"></a>Mevcut Application Gateway ve AKS kümesinde AGIC eklentisini etkinleştir
AGIC eklentisini devre dışı bırakırsanız ve eklentiyi yeniden etkinleştirmeniz veya var olan bir Application Gateway ve AKS kümesini kullanarak eklentiyi etkinleştirmek istiyorsanız aşağıdaki komutu çalıştırın:

```azurecli-interactive
appgwId=$(az network application-gateway show -n <application-gateway-name> -g <resource-group-name> -o tsv --query "id") 
az aks enable-addons -n <AKS-cluster-name> -g <AKS-cluster-resource-group> -a ingress-appgw --appgw-id $appgwId
```

## <a name="next-steps"></a>Sonraki adımlar
Mevcut bir Application Gateway ve AKS kümesini kullanarak AGIC eklentisinin nasıl etkinleştirileceği hakkında daha fazla bilgi için bkz. [agic eklenti brownfield dağıtımı](tutorial-ingress-controller-add-on-existing.md).