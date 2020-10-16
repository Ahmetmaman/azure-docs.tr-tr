---
title: Azure Güvenlik Duvarı tehdit zekası yapılandırması
description: Azure Güvenlik Duvarı ilkenize yönelik tehdit zekası tabanlı filtreleme işlemlerini, ve bilinen kötü amaçlı IP adreslerinden ve etki alanlarından gelen trafiği uyaracak ve reddedecek şekilde nasıl yapılandıracağınızı öğrenin.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: article
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: a663c5f3bcf3492c4a9bc74fe93c6ed6a86137ee
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90530650"
---
# <a name="azure-firewall-threat-intelligence-configuration"></a>Azure Güvenlik Duvarı tehdit zekası yapılandırması

Tehdit zekası tabanlı filtreleme, Azure Güvenlik Duvarı ilkenizin ve bilinen kötü amaçlı IP adreslerinden ve etki alanlarından gelen trafiği uyarmak ve reddetmek için yapılandırılabilir. IP adresleri ve etki alanları, Microsoft Tehdit Analizi akışından alınır. [Intelligent Security Graph](https://www.microsoft.com/security/operations/intelligence) , Microsoft Threat Intelligence 'ı güçlendirir ve Azure Güvenlik Merkezi dahil birden çok hizmet tarafından kullanılır.<br>

Tehdit zekası tabanlı filtreleme 'yi yapılandırdıysanız, ilişkili kurallar NAT kurallarından, ağ kurallarından veya uygulama kurallarından önce işlenir.

:::image type="content" source="media/threat-intelligence-settings/threat-intelligence-policy.png" alt-text="Tehdit bilgileri ilkesi":::

## <a name="threat-intelligence-mode"></a>Tehdit bilgileri modu

Bir kural tetiklendiğinde yalnızca bir uyarıyı günlüğe kaydedebilir veya uyarı ve reddetme modunu seçebilirsiniz.

Varsayılan olarak, tehdit zekası tabanlı filtreleme, uyarı modunda etkindir.

## <a name="allowed-list-addresses"></a>İzin verilen liste adresleri

Tehdit bilgileri 'nin belirttiğiniz adreslerin, aralıkların veya alt ağların hiçbirini filtrelemesine olanak sağlamak için izin verilen IP adreslerinin bir listesini yapılandırabilirsiniz.



## <a name="logs"></a>Günlükler

Aşağıdaki günlük alıntısı tetiklenen bir kural gösterir:

```
{
    "category": "AzureFirewallNetworkRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallThreatIntelLog",
    "properties": {
         "msg": "HTTP request from 10.0.0.5:54074 to somemaliciousdomain.com:80. Action: Alert. ThreatIntel: Bot Networks"
    }
}
```

## <a name="testing"></a>Test Etme

- **Giden sınama** -giden trafik uyarıları nadir bir oluşum olmalıdır, çünkü ortamınız tehlikede olduğu anlamına gelir. Giden uyarıların test sağlanmasına yardımcı olmak için bir uyarıyı tetikleyen bir test FQDN 'SI oluşturulmuştur. Giden testleriniz için **testmaliciousdomain.eastus.cloudapp.Azure.com** kullanın.

- **Gelen sınama** -güvenlik duvarında DNAT kuralları yapılandırılırsa, gelen trafikte uyarıları görmeyi bekleyebilir. Bu, DNAT kuralında yalnızca belirli kaynaklara izin verildiğinden ve trafik başka bir şekilde reddedilse bile geçerlidir. Azure Güvenlik Duvarı tüm bilinen bağlantı noktası tarayıcıları üzerinde uyarı vermez; yalnızca kötü amaçlı etkinlikler için de bilinen tarayıcılarda.

## <a name="next-steps"></a>Sonraki adımlar

- [Microsoft Güvenlik Zekası raporunu](https://www.microsoft.com/en-us/security/operations/security-intelligence-report) gözden geçirin