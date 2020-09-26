---
title: Azure Event Hubs güvenlik duvarı kuralları | Microsoft Docs
description: Belirli IP adreslerinden Azure Event Hubs bağlantılara izin vermek için güvenlik duvarı kurallarını kullanın.
ms.topic: article
ms.date: 07/16/2020
ms.openlocfilehash: ab7f835187a33b5e4d95c160831337172a5ed74e
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91318543"
---
# <a name="allow-access-to-azure-event-hubs-namespaces-from-specific-ip-addresses-or-ranges"></a>Belirli IP adreslerinden veya aralıklardan Azure Event Hubs ad alanlarına erişime izin ver
Varsayılan olarak, istek geçerli kimlik doğrulaması ve yetkilendirmeyle geldiği sürece, Event Hubs ad alanlarına internet 'ten erişilebilir. IP güvenlik duvarı ile, [CIDR (sınıfsız etki alanları arası yönlendirme)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) gösteriminde yalnızca bir dizi IPv4 adresi veya IPv4 adres aralığı ile sınırlayabilirsiniz.

Bu özellik, Azure Event Hubs yalnızca belirli bilinen sitelerden erişilebilir olması gereken senaryolarda yardımcı olur. Güvenlik duvarı kuralları, belirli IPv4 adreslerinden kaynaklanan trafiği kabul etmek için kuralları yapılandırmanızı sağlar. Örneğin, [Azure Express Route][express-route]ile Event Hubs kullanıyorsanız, yalnızca şirket ıçı altyapı IP adreslerinden gelen trafiğe izin vermek için bir **güvenlik duvarı kuralı** oluşturabilirsiniz. 

>[!IMPORTANT]
> Event Hubs ad alanınız için güvenlik duvarı kurallarını açmak, istekler izin verilen genel IP adreslerinden çalışan bir hizmetten kaynaklanmadığı takdirde varsayılan olarak gelen istekleri engeller. Engellenen istekler diğer Azure hizmetlerinden, Azure portal, günlük ve ölçüm hizmetlerinden ve bu şekilde devam eder. 
>
> IP filtrelemesi etkinleştirildiğinde Event Hubs kaynaklarına erişemeyen hizmetlerden bazıları aşağıda verilmiştir. Listenin **ayrıntılı olduğunu** unutmayın.
>
> - Azure Stream Analytics
> - Azure IoT Hub yolları
> - Azure IoT Device Explorer
> - Azure Event Grid
> - Azure Izleyici (Tanılama Ayarları)
>
> Özel durum olarak, IP filtrelemesi etkinleştirildiğinde bile belirli güvenilen hizmetlerden Event Hubs kaynaklara erişime izin verebilirsiniz. Güvenilen hizmetlerin bir listesi için bkz. [Güvenilen Microsoft Hizmetleri](#trusted-microsoft-services).

## <a name="ip-firewall-rules"></a>IP güvenlik duvarı kuralları
IP güvenlik duvarı kuralları Event Hubs ad alanı düzeyinde uygulanır. Bu nedenle, kurallar desteklenen herhangi bir protokolü kullanarak istemcilerden gelen tüm bağlantılara uygulanır. Event Hubs ad alanındaki izin verilen bir IP kuralıyla eşleşmeyen bir IP adresinden gelen bağlantı girişimleri yetkisiz olarak reddedilir. Yanıt, IP kuralından bahsetmez. IP filtresi kuralları sırasıyla uygulanır ve IP adresiyle eşleşen ilk kural kabul etme veya reddetme eylemini belirler.

## <a name="use-azure-portal"></a>Azure portalı kullanma
Bu bölümde, bir Event Hubs ad alanı için IP güvenlik duvarı kuralları oluşturmak üzere Azure portal nasıl kullanılacağı gösterilmektedir. 

1. [Azure portal](https://portal.azure.com) **Event Hubs ad alanına** gidin.
4. Sol menüdeki **Ayarlar** altında **ağ** ' ı seçin. **Ağ** sekmesini yalnızca **Standart** veya **adanmış** ad alanları için görürsünüz. 
    > [!NOTE]
    > Varsayılan olarak, **Seçilen ağlar** seçeneği aşağıdaki görüntüde gösterildiği gibi seçilidir. Bu sayfaya bir IP güvenlik duvarı kuralı belirtmezseniz veya bir sanal ağ eklerseniz, ad alanına **genel İnternet** üzerinden erişilebilir (erişim anahtarı kullanılarak).  

    :::image type="content" source="./media/event-hubs-firewall/selected-networks.png" alt-text="Ağlar sekmesi-seçili ağlar seçeneği" lightbox="./media/event-hubs-firewall/selected-networks.png":::    

    **Tüm ağlar** seçeneğini belirlerseniz, Olay Hub 'ı HERHANGI bir IP adresinden (erişim anahtarı kullanılarak) gelen bağlantıları kabul eder. Bu ayar 0.0.0.0/0 IP adresi aralığını kabul eden bir kuralla eşdeğerdir. 

    !["Tüm ağlar" seçeneği belirlenmiş "güvenlik duvarı ve sanal ağlar" sayfasını gösteren ekran görüntüsü.](./media/event-hubs-firewall/firewall-all-networks-selected.png)
1. Belirli IP adreslerine erişimi kısıtlamak için **Seçili ağlar** seçeneğinin seçili olduğunu onaylayın. **Güvenlik duvarı** bölümünde şu adımları izleyin:
    1. Geçerli istemci IP 'nize ad alanına erişim sağlamak için **ISTEMCI IP adresi ekle** seçeneğini belirleyin. 
    2. **Adres aralığı**IÇIN, CIDR gösteriminde belirli bir IPv4 adresi veya bir IPv4 adresi aralığı girin. 
3. **Güvenilen Microsoft hizmetlerinin bu güvenlik duvarını atlamasına izin**vermek isteyip istemediğinizi belirtin. Ayrıntılar için bkz. [Güvenilen Microsoft Hizmetleri](#trusted-microsoft-services) . 

      ![Güvenlik Duvarı-tüm ağlar seçeneği seçildi](./media/event-hubs-firewall/firewall-selected-networks-trusted-access-disabled.png)
3. Ayarları kaydetmek için araç çubuğunda **Kaydet** ' i seçin. Onayın Portal bildirimlerinde gösterilmesi için birkaç dakika bekleyin.

    > [!NOTE]
    > Belirli sanal ağlara erişimi kısıtlamak için bkz. [belirli ağlardan erişime Izin ver](event-hubs-service-endpoints.md).

[!INCLUDE [event-hubs-trusted-services](../../includes/event-hubs-trusted-services.md)]


## <a name="use-resource-manager-template"></a>Resource Manager şablonu kullanma

> [!IMPORTANT]
> Güvenlik duvarı kuralları, Event Hubs **Standart** ve **adanmış** katmanlarında desteklenir. Temel katmanda desteklenmez.

Aşağıdaki Kaynak Yöneticisi şablonu, var olan bir Event Hubs ad alanına bir IP filtre kuralı eklenmesine olanak sağlar.

Şablon parametreleri:

- **IPMask** , CIDR gösteriminde tek bir IPv4 ADRESIDIR veya IP adresi bloğudur. Örneğin, CıDR gösteriminde 70.37.104.0/24, Aralık için önemli olan önek bit sayısını belirten, 70.37.104.0 ile 70.37.104.255 arasındaki 256 IPv4 adresini temsil eder.

> [!NOTE]
> Mümkün olan reddetme kuralları olmadığı sürece, Azure Resource Manager şablonu, bağlantıları kısıtlayameyen **"Izin ver"** olarak ayarlanmış varsayılan eylemi içerir.
> Sanal ağ veya güvenlik duvarları kuralları yaparken, ***"DefaultAction"*** öğesini değiştirmemiz gerekir
> 
> Kaynak
> ```json
> "defaultAction": "Allow"
> ```
> şöyle değiştirin:
> ```json
> "defaultAction": "Deny"
> ```
>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "eventhubNamespaceName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Event Hubs namespace"
        }
      },
      "location": {
        "type": "string",
        "metadata": {
          "description": "Location for Namespace"
        }
      }
    },
    "variables": {
      "namespaceNetworkRuleSetName": "[concat(parameters('eventhubNamespaceName'), concat('/', 'default'))]",
    },
    "resources": [
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[parameters('eventhubNamespaceName')]",
        "type": "Microsoft.EventHub/namespaces",
        "location": "[parameters('location')]",
        "sku": {
          "name": "Standard",
          "tier": "Standard"
        },
        "properties": { }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.EventHub/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.EventHub/namespaces/', parameters('eventhubNamespaceName'))]"
        ],
        "properties": {
          "virtualNetworkRules": [<YOUR EXISTING VIRTUAL NETWORK RULES>],
          "ipRules": 
          [
            {
                "ipMask":"10.1.1.1",
                "action":"Allow"
            },
            {
                "ipMask":"11.0.0.0/24",
                "action":"Allow"
            }
          ],
          "trustedServiceAccessEnabled": false,
          "defaultAction": "Deny"
        }
      }
    ],
    "outputs": { }
  }
```

Şablonu dağıtmak için [Azure Resource Manager][lnk-deploy]talimatlarını izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure sanal ağlarına Event Hubs erişimi kısıtlayan için şu bağlantıya bakın:

- [Event Hubs için sanal ağ hizmeti uç noktaları][lnk-vnet]

<!-- Links -->

[express-route]:  ../expressroute/expressroute-faqs.md#supported-services
[lnk-deploy]: ../azure-resource-manager/templates/deploy-powershell.md
[lnk-vnet]: event-hubs-service-endpoints.md
