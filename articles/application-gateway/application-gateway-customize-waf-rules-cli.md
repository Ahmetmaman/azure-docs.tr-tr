---
title: Azure Application Gateway'de - Azure CLI Web uygulaması güvenlik duvarı kurallarını özelleştirme | Microsoft Docs
description: Bu makalede, Azure CLI ile Application Gateway içindeki web uygulaması güvenlik duvarı kurallarını özelleştirme hakkında bilgi sağlar.
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: ''
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: victorh
ms.openlocfilehash: 95eb0ef48f3e0cb6e835dc0582cc652f06315d44
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2019
ms.locfileid: "55992866"
---
# <a name="customize-web-application-firewall-rules-through-the-azure-cli"></a>Azure CLI aracılığıyla Web uygulaması güvenlik duvarı kurallarını özelleştirme

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-customize-waf-rules-portal.md)
> * [PowerShell](application-gateway-customize-waf-rules-powershell.md)
> * [Azure CLI](application-gateway-customize-waf-rules-cli.md)

Azure Application Gateway web uygulaması Güvenlik Duvarı (WAF), web uygulamaları için koruma sağlar. Bu korumalar, açık Web uygulaması güvenlik Project (OWASP) çekirdek kural kümesi (CRS tarafından) sağlanır. Bazı kurallar, hatalı pozitif sonuçları neden ve gerçek trafiği engelleyin. Bu nedenle, uygulama ağ geçidi kural gruplarının ve kuralların özelleştirme yeteneği sağlar. Belirli bir kural gruplarının ve kuralların hakkında daha fazla bilgi için bkz. [web uygulaması güvenlik duvarı CRS kural gruplarının ve kuralların listesi](application-gateway-crs-rulegroups-rules.md).

## <a name="view-rule-groups-and-rules"></a>Görünüm kural gruplarının ve kuralların

Aşağıdaki kod örnekleri, kuralları ve yapılandırılabilir bir kural gruplarını görüntüleme gösterilmektedir.

### <a name="view-rule-groups"></a>Kural grupları görüntüleyin

Aşağıdaki örnek, kural gruplarını görüntülemek gösterilmektedir:

```azurecli-interactive
az network application-gateway waf-config list-rule-sets --type OWASP
```

Aşağıdaki çıktı, önceki örnekte kesilmiş bir yanıt.

```json
[
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_3.0",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "REQUEST-910-IP-REPUTATION",
        "rules": null
      },
      ...
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  },
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_2.2.9",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
   "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "crs_20_protocol_violations",
        "rules": null
      },
      ...
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "2.2.9",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  }
]
```

### <a name="view-rules-in-a-rule-group"></a>Bir kural gruptaki kurallarını görüntüle

Aşağıdaki örnek, belirtilen kural grupta kurallarını görüntülemek gösterilmektedir:

```azurecli-interactive
az network application-gateway waf-config list-rule-sets --group "REQUEST-910-IP-REPUTATION"
```

Aşağıdaki çıktı, önceki örnekte kesilmiş bir yanıt.

```json
[
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_3.0",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "REQUEST-910-IP-REPUTATION",
        "rules": [
          {
            "description": "Rule 910011",
            "ruleId": 910011
          },
          ...
        ]
      }
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  }
]
```

## <a name="disable-rules"></a>Kuralları devre dışı bırak

Aşağıdaki örnek, kuralları devre dışı bırakır `910018` ve `910017` bir uygulama ağ geçidi:

```azurecli-interactive
az network application-gateway waf-config set --resource-group AdatumAppGatewayRG --gateway-name AdatumAppGateway --enabled true --rule-set-version 3.0 --disabled-rules 910018 910017
```

## <a name="next-steps"></a>Sonraki adımlar

Devre dışı kurallarınızı yapılandırdıktan sonra WAF günlükleri görüntüleme hakkında bilgi edinebilirsiniz. Daha fazla bilgi için [Application Gateway tanılama](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
