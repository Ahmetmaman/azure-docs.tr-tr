---
title: İç sanal ağlar ile Azure API Management'ı kullanma | Microsoft Docs
description: Ayarlama ve Azure API Management'ı bir iç sanal ağ yapılandırma hakkında bilgi edinin
services: api-management
documentationcenter: ''
author: vladvino
manager: kjoshi
editor: ''
ms.assetid: dac28ccf-2550-45a5-89cf-192d87369bc3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: apimpm
ms.openlocfilehash: 9a2cf35203c673d6296754360ac4f794241d4c43
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2019
ms.locfileid: "57008687"
---
# <a name="using-azure-api-management-service-with-an-internal-virtual-network"></a>Azure API Management hizmeti bir iç sanal ağ ile kullanma
Azure sanal ağlar ile Azure API Management API'leri değil internet üzerinden erişilebilen yönetebilirsiniz. VPN'si teknolojileri birkaç bağlantı kurmak kullanılabilir. API Management, iki ana modda bir sanal ağ içinde dağıtılabilir:
* Dış
* İç

API yönetimi, dahili sanal ağ modunda dağıttığında, tüm hizmet uç noktaları (ağ geçidi, Geliştirici Portalı, Azure portalı, doğrudan yönetim ve Git) yalnızca erişimini denetleyen bir sanal ağ içinde görülebilir. Hizmet uç noktalarını hiçbiri genel DNS sunucusunda kayıtlı.

API Management, iç modda kullanma, aşağıdaki senaryoları elde edebilirsiniz:

* Siteden siteye veya Azure ExpressRoute VPN bağlantıları kullanarak özel veri merkezinizde güvenli bir şekilde erişmesini dışındaki üçüncü taraflarca barındırılan API'ler olun.
* Karma bulut senaryolarında, bulut tabanlı API'ler ve ortak bir ağ geçidi üzerinden şirket içi API'ler göstererek etkinleştirin.
* Apı'lerinizi tek bir ağ geçidi uç noktası kullanarak birden çok coğrafi bölgelerde barındırılan yönetin. 

[!INCLUDE [premium-dev.md](../../includes/api-management-availability-premium-dev.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu makalede açıklanan adımları gerçekleştirmek için aşağıdakiler gerekir:

+ **Etkin bir Azure aboneliğine**.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ **Azure API Management örneği**. Daha fazla bilgi için [Azure API Management örneği oluşturma](get-started-create-service-instance.md).

## <a name="enable-vpn"> </a>API yönetimi bir iç sanal ağ oluşturma
Bir iç sanal ağ API Management hizmetinde bir iç yük dengeleyici (ILB) barındırılır.

### <a name="enable-a-virtual-network-connection-using-the-azure-portal"></a>Azure portalını kullanarak bir sanal ağ bağlantısını etkinleştirme

1. Azure API Management Örneğinize göz atın [Azure portalında](https://portal.azure.com/).
2. **Sanal ağ**'ı seçin.
3. Sanal ağ içinde dağıtılmak üzere API Management örneği yapılandırın.

    ![Bir Azure API Yönetimi'nde bir iç sanal ağ ayarlama menüsü][api-management-using-internal-vnet-menu]

4. **Kaydet**’i seçin.

Dağıtım başarılı olduktan sonra Panoda hizmetinizin iç sanal IP adresi görmeniz gerekir.

![API Yönetimi panosu ile yapılandırılmış bir iç sanal ağ][api-management-internal-vnet-dashboard]

> [!NOTE]
> Azure portalında kullanılabilir Test Konsolu çalışmaz **dahili** VNET dağıtılan hizmet, ağ geçidi URL'si Genel DNS kayıtlı değil. Sağlanan Test Konsolu kullanmalısınız **Geliştirici Portalı**.

### <a name="enable-a-virtual-network-connection-by-using-powershell-cmdlets"></a>PowerShell cmdlet'lerini kullanarak bir sanal ağ bağlantısını etkinleştirme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell cmdlet'lerini kullanarak, sanal ağ bağlantısı da etkinleştirebilirsiniz.

* Bir sanal ağ içinde bir API Management hizmeti oluşturun: Cmdlet'i kullanmak [yeni AzApiManagement](/powershell/module/az.apimanagement/new-azapimanagement) bir sanal ağ içinde bir Azure API Management hizmeti oluşturma ve iç sanal ağ türü kullanacak şekilde yapılandırın.

* Var olan bir dağıtımının bir sanal ağ içinde bir API Management hizmeti güncelleştirin: Cmdlet'i kullanmak [güncelleştirme AzApiManagementRegion](/powershell/module/az.apimanagement/update-azapimanagementregion) bir API Yönetimi hizmetiniz bir sanal ağ içinde hareket ve iç sanal ağ türü kullanacak şekilde yapılandırın.

## <a name="apim-dns-configuration"></a>DNS yapılandırması
API Management, dış sanal ağ modunda olduğunda, DNS, Azure tarafından yönetilir. İç sanal ağ modu için kendi yönlendirme yönetmek zorunda.

> [!NOTE]
> API Management hizmeti, IP adreslerinden mi geliyor isteklerini dinlemez. Bu yalnızca, hizmet uç noktaları üzerinde yapılandırılan ana bilgisayar adı için isteklere yanıt verir. Bu uç noktaları, ağ geçidi, Azure portalı ve Geliştirici Portalı, doğrudan yönetim uç noktası ve Git içerir.

### <a name="access-on-default-host-names"></a>Varsayılan ana bilgisayar adları üzerinde erişim
Örneğin, "contoso" adlı bir API Management hizmeti oluşturduğunuzda, aşağıdaki hizmet uç noktaları varsayılan olarak yapılandırılır:

   * Ağ geçidi veya proxy: contoso.azure-api.net

   * Azure portalı ve Geliştirici Portalı: contoso.portal.azure-api.net

   * Doğrudan yönetim uç noktası: contoso.management.azure-api.net

   * Git: contoso.scm.azure-api.net

Bu API Management hizmet uç noktalarına erişmek için API Management dağıtıldığı sanal ağa bağlı bir alt ağdaki bir sanal makine oluşturabilirsiniz. 10.0.0.5 hizmetiniz için iç sanal IP adresi olduğunu varsayarsak, ana bilgisayarlar dosyası, % SystemDrive%\drivers\etc\hosts, şu şekilde eşleyebilirsiniz:

   * 10.0.0.5 contoso.azure-api.net

   * 10.0.0.5 contoso.portal.azure-api.net

   * 10.0.0.5     contoso.management.azure-api.net

   * 10.0.0.5 contoso.scm.azure-api.net

Tüm hizmet uç noktaları, oluşturduğunuz sanal makineden erişebilirsiniz. Bir sanal ağda özel DNS sunucusu kullanıyorsanız, ayrıca bir DNS kayıtları oluşturmak ve bu uç noktalar her yerden erişim sanal ağınızda. 

### <a name="access-on-custom-domain-names"></a>Özel etki alanı adları hakkında daha fazla erişim

   1. API Management hizmeti ile varsayılan konak adları erişmek istemiyorsanız, aşağıdaki görüntüde gösterildiği gibi tüm hizmet uç noktalarınıza için özel etki alanı adlarını ayarlayabilirsiniz: 

   ![API Management için özel bir etki alanı ayarlama][api-management-custom-domain-name]

   2. Ardından, yalnızca sanal ağınızdaki erişilebilir uç noktalarına erişmek için DNS sunucunuzun kayıtlarını oluşturabilirsiniz.

## <a name="routing"> </a> Yönlendirme
+ Yük dengeli özel bir sanal IP adresi alt ağı aralığından ayrılmış ve sanal ağ içindeki API Management hizmet uç noktalarından erişmek için kullanılır.
+ Yük dengeli genel IP adresi (VIP), Yönetim Hizmeti uç noktasına erişim yalnızca bağlantı noktası üzerinden 3443 sağlamak için ayrıca ayrılacaktır.
+ Bir alt ağ IP aralığı (DIP) ağdan bir IP adresi, sanal ağ içindeki kaynaklara erişmek için kullanılacak ve sanal ağ dışındaki kaynaklara erişmek için genel bir IP adresi (VIP) kullanılır.
+ Genel yük dengeli ve özel IP adresleri, Azure portalında genel bakış/Essentials dikey penceresinde bulunabilir.

## <a name="related-content"> </a>İlgili içerik
Daha fazla bilgi için aşağıdaki makalelere bakın:
* [Bir sanal ağda Azure API Management ayarlanıyor sırasında ortak ağ yapılandırma sorunları][Common network configuration problems]
* [Sanal ağ hakkında SSS](../virtual-network/virtual-networks-faq.md)
* [DNS'de bir kaydı oluşturma](https://msdn.microsoft.com/library/bb727018.aspx)

[api-management-using-internal-vnet-menu]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-menu.png
[api-management-internal-vnet-dashboard]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-dashboard.png
[api-management-custom-domain-name]: ./media/api-management-using-with-internal-vnet/api-management-custom-domain-name.png

[Create API Management service]: get-started-create-service-instance.md
[Common network configuration problems]: api-management-using-with-vnet.md#network-configuration-issues

