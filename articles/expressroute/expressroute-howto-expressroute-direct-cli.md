---
title: "Azure ExpressRoute: ExpressRoute Direct 'i yapılandırma: CLı"
description: Bu makale, Azure CLı kullanarak ExpressRoute Direct 'i yapılandırmanıza yardımcı olur
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: cherylmc
ms.openlocfilehash: 47ee05113d46f66efd02978fed09cf72edc5ac1c
ms.sourcegitcommit: 57669c5ae1abdb6bac3b1e816ea822e3dbf5b3e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77049932"
---
# <a name="configure-expressroute-direct-by-using-the-azure-cli"></a>Azure CLı kullanarak ExpressRoute Direct 'i yapılandırma

Microsoft'un küresel ağı dünya genelindeki stratejik dağıtılmış eşleme konumlarda doğrudan bağlanmak için Azure ExpressRoute doğrudan kullanabilirsiniz. Daha fazla bilgi için bkz. [ExpressRoute Direct Connect hakkında](expressroute-erdirect-about.md).

## <a name="resources"></a>Kaynağı oluşturma

1. Azure'da oturum açın ve ExpressRoute içeren aboneliği seçin. ExpressRoute doğrudan kaynak ve, ExpressRoute devreleri aynı abonelikte olması gerekir. Azure CLI, aşağıdaki komutları çalıştırın:

   ```azurecli
   az login
   ```

   Hesabın aboneliklerini denetleyin: 

   ```azurecli
   az account list 
   ```

   Bir ExpressRoute bağlantı hattı oluşturmak istediğiniz aboneliği seçin:

   ```azurecli
   az account set --subscription "<subscription ID>"
   ```

2. Expressrouteportslocation ve expressrouteport API 'Lerine erişmek için aboneliğinizi Microsoft. Network 'e yeniden kaydedin

   ```azurecli
   az provider register --namespace Microsoft.Network
   ```
3. ExpressRoute doğrudan'ın desteklendiği tüm konumların listesi:
    
   ```azurecli
   az network express-route port location list
   ```

   **Örnek çıkış**
  
   ```azurecli
   [
   {
    "address": "21715 Filigree Court, DC2, Building F, Ashburn, VA 20147",
    "availableBandwidths": [],
    "contact": "support@equinix.com",
    "id": "/subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-Ashburn-DC2",
    "location": null,
    "name": "Equinix-Ashburn-DC2",
    "provisioningState": "Succeeded",
    "tags": null,
    "type": "Microsoft.Network/expressRoutePortsLocations"
   },
   {
    "address": "1950 N. Stemmons Freeway, Suite 1039A, DA3, Dallas, TX 75207",
    "availableBandwidths": [],
    "contact": "support@equinix.com",
    "id": "/subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-Dallas-DA3",
    "location": null,
    "name": "Equinix-Dallas-DA3",
    "provisioningState": "Succeeded",
    "tags": null,
    "type": "Microsoft.Network/expressRoutePortsLocations"
   },
   {
    "address": "111 8th Avenue, New York, NY 10011",
    "availableBandwidths": [],
    "contact": "support@equinix.com",
    "id": "/subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-New-York-NY5",
    "location": null,
    "name": "Equinix-New-York-NY5",
    "provisioningState": "Succeeded",
    "tags": null,
    "type": "Microsoft.Network/expressRoutePortsLocations"
   },
   {
    "address": "11 Great Oaks Blvd, SV1, San Jose, CA 95119",
    "availableBandwidths": [],
    "contact": "support@equinix.com",
    "id": "/subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-San-Jose-SV1",
    "location": null,
    "name": "Equinix-San-Jose-SV1",
    "provisioningState": "Succeeded",
    "tags": null,
    "type": "Microsoft.Network/expressRoutePortsLocations"
   },
   {
    "address": "2001 Sixth Ave., Suite 350, SE2, Seattle, WA 98121",
    "availableBandwidths": [],
    "contact": "support@equinix.com",
    "id": "/subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-Seattle-SE2",
    "location": null,
    "name": "Equinix-Seattle-SE2",
    "provisioningState": "Succeeded",
    "tags": null,
    "type": "Microsoft.Network/expressRoutePortsLocations"
   }
   ]
   ```
4. Kullanılabilir bant genişliğini önceki adımda listelenen konumlardan birine sahip olup olmadığını belirleyin:

   ```azurecli
   az network express-route port location show -l "Equinix-Ashburn-DC2"
   ```

   **Örnek çıkış**

   ```azurecli
   {
   "address": "21715 Filigree Court, DC2, Building F, Ashburn, VA 20147",
   "availableBandwidths": [
    {
      "offerName": "100 Gbps",
      "valueInGbps": 100
    }
   ],
   "contact": "support@equinix.com",
   "id": "/subscriptions/<subscriptionID>/providers/Microsoft.Network/expressRoutePortsLocations/Equinix-Ashburn-DC2",
   "location": null,
   "name": "Equinix-Ashburn-DC2",
   "provisioningState": "Succeeded",
   "tags": null,
   "type": "Microsoft.Network/expressRoutePortsLocations"
   }
   ```
5. Önceki adımda seçtiğiniz konum temel alan bir ExpressRoute doğrudan kaynağı oluşturun.

   ExpressRoute doğrudan QinQ hem Dot1Q kapsülleme destekler. QinQ seçerseniz, her bir ExpressRoute bağlantı hattında S etiketi dinamik olarak atanır ve ExpressRoute doğrudan kaynak benzersizdir. Her C-Tag devredeki benzersiz olmalıdır. bağlantı hattını ancak ExpressRoute doğrudan kaynak arasında değil.  

   Dot1Q sarması'nı seçerseniz, tüm ExpressRoute doğrudan kaynak arasında benzersizlik, C etiketleme (VLAN) yönetmeniz gerekir.  

   > [!IMPORTANT]
   > ExpressRoute doğrudan yalnızca bir saklama türü olabilir. ExpressRoute doğrudan kaynak oluşturduktan sonra kapsülleme türünü değiştiremezsiniz.
   > 
 
   ```azurecli
   az network express-route port create -n $name -g $RGName --bandwidth 100 gbps  --encapsulation QinQ | Dot1Q --peering-location $PeeringLocationName -l $AzureRegion 
   ```

   > [!NOTE]
   > Ayrıca, **kapsülleme** özniteliğini **Dot1Q**olarak da ayarlayabilirsiniz. 
   >

   **Örnek çıkış**

   ```azurecli
   {
   "allocationDate": "Wednesday, October 17, 2018",
   "bandwidthInGbps": 100,
   "circuits": null,
   "encapsulation": "Dot1Q",
   "etag": "W/\"<etagnumber>\"",
   "etherType": "0x8100",
   "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct",
   "links": [
    {
      "adminState": "Disabled",
      "connectorType": "LC",
      "etag": "W/\"<etagnumber>\"",
      "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct/links/link1",
      "interfaceName": "HundredGigE2/2/2",
      "name": "link1",
      "patchPanelId": "PPID",
      "provisioningState": "Succeeded",
      "rackId": "RackID",
      "resourceGroup": "Contoso-Direct-rg",
      "routerName": "tst-09xgmr-cis-1",
      "type": "Microsoft.Network/expressRoutePorts/links"
    },
    {
      "adminState": "Disabled",
      "connectorType": "LC",
      "etag": "W/\"<etagnumber>\"",
      "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct/links/link2",
      "interfaceName": "HundredGigE2/2/2",
      "name": "link2",
      "patchPanelId": "PPID",
      "provisioningState": "Succeeded",
      "rackId": "RackID",
      "resourceGroup": "Contoso-Direct-rg",
      "routerName": "tst-09xgmr-cis-2",
      "type": "Microsoft.Network/expressRoutePorts/links"
    }
   ],
   "location": "westus",
   "mtu": "1500",
   "name": "Contoso-Direct",
   "peeringLocation": "Equinix-Ashburn-DC2",
   "provisionedBandwidthInGbps": 0.0,
   "provisioningState": "Succeeded",
   "resourceGroup": "Contoso-Direct-rg",
   "resourceGuid": "02ee21fe-4223-4942-a6bc-8d81daabc94f",
   "tags": null,
   "type": "Microsoft.Network/expressRoutePorts"
   }  
   ```

## <a name="state"></a>Bağlantıların AdminState 'i Değiştir

Bu işlem, bir katman 1 test gerçekleştirmek için kullanın. Her bir çapraz bağlantı her birincil ve ikincil bağlantı yönlendiricisi içine düzgün yüklendiğinden emin olun.

1. Bağlantıları **etkin**olarak ayarlayın. Her bağlantıyı **etkin**olarak ayarlamak için bu adımı tekrarlayın.

   Bağlantılar [0] birincil bağlantı ve bağlantılar [1] ikincil bağlantı noktası.

   ```azurecli
   az network express-route port update -n Contoso-Direct -g Contoso-Direct-rg --set links[0].adminState="Enabled"
   ```
   ```azurecli
   az network express-route port update -n Contoso-Direct -g Contoso-Direct-rg --set links[1].adminState="Enabled"
   ```
   **Örnek çıkış**

   ```azurecli
   {
   "allocationDate": "Wednesday, October 17, 2018",
   "bandwidthInGbps": 100,
   "circuits": null,
   "encapsulation": "Dot1Q",
   "etag": "W/\"<etagnumber>\"",
   "etherType": "0x8100",
   "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct",
   "links": [
    {
      "adminState": "Enabled",
      "connectorType": "LC",
      "etag": "W/\"<etagnumber>\"",
      "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct/links/link1",
      "interfaceName": "HundredGigE2/2/2",
      "name": "link1",
      "patchPanelId": "PPID",
      "provisioningState": "Succeeded",
      "rackId": "RackID",
      "resourceGroup": "Contoso-Direct-rg",
      "routerName": "tst-09xgmr-cis-1",
      "type": "Microsoft.Network/expressRoutePorts/links"
    },
    {
      "adminState": "Enabled",
      "connectorType": "LC",
      "etag": "W/\"<etagnumber>\"",
      "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct/links/link2",
      "interfaceName": "HundredGigE2/2/2",
      "name": "link2",
      "patchPanelId": "PPID",
      "provisioningState": "Succeeded",
      "rackId": "RackID",
      "resourceGroup": "Contoso-Direct-rg",
      "routerName": "tst-09xgmr-cis-2",
      "type": "Microsoft.Network/expressRoutePorts/links"
    }
   ],
   "location": "westus",
   "mtu": "1500",
   "name": "Contoso-Direct",
   "peeringLocation": "Equinix-Ashburn-DC2",
   "provisionedBandwidthInGbps": 0.0,
   "provisioningState": "Succeeded",
   "resourceGroup": "Contoso-Direct-rg",
   "resourceGuid": "<resourceGUID>",
   "tags": null,
   "type": "Microsoft.Network/expressRoutePorts"
   }
   ```

   `AdminState = "Disabled"`kullanarak bağlantı noktalarını kapatmak için aynı yordamı kullanın.

## <a name="circuit"></a>Devre oluşturma

Varsayılan olarak, ExpressRoute doğrudan kaynağı içeren abonelik 10 bağlantı hatları oluşturabilirsiniz. Microsoft Support varsayılan sınırı artırabilirsiniz. Sağlanan ve kullanılan bant genişliğini izlemek için sorumlu olursunuz. Sağlanan bant genişliği, bant genişliğinin ExpressRoute doğrudan kaynaktaki tüm devreler toplamıdır. Kullanılan bant genişliği, temel alınan fiziksel arabirimlerin fiziksel kullanımdır.

Yalnızca Burada özetlenen senaryoları desteklemek için ExpressRoute doğrudan üzerinde ek bağlantı hattı bant genişlikleri kullanabilirsiniz. 40 GB/sn ve 100 GB/sn bant genişlikleri var.

**Skutier** yerel, standart veya Premium olabilir.

**Skufamily** , doğrudan ExpressRoute Direct üzerinde desteklenmediği için MeteredData olmalıdır.
ExpressRoute doğrudan kaynak üzerinde bir bağlantı hattı oluşturun:

  ```azurecli
  az network express-route create --express-route-port "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct" -n "Contoso-Direct-ckt" -g "Contoso-Direct-rg" --sku-family MeteredData --sku-tier Standard --bandwidth 100 Gbps
  ```

  Diğer bant genişlikleri, 5 GB/sn, 10 GB/sn ve 40 GB/sn içerir.

  **Örnek çıkış**

  ```azurecli
  {
  "allowClassicOperations": false,
  "allowGlobalReach": false,
  "authorizations": [],
  "bandwidthInGbps": 100.0,
  "circuitProvisioningState": "Enabled",
  "etag": "W/\"<etagnumber>\"",
  "expressRoutePort": {
    "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRoutePorts/Contoso-Direct",
    "resourceGroup": "Contoso-Direct-rg"
  },
  "gatewayManagerEtag": "",
  "id": "/subscriptions/<subscriptionID>/resourceGroups/Contoso-Direct-rg/providers/Microsoft.Network/expressRouteCircuits/ERDirect-ckt-cli",
  "location": "westus",
  "name": "ERDirect-ckt-cli",
  "peerings": [],
  "provisioningState": "Succeeded",
  "resourceGroup": "Contoso-Direct-rg",
  "serviceKey": "<serviceKey>",
  "serviceProviderNotes": null,
  "serviceProviderProperties": null,
  "serviceProviderProvisioningState": "Provisioned",
  "sku": {
    "family": "MeteredData",
    "name": "Standard_MeteredData",
    "tier": "Standard"
  },
  "stag": null,
  "tags": null,
  "type": "Microsoft.Network/expressRouteCircuits"
  }  
  ```

## <a name="next-steps"></a>Sonraki adımlar

ExpressRoute Direct hakkında daha fazla bilgi için bkz. [genel bakış](expressroute-erdirect-about.md).
