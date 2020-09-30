---
title: Azure Işlevlerinde ağ hakkında sık sorulan sorular
description: Azure Işlevleri ile ağ iletişimi için en yaygın soruların ve senaryolardan bazılarına yanıtlar.
ms.topic: troubleshooting
ms.date: 4/11/2019
ms.reviewer: glenga
ms.openlocfilehash: d07484943f8ba85cc8e46737f3ef85b6e25d5187
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91538067"
---
# <a name="frequently-asked-questions-about-networking-in-azure-functions"></a>Azure Işlevlerinde ağ hakkında sık sorulan sorular

Bu makalede, Azure Işlevlerinde ağlarla ilgili sık sorulan sorular listelenmektedir. Daha kapsamlı bir genel bakış için bkz. [işlevler ağ seçenekleri](functions-networking-options.md).

## <a name="how-do-i-set-a-static-ip-in-functions"></a>Işlevlerde statik IP Nasıl yaparım? mı?

Bir App Service Ortamı işlevin dağıtımı, işleviniz için statik gelen ve giden IP 'nin tek yoludur. App Service Ortamı kullanma hakkında ayrıntılı bilgi için, bir [App Service ortamı ile iç yük dengeleyici oluşturma ve kullanma](../app-service/environment/create-ilb-ase.md)makalesini başlatın.

## <a name="how-do-i-restrict-internet-access-to-my-function"></a>Nasıl yaparım? Internet erişimini işlevme kısıtlamak istiyor musunuz?

Internet erişimini birkaç yolla kısıtlayabilirsiniz:

* [IP kısıtlamaları](../app-service/app-service-ip-restrictions.md): IP aralığı ile işlev uygulamanıza gelen trafiği kısıtlayın.
    * IP kısıtlamaları ' nın altında, işlevinizi yalnızca belirli bir sanal ağdan gelen trafiği kabul edecek şekilde kısıtlayan [hizmet uç noktalarını](../virtual-network/virtual-network-service-endpoints-overview.md)de yapılandırabileceksiniz.
* Tüm HTTP Tetikleyicileri kaldırılıyor. Bazı uygulamalarda, yalnızca HTTP tetikleyicilerinin oluşmaması ve işlevinizin tetiklenmesi için başka herhangi bir olay kaynağının kullanılması yeterlidir.

Azure portal düzenleyicisinin çalışan işlevinize doğrudan erişim gerektirdiğini unutmayın. Azure portal üzerinden yapılan herhangi bir kod değişikliği, IP 'nin onaylanan listeye eklenmesini sağlamak için kullandığınız cihazın portala göz atamasını gerektirir. Ancak, ağ kısıtlamalarına sahip olan platform özellikleri sekmesinde herhangi bir şeyi kullanmaya devam edebilirsiniz.

## <a name="how-do-i-restrict-my-function-app-to-a-virtual-network"></a>İşlev uygulamamı bir sanal ağ ile kısıtlıyor Nasıl yaparım??

[Hizmet uç noktalarını](./functions-networking-options.md#private-site-access)kullanarak bir işlev uygulaması için **gelen** trafiği bir sanal ağ ile kısıtlayabileceksiniz. Bu yapılandırma, işlev uygulamasının internet 'e giden çağrılar yapmasına hala izin verir.

Bir işlevi tamamen kısıtlamak için bir sanal ağ üzerinden akan tüm trafik, dahili yük dengeli bir App Service Ortamı kullanmaktır. Bu seçenek, sitenizi bir sanal ağ içindeki adanmış bir altyapıya dağıtır ve tüm Tetikleyicileri ve trafiği sanal ağ üzerinden gönderir. 

App Service Ortamı kullanma hakkında ayrıntılı bilgi için, bir [App Service ortamı ile iç yük dengeleyici oluşturma ve kullanma](../app-service/environment/create-ilb-ase.md)makalesini başlatın.

## <a name="how-can-i-access-resources-in-a-virtual-network-from-a-function-app"></a>Bir işlev uygulamasından bir sanal ağdaki kaynaklara nasıl erişebilirim?

Sanal ağ tümleştirmesini kullanarak, çalışan bir işlevden bir sanal ağdaki kaynaklara erişebilirsiniz. Daha fazla bilgi için bkz. [sanal ağ tümleştirmesi](functions-networking-options.md#virtual-network-integration).

## <a name="how-do-i-access-resources-protected-by-service-endpoints"></a>Hizmet uç noktaları tarafından korunan Nasıl yaparım? erişim kaynakları?

Sanal ağ tümleştirmesini kullanarak, çalışan bir işlevden hizmet uç nokta korumalı kaynaklara erişebilirsiniz. Daha fazla bilgi için bkz. [sanal ağ tümleştirmesi](functions-networking-options.md#virtual-network-integration).

## <a name="how-can-i-trigger-a-function-from-a-resource-in-a-virtual-network"></a>Sanal ağdaki bir kaynaktan bir işlevi nasıl tetikleyebilirim?

[Hizmet uç noktaları](./functions-networking-options.md#private-site-access)kullanılarak sanal BIR ağdan http tetikleyicilerinin çağrılmasına izin verebilirsiniz. 

Ayrıca, işlev uygulamanızı bir Premium plana, App Service plana veya App Service Ortamı dağıtarak bir sanal ağdaki diğer tüm kaynaklardaki bir işlevi tetikleyebilirsiniz. Daha fazla bilgi için bkz. [http olmayan sanal ağ Tetikleyicileri](./functions-networking-options.md#virtual-network-triggers-non-http)

## <a name="how-can-i-deploy-my-function-app-in-a-virtual-network"></a>İşlev uygulamamı sanal bir ağda nasıl dağıtabilirim?

Bir App Service Ortamı dağıtım, bir sanal ağın içinde bir işlev uygulaması oluşturmanın tek yoludur. Bir App Service Ortamı iç yük dengeleyiciyi kullanma hakkında ayrıntılı bilgi için, bir [App Service ortamı ile iç yük dengeleyici oluşturma ve kullanma](../app-service/environment/create-ilb-ase.md)makalesini başlatın.

Sanal ağ kaynaklarına yalnızca tek yönlü erişiminizin olması veya daha az kapsamlı ağ yalıtımı olması gereken senaryolarda, bkz. [işlevlere genel bakış](functions-networking-options.md).

## <a name="next-steps"></a>Sonraki adımlar

Ağ ve işlevler hakkında daha fazla bilgi için: 

* [Sanal ağ tümleştirmesiyle çalışmaya başlama öğreticisini izleyin](./functions-create-vnet.md)
* [Azure Işlevleri 'ndeki ağ seçenekleri hakkında daha fazla bilgi edinin](./functions-networking-options.md)
* [App Service ve Işlevlerle sanal ağ tümleştirmesi hakkında daha fazla bilgi edinin](../app-service/web-sites-integrate-with-vnet.md)
* [Azure 'da sanal ağlar hakkında daha fazla bilgi edinin](../virtual-network/virtual-networks-overview.md)
* [App Service ortamlarla daha fazla ağ özelliği ve denetimi etkinleştirin](../app-service/environment/intro.md)
