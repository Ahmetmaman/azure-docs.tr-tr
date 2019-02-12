---
title: Azure uygulama teklif önkoşulları | Microsoft Docs
description: Azure uygulaması yayımlama önkoşulları Azure Marketi'nde teklif.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: pbutlerm
ms.openlocfilehash: b3f978635127ef6aabb123d1c95b76ed06fccbbf
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56097799"
---
# <a name="azure-application-prerequisites"></a>Azure uygulama önkoşulları

Bu makalede, Azure Market'te bir yönetilen uygulama teklif yayımlamak için teknik ve işletmeye önkoşulları açıklanır.  Zaten yapmadıysanız, videoyu görüntülemek [yapı çözüm şablonları ve yönetilen uygulamalar, Azure Market'te](https://channel9.msdn.com/Events/Build/2018/BRK3603).


## <a name="technical-requirements"></a>Teknik gereksinimler

Teknik gereksinimler, aşağıdaki öğeleri ekleyin:

*   Azure Resource Manager şablonları hakkında daha fazla bilgi için bkz. [yapısını ve Azure Resource Manager şablonları söz dizimini anlamak](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates). Bu makalede, Azure Resource Manager şablon yapısını açıklar. Bu, bir şablon ve bu bölümlerdeki kullanılabilir olan özellikleri farklı bölümlerini sayısını gösterir. Şablonda, JSON ve dağıtımınız için değerleri oluşturmada kullanabileceğiniz ifadeler bulunur. 
* Azure hızlı başlangıç şablonları.<br> Daha fazla bilgi için bkz.

  * [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/documentation/templates/). Daha fazlasını gerçekleştirmek için topluluk tarafından sağlanan kaynaklarla Azure Resource Manager üzerinden Azure kaynaklarını dağıtın. Azure Resource Manager, uygulamalarınızı bildirim temelli bir şablon aracılığıyla sağlamanıza olanak tanır. Tek bir şablonda birden çok hizmeti bağımlılıklarıyla birlikte dağıtabilirsiniz. Uygulama yaşam döngüsünün her aşamasında uygulamanızı tekrar tekrar dağıtmak için aynı şablonu kullanırsınız.
  * [GitHub: Azure Resource Manager hızlı başlangıç şablonları](https://github.com/azure/azure-quickstart-templates). Bu depo, topluluk tarafından katkıda bulunulan tüm şu anda kullanılabilir Azure Resource Manager şablonlarını içerir. Aranabilir şablon dizin konumunda tutulur https://azure.microsoft.com/en-us/documentation/templates/.
* UI tanımı oluşturma<br>
Daha fazla bilgi için [yönetilen uygulamanız için oluşturma Azure portal kullanıcı arabirimi](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview). Bu makalede createUiDefinition.json dosyasının temel kavramlar tanıtılmaktadır. Azure portalı, yönetilen bir uygulama oluşturmak için kullanılan kullanıcı arabirimi oluşturmak için bu dosyayı kullanır.


## <a name="business-requirements"></a>İş gereksinimleri

Aşağıdaki yordam, sözleşmeye dayalı ve yasal yükümlülüklerin yerine iş gereksinimleri şunlardır:

* Kayıtlı bir bulut Market yayımcı olmalıdır. Kayıtlı değil, bulut Market yayımcısı olun makaledeki adımları izleyin.

>[!NOTE]
>Bulut iş ortağı portalında oturum açmak için aynı Microsoft Developer Center kayıt hesabı kullanmanız gerekir. Azure Marketi Teklifleriniz için yalnızca bir Microsoft hesabı olması gerekir. Bu hesap, bireysel hizmetlerin veya teklifler için belirli olmamalıdır.

* Şirketinizin (veya yan kuruluşunun), bir satış gelen-Azure Marketi tarafından desteklenen ülkesi olmalıdır. Bu ülkeler güncel bir listesi için bkz. [Microsoft Azure Marketi katılım ilkeleri](https://azure.microsoft.com/support/legal/marketplace/participation-policies/).
* Ürününüzün Azure Marketi tarafından desteklenen faturalandırma modelleri ile uyumlu bir şekilde lisanslanmalıdır. Daha fazla bilgi için [faturalandırma seçenekleriyle](https://docs.microsoft.com/azure/marketplace/marketplace-commercial-transaction-capabilities-and-considerations) Azure Marketi'nde.
* Teknik Destek kullanılabilir müşterilere ticari açıdan makul bir şekilde yapmaktan sorumlu olursunuz. Bu destek, ücretsiz, ücretli veya topluluk yaklaşım olabilir.
* Yazılımınızı ve üçüncü taraf yazılım bağımlılıkları lisansı sağlamaktan sorumlu.
* Teklifinizin Azure Market'te ve Azure Portalı'nda listelenmesi ölçütlerini karşılayan içeriği sağlamanız gerekir.
* Yayımcı anlaşması ve Microsoft Azure Marketi katılım ilkeleri, koşulları kabul etmelisiniz.
* Microsoft Azure Web sitesi kullanım koşulları, Microsoft gizlilik bildirimi ve Microsoft Azure sertifikası Program Sözleşmesi ile uyumlu olmalıdır.


## <a name="publishing-requirements"></a>Yayımlama gereksinimleri

Yeni bir Azure uygulaması teklif yayımlamak için aşağıdaki gereksinimleri karşılaması gerekir:

* Kullanıma hazır, meta veriler bulunur. Aşağıdaki listede (kapsamlı olmayan), bu meta veri örneği gösterilmektedir:
  * Bir başlık
  * Bir açıklama (HTML biçiminde)
  * Bir logo görüntüsü (PNG biçiminde) ve bu resim boyutları düzelttik: 40 x 40 piksel, 90 x 90 piksel, 115 x 115 piksel ve 255 x 115 piksel.
* A *kullanım koşullarını* ve *gizlilik ilkesi*
* Belgeler
* Destek kişileri


## <a name="next-steps"></a>Sonraki adımlar

Tüm gereksinimlerini karşılamanızın ardından hazır olacaksınız [Azure uygulaması teklif oluşturma](./cpp-create-offer.md). 
 
