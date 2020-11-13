---
title: Ön kapı için WAF hız limiti kuralını Yapılandırma-Azure PowerShell
description: Varolan bir ön kapı uç noktası için hız limiti kuralını yapılandırmayı öğrenin.
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 02/26/2020
ms.author: victorh
ms.openlocfilehash: 23b893bad591af5f1e923b68e8d30453f859792b
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94563486"
---
# <a name="configure-a-web-application-firewall-rate-limit-rule-using-azure-powershell"></a>Azure PowerShell kullanarak bir Web uygulaması güvenlik duvarı hız sınırı kuralı yapılandırma
Azure ön kapısının Azure Web uygulaması güvenlik duvarı (WAF) hız sınırı kuralı, tek dakikalık bir süre içinde istemcilerden izin verilen isteklerin sayısını denetler.
Bu makalede, istemcilerden Azure PowerShell kullanarak URL 'de */promosyon* içeren bir Web uygulamasına izin verilen isteklerin sayısını denetleyen bir WAF hız sınırı kuralının nasıl yapılandırılacağı gösterilmektedir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!NOTE]
> Her istemci IP adresi için hız sınırları uygulanır. Ön kapılarınıza farklı IP adreslerinden erişen birden fazla istemciniz varsa, bunların kendi hız limitleri uygulanmış olur.

## <a name="prerequisites"></a>Ön koşullar
Bir hız sınırı ilkesi ayarlamaya başlamadan önce, PowerShell ortamınızı ayarlayın ve bir ön kapı profili oluşturun.
### <a name="set-up-your-powershell-environment"></a>PowerShell ortamınızı hazırlama
Azure PowerShell, Azure kaynaklarınızı yönetmek için [Azure Resource Manager](../../azure-resource-manager/management/overview.md) modelini kullanan bir dizi cmdlet sunar. 

[Azure PowerShell](/powershell/azure/)'i yerel makinenize yükleyebilir ve herhangi bir PowerShell oturumunda kullanabilirsiniz. Sayfadaki yönergeleri izleyerek Azure kimlik bilgilerinizle oturum açın ve az PowerShell Module ' ü kullanın.

#### <a name="connect-to-azure-with-an-interactive-dialog-for-sign-in"></a>Oturum açmak için etkileşimli iletişim kutusuyla Azure 'a bağlanın
```
Connect-AzAccount

```
Ön kapı modülünü yüklemeden önce, PowerShellGet 'in geçerli sürümünün yüklü olduğundan emin olun. Aşağıdaki komutu çalıştırın ve PowerShell 'i yeniden açın.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 

#### <a name="install-azfrontdoor-module"></a>Install az. Frontkapı Module 

```
Install-Module -Name Az.FrontDoor
```
### <a name="create-a-front-door-profile"></a>Ön kapı profili oluşturma
[Hızlı başlangıç: ön kapı profili oluşturma](../../frontdoor/quickstart-create-front-door.md) bölümünde açıklanan yönergeleri Izleyerek bir ön kapı profili oluşturun

## <a name="define-url-match-conditions"></a>URL eşleştirme koşullarını tanımlayın
[New-AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject)kullanarak bir URL eşleştirme koşulu TANıMLAYıN (URL/promosyon içerir).
Aşağıdaki örnek, *RequestUri* değişkeninin değeri olarak */promosyon* ile eşleşir:

```powershell-interactive
   $promoMatchCondition = New-AzFrontDoorWafMatchConditionObject `
     -MatchVariable RequestUri `
     -OperatorProperty Contains `
     -MatchValue "/promo"
```
## <a name="create-a-custom-rate-limit-rule"></a>Özel bir hız sınırı kuralı oluşturma
[New-AzFrontDoorWafCustomRuleObject](/powershell/module/az.frontdoor/new-azfrontdoorwafcustomruleobject)kullanarak bir hız sınırı ayarlayın. Aşağıdaki örnekte, sınır 1000 olarak ayarlanır. Bir dakika boyunca 1000 aşan promosyon sayfasına herhangi bir istemciden gelen istekler, bir sonraki dakika başlamadan önce engellenir.

```powershell-interactive
   $promoRateLimitRule = New-AzFrontDoorWafCustomRuleObject `
     -Name "rateLimitRule" `
     -RuleType RateLimitRule `
     -MatchCondition $promoMatchCondition `
     -RateLimitThreshold 1000 `
     -Action Block -Priority 1
```


## <a name="configure-a-security-policy"></a>Güvenlik ilkesi yapılandırma

Kullanarak ön kapı profilini içeren kaynak grubunun adını bulun `Get-AzureRmResourceGroup` . Ardından, ön kapı profilini içeren belirtilen kaynak grubunda [New-AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy) kullanarak bir güvenlik ilkesini özel bir hız sınırı kuralıyla yapılandırın.

Aşağıdaki örnek, *MyResourceGroupFD1* kaynak grubu adını, [hızlı başlangıç: ön kapı oluşturma](../../frontdoor/quickstart-create-front-door.md) makalesinde sunulan [yönergeleri kullanarak ön](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy)kapı profilini oluşturduğunuz varsayımıyla birlikte kullanır.

```powershell-interactive
   $ratePolicy = New-AzFrontDoorWafPolicy `
     -Name "RateLimitPolicyExamplePS" `
     -resourceGroupName myResourceGroupFD1 `
     -Customrule $promoRateLimitRule `
     -Mode Prevention `
     -EnabledState Enabled
```
## <a name="link-policy-to-a-front-door-front-end-host"></a>İlkeyi bir ön kapı ön uç konağına bağlama
Güvenlik İlkesi nesnesini var olan bir ön kapı ön uç konağına bağlayın ve ön kapı özelliklerini güncelleştirin. İlk olarak [Get-Azfrontkapısı](/powershell/module/Az.FrontDoor/Get-AzFrontDoor) komutunu kullanarak ön kapı nesnesini alın.
Ardından, ön uç *Webapplicationfirewallpolicylink* özelliğini, [set-azfrontkapısı](/powershell/module/Az.FrontDoor/Set-AzFrontDoor) komutunu kullanarak önceki adımda oluşturulan "$ratePolicy" öğesinin *RESOURCEID* değerini olarak ayarlayın. 

Aşağıdaki örnek, [hızlı başlangıç: ön kapı oluşturma](../../frontdoor/quickstart-create-front-door.md) makalesinde sunulan yönergeleri kullanarak ön kapı profilini oluşturduğunuz varsayımıyla *myResourceGroupFD1* kaynak grubu adını kullanır. Ayrıca, aşağıdaki örnekte $frontDoorName, ön kapı profilinizin adıyla değiştirin. 

```powershell-interactive
   $FrontDoorObjectExample = Get-AzFrontDoor `
     -ResourceGroupName myResourceGroupFD1 `
     -Name $frontDoorName
   $FrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $ratePolicy.Id
   Set-AzFrontDoor -InputObject $FrontDoorObjectExample[0]
 ```

> [!NOTE]
> Bir güvenlik ilkesini ön kapı ön ucuna bağlamak için yalnızca *Webapplicationfirewallpolicylink* özelliğini bir kez ayarlamanız gerekir. Sonraki ilke güncelleştirmeleri otomatik olarak ön uca uygulanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Ön kapı](../../frontdoor/front-door-overview.md)hakkında daha fazla bilgi edinin.